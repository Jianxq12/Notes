# JDM

## 京东快报轮播图

> 实现京东快报每2秒钟滚动一次，要求无缝

思路：

	//1. 使用定时器让ul进行滚动   
		//1.1 设置下标index++   1.2 设置过渡  1.3 使用translateY进行滚动
	//2. 给ul注册过渡结束事件，当ul需要瞬间变回来。
过渡结束事件：

`transitionend`事件在 CSS 完成过渡后触发。

```javascript
ul.addEventListener("transitionend", function () {
  //过渡结束时，会触发transitionend事件
})
```

## 京东轮播图

思路：

```javascript
//1. 页面结构：首尾都需要放一张假图片
//2. 初始化index=1
//3. 设置定时器让ul移动， 设置下标index++  设置过渡  使用translateX进行滚动
//4. 给ul注册过渡结束事件，当ul需要瞬间变回来。 让点跟着移动
//5. 方法封装
```



## touch事件

移动端新增了4个与手指触摸相关的事件。

```javascript
//touchstart:手指放到屏幕上时触发
//touchmove:手指在屏幕上滑动式触发（会触发多次）
//touchend:手指离开屏幕时触发
//touchcancel:系统取消touch事件的时候触发,比如电话
```

每个触摸事件被触发后，会生成一个event对象，event对象中`changedTouches`会记录手指滑动的信息。

```javascript
e.touches;//当前屏幕上的手指
e.targetTouches;//当前dom元素上的手指。
e.changedTouches;//触摸时发生改变的手指。(重点)
```

这些列表里的每次触摸由touch对象组成，touch对象里包含着触摸信息，主要属性如下

```javascript
clientX / clientY: //触摸点相对浏览器窗口的位置
pageX / pageY:     //触摸点相对于页面的位置
```

## 通过touch滑动轮播图

```javascript
//1. 给ul注册touch相关的三个事件（注意清除浮动，不然触发不到touchmove事件）
//2. 在touchstart中
	//1. 记录开始的位置
	//2. 清除定时器
//2. 在touchmove中
	//1. 记录移动的距离
	//2. 清除过渡
	//3. 让ul跟着移动
//3. 在touchend中
	//1. 记录移动的距离
	//2. 判断移动距离是否超过1/3屏，判断上一屏还是下一屏，或者是吸附
	//3. 添加过渡
	//4. 执行动画
	//5. 开启定时器

//4. 优化：快速滑动的实现逻辑

//5. 优化：重置大小时轮播图错位。
```





# zepto框架介绍（了解）

> **Zepto**是一个轻量级的**针对现代高级浏览器的JavaScript库， **它与jquery**有着类似的api**。 如果你会用jquery，那么你也会用zepto。

[github地址](https://github.com/madrobby/zepto)

[中文文档](http://www.css88.com/doc/zeptojs_api/)



## zepto与jquery的区别

+ jquery针对pc端，主要用于解决浏览器兼容性问题，zepto主要针对移动端。
+ zepto比jquery轻量，文件体积更小
+ zepto封装了一些移动端的手势事件



## zepto的基本使用

zepto的使用与jquery基本一致，zepto是分模块的，需要某个功能，就需要引入某个zepto的文件。

```javascript
<script src="zepto/zepto.js"></script>
<script src="zepto/event.js"></script>
<script src="zepto/fx.js"></script>
<script>

  $(function () {
    $(".box").addClass("demo");

    $("button").on("click", function () {
      $(".box").animate({width:500}, 1000);
    });
  });


</script>
```



## zepto的定制

安装Nodejs环境

1、下载zepto.js

2、解压缩

3、cmd命令行进入解压缩后的目录

4、执行`npm install`命令

5、编辑make文件的`41行`,添加自定义模块并保存，

7、然后执行命令 `npm run-script dist`

8、查看目录dist即构建好的zepto.js



## zepto手势事件

zepto中根据`touchstart touchmove touchend`封装了一些常用的手势事件

```javascript
tap   // 轻触事件,用于替代移动端的click事件，因为click事件在老版本中会有300ms的延迟
swipe //手指滑动时触发
swipeLeft  //左滑
swipeRight  //右滑
swipeUp    //上滑
swipeDown   //下滑
```

关于tap事件与click事件

1. click事件在pc端非常用，但是在移动端会有300ms左右的延迟，比较影响用户的体验，300ms用于判断双击还是长按事件，只有当没有后续的的动作发生时，才会触发click事件
2. tap事件只要轻触了，就会触发，体验更好。

【使用zepto实现京东轮播图】

# 分类页

1. 京东分类页是一个全屏的页面 ，没有滚动条，但是可以做到区域滚动，即子元素可以在父元素中进行滚动。
2. 左侧和右侧都很长，在滑动过程中还可以有反弹的效果。

**左侧滑动实现思路**

```javascript
//1. 给ul注册touch的三个事件
//2. 在touchmove中让ul进行滑动，
//3. 使用currentY变量记录滑动的距离，每次滑动时需要加上currentY的值，滑动结束，需要更新currentY
//4. 限制滑动距离：  
var max = 50;
var min = nav.offsetHeight - ul.offsetHeight - 50;
//5. 吸附，在滑动结束时判断，如果超出反弹范围，吸附回来。
```



**iscroll插件的使用**

[git下载地址](https://github.com/cubiq/iscroll)

[iscroll参考文档](http://www.mamicode.com/info-detail-331827.html)

**注意**

1. 使用iscroll时，一定要满足父盒子嵌套了子盒子
2. 子盒子大小一定要超过父盒子的大小

```javascript
new IScroll('.jd_content', {
    scrollX:false,//横向滚动
    scrollY:true//纵向滚动
  });
```



