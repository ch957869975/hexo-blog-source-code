---
title: ES6其他特性
date: 2019-02-06 17:48:01
tags:
  - JavaScript
  - ES6
---

## 1.模板字符串

<!--more-->

```js
var address = "南京"
//es5
console.log("小明在" + address + "工作")
// es6
console.log(`小明在${address}工作`)
```

关于模板字符串还有许多其他的技巧

## 2.解构赋值

```js
const people = {
  name: "lux",
  age: 20
}
const name = people.name
const age = people.age
console.log(name + " --- " + age)
// 在es6之前我们是用上面这样的方法来取值的，一点毛病没有，es6之后我们是来使用解构的形式来取值的，像下面这样
//对象
const people = {
  name: "lux",
  age: 20
}
const { name, age } = people
console.log(`${name} --- ${age}`)
//数组
const color = ["red", "blue"]
const [first, second] = color
console.log(first) //'red'
console.log(second) //'blue'
```
