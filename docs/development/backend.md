---
layout: default
title: Backend
parent: Development
nav_order: 5
---

# Backend
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

#### Add SSL certificate configuration to Dev Container
{: .no_toc }
Add the following in to .devcontainer/devcontainer.json:
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
On you local machine where you web broser is running, run the following command:
**Windows PowerShell**

```powershell
dotnet dev-certs https --trust; dotnet dev-certs https -ep "$env:USERPROFILE/.aspnet/https/aspnetapp.pfx" -p "SecurePwdGoesHere"
```

**macOS/Linux terminal**

```powershell
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
1. In Visual Studio Code, go to `Run & Debug` menu. 
1. Click `Run & Debug` button. 
1. Select `.Net 5+ and .Net core`. A new directory named `.vscode` should be created with 2 files: `launch.json` and `tasks.json`.
1. Press `F5` to launch the debugger. Swagger page should be available at `/swagger/index.html`. For example, `https://localhost:7136/swagger/index.html`