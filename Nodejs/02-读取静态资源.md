### 案例1: 编写 http 服务程序 - 读取静态资源响应用户请求

##### ☆ 步骤: 

> 别忘了看控制台的样式 content-type

``` js
1. 创建 index.html、  404.html  两个页面;
2. 演示通过读取最简单的 html 文件来响应用户;
3. 演示读取 **外部css 样式** 的 html 文件来响应用户;
4. 演示读取具有 **img 标签** 的 html 文件来响应用户;
```

- [Content-Type参考 OSChina](http://tool.oschina.net/commons)

☆代码:

```js
//1. 加载 http 模块
var http = require('http')
var fs = require('fs');
var path = require('path')

//2. 创建 开启 监听
http.createServer(function (req, res) {
  console.log('有人访问服务器了');
  if (req.url == '/' || req.url == '/index') {
    // 读取 html 文件
    fs.readFile(path.join(__dirname, './htmls/index.html'), function (err, data) {
      // 抛出错误
      if (err) {
        throw err
      }
      // 返回给浏览器
      res.end(data);
    })
  } else if (req.url == '/images/dog.jpg') {
    console.log('请求图片了');
    res.setHeader('Content-Type', 'image/jpeg;charset=utf-8');
    fs.readFile(path.join(__dirname, './images/dog.jpg'), function (err, data) {
      // 抛出错误
      if (err) {
        throw err
      }
      // 返回给浏览器
      res.end(data);
    })
  } else if (req.url == '/css/demo.css') {
    console.log('哟人请求 css 了')
    res.setHeader('Content-Type', 'text/css;charset=utf-8')
    fs.readFile(path.join(__dirname, './css/demo.css'), function (err, data) {
      // 抛出错误
      if (err) {
        throw err
      }
      // 返回给浏览器
      res.end(data);
    })
  } else {
    // 读取 html 文件
    fs.readFile(path.join(__dirname, './htmls/404.html'), function (err, data) {
      // 抛出错误
      if (err) {
        throw err
      }
      // 返回给浏览器
      res.end(data);
    })
  }
  
}).listen(8080, function () {
  console.log('服务器开启了,http://localhost:8080');
})
```



### 案例1改进 :  整合优化代码---模拟 Apache 实现静态资源服务器

- mime 第三方模块使用

- [mime 使用](https://www.npmjs.com/package/mime) 

  ``` js
  1. 使用终端命令行演示:
  mine demo.css  -> text/css
  mime demo.jpg  --> image/jpeg
  mime p/demo.css  --> ok
  mime p/demo.jpg  --> ok
  总结:  mime 文件/路径  只看后缀
  2. 终端可以用的原因是得需要安装: 在终端任意地方  npm i mime -g 即可
  3. 那么在代码里怎么写呢?
    ----查看 npm 官网
  ```

  ​

  - 安装: `npm init -y `   `npm i mime -S`   (文件夹不要使用中文)
  - 加载:`var mime = require('mime')`
  - 使用: `mime.getType('.../XXX.css') -----> text/css`
  - 总结: `mime.getType('文件名/文件路径')  看中是 后缀名`

```js
// 目标: 演示 静态资源
// 能不能把所有的静态资源文件判断放在一个 if 语句下面
// 目前来看,找不到共同点啊??
// 给一个 public 公共的文件夹

//1. 加载 http 模块
var http = require('http')
var fs = require('fs');
var path = require('path')
const mime = require('mime')

//2. 创建 开启 监听
http.createServer(function (req, res) {

  req.url = req.url.toLowerCase();

  console.log('有人访问服务器了');

  res.setHeader('Content-type',mime.getType(req.url));

  if (req.url == '/' || req.url == '/index') {
    // 读取 html 文件
    fs.readFile(path.join(__dirname, './htmls/index.html'), function (err, data) {
      // 抛出错误
      if (err) {
        throw err
      }
      // 返回给浏览器
      res.end(data);
    })
  } else if(req.url.startsWith('/public')){
    //   req.url
    //   /public/css/demo.css
    //   /public/images/dog.jpg

    // getType('..../.css') ==> text/css
    // getType('..../.jpg') ==> imgae/jpeg

    console.log(req.url)

    fs.readFile(path.join(__dirname, req.url), function (err, data) {
      // 抛出错误
      if (err) {
        throw err
      }
      // 返回给浏览器
      res.end(data);
    })
  }else {
    // 读取 html 文件
    fs.readFile(path.join(__dirname, './htmls/404.html'), function (err, data) {
      // 抛出错误
      if (err) {
        throw err
      }
      // 返回给浏览器
      res.end(data);
    })
  }

}).listen(8080, function () {
  console.log('服务器开启了,http://localhost:8080');
})
```



### 在请求服务器的时候，请求的 url 就是一个标识！

```js
   // jd  taobao 都可以  只是个标识
  if (req.url == '/jd') {

    fs.readFile(path.join(__dirname,'./index.html'),function (err,data) {
      
      if (err) {
        throw err
      }

      res.end(data)
    })
    
  } else {
    res.end('404')
  }
```



### request  和 response  介绍

> 用一个简单的判断 index 并返回 ok 的例子

- [node HTTP 官网](https://nodejs.org/dist/latest-v6.x/docs/api/http.html)


- 在 node 官网找 需要对应: **request ( http.IncomingMessage )  **和 **response ( http.ServerResponse ) **

#### ☆ request 对象

- 服务器解析用户提交的 http 请求报文，将结果解析到 request 对象中。凡是要获取和用户请求相关的数据都可以通过 request 对象获取

- request 对象类型 <http.IncomingMessage>, 继承自stream.Readable

- request 对象常用成员: 

- **1. request.headers — 请求头 (对象)**     

  //https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/User-Agent

  ```js
  // 打印 request.headers
  { host: 'localhost:8080',
    connection: 'keep-alive',
      // 人家默认的,不要动它,,知道也没用,,也改变不了
    'user-agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/61.0.3163.100 Safari/537.36',
      //User-Agent 告诉HTTP服务器， 客户端使用的操作系统和浏览器的名称和版本.
      
    'upgrade-insecure-requests': '1',//是否可使用更高的版本进行通信
    accept: 'text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8',
      //浏览器端可以接受的媒体类型,
    'accept-encoding': 'gzip, deflate, br',
      // 浏览器申明自己接收的编码方法 通常指定压缩方法，是否支持压缩，支持什么压缩方法（gzip，deflate）
    'accept-language': 'zh-CN,zh;q=0.8,en;q=0.6' 
   //浏览器申明自己接收的语言。
  }
  ```

- **2. request.rawHeaders — 请求头 (数组)**

  ```js
  // 打印 request.rawHeaders
  [ 'Host',
    'localhost:8080',
    'Connection',
    'keep-alive',
    'Cache-Control',
    'max-age=0',
    'Upgrade-Insecure-Requests',
    '1',
    'User-Agent',
    'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/61.0.3163.100 Safari/537.36',
    'Accept',
    'text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8',
    'Accept-Encoding',
    'gzip, deflate, br',
    'Accept-Language',
    'zh-CN,zh;q=0.8,en;q=0.6' ]
  ```

  ​

- **3. request.httpVersion  — http 版本**

  ```js
  1.1 
  ```

- **4. request.method  — 请求方式**

  ```js
  GET / POST ....
  ```

- **5. request.url  — 请求 url 路径**

  ```js
  /
  /index
  /login/pass
  ```

#### ☆ response 对象

- 在服务器端用来向用户做出响应的对象。凡是需要向用户（客户端）响应的操作，都需要通过 response 对象来进行。

- response 对象类型 <http.ServerResponse>

- response 对象常用成员: 

- **1. response.write(chunk[, encoding]_[, callback]) — 写入数据 发送浏览器 **

  - 参数1:  要写入的数据,可以是字符串 或者 二进制数据   **必填 **    <string> | <Buffer>
  - 参数2: 编码, 默认是 utf8 ;选填
  - 参数3: 回调函数;选填

- **2. response.end([data]_[, encoding]_[, callback]) — 结束响应  (★★)**

  - 参数1: 结束响应前要发送的数据,  选填    <string> | <Buffer>
  - 参数2: 编码,  选填
  - 参数3: 回调函数,  选填
  - 注意: 每次响应都必须调用该方法，用来结束响应

  ```js
  - This method signals to the server that all of the response headers and body have been sent; that server should consider this message complete. The method, response.end(), MUST be called on each response.

  - res.end()这个方法告诉服务器所有要发送的响应头和响应体都发送完毕了。可以人为让这次响应结束了。
  ```

  ​

- **3. response.setHeader(name, value) — 设置响应报文头 (★★)** 

  - 告诉浏览器解析文本是以什么格式解析,又以什么编码格式解析

  ```js
  // 可多次使用 
  res.setHeader('Content-Type','text/plain;charset=utf-8')
  ```

- **4. response.statusCode — 设置或者读取 http 状态码**

- **5. response.statusMessage — 设置或读取 http 响应状态消息**

​        [MDN-HTTP response status codes](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status)

- 注意: (顺序不要乱写,尽量参考下面)

  ```js
    // 设置响应报文头
    res.statusCode = 400;
    res.statusMessage = 'hehe'
    res.setHeader('Content-Type','text/plain;charset=utf-8')

    // 设置响应报文头  (约等于上面)
    res.writeHead(200,'OK',{
      'Content-Type':'text/plain;charset=utf-8'
    })
   
    // 写入数据,,返回给浏览器
    res.write('哈哈哈')

    // 结束响应
    res.end('over')
  ```

  **6. response.writeHead(statusCode [, statusMessage]_[, headers]) — 设置响应头信息  (★)**

  - 参数1: 状态码

  - 参数2: 状态信息

  - 参数3: 响应头

  - 实例代码:

    ```js
    // 示例代码：
    res.writeHead(200, 'OK', {
      'Content-Type': 'text/html; charset=utf-8',
      'Content-Length': Buffer.byteLength(msg)
    });
    ```

  注意:

  ```js
  1. This method must only be called once on a message and it must be called before response.end() is called.

  - 这个方法在每次请求响应前都必须被调用（只能调用一次）。并且必须在end()方法调用**前**调用
  ```

  ```js
  2. If you call response.write() or response.end() before calling this, the implicit/mutable headers will be calculated and call this function for you.

  - 如果在调用writeHead()方法之前调用了write() 或 end()方法，系统会自动帮你调用writeHead()方法，并且会生成默认的响应头
  ```

  ####

- ####总结

  ```js
  1. res.end() 放在最后;
  2. setHeader/statusCode/statusMessage  和 writeHead(statusCode [, statusMessage]_[, headers]) 最好只使用一个
  3. setHeader() 最好放在最前
  ```

  ​

### NPM  —> 详见 NPM 文档

### package.json、package-lock.json 文件介绍

1. ##### package.json

   ```Js
   {
     "name": "05-hackernews",
     "version": "1.0.0",
     "description": "",
     "main": "index.js",
     "scripts": {
       "test": "echo \"Error: no test specified\" && exit 1"
     },
     "keywords": [],
     "author": "",
     "license": "ISC",
     "dependencies": {
       "mime": "^2.0.3"
     }
   }
   ```

2. ##### package-lock.json

   ```Js
   {
     "name": "05-hackernews",
     "version": "1.0.0",
     "lockfileVersion": 1,
     "requires": true,
     "dependencies": {
       "mime": {
         "version": "2.0.3",
         "resolved": "https://registry.npmjs.org/mime/-/mime-2.0.3.tgz",
         "integrity": "sha512-TrpAd/vX3xaLPDgVRm6JkZwLR0KHfukMdU2wTEbqMDdCnY6Yo3mE+mjs9YE6oMNw2QRfXVeBEYpmpO94BIqiug=="
       }
     }
   }
   ```

   ​

### ★ HackerNews 网站案例

1. [模仿网站 HackerNews](https://news.ycombinator.com/)

2. 步骤: (有待二次加工)

   ```js
   /**
    * 步骤:
    * 1. 简单的 http 服务器程序 + 导入 views + resources
    * 2. 根据不同的 ul, 加载不同的 html 静态页面
    * 3. 加载 静态资源   
    *    //1. 拼接路径
         //2. 读取静态资源
         //3. 返回给浏览器
    * 4. 处理 add 提交的 get 请求
    *    4.1 修改 submit.html 的代码 (method:get+action:/add) 
    *    4.4 添加 url 路由
    *    4.5 加载 node 自带的 url 模块  URL.parse(url,true)
    *    4.6 拼接数组 + 写入 data.json + 重定向
    * 5. 封装
    * 6. 每次添加新数据,都会把之前的给覆盖掉问题
    *    -  从 data.json 取出来之前的,然后添加进去,再写入 data.json
    *    -  不能使用 appendFile, 以为不可能是数组跟对象拼接在一起吧  
    *    -  注意: 第一次读取文件肯定有错误,,但是没有必要抛出异常
    *    - 第一次解析数组是个 undefined, 给个可选的 '[]'
    * 7. post 请求
    *    7.1  修改 html 的 method 和 action
    *    7.2 从 data.json 读取数据
    *    7.3 拼接新对象 
    *     - 获取 post 传递过来的数据
    *     - 是通过一段一段的 buffer 传过来的额
    *     - 所以我们需要 buffer 一段一段的接收
    *     - 然后合并在一起,通过 toString 转化为一起,但是是个查询字符串
    *     - 查询字符串需要使用 querystring 处理
    *    7.4 写入到 data.json
    *      
    */
   ```

   ​


#### 模板引擎 [underscore](http://underscorejs.org/#template)

1. 安装: `npm i underscore -S`
2. 加载:

```js
var _ = require('underscore');
```

3. 使用:underscore 三步走

   ```js
   //1. 有个模板文件 
   var oldHtml = '<h1><%= name %></h1>'

   //2. 生成模板函数 
   // 参数: 模板文件
   // 返回值: 模板函数
   var template = _.template(oldHtml)

   //3. 传值
   var newHtml = template({ name:' 小新哥' })
   console.log(newHtml);
   ```

   ​

#### 补充:

- 1、注意在发送不同类型的文件时，要设置好对应的`Content-Type`
  - [Content-Type参考 OSChina](http://tool.oschina.net/commons)
  - [Content-Type参考 MDN](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/MIME_types)
- 2、HTTP状态码参考
  - [w3org参考](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html)
  - [MDN-HTTP response status codes](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status)
- 3、在html页面中写相对路径'./' 和 绝对路径 '/'的含义 。
  - 网页中的这个路径主要是告诉浏览器向哪个地址发起请求用的
  - **'./'**  表示本次请求从相对于当前页面的请求路径（即服务器返回当前页面时的请求路径）开始
  - **'/'**   表示请求从根目录开始

步骤2:草稿:

```js
1. 编写一个简单的 http 服务器

2. 根据不同的 url 加载不同的静态页面
 2.1 根据不同的 url 返回不同的文测试
 2.2 add get 显示 404 的原因是因为大小写GET 转化一下
 2.3 导入 html 页面(无模板) 资源 `views` 和 静态资源 `resources`
 2.4 根据不同的 url 请求读取 html 页面  index detail submit
 2.5 静态资源有问题: mime 处理样式 + 读取静态资源文件

3. 封装 res.ml_render()

4. 处理 add get
    4.1. 获取数据  
    ```js
     if (err && err.code != 'ENOENT') {
        throw err;
      }
    ````
    4.1.1
       ```js
       var URL = require('url');
       var urlObj =  URL.parse(req.url);
       ```
    4.1.2 req.url 已经不再是等于 '/add'了

    4.2. 写入数据到 data.json

    4.3. 重定向
    ```js
     res.statusCode = 301;
     res.statusMessage = 'Moved Permanently';
     res.setHeader('location','/')

    ```

5. 处理 add get
 - // 注意这里,因为 end 介绍是异步的,   所以 如果提前 end 了,就监听不到 二进制数据了

----
 6. 模板字符串:
  var template = _.template(data.toString());
        //3. 传值

  7. id
```

