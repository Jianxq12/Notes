# Vue 第1天

## MVC MVP MVVM MVW MV*
1. MVC
    Model View Controller 
    Model： 负责提供数据
    View:   负责数据展示与交互
    Controller： 负责接收到View中用户交互的结果之后，经过特定的处理之后转交给Model
    
2. MVP
    Model View Presenter
    Model： 负责提供数据
    View:   负责数据展示与交互
    Presenter: 负责View和Model之间的数据传输 （桥梁） 这里的代码都是自己实现的

3. MVVM
    Model View Presenter
    Model： 负责提供数据
    View:   负责数据展示与交互
    View-Model: 负责View和Model之间的数据传输 （桥梁） 这里的代码都是框架实现的！

MVW: Model View Whatever
MV*

## Vue的基本使用
1. 安装vue
    1.1 下载vue.js文件
    1.2 通过cdn引用
    1.3 npm直接安装vue包

2. 使用
    2.1 引入vue.js文件
    2.2 为vm对象创建视图部分  html
    2.3 创建Vue实例 `var vm = new Vue()`
    2.4 在创建Vue实例的时候，需要通过el将vm对象和视图关联起来
    2.5 可以在视图中书写指令 引用vm对象中的数据（data）

    注意: 在视图中使用的数据，必须现在data中声明好，如果data中没有，却在视图中引用，会报错！

## Vue的指令
1. 插值表达式： {{数据名}}
2. 双向数据绑定的指令： v-model
3. 属性绑定的指令:   v-bind:属性名="表达式、数据名"  简写 :属性名="表达式、数据名" 
4. 事件绑定： v-on:事件类型="函数名、表达式"  如果是函数名，则去methods中找对应的函数  简写 @事件类型="函数名、表达式"  
5. v-text v-html: 可以给元素设置内容，v-text会修改innerText v-html就相当于是innerHTML，  v-text不支持渲染html代码，而v-html支持
6. v-cloak: 这个可以和css样式配合，解决闪烁问题
7. v-for: 可以用来遍历数组、对象、字符串、数字，生成对应的元素
    ```html
        <div v-for="item in items">
        <div v-for="(item, index) in items">
        <div v-for="(item, key,index) in items">
        <div v-for="item in 10">
    ```
8. v-if v-show 都可以用来控制元素的显示和隐藏，但是原理不一样， v-show只是简单的通过css样式display来控制显示和隐藏，而v-if会直接控制元素存在与否，如果条件是false，v-show会加上display:none， 而v-if会直接将元素从页面中移除， v-if在频繁的切换下会比较耗性能


## 事件修饰符
@click.修饰符.修饰符="表达式"

.stop 阻止冒泡
.prevent 阻止默认行为
.self 只有自己能触发
.once 只触发一次
.capture 在捕获阶段触发

## 品牌管理案例
## 隔行变色案例