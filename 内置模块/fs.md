首先不用说第一步就是引入模块
```javascript
const fs = require('fs');
```

## 判断文件是否存在
#### fs.exists() `废弃的: 使用 fs.stat() or fs.access() 代替`
```javascript
fs.exists('./2018/2/123.txt', exists => {
    // 回调函数接收一个参数exists，布尔值
    console.log(exists)    //true或false
})
```
该方法被废弃原因主要有两点
>* Node.js的回调函数一般第一个参数是error
>* 在调用fs.exists()检查文件是否存在，与调用fs.open()之类的方法对这个文件进行操作，这两个操作之间的这段时间程序很可能会改变文件。因此应该推荐用户直接对文件进行操作，再添加个处理错误的事件就好了

#### fs.access()
```javascript
fs.access('./2018/2/123.txt', err => {
    if(err) throw err;
    console.log('123.txt文件存在');
});
```

## 读取文件状态
#### fs.stat() 也可用作判断文件是否存在
```javascript
fs.stat('./2018/2/123.txt', (err, stats) => {
    if(err) throw err;
    console.log(stats)    //返回文件的信息对象
})
```
回调函数的第二个参数stats是文件的信息对象，一般常用两个，判断是文件还是文件夹
>* stats.isFile() -- 是否文件
>* stats.isDirectory() -- 是否文件夹