---
title: 我们为什么需要async/await?
date: 2019-03-26 9:00:26
tags:
  - JavaScript
categories:
  - JavaScript
---

![](https://user-gold-cdn.xitu.io/2019/3/25/169b4d7c57ad2ba6?w=1270&h=642&f=png&s=61453)
内有多只猫咪 ，慎入。

## async 是什么 & async 的基本用法

> `async function` 声明用于定义一个返回 `AsyncFunction` 对象的异步函数。异步函数是指通过事件循环异步执行的函数，它会通过一个隐式的 `Promise` 返回其结果。但是如果你的代码使用了异步函数，它的语法和结构会更像是标准的同步函数。
> 引用自 MDN。

js 的方法和语法糖多数都是语义化的，从字面意思上来说，async 代表`异步的`，用来表示一个异步的函数，返回一个`promise`,可以使用 then 方法添加回调。
可以看下这个例子：

```js
const foo = async () => {
  return { name: '芬达' }
}
console.log(foo())
```

![](https://user-gold-cdn.xitu.io/2019/3/25/169b5007faef78cc?w=361&h=98&f=png&s=11660)
可以看到返回的是 promise 哦。这样不就可以愉快的写 then 式回调了嘛。~~偷笑！~~
简单来说，只要使用了`async`, 就会返回一个`promise`。

![](https://user-gold-cdn.xitu.io/2019/3/25/169b506cf80d02fd?w=700&h=700&f=jpeg&s=41556)
~~猫咪分割线/猫咪分割线/猫咪分割线/猫咪分割线/猫咪分割线/猫咪分割线/~~

## await 与 async 搭配的基本用法

### await 在等谁

await 在英汉词典中是动词 **等候** 的意思，用法如下：

```js
// 只能用在async函数中
const val = await promise
```

`await`可以让 js 进行等待，直到`promise`执行并返回结果时才会继续往下执行。可以看一个小例子：

```js
const foo2 = async () => {
  const promise = new Promise((reslove, reject) => {
    setTimeout(() => {
      reslove('芬达')
    }, 1000)
  })
  const ret = await promise
  console.log(ret)
}
foo2()
```

上面代码会在 1s 后打印`芬达`。  
从上面代码来说，await 在等一个承诺，好吧，是`promise`,更严谨的来说，他是在等待一个表达式，只不过这个表达式可以是 promise，也可以是其他任意表达式的结果。所以，下面的代码是完全可以运行的：

```
const foo3 = () => '芬达'
const foo4 = async () => {
    const ret = await foo3()
    console.log(ret)
}
foo4()
```

当然也是打印的`芬达`。

### await 等到了之后会做什么

通俗点说，你可以把 await 看做是一个运算符号，如果它等到的不是 promise，那么他的运算结果就是当前它所等到的值，如果等到的是 promise，那么他会临时阻塞后续代码，直到 promise 对象 resolve，取到 resolve 的值，将其作为运算结果返回。  
emmmmm, 就是这样。

![](https://user-gold-cdn.xitu.io/2019/3/25/169b528636e3055d?w=500&h=313&f=jpeg&s=28421)
~~猫咪分割线/猫咪分割线/猫咪分割线/猫咪分割线/猫咪分割线/猫咪分割线/~~

## async 和 promise 的联系

`async` 可以看做是`promise`与`Generator`的语法糖，但对其做了改进。

- 内置执行器，`Generator`的执行必须依靠执行器，但`async`的执行器与生俱来，使得`async`函数与普通函数的调用别无二致。
- 更好的语义化，~~就不做解释了吧~~
- 返回值是 promise，对开发者友好，感觉这个才是最重要的有木有。

![](https://user-gold-cdn.xitu.io/2019/3/25/169b54398321aa3a?w=534&h=300&f=jpeg&s=17342)
~~猫咪分割线/猫咪分割线/猫咪分割线/猫咪分割线/猫咪分割线/猫咪分割线/~~

## 它能为我们做什么？

这个问题的答案应该是毋庸置疑的，必然是为了解决回调地狱。`async/await`的优势就在于处理 then 式调用链呢。
我们可以先假设一个场景: 用户登录之后拿到`userId`,然后在去调用接口拿到`token`,最后在其他接口的请求头里添加上`token`字段.....~~别问为什么这么白痴的设计，假设而已~~

初级前端的写法：

```js
ajax('login', { username, password }, ({ userId }) => {
  ajax('getToken', { userId }, ({ token }) => {
    ajax('getOtherInfo', { token }, res => {
      // do something...
    })
  })
})
```

是不是头皮发麻，当然，如果你是写这样的代码的人，你可能觉得还可以接受，如果你是维护这样的代码的，你就会明白有多难维护，这里只写了三层，实际中甚至更多。意味着层级嵌套，牵一发而动全身，要改都得改的死胡同，意味着代码缩进都能恶心坏你，所以，初级大圆满前端是如何写的呢？

```js
ajax('login', { username, password })
  .then(({ userId }) => ajax('getToken', { userId }))
  .then(({ token }) => ajax('getOtherInfo', { token }))
  .then(res => {
    // do something...
  })
```

这样一来，利用链式 then 调用，既可以清晰的展现 api 依赖关系，又可以优雅的缩进代码，但我们是讲`async`的啊老铁，所以还是看下吧`asynv`大法。

```js
async function foo({ username, password }) {
  const { userId } = await ajax('login', { username, password })
  const { token } = await ajax('getToken', { userId })
  const res = await ajax('getOtherInfo', { token })
  // do something ...
}
```

上面有提到，`await`会临时阻塞后续代码的执行，这就代表着接口调用会按照我们的代码顺序"同步"执行（说是同步，**实际上还是异步请求**，表现为同步行为是语法糖的原因）。这样，代码就会按照我们的预期执行，接口依赖关系也更加明了，维护起来也是赏心悦目的。

![](https://user-gold-cdn.xitu.io/2019/3/25/169b5749469a3522?w=500&h=276&f=gif&s=489765)

~~猫咪分割线/猫咪分割线/猫咪分割线/猫咪分割线/猫咪分割线/猫咪分割线/~~

## 它的优点和缺陷

上面有谈到他一部分的优点，其实还有如下优点

- 语法简洁，使代码可读性更高
- 能使用`try catch`捕获异常
- 使代码更加符合思维逻辑。

至于缺点呢...

- 需要 babel 编译
- 缺少 abort 请求中断，缺少异步控制流程。
- 异常捕获较为麻烦
- 没有依赖关系的请求需要借助 promise.all

![](https://user-gold-cdn.xitu.io/2019/3/25/169b5815659f27b9?w=500&h=500&f=jpeg&s=28853)

~~猫咪分割线/猫咪分割线/猫咪分割线/猫咪分割线/猫咪分割线/猫咪分割线/~~

## 如何优雅的在 async/await 中处理错误

理论上来说，`await`等到的可能是 promise.reject,这种情况可以使用`try/catch`来捕获异常，ok 没毛病，像下面这样

```js
try {
  const { userId } = await ajax('login', { username, password })
} catch {
  throw new Error('no user found')
}
```

But，如果像上面一样有很多个`await`呢，怎么办，每次都要写一下嘛，这样岂不是很难受？上周在沸点看到一位大佬对 promise 的异常处理，很有意义。如图
![](https://user-gold-cdn.xitu.io/2019/3/25/169b58a27cf51e28?w=1098&h=1062&f=jpeg&s=85317)，
同样的啊，`async`也是可以借助`promise`来实现统一的异常捕获。

```js
function util(promise) {
  return promise.then(data => [null, data]).catch(err => [err])
}
async function foo({ usernam, password }) {
  let userId, token, err
  ;[err, { userId }] = await util(ajax('login', { username, password }))
  // 因默认数据返回是对象，所以加了解构，部分省略...
  // do something
}
```

没有大佬的考虑全面，但也足以应付日常需求了
![](https://user-gold-cdn.xitu.io/2019/3/25/169b59377ec4cebd?w=500&h=313&f=jpeg&s=23324)
~~猫咪分割线/猫咪分割线/猫咪分割线/猫咪分割线/猫咪分割线/猫咪分割线/~~

## 简单总结

`async/await`总的来说，是一个优秀的异步解决方案，利大于弊，值得一用。

因为我平时用 async 的频率很低，所以专门总结了这篇文字。如果有什么错误还请各位大佬指正。

## 参考

[Async +Await](https://juejin.im/post/5ab60c606fb9a028bc2db1d4)  
[精读 Async/Await 更优越的 6 大理由](https://zhuanlan.zhihu.com/p/26505825)
