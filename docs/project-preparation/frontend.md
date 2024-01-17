---
layout: default
title: Frontend
parent: Project Preparation
nav_order: 4
---

# Frontend

## Angular
Angular is a popular open-source web application framework led by the Angular Team at Google and by a community of individuals and corporations. Angular is a complete rewrite from the same team that built AngularJS, its predecessor.

This project use Angular 17.

[Learn more about Angular](https://angular.io/guide/what-is-angular)

## Prerequisites
Follow the instruction to create a dev container: [Developing inside a Container]({% link docs/project-preparation/devcontainer.md %})

### Add Angular to Dev Container
{: .no_toc }
In the directory `.devcontainer` add a new file named `postCreateCommand.sh` within the following:
```bash
#!/usr/bin/env bash

sudo npm install -g npm@latest @angular/cli
echo "source <(ng completion script)" >> /home/node/.bashrc

yarn install
```

The file `postCreateCommand.sh` must be executable. Run the following command:
```bash
chmod +x .devcontainer/postCreateCommand.sh
git update-index --chmod=+x .devcontainer/postCreateCommand.sh
```

Edit the file `devcontainer.json` and change the line beginning with `"postCreateCommand":` for the following:
```jsonc
"postCreateCommand": "./.devcontainer/postCreateCommand.sh",
```

### Add Visual Studio Code Extensions to Dev Container
{: .no_toc }
In the Dev Container file, you can add Visual Studio Code extensions. For example:
```jsonc
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

## Post project creation

### Generate Environments files
To generate default environements files, execute the following command:
```bash
ng generate environments
```
[Learn more about Angular - Configuring application environments](https://angular.io/guide/build#configuring-application-environments)

### End-to-end test preparation
Angular include Karma to do end-to-end tests. To make works with Dev Container, it's require some preparation.

Karma looking for a web browser to run the tests. It can connect with your local browser even running inside the Dev Container but still try to find the browser process. To fix that, we just have to remove the browser configuration.

**Once the new project created**, modify the karma's configuration generated with the following command:
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

### ESLint
ESLint is a popular static code analysis tool used in software development for identifying problematic patterns or code that doesn't adhere to certain style guidelines.
1. Install the `@angular-eslint/schematics` package with the following command:
```bash
ng add @angular-eslint/schematics
```
1. As web use Webpack, we need to modify the configuration file. Open the file `.eslintrc.json` and do the following modifications:
```jsonc
...
  "root": true,
  "parserOptions": {
    "ecmaVersion": "latest",
    "sourceType": "module"
  },
  "ignorePatterns": [
    "projects/**/*"
  ],
...
```
{: .highlight }
That may not the best solution. However, that workarround works.
