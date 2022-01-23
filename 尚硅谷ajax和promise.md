# Ajax

**Asynchronous JavaScript and XML**

在网页**不刷新**的情况下，发起请求。可以在浏览器中向服务器发送异步请求。

## 介绍

### ajax

### XML

* 可扩展标记语言
* 被设计来传输和存储数据
* 与HTML类似，但是HTML里都是预定义标签（用来呈现数据），但XML都是自定义标签

**现在已经被JSON取代。**（简洁且方便转换）

### 优缺点

#### 优点

* 无须页面刷新就能与服务器进行通信
* 允许根据用户时间来更新部分页面内容

#### 缺点

* 没有浏览历史，不能回退

* 存在**跨域**问题（同源策略）

* SEO不友好 

  因为网页爬虫爬不到了，信息都是异步请求的

## Http协议

hyper text transport protocol  超文本传输协议

协议详细规定了浏览器和万维网服务器之间互相通信的规则。

### 请求（报文）

重点是格式和参数

* 请求行
  * 请求方式 GET/POST
  * url路径（含参数）
  * 协议版本 HTTP/1.1 2.0
* 请求头
  * Host
  * Cookie
  * Content-type （可能是application/x-www-form-urlencoded)
  * User-Agent
* 请求空行（必须有）
* 请求体
  * GET没有请求体
  * POST可以有请求体

### 响应（报文）

* 行
  * 协议版本 HTTP/1.1 2.0
  * 响应状态码
    * 401
    * 403
    * 404
    * 500
    * 200
  * 响应状态字符串
* 头
  * Content-length: 2048（举例）
  * Content-type （可能是text/html;charset=utf-8
  * Content-encoding  压缩方式（gzip）
  * **Access-Control-Allow-Origin**  设置允许跨域（的域名）。 * 表示通配符
* 空行（必须有）
* 响应体

### console - network查看通信报文

请求和响应头

view raw data 能看到请求行/响应行

响应体有自己的tab

## ajax请求

### 原生ajax

#### 基本操作

```javascript
// 1. 创建对象
const xhr = new XMLHttpRequest();
// 2. 初始化 设置请求方法和url
xhr.open('GET', 'https://127.0.0.1/server');
// 3. 发送请求
xhr.send();
// 4. 为请求绑定事件，处理服务器返回的结果
xhr.onreadystatechange = function() {
	if (xhr.readystate === 4) {
    // 2xx 都表示成功
    if (xhr.status >= 200 && xhr.status < 300) {
      // 处理 行，头，空行，体
      console.log(xhr.status); // 响应状态码
      console.log(xhr.statusText); // 响应状态字符串
      console.log(xhr.getAllResponseHeaders()); // 所有响应头
      console.log(xhr.response); // 响应体
    }
  }
};
```

readystate是xhr对象中的属性，表示状态0，1，2，3，4（未初始化，open调用完毕，send调用完毕，服务端返回部分结果，服务端返回所有结果）

#### 设置请求参数

`xhr.open('GET', 'https://127.0.0.1/server?xx=jj&yy=jj');`  直接加在url上

#### 发送post请求

初始化：`xhr.open('POST', url);`

设置请求体： `xhr.send('a=100&b=200');`   放到send方法里

post请求体的格式没有限制，只要后端能handle就行。（最常见是上面那种，也有formData的类型）

#### 设置请求头信息

`xhr.setRequestHeader(名字，内容)`

比如： `xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded')`

> 除了预定义的请求头，也可以自定义请求头。但这种请求一般会被拒绝，需要后端进行其他实现
>
> 比如需要设置响应头: response.setHeader('Access-Control-Allow-Header', '*'); 用来定义可接受的自定义请求头
>
> 同时由于浏览器会再发一个OPTIONS请求进行权限校验，所以可能需要app.post --> app.all

#### 服务端响应JSON数据

（一般也是向服务端发送JSON数据）

我们需要传输字符串，所以以下这组API能帮助我们在JSON对象和字符串之间手动转换。

* JSON.stringify(data对象)
* JSON.parse(string)

