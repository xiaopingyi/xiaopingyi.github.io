---
layout: post
title: 终止异步请求
categories: 网络请求
description: 终止异步请求
keywords: XMLHttpRequest、axios
---

问题：A页面跳转B页面，如何取消A页面还没有返回的请求

日常开发中没有接触到这类问题，一般页面中请求未完成是不让用户继续操作的，但某些情况下也需要取消请求，例如接口返回的数据很多，需要等待时间较长，这时很影响用户体验，用户需要取消这个请求，于是这里就涉及到终止网络请求

### XMLHttpRequest

#### 介绍
使用 XMLHttpRequest（XHR）对象可以与服务器交互。您可以从URL获取数据，而无需让整个的页面刷新。这允许网页在不影响用户的操作的情况下更新页面的局部内容

#### 使用方法
```javascript
var xhr = new XMLHttpRequest(),
    method = "GET",
    url = "https://developer.mozilla.org/";
xhr.open(method,url,true);

xhr.addEventListener('load',()=>{
      // 从列表中移除请求
      this.$store.actions.removeXhrRequest(source)
})

xhr.send(); // 发送请求

xhr.abort(); // 终止请求
```

### Axios
#### 介绍
Axios 是一个基于 promise 的 HTTP 库，可以用在浏览器和 node.js中;Axios发起请求的本质也是创建一个XMLHttpRequest

#### 使用方法
```javascript
import axios from 'axios'
var CancelToken = axios.CancelToken;
var source = CancelToken.source(); // {token: CancelToken,cancel: Canceler;}

// get请求
axios.get('/api/xxxxx/xxxxx', {
  cancelToken: source.token
}).catch(function(thrown) {
  if (axios.isCancel(thrown)) {
    console.log('Request canceled', thrown.message);
  } else {
    // 处理错误
  }
});
this.$store.actions.pushXhrRequest(source)

// 取消请求（message 参数是可选的）
source.cancel('Operation canceled by the user.');
```

其他的请求也有类似的方法终止异步请求；思想类似

### 路由跳转终止异步请求
#### 思路

1. 发起请求时将请求保存
2. 每次请求完成后将请求移除
3. 路由切换时，将未请求完成的请求终止

#### 实现
```javascript
import Vue from 'vue'
import Vuex from 'vuex'

export default new Vuex.Store({
    state: {
        xhrRequest : []
    },
    mutations: {
        setXhrRequest(state, payload){
          state.xhrRequest = [...payload];
        }
    },
    actions: {
      pushXhrRequest(content,req){
        context.commit(setXhrRequest,req)
      }
    },
    modules: {

    }
})
```

```javascript
router.beforeEach((to, from, next) => {
  this.$store.state.xhrRequest.map(xhr=>{
    xhr.abort(); // 终止请求
  })
  window.lastUrl = from //保存最后一个路由地址
  // login(next);
  next();
})
```
