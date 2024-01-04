---
layout: default
title: Project Creation
parent: Security API
nav_order: 1
---

# Project Creation
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Prerequisites
See [Project Preparation - Backend API]({% link docs/project-preparation/backend-api.md %})

## Create a new ASP.NET Core project
Create the project using `webapi` template with controllers:
```bash
dotnet new webapi --use-controllers -o src/SecurityAPI
```

## Test the project
1. In Visual Studio Code, with no file open, press `F5`.
1. If the debug configuration did not generate automatically, select `.Net5+ and .Net Core`.
1. Press `F5` to launch the debugger. Swagger page should be available at `/swagger/index.html`. For example, `https://localhost:7136/swagger/index.html`
