

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

删除文件中注解掉的默认导入的样式文件 `imort './style.css'`，否则导致显示页面有样式问题

src/main.js ▼

```js
import { createApp } from "vue";
//import './style.css'
import App from "./App.vue";

const app = createApp(App);
app.mount("#app");
```

### 3.指定项目的开发模式

指定为开发环境 `--mode dev`

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

### 4.创建config配置文件

src\config\config.js

```js
// 获取当前环境
// vite创建的Vue项目中需要使用import.meta.env来访问环境变量
const env = import.meta.env.MODE || "dev";

// 环境配置
const EnvConfig = {
  // 配置开发环境
  dev: {
    // 开发环境的后端接口
    baseApi: "/api",
    // 开发环境的mock地址
    mockApi: "http://......",
  },
  // 配置生产环境
  prod: {
    // 生产环境的后端接口
    baseApi: "/api",
    // 生产环境的mock地址
    mockApi: "http://......",
  },
  // 配置测试环境
  test: {
    // 测试环境的后端接口
    baseApi: "/api",
    // 测试环境的mock地址
    mockApi: "http://......",
  },
};

export default {
  // 导出当前环境配置（...是ES6中的扩展运算符）
  ...EnvConfig[env],
  env,
  // 是否启用mock接口
  mock: false,
  // 命名空间（项目名称）给localStorage使用
  namespace: "manager",
};
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
......
// 导入路由
import router from "./router/router.js";
//使用路由
app.use(router);
......
```

### 2.ElementPlus插件

main.js ▼

```js 
......
// 导入ElementPlus插件和样式
import ElementPlus from "element-plus";
import "element-plus/dist/index.css";
//使用ElementPlus插件
app.use(ElementPlus);

// 导入ElementPlus图标
import * as ElementPlusIconsVue from "@element-plus/icons-vue";
for (const [key, component] of Object.entries(ElementPlusIconsVue)) {
  // 注册图标组件
  app.component(key, component);
}
......
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
......
// 导入自定义的后端接口集中管理文件
import api from "./api/api.js";
//将api挂载到全局属性上，以便在Vue组件中使用
app.provide("api", api);
......
```

### 4.二次封装axios

src\utils\request.js ▼
```js
import axios from "axios";
import config from "./../config/config.js";
import { ElMessage } from "element-plus";
import router from "./../router/router.js";

const service = axios.create({
  // 设置接口的基础地址
  baseURL: config.baseApi,
  // 请求超时时间，单位ms
  timeout: 3000,
});

// 请求拦截器,判断请求头中是否有token,如果没有token使用Bearertoken。
service.interceptors.request.use((req) => {
  // 获取请求头
  const headers = req.headers;
  // 判断请求头中是否有token
  if (!headers.Authorization) {
    // 如果没有token，设置token
    headers.Authorization = "Bearer token";
  }
  // 返回请求头（放行）
  return req;
});

// 设置响应拦截器根据不同的状态码 进行不同的处理
service.interceptors.response.use((res) => {
  // 根据后端封装的对象信息，解构获取后端的封装信息（此项目中后端封装统一数据格式为：{code:'',data:{}, message:""}）
  const { code, data, message } = res.data;
  // 根据后端返回不同的状态码做出不同的处理
  if (code === 200) {
    return data;
  } else if (code === 400) {
    // ElMessage是element-plus中的消息提示组件，功能类似alert
    ElMessage.error(message || "服务器内部异常");
    setTimeout(() => {
      router.push("/login");
    }, 2000);
    // Promise是ES6中新增的一个对象，用来处理异步操作
    return Promise.reject(message || "服务器内部异常");
  } else {
    ElMessage.error(message || "服务器异常：无异常信息");
    return Promise.reject(message || "服务器异常：无异常信息");
  }
});

//封装axios发送请求的第一种方法
function request(options) {
  // 不指定请求方式时 默认为get
  options.method = options.method || "get";

  if (options.method.toLowerCase === "get") {
    //前端发送请求时,无论是get请求还是post请求统一都使用data进行参数传递
    //如果是get请求,将data中的参数封装到params中
    options.params = options.data;
  }
  //判断当前项目的环境,如果不是生产环境
  if (config.env != "prod") {
    //判断是否开启了mock,如果开启mock,就是mockApi的地址。如果没有开启mock,就是baseApi的地址。
    service.defaults.baseURL = config.mock ? config.mockApi : config.baseApi;
  } else {
    //如果是生产环境,直接使用baseApi的地址
    service.defaults.baseURL = config.baseApi;
  }
  return service(options);
}

//封装axios发送请求的第二种方法
["get", "post", "put", "delete", "patch"].forEach((item) => {
  // 函数[] 括号内可以放变量，变量名就是函数名
  request[item] = (url, data, options) => {
    // 传入的属性名与使用的属性名一致，可以将url:url写为url
    return request({
      url,
      method: item,
      data,
      ...options,
    });
  };
});

export default request;
```

main.js ▼
```js
......
// 导入二次封装的request.js
import request from './utils/request.js'
//将request挂载到全局属性上
app.provide("request", request);
 ......
```

