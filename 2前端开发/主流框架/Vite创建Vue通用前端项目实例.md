

# Vite创建Vue项目实例（通用同台系统-前端）

## 一、Vite创建项目

### 1.创建项目工具Vite

查看Vite是否已安装

```bash
vite --version
```

> 若没有安装则执行安装
>
> ```bash
> npm install vite -g
> ```
>

### 2.创建项目

```bash
npm init vite@latest 项目名
```

> 本项目使用	Vue + JavaScript

### 3.配置项目插件/依赖

```bash
cd 项目路径
npm install vue vue-router axios vuex@next element-plus -S
npm install sass -D
......
```

> 1. **vue**
>    核心框架 Vue.js 本身，用于构建用户界面的渐进式 JavaScript 框架。
> 2. **vue-router**
>    Vue.js 官方的路由管理器，用于实现单页面应用（SPA）的路由功能，支持路由导航、参数传递、路由守卫等。
> 3. **axios**
>    基于 Promise 的 HTTP 客户端，用于浏览器和 Node.js 环境，处理 HTTP 请求（如数据获取、提交表单等）。
> 4. **vuex@next**
>    Vue.js 官方的状态管理模式库，用于集中管理应用的共享状态（如用户登录信息、全局配置等），尤其适合中大型项目的数据流转与组件通信。`@next` 表示安装 Vuex 4.x 版本（兼容 Vue 3.x）
> 5. **element-plus**
>    基于 Vue 3 的桌面端组件库，提供丰富的 UI 组件（按钮、表单、表格等），帮助快速搭建美观、统一的界面。
> 6. **Sass**
>    （Syntactically Awesome Stylesheets）是一种 CSS 预处理器，扩展了 CSS 语法，支持变量、嵌套规则、混合器（Mixins）、导入等高级特性，最终编译为标准 CSS。

> **`-S`**：等价于 `--save`，表示将依赖保存到 `package.json` 的 `dependencies` 中（生产环境依赖）
>
> **`-D`** 等价于 `--save-dev`，表示将包安装到项目的 **开发依赖**（`devDependencies`）中

### 5.安装项目依赖

```bash
cd 项目路径（若已在项目路径则忽略）
npm install
```

> `npm install`（简写 `npm i`）是 npm（Node Package Manager）的核心命令之一。根据项目根目录下的 `package.json` 文件和 `package-lock.json`（或 `npm-shrinkwrap.json`），安装所有指定的依赖包到项目的 `node_modules` 目录中

### 6.启动项目

```bash
cd 项目路径（若已在项目路径则忽略）
npm run dev
```
```bash
cd 项目路径（若已在项目路径则忽略）
npm run build
npm run preview
```

> - `npm run dev`	用于启动开发服务器，提供热重载功能，允许开发者在不刷新浏览器的情况下看到代码更改的效果。不进行生产环境优化以便于开发者调试
>
> - `npm run build`	用于执行构建过程，生成优化后的静态文件，这些文件适合部署到生产环境。构建后的文件通常放置 在项目的  dist 或 build 目录下，准备部署到生产服务器
>
>   `npm run preview`	使用`npm run build`命令构建好项目以后就可以使用此命令浏览项目
>

## 前端架构设计

```text
project-root/
├── api/                    # API接口管理
│   └── index.js            # API入口文件
├── assets/                 # 静态资源
│   ├── images/             # 图片资源
│   ├── styles/             # 样式资源
│   └── fonts/              # 字体资源
├── components/             # 组件
│   ├── common/             # 通用组件
│   └── layout/             # 布局组件
├── config/                 # 配置文件
│   └── config.js           # 项目配置
├── router/                 # 路由
│   └── router.js            # 路由配置
├── store/                  # Vuex状态管理
│   └── store.js            # Vuex配置
├── utils/                  # 工具函数
│   ├── request.js          # axios二次封装
│   └── storage.js          # 本地存储二次封装
└── views/                  # 视图组件
    ├── Home.vue            # 首页
    └── About.vue           # 关于页
```



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
app.use(
  ElementPlus,
  // 默认使用小的图标
  { size: "small" }
);


// 导入ElementPlus图标
import * as ElementPlusIconsVue from "@element-plus/icons-vue";
for (const [key, component] of Object.entries(ElementPlusIconsVue)) {
  // 注册图标组件
  app.component(key, component);
}
......
```

### 3.使用vux插件

src\vuex\store.js ▼

```js
import { createStore } from "vuex";

import $storage from "./../utils/storage.js";

const store = createStore({
  // state 定义单文件组件之间的共享数据
  state: {
    //定义共享数据
    loginUser: $storage.getItem("loginUser") || {},
  },
  // mutations 定义保存信息的函数
  mutations: {
    savaLoginUser(state, payload) {
      state.loginUser = payload;
      $storage.setItem("loginUser", state.loginUser);
    },
    cleanLoginUser(state) {
      state.loginUser = {};
      $storage.clearItem("loginUser");
    },
  },
});

export default store;
```

main.js ▼

```js
......
// 导入vuex
import $store from "./vuex/store.js";
// 使用vuex
app.use($store);
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

#### 加入token验证机制

修改 src\utils\request.js ▼

```js
......
// 导入store对象
import store from "./../vuex/store.js";
......
// 请求拦截器
service.interceptors.request.use((req) => {
  // 获取请求头
  const headers = req.headers;

  //如果不是登录请求,其他全部请求都需要携带token令牌
  if (req.url != "/users/login") {
    // 将token放入请求头中（没有登录则为空值）
    headers.token = store.state.loginUser.token;
  }

  return req;
});
......
```

#### 后端接口统一管理

src\api\api.js ▼

```js
// 导入二次封装后的axios
import $request from "./../utils/request.js";

export default {
  //调用的后端接口函数 集中管理
  // 使用封装的$request向后端发送请求
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

### 5.二次封装本地存储localStorage

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

## 四、创建基础组件文件

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

<style lang="scss" scoped>
.basic-layout {
  position: relative;
  .nav {
    position: fixed;
    width: 200px;
    height: 100vh;
    background-color: #001529;
    color: white;
    overflow-y: auto;
    overflow-x: hidden;
    transition: width 0.3s; // 收缩展开过渡动画
    .logo {
      display: flex;
      align-items: center;
      font-size: 16px;
      height: 50px;
      img {
        margin: 0 16px;
        width: 32px;
        height: 32px;
      }
    }
    .nav-menu {
      height: calc(100vh - 50px);
      border-right: none;
    }
    &.fold {
      width: 64px;
    }
    &.unfold {
      width: 200px;
    }
  }
  .content {
    margin-left: 200px;
    &.fold {
      margin-left: 64px;
    }
    &.unfold {
      margin-left: 200px;
    }
    .top-crumb {
      height: 50px;
      line-height: 50px;
      display: flex;
      justify-content: space-between;
      border-bottom: 1px solid #ddd;
      padding: 0 20px;
      .top-crumb-left {
        display: flex;
        align-items: center;
        .fold-menu {
          cursor: pointer;
        }
        .bread {
          margin-left: 10px;
        }
      }
      .user-info {
        .notice {
          margin-right: 15px;
          line-height: 25px;
        }
        .username-span {
          cursor: pointer;
          color: #4091ff;
          line-height: 45px;
          margin-right: 15px;
          outline: unset;
        }
      }
    }
    .wrapper {
      background: #eef0f3;
      padding: 20px;
      height: calc(100vh - 50px);
      .main {
        background-color: #fff;
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
  <div class="welcome">
    <div class="content">
      <div class="sub-title">欢迎使用</div>
      <div class="title">通用后台管理系统</div>
      <div class="desc">技术栈:Vue3.0+ElementPlus+SpringBoot</div>
    </div>
    <div class="img"></div>
  </div>
</template>

<script></script>

<style scoped lang="scss">
.welcome {
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: white;
  height: 100%;
  .content {
    .sub-title {
      font-size: 30px;
      line-height: 42px;
      color: #333;
    }
    .title {
      font-size: 40px;
      line-height: 62px;
      color: #409eff;
    }
    .desc {
      text-align: right;
      font-size: 14px;
      color: #999;
    }
  }
  .img {
    background: url(../assets/images/welcome.png) no-repeat;
    margin-left: 120px;
    width: 371px;
    height: 438px;
  }
}
</style>
```

