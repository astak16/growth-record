使用`babel-loader`，`babel`的配置文件是: `.babelrc`。

`bable`有两个比较重要的配置`persets`，`plugins`。

`plugins`可以理解为：一个`plugins`对应一个功能，`presets`可以理解为：一些`bable-plugins`的集合。

## 解析`es6`

解析`es6`只需要安装`presets`就可以了。

需要安装`@babel/core`，`@babel/preset-env`，`babel-loader`

`.babelrc`文件：

```js
{
  "presets": ["@babel/preset-env"]          //解析 es6 的 babel preset 配置
}
```

`webpack.config.js`文件：

```js
module.exports = {
    module: {
        rules: [{ test: /.js$/, use: "babel-loader" }]
    }
}
```

## 解析`React JSX`

安装`react`，`react-dom`，`@babel/preset-react`

`.babelrc`文件：

```js
{
  "presets": ["@babel/preset-react"]        //解析 React 的 babel preset 配置
}
```

`webpack.config.js`文件不变