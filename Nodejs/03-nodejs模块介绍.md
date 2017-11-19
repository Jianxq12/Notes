### node.js模块 

科普知识 :  在 node.js 开发中,每一个文件就可以认为是一个模块;

#### 一、node.js 模块分类

> var fs = require('fs');

#### 核心模块 Core Module .   也叫 内置模块、原生模块

- fs	    读写文件 
- http     创建服务并监听服务
- path     拼接路径
- url        解析 req.url 设置参数 true, 把 query 的查询字符串转化为对象啊 ( /add?id=12&name='yaya')
- querystring    解析查询字符串  ( ? 后面的:  id=2&name='xingge' )
- …...
  - 所有内置模块在`安装` node.js 的时候,就已经编译成二进制文件了, 可以直接加载运行(速度较快);
  - 部分内置模块, 在 node.exe 这个进程`启动`的时候,就已经默认加载了,所以可以直接使用  ( module )
- 1. 提前准备好  2. 速度快 

#### 文件模块

> var a = require('./a'); 

- 说明 : 根据文件路径引入的模块
- 如果加载时,没有指定后缀名, 那么就按照如下顺序加载响应模块  ( 例如 :  require ( ' ./demo  ' ))
  1. .js
  2. .json
  3. .node (C/C++编写的模块)


 #### 自定义模块 ( 第三方模块 )

> var mime = require('mime');

- 说明 : 需要额外通过 npm 安装的模块


- mime   根据路径/文件 后缀名 转化为对应的样式( Content-Type) , (mime.getType() )
- underscore   模板引擎 : 给 html 文件赋值
- …...


#### 二、require 加载模块顺序

> 代码演示

1. 看 require() 加载模块时传入的参数是否 **以 './' 或 '../' 或'/' ** 等等这样的方式 **开头**  (相对路径和绝对路径都可以)   
2. 是, **说明是 文件模块** , 那么会按照传入的路径直接去查询对应的模块  —> 
   - require('./test.js')   ==> 如果是具体的文件名
     - 直接根据给定的路径去加载模块,找到了,加载成功,找不到加载失败
   - require('./test')  ==> 不是具体的文件名
     - 第一步: 根据指定的路径,依次添加文件后缀 .js、.json、.node 进行匹配,如果找不到匹配,执行第二步
     - 第二步: 找不到会认为是 test 文件夹, 查找是否有 test 目录,, (尝试找 test 包)
       - 找不到: 加载失败
       - 找到了,依次在 test 目录下查找 package.json 文件（找到该文件后尝试找 main 字段中的入口文件）、index.js、index.json、index.node，找不到则加载失败
3. 不是, 那么就认为传入的是 **模块名称**, （比如：require('http')、require('mime')）
   - 先从内置模块里找,找到说明是内置模块: 直接加载内置模块

   - 不是内置模块
     - 依次递归查找 node_modules 目录中是否有相应的包
     - 从当前目录开始,依次递归查找所有父目录下的 node_modules 目录中是否包含相应的包

       ​

```js
// 情况一: require() 参数是一个路径
require('./b')

// 先找:
// b.js
// b.json
// b.node
// b 文件夹 --> package.json -> main(入口文件: app.js --> index.js/index.json/index.node)


// 情况二: require() 参数不是一个路径 ,那就是模块  (包括: require('b.js') )
require('b.js')  //  或 require('http') 或 require('mime')
//1. 先从内置模块里找,找到就加载
//2. 内置模块里找不到,就是第三方模块  --> node_modules 找 --> 父级目录里找
```



### require 加载模块注意点

1. 所有模块第一次加载完毕后, 都会有缓存; 

2. 每次加载模块的时候,都优先从缓存中加载,缓存中没有的情况下, 才会按照 node.js 加载模块的规则去查找;

3. 内置模块 在 node.js 源码编码的时候,都已经编译为二进制执行文件了,所以加载速度较快  
   - 内置模块加载的优先级 , 仅次于缓存加载

4. 如果 想加载的 第三方模块 和内置模块 重名

   - 只加载内置模块


   - 除非: 通过路径的方式加载第三方模块

5. 内置模块,只能通过`模块名称`来加载,  (错误示例: require('./http') )

6. require() 加载模块使用 ./ 相对路径时，相对路径是相对当前模块，不受执行 node 命令的路径影响

> 演示:
>
> 1. a 引入 b 打印 log(99)
> 2. 多次 引入 b , 打印几次? 缓存?

### 三、require函数加载模块原理（通过源码看）( 了解 )

- 加载模块,内部到底到了什么?


- **Moduel.prototype.require . js**

### 

### 四、补充 CommonJS 规范