### 3.创建登录页面组件

src\views\Login.vue ▼

```vue
<template>
  <div class="login-wrapper">
    <div class="modal">
      <h2 class="title">通用后台管理系统</h2>
      <!-- :rules 绑定rules属性 prop定义规则名 -->
      <el-form :model="users" :rules="rules" ref="loginForm" @keyup.enter="handleLogin()">
        <el-form-item label="用户名" prop="userName">
          <el-input
            placeholder="请输入用户名"
            type="text"
            v-model="users.userName"
            prefix-icon="user" />
        </el-form-item>
        <el-form-item label="密&nbsp;&nbsp;&nbsp;&nbsp;码" prop="userPwd">
          <el-input
            placeholder="请输入密码"
            type="password"
            v-model="users.userPwd"
            prefix-icon="lock" />
        </el-form-item>
        <el-form-item>
          <el-button class="btn-login" type="primary" @click="handleLogin()"> 登录 </el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script setup>
import { inject, ref } from 'vue';
import { useRouter } from 'vue-router';

import { useStore } from 'vuex';
const $store = useStore();

const $router = useRouter();
const $api = inject('api');

let users = ref({
  userName: '',
  userPwd: '',
});
// 定义规则数组
const rules = ref({
  // required：是否必填项   message: 提示信息   trigger: 触发事件
  userName: [
    { required: true, message: '用户名为必填项', trigger: 'blur' },
    { min: 3, max: 10, message: '用户名长度在3-10个字符之间', trigger: 'blur' },
  ],
  userPwd: [
    { required: true, message: '密码为必填项', trigger: 'blur' },
    { min: 3, max: 10, message: '密码长度在3-10个字符之间', trigger: 'blur' },
  ],
});

// 获取表单属性ref="loginForm"的DOM对象
const loginForm = ref();
const handleLogin = () => {
  loginForm.value.validate((valid) => {
    if (valid) {
      // 调用集中管理请求api中的登录接口
      $api.login(users.value).then((res) => {
        // 将登录用户信息存储到本地存储中
        $store.commit('savaLoginUser', res);
        $router.push('/home');
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

src\router\router.js ▼

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
......
```

### 4.创建动态主页侧边栏

src\components\TreeMenu.vue ▼

```vue
<template>
  <template v-for="item in menus" :key="item.path">
    <!-- 遍历展示菜单（有子菜单的项） -->
    <el-sub-menu
      v-if="item.children && item.children.length > 0"
      :index="item.path"
    >
      <!-- el标签定义的el-sub-menu需要<template #title></template>插槽 -->
      <template #title>
        <el-icon>
          <el-icon><component :is="item.icon" /></el-icon>
        </el-icon>
        <span>{{ item.name }}</span>
      </template>
      <!-- 递归展示子菜单 -->
      <tree-menu :menus="item.children" />
    </el-sub-menu>

    <!-- 处理没有子菜单的项 -->
    <el-menu-item v-else-if="item.hide === false" :index="item.path">
      <el-icon><component :is="item.icon" /></el-icon>
      <span>{{ item.name }}</span>
    </el-menu-item>
  </template>
</template>

<script setup>
import { defineProps } from "vue";

// 定义组件接收的属性
const props = defineProps({
  menus: {
    type: Array,
    // 工厂函数返回默认值
    default: () => [],
    required: true,
  },
});
</script>
```

### 5.创建导航栏组件

src\components\Breadcrumb.vue ▼

```vue
<template>
  <el-breadcrumb :separator-icon="ArrowRight">
    <el-breadcrumb-item
      v-for="(item, index) in breadcrumbList"
      :key="item.path"
    >
      <router-link v-if="index == 0" to="/home">首页</router-link>
      {{ item.meta.title }}
    </el-breadcrumb-item>
  </el-breadcrumb>
</template>

<script setup>
import { ArrowRight } from "@element-plus/icons-vue";
import { computed } from "vue";
import { useRoute } from "vue-router";

const $route = useRoute();
// 获取用户点击的路由完整路径
let breadcrumbList = computed(() => {
  // matched方法返回当前点击路由地址的所有路径（包含父级）
  return $route.matched;
});
console.log("from Breadcrumb.vue:");
console.log(breadcrumbList.value);
</script>
```

### 5.应用导航栏与侧边栏组件

src\views\Home.vue ▼

