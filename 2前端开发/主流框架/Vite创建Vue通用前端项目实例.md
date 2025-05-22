

# Vite创建Vue项目实例（通用前端设计）

## 一、Vite创建项目

### 1.确保安装Vite工具

```bash
vite --version
```

> 没有安装过执行安装
>
> ```bash
> npm install vite -g
> ```
>
> 使用Vite创建项目 项目名是manager 

### 2.创建项目

```bash
npm init vite@latest manager
```

> 选择框架framework：Vue		选择语言：JavaScript

### 3.安装插件

```bash
cd 项目路径
npm install vue-router vue element-plus axios -S
npm install ...
```

### 4.安装开发依赖

```bash
cd 项目路径
npm install sass -D
```

### 5.启动项目

```bash
cd 项目路径
npm install

npm run dev
npm run build
npm run preview
```

> `npm run dev`	用于启动开发服务器，提供热重载功能，允许开发者在不刷新浏览器的情况下看到代码更改的效果。不进行生产环境优化以便于开发者调试
>
> `npm run build`	用于执行构建过程，生成优化后的静态文件，这些文件适合部署到生产环境。构建后的文件通常放置 在项目的  dist 或 build 目录下，准备部署到生产服务器
>
> `npm run preview`	使用`npm run build`命令构建好项目以后就可以使用此命令浏览项目



## 二、项目相关文件配置

### 1.配置开发服务器和插件
vite.config.js ▼
```js
//Vite 提供的类型辅助函数，用于定义配置对象，提供类型检查和智能提示
import { defineConfig } from "vite";
//Vue 官方的 Vite 插件，用于支持 Vue 单文件组件（.vue 文件）
import vue from "@vitejs/plugin-vue";
export default defineConfig({
  //配置开发服务器
  server: {
    //前端项目端口号
    port: 8888,
    //配置 API 代理规则，用于解决跨域问题
    proxy: {
      //匹配所有以 /api 开头的请求路径
      "/api": {
        //转发到后端服务的地址和端口
        target: "http://localhost:8080/",
        //允许跨域访问
        changeOrigin: true,
        //重写请求路径，将 /api 前缀移除
        rewrite: (path) => path.replace(/^\/api/, ""),
      },
    },
  },
  //注册 Vite 插件，这里使用 vue() 启用 Vue 支持
  plugins: [vue()],
});
```

### 2.修改main.js文件【可选】

src/main.js ▼

```js
import { createApp } from "vue";
//import './style.css'
import App from "./App.vue";

//vite中需要使用import.meta.env来访问环境变量
console.log("运行模式:",import.meta.env.MODE)

const app = createApp(App);
app.mount("#app");
```

> 注意删除文件中注解的默认导入的样式文件 `imort './style.css'`，否则导致显示页面有样式问题

### 3.指定项目的开发模式

package.json ▼

```json
......
"scripts": {
    "dev": "vite --mode dev",
    "build": "vite build --mode dev",
    "preview": "vite preview"
},
......
```



## 三、插件等文件配置

> **<span style="color:green">TIP：插件文件与组件文件可以交叉进行，这个笔记中是先将全部需要的插件等文件配置好再创建组件文件</span>**

### 1.配置路由跳转

src/router/router.js ▼

```js
import { createRouter, createWebHashHistory } from "vue-router";
// import { createRouter, createWebHistory } from "vue-router";

const routes = [
    //根据组件编写路由
];

const router = createRouter({
  routes,
  history: createWebHashHistory(),
  // history: createWebHistory(),
});

// 路由守卫【可选】
// router.beforeEach((to, from, next) => {});

export default router;
```

App.vue ▼

```vue
<template>
  <router-view></router-view>
</template>
......
```

main.js ▼

```js
import { createApp } from "vue";
import App from "./App.vue";

// 导入路由
import router from "./router/router.js";

const app = createApp(App);

//使用路由
app.use(router);

app.mount("#app");
```

### 2.ElementPlus插件

main.js ▼

```js 
import { createApp } from "vue";
import App from "./App.vue";

import router from "./router/router.js";
// 导入ElementPlus插件和样式
import ElementPlus from "element-plus";
import "element-plus/dist/index.css";

const app = createApp(App);

//使用ElementPlus插件
app.use(ElementPlus);
app.use(router);

app.mount("#app");
```

### 3.后端接口统一管理

