# Vue Cli

官网地址 https://router.vuejs.org/zh/index.html

## 一、单文件组件

Vue 单文件组件即为.vue后缀文件,缩写为SFC是 一种特殊的文件格式, 它允许将Vue组件的模板样式与逻辑等封装到单个文件中

在Vue文件中编写的代码，通过Vue CLI脚手架将代码编译html文件然后在浏览器中运行

### 使用Vue CLI脚手架工具创建Vue项目

 (1) 安装Vue CLI 脚手架工具  npm  install  -g  @vue/cli  5.0.8 

(2) 使用脚手架创建Vue项目 vue  create  项目名称 

(3) 启动Vue项目 cd  项目路径 npm  run   serve

## 二、相关命令

查看vue cli 版本

```bash
vue --version
```

安装vue cli 脚手架

```bash
npm install -g @vue/cli
```

创建vue项目

```bash
vue create 项目名称
```

安装插件

```bash
npm install XX --参数
```

启动项目

```bash
npm run serve
```

禁用/启用 SSL 验证

```bash
npm config set strict-ssl false

npm config set strict-ssl true
```

更换 npm 注册表

```bash
npm config set registry https://registry.npmjs.org/		（官方）
npm config set registry https://registry.npmmirror.com	（国内推荐）
```

更新npm根证书

```bash
npm install -g npm@latest
```

## 