我们也可以在客户端自动转换

* xhr.responseType = 'json'

  就不需要再手动JSON.parse(string)

#### nodemon

一个帮助自动重启（有文件改变时）基于nodejs的server application的工具

#### IE浏览器缓存问题

在IE浏览器中，对一个相同的请求，请求多次，返回的都是缓存的结果。这样，对于那些时效性强的api请求就不太合理。

一个解决办法：增加唯一的参数，使每次请求对浏览器来说都不一样

`xhr.open('GET', 'url?t=' + Date.now());`

其他浏览器不存在这个问题

#### ajax请求超时与网络异常

```javascript
xhr.timeout = 2000; // 设置超时
xhr.ontimeout = function() {
  // 请求超时的回调函数
};
xhr.onerror = function() {
  // 网络异常的回调函数
}
// ontimeout 和 onerror 都会自动取消请求
```

#### ajax取消请求

`xhr.abort()`  手动取消请求

#### ajax请求重复发送问题

可以用一个变量记录是否已经有一个相同的请求正在sending

```javascript
let isSending = false;
let xhr;

xx.onclick = function() {
  if (isSending) {
    xhr.abort();
  }
  
  xhr = new XMLHttpRequest();
  xhr.open('GET', url);
  xhr.send();
  isSending = true;
  xhr.onreadystatechange = function() {
    if (xhr.readystate === 4) {
      isSending = false;
      // xxxx
    }
  }
}
```

### jQuery中的ajax

```javascript
$('button').eq[0].click(function() {
  // 第一个参数是请求路径
  // 第二个参数是 请求携带的参数
  $.get(url, {a: 100, b: 200}, function(data) {
    // data是响应体 xhr.response
  }, 'json'); // 第四个参数可选，如果是json的话，会直接将响应体转换成json对象，不然默认是字符串
};
```

```	javascript
$('button').eq[0].click(function() {
  // 第一个参数是请求路径
  // 第二个参数是 请求携带的参数
  $.post(url, {a: 100, b: 200}, function(data) {
    // data是响应体 xhr.response
  }, 'json'); // 第四个参数可选，如果是json的话，会直接将响应体转换成json对象，不然默认是字符串
};
// post请求时，第二个参数不会直接加在url后面，而是作为请求体发送
```

以上两种比较简单好用，但是如果需要更个性化的话，需要一个通用版本：

```javascript
$.ajax({
  // url
  url: 'xxx/jjj',
  // 参数
  data: {a: 100, b : 200},
  // 请求类型
  type: 'GET',
  // 响应体结果（可选）
  dataType: 'json',
  // 成功的回调函数
  success: function(data) {
    
  },
  // 超时设置
  timeout: 2000,
  // 失败的回调函数（包括超时和网络异常）
  error: function() {
    
  },
  // 头信息
  headers: {
    c: 200
  }
})；
```

### axios

目前最为流行的ajax工具包（react和vue推荐使用）

axios有很多请求配置项，比jquery有更多可以设置的东西，具体可见doc：https://axios-http.com/docs/intro

Automatic transforms for JSON data --> 自动转换json数据

axios各种请求的方式，参数设置：https://axios-http.com/docs/instance

可设置的额外config： https://axios-http.com/docs/req_config

```javascript
// Request method aliases
axios.get(url, config);
axios.get(url, {
  params: {
    aa: 1,
    bb: 3
  },
  headers: {
    // 各种头信息
  }
}).then(function(value) {
  // value.data 才是响应体
  // value的schema --> https://axios-http.com/docs/res_schema
});

// 可以设置defaults的配置 -> https://axios-http.com/docs/config_defaults
axios.defaults.baseURL = 'https://api.example.com';

axios.post(url, data, config); // data是请求体
axios.get(url, {username: xxx, password: xxx}, {
  params: {
    aa: 1,
    bb: 3
  },
  headers: {
    // 各种头信息
  }
}).then(function(value) {
});
```

Axios API: (以上例子都是axios的instance api)

