---
layout: default
title: Project Creation
parent: Security Frontend
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
See [Project Preparation - Frontend]({% link docs/project-preparation/frontend.md %})

## Create a new Angular project
Create the project using Angular CLI commend:
```bash
ng new security --directory .
```

## Test the project
1. In Visual Studio Code, with no file open, press `F5`. Your browser should open and display the default Angular template.

## Module federation
In a module federation architecture, Security Frontend is a **Remote Module**.

It's configured to expose the shell required components.

To prepare the project as such, execute the following command:
```bash
ng add @angular-architects/module-federation --project sandcastle --type remote --port 4255
```

The exposed components are configured in the file `webpack.config.js`.