
`css-loader`用于加载`.css文件`，并且转换成`commonjs`对象。

`style-loader`将样式通过`<style>`标签插入到`head`中。

## 解析`css`

需要安装`css-loader`和`style-loader`。

```js
module.exports = {
  module: {
    rules: [
      { test: /.css$/, use: ["style-loader", "css-loader"] }
    ]
  }
};
```
`style-loader`必须要在`css-loader`之前，多个`loader`，`webpack`会进行链式调用

## 解析`less`

需要安装`css-loader`，`style-loader`和`less-loader`。

```js
module.exports = {
  module: {
    rules: [
      { test: /.less$/, use: ["style-loader", "css-loader", "less-loader"] }
    ]
  }
};
```

## 解析图片

`file-loader`可以用于处理文件。

需要安装`file-loader`

```js
module.exports = {
  module: {
    rules: [
        { test: /.(png|jpeg|jpg|gif)$/, use: ["file-loader"] }
    ]
  }
};
```

## 解析字体

需要安装`file-loader`

```js
module.exports = {
  module: {
    rules: [
        { test: /.(woff|woff2|eot|ttf|otf)$/, use: ["file-loader"] }
    ]
  }
};
```

## `url-loader`

`url-loader`也可以处理图片和字体，可以设置较小资源自动`base64`

需要安装`url-loader`

```js
module.exports = {
  module: {
    rules: [
       {
         test: /.(png|jpeg|jpg|gif)$/,
         use: [
           {
             loader: "url-loader",
             options: {
               limit: 10240     //10k，意思是如果图片小于 10k webpack 在打包时会做 base64 处理
             }
           }
        ]
      }
    ]
  }
};
```