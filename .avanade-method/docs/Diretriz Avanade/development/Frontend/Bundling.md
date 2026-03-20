The **BizApps Core Accelerator** utilizes Typescript code which enables fast development times, more readable and easily reusable code.

This code however cannot be directly deployed to Dataverse, as it currently does not support modules. In addition not all browser have full support for all EcmaScript features, so we need to transpile our code down to a supported EcmaScript version.

!!! info
    Note that in order to replace the web resources with the latest bundled code upon solution build, you need to add the names of the bundled `.js` files to the [Configuration.json](../../getting-started/overview/configuration-json.md).

# How to bundle your code

When creating your project, as described in the [Getting Started](../../getting-started/index.md) section, the full bundling pipeline is already set up for you. To bundle your code you have several options:
- Open a command prompt and type `npm run build`
- Inside **Visual Studio** use the **Task Runner Explorer** to execute the `build` task

This command will bundle and transpile all web resources entry points registered in the `bundle.config.js`.

# Bundle in Develop Mode

A develop mode is provided, which does not transpile the code. This enables a better debugging experience. To bundle in the develop mode simply execute the **bundle-dev** task with the **Task Runner Explorer** or from the command line: `npm run build-dev`.

# Register an entry point

The `build` command only bundles and transpiles files, which are listed in the file `bundle.config.js`. It can be found in the root folder of your web project: `Project/Web/bundle.config.js` and should look something like this: 

```js
module.exports = [
    "./src/forms/Account/Form/account.form.contract.js",
    "./src/forms/Account/Ribbon/account.ribbon.contract.js"
];
```

It has a default export, which is an array containing the relative path of all JavaScript files that should be bundled. To register a new entry point, simply add it to the array.

# Setting the Target Browsers

As described earlier the EcmaScript 2018 code has to be transpiled down to earlier EcmaScript versions, to get better browser support. The pipeline decides itself which EcmaScript version to target and which polyfills shall be included based on the configured target browsers. These can be set in the file `babel.config.js`, which is located at the root of the web project: `Project/Web/babel.config.js`:

```js
// Configure Target Browsers
const targetBrowsers = {
    chrome:  "90",
    firefox: "91",
    edge:    "90",
};
```

Simply edit the entries for **Chrome**, **Firefox** and **Edge** or add a new one according to [the Documentation](https://babeljs.io/docs/en/babel-preset-env#targets)

# The Bundled File

The bundled files will always be dropped to the `dist` directory of the web project. This folder structure will be used to deploy the web resources to the Dataverse environment.

The classes and object of the entry point are directly attached to the `window` object. For more information check out the [webpack documentation](https://webpack.js.org/configuration/output/#expose-via-object-assignment).

# Advanced: The pipeline

The bundling pipeline is powered by the popular module bundling engine [webpack](https://webpack.js.org/). This allow for a modular and highly configurable pipeline. The **BizApps Core Accelerator** provides a default webpack configuration, which is sufficient for most projects. However it can easily be expanded or you can create a custom configuration on your own.

As described earlier there are two npm script registered to bundle your code: `build` and `build-dev`. These are registered inside your `package.json` file:

```json
...
"scripts": {
  "build-dev": "node scripts/build/build.js --env dev --clean",
  "build": "node scripts/build/build.js --env prod --clean"
},
....
```

As you can see both these commands point to the `scripts/build/build.js` script. The only difference between these commands is the `env` parameter.

The `build.js` script is a simple wrapper around webpack to output a more readable build status. The script only consists of a few lines:

```js
const { buildDSS } = require("@avanade/dss-sdk/scripts/build.js");  // Import the build script from the DSS npm package
const webpackConfigFunction = require("../webpack/webpack.config"); // Import the webpack config

buildDSS(webpackConfigFunction); // Execute the build task
```

The main part of this script is located in the DSS npm package. The script above only imports the `buildDSS` function and executes it with the webpack config object as a parameter. By default the webpack config is resolved from the file `scripts/webpack/webpack.config.js`. This file decides based on the `env` parameter, which webpack config to use: `webpack.dev.config.js` or `webpack.prod.config.js`. Both of these only contains the values which are different between the `dev` and `prod` environment. Anything else is located in the `webpack.base.config.js`.

# Advanced: The DSS base webpack configuration

You can always take a look at the `webpack.base.config.js` provided in the DSS Core Package with [THIS LINK](https://innersource.visualstudio.com/DSS-Framework/_git/Core?path=%2FWeb%2Fscripts%2Fwebpack.base.config.js&version=GBmaster).
