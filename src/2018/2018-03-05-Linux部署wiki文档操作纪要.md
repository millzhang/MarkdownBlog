### 项目简述

通过`gitbook`开源框架和github私有化部署Wiki文档

### 客户端部署

> http://zitiao.org/deploy/

### 服务端部署（基于`Linux`）

1. 下载`node`，通过命令 `wget http://nodejs.org/dist/v8.9.3/node-v8.9.3-linux-x64.tar.gz`
2. 解压文件, `tar -zxvf node-v8.9.3-linux-x64.tar.gz`
3. 修改文件夹名称 `mv node-v8.9.3-linux-x64.tar.gz node`
4. 测试是否安装成功
```
cd node/bin && ls
./node -v
```
5. 添加环境变量：

首先，在`root`目录中找到`.bash_profile`文件，没有则创建，编辑该文件，添加如下语句

```
PATH=$PATH:$HOME/bin:/opt/node/bin
```
修改完成后，`source ~/.bash_profile`，使文件失效。
然后在任何目录下可执行`node -v 或者npm -v`，检测是否可执行。

[参考文档](https://segmentfault.com/a/1190000008479977)

### 安装`git`

```
yum install git-core 或者
apt-get install git
```

使用`git --version`检测是否安装成功。

安装完成后可使用`git clone`下载`http`服务下的代码（非ssh,ssh需要认证）。

### 顺便一提

关于`npm`切换代理，使用如下命令

```
npm config set registry=https://registry.npm.taobao.org
```

### 对`node`服务使用`pm2`进行进程管理

> pm2 是一个带有负载均衡功能的Node应用的进程管理器。当你要把你的独立代码利用全部的服务器上的所有CPU,并保证进程永远都活着,0秒的重载, PM2是完美的。

```python
# 全局安装
npm install pm2 --g
```

新建`pm2.json`文件：

```json
{
    "apps": [
        {
            "name": "cloudwalk-wiki",
            "script": "./app.js",
            "cwd": "./",
            "max_memory_restart": "1G",
            "autorestart": true,
            "node_args": [],
            "args": [],
            "env": {}
        }
    ]
}
```

新建`app.js`：

```js
//使用child_process模块，执行命令行
var exec = require('child_process').exec;
//运行`gitbook serve`启动服务器
exec('gitbook serve', function (error, stdout, stderr) {
    // 获取命令执行的输出
});
```

运行：

```
pm2 start pm2.json
```

#### 更换端口

```
gitbook serve --port xxxx
```

#### 更多参考

1. [pm2](https://github.com/Unitech/pm2)
2. [gitbook入门详解](https://yq.aliyun.com/articles/64766?spm=5176.10695662.1996646101.searchclickresult.9b9c33f0go92vE)
