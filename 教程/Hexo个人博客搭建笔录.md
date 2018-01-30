### 参考

* [hexo官方文档](https://hexo.io/zh-cn/docs/)
* [博客](http://baixin.io/2015/08/HEXO%E6%90%AD%E5%BB%BA%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/)

### 环境装备

1. nodejs(最后大于6.0);
2. `hexo`,通过`npm install hexo -g`安装;
3. github仓库(username.github.io)

<!-- more -->

### 常用命令

* `hexo init`创建文件夹;
* `hexo clean`清除文件;
* `hexo g/hexo generate`生成静态文件;
* `hexo d/hexo deploy`远程发布 ;
* `hexo server`启动本地服务

### hexo与github仓库的关联提交

在博客根目录下的`_config.yml`的文件中,修改如下代码:

```
deploy:
    type: git
    repo: https://github.com/MillZhang/millzhang.github.io.git #远程git仓库
    branch: master
```

### 主题Themes

```
git clone https://github.com/litten/hexo-theme-yilia.git themes/yilia
```

该主题需安装插件,生成所有文章的目录

```js
//仅支持node>6.0
npm i hexo-generator-json-content --save
```

安装完成后需要在根目录下的`_config.yml`中添加如下的配置:

```
# 所有文章目录
jsonContent:
    meta: false
    pages: false
    posts:
      title: true
      date: true
      path: true
      text: false
      raw: false
      content: false
      slug: false
      updated: false
      comments: false
      link: false
      permalink: false
      excerpt: false
      categories: false
      tags: true
```


### 一些小技巧

* 让文章只显示一部分,只需要在文章中添加`<!--more-->`,`<!--more-->`后面的内容就不显示了;
* 给文章分类(tags),需要在稳重添加`tags:[node,javascript]`

### 插件

1.使用git同步到git仓库

```
npm install hexo-deployer-git --save
```

2.生成易于搜索引擎搜素的网站地图

```
npm install hexo-generator-sitemap --save
```

3.生成rss订阅文件

```
npm install hexo-generator-feed --save
```

### hexo免密提交

1.输入指令，进入.ssh文件夹
```
cd ~/.ssh/
```
如无文件夹,可手动创建`.ssh`文件夹

2.配置全局的name和email，这里是的你github或者bitbucket的name和email

```
git config --global user.name "millzhang"  
  
git config --global user.email "876753183@qq.com"  
```

3.生成key

```
ssh-keygen -t rsa -C "876753183@qq.com" 
```

连续按三次回车，这里设置的密码就为空了，并且创建了key。

4.拷贝`id_rsa.pub`文件中的`key`,在`millzhang.github.io`的`Settings`中找到`Deploy Key`粘贴.

5.在博客根目录下的`_config.yml`中修改

```
deploy:
    type: git
    repo: git@github.com:MillZhang/millzhang.github.io.git #修改
    branch: master

```

6.使用`hexo d`就可以免密提交啦!

### 个人域名的绑定

1.在腾讯云申请域名`zy1991.cn`,进行实名认证.
2.在腾讯云进行域名解析,如下图操作:
![操作示意图](http://oritfw5nq.bkt.clouddn.com/QQ%E6%88%AA%E5%9B%BE20170615141211.png)
3.进入博客根目录下的`source`文件夹,新建`CNAME`,`CNAME`中输入域名`zy1991.cn`;
4.`hexo clean & hexo g & hexo d`即可访问自定义域名`zy1991.cn`下的博客啦;

[注]: [检测工具](https://www.qcloud.com/product/tools?from=cns) 可查看域名的解析状态

### 网易云跟帖

这部分比较简答,注册网易云跟帖账号,按步骤绑定项目,直接拷贝代码至主题下的`post.ejs`

```html
<div class="comment">
    <div id="cloud-tie-wrapper" class="cloud-tie-wrapper"></div>
</div>
```

```js
<!-- 网易云跟帖-->
<script src="https://img1.cache.netease.com/f2e/tie/yun/sdk/loader.js"></script>
<script>
var cloudTieConfig = {
  url: document.location.href, 
  sourceId: "",
  productKey: "40a8ee22b6084862a8907e6902e525fa",
  target: "cloud-tie-wrapper"
};
var yunManualLoad = true;
Tie.loader("aHR0cHM6Ly9hcGkuZ2VudGllLjE2My5jb20vcGMvbGl2ZXNjcmlwdC5odG1s", true);
</script>
```

### 网易云音乐

这部分做了些自定义,目的是想让每篇文章可控制不同的音乐,并可限制是否自动播放,功能比较简单,直接上代码,同样是在`post.ejs`中:

```
<!-- 网易云音乐-->
<% var musicId=page.music ? page.music : config.defaultMusic,
       isAuto = page.musicAuto ? page.musicAuto : config.defaultMusicAuto;
    if(musicId == undefined){ musicId = 586299};
    var musicLink = '//music.163.com/outchain/player?type=2&id='+musicId+'&auto='+Number(isAuto)+'&height=66';
 %>
<iframe class="cloud-music" frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 
    src="<%= musicLink %>">
</iframe>
```

在`_config.yml`中添加了如下的配置:

```
defaultMusic: 586299 //网易云音乐外链播放id
defaultMusicAuto: false //默认不开启自动播放
```
