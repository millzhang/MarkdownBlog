### `vue`中，如何全局使用`scss`的变量和方法

* [文档]（https://vue-loader.vuejs.org/en/configurations/pre-processors.html）

1. 安装`sass-resources-loader`;
2. 在`build/utils`中加入如下代码

```js
    scss: generateLoaders('sass').concat({
      loader: 'sass-resources-loader',
      options: {
        resources: path.resolve(__dirname, '../src/assets/styles/_variables.scss')
      }
    }),
```


* * * * *

### `vue`中`import jsweixin-sdk`报错

错误：

> `Cannot read property 'title' of undefined`

原因:
这个问题的原因是，里面在执行`this.document.title`的时候出的问题，这个`js`期望实在浏览器全局作用域下执行（`this`指向`window`，但是webpack之后，是在一个`function`作用域下执行，因此`this.document`为`undefined`。

解决方案如下:

* 在`html`中使用`script`引入
* `webpack`有个`script-loader`可以让模块文件在global环境下执行
* 改源码，将`jweixin-1.2.0.js`中第一个`this`改为`window`


* * * * *

### `webpack`使用`ip`访问

> 在 npm run dev 时添加参数 --host 0.0.0.0即可。


* * * * *

### `pro.env.js`中设置动态参数

```js
let version = process.env.npm_package_version.toString();
module.exports = {
  NODE_ENV: '"production"',
  VERSION: JSON.stringify(version)//这里需要使用josn.stringify转为字符串，不然会报错
}

```