---
title: Dependency Injection
---

As we scaled up the features of our application and started dividing more chunks of the project in distinct sections, our previous DI approach became clunky. In order to get dependencies to the componenets that actually needed them, we would have to drill down layers of function parameters. 

Example:
Let's say we want to instantiate a database connection pool for our project, we want to be able to 

1. send the connection to functions that access our database
2. reuse the connection across the project
3. only have one connection for the project

What transpired in our code was the instantiation of a single `Env` struct that contained our database connection. 
``` go
type Env struct {
	DB *pgxpool.Pool
}
```
We then proceeded to pass the `Env` struct through our application to wherever it needed to be.
``` go title="main.go" hl_lines="4 5 16"
// We instantiate an env struct that contains a DB connection, 
// then pass the env to wherever the code needs it
func main() {
	env := &models.Env{DB: setupDatabaseConnection()}
	ginEngine := setUpEngine(env)

    ...
}

func setUpEngine(env *models.Env) *gin.Engine {
	r := gin.Default()
	store := cookie.NewStore([]byte("secret"))
	r.Use(sessions.Sessions("schedulii", store))
	r.Use(gin.Logger())
	r.Use(middleware.CORSMiddleware)
	router.SetupRoutes(r, env)
	return r
}
```

``` go title="routes.go" hl_lines="4 5"
func SetupRoutes(engine *gin.Engine, env *models.Env) {
	...

    engine.GET("/readUser", database.ReadUserHandler(env))
    engine.GET("/readGroup", database.ReadGroupHandler(env))
}
```

``` go title="handler.go" hl_lines="4"
func ReadEventHandler(env *models.Env) gin.HandlerFunc {
    ...

    event, err = data_srv.ReadEvent(env, event)
    if err != nil {
        c.JSON(http.StatusBadRequest, gin.H{"read error": err.Error()})
        return
    }
    c.JSON(200, event)
	return gin.HandlerFunc(fn)
}
```

``` go title="service.go" hl_lines="3"
func ReadEvent(env *models.Env, event data_model.Event) (data_model.Event, error) {
    // NOTE: (1)
	query := "SELECT * FROM Events WHERE eventid = ($1)"
	queryResult := env.DB.QueryRow(
		context.Background(),
		query,
		event.EventId,
	)
	
    ...
}
``` 

1.  Technically a 'service' should not include any database access logic. At the time of writing this, I hadn't yet implemented 'repositories' yet so we kept all logic and retrieval in the service. 

To remedy this, we had to look for a different way to handle dependency injection without our application. We ended up looking towards `wire`, a DI tool developed by Google. By writing `providers` and using `injectors`, we could decompose our application and also organize dependencies to the components that needed them. 

## Wire Exemplar
We first have to create `provider` functions that allow wire to create instances of structs for us to use. Here is an example of a `provider` from one of our services. 

```go title="service.go"
type EventService struct {
	db *pgxpool.Pool 
}

func NewEventService(db *pgxpool.Pool) EventService {
	return EventService{
		db: db,
	}
}
```

We can then let wire know about our `providers` so that it can generate our DI code. 

``` go title="wire.go"
//+build wireinject

package main

import ...

var AppSet = wire.NewSet(
    // setting up database connection
	db.NewDatabaseConnection,

    // setting up gin.engine for our web server
	gin.Default, 

    // setting up service and handler for our events data
	data_srv.NewEventService,
	data_handler.NewEventHandler,

    // setting up our router for our app
	routes.NewRouter,

    // creating the app and combining everything
	NewScheduliiApp, 
)

// entrypoint to the injected app
func InitializeApp() (ScheduliiApp, error) {
    wire.Build(AppSet)
	return ScheduliiApp{}, nil
}
```

When we run `wire` on our go project, it generates this new file called `wire_gen.go`. We can then reference the newly created `InitializeApp()` function in our `main()` function to get a `ScheduliiApp` with all our required dependencies. 

``` go title="wire_gen.go"
package main

import ...

func InitializeApp() (ScheduliiApp, error) {
	pool, err := db.NewDatabaseConnection()
	if err != nil {
		return ScheduliiApp{}, err
	}
	engine := gin.Default()
	userService := data_srv.NewUserService(pool)
	userHandler := data_handler.NewUserHandler(userService)
	eventService := data_srv.NewEventService(pool)
	eventHandler := data_handler.NewEventHandler(eventService)
	groupService := data_srv.NewGroupService(pool)
	groupHandler := data_handler.NewGroupHandler(groupService)
	router := routes.NewRouter(engine, userHandler, eventHandler, groupHandler)
	scheduliiApp := NewScheduliiApp(pool, engine, router)
	return scheduliiApp, nil
}
```

## Resources
- [Dependency Injection in Go](https://blog.drewolson.org/dependency-injection-in-go)
