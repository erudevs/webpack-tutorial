# Webpack: Cache Busting and HtmlWebpackPlugin

In this part, we are focuses on the configuration needed to ensure files produced by webpack
compilation can remain cached unless our content changed. Webpack provide a method of templating the
filenames using bracketed strings called **substitution**. The `[contenthash]` substitution will add
unique hash based on the content of an asset. When the asset's content changes, `[contenthash]` will
change as well.

In `webpack.config.js` we add `[contenthash]` to the output filename :

```javascript
output: {
  filename: "bundle.[contenthash].js",
  path: path.resolve(__dirname, "dist"),
},
```

When we build our code, webpack will read that `[contenthash]` and then create hashed filename with
something like `bundle.64f57b6ac62971c0def8.js` (it generated by webpack via the hash).

But, how we dynamically call the script tag with the `bundle.js` change as we change our code as we
know that for now, our html structure is something like this :

```html
<script src="./dist/bundle.js"></script>
```

There's when `HtmlWebpackPlugin` comes. According to the webpack docs
https://webpack.js.org/plugins/html-webpack-plugin

> The HtmlWebpackPlugin simplifies creation of HTML files to serve your webpack bundles. This is
> especially useful for webpack bundles that include a hash in the filename which changes every
> compilation. You can either let the plugin generate an HTML file for you, supply your own template
> using lodash templates, or use your own loader.

Install `HtmlWebpackPlugin` :

```npm
npm i --save-dev html-webpack-plugin
```

Import the installed plugin to `webpack.config.js` :

```javascript
const HtmlWebpackPlugin = require("html-webpack-plugin");
```

Add plugins to `module.exports` object :

```javascript
const HtmlWebpackPlugin = require("html-webpack-plugin");

module.exports = {
  // Other codes hided
  plugins: [
    new HtmlWebpackPlugin({
      // template: "link-to-template-file.[html, hbs, ejs, etc]",
      template: "./src/template.html",
    }),
  ],
};
```

Create our template in this case our html file inside `src` folder with the name that matches our
name in config plugin file :

**project**

```
webpack-tutorial
|- /assets
|- /dist
|- /node_modules
|- /src
  |- template.html
|- webpack.config.js
|- package-lock.json
|- package.json
|- readme.md
```

Copy the code from `index.html` at root project and paste to `template.html` inside `src` folder,
and we can remove `script` tag that includes inside html since it'll generated by webpack with
hashed script name when we build our code.

Now, when we changes something in our app and build through webpack, it'll create inside `dist`
folder new `index.html` with the hashed `script` tag.
