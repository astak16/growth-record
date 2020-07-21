什么是文件指纹？

打包后输出的文件名的后缀。

## 文件指纹如何生成

`Hash`：和整个项目的构建相关，只要项目文件有修改，整个项目构建的`hash`值就会更改。

`Chunkhash`：和`webpack`打包的`chunk`有关，不同的`entry`会生成不同的`chunkhash`值（js指纹）

`Contenthash`：根据文件内容来定义`hash`，文件内容不变，则`contenthash`不变。（css指纹）

## `js`的文件指纹设置

设置`output`的`filename`，使用`[chunkhash]`

```js
module.export = {
    output: {
        path: path.join(__dirname, "dist"),
        filename: "[name]_[chunkhash:8].js"
    }
}
```

## `css`的文件指纹设置

设置`MiniCssExtractPlugin`的`filename`，使用`[contenthash]`

`style-loader`是把`css`插入到`head`里面，而插件是把`css`提取为文件。他们之间是互斥的。

```js
const MiniCssExtractPlugin = require("mini-css-extract-plugin")
module.export = {
    module: {
        rules: [
            { test: /.css$/, use: [MiniCssExtractPlugin.loader, "css-loader"] },
            { test: /.less$/, use: [MiniCssExtractPlugin.loader, "css-loader", "less-loader"] },
        ]
    },
    plugins:[
        new MiniCssExtractPlugin({
            filename:'[name]_[contenthash:8].css'
        })
    ]
}
```

## 图片的文件指纹设置

设置`file-loader`的`name`，使用`[hash]`
|占位符名称|含义|
|:--:|:--:|
|`[ext]`|资源后缀名|
|`[name]`|文件名称|
|`[path]`|文件的相对路径|
|`[folder]`|文件所在的文件夹|
|`[contenthash]`|文件的内容`hash`，默认是`md5`生成|
|`[hash]`|文件内容的`hash`,默认是`md5`生成|
|`[emoji]`|一个随机的指代文件内容的`emoj`|

```js
module.export = {
    module: {
        rules: [
          {
            test: /.(png|jpeg|jpg|gif)$/,
            use: [
              {
                loader: "file-loader",
                options: {
                  name:"[name]_[hash:8].[ext]" 
                }
              }
            ]
          }
        ]
    }
 }
```