```vue
<template>
  <div class="basic-layout">
    <!--左侧菜单类-->
    <div :class="['nav', isCollapse ? 'fold' : 'unfold']">
      <div class="logo">
        <img src="../assets/images/logo.png" />
        <span>通用管理系统</span>
      </div>
      <!-- 菜单 -->
      <!-- default-active	页面加载时默认激活菜单的 index -->
      <el-menu
        class="nav-menu"
        background-color="#001529"
        text-color="#fff"
        :default-active="activeMenu"
        :router="true"
        :collapse="isCollapse">
        <!-- 调用菜单组件 -->
        <tree-menu :menus="menuTree" />
      </el-menu>
    </div>

    <!-- 导航栏 -->
    <div :class="['content', isCollapse ? 'fold' : 'unfold']">
      <div class="top-crumb">
        <div class="top-crumb-left">
          <div class="fold-menu" @click="toggleNav">
            <el-icon>
              <Operation />
            </el-icon>
          </div>
          <div class="bread"><Breadcrumb /></div>
        </div>
        <div class="user-info">
          <el-badge :is-dot="true" class="notice" type="danger">
            <el-icon>
              <Bell />
            </el-icon>
          </el-badge>
          <el-dropdown @command="handleCommand">
            <span class="username-span"> {{ userName }} </span>
            <template #dropdown>
              <el-dropdown-menu>
                <el-dropdown-item command="userCenter"> 个人中心 </el-dropdown-item>
                <el-dropdown-item command="updatePwd"> 修改密码 </el-dropdown-item>
                <el-dropdown-item command="loginOut"> 退出登录 </el-dropdown-item>
              </el-dropdown-menu>
            </template>
          </el-dropdown>
        </div>
      </div>
      <div class="wrapper">
        <router-view></router-view>
      </div>
    </div>
  </div>
</template>

<script setup>
import TreeMenu from './../components/TreeMenu.vue';
import Breadcrumb from './../components/Breadcrumb.vue';

import { computed, inject, onMounted, ref } from 'vue';
import { ElMessage } from 'element-plus';
import { useStore } from 'vuex';
import { useRouter } from 'vue-router';
const $store = useStore();
const $router = useRouter();
const $api = inject('api');

// 使用computed计算属性来获取当前路由的路径
const activeMenu = computed(() => $router.currentRoute.value.path);

let isCollapse = ref(false);
const toggleNav = () => {
  isCollapse.value = !isCollapse.value;
};

let userName = $store.state.loginUser.userName;

const handleCommand = (key) => {
  if (key === 'userCenter') {
    ElMessage.info('个人中心');
  } else if (key === 'updatePwd') {
    ElMessage.info('更新密码');
  } else if (key === 'loginOut') {
    $store.commit('cleanLoginUser');
    ElMessage.info('退出登录');
    $router.push('/login');
  }
};

let menuTree = ref([]);
onMounted(() => {
  $api.getUserInfo().then((res) => {
    menuTree.value = res.allRouters;
  });
});
</script>

<style lang="scss" scoped>
.basic-layout {
  position: relative;
  .nav {
    position: fixed;
    width: 200px;
    height: 100vh;
    background-color: #001529;
    color: white;
    overflow-y: auto;
    overflow-x: hidden;
    transition: width 0.3s; // 收缩展开过渡动画
    .logo {
      display: flex;
      align-items: center;
      font-size: 16px;
      height: 50px;
      img {
        margin: 0 16px;
        width: 32px;
        height: 32px;
      }
    }
    .nav-menu {
      height: calc(100vh - 50px);
      border-right: none;
    }
    &.fold {
      width: 64px;
    }
    &.unfold {
      width: 200px;
    }
  }
  .content {
    margin-left: 200px;
    &.fold {
      margin-left: 64px;
    }
    &.unfold {
      margin-left: 200px;
    }
    .top-crumb {
      height: 50px;
      line-height: 50px;
      display: flex;
      justify-content: space-between;
      border-bottom: 1px solid #ddd;
      padding: 0 20px;
      .top-crumb-left {
        display: flex;
        align-items: center;
        .fold-menu {
          cursor: pointer;
        }
        .bread {
          margin-left: 10px;
        }
      }
      .user-info {
        .notice {
          margin-right: 15px;
          line-height: 25px;
        }
        .username-span {
          cursor: pointer;
          color: #4091ff;
          line-height: 45px;
          margin-right: 15px;
          outline: unset;
        }
      }
    }
    .wrapper {
      background: #eef0f3;
      padding: 20px;
      height: calc(100vh - 50px);
      .main {
        background-color: #fff;
        height: 100%;
      }
    }
  }
}
</style>
```

### 添加页面所需接口

src\api\api.js ▼

```js
import $request from "./../utils/request.js";

// 调用的后端接口函数 集中管理
export default {
  //////////////////// 基础 //////////////////// 
  // 添加一个登录的函数
  login(params) {
    return $request.post("/users/login", params);
  },
  // 获取用户的路由信息和权限信息
  getUserInfo() {
    return $request.get("/users/info");
  },
};
```

## 五、项目组件文件

### 页面整体框架

```vue
<template>
  <div class="user-mange">
    <div class="query-form">
      <!-- 条件查询部分 -->
    </div>

    <div class="base-table">
      <div class="action">
        <!-- 操作按钮部分 -->
      </div>
      <div>
        <!-- 遍历结果集部分  -->
      </div>
      <div>
        <!-- 分页跳转部分（需要分页的话）  -->
      </div>
    </div>

    <div>
      <!-- 新增/编辑/分配权限等 对话框部分  -->
    </div>
  </div>
</template>

<script setup></script>
```

### 添加通用样式

src\App.vue

```vue
<style>
@import './assets/style/reset.css';
@import './assets/style/index.scss';
</style>
```

### 配置路由

src\router\router.js ▼

```js
......
const routes = [
  {
    ......,
    children: [
      ......,
      {
        path: "/system/user",
        name: "User",
        component: () => import("./../views/User.vue"),
        meta: {
          title: "用户管理",
        },
      },
      {
        path: '/system/menu',
        name: 'menu',
        component: () => import('./../views/Menu.vue'),
        meta: {
          title: '菜单管理',
        },
      },
      {
        path: '/system/role',
        name: 'role',
        component: () => import('./../views/Role.vue'),
        meta: {
          title: '角色管理',
        },
      },
    ],
  },
  ......
];
......
```

### 1.用户管理页面

#### 新增所需接口

src\api\api.js ▼

```js
import $request from './../utils/request.js';
export default {
  ......
  //////////////////// 用户管理页面 ////////////////////
  //查询部门集合
  getDepartmentList() {
    return $request.post('/depts/all');
  },
  //查询角色集合
  getRoleList() {
    return $request.get('/roles/list');
  },
  //分页查询用户信息集合
  pageUserList(params) {
    return $request.post('/users/list', params);
  },
  // 新增用户
  saveUser(form) {
    return $request.post('/users/save', form);
  },
  // 删除用户
  deleteUser(idArray) {
    return $request.post('/users/delete', idArray);
  },
  //查询用户详情
  getUserDetail(id) {
    return $request.get('users/detail/' + id);
  },
  //更新用户
  updateUser(form) {
    return $request.put('/users/update', form);
  },
};
```

#### 用户管理页面

src\views\User.vue ▼

