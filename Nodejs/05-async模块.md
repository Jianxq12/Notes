### async

一个解决回调地狱的模块



#### 以前: 回调地狱

```js
//1. 获取 cities 数据
db.find(CITIES,function (err,data_cities) {
  // console.log(data_cities)

  //2. 获取 majors 数据
  db.find(MAJORS,function (err,data_majors) {
    // console.log(data_majors)  

    //3. 根据 _id ,再获取个人信息
    var _id = db.ml_ObjectId(req.query._id);

    db.findOne(STUDENTS,_id,function (err,doc) {

      //4. 才开始渲染
      res.render('edit',{item:doc,cities:data_cities,majors:data_majors})

    })
  })
})
```





###async -parallel   异步并行  -  使用:

1. 安装: `npm i async -S`

2. 加载 : `var async = require('async')`

3. 使用: 

   ```
   async.parallel({tasks}, callback);
   ```

4. 一定要在每个 callback 里,返回错误  err,



### DEMO

1. 一定要有返回错误
2. 返回的值都到 result 里

````js
//准备: a.txt,     b.txt,      c.txt
async.parallel({
  one:function (callback) {
    fs.readFile('./a.txt','utf8',function (err,data) {
      console.log('aa')
      callback(err,'aa');

    })
  },
  two:function (callback) {
    fs.readFile('./b.txt','utf8',function (err,data) {
 
      console.log('bbb')
      callback(err,'bb');

    })
  },
  three :function (callback) {
    fs.readFile('./c.txt','utf8',function (err,data) {
      console.log('cc')
      callback(err,'cc');

    })
  }

},function (err,results) {

  console.log('有结果了')
  console.log(results);
  
})

````



#### SMS 里的 显示 edit 编辑页面修改:

```js

  async.parallel({
    // 1. 读取城市
    cities: function (callback) {
      db.findAll('cities', function (err,data_cities) {
        if (err) {
          throw err;
        }
        callback(err,data_cities);
      })
    },
    //2. 读取专业数据
    majors:function (callback) {
      db.findAll('majors', function (err,data_majors) {

        if (err) {
          throw err;
        }
     
        callback(err,data_majors);
    
      })
    },
    //3. 读取学生数据
    stu:function (callback) {
       //3.1 获取 id
       var _id = mongodb.ObjectId(req.query._id);
       db.findOne(STUDENTS, _id, function (err,doc) {

        if (err) {
          throw err;
        }
     
        callback(err,doc);
      })
    }

  },function (err,results) {
    //4. 回调都完成了,,,,开始渲染
    console.log(results);
     //渲染
    res.render('edit',{ cities: results.cities ,  majors:results.majors , item:results.stu })
  })
  
```

