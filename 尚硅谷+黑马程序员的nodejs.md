### Nodejs

Nodejs: https://nodejs.org/api/

Express: https://expressjs.com/

《深入浅出Node.js》

#### 尚硅谷视频内容

##### 基础补充

* 命令行
  * dir, cd, md, rm
  * .
  * ..
  * 环境变量
    * PATH
    * 打开或执行文件时，会首先根据当前路径进行寻找；如果没找到会去PATH里的路径下依次寻找；如果还没有找到则会报错
* 进程和线程（操作系统）：进程是具有一定独立功能的程序关于某个数据集合上的一次运行活动，进程是系统进行**资源分配和调度的一个独立单位**。 线程是进程的一个实体，是**CPU调度和分派的基本单位**，它是比进程更小的能独立运行的基本单位。

* 简介

  * Node.js是一个能够在**服务器端**运行js代码的开放源代码、跨平台的**js运行环境**。

  * 采用google开发的V8引擎运行js，使用事件驱动、非阻塞和异步I/O等技术来提高性能。

  * V8引擎 --> 想用js开发高性能web服务器（单线程，异步） -->  Node.js

  * 奇数版为开发版，偶数版为稳定版

  * 用途

    * web服务api，比如REST
    * 实时多人游戏
    * 后端的web服务，比如跨域、服务器端的请求
    * 基于web的应用
    * 多客户端的通信

  * 用node执行js

    `node hello.js`

* 模块化（了解CommonJs规范）

  * 在nodejs中，一个js文件就是一个模块
  * 在node中，每个js文件中的js代码都是独立运行在一个函数中(可以通过**arguments或者arguments.callee**来证明)，而不是全局作用域中，所以一个模块中的函数和变量不能在其他模块中被访问到
  * 在node中，通过require去引入外部模块，传入一个参数为路径，返回一个对象为引入的模块。
  * 通过exports去导出一个模块中的方法或变量给其他模块使用。比如`exports.add = function(a,b) {return a + b;}`
  * 模块标识
    * 引入模块时，使用的就是模块标识
    * 两类模块
      * 核心模块
        * 由node引擎提供的模块
        * 模块标识就是模块的名字
        * `require("fs")`
      * 文件模块
        * 用户自己编写的模块
        * 模块标识是文件的路径（绝对或相对路径）
        * `require("./xxx")`
  * 在node中有个全局变量叫`global`，作用和网页中的window类似
  * 模块中的代码实际上是包装在一个函数里执行的，并且该函数执行时，传入了5个实参，分别为`(exports, require, module, __filename, __dirname)`
    * exports： 用来暴露变量或者函数（对象）
    * require：用来引入外部模块
    * module：模块本身，module.exports === exports -> true, 所以我们可以通过两种方式暴露变量或函数
    * __filename：当前模块的完整路径
    * __dirname：当前模块文件夹的完整路径
  * exports VS module.exports (理解栈内存和堆内存)
    * exports = module.exports
    * 都可以使用exports.xxx = xxx 和 module.exports.xxx = xxx去暴露变量或函数
    * 但是只能module.exports = {xxx: xxx} 而不能exports = {xxx: xxx} 去暴露
      * 一个是在改变对象本身，一个是在改变变量的指向（赋予了新的对象，但并未改变module.exports这个被别的模块调用的时返回对象）

##### 包 package

* CommonJs规范 允许我们把**一组相关的模块**组合到一起，形成一套完整的工具。
* 包规范： 包结构+ 包描述文件（package.json)
* npm：node package manger
  * 包规范是理论，npm是一种实践
  * npm帮助第三方模块的发布、安装、依赖
  * npm常用命令
    * npm init  帮助生成项目的package.json
    * npm -v
    * npm version 查看所有模块的版本
    * npm search pkg  搜索包
    * npm install/i pkg 安装包
    * npm remove/r pkg 删除包
    * npm install pkg --save 安装包并加入到依赖中
    * npm install 根据package.json中的依赖，下载当前项目所依赖的包
    * npm install pkg -g 全局安装包，一般都是工具（计算机所需而不是开发所需）
    * npm install pkg --registry=地址  从镜像源安装
    * npm config set registry 地址   设置镜像源
    * npm show pkg version 查看某个包的最新版本
    * ....
  * cnpm，国内npm的镜像版本： https://github.com/cnpm/cnpm
  * 下载的包都在node_module文件夹下，直接通过包名引入
  * 通过包名引入时，会首先在当前目录的node_modules里寻找是否有这个包，找不到的话，会向上一级目录去寻找node_modules文件夹，直到磁盘的根目录。

##### Buffer（缓冲区）

* Buffer的结构跟数组很像，操作方法也很类似

* 数组中不能储存二进制数据（比如视频、音频文件等），而buffer就是专门用来存储二进制数据的。Buffer 用于读取或操作二进制数据流，做为 Node.js API 的一部分使用时无需 require，用于操作网络协议、数据库、图片和文件 I/O 等一些需要大量二进制数据的场景（处理跟不上数据时，需要先存放到buffer）

* 使用buffer不需要引入模块，直接使用即可

  ```javascript
  const str = 'test';
  const buffer = Buffer.from(str); // 将字符串保存到buffer中
  ```

* 在buffer中存储的都是二进制数据，但是以16进制显示出来。

* 字节是数据传输最小的单位 （8 bits = 1 byte）

* 英文一个字母占用一个字节，中文一个汉字占用3个字节

* buffer.length 表示占用内存的大小；与字符串的长度不同

* buffer的构造函数都是不推荐使用的 deprecated

* 创建一个大小确定的buffer --> `const buffer = Buffer.alloc(10)`

* 可以通过索引对buffer进行赋值；当位数溢出时，只储存后8位。

* 大小一旦确定就无法更改，因为**buffer实际上是对底层内存的直接操作**。

* 数字在控制台或者页面输出时，都显示成十进制。

  ```javascript
  const buffer = Buffer.alloc(10);
  
  buffer[0] = 0xaa;
  
  console.log(buffer[0]); // 170
  console.log(buffer[0].toString(16)); // AA
  console.log(buffer[0].toString(2));
  ```

* 另一个跟alloc相似的函数：Buffer.allocUnsafe --> 也是指定大小的buffer，但是可能含敏感数据（因为该方法不会对分配的内存进行清零，可能还留有之前存储过的数据；而alloc在分配内存后还会帮助清空）

##### fs 文件系统

文件系统简单来说就是通过node来操作系统中的文件。

在Node中，与文件系统的交互非常重要。服务器的本质就是把本地文件发送给远程的客户端。

Node通过fs模块与文件系统进行交互，提供了一些标准文件访问API来打开、读取、写入文件以及与其交互。

fs是核心模块，不需要下载； `var fs = require('fs')`引入fs模块

* fs模块中的所有操作都有同步和异步两种形式

  * 同步会阻塞程序的执行，只有操作完成才会向下执行代码
  * 异步不会阻塞程序执行，在操作完成时，通过**回调函数**将结果返回

* **同步**的文件写入

  * 打开文件 `fs.openSync(path[, flags, mode])`
    * path: 要打开文件的路径
    * flags：打开文件要做的操作类型
      * r 只读的
      * w 可写的
    * mode：设置文件的操作权限，一般不传
    * 返回值是打开文件的描述符，可通过该描述符对此文件进行操作
  * 向文件写入内容 `fs.writeSync(fd, string[, position[, encoding]])` 还有其他写入的方法
    * fd 要写入文件的描述符
    * string 要写入的字符串
    * position 写入内容的位置
    * encoding 写入的编码，默认 utf-8
  * 保存并关闭文件 `fs.closeSync(fd)`

  ```javascript
  var fs = require('fs');
  
  var fd = fs.openSync('hello.txt', 'w');
  fs.writeSync(fd, 'Hello World!');
  fs.closeSync(fd);
  ```

* **异步**的文件写入

  `fs.open`

  `fs.write`

  `fs.close`

  ```javascript
  var fs = require('fs');
  // 异步方法的返回值可以在回调函数的参数中获得。
  fs.open('hello.txt', 'w', function(err, fd) {
    if (!err) {
      console.log('打开文件成功');
      fs.write(fd, '我在异步写入文件', function(err) { // 还有两个参数 written, string
        if (!err) {
          console.log('文件写入成功');
          fs.close(fd, function(err) {
            if (!err) {
              console.log('文件关闭成功');
            }
          })
        }
      })
    } else {
      console.log(err);
    }
  })
  ```

  异步的好处是：

  * 性能优于同步
  * 能比较好处理异常，不会影响其他程序

