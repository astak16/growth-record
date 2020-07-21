作用：通过`source map`定位到源代码
* [`source map`科普文：](http://www.ruanyifeng.com/blog/2013/01/javascript_source_map.html)

开发环境开启，线上环境关闭
* 线上排查问题的时候可以将`source map`上传到错误监控系统

## `source map`关键字

* `eval`：使用`eval`包裹模块代码
* `source map`：产生`map`文件
* `cheap`：不包含列信息
* `inline`：将`.map`作为`DataURI`嵌入，不单独生成`.map`文件
* `module`：包含`loader`的`sourcemap`

## `source map`类型
|devtool|首次构建|二次构建|是否适合生产环境|可以定位的代码|
|:--:|:--:|:--:|:--:|:--:|
|`(none)`|+++|+++|yes|最终输出的代码|
|`eval`|+++|+++|no|`webpack`生成的代码(一个个的模块)|
|`cheap-eval-source-map`|+|++|no|经过loader转换后的代码(只能看到行)|
|`cheap-module-eval-source-map`|o|++|no|源代码(只能看到行)|
|`eval-source-map`|--|+|no|源代码|
|`cheap-source-map`|+|o|yes|经过`loader`转换后的代码只能看到行)|
|`cheap-module-source-map`|o|-|yes|源代码(只能看到行)|
|`inline-cheap-source-map`|+|-|no|经过`loader`转换后的代码(只能看到行)|
|`inline-cheap-module-source-map`|o|-|no|源代码(只能看到行)|
|`source-map`|--|--|yes|源代码|
|`Inline-source-map`|--|--|no|源代码|
|`hidden-source- map`|--|--|yes|源代码|

在配置中增加`devtool`选项就可以了。