1. [CommonJS 规范](http://www.commonjs.org/)
2. [CommonJS规范-ruanyifeng](http://javascript.ruanyifeng.com/nodejs/module.html)
3. CommonJS 是为 JavaScript 语言制定的一种 模块规范、编程 API规范
4. node.js 遵循了 CommonJS规范


- 我们之前学过一种规范: 标准的 jS 规范,官网推荐的: ECMAScript;
- 这个规范规范js 里有哪里数据类型,比如数组和字符串,也规范了数组和字符串有哪有方法 api,,,也规范了怎么写循环,,
- 就因为有这些规范,所以 JS chrome V8 引擎里,也实现这里这些基本的 api, 就没有像 console 和 setTimeout 这些 api,, 这些都是浏览器  web API



-  nodejs 的规范:  commonjs 规范,
-  为什么会出现 commonjs 规范呢,,,因为官网 ECMA 的规范,只规范了最基本的 能力,, 它并没有规范写大项目的时候怎么进行模块化也没有规范模块化之间怎么进行通信,,,
-  commonjs 就是规范了这些而出的,,它告诉我们开发大项目如何使用模块化开发,如果进行模块之间的通信,
-  它只是在官网的 ECMASCript 规范上又拓展 了一些新的规范,,
-  因为 node 遵循了这些规范,所以 node 就能够进行模块化开发,,





###四. 模块之间是怎么通讯的

```js
//1.1  b.js 的内容,, a 可以引入一下
function add(num1,num2) {
  return num1 + num2;
}

var num = add(20,20);
console.log(num);

//1.2  引入后
var b = require('./a')   // 执行打印结果
console.log(b);   // 打印是一个空对象

//2. 如果想反悔一个字符串? 或者其他的呢??
// return 'hello world'  => NO

//导出一个值
module.exports = 'hello world'
module.exports = 666
module.exports = function () {
    console.log('这是 b 模块里的值')
}

/**
 * 知识点总结:
 * 1. 加载模块 require()
 * 2. b 接收过来的东东 默认是 {};
 * 3. 导出值,用 module.exports
 *  - 可以导出字符串
 *  - 可以导出 number
 *  - 可以导出 函数
 * 
 * 4. 通信我们已经知道了,,在一个模块里面,,导出用 module.exports ,导入是 require()
 * 5. module.exports 到底是个什么东西呢????
 */

```



### 五、module.exports 介绍  ( 重点 )

1. 导出模块  =>  以对象的形式导出

   外面加载 就是 加载 module.exports 的值,,

   module: 代表当前模块  exports 是 当前模块的一个空对像

2. ```js
   /**
    * 专门介绍一下 module.exports
    * 常见使用
    */

    //1. 它是默认是一个空对象
   console.log(module.exports); // => {}

    //2. 可以直接导出一个对象
    module.exports = {
       age:19,
       name:' 哈哈',
       say:function () {
         console.log('say hello')
       }
     }

   //3. 既然 module.exports 是一个空对象,也可以通过添加对象的属性
   module.exports.age = 12;
   module.exports.name = '哈哈';
   module.exports.say = function() {
     console.log('hello')
   }

   //4. 可以导出对象方法,利用它传值 (重点)
   module.exports.play = function (playName) {
     console.log('玩'+playName);
   }
   module.exports.eat = function (food) {
     console.log('吃'+food)
   }

   //5.导出对象还有一个 exports
   exports.drinking = function () {
      console.log('喝')
    }
   // ??????  exports 和 module.exports 有什么区别???
   ```






###六、module.exports 与 exports 的区别

- 1. 两者都指向同一块  =>    **内存空间**;

  2. ```js
     /**
      * module.exports 和 export 是的区别
      *
      */

        //1. 导出一个对象
        // 目前来看,,module.exports 和 exports 没有没有什么 区别 
        module.exports.name = '哈哈';
        exports.age = 13;
        exports.say = function () {
          console.log('say hello')
        }

        //2. exports 只是 module.exports 的一个别名 ,快捷方式
        // 导出对象,只导出 module.exports
        module.exports = 'hello world'
        exports = 'world'

        //3. 通过函数名 和返回值,可以得到  require() 加载过来的模块是人家通过 module.exports 导出来的
     	[画图 左边栈 右边堆 和地址指向问题]

        //4. exports 和 module.exports 开始默认的时候,指向的同一片内存
     ```


        function require(/* ... */) {
          const module = { exports: {} };
    
          ((module, exports) => {
            // 模块代码在这。在这个例子中，定义了一个函数。
            function someFunc() {}
            exports = someFunc;
            // 此时，exports 不再是一个 module.exports 的快捷方式，
            // 且这个模块依然导出一个空的默认对象。
            module.exports = someFunc;
            // 此时，该模块导出 someFunc，而不是默认对象。
          })(module, module.exports);
    
          return module.exports;
        }
     ```

