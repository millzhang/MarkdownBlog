### 下载

这里我们使用镜像下载,[镜像地址](http://mirrors.sohu.com/mysql/).

搜索下载`msi`文件,亲测`zip`文件下没有`ini`文件,需要手动新建.

### 环境变量设置

```
//环境变量配置到bin目录
C:\Program Files\MySQL\MySQL Server 5.7\bin
```

### 默认文件配置

在`C:\Program Files\MySQL\MySQL Server 5.7`目录下,有一个`my-default.ini`,打开配置参数:

```js
 basedir = C:\Program Files\MySQL\MySQL Server 5.7
 datadir = C:\Program Files\MySQL\MySQL Server 5.7\data //若无,不需要手动创建,下面会有操作
 port = 3306
 character-set-server=utf8
 default-storage-engine=INNODB 
```

### 安装`mysqld`

**这里必需要使用管理员身份来运行cmd**

在`bin`目录下运行`mysqld install`,

运行成功提示`service successfully installed`
如没有使用管理员运行,则提示`Install/Remove of the Service Denied!`;


### 启动/关闭服务

同样是在`bin`目录下,运行`net start mysql`,如要关闭服务则运行`net stop mysql`

### 服务无法启动

如果启动过程中,遇到:

> MySQL服务无法启动    服务没有报告任何错误    请键入NET HELPMSG 3534 以获得更多帮助

是由于`data`目录的缺失,这里我们还需要运行`mysqld --initialize-insecure --user=mysql`,运行完成后`MySQL`会自动新建一个`data`文件夹,并且建好默认的数据库,登录用户名为`root`,密码为空.
然后使用`net start mysql`就可以启动`mysql`服务了.

<p class="over">Over!</p>