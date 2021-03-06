---
layout: post
title:  "Web 性能优化"
date:   2018-03-27 15:05:00

categories: other
tags: performance
author: "Victor"
---

## 为什么要做性能优化
页面加载与交互响应是影响用户使用的关键因素。过于长的等待时间或操作响应延迟都会影响用户的正常使用。优化的目的是让用户有 “快” 的感受。

## 如何做性能优化

有很多潜在因素都可能会延缓服务器响应，例如应用逻辑缓慢、数据库查询缓慢、路由缓慢、框架、库、资源CPU不足或内存不足等。需要考虑所有这些因素，才能改善服务器的响应时间。

组成交互响应的时间是由：请求接收时间 + 数据整理时间 + 格式转换时间 + 数据返回接收时间组成。

### 资源加载

* 静态资源是包括面页显示资源、业务交互逻辑脚本资源、图片、声音、文字等没有状态的资源。一般是只加载一次，或不存在业务上变化逻辑的资源。
* 动态资源指的是所有数据资源，都是有状态需要持久化的资源。一般是通过特定的后端服务提供的并且存在业务变化逻辑的资源。如：用户列表、用户信息、商品信息、图表数据信息等。

### 数据加载

1. 发出请求。（客户端处理）
2. 服务器接收到请求。(受到具体网络情况影响。如：网络延迟、网络带宽等)
3. 服务器整理数据。**优化的重点，如：数据库操作效率低，逻辑冗余等**
4. 服务器返回数据。**优化的重点，转换算法处理不恰当或算法效率低**
5. 页面接收返回的数据。(受到具体网络情况影响。如：网络延迟、网络带宽等)
6. 页面渲染并呈现返回的数据。（客户端处理）

熟悉网站优化的开发者应该都知道，只要提到网页性能优化，就绕不开雅虎军规。总的来说，优化分成 传输快 vs 体验快。

1. 加载速度真的很快，用户打开输入网址按下回车立即看到了页面
2. 加载速度并没有变快，但用户感觉你的网站很快

### 传输快
所谓的真快就是网站资源以最快的速度到达用户浏览器。

- 传输的内容体积要小
  * 图片要压缩
  * 图片根据支持情况选择体积更小的格式如 webp
  * css、js 内容压缩
  * 服务端开启 Gzip，在传输数据之前再次压缩
- 传输的内容数量要少
  * 图片图标合并（css sprite）、svg 图标合并（svg sprite）
  * css、js 文件打包合并
- 网速要足够快
  * 服务器出口带宽要够
  * 考虑到南北差异、运营商差异，在不同地区部署服务器
  * 静态资源放 CDN
- 服务器响应要及时
  * 接口响应速度要快(数据库优化、查询优化、算法优化)
  * cpu、内存、硬盘读写不要成为瓶颈；多加几台机器
  * 重要页面(首页)静态化。服务端提前渲染后首页生成静态页面，用户访问首页直接返回静态页面，不需要像其他页面那样还需加载 css、js 再获取数据渲染展示
- 能重复利用的资源要利用好
  * 服务器设置合适的静态资源缓存时间
  * 前端文件打包时做合理的分块，让公共的资源缓存后能被多个页面复用
- 暂时不需要的资源先不要
  * 图片懒加载
  * 功能、模块、组件按需加载
- 将来需要的资源抽空要先拿到
  * DNS 预解析、预连接、预获取、预渲染

### 体验快

所谓的体验快就是让用户觉得网站的交互是“流畅的”、“舒适的”。

- 滚动页面不要有迟滞感
  - 对于短时连续大量触发的操作要做节流
- 一些常见操作不要感觉拖泥带水
  - DOM 的操作不要过于频繁
  - 不要出现内存泄露
  - 优化复杂运算
- 动画不要卡顿
  - 多用 CSS 动画，少用 JS 动画
  - 开启硬件加速
  - 不要用 setTimeout/setInterval 去模拟动画
  - 动画或者过渡的执行时间不要太久

## 性能优化的建议

* 臆想的优化不是优化，无明显成效的优化不是优化。
* 浏览器的性能已经足够快，不要因为“过渡优化”牺牲代码的可读性。
* 先做简单见效快的优化，再做复杂见效慢的优化。一张未压缩的大图片可能抵消辛辛苦苦做的全部其他技术优化。

## 相关链接

* [来，聊一聊性能优化](https://zhuanlan.zhihu.com/p/32150769)
* [说走就走的性能优化之旅](https://zhuanlan.zhihu.com/p/23834442)
* [雅虎前端优化35条规则翻译](https://github.com/creeperyang/blog/issues/1)
* [雅虎前端优化35条规则原文](https://developer.yahoo.com/performance/rules.html)
* [页面加载与API服务响应--- 性能标定与标准](https://zhuanlan.zhihu.com/p/34020557)
* [如何读懂火焰图？ - 阮一峰的网络日志](http://www.ruanyifeng.com/blog/2017/09/flame-graph.html)
* [如何使用 Timeline 工具](https://developers.google.com/web/tools/chrome-devtools/evaluate-performance/timeline-tool)
