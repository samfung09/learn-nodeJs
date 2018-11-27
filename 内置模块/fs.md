## 目录
* [文件写入](#user-content-文件写入)

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
    console.log(stats)    //返回文件状态对象
})
```
回调函数的第二个参数stats是文件状态信息对象，一般常用两个，判断是文件还是文件夹
>* stats.isFile() -- 是否文件
>* stats.isDirectory() -- 是否文件夹

## 读取文件内容
#### fs.readFile(path, {options}, callback)
该方法适合读取体积较小的文件，其中第二个参数`options可选，默认值为{encoding: null, flag: 'r'}`

如果未指定字符编码，则返回原始的 buffer，如果要想得到字符串还需调用toString方法进行装换
```javascript
fs.readFile('./2018/2/123.txt', (err, data) => {
    if(err) throw err;
    console.log(data);  //<Buffer 68 65 6c 6c 6f 20 77 6f 72 6c 64>
})
```
如果指定了字符编码
```javascript
fs.readFile('./2018/2/123.txt', 'utf8', (err, data) => {
    if(err) throw err;
    console.log(data);  //hello world
})
```

## 文件写入
#### fs.writeFile(file, data, {options}, callback)
该方法第三个参数`options可选，默认值为{encoding: 'utf8', mode: 0o666, flag: 'w'}`

如果文件不存在则会创建文件，注意`不能创建目录`，data可以是字符串或Buffer。
```javascript
fs.writeFile('./2018/2/123.txt', 'hello world', err => {
    if(err) throw err;
    console.log('写入成功');    // 此时文件内容：hello world
})
```
后面追加内容
```javascript
fs.writeFile('./2018/2/123.txt', 'hello world', {flag: 'a'}, err => {
    if(err) throw err;
    console.log('写入成功');    // 此时文件内容：hello worldhello world
})
```

## 追加文件内容
#### fs.appendFile(path, data, {options}, callback)
该方法第三个参数`options可选，默认值为{encoding: 'utf8', mode: 0o666, flag: 'a'}`

该方法跟fs.writeFile()设置{flag: 'a'}效果相同。如果文件不存在则会创建文件，注意`不能创建目录`，data可以是字符串或Buffer。
```javascript
fs.appendFile('./2018/2/123.txt', '这是appendFile', err => {
    if(err) throw err;
    console.log('内容追加成功')    // 此时文件内容：hello world这是appendFile
})
```

## 创建目录
#### fs.mkdir(path, {options}, callback)
该方法第二个参数`options可选，不过好像用处不大，默认就行`
```javascript
fs.mkdir('./2018/2/2333', err => {
    if(err) throw err;
    console.log('创建目录成功')
});
```
该方法只能创建一级目录不能创建多级目录，可自行递归创建多级目录[我csdn博客的文章](https://blog.csdn.net/samfung09/article/details/80906577)

## 读取目录下的文件
#### fs.readdir(path, {options}, callback)
该方法第二个参数`options可选，默认值为{encoding: 'utf8', withFileTypes: false}`
```javascript
fs.readdir('./2018/2', (err, files) => {
    if(err) throw err;
    console.log(files)    // [ '123.txt', '3.jpg' ]
})
```

## 删除文件
#### fs.unlink(path, callback)
```javascript
fs.unlink('./2018/2/bbb.txt', err => {
    if(err) throw err;
    console.log('文件删除成功')
})
```
与fs.stat()配合判断文件是否存在，存在则删除
```javascript
fs.stat('./2018/2/bbb.txt', (err, stats) => {
    if(err) throw err;
    if(stats.isFile()){
        fs.unlink('./2018/2/bbb.txt', err => {
            if(err) throw err;
            console.log('文件删除成功')
        })
    }
})
```
不过一般在项目中不会这样写，会用promise将每个方法封装起来

## 文件重命名
#### fs.rename(oldPath, newPath, callback)
把文件oldPath重命名为newPath。如果newPath已存在，则覆盖它。
```javascript
fs.rename('./2018/2/旧文件.txt', './2018/2/新文件.txt', err => {
    if(err) throw err;
    console.log('已完成重命名');
});
```
剪切文件
```javascript
fs.rename('./2018/2/旧文件.txt', './2018/2/33/新文件.txt', err => {
    if(err) throw err;
    console.log('文件剪切成功');    //此时./2018/2目录下已经没有 旧文件.txt，而被剪切到了./2018/2/33目录下，并且重名民为 新文件.txt
});
```

## 删除目录
#### fs.rmdir(path, callback)
```javascript
fs.rmdir('./2018/2/2333', err => {
    if(err) throw err;
    console.log('删除目录成功')
})
```