```vue
<template>
  <div class="user-mange">
    <!-- 查询条件框部分 -->
    <div class="query-form">
      <el-form :inline="true" ref="queryForm" :model="queryParams">
        <el-form-item label="用户名：" prop="userName">
          <el-input placeholder="请输入用户名" v-model="queryParams.userName" />
        </el-form-item>
        <el-form-item label="用户状态：" prop="status">
          <el-select style="width: 100px" v-model="queryParams.status">
            <el-option label="任意" :value="0" />
            <el-option
              v-for="item in statusList"
              :key="item.id"
              :value="item.id"
              :label="item.name" />
          </el-select>
        </el-form-item>
        <el-form-item label="所在部门" prop="deptId">
          <el-select style="width: 100px" v-model="queryParams.deptId">
            <el-option label="任意" :value="0" />
            <el-option
              v-for="item in deptsList"
              :key="item.id"
              :value="item.id"
              :label="item.deptName" />
          </el-select>
        </el-form-item>
        <el-form-item label="用户角色：" prop="roleId">
          <el-select style="width: 100px" v-model="queryParams.roleId">
            <el-option label="任意" :value="0" />
            <el-option
              v-for="item in rolesList"
              :key="item.id"
              :value="item.id"
              :label="item.roleName" />
          </el-select>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="pageUserList()">查询</el-button>
          <el-button type="info" @click="resetForm()">重置</el-button>
        </el-form-item>
      </el-form>
    </div>

    <div class="base-table">
      <!-- 操作按钮 -->
      <div class="action">
        <el-button type="primary" @click="handleSave()">新增</el-button>
        <el-button type="danger" @click="handleDeleteBatch()">批量删除</el-button>
      </div>
      <!-- 分页遍历查询结果集  -->
      <div>
        <!-- 
        El提供的@selection-change 多选框事件
        El提供的:data 数据源
        -->
        <el-table border :data="userList" @selection-change="handleSelectionChange">
          <!-- 复选框 -->
          <el-table-column type="selection" width="55" />
          <el-table-column
            v-for="item in columns"
            :key="item.prop"
            :prop="item.prop"
            :label="item.label"
            :formatter="item.formatter" />
          <el-table-column fixed="right" label="操作" min-width="120">
            <!-- #default="scope"语法糖等价Vue2的slot="default"或未命名插槽 能接收到表格行数据的row、column数据 -->
            <template #default="scope">
              <el-button type="warning" size="small" @click="handleEdit(scope.row)">
                编辑
              </el-button>
              <el-button type="danger" size="small" @click="handleDelete(scope.row.userId)">
                删除
              </el-button>
            </template>
          </el-table-column>
        </el-table>
      </div>
      <!-- El提供的@current-change 点击换页事件 -->
      <div>
        <el-pagination
          layout="prev,pager,next"
          :page-size="pageParams.pageSize"
          :total="pageParams.totalCount"
          @current-change="changePage">
        </el-pagination>
      </div>
    </div>

    <!-- El对话框（新增/编辑用户） -->
    <div>
      <el-dialog title="新增/编辑用户" v-model="showDialog">
        <el-form ref="dialogForm" :model="dialogFormData" :rules="rules">
          <el-form-item label="用户名:" prop="userName">
            <el-input
              v-model="dialogFormData.userName"
              placeholder="请输入用户名"
              :disabled="dialogAction == 'edit'" />
          </el-form-item>
          <el-form-item label="用户邮箱:" prop="userEmail">
            <el-input
              v-model="dialogFormData.userEmail"
              placeholder="请输入邮箱"
              :disabled="dialogAction == 'edit'" />
          </el-form-item>
          <el-form-item label="用户手机:" prop="mobile">
            <el-input v-model="dialogFormData.mobile" placeholder="请输入手机号" />
          </el-form-item>
          <el-form-item label="用户岗位:" prop="job">
            <el-input v-model="dialogFormData.job" placeholder="请输入岗位" />
          </el-form-item>
          <el-form-item label="用户状态:" prop="state">
            <el-select v-model="dialogFormData.state">
              <el-option :value="0" label="请选择状态" />
              <el-option :value="1" label="在职" />
              <el-option :value="2" label="离职" />
              <el-option :value="3" label="试用期" />
            </el-select>
          </el-form-item>
          <el-form-item label="用户角色:" prop="roleId">
            <el-select v-model="dialogFormData.roleId">
              <el-option :value="0" label="请选择角色" />
              <el-option
                v-for="temp in rolesList"
                :key="temp.id"
                :value="temp.id"
                :label="temp.roleName" />
            </el-select>
          </el-form-item>
          <el-form-item label="用户所属部门:" prop="deptId">
            <el-select v-model="dialogFormData.deptId">
              <el-option :value="0" label="请选择部门" />
              <el-option
                v-for="temp in deptsList"
                :key="temp.id"
                :value="temp.id"
                :label="temp.deptName" />
            </el-select>
          </el-form-item>
        </el-form>
        <template #footer>
          <div class="dialog-footer">
            <el-button type="success" @click="handleSubmit()"> 提交 </el-button>
            <el-button type="info" @click="handleCancel()"> 取消 </el-button>
          </div>
        </template>
      </el-dialog>
    </div>
  </div>
</template>

<script setup>
import { ElMessage, ElMessageBox } from 'element-plus';
import { getCurrentInstance, inject, onMounted, ref } from 'vue';
const $api = inject('api');

// 获得当前组件实例，相当于Vue2中的this
const { ctx } = getCurrentInstance();

const columns = [
  //定义表格字段名和字段值（字段值为后端结果集的字段）
  { label: '用户ID', prop: 'userId' },
  { label: '用户名', prop: 'userName' },
  { label: '邮箱', prop: 'userEmail' },
  { label: '手机号', prop: 'mobile' },
  { label: '用户角色', prop: 'roleName' },
  { label: '所属部门', prop: 'deptName' },
  {
    label: '用户状态',
    prop: 'state',
    formatter: (row, column, value) => {
      return {
        1: '在职',
        2: '离职',
        3: '试用期',
      }[value];
    },
  },
  { label: '最后登录时间', prop: 'lastLoginTime' },
];

let deptsList = ref([]);
let rolesList = ref([]);
let statusList = ref([
  { name: '在职', id: 1 },
  { name: '离职', id: 2 },
  { name: '试用期', id: 3 },
]);
onMounted(() => {
  $api.getDepartmentList().then((res) => {
    deptsList.value = res;
  });
  $api.getRoleList().then((res) => {
    rolesList.value = res;
  });
  pageUserList();
});

//////////////////// 分页查询 ////////////////////
let userList = ref([]);
let queryParams = ref({
  // 后端需求的参数
  userName: '',
  status: 0,
  deptId: 0,
  roleId: 0,
  // 其他必填参数
  pageNum: 1,
  pageSize: 6,
  mobile: '',
});
let pageParams = ref({
  totalCount: 0,
  pageSize: queryParams.value.pageSize,
});
const pageUserList = () => {
  $api.pageUserList(queryParams.value).then((res) => {
    userList.value = res.currentPageRecords;
    pageParams.value.totalCount = res.totalCount;
    pageParams.value.pageSize = res.pageSize;
  });
};
const changePage = (pageNum) => {
  queryParams.value.pageNum = pageNum;
  pageUserList();
};

const resetForm = () => {
  //获取当前组件中ref值为queryForm的表单实例
  //调用Vue提供的resetFields方法（清空表单绑定的model中的prop）
  ctx.$refs.queryForm.resetFields();
  pageUserList();
};

//////////////////// 新增/编辑对话框 ////////////////////
// 新增与编辑公用一个表单，通过dialogAction标识当前对话框的类型
let dialogAction = ref('');
// 新增/编辑用户兑换框是否展示，默认不展示
let showDialog = ref(false);
// 表单信息
let dialogFormData = ref({
  state: 0,
  roleId: 0,
  deptId: 0,
});
// 前端表单校验规则
const rules = ref({
  userName: [{ required: true, message: '用户名不能为空', trigger: 'blur' }],
  userEmail: [
    { required: true, message: '邮箱不能为空', trigger: 'blur' },
    { type: 'email', message: '邮箱格式不正确', trigger: ['blur', 'change'] },
    // 或正则表达式 pattern: /^[0-9a-zA-Z]+@[0-9a-zA-Z]{2,}\.[a-zA-Z]{2,}$/
  ],
  mobile: [
    { required: true, message: '手机号不能为空', trigger: 'blur' },
    // 正则表达式校验
    { pattern: /^1\d{10}$/, message: '手机号格式不正确', trigger: ['blur', 'change'] },
  ],
  job: [{ required: true, message: '岗位不能为空', trigger: 'blur' }],
  // 自定义验证规则
  state: [
    {
      validator: (rule, value, callback) => {
        return value == 0 ? callback(new Error('请选择状态')) : callback();
      },
      trigger: 'blur',
    },
  ],
  roleId: [
    {
      validator: (rule, value, callback) => {
        return value == 0 ? callback(new Error('请选择角色')) : callback();
      },
    },
  ],
  deptId: [
    {
      validator: (rule, value, callback) => {
        return value == 0 ? callback(new Error('请选择部门')) : callback();
      },
    },
  ],
});
// 对话框唤起函数
// 新增对话框
const handleSave = () => {
  dialogAction.value = 'add';
  showDialog.value = true;
};
// 编辑对话框
const handleEdit = (row) => {
  dialogAction.value = 'edit';
  showDialog.value = true;
  //ctx.$nextTick() 可以在DOM更新完成后再执行代码，确保获取到的是最新的DOM状态
  ctx.$nextTick(() => {
    // 与新增公用一个表单，通过El的#default获取原数据 Object.assign(拷贝对象, 原数据)
    Object.assign(dialogFormData.value, row);
  });
};
// 对话框提交函数
const handleSubmit = () => {
  ctx.$refs.dialogForm.validate((valid) => {
    if (valid) {
      if (dialogAction.value == 'add') {
        $api.saveUser(dialogFormData.value).then((res) => {
          res ? ElMessage.success('操作成功') : ElMessage.error('操作失败');
          pageUserList();
        });
      } else if (dialogAction.value == 'edit') {
        $api.updateUser(dialogFormData.value).then((res) => {
          res ? ElMessage.success('操作成功') : ElMessage.error('操作失败');
          pageUserList();
        });
      }
      handleCancel();
    }
  });
};
// 对话框取消函数
const handleCancel = () => {
  ctx.$refs.dialogForm.resetFields();
  showDialog.value = false;
};

//////////////////// 删除用户 ////////////////////
// 配合批量删除及后端接口
let deleteParams = ref({
  userIds: [],
});
// 删除单个用户函数
const handleDelete = (userId) => {
  ElMessageBox.confirm('确定删除用户吗？', '提示', {
    confirmButtonText: '确定',
    cancelButtonText: '取消',
    type: 'warning',
  }).then(() => {
    // 保存到deleteParams中 push添加到集合最后
    deleteParams.value.userIds.push(userId);
    $api.deleteUser(deleteParams.value).then((res) => {
      res ? ElMessage.success('删除成功') : ElMessage.error('删除失败');
      deleteParams.value.userIds = [];
      pageUserList();
    });
  });
};
// 多选框事件函数
const handleSelectionChange = (userIds) => {
  // map遍历userIds数组，返回一个新数组，新数组元素数量不变，元素内容为userId
  deleteParams.value.userIds = userIds.map((item) => item.userId);
};
// 批量删除函数
const handleDeleteBatch = () => {
  if (deleteParams.value.userIds.length === 0) {
    ElMessage.warning('未选择任何用户！');
  } else {
    ElMessageBox.confirm('确定批量删除用户吗？', '批量删除警示', {
      confirmButtonText: '确定',
      cancelButtonText: '取消',
      type: 'error',
    }).then(() => {
      $api.deleteUser(deleteParams.value).then((res) => {
        res ? ElMessage.success('删除成功') : ElMessage.error('删除失败');
        deleteParams.value.userIds = [];
        pageUserList();
      });
    });
  }
};
</script>

<style scoped></style>
```

