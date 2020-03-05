# Linting

> **Reference:**
>
> [How to setup ESLint and Prettier for your React apps](https://thomlom.dev/setup-eslint-prettier-react/) &mdash; Article by <cite>Thomas Lombart</cite>

## Why

The first layer of safety in your app is the linter!

- statically (without executing) analyses your code
- looks for:
  - common errors
  - bad practices

## [ESLint](https://eslint.org/)

A pluggable linting utility for JavaScript which runs linting [rules](https://eslint.org/docs/rules/) that may trigger warning or errors to let you know if your code is right or wrong.

### Install ESLint

```bash
# Install as dev dependency for your project
yarn add -D eslint
```

(_Optional_) [VS Code ESLint Extension](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)

### Configure ESLint

- Either...
  - create `.eslintrc` OR
  - add `eslintConfig` key to `package.json`
- Example options:
  - parserOptions
  - environment
  - globals
  - plugins - eg. `eslint-plugin-react`
  - rules - `off` | `warn` | `error`
  - extends

#### In-Line Configuration

If the linter is barking at a problem that you **WANT** to keep, modify your configuration in-line with this special comment*:

```javascript
// eslint-disable-next-line
```

***Do not abuse this!**

#### ESLint Configuration Examples

- env: `require` instead of `import`
- rules: disables use of `console.log`

```json
{
    "env": {
        "commonjs": true,
        "es6": true,
        "node": true
    },
    "extends": "eslint:recommended",
    "parserOptions": {
        "ecmaVersion": 2018
    },
    "rules": {
        "no-console": "error"
    }
}
```

- env: include all the global variables from these
- rules:
  - generate warning when `console.log` is used
  - disables use of `eval`
  - error if import statement comes after non-import statements

```json
{
    "env": {
        "browser": true,
        "jest": true,
        "es6": true
    },
    "plugins": ["import"],
    "extends": "eslint:recommended",
    "parserOptions": {
        "ecmaVersion": 2018,
        "sourceType": "module"
    },
    "rules": {
        "no-console": "warn",
        "no-eval": "error",
        "import/first": "error"
    }
}
```

- [`create-react-app`'s configuration](https://github.com/facebook/create-react-app/blob/master/packages/eslint-config-react-app/index.js)

### ESLint CLI

Script examples for `package.json`:

```json
{
    "scripts": {
        // lint entire codebase
        "lint": "eslint .",
        // lint and try to fix
        "lint:fix": "eslint --fix .",
        // lint only js and jsx files
        "lint:js": "eslint '**/*.js?(x)' --fix",
    }
}
```

- Here you can:
  - lint entire codebase
  - lint and try to fix
  - lint only js and jsx files
