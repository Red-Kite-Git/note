# Vue插件拓展

## 一、路由 vue-router

### 1.介绍
路由的概念来源于服务端，在服务端中路由描述的是 URL与处理函数之间的映射关系，当然也会处理不同的 URL来展示不同的视图界面。随着Ajax的盛行，无刷新 交互成为了当下的主流，我们更希望在无刷新的情况下 完成不同URL来展示不同的视图界面，即在一个页面中 完成路由的切换（俗称：单页面应用开发SPA）这就是 前端路由。

在一个页面中完成URL与UI的 映射关系，一般我们有两种实现
* hash模式 hash 是 URL 中 hash (#) 及后面的那部分，常用作锚 点在页面内进行导航，改变 URL 中的 hash 部分不会 引起页面刷新。 
* history模式 history对象提供了pushState方法和popstate事 件,pushState方法可以让URL发生改变但并不会引起页 面的刷新,而popstate事件则用来监听URL改变的值,这 样就可以显示不同的UI内容 

Vue框架给我们提供了一个第三方的路由框架，即： vue-router。vue-router提供了两种路由模式，可自 由选择，而且在开发阶段，cli脚手架还帮我们处理了 history找不到页面的情况。 

### 2.配置vue-router

#### <1>安装路由插件

```bash
cd 项目所在路径
npm install vue-router
```

#### <2>创建单文件组件	src/component/xxx.vue

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
>```js
>//先导入组件，后使用
>import 组件名1 from "组件相对路径";
>const routes = [{
>    path: "/请求路径2",
>    component: 组件名1,
>}];
>```
>
>```js
>// 直接使用
>const routes = [{
>    path: "/请求路径2",
>    component: () => import("组件相对路径")
>}];
>```

```js
// 导入组件
import 组件名1 from "组件相对路径";
......;

// 导入路由函数
import { createRouter, createWebHistory } from "vue-router";

// 配置请求地址对应的路径
const routes = [
  {
    // 定义路由请求路径
    path: "/路由路径1",
    // 定义路由名称（用于通过名称跳转，可以不定义）
    name: "路由名称1"
    // 定义对应的组件
    component: 组件名1,
  },
  {
    path: "/路由路径2",
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

> 两种路由方式：
>
> ```js
> import { createRouter, createWebHistory } from "vue-router";
> const 路由名 = createRouter({
> 	history: createWebHistory(),
> });
> ```
>
> ```js
> import { createRouter, createWebHashHistory } from "vue-router";
> const 路由名 = createRouter({
> 	history: createWebHashHistory(),
> });
> ```
>
> 

#### <4>配置	src/main.js

```js
import { createApp } from "vue";
import App from "./App.vue";

import router from "./router";

// 使用路由
createApp(App).use(router).mount("#app");
```

### 3.路由组件跳转

#### <1>声明式路由

按照 router/index.js 中配置的path**路由路径**跳转

```html
<router-link to="/路由路径"> 展示内容 </ router-link >
```

按照 router/index.js 中配置的path**路由名称**跳转

```html
<router-link :to="/路由名称"> 展示内容 </ router-link >
```

#### <2>编程式路由

通过JS代码控制路由（Vue3）

```js
import { useRouter } from "vue-router";
const $router = useRouter();
$router.push("/路径")
```

> Tip：router与route的区别
> router用于调用路由方法，route用于获取路由信息
> router可以使用`router.createRoute.value`获取路由信息（相当于route）

### 4.路由组件跳转传参（声明式为例）

#### <1>普通传参query

参数在请求地址后面以?方式进行追加

```html
<router-link to="/路由路径?参数名1=值&参数名2:值2"> 展示内容 </ router-link >

<router-link to="/路由路径" ,{ 参数名1:值1 , 参数名2:值2 , ... }> 展示内容 </ router-link >
```

跳转到的组件获取值

```js
let 接收变量 = $route.params.参数名
```



#### <2>Rest风格传递参数 

路由配置中在path路径后添加 `/:参数名`

```js
const routes = [{
    path: "/路由路径/:参数名",
	...,
}];
```

参数在请求地址后面以 /参数 方式进行追加

```vue
<router-link to="/路由路径/" + 参数> 展示内容 </ router-link >
```

跳转到的组件获取值

```js
let 接收变量 = $route.path.参数名
```



#### <3>通过名字名称跳转传递参数

在路由配置中确保 定义了路由名称`name:路由名`

```js
const routes = [{
    name: "路由名称",
	...,
}];
```

`:to` 请求

```vue
<router-link :to= { naem:"路由名称", params:{ 参数1:值2 , 参数2:值2 , ... } } > 展示内容 </ router-link >
```

跳转到的组件获取值

```js
let 接收变量 = $route.path.参数名
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

Vue2写法 ▼
```js
import { createApp } from "vue";
import App from "./App.vue";

// 导入axios插件
import axios from "axios";

const app = createApp(App);
// 配置全局属性（在任意文件可以使用$http）
app.config.globalProperties.$http = axios;

app.mount("#app");
```

Vue3写法 ▼
```js
import { createApp } from "vue";
import App from "./App.vue";

import axios from "axios";

const http = axios.create({
  // 服务器IP地址
  baseURL: "http://localhost:80/",
  // 请求超时时间
  timeout: 5000,
});

const app = createApp(App);

app.provide("$http", http);

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
this.$http.get("后端接口请求路径" + this.定义的数据).then((response) => {
	// 对响应数据的操作
});
```

#### <3>传递对象（JSON数据） boby方式传参

```js
this.$http.post("后端接口请求路径",对象).then((response) => {
	// 对响应数据的操作
});
this.$http.post("后端接口请求路径" + this.定义的对象类型数据).then((response) => {
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
  // 指定存储方式（位置）
  storage: window.localStorage,
  // 指定持久化数据（不指定默认store中state的全部共享数据）
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
  // 使用定义的 vuexLocal插件
  plugins: [vuexLocal.plugin],
});

export default store;
```

#### <3>在组件导入使用（vue3语法）

```vue
<script setup>
// 导入vuex的useStore
import { useStore } from "vuex";
// 定义 变量名$store 方便使用 useStore() 函数
const $store = useStore();

$http.get("district/list").then((res) => {
	districtList.value = res.data.data;
    // 使用commit方法调用 定义的setList函数 将数据保存
	$store.commit("setList", districtList.value);
});
</script>
```

```vue
<script setup>
// 导入vuex的useStore
import { useStore } from "vuex";
// 定义局部命令
const $store = useStore();

// 将数据从state中拿到组件
let districtList = ref([]);
districtList.value = $store.state.districtList;
</script>
```





## 五、存储库 Pinia与持久化

### 1.介绍

 Pinia是Vue的官方状态管理库,专为Vue3设计 它允许跨组件或页面共享状态

用来替代Vuex 并提供更简洁的API和更好的TypeScript支持

### 2.使用

#### <1>安装

```bash
cd 项目所在路径
npm install pinia
npm install pinia-plugin-persistedstate
```

#### <2>创建文件	src/store/index.js

```js
// 导入函数
import { defineStore } from "pinia";
import { ref } from "vue";

// 定义方法
const useDisplayStore = defineStore(
  // 参数1：唯一ID （本地存储的名称就是唯一ID）
  "displayStore",
    
  // 参数2：函数
  () => {
    // 保存数据的集合
    const dataList = ref([]);
    // 保存数据的方法
    const setDataList = (data) => {
      dataList.value = data;
    };
    // 返回数据
    return {
      dataList,
      setDataList,
    };
  },
    
  // 参数3：持久化开启和关闭
  {
    // 开启pinia持久化
    persist: true,
  }
);

// 导出函数
export { useDisplayStore };
```

#### <3>配置	src\main.js

```js
import { createApp } from "vue";
import App from "./App.vue";

// import store from "./store";
// 注册pinia
import { createPinia } from "pinia";
// 注册pinia持久化插件
import PiniaPersisit from "pinia-plugin-persistedstate";
// 创建pinia实例
const pinia = createPinia();
// pinia使用持久化插件
pinia.use(PiniaPersisit);

const app = createApp(App);
// 根组件使用pinia
app.use(pinia);

//导入store中定义的函数（在注册pinia之后）
import { useDisplayStore } from "@/store";
//通过调用函数方式 获取store实例
const displayStore = useDisplayStore();
// 将store实例注入到全局
app.provide("$displayStore", displayStore);

app.mount("#app");
```

#### <4>在组件导入使用（vue3语法）

```vue
<script setup>
const $displayStore = inject("displayStore");
// 保存到本地数据
$displayStore.setList(集合.value);

// 拿出本地数据
$displayStore.dataList;
</script>
```