src\api\api.js ▼

```js
export default {
	//调用的后端接口函数 集中管理
};
```

main.js ▼

```js 
import { createApp } from "vue";
import App from "./App.vue";

import router from "./router/router.js";
import ElementPlus from "element-plus";
import "element-plus/dist/index.css";
// 导入自定义的后端接口集中管理文件
import api from "./api/api.js";

const app = createApp(App);

//将api挂载到全局属性上，以便在Vue组件中使用
app.config.globalProperties.$api = api;
app.use(ElementPlus);
app.use(router);

app.mount("#app");
```



## 四、创建组件文件

> **<span style="color:green">TIP：插件文件与组件文件可以交叉进行，这个笔记中是先将全部需要的插件等文件配置好再创建组件文件</span>**

### 1.创建项目首页组件

src\views\Home.vue ▼

```vue
<template>
  <div class="basic-layout">
    <!--左侧菜单类-->
    <div class="nav-side"></div>

    <!--导航栏-->
    <div class="content-right">
      <div class="nav-top">
        <div class="bread">导航栏</div>
      </div>
      <div class="wrapper">
        <!--主页面展示内容-->
        <div class="main-page">
          <router-view></router-view>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped lang="scss">
.basic-layout {
  position: relative;
  .nav-side {
    position: fixed;
    width: 200px;
    height: 100vh;
    background-color: #001529;
    color: #fff;
    /*当元素内容超出容器高度时,自动显示垂直滚动条*/
    overflow-y: auto;
    transition: width 0.5s;
  }
  .content-right {
    margin-left: 200px;
    .nav-top {
      height: 40px;
      line-height: 40px;
      display: flex;
      justify-content: space-between;
      border-bottom: 1px solid #ddd;
      padding: 0 20px;
    }
    .wrapper {
      background-color: #eef0f3;
      padding: 20px;
      height: 87vh;
      .main-page {
        background-color: white;
        height: 100%;
      }
    }
  }
}
</style>
```

### 2.创建主页面中的欢迎页

```vue
<template>
  <div>
    <h1>欢迎来到主页面</h1>
  </div>
</template>
```

### 3.创建登录页面组件

```vue
<template>
  <div class="login-wrapper">
    <div class="modal">
      <h2 class="title">通用后台管理系统</h2>
      <el-form>
        <el-form-item label="用户名">
          <el-input placeholder="请输入用户名" />
        </el-form-item>
        <el-form-item label="密&nbsp;&nbsp;&nbsp;&nbsp;码">
          <el-input placeholder="请输入密码" />
        </el-form-item>
        <el-form-item>
          <el-button class="btn-login" type="primary">登录</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script></script>

<style scoped lang="scss">
.login-wrapper {
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: #f9fcff;
  width: 100vw;
  height: 100vh;
  .modal {
    width: 500px;
    padding: 50px;
    background: #fff;
    border-radius: 5px;
    box-shadow: 0px 0px 10px 3px #c7c9c7;
    .title {
      font-size: 25px;
      line-height: 1.5;
      text-align: center;
      margin-bottom: 30px;
    }
    .btn-login {
      width: 100%;
    }
  }
}
</style>
```

### 配置路由src/router/index.js ▼

```js
......
const routes = [
  {
    path: "/home",
    name: "home",
    component: () => import("./../views/Home.vue"),
    redirect: "/home/welcome",
    children: [
      {
        path: "welcome",
        name: "welcome",
        component: () => import("./../views/Welcome.vue"),
        meta: {
          title: "后台管理系统",
        },
      },
    ],
  },
  {
    path: "/login",
    name: "login",
    component: () => import("./../views/Login.vue"),
    meta: {
      title: "登录",
    },
  },
];
......
```

修改main.js文件在项目中引用ElmentPlus插件 ...... import ElementPlus from 'element-plus' import 'element-plus/dist/index.css' //创建app实例 const app=createApp(Ap //使用ElementPlus app.use(ElementPlus) //挂载到id为app的div中 app.mount('#app') 在views文件夹下创建Login.vue文件  

 



