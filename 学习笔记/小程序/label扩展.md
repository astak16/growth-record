不想用小程序原生`button`，但又想拥有原生`button`的开发能力，如何实现呢？

借助原生标签`label`，就可以实现自定义`button`，且拥有和原生`button`一样的开放能力。

```ts
<label for="button">
  <view class="button">我是按钮</view>
</label>
<button id="button" open-type="contact"></button>
```

如上面代码所示，`label`标签有一个`for`属性，用来绑定组件`id`。此时`.button`已经拥有和原生`button`一样的开放能力，并且可以自由玩耍。

当然了原生`button`不能让它出现在页面中。

一个比较好的方法是使用绝对定位。

```css
button {
  position: absolute;
  top: -1000px;
  left: -1000px;
}
```
使用绝对定位，并且`top`和`left`都设置的比较大，可以让这个元素远离页面。

虽然`display:none`也可以隐藏元素，但在页面中还是有这个组件，鼠标放上去可以看到它，以后在调试时会影响判断。

`Tips`：可以使用扩展的还有`checkbox`，`radio`，`switch`组件。

总结：`button`、`checkbox`、`radio`、`switch`组件配合`label`使用有奇效