```javascript
axios({
  url: xxx, // 请求路径
  method: 'POST', // 请求方式
  params: {
    // 请求路径参数
  },
  headers: {
    // 头信息
  },
  data: {
    // 请求体
  }
}).then(response => {
  // in response, there are status, statusText, headers, config, data....
})
```

### fetch

fetch第二个参数可配置的参数： https://developer.mozilla.org/zh-CN/docs/Web/API/fetch#%E5%8F%82%E6%95%B0

注意body请求体可接受的格式

```javascript
fetch('url with params', {
  method: 'POST',
  headers: {
    
  },
  body: 'username=admin&password=admin',
  // credentials:
}).then(response => {
  // 如果需要得到response的响应体
  // 则需要 response.json() / response.text() --> 也是返回一个promise
})
```

## 跨域

### same-origin policy

同源策略是一种浏览器的安全策略，**协议、域名、端口号必须完全相同**。

ajax也是默认遵循同源策略的。

违背了同源策略就是跨域 --> 经常出现

### jsonp

json with padding，非官方的解决跨域的方式，只支持get请求。

script标签本身就可以**跨域加载异域的js并执行**的。

**原理：**

添加script标签，并利用script标签可以跨域加载js执行的特性，后端返回的是一段js代码（一般就是调用前端的一个函数，并将准备好的参数传入，然后<u>以字符串的形式</u>返回）

#### 原生jsonp请求

```html
<script>
  function handle(data) {
    // xxx
  }
  
  input.onblur = function() {
    const script = document.createElement('script');
    
    script.src = `url?username=${this.value}`; // 向后端发jsonp请求，可以加上回调函数的名字
    document.appendChild(script);
  }
</script>
```

在后端，应该是最终是类似这样的代码：

```javascript
response.end(`handle(${JSON.stringify(data)})`); //可以在请求时，传入回调函数名，然后再此处返回时代替这个handle
```

#### jQuery发送jsonp请求

```javascript
$.getJSON('url?callback=?', function(data){ // callback=?是jQuery的固定写法
  // data就是返回的数据
})
```

后端应该是

```javascript
let cb = req.query.callback;

res.end(`${cb}(${JSON.stringify(data)})`);
```

### CORS

Cross-origin resource sharing 跨域资源分享

CORS是**官方**的跨域解决方案，特点是**不需要在客户端做任何特殊的操作，完全在服务器中进行处理。**

支持包括get和post的所有请求。

CORS标准新增了一组HTTP首部字段，允许服务器声明哪些源站通过浏览器有权限访问哪些资源。

#### 如何工作

CORS是通过设置响应头来告诉浏览器，该请求允许跨域，浏览器收到该响应头之后就会对响应进行放行。

**可操作的响应头**： https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#the_http_response_headers

一般需要配置：

Access-Control-Allow-Origin

Access-Control-Allow-Methods

Access-Control-Allow-Headers











# Promise

## 介绍和基本使用

### 什么是Promise

es6引入的用来解决**异步编程**的新的解决方案。（旧方案是使用回调函数）

* 文件处理
* 数据库操作
* ajax
* 定时器

可以解决回调地狱的问题，并能更好的处理error

从语法上来说，Promise是一个构造函数；从功能上来说，Promise对象用来封装一个异步操作并可以获取其成功或失败的结果值。

### 为什么要用Promise

1. 支持链式调用，可以解决回调地狱问题

   回调地狱：可读性差；异常处理能力差

2. 指定回调函数的方式更灵活

   可以在异步任务开始后（甚至结束后）绑定回调函数

### promise初体验

```javascript
// 步骤就是创建一个Promise的对象，把异步操作封装在构造函数中，然后择机调用resolve或reject
const p = new Promise((resolve, reject) => {
  // 异步操作，然后最终调用resolve或reject将promise的状态设为成功或失败
  setTimeout(() => {
    resolve(); // resolve和reject被调用时，传入的参数为then中回调函数的实参
  }, 1000);
});

p.then(value => {}, error => {}); // 第一个参数是resolve后的回调，第二个参数是reject后的回调
```

### promise实践练习

