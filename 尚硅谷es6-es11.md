##  EcmaScript

是脚本语言的规范，js是es的一种实现。

ECMA - 欧洲计算机制造商协会，目标是评估、开发、认可电信和计算机标准。

ECMAScript是Ecma国际通过ECMA-262标准化的脚本程序设计语言。

TC39是推进EcmaScript发展的委员会。

兼容性问题 --> Babel

## ES6

### let

1. 变量不能重复声明（可以不赋值）
2. 块级作用域，作用域以 {} 代码块为准 （es5里是全局作用域，函数作用域）
3. 不存在变量提升（与var不同）
4. 不影响作用域链

案例： for循环 加 回调函数  --> i的问题 （还是作用域问题，var赋给了window，let是块级作用域）

### const

定义常量

1. 一定要赋初始值
2. 一般常量使用大写（convention）
3. 常量的值不能修改
4. 块级作用域
5. 对于数组和对象的元素修改，不算做对常量的修改（数组或对象的地址值不能修改）

### 解构赋值

ES6允许按照一定模式从数组或对象中提取值，对变量进行赋值。

1. 数组

   ```javascript
   const arr = [1,2,3,4];
   let [a,b,c] = arr;
   ```

2. 对象

   ```javascript
   const obj = {a: 1, b: 2, c: new Function()};
   let {a, b, c} = obj;
   ```

### 模板字符串

反引号 ``

1. 字符串声明

2. 内容中可以直接出现换行符 （写html tags时会非常方便，可读性好）

3. 变量拼接

   ```javascript
   const name = 'linfeng';
   
   console.log(`${name}最帅`);
   ```

### 对象的简化写法

ES6 允许在对象的大括号里，直接写入**变量和函数**，作为对象的属性和方法

```javascript
const name = 'linfeng';
const fun1 = function() {};

const obj = {
  name,
  fun1,
  fun2(num1) {
    console.log(num1);
  }
};
```

### 箭头函数

ES6允许使用箭头来定义函数

```javascript
let fun = function() {};
let fun1 = () => {};
```

1. **this是静态的，this始终指向函数声明时所在的<u>作用域</u>下的this的值** （不取决于如何调用这个函数）

   ```javascript
   const a = {name: 'linfeng'};
   
   window.name = '123';
   
   const fun = () => {console.log(this.name)};
   fun();//123
   fun.call(a); //123
   ```

2. 不能作为构造函数实例化对象

3. 不能使用arguments变量

4. 箭头函数的简写

   1. 省略小括号 --> 当形参有且只有一个时

   2. 省略花括号 --> 当代码块只有一条语句时

      此时，return也必须省略，并且语句的执行结果就是函数的返回值

对于箭头函数的**this是静态**的实践：

```javascript
const btn = document.getElementById('test');

btn.addEventListener('click', () => {
  console.log(this); // window
});

btn.addEventListener('click', function() {
  console.log(this); // button
});

btn.onclick = function() {
  console.log(this); // button
}

btn.onclick = () => {
  console.log(this); // window
}
```

实践2：取偶数

```javascript
const result = arr.filter(item ==> item % 2 === 0);
```

箭头函数适合什么？

* 比较适合与this无关的回调，比如定时器，数组方法的回调
* 其实在React中也适合做与this相关的方法。 （React中绑定事件响应函数时，this会丢失，这时使用箭头函数，反而因为其静态属性，能不丢失this内容）

不适合：

* 事件回调 （使用function定义时，this指向事件源）

* 对象的方法

  ```javascript
  const obj = {
    name: '123',
    getName: () => this.name // 应该用function来定义
  }
  
  console.log(obj.getName()); // 取的是window下的name属性
  ```

### 参数默认值

1. 允许给函数参数赋值初始值

   ```javascript
   function add(a, b, c = 10) {
     // xxx
   }
   ```

​		一般把有默认值的参数放到靠后位置

2. 可以与解构赋值结合

   ```javascript
   const {name, age = 12} = obj;

### rest参数

ES6 引入了rest参数，用于获取函数的实参，来代替arguments

rest是一个数组，且必须要放到最后

```javascript
function fun(a, b, ...args) {
  // args是一个数组，保存着除了a和b之外的实参
}
```

也可以用于解构赋值

```javascript
const arr = [1,2,3,4];
const [a, b, ...rest] = arr;

