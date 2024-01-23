---
layout: default
title: Project Creation
parent: Sandcastle App
nav_order: 2
---

# Project Creation
## Prerequisites
See [Project Preparation - Frontend]({% link docs/project-preparation/frontend.md %})

## Create a new Angular project
Create the project using Angular CLI commend:
```bash
ng new sandcastle --directory .
```

## Build & Run the project
VSCode should be ready to run the project.
1. In Visual Studio Code, with no file open, press `F5`. Your browser should open and display the default Angular template.

## Module federation
In a module federation architecture, Sandcastle App is the **Shell** or the host.

In our case we going to set the shell as a `dynamic-host`. That configuration let us to define remote modules in a manifest stored in the `assets` directory. This configuration give ous the liberty to set remote modules in the deployment process. So for example, we could chose to add or remove remote modules based on the targeted environment.

To prepare the project as such, execute the following command:
```bash
ng add @angular-architects/module-federation --project sandcastle --type dynamic-host --port 4241
```
The remote modules manifest is located at: `src/assets/mf.manifest.json`