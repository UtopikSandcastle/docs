---
layout: default
nav_order: 5
parent: Backend
grand_parent: Development
title: Project preparation
---

# Project preparation
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## .NET 8
.NET 8, is a free, open-source, cross-platform framework developed by Microsoft for building modern, cloud-based, and internet-connected applications. It is a significant redesign of the older .NET Framework, created to cater to the evolving needs of modern application development.

The backend projects focusing on the ASP.NET Core.

[Learn more about ASP.NET Core](https://learn.microsoft.com/en-us/aspnet/core/)

## Backend development
### Requirement
Follow the instruction to create a dev container: [Developing inside a Container]({% link docs/development/devcontainer.md %})

#### Add a Docker compose file to the `.devcontainer` directory
The backend API depend on a MongoDB server. An easy way to deploy that server is to use a Docker compose file.
Create a file named `docker-compose.yml` in the directory `.devcontainer` directory and add it the following:
```yml
services:
  devcontainer:
    image: mcr.microsoft.com/devcontainers/dotnet:1-8.0-bookworm
    volumes:
      - ../..:/workspaces:cached
      - ~/.aspnet:/home/vscode/.aspnet
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

Modify the file `.devcontainer/devcontainer.json` and replace the Docker image configuration by the following:
```json
{
  // "name": "C# (.NET)",
  // Or use a Dockerfile or Docker Compose file. More info: https://containers.dev/guide/dockerfile
  // "image": "mcr.microsoft.com/devcontainers/dotnet:1-8.0-bookworm",

  "dockerComposeFile": "docker-compose.yml",
  "service": "devcontainer",
  "workspaceFolder": "/workspaces/${localWorkspaceFolderBasename}",
  ...
}
```
Now Dev Container going the load Docker compose file before applying the Dev Container configuration.

#### Add Visual Studio Code Extensions to Dev Container
In the Dev Container file, you can add Visual Studio Code extensions. For example:
```json
{
  ...
  "customizations": {
    "vscode": {
      "extensions": [
        "ms-dotnettools.csharp", // C# Dev Kit for Visual Studio Code
				"ms-dotnettools.csdevkit" // C# for Visual Studio Code
      ]
    }
  }
}
```

#### Add SSL certificate configuration to Dev Container
{: .no_toc }
Add the following in to `.devcontainer/devcontainer.json`:
```json
{
  ...
  "remoteEnv": {
    "ASPNETCORE_Kestrel__Certificates__Default__Password": "SecurePwdGoesHere",
    "ASPNETCORE_Kestrel__Certificates__Default__Path": "${containerEnv:HOME:/home/vscode}/.aspnet/https/aspnetapp.pfx"
  },
  ...
}
```
#### Export the SSL certificate for development
{: .no_toc }
On you local machine where you web browser is running, run the following command:
**Windows PowerShell**
```powershell
dotnet dev-certs https --trust; dotnet dev-certs https -ep "$env:USERPROFILE/.aspnet/https/aspnetapp.pfx" -p "SecurePwdGoesHere"
```

**macOS/Linux terminal**
```bash
dotnet dev-certs https --trust; dotnet dev-certs https -ep "${HOME}/.aspnet/https/aspnetapp.pfx" -p "SecurePwdGoesHere"
```

#### Add the SSL certificate to the Dev Container
{: .no_toc }
1. Open the directory where the certificate ha been exported and drag it in the directory open in your Dev Container.
1. Move the certificate to the configured directory with the following command:
```bash
mkdir -p ~/.aspnet/https && mv aspnetapp.pfx $_
```

[Learn more about Enabling HTTPS in ASP.NET using your own dev certificate](https://github.com/devcontainers/templates/tree/main/src/dotnet)

### Create a new ASP.NET Core project
Create the project using `webapi` template with controllers:
```bash
dotnet new webapi --use-controllers -o src/SecurityAPI
```

### Test the project
1. In Visual Studio Code, with no file open, press `F5`.
1. If the debug configuration did not generate automatically, select `.Net5+ and .Net Core`.
1. Press `F5` to launch the debugger. Swagger page should be available at `/swagger/index.html`. For example, `https://localhost:7136/swagger/index.html`