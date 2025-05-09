# Vue Cli

官网地址 https://router.vuejs.org/zh/index.html

## 一、单文件组件

### 1.vue文件介绍

单文件组件即为.vue后缀文件,缩写为SFC是 一种特殊的文件格式, 它允许将Vue组件的模板样式与逻辑等封装到单个文件中

在Vue文件中编写的代码，通过Vue CLI脚手架将代码编译html文件然后在浏览器中运行



###  2.创建vue项目

安装Vue CLI 脚手架工具（全局）

```bash
npm install -g @vue/cli 5.0.8
```

使用脚手架创建Vue项目（在项目准备存放的文件夹）

```bash
cd 项目准备存放的文件夹
vue create 项目名称 
```

启动Vue项目

```bash
cd 项目路径
npm run 项目名
```



### 3.vue文件组成特性

* **模板（Template）**：定义组件的 HTML 结构，可以使用 Vue 的模板语法。

```vue
<template>
  <!-- 组件的HTML结构 -->
  <div>
  </div>
</template>
```

* **脚本（Script）**：组件的 JavaScript 逻辑，包括数据、方法、生命周期函数等，不同逻辑之间用`，`隔开

```vue
<script>
// 导出组件
export default {
  // 组件的逻辑部分
  data() {
    return {
        数据...,
    }
  },
  // 定义组件用到的函数方法
  methods: {
      
  }，
  // 在特定的生命周期执行的操作
  生命周期函数（）{
    
  },
  // 其他逻辑
  ......
}
</script>
```

* **样式（Style）**：组件的 CSS 样式，可以使用`scoped`属性限制样式作用域。

```vue
<style scoped>
/* 组件的样式，scoped表示只作用于当前组件 */
</style>
```



## 二、Cli相关命令

### 1.查看vue cli 版本

```bash
vue --version
```

### 2.安装vue cli 脚手架

```bash
npm install -g @vue/cli
```

### 3.创建vue项目

```bash
vue create 项目名称
```

### 4.安装插件

```bash
npm install XX --参数
```

>#### 参数：
>
>#### 依赖类型相关
>
>* `--save`（缩写为 `-S`）：把包添加到 `package.json` 文件里的 `dependencies` 字段，表明该包是项目运行时所需的依赖。例如 `npm install axios --save` 会将 `axios` 作为项目运行依赖安装。在 npm 5 之后，此参数为默认行为。
>* `--save-dev`（缩写为 `-D`）：将包添加到 `package.json` 文件中的 `devDependencies` 字段，意味着这个包仅在开发和测试阶段使用，像 `npm install jest --save-dev` 会把 `jest` 作为开发依赖安装。
>* `--save-optional`（缩写为 `-O`）：把包添加到 `package.json` 文件的 `optionalDependencies` 字段，这表示该包是可选的，即便安装失败项目也能继续运行。
>
>#### 安装行为相关
>
>* `--force`（缩写为 `-f`）：强制重新下载并安装所有依赖，即便本地已有相同版本的包。比如项目依赖存在冲突时，可使用 `npm install --force` 来重新安装所有依赖。
>* `--no-package-lock`：在安装包时不生成或更新 `package-lock.json` 文件。`package-lock.json` 可锁定依赖的版本，确保在不同环境中安装相同版本的依赖。
>* `--production`：仅安装 `package.json` 文件中 `dependencies` 字段里的包，而不安装 `devDependencies` 字段中的包，通常用于生产环境部署。
>
>#### 其他参数
>
>* `--global`（缩写为 `-g`）：全局安装包，将包安装到系统的全局目录，而非项目的 `node_modules` 目录。全局安装的包可在命令行中直接使用，例如 `npm install http-server -g` 可全局安装 `http-server`。
>* `--verbose`：显示详细的安装信息，有助于调试安装过程中出现的问题。
>* `--dry-run`：模拟安装过程，展示会被安装的包，但并不实际安装，可用于预先查看安装操作的结果。

### 5.启动项目

```bash
npm run serve
```

### 6.禁用/启用 SSL 验证

```bash
npm config set strict-ssl false

npm config set strict-ssl true
```

### 7.更换 npm 注册表

```bash
npm config set registry https://registry.npmjs.org/		（官方）
npm config set registry https://registry.npmmirror.com	（国内推荐）
```

### 8.更新npm根证书

```bash
npm install -g npm@latest
```

## 