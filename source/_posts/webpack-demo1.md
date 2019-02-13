---
title: 【webpack学习之路】1. 初步配置webpack
date: 2019-02-06 12:32:13
tags:
  - webpack
categories:
  - webpack
---

# 开始前的准备

## init

创建一个空项目`webpack-demo`,并创建 `.gitingnore`

```
mkdir webpack-demo
touch .gitingnore

```

后续会在`webpack-demo`中创建多个 demo，每个 demo 单独成为一个小项目，所以`.gitignore`得把 demo 下的`node_moudules`忽略掉

```ignore
.DS_Store
**/node_modules
.idea
.vscode
```

# 安装

## 全局安装

```
npm i webpack -g
```

## 局部安装

```
npm i webpack
```

# 对 webpack 进行初步配置

进入到我们上一步创建的空项目中，创建一个 demo01 的子项目

```
mkdir demo01
cd demo01
npm init -y
npm i webpack-cli webpack -D
```

## 配置目录结构

当然，使用`yarn`作为包管理工具也是可以的。项目结构如下

```
├── dist
│   ├── bundle.js
│   └── index.html
├── package-lock.json
├── package.json
├── src
│   └── index.js
└── webpack.config.js
```

ps: `package-lock.json` 不用过多关注

### index.html & index.js

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Document</title>
  </head>
  <body>
    <script src="bundle.js"></script>
  </body>
</html>
```

对应的 js 就是

```javascript
const render = (tagname = 'div') => {
  const elem = document.createElement(tagname)
  elem.innerHTML = 'Hello Webapck !'
  return elem
}

document.body.appendChild(render())
```

暂时还没配置`babel`,请在谷歌浏览器预览

### 配置 webpack

webpack.config.js

```js
const path = require('path') //node自带的path模块
module.exports = {
  entry: './src/index.js', //入口文件配置
  output: {
    //出口文件配置
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist')
  }
}
```

## 使用 npm 脚本

```json

"scripts": {
  // ...
  "build": "webpack" // 在package.json中添加一行命令
}
```

在控制台中就可以执行`npm run build`命令了,然后打开页面可以看到页面正确渲染了。

感谢阅读，[源码在此](https://github.com/ch957869975/webpack-demo/tree/master/demo01)
