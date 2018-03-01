
### 下载

官网下载需要墙，故使用镜像下载。

镜像地址：http://dl.mongodb.org/dl/win32/x86_64

![](images/screenshot_1514254241844.png)

下载`msi`文件，一路安装.

### 环境变量

在环境变量`path`中添加如下路径：
```js
G:\softs\mongo\bin
```
校验`mongod`命令，使用`cmd`输入`mongod -v`:

![](images/screenshot_1514254379686.png)

故，安装完毕。

### 配置

在根目录中，新建`data,logs,mongo.cong`文件

![](images/screenshot_1514254478267.png)

配置文件如下:

```
dbpath=G:\softs\mongo\data #数据库路径  
logpath=G:\softs\mongo\logs #日志输出文件路径  
logappend=true #错误日志采用追加模式  
journal=true #启用日志文件，默认启用  
quiet=false #这个选项可以过滤掉一些无用的日志信息，若需要调试使用请设置为false  
port=27017 #端口号 默认为27017  
```

### 启动服务

```js
mongod --dbpath g:\softs\mongo\bin
```

![](images/screenshot_1514254574491.png)

启动成功！

### 可视化工具

* 使用`mongovue`
* [Robomongo](https://robomongo.org/download)

<p class="over"></p>
### 下载

官网下载需要墙，故使用镜像下载。

镜像地址：http://dl.mongodb.org/dl/win32/x86_64

![](images/screenshot_1514254241844.png)

下载`msi`文件，一路安装.

### 环境变量

在环境变量`path`中添加如下路径：
```js
G:\softs\mongo\bin
```
校验`mongod`命令，使用`cmd`输入`mongod -v`:

![](images/screenshot_1514254379686.png)

故，安装完毕。

### 配置

在根目录中，新建`data,logs,mongo.cong`文件

![](images/screenshot_1514254478267.png)

配置文件如下:

```
dbpath=G:\softs\mongo\data #数据库路径  
logpath=G:\softs\mongo\logs #日志输出文件路径  
logappend=true #错误日志采用追加模式  
journal=true #启用日志文件，默认启用  
quiet=false #这个选项可以过滤掉一些无用的日志信息，若需要调试使用请设置为false  
port=27017 #端口号 默认为27017  
```

### 启动服务

```js
mongod --dbpath g:\softs\mongo\bin
```

![](images/screenshot_1514254574491.png)

启动成功！

### 可视化工具

* 使用`mongovue`
* [Robomongo](https://robomongo.org/download)
