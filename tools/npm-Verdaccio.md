# Verdaccio

![Verdaccio logo](https://camo.githubusercontent.com/f8c70ef56bccfb75abb24bafad5a0ae20347a74c/68747470733a2f2f63646e2e76657264616363696f2e6465762f726561646d652f76657264616363696f4032782e706e67)

A lightweight private npm proxy registry: <https://www.verdaccio.org/>

> The examples in this document are using Verdaccio **v4.4.2**
>
> - Reference video: [local npm server](https://www.youtube.com/watch?v=vc2wMwcDKOE) on YouTube channel _falconWings_
> - A talk on Verdaccio at a ViennaJS meetup: [Introduction to Verdaccio](https://www.youtube.com/watch?v=hDIFKzmoCaA) on YouTube channel _Pusher_

## Setup and Run

```bash
# Install globally
npm install -g verdaccio
# OR `yarn global add verdaccio`

# Run with default config
verdaccio
# OR use a custom port `verdaccio --listen 9001`
```

This will start a server (default: <http://localhost:4873>) where packages will populate as they are published.

## Package Registration

### Create the Package

- Create your project folder
- Initialize a `package.json` file
- `package.json` needs:
  - **name**: name of your package as it will appear on npm
    - should be the same name as your project folder
  - **description**: optional but recommended
  - **main**: points to the main entrypoint file
- Create the entrypoint and `README.md`
  - `README.md` will be read by Verdaccio for each component on the listing page
  - Alternatively, do any necessary building/bundling of your package and output to the entrypoint location

```bash
mkdir my-package
npm init -y
# OR `yarn init -y`

# Create entry point referenced in `package.json` as well as `README.md`
touch ./lib/index.js && touch README.md
```

```javascript
/// ./lib/index.js
const function myPackageThing(name) {
    console.log(`Hello ${name}!`);
}

module.exports = myPackageThing;
```

```markdown
[///]: <> (README.md)

# My Package Thing

This function will return "Hello _[name]_!" where `[name]` is the string passed to the function.
```

### Publish the Package

```bash
# Start server
verdaccio

# Add user (if needed)
npm adduser --registry http://localhost:4873

# Publish to Verdaccio
npm publish --registry http://localhost:4873
```

Refresh the Verdaccio server to see the package listed.

### Unpublish Packages

```bash
npm unpublish --force my-package --registry http://localhost:4873
```

## Consuming Packages

```bash
mkdir my-project
npm init -y
npm install my-package --registry http://localhost:4873
```

```javascript
/// main.js
const myPackageThing = require(myPackageThing);
myPackageThing("Homer Simpson");
```

Bundle your app and test it in a browser or just run it in Node of applicable.
