---
title: web缓存机制
date: 2018-07-19 17:15:50
tags:
- cache
categories:
- 前端
description: 关于前端缓存机制的学习笔记
---
# 缓存
前端主要的缓存方法有
- Cache-Control, Expires, Last-modified, ETag, localStorage, sessionStorage  

下面将依次介绍。
## Cache-Control
### max-age（单位 s）
指定设置缓存的最大有效时间，定义的是时间长短。浏览器向服务器发送请求后，在 max-age 时间里不会再向浏览器发送请求
![cache1](/images/web-cache/cache1.png)
如图，缓存有效期为2592000秒（30天），即30天内都会使用这个版本的资源，即使服务器上的资源发生改变浏览器也不会得到通知，max-age会覆盖 Expires

### s-maxage（单位 s） 和max-age一样，但只用于共享缓存（比如CDN）
如当s-maxage=60时，在这60秒中，即使CDN的内容更新，浏览器也不会请求。
> max-age用于普通缓存，s-maxage用于代理缓存。

如果存在s-maxage，则会覆盖max-age和 Expires header

public    指定响应会被缓存，在多用户间共享
private    响应只作为私有缓存，不在用户间共享    （如果要求http认证，响应会自动设置为private）
![cache2](/images/web-cache/cache2.png)
![cache3](/images/web-cache/cache3.png)

no-cache
    指定不缓存响应，表明资源不尽兴缓存
    设置 no-cache 不代表浏览器不缓存，而是在缓存前向服务器确认资源是否被更改，所以有时候只设置no-cache防止缓存是不保险的，还可以加上private指令，将过期时间设置为过去的时间。

no-store    绝对禁止缓存
must-revalidate    指定如果页面过期就去服务器进行获取。    不常用

## Expires    （HTTP 1.1开始，使用Cache-Control：max-age替代）
缓存过期时间
指定资源到期的时间，是服务器端的具体的时间。
Expires = max-age + 请求时间
需要和 last-modified 结合使用
cache-control 的优先级更高
Expires 是服务器响应消息头字段，在响应http请求时告诉浏览器在过期时间前浏览器可以直接从浏览器缓存中获取数据，无需再次请求。
![cache4](/images/web-cache/cache4.png)

## Last-modified
服务器文件的最后修改时间，需要和 cache-control 结合使用，是检查服务器资源是否更新的一种方式。
当浏览器再次请求时，会向服务器传送 If-Modified-Since 报头，询问 Last-Modified 时间点之后资源是否被修改过，如果没修改，返回304，使用缓存；如果修改过，再次向服务器请求资源，返回码和首次请求一样为200，资源为服务器端最新资源。
![cache5](/images/web-cache/cache5.png)

## ETag
根据实体内容生成一段hash字符串，标识资源状态，由服务器端产生。
浏览器会将这串字符串传回服务器，验证资源是否已修改
![cache6](/images/web-cache/cache6.png)

ETag可以解决Last-modified存在的问题：
- 1、一些服务器不能精确得到资源的最后修改时间，这样就无法通过 last-modified 判断资源是否更新；
- 2、如果资源修改频繁，在秒以下的单位时间内进行修改，而 last-modified 只能精确到秒；
- 3、如果资源的 last-modified 改变了，但是资源内容没变，ETag就认为资源还没有修改。


## LocalStorage 和 SessionStorage
LocalStorage在PC上的兼容性不太好，而且当网络速度快、协商缓存响应快时使用localStorage的速度比不上304。并且不能缓存css文件。而移动端由于网速慢，使用localStorage要快于304。

# 缓存流程
![cache7](/images/web-cache/cache7.png)

## cache-control 流程
![cache8](/images/web-cache/cache8.png)
