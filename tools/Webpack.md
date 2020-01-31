# Webpack

> **Reference**: YouTube video on `freeCodeCamp.org`'s channel by Colt Steele
>
> - video: [Learn Webpack - Full Tutorial for Beginners](https://www.youtube.com/watch?v=MpGLUVbqoYQ)
> - repo: [https://github.com/Colt/webpack-demo-app](https://github.com/Colt/webpack-demo-app)

At its core, Webpack is a static module bundler for modern JavaScript applications. When Webpack processes your application, it internally builds a dependency graph which maps every module your project needs and generates one or more bundles. [Source: <cite><https://webpack.js.org/concepts/></cite>]

## Installing and Running Webpack and Webpack-CLI

First, install webpack and add script to `package.json`

```bash
npm init -y

# devDependencies
npm install --save-dev webpack webpack-cli
```

```json
...
"scripts": {
    "start": "webpack"
},
...
```

- Create a `./src` dir and add an `index.js` with some code
- If you run `npm start` you will find your code has been output to `./dist/main.js`

## Imports, Exports, & Webpack Modules

`./src/index.js` acts as a window into our app for Webpack. The file can be as simple as:

```javascript
import { run } from "./app/app"`
run();
```

Webpack will drill down and see what each import relies on and package it up in the correct order.

## Configuring Webpack

Update the script for Webpack and add `webpack.config.js` to the root.

```json
...
"scripts": {
    "start": "webpack --config webpack.config.js"
},
...
```

```javascript
/// ./webpack.config.js
const path = require("path");

module.exports = {
    mode: "development",
    //devtool: "none",
    entry: "./src/index.js",
    output: {
        filename: "main.js",
        path: path.resolve(__dirname, "dist")
    }
};
```

- `entry` specifies the entry point (window into our app)
- `output` specifies the output location for the bundle
- `mode` is "production" by default but if set to "development" it will stop minifying the code for better troubleshooting of bugs during dev time
- `devtool` when set to "none" will get rid of the `eval()` formatting of your bundled code

## Loaders, CSS, & SASS

Loaders are used to let Webpack handle different types of files which are not JavaScript.

---

Add loader devDependencies and configure them in `webpack.config.js`

```bash
# devDependencies
npm install --save-dev css-loader style-loader
```

```javascript
/// webpack.config.js (module.exports)
module: {
    rules: [
        // uses these loaders ONLY on `*.css` files
        {
            test: /\.css$/,
            use: ["style-loader", "css-loader"]
        }
    ];
}
```

- add `rules` (_Array_) inside `modules` (_Object_)
  - **NOTE:** loaders will get loaded in **reverse order**
- `css-loader` takes your CSS and turns it into JS
- `style-loader` will inject the "css-loaded" JS to the DOM via `<style>` tag

### Adding SASS

```bash
# devDependencies
npm install --save-dev node-sass sass-loader
```

```javascript
/// webpack.config.js (module.exports)
module: {
    rules: [
        {
            test: /\.scss$/,
            use: [
                "style-loader", //3. Inject styles into DOM
                "css-loader", //2. Turns css into commonjs
                "sass-loader" //1. Turns sass into css
            ]
        }
    ];
}
```

- `sass-loader` loads a SASS or SCSS file and compiles it into CSS
  - requires `css-loader` after the fact to compile into JS module
  - recommended to use `mini-css-extract-plugin` to extract it out into a separate file

## Cache Busting and Plugins

Add context hashed suffixes to your files. To make life easier, get Webpack to generate your HTML file and auto import your context hashed files. For this, we use plugins.

The `plugins` option is used to customize the Webpack build process in a variety of ways. Webpack comes with a variety built-in plugins available under `webpack.[plugin-name]`. [Source: <cite><https://webpack.js.org/configuration/plugins/></cite>]

```bash
# devDependencies
npm install --save-dev html-webpack-plugin
```

```javascript
/// ./webpack.config.js
const path = require("path");
var HtmlWebpackPlugin = require("html-webpack-plugin");

module.exports = {
    ...,
    output: {
        filename: "main-[contentHash].js",
        path: path.resolve(__dirname, "dist")
    },
    plugins: [
        new HtmlWebpackPlugin({
            template: "./src/template.html"
        })
    ],
    ...
};
```

- create `./src/template.html` and add all the markup you want to include
- adding `[contentHash]` to the output filename will generate the suffix for you
- `HtmlWebpackPlugin`: simplifies creation of HTML files to serve your webpack bundles.
  - This is especially useful for webpack bundles that include a hash in the filename which changes every compilation.
  - You can either let the plugin generate an HTML file for you, supply your own template using lodash templates, or use your own loader.
  - [https://webpack.js.org/plugins/html-webpack-plugin/](https://webpack.js.org/plugins/html-webpack-plugin/)

## Splitting Dev & Production

```bash
# devDependencies
npm install --save-dev webpack-dev-server webpack-merge
```

- `webpack-dev-server` serves a webpack app in memory (doesn't use `./dist`)
- `webpack-merge` provides a merge function that concatenates arrays and merges objects creating a new object

```json
"scripts": {
    "start": "webpack-dev-server --config webpack.dev.js --open",
    "build": "webpack --config webpack.prod.js"
}
```

```javascript
/// webpack.common.js
...
module.exports = {
    entry: "./src/index.js",
    module: {
        rules: [
            {
                test: /\.scss$/,
                use: [
                    "style-loader", //3. Inject styles into DOM
                    "css-loader", //2. Turns css into commonjs
                    "sass-loader" //1. Turns sass into css
                ]
            }
        ];
    },
    plugins: [
        new HtmlWebpackPlugin({
            template: "./src/template.html"
        })
    ],
    ...
};
```

```javascript
/// webpack.dev.js
const path = require("path");
const common = require("./webpack.common");
const merge = require("webpack-merge");

module.exports = merge(common, {
    mode: "development",
    devtool: "none",
    output: {
        filename: "main.js",
        path: path.resolve(__dirname, "dist")
    }
});
```

```javascript
/// webpack.prod.js
const path = require("path");
const common = require("./webpack.common");
const merge = require("webpack-merge");

module.exports = merge(common, {
    mode: "production",
    output: {
        filename: "main.[contentHash].js",
        path: path.resolve(__dirname, "dist")
    }
});
```

## Html-loader, File-loader, & Clean-webpack

```bash
# devDependencies
npm install --save-dev file-loader html-loader

# dependencies
npm install --save clean-webpack-plugin
```

- `html-loader` exports HTML as string. HTML is minimized when the compiler demands.
- `file-loader` resolves `import`/`require()` on a file into a url and emits the file into the output directory.
- `html-loader` will encounter the `src` attribute (eg for ah `<img>` tag) and `file-loader` will take control on how to handle the source file based on the configured `test` option.

```javascript
/// webpack.common.js (module.exports)
module: {
    rules: [
        ...,
        {
            test: /\.html$/,
            use: ["html-loader"]
        },
        {
            test: /\.(svg|png|jpg|gif)$/,
            use: {
                loader: "file-loader",
                options: {
                    esModule: false,
                    name: "[name].[hash].[ext]",
                    outputPath: "imgs"
                }
            }
        }
    ];
}
```

- `clean-webpack-plugin` will delete the `./dist` dir every build
  - can add it to `dev` config but in this case we are using `webpack-dev-server` for dev which uses memory instead of a `dist` folder

```javascript
/// webpack.prod.js
...
const CleanWebpackPlugin = require("clean-webpack-plugin");

module.exports = merge(common, {
    ...,
    plugins: [new CleanWebpackPlugin()]
});
```

## Multiple Entrypoints & Vendor.js

Sometimes you might want to separate out our app code from vendor code. Vendor code changes much less frequently so a vendor bundle can be cached while the main bundle can cache bust as needed.

---

- Create `./src/vendor.js`
  - import any required devDependencies into it
- Add an additional entry point
- Update output filenames if needed in respective config files

```javascript
/// webpack.common.js
module.exports = {
    entry: {
        main: "./src/index.js",
        vendor: "./src/vendor.js"
    },
    ...
```

```javascript
/// webpack.dev.js
module.exports = merge(common, {
    ...
    output: {
        filename: "[name].bundle.js",
        ...
    }
    ...
```

```javascript
/// webpack.prod.js
module.exports = merge(common, {
    ...
    output: {
        filename: "[name].[contentHash].bundle.js",
        ...
    }
    ...
```

## Extract CSS & Minify HTML/CSS/JS

Separate CSS for better performance. Import it into `head` rather than before closing `body` tag to avoid the flash of unstyled content (FOUC). On production, instead of injecting CSS with `style-loader` use `mini-css-extract-plugin` to generate the separated file.

---

- install the devDependencies
- if needed, move the CSS related rules out from the `common` config file to the `dev` one
- create the new rules in `prod` (replace `style-loader`)

```bash
# devDependencies
npm install --save-dev mini-css-extract-plugin
```

```javascript
/// webpack.prod.js
...
const MiniCssExtractPlugin = require("mini-css-extract-plugin");

module.exports = merge(common, {
    ...
    plugins: [
        new MiniCssExtractPlugin({ filename: "[name].[contentHash].css" }),
        new CleanWebpackPlugin()
    ],
    module: {
        rules: [
            {
                test: /\.scss$/,
                use: [
                    MiniCssExtractPlugin.loader, // Extract css into files
                    "css-loader",
                    "sass-loader"
                ]
            }
        ]
    }
});
```

### Minification

- Improves performance even more by minifying production code.
- add `optimization` property to `prod` config
- specifiy the `minimizer` for CSS & HTML
  - CSS: `optimize-css-assets-webpack-plugin`
  - HTML: `html-webpack-plugin` (already using this for template & cache busting)
    - may need to strip out of `common` config and spread to `dev`/`prod`
  - **NOTE:** specifiying the `minimizer` overrides the Webpack defaults (`terser-webpack-plugin`)
    - must re-add or replace the minimizer for JS
    - if re-adding, no need to add package as Webpack is already using it

```bash
# devDependencies
npm install --save-dev optimize-css-assets-webpack-plugin
```

```javascript
/// webpack.prod.js
...
const OptimizeCssAssetsPlugin = require("optimize-css-assets-webpack-plugin");
const TerserPlugin = require("terser-webpack-plugin");
var HtmlWebpackPlugin = require("html-webpack-plugin");

module.exports = merge(common, {
    ...
    optimization: {
        minimizer: [
            new OptimizeCssAssetsPlugin(),
            new TerserPlugin(),
            new HtmlWebpackPlugin({
                template: "./src/template.html",
                minify: {
                    removeAttributeQuotes: true,
                    collapseWhitespace: true,
                    removeComments: true
                }
            })
        ]
    }
});
```

## Appendix - Example Files

```javascript
/// webpack.common.js
module.exports = {
    entry: {
        main: "./src/index.js",
        vendor: "./src/vendor.js"
    },
    module: {
        rules: [
            {
                test: /\.html$/,
                use: ["html-loader"]
            },
            {
                test: /\.(svg|png|jpg|gif)$/,
                use: {
                    loader: "file-loader",
                    options: {
                        esModule: false,
                        name: "[name].[hash].[ext]",
                        outputPath: "assets"
                    }
                }
            }
        ]
    }
};
```

```javascript
/// webpack.dev.js
const path = require("path");
const common = require("./webpack.common");
const merge = require("webpack-merge");
var HtmlWebpackPlugin = require("html-webpack-plugin");

module.exports = merge(common, {
    mode: "development",
    output: {
        filename: "[name].bundle.js",
        path: path.resolve(__dirname, "dist")
    },
    plugins:[
        new HtmlWebpackPlugin({
            template: "./src/template.html"
        })
    ],
    module: {
        rules: [
            {
                test: /\.scss$/,
                use: [
                    "style-loader", //3. Inject styles into DOM
                    "css-loader",   //2. Turns css into commonjs
                    "sass-loader"   //1. Turns sass into css
                ]
            }
        ]
    }
});
```

```javascript
/// webpack.prod.js
const path = require("path");
const common = require("./webpack.common");
const merge = require("webpack-merge");
const CleanWebpackPlugin = require("clean-webpack-plugin");
const MiniCssExtractPlugin = require("mini-css-extract-plugin");
const OptimizeCssAssetsPlugin = require("optimize-css-assets-webpack-plugin");
const TerserPlugin = require("terser-webpack-plugin");
var HtmlWebpackPlugin = require("html-webpack-plugin");

module.exports = merge(common, {
    mode: "production",
    output: {
        filename: "[name].[contentHash].bundle.js",
        path: path.resolve(__dirname, "dist")
    },
    optimization: {
        minimizer: [
            new OptimizeCssAssetsPlugin(),
            new TerserPlugin(),
            new HtmlWebpackPlugin({
                template: "./src/template.html",
                minify: {
                    removeAttributeQuotes: true,
                    collapseWhitespace: true,
                    removeComments: true
                }
            })
        ]
    },
    plugins: [
        new MiniCssExtractPlugin({ filename: "[name].[contentHash].css" }),
        new CleanWebpackPlugin()
    ],
    module: {
        rules: [
            {
                test: /\.scss$/,
                use: [
                    MiniCssExtractPlugin.loader, // Extract css into files
                    "css-loader",
                    "sass-loader"
                ]
            }
        ]
    }
});
```
