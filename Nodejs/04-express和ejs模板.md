

###Node.js 第四天



####一、模块化改造 Hacker News 思路(5个模块)

1. 模块化开发: 就是吧不同的功能,放到不同的模块中(.js);
2. 为什么要进行模块化? 
   - 有利于多人合作开发
   - 方便代码的维护,迭代更新
3. 怎么进行模块化?
   - 1. 创建模块  :  router.js
     2. 导出模块: 
        - module.exports = {}     用在配置信息
        - module.exports.index = fun()     导出多少,,, 
        - module.exports = func()  直接导出一个,,而且需要参数  
        - 1. 封装的内容
          2. 是否有参数,函数 并且,参数传过来
          3. 是否有加载模块,,(自定义的函数 )
     3. 加载模块  require('./router');
     4. 使用模块



###二、Express

- 什么 express?
  -  Express 是基于 `Node.js` 平台，快速、开放、极简的一个'' web 开发框架'' , 
  -  同时也是 'Node.js' 的一个 第三方模块。
- 为什么学习 express 框架?
  - 让我们基于 Node.js 开发 web 应用程序更高效
  - 不会 express, 等于没有学过 node
- express 官方网站
  - [英文官网 - http://expressjs.com/](http://expressjs.com/)
  - [中文官网 - http://www.expressjs.com.cn/](http://www.expressjs.com.cn/)

#### Express 特点  

> 先了解,知道个概念,,后面会细讲

1. 实现了`路由`功能   (eg: express.Router())
2. `中间件`（函数）功能;  (eg. App.use('/',function{ … }))、  配合 use 一块使用
3. 对` req 和 res` 对象的`扩展`
4. 可以集成其他`模板引擎` (eg: ejs )




###★ Express 基本使用

##### 1. 基本步骤

- 安装:   `npm i express -S`        // 安装之前: 先`npm init -y` 初始化 package.json 文件
- 加载:   `var express = require('express')`
- 实例:   `var app = express()`      // 实例化 express 对象
- 使用:   ` app.get()/app.use()/app.all() …. App.listen()`

##### 2. 演示 Hello World 案例

```js
//1. 加载express
var express = require('express');

//2. 实例
var app = express();   //  (类似于创建一个 server 对象)

//3. 使用
// 参数1: 路径
// 参数2: 回调函数
app.get('/',function (req,res) {
  res.end('Hello world')
})

// 开启服务器
app.listen(8080,function () {
  console.log('服务器开启了 http://localhost:8080')
})

//1. 步骤 1-2-3-4
//2. 
```

##### 3.  res.send() 和 res.end() 的异同 ? 

- 相同点: 都能够结束响应,把内容响应给浏览器

- 不同点: 

  - 1. send () 不乱码:

    ```js
    res.send() 会自动发送更多的响应报文头,其中就包括: Content-Type:text/html; charset=utf-8 所以没有乱码;
    ```

  - 1. 参数类型不同:

    ```js
    - res.send() 参数可以是 a Buffer object、a String , 还有 an object、an Array
    - res.end() 参数类型只能是: Buffer 或者 String 
    ```

- 总结: 以后在 express 推荐使用: send()  发送HTTP响应;



**4.  res 的其他几个常用的方法**

- **res.redirect([status,] path) : 重定向**

  ```js
    res.redirect('https://www.baidu.com');
    res.redirect(301, 'https://www.baidu.com');
  ```

- **res.sendFile(path [, options]_[, fn])  读取文件**

  ```js
  // 以前 : 读取文件并响应
    fs.readFile(path.join(__dirname,'./index.html'),function (err,data) {
      if (err) {
        throw err
      }
      res.end(data);
    })

  // 现在 : 
  // 2.1 不需要回调函数
  res.sendFile(path.join(__dirname,'./demo.html'))
  // 2.2 需要回调函数
  res.sendFile(path.join(__dirname,'./demo.html'),function (err) {
      if (err) {
        throw err
      }
      console.log('ok')
    })
  ```

- **res.status() : 设置状态** 

  ```js
   res.status(404).send('文件不存在！');
  ```


###★ Express 中注册路由的方法

#####方法一 :  app.METHOD()

> 固定类型,路径完全匹配

​	**1. 基本用法**

- Method 是一个 http 请求方法: 例如:  get / post / put /  delete 等等

- ```js
  1. 请求方式固定
  2. 路径完全匹配
  ```

  ```js
  // 参数1: 路径
  // 参数2: callback 回调  
  app.get('/index',function (req,res) {
    
      res.send('index')
  })
  ```

  ![pathname](md-imgs/pathname.png)

  ​

#####方法二 :   app.use()

> 开头是: /index 就匹配
>
> 任意类型,路径开始相同就匹配

- 1. 在进行路由匹配的时候,不限定方法,什么请求方法都可以 
- 1. 请求路径的第一部分只要与 /index 相等即可,,,并不要求请求路径 ( pathname ) 完全匹配

```js
app.use('/index',function (req,res) {
  res.send('hello 你好世界')
})
```

##### 方法三 : app.all()  

> 任意类型, 路径完全匹配

- 1. 不限定请求方法;
  2. 请求路径的 pathname 必须完全匹配;

  ```js
  app.all('/index',function () {
    res.send('index');
  })
  ```

  ​

###★ Express 处理静态资源

1. 使用 express 按照以前的逻辑写:

   ```js
   //3. 注册路由
   app.get('/',function (req,res) {

     res.sendFile(path.join(__dirname,'./demo.html'));
   })

   // if(req.url.startWith('/public)) { 拼接路径 返回绝对路径的文件 }

   // 处理静态资源
   //app.use(path,callback)  //use 请求路径的第一部分只要与 path 路径匹配即可
   app.use('/public',function(req,res){

     res.sendFile(path.join(__dirname,'./public/demo.css'));

   })
   ```

   ​

2. #####使用 express 的内置模块   [express.static](http://www.expressjs.com.cn/starter/static-files.html)

   ```js
   //官网原话:
   通过 Express 内置的 express.static 可以方便地托管静态文件，例如图片、CSS、JavaScript 文件等。

   将`静态资源文件所在的目录`作为参数传递给 `express.static 中间件`,就可以提供静态资源文件的访问了。
   ```

    ##### 2.1 使用路径   /public   (推荐使用)

>  href="./public/demo.css"     src="./public/dog.jpg"

````js
// 因为1 express.static 是中间件 中间件也就是函数 所以,,可以替换后面的函数
 官网的话:  app.use('/public',express.static());
//因为2.上面说到`静态资源文件所在的目录`作为参数传,所以传目录
// 目录:  path.join(__dirname,'./public')
app.use('/public',express.static(path.join(__dirname,'./public')));
````
   ##### 2.2  使用路径 /

```js
//1. 如果把引入 css 和 图片的地方改为: 'href="./demo.css' 和 src="./dog.jpg"
// 那么请求路径的第一个部分就是   /   那么 use 匹配的就是匹配 /
// 同理也是,,只要前面的匹配,,后面把静态文件夹目录作为参数传过去
app.use('/',express.static(path.join(__dirname,'./public')));
// express 的 use 规定 如果匹配的根目录 是  / 可以省略
app.use('/',express.static(path.join(__dirname,'./public')));
```

###  额外补充:

1.  例如:静态文件叫: demo.css
   - ./demo.css  —> 请求路径的第一个部分是: /
   - ./public/demo.css  —> 请求路径的第一个部分是: /public
2.  express 中的 req.url 已经不是之前的那个了
   - /public/demo.css  —> http的 req.url = /public/demo.css 
   - /public/demo.css  —> express的 req.url = /demo.css 

### ★ 使用 Express 封装 Hacker News



> 1. 创建一个入口模块:  server.js
> 2. 安装: express + 编写启动服务模块(不同的 url)
> 3. 配置模块 config.js
> 4. 路由模块
>    - router.get('/submit',handler.submit)
> 5. 业务模块:  module.exports.index = function(req,res){..}
> 6. 加载数据  (先加载静态的)  
>    - res.sendFile() [res.sendFile](http://www.expressjs.com.cn/4x/api.html#res.sendFile) 作展示数据可以、传值不可以;  (先和 submit 的不需要传值的加载出来)
>    - 静态资源:router.use('/resources',express.static(path.join(__dirname,'./resources')))
>    - res.render() 查看官网:[res.render](http://www.expressjs.com.cn/4x/api.html#res.render)
>    - 想要实现效果,必须要配合模板引擎:[express 模板引擎](http://www.expressjs.com.cn/guide/using-template-engines.html)
> 7. express 配合 ejs 使用模板引擎 (取值赋值)
>    - 在 demo 里展示个例子
>    - 在项目里创建一个 demo.ejs 使用
>    - 做一个假值: list = []; 自己编写的
> 8. addGet 和 addPost?  mondb 再用

#### Express的路由模块

- 正常的配置是这样的:

  ```js
  //创建 router.js
  module.exports = function (app) {
    app.get('/',function (req,res) {
      res.send('index')
    })
    app.get('/submit',function (req,res) {
      res.send('submit')
    })
  }
  // app.js 呢
  var router = require('./router') 
  router(app);
  ```

- 效果是能完成

   但是这样暴露了 app, , app 是个对象,,,,可以随时改里面的东西, 很不安全

- 所以: 使用 express 自带的路由类: **express.Router()**

  ```js
  //创建 router.js
  //1. 加载 express 和 router
  var express = require('express');
  var router  = express.Router()

  //2. 配置 router
  router.get('/',function (req,res) {
    res.send('index')
  })
  router.get('/submit',function (req,res) {
    res.send('submit')
  })

  //3. 导出 router
  module.exports = router;

  // app.js
  var router = require('./router') //加载

  // 路由 挂载到 app 上
  // app.use([path,] fn)  
  // app.use('/' fn) 
  app.use(router);
  ```

#### Express 的读取页面和传值问题??

```js
module.exports.index = function (req,res) {

  //1. 读取页面  正常读取网页没有问题  
  //2. 但是  无法传值 
  res.sendFile(path.join(__dirname,'./views/index.html'),function (err) {
    if (err) {
      throw err
    }    
  })

 
  //2. res.render() 配合 ejs 这个模板引擎使用
  res.render('index',{name:' 哈'})
}
```



####  Express 中的 res.render() 配合 ejs [模板引擎](http://www.expressjs.com.cn/guide/using-template-engines.html) 

   一、使用后缀`.ejs`的

```js
//1. app.js 模块
//配置使用 ejs 模板引擎
// 配置 express 使用 ejs 引擎
// 配置模板引擎要放到正式处理请求之前,否则可能不能用
// 当使用 esj 模板引擎的时候,模板文件的后缀必须是 .ejs
//1. 安装 esj:
   `npm i ejs -SD`
//2. 配置 
//   - 告诉 express 模板文件的目录    
//    + views 是固定的
//   - 告诉 express 要使用的模板引擎
app.set('views', path.join(__dirname,'./views'))
app.set('view engine','ejs');

//2. handler.js 模块  ---前提: 在 views 下创建一个 test.ejs 文件  
 // 测试 views/test.ejs 已经 ok
  res.render('test',{name:' 星哥'})

//3. test.ejs
 <h1><%= name %></h1>
```

二、 使用后缀 `.html`的

```js
//1. app.js 模块
// 配置使用 ejs 模板引擎 , 修改默认查找的模板文件后缀为.html
//1. 设置模板文件的目录
app.set('views',path.join(__dirname,'./views'));
//2. 创建一个自己的模板引擎, 用来识别后缀是 html 的
// 把渲染 esj 的功能都交给渲染 html 后缀的模板引擎上
app.engine('html',require('ejs').renderFile);

//3. 使用模板引擎
app.set('view engine','html')

//2. handler.js 模板
 //要把 .ejs 全部变为 .html 才可以啊
  res.render('test01',{age:90})

//3. test01.html  是 views 目录下创建的模板
<h1><%= age %></h1>
```



###★ 模板引擎 - ejs : 

- esj 官网](https://www.npmjs.com/package/ejs)
- 说明: express 里可以渲染文件 res.sendFile(),,但是不能传值,所以放弃使用;

**◆ ejs 基本使用**

1. 安装: `npm i ejs -S`

2. 加载: `var ejs = require('ejs')`

3. 使用: 官网提供:

   ```js
   // Example
   <% if (user) { %>
     <h2><%= user.name %></h2>
   <% } %>
     
   // Usage
   // 方法一:
   var template = ejs.compile(str, options);
   template(data);    // 返回一个新的 html
   // => Rendered HTML string 
    
   // 方法二:
   ejs.render(str, data, options);   // 返回一个新的 html
   // => Rendered HTML string  
    
   // 方法三:
   ejs.renderFile(filename, data, options, function(err, str){   //str 就是新的 html
       // str => Rendered HTML string 
   });
   ```

- 方法一: **ejs.compile(str, options)** (适用于 html 代码片段)

  ```js
  /// 方法一: 
  //html 片段
  var oldHtml = '<h1><%= name %></h1>'

  //模板函数
  var template = ejs.compile(oldHtml);

  //传值
  var newHtml = template({name:'哈哈'})

  console.log(newHtml)
  ```

  ​

- 方法二: **ejs.render(str, data, options)**

  ```js
  /// 方法二:
  //html 片段
  var oldHtml = '<h1><%= name %></h1>'

  // 渲染并赋值
  var newHtml = ejs.render(oldHtml,{name:'哈'})

  console.log(newHtml)
  ```

  ​

- 方法三: **ejs.renderFile(filename, data, options, function(err, str)**

  ```js
   /// 方法三:
   ejs.renderFile(path.join(__dirname,'./index.html'),{title:'这是标题',name:' 星哥'},function (err,str) {
     console.log(str); // 新的 html
   })

   // index.html 代码

   <body>

   <% for ( var i = 0;i < 5; i++ ) {%>

       <li><%= name %> </li>

     <% } %>

   </body>
  ```




1.  demo 演练:

    ```js
    /**
    * 使用 ejs 后缀
    */
    var express = require('express')
    var app = express();
    ```

   //1. 告诉程序 模板文件放在什么地方
   app.set('views',path.join(__dirname,'./Views'))
   //2. 自定义自己的模板引擎
   app.engine('html',require('ejs').renderFile)
   // //2. 使用引擎
   app.set('view engine','html')

   //注册路由
   app.get('/',function (req,res) {

       // 直接写 views 下的文件名就可以 
       res.render('demo',{name:' 牛逼'})

   })

   //开启服务器
   app.listen(9090,function () {
     console.log('开启了 http://localhost:9090')
   })
   ```

5. 项目实战呢? 配合 render

   ```js
   // handler.js 
   //1. 获取本地数据
      ml_readData(function (data) {
       
           //2. 数组接收
           var list = JSON.parse(data || '[]');
       
           //3. 开始渲染页面
           // res.ml_render(path.join(__dirname, './views/index.html'), list);
       
             res.render('index',{list:list});
         })

   ```

   ​