console.log(rest); // [3, 4]
```

### spread扩展运算符

`...`

与rest参数看上去很像，但是使用的位置不同

spread运算符能将数组转化成逗号分隔的参数序列

```javascript
const arr = [1,2,3];
const fun = function() {};

fun(...arr);
```

**ES9**中也为对象提供了像数组一样的rest参数和扩展运算符

应用：

1. 数组的合并

   `[...arr1, ...arr2]`

2. 数组的克隆

   `[...tobecloned]` 对数组的内容是shallow clone

3. 将伪数组转为真正的数组

   ```javascript
   const nodes = document.querySelector('xxx');
   
   const nodesArr = [...nodes];
   ```

### Symbol

ES6引入的一种新的primitive type的数据类型，表示独一无二的值。

是js的第七种数据类型，类似字符串。

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Symbol

#### 特点

1. Symbol的值是唯一的，用来解决命名冲突的问题
2. 不能与其他数据进行运算
3. Symbol定义的对象属性不能用for in循环遍历，但是可以使用Reflect.ownKeys来获取对象的所有keys

#### 创建Symbol

```javascript
let s = Symbol(); // symbol的唯一性 对我们不可见，但在内部已经实现了
typeof s; // 'symbol'

let s2 = Symbol('abc'); // abc只是描述这个symbol：an optional string as its description
let s3 = Symbol('abc');
s2 !== s3; // true

let s4 = Symbol.for('key');
let s5 = Symbol.for('key');
s4 === s5; // true 

// Every Symbol() call is guaranteed to return a unique Symbol.
// Every Symbol.for("key") call will always return the same Symbol for a given value of "key". When Symbol.for("key") is called, if a Symbol with the given key can be found in the global Symbol registry, that Symbol is returned. Otherwise, a new Symbol is created, added to the global Symbol registry under the given key, and returned.
```

#### 回顾一下js中的数据类型

USONB

u: undefined

s: string, symbol

o: object

n: null, number

b: boolean

#### Symbol的使用

给对象添加属性和方法，表示独一无二的。（防止可能出现的命名重复带来的问题）

可以非常**高效且安全**地给对象**添加属性和方法**。

```javascript
const obj = {up: '1', down: '2'};
const methods = {
  up: Symbol(),
  dowm: Symbol()
};

obj[methods.up] = function() {};
obj[methods.down] = function() {};

