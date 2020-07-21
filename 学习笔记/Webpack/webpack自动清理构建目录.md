
每次构建的时候不会清理目录，造成构建的输出目录`output`文件越来越多

## 通过`npm scripts`清理构建目录

使用这种方法不是太优雅。

````js
rm -rf / dist & webpack
rimraf. dist & webpack
```

## 自动清理构建项目

需要安装`clean-webpack-plugin`。它会自动清理改动的文件。

```js
const {CleanWebpackPlugin} = require("clean-webpack-plugin")
module.export = {
    plugins: [new CleanWebpackPlugin()]
}
```
