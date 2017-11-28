# Vue 第6天

## Webpack的配置和使用
### 安装
1. 全局安装
```
npm install webpack -g
```
2. 本地安装
```
npm install webpack -D
```
### 使用
#### 直接通过命令参数来执行
1. 如果是全局安装的
```
webpack 入口文件 出口文件
```
2. 如果是本地安装的
    2.1 先在package.json的script标签中添加一条命令（就和全局的命令一模一样）
    2.2 通过npm run 命令名 来执行命令

#### 通过配置文件来执行
如果是使用的配置文件，那么在使用webpack的命令的时候，就不需要再传递参数了！

配置文件名为 webpack.config.js, 这个文件一般被放在网站的根目录下

webpack.config.js是一个node.js的模块，所以里面的代码，就是nodejs代码，需要通过nodejs模块化的方式，将配置对象进行导出！

基础配置项
```js
var path = require("path")

module.exports = {
    entry: path.join(__dirname, "入口文件路径"),
    output: {
        path: path.join(__dirname, "dist"),
        filename: "bundel.js"
    }
}

```

### 使用webpack打包别的类型的文件
webpack默认支持的文件类型，只有js文件，如果在项目中出现了其他类型的文件，那么webpack本身是无法进行处理的，所以我们需要为每一个类型的文件，单独指定一个加载器（loader）

#### css
在webpack的项目中，如果要使用css文件，我们可以将css文件当做一个模块，使用import的方式引入到js当中，最终webpack会将css文件中的内容，一起打包到最终的js文件中，也可以在页面中生效

要打包css文件，就需要给css文件配置loader，需要两个loader： style-loader css-loader

1. 安装相应的loader
```
npm install style-loader css-loader -D
```
2. 在webpack.config.js中配置css文件对应的loader
```js
var path = require("path")

module.exports = {
    entry: path.join(__dirname, "入口文件路径"),
    output: {
        path: path.join(__dirname, "dist"),
        filename: "bundel.js"
    },
    //module属性中就可以进行loader的配置
    module: {
        rules: [
            {
                test: /\.css$/,
                //use中放的就是加载器的名称，要注意，调用的顺序是从后向前的
                use: ["style-loader", "css-loader"]
            }
        ]
    }
}
```

#### less sass
less需要的loader:  style-loader css-loader less-loader        依赖项： less
sass需要的loader:  style-loader css-loader sass-loader        依赖项： node-sass

1. 安装相应的loader

    1.1 less-loader:
    ```
        npm install less-loader less -D
    ```
    1.2 sass-loader
    ```
        npm install sass-loader node-sass -D
    ```
2. 配置加载器
```js
var path = require("path")

module.exports = {
    entry: path.join(__dirname, "入口文件路径"),
    output: {
        path: path.join(__dirname, "dist"),
        filename: "bundel.js"
    },
    //module属性中就可以进行loader的配置
    module: {
        rules: [
            {
                test: /\.css$/,
                //use中放的就是加载器的名称，要注意，调用的顺序是从后向前的
                use: ["style-loader", "css-loader"]
            },
            {
                //less文件
                test: /\.less$/,
                use: ["style-loader", "css-loader", "less-loader"]
            },
            {
                //sass文件
                test:  /\.sass$/,
                use: ["style-loader", "css-loader", "sass-loader"]
            }
        ]
    }
}
```

#### 图片 字体 打包
图片需要的loader: url-loader    依赖项： file-loader
字体需要的loader: url-loader

1. 安装
```
npm install url-loader file-loader -D
```
2. 配置
```js
var path = require("path")

module.exports = {
    entry: path.join(__dirname, "入口文件路径"),
    output: {
        path: path.join(__dirname, "dist"),
        filename: "bundel.js"
    },
    //module属性中就可以进行loader的配置
    module: {
        rules: [
            {
                test: /\.css$/,
                //use中放的就是加载器的名称，要注意，调用的顺序是从后向前的
                use: ["style-loader", "css-loader"]
            },
            {
                //less文件
                test: /\.less$/,
                use: ["style-loader", "css-loader", "less-loader"]
            },
            {
                //sass文件
                test:  /\.sass$/,
                use: ["style-loader", "css-loader", "sass-loader"]
            },
            {
                //图片文件
                test:  /\.(jpg|gif|png|jpeg|svg|bmp)$/,
                use: [{
                    loader: "url-loader",
                    options: {
                        //如果图片超过下面这个数据所标注的大小，那么图片将不会被转换成base64的格式，直接会将图片文件扔到dist目录里
                        limit: 1024 * 50
                    }
                }]
            },
            {
                //字体文件
                test: /\.(woff|woff2|eot|ttf)$/,
                use: ["url-loader"]
            }
        ]
    }
}
```