### 2.菜单管理页面

#### 新增所需接口

src\api\api.js ▼

```js
import $request from './../utils/request.js';
export default {
  ......
  //////////////////// 菜单管理页面 ////////////////////
  // 查询菜单集合
  getMenuList(params) {
    return $request.post('/menus/list', params);
  },
  // 删除菜单
  deleteMenu(id) {
    return $request.delete('/menus/delete/' + id);
  },
  // 新增菜单
  saveMenu(form) {
    return $request.post('/menus/save', form);
  },
  // 更新菜单
  updateMenu(form) {
    return $request.post('/menus/update', form);
  },
};
```

#### 菜单管理页面

src\views\Menu.vue ▼

```vue
<template>
  <div class="user-manage">
    <!-- 查询条件部分 -->
    <div class="query-form">
      <el-form :inline="true" ref="queryForm" :model="queryParams">
        <el-form-item label="菜单名称" prop="menuName">
          <el-input placeholder="请输入菜单名称" v-model="queryParams.menuName" />
        </el-form-item>
        <el-form-item label="菜单编码" prop="menuCode">
          <el-input placeholder="请输入菜单编码" v-model="queryParams.menuCode" />
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="selectMenuList()">查询</el-button>
          <el-button type="info" @click="resetQueryForm()">重置</el-button>
        </el-form-item>
      </el-form>
    </div>
    <!-- 查询结果部分 -->
    <div class="base-table">
      <div class="action">
        <el-button type="primary" @click="handleSave('Global')">新增</el-button>
      </div>
      <!-- El提供的表格-树形数据与懒加载 row-key属性、tree-props、default-expand-all -->
      <div>
        <el-table :data="menuList" row-key="id" :tree-props="{ children: 'children' }" border>
          <el-table-column
            v-for="item in columns"
            :key="item.label"
            :label="item.label"
            :prop="item.prop"
            :formatter="item.formatter" />
          <el-table-column fixed="right" label="操作" min-width="120">
            <template #default="scope">
              <el-button type="warning" @click="handleEdit(scope.row)"> 编辑 </el-button>
              <el-button type="danger" @click="handleDelete(scope.row.id)"> 删除 </el-button>
              <el-button type="primary" @click="handleSave('Partial', scope.row.id)">
                新增
              </el-button>
            </template>
          </el-table-column>
        </el-table>
      </div>
    </div>
    <!-- 新增/编辑对话框 -->
    <div>
      <el-dialog title="新增/编辑菜单" v-model="showDialog">
        <el-form ref="dialogForm" :model="dialogFormData" :rules="rules">
          <el-form-item label="菜单类型：" prop="menuType">
            <el-radio-group v-model="dialogFormData.menuType">
              <el-radio value="1">菜单</el-radio>
              <el-radio value="2">按钮</el-radio>
            </el-radio-group>
          </el-form-item>
          <el-form-item label="父级菜单：" prop="parentId">
            <!-- El的级联选择器 props.checkStrictly父子节点取消选中关联 -->
            <el-cascader
              clearable
              style="width: 40%"
              :options="menuList"
              :props="{ checkStrictly: true, value: 'id', label: 'menuName' }"
              v-model="dialogFormData.parentId"
              placeholder="请选择父级菜单" />
            <span style="color: red">无选择默认为顶级菜单</span>
          </el-form-item>
          <el-form-item label="菜单/按钮名称：" prop="menuName">
            <el-input v-model="dialogFormData.menuName" placeholder="请输入菜单名称" />
          </el-form-item>
          <el-form-item label="菜单图标：" prop="icon" v-show="dialogFormData.menuType == '1'">
            <el-select v-model="dialogFormData.icon" placeholder="请选择菜单图标">
              <el-option
                v-for="item in iconList"
                :key="item"
                :label="item.label"
                :value="item.value" />
            </el-select>
          </el-form-item>
          <el-form-item label="路由地址：" prop="path" v-show="dialogFormData.menuType == '1'">
            <el-input v-model="dialogFormData.path" placeholder="请输入路由地址" />
          </el-form-item>
          <el-form-item label="组件路径：" prop="component" v-show="dialogFormData.menuType == '1'">
            <el-input v-model="dialogFormData.component" placeholder="请输入组件路径" />
          </el-form-item>
          <el-form-item label="权限标识：" prop="perm" v-show="dialogFormData.menuType == '2'">
            <el-input v-model="dialogFormData.perm" placeholder="请输入权限标识" />
          </el-form-item>
        </el-form>
        <template #footer>
          <div class="dialog-footer">
            <el-button type="success" @click="handleSubmit()"> 提交 </el-button>
            <el-button type="info" @click="handleCancel()"> 取消 </el-button>
          </div>
        </template>
      </el-dialog>
    </div>
  </div>
</template>

<script setup>
import { ElMessage, ElMessageBox } from 'element-plus';
import { getCurrentInstance, inject, onMounted, reactive, ref } from 'vue';

const $api = inject('api');

const { ctx } = getCurrentInstance();

onMounted(() => {
  selectMenuList();
});

//////////////////// 查询 ////////////////////
let queryParams = reactive({});
// 重置表单函数
const resetQueryForm = () => {
  ctx.$refs.queryForm.resetFields();
  selectMenuList();
};
let menuList = ref([]);
let columns = ref([
  { label: '菜单名称', prop: 'menuName' },
  {
    label: '菜单类型',
    prop: 'menuType',
    formatter(row, column, value) {
      return {
        1: '菜单',
        2: '按钮',
      }[value];
    },
  },
  { label: '菜单编码', prop: 'menuCode' },
  { label: '权限标识', prop: 'perm' },
  { label: '路由地址', prop: 'component' },
  { label: '菜单图标', prop: 'icon' },
]);

const selectMenuList = () => {
  $api.getMenuList(queryParams).then((res) => {
    menuList.value = res;
  });
};

//////////////////// 新增/编辑对话框 ////////////////////
let showDialog = ref(false);
let dialogAction = ref('');
let dialogFormData = reactive({
  menuType: '1',
});
// 图标下拉框列表
const iconList = [
  { value: 'House', label: 'House' },
  { value: 'Setting', label: 'Setting' },
  { value: 'Menu', label: 'Menu' },
  { value: 'Tools', label: 'Tools' },
  { value: 'UserFilled', label: 'User' },
  { value: 'Grid', label: 'Grid' },
  { value: 'Avatar', label: 'Role' },
  { value: 'OfficeBuilding', label: 'Department' },
  { value: 'Expand', label: 'Procedure' },
  { value: 'DataLine', label: 'DataLine' },
];
// 前端表单校验规则
const rules = reactive({
  menuName: [
    { required: true, message: '菜单名称不能为空', trigger: 'blur' },
    { max: 10, message: '名称长度不能超过10个字符', trigger: ['change'] },
  ],
});
// 对话框唤起函数
const handleSave = (type, parentId) => {
  dialogAction.value = 'save';
  showDialog.value = true;
  if (type == 'Global') {
    //
  } else if (type == 'Partial') {
    dialogFormData.parentId = parentId;
  }
};
const handleEdit = (row) => {
  dialogAction.value = 'edit';
  showDialog.value = true;
  //ctx.$nextTick() 可以在DOM更新完成后再执行代码，确保获取到的是最新的DOM状态
  ctx.$nextTick(() => {
    // 与新增公用一个表单，通过El的#default获取原数据 浅拷贝Object.assign(拷贝对象, 原数据)
    Object.assign(dialogFormData, row);
  });
};
// 对话框取消函数
const handleCancel = () => {
  ctx.$refs.dialogForm.resetFields();
  showDialog.value = false;
};
// 对话框确认函数
const handleSubmit = () => {
  ctx.$refs.dialogForm.validate((valid) => {
    if (valid) {
      // 如果选择了父级菜单，则获取父级菜单的id，否则设置为-1（数据库设计-1表示顶级菜单，没有父级菜单）
      dialogFormData.parentId = !dialogFormData.parentId
        ? -1
        : dialogFormData.parentId.length > 0
        ? dialogFormData.parentId[0]
        : dialogFormData.parentId;
      if (dialogAction.value === 'save') {
        $api.saveMenu(dialogFormData).then((res) => {
          console.log(dialogFormData);
          res ? ElMessage.success('操作成功') : ElMessage.error('操作失败');
          selectMenuList();
        });
      } else if (dialogAction.value === 'edit') {
        $api.updateMenu(dialogFormData).then((res) => {
          res ? ElMessage.success('操作成功') : ElMessage.error('操作失败');
          selectMenuList();
        });
      }
      handleCancel();
    }
  });
};

//////////////////// 删除 ////////////////////
const handleDelete = (id) => {
  ElMessageBox.confirm('确定删除该菜单吗？', '提示', {
    confirmButtonText: '确定',
    cancelButtonText: '取消',
    type: 'warning',
  }).then(() => {
    $api.deleteMenu(id).then((res) => {
      ElMessage.warning('删除成功');
      selectMenuList();
    });
  });
};
</script>
```