// 或者直接添加
const s = Symbol('1');
const obj2 = {
  [s]: function(){}
};
obj[s]();
```

#### Symbol内置属性

ES6还提供了一些内置的symbol的属性的值，指向语言内部使用的方法。

通过对**对象**进行这些属性值的设置，可以改变对象在**特定场景下的表现结果**。（不是主动去调用，而是在某些情况下自动被调用）

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Symbol#static_properties

* **Symbol.hasInstance**

  当其他对象使用instanceof运算符，判断是否为该对象的实例对象时，会调用这个方法

  ```javascript
  class Person{
    static [Symbol.hasInstance](params) {
      console.log(params);
      return false;
    }
  }
  
  let a = {};
  console.log(a instanceof Person);
  // {}
  // false 
  // instanceof会调用Person的[Symbol.hasInstance]方法，且[Symbol.hasInstance]方法的返回值，决定了instanceof
  ```

* Symbol.isConcatSpreadable

  是一个boolean值，表示该对象用于Array.prototype.concat() 时，是否可以展开

  ```javascript
  const arr = [1,2,3];
  const arr2 = [4,5,6];
  
  arr2[Symbol.isConcatSpreadable] = false;
  
  arr.concat(arr2); // [1,2,3,[4,5,6]]
  ```

* Symbol.unscopables

  该对象制定了使用with关键字时，哪些属性会被with环境排除

* Symbol.match

  当执行str.match()时，如果该属性存在，会调用它，返回该方法的返回值

* Symbol.replace

  当执行str.replace()时，如果该属性存在，会调用它，返回该方法的返回值

* Symbol.search

  当执行str.search()时，如果该属性存在，会调用它，返回该方法的返回值

* Symbol.split

  当执行str.split()时，如果该属性存在，会调用它，返回该方法的返回值

* **Symbol.iterator**

  A method returning the default iterator for an object. Used by `for...of`.

* **Symbol.toPrimitive**

  该对象被转为原始类型的值时，会调用这个方法，返回该对象对应的原始类型值

* **Symbol.toStringTag**

  调用toString时，返回该方法的返回值

* Symbol.species

  创建衍生对象时，会使用该属性

### 迭代器 iterator

iterator是一种接口，为各种不同的数据结构提供统一的访问机制。任何数据结构只要部署了iterator接口，就能完成遍历操作。

1. ES6产生了一种新的遍历方法` for of`， iterator接口主要供for of使用

2. 原生就具备了iterator接口的数据结构

   1. Array
   2. Arguments
   3. Set
   4. Map
   5. String
   6. TypedArray
   7. NodeList

3. 工作原理：

   * 创建一个指针对象（其实就是**[Symbol.iterator]要返回的对象**），指向当前数据结构的起始位置
   * 第一次调用对象的next方法，指针自动指向第一个成员
   * 之后每次调用next方法，指针往后移一位，直到指向最后一个成员
   * next方法返回 {value: xx, done: boolean}
   * **想要自定义遍历规则时，一定要想到迭代器**

   ```javascript
   const arr = [1,2,3,4];
   
   const iterator = arr[Symbol.iterator]();
   
   iterator.next(); // {value: 1, done: false}
   iterator.next();
   iterator.next();
   iterator.next();
   iterator.next(); // {value: undefined, done: true}
   ```

#### 迭代器实践：自定义对象遍历规则

iteratable object ---> 必须定义了iterator

可以出面试题

```javascript
const obj = {
  names: ['1', '2', '3', '4'],
  [Symbol.iterator]() {
    let index = 0;
    
    return {
      next: () => { // 要么用_this去保存this context，要么这里用箭头函数（this指向函数定义时作用域的this）
        if (this.names.length > index) {
          return {
            value: this.names[index++],
            done: false
          };
        } else {
          return {
            value: undefined,
            done: true
          };
        }
      }
    };
  }
}

for (let name of obj) {
  console.log(name);
}
```

### 生成器 generator

生成器其实就是一个特殊的函数，是ES6提供的一种异步编程的解决方案，语法行为与传统函数完全不同。

之前的异步编程都是靠纯回调函数来实现的。

声明函数时，*****的位置只要在function关键词和函数名之间即可。

生成器函数返回的是一个迭代器对象，如果要执行生成器函数里的代码，则需要通过调用返回的迭代器对象的**next方法**才行。

**yield关键字**，作为生成器函数代码的**分隔符**，每一次next调用，都只执行两个相邻的yield关键字（或头或尾）之间的代码，且yield后面的参数或字面量是next方法返回的对象的value值。

next方法可以接受实参，且会作为对应的（上一个）yield的返回值。相当于可以为下一段代码提供一些参数。（帮助异步编程的解决）

#### 生成器函数声明和next方法调用

```javascript
function * gen() {
  console.log(111);
  yield '第一个yield';
  console.log(222);
  yield '第二个yield';
  console.log(333);
};

const iterator = gen();

console.log(iterator.next());
// 111
// {value: '第一个yield', done: false}
console.log(iterator.next());
// 222
// {value: '第二个yield', done: false}
console.log(iterator.next());
// 333
// {value: undefined, done: true}
```

#### 生成器函数返回的迭代器对象的遍历

```javascript
function * gen() {
  yield '第一个yield';
  yield '第二个yield';
}

const iterator = gen();

for (let value of iterator) {
  console.log(value);
}
// 第一个yield
// 第二个yield

console.log(iterator.next()); // {value: undefined, done: true};
```

#### next函数传参

```javascript
function * gen(arg) {
  console.log(arg);
  let p1 = yield '第一个yield';
  console.log(p1);
  let p2 = yield '第二个yield';
  console.log(p2);
}

const iterator = gen('test0');

