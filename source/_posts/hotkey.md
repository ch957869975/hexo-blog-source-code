---
title: hotkey.js 的使用
date: 2019-02-18 14:38:23
tags:
  - JavaScript
categories:
  - JavaScript
---

# hotkey.js 的使用

在一个小项目中遇到了快捷键的小需求，所以选择了 `hotkey.js`。目前该项目的 star 数 3277。

## 安装

```
npm install hotkeys-js --save
```

**注意**我们是要在线上仍然使用这个 js 库，所以并不是安装到开发依赖中。

---

## 用法

```js
import hotkeys from 'hotkeys-js'

hotkeys('f5', function(event, handler) {
  // Prevent the default refresh event under WINDOWS system
  event.preventDefault()
  alert('you pressed F5!')
})
hotkeys('ctrl+a,ctrl+b,r,f', function(event, handler) {
  switch (handler.key) {
    case 'ctrl+a':
      alert('you pressed ctrl+a!')
      break
    case 'ctrl+b':
      alert('you pressed ctrl+b!')
      break
    case 'r':
      alert('you pressed r!')
      break
    case 'f':
      alert('you pressed f!')
      break
  }
})
```

我的需求是多组跨快捷键的定义，所以写法我采用的是第二种。

## 注意

`hotkey`默认不监听 `input` `select` `textarea`，即元素获取焦点时不触发定义的快捷键。
要想达到监听的需求，需要写一句这样的代码
`hotkeys.filter = () => true`，将所有情况全部返回 true

## 总结

用这个库我成功的为自己的 markdown 在线编辑器添加了快捷键功能。[编辑器地址](https://ch957869975.github.io/md-editor/dist/)
