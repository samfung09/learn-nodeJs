## 目录
* [构建与转换](#user-content-构建与转换)
* [Buffer的拼接](#user-content-Buffer的拼接)

&nbsp;

Buffer属于固有类型，因此无需引入。

`由于纯二进制格式太长且难以阅读，所以Buffer通常表现为十六进制的字符串。`
## 构建与转换
#### new Buffer() `不推荐: 使用 Buffer.from() 代替`
```javascript
// 根据一个二进制数组创建
let buffer1 = new Buffer([0x48, 0x65, 0x6c, 0x6c, 0x6f, 0x20, 0x4e, 0x6f, 0x64, 0x65])
console.log(buffer1)     //<Buffer 48 65 6c 6c 6f 20 4e 6f 64 65>

// 根据字符串创建
let buffer2 = new Buffer('Hello Node')
console.log(buffer2)     //<Buffer 48 65 6c 6c 6f 20 4e 6f 64 65>
```
#### Buffer.from() `推荐`
```javascript
// 根据一个二进制数组创建
let buffer1 = Buffer.from([0x48, 0x65, 0x6c, 0x6c, 0x6f, 0x20, 0x4e, 0x6f, 0x64, 0x65])
console.log(buffer1)     //<Buffer 48 65 6c 6c 6f 20 4e 6f 64 65>

// 根据字符串创建
let buffer2 = Buffer.from('Hello Node')
console.log(buffer2)     //<Buffer 48 65 6c 6c 6f 20 4e 6f 64 65>
```
#### 用toString(encoding, start, end)将Buffer对象转换成字符串
`如果调用toString时不传参，会默认采用utf-8编码转换整个Buffer对象`
```javascript
let buffer1 = Buffer.from([0x48, 0x65, 0x6c, 0x6c, 0x6f, 0x20, 0x4e, 0x6f, 0x64, 0x65])
console.log(buffer1.toString())     //Hello Node
console.log(buffer1.toString('utf8', 0, 5))     //Hello
```
Buffer支持的编码类型有限，只有以下6种，不过也已经覆盖了最常用的编码类型
>* ASCII
>* Base64
>* Binary
>* Hex
>* UTF-8
>* UTF-16LE/UCS-2

## Buffer的拼接
#### 先来一个不推荐的写法
```javascript
// 创建一个读取流
let reader = fs.createReadStream('a.txt');
let data = '';
reader.on('data', chunk => {
    data += chunk;
})
reader.on('end', () => {
    console.log(data);
})
```
上面代码之所以不被推荐是因为可能会出现乱码。大家知道数据流读取每次默认是64KB，而在utf-8中占每个中文字符占3个字节，且data += chunk相当于data += chunk.toString()，所以流每读取一段就有可能造成中文字符被截断，因此出现乱码情况。
#### Buffer.concat() `推荐`
```javascript
// 创建一个读取流
let reader = fs.createReadStream('a.txt');
let data = [];
reader.on('data', chunk => {
    data.push(chunk);
})
reader.on('end', () => {
    let buf = Buffer.concat(data);
    console.log(buf.toString());
})
```