1. 用Promise对fs模块方法进行调用

   ```javascript
   const fs = require('fs');
   
   fs.readFile(url, function(err, data) {
     // 回调函数的形式
   });
   
   // 用Promise进行封装
   const p = new Promise((resolve, reject) => {
     fs.readFile(url, function(err, data) {
     	if (err) {
         reject(err);
       } 
       resolve(data);
   	});
   });
   
   p.then(value => {}, err => {});
   ```

2. Ajax

### Promise封装练习

1. fs模块方法封装

   ```javascript
   const fs = require('fs');
   
   function mineReadFile(path) {
     return new Promise((resolve, reject) => {
       fs.readFile(path, function(err, data) {
     		if (err) {
         	reject(err);
       	} 
       	resolve(data);
       };
     });
   }

2. ajax封装（此处只做get）

   ```javascript
   function sendGetAjax(url) {
     return new Promise((resolve, reject) => {
       const xhr = new XMLHttpRequest();
       
       // xhr.responseType = 'json'; 
       xhr.open('GET', url);
       xhr.send();
       xhr.onreadystatechange = function() {
   			if (xhr.readystate === 4) {
       		if (xhr.status >= 200 && xhr.status < 300) {
             resolve(xhr.response);
           } else {
             reject(xhr.status);
           }
     		}
   		};
     });
   }
   ```

### util.promisify()

可以自动把一个回调函数模式的异步方法转换成promise模式的异步方法。 前提是 最后一个参数是 `(err, data) => {}` 类型的回调函数。

https://nodejs.org/dist/latest-v8.x/docs/api/util.html#util_util_promisify_original

只能在nodejs环境下使用

### promise的状态

pending, resolved(fulfilled), rejected

只能从pending变为resolved或者从pending变为rejected，且一个promise对象的状态只能改变一次。

Promise对象中有两个属性：

* **PromiseState**

* **PromiseResult**

  保存着promise对象成功或失败的结果

## Promise API

#### 基本流程

![Screen Shot 2021-11-10 at 10.03.03 PM](file:///Users/hualinfeng/Library/Application%20Support/typora-user-images/Screen%20Shot%202021-11-10%20at%2010.03.03%20PM.png?lastModify=1636610657)

#### 构造函数，then，catch

1. 构造函数

   Promise(executor)  => Promise

   **executor会在promise内部立即同步调用**

2. Promise.prototype.then

   (onResolved, onRejected) => Promise

   返回一个新的promise对象

3. Promise.prototype.catch

   只能接受失败的回调函数，其实也是调用的then方法

#### Promise.resolve

(value) => Promise

快速返回一个**成功或失败**的promise对象

* value为一个非promise类型的值，则返回的是一个成功的promise对象，且**值为value**
* value为一个promise对象，则返回的promise对象取决于value的状态和结果值。如果value最终是个成功的promise，则Promise.resolve(value)也返回一个成功的promise且值为**value返回的值**；反之一样，失败的promise对象。

#### Promise.reject

(value) => Promise

快速返回一个**失败**的promise对象

且不管value是什么类型，promise的**值都为value**（value也可能是一个promise对象）

#### Promise.all

Array of promises => Promise

返回一个promise对象

只有所有promises都成功了，返回的promise对象才是fulfilled的，且值为每个promise返回值组成的array

只要一个promise失败，返回的就是一个rejected的promise对象，且值为第一个失败的promise的返回值

```javascript
const p1 = Promise.resolve(1);
const p2 = Promise.resolve(2);
const p3 = Promise.reject(3);
const p4 = Promise.reject(4);

Promise.all([p1, p2])
.then(([p1Res, p2Res]) => {
  console.log(p1Res); // 1
  console.log(p2Res); // 2
});

Promise.all([p1, p3])
.catch(reason => {
  console.log(reason); // 3
});

Promise.all([p4, p3])
.catch(reason => {
  console.log(reason); // 4
});
```

#### Promise.race

Array of promises => Promise

返回一个promise对象

第一个完成状态转换的promise的状态，就是返回的promise对象的状态，值也是第一个完成状态转变的promise的结果

```javascript
const promise1 = new Promise((resolve, reject) => {
  setTimeout(resolve, 500, 'one');
});

