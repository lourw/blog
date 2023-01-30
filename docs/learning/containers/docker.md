---
title: Docker
description: An brief summary of what this project was about
---
## Overview

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
