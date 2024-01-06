---
layout: default
title: MongoDB
parent: Access Control API
nav_order: 3
---

# MongoDB
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Introduction
MongoDB is a popular open-source NoSQL database program. It is known for its flexibility, scalability, and performance, particularly in handling large volumes of data and in applications where data structures can change over time.

## Deploy MongoDB within the Dev Container
Like saw in[Project Preparation - Backend API]({% link docs/project-preparation/backend-api.md %}), the Dev Container is configured to use a Docker compose file. So MongoDB container could be easily deploy with the Dev Container.

To add MongoDB to the Docker compose file, modify the file `docker-compose.yml` as following:
```yml
services:
  devcontainer:
    image: mcr.microsoft.com/devcontainers/dotnet:1-8.0-bookworm
    volumes:
      - ../..:/workspaces:cached
      - ~/.aspnet/https:/home/vscode/.aspnet/https
    network_mode: service:mongo
    command: sleep infinity

  mongo:
    image: mongo:7.0.4
    restart: unless-stopped

  mongo-express:
    image: mongo-express
    ports:
      - :8081
    restart: unless-stopped
```
`mongo-express` is a web-based MongoDB admin interface written with Node.js, Express, and Bootstrap3. The mongo-express's port is random, so check your Docker server you find out at which port the container has been set. The default username and password are `admin` and `pass`.

[Learn more about mongo-express](https://github.com/mongo-express/mongo-express)

## Add MongoDB to a ASP.NET Core project
See [Create a web API with ASP.NET Core and MongoDB](https://learn.microsoft.com/en-us/aspnet/core/tutorials/first-mongo-app?view=aspnetcore-8.0&tabs=visual-studio)