iterator.next();
// test0
iterator.next('test1');
// test1
iterator.next('test2');
// test2
// {value: undefined, done: true};
```

#### 生成器使用实例

1. 生成器解决回调地狱问题

   要求1s之后打印111，完成之后2s之后，打印222，完成之后3s之后，打印333

   ```javascript
   function one() {
     setTimeout(() => {
       console.log(111);
       iterator.next();
     }, 1000);
   }
   function two() {
     setTimeout(() => {
       console.log(222);
       iterator.next();
     }, 2000);
   }
   function three() {
     setTimeout(() => {
       console.log(333);
       iterator.next();
     }, 3000);
   }
   
   function * gen() {
     yield one();
     yield two();
     yield three();
   }
   
   const iterator = gen();
   
   iterator.next();
   ```

2. 先得到用户数据 --> 订单数据 --> 商品数据

   这里会需要在得到用户数据之后，作为实参传入next方法

   然后在yield语句处获得，再传给获取订单数据的方法。。

   整体结构还是跟第一个例子一样，但是需要通过next来获取每个阶段的数据

### Promise

详见promise课程笔记

### 集合 Set

类似于数组，但是**成员值都是唯一**的。

集合实现了iterator接口，所以可以使用扩展运算符和for of进行遍历

`...new Set();`

`for (let value of new Set([1,2,3]))`

#### 属性和方法

* size

* add 

  添加一个新元素，返回最新的集合

* delete 

  删除数组，返回boolean值

* has 

  检查集合中是否有某个元素，返回boolean值

* clear

  清空集合

#### 集合的实践

1. 数组去重

   ```javascript
   const arr = [1,1,1,1,3,4,5];
   const result = [...new Set(arr)];
   ```

2. 求交集

   ```javascript
   const arr1 = [1,1,1,1,3,4,5];
   const arr2 = [2,2,2,2,3,4,5];
   
   const result = [...new Set(arr1)].filter(item => new Set(arr2).has(item));
   ```

3. 求并集

   ```javascript
   [...new Set([...arr1, ...arr2])];
   ```

4. 求差集

   ```javascript
   [...new Set(arr1)].filter(item => !(new Set(arr2).has(item)));
   ```

### Map

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map

Constructor: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map/Map

类似于对象，区别是，map的key不仅可以是字符串，可以是其他数据类型（包括对象）。

Map实现了iterator接口，所以可以使用扩展运算符和for of进行遍历

用for of遍历时，每个value都是一个数组，第一项为key，第二项为value

#### 属性和方法

* size
* set
* delete
* get
* clear

### Class 类

class类是ES6引入的，更接近传统语言的写法

class可以看做是es5中对象原型写法的语法糖，绝大部分功能，es5也都能做到。新的class写法只是想对象原型的写法更清晰、更像面对对象编程的语法而已。

#### class定义

```javascript
class Phone {
  // 构造方法，必须是这个名字
  constructor(brand, price) {
    this.brand = brand;
    this.price = price;
  }
  
  // 对象方法的定义 （不能像es5那样 function call()）
  call() {
    console.log('calling');
  }
  
  // 但是其实也可以像下面这样写法：直接赋给Person的实例对象
  getBrand = function() {
    console.log('xxx');
  };
  // 或者箭头函数
  getPrice = () => {
    console.log('xxx');
  };
}
```

#### 静态成员

用`static`声明的属性和方法，是类的静态成员，属于类而不属于实例对象。

#### 继承

##### es5构造函数继承

```javascript
function Phone(brand, price) {
  this.brand = brand;
  this.price = price;
}

Phone.prototype.call = function() {};

function SmartPhone(brand, price, color, size) {
  // 借用父类的构造函数，简化一些属性的初始化
  Phone.call(this, brand, price);
  this.color = color;
  this.size = size;
}

// 完成原型链的继承
SmartPhone.prototype = new Phone();
SmartPhone.prototype.constructor = SmartPhone;

// 为子类添加方法
SmartPhone.prototype.photo = function() {};
// ...
```

##### class的类继承

与es5的继承，结果是一样的

```javascript
class Phone {
  constructor(brand, price) {
    this.brand = brand;
    this.price = price;
	}
  
  call() {
    // xxx;
  }
}

class SmartPhone extends Phone {
  constructor(brand, price, color, size) {
    super(brand, price);
    this.color = color;
    this.size = size;
	}
  
  photo() {
    // xxx;
  }
  
  // ...
}
```

##### 方法重写

在子类声明一个与父类名字相同的方法，可以对该方法进行重写。

由于原型链先找最近的方法，所以重写的方法会被调用。

#### getter和setter

可以对对象的属性进行方法的绑定。

get通常对对象的动态属性进行封装，set可以对属性的修改进行更多的操作（比如合法性判断）

setter必须接受一个参数。

```javascript
class Phone {
  constructor(brand, price) {
    this.brand = brand;
    this.price = price;
	}
  
