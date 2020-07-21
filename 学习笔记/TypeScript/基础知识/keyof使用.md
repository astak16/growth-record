先看下面的代码：

```ts
interface Person {
  name: string;
  age: number;
  gender: string;
}

class Teacher {
  constructor(private info: Person) {}

  getInfo(key: string){
    return this.info[key];        // 报错
  }
}

const teacher = new Teacher({
  name: "zhangsan",
  age: 18,
  gender: "male"
});
const username = teacher.getInfo("name")    //username 显示的类型是 any
```

声明一个类`Tacher`，接收参数`info`继承`Person`。

类里面有一个方法`getInfo`，接收参数`key`，类型是`string`，返回对应的结果。

那`this.info[key]`为啥会报错呢？

因为`info`是`Person`类型的，而传递进来的`key`不能保证是`info`类型中定义的字段，所有这里就报错了。

这里要限制`getInfo`的传递参数应该怎么做呢？

这里要用到类型保护机制了。

```ts
getInfo(key: string){
  if(key === "name" || key === "age" || key === "gender"){
    return this.info[key];
  }
}
```

或者，在调用的时候用类型断言

```ts
teacher.getInfo("name") as string
```

这样使用都太麻烦了，有没有更好的方法呢？

使用`keyof`关键词。

```ts
getInfo<T extends keyof Person>(key: T): Person[T] {
  return this.info[key];
}
```

`keyof`相当于遍历

```ts
T extends keyof Person
/* 相当于 */
T extends "name"
T extends "age"
T extends "gender"
```

也就是说泛型`T`对`Person`中定义的每个字段都继承了

当你再次调用`getInfo`是，如果传递的不是`Person`中的字段就会报错。