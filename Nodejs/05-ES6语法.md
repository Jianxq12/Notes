### ES6 常见用法 

### 一、let 和 var 的用法 

> let 是 ES6 出来的

1. 作用域

   - 用 var 定义的变量,只有两个作用域 全局作用域 和 函数作用域
   - let 不仅在有两个作用域,还多了一个块作用域 

2. 变量提升问题:

   - var 能让变量提升  

   - let 不能让变量提升

     ```js
     console.log(a)
     var a = 3;

     var a;
     log(a)
     a = 3;
     ```

     ​

3. 不能重复命名

   - var 可以重复命名
   - let 不可以

4. 总结:

   ```js
   只要语法支持: 都使用 let 来代替 var
   ```

   ​

###二、const

const 声明一个只读的常量。一旦声明，常量的值就不能改变。

```js
//1. 变量问题
const num = 14;
num = 15;
console.log(num)

const num;
num = 25;
console.log(num)  // 常量必须初始化

//2. 对象问题
const obj = {
  name:' 鸭王'
}

obj.name = '老宫'
console.log(obj)  // 老宫
```

总结:

```js
1.变量问题, 定义的时候就要给初始化值了
2.对象问题,,为什么不报错,,因为 obj 的值是个什么?是地址,, 又因为这里的 obj 的值没有变,这是对象内的属性变了而已;  [画图] 
3. 常用: 常用字符串 + 大小写
const DB_PATH = 'mongogb:127.0.0.1:27017/data';
```



### 三、模板字符串

1. 替换模板 :  ``

   静态字符串 :  ''

   动态字符串 : ` ${name } `  

   // 不推荐   'sdfsdf' + name

2. 引用变量: `${ name }`

3. 代码:

   ```js
   // html
    <div>
       <h3>点击切换用户信息</h3>
       <div id="box">
         <div>
           姓名:<span>鸭王</span>
         </div>
         <div>
           年龄:<span>14</span>
         </div>
       </div>

       <button id="btn">走你</button>
     </div>

   //以前的做法:
   1. art-template
   2. ''+obj.name+''

   // js
   var btn = document.querySelector('#btn');
       btn.onclick = function () {
         // 模拟请求的数据
         var  obj = {
           name:'老宫',
           age:12
         }
         // 模板字符串
         var tplStr = `<div>
           姓名:<span>${obj.name}</span>
         </div>
         <div>
           年龄:<span>${obj.age}</span>
         </div>`
         // 赋值回去
         var box = document.querySelector('#box');
         console.log(box,tplStr);
         box.innerHTML = tplStr;
       }
       
       ---------------
     // 聪版: '姓名: <span>' + obj.name + '</span>' +
     // art-template 版: npm 安装: 单独一个引入: 
         <script src='./node_modules/art-template/lib/template-web.js'></script>
     // 模板:
      <script id="tplBox" type="text/html">
         <div>
           姓名: <span> <%= list.name %> </span>
         </div>
         <div>
           年龄: <span> <%= list.age %></span>
         </div>
       </script>
   ```

   ​

###四、函数问题

1. 默认参数

   ```js
   function add(x=1,y=10) {
     console.log(x+y);
   }
   add();  (data || 1)   
   add && add()
   ```

2. 解构赋值

   ```js
   function add( { name,age } ) {
     console.log(name,age);
   }

   var obj = {
     name:'鸭王',
     age:18,
     love:' 妖'
   }
   add(obj);
   ```

3. 箭头函数

```js
//1. 
function () {
  
}

// function 删掉 , ()后面加个 =>
()=>{
   
}

//2. 
function add() {
  console.log('呵呵')
}
add =()=> {
  console.log('呵呵')
} 

// add();


//3. settimeout
setTimeout(function() {
  console.log(22)
}, 2000);

setTimeout(()=> {
  console.log(22)
}, 2000);

//4. map
var arr = [1,2,3,4];

arr = arr.map(function (item) {
  return item + 10; 
})

arr = arr.map( (item)=> {
  return item + 10; 
})
// 注意
// 如果参数只有一个,可以省略小括号 
// 如果{}代码只有1行,可以省略 {} 并且默认返回 return 
arr = arr.map(item=> item + 10 )

console.log(arr);

```



### 五、对象

1. key=value 省略一个

   ```js
   //1. 省略相同的
    function add(obj) {
      console.log(obj)
    }

   // 传值
   // 传一个名字
   var name = '老宫'
    add({name})
   ```

   ​

2. 继承

   ``` js
   //1 以前

   // 定义类
    function Father(name) {
      this.money = '1000000'
    }

   //继承???
   function Son(money) {
     Father.call(this);
   }


    // 实例化
    var f = new Father()
    console.log(f )
    var s = new Son();
    console.log(s);

   //2.  class 继承
   class Father {
     constructor(){
       this.money = 100000;
     }
   }

   //继承
   class Son extends Father {

     constructor( name){
       super(name);
       
       this.love = '妖妖'
     }
   }

   var f = new Father();
   console.log(f)

   var s = new Son('老宫');
   console.log(s);
   ```

   ​