  get price() {
    return this.price;
  }
  
  set price(newPrice) {
    if (typeof newPrice = 'number') {
      this.price = new Price;
    }
  }
}

const p = new Phone('apple', 999);

p.price; // 调用了getter
p.price = 1099; // 调用了setter
```

### ES6的数值扩展

1. Number.EPSILON

   表示JavaScript中的最小精度

   主要是用于浮点数运算上面

   ```javascript
   console.log(0.1 + 0.2 === 0.3); // false
   
   function equal(num1, num2) {
     if (Math.abs(num1 - num2) < Number.EPSILON) {
       return true;
     }
     
     return false;
   }
   equal(0.1 + 0.2, 0.3); // true
   ```

2.  引入了二进制和八进制

   `let b = 0b1010`;

   `let o = 0o777;`

   加上之前有的十进制和十六进制 0x

3. Number.isFinite

   是否为有限数

4. Number.isNaN

   检测是否为NaN

5. Number.parseInt

   字符串转换成整数

6. Number.parseFloat

7. Number.isInteger

8. Math.trunc

   抹掉数字的小胡部分

9. Math.sign

   正数，负数还是0

### ES6的对象方法扩展

1. Object.is

   判断两个值是否完全相等

   与 === 基本差不多

   ```javascript
   // 例外
   NaN === NaN; // false
   Object.is(NaN, NaN); // true
   ```

2. Object.assign

   对象的合并

3. Object.setPrototype 与 Object.getPrototypeof

   设置和获取对象的原型对象

### 模块化

将一个大的程序文件，拆分成许多小的文件，然后将小文件组合起来。

优点：

* 防止命名冲突
* 代码复用
* 高维护性

ES6之前，js没有自己的模块化，是依靠社区指定的规范和开发的产品来对项目代码进行拆分。

* CommonJS  --> NodeJS, Browserify
* AMD --> requireJS
* CMD --> seaJS

#### ES6的模块化语法

用script引入时，需要 `type='module'`

##### export

1. 在想要暴露的变量声明前，加一个export即可

   `export function fun() {};`

2. 统一暴露

   `export{school, func, findJob};`

3. 默认暴露

   `export default 加上想暴露的任意数据`

   如果是使用 import * as  module from 'xxx' 这种引入的话，需要module.default才能得到默认暴露的数据

##### import

1. 通用方式

   `import * as m1 from 'xxx'`

2. 解构赋值形式

   `import {school, findJoc} from 'xxxx'`

   named importing

   如果使用这种方法，出现重名问题时，可以使用 as 来重命名

   如果是default导出：

   第一种方式应该要 `m1.default.xxx`，第二种方式必须要重命名 `import {default as m2} from 'xxx'`

3. 简便形式 （只能用于默认暴露）

   `import defaultOutput from 'xxxx'`

   注意没有花括号 (default inporting)

#### Babel对es6及以上代码转换

1. 安装 bable-cli, babel-preset-env, browserify(webpack)

2. `npx babel src/js -d dist/js --presets=babel-preset_env`  --> 进行代码转换（一般转成commonJS规范）

   后面那个presets也可以单独再写个config文件进行配置

3. 打包  `npx browserify dist/js/app.js -o dist/bundle.js`

4. 在html里引入dist/bundle.js即可

#### ES6模块中引入npm包

`import $ from 'jquery'`

`$('body').css('background', pink)`



## ES7新特性

* Array.prototype.includes

  判断一个element在不在一个数组中

  返回boolean值

* ** 

  与 `Math.pow()`一样

  2 ** 10 === Math.pow(2, 10)



## ES8

### async and await

见Promise学习笔记

### 对象方法扩展

* Object.keys(obj)

  返回对象的keys

* Object.values(obj)

  返回对象的values

* Object.entries(obj)

  返回一个数组，每个元素都是key-value的二元数组

  可以用于构造Map： `new Map(Object.entries(obj))`

* Object.getOwnPropertyDescriptors(obj)

  返回一个对象属性的**描述对象**

  该方法其实对应于Object.create()，可以用于对于一个对象的深层次拷贝

  ```javascript
  // Object.create(原型对象，描述对象)
  // 比如说
  Object.create(null, {
    // 属性名
    name: {
      // 属性值
      value: 'abc',
      // 属性特性
      enumerable: false, // 是否可以枚举
      writable: true, // 是否可写
     	configurable: true // 是否可以删除
    }
  })
  ```

## ES9

### 对象的扩展运算符和rest参数

#### rest参数

```javascript
const {host, post, ...rest} = {host:'1', post: true, username: '123', password: '456'};

