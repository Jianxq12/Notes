# REM

## rem是什么？

`rem`（font size of the root element）是指相对于`根元素`的字体大小的单位。它就是一个相对单位。

`em`（font size of the element）是指相对于当前元素的字体大小的单位。它也是一个相对单位。

它们之间其实很相似，只不过计算的规则一个是依赖根元素，一个是当前元素计算。

```css
html{
  font-size:16px;
}
body {
  font-size:20px;
}
div.em {
  /*em的计算方式参照的当前元素的font-size，如果不设置，默认继承自父盒子*/
  width:2em;
  height:2em;
  background-color:red;
}
/*rem的计算方式参照的是html的font-size*/
div.rem {
  width:2rem;
  height:2rem;
  background-color:blue;
}
```



## 为什么要用rem？

> rem的主要目的就是解决用于不同屏幕的适配问题。rem能够等比例的适配所有的屏幕。

由于市面上手机种类繁多，导致移动端的屏幕种类非常的混乱，比如有常见的`320px 360px 375px 384px 480px 640px`等。在开发中，美工一般只会提供750px或者是640px的设计稿，这就要求我们通过一张设计稿能够适配所有的屏幕。通常解决方案如下：

- 流式布局：虽然可以让各种屏幕都适配，但是显示效果不是非常的友好，因为只有几个尺寸的手机能够完美的显示出来视觉设计师和交互最想要的效果。但是目前使用流式布局的公司非常多，比如 [亚马逊](https://www.amazon.cn/) 、[京东](https://m.jd.com/) 、[携程](https://m.ctrip.com/)
- 响应式布局：响应式这种方式在国内很少有大型企业的复杂性的网站在移动端用这种方法去做，主要原因是***工作大，维护性难*** 。所以一般都是中小型的门户或者博客类站点会采用响应式的方法从PC端页面到移动端页面以及web app直接一步到位，因为这样反而可以节约成本。
- rem布局：rem能够适配所有的屏幕，与less配合使用效果会更好。目前使用rem布局的有：[淘宝](https://m.taobao.com) 、 [苏宁](https://m.suning.com/)



## rem与响应式

因为rem的基准点是根元素html的字体大小，因此我们只需要设置不同屏幕的html的font-size大小不一样就可以达到不同屏幕的适配了。 

### rem与媒体查询

使用rem配合媒体查询可以适配多个终端

```css
@media (min-width: 320px) {
  html {
    font-size: 16px;
  }
}

@media (min-width: 360px) {
  html {
    font-size: 18px;
  }
}
@media (min-width: 384px) {
  html {
    font-size: 19.2px;
  }
}

@media (min-width: 414px) {
  html {
    font-size: 20.7px;
  }
}
```

优点：使用媒体查询适配，速度快。

缺点：适配多个终端时，需要添加响应的代码。

### rem与javascript

通过javascript获取可视区的宽度，计算font-size的值，也可以适配多个终端。

```javascript
function responsive() {
  var uiWidth = 750;//设计图宽度
  var fontSize = 50;//设计图中1rem的大小
  //当前屏幕的大小
  var pageWidth = window.innerWidth;
  if(pageWidth >= 750) {
    pageWidth = 750;
  }
  if(pageWidth <= 320) {
    pageWidth = 320;
  }
  //说白了就是把一个屏幕分成了15rem
  document.documentElement.style.fontSize = (50/750 * pageWidth).toFixed(2) + "px";
}
```

优点：直接适配所有的终端

缺点：必须在页面加载之前设置html的font-size值，不然会出现文字大小调动的情况。



# 苏宁易购

## 适配主流浏览器

```less
//适配主流浏览器
//320 360 375 384 400 414 424 480 540 720 750
//把屏幕分成15rem
.adapter(@screen:320px) {
  @media (min-width: @screen) {
    html {
      font-size: round(@screen/15, 2);
    }
  }
}
```



## zepto简介

> Zepto是一个轻量级的针对现代高级浏览器的JavaScript库， 它与jquery有着类似的api。 如果你会用jquery，那么你也会用zepto。

优点：

- API与jquery基本类似，但不是100%完全覆盖，学习成本低
- 比jquery轻量，只有10k不到。

[zepto中文API](http://www.css88.com/doc/zeptojs_api/)

[别再迷信 zepto 了](http://www.cnblogs.com/batsing/p/zepto.html)



## swiper插件

> Swiper是纯javascript打造的滑动特效插件，面向手机、平板电脑等移动终端。

[swiper中文网](http://www.swiper.com.cn/)

















