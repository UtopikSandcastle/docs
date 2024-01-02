---
layout: default
title: Frontend
parent: Development
nav_order: 4
---

# Frontend
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Angular
Angular is a popular open-source web application framework led by the Angular Team at Google and by a community of individuals and corporations. Angular is a complete rewrite from the same team that built AngularJS, its predecessor.

[Learn more about Angular](https://angular.io/guide/what-is-angular)

## Sandcastle App
The app is the entry point to web portal. It's the first frontend reached. It load the microfrontends.

## Frontend development
### Requirement
{: .no_toc }
Follow the instruction to create a dev container: [Developing inside a Container]({% link docs/development/devcontainer.md %})

#### Add Angular to Dev Container
{: .no_toc }
In the directory `.devcontainer` add a new file named `postCreateCommand.sh` within the following:
```bash
#!/usr/bin/env bash

sudo apt-get update
sudo apt-get install -y xdg-utils

sudo npm install -g npm@latest @angular/cli
npm install
```

The file `postCreateCommand.sh` must be executable. Run the following command:
```bash
chmod +x .devcontainer/postCreateCommand.sh
git update-index --chmod=+x .devcontainer/postCreateCommand.sh
```

Edit the file `devcontainer.json` and change the line beginning with `"postCreateCommand":` for the following:
```json
"postCreateCommand": "./.devcontainer/postCreateCommand.sh",
```

Rebuild the container to apply the modification. [Learn more about Rebuild Dev Container](https://code.visualstudio.com/docs/devcontainers/create-dev-container#_rebuild)

#### TODO: E2E dependencies
{: .no_toc }


### Create an new Angular project for Sandcastle App
{: .no_toc }
`ng new Sandcastle --directory .`