rest; // {username: '123', password: '456'}
```

#### 扩展运算符

可以用于合并（覆盖）对象

`{...obj1, ...obj2}`

### 正则扩展

正则表达式cheatsheet：https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions/Cheatsheet

#### 命名捕获分组

在没有出现这个语法前，我们是使用下标去获取捕获分组的内容

```javascript
const str = '<a href="www.google.com">text</a>';
const regex = /<a href="(.*)">(.*)<\/a>/; // () 表示需要捕获的东西

const res = regex.exec(str);
// 通过 res[1]得到第一个捕获分组，即 url；通过res[2]得到文字内容
// 此时的 res.groups 是undefined
```

命名捕获分组：

```javascript
const str = '<a href="www.google.com">text</a>';
const regex = /<a href="(?<url>.*)">(?<content>.*)<\/a>/; 
// () 表示需要捕获的东西
// ?<xxxx> 表示捕获分组的命名

const res = regex.exec(str);

// 命名捕获分组的东西，可以通过 res.groups.xxx 得到
// 比如 res.groups.url
```

#### 反向断言 negative assertion

断言： https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions/Cheatsheet#other_assertions

正向断言lookahead： `x(?=y)`         negative `x(?!y)`

反向断言lookbehind： `(?<=y)x`     negative  `(?<!y)x`

#### dotAll 模式

dot  `.` 元字符（除换行符外的任意单个字符）

> 补充一下： `.*?` 后面的问号（used immediately after any of the quantifiers `*`, `+`, `?`, or `{}`）表示non-greedy，matching the minimum number of times；默认是贪婪模式

```javascript
// 举例说明：
const tag = '<a>abcd</a><a>abcd</a>';

/<a>(.*)<\/a>/.exec(tag); 
// 捕获到的是 "abcd</a><a>abcd"

/<a>(.*?)<\/a>/.exec(tag);
// 捕获到的是 "abcd"
```

dotAll模式：就是元字符能匹配任意字符的模式，需要添加正则修正符（modifier） `s`

实例：

```javascript
const str = `
	<li>
		<a>xxxx</a>
		<p>xxxxx</p>
	</li>
	<li>
		<a>xxxx</a>
		<p>xxxxx</p>
	</li>
`;
// 没有dotAll模式时，需要用/s+去判断标签之间的空格
// 开启dotAll模式后，可以用.*来检测，需要加？表示取消贪婪
const regex = /<li>.*?<a>(.*?)<\/a>.*?<p>(.*?)<\/p>.*?<\/li>/sg; // 开启全局模式，获取所有匹配的字符串
let result;
let data = [];

while (result = regex.exec(str)) {
  data.push({name: result[1], time: result[2]});
}

console.log(data);
// [{name, time}, {name, time}]
```



## ES10

#### Object.fromEntries(iterable)

是Object.entries(obj) 的逆运算

transforms a list of key-value pairs into an object. 将key-value pairs的列表转换成一个对象

可以是二维数组，可以是map，也可以是其他iterable的对象

#### trimStart 和 trimEnd

trim 清除字符串开头和结尾的空白

ES10引入了这两个方法，可以选择清除开头或是结尾的空白

#### 数组扩展：flat与flatMap

* flat

  将一个多维数组转换成一个较低维数组

  接受**一个参数depth**，表示需要降低的维度数量，默认为1

  ```javascript
  const arr1 = [0, 1, 2, [3, 4]];
  
  console.log(arr1.flat());
  // expected output: [0, 1, 2, 3, 4]
  
  const arr2 = [0, 1, 2, [[[3, 4]], 5]];
  
  console.log(arr2.flat(2));
  // expected output: [0, 1, 2, [3, 4], 5]

