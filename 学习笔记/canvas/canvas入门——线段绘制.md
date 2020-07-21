## `moveTo`、`lineTo`

使用`canvas`绘制一条线段，默认已经拿到`canvas`的上下文`context`

绘制一条直线需要使用`context`提供的`moveTo`、`lineTo`方法

`moveTo`是线段的起点，`lineTo`是线段下一个点，最后使用`stroke`进行描边。如下代码

```js
context.moveTo(100,100)
context.lineTo(200,200)
context.stroke()
```

效果图：

![1.png](https://i.loli.net/2019/09/08/y7cTXhtGOsVJRU2.png)

如果一段线段分为多段，可以多次使用`lineTo`方法，将每个顶点都填进去就可以了

```js
context.moveTo(100,100)
context.lineTo(200,200)
context.lineTo(200,400)
context.stroke()
```

效果图：

![2.png](https://i.loli.net/2019/09/08/qzQt5baM7wjeB42.png)

## `strokeStyle`、`lineWidth`

`content`还提供了`strokeStyle`和`lineWidth`两个方法，这两个方法可以的作用是`描边颜色`，`线宽`。

```js
ctx.moveTo(100, 100)
ctx.lineTo(200, 200)
ctx.lineTo(200, 400)
ctx.strokeStyle = 'red'
ctx.lineWidth = 5
ctx.stroke()
```

效果图

![3.png](https://i.loli.net/2019/09/08/4NfSxrkmvUM6OLh.png)

如果我想绘制两条不同的线段呢？使用`moveTo`就能开始新的一条线段绘制

```js
ctx.moveTo(100, 100)
ctx.lineTo(200, 200)
ctx.lineTo(200, 400)
ctx.strokeStyle = 'red'
ctx.lineWidth = 5
ctx.stroke()

ctx.moveTo(300, 100)
ctx.lineTo(400, 200)
ctx.lineTo(400, 400)
ctx.strokeStyle = 'blue'
ctx.lineWidth = 10
ctx.stroke()
```

效果图：

![4.png](https://i.loli.net/2019/09/08/NhGb9tZsnI2Ri6W.png)

看到效果图之后，发现和预想的不一样啊，为什么第一段代码没有生效？

这是因为`canvas`是基于状态绘制的，上面一段代码写了`strokeStyle = 'red'`，然后`stroke`绘制；然后下面开始一段新的线段绘制写了`strokeStyle = 'blue'`，在写这句话的时候，已经把上面的`red`改成了`blue`，使用最后一个`stroke`又进行绘制了。

也就是说不管你上面写了多少属性，只要最后一个`stroke`前对属性改动，都会被覆盖。

怎么解决这个问题呢？

## `beginPath`、`closePath`

`context`提供了`beginPath`和`closePath`，这两个方法是告诉`canvas`我要开始绘制和结束绘制了，你不要给我随便改变属性

```js
ctx.beginPath()
ctx.moveTo(100, 100)
ctx.lineTo(200, 200)
ctx.lineTo(200, 400)
ctx.strokeStyle = 'red'
ctx.lineWidth = 5
ctx.closePath()
ctx.stroke()

ctx.beginPath()
ctx.moveTo(300, 100)
ctx.lineTo(400, 200)
ctx.lineTo(400, 400)
ctx.strokeStyle = 'blue'
ctx.lineWidth = 10
ctx.closePath()
ctx.stroke()
```

效果图：

![5.png](https://i.loli.net/2019/09/08/8RWh7BjurZmTyXf.png)

这里有个比较奇怪的是，最后我没有回到原点呀，为什么最后给我连起来了呢？

这是因为使用了`closePath`这个方法，使用`closePath`时，如果最后一个点没有回到起始位置，它会为你把首尾连接起来。

如果之后想连接起来，可以不用写`closePath`，其实，只要写了`beginPath`，`canvas`就已经知道了你要重新绘制新线段了。

```js
ctx.beginPath()
ctx.moveTo(100, 100)
ctx.lineTo(200, 200)
ctx.lineTo(200, 400)
ctx.strokeStyle = 'red'
ctx.lineWidth = 5
ctx.stroke()

ctx.beginPath()
ctx.moveTo(300, 100)
ctx.lineTo(400, 200)
ctx.lineTo(400, 400)
ctx.strokeStyle = 'blue'
ctx.lineWidth = 10
ctx.stroke()
```

效果图：

![6.png](https://i.loli.net/2019/09/08/dvr834enlQLUV1t.png)

这里要说明的一点是，写在`stroke`是绘制，`stroke`上面的那些都是状态，把状态写在`stroke`下面当然是不生效的的。

![7.png](https://i.loli.net/2019/09/08/qdGDIOlZtzNvwVo.png)

## 最后

总结一下：

* 使用`moveTo`、`lineTo`绘制线段
* `beginPath`开始新的线段绘制
* `closePath`绘制线段首尾未相连，它会连起来
* `canvas`绘制是基于状态绘制的，绘制之后线段的属性就不能被改变了。