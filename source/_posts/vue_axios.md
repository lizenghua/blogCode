---
title: vue-axios封装和api接口管理
tags:
 - vue
 - 架构
---

### 项目中关于axios的目录结构

```js
├── src
|    ├── api
|    |    └── modules // 按模块就行区分，有利于多人协作开发
|    |    |   ├── home.js
|    |    |   ├── category.js
|    |    |   └── .....
|    |    ├── base.js // 管理接口域名（便于处理接口域名有多个的情况）
|    |    └── index.js // api接口的统一出口文件
|    ├── utils
|    |    └── http.js // 封装axios
|    ├── main.js
```

此文档中需要安装axios插件，使用vant的UI组件库
```js
npm i axios -S

npm i vant -S
//按需引入组件：安装babel插件
npm i babel-plugin-import -D
// babel.config.js 中配置
module.exports = {
  plugins: [
    ['import', {
      libraryName: 'vant',
      libraryDirectory: 'es',
      style: true
    }, 'vant']
  ]
};
```
> 接下来先将各相关文件的代码贴出来<br/>

<br/>
<!-- more -->

- ### utils文件夹下的 http.js
> 公用函数进行抽出，简化代码，尽量保证单一职责原则
```js
/*
 * @message: axios封装
 * @Author: lzh
 * @since: 2019-09-25 18:49:33
 * @lastTime: 2019-09-26 10:26:32
 * @LastAuthor: Do not edit
 * @copyright: lizenghua
 */
import axios from "axios";
import QS from "qs"; // 序列化post类型的数据
import { Toast } from "vant";
import router from "../router";

/**
 * @description: 提示函数 （禁止点击蒙层，显示一秒后关闭）
 * @param {String} msg 【提示文本】
 */
const tip = msg => {
  Toast({
    message: msg,
    duration: 1000,
    forbidClick: true
  });
};

/**
 * @description: 跳转登录页 (携带当前页面路由，以及在登录页面完成登录后返回当前页面)
 */
const toLogin = () => {
  router.replace({
    path: "/login",
    query: {
      redirect: router.currentRoute.fullPath
    }
  });
};

/**
 * @description: 请求失败后的错误统一处理
 * @param {Number} status 【请求失败的状态码】
 * @param {String} msg 【请求失败时携带的错误文本信息】
 */
const errorHandle = (status, msg) => {
  switch (status) {
    case 404:
      tip("请求的资源不存在");
      break;

    default:
      console.log(msg);
      break;
  }
};

// 设置请求超时
axios.defaults.timeout = 1000 * 12;
// 设置post请求头
axios.defaults.headers.post["Content-Type"] =
  "application/x-www-form-urlencoded;charset=UTF-8";

// 请求拦截器
axios.interceptors.request.use(
  config => {
    // Do something before request is sent
    return config;
  },
  error => {
    // Do something with request error
    return Promise.reject(error);
  }
);

// 响应拦截器
axios.interceptors.response.use(
  response => {
    // Do something before response is sent
    return response;
  },
  error => {
    // Do something with response error
    return Promise.reject(error);
  }
);

/**
 * @description: 请求方法封装
 * @param {String} url 【请求的url地址，默认空字符串】
 * @param {Object} params 【请求时携带的参数，默认空对象】
 * @param {String} type 【请求方法，默认get请求】
 * @return: 【Promise对象】
 */
export function $axios(url = "", params = {}, type = "GET") {
  let promise;
  return new Promise((resolve, reject) => {
    if (type.toUpperCase() === "GET") {
      promise = axios.get(url, { params: params });
    } else if (type.toUpperCase() === "POST") {
      promise = axios.post(url, QS.stringify(params));
    }
    promise
      .then(res => {
        resolve(res.data);
      })
      .catch(err => {
        reject(err.data);
      });
  });
}

```
<br/>

- ### api文件夹下的 base.js
> 考虑到接口会有多个不同域名的情况，所以通过js变量来控制接口域名;<br/>如果我们的项目不是高并发的大项目只有一个域名的接口，则可以在http.js中使用node环境变量来控制baseUrl的值而不需要此文件
```js
// 附加代码(在http.js中添加)：node环境切换
if (process.env.NODE_ENV == 'development') {    
    axios.defaults.baseURL = '/api';
} else if (process.env.NODE_ENV == 'debug') {    
    axios.defaults.baseURL = '';
} else if (process.env.NODE_ENV == 'production') {    
    axios.defaults.baseURL = 'http://api.xxx.com/';
}
```
```js
/*
 * @message: 管理接口域名【便于处理接口域名有多个的情况】
 * @Author: lzh
 * @since: 2019-09-25 20:36:51
 * @lastTime: 2019-09-26 10:04:18
 * @LastAuthor: Do not edit
 * @copyright: lizenghua
 */
const baseurl = {
  main: "http://xx.xx.com/xx/api",
  sub: "http://xxx.xxx.com/xx/api"
};

export default baseurl;
```
<br/>

- ### api文件夹 -- modules文件夹下的home.js
```js
/*
 * @message: 首页模块接口
 * @Author: lzh
 * @since: 2019-09-25 20:39:54
 * @lastTime: 2019-09-26 10:04:07
 * @LastAuthor: Do not edit
 * @copyright: lizenghua
 */
import baseurl from "../base";
import { $axios } from "@/utils/http";

const home = {
  // 轮播图
  getHomeBanner() {
    return $axios(`${baseurl.main}/homeApi`);
  }
};
export default home;
```
<br/>

- ### api文件夹中的 index.js
> 把api接口根据功能划分为多个模块，利于多人协作开发
```js
/*
 * @message:api接口的统一出口
 * @Author: lzh
 * @since: 2019-09-25 20:36:45
 * @lastTime: 2019-09-25 20:41:44
 * @LastAuthor: Do not edit
 * @copyright: lizenghua
 */
// 首页模块接口
import home from "./modules/home";
// 分类模块接口
import category from "./modules/category";
// 其他模块....

export default {
  home,
  category
};
```
<br/>

- #### main.js
```js
/*
 * @message: 入口文件
 * @Author: lzh
 * @since: 2019-09-25 11:45:44
 * @lastTime: 2019-09-26 10:04:30
 * @LastAuthor: Do not edit
 * @copyright: lizenghua
 */
import Vue from "vue";
import App from "./App.vue";
import router from "./router";
import api from "./api";

// 引入全局ui组件
import "@/plugins/vant";

// 将api挂载到vue的原型上，方便api调用
Vue.prototype.$api = api;
```
<br/>

- #### 调用接口（接口的使用）
```html
<!--
 * @message: 
 * @Author: lzh
 * @since: 2019-09-25 16:47:46
 * @lastTime: 2019-09-26 10:32:34
 * @LastAuthor: Do not edit
 * @copyright: lizenghua
 -->
<template>
  <div id="home">
    首页
  </div>
</template>

<script>
export default {
  name: "Home",
  created() {
    this.onLoadBanner();
  },
  methods: {
    onLoadBanner() {
      // 接口的使用方式：
      this.$api.home.getHomeBanner().then(res => {
        console.log(res);
      });
    }
  }
};
</script>

<style lang="scss" scoped></style>

```


***
#### 参考文章：
##### 1. 作者：愣锤（https://juejin.im/post/5b55c118f265da0f6f1aa354#comment）