六 根据项目开发模式不同,使用不同的服务器端 接口 修改config目录下的index.js添加项目的配置 const env=import.meta.env.MODE || 'dev' const EnvConfig={ dev:{ //baseApi: 项目后端接口地址，根据环境配置不同，指向不同的后 端服务。 baseApi:'/api', mockApi:'https://www.fastmock.site/mock/c1c302e8baed9894c48c17e4738c092e/api' }, test:{ baseApi:'/api', mockApi:'https://www.fastmock.site/mock/c1c302e8baed9894c48c17e4738c092e/api' }, prod:{ baseApi:'/api', mockApi:'' } } //导出相关的配置信息 export default{ env, mock:false, namespace:'manager',  ...EnvConfig[env] } 



七 对axios和localStorage进行二次封装 在utils文件夹中

 1 添加request.js文件,对axios二次封装 import axios from 'axios' import config from './../config' import { ElMessage } from 'element-plus' import router from '../router/router.js' const service=axios.create({ baseURL: config.baseApi, timeout: 8000 }) // 请求拦截器,判断请求投中是否有token,如果没有token使用Bearer token。 service.interceptors.request.use((req)=>{ const headers=req.headers; if(!headers.Authorization){ headers.Authorization='Bearer token' } return  req; }) // 设置响应拦截器根据不同的状态码 进行不同的处理 service.interceptors.response.use((res)=>{ //res.data 后端返回的result对象对应的json格式的数据 {code:'',data: {}, msg:""} const {code,data,msg} = res.data; if(code===200){ return data; }else if(code === 40001){ ElMessage.error(msg||'登录超时，请重新登录'); setTimeout(()=>{ router.push("/login"); },1000); return Promise.reject(msg||'登录超时，请重新登录'); }else{ ElMessage.error(msg||'服务器异常'); return Promise.reject(msg||'服务器异常'); } }) //封装axios发送请求方法 function request(options){ options.method=options.method || 'get' //前端发送请求时,无论是get请求还是post请求统一都使用data进行参 数传递 if(options.method.toLowerCase==='get'){ //如果是get请求,将data中的参数封装到params中 options.params=options.data; } //判断当前项目的环境,如果不是生产环境 if(config.env !='prod'){ //判断是否开启了mock,如果开启mock,就是mockApi的地址。如果 没有开启mock,就是baseApi的地址。 service.defaults.baseURL=config.mock? config.mockApi:config.baseApi; }else{ //如果是生产环境,直接使用baseApi的地址 service.defaults.baseURL=config.baseApi; } return service(options); } export default request; 修改main.js文件添加以下内容 ...... import request from './utils/request' //将request挂载到全局属性上，以便在Vue组件中使用。 app.config.globalProperties.$request=request; ...... 上面的函数定义好以后我们就可以调用后端接口,类似于 下面的一段代码 this.$request({ method:'get', url: "/login", data: { name:'jack' } }).then(res=>{ console.log(res); }) 实际是有些人习惯于类似像下面一样调用后端接口 this.$request.get("/login",{name:"jack"}).then(()=>{ console.log(res); }) 所以我们需要扩展request.js中的请求处理,在文 件中添加以下内容 ...... //遍历数组中的每个元素 ['get','post','put','delete','patch'].forEach((item)=>{ //将以上数组的每个元素定义为request函数的的静态属性,属性的值 是一个函数 //所以经过以下代码的处理 request函数中拥有了get 属性 post属性 put属性 delete属性 patch属性  属性的值是一个函数 //在函数中直接调用上面封装好的request函数 request[item]=(url,data,options)=>{ return request({ url, data, method:item, ...options })         } }); ...... export default request; 经过上面的处理我们就可以使用类似以下方式发送请求 this.$request.get("/login",{name:"jack"}).then(res=>{}) 

2 对localStorage进行二次封装 让localStorage支持按照不同的项目名,分开存储数据。 并且可以将对象信息转为JSON格式字符串存储 添加storage.js文件文件: import config from './../config' export default{ getStorage(){ return JSON.parse(window.localStorage.getItem(config.namespace) || "{}"); }, setItem(key,val){ let storage=this.getStorage() storage[key]=val window.localStorage.setItem(config.namespace,JSON.stringify(storage)) }, getItem(key){ return this.getStorage()[key] }, clearItem(key){ let storage=this.getStorage() delete storage[key] window.localStorage.setItem(config.namespace,JSON.stringify(storage)) }, clearAll(){ window.localStorage.clear() } } 修改main.js文件,导入storage ...... import storage from './utils/storage' app.config.globalProperties.$storage=storage ......