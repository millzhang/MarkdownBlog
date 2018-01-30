### 介绍

`pdf.js`可以实现在`html`下直接浏览pdf文档，是一款开源的`pdf`文档读取解析插件

`pdf.js`主要包含两个库文件，一个`pdf.js`和一个`pdf.worker.js`，，一个负责`API`解析，一个负责核心解析

* [下载地址](http://oritfw5nq.bkt.clouddn.com/pdf.zip)
* [github](https://github.com/rkusa/pdfjs)

### 使用步骤

将文件解压到`static`目录下,在预览页面中使用,使用`iframe`访问`static中web内的viewer.html`文件,`pdf`路径通过参数传递,即可使用该插件访问`pdf`文件

```html
   <iframe class="pdf-viewer" :src='"/static/pdf/web/viewer.html?file=http://image.cache.timepack.cn/nodejs.pdf"' width="50%" height="800" scrolling="no">
      您的浏览器不支持PDF阅读
    </iframe>
```

### 问题

`pdf`兼容`ie,firefox,chrome`等主流的浏览器,故浏览器兼容方面无需担心.然后主要注意的是:

1. 源码必须放在`static`目录下作为静态资源引入项目,不然会影响`webpack`编译;
2. 访问网络`pdf`文件存在跨域问题,目前暂时是这样配置:`Access-Control-Allow-Origin:*`


### 参考
1. https://www.cnblogs.com/jacksoft/p/5302587.html
2.  https://github.com/lewiscutey/PDF/tree/gh-pages
3.  https://github.com/rkusa/pdfjs
4.  http://blog.csdn.net/xiao_bin_shen/article/details/77778514
