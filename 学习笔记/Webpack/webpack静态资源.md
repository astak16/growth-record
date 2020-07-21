资源内联的意义

代码层面

* 页面框架的初始化脚本
* 上报相关打点
* `css`内联避免页面闪动

请求层面：減少`HTTP`网络请求数

* 小图片或者字体内联（`url-loader`）

需要安装`raw-loader`的`0.5.1`版本

## `HTML`内联

`raw-loader`内联`html`

```js
<head>
    ${require('raw-loader!./meta.html')}
</head>
```

## `JS`内联

`raw-loader`内联`JS`

```js
<head>
  <script>${require('raw-loader!babel-loader!../node_modules/lib-flexible/flexible.js')}</script>
</head>
```

## `css`内联

方案一：借助`style-loader`

```js
module.exports = {
  module: {
    rules: [
      { 
        test: /.scss$/, 
        use: 
         {
            loader: 'style-loader',
            options: {
                insertAt: 'top',    //样式插入到<head>
                singleton: true     //将所有的 style 标签合并成一个
            },
            'css-loader',
            'sass-loader'
         } 
      }
    ]
  }
};
```

方案二：`html-inline-css-webpack-plugin`

方案二使用的更加广泛。

