### 设置环境变量

```
vim /etc/profile

source /etc/profile #生效
```

### 安装`node`

```
wget https://nodejs.org/dist/v4.5.0/node-v4.5.0-linux-x86.tar.gz
tar xvzf node-v4.5.0-linux-x86.tar.gz
```

### 安装`cnpm`

`npm install -g cnpm --registry=https://registry.npm.taobao.org`

### 解压

```js
tar -xzvf file.tar.gz
tar -xzvf file.tar.gz -C XXX
```