### 3.角色管理页面

#### 新增所需接口

```js
import $request from './../utils/request.js';
export default {
  ......
  //////////////////// 角色管理页面 ////////////////////
  // 分页查询角色集合
  pageRoleList(params) {
    return $request.post('/roles/page', params);
  },
  // 新增角色
  saveRole(form) {
    return $request.post('/roles/save', form);
  },
  // 更新角色
  editRole(form) {
    return $request.post('/roles/update', form);
  },
  // 删除角色
  deleteRole(id) {
    return $request.delete('/roles/delete/' + id);
  },
  // 查询角色拥有的菜单以及所有的菜单信息
  getRoleMenu(id) {
    return $request.get('/roles/all/menus/' + id);
  },
  // 更新角色对应的菜单
  updateRoleMenu(params) {
    return $request.post('/roles/menus/update', params);
  },
};
```

src\api\api.js ▼

```js
import $request from './../utils/request.js';
export default {
  ......
  //////////////////// 角色管理页面 ////////////////////
  // 查询角色集合
  getRoleList() {
    return $request.get('/roles/list');
  },
  // 分页查询角色集合
  pageRoleList(params) {
    return $request.post('/roles/page', params);
  },
  // 新增角色
  saveRole(form) {
    return $request.post('/roles/save', form);
  },
  // 更新角色
  editRole(form) {
    return $request.post('/roles/update', form);
  },
  // 删除角色
  deleteRole(id) {
    return $request.delete('/roles/delete/' + id);
  },
  // 查询角色拥有的菜单以及所有的菜单信息
  getRoleMenu(id) {
    return $request.get('/roles/all/menus/' + id);
  },
  // 更新角色对应的菜单
  updateRoleMenu(params) {
    return $request.post('/roles/menus/update', params);
  },
};
```

