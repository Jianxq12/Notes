使用最基本的 http + if 路由 来做 hackernews

###步骤:

\```

1步.创建 server.js + 素材(resources+views);

2步. 设置路由: 根据不同的 url + method, 加载不同的 html 静态文件;

​    \- /index   index.html

​    \- /detail  detail.html

​    \- /submit  submit.html

​    \- /add     get

​    \- /add     post

​    \- /404     404

3步. 加载静态资源文件

   (req.url.startsWith('/resources') && req.method == 'get')

​    //3.1. 拼接路径

​       var file = path.join(__dirname,req.url);

​    //3.2. 读取文件

​    //3.3  结束响应

4步. 封装渲染

  \- 封装 渲染页面 = 读取文件 + res.end(data) + 静态资源的 

  \-  ml_render(path.join(__dirname, './views/submit.html'), res);

  \-  res.ml_render(path.join(__dirname,'./views/submit.html'));

​      

5步. 处理 add 提交的 get 请求

  \- 修改:   <form method="get" action="/add">

  \- 需要涉及 获取`/add?title=111&url=111&text=111`的key 对一个的值,,,而且不止一次用到, detail 也会用到

  \- 所以: 我们第一用插件 url ,第二,提到前面去

  \- 写入到本地之后,重定向

6步. 先获取,再拼接 , 给个 id

  \- 每次这是写入一个数组,一个数组又一个只有一个对象,,所以不合理

  \- 解决办法: 先从本地获取一个数组,然后把这个对象放到数组里去

  \- 先读取文件,再写入文件

  \- 顺便给个 id

7步. 处理 add 提交的 post 请求

   \-  修改:   <form method="post" action="/add">

   \- 获取新对象 (post 请求的数据是 buffer 一段一段传的,我们需要监听 data 和 end)

   \- 注意坑.不要把 res.end() 放在外面,以为是异步,就不会监听 data 了

\-----------------------------------------

8步: 在 index.html 读取数据并且赋值  detail.html 也是

​    \- 读取数据

​    \- 使用模板赋值 underscore

​    \- 注意. detai.html 少了一个 utf-8

\9. 封装

  \- 封装1: 读取文件

  \- 封装2: 写入文件

  \- 封装3: post 传值

\```

###涉及知识点:

1.内置模块 url ,转化 包含查询字符串的 req.url

 \- var URL = require('url')

 \- URL.parse(req.url)  -> query 依然是查询字符串

 \- URL.parse(req.url,true)  -> query 才是一个 url 对象

2.内置模块 queryString, 查询字符串 转化为对象

  \- var queryString =  require('queryString');

  \- querystring.parse(bodypost)

\3. 第三方模板: mime 

​    \- 静态资源转化样式 防止报警告

​    \- var mime = require('mime')

​    \- mime.getType('路径')

\4. 第三方模块: underscore

\```js

var _ = require('underscore');

//使用 underscore 三步走

//1. 有个模板文件 

var oldHtml = '<h1><%= name %></h1>'

//2. 生成模板函数 

// 参数: 模板文件

// 返回值: 模板函数

 var template = _.template(oldHtml)

//3. 传值

 var newHtml = template({ name:' 小新哥' })

 console.log(newHtml);

\```

###其他:

\1. 读取文件如果发生错误,不需要 throw err ,因为这是做项目了

if (err) {

  res.writeHead(404,'Not Found')

  res.end('404 no found page')

}

\2. 处理大小写 因为 method = GET 和 POST

req.url  = req.url.toLowerCase();

req.method = req.method.toLowerCase();

\3. 数组转化为字符串

JSON.stringify(list)

\4. 重定向

res.statusCode = 301;

res.statusMessage = 'Moved Permanently';

res.setHeader('Location','/');

res.end()  // 一定要加上这个

\5. 错误,又不是文件不存在的错误处理

if (err && err.code !== 'ENOENT') {

​      throw err;

}

\6. 拼接 buffer 的传递

var bufferArr = [];

req.on('data',function (chunk) {

  console.log('111')

  bufferArr.push(chunk);

})

req.on('end',function () {

  console.log('22')

 //把 buffer数组拼组成一个完整的 buffer

  var postBody =  Buffer.concat(bufferArr);

  postBody = postBody.toString();

  console.log(postBody);//title=44&url=444&text=444

  //4. 把字符串转化为对象

  postBody =  queryString.parse(postBody);

  postBody.id = list.length;

  list.push(postBody);

}

  7.注意,,不要在最后写 res.end(), 因是异步的 因为结束响应,,就不会传了

  ml_readData(function(data){

<!--   

  })

  var  data = null;

 ...

 data = 13 -->

 console.log(data)

 

  })