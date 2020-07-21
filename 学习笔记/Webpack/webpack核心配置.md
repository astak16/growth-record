## `entry`

`entry`用来指定`webpack`的打包入囗

依赖的入口是`entry`，对于非代码比如图片、字体依赖也会不断加入到依赖图中。

### 单入口

单入口：`entry`是一个字符串，一般用于单页应用

```js
module.exports = {
    entry: './path/to/my/entry/file.js'
}
```

### 多入口

多入口：`entry`是一个对象，一般用于多页应用

```js
module.exports = {
    entry: {
        app: './src/app.js',
        adminApp: './src/adminApp.js'
    }
}
```

## `output`

`output`用来告诉`webpack`如何将编译后的文件输出到磁盘。

### `entry`单入口

`entry`单入口，`output`只需指定`filename`和`path`两个参数就可以了

```js
module.exports = {
    entry: './path/to/my/entry/file.js',
    output: {
        filename: 'bundle.js',
        path: __dirname + '/dist'
    }
}
```

### `entry`多入口

`entry`多入口，`output`的`filename`利用了一个占位符`[name]`，来指定打包后的名称

```js
module.exports = {
    entry: {
        app: './src/app.js',
        adminApp: './src/adminApp.js'
    },
     output: {
        filename: '[name].js',
        path: __dirname + '/dist'
    }
}
```

## `loaders`

`webpack`开箱即用只支持`JS`和`JSON`两种文件类型，通过`Loaders`去支持其它文件类型并且把它们转化成有效的模块，并且可以添加到依赖图中。

`loaders`本身是一个函数，接受源文件作为参数，返回转换的结果。

### 常见的`loaders`

|名称|描述|
|:--:|:--:|
|`babel-loader`|转换`es6、es7`等`js`新特性语法|
|`css-loader`|支持`css`文件的加载和解析|
|`less-loader`|将`less`文件转换成`css`|
|`ts-loader`|将`ts`转换成`js`|
|`file-loader`|进行图片、字体等的打包|
|`raw-loader`|将文件以字符串的形式导入|
|`thread-loader`|多进程打包`js`和`css`|

`test`指定匹配规则，`use`指定使用的`loader`名称

```js
module.exports = {
    module: {
        rules: [
            {test: /\.txt$/, use: 'raw-loader'}
        ]
    }
}
```

## `plugins`

插件用于`bundle`文件的优化，资源管理和环境变量注入，作用于整个构建过程。

任何`loaders`没法做的事情，都可以用`plugins`解决。

### 常见的`plugins`

|名称|描述|
|:--:|:--:|
|`CommonsChunkPluign`|将`chunks`相同的模块代码提取成公共`js`|
|`CleanWebpackPlugin`|清理构建目录|
|`ExtractTextWebpackPlugin`|将`css`从`bundle`文件里提取成一个独立的`css`文件|
|`CopyWebpackPlugin`|将文件或者文件夹拷贝到构建的输出目录|
|`HtmlWebpackPlugin`|创建`html`文件去承载输出的`bundle`|
|`UglifyjsWebpackPlugin`|压缩`js`|
|`ZipWebpackPlugin`|将打包出的资源生成一个`zip`包|


```js
module.exports = {
    plugins: [
        new HtmlWebpackPlugin({template: './src/index.html'})
    ]
}
```

## `mode`

`Mode`用来指定当前的构建环境是：`production`、`development`还是`none`。

设置`mode`可以使用`webpack`内置的函数，默认值为`production`

### 常见的`mode`配置

|选项|描述|
|:--:|:--|
|`development`|设置`process.env.NODE_ENV`的值为`development`。开启`NamedChunksPlugin`和`NamedModulesPlugin`|
|`production`|设置`process.env.NODE_ENV`的值为`production`。开启`FlagDependencyUsagePlugin`，`FlagIncludedChunksPlugin`，`ModuleConcatenationPlugin`，`NoEmitonErrorsPlugin`，`OccurrenceOrderPlugin`，`SideEffectsFlagPlugin`和`TerserPlugin`|
|`none`|不开启任何优化选项|

`development`开启的两个插件会在代码热更新阶段在控制台中打印出是哪个模块发生了热更新，这个模块的路劲是啥
`production`开启的插件默认会在生产环境压缩代码等。