# Vue Cli

## 一、Cli相关命令

查看vue cli 版本
```bash
vue --version
```

下载vue cli 脚手架
```bash
npm install -g @vue/cli
```

创建vue 项目
```bash
vue create vue-s1
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
npm config set registry https://registry.npmjs.org/

npm config set registry https://registry.npmmirror.com
```

更新npm根证书
```bash
npm install -g npm@latest
```