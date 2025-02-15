---
title: SharePoint Framework toolchain
description: The toolchain is the set of build tools, framework packages, and other items that manage building and deploying your SharePoint Framework client-side projects.
ms.date: 08/05/2021
ms.prod: sharepoint
localization_priority: Priority
---

# SharePoint Framework toolchain

The SharePoint Framework toolchain is the set of build tools, framework packages, and other items that manage building and deploying your client-side projects.

The toolchain:

- Helps you build client-side components such as web parts.
- Helps you test client-side components in your local development environment by using tools such as the SharePoint Workbench.
- Enables you to package and deploy to SharePoint.
- Provides you with a set of build commands that help you complete key build tasks such as code compilation, packaging the client-side project into a SharePoint app package, and more.

> [!IMPORTANT]
> The local workbench does not support Internet Explorer 11.

## Use npm packages

Before diving into the various toolchain components, it’s important to understand how the SharePoint Framework uses [npm](https://www.npmjs.com/) to manage different modules in the project. npm is one of the preferred open-source package managers for JavaScript client-side development.

A typical npm package consists of one or more reusable JavaScript code files, called modules, along with a list of dependency packages. When you install the package, npm also installs those dependencies.

The official [npm registry](https://www.npmjs.com/) consists of hundreds of packages that you can download to build your application. You can also [publish your own packages](https://docs.npmjs.com/getting-started/what-is-npm) to npm and share them with other developers. The SharePoint Framework uses some of the npm packages in the toolchain and also publishes its [own packages to the npm registry](https://www.npmjs.com/search?q=%40microsoft%2Fsp-).

### SharePoint Framework packages

The SharePoint Framework consists of several npm packages that work together to help you build client-side experiences in SharePoint.

The following npm packages are in the SharePoint Framework:

- [@microsoft/generator-sharepoint](https://www.npmjs.com/package/@microsoft/generator-sharepoint). A [Yeoman](http://yeoman.io/) plug-in for use with the SharePoint Framework. Using this generator, developers can quickly set up a new client-side web part project with sensible defaults and best practices.
- [@microsoft/sp-client-base](https://www.npmjs.com/package/@microsoft/sp-client-base). Defines the core libraries for client-side applications built using the SharePoint Framework.
- [@microsoft/sp-webpart-workbench](https://www.npmjs.com/package/@microsoft/sp-webpart-workbench). The SharePoint Workbench is a standalone environment for testing and debugging client-side web parts.
- [@microsoft/sp-module-loader](https://www.npmjs.com/package/@microsoft/sp-module-loader). A module loader that manages versioning and loading of client-side components, web parts, and other assets. It also provides basic diagnostic services. It is built on familiar standards such as SystemJS and webpack, and is the first part of the SharePoint Framework to load on a page.
- [@microsoft/sp-module-interfaces](https://www.npmjs.com/package/@microsoft/sp-module-interfaces). Defines several module interfaces that are shared by the SharePoint Framework module loader and also the build system.
- [@microsoft/sp-lodash-subset](https://www.npmjs.com/package/@microsoft/sp-lodash-subset). Provides a custom bundle of [lodash](https://lodash.com/) for use with the SharePoint Framework's module loader. To improve runtime performance, it only includes a subset of the most essential lodash functions.
- [@microsoft/sp-tslint-rules](https://www.npmjs.com/package/@microsoft/sp-tslint-rules). Defines custom tslint rules for usage with SharePoint client-side projects.
- [@microsoft/office-ui-fabric-react-bundle](https://www.npmjs.com/package/@microsoft/office-ui-fabric-react-bundle). Provides a custom bundle of [office-ui-fabric-react](https://www.npmjs.com/package/office-ui-fabric-react) that is optimized for use with the SharePoint Framework's module loader.

### Common build tool packages

Along with the SharePoint Framework packages, a common set of build tools is also used to perform build tasks such as compiling TypeScript code to JavaScript and converting SCSS to CSS.

The following common npm build tools packages are in the SharePoint Framework:

- [@microsoft/sp-build-core-tasks](https://www.npmjs.com/package/@microsoft/sp-build-core-tasks). A collection of tasks for the SharePoint Framework build system, which is based on gulp. The **sp-build-core-tasks** package implements operations specific to SharePoint, such as packaging solutions and writing manifests.
- [@microsoft/sp-build-web](https://www.npmjs.com/package/@microsoft/sp-build-web). Imports and configures a set of build tasks that are appropriate for a build target that runs in a web browser (as opposed to a Node.js environment). This package is intended to be imported directly by a gulp file that uses the SharePoint Framework build system.
- [@microsoft/gulp-core-build](https://www.npmjs.com/package/@microsoft/gulp-core-build). The core gulp build tasks for building TypeScript, HTML, less, and other build formats. This package depends on several other npm packages that contain the following tasks:
  - [@microsoft/gulp-core-build-typescript](https://www.npmjs.com/package/@microsoft/gulp-core-build-typescript)
  - [@microsoft/gulp-core-build-sass](https://www.npmjs.com/package/@microsoft/gulp-core-build-sass)
  - [@microsoft/gulp-core-build-webpack](https://www.npmjs.com/package/@microsoft/gulp-core-build-webpack)
  - [@microsoft/gulp-core-build-serve](https://www.npmjs.com/package/@microsoft/gulp-core-build-serve)
  - [@microsoft/gulp-core-build-karma](https://www.npmjs.com/package/@microsoft/gulp-core-build-karma)
  - [@microsoft/gulp-core-build-mocha](https://www.npmjs.com/package/@microsoft/gulp-core-build-mocha)
[@microsoft/loader-cased-file](https://www.npmjs.com/package/@microsoft/loader-cased-file). A wrapper for webpack's [file-loader](https://www.npmjs.com/package/file-loader) that can be used to modify the casing of the resulting file name. This is useful in some scenarios, such as when using a content delivery network (CDN) that only allows lowercase file names.
- [@microsoft/loader-load-themed-styles](https://www.npmjs.com/package/@microsoft/loader-load-themed-styles). A loader that wraps the loading of CSS in script equivalent to `require('load-themed-styles').loadStyles( /* css text */ )`. It is designed to be a replacement for style-loader.
- [@microsoft/loader-raw-script](https://www.npmjs.com/package/@microsoft/loader-raw-script). A loader that loads a script file's contents directly in a webpack bundle by using an `eval(…)`.
- [@microsoft/loader-set-webpack-public-path](https://www.npmjs.com/package/@microsoft/loader-set-webpack-public-path). A loader that sets the __webpack_public_path__ variable to a value specified in the arguments, optionally appended to the SystemJS baseURL property.

## Scaffold a new client-side project

The SharePoint generator scaffolds a client-side project with a web part. The generator also downloads and configures the required toolchain components for the specified client-side project.

### Install npm packages

The generator installs required npm packages locally in the project folder. npm allows you to install a package either locally to your project or globally. There are benefits to both, but the [general guidance](https://nodejs.org/en/blog/npm/npm-1-0-global-vs-local-installation/) is to install the npm packages locally if your code depends on those package modules. In the case of a web part project, your web part code depends on the various SharePoint and common build packages, and thus requires those packages to be installed locally.

As the packages are installed locally, npm also installs the dependencies associated with each package. You can find the packages installed under the **node_modules** folder in your project folder. This folder contains the packages along with all their dependencies. It is ideal that this folder contains dozens to hundreds of folders, because npm packages are always broken down to smaller modules, which result in dozens to hundreds of packages being installed. The key SharePoint Framework packages are located under the **node_modules\@microsoft** folder. The **\@microsoft** is an npm scope that collectively represents [packages published by Microsoft](https://www.npmjs.com/~microsoft).

Every time you create a new project using the generator, the generator installs the SharePoint Framework packages along with its dependencies for that specific project locally. In this way, npm makes it easier to manage your web part projects without affecting other projects in the local dev environment.

### Specify dependencies with package.json

The **package.json** file in the client-side project specifies the list of dependencies the project depends on. The list defines what dependencies to install. As described earlier, each dependency could contain several more. npm allows you to define both runtime and build dependencies for your package by using the `dependencies` and `devDependencies` properties. The `devDependencies` property is used when you want to use that module in your code as in the case of building web parts.

The following is the **package.json** of the [helloworld-webpart](../web-parts/get-started/build-a-hello-world-web-part.md).

```json
{
  "name": "helloword-webpart",
  "version": "0.0.1",
  "private": true,
  "engines": {
    "node": ">=0.10.0"
  },
  "dependencies": {
    "@microsoft/sp-client-base": "~1.0.0",
    "@microsoft/sp-core-library": "~1.0.0",
    "@microsoft/sp-webpart-base": "~1.0.0",
    "@types/webpack-env": ">=1.12.1 <1.14.0"
  },
  "devDependencies": {
    "@microsoft/sp-build-web": "~1.0.0",
    "@microsoft/sp-module-interfaces": "~1.0.0",
    "@microsoft/sp-webpart-workbench": "~1.0.0",
    "gulp": "~3.9.1",
    "@types/chai": ">=3.4.34 <3.6.0",
    "@types/mocha": ">=2.2.33 <2.6.0"
  },
  "scripts": {
    "build": "gulp bundle",
    "clean": "gulp clean",
    "test": "gulp test"
  }
}
```

While there is lot of packages installed for the project, they are required only for building the web part in the dev environment. With the help of these packages, you can depend on the modules, and you can build, compile, bundle, and package your web part for deployment. The final minified bundled version of the web part that you deploy to a CDN server or SharePoint does not include any of these packages. That said, you can also configure to include certain modules depending on your requirements. For more information, see [Add an external library to a web part](../web-parts/basics/add-an-external-library.md).

### Work with source control systems

As project dependencies increase, the number of packages to install also increases. You don’t want to check in the **node_modules** folder, which contains all the dependencies, to your source control system. You should exclude the **node_modules** from the list of files to ignore during check-ins.

If you are using **git** as your source control system, the Yeoman scaffolded web part project includes a **.gitignore** file that excludes the **node_modules** folder, among other things.

When you check out, or clone, the web part project from your source control system the first time, run the command to initialize and install all the project dependencies locally:

```console
npm install
```

After you run the command, npm scans the **package.json** file and installs the required dependencies.

### Update developer information

Starting from version 1.11, the solution manifest defined in the **package-solution.json** file has been extended with the `developer` section that allows you to store additional information about your organization. Information from this section is validated when you publish your solution to the marketplace. Even if you're building a solution for internal use, it's recommended that you fill in the different properties in your solution manifest. If you specify this information, in the future you might get access to additional insights into the usage of your application.

> [!IMPORTANT]
> If you choose to expose your web parts in Microsoft Teams, users will see the information from the `developer` section when installing your app in Teams.

> [!IMPORTANT]
> Developer section is required to contain valid information for any SharePoint Framework solution which will be available from the Office store or from AppSource.

Following properties are available as a part of the `developer` section:

  Attribute    |                                                         Description                                                         |          Mandatory
-------------- | --------------------------------------------------------------------------------------------------------------------------- | ---------------------------
`name`         | Name of the organization that built the application                                                                         | Yes
`websiteUrl`   | URL of a website with additional information about the application                                                          | Yes
`mpnId`        | Microsoft Partner Network ID (more details on [MS Partner Network](/partner-center/mpn-overview)) | No (but highly recommended)
`privacyUrl`   | Privacy statement URL                                                                                                       | Yes
`termsOfUseUrl` | Terms of use URL                                                                                                            | Yes

## Build tasks

The SharePoint Framework uses [gulp](http://gulpjs.com/) as its task runner to handle processes like the following:

- Bundle and minify JavaScript and CSS files.
- Run tools to call the bundling and minification tasks before each build.
- Compile LESS or Sass files to CSS.
- Compile TypeScript files to JavaScript.

The toolchain consists of the following gulp tasks defined in the [@microsoft/sp-build-core-tasks](https://www.npmjs.com/package/@microsoft/sp-build-core-tasks) package:

- **build** - Builds the client-side solution project.
- **bundle** - Bundles the client-side solution project entry point and all its dependencies into a single JavaScript file.
- **serve** - Serves the client-side solution project and assets from the local machine.
- **clean** - Cleans the client-side solution project's build artifacts from the previous build and from the build target directories (lib and dist).
- **test** - Runs unit tests, if available, for the client-side solution project.
- **package-solution** - Packages the client-side solution into a SharePoint package.
- **deploy-azure-storage** - Deploys client-side solution project assets to Azure Storage.

To initiate different tasks, append the task name with the gulp command. For example, to compile and then preview your web part in the SharePoint Workbench, run the following command:

```console
gulp serve
```

> [!NOTE]
> You cannot execute multiple tasks at the same time.

The **serve** task runs the different tasks and finally launches SharePoint Workbench.

![gulp serve task](../../images/toolchain-gulp-serve-task.png)

### Build targets

In the previous screenshot, you can see that the task indicates your build target as follows:

```console
Build target: DEBUG
```

If no parameter is specified, the commands target the BUILD mode. If the `ship` parameter is specified, the commands target the SHIP mode.

Usually, when your web part project is ready to ship or deploy in a production server, you target SHIP. For other scenarios, such as testing and debugging, you would target BUILD. The SHIP target also ensures that the minified version of the web part bundle is built.

To target SHIP mode, append the task with `--ship`:

```console
gulp --ship
```

In DEBUG mode, the build tasks copy all of the web part assets, including the web part bundle, into the **dist** folder.

In SHIP mode, the build tasks copy all of the web part assets, including the web part bundle, into the **temp\deploy** folder.

## See also

- [SharePoint Framework development tools and libraries](../tools-and-libraries.md)
- [Scaffold projects by using Yeoman SharePoint generator](scaffolding-projects-using-yeoman-sharepoint-generator.md)
- [SharePoint Framework Overview](../sharepoint-framework-overview.md)