> 封装的axios发送请求的第一种方法
> ```js
> $request({
>  method: "请求类型",
>  url: "请求路径",
>  data: { 参数名:"值" },
> }).then((res) => {
> 	// 对响应数据的操作
> }).catch(err=>{});
> ```
> 封装的axios发送请求的第二种方法
>
> ```js
> $request.请求方式("url",data)
> .then(res=>{
>     // 对响应数据的操作
> }).catch(err=>{});
> ```
>
> 

### 5.二次封装localStorage

localStorage中存储的是一个对象Object，二次封装后可以转为JSON数据
key：命名空间（项目名称）
data(存储数据JSON): {user: {name: "张三", age: 18}, token: "xxx" }}

src\utils\storage.js ▼
```js
import config from "./../config/config.js";

export default {
  getStorage() {
    // 根据项目名获取项目的命名空间数据（对象Object） 转为JSON格式后返回  一开始没有数据返回空JSON"{}"
    return JSON.parse(window.localStorage.getItem(config.namespace) || "{}");
  },
  setItem(key, value) {
    // 获取命名空间数据
    let storage = this.getStorage();
    // 将新的数据添加到原本的命名空间数据（对象Object）中（添加属性并赋值）
    storage[key] = value;
    // 将更新后的命名空间数据存储到localStorage中
    window.localStorage.setItem(config.namespace, JSON.stringify(storage));
  },
  getItem(key) {
    // 从JSON数据中根据key获取到对应的value
    return this.getStorage()[key];
  },
  clearItem(key) {
    // 获取命名空间数据
    let storage = this.getStorage();
    // 根据key删除命名空间数据中的对应数据
    delete storage[key];
    // 将更新后的命名空间数据存储到localStorage中
    window.localStorage.setItem(config.namespace, JSON.stringify(storage));
  },
  clearAll() {
    // 清空localStorage中的所有数据
    window.localStorage.clear();
  },
};
```

main.js ▼

```js
......
// 导入二次封装的storage.js
import storage from "./utils/storage";
// 将storage挂载到全局属性上
app.provide("storage", storage);
 ......
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

src\views\Welcome.vue ▼

```vue
<template>
  <div>
    <h1>欢迎来到主页面</h1>
  </div>
</template>
```

### 3.创建登录页面组件

src\api\api.js ▼

```js
import $request from "./../utils/request.js";

export default {
  // 添加一个登录的函数
  login(params) {
    return $request.post("/users/login", params);
  },
};
```

src\views\Login.vue ▼

```vue
<template>
  <div class="login-wrapper">
    <div class="modal">
      <h2 class="title">通用后台管理系统</h2>
      <!-- :rules 绑定rules属性 prop定义规则名 -->
      <el-form
        :model="users"
        :rules="rules"
        ref="loginForm"
        @keyup.enter="handleLogin"
      >
        <el-form-item label="用户名" prop="userName">
          <el-input
            placeholder="请输入用户名"
            type="text"
            v-model="users.userName"
            prefix-icon="user"
          />
        </el-form-item>
        <el-form-item label="密&nbsp;&nbsp;&nbsp;&nbsp;码" prop="userPwd">
          <el-input
            placeholder="请输入密码"
            type="password"
            v-model="users.userPwd"
            prefix-icon="lock"
          />
        </el-form-item>
        <el-form-item>
          <el-button class="btn-login" type="primary" @click="handleLogin">
            登录
          </el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script setup>
import { inject, ref } from "vue";
import { useRouter } from "vue-router";

const $router = useRouter();
const $storage = inject("storage");
const $api = inject("api");

let users = ref({
  userName: "",
  userPwd: "",
});
// 定义规则数组
const rules = ref({
  // required：是否必填项   message: 提示信息   trigger: 触发事件
  userName: [
    { required: true, message: "用户名为必填项", trigger: "blur" },
    { min: 3, max: 5, message: "用户名长度在3-5个字符之间", trigger: "blur" },
  ],
  userPwd: [
    { required: true, message: "密码为必填项", trigger: "blur" },
    { min: 3, max: 10, message: "密码长度在3-10个字符之间", trigger: "blur" },
  ],
});

// 获取表单属性ref="loginForm"的DOM对象
const loginForm = ref();
const handleLogin = () => {
  loginForm.value.validate((valid) => {
    if (valid) {
      // 调用集中管理请求api中的登录接口
      $api.login(users.value).then((res) => {
        $storage.setItem("loginUser", res);
        $router.push("/home");
      });
    }
  });
};
</script>

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



### 配置登录、主页-欢迎页的路由

src/router/index.js ▼

```js
......
const routes = [
  {
    path: "/home",
    name: "home",
    component: () => import("./../views/Home.vue"),
    // 进入home默认展示welcome页面（重定向）
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

// 使用路由守卫 拦截所有路由请求
router.beforeEach((to, from, next) => {
  // 判断是否配置了标题的元数据mata
  if (to.meta.title) {
    // 动态设置浏览器标题
    document.title = to.meta.title;
  } else {
    // 默认标题
    document.title = "后台管理系统";
  }
  next();
});
......
```

### 4.
