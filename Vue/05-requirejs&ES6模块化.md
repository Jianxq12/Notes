# Vue 第5天

## es6新内容
### class
```js
class Person{
    construcotr(name, age){
        this.name = name;
        this.age = age;
    }

    sayHello(){}

    static sayHi(){}
}


class Student extends Person{
    constructor(name, age, stuNo){
        super(name, age);
        this.stuNo = stuNo
    }
}
```

### 解构赋值
```js
var obj  = {name: "张志潮", age: 30}
var {name, age} = obj;

var {name: name1, age: age1} = obj;

var arr = [1, 3, 4, 6]
var [num1, num2, ...num3] = arr; 
```

### 扩展对象
```js
var obj  = {name: "张志潮", age: 31}

var obj1 = {...obj}

```

## 模块化
1. 什么是模块化
2. 模块的作用
3. 怎么实现模块化

### 模块化的标准
1. CommonJS
2. AMD: 异步模块定义  Async Module Defination require.js
3. CMD: 通用模块定义  Common Module Defination  sea.js

## requirejs
1. 定义模块
```js
define("名字", [依赖项], function(接收依赖项的返回值){
    //模块自己的内容
    //如果模块有需要提供给别人使用的内容
    //需要使用return关键字进行返回
    return 内容
})

define(function(){})
define([依赖项], function(接收依赖项的返回值){})
```

2. 引用模块
```js
require(["模块路径"], function(接收依赖项的返回值){
    //在模块加载完成之后会执行的代码
})
```

### requirejs的路径的三种情况
1. 不做任何配置，路径会以当前引用模块的文件作为参照
2. 使用data-main属性，路径会以data-main属性所指向的文件作为参照
3. 使用require.config方法配置了baseUrl后，路径会以baseUrl作为参照


### requirejs配置
```js
require.config({
    //设置基础路径
    baseUrl: "这里一般都用绝对路径",
    //设置模块的路径别名
    paths: {
        "别名": "模块具体路径（不带.js后缀）"
    },
    shim: {
        //为不支持模块化的第三方内容设置依赖项以及导出项
        "别名": {
            //依赖项:
            deps: ["依赖项别名", ...],
            exports: "导出的内容的全局变量名（只能导出一个）"
        }
    }
})

//通过以上方法配置之后，模块的查找都会以baseUrl作为基础，如果path中有模块的别名配置，那么路径查找遵循 baseUrl+paths

//如果路径中出现了如下任意一种情况，则查找方式和baseUrl就没有任何关系了
//1. /开头的路径
//2. .js结尾的路径
//3. http:// https://开头的路径
```
## es模块化
1. 模块导出项设置
```js
// 1. 在变量或者函数声明前直接加export
export var a = 10;
export var obj = {};
export function test(){};

//2. 通过花括号导出
var a = 10;
var obj = {};
export {a, obj}

//3. 默认导出项 一个模块只能有一个
export default function(){}

//4. 既导出默认项，又导出别的项
export {a as 别名, obj as default}
```
2. 模块导入的方式
```js
//对应第一种导出方式
import {变量名, 变量名} from "模块路径"

//对应第二种导出方式
import {变量名, 变量名} from "模块路径"

//第一和第二种导出方式都可以用如下方式导入
import * as 随便一个名字 from "模块路径"

//第三种导出方式对应的导入方式
import 随便一个名字 from "模块路径"
```

## webpack的简单使用
[webpack官网](https://doc.webpack-china.org/)

### 安装
1. 全局安装
```bash
npm install webpack -g
```

### 使用
```bash
webpack 入口文件路径 输出文件路径
```

### webpack支持的模块化标准
webpack打包的时候认识下面这些标准的导入导出的代码，可以帮你做导入导出的事情！！

1. CommonJS
2. AMD
3. ES6