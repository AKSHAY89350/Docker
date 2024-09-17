# Multi-Container .NET Web Application

This project demonstrates how to create, containerize, and manage a multi-container .NET web application using Docker and Docker Compose. The application includes two services:

- **service1**
- **service2**

This README file provides a complete guide for setting up, running, and managing the application, as well as instructions for pushing and pulling Docker images.

---

## Table of Contents

1. [Prerequisites](#prerequisites)
2. [Setup and Installation](#setup-and-installation)
3. [Creating Dockerfiles](#creating-dockerfiles)
   - [Dockerfile for service1](#dockerfile-for-service1)
   - [Dockerfile for service2](#dockerfile-for-service2)
4. [Building Docker Images](#building-docker-images)
5. [Running Containers Locally](#running-containers-locally)
6. [Using Docker Compose](#using-docker-compose)
7. [Pushing Images to Docker Hub](#pushing-images-to-docker-hub)
8. [Pulling and Running Images from Docker Hub](#pulling-and-running-images-from-docker-hub)

---

## Prerequisites

Ensure the following tools are installed before proceeding:

- **Docker**: Version 20.10 or later
- **Docker Compose**: Version 1.29 or later
- **.NET SDK**: Version 6.0 or later
- **Git**: For version control
- **GitHub Account**: For repository and project management
- **Docker Hub Account**: For image storage and management



## Setup and Installation

# Clone the Repository
```
git clone https://github.com/AKSHAY89350/Docker
cd Docker
```
## Creating Dockerfiles

# Dockerfile for service1

In the `service1/` directory, create a `Dockerfile` with the following content:

```dockerfile

#See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["service1/service1.csproj", "service1/"]
RUN dotnet restore "service1/service1.csproj"
COPY . .
WORKDIR "/src/service1"
RUN dotnet build "service1.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "service1.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "service1.dll"]
````



## Building Docker Images
# Build Docker Image for service1
```
cd service1
docker build -t service1:latest .
# Build Docker Image for service2

cd ../service2
docker build -t service2:latest .
````

---
# Running Containers Locally

# Run Container for service1
```
docker run -d -p 7000:80 service1:latest
```
# Run Container for service2
```
docker run -d -p 9000:80 service2:latest
```


## Access the services at:

```
http://localhost:7000 for service1
```
```
http://localhost:8000 for service2
```
## Using Docker Compose
# Create docker-compose.yml

In the project root directory:
```dockerfile
version: '3.8'

services:
  service1:
    image: ${DOCKER_REGISTRY-}service1
    build:
      context: .
      dockerfile: service1/Dockerfile
    ports:
      - "7000:80"

  service2:
    image: ${DOCKER_REGISTRY-}service2
    build:
      context: .
      dockerfile: service2/Dockerfile
    ports:
      - "9000:80"

```
## Build and Bring Up the Application
docker-compose up --build

# The services will be accessible at:
```
http://localhost:7000 for service1
http://localhost:9000 for service2
```
# Stopping the Services
docker-compose down

## Pushing Images to Docker Hub
# Log in to Docker Hub

docker login
Tag and Push Images


## For service1:
```
docker tag service1:latest akshay89350/service1:latest
docker push akshay89350/service1:latest
```

# For service2:
```
docker tag service2:latest akshay89350/service2:latest
docker push akshay89350/service2:latest
```


## Pulling and Running Images from Docker Hub
# Pull Images
For service1:
```
docker pull akshay89350/service1:latest
```

For service2:
```
docker pull akshay89350/service2:latest
```

# Run Containers
For service1:
```
docker run -d -p 7000:80 akshay89350/service1:latest
```

For service2:
```
docker run -d -p 9000:80 akshay89350/service2:latest
```
