---
layout: default
nav_order: 5
parent: Project Preparation
title: Backend API
---

# Backend API
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## .NET 7
.NET 7, is a free, open-source, cross-platform framework developed by Microsoft for building modern, cloud-based, and internet-connected applications. It is a significant redesign of the older .NET Framework, created to cater to the evolving needs of modern application development.

The backend projects focusing on the ASP.NET Core.

[Learn more about ASP.NET Core](https://learn.microsoft.com/en-us/aspnet/core/)

## Backend development
### Prerequisites
Follow the instruction to create a dev container: [Developing inside a Container]({% link docs/project-preparation/devcontainer.md %})

### Export the local SSL certificate for development
On you local machine where you web browser is running, run the following command:
**Windows PowerShell**
```powershell
dotnet dev-certs https --trust; dotnet dev-certs https -ep "$env:USERPROFILE/.aspnet/https/aspnetapp.pfx" -p "SecurePwdGoesHere"
```

**macOS/Linux terminal**
```bash
dotnet dev-certs https --trust; dotnet dev-certs https -ep "${HOME}/.aspnet/https/aspnetapp.pfx" -p "SecurePwdGoesHere"
```

If you are using a remote connection SSH, copy that certification to that server in the directory `~/.aspnet/https`.

[Learn more about Enabling HTTPS in ASP.NET using your own dev certificate](https://github.com/devcontainers/templates/tree/main/src/dotnet)

### Add SSL certificate configuration to Dev Container
Add the following in to `.devcontainer/devcontainer.json`:
```jsonc
{
  ...
  "remoteEnv": {
    "ASPNETCORE_Kestrel__Certificates__Default__Password": "SecurePwdGoesHere",
    "ASPNETCORE_Kestrel__Certificates__Default__Path": "${containerEnv:HOME:/home/vscode}/.aspnet/https/aspnetapp.pfx"
  },
  ...
}
```

### Add a Docker compose file to the `.devcontainer` directory
The backend API depend on a MongoDB server. An easy way to deploy that server is to use a Docker compose file.
Create a file named `docker-compose.yml` in the directory `.devcontainer` directory and add it the following:
```yml
services:
  devcontainer:
    image: mcr.microsoft.com/devcontainers/dotnet:1-8.0-bookworm
    volumes:
      - ../..:/workspaces:cached
      - ~/.aspnet/https:/home/vscode/.aspnet/https
    command: sleep infinity
```
The volume `~/.aspnet/https:/home/vscode/.aspnet/https` give access to the Dev Container to the local SSL Certificate.

Modify the file `.devcontainer/devcontainer.json` and replace the Docker image configuration by the following:
```jsonc
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

### Add OpenAPI tools to the Dec Container
In the Dev Container file, add the following post command:
```jsonc
...
  // Use 'postCreateCommand' to run commands after the container is created.
  "postCreateCommand": "dotnet new tool-manifest && dotnet tool install Swashbuckle.AspNetCore.Cli --version 6.5.0",
...
```
That tools can create OpenApi configuration file.

### Add Visual Studio Code Extensions to Dev Container
In the Dev Container file, you can add Visual Studio Code extensions. For example:
```jsonc
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

### Dev Container ready
{: .no_toc }
Your file `devcontainer.json` should look like this:
```jsonc
// For format details, see https://aka.ms/devcontainer.json. For config options, see the
// README at: https://github.com/devcontainers/templates/tree/main/src/dotnet
{
  // "name": "C# (.NET)",
  // Or use a Dockerfile or Docker Compose file. More info: https://containers.dev/guide/dockerfile
  // "image": "mcr.microsoft.com/devcontainers/dotnet:1-8.0-bookworm",

  "dockerComposeFile": "docker-compose.yml",
  "service": "devcontainer",
  "workspaceFolder": "/workspaces/${localWorkspaceFolderBasename}",

  "remoteEnv": {
    "ASPNETCORE_Kestrel__Certificates__Default__Password": "SecurePwdGoesHere",
    "ASPNETCORE_Kestrel__Certificates__Default__Path": "${containerEnv:HOME:/home/vscode}/.aspnet/https/aspnetapp.pfx"
  },

  // Features to add to the dev container. More info: https://containers.dev/features.
  // "features": {},

  // Use 'forwardPorts' to make a list of ports inside the container available locally.
  // "forwardPorts": [8081],
  // "portsAttributes": {
  // 		"8081": {
  // 			"protocol": "http"
  // 		}
  // },

  // Use 'postCreateCommand' to run commands after the container is created.
  "postCreateCommand": "dotnet new tool-manifest && dotnet tool install Swashbuckle.AspNetCore.Cli --version 6.5.0",


  // Configure tool-specific properties.
  "customizations": {
    "vscode": {
      "extensions": [
        "ms-dotnettools.csharp", // C# Dev Kit for Visual Studio Code
        "ms-dotnettools.csdevkit" // C# for Visual Studio Code
      ]
    }
  }

  // Uncomment to connect as root instead. More info: https://aka.ms/dev-containers-non-root.
  // "remoteUser": "root"
}

```

### Generate OpenApi configuration file on build
In the project file (`.csproj`) and add the following:
```xml
...
  <Target Name="OpenAPI" AfterTargets="Build">
      <Exec Command="dotnet swagger tofile --output openapi.json $(TargetPath) v1" WorkingDirectory="$(TargetDir)" />
  </Target>
```

Install the OpenApi package:
```bash
dotnet add package Microsoft.AspNetCore.OpenApi --version 7.0.14
```
Now the file `openapi.json` can be found in `src/{project}/bin/Debug/net7.0/`

### Set Swagger as default page (Optional)
Swagger is a tool that help to test the API. It show all endpoints and let you trigger actions. 

[Learn more about ASP.NET Core web API documentation with Swagger / OpenAPI](https://learn.microsoft.com/en-us/aspnet/core/tutorials/web-api-help-pages-using-swagger?view=aspnetcore-8.0)

Once you project has been created, in the file `Program.cs`, change Swagger UI as following:
```c#
...
  app.UseSwaggerUI(c =>
  {
    c.SwaggerEndpoint("/swagger/v1/swagger.json", "Utopik Sandcastle Access Control API V1");
    c.InjectStylesheet("/swagger/custom.css");
    c.RoutePrefix = String.Empty;
  });
...
```

### Enable Swagger annotation (Optional)
By default, Swagger will use your models as is. The problem that could give is when you try to use the Swagger UI, you may got error because some element of the model cannot be send to the backend. For example, Mongo database do not accept that you try to set an Id and then return a error. To avoid to have unwanted element from the model, we can use annotation to indicate to Swagger which field to set as read-only.

To enable Swagger annotation, in `Program.cs` change `builder.Services.AddSwaggerGen` as following:
```c#
...
builder.Services.AddSwaggerGen(c =>
{
  c.EnableAnnotations();
});
...
```

### Dockerfile
In the project directory, add a new file name `Dockerfile` within the following:
```Dockerfile
# Build project
FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build-env
WORKDIR /App
# Copy everything
COPY . ./
# Add OpenAPI tools
RUN dotnet tool install Swashbuckle.AspNetCore.Cli --version 6.5.0
# Restore as distinct layers
RUN dotnet restore
# Build and publish a release
RUN dotnet publish -c Release -o out

# Build runtime image
FROM mcr.microsoft.com/dotnet/aspnet:7.0
WORKDIR /App
COPY --from=build-env /App/out .
ENTRYPOINT ["bash", "-c", "dotnet `echo $(basename /App/*.deps.json .deps.json)`.dll"]
```
That file is used by the CI to build the docker image and publish it.

The command `COPY` will also copy the `bin` directory and other unwanted files. That going to slow down the build process. To fix it, in the project directory add a new file named `.dockerignore` within the following:
```
Dockerfile
**/[b|B]in/
**/[O|o]bj/
```