---
layout: default
title: Prerequisites
parent: Project Preparation
nav_order: 1
---

# Prerequisites
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Visual Studio Code
Visual Studio Code, often abbreviated as VS Code, is a free, open-source code editor developed by Microsoft. It's available for Windows, macOS, and Linux operating systems. It's able to support for multiple pProgramming Languages, integrate debugging tools, remote development and more...

[Learn more about Visual Studio Code](https://code.visualstudio.com/)

## Docker server

Using a Docker server is required to deploy some project's dependencies. Like database, messaging, etc... Also, it's possible to develop inside containers. That guarantee to always working in a clean environment. That avoid the ["works on my machine" effect](https://codingforspeed.com/but-it-works-on-my-machine/).

### Docker Desktop
On windows, Docker Desktop is a simple solution. It's use ["Windows Subsystem for Linux"](https://learn.microsoft.com/en-us/windows/wsl/about) to install a Docker server. It's well integrate to Windows and Visual Studio Code.

[Learn more about Docker Desktop](https://docs.docker.com/desktop/)

### Remote Docker server
Visual Studio Code is able to connect to a remote server for developing or deploying Docker container. It's  possible to combine both. Developing inside a container is very stable as the environment is very controlled.

[Learn more about Developing inside a Container](https://code.visualstudio.com/docs/devcontainers/containers)