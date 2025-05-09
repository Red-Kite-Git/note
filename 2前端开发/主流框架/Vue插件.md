# Vue插件拓展

## 一、路由 vue-router

### 1.介绍
路由的概念来源于服务端，在服务端中路由描述的是 URL与处理函数之间的映射关系，当然也会处理不同的 URL来展示不同的视图界面。随着Ajax的盛行，无刷新 交互成为了当下的主流，我们更希望在无刷新的情况下 完成不同URL来展示不同的视图界面，即在一个页面中 完成路由的切换（俗称：单页面应用开发SPA）这就是 前端路由。

在一个页面中完成URL与UI的 映射关系，一般我们有两种实现
* hash模式 hash 是 URL 中 hash (#) 及后面的那部分，常用作锚 点在页面内进行导航，改变 URL 中的 hash 部分不会 引起页面刷新。 
* history模式 history对象提供了pushState方法和popstate事 件,pushState方法可以让URL发生改变但并不会引起页 面的刷新,而popstate事件则用来监听URL改变的值,这 样就可以显示不同的UI内容 

Vue框架给我们提供了一个第三方的路由框架，即： vue-router。vue-router提供了两种路由模式，可自 由选择，而且在开发阶段，cli脚手架还帮我们处理了 history找不到页面的情况。 

### 2.使用vue-router

#### <1>安装路由插件

```bash
cd 项目所在路径
npm install vue-router
```

#### <2>创建单文件组件	src\components\文件名.vue

```vue
<template>
  <div>
    <h1>欢迎！</h1>
  </div>
</template>

<script>
export default {
  name: "组件名1",
};
</script>

<style scoped></style>

```

#### <3>配置路由文件	src/router/index.js

>导入并使用组件的两种方式：
>
>* import 组件名 from "@/components/welcome.vue"  +  component: 组件名
>* component: () => import("@/components/list.vue"),

```js
// 导入组件
import 组件名1 from "组件相对路径";
......;

// 导入路由函数
import { createRouter, createWebHistory } from "vue-router";

// 配置请求地址对应的路径
const routes = [
  {
    path: "/请求路径1",
    component: 组件名1,
  },
  {
    path: "/请求路径2",
    component: () => import("组件相对路径"),
  },
  ......,
];
// 定义路由对象
const 路由名 = createRouter({
  // 路由规则
  routes,
  // 路由方式
  history: createWebHistory(),
});

// 导出路由对象
export default 路由名;
```

#### <4>配置	src\main.js

```js
import { createApp } from "vue";
import App from "./App.vue";

import router from "./router";

// 使用路由
createApp(App).use(router).mount("#app");
```

### 3.动态路由模式与编程路由模式



### 4.路由组件跳转与传参

* 超链接方式组件跳转 

(1) 通过路由路径跳转传递参数 查看  

 (2) 通过名字名称跳转传递参数 foo 

* JS代码方式组件跳转

 (1) 参数在请求地址后面以?方式进行追加

```
this.$router.push({ name:'组件名称', query:{  "参数名称":"参数值" } }) 
```

(2) Rest风格传递参数 

```
this.$router.push("/地址/参数值"); 
```

3 组件中获得参数 

(1) 普通方式传递参数获得参数值 

```
this.$route.query.参数名 
```

(2) rest风格传递参数获得参数值 

```
this.$route.params.参数名 
```

四 路由配置rest风格传递参数 

```
{
	path:'地址/:id',
    name:"组件名称", 
    component: 组件
}
```



## 二、HTTP 客户端 axios

### 1.介绍

发送HTTP请求，调用后端接口

### 2.使用axios

#### <1>安装前后端交互插件

```bash
cd 项目所在路径
npm install axios
```

#### <2>修改	src\main.js

>使用axios插件要在需要使用的组件中导入，一般在main.js中配置全局属性，避免频繁导入

```js
import { createApp } from "vue";
import App from "./App.vue";

// 导入axios插件
import axios from "axios";

const app = createApp(App);
// 配置全局属性
app.config.globalProperties.$http = axios;

app.mount("#app");
```

### 3.在单文件组件中使用函数

>* 请求类型要与后端接口指定的请求类型一致（get、post、put、delete）
>* response响应数据具体是什么样的 在浏览器的开发者工具中网络响应中查看，再进行操作

```js
this.$http
    // response 是响应的数据
    .请求类型("后端接口请求路径").then((响应数据接收变量response) => {
    	// 对响应数据的操作（保存在内存）
    	this.data中定义的数据 = response.data.data
	}).catch((异常信息接收变量error) => {
        // 对程序异常处理的操作
    	console.log(error);
	});
```

### 4.请求传参

#### <1>普通（路径 ? 参数）query方式传参：

```js
this.$http.get("后端接口请求路径",{params: { 参数名: 参数值 }}).then((response) => {
	// 对响应数据的操作
});
```

#### <2>rest风格（路径 / 参数）path方式传参：

```js
this.$http.get("后端接口请求路径/参数").then((response) => {
	// 对响应数据的操作
});
this.$http.get("后端接口请求路径" + this.data中定义的数据).then((response) => {
	// 对响应数据的操作
});
```

#### <3>传递对象（JSON数据） boby方式传参

```js
this.$http.put("后端接口请求路径",对象).then((response) => {
	// 对响应数据的操作
});
this.$http.get("后端接口请求路径" + this.data中定义的对象类型数据).then((response) => {
	// 对响应数据的操作
});
```



## 三、存储数据 vuex

### 1.介绍

使用axios发送请求获取后端的返回值数据（JSON），需要多次使用返回值数据便需要多次重复发送请求

配合vuex可以将返回值数据保存下来，需要再次发送相同请求获取数据时直接使用vuex拿到保存的数据

### 2.使用vuex

#### <1>安装vuex

```bash
cd 项目所在路径
npm install vuex
```

#### <2>配置文件	src/store/index.js

```js
import { createStore } from "vuex";
......

const store = createStore({
  // state 定义单文件组件之间的共享数据
  state: {
    //定义共享数据
    共享数据...,
  },
  // mutations 定义保存信息的函数
  mutations: {
    函数名(state, payload) {
      state.共享数据 = payload;
    },
  },
});

export default store;
```

#### <3>配置	src\main.js

```js
import { createApp } from "vue";
import App from "./App.vue";

import store from "./store";

const app = createApp(App);

app.use(store);

app.mount("#app");
```



## 四、持久化数据 vuex-persist

### 1.介绍

vuex单独使用时，数据缓存在内存中，刷新页面会丢失

vuex配合vuex-persist一起使用，可以将vuex中的共享数据持久化保存到LocalStorage

### 2.使用vuex-persist

#### <1>安装vuex-persist插件

```bash
cd 项目所在路径
npm install vuex-persist
```

#### <2>修改文件	src/store/index.js

```js
import { createStore } from "vuex";
import VuexPersist from "vuex-persist";

// 定义vuexLocal插件
const vuexLocal = new VuexPersist({
  // 指定存储方式
  storage: window.localStorage,
  // 指定持久化数据（不写默认全部）
  reducer: (state) => ({
    membersList: state.membersList,
  }),
});

const store = createStore({
  state: {
    共享数据...,
  },
  mutations: {
    函数名(state, payload) {
      state.共享数据 = payload;
    },
  },
  // 使用vuexLocal插件
  plugins: [vuexLocal.plugin],
});

export default store;
```

