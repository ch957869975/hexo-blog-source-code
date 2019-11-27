---
title: 斐波那契数列
date: 2019-02-06 17:48:27
tags:
  - JavaScript
  - 面试题
---

ps：斐波那契数列数列，[1,2,3,5,8,13,21...],从第三项开始，当前项是前两项的和的一组数

今天在微信群看到了这个面试题，求 第 100 个的斐波那契数

## 常规

常见的写法是这样的

```js
function fibonacci1(n) {
  if (n === 1 || n === 2) return n
  return fibonacci1(n - 1) + fibonacci1(n - 2)
}
// fibonacci1(50) = 20365011074
// time = 140550.01586914062ms
```

<!--more-->

ok，代码没问题，打印一下吧，`console.log(fibonacci(100))`,发现打印不出来，计算太庞大了，简单 3 行的递归 function，竟然导致了浏览器崩溃，难以置信，但事实如此。

也别第 100 个了，就第 50 个，计算需要 140 多秒，别问我怎么知道的，`console.time` 和`console.timeEnd`了解一下。

## 优化

考虑一下怎么优化，用时长应该是需要递归调用所以才比较耗费时间，但我们好像只需要`fibonacci(n - 1)`与`fibonacci(n - 2)`两个值，把它作为变量存储起来，可以大大减少内存开销。

```js
function fibonacci2(n) {
  let current = 1
  let next = 1
  let temp
  for (let i = 0; i < n; i++) {
    temp = current
    current = next
    next += temp
  }
  return current
}
//fibonacci2(50) = 20365011074
// time = 4.26513671875ms
```

emmmm,用时 4.几 ms，ms！！！不是 s！！提升万倍不是梦

## ES6 考虑一下？

递归累加，高阶函数 reduce 干这个活不是最合适的吗,reduce 接收 2 个参数，一个是为累加器，另外一个为累加器的初始值
，下面是 js 代码,这里 p 保存 F(n-1)值，而 seed 则保存 F(n-2)的值

```js
function fibonacci3(n) {
  let seed = 0
  return [...Array(n)].reduce(p => {
    const temp = p + seed
    seed = p
    return temp
  }, 1)
}
//fibonacci3(50) = 20365011074
// time = 4.301025390625ms
```

## 数学公式

百度到的，根据斐波那契公式改写，感觉很神奇，数学学渣表示看不懂

```js
function fibonacci4(n) {
  const SQRT_FIVE = Math.sqrt(5)
  return Math.round(
    (1 / SQRT_FIVE) *
      (Math.pow(0.5 + SQRT_FIVE / 2, n + 1) -
        Math.pow(0.5 - SQRT_FIVE / 2, n + 1))
  )
}
//fibonacci4(50) = 20365011074
// time = 2.782958984375ms
```

目前数学公式是性能最好的一个

emmmmm,为什么我查到的资料，斐波那契数列都是从 0 开始的，我是举了一个假的 🌰 吗 ？？ ，代码就这样了，道理是相同的，如果从 0 开始，改下限制条件就是了。

不开心，:smiling_imp:

晚安, :last_quarter_moon_with_face:

## 一个从 0 开始的斐波那契数列

```js
function getFibonacci(n) {
  var fibarr = []
  var i = 0
  while (i < n) {
    if (i <= 1) {
      fibarr.push(i)
    } else {
      fibarr.push(fibarr[i - 1] + fibarr[i - 2])
    }
    i++
  }
  return fibarr
}
// n 为数列长度
```

## TODO

- [ ] 通过 generator 实现
- [ ] 通过尾调用实现
