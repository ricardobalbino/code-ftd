# Setup Prettier

## Introduction

Having clean and well formatted code is crucial. In order to avoid manual effort for coding, code review and corrections, it is recommended to have tooling support. `prettier` is an opinionated tool which simplifies frontcode formatting and nicely integrates into Visual Studio Code.

## Configuration

1. Add a `.prettierrc` file to your project (for example in the Web folder).
2. Add the following content.
```json
{
  "trailingComma": "es5",
  "tabWidth": 4,
  "semi": true,
  "singleQuote": false,
  "printWidth": 140
}
```
3. Install the `Prettier - Code formatter` extension in Visual Studio Code.
4. Optional. Modify configuration to taste as described [here](https://prettier.io/docs/en/configuration.html).
