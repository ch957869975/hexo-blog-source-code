---
title: 虚拟 dom 实现（今日头条面试题）
date: 2019-02-06 17:49:13
tags:
  - JavaScript
  - 面试题
categories:
  - JavaScript
---

给出如下虚拟 dom 的数据结构，如何实现简单的虚拟 dom，渲染到目标 dom 节点数

```json
{
  "tagName": "ul",

  "props": { "class": "list" },

  "children": [{ "tagName": "li", "children": ["douyin"] }, { "tagName": "li", "children": ["toutiao"] }]
}
```

构建一个 render 函数，将 demoNode 对象渲染为以下 dom

```html
<ul class="list">
  <li>douyin</li>
  <li>toutiao</li>
</ul>
```

这里实际上有两个关键点：

1.通过 JavaScript 来构建虚拟的 DOM 树结构，并将其呈现到页面中；

2.当数据改变，引起 DOM 树结构发生改变，从而生成一颗新的虚拟 DOM 树，将其与之前的 DOM 对比，将变化部分应用到真实的 DOM 树中，即页面中。

## 构建虚拟 dom

构造函数：

```js
/*
 * 构建dom树
 * @Params:
 * tagName(string)(requeired)
 *  props(object)(optional)
 * children(array)(optional)
 * */

function Element({ tagName, props, children }) {
  if (!(this instanceof Element)) {
    return new Element({ tagName, props, children })
  }
  this.tagName = tagName
  this.props = props || {}
  this.children = children || []
}
```

而我们需要像这样去调用它：

```js
var elem = Element({
  tagName: 'ul',
  props: { class: 'list' },
  children: [Element({ tagName: 'li', children: ['douyin'] }), Element({ tagName: 'li', children: ['toutiao'] })]
})
```

然后需要考虑的是，如何把节点插入到真实 dom 中，需要实现一个 render 函数，优先考虑`深度优先遍历（DFS）`

```js
Element.prototype.render = function() {
  var el = document.createElement(this.tagName),
    props = this.props,
    propName,
    propValue
  for (propName in props) {
    propValue = props[propName]
    el.setAttribute(propName, propValue)
  }
  this.children.forEach(function(child) {
    var childEl = null
    if (child instanceof Element) {
      childEl = child.render()
    } else {
      childEl = document.createTextNode(child)
    }
    el.appendChild(childEl)
  })
  return el
}
```

假设我们将这个 dom 结构插入到 body 中

```js
var elem = Element({
  tagName: 'ul',
  props: { class: 'list' },
  children: [Element({ tagName: 'li', children: ['douyin'] }), Element({ tagName: 'li', children: ['toutiao'] })]
})
document.querySelector('body').appendChild(elem.render())
```

晚安, 🌛
