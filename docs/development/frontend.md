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

This project use Angular 17.

[Learn more about Angular](https://angular.io/guide/what-is-angular)

## Sandcastle App
The app is the entry point to web portal. It's the first frontend reached. It load the microfrontends.

## Frontend development
### Requirement
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

In the Dev Container file, you can add Visual Studio Code extensions. For example:
```json
{
  ...
  	"customizations": {
		"vscode": {
			"extensions": [
				"Angular.ng-template",    // Angular helper
				"dbaeumer.vscode-eslint", // Code formatter
				"ms-vscode.live-server"   // HTML file processing for End-to-end report
			]
		}
	}
}
```

Rebuild the container to apply the modification. [Learn more about Rebuild Dev Container](https://code.visualstudio.com/docs/devcontainers/create-dev-container#_rebuild)

#### End-to-end test preparation
{: .no_toc }
Angular include Karma to do end-to-end tests. To make works with Dev Container, it's require some preparation.

Karma looking for a web browser to run the tests. It can connect with your local browser even running inside the Dev Container but still try to find the browser process. To fix that, we just have to remove the browser configuration.

To modify the karma's configuration, generate the configuration file with this command:
```
ng generate config karma
```

In the karma.conf.js file, comment out the line `browser` like the following:
```js
...
    reporters: ['progress', 'kjhtml'],
    // browsers: ['Chrome'],
    restartOnFileChange: true
...
```

### Create a new Angular project for Sandcastle App
`ng new Sandcastle --directory .`

