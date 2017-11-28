# webpack-dev-server

## 安装
1. 全局安装
```
npm install webpack-dev-server -g
```
2. 本地安装（推荐）
```
npm install webpack-dev-server -D
```

## 使用
1. 全局
```
webpack-dev-server --port 9999 --contentBase ./src --hot
```
2. 本地
```
scripts: {
    "dev": "webpack-dev-server --port 9999 --contentBase ./src --hot"
}

npm run dev
```

## 参数：
1. port 运行服务器的端口
2. contentBase 访问index.html的目录
3. hot 启用热更新