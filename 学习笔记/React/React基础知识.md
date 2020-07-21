## `React.FunctionComponent`

`React`提供了一个组件类型`React.FunctionComponent`，可简写`React.FC`，

- 可以接收一个泛型`p`，默认是`{}`
- `children`，返回一个`React.ReactNode`，这个`children`是任何`component`都拥有的
- 静态属性`defaultProps`，组件的默认属性，外部可以不传这个属性。

```ts
interface IHelloProps {
  message?: string;
}

const Hello: React.FunctionComponent<IHelloProps> = props => {
  return <h2>{props.message}</h2>;
};

Hello.defaultProps = {
  message: "Hello  world" 
};
```

## `React Hook`
- 完全可选
- 百分百向后兼容
- 没有计划从`React`移除`class` 

`Hook`是一个特殊的函数，它可以让你勾入`React`特性，例如`useState`就允许在`React`函数组件添加`state Hook`。

在编写函数组件时，意识到要向里面添加一些`State`时，以前的做法是必须转换成`Class`类型的组件，现在可以在现有的函数组件中使用`Hook`

## `useState`

分开使用
```ts
import React, { useState } from "react";

const LikeButton: React.FC = () => {
  const [like,setLike] = useState(0)
  const [on,setOn] = useState(true)
  return(
    <>
      <button onClick={()=>{setLike(like + 1)}}>
        {like}👍
      </button>
      <button onClick={()=>{setOn(!on)}}>
        {on ? "ON" : "OFF"}
      </button>
    </>
  )
};

export default LikeButton;
```

合在一起使用

```ts
import React, { useState } from "react";

const LikeButton: React.FC = () => {
  const [obj,setObj] = useState({like:1,on:true})
  return(
    <>
      <button onClick={()=>{setObj({ like: obj.like + 1, on: obj.on })}}>
        {obj.like}👍
      </button>
      <button onClick={()=>{setObj({ like: obj.like, on: !obj.on })}}>
        {obj.on ? "ON" : "OFF"}
      </button>
    </>
  )
};

export default LikeButton;
```

## 声明周期函数

### `shouldComponentUpdate`

用途：返回`true`，表示不阻止`UI`更新；返回`false`，表示阻止`UI`更新

作用：允许我们手动判断是否要进行组件更新，我们可以根据应用场景灵活地设置返回值，以避免不必要的更新。

```ts
this.state = { n: 1 }
shouldComponentUpdate(nextProps, nextState) {
  if(nextProps.n === this.state.n) {
    return false
  }else {
    return true
  }
}
```

使用`PureComponent`可以在数据更新时，新数据和老数据一样时，阻止视图更新（只是第一层）