* [简单文件写入](https://nodejs.org/api/fs.html#fs_fs_writefile_file_data_options_callback)

  实际上，上述两种写入方法在日常开发中用的不多。

  `fs.writeFile(file, data[, options], callback)`

  `fs.writeFileSync(file, data[, options])`

  * file 写入文件的路径（相对或绝对路径均可）

  * data 要写入的数据

  * options 可以对写入操作进行一些设置，如果是string，则表示encoding，如果是object：

    * encoding

    * mode

    * flag

      常用：

      * r 读
      * w 写（覆盖）
      * a 追加

    * signal

  * callback 写入完成后执行的回调函数

* **流式文件写入**

  之前都是一次性写入（提前准备buffer然后一次性写入），但是如果文件很大，会占用很多内存（导致内存溢出），对性能影响很大。

  * [创建一个可写流](https://nodejs.org/api/fs.html#fs_fs_createwritestream_path_options)

    ``fs.createWriteStream(path[, options])``

    options可参考官网链接 ^^

  * 可以为流对象进行事件绑定

    `ws.on('open', function() {xxxx})`

    `ws.once('open', function() {xxxx})` --> 只监听一次

  * 通过流对象进行文件写入（可多次写入）

    `ws.write('xxxxx')`

    `ws.write('abc')`

    `ws.write('def')`

    `ws.write('ghi')`

  * 关闭流

    `ws.end()` 会等还在写的内容结束之后再关闭

    `ws.close()` 直接关闭流

* 简单文件读取

  其实还有同步文件读取和异步文件读取，与写入类似。

  ``fs.readFile(path[, options], callback)``

  ``fs.readFileSync(path[, options])``

  ```javascript
  var fs = require('fs');
  
  fs.readFile('hello.jpg', function(err, data) {
    if (!err) {
      console.log(data); // data是一个buffer对象（更好的通用性）
      fs.writeFile('copy.jpg', data, function() {});
    }
  })
  ```

* **流式文件读取**

  适用于大文件，可以分多次读取到内存中

  * 创建可读流

    `var rs = createReadStream('hello.jpg');`

  * 监听流的时间（开启，关闭。。）

    `rs.once('open', function() {xxx})`

    `rs.once('close', function() {xxx})`

  * 读取数据

    ```javascript
    var rs = createReadStream('hello.mp3');
    var ws = createWriteStream('copy.mp3');
    
    rs.once('open', function() {
      console.log('可读流打开了');
    })
    
    rs.once('close', function() {
      console.log('可读流关闭了');
      ws.end(); // 在可读流的关闭事件中，关闭可写流
    })
    
    ws.once('open', function() {
      console.log('可写流打开了');
    })
    
    ws.once('close', function() {
      console.log('可写流关闭了');
    })
    
    rs.on('data', function(data) {
      console.log(data.length); // 每一次最多读取65536字节
      rw.write(data); // 传输到可写流中
    }) // 读取完数据后，会自动关闭可读流，不需要手动关闭
    
    
    // 如果需要可读流向可写流中写数据，可以简化成
    rs.pipe(ws);
    ```

* 其他fs模块的方法

  * 检查路径是否存在

    `fs.exists(path)`  --> 已经废弃

    `fs.existsSync(path)`

  * 获取文件的状态

    `fs.stat(path, function(err, stat) {})` //回调函数中stat是个[Stats对象](https://nodejs.org/api/fs.html#fs_class_fs_stats)

    `fs.statSync(path)`

  * 删除文件

    `fs.unlink(path, callback)`

    `fs.unlinkSync(path)`

  * 读取目录的结构

    `readdir(path, function(err, files) {console.log(files)})` // 回调的第二个参数是个数组，是该路径下文件或文件夹的名字

  * 截断文件

    将文件修改为指定大小的文件

    `fs.truncate(path, len, callback)`

  * 建立目录（文件夹）

    `fs.mkdir(path[,mode], callback)`

  * 删除目录

    `fs.rmdir(path, callback)`

  * 重命名文件夹

    `fs.rename(oldPath, newPath, callback)`

    当新旧路径不是在同一个文件夹下时，间接实现了剪切的功能

  * [监事文件的修改](https://nodejs.org/api/fs.html#fs_fs_watchfile_filename_options_listener)

    ``fs.watchFile(filename[, options], listener)``

    options可以配置检查文件状态的间隔

    listener有两个参数，cur和prev，都是Stats对象





### [黑马程序员视频内容](https://www.bilibili.com/video/BV1Ns411N7HU)

#### Day1

##### Node.js是什么

* 是一个js的运行环境，可以解析执行js代码
  * 浏览器中的js
    * EcmaScript
    * BOM
    * DOM
  * Nodejs中的js
    * 没有DOM, BOM
    * EcmaScript
    * 为js提供一些服务器级别的API
      * 文件读写（fs模块）
      * 网络服务构建
      * 通络通信
      * http服务器
      * 等等
* 事件驱动 + 非阻塞I/O模型（异步）
* npm
* 构建于Chrome的V8引擎（目前公认最快的）之上
  * js引擎认识js代码，帮助解析和执行
  * nodejs从v8引擎中移植出来，开发了一个独立的js运行时环境。

##### Node.js能做什么

* web服务器开发
* 命令行工具
  * npm
  * 。。。

##### 能学什么？

* B/S编程模型（Browser/Server）
* 模块化编程
* Node常用API
* 异步编程
* Express Web开发框架
* ES6
* 。。。

环境安装

用node执行js文件

* 不要用`node.js`命名，也最好不用中文命名

node上的js具有<u>读写文件</u>的能力 --> fs模块

##### http服务

用node<u>构建网页服务器</u> --> 核心模块 http

```javascript
var http = require('http');
var server = http.createServer();

server.on('request', function(req, res) {
  console.log(req.url);
  // response对象的write可以给客户端发送响应数据，可以使用多次
  // 对于不同请求路径，返回不同的响应数据。
  // res.write('aaa');
  // res.write('bbb');
  // 相应完了之后一定要end，通知客户端
  // res.end();
  
  // 简写方式
  // res.end('aaabbb');
  
  // req.url --> 端口号之后的那部分路径，以/开头
  // 响应内容只能是二进制数据(Buffer）或者字符串
  if (req.url === '/products') {
    var p = [{name: '123', price: 123}];
    
    res.end(JSON.stringfy(p));
  }
})

server.listen(3000， function() { // 浏览器默认的http端口号是80
  console.log('服务器启动成功，请使用127.0.0.1:3000/进行访问');
});
```

浏览器有默认行为去请求 `favicon.ico`

##### Node中的js

###### 核心模块

Node为js提供了很多服务器级别的API，这个Api绝大多数被包装到了一个具名的核心模块中。

比如文件操作的`fs`模块，http服务构建的`http`模块，路径操作`path`模块，操作系统信息模块`os`。。。还会有其他一些模块，可以在官网上找到详细的介绍。

var xxx = require('xxx') --> 引入模块

###### 模块系统

* 具名的核心模块
* 用户自定义的模块
  * 相对路径必须加`./` or `../`

在Node中，没有全局作用域，**只有模块作用域**。（Node.js 是CommonJs规范的实现）

**加载和导出** `require and module.exports`

* 在模块中，node帮我们做了一件事 `var exports = module.exports;`

其他内容也在尚硅谷视频中讲过。（link to 基础补充）

###### 网上补充内容，关于两种模块化规范

CommonJS用**同步**的方式加载模块。在服务端，模块文件都存在本地磁盘，读取非常快，所以这样做不会有问题。但是在浏览器端，限于网络原因，**CommonJS不适合浏览器端模块加载**，更合理的方案是使用异步加载

<u>**CommonJs和ES6区别**</u>

（1） CommonJS 模块输出的是一个值的拷贝，ES6 模块输出的是值的引用。

CommonJS 模块输出的是值的拷贝，也就是说，一旦输出一个值，模块内部的变化就影响不到这个值。
ES6 模块的运行机制与 CommonJS 不一样。JS 引擎对脚本静态分析的时候，遇到模块加载命令import，就会生成一个只读引用。等到脚本真正执行时，再根据这个只读引用，到被加载的那个模块里面去取值。换句话说，ES6 的import有点像 Unix 系统的“符号连接”，原始值变了，import加载的值也会跟着变。因此，ES6 模块是动态引用，并且不会缓存值，模块里面的变量绑定其所在的模块。

（2） CommonJS 模块是运行时加载，ES6 模块是编译时输出接口。


运行时加载: CommonJS 模块就是对象；即在输入时是先加载整个模块，生成一个对象，然后再从这个对象上面读取方法，这种加载称为“运行时加载”。


编译时加载: ES6 模块不是对象，而是通过 export 命令显式指定输出的代码，import时采用静态命令的形式。即在import时可以指定加载某个输出值，而不是加载整个模块，这种加载称为“编译时加载”。


CommonJS 加载的是一个对象（即module.exports属性），该对象只有在脚本运行完才会生成。而 ES6 模块不是对象，它的对外接口只是一种静态定义，在代码静态解析阶段就会生成。

<u>Node.js如何处理ES6模块</u>： https://www.ruanyifeng.com/blog/2020/08/how-nodejs-use-es6-module.html

<u>浏览器如何支持import和require</u>： https://segmentfault.com/a/1190000023078400

* 原生浏览器不支持exports/require
* babel会将esModule规范转化成commonjs规范
* webpack、gulp以及其他构建工具会对commonjs进行处理，使之支持浏览器环境
* import/export 在浏览器中无法直接使用，我们需要在引入模块的 <script> 元素上添加type="module属性。

##### ip地址和端口号

* 计算机只有一个物理网卡，而且在同一个局域网中，网卡的地址是唯一的。（需要学习networks基础内容）
* IP地址用来定位计算机（服务器），端口号用来定位具体的应用程序（所有需要联网通信的应用程序都必须具有端口号，没有端口号无法通信）。

* 客户端（比如浏览器）在发请求的时候，会自动申请一个端口号，使用该端口号与后台服务器进行通信。在服务器端也可以通过request对象来获取 对服务器发起请求的ip地址和端口号，但是我们其实不需要知道，在返回响应数据的时候，response知道要把数据传给谁。

* 端口号的范围是0 ~ 65535之间
* 默认的端口号， http 80，开发阶段不要去使用这些端口号。

##### 响应内容 Content-Type

在服务器端发送的数据，默认是utf-8编码的。

但是浏览器不知道接收到的数据是utf-8编码的，在不知道响应数据类型的情况下，浏览器会使用当前操作系统的默认编码去解析。

在http协议中，Content-Type就是用来定义请求/响应数据的编码类型。在Apache等服务器软件中，会帮助修改Content-Type等。

```javascript
var http = require('http');
var server = http.createServer();

server.on('request', function(req, res) {
  var url = req.url;
  
  if (url === '/plain') {
    res.setHeader('Content-Type', 'text/plain; charset=utf-8');
    res.end('plain text');
  } else if (url === '/html') {
    res.setHeader('Content-Type', 'text/html; charset=utf-8');
    res.end('<p>这是html标签</p>'); // 如果是text/plain则不会当做html tag
  }
})

server.listen(3000， function() { // 浏览器默认的http端口号是80
  console.log('服务器启动成功，请使用127.0.0.1:3000/进行访问');
});
```

##### 结合文件系统和Content-Type

MIME types: https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/MIME_types

```javascript
var http = require('http');
var fs = require('fs');
var server = http.createServer();

server.on('request', function(req, res) {
  var url = req.url;
  
  if (url === '/') {
    fs.readFile('./resource/index.html', function(err, data) {
      if (!err) {
        res.setHeader('Content-Type', 'text/html; charset=utf-8');
        res.end(data);
      }
    })
  } else if (url === '/image') {
    fs.readFile('./resource/image.jpg', function(err, data) {
      if (!err) {
        res.setHeader('Content-Type', 'image/jpeg'); // 编码一般指字符编码
        res.end(data);
      }
    })
  }
})

server.listen(3000， function() { // 浏览器默认的http端口号是80
  console.log('服务器启动成功，请使用127.0.0.1:3000/进行访问');
});
```



#### Day2

当使用无分号的代码风格时，如果代码以(, [, `开头，需要加上分号避免一些语法解析错误。

##### 初步实现Apache功能

###### 能通过url访问服务器下的资源

```javascript
var http = require('http');
var fs = require('fs');
var server = http.createServer();

var wwwDir = 'xx/xx/www';

server.on('request', function(req, res) {
  var url = req.url;
  var filePath = '/index.html';
  
  if (url !== '/') {
    filePath = url;
  }
  
  fs.readFile(wwwDir + filePath, function(err, data) {
    if (err) {
      return res.end('404 Not found');
    }
    
    res.end(data);
  }
});

server.listen(3000， function() { // 浏览器默认的http端口号是80
  console.log('服务器启动成功，请使用127.0.0.1:3000/进行访问');
});
```

Html文件中meta tag也能指定编码方式。

###### 文件目录

1. 创建模块html
2. 得到wwwDir目录下的文件名和目录名 fs.readdir
3. 将得到的文件名和目录名替换到template.html里

* 使用字符串替换

```javascript
// 在template.html里预留需要用动态数据替换的地方，并留下记号，比如$$

var http = require('http');
var fs = require('fs');
var server = http.createServer();

var wwwDir = 'xx/xx/www';

server.on('request', function(req, res) {
  // var url = req.url;
  var content = '';
  
  fs.readFile('./template.html', function(err, data) {
    if (err) {
      return res.end('404 Not found');
    }
    
    fs.readdir(wwwDir, function(err, files) {
      files.forEach(item => {
        content += `xxxxx${item}xxxxx`; //此处根据动态数据准备好要替换的内容
      })
      
      data = data.toString();
      // 将模板中预留的位置替换成有意义的内容
      data = data.replace('$$', content);
      
      res.end(data);
    });
  }
});

server.listen(3000， function() { // 浏览器默认的http端口号是80
  console.log('服务器启动成功，请使用127.0.0.1:3000/进行访问');
});
```

* 在服务器端使用模板引擎

**art-template**

模板引擎不关心字符串内容，只关心模板标记语法（需要替换的地方），比如 {{}} --> mustache语法

 浏览器端和服务器端的模板引擎使用方法有点不同，查看文档使用API

```javascript
var http = require('http');
var fs = require('fs');
var template = require('art-template');
var server = http.createServer();

var wwwDir = 'xx/xx/www';

server.on('request', function(req, res) {
  // var url = req.url;
  var content = '';
  
  fs.readFile('./template.html', function(err, data) {
    if (err) {
      return res.end('404 Not found');
    }
      
    fs.readdir(wwwDir, function(err, files) {
      // 默认读取到的是二进制数据，我们需要转化成字符串才能传给template的api
      var ret = template(data.toString(), {
        files
      });
      
      res.end(ret);
    });
  }
});

server.listen(3000， function() { // 浏览器默认的http端口号是80
  console.log('服务器启动成功，请使用127.0.0.1:3000/进行访问');
});
```

```html
<-- 这是 template.html 可能的结构 -->
xxxxx
{{each files}}
	xxxx
	xxxx
	{{$value}}
	xxx
{{/each}}
xxxxx
```

##### 服务端渲染和客户端渲染

服务端渲染其实就是在服务端使用模板引擎，将数据填入html后，直接响应给客户端。客户端无需再做处理即可渲染。

客户端渲染 --> 响应给客户端的是模板html，需要多一次ajax请求取回数据，然后在客户端使用模板引擎去替换需要用数据替换的地方。

各有优势：

客户端渲染不利于SEO搜索引擎优化，但是用户体验更好（异步请求数据，局部页面刷新）

服务端渲染是可以被爬虫抓取到的，容易被搜索引擎搜到。

##### 留言板实例

###### 处理网站中的静态资源

当浏览器收到HTML响应内容后，就要开始从上到下依次解析。

当在解析过程中，发现link, script, img, iframe, video, audio等带有src或者href（link）属性标签的时候，浏览器会自动对这些静态资源发起新的请求。

为了方便统一处理这些静态资源，我们约定把所有的静态资源都存放在public目录中，`/public/img`, `/public/js`, `/public/css`, `/public/lib`

在服务端中，文件的路径不要写相对路径，因为这个时候<u>所有资源都是通过url标识来获取</u>的。浏览器在真正发送请求时，会自动把host name和端口号加上。

我们是可以控制哪些资源能被访问到，哪些不能。

###### 页面跳转 + 404处理

```javascript
var fs = require('fs');
var http = require('http');

http.createServer(function(req, res) {
  var url = req.url;
  
  if (url === '/') {
      fs.readFile('./view/index.html', function(err, data) {
        if (err) {
          return res.end('404 Not found');
        }
        res.end(data);
      };
  } else if (url.indexOf('/public/') === 0) { // 处理静态资源的请求
      fs.readFile('.' + url, function(err, data) {
        if (err) {
          return res.end('404 Not found');
        }
        res.end(data);
      };
  } else if (url === '/post') {
      fs.readFile('./view/post.html', function(err, data) {
        if (err) {
          return res.end('404 Not found');
        }
        res.end(data);
      };
  } else {
       // 404 处理 
       fs.readFile('./view/404.html', function(err, data) {
        if (err) {
          return res.end('404 Not found');
        }
        res.end(data);
      };
  }
}).listen(3000， function() { // 浏览器默认的http端口号是80
  console.log('服务器启动成功，请使用127.0.0.1:3000/进行访问');
});

```

###### 处理表单提交

核心模块 url，我们可以通过**url.parse(req.url)**得到一个方便处理的路径对象。第二个参数为true时，路径对象中的query会从字符串转化成一个对象。

该路径对象包括`pathname`, `query`等信息。我们需要使用pathname去替换之前的url作为if else 语句的条件，因为此时url已经不方便使用了。(pathname是不包含查询字符串的内容)

我们需要：

* 在服务端处理请求的pathname
* 根据query，添加新的评论到储存数据的数组里
* **服务器重定向**：redirect到路径 `/`，由于数据已经发生变化，所以视图也会变化（新的评论被加上了）
  * 如何通过服务器让客户端重定向？
    * 将状态码设置成302 临时重定向 statusCode
    * 在响应头中通过Location告诉客户端往哪里重定向  setHeader
  * 如果客户端发现收到服务器的响应状态码是302，会自动去响应头中找Location，对此发起新的请求
* Node不适合新手入门服务端的原因：比较**底层且灵活**。（比如重定向需要手动设置状态码等）
* Node的Express框架会帮忙封装一些api简化编码难度。 --> 更专注于业务而不是底层细节

```javascript
// 该老师上课讲的api已经legacy，可以通过node官网看到最新的WHATWG URL Api
// The url module provides two APIs for working with URLs: a legacy API that is Node.js specific, and a newer API that implements the same WHATWG URL Standard used by web browsers.
// https://nodejs.org/api/url.html#url_url_searchparams
var fs = require('fs');
var http = require('http');
var template = require('art-template');
var urlLib = require('url');
var {URL} = urlLib;
var comments = [];

http.createServer(function(req, res) {
  var url = new URL(req.url);
  var pathname = url.pathname;
  
  if (pathname === '/') {
      fs.readFile('./view/index.html', function(err, data) {
        if (err) {
          return res.end('404 Not found');
        }
        
        var htmlStr = template.render(data.toString(), {
          comments: comments
        });
        
        res.end(htmlStr);
      };
  } else if (pathname.indexOf('/public/') === 0) { // 处理静态资源的请求
      fs.readFile('.' + url, function(err, data) {
        if (err) {
          return res.end('404 Not found');
        }
        res.end(data);
      };
  } else if (pathname === '/post') {
      fs.readFile('./view/post.html', function(err, data) {
        if (err) {
          return res.end('404 Not found');
        }
        res.end(data);
      };
  } else if (pathname === '/pinglun') {
        // 读取query内容，添加数据到comments状态中
        var newComment = {};
        
        url.searchParams.forEach((value, name) => {
          newComment[name] = value;
        })
        
        comments.unshift(newComment);
        
        // 如果使用url的legacy api，我们可以像老师那样写
        // comments.unshift(urlLib.parse(req.url, true).query);
        
        // 重定向至 /
        res.statusCode = 302;
        res.setHeader('Location', '/');
        res.end();
  } else {
       // 404 处理 
       fs.readFile('./view/404.html', function(err, data) {
        if (err) {
          return res.end('404 Not found');
        }
        res.end(data);
      };
  }
}).listen(3000， function() { // 浏览器默认的http端口号是80
  console.log('服务器启动成功，请使用127.0.0.1:3000/进行访问');
});
```

###### 项目总结

1. / --> index.html

2. 开放public目录中的静态资源

   /public/xxxx   --> 读取响应public目录中的具体资源

3. /post --> post.html

4. /pinglun

   1. 接收表单提交数据
   2. 存储表单提交的数据
   3. 重定向到评论页
      * statusCode
      * setHeader

REPL(read, eval, print, loop): node在terminal里运行;就跟在浏览器console里写code一样。**所有核心模块不需要require引入**。可以帮助测试一些api

#### Day3

##### 课前反馈补充

* 版本号问题：

大版本.minor.patch

新增功能比较多，可能去除了某些功能；加入了新功能；修复bug，性能提升

~ caret prefix: update you to all future patch versions; ^ tilde prefix: all future patch/minor versions

一般是客户端软件、框架开发者比较关注。

* each, forEach, jQuery, 模板引擎？？

模板引擎语法：

```
{{each 数组}}
xxxx 这里写需要从传入的数组的数据中，转化的html
数组的item通过 $value得到
{{/each}}
```

jQuery

```javascript
$.each(数组, function)
$('div').each(function) // jQuery选择器得到的一个伪数组对象
// 可以通过 [].slice.call(jQuery实例对象,必须有length属性) 转化成数组
```

forEach是ES5原生js的代码，IE8以下低版本不支持。（此时可考虑jQuery）

* Nodejs是高级前端工程师必备技能
* php中，无论是代码还是网页都可以用过web url来访问；但是在Node中开启的服务器(默认是黑盒子)，用户可以访问哪些资源由开发人员编写的代码为准，自定义非常灵活。
* 301永久重定向，浏览器会缓存结果，下次访问时会直接请求重定向后的地址；302临时重定向，浏览器不会记住，下次请求a时，还是会通过a的重定向告诉浏览器要请求b

##### 模块系统

Node编写应用主要是使用

* EmcaScript语言

* 核心模块
  * fs
  * http
  * url
  * path
  * os

* 第三方模块（需要npm下载后使用

* 自己写的模块

###### CommonJs的模块规范

Node中的javascript(模块化):

* 模块作用域

* 使用require加载模块

  `var 自定义变量名 = require('path to module');`

* 使用exports接口对象来导出模块中的成员

  导出多个成员：

  * ```javascript
    exports.a = function(xx);
    exports.b = 'hello';
    exports.c = 12;
    ```

  * ```javascript
    module.exports = {
      a: function(xxx),
      b: 'hello',
      c: 12
    };
    ```

  导出单个成员：

  ```javascript
  module.exports = function(xxx);
  ```

###### exports 和 module.exports 区别（原理解析）

在尚硅谷视频的基础补充那部分也讲过。

实际上 exports === module.exports --> true --> Node为了导出方便，执行了 `var exports = module.exports`

谁来require这个模块，谁就得到`module.exports` 

当需要直接导出某个成员时（某个字符串、方法等等），必须要用`modules.exports`而不是exports

注意栈内存和堆内存。（对象引用的问题）

反正，使用module.exports永远不会错。。

###### require加载规则

* 优先从缓存加载

  * 如果一个模块已经被加载过了，再次require的时候不会再去执行那个模块中的代码，直接得到modole.exports的值
  * 为了减少模块的重复加载，提高模块加载的效率

* 模块标识分析

  * 路径形式的模块

    * ./ or ../比较多
    * .js后缀名课省略

  * 核心模块的本质也是文件。已经被编译到了二进制文件中了，我们只需要按照名字来加载就行了。

  * 第三方模块

    * 不可能有一个第三方包和核心模块是一样的

    * **加载规则**：

      * 先找到当前文件所处目录中的node_modules目录
      * 找到跟包名相同的文件夹  node_modules/art-template
      * 寻找package.json文件 -->  node_modules/art-template/package.json
      * 寻找package.json文件中的main属性
      * mian属性记录了入口模块
      * 所以实际上也是加载了文件

      如果package.json不存在或者指定的main是不对的，则会寻找该目录下的index.js作为默认备选项。

      如果以上都不能满足，则会去上一级目录进行相同的加载规则，直到磁盘根目录。

      如果最终没有找到的话，报错 --> cannot find module xxx

    * 注意：我们的项目有且只有一个node_modules文件夹，不可能出现多个。并且一般都存放在项目的根目录，这样的话，所有子目录中的文件都能加载到第三方包

###### package.json 和 npm

* 包说明文件：

通过`npm init` 生成包说明文件，填入一些跟项目相关的信息。比如说入口文件(entry point)，版本号等。

通过`npm i --save xxx` 下载指定的第三方包并将依赖项信息记录到package.json的dependencies属性下。（npm 5之后不用加--save，会自动保存依赖信息）

如果node_modules被删掉了，也可以通过`npm i`把记录在包说明文件中的依赖包下载回来。

* package-lock.json （老版本的npm没有这个文件，是在npm install的时候创建的）

该文件中会保存`node_modules`中所有包的信息。

1. 重新npm install的时候速度比较快
2. 锁定版本号，防止自动升级新版（除非自己npm install）

 注意，^ 和 ~ 以及不加符号的版本号是不同的。

* npm：

可以查到有哪些第三方包；可以上传自己写的包给别人下载

命令行工具

升级npm： `npm install --global npm`

常用命令：

`npm init -y` 跳过向导，快速生成说明文件  --> `npm init [--yes]`

`npm install xxx`

`npm install --save xxx`  下载并记录到dependencies下

`npm i -S xxx`

`npm uninstall (--save) xxx`

`npm un (-S) xxx`

`npm help`

`npm 命令x help` 查看某个命令的使用帮助

###### 如何解决npm被墙的问题？（或者如何使用其他服务器进行下载？）

* 使用淘宝的镜像源

  `npm install --global cnpm`

  之后就用 cnpm代替所有npm进行包管理器的操作

* **设置镜像源**（公司里也是这样做的）

  `npm install xxx --registry https://registry.npm.taobao.org`

  我们也可以把这个加入到配置文件中，这样的话，每次使用npm都会从此镜像源下载。

  `npm config set registry https://registry.npm.taobao.org`

  查看是否镜像源设置成功：

  `npm config get registry`  或者 `npm config list`

  恢复初始的镜像源：

  `npm config set registry https://registry.npmjs.org`

##### Express框架

因为原生的http满足不了日常开发的需求，所以为了业务开发的需求，我们需要用到express这种web框架来加快我们的开发效率，使我们的代码高度统一。

很多我们之前需要手动去实现的细节，在express框架中，可能都是call api就能实现。express框架会帮忙在response中设置content-type; 对于没开放的资源，会处理404; 开放静态资源也是express.static一句话能搞定。

```javascript
const express = require('express');
const app = express(); // 相当于之前的http.createServer()

app.get('/', function(req, res) {
  res.send('Hellow world');
});

// 公开指定目录
app.use('/public/', express.static('./public/'));

// 模板引擎结合express框架更简单
```

#### Day4

##### Express框架

###### 文件操作的路径和模块标识路径

文件操作（fs.readFile）：

* `./data/a.js` 与 `data/a.js` 意义相同
* `/data/a.js` 是绝对路径，从磁盘根目录开始找

模块标识：

* 使用相对路径时，不能省略 ./
* `require('./data/a.js');`
* 绝对路径

###### Hello world

express提供了更多的api，原先就存在于http库中的api依然可以使用。

```javascript
const express = require('express');
const app = express();

app.get('/', function(req, res) {
  // res.write('hello world');
  // res.end();
  res.send('hello world');
});

app.listen(3000, function() {
  console.log('server is running...');
})
```

###### 修改代码后自动重启

基于nodejs的第三方库`nodemon`

```shell
npm install --global nodemon

# node app.js
nodemon app.js
```

###### 基本路由

请求方法

请求路径

处理函数

###### 静态服务资源

[如何暴露静态资源](https://expressjs.com/en/starter/static-files.html)

```javascript
// 访问 /public/xxx时暴露public文件夹中的资源
app.use('/public', express.static('public'));

// 访问 /files/xxx时暴露public文件夹中的资源
app.use('/files', express.static('public'));

// 访问 /xxx时暴露public文件夹中的资源 (此时访问资源不需要加/public)
app.use(express.static('public'));

// 访问 /static/xxx时暴露public文件夹中的资源(safer to use the absolute path of the directory that you want to serve)
app.use('/static', express.static(path.join(__dirname, 'public')))
```

###### 在Expressz中配置使用art-template模板引擎

[官网教程](https://aui.github.io/art-template/express/)

[express官网的模板引擎教程](https://expressjs.com/en/guide/using-template-engines.html): 还有很多其他热门的模板引擎，如果使用了`app.set('view engine', 'pug') 第二个参数是要使用的模板引擎`, don’t have to specify the engine or load the template engine module in your app

* 安装

  ```shell
  npm install --save art-template
  npm istall --save express-art-template
  ```

* 配置

  ```javascript
  // view engine setup
  // 第一个参数是，当渲染以.art为后缀的文件时，使用art-template模板引擎
  // 也可以改为html（如果部分editor不能识别art文件
  // express-art-template 是专门用来在Express中把art-template整合到express中
  app.engine('art', require('express-art-template'));
  ```

* 使用

  ```javascript
  // Express 为response对象提供了render函数
  // 默认是不可用的，但配置模板引擎后可以使用
  // res.render('html模板文件', {模板参数})
  // 第一个参数不能写路径，默认会去项目中的views目录下查找该模板文件 --> express规定的
  app.get('/', function(req, res) {
    res.render('404.art', {title: 'hello world'});
  }); // 也就是说现在该文件的路径应该是  ./views/404.art
  ```

* 如果想要修改views视图渲染存储目录

  ```javascript
  app.set('views', '目录路径'); //这样的话，每次res.render会去自定义的文件目录去找需要渲染的文件
  ```

###### 使用express框架重写留言板实例

```javascript
var express = require('express');
var app = express();

var comments = [];

// 配置模板引擎
app.engine('art', require('express-art-template'));

// 暴露public文件资源
app.use('/public', express.static('public')); //app.use的middleware会在每次请求时执行

app.get('/', function(req, res) {
  res.render('index.art', {comments});
});

app.get('/post', function(req, res) {
	res.render('post.art');
});

app.get('/pinglun', function(req, res) {
	console.log(req.query);
  
  comments.unshift(req.query);
  res.redirect('/'); // 结合了原生nodejs的 res.statusCode = 302; res.setHeader('Location', '/');
});

app.listen(3000, function(req, res) {
  console.log('running.....');
});
```

###### 如何在express中获取表单POST请求数据

因为是POST请求，请求数据已经不能通过req.query获取。

Express中没有内置的获取表单post请求体的API，我们需要借助第三方包 `body-parser `

[body-parser](http://expressjs.com/en/resources/middleware/body-parser.html)

* 安装

  ```shell
  npm install --save body-parser
  ```

* 配置

  ```javascript
  var bodyParser = require('body-parser');
  
  // parse application/x-www-form-urlencoded
  app.use(bodyParser.urlencoded({ extended: false }))
  
  // parse application/json
  app.use(bodyParser.json())
  ```

* 使用

  ```javascript
  // 做完上述 安装和配置 后
  app.post('/post', function(req, res) {
    console.log(req.body); // 在request对象上多了一个body属性，就是post的请求体数据
    
    comments.unshift(req.body);
    res.redirect('/');
  })
  ```

##### Express实例：crud 学生管理系统

###### 准备阶段

* npm init -y 创建package.json
* 安装需要用的包 比如express, art-template, express-art-template...
* 搭建需要使用的文件和文件目录
  * public --> css, img, js
  * views 存放需要渲染的模板html
  * app.js 主功能文件
* 从bootstrap上面找一个模板网页并确定能work，并且在app.js主文件中做好准备工作
  * 修改一些细节，比如说引入的文件路径，删除一些不必要的引用（我们只需要样式和html即可）
  * 搭建起最基本的app.js，开放需要开放的静态资源，配置好模板引擎。

我们之前都是存放的临时数据，在这个案例中，我们要实现**数据持久化**。（之后会讲使用数据库存放数据，在这个案例中，我们把数据存放到静态文件（json文件）中，实现数据持久化）

###### 从文件读取数据

```javascript
var fs = require('fs');

// 省去一些之前写过的配置代码。。。

app.get('/', function(req, res) {
  fs.readFile('./db.json', function(err, data) {
    if (err) {
      return res.status(500).send('Server error.');
    }
    
    var students = JSON.parse(data).students; // 从文件读取的一定是string需要转化
    
    res.render('index.art', {students: students});
  });
});
```

第三方的东西不要太纠结，先以解决问题为主。如果有时间可以再仔细研究源码。比如react中的插件，中间件；比如webpack中也有很多插件。。

###### 路由设计

请求方法、请求路径、get参数、post参数、备注（作用）

路由设计需要在实现功能前完成

###### 路由模块的提取

```javascript
// app.js
// 创建服务
// 做一些服务相关的配置
//    模板引擎
//    body-parser
//    提供静态资源
// 挂载路由
// 监听端口启动服务

// 省略部分代码
var router = require('./router');
// 把路由容器挂载到app服务中
app.use(router);
```



```javascript
// router.js 路由模块
// 我们可以通过exports一个function自行进行封装
// Express提供了一种更好的方式来封装路由模块
var express = require('express');
// 创建路由容器，可以在app.js中 app.use(router) 去挂载路由
var router = express.Router();

router.get('/students', function(req, res) {});
router.get('/students/new', function(req, res) {});
router.post('/students/new', function(req, res) {});
router.get('/students/edit', function(req, res) {});
router.post('/students/edit', function(req, res) {});
router.get('/students/delete', function(req, res) {});

module.exports = router;
```

###### 添加数据和body-parser配置

###### 封装对students数据操作（增删改查）的模块

**得到或使用异步函数产生的数据或结果的唯一方式就是回调函数callback**。

* 封装查看学生数据的api

```javascript
// students.js 用于封装处理学生数据的API
var fs = require('fs');

exports.find = function(callback) {
  fs.readFile('./db.json', 'utf8', function(err, data) {
    if (err) {
      callback(err);
    }
    
    callback(null, JSON.parse(data).students);
  });
};

// 在app.js中如何使用这些封装好的异步API
var Util = require('./students.js');

app.get('/students', function(req, res) {
  Util.find(function(err, data) {
    if (err) {
      return res.status(500).send('Server error.');
    }
    
    res.render('index.art', {students: data});
  }); // 异步编程：给异步运行的代码，传递回调函数，才能使用异步函数的结果。
});
```

* 封装保存学生数据的api

```javascript
// students.js 用于封装处理学生数据的API
var fs = require('fs');

exports.save = function(student, callback) {
  fs.readFile('./db.json', 'utf8f', function(err, data) {
    if (err) {
      callback(err);
    }
    
    var students = JSON.parse(data).students;
    
    student.id = students[students.length - 1].id + 1; // 处理新的id
    students.push(student);
    
    fs.writeFile('./db.json', JSON.stringfy({students: students}), function(err) {
      if (err) {
        callback(err);
      }
      
      callback(null);
    });
  });
};

// 在app.js中如何使用这些封装好的异步API
var Util = require('./students.js');

app.get('/students/new', function(req, res) {
  res.render('new.art');
});

app.post('/students/new', function(req, res) {
  Util.save(req.body, function(err) {
    if (err) {
      return res.status(500).send('Server error.');
    }
    
    res.redirect('/students');
  });
});
```

异步编程的难点就是使用者负责定义和传递操作异步操作返回的数据的函数，而异步操作的封装者负责决定什么时候去调用执行回调函数。（上层定义，下层调用）与之前逻辑相反。

###### 完成渲染编辑页面 + 实现更新功能

```javascript
// students.js 用于封装处理学生数据的API
exports.findById = function(id, callback) {
  fs.readFile('./db.json', function(err, data) {
    if (err) {
      callback(err);
    }
    
    var students = JSON.parse(data).students;
    var stu = students.find(function(item) {
      item.id === id;
    });
    
    callback(null, stu);
  });
};

exports.updateById = function(student, callback) {
  fs.readFile('./db.json', function(err, data) {
    if (err) {
      callback(err);
    }
    
    student.id = parseInt(student.id); //需要将id转化成int才能正确保存。（其实应该要考虑age也要保存成int）
    var students = JSON.parse(data).students;
    var stu = students.find(function(item) {
      item.id === student.id;
    });
    
    for (var key in student) {
      stu[key] = student[key];
    }
    
    fs.writeFile('./db.json', JSON.stringfy({students: students}), function(err) {
      if (err) {
        callback(err);
      }
      
      callback(null);
    })
  });
};

// 在app.js中如何使用这些封装好的异步API
var Util = require('./students.js');

// 需要注意的是，在html的编辑链接里，href需要带上id这个parameter
app.get('./students/edit', function(req, res) {
  var id = parseInt(req.query.id); // query里的都是string
  
  Util.findById(id, function(err, student) {
    if (err) {
      return res.status(500).send('Server error.');
    }
    
    res.render('edit.art', {student: student});
    // 在edit.art这个渲染文件中，我们需要补充一个tag
    // <input type='hidden' name='id' value={{student.id}}>
    // 来补充一个表单提交需要的参数，但是不会给用户看到的元素
  })
})

app.post('./students/edit', function(req, res) {
  Util.updateById(req.body, function(err) {
    if (err) {
      return res.status(500).send('Server error.');
    }
    
    res.direct('./students');
  })
});
```

###### 完成删除功能

```javascript
// students.js 用于封装处理学生数据的API
exports.deleteById = function(id, callback) {
  fs.readFile('./db.json', function(err, data) {
    if (err) {
      callback(err);
    }
    
    var students = JSON.parse(data).students;
    var deleteIndex = students.findIndex(function(item) {return item.id === id;});
    
    students.splice(deleteIndex, 1); // 从数组中，去掉index开始的n个元素
    
    fs.writeFile('./db.json', JSON.stringfy({students: students}), function(err) {
      if (err) {
        callback(err);
      }
      
      callback(null);
    })
  });
};

// 在app.js中如何使用这些封装好的异步API
var Util = require('./students.js');

// 需要注意的是，在html的编辑链接里，href需要带上id这个parameter
app.get('./students/delete', function(req, res) {
  var id = parseInt(req.query.id); // query里的都是string
  
  Util.deleteById(id, function(err) {
    if (err) {
      return res.status(500).send('Server error.');
    }
    
    res.redirect('./students');
  })
})
```

###### 总结

该案例最主要是训练对增删改查方法（操作数据文件）的函数封装，强调回调函数的重要性。（异步编程）

步骤：

* 处理模板
* 配置开放静态资源
* 配置模板引擎等第三方库
* 简单路由 --> 先渲染静态页面出来
* 路由设计
* 提取路由模块（可以根据路由设计写好接口）
* 需要封装student.js来提供处理文件数据的API
  * 先写文件结构（接口， `exports.save = function(){}`
* 实现具体功能呢
  * 通过路由收到请求
  * 提取请求中的数据
    * get: req.query
    * post: req.body
  * 调用封装在student.js中的API 处理数据
  * 根据操作结果给客户端发送响应数据或跳转

#### Day5

##### 课前反馈

回调函数

基于XMLHttpRequest的ajax api封装

关于js模块化的问题

* js天生不支持模块化
* node中能有模块化是因为commonjs规范
* ES6 module是官方规范下的对js的模块化支持
* 但是因为很多js运行环境还不支持es6，所以需要编译器工具打包之后，才能在低版本浏览器上运行。
* 还有其他一些方法去支持模块化：AMD(require.js), CMD(sea.js), UMD...

为什么挂载router的时候需要用app.use --> 涉及到中间件的概念，在后面会讲到

package-lock.json文件（已加到之前讲包的地方）

ES6里的一些新api，比如find, findIndex --> 练习自己用比较原始的js代码去封装一下这些api

如何在express框架中定制404页面？？ 在自己的路由之后增加一下app.use(中间件)， 会在之后的课讲到。

##### 数据库 MongoDB

[安装](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-os-x/)

[教程](https://www.runoob.com/mongodb/mongodb-tutorial.html)

###### 如何开启和停止数据库

开启： `mongod` (MongoDB默认使用执行mongod命令所处盘符根目录下的/data/db作为数据储存目录)

修改默认储存目录： `mongod --dbpath=数据储存的目录`

停止：`command + c`

连接数据库: `mongo`

###### 基本感知

`show dbs`  查看显示所有数据库

`db` 查看当前操作的数据库

`use 数据库名称` 切换到某个数据库（如果没有，会新建）

插入数据： `db.students.insertOne({"name": "Apple"})` 往当前数据库的students集合插入一条数据

`show collections` 查看当前数据库下的集合

`db.students.find()` 查找students这个集合中所有数据

###### 在Node中如何操作MongoDB数据库

* 使用官方的`mongoDB`包来进行操作（比较原生，写起来复杂一点）

  https://github.com/mongodb/node-mongodb-native

* 使用第三方`mongoose`

  是基于官方出的`mongoDB`包的再一次封装

  https://mongoosejs.com/

  下面是官网举例：

  ```javascript
  const mongoose = require('mongoose');
  // 链接MongoDB数据库
  mongoose.connect('mongodb://localhost:27017/test', {useNewUrlParser: true, useUnifiedTopology: true});
  // 创建一个模型 --> 设计数据库
  const Cat = mongoose.model('Cat', { name: String });
  // 实例化一个Cat
  const kitty = new Cat({ name: 'Zildjian' });
  // 持久化保存数据
  kitty.save().then(() => console.log('meow'));

###### mongoose 开始

* 创建结构，发布模型

```javascript
const mongoose = require('mongoose');
const { Schema } = mongoose;

// 1. 连接数据库
mongoose.connect('mongodb://localhost:27017/your_database');

// 2. 设计文档结构（表结构）
// 字段名称就是表结构中的属性名称
// 增加约束是为了数据的完整性，不要有脏数据
const blogSchema = new Schema({
  title:  String, // String is shorthand for {type: String}
  author: String,
  body:   String,
  comments: [{ body: String, date: Date }],
  date: { type: Date, default: Date.now },
  hidden: Boolean,
  meta: {
    votes: Number,
    favs:  Number
  }
});

const userSchema = new Schema({
  username: {
    type: String,
    required: true
  },
  password: {
    type: String,
    required: true
  },
  email: String
});

// 3. 将文档结构发布为模型
// mongoose.model 用来将一个结构发布为模型
// 第一个参数：大写单数名词表示数据库的名称，mongoose会自动转成小写复数名词作为集合名称
// 第二个参数：架构schema
// 返回值： 模型构造函数
const User = mongoose.model('User', userSchema);

// 4. 我们可以通过模型构造函数对集合中的数据进行增删改查操作
```

https://mongoosejs.com/docs/models.html

API docs: https://mongoosejs.com/docs/api.html

API docs中还有很多相同功能但不同用法的api

* 增加数据

```javascript
const User = mongoose.model('User', userSchema);
// An instance of a model is called a document.
const user = new User({
  username: 'admin',
  password: '1234',
  email: '123@yahoo.com'
});
user.save(function(err, result) {
  if (err) {
    console.log('保存失败');
  } else {
    console.log('保存成功:', result);
  }
})

// OR....
User.create({
  username: 'admin',
  password: '1234',
  email: '123@yahoo.com'
}, function(err, result) {
  xxxx
});
```

* 查询数据

https://mongoosejs.com/docs/queries.html

```javascript
// 查询所有
User.find(function(err, res) {xxxx}); // [{},{},{}]

// 按条件查询所有
User.find({name: 'admin'}, function(err, res) {xxx}); // [{}]

// 按条件查询一个
User.findOne({name: 'admin'}, function(err, res) {xxx}); // {}
```

* 删除数据

```javascript
//删除所有符合要求的数据
User.remove({name: 'admin'}, function(err, res) {xxx});

// 删除符合条件的一个
User.findOneAndRemove({conditions}, [option], [callback]);

// 按照id删除一个
User.findByIdAndRemove(id, [option], [callback]);
```

* 更新数据

```javascript
// 有很多api可以使用，这里以findByIdAndUpdate为例
// https://mongoosejs.com/docs/api.html#model_Model.findByIdAndUpdate
User.findByIdAndUpdate('_id to query', {password: 'abc'}, function(err, res) {xxx});

// 根据条件更新所有
User.update(conditions, doc, [option], [callback]);

// 根据条件更新一个
User.findOneAndUpdate([conditions], [update], [option], [callback]);
```

###### 用数据库重写crud学生管理系统

* 封装数据库模型

```javascript
const mongoose = require('mongoose');
const { Schema } = mongoose;

mongoose.connect('mongodb://localhost:27017/your_database');

const studentSchema = new Schema({
  name: {
    type: String,
    required: true
  },
  age: {
    type: Number,
    required: true
  },
  age: {
    type: Number,
    enum: [0, 1],
    default: 0
  },
  hobbies: {
    type: String
  }
});

module.exports = mongoose.model('Student', studentSchema);
```

* 在路由模块使用数据库模型进行数据操作（之前是去call 自己封装的文件数据操作）

```javascript
const Students = require('./students');

app.get('/students', function(req, res) {
  Students.find(function(err, data) {
    if (err) {
      return res.status(500).send('Server error.');
    }
    
    res.render('index.art', {students: data});
  });
});

app.get('/students/new', function(req, res) {
  res.render('new.art');
});

app.post('/students/new', function(req, res) {
  new Students(req.body).save(function(err) {
    if (err) {
      return res.status(500).send('Server error.');
    }
    
    res.redirect('/students');
  });
});

// 需要注意的是，在html的编辑链接里，href需要带上_id这个parameter
app.get('./students/edit', function(req, res) {
  // 直接access _id是mongoose.Types.ObjectId，会自带双引号，我们还需要想办法去除双引号，再进行数据库findById的操作; try to directly access id when we get the data.(student.id instead of student._id)
  // var id = req.query.id.replace(/"/g, '');
  var id = req.query.id;
  
  Students.findById(id, function(err, student) {
    if (err) {
      return res.status(500).send('Server error.');
    }
    
    res.render('edit.art', {student: student});
    // 在edit.art这个渲染文件中，我们需要补充一个tag
    // <input type='hidden' name='id' value={{student._id}}>
    // 来补充一个表单提交需要的参数，但是不会给用户看到的元素
  })
})

app.post('./students/edit', function(req, res) {
  // var id = req.query.id.replace(/"/g, '');
  var id = req.query.id;
  
  Students.findByIdAndUpdate(id, req.body, function(err) {
    if (err) {
      return res.status(500).send('Server error.');
    }
    
    res.direct('./students');
  })
});

// 需要注意的是，在html的编辑链接里，href需要带上_id这个parameter
app.get('./students/delete', function(req, res) {
  // var id = req.query.id.replace(/"/g, '');
  var id = req.query.id;
  
  Students.findByIdAndDelete(id, function(err) {
    if (err) {
      return res.status(500).send('Server error.');
    }
    
    res.redirect('./students');
  })
})
```

###### 使用nodejs操作MySQL数据库

需要下载一个第三方库`mysql`

[npm mysql介绍](https://www.npmjs.com/package/mysql)

```javascript
var mysql = require('mysql');
// 创建连接
var connection = mysql.createConnection({
  host     : 'localhost',
  user     : 'me',
  password : 'secret',
  database : 'my_db'
});
// 连接数据库
connection.connect();
// 使用connection.query进行数据库增删改查操作
connection.query('SELECT 1 + 1 AS solution', function (error, results, fields) {
  if (error) throw error;
  console.log('The solution is: ', results[0].solution);
});
// 断开数据库
connection.end();
```

##### Promise 补充内容

###### 回调地狱 callback hell

为了控制异步编程的执行顺序（比如说异步函数a必须要在异步函数b有返回值之后才能执行），使用回调嵌套。（代码丑陋，难以维护）

###### 使用Promise解决回调地狱

* 基本语法

**Promise API**

```javascript
import fs from 'fs';

const p = new Promise(function(resolve, reject) {
  fs.readFile('xxxx', function(err, data) {
    if (err) {
      reject(err);
    } else {
      resolve(data);
    }
  })
})
```

在promise构造器中，传入一个异步编程的函数。

一共有三种状态，pending, resolved, rejected

只能从pending转化成resolved或者rejected

resolve对应到then的第一个函数参数

reject对应到then的第二个函数参数或者catch

```javascript
// 接上面block的代码
p.then(function(data) {
  console.log('读取文件成功', data); // 对应到resolve
}, function(err) {
  console.log('读取文件失败', err); // 对应到reject
});
// 或者
p.then(function(data) {
  console.log('读取文件成功', data);
}).catch(err) {
  console.log('读取文件失败', err);
});
```

* **链式then语句**
  * 在then中return的不是promise对象
    * return的值作为下一个then语句的第一个函数的参数
    * 如果没有return，则下一个then语句的第一个函数的参数是undefined
  * 在then中return一个promise对象
    * 下一个then语句的第一个函数就是promise对象的resolve要执行的函数，第二个函数就是promise对象的reject要执行的函数
* 如何解决回调地狱？（优雅的代码）

```javascript
import fs from 'fs';

function pReadFile(filePath) {
  return new Promise(function(resolve, reject) {
    fs.readFile(filePath, function(err, data) {
      if (err) {
        reject(err);
      } else {
        resolve(data);
      }
  })
  });
}

pReadFile('path1').then(function(data) {
  console.log(data);
  return pReadFile('path2');
}).then(function(data) {
  console.log(data);
  return pReadFile('path3');
}).then(function(data) {
  console.log(data);
});
```

###### 应用场景

当一个页面的渲染，需要多个API 的response时。。

###### 封装promise版本的ajax方法

```javascript
function pGet(url, callback) {
  return new Promise(function(resolve, reject) {
    var oReq = new XMLHttpRequest();
    oReq.onload = function() {
      callback && callback(JSON.parse(oReq.responseText));
      resolve(JSON.parse(oReq.responseText));
    };
    oReq.onerror = function(err) {
      reject(err);
    };
    oReq.open('get', url, true);
    oReq.send();
  })
}
```

###### Promise数据库操作

所有数据库操作都是异步的。

mongoose的所有API都支持Promise和callback

举例：

```javascript
// User是MongoDB的一个model
User.findOne({
  name: 'admin'
}).then(function(user) {
  if (user) {
    console.log('已经注册了');
  } else {
    return new User({
      name: 'admin',
      password: 'xx',
      email: 'xx'
    }).save()
  }
}).then(function(err) {
  if (err) {
    console.log('注册失败');
  }
});
```

> Promise参考文档: https://es6.ruanyifeng.com/#docs/promise

#### Day6

##### 多人社区案例

###### 项目结构

`npm init -y`

`git init`

`README.md`

`.gitignore`

`npm i express mongoose`

文件目录：public静态资源文件夹

app.js最基本的功能：开放静态资源，提供`/` 内容，监听端口号

```javascript
var express = require('express');
var path = require('path');

var app = express();

app.use('/public/', express.static(path.join(__dirname, './public/')));
app.use('/node_module/', express.static(path.join(__dirname, './node_module/')));

app.get('/', function(req, res) {
  res.send('index');
});

app.listen(5000, function(req, res) {
  console.log('running on port 5000....');
});
```

###### path模块

`basename` 给定路径的文件名（包含扩展名）

`dirname`  给定路径的目录

`extname`  给定路径的扩展名

`isAbsolute` 判断路径是否绝对路径

`parse`  给定路径，返回一个对路径解析后的对象（一些方法的组合）包含 root, dir, base, ext, name..

`join`  把n个路径，拼接成一个路径（避免手动拼接带来的问题，还能处理问题参数，保证最后拼接出来的路径是正确的）

> 参考文档： https://nodejs.org/api/path.html

###### Node中的其他成员

除了require、exports等模块相关的API之外， 还有：

* __dirname

  动态地获取当前文件模块所属目录的绝对路径

* __filename

  动态地获取当前文件的绝对路径

Node中，在文件操作中，使用相对路径不可靠。因为文件操作中，相对路径是相对于**执行node命令所处的路径**。（而不是相对于文件模块的路径）也就是说，如果在其他路径的文件中，加载另一个模块，有可能因为相对路径的问题，而引起路径不对的问题。

所以我们需要把操作文件的相对路径变为（动态的）绝对路径。

这个时候，我们就需要`__dirname`和`__filename`来获取动态的绝对路径，并且使用`path.join` 实现路径拼接。（避免手动拼接带来的一些低级错误）

> 一般在开发命令行工具，如npm webpack时，这个设计是必须有用的一个特性

**推荐：在操作文件时，都统一使用动态的绝对路径。**

> 模块的路径标识与文件系统的路径标识不同。

###### art-template

`npm i art-template express-art-template`

```javascript
app.engine('html', require('express-art-template'));
app.set('views', path.join(__dirname, './views/'));
```

> 参考官方文档： https://aui.github.io/art-template/docs/syntax.html

模板引擎其实就是帮助替换一些字符串（说实话我觉得react已经完成了这部分组件化的功能）

art-template模板引擎中的include、extend、block

* 子模板：include语法

  用来引入一些公共的元素，比如说header和footer

  标准语法：

  `{{include './head.html'}}`

  ```html
  <body>
    {{include './head.html'}}
    <div>
      Hello World
    </div>
    {{include './footer.html'}}
  </body>
  ```

* 模块继承：extend、block语法

  在被继承的模板html中留下一个变量，可以在继承的模板中填充那个变量。 重复利用模板的很多部分，又可以补充个性化的内容。

  标准语法：

  被继承的模板：

  ```html
  <!--layout.html-->
  <!doctype html>
  <html>
  <head>
    <meta charset="utf-8">
    <title>{{block 'title'}}默认标题{{/block}}</title> <!--默认的网页title-->
  
    {{block 'head'}}
      <link rel="stylesheet" href="main.css">  <!--默认的css文件-->
    {{/block}}
  </head>
  <body>
    {{block 'content'}}默认内容{{/block}}
    {{block 'scripts'}}{{/block}}
  </body>
  </html>
  ```

  继承的模板：

  ```html
  <!--index.html-->
  {{extend './layout.html'}}
  
  {{block 'title'}}{{title}}{{/block}}
  
  {{block 'head'}}
      <link rel="stylesheet" href="custom.css">
  {{/block}}
  
  {{block 'content'}}
    <p>This is just an awesome page.</p>
  {{/block}}
  
  {{block 'scripts'}}
    <script>
      window.alert('hello!');
    </script>
  {{/block}}
  ```

###### 案例的资源页面

上课老师准备好了。使用了上面讲的art-template的子模板和模板继承。

###### 注册登录的路由

* 设计

  | 路径      | 请求方式 | get参数 | post参数                  | 是否需要登录 | 备注         |
  | --------- | -------- | ------- | ------------------------- | ------------ | ------------ |
  | /         | GET      |         |                           |              | 首页         |
  | /register | GET      |         |                           |              | 注册页面     |
  | /register | POST     |         | Email, nickname, password |              | 处理注册请求 |
  | /login    | GET      |         |                           |              | 登录页面     |
  | /login    | POST     |         | email, password           |              | 处理登录请求 |
  | /logout   | GET      |         |                           |              | 处理登出请求 |

* 配置路由

  ```javascript
  // router.js
  var express = require('express');
  var router = express.Router();
  
  router.get('/', function(req, res) {
    res.render('index.html');
  });
  
  router.get('/login', function(req, res) {
    res.render('login.html');
  });
  
  router.post('/login', function(req, res) {
  });
  
  router.get('/register', function(req, res) {
    res.render('register.html');
  });
  
  router.post('/register', function(req, res) {
  });
  
  router.get('/logout', function(req, res) {
  });
  
  module.exports = router;
  ```

  ```javascript
  // app.js
  var router = require('./router');
  
  // ...
  app.use(router); // 挂载router
  ```

* 渲染注册页面

  配置body-parser来获取post请求的参数

* 设计用户数据模型

  ```javascript
  var mongoose = require('mongoose');
  // 连接数据库
  mongoose.connect('mongodb://localhost:27017/test', {useNewUrlParser: true, useUnifiedTopology: true});
  var Schema = mongoose.Schema;
  
  var userSchema = new Schema({
    email: { // 还可以对邮箱格式进行限制
      type: String,
      required: true
    },
    nickname: { // 可以对长度等进行限制
      type: String,
      required: true
    },
    password: { // 可以对长度、能否特殊字符等进行限制
      type: String,
      required: true
    },
    created_time: {
      type: Date,
      default: Date.now
    },
    last_modified_time: {
      type: Date,
      default: Date.now
    },
    avatar: {
      type: String,
      default: '/public/img/default.jpg'
    },
    bio: {
      type: String,
      default: '' // 为了数据的一致性
    },
    gender: {
      type: Number,
      enum: [-1, 0, 1],
      default: -1
    },
    birthday: {
      type: Date
    },
    status: {
      type: Number,
      enum: [0, 1, 2],
      default: 0
    }
  });
  
  module.exports = mongoose.model('User', userSchema); // 数据库名称 users
  ```

###### 处理注册请求

```javascript
router.post('/register', async function(req, res) {
  // 1. 获取post参数
  // 2. 操作数据库
  // 3. 发送响应
  var body = req.body;
  User.findOne({
    $or: [{
      email: body.email
    }, {
      nickname: body.nickname
    }]
  }, function(err, data) {
    // 将响应转化成json
    // 1. 自己手动转换： "{'status': 'ok'}"
    // 2. 利用JSON.stringfy
    // 3. res.json() 接受一个对象，会自动转化成json字符串
    
    // 客户端可以根据error_code和message进行对应的处理
    if (err) {
      res.status(500).json({
        error_code: 500,
        message: 'Internal error'
      });
    }
    
    if (data) {
      res.status(200).json({
        error_code: 1,
        message: 'Username or nickname is already existed'
      });
    }
    
    // 在保存数据时，需要对密码等比较隐私的数据进行加密
    // 可以在npm中搜索server side的加密包
    body.password = md5(md5(body.password));
    new User(body).save(function(err, user) {
      if (err) {
        res.status(500).json({
        	error_code: 500,
        	message: 'Internal error'
      	});
      }
      
      // 学完session之后加的：
      req.session.user = user;
      
      res.status(200).json({
        error_code: 0,
        message: 'ok'
      });
    })
  })
});

// 用es7的async/await语法
router.post('/register', function(req, res) {
  // 1. 获取post参数
  // 2. 操作数据库
  // 3. 发送响应
  var body = req.body;
  try {
    if (await User.findOne({email: body.email})) {
      res.status(200).json({
        error_code: 1,
        message: 'Username is already existed'
      });
    }
    
    if (await User.findOne({nickname: body.nickname})) {
      res.status(200).json({
        error_code: 2,
        message: 'Nickname is already existed'
      });
    }
    
    // 在保存数据时，需要对密码等比较隐私的数据进行加密
    // 可以在npm中搜索server side的加密包
    body.password = md5(md5(body.password));
    await new User(body).save();
    
    res.status(200).json({
        error_code: 0,
        message: 'ok'
    });
  } catch(err) {
    res.status(500).json({
        error_code: 500,
        message: 'Internal error'
    });
  }
});
```

###### 表单同步提交和异步提交

表单具有默认的提交行为，默认是同步的。当表单提交后，浏览器会锁死，等待服务器的响应。服务器的响应会直接覆盖当前页面。（用户体验比较差）

也有办法去解决：在服务端响应的时候，直接响应页面，并且可以保存之前的数据。

在ajax诞生之后，异步提交让用户交互性更好。（同步提交也有优点，服务端处理更安全一些，比如github的注册登录页面）

为了降低前后端耦合，页面的渲染尽量交给前端来做。 所以倾向于表单异步提交。

###### 服务端重定向对于异步请求无效

服务端的重定向`res.redirect` 只对于同步请求有效。 对于异步请求，如果需要页面跳转的话，需要根据请求的响应，然后`window.location.href = xxx` 进行页面跳转。

###### 通过Session保存登录状态

Cookie 可以在客户端用来保存一些不敏感的数据，比如用户名。。

**Session** 可以在服务端保存一些数据用于跟踪用户的状态。 比如登录状态。

由于HTTP协议无状态，所以并不知道是哪个用户操作的，所以服务端要为特定的用户创建了特定的Session，用于标识这个用户，并且跟踪用户。这个Session是保存在服务端的，有一个唯一标识。在服务端保存Session的方法很多，内存、数据库、文件都有。

思考一下服务端如何识别特定的客户？这个时候Cookie就登场了。每次HTTP请求的时候，客户端都会发送相应的Cookie信息到服务端。实际上大多数的应用都是用 Cookie 来实现Session跟踪的，第一次创建Session的时候，服务端会在HTTP协议中告诉客户端，*需要在 Cookie 里面记录*一个**Session ID(sid)**，以后每次请求把这个会话ID发送到服务器，我就知道你是谁了。有人问，如果客户端的浏览器禁用了 Cookie 怎么办？一般这种情况下，会使用一种叫做URL重写的技术来进行会话跟踪，即每次HTTP交互，URL后面都会被附加上一个诸如 sid=xxxxx 这样的参数，服务端据此来识别用户。（客户端拿着钥匙，服务端保存着数据，更安全）

**Express框架本身不支持cookie和session**，我们需要一个第三方库 `express-session`来实现session存储登录状态。

> 文档链接： https://github.com/expressjs/session#readme

`npm install express-session`

```javascript
var session = require('express-session');

//.... 在挂载路由前配置中间件
app.use(session({
  secret: 'keyboard cat',
  resave: false,
  saveUninitialized: true
}))

// 获取session数据
req.session.xxx
// 添加session数据
req.session.xxx = xxx;

// 比如在session中的记录登录状态，可以在返回响应的时候，对渲染的页面进行逻辑判断或者获取变量的值。
// 可以渲染登录用户的用户名等信息
```

解读配置项：

* secret

  配置加密字符串，会在原有加密基础上，和这个字符串拼接起来在进行一次啊加密

  为了增加安全性，防止客户端恶意伪造

* saveUninitialized

  为true时，不论是否使用session，都会在cookie中存下session ID （钥匙）

  为false时，只有在使用session后，才会在cookie中存下session ID

提示： 默认session数据是存在内存里的。服务器重启之后，数据会丢失。真实的产生环境中，我们会对session进行持久化存储。 （比如有插件能帮忙进行数据库存储）

###### 处理完成登录功能

```javascript
router.post('/login', function(req, res) {
  var body = req.body;
  
  User.findOne({
    email: body.email,
    password: md5(md5(body.password))
  }, function(err, user) {
    if (err) {
      res.status(500).json({
        error_code: 500,
        message: 'Internal error'
    	});
    }
    
    if (!user) {
      res.status(200).json({
        error_code: 1,
        message: 'username or password is not valid'
    	});
    }
    
    req.session.user = user;
    // 异步请求，服务端不能使用重定向
    res.status(200).json({
        error_code: 0,
        message: 'ok'
   	});
  })
});
```

###### 处理用户退出功能

```javascript
router.get('/logout', function(req, res) {
  // 清除登录状态
  // 因为是同步请求，可以在服务端进行重定向
  req.session.user = null;
  req.redirect('/login');
});
```

###### 目录结构（其实应该是最一开始要写的）

> app.js  应用的入口文件
>
> controllers
>
> models  数据库mongoose设计的数据模型
>
> node_modules  第三方包
>
> package.json  包描述文件
>
> package-lock.json   第三方包版本锁定文件（npm5之后才有）
>
> public  公共的静态资源
>
> README.md  项目说明文件
>
> routes	业务复杂时，根据业务的分类储存到目录中
>
> views  存储视图目录

##### 中间件

自来水厂生产流程图

处理过程中的每一个小步骤可以抽象成一个中间件。比如说解析请求的body，解析cookie等等。一个个步骤完成之后，再进行业务处理。（中间件可能对于req和res进行改造， 方便后面的业务处理）

中间件的本质就是一个请求处理方法，我们把用户从请求到响应的整个过程分发到多个中间件中去处理，这样做的目的就是提高代码的灵活性，动态可扩展的。

###### express中的中间件

> 官方文档参考： http://expressjs.com/en/guide/using-middleware.html

当请求进来时，会从第一个中间件进行匹配

* 如果匹配，则执行该中间件；
* 如果不匹配，则继续判断匹配下一个中间件

中间件本身就是一个方法，接受三个参数：

* request 请求对象

* response 响应对象

* next 下一个中间件

  next是一个方法，用于**调用下一个可匹配的中间件**

**同一个请求经过的中间件都是同一个请求对象和响应对象**

###### 应用程序级别中间件

* 万能匹配（不关心请求方法和请求路径）

  ```javascript
  app.use(function(req, res, next) {
    console.log(req.url);
    next(); // 执行下一个可匹配的中间件，如果不call这个next，会一直停在这个中间件里
  })

* 只关心路径

  ```javascript
  app.use('/a', function(req, res, next) {
    console.log(req.url); // 该url 不包括一开始的/a
    next();
  })
  ```

###### 路由级别的中间件

* 关心请求路径和请求方法

  ```javascript
  app.get('/', function(req, res, next) {
    res.send('123');
  })
  ```

###### 错误处理的中间件

一般最后两个中间件：

* 处理404的中间件（如果之前没有能匹配的中间件，则进入该中间件）

  ```javascript
  app.use(function(req, res) {
    res.render('404.html');
  });
  ```

* 全局处理错误的中间件（一般是最后一个，出现错误统一处理）

  如果之前某个中间件出现了类似 `next(err)`， 则会直接跳转到该中间件进行错误处理。

  ```javascript
  app.use(function(req, res, next) {
    fs.readFile('xxx', function(err) {
      if (err) {
        next(err);
      }
    });
  });
  ```

  **必须接受四个参数！！！**(否则会跟其他中间件混淆)

  ```javascript
  app.use(function(err, req, res, next) {
    console.log(err.message);
    console.log(err.stack);
    res.status(500).send('Internal error!');
  });
  ```

###### 内置中间件

express.static

express.json

express.urlencoded

###### 第三方中间件

> 此处列举了官方推荐的express第三方中间件： http://expressjs.com/en/resources/middleware.html

body-parser

compression

cookie-parser

express-seesion

等等



















