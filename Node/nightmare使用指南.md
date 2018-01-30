### 关于

nightmare是基于`electron`的一个自动化测试的套件,内置了浏览器,当然也可用于爬虫.我也分别对这两个功能进行了简单的测试.后续会深入研究.

[github地址](https://github.com/segmentio/nightmare)

### 简单操作,编写一个自动操作的demo

#### 安装

```js
npm i nightmare -S
```

#### ES6支持

1. 安装babel
```js
//安装三个babel套件
//分别是es6基础编译包,扩展支持包,运行环境包
npm i babel-register  babel-polyfill babel-preset-env -D
```

2. 创建`.baberc`文件

```
{
    "presets": ["env"]
}
```

3. 创建入口文件`index.js`

```js
//依赖babel
require("babel-register");
require("babel-polyfill");

require('测试demo的入口js文件')
```

#### 实现自动化浏览

以`http://demo.timepack.cn/web`为例:

1.  登录

```js
const login = async() => {
    console.log('开始登录...', '>>>>>>>>')
    await nm.goto('http://demo.timepack.cn/web');
    await nm.click('.tab :nth-child(7)');
    await nm.wait('#inputForm');
    //输入用户名和密码
    await nm.type('#account', 'xxxxx');
    await nm.type('#password', 'xxxx');
    //获取验证码
    const code = await nm.evaluate(() => {
        return document.querySelector('#v-code').innerText;
    })
    console.log(`code:${code}`)
    await nm.type('#code', code);
    //点击登录
    await nm.click('.pw-loginBtn');
    console.log('登录成功...', '>>>>>>>>');
}

```

2. 新建一个表单

```js
/**
 * 新建活动
 * @return {[type]} [description]
 */
const newActivity = async() => {
    console.log('开始创建新的活动...', '>>>>>>>>');
    await nm.wait(3000);
    await nm.click('#activity_id');
    await nm.wait('#activityList');
    await nm.click('.title>.button');
    await nm.wait('#inputForm');
    await nm.type('#title', `${title}标题${new Date().getTime()}`);
    await nm.type('#content', `${title}内容${new Date().getTime()}`);
    await nm.type('#start', `2017-01-01`);
    await nm.type('#end', `2017-12-31`);
    await nm.type('#upperLimit', 188);
    await nm.click('.personal-edit>a:first-child');
    await nm.wait('#activityList');
    console.log('创建活动完成...', '>>>>>>>>');
}

```

3. 退出

```js
const close = async() => {
    console.log('测试完成...', '>>>>>>');
    await nm.end();
}

```

4. 登录的超时和多次尝试操作

	首先写个通用的方法:

```js
//多次执行函数方法
const runTimes = async(fn, times, timeout) => {
    for (let i = 0; i < times; i++) {
        try {
            return await outTime(fn, timeout);
        } catch (e) {
            logger.warn(e.message);
        }
    }
}
//登录限时
const outTime = (fn, timeout) => {
    return Promise.race([
        fn(),
        new Promise((resolve, reject) => {
            setTimeout(() => {
                reject(new Error('操作超时了!!!'));
            }, timeout);
        })
    ])
}

export default {runTimes}
```

5. 运行方法

```js
const run = async() => {
    await helper.runTimes(login, 3, 10000);
    await newActivity();
    await close();
}

run();

```
```js
//命令行运行则开始操作
node index.js
```

以上,Over!

* * * * *


### 爬虫使用

现在我们使用`nightmare`对百度图片进行爬虫操作.

首先,我们还需要`log4js`进行日志的记录(关于`log4js`强大的功能需要单独研究,这里我们简单的使用下吧!).

#### 包导入

```js
const Nightmare = require('nightmare')
const log4js = require('log4js');
const logger = log4js.getLogger();
logger.level = 'debug';
```

#### 配置文件

我们新建一个`config.js`,对我们需要的参数进行一些简单的预设.首先我们的目标是`website`,搜索关键词是`keywords`,这里指的是百度图片输入框内的内容,然后图片下载的主路径是`diskPath`.
关于`scroll`,预期我们发现百度图片采用了图片滚动懒加载,故我们需要模拟滚动条的动作,从而方便一次性获取更多的图片.
    
```js
export default {
    website: 'http://image.baidu.com/',
    diskPath: 'G:/images/',
    keywords: '女明星',
    scroll: {
        start: 0,//滚动起始位置
        step: 1024,//每次滚动的高度
        times: 100,//滚动次数
        interval: 100//滚动时间间隔
    }
}
```

#### 图片下载

图片下载我们采用异步批量下载的方式,同时使用到了`bagpipe`插件,用来控制并发的数量.

```js
//下载图片
const download = (target, list) => {
    if (!fs.existsSync(target)) {
        logger.info('创建新的目录!');
        fs.mkdirSync(target);
    }
    list.forEach((item, index) => {
        var destImage = path.resolve(target, uuidV4() + '.jpg');//uuid用来重命名图片
        bagpipe.push(saveImageFlie, item, destImage, function(err, data) {});
    });
}

const saveImageFlie = (src, dest, callback) => {
    request.head(src, function(err, res, body) {
        if (src) {
            request(src).pipe(fs.createWriteStream(dest)).on('close', function() {
                logger.info(`第${index++}张图片下载完成`);
                callback(null, dest);
            });
        }

    });
};

```
#### 分析dom结构,获取图片列表

```js
const getImageList = async() => {
    return await nm.evaluate(() => {
        let elements = document.querySelectorAll('.main_img');
        let result = [];
        for (var i = 0; i < elements.length; ++i) {
            var item = elements[i];
            result.push(item.getAttribute('data-imgurl'));
        }
        return result;
    });
}

```

#### 屏幕延时自动滚动

```js
//主要是图片有个加载时间,所以最好限定个延时
const autoSroll = async() => {
    logger.debug(`开始滚动屏幕`);
    for (let i = 0; i < config.scroll.times; i++) {
        let newPos = config.scroll.start + config.scroll.step * i;
        await outTime(newPos, config.scroll.interval);
    }
}

const outTime = (pos, timeout = 500) => {
    return Promise.all([
        nm.scrollTo(pos, 0),
        new Promise((resolve, reject) => {
            setTimeout(() => {
                resolve();
            }, timeout);
        })
    ])
}

```

* 完整代码参考[github](https://github.com/MillZhang/Node/blob/master/crawler/pic.js)

#### 说明

这里获取的都是缩略图,需要获取更高清的图片,我们还需要通过图片点击进去,后续再研究.