### ES6 代码打包
用到的loader: babel-loader    依赖项： babel-core
用到转码规则： babel-preset-env
1. 安装
```
npm install babel-loader babel-core -D
```
2. 配置
    2.1 创建一个.babelrc 用来指定转码的规则
    ```js
    {
        presets: [
            "env"
        ]
    }
    ```
    2.2 webpack.config.js配置加载器

    ```js
    var path = require("path")

    module.exports = {
        entry: path.join(__dirname, "入口文件路径"),
        output: {
            path: path.join(__dirname, "dist"),
            filename: "bundel.js"
        },
        //module属性中就可以进行loader的配置
        module: {
            rules: [
                {
                    test: /\.css$/,
                    //use中放的就是加载器的名称，要注意，调用的顺序是从后向前的
                    use: ["style-loader", "css-loader"]
                },
                {
                    //less文件
                    test: /\.less$/,
                    use: ["style-loader", "css-loader", "less-loader"]
                },
                {
                    //sass文件
                    test:  /\.sass$/,
                    use: ["style-loader", "css-loader", "sass-loader"]
                },
                {
                    //图片文件
                    test:  /\.(jpg|gif|png|jpeg|svg|bmp)$/,
                    use: [{
                        loader: "url-loader",
                        options: {
                            //如果图片超过下面这个数据所标注的大小，那么图片将不会被转换成base64的格式，直接会将图片文件扔到dist目录里
                            limit: 1024 * 50
                        }
                    }]
                },
                {
                    //字体文件
                    test: /\.(woff|woff2|eot|ttf)$/,
                    use: ["url-loader"]
                },
                {
                    //ES6
                    test: /\.js$/,
                    //排除node_modules中的内容
                    exclude: /node_modules/,
                    use: ["babel-loader"]
                }
            ]
        }
    }
    ```
### html-webpack-plugin的使用
作用： 将src目录下的index.html作为模板，每次在打包的时候，都会将其复制到dist目录下，并且自动为其引入bundle.js

1. 安装
```
npm install html-webpack-plugin -D
```
2. 配置webpack.config.js
```js
var HtmlWebpackPlugin = require("html-webpack-plugin")
var path = require("path")

module.exports = {
    entry: path.join(__dirname, "入口文件路径"),
    output: {
        path: path.join(__dirname, "dist"),
        filename: "bundel.js"
    },
    plugins:[
        new HtmlWebpackPlugin({
            template: path.join(__dirname, "src/index.html")
        })
    ]
}

```

## webpack-dev-server的使用
如果没有这个东西，那么每次在修改代码之后，都需要重新进行打包，才能看到效果！

webpack-dev-server可以启动一个简易的http服务器，当代码发生改变的时候，实时的进行打包并且刷新页面显示效果！ 这个东西只会打包更新了的内容，并不会将整个项目完整的打包，所以效率会比较高，而且它打包的内容都是直接放在内存里面的，所以访问速率也会非常快

1. 安装
```
npm install webpack-dev-server -D
```

2. 使用

    2.1 直接给命令传参
    ```
    webpack-dev-server --port 9000 --contentBase ./src --hot
    ```
    2.2 在webpack.config.js中进行配置
    ```js
    var path = require("path");
    
    //如果使用webpack.config方式进行配置，那么必须有这个包引入
    var webpack = reuqire("webpack");

    module.exports = {
        entry: path.join(__dirname, "入口文件路径"),
        output: {
            path: path.join(__dirname, "dist"),
            filename: "bundel.js"
        },
        devServer: {
            port: 9999,
            contentBase: "./src",
            hot: true
        },
        plugins:[
            //这边是给dev-server设置一个插件，如果没有这个插件则热更新会报错
            new webpack.HotModuleReplacementPlugin()
        ]
    }

    ```

## babel-cli的使用
1. 安装
```
npm install babel-cli -g
npm install babel-preset-env
```
2. 配置
创建.babelrc文件
```
{
    presets: [
        "env"
    ]
}
```
3. 使用
```
babel 要转换的文件的路径 --out-file 输出的文件的路径
```

## 单文件组件
就是将组件相关的内容（HTML JS CSS）全部放到了一个.vue文件当中

```html
<template>
    <div>
        //这里就是组件的html部分也就是所谓的模板
    </div>
</template>

<script>
    export default {
        data(){},
        filters: {},
        components: {},
        watch: {},
        directives: {},
        created(){}
    }
</script>

scoped属性标志着这个style标签中的css样式是只对当前组件有效！
<style scoped>
</style>
```

## Vue-Cli
这是一个可以快速生成一个项目基本结构的vue工具

1. 安装
```
npm install vue-cli -g
```
2. 使用
```
vue init webpack 项目名称
```

## 在项目模板中写代码
1. 创建组件！
2. 配置路由！

项目模板的获取两种方式：
1. 自己写的模板
    https://github.com/avakpan/vue-template
    
2. vue-cli生成模板