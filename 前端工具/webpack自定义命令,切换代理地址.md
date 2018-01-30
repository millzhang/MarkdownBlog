### 场景描述

有时候需要模拟线上环境，拉取线上环境数据定位问题（这种情况，尽量避免啦），所以我们在`proxyTable`中设置代理链接时，需要配置两个地址，分别为`test_url`和`online_url`;

>[info] 已知，通过`npm run dev`启动开发环境，`-- xx`可以向`process`主进程中传递参数


### 代码

```js
proxyTable: {
  '/api': {
    target: process.argv.includes('online') ? PROXY_URL.online : PROXY_URL.test,
    changeOrigin: true,
    logLevel: 'debug',
    pathRewrite: {
      '^/api': '/api'
    }
  }
}

```