const promise2 = new Promise((resolve, reject) => {
  setTimeout(resolve, 100, 'two');
});

Promise.race([promise1, promise2]).then((value) => {
  console.log(value);
  // Both resolve, but promise2 is faster
});
// expected output: "two"

```

#### Promise.allSettled

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/allSettled

Array of promises => Promise

返回一个promise对象，当所有传入的promises已经resolved或rejected后，返回的promise对象的状态会异步地变成fulfilled，且值为每个promise对应的结果：{status, value} 或者 {status, reason} 取决于该promise的状态。

当且仅当，input是一个空array，Promise.allSettled会直接返回一个成功的promise，且值为空数组。

通常，当多个不互相依赖的异步操作 或者 想知道每个promise的结果时，我们会用Promise.allSettled

如果任务有互相依赖，或者希望当一个promise失败后，马上拒绝所有还在进行的promises，我们可以考虑使用Promise.all

```javascript
Promise.allSettled([
  Promise.resolve(33),
  new Promise(resolve => setTimeout(() => resolve(66), 0)),
  99,
  Promise.reject(new Error('an error'))
])
.then(values => console.log(values));

// [
//   {status: "fulfilled", value: 33},
//   {status: "fulfilled", value: 66},
//   {status: "fulfilled", value: 99},
//   {status: "rejected",  reason: Error: an error}
// ]
```

## 关键问题

1. 如何改变promise的状态？

   * resolve
   * reject
   * throw error

2. 一个promise对象指定多个回调函数（成功或失败），都会调用吗？

   是的！都会调用。

   ```javascript
   const p = Promise.resolve(1);
   
   p.then(value => console.log(value)); // 1
   p.then(value => console.log(value)); // 1
   p.then(value => console.log(value)); // 1
   
   // 注意区别于 p.then(value => console.log(value)).then(value => console.log(value)); 1, undefined
   ```

3. 改变状态和指定回调，先后顺序？

   都有可能，取决于promise执行器里的任务。

   如果马上能改变状态或者状态改变早于 .then ，则先改变状态，后指定回调；

   一般来说都是先指定回调，再改变状态，promise执行器的任务大概率是异步代码。

4. **promise.then() 返回的新promise的结果状态由什么决定**？

   取决于then里指定的**回调函数执行的结果**

   * 抛出异常：新promise的状态变为rejected，reason为抛出的error

   * 返回的是非Promise类型的值： 新promise的状态变为resolved，value为返回的值

     ```javascript
     Promise.resolve(1)
     .then(value => value + 1)
     .then(value => console.log(value)); // 2
     ```

   * 返回的是一个promise对象：此promise的结果就会成为新promise的结果（包括状态和结果值）

     ```javascript
     Promise.resolve(1)
     .then(value => {
       console.log(value); // 1
       
       return Promise.reject(2);
     })
     .then(value => console.log(value), reason => console.log('reason: ', reason)); // reason: 2
     ```

5. 串联多个任务

   promise的链式调用 + then返回的也是一个promise对象（且基于第四点的规则）

6. 异常穿透

   当使用promise的then链式调用时，可以在最后指定失败的回调。 --> 异常穿透现象

   前面任何操作出现问题，都会传到最后的失败的回调中统一处理。

7. 如果中断promise链

   有且只有一个方式：在需要中断的地方，return 一个 pending状态的promise对象

   比如： `return new Promise(() => {});`

## 自定义封装 --> 面试必考题

### 封装流程：

1. 初始结构搭建
2. resolve和reject结构搭建
3. resolve和reject代码实现
4. throw抛出异常改变状态
5. promise状态只能修改一次
6. then方法执行回调（同步任务）
7. 异步任务回调的执行（是**在状态改变时才去call回调函数**，仅能在resolve和reject方法中改变状态）
8. 指定多个回调的实现（**不是**then链式调用）
9. 同步修改状态，then方法的返回结果 （关键问题中的第四点，**then返回的promise的状态和结果怎么确定？**）
10. 异步修改状态，then方法的返回结果（注意我们之前的结构，异步的状态修改是在resolve和reject中进行的，所以我们需要自定义一下this.callbacks里的回调函数，不仅是要call传入的回调函数，而且还要保证then方法的返回结果是正确的）
11. then方法的完善和优化
12. catch方法，异常穿透，值传递 （onResolved, onRejected回调没有传入）
13. Promise.resolve
14. Promise.reject
15. Promise.all （**只要是promise，一定能调then方法**，获取状态和值）
16. Promise.race
17. then方法的回调函数是异步调用的
18. class版本的实现

### 最终版code：

```javascript
// Promise是这样被使用的
let p = new Promise((resolve, reject) => { // 这里是形参
  resolve('ok');
  // reject('error');
})

