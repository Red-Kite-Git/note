# Git

## 一、简介

[Git官网](https://git-scm.com/)

## 二、常用命令

### 查看版本

```bash
git --version
```

### 设置用户名与邮箱

```bash
git config --global user.name "用户名"
git config --global user.email "邮箱"
```

### 初始化仓库

```bash
git init
```

### 克隆远程仓库

```bash
git clone <repository_url>
```

### 查看修改状态

```bash
git status
```

### 提交更改

添加文件到暂存区

```bash
git add <file_name>        # 添加单个文件
git add .                  # 添加所有更改
```

提交到本地仓库

```bash
git commit -m "Your commit message"
```

### 同步远程仓库

设置远程仓库地址

```bash
git remote add origin <repository_url>
```

推送到远程仓库

```bash
git push -u origin <branch_name>
```

拉取远程更新

```bash
git pull origin <branch_name>
```

### 分支管理

创建分支

```bash
git branch <new_branch_name>
```

切换分支

```bash
git checkout <branch_name>
```

删除分支

```bash
git branch -d <branch_name>
```

合并分支

```bash
git checkout <target_branch>
git merge <source_branch>
```

### 查看日志和历史

```bash
git log
```

### 标签管理

创建标签

```bash
git tag <tag_name>
```


推送标签

```bash
git push origin <tag_name>
```

删除标签

```bash
git tag -d <tag_name>
git push origin :refs/tags/<tag_name>
```



## 三、VScode中集成的Git

原文链接：https://blog.csdn.net/lixin5456985/article/details/143929619

1. 初始化仓库
  1.1 新建本地 Git 仓库
  打开 VS Code 的工作目录。
  使用内置 Git 进行初始化：
  图形界面操作：
  点击左侧 Source Control 图标。
  点击 Initialize Repository 按钮。

2. 克隆远程仓库
  图形界面操作：
  点击左侧 Source Control 图标。
  点击三点菜单 (···)，选择 Clone Repository。
  输入远程仓库 URL。

3. 查看修改状态
  图形界面操作：
  在 Source Control 面板，查看当前工作区文件的状态：
  M 表示已修改文件。
  A 表示新添加文件。
  D 表示删除文件。

4. 提交更改
  3.1 添加文件到暂存区
  图形界面操作：
  在 Source Control 面板中，点击文件旁的 + 图标。

  3.2 提交到本地仓库
  图形界面操作：
  在 Source Control 面板顶部输入提交信息，点击对勾（✔）提交。

5. 同步远程仓库
  4.1 设置远程仓库地址
  命令行操作：
  git remote add origin <repository_url>
  4.2 推送到远程仓库
  图形界面操作：
  点击三点菜单 (···)，选择 Push。
  命令行操作：
  git push -u origin <branch_name>
  4.3 拉取远程更新
  图形界面操作：
  点击三点菜单 (···)，选择 Pull。
  命令行操作：
  git pull origin <branch_name>

6. 分支管理
  5.1 创建分支
  图形界面操作：
  点击底部状态栏的分支名称，选择 Create New Branch。
  命令行操作：
  git branch <new_branch_name>
  5.2 切换分支
  图形界面操作：
  点击底部状态栏的分支名称，选择目标分支。
  命令行操作：
  git checkout <branch_name>
  5.3 删除分支
  图形界面操作：
  点击三点菜单 (···)，选择 Delete Branch。
  命令行操作：
  git branch -d <branch_name>

7. 合并分支
  图形界面操作：
  切换到目标分支。
  点击三点菜单 (···)，选择 Merge Branch，然后选择要合并的分支。
  命令行操作：
  git checkout <target_branch>
  git merge <source_branch>

8. 解决冲突
  冲突检测：
  在合并或拉取操作后，冲突文件会显示在 Source Control 面板。
  解决方法：
  点击冲突文件，使用内置的冲突解决工具（选择保留的版本）。
  解决后重新暂存文件并提交。

9. 查看日志和历史
  图形界面操作：
  安装扩展插件如 GitLens，以可视化方式查看历史记录。
  命令行操作：
  git log





资料https://jrhar.blog.csdn.net/article/details/116703787?fromshare=blogdetail&sharetype=blogdetail&sharerId=116703787&sharerefer=PC&sharesource=yc5338835&sharefrom=from_link