* flatMap

  相当于是flat和map的结合操作

  ```javascript
  let arr1 = [1, 2, 3, 4];
  
  arr1.map(x => [x * 2]);
  // [[2], [4], [6], [8]]
  
  arr1.flatMap(x => [x * 2]);
  // [2, 4, 6, 8]
  
  // only one level is flattened
  arr1.flatMap(x => [[x * 2]]);
  // [[2], [4], [6], [8]]
  
  ```

  另一个更好更实用的例子：

  ```javascript
  let arr1 = ["it's Sunny in", "", "California"];
  
  arr1.map(x => x.split(" "));
  // [["it's","Sunny","in"],[""],["California"]]
  
  arr1.flatMap(x => x.split(" "));
  // ["it's","Sunny","in", "", "California"]
  ```

#### Symbol.prototype.description

返回Symbol objects的optional description

```javascript
Symbol('desc').toString();   // "Symbol(desc)"
Symbol('desc').description;  // "desc"
Symbol('').description;      // ""
Symbol().description;        // undefined

// well-known symbols
Symbol.iterator.toString();  // "Symbol(Symbol.iterator)"
Symbol.iterator.description; // "Symbol.iterator"

// global symbols
Symbol.for('foo').toString();  // "Symbol(foo)"
Symbol.for('foo').description; // "foo"
```



## ES11

#### 私有属性

在属性名前面加一个 `#` 表示私有属性。只能在类内部使用。

也可以在方法名前加一个`#` 表示私有方法

```javascript
class Person {
  name;
  #age;
  
  constructor(name, age) {
    this.name;
    this.#age = age;
  }
}
```

#### Promise.allSettled

接收一个全是promise的数组作为参数，返回一个永远成功的promise对象

返回的promise对象，包含了传入promises的每一个promise的状态和结果。

```javascript
// [{status: 'fulfilled', value: 123}, {status: 'rejected', reason: 'error'}]

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

与之类似的是Promise.all：

如果想要知道每个promise的状态和结果，则选用Promise.allSettled

如果需要每个promise都成功才能继续下一步，则选用Promise.all

#### String.prototype.matchAll

该方法对于在字符串中，使用正则表达式进行**数据的批量提取**非常有用。

```javascript
const str = `
	<li>
		<a>xxxx</a>
		<p>xxxxx</p>
	</li>
	<li>
		<a>xxxx</a>
		<p>xxxxx</p>
	</li>
`;
// 没有dotAll模式时，需要用/s+去判断标签之间的空格
// 开启dotAll模式后，可以用.*来检测，需要加？表示取消贪婪
const regex = /<li>.*?<a>(.*?)<\/a>.*?<p>(.*?)<\/p>.*?<\/li>/sg; // 开启全局模式，获取所有匹配的字符串
const result = str.matchAll(regex); // 返回的是一个正则表达式迭代器（iterator）
// 可以用for of进行遍历，也可以用扩展运算符转换成一个array
const arr = [...result];
```

#### 可选链操作符

`?.`  OR `?[]`

```javascript
// 只要如果一个对象很深，我们做判断需要：
obj && obj.a && obj.a.b && obj.a.b.c // 才能获取到c
// 可选链操作符
obj?.a?.b?.c;
```

#### 动态import

VS 静态import（ES6引入） 无法按需加载，对于性能有些影响

```javascript
import * as m1 from 'path'; // 静态import，不管用不用都被加载了

const btn = xxxx;

btn.onclick = function() {
  // 动态import实现了按需加载
  // 使用import函数按需加载，返回的是一个promise，返回值是整个module对象
  import('path').then(module => {
    module.fun(); // path模块导出的fun方法
  })
}
```

#### BigInt类型

为了进行更大数值的运算

BigInt类型的整数，不能跟其他类型的进行运算，只能与BigInt类型运算。

且浮点数不能被转换成BigInt类型

```javascript
let n = 123n; // 结尾是n，表示是大整型

typeof n; // bigint

// BigInt能将普通整型转化成大整型
BigInt(100);

// Number.MAX_SAFE_INTEGER 举例
const max = Number.MAX_SAFE_INTEGER;

console.log(BigInt(max));
console.log(BigInt(max) + BigInt(1));
console.log(BigInt(max) + BigInt(2));
```

#### 绝对全局对象 globalThis

无论code的运行环境是什么（浏览器，nodejs），这个变量永远指向全局对象。

如果在写代码的时候，需要对全局对象进行操作，可以直接使用这个变量。









































