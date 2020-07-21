`webpack`默认配置文件:`Webpack.config.js`，可以通过`Webpack --config`指定配置文件

## `webpack`配置组成
```js
module.exports = {
    entry: './src/index.js',    //①打包的入口文件
    output: './dist/main.js',   //②打包的输出
    mode: 'production',         //③3环境
    module: {
        rules: [              //④ Loader配置
            {test: /\.txt$/, use: 'raw-loader'},
        ]
    },
    plugins: [                 //⑤插件配置
        new HtmlwebpackPlugin({
            template:'./src/index.html'
        })
    ]
}
```

## `webpack`示例
```js
"use strict";
const path = require("path");

module.exports = {
  entry: "./src/index.js",
  output: {
    path: path.join(__dirname, "dist"),
    filename: "bundle.js"
  },
  mode: "production"
};
```

## 配置`script`脚本

每次在命令行中输入`./node_modules/.bin/webpack`挺麻烦的。

`package.json`文件中的`script`能读到`node_modules/.bin`的文件，可以直接在里面写上要执行的命令。

在命令行中执行`npm run xxx`

```js
"scripts": {
    "build": "webpack"
},
```