### 简介

>[info]Nginx 是一个高性能的 Web 和反向代理服务器, 它具有有很多非常优越的特性:

>[info]作为 Web 服务器：相比 Apache，Nginx 使用更少的资源，支持更多的并发连接，体现更高的效率，这点使 Nginx 尤其受到虚拟主机提供商的欢迎。能够支持高达 50,000 个并发连接数的响应，感谢 Nginx 为我们选择了 epoll and kqueue 作为开发模型.

>[info]作为负载均衡服务器：Nginx 既可以在内部直接支持 Rails 和 PHP，也可以支持作为 HTTP代理服务器 对外进行服务。Nginx 用 C 编写, 不论是系统资源开销还是 CPU 使用效率都比 Perlbal 要好的多。

>[info]作为邮件代理服务器: Nginx 同时也是一个非常优秀的邮件代理服务器（最早开发这个产品的目的之一也是作为邮件代理服务器），Last.fm 描述了成功并且美妙的使用经验。

### 基本功能

* 处理静态文件，索引文件以及自动索引；
* 反向代理加速(无缓存)，简单的负载均衡和容错；
* FastCGI，简单的负载均衡和容错；
* 模块化的结构。过滤器包括gzipping, byte ranges, chunked responses, 以及 SSI-filter 。在SSI过滤器中，到同一个 proxy 或者 FastCGI 的多个子请求并发处理；
* SSL 和 TLS SNI 支持；

### 基本操作

#### 启动

```js
sudo /usr/local/nginx/nginx  //直接执行，即可启动
```

#### 重载

定位到ngnix的`sbin`目录下，执行如下的操作

```js
./ngnix -s reload
```

#### 从容停止，等所有请求结束后关闭服务

```js
ps -ef |grep nginx
kill -QUIT  nginx主进程号
```

#### 快速停止

```js
ps -ef |grep nginx

kill -TERM nginx主进程号 

```

#### 杀死进程

```js
kill -9 nginx主进程号
```

### 反向代理（跨域）

```js
http {
    server {
        listen	8000;
        server_name		www.domain.com;
        location /h1{
            index index.html;
            # 静态资源1服务端目录
            root  /var/www/domain.com/htdocs1;
        }
        # 日志地址
        access_log	logs/8000.access.log main;
    }
    server {
        listen	8002;
        server_name		www.domain.com;
        location /h2{
            index index.html;
            # 静态资源2服务端目录
            root  /var/www/domain.com/htdocs2;
        }
       	location /api {
            # 反向代理
            proxy_pass www.domain.com:8001 #已知的请求地址服务器
        }
        # 日志地址
         access_log	logs/8002.access.log main;
    }
}
```

访问链接`www.domain.com:8003/h2`即可成功访问部署在`www.domain.com:8002`上的页面，
页面自动重定向请求`www.domain.com:8001`上的API。

### 引用

*  [中文文档](http://www.nginx.cn/doc/)
*  [用nginx的反向代理机制解决前端跨域问题](https://www.cnblogs.com/gabrielchen/p/5066120.html)
