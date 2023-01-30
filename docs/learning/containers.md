---
title: Containers
description: An brief summary of what this project was about
---
## Overview

I love using this diagram to illustrate the difference between containers and virtual machines. 

=== "Virtual Machine"
    ```mermaid
    graph BT
        A[Infrastructure] --> B
        B[Hypervisor] --> C
        subgraph C[VM1]
            direction BT    
            D[Guest OS] --> E[App]
        end
        B --> F
        subgraph F[VM2]
            direction BT    
            G[Guest OS] --> H[App]
        end
        B --> I
        subgraph I[VM3]
            direction BT    
            J[Guest OS] --> K[App]
        end
    ```

    In order to create a virtual machine, you need to allocate enough CPU, memory and storage for each of the VM operating systems. Setting up these operating systems can also be a slow process. If your app is lightweight, more resources may be going into running the VM rather than to supporting your application. 

    Note: Hypervisor is just the software application that creates virtual machines. 



=== "Container"
    ```mermaid
    graph BT
        A[Infrastructure] --> B
        B[Host OS] --> C
        C[Container Engine] --> D
        subgraph D[Container 1]
            E[App]
        end
        C --> F
        subgraph F[Container 2]
            G[App]
        end
        C --> H
        subgraph H[Container 3]
            I[App]
        end
    ```

    We use a container engine (Docker, Podman) to create multiple containers for our apps. All of these apps are isolated from each other in a similar fashion to the VM example. However, the container engine running the containers utilizes the underlying host's operating system and infrastructure. You use less resources creating the containers and therefore more of your compute resources can go to running your apps. 

    Note: Because you are not mocking the underlying OS of the machine, differing architectures may make containers incompatible with certain systems. You cannot run an x86 architecture container on ARM computers for example. 

## Terminology
* **Image**: A "recipe" for creating a container
* **Container**: 
* **Docker**: A service on your computer that enables you to create a run lightweight and replicatable environments for code called containers. 
Dockerfile: 

## What are containers used for?
- **Code Development**  
    With a team of developers all using different devices, it can be time consuming setting up new environments on everyone's machines. Why not have an environment built into a container and allow developers to developer code right into it?
- **Deployment**  
    Rather than setting up a VM with all the dependencies and manually configuring a deployment for a service, just run the service from a container. Not only is it faster to run a pre-configured environment on a container, but if something goes wrong, just swap your service out with another container. 

## Using Docker
### Main Process
1. Using a dockerfile, create an image
2. Using the image, create a container
3. Run the container

#### Dockerfile
When create your dockerfile, here are the main steps involved:  

1. Use a base image
    You can find a lot of free images online at [DockerHub](https://hub.docker.com/)

    ```dockerfile
    FROM ubuntu:latest
    ```

2. Set working directory
    Set the directory within the container that you will be working from

    ```dockerfile
    WORKDIR /app
    ```

3. Copy source code 
    Copy your code from your local directory to your dockerfile. Remember that your dockerfile sits in the root folder of your working directory, so you often can just copy everything from your working directory into the new image. 

    ```dockerfile
    COPY . .
    ```

4. Other commands
    Whatever other commands you wish to do within your container
        ```dockerfile
        # run commands
        RUN go mod download && go mod verify 

        # set environment variables in your container
        ENV GIN_MODE=release
        ```

#### Command-line Commands
1. Build an image using your dockerfile

    ```bash
    docker build -t <image-name> .
    ```

2. Create a container using your new image

    ```bash
    docker run -it -p 8080:8080 <image-name>
    ```

    NOTE: `-it` creates an interactive terminal session you can use to interact with your container. 

## Multi-Stage Builds
Sometimes you may want to break down your project into multiple stages to better organize and simplify the process. 

## Example:
You have web-app that contains a backend and frontend. Your backend is written in Golang and your frontend uses React. You need to do the following:

1. Build your backend server
2. Build your frontend client
3. Combine both final assets into a container that will deploy both the frontend and backend

### Sample Dockerfile
```dockerfile
FROM node:18 as frontend-build
# Set up npm dependencies
WORKDIR /app
COPY ./frontend/package*.json ./
RUN npm install

# Build assets for frontend
COPY ./frontend/. ./
RUN npm run build

FROM golang as backend-build
# Setup golang dependencies
WORKDIR /app/backend
COPY ./backend/go* ./
RUN go mod download && go mod verify

# Build deployment binary
COPY ./backend/. ./
RUN CGO_ENABLED=0 go build -o bin/schedulii ./src/main.go

FROM alpine:latest
# Configure for golang compatibility
RUN apk add gcompat && apk add --update curl

# Ensure server runs in production mode
ENV GIN_MODE=release

# Grab binaries and build assets from build stages
WORKDIR /app
COPY --from=frontend-build /app/build ./frontend/build
COPY --from=backend-build /app/backend/bin/. ./backend/bin/.

# Run application
EXPOSE 8080
WORKDIR /app/backend/bin
CMD ["./app"]
```

## References
* [Diagram Source](https://www.docker.com/resources/what-container/)
