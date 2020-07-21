文件监听是在发现源码发生变化时，自动重新构建岀新的输岀文件。

`webpack`开启监听模式，有两种方式：
1. 启动`webpack`命令时，带上`-watch`参数
2. 在配置`webpack.config.js`中设置`watch:true`

在`package.json`文件里`scripts`中添加一个`watch`的命令（需要手动刷新浏览器）

```js
 "scripts": {
    "watch": "webpack --watch"
  }
```

## `webpack`文件监听原理分析

轮询判断文件的最后编辑时间是否变化

某个文件发生了变化，并不会立刻告诉监听者，而是先缓存起来，等`aggregateTimeout`

```js
module.export = {
    //默认 false，也就是不开启
    watch: true,
    //只有开启监听模式时, watchOptions 才有意义
    wathcOptions: {
        //默认为空，不监听的文件或者文件夹，支持正则匹配，忽略 node_modules 文件监听的性能会有所提升
        ignored: /node_modules/,
        //监听到变化发生后会等 300ms 再去执行，默认300ms
        aggregateTimeout: 300,
        //判断文件是否发生变化是通过不停询问系统指定文件有没有变化实现的，默认每秒问 1000 次
        poll: 1000
    }
}
```

## 热更新

需要安装`webpack-dev-server`（可以不用安装）

`WDS`不刷新浏览器

`WDS`不输出文件，而是放在内存中

使用`HotModuleReplacementPlugin`插件，它是`webpack`自带的。

`webpack.config.js`：

```js
const webpack = require("webpack");

module.export = {
    plugins: [new webpack.HotModuleReplacementPlugin()],  
    devServer: {
        contentBase: path.join(__dirname, "dist"),
        hot: true
    }
}
```

`package.json`：

```js
"scripts": {
    "dev": "webpack-dev-server --open"
}
```