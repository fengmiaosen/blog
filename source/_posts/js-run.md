---
title: 从一道前端面试题看JavaScript的核心运行机制
date: 2017-03-15 17:26:53
tags: [javascript]
---


```js
setTimeout(function() {
  console.log(1)
}, 0);
new Promise(function executor(resolve) {
  console.log(2);
  for( var i=0 ; i<10000 ; i++ ) {
    i == 9999 && resolve();
  }
  console.log(3);
}).then(function() {
  console.log(4);
});
console.log(5);
```

运行结果：


```
2
3
5
4
1
```

# 参考资料

* [Excuse me？这个前端面试在搞事](https://zhuanlan.zhihu.com/p/25407758)！
* [深入理解 JavaScript 运行机制](https://juejin.im/entry/5847c101128fe10058bcef6e)
* [JavaScript 运行机制详解：再谈Event Loop](http://www.ruanyifeng.com/blog/2014/10/event-loop.html)
* [从setTimeout谈JavaScript运行机制](http://www.cnblogs.com/zichi/p/4604053.html)
* [深入理解 Javascript 运行机制及原型](http://coolnuanfeng.github.io/javascriptRun)



