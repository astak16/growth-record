
多页面打包基本思路

每个页面对应一个`entry`，一个`html-webpack-plugin`

缺点：每次新增或删除页面需要改`webpack`配置

通用方案，利用`glob.sync`，所有页面都放在`src`下，所有入口文件都叫`index.js`

```js
entry: glob.sync(path.join(__dirname,'./src/*/index.js'))
```

安装`glob`。

```js
const glob = require("glob");
const HtmlWebpackPlugin = require("html-webpack-plugin");

const setMPA = () => {
  const entry = {};
  const htmlWebpackPlugins = [];
  const entryFiles = glob.sync(path.join(__dirname, "./src/*/index.js"));
  entryFiles.forEach(entryFile => {
    const match = entryFile.match(/src\/(.*)\/index\.js/);
    const pageName = match && match[1]

    entry[pageName] = entryFile;
    htmlWebpackPlugins.push(
      new HtmlWebpackPlugin({
        template: path.join(__dirname, `src/${pageName}/index.html`),
        filename: `${pageName}.html`,
        chunks: [pageName],
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
    );
  });

  return {
    entry,
    htmlWebpackPlugins
  };
};

const { entry, htmlWebpackPlugins } = setMPA();
module.exports = {
    entry,
    plugins: [].concat(htmlWebpackPlugins)
}
```