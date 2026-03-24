# Typescript

## Introduction

With the `BizApps Core Accelerator` you'll write any frontend code using Typescript. In addition we're using [babel.js](https://babeljs.io/) to transpile the resulting javascript down to ES2015 or even ES5 code (depending on your browser configuration - see `babel.config.js`). This gives us the ability to use all the newest Typescript syntactic sugare & features without needing to wait for full browser support. This document provides a quick introduction to the most important new language features and syntax. For a full feature list please take a look at [this](https://www.javascripttutorial.net/es-next/) site.

You can learn more about how bundling works in the BizApps Core Accelerator [here](Bundling.md).

## Compatibility

As briefly described we utilize transpilation of Typescript/Javascript code down until ES5 Javascript code. This is being done be babel.js and our gulp/webpack bundling pipeline. Based on configurable targeted browsers the pipeline will integrate code that implements a feature on web browsers that do not support the feature. This approach is called Polyfill. You can learn more about it [here](https://en.wikipedia.org/wiki/Polyfill_(programming)).

## Features

For information and documentation on how to write Typescript code head on over to the official [TypescriptLang](https://www.typescriptlang.org/docs/handbook) page.

Most prominent features/syntaxes are:
- [Classes](https://www.tutorialsteacher.com/typescript/typescript-class)
- [Data modifiers](https://www.tutorialsteacher.com/typescript/data-modifiers)
- [Interfaces](https://www.tutorialsteacher.com/typescript/typescript-interface)
- [Type annotations](https://www.tutorialsteacher.com/typescript/type-annotation)
- [Generics](https://www.tutorialsteacher.com/typescript/typescript-generic)
- [Arrow Functions](https://www.tutorialsteacher.com/typescript/arrow-function)
- [for loops](https://www.tutorialsteacher.com/typescript/for-loop)
- [Async/Await](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-1-7.html)

### Modules

Typescript utilizes modules. These enable a cleaner way of developing large Typescript applications.
Please see [this](https://www.tutorialsteacher.com/typescript/typescript-module) detailed explanation for more details.

Modules are stored in files and there is always a single module per file. This can either export a default value or have several named exports.

These exported values can be imported by other modules using the file path and name of the exported value.

```ts
// File helper.js

// Named Export "HelperClass"
export class HelperClass {
    static doStuff(value: string): string {
        // do some stuff
        return newValue;
    }
}
```

```ts
// File main.js

import { HelperClass } from "./helper.ts";

const someValue = "old value";
const newValue = HelperClass.doStuff(someValue);
```

### var vs. let vs. const

TypeScript follows the same rules as JavaScript for variable declarations. Variables can be declared using: var, let, and const.

`const` means that the variable cannot be reassigned:

```ts
const value = 3;
value = 5; // ERROR
```

while `let` allows you to reassign values:

```ts
let value = 3;
value = 5;
```

`var` behavior and scope is the same as with plain Javascript.

## Usage of Javascript

Whilst Typescript is the default language for frontend code in the BCA and recommended by Microsoft (see for example [PCF development](https://docs.microsoft.com/en-us/power-apps/developer/component-framework/implementing-controls-using-typescript)), it is also possible to use Javascript.

Javascript and Typescript can be mixed like it is the same thing. One service might be written in JavaScript whereas another one is already migrated to Typescript.

A webresource (or PCF component) can use both these services without any issues.

For more information see:

* [https://www.typescriptlang.org/tsconfig#JavaScript_Support_6247](https://www.typescriptlang.org/tsconfig#JavaScript_Support_6247)
* [https://www.typescriptlang.org/docs/handbook/migrating-from-javascript.html](https://www.typescriptlang.org/docs/handbook/migrating-from-javascript.html)
