## `css`样式自动补齐

安装`postcss-loader`，它有一个插件`autoprefixer`可以自动补齐`css`的前缀。

`autoprefixer`它是后置处理器，它和预处理器不同，预处理器是在打包的时候处理，`autoprefixer`是在代码处理好之后，样式已经生成了，在进行后置处理。

```js
module.exports = {
  module: {
    rules: [
      {
        test: /.less$/,
        use: [
          MiniCssExtractPlugin.loader,
          "css-loader",
          "less-loader",
          {
            loader: "postcss-loader",
            options: {
              plugins: () => [
                require("autoprefixer")({
                  overrideBrowserslist: ["last 2 version", ">1%", "ios 7"]      //最新的两个版本，用户大于1%，且兼容到 ios7
                })
              ]
            }
          }
        ]
      }
    ]
  }
};
```

## `px`自动转成`rem`

需要安装`px2rem-loader`和`lib-flexible`。

`lib-flexible`是页面需要直接引用的，安装时用`-S`。

```js
module.exports = {
    module: {
        rules: [
          {
            loader: "px2rem-loader",
            options: {
              remUnit: 75,      //一个 rem 对应多少个 px
              remPrecision: 8   //px 转 rem 保留多少个小数点
            }
          }  
        ]
    }
}
```