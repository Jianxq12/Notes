# babel-cli的使用

## 安装
1. 全局安装
```
npm install babel-cli -g
```
2. 本地安装
```
npm install babel-cli -D
```

## 配置
因为babel转码需要转码规则，所以需要先先下载对应的转码规则
babel-preset-env

然后在当前文件夹下创建一个.babelrc文件
```js
{
    presets: [
        "env"
    ]
}
```

## 使用
1. 全局安装
```
babel 要被转码的文件路径 --out-file 输出的文件路径
```

2. 本地安装的
```package.json里面配置scripts
scripts:{
    "babel": "babel 要被转码的文件路径 --out-file 输出的文件路径"
}

npm run babel
```


## babel的插件
babel通过转码规则，本身只能将代码中的ES6新语法进行转换，转成ES5的内容
但是ES6中为对象等等添加的新的方法，属性，babel是不会处理的！

要处理这些内容，需要用到babel的插件
1. babel-polyfill
2. babel-plugin-transform-runtime