p.then(value => {
  console.log(value);
}, reason => {
  console.log(reason);
})
//.catch(error) {console.log(error);}

// 自定义Promise
// 声明构造函数
function Promise(executor) {
  this.PromiseState = 'pending';
  this.PromiseResult = null;
  // callbacks是异步任务完成后需要call的回调函数
  this.callbacks = [];
  
  // 注意resolve和reject都是函数直接调用的模式，this指向的是window
  // 我们可以利用闭包存一下this，或者在下面传入executor执行器前，使用bind绑定this内容
  const self = this;
  
  function resolve(data) {
    // 判断当前promise对象的状态，只能修改一次状态
    if (self.PromiseState !== 'pending') {
      return;
    }
    
    // 1, 修改对象状态
    self.PromiseState = 'fulfilled';
    // 2, 修改结果值
    self.PromiseResult = data;
    
    // 如果有需要call的回调函数，需要在状态改变时call （回调函数一般是then方法中被赋予的）
    self.callbacks.forEach(cb => {
      // 回调函数是异步执行的（需要进callback queue）
      setTimeout(() => {
        cb.onResolved(data);
      }, 0);
    });
  }
  
  function reject(reason) {
    // 判断当前promise对象的状态，只能修改一次状态
    if (self.PromiseState !== 'pending') {
      return;
    }
    
    // 1, 修改对象状态
    self.PromiseState = 'rejected';
    // 2, 修改结果值
    self.PromiseResult = reason;
    
    // 如果有需要call的回调函数，需要在状态改变时call （回调函数一般是then方法中被赋予的）
    self.callbacks.forEach(cb => {
      // 回调函数是异步执行的（需要进callback queue）
      setTimeout(() => {
        cb.onRejected(reason);
      }, 0);
    });
  }
  
  // 注意执行器executor是在promise对象被创建时，同步被调用的
  // 执行器函数执行时，抛出异常，应改变promise状态
  try {
    executor(resolve, reject);
  } catch (error) {
    reject(error);
  }
}