#### 角色管理页面

src\views\Role.vue ▼

```vue
<template>
  <div class="user-manage">
    <!-- 查询条件部分 -->
    <div class="query-form">
      <el-form :inline="true" ref="queryForm" :model="queryParams">
        <el-form-item label="角色名称" prop="roleName">
          <el-input placeholder="请输入角色名称" v-model="queryParams.roleName" />
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="pageRoleList()">查询</el-button>
          <el-button type="info" @click="resetQueryForm()">重置</el-button>
        </el-form-item>
      </el-form>
    </div>
    <!-- 查询结果部分 -->
    <div class="base-table">
      <div class="action">
        <el-button type="primary" @click="handleSave()">新增</el-button>
      </div>
      <!-- El提供的表格 -->
      <div class="table-wrap">
        <el-table :data="roleList" border>
          <el-table-column
            v-for="item in columns"
            :key="item.label"
            :label="item.label"
            :prop="item.prop"
            :formatter="item.formatter" />
          <el-table-column fixed="right" label="操作" min-width="120">
            <template #default="scope">
              <el-button type="primary" @click="handleUpdate(scope.row.id)"> 分配权限 </el-button>
              <el-button type="warning" @click="handleEdit(scope.row)"> 编辑角色 </el-button>
              <el-button type="danger" @click="handleDelete(scope.row.id)"> 删除角色 </el-button>
            </template>
          </el-table-column>
        </el-table>
      </div>
      <!-- El提供的@current-change 点击换页事件 -->
      <div class="pagination">
        <el-pagination
          layout="prev,pager,next"
          :page-size="pageParams.pageSize"
          :total="pageParams.totalCount"
          @current-change="changePage">
        </el-pagination>
      </div>
    </div>
    <!-- 新增/编辑对话框 -->
    <div class="dialog">
      <el-dialog title="新增/编辑角色" v-model="showDialog">
        <el-form :model="dialogFormData" :rules="rules" ref="dialogForm">
          <el-form-item label="角色名称：" prop="roleName">
            <el-input v-model="dialogFormData.roleName" placeholder="请输入角色名称" />
          </el-form-item>
          <el-form-item label="角色备注：" prop="remark">
            <el-input
              type="textarea"
              v-model="dialogFormData.remark"
              placeholder="请输入角色备注" />
          </el-form-item>
        </el-form>
        <template #footer>
          <div class="dialog-footer">
            <el-button type="success" @click="handleSubmit()"> 确定 </el-button>
            <el-button type="info" @click="handleCancel()"> 取消 </el-button>
          </div>
        </template>
      </el-dialog>
    </div>
    <!-- 权限分配对话框 -->
    <div class="dialog">
      <el-dialog v-model="showDialog2" title="分配角色权限">
        <el-form ref="permissionForm">
          <el-form-item label="权限分配：">
            <el-tree
              ref="treeSelect"
              style="max-width: 600px"
              :data="dialogFormData2"
              default-expand-all
              show-checkbox
              node-key="id"
              :default-checked-keys="defaultCheckedKeys"
              :props="{ label: 'menuName', children: 'children' }" />
          </el-form-item>
        </el-form>
        <template #footer>
          <div class="dialog-footer">
            <el-button type="success" @click="handleSubmit2()"> 确定 </el-button>
            <el-button type="info" @click="handleCancel2()"> 取消 </el-button>
          </div>
        </template>
      </el-dialog>
    </div>
  </div>
</template>

<script setup>
import { ElMessage, ElMessageBox } from 'element-plus';
import { getCurrentInstance, inject, onMounted, reactive, ref } from 'vue';

const $api = inject('api');

const { ctx } = getCurrentInstance();

onMounted(() => {
  pageRoleList();
});

//////////////////// 分页查询 ////////////////////
let queryParams = reactive({
  roleName: '',
  currentPage: 1,
  pageSize: 3,
});
let pageParams = ref({
  totalCount: 0,
  pageSize: queryParams.pageSize,
});
// 重置表单函数
const resetQueryForm = () => {
  ctx.$refs.queryForm.resetFields();
  pageRoleList();
};
let roleList = reactive([]);
let columns = ref([
  { label: '角色ID', prop: 'id' },
  { label: '角色名称', prop: 'roleName' },
  { label: '角色备注', prop: 'remark' },
  { label: '创建时间', prop: 'creationDate' },
]);

const pageRoleList = () => {
  $api.pageRoleList(queryParams).then((res) => {
    // 方式一：追加数据（适用于分页加载，如页码递增时）
    // roleList.push(...res.currentPageRecords); // 使用展开运算符避免嵌套数组
    // 方式二：直接覆盖数据（适用于刷新或重置页码时）
    roleList.splice(0, roleList.length, ...res.currentPageRecords);
    pageParams.value.totalCount = res.totalCount;
    pageParams.value.pageSize = res.pageSize;
  });
};
const changePage = (pageNum) => {
  queryParams.currentPage = pageNum;
  pageRoleList();
};

//////////////////// 新增/编辑对话框 ////////////////////
let showDialog = ref(false);
let dialogAction = ref('');
let dialogFormData = reactive({});
// 前端表单校验规则
const rules = reactive({
  roleName: { required: true, message: '角色名称不能为空', trigger: 'blur' },
  remark: [
    { required: true, message: '角色备注不能为空', trigger: 'blur' },
    { max: 50, message: '备注信息不能超出50个字符', trigger: ['change', 'blur'] },
  ],
});
// 对话框唤起函数
const handleSave = () => {
  dialogAction.value = 'save';
  showDialog.value = true;
};
const handleEdit = (row) => {
  dialogAction.value = 'edit';
  showDialog.value = true;
  ctx.$nextTick(() => {
    Object.assign(dialogFormData, row);
  });
};
// 对话框取消函数
const handleCancel = () => {
  ctx.$refs.dialogForm.resetFields();
  showDialog.value = false;
};
// 对话框确认函数
const handleSubmit = () => {
  ctx.$refs.dialogForm.validate((valid) => {
    if (valid) {
      if (dialogAction.value === 'save') {
        $api.saveRole(dialogFormData).then((res) => {
          res ? ElMessage.success('操作成功') : ElMessage.error('操作失败');
          pageRoleList();
        });
      } else if (dialogAction.value === 'edit') {
        $api.editRole(dialogFormData).then((res) => {
          res ? ElMessage.success('操作成功') : ElMessage.error('操作失败');
          pageRoleList();
        });
      }
      handleCancel();
    }
  });
};

//////////////////// 分配权限对话框 //////////////////
let showDialog2 = ref(false);
let dialogFormData2 = ref({});
let defaultCheckedKeys = ref([]);
// 对话框唤起函数
const handleUpdate = (id) => {
  updateParams.value.roleId = id;
  $api.getRoleMenu(id).then((res) => {
    dialogFormData2.value = res.allMenus;
    defaultCheckedKeys.value = res.roleMenus.map((item) => item.id);
  });
  showDialog2.value = true;
};
// 对话框取消函数
const handleCancel2 = () => {
  showDialog2.value = false;
};
let updateParams = ref({
  roleId: '',
  menusId: [],
});
// 对话框确认函数
const handleSubmit2 = () => {
  // El-tree控件提供的getCheckedNodes(boolean1,boolean2)方法
  // boolean1:是否只返回被选中的叶子节点，boolean2:是否返回半选中的节点
  updateParams.value.menusId = ctx.$refs.treeSelect
    .getCheckedNodes(false, true)
    .map((item) => item.id);
  $api.updateRoleMenu(updateParams.value).then((res) => {
    res ? ElMessage.success('分配权限成功') : ElMessage.error('分配权限失败');
    pageRoleList();
  });
  handleCancel2();
};

//////////////////// 删除 ////////////////////
const handleDelete = (id) => {
  ElMessageBox.confirm('确定删除该菜单吗？', '提示', {
    cancelButtonText: '取消',
    confirmButtonText: '确定',
    type: 'warning',
  }).then(() => {
    $api.deleteRole(id).then((res) => {
      ElMessage.warning('删除成功');
      pageRoleList();
    });
  });
};
</script>
```

