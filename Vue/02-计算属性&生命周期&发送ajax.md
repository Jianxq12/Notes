# Vue 第2天

## Vue的双向绑定原理(扩展)
1. Object.defineProperty
2. 观察者模式(发布-订阅模式)

```js

//双向绑定原理
 var obj = {};

        // obj.name = ""
        // obj["name"]

        //Object.defineProperty
        Object.defineProperty(obj, "name", {
            // writable: true,
            // configurable: true,
            // enumerable: true,
            // value: "zhangzhichao",
            
            get: function(){
                //get方法会在别人获取当前对象的这个属性值的时候被调用
                //获取到的值，其实就是get方法的返回值
                console.log("你获取了我的属性，我知道了！！！")
                return this._name;
            },
            set: function(value){
                //set方法会在别人为当前对象的这个属性赋值（使用=）的时候被调用
                //value这个形参接收到的内容，就是赋值的时候=右边的内容
                console.log("你给我的属性赋值了，我知道了！！ 我的属性要发生变化了！")
                console.log(value);
                this._name = value;
            }
        })
```

## 计算属性
```js
var vm = new Vue({
    el: "#app",
    data: {
        num1: 0,
        num2: 0
    },
    computed: {
        result(){
            return this.num1 + this.num2;
        },
        result: {
            get(){
                //在获取值的时候会被调用，获取到的值就是这个函数的返回值
                //上面的简写形式的函数其实就是这个get函数
            },
            set(value){
                //当使用计算属性做双向绑定的时候会用到（很少用），计算属性被赋值的时候
                //会调用这个方法，我们可以在这个方法中做一些对应的操作
            }
        }
    }
})
```


## 生命周期钩子函数
1. beforeCreate     在数据初始化之前调用
2. created          在数据初始化完成时调用，这是最早的可以访问到数据的钩子函数
3. beforeMount      在元素被创建并挂载页面之前调用
4. mounted          在元素被创建并挂载页面之后调用
5. beforeUpdate     在有数据变化，更新dom之前调用
6. updated          在有数据变化，更新dom之后调用
7. beforeDestroy    在vue对象被销毁之前调用
8. destroyed        在vue对象被销毁之后调用

####生命周期大致过程
1. 调用 new Vue()创建一个Vue实例
2. 首先初始化生命周期
3. 初始化了数据
    我们自己写的数据是在data里面的，但是最终在页面中使用的数据，其实是vue实例中的数据，
    在初始化数据的时候，vue将data中所有的数据，通过Object.defineProperty方法全部挂载到了Vue实例上，我们才可以在页面中使用这些数据
4. 判断在创建对象的时候，传递进去参数中是否有el参数，如果有，就继续判断是否，有template参数，如果没有
5. 把我们在el中指定的元素，作为模板
6. 使用上一步创建出来的模板，将数据渲染进去，产生真实的DOM元素，这个DOM对象其实就是 vm.$el,  将创建出来的DOM对象，替换回el参数对应的元素所在的位置
7. 此时页面已经展示完毕，进入一个循环阶段
        在这个循环阶段中，Vue会不间断的监视数据的变化，如果有数据发生变化，则创建虚拟DOM，在虚拟DOM应用数据的变换，将虚拟DOM和页面中的DOM树进行对比，找出差异之后，将有差异的部分进行更新
8. 如果有人调用vm.$destroy方法，这个Vue实例将会被销毁
9. 在销毁的过程中，会释放所有的资源，比如：监视器、事件、子组件。。
10. Vue实例被销毁



## ajax
1. vue-resource
```js
this.$http.get(url).then(res=>{}, err=>{})
this.$http.post(url, 数据是一个对象, {emulateJSON: true}).then(res=>{}, err=>{})

```

2. axios
```js

axios.interceptors.request.use(config => {
    //自己的加工逻辑全部可以在这里进行
    //请求会先经过这里，被拦截下来，这个函数执行完毕之后，才会发送出去
    //我们可以在这里对请求中的任意内容进行更改，比如 请求头，比如请求数据

    // config.headers
    // config.data

    return config;
})

axios({
    url: "",
    method: "",
    data: 默认支持字符串，如果要使用对象，则需要自己添加拦截器
}).then(res => {}, err => {

})

```

## 箭头函数
1. 语法
    (形参列表) => { 函数体 }

2. 简写
```js
        //1. 如果只有一个参数
            //可以省略参数的小括号
        // var test = a => {
        //     console.log(a);
        // }

        //2. 如果函数体中只有一个语句
        //     //可以省略大括号
        // var test = a => console.log(a);

        
        //3. 如果函数体中只有一个语句，并且这个语句是个返回语句
        //可以省略大括号和return关键字
        //返回值就是后面的表达式的值
        // var test = (a, b) => a + b;

        // setTimeout(_=>console.log("1"), 1000);

        //注意： 在箭头函数中，没有自己的this，没有自己的arguments

```

## rest参数
```js
var test = (a, ...b) => {
            console.log(b);

            //arguments： 是一个伪数组
                //arguments是将所有的实参存储起来了
            //rest参数： 是一个真数组
                //rest，是将当前rest参数所在位置之后所有的内容接收到了，之前是可以有别的参数的
        }

```