// 添加then方法
Promise.prototype.then = function(onResolved, onRejected) {
  const self = this; // 避免丢失this执行上下文对象
  
  // 需要判断是否传入了onResolved, onRejected
  // 实现异常穿透
  if (typeof onRejected !== 'function') {
    onRejected = reason => {
      throw reason; // 直到某一层有onRejected 或 到catch里处理
    };
  }
  // 实现值传递
  if (typeof onResolved !== 'function') {
    onResolved = value => value; // 直接将value传递给下一个then
  }
  
  return new Promise((resolve, reject) => {
    // 封装一下 对then方法返回的promise的对象的状态和值 的方法
    function thenReturnedPromise(cb) {
      try {
        let result = cb(self.PromiseResult);
      
        if (result instanceof Promise) {
          // 回调函数返回的是一个Promise对象，则then返回的promise对象状态由回调函数返回的Promise对象决定
          // 调用then方法可以得到回调函数返回的Promise对象的状态和值， 从而确定then需要返回的Promise对象的状态和值
          result.then(data => {
            resolve(data);
          }, reason => {
            reject(reason);
          });
        } else {
          // 回调函数返回的是一个非Promise值
          resolve(result);
        }
      } catch (e) {
        // 回调函数中有throw error
        reject(e);
      }
    }
    // 处理同步任务，promise的executor中 没有异步任务，then被call时，状态已经改变
    if (this.PromiseState === 'fulfilled') {
      // 回调函数是异步执行的（需要进callback queue）
      setTimeout(() => {
        thenReturnedPromise(onResolved);
      }, 0);
    }

    if (this.PromiseState === 'rejected') {
      // 回调函数是异步执行的（需要进callback queue）
      setTimeout(() => {
        thenReturnedPromise(onRejected);
      }, 0);
    }

    // 以下处理异步任务，在then被call时，状态还未改变
    // 我们需要把回调函数保存起来，当状态改变时再去执行回调；为了实现then返回的promise的状态和结果，需要对回调函数进行加工
    // 同时，我们还需要处理 指定多个回调的情况， this.callbacks.push
    if (this.PromiseState === 'pending') {
      this.callbacks.push({
        onResolved: function() {
          thenReturnedPromise(onResolved);
        },
        onRejected: function() {
          thenReturnedPromise(onRejected);
        }
      });
    }
  });
};

// 添加catch方法
Promise.prototype.catch = function(onRejected) {
  return this.then(undefined, onRejected);
}

// 添加 static resolve方法
Promise.resolve = function(value) {
  return new Promise((resolve, reject) => {
    if (value instanceof Promise) {
      value.then(data => {
        resolve(data);
      }, reason => {
        reject(reason);
      });
    } else {
      resolve(value);
    }
  });
}

// 添加 static reject方法
Promise.reject = function(reason) {
  return new Promise((_, reject) => {
    reject(reason);
  });
}

// 添加static all方法
Promise.all = function(promises) {
  return new Promise((resolve, reject) => {
    let count = 0;
    let res = [];
    
    promises.forEach((promise, index) => {
      promise.then(data => {
        // 进入这里说明promise已经成功
        count++;
       	res[index] = data;
        
        if (count === promises.length) {
          resolve(res);
        }
      }, reason => {
        // 进入这里说明promise已经失败
        reject(reason);
      });
    });
  });
}

// 添加static race方法
Promise.race = function(promises) {
  return new Promise((resolve, reject) => {
    promises.forEach(promise => {
      promise.then(data => {
        // 进入这里说明promise已经成功
        resolve(res);
      }, reason => {
        // 进入这里说明promise已经失败
        reject(reason);
      });
    });
  });
}

// 添加static allSettled方法
Promise.allSettled = function() {
  // 我觉得写法跟上面几个差不多，肯定也是调用then方法
}
```

### class版本结构：

```javascript
class Promise {
  constructor() {
    
  }
  
  then(onResolved, onRejected) {
    
  }
  
  catch(onRejected) {
    
  }
  
  static resolve(value) {
    
  }
  
  static reject(reason) {
    
  }
  
  static all(promises) {
    
  }
  
  static race(promises) {
    
  }
  
  static allSettled(promises) {
    
  }
}
```

## async/await

### async函数

返回值是一个promise对象，且该promise对象的结果由async函数执行的<u>返回值</u>决定。（规则跟then的返回的promise的状态和结果的规则一样。

```javascript
async function test() {
  
}

let x = test();
console.log(x);
```

### await表达式

await右侧的表达式一般为一个promise对象，但也可以是其他值。

* 如果是promise对象，await返回的是promise成功的值。
* 如果是其他值，则await直接返回该值。

注意：

* await必须写在async函数中，但是async函数中可以没有await表达式
* 如果await的promise失败了，就会抛出异常。我们需要通过try catch语句捕获

### async和await的结合使用

代码变得非常好看，可读性很强。

拿到成功的promise的数据非常容易。（不用很长的then链式调用，更不用向回调函数一样麻烦）

```javascript
const myPromise = new Promise(resolve => setTimeout(() => resolve(100), 1000));

async function main() {
  const data = await myPromise;
  
  console.log(data);
}

main();
```















































