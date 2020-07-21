代码压缩主要有三块`html`压缩，`css`压缩，`js`压缩。

## `js`压缩

`webpack`内置了`uglifyjs-webpack-plugin`插件。默认打包出来的文件都是已经被压缩过的了，这里不需要做额外的配置。

可以手动安装这个插件，然后可以额外的设置一下配置，比如开启并行压缩，需要将`parallel`设置`true`

```js
const UglifyJsPlugin = require('uglifyjs-webpack-plugin');
module.exports = {
    optimization: {
        minimizer: [
            new UglifyJsPlugin({
                parallel: true,//开启多进程
            }),
        ],
    },
};
```

## `css`压缩

需要安装`optimize-css-assets-webpack-plugin`，同时安装`css`预处理器`cssnano`。

```js
const OptimizeCssAssetsPlugin = require("optimize-css-assets-webpack-plugin");
module.export = {
    plugins: [
        new OptimizeCssAssetsPlugin({
          assetNameRegExp: /\.css$/g,
          cssProcessor: require("cssnano")      //需要安装预处理器 cssnano
        })
      ]
}
```

## `html`压缩

需要安装`html-webpack-plugin`，设置压缩参数。

对于多页面应用，现在是一个页面就要写一个插件，当然会有更好的处理方式

```js
const HtmlWebpackPlugin = require("html-webpack-plugin");
module.export = {
    plugins: [
        new HtmlWebpackPlugin({
          template: path.join(__dirname, "src/search.html"),    //html 模版所在的位置，模版里面可以使用 ejs 的语法
          filename: "search.html",              //打包出来后的 html 名称
          chunks: ["search"],           //打包出来后的 html 要使用那些 chunk
          inject: true,                //设置为 true，打包后的 js css 会自动注入到 HTML 中
          minify: {
            html5: true,
            collapseWhitespace: true,
            preserveLineBreaks: false,
            minifyCSS: true,
            minifyJS: true,
            removeComments: false
          }
        }),
        new HtmlWebpackPlugin({
          template: path.join(__dirname, "src/index.html"),
          filename: "index.html",
          chunks: ["index"],
          inject: true,
          minify: {
            html5: true,
            collapseWhitespace: true,
            preserveLineBreaks: false,
            minifyCSS: true,
            minifyJS: true,
            removeComments: false
          }
        })
      ]
}
```