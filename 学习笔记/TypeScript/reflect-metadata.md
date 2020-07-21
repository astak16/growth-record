安装[`reflect-metadata`](https://github.com/rbuckton/reflect-metadata)

## 对象中使用
```ts
const user = {
  name: "zhangsan"
}
Reflect.defineMetadata("data", "test", user)  //存储元数据
Reflect.getMetadata("data", user)   //读取元数据
```

## 类中使用

```ts
@Reflect.metadata("data", "test")
class User{
  name = "zhangsan"
}

Reflect.getMetadata("data", User)
```

## 类的属性和方法中使用
```ts
class User{
  @Reflect.metadata("data", "test")
  name = "zhangsan"

  @Reflect.metadata("data1", "test1")
  getName(){}
}

Reflect.getMetadata("data", User.prototype, "name")
Reflect.getMetadata("data1", User.prototype, "getName")
```

## 装饰器的执行顺序

方法的装饰器比类的装饰器先执行，因为方法的装饰器先执行，才能在类的装饰中拿到数据
```ts
function showData(target: typeof User) {
  for(let key in target.prototype){
    const data = Reflect.getMetadata("data", target.prototype, key)
  }
}

@showData
class User{

  @Reflect.metadata("data", 'name')
  getName(){}

  @Reflect.metadata("data", 'age')
  getAge(){}
}
```

## 封装发的装饰器
```ts
function showData(target: typeof User) {
  for(let key in target.prototype){
    const data = Reflect.getMetadata("data", target.prototype, key)
  }
}

function setData(dataKey: string, msg: string) {
  return function(target: User, key: string) {
    Reflect.defineMetadata(dataKey, msg, target, key)
  }
}

@showData
class User{

  @setData("data", "name")
  getName(){}

  @setData("data", "age")
  getAge(){}
}
```

`````````````````````````````````