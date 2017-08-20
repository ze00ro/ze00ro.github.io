---
layout: dev
title: "从小程序 学习 promise"
date: 2017-03-18 00:00:01
categories: article
tags: 

---

### 初遇回调地狱

我 js 一直都比较弱, 很多时候都是用 jquery 来操作. 学习小程序的时候顺便提高下 js 水平. 以前只会用 callback, 但是小程序的登录需要来来回回交互好几次, 这样回调下来就很深.

```javascript
// 不断的回调...
loginFlow: function (successCb, failCb) {
   var that = this;
   // 登录
   wx.login({
       success: function (res) {
           if (res.code) {
               // 获取 session
               wx.request({
                   url: that.globalData.apiDomain + '/session',
                   method: 'POST',
                   data: {
                       code: res.code
                   },
                   success: function (sessionRes) {
                       if (sessionRes.statusCode == 200) {
                           // 保存到服务端
                           wx.getUserInfo({
                           ...
```

1. 从 本地存储 取出登录状态, 就像 cookie, 是否需要登录
2. wx.login 返回 code
3. 用 code 到自己服务端请求接口 换 自己的session
4. session 存储起来作为登录状态
5. wx.getUserInfo 拿到加密的用户信息
6. 用 session 和 加密信息 到自己服务端解密

所以至少需要四层回调, 才可以把用户标识和自己的服务联系起来.

### 发现 promise

于是各种搜索, 发现大师们用 promise 来解决回调问题, 之前小程序不支持 promise, 还需要引入, 现在不需要引入了, 直接用 new Promise 就可以.

```javascript
// 用 promise 后
asyncLogin: function() {
   return new Promise(function (resolve, reject) {
       wx.login({
           success: resolve,
           fail: reject
       })
   });
},
asyncGetUser: function() {
   return new Promise(function (resove, reject) {
       wx.getUserInfo({
           success: resove,
           fail: reject
       })
   });
},
loginFlow: function () {
   var that = this;
   if (!this.globalData.session || !this.globalData.localUserInfo) {
       return that.asyncLogin({}).then(function (res) {
           // 用 code 去获取 session
           return that._http('/mina/session', 'GET', {code: res.code});

       }).then(function (res) {
           if (res.statusCode == 200) {
               // 把 session 放入 storage, 并且获取用户信息
               console.log("loginFlow > session fetched!");
               that.freshSession(res.data.session);
               return that.asyncGetUser({});
           } else {
               return Promise.reject(new Error("换取 session 失败"));
           }

       }).then(function (resUser) {
           // 把用户信息上传
           return that._http('/mina/user/auto', 'POST', resUser);

       }).then(function (res) {
       ...
```    

这样看起来就好很多了, 清晰的就能知道流程, 统一处理错误.

----

- [promise 迷你书](http://liubin.org/promises-book)
- [小程序中使用 promise](https://www.daier.org/121413.html)
- [回调地狱](http://www.cnblogs.com/huangxiaolu/p/3436488.html)








