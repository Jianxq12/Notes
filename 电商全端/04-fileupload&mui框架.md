# 文件上传步骤

1. 引包

```javascript
<script src="lib/jquery/jquery.js"></script>
<script src="lib/jquery-fileupload/jquery.ui.widget.js"></script>
<script src="lib/jquery-fileupload/jquery.fileupload.js"></script>
```



2. 保证项目public下有两个文件夹  `upload/brand`和`upload/product`



3. 书写html标签

```html
<!--
  id:目的是为了获取
  type:file  只有type:file才能选文件
  name: pic1  后端通过这个name属性来获取这个文件
  data-url: 写后端给的接口

  还需要去js中初始化文件上传
-->
<input id="fileupload" type="file" name="pic1" class="btn btn-default" data-url="/category/addSecondCategoryPic">
```

4. 写js初始化图片上传

```javascript
$("#fileupload").fileupload({
  dataType:"json",
  //每次图片上传完成都会执行这个回调函数
  done:function(e, data){
    //data.result存储了图片的地址。
  }
});
```



# 乐淘移动端

## mui基本介绍

> bootstrap:响应式前端ui框架，既可以适配电脑终端，也可以是适配移动端。
>
> mui:最接近原生APP体验的高性能前端框架 ,mui是一个移动端的ui框架，只针对移动端

+ mui官网：http://dev.dcloud.net.cn/mui/
+ mui文档：http://dev.dcloud.net.cn/mui/ui/
+ 组件列表：http://dcloud.io/hellomui/



## 项目搭建



# 主页开发

## 主页布局

上下固定布局，中间相对定位，使用padding留出剩余空间



## 顶部与底部通栏



## 主体区区域滚动



## 轮播图组件



## 导航栏与商品区块





# 分类面开发

## 静态页面开发

## 左侧与右侧滚动

## 左侧渲染

## 右侧渲染第一页

## 点击时渲染

## 无数据时处理

