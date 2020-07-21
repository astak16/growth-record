## 抽象类

```js
abstract class Geom{
  width: number
  height: number
  getType(){
    return "Geom"
  }
  abstract getArea():number
}
class Squire extends Geom{
  getArea(){
    return this.width * this.height
  }
}
class Trangle extends Geom{
  getArea(){
    return this.width * this.height / 2
  }
}
```

## 单例

```js
class Demo {
  private static instance: Demo;
  private constructor() {}
  static getInstance() {
    if (!Demo.instance) {
      Demo.instance = new Demo();
    }
    return this.instance;
  }
}

const demo1 = Demo.getInstance();
const demo2 = Demo.getInstance();
console.log(demo1 === demo2);
```