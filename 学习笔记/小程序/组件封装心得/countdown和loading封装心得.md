## `countdown`
### 一位数字补`0`
```ts
let num = 1
num.toString()
let zeroNum = num[1] ? num : '0' + num
```

### 倒计时使用`setTimeout`
```ts
let timer,n=0;
function init(){
  clearTimeout(timer)
  timer = setTimeout(() => addN(), 1000)
}
function addN(){
  n += 1;
  console.log(n)
  init()
}
```

## `loading`

### `wx.lin.showLoading`和`wx.lin.hideLoading`

在使用`loading`组件时，发现一个好用的`api`：`wx.lin.showloading`。

看了源码后，才知道原来实现很简单。

就是在`wx`上面挂载了一个`lin`，然后在`lin`上分别挂载了`showLoading`和`hideLoading`。

```ts
wx.lin = wx.lin || {}
wx.lin.showLoading = (option) => { }
wx.lin.hideLoading = () => {this.setData({ show: false })}
```

## 析构

```ts
const init = (options) => {
  const {aa = true, bb = true} = {...options}
  console.log(aa)
} 

const init = (options) => {
  const {aa = true, bb = true} = options
  console.log(aa)
}
```