### stack & heap

当我们调用一个函数，它的地址、参数、局部变量都会压入到一个 `stack` 中，当函数执行完毕后，本地变量会从`stack`中弹出，但是只针对`numbers string boolean`这些基本数据类型。而对象、数组的值是存在`heap堆`中的，
`stack`只存放了他们对于的指针。当函数之行结束从 `stack` 中弹出来时，只有对象的指针被弹出，而真正的值依然存在 `heap` 中，然后由垃圾回收器自动的清理回收。[引用](https://github.com/ccforward/cc/issues/47)

### macrotack & microtasks

* macrotasks: setTimeout setInterval setImmediate I/O UI渲染
* microtasks: Promise process.nextTick Object.observe MutationObserver

#### 从规范中理解

- 一个事件循环(event loop)会有一个或多个任务队列(task queue) task queue 就是 macrotask queue
- 每一个 event loop 都有一个 microtask queue
- task queue == macrotask queue != microtask queue
- 一个任务 task 可以放入 macrotask queue 也可以放入 microtask queue 中
- 当一个 task 被放入队列 queue(macro或micro) 那这个 task 就可以被立即执行了