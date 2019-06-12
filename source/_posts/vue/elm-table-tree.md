---
title: Element 之 TreeTable Node 未刷新处理
date: 2019-06-10 14:24
categories: ['前端']
tags: ['element','vue','前端']
comments: true
---

# 问题描述

### element tree table开启 lazy 后，修改Node下的内容，无法刷新

# 处理思路

本来以为是版本的问题，当前使用的是 2.9.1 ，降了版本后，发现不行，**First Blood!!!**

询问了钉钉Element群里的人，有遇到和我一样问题的人，没有结果，**Double Kill!!!**

最后想到了，那就重新刷新一下页面？

1. window.location.reload()
2. this.$router.go(0)
3. ~~手动刷新~~

页面刷新当然可以，但是，我们这个是单页面系统，重新刷新的话，**资源的重新加载+基础页面的重新渲染**，这个代价太大了！

vue这么强大的框架，肯定有解决方案的。

找了一下，果然

**provide / inject** 完美解决

核心思路： 在父组件中通过**provide**来提供变量，然后在子组件中通过**inject**来注入变量

参考：[vue刷新当前页面](https://www.cnblogs.com/conglvse/p/9796551.html)

# END

上述的方法暴力的解决了node 节点数据部分的问题，带来的影响就是体验上有点不太好，每次刷新，之前的展开的node都会没有了。

目前暂时没找到更好的处理的方法，可能没掌握Element正确的使用姿势吧。之后有更好的方案，再更新。

未完，待续...







