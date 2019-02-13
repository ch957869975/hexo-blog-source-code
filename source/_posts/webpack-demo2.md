---
title: 【webpack学习之路】2. 配置加载css/图片/字体
date: 2019-02-06 14:14:55
tags:
  - webpack
categories:
  - webpack
---

# 加载 css，图片，字体 简单配置

上一步对 webpack 入口和出口进行了简单配置，并成功引用。上一步的[配置参考](https://ch957869975.github.io/hexo-blog/2019/02/06/webpack-demo1/#more)

## 配置加载 css

在 webpack 中，所有的文件均被视为模块。特定类型的模块需要特定的解释器来解释，这种解释机制被称为`loader`。对 css 而言，需要用到`style-loader`与`css-loader`。先安装这两个 loader

```
npm i css-loader style-loader -D
```

### 配置 loader

在`webpack.config.js`中配置 `loader`

```js
const path = require('path')

module.exports = {
  entry: './src/index.js',
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist')
  },
  module: {
    rules: [
      {
        test: /\.css$/, //使用正则表达式匹配某种类型的文件，这里配置的是后缀为css的文件
        use: ['style-loader', 'css-loader'] //loader名称
      }
    ]
  }
}
```

配好之后，就可以在 js 中通过 import 导入你想加载的 css 文件了，在`src`下新建`style.css`

```css
.hello {
  color: red;
  font-size: 20px;
}
```

`index.html`与上一文章相同

```js
import './style.css'
const render = (tagname = 'div') => {
  const elem = document.createElement(tagname)
  elem.innerHTML = 'Hello Webapck demo02!'
  return elem
}
document.body.appendChild(render())
```

执行 `npm run build`后，打开`dist/index.html`,可以看到
![](https://ws2.sinaimg.cn/large/006tNc79ly1fzwr6qku10j30gi04cdfp.jpg)

得知，样式得到正常加载

## 配置加载图片

加载图片需要用到`file-loader`,先安装之。

```
npm i file-loader -D
```

配置到`webpack.config.js`中

```js
rules: [
  // ......
  {
    test: /\.(png|svg|jpg|gif)$/,
    use: ['file-loader']
  }
]
```

然后将`heart.png`放进`src`目录，修改`src/index.js`

```js
// ...
import picture from './heart.png'

const render = (tagname = 'div') => {
  const elem = document.createElement(tagname)
  elem.innerHTML = 'Hello Webapck demo02!'
  elem.classList.add('hello')
  const pic = new Image()
  pic.src = picture
  element.appendChild(pic)
  return elem
}
document.body.appendChild(render())
```

执行`npm run build`之后打开页面，效果如图

![](https://ws4.sinaimg.cn/large/006tNc79ly1fzwrrjy0n8j30r20e6749.jpg)

图片被正常加载进去了，那好，我们现在把 js 中的图片删去，以背景图的方式添加尝试一下。修改`style.css`

```css
.hello {
  color: red;
  width: 300px;
  height: 300px;
  margin: 30px;
  font-size: 20px;
  background-image: url('./heart.png');
  background-size: 100%;
  background-repeat: no-repeat;
}
```

重新打包并打开页面

![](https://ws2.sinaimg.cn/large/006tNc79ly1fzws1zecqlj30gy0daq2z.jpg)

样式得以正常加载

## 配置字体文件

上面已经添加了`file-loader`,这里直接配置字体文件的加载

```js
rules: [
  // ...
  {
    test: /\.(png|svg|jpg|gif|woff|woff2|eot|ttf|otf|TTF)$/,
    use: ['file-loader']
  }
]
```

添加自定义样式

```css
@font-face {
  font-family: 'MyFont';
  src: url('./font.TTF') format('truetype');
  font-weight: bold;
  font-style: normal;
}
.hello {
  /*上部分省略*/
  font-family: MyFont;
}
```

重新打包，页面打开，效果如下

![](https://ws1.sinaimg.cn/large/006tNc79ly1fzwsje9evbj30ew0dwmx6.jpg)

字体得以正常加载

本节[源码在此](https://github.com/ch957869975/webpack-demo/tree/master/demo02)
