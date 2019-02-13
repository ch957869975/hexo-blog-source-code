---
title: 两款简洁的 loading 动画
date: 2019-02-06 17:29:51
tags:
  - css
categories:
  - css
---

## HTML

```html
<!-- 动画1 -->
<div class="donut"></div>
<!-- 动画2 -->
<div class="bouncing-loader">
  <div></div>
  <div></div>
  <div></div>
</div>
```

## CSS

```css
@keyframes donut-spin {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}

.donut {
  margin: 0 auto;
  border: 4px solid rgba(0, 0, 0, 0.1);
  border-left-color: #7983ff;
  border-radius: 50%;
  width: 30px;
  height: 30px;
  animation: donut-spin 1.2s linear infinite;
}

@keyframes bouncing-loader {
  from {
    opacity: 1;
    transform: translateY(0);
  }
  to {
    opacity: 0.1;
    transform: translateY(-1rem);
  }
}

.bouncing-loader {
  display: flex;
  justify-content: center;
}

.bouncing-loader > div {
  width: 1rem;
  height: 1rem;
  margin: 3rem 0.2rem;
  background: #000;
  border-radius: 50%;
  animation: bouncing-loader 0.6s infinite alternate;
}

.bouncing-loader > div:nth-child(2) {
  animation-delay: 0.2s;
}

.bouncing-loader > div:nth-child(3) {
  animation-delay: 0.4s;
}
```

## 效果

录屏速度有点快，实际上动画速度是正常的（移动端可能不会动）  
![](https://ws3.sinaimg.cn/large/006tNc79gy1fzwu5p6thog308y07uaba.gif)
