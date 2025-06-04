### 1、下载mysql服务端

官网下载链接：https://dev.mysql.com/downloads/mysql/5.7.html

下载后，解压到D盘

新建一个`my.ini`配置文件 ▼

```ini
[mysql]
# 设置mysql客户端默认字符集
default-character-set=utf8 
[mysqld]
#设置3306端口
port = 3306 
# 设置mysql的安装目录
basedir="D:\MySQL\mysql-5.7.36-winx64"
# 设置mysql数据库的数据的存放目录
datadir="D:\MySQL\mysql-5.7.36-winx64\data"
# 允许最大连接数
max_connections=200
# 服务端使用的字符集默认为8比特编码的latin1字符集
character-set-server=utf8
# 创建新表时将使用的默认存储引擎
default-storage-engine=INNODB
```

### 2、MySql安装步骤

`window+R`启动命令窗口，输入`cmd`，打开`cmd`窗口，输入`d:`进入到D盘，然后`cd MySQL\mysql-5.7.36-winx64\bin`，到对应的bin目录，执行命令，执行之后会生成data这个文件夹

```shell
mysqld --initialize-insecure --user=mysql
```

mysql初始化之后，可以执行`install`命令：

```shell
mysqld -install
```

> TIP：如果不想到bin目录执行命令，就需要自行设置环境变量，添加参数到path，D:\MySQL\mysql-5.7.36-winx64\bin

mysql install命令执行之后，在我的电脑->管理->服务，是可以看到mysql服务的，可以用命令执行，也可以右键点击启动

```shell
net start mysql
```

启动mysql服务成功了

ok，mysql绿色版就安装成功了，然后密码？好像没有设置密码，可以在刚才生成的data文件夹里找到一个计算机名称命名的文件，比如`admin.err`，找到之后打开，可以找到答案，有些版本是生成一个随机密码，有些默认没密码



root管理员登录：注意，要设置环境变量，不然得到bin目录才能执行

```sql
mysql -u root -p
alter user 'root'@'localhost' IDENTIFIED with mysql_native_password by 'root';
```

用Navicat测试连接



### 3、遇到的问题

执行mysqld install，出现The service already exists提示，那是因为之前安装过，不过没卸载完全，所以需要命令执行一下

```bash
sc query mysql
```

删除mysql一些卸载残余

```
sc delete mysql
```

**无法启动此程序，因为计算机中丢失VCRUNTIME140.dll 尝试重新安装此程序以解决此问题**
 执行net start mysql时提示丢失VCRUNTIME140.dll ，需要安装Microsoft.Net.Framework 4.6.1和Visual C++ Redistributable for Visual Studio 2015
去微软官网下载Microsoft.Net.Framework 4.6.1
下载地址：https://www.microsoft.com/zh-CN/download/details.aspx?id=49981

去微软官网下载Visual C++ Redistributable for Visual Studio 2015
下载地址：https://www.microsoft.com/zh-cn/download/details.aspx?id=48145

**无法定位程序输入点fesetround于动态链接库MSVCR120.dll上**
 下载 Microsoft Visual C++ 2013 Redistributable Package 安装，链接：
http://download.microsoft.com/download/b/e/8/be8a5444-cdd8-4d3d-ae09-a0979b05aee3/vcredist_x64.exe，参考：https://blog.csdn.net/write6/article/details/79204755

**win7安装.net framework 4.6失败的解决方法**

参考：http://www.dnxtc.net/zixun/WIN7yingyong/2017-11-09/1842.html