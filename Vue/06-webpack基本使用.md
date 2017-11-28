# webpack的使用

## 全局安装
```bash
npm install webpack -g
```

使用方式：
```bash
webpack 入口文件 输出文件
```

## 本地安装(官方推荐)
```bash
npm install webpack -D
```
本地安装之后，并不能直接在bash、cmd中执行webpack命令

如果要进行webpack打包操作 ，那么需要将webpack命令写在scripts对象中
通过npm run 命令名 来执行webpack!

## package.json文件中的scripts的内容介绍

scripts中所放的内容，其实就是一个个的bash命令，他们又特定的名字
可以使用名字将这个bash命令执行

```
npm run 命令名字
```


## webpack命令的使用
1. 直接传参执行
```bash
webpack 入口文件 出口文件
```

2. 不传参数，使用配置文件
```bash
webpack
```
当直接执行webpack命令的时候，webapck会在当前文件夹下搜索配置文件，配置文件的名字为（webpack.config.js）