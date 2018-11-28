## 目录
* [路径组合](#user-content-路径组合)
* [路径解析](#user-content-路径解析)
* [获取相对路径](#user-content-获取相对路径)

&nbsp;

首先不用说第一步先引入模块
```javascript
const path = require('path');
```
另外要注意的是，`当前系统的路径分隔符，Unix系统是"/"，Windows系统是"\"`

## 路径组合
#### path.join([...paths])
```javascript
path.join(__dirname, 'aaa')
path.join(__dirname, './aaa')
path.join(__dirname, '/aaa')
// 上面三个均返回 c:\Users\Administrator\Desktop\aaa
path.join(__dirname, '../aaa')
// 返回 c:\Users\Administrator\aaa
path.join(__dirname, '../aaa', 'bbb')
// 返回 c:\Users\Administrator\aaa\bbb
```
#### path.resolve([...paths])
`该方法大体上跟path.join()差不多，最大的区别是，当第二或之后的参数路径是个绝对路径那么函数就返回该绝对路径`
```javascript
path.resolve(__dirname, 'aaa')
path.resolve(__dirname, './aaa')
// 上面两个个均返回 c:\Users\Administrator\Desktop\aaa
path.resolve(__dirname, '/aaa')
// 返回 c:\aaa
path.resolve(__dirname, '../aaa')
// 返回 c:\Users\Administrator\aaa
path.resolve(__dirname, '../aaa', 'bbb')
// 返回 c:\Users\Administrator\aaa\bbb
```

## 路径解析
#### path.parse(path)
```javascript
let filepath = path.join(__dirname, '2018/2/3.jpg');
path.parse(filepath)
// 返回对象
{ 
    root: 'c:\\',
    dir: 'c:\\Users\\Administrator\\Desktop\\2018\\2',  // 上级目录
    base: '3.jpg',      // 路径最后一部分
    ext: '.jpg',    // 文件后缀
    name: '3'       // 文件名
}
```
#### path.basename(path, ext) 获取路径最后一部分
该方法`第二个参数ext为文件扩展名，可选`
```javascript
path.basename('/foo/bar/baz/asdf/quux.html');
// 返回: 'quux.html'
path.basename('/foo/bar/baz/asdf/quux.html', '.html');
// 返回: 'quux'
```
#### path.dirname(path) 获取路径的目录名
```javascript
path.dirname('/foo/bar/baz/asdf/quux');
// 返回: '/foo/bar/baz/asdf'
```
#### path.extname(path) 获取后缀名
返回路径最后的.及以后的字符
```javascript
path.extname('index.html');
// 返回: '.html'
path.extname('index.coffee.md');
// 返回: '.md'
path.extname('index.');
// 返回: '.'
path.extname('index');
// 返回: ''
path.extname('.index');
// 返回: ''
```

## 获取相对路径
#### path.relative(from, to)
`第一个参数是参照物路径（一般是当前路径），第二个参数是要知道的路径`
```javascript
path.relative('/data/orandea/test/aaa', '/data/orandea/impl/bbb');
// 返回: '../../impl/bbb'
```