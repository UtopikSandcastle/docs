---
layout: default
title: Project Creation
parent: Access Control API
nav_order: 2
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
dotnet new webapi --use-controllers -o src/AccessControlAPI -n UtopikSancastle.AccessControle.API
```

## Test the project
1. In Visual Studio Code, with no file open, press `F5`.
1. If the debug configuration did not generate automatically, select `.Net5+ and .Net Core`.  Your browser should open. If the Swagger page is not display, go to `/swagger/index.html`. For example, `https://localhost:7136/swagger/index.html`
