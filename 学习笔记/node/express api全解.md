## **`express`相关`api`介绍**

### **`express.json`**

这是一个`express` 中间件的功能，主要作用处理请求中的`json`数据，并挂载在`body`中。

```js
import * as express from "express";

const app = express();

app.use(express.json()); // 解析请求中的 json 数据
app.use((request, response, next) => {
  response.send(request.body);
});
app.listen(3000, () => {
  console.log("监听成功");
});
```

如果不使用中间件的话，需要自己去处理请求中的`json`数据，通过监听`request`的`data`和`end`事件，得到完整的请求数据。

```js
import * as express from "express";

const app = express();

app.use((request, response, next) => {
  const data = [];
  request.on("data", (chunk) => {
    data.push(chunk);
  });
  request.on("end", () => {
    response.send(JSON.parse(data[0].toString()));
  });
});
app.listen(3000, () => {
  console.log("监听成功");
});
```

### **`express.urlencoded`**

处理`x-www-form-urlencoded`形式的请求

这个格式的数据会被编码成以`"&"`分隔键-值对，同时以`"="`分隔键和值，比如`name=zhangsan&age=18`

### **`express.static`**

设置一个静态目录，当访问资源的时候，会去这个静态目录中找对应的资源

```js
import * as express from "express";

const app = express();

app.use(express.static("public")); // 设置静态资源目录
app.use((request, response, next) => {
  response.send(request.body);
});
app.listen(3000, () => {
  console.log("监听成功");
});
```

静态资源目录

```bash
public
  --home.html
  --about.html
```

当浏览器访问 `/home.html`时，会去自动匹配`public/home.html`文件，并将它返回给前端。

## **`app`相关`api`介绍**

### **`app.locals`**

设置本地变量，在整个应用中都是可以访问的。

```js
app.locals.name = "张三";

console.log(app.locals.name);
/** 等价于 */
console.log(request.app.locals.name);
```

### **`app.set`/`app.get`**

1. 设置任意想要存储的值，和`app.locals`类似，在整个应用中都是可以访问的。

   ```js
   app.set("name", "zhangsan");
   app.get("name");
   ```

2. 特殊的访问值

- `views`：视图全部在`public`目录中，视图的引擎是`pug`

  ```js
  app.set("views", "public");
  app.set("view engine", "pug");
  ```

  - `env`：设置环境变量
  - `etag`：设置`etag`

### **`app.param`**

用于处理路径中的参数，如果匹配就会执行后面的回调函数。

```js
import * as express from "express";

const app = express();
app.param("id", (req, res, next, id) => {
  console.log(id);
  next();
});
app.get("/user/:id", (req, res, next) => {
  res.send("1");
});

app.listen(3000, () => {
  console.log("监听成功");
});
```

### **`app.route`/`app.all`/`app.get`/`app.post`**

```js
app
  .route("/user")
  .all((req, res, next) => {...}) // 处理所有的情况
  .get((req, res, next) => {...}) // 处理get请求
  .post((req, res, next) => {...}); // 处理post请求
```

## **`request`相关`api`**

### **`request.baseUrl`**

设置公共的`api`访问路径

```js
import * as express from "express";

const app = express();
const router = express.Router();
router.get("/jp", (req, res) => {
  console.log(req.baseUrl); // /greet
  res.send(req.baseUrl);
});

app.use("/greet", router);
app.listen(3000, () => {
  console.log("监听成功");
});
```

### **`request.ip`**

获取请求`ip`，不过这个`ip`不准，只能知道一个大概的范围，比如在一个学校里，你获取到的`ip`可能都是相同的。

### **`request.params`**

获取请求路径中的参数

```js
import * as express from "express";

const app = express();
app.use("/greet/jp/:id", (req, res, next) => {
  console.log(req.params);
  res.send(req.params);
});
app.listen(3000, () => {
  console.log("监听成功");
});
```

### **`request.path`**

用于获取请求路径，不能在`app.use`中使用。

```js
import * as express from "express";

const app = express();
app.get("/greet", (req, res, next) => {
  console.log(req.path);
  res.send(req.path);
});
app.listen(3000, () => {
  console.log("监听成功");
});
```

### **`request.query`**

用于获取查询参数。

### **`request.xhr`**

判断请求头`X-Requested-With`值是不是`XMLHttpRequest`。

### **`request.acceptsLanguages()`**

```js
//请求头
Accept-Language: en-GB,en-US;q=0.9,en;q=0.8,zh-CN;q=0.7,zh;q=0.6

//后端可以通过判断当前支持的语言
request.acceptsLanguages()  // ['en-GB', 'en-US', 'en', 'zh-CN', 'zh']
request.acceptsLanguages("zh")  // zh
request.acceptsLanguages("fr")  // false
```

## **`response`相关`api`**

### **`response.format`**

根据请求头`Accept`返回对应的内容

```js
import * as express from "express";

const app = express();
app.get("/test", (req, res, next) => {
  res.format({
    "text/plain": function () {
      res.send("hey");
    },
    "text/html": function () {
      res.send("<p>hey</p>");
    },
    "application/json": function () {
      res.send({ message: "hey" });
    },
    default: function () {
      res.status(406).send("Not Acceptable");
    },
  });
});
app.listen(3000, () => {
  console.log("监听成功");
});
```

### **`response.local`**

用来重定向，配合状态码`301`来使用，等同于`res.redirect`
