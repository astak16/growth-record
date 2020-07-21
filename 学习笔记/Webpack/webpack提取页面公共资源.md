## 基础库分离

思路：将`react`、`react-dom`基础包通过`cdn`引入，不打入`bundle`中。

方法：使用`html-webpack-externals-plugin`

安装`html-webpack-externals-plugin`

`webpack.prod.js`文件：

```js
const HtmlWebpackExternalsPlugin = require("html-webpack-externals-plugin")

module.exports = {
  plugins: [
    new HtmlWebpackExternalsPlugin({
      externals: [
        {
          module: "react",
          entry: "https://11.url.cn/now/lib/16.2.0/react.min.js",
          global: "React"
        },
        {
          module: "react-dom",
          entry: "https://11.url.cn/now/lib/16.2.0/react-dom.min.js",
          global: "ReactDOM"
        }
      ]
    })
  ]
};
```

`html`文件：

```js
<script type="text/javascript" src="https://11.url.cn/now/lib/16.2.0/react.min.js"></script>
<script type="text/javascript" src="https://11.url.cn/now/lib/16.2.0/react-dom.min.js"></script>
```

## 利用`SplitChunksPlugin`进行公共脚本分离

`Webpack4`内置的，替代`CommonsChunkPlugin`插件。

`chunks`参数说明
* `async`异步引入的库进行分离(默认)
* `initial`同步引入的库进行分离
* `all`所有引入的库进行分离(推荐)

```js
module.exports = {
  optimization: {
    splitChunks: {
      chunk: "async",
      minSize: 0,       //抽离公共包，最小的大小，单位是字节
      minChunks: 1,     //公共代码大于 1 处的时候，将它提取出来
      maxSize: 0,       //最大的大小，单位是字节
      maxAsyncRequests: 5,
      maxInitialRequests: 3,    //同时请求异步资源的次数
      automaticNameDelimiter: "~",
      name: true,
      cacheGroups: {
        commons: {
          name: "commons",
          chunks: "all",
          minChunks: 2
        }
      }
    }
  }
};
```

## 利用`SplitChunksPlugin`分离基础包

`test`：匹配出需要分离的包

```js
module.exports = {
  optimization: {
    splitChunks: {
      cacheGroups: {
        commons: {
          test: /(react|react-dom)/,      //设置匹配规则
          name: "vendors",               //设置分离出来的名称
          chunks: "all"
        }
      }
    }
  }
};
```
