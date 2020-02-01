# Create React App From Scratch

Let's try to create your react app without relying on `create-react-app`!

## Hold Up A Minute

I am not a fan of Medium[dot]com but apparently usejournal[dot]com (never heard of it before) is a Medium site?

Anyway, this tutorial works but it is by no means complete or following the best of best practices. This is just an example of how you can go about making your own React app w/o having to use prepackaged toolchains.

### Notes

> **Reference:** [Creating a React App… From Scratch
](https://blog.usejournal.com/creating-a-react-app-from-scratch-f3c693b84658) by Jedai Saboteur
>
> - **NOTE:** at the time of this writing, my environment had the following installed:
>   - **node v13.7.0**
>   - **npm v6.13.6**
>   - **yarn v1.21.1**

## Setup

```bash
# Make your app folder and initialize it
mkdir myapp
cd myapp
yarn init -y

# Create a couple of folders
mkdir public && mkdir src
```

- for this example static assets will go into `./public`
- create `./public/index.html` with the following markup...

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">

    <title>My React App From Scratch</title>
    <meta name="description" content="Demo React app from scratch. No toolchains!" />
</head>

<body>
    <div id="root"></div>

    <noscript>
        You need to enable JavaScript to run this app.
    </noscript>

    <script src="../dist/bundle.js"></script>
</body>

</html>
```

### Babel

```bash
# Install devDependencies
yarn add --dev @babel/core @babel/cli @babel/preset-env @babel/preset-react

# create `.baberc`
touch ./.babelrc
```

- `preset-react` and `preset-env` are both presets that _transform specific flavors of code_
  - in this case, ES6+ and JSX specifically
- edit `.babelrc` with the following...

```json
{
  "presets": ["@babel/env", "@babel/preset-react"]
}
```

### Webpack

For more Webpack references, see [@farhanjiwani/dev-notes/tools/Webpack.md](https://github.com/farhanjiwani/dev-notes/blob/master/tools/Webpack.md). For this example, we will just use a single `webpack.config.js` file.

```bash
# Install devDependencies
yarn add --dev webpack webpack-cli webpack-dev-server style-loader css-loader babel-loader

# create `webpack.config.js`
touch ./webpack.config.js
```

```javascript
/// webpack.config.js
const path = require("path");
const webpack = require("webpack");

module.exports = {
    entry: "./src/index.js",
    mode: "development",
    module: {
        rules: [
            {
                test: /\.(js|jsx)$/,
                exclude: /(node_modules|bower_components)/,
                loader: "babel-loader",
                options: { presets: ["@babel/env"] }
            },
            {
                test: /\.css$/,
                use: ["style-loader", "css-loader"]
            }
        ]
    },
    resolve: { extensions: ["*", ".js", ".jsx"] },
    output: {
        path: path.resolve(__dirname, "dist/"),
        publicPath: "/dist/",
        filename: "bundle.js"
    },
    devServer: {
        contentBase: path.join(__dirname, "public/"),
        port: 3000,
        publicPath: "http://localhost:3000/dist/",
        hotOnly: true
    },
    plugins: [new webpack.HotModuleReplacementPlugin()]
};
```

- `resolve.extensions`: sets the order in which to resolve the extensions
  - this is what enables users to leave off the extension when importing
  - eg. `import MyComponent from "path/to/component"; //component.js`
  - **NOTE:** this overrides Webpack's default array for resolving modules
- `HotModuleReplacementPlugin` (HMR) exchanges, adds, or removes modules while an application is running, without a full reload
  - This can significantly speed up development by
    - retaining app state
    - updates only what was changed
    - can update CSS or JS _instantly_
    - **NOTE:** **HMR SHOULD NEVER BE USED IN PRODUCTION**
- `output.publicPath` is required for the dev server to avoid 404s while `devServer.publicath` tells the dev server where the bundle is

### Finally... React

```bash
# Install devDependencies
yarn add --dev react react-dom

# create `index.js`, `App.js` and `App.css`
touch ./src/index.js && touch ./src/App.js && touch ./src/App.css
```

```javascript
/// ./src/index.js
import React from "react";
import ReactDOM from "react-dom";
import App from "./App";

ReactDOM.render(<App />, document.getElementById("root"));
```

```javascript
/// ./src/App.js
import React, { Component } from "react";
import "./App.css";

const App = () => {
    return (
        <div className="App">
            <h1>Hello, World!</h1>
        </div>
    );
};

export default App;
```

```css
/* App.css */
.App {
    margin: 1rem;
    font-family: Arial, Helvetica, sans-serif;
    background-color: #aaf;
    padding: 0.5rem 1rem 1rem;
}
```

#### React Hot Loader

You can install this as regular dependency — as per the documentation:

> **NOTE:** You can safely install `react-hot-loader` as a regular dependency instead of a dev dependency as it automatically ensures it is not executed in production and the footprint is minimal.

```bash
yarn add react-hot-loader
```

```javascript
/// ./src/App.js
...
import { hot } from "react-hot-loader"; // add this import
...
export default hot(module)(App);        // update the export statement
```

## Start Coding

- Add the following scripts to `package.json`:
  - `"start": "webpack-dev-server"`
  - `"build": "webpack`
- run `yarn start`

**NOTE:** For production, you would want to make a separate Webpack config and use that.

## Appendix - Final `package.json`

```json
{
    "name": "myapp",
    "version": "1.0.0",
    "main": "index.js",
    "scripts": {
        "start": "webpack-dev-server",
        "build": "webpack"
    },
    "license": "MIT",
    "devDependencies": {
        "@babel/cli": "^7.8.4",
        "@babel/core": "^7.8.4",
        "@babel/preset-env": "^7.8.4",
        "@babel/preset-react": "^7.8.3",
        "babel-loader": "^8.0.6",
        "css-loader": "^3.4.2",
        "react": "^16.12.0",
        "react-dom": "^16.12.0",
        "style-loader": "^1.1.3",
        "webpack": "^4.41.5",
        "webpack-cli": "^3.3.10",
        "webpack-dev-server": "^3.10.2"
    },
    "dependencies": {
        "react-hot-loader": "^4.12.19"
    }
}
```
