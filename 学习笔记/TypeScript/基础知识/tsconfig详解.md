运行`tsc`时会去读`tsconfig.json`文件，但是执行`tsc dome.ts`就不会去读`tsconfig.json`文件了。

`files`对某个文件编译

```ts
"files": [./dome.ts]
```

如果只需要对某个文件执行`tsc`，需要用`include`配置，可以对整个文件夹编译

```ts
{
  "include": ["./dome.ts"]
}
```

不想编译某个文件，需要使用`exclude`配置

```ts
{
  "exclude": ["./dome.ts"]
}
```

## 导入基础文件配置

```ts
"extends": "./tsconfig.base"    // 导入基础文件中的配置
```

保存文件时编译器自动编译

```ts
"compileSave": true   // 保存文件时让编译器自动编译，vscode目前不支持
```

## `compilerOptions`配置

### 增强编译

```ts
"incremental": true,      // 增强编辑，编译过的不在编译
"tsBuildInfoFile": "./buildFile",     // 编译文件的存储位置
"diagnostics": true,   // 显示编译的详细信息
"downlevelIteration": true    // 降级遍历器的实现(es3/5)

```

### 模块相关

```ts
"target": "es5",        // 目标语言的版本
"module": "amd",        // 生成代码的模块标准
"outFile": "./app.js",  // 将多个文件依赖的文件生成一个文件，可以用在 AMD 模块中
"lib": [],           // ts 需要引用的声明文件，es5 默认引入"dom"、"es5"、"scripthost"，高级版本的特性，比如"es2019.array."
"noEmitHelpers": true,  // 不生成 helper 函数，需要额外安装 ts-helpers
"importHelpers": true,  // 通过 tslib 引入 helper 函数，文件必须是模块
"esModuleInterop": true,    // 允许 export = 导出，由 import from 导入
"allowUmdGlobalAccess": true,   // 允许在模块中访问 UMD 全局变量
"moduleResolution": "node",     // 模块解析策略
"baseUrl": "./",         // 解析非相对模块的基地址
"paths": {               // 不想使用默认的导入版本，手动指定，路径相对于 baseUrl 中的路径
  "jquery": ["node_modules/jquery/dist/jquery.slim.min.js"]
},
"rootDirs": ["src", "out"],   // 将多个路径放在一个虚拟目录下，用户运行时
```

### 文件相关

```ts
"allowJs": true,          /* 允许编译 js */
"checkJs": true,          /* 检查 js 文件语法错误 */
"sourceMap": true,        /* 启用 sourceMap */
"inlineSourceMap": true,  /* sourceMap 会包含在生成的 js 文件当中 */
"declarationMap": true    /* 生成声明文件的 sourceMap */
"outDir": true,           /* 打包文件 */
"rootDir": true,          /* 编译入口 */
"noUnusedLocals": true,                /* 局部变量未使用提示 */
"noUnusedParameters": true,            /* 函数参数未使用提示 */
"removeComments": true,    /* 删除注释 */
"noEmit": true,            /* 不输出文件 */
"noEmitOnError": true,     /* 发生错误时不输出文件 */
"noUnusedLocals": true,    /* 检查只声明，未使用的局部变量 */
"noUnusedParameters": true,           /* 检查未使用的函数参数 */
"noFallthroughCasesInSwitch": true,   /* 防止 switch 语句贯穿 */
"noImplicitReturns": true, /* if..else 都要有返回值 */
"listEmittedFiles": true,  /* 打印输出文件 */
"listFiles": true          /* 打印编译的文件（包括引用的声明文件） */
```

### 声明文件

```ts
"declaration": true,          // 生成声明文件
"declarationDir": "./d",      // 声明文件路径
"emitDeclarationOnly": true,  // 只生成声明文件
"typeRoots": [],    // 声明文件目录，默认 node_modules/@types
"types": []         // 声明文件包
```

## 严格检查

启用`strict`，下面这些都会启用
```ts
 "strict": true,
// "noImplicitAny": true,         /* 显示指示 any */
// "strictNullChecks": true,      /* null, undefined 不能赋值给其他类型 */
// "strictFunctionTypes": true,   /* 不允许函数参数双向绑定 */
// "strictBindCallApply": true,   /* 严格的 bind/call/apply 检查 */
// "strictPropertyInitialization": true,    /* 类的实例属性必须初始化 */
// "noImplicitThis": true,        /* 不允许 this 有隐式的 any 类型 */
// "alwaysStrict": true,          /* 在代码中注入 "use strict" */
```

## 其他

```ts
"jsx": "react"   // preserve react-native react
/*
 * preserve     生成的代码会保留 jsx 格式，文件扩展名也是 jsx
 * react-natvie   生成的代码是 jsx 格式，文件扩展名是 js
 * react        生成的代码是存 js 语法，
 */
```

## 解析策略

`moduleResolution`，有两种取值
- `classic`
- `node`


`classic`解析策略

```ts
// 相对导入
/root/src/moduleA.ts
import {b} from "./moduleB"

/root/src/moduleB.ts
/root/src/moduleB.d.ts

// 非相对导入
/root/src/moduleA.ts
import {b} from "moduleB"

/root/src/node_modules/moduleB.ts
/root/src/node_modules/moduleB.d.ts
/root/node_modules/moduleB.ts
/root/node_modules/moduleB.d.ts
/node_modules/moduleB.ts
/node_modules/moduleB.d.ts
```

`node`解析策略

```ts
// 相对导入
/root/src/moduleA.ts
import {b} from "./moduleB"

/root/src/moduleB.ts
/root/src/moduleB.tsx
/root/src/moduleB.d.ts
/root/src/moduleB/package.json("types"属性)
/root/src/moduleB/index.ts
/root/src/moduleB/index.tsx
/root/src/moduleB/index.d.ts

// 非相对导入
/root/src/moduleA.ts
import {b} from "moduleB"

/root/src/node_modules/moduleB.ts
/root/src/node_modules/moduleB.tsx
/root/src/node_modules/moduleB.d.ts
/root/src/node_modules/moduleB.package.josn("types"属性)
/root/src/node_modules/moduleB/index.ts
/root/src/node_modules/moduleB/index.tsx
/root/src/node_modules/moduleB/index.d.ts

/root/node_modules/moduleB.ts
/root/node_modules/moduleB.tsx
/root/node_modules/moduleB.d.ts
/root/node_modules/moduleB.package.josn("types"属性)
/root/node_modules/moduleB/index.ts
/root/node_modules/moduleB/index.tsx
/root/node_modules/moduleB/index.d.ts

/node_modules/moduleB.ts
/node_modules/moduleB.tsx
/node_modules/moduleB.d.ts
/node_modules/moduleB.package.josn("types"属性)
/node_modules/moduleB/index.ts
/node_modules/moduleB/index.tsx
/node_modules/moduleB/index.d.ts
```

## 构建

每个目录都可以有一个自己的`tsconfig.json`

```ts
/src/client/tsconfig.json
/src/common/tsconfig.json
/src/server/tsconfig.json
/test/tsconfig.json
/tsconfig.json
```

目录里面的`tsconfig.json`需要继承自基础的`tsconfig.json`
```ts
{
  "extends": "../tsconfig.json",
  "compilerOptions": {    /* 输出目录 */
    "outDir": "../../dist/common"
  },
  "refercence": [       /* 依赖 */
    { "path": "../src/client" },
    { "path": "../src/server" }
  ]
}
```

编译`tsc -b src/server --verbose`