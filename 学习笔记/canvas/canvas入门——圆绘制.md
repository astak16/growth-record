## `arc`
语法：`arc(x, y, radius, startAngle, endAngle, anticlockwise)`

前面两个参数是`x`坐标，`y`坐标，第三个参数是`半径`，第四个参数是开始的弧度，第五个参数是结束的弧度，第六个参数是顺时针还是逆时针，默认是顺时针。

看下面代码，这样就能绘制一个圆了。

```js
ctx.arc(100, 100, 50, 0, 2 * Math.PI)
ctx.stroke()
```

效果图：

![1.png](https://i.loli.net/2019/09/08/p5qBhTmwc4onSdl.png)

这里要说明的一点是，不管顺时针还是逆时针，圆的弧度的位置是不变的，不会因为顺势转或者逆时针而改变，`0.5pi`的位置

![2.png](https://i.loli.net/2019/09/08/ovIuapVFgLqhkJ7.png)

```js
ctx.arc(100, 100, 50, 0, 1.5 * Math.PI)
ctx.stroke()
```

![3.png](https://i.loli.net/2019/09/08/rdDejURxWIZAcsM.png)

```js
ctx.arc(100, 100, 50, 0, 1.5 * Math.PI,true)
ctx.stroke()
```
![4.png](https://i.loli.net/2019/09/08/dVPwQOa1q7CRfY3.png)

上面代码第一个是顺时针绘制的，3/4 个弧度，用逆时针的话就是 1/4 个弧度，它的意思是从 0 开始，顺时针到 1.5pi的位置