### 4.部门管理页面

#### 新增所需接口

src\api\api.js ▼

```js
import $request from './../utils/request.js';
export default {
  ......
  //////////////////// 部门管理页面 ////////////////////
  // 查询部门集合
  getDeptList(params) {
    return $request.post('/depts/list', params);
  },
  // 新增部门
  saveDept(form) {
    return $request.post('/depts/save', form);
  },
  // 查询所有用户
  getAllUser() {
    return $request.get('/users/all');
  },
  // 更新部门
  updateDept(form) {
    return $request.post('/depts/update', form);
  },
  // 删除部门
  deleteDept(id) {
    return $request.delete('/depts/delete/' + id);
  },
  // 更新部门
  editDept(form) {
    return $request.post('/depts/update', form);
  },
};
```

#### 用户管理页面

src\views\Dept.vue ▼

```vue
<template>
  <div class="user-mange">
    <!-- 条件查询部分 -->
    <div class="query-form">
      <el-form :inline="true" ref="queryForm" :model="queryParams">
        <el-form-item label="部门名称" prop="deptName">
          <el-input v-model="queryParams.deptName" placeholder="请输入部门名称" />
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="getDeptList()">查询</el-button>
          <el-button type="info" @click="resetForm()">重置</el-button>
        </el-form-item>
      </el-form>
    </div>

    <div class="base-table">
      <!-- 操作按钮部分 -->
      <div class="action">
        <el-button type="primary" @click="handleSave()">新增</el-button>
      </div>
      <!-- 遍历结果集部分  -->
      <div class="table-wrap">
        <el-table border :data="deptList">
          <el-table-column
            v-for="item in columns"
            :key="item.label"
            :label="item.label"
            :prop="item.prop"
            :formatter="item.formatter" />
          <el-table-column fixed="right" label="操作" min-width="120">
            <template #default="scope">
              <el-button type="warning" @click="handleEdit(scope.row)"> 编辑 </el-button>
              <el-button type="danger" @click="handleDelete(scope.row.id)"> 删除 </el-button>
            </template>
          </el-table-column>
        </el-table>
      </div>
    </div>
    <!-- 新增/编辑 对话框部分  -->
    <div class="dialog">
      <el-dialog title="新增/编辑部门" v-model="showDialog">
        <el-form :model="dialogFormData" :rules="rules" ref="dialogForm">
          <el-form-item label="部门名称：" prop="deptName">
            <el-input v-model="dialogFormData.deptName" placeholder="请输入部门名称" />
          </el-form-item>
          <el-form-item label="部门负责人：" prop="userId">
            <el-select v-model="dialogFormData.userId">
              <el-option :value="0" label="暂无负责人" />
              <el-option
                v-for="item in userMange"
                :key="item.id"
                :label="item.userName"
                :value="item.userId" />
            </el-select>
          </el-form-item>
        </el-form>
        <template #footer>
          <div class="dialog-footer">
            <el-button type="success" @click="handleSubmit()"> 确定 </el-button>
            <el-button type="info" @click="handleCancel()"> 取消 </el-button>
          </div>
        </template>
      </el-dialog>
    </div>
  </div>
</template>

<script setup>
import { ElMessage, ElMessageBox } from 'element-plus';
import { getCurrentInstance, inject, onMounted, reactive, ref } from 'vue';

const { ctx } = getCurrentInstance();

const $api = inject('api');

onMounted(() => {
  getDeptList();
});

//////////////////// 条件查询 ////////////////////
let queryParams = reactive({
  deptName: '',
});
let columns = [
  { label: '部门ID', prop: 'id' },
  { label: '部门名称', prop: 'deptName' },
  { label: '部门负责人', prop: 'userName' },
  { label: '负责人邮箱', prop: 'userEmail' },
];
let deptList = ref([]);
const getDeptList = () => {
  $api.getDeptList(queryParams).then((res) => {
    deptList.value = res;
  });
};
const resetForm = () => {
  ctx.$refs.queryForm.resetFields();
  getDeptList();
};

//////////////////// 新增/编辑对话框 ////////////////////
let showDialog = ref(false);
let dialogAction = ref('');
let dialogFormData = reactive({
  userId: 0,
});
// 前端表单校验规则
const rules = reactive({
  deptName: { required: true, message: '部门名称不能为空', trigger: 'blur' },
});
let userMange = ref([]);
// 对话框唤起函数
const handleSave = () => {
  dialogAction.value = 'save';
  showDialog.value = true;
  $api.getAllUser().then((res) => {
    userMange.value = res;
  });
};
const handleEdit = (row) => {
  dialogAction.value = 'edit';
  showDialog.value = true;
  ctx.$nextTick(() => {
    $api.getAllUser().then((res) => {
      userMange.value = res;
    });
    Object.assign(dialogFormData, row);
  });
};
// 对话框取消函数
const handleCancel = () => {
  ctx.$refs.dialogForm.resetFields();
  showDialog.value = false;
};
// 对话框确认函数
const handleSubmit = () => {
  ctx.$refs.dialogForm.validate((valid) => {
    if (valid) {
      if (dialogAction.value === 'save') {
        $api.saveDept(dialogFormData).then((res) => {
          res ? ElMessage.success('操作成功') : ElMessage.error('操作失败');
          getDeptList();
        });
      } else if (dialogAction.value === 'edit') {
        $api.editDept(dialogFormData).then((res) => {
          res ? ElMessage.success('操作成功') : ElMessage.error('操作失败');
          getDeptList();
        });
      }
      handleCancel();
    }
  });
};

//////////////////// 删除 ////////////////////
const handleDelete = (id) => {
  ElMessageBox.confirm('确定删除该部门吗？', '提示', {
    cancelButtonText: '取消',
    confirmButtonText: '确定',
    type: 'warning',
  }).then(() => {
    $api.deleteDept(id).then((res) => {
      ElMessage.warning('删除成功');
      getDeptList();
    });
  });
};
</script>
```

