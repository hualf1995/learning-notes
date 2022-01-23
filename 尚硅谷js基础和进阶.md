##  js 基础

### 其他基础知识

#### 基础数据类型

* string
* number
* boolean
* null
* undefined

#### 运算符

#### 条件语句（循环等）

### 对象

#### 分类

* 内建对象
  * es标准下提供的对象
  * 比如 Math, Function, Number, Boolean...
* 宿主对象
  * js的运行环境提供的对象，主要指浏览器提供的对象
  * BOM, DOM
* 自定义对象

#### 基本操作

构造函数：用来构造对象的函数， new

添加属性

访问属性

修改属性

删除属性 delete xx.xxx

#### 属性名和属性值

属性名：不规定一定要遵守标识符的规则，但是推荐遵守

读取、添加特殊的属性名时，不能使用 `.` ，需要使用 `[]`

`obj['123'] = 'apple';  console.log(obj['123']);`

使用`[]`这种方式去操作属性，更加灵活。在中括号中可以直接传递**变量**

属性值：可以是任意类型

可以通过 in 运算符，去检查一个对象中是否含有指定的属性。

#### 基本数据类型和引用数据类型

栈内存和堆内存

js的变量都是保存在栈内存中

* 基本数据类型的值直接在栈内存中存储
* 值和值之间是独立存在的，修改一个变量不会影响其他的变量

* 对象是保存到堆内存中的，每创建一个新对象，都会在堆内存中开辟一个新的空间
* 而变量保存的是对象的内存地址（对象的引用）
* 如果两个变量，保存的是同一个对象引用，当通过一个变量**修改属性**时，另一个也会被影响。（堆内存发生改变）

=== 比较：

* 基本数据类型，就是比较值 (boolean, number, string, null, undefined, bigint, symbol)
* 引用数据类型，比较的是对象的<u>内存地址</u>

#### 对象字面量

属性名可以加引号也可以不加，但是如果是特殊的名字（数字，特殊字符等），必须要加引号

#### 方法

如果一个函数是一个对象的属性，则该函数被称为方法（method）。仅是名称的区别。

#### 枚举对象属性

for in 语句

```javascript
for (var 变量 in 对象) {
  // 属性名直接赋值给变量
  // 需要通过中括号的方式去使用变量访问对象
  // 根据之前在精粹那本书的介绍，该循环包括函数和在原型中的属性。
}
```

### 函数

函数也是对象。函数中可以封装一些功能（代码），在需要的时候可以执行这些功能（代码）

使用typeof检查一个函数对象时，返回 function

#### 创建函数对象

* 将要封装的代码以字符串形式传递给构造函数。

  `var fun = new Function("console.log('sss');");`

* 函数声明

  Function 函数名([形参1，形参2，。。。形参N])

  ```javascript
  function fun2() {
  	console.log('ssss');
  }
  ```

* 函数表达式（匿名函数）

  var fun3 = function([形参1，形参2，。。。形参N])

#### 参数、返回值

在定义函数时，可以指定形参，多个形参之间用逗号隔开。**声明形参就相当于在函数内部声明了对应的变量，但不赋值**。

在调用函数时，可以在括号中指定实参（实际参数），实参将会被赋值给函数中对应的形参。（实参可以是任意类型）

调用函数时，解析器不会检查实参类型也不会检查实参的数量（多余的不会被赋值，少传的是undefined）。所以有可能出现接收到非法的实参。（ts出现的意义）

return 值， 如果不跟返回值就返回undefined，return可以返回任意类型的值。

return语句之后的代码不再执行。

函数参数太多时，可以考虑打包进一个对象再往下传。

#### 立即执行函数

函数定义之后被立即执行。往往只会执行一次。

#### 作用域

指一个变量的作用范围。

* 全局作用域

  * 直接编写在script标签的js代码都在全局作用域。
  * 全局作用域在页面打开的时候创建，在页面关闭时销毁
  * 在全局作用域中有一个全局对象 window
    * window代表一个浏览器的窗口，由浏览器创建，可以直接使用
    * 在全局作用域中，创建的变量都会作为window对象的属性保存，创建的函数都会作为window对象的方法保存
  * 全局作用域中的变量都是全局变量，整个页面可以访问。

* 函数作用域

  * 调用函数时创建函数作用域，函数执行完之后销毁函数作用域

  * 每调用一次函数就会创建一个新的作用域，之前互相独立

  * 函数作用域中能访问到全局作用域

  * 当在函数作用域中操作一个变量时，会先在自身的作用域中查找，如果没有，则向上一级作用域查找，直到全局作用域。

    不使用var创建的变量就会成为全局变量

    ```javascript
    function fun() {
      d = 100; // 相当于 window.d = 100;
    }
    
    console.log(d); // 100
    ```

* 块级作用域（es6）

  作用域为`{}` 

* **补充：变量的声明提前**

  **var声明的变量，会在所有代码之前被声明。** variable hoisting

  ```javascript
  // 区别以下几种情况
  // 情况1
  var a = 1;
  console.log(a);  // 1
  
  // 情况2
  console.log(a); // a is not defined
  a = 1;
  
  // 情况3（变量提升）
  console.log(a); // undefined
  var a = 1;
  ```

* **补充：函数的声明提前**

  * 使用函数声明形式创建的函数，也会在所有代码执行前被创建，所以可以在声明前调用执行。

    ```javascript
    fun();
    function fun() {
      // ...
    }
    ```

  * 使用函数表达式创建的函数，不被声明提前。

    ```javascript
    fun(); // 不能执行undefined，此时由于变量提升，fun是undefined
    
    var fun = function() {
      // ...
    }
    ```

* **补充 const, let, var:**

  * var
    * 声明的变量的作用域是函数作用域
    * 能够重复声明变量
    * 声明的变量能够声明提前

  * let
    * 声明的变量的作用域是块级作用域
    * 不能重复声明已存在的变量
    * 有暂时死区，不会被提升
  * const
     * let的特性它基本都有
    * const必须初始化赋值

### 对象与函数

#### this

解析器在调用函数时，每次都会向函数内部传递进一个this参数

this参数指向一个对象，这个对象被称为函数执行的上下文对象。

根据函数的调用方式，this的值不同：

（其实这两种方式是同一种，当以函数形式调用时，其实相当于`window.func`来调用）

* 以函数的形式调用时，this永远都是window

  解决办法：

  ```javascript
  myObj.double = function() {
    var that = this; // 解决函数调用模式，this被错误绑定到window的问题
   	var helper = function() {
      // 因为这个helper函数将会以函数调用模式被调用，所以不能用this去访问myObj
      that.value = 2 * that.value;
    };
    helper(); // 函数调用模式
  };
  
  myObj.double();
  ```

* 以方法的形式调用时，this指向调用方法的对象（是我们期待的那种情况）

* 以构造函数的形式调用，this就是新创建的那个对象（详见下文）

#### 使用工厂模式创建对象

```javascript
function createPerson(name, age, gender) {
  const obj = new Object();
  
  obj.name = name;
  obj.age = age;
  obj.gender = gender;
  obj.sayHello = function() {
    alert(this.name);
  };
  
  return obj
}

const a = createPerson(xx,xxx,xxxx);
const b = createPerson(xx,xxx,xxxx);
//....
```

可以批量创建对象。

缺陷：所有对象的类型都是object

#### 构造函数

习惯上首字母大写

构造函数就是一个函数，创建方式与普通函数没有区别。区别在于，构造函数需要使用new来调用。

##### 构造函数的**执行流程**

1. 立刻创建一个新的对象
2. 将新建的对象设置为函数中的this，在构造函数中可以使用this来引用新建的对象
3. 逐行执行函数中的代码
4. 将新建的对象作为返回值返回

使用同一个构造函数创建的对象，是同一类。（我们也将一个构造函数成为一个**类**）

通过构造函数创建的对象，我们称为是该类的一个**实例对象**。

> 使用instanceof 语句可以检查一个对象是否是一个类的实例对象
>
> objtect instanceof Person

所有对象都是Object的后代，instanceof都返回true

```javascript
function Person(name, age) {
  this.name = name;
  this.age = age;
  this.sayHello = function() {
    console.log(this.name);
  };
};

var a = new Person('xx', 1);
var b = new Person('xxx', 2);
```

##### 构造函数内部添加方法有个问题

构造函数每执行一次，都会创建一个新的（相同的）方法。这是完全没必要的，我们可以使所有对象共享一个方法。

一种解决方法：将共享的方法在全局作用域中定义

```javascript
function Person(name, age) {
  this.name = name;
  this.age = age;
  this.sayHello = sayHello;
};
function sayHello = function() {
  console.log(this.name);
};
```

这种解决方法能解决性能的问题，但是也有其他问题：

* 污染了全局作用域的命名空间
* 很不安全，有可能被误用或者被覆盖

所以我们需要一种好的方案：原型对象

#### 原型对象

我们创建的每一个函数，解析器都会向**函数**添加一个属性**prototype**  --> 这个属性对应的对象就是**原型对象**。

* 如果函数作为普通函数调用，则prototype没有什么作用

* 如果函数作为构造函数被调用，它所创建的对象都会有一个隐含的属性，该属性`__proto__` 指向该构造函数的原型对象（也是构造函数的prototype属性）

  ```javascript
  function Person() {
    
  }
  
  const per1 = new Person();
  const per2 = new Person();
  
  console.log(Person.prototype === per1.__proto__); // true
  console.log(Person.prototype === per2.__proto__); // true
  ```

原型对象相当于一个公共区域，所有同一个类的实例都可以访问到这个原型对象。

所以我们可以将所有对象共有的内容都放到原型对象中。（既不会污染全局变量，又不需要为每个对象创建函数）

##### 当我们访问对象的属性或方法时

会**先在对象自身**中寻找，如果找到就直接使用；如果没有，则会顺着**原型链去原型对象中寻找**（原型对象也是对象，也有原型，所以会一层层往上找），**直到找到Object的原型**。

Object的原型没有原型，如果Object的原型中也没找到则返回undefined

```javascript
function MyClass() {
  
}

const mc = new MyClass();

console.log(mc.hasOwnProperty('hasOwnProperty'));

// mc._proto__ -> MyClass.prototype --> 一个Object类的实例对象
// mc._proto__.__proto__ -> Object.prototype
// mc._proto__.__proto__.__proto__ --> null
```

> xx in object --> 检查对象中是否有某个属性（包括原型链）
>
> object.hasOwnProperty('xxx')  --> 检查对象本身是否含有某个属性（不包括原型链）

#### toString

当我们直接在页面中打印一个对象时(console.log)，实际上是输出对象的`toString`方法的返回值。

如果不想打印对象输出[object 0bject]， 我们需要在该对象的类的原型对象上添加一个自定义的toString方法

```javascript
function Person(name, age) {
  this.name = name;
  this.age = age;
}

Person.prototyge.toString = function() {
  return `Person[name=${this.name}, age=${this.age}]`
}
```

#### 垃圾回收

当一个对象没有任何变量或者属性对它进行引用时，此时我们将永远无法操作该对象。这种对象就是垃圾，过多会占用大量的内存空间，导致程序运行缓慢。

在JS中拥有自动的垃圾回收机制，会自动将这些对象从内存中销毁。我们只需要将不再使用的对象设置为null即可。

栈内存中的变量会在生命周期结束后被销毁。（比如函数中的变量会在函数执行结束后被销毁）

### 数组

数组也是一个对象。

属性名都是数字，我们叫它**索引**。它是从0开始的整数。

数组的存储性能比其他普通对象要好。

#### 基本操作

添加

读取（如果不存在的索引，返回undefined）

修改

#### length属性

* 获取连续数组的长度
* 获取非连续数组的最大索引值+1
* 修改数组长度
  * 如果设置的length大于原长度，多出来的部分会空出来，设置成undefined
  * 如果设置的length小于原长度，多出来的元素会被删除

#### 创建数组

* 构造函数  
  * 值得注意的是，`new Array(10)` 是创建一个长度为10 的数组（一般很少用，因为js的数组不是定长的）
  * new Array()
  * new Array(1,2,3,4,5)
* 数组字面量创建
  * const a = [1,2,3,4];

数组中的元素可以是任意的数据类型。

二维数组，n维数组

#### 数组方法

##### 基本方法

* push

  向数组末尾添加一个或多个元素，并返回新的长度

* pop

  删除数组的最后一个元素，并返回被删除的元素

* unshift

  向数组开头添加一个或多个元素，并返回新的长度

  其他元素的索引会依次调整

* shift

  删除数组的第一个元素，并返回该元素

  其他元素的索引会依次调整

以上四个方法可以模拟stack，queue， deque

##### 数组的遍历

* for循环

* forEach

  IE8以上的浏览器支持

  接收一个函数作为参数，遍历数组的每个元素，执行该函数 --> 回调函数

  该回调函数的参数：当前元素， 当前元素的索引，正在遍历的数组

##### 其他方法

* slice

  从数组提取指定部分的元素，返回一个新的数组

  参数：start， end（可选，不包括end索引的元素）

  参数为负数时，是从后往前数，相当于length + start（此时start是个负数）

* splice

  可以用于**删除**数组中的指定部分的元素，也可以向原数组的指定位置**添加**元素。

  该方法**会改变原数组**，并且返回被删除的元素。

  参数：

  1. 表示开始位置的索引
  2. 要删除的个数
  3. 第三个及以后的参数，是需要插入到开始位置索引的元素

  ```javascript
  const arr = [0,1,2,3];
  
  arr.splice(1,1); // arr => [0,2,3]
  arr.splice(2,1,4,5,6); // arr => [0,2,4,5,6]
  arr.splice(1,0,1); // arr => [0,1,2,4,5,6]
  ```

  该方法比较灵活也比较多功能化。

  练习：数组去重

* concat

  连接两个或多个数组，并将新数组返回

  不影响原数组

* join

  将数组转化成字符串

  不影响原数组

  参数为连接数组的连接符，默认为`,`

* reverse

  反转数组，会对原数组产生影响

* sort

  对数组进行排序，会对原数组产生影响

  默认按照Unicode编码进行排序，所以对数字或其他类型元素进行排序时，可能产生错误的结果。

  所以我们需要自定义排序的规则。

  sort方法接收一个回调函数作为参数，该回调函数接收两个形参：

  * 浏览器会分别使用数组中的元素作为实参去调用回调函数
  * 使用哪个元素去调用是不确定的，但是**第一个形参一定在第二个形参前面**
  * 根据回调函数的返回值来决定元素的顺序
    * 如果**返回值大于0，则两个元素交换位置**
    * 如果返回值小于0，则位置不变
    * 如果返回值等于0，认为两个元素相等，也不会交换位置

  ```javascript
  const arr = [3,4,1,2,5];
  
  arr.sort(function(a, b) {
    return a - b;
  });
  // arr -> 1,2,3,4,5
  arr.sort(function(a, b) {
    return b - a;
  });
  // arr -> 5,4,3,2,1
  ```

### 函数的方法

#### call 和 apply

相同点：

* 都是函数对象的方法，需要通过函数对象来调用
* 调用call和apply都会调用函数执行
* 第一个参数是函数执行时的上下文对象this
  * 我们可以通过传入不同的对象来控制this参数

不同点：

如果函数需要传入参数的话：

* call从第二个参数开始，依次传入所需的实参
* apply，将需要传入的实参封装到一个数组中，作为第二个参数传入

这两个方法是this对象的第四种情况！

#### bind

创建一个新的函数，在 bind() 被调用时，这个新函数的 this 被指定为 bind() 的第一个参数，而其余参数将作为新函数的参数，供调用时使用。

#### arguments

调用函数时，每次都会传递两个隐含的参数：

* this
* arguments
  * 是一个类数组对象。只有length和索引功能，没有array对象的方法
  * **传递的实参**都会被保存在arguments中
  * arguments.length 可以获取实参的个数
  * 即使不定义形参，也可以通过该参数来获取实参
  * arguments有一个属性callee，对应一个函数对象，就是当前正在执行的函数的对象

### Date对象

使用Date对象来表示一个时间。

#### 构造date对象

* 不指定时间

  `var d = new Date();`

  会封装为当前代码执行的时间。（统一的timestamp，与时区无关）

* 指定时间

  传入一个表示时间的字符串作为参数 （不传时区的话，以客户端时区为准，可能会出问题）

  格式为 `月份/日/年 时:分:秒`

  `var d = new Date("09/09/2021 10:20:30");`

#### 相关方法

* getDate

  返回当前日期对象的 ”日“

* getDay

  返回当前日期对象的 ”星期“  范围是 0 - 6

* getMonth

  0 - 11

* getFullYear

  年份

* getHours, getMinutes, getSeconds, getMilliseconds

* getTime

  获取当前日期对象的时间戳(**timestamp**)

  返回格林威治标准时间的1970年1月1日0时0分0秒至今的毫秒数

  计算机底层在保存时间时，使用的都是时间戳

  ```javascript
  // 如果我现在在美国西部时区
  var d = new Date('01/01/1970');
  console.log(d.getTime()); // 返回的是28800000
  ```

* Date.now()

  获取当前的时间戳

### Math

Math与其他对象不同，它不是一个构造函数，而是一个工具类。

它封装了数学运算相关的属性和方法。 直接通过`Math.`来调用

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math

生成x-y之间的随机数：

`Math.round(Math.random * (y - x) + x)`

### 包装类

JS向我们提供了3个包装类：

* String()
* Number()
* Boolean()

将一些基本数据类型转换成对应的对象。

但是注意：我们在实际应用中不太会用到基础数据类型的对象，会对比较或判断带来一些问题。



当我们**对基本数据类型的值去调用属性和方法**时，（对字符串来说非常常见！什么toUpperCase之类的方法）

浏览器会临时使用包装类将其转换为对象，然后再调用对象的属性和方法。调用完之后，再将其转换为基本数据类型。

```javascript
var s = 123;

console.log(typeof s); // number

s = s.toString();

console.log(typeof s); // string

s.hello = 'hello';

console.log(s.hello); // undefined

console.log(typeof s); // string
```

#### 字符串的方法

在底层，字符串是以字符数组的形式保存的。

操作字符串可以想成操作数组。

* 使用索引获取指定位置的字符

* length

* charAt

  同索引

* charCodeAt

  根据索引获取指定位置的字符的Unicode编码

* String.formCharCode

  根据字符编码获取字符

* concat

* indexOf

  检索一个字符串是否含有指定的内容，返回第一次出现的索引

  第一个参数是需要查找的字符串，第二个参数是开始查找的位置

* lastIndexOf

  从后往前查找一个字符串

* match, replace, search 需要配合正则表达式使用，会在之后讲解。（精粹那本书也很详细了）

* slice

* substring

  与slice类似，但是不接受参数为负数，如果传递了负数，默认为0

  并且它会自动调整参数的位置，如果第二个参数比第一个参数小，则交换位置

* substr

  反对使用它

* split

  将一个字符串拆分成一个数组。

  需要一个字符串作为参数，并以此字符串去拆分数组

* toLowerCase

* toUpperCase

### 正则表达式

正则表达式可以定义一些字符串的规则：

* 来检查一个字符串是否符合规则
* 提取字符串中符合规则的内容

#### 创建正则表达式对象

* 构造函数

  new RegExp("正则表达式", "匹配模式")

  匹配模式：

  * i 忽略大小写
  * g 全局匹配模式（查找所有匹配，而不是找到第一个匹配后停止）

  更加灵活，可以在第一个参数中使用变量。

* 字面量

  `/正则表达式/匹配模式`   注意不是字符串

  更加简单

#### 正则表达式方法

* test

  用来检查一个字符串是否符合正则表达式的规则

  regexp.test(string)

#### 字符串String中与正则表达式相关的方法

string.method(正则表达式)

* split

  将一个字符串拆分成一个数组。（参数可以是字符串或者正则表达式）

  如果参数传递的是一个正则表达式，会根据正则表达式去拆分

  ```javascript
  var str = '12a34b231d55';
  var reg = /[a-z]/;
  
  str.split(reg);
  ```

* search

  可以搜索字符串中是否含有指定内容。（参数可以是字符串或者正则表达式）

  返回第一次出现指定内容的索引，或者 -1

* match

  可以根据一个正则表达式，从一个字符串中将符合条件的内容提取出来。

  默认情况下，只返回第一个符合要求的内容。可以设置g匹配模式，返回所有匹配内容。

  返回的内容封装在一个数组中。

* replace

  将字符串中的指定内容替换成新的内容。

  默认情况下，只替换第一个匹配的内容。可以设置g匹配模式，返回所有匹配内容。

#### 语法

* 使用 `|` 表示或

  `var reg = /a|b|c/`

* 使用`[]`， [] 中的内容也是或的关系

  查找<u>方括号</u>之间的任何字符

  可以使用连字符` -`

  [a-z]  --> [abc.....xyz]  --> /a|b......|y|z/  任意小写字母

  [A-z]  任意字母

  [0-9]  任意数字

* `[^]`

  查找**任何不在**<u>方括号</u>之间的字符

* 量词

  设置一个内容出现的次数

  量词只对它前面的一个内容起作用。（所以可能根据需求要进行打包 -- 括号括起来）

  ```javascript
  // 比较以下两种
  /ab{1,3}c/   --> abbc
  /(ab){1,3}c/ --> ababc
  ```

  {n}  正好出现n次

  {m, n} 出现m到n次

  {m, } 出现m次以上

  `+` 表示 一个或多个，相当于 {1, }

  `*` 表示 零个或多个，相当于 {0, }

  `?` 表示 零个或一个，相当于{0,1}

* `^`  和 `$`

  ^ 表示开头

  $ 表示结尾

  比如，`^a` 表示以a开头，`a$` 表示以a结尾

  如果在一个正则表达式中同时使用了 ^ 和 $，则要求字符串必须完全符合正则表达式的规定

  例子：用正则表达式验证一个字符串是否为合法手机号

  ```javascript
  // 以1开头
  // 第二位数字是 3-9
  // 后面都是数字
  // 总长11位
  var reg = /^1[3-9][0-9]{9}$/;
  ```

* 元字符(metacharacter)

  拥有特殊含义的字符。

  * `.`

    表示任意字符

  * \w

    任意数字，字母和_

    相当于 [A-Za-z_]

  * \W

    \w相反

  * \d

    [0-9]

  * \s

    空格

  * \b

    单词边界

    表示是否是一个独立单词（左边或者右边是不跟其他单词连起来的）

    `/child\b/`  --> hellochild --> true

    `/\bchild\b/` --> hellochild --> false, hello child --> true

* 转义字符 `\`

  如果需要单纯表达一个字符，则可能需要转义字符**对特殊字符进行转义**。比如元字符，比如在[]中的连字符-，比如？* $等等

  例子： `/\./`    `/\\/`

  值得注意的是，如果使用正则表达式的构造函数，需要注意传入的参数是一个字符串string，在字符串中，反斜杠本身就代表了转义字符。也就是说，在字符串中，单独一个`"\"` 是不合法的，需要`"\\"`才表示一个反斜杠。而由于构造函数需要的是一个字符串，我们又需要在正则表达式中使用转义字符，所以如果正则表达式需要一个`\`，我们需要在构造函数中用`\\` 来代替。

  例子：

  ```javascript
  // 如果所需的正则表达式字面量是
  /\./
  // 则构造函数中需要传入以下字符串
  "\\."
  
  // 如果所需的正则表达式字面量是
  /\\/
  // 则构造函数中需要传入以下字符串
  "\\\\"
  
  // 输入的是四个斜杠的字符串，但是你console.log出来的其实是两个斜杠，构造函数将返回一个 \//\ 的正则表达式对象
  ```

* 练习：去除一个字符串的开头和结尾的空字符

  ```javascript
  var reg = /^\s* | \s*$/g;
  var str = 'xxxx';
  
  str = str.replace(reg, '');
  ```

  官方的**trim方法**的实现：

  ```javascript
  if (!String.prototype.trim) {
    String.prototype.trim = function () {
      return this.replace(/^[\s\uFEFF\xA0]+|[\s\uFEFF\xA0]+$/g, '');
    };
  }
  ```

#### 练习： 邮件的正则表达式

第一步就是用文字写出需要满足的规则：

linfeng.hua@yahooinc.com

任意字母数字下划线   .任意字母数字下划线   @  任意字母数字  .任意字母  .任意字母

```javascript
var emailReg = /^\w{3,}(\.\w+)*@[A-z0-9]+(\.[A-z]{2,5}){1,2}$/;
```

### DOM -- document object model

#### DOM 简介

文档：整个HTML网页文档

对象：将网页中的每一个部分都转换为了一个对象

模型：使用模型来表示对象之间的关系，方便我们获取对象 html dom tree

节点：

* 文档节点 --> 整个html文档

  nodeName: #document, nodeType: 9, nodeValue: null

* 元素节点 --> html tag

  nodeName: 标签名, nodeType: 1, nodeValue: null

* 属性节点

  nodeName: 属性名, nodeType: 2, nodeValue: 属性值

* 文本节点

  nodeName: #text, nodeType: 3, nodeValue: 文本内容

浏览器已经为我们提供了文档节点，我们可以通过文档节点去找到并操作其他节点对象。

这个对象就是`window.document` 或者直接 `document`

#### 事件简介

文档或浏览器窗口中发生的一些特定的交互瞬间。

可以通过查找 DOM Event 知道具体有哪些事件。

https://developer.mozilla.org/en-US/docs/Web/API/GlobalEventHandlers  --> onEvent properties

https://developer.mozilla.org/en-US/docs/Web/Events/Event_handlers  --> overview (onEvent vs addEventListener)

我们关心的是如何处理事件：

* 直接在html tag上设置对应的事件属性

  比如 `<button onclick="alert('xxxx')">xxx</button`

  这种写法：结构和行为耦合，不方便维护，所以不推荐使用

* 为对应事件绑定处理函数的形式来响应事件

  this指向绑定事件的元素对象

  ```javascript
  const btn = document.getElementById('btn');
  // 响应事件
  btn.onclick = function() {
    alert('xxxx');
  }
  
  btn.addEventListener('click', function() {xxxxx});
  ```

#### 文档的加载

浏览器在加载一个页面时，是按照自上向下的顺序加载的。

所以如果script标签写在body上面时，因为页面还未被加载，无法获取DOM对象，也无法操作DOM对象。如果真的一定要写在body上面的话，一定要将操作DOM的代码包在 `window.load`的响应函数中。一般我们希望先加载body再加载js代码（性能问题），所以一般都是script写在body下面。

#### DOM 操作

##### DOM 查询

通过document调用:

* getElementById

* getElementsByTagName

  根据标签名来获取一组元素节点对象

  返回一个<u>类数组</u>对象，包含所有查询到的元素

* getElementsByName

> 如果需要读取元素节点属性：
>
> 直接使用 元素节点对象.属性名
>
> 例外：class不能这样用，应该写成元素节点对象.className

通过具体的元素节点调用：（获取子节点）

* getElementsByTagName

  在指定的元素节点的后代中，根据标签名来获取一组元素节点对象

* childNodes

  获取包括文本节点、属性节点、元素节点的所有**子节点**

  根据DOM标准，标签间的空白也会被当成文本节点。（但是在IE8以下的版本中，不会将空白当做文本节点）

* children

  获取当前元素的所有**子元素**

  推荐使用这个！

* firstChild

  返回当前元素的第一个子节点（包括空白文本节点）

* firstElementChild

  返回当前元素的第一个子元素

  不兼容 IE8以下的版本

* lastChild

通过具体的元素节点调用：（获取父节点和兄弟节点）

* parentNode

  返回该元素节点的父节点（也就是父元素，不可能是文本节点）

* previousSibling

  返回前一个兄弟节点（包括空白文本节点）

* previousElementSibling

  返回前一个兄弟元素

  不兼容 IE8以下的版本

* nextSibling

##### **innerHTML** 

通过这个属性可以获取元素内部的**html代码**

对于自结束标签，这个属性没有意义。

##### innerText

 通过这个属性可以获取元素内部的**文本内容**

与innerHTML类似，但是会自动将html tag去除

有时候如果一个元素节点内只有文本节点，我们也可以通过 `currentNode.firstChild.nodeValue` 来获取文本内容。

##### 全选练习

checkbox这个input有个checked属性可以获取或者修改勾选框的勾选状态。value是勾选框后面的文本。

在事件的响应函数中，this指向绑定的元素。

```javascript
const checkbox = document.getElementById('xxx');

checkbox.onclick = function() {
	console.log(this === checkbox); // true
  // 此处跟react的onClick有所不同，因为那个是react帮你调用，这里是checkbox直接调用。还是跟调用的方式有关。
}
```

##### DOM查询剩余方法

* document.body

  获取body标签

* document.documentElement

  获取html根标签

* document.all

  页面中的所有元素

* document.getElementsByClassName()

  根据元素的class查询一组元素节点对象

  不兼容IE8以下的浏览器

* document.querySelector()

  需要一个选择器的字符串作为参数，可以根据css选择器来查询一个元素节点对象。

  功能非常强大。

  https://developer.mozilla.org/en-US/docs/Glossary/CSS_Selector

  能够兼容IE8浏览器。（弥补了document.getElementsByClassName不能使用）

  如果能找到多个满足条件的元素，只会返回第一个元素。

* document.querySelectorAll()

  将符合条件的元素封装到一个类数组对象中返回。

   能够兼容IE8浏览器。

##### DOM增删改

DOM node: https://developer.mozilla.org/en-US/docs/Web/API/Node

Node是一个接口，各种类型的 DOM API 对象会从这个接口继承。 （包括Element类）

Element: https://developer.mozilla.org/en-US/docs/Web/API/Element

Element 是一个通用性非常强的基类，所有 Document 对象下的对象都继承自它。

* document.createElement()

  可以用于创建一个元素节点对象。

  需要一个标签名作为参数，根据此标签名创建对象。

* document.createTextNode()

  可以用于创建一个文本节点对象

  需要文本内容作为参数。

* appendChild

  向一个父节点中添加一个新的子节点

  ```javascript
  const li = document.createElement('li');
  const text = document.createTextNode('guangzhou');
  
  li.appendChild(text);
  city.appendChild(li);
  ```

* insertBefore

  `parentNode.insertBefore(newNode, referenceNode)`

  可以在指定的子节点前面插入新的子节点

* replaceChild

  `parentNode.replaceChild(newChild, oldChild)`

* removeChild

  `node.removeChild(child)`

  child.parentNode.removeChild(child)

* 使用**innerHTML**也能完成DOM的增删改的相关操作。

  但是如果只用这种方法，动静太大，所有该父节点下的内容都更新了。

  **一般我们使用混合模式**：

  ```javascript
  const li = document.createElement('li');
  
  li.innerHTML = 'guangzhou';
  
  city.appendChild(li); // 这样其他子节点不会更新
  ```

##### 练习： 添加删除记录

如果不希望默认行为出现，可以在响应函数的最后 return false （如果是超链接，可以设置href="javascript:;"）

window对象的一些方法：

https://developer.mozilla.org/en-US/docs/Web/API/Window

* alert

* prompt

* confirm

  弹出一个带有确认和取消按钮的提示框，参数（字符串）会作为提示文字显示出来。

  如果用户点击确认，则返回true；否则返回false

#### css样式相关

##### object.style

通过js修改元素的样式：

`元素.style.样式名 = 样式值`

注意：css样式中含有`-` 的样式名在js中需要修改为驼峰命名法， 比如 background-color --> backgroundColor

通过js读取元素样式：

`元素.style.样式名`

注意：如果有设置`!important`，则即使通过js也无法改变样式

注意：通过style属性的设置和读取**都是内联样式**，无法读取样式表中的样式。

##### object.currentStyle

用来读取当前元素正在显示的样式

如果当前元素没有设置该样式，则返回默认值。（*left可能会返回**auto** 而不是 0*）

只读。

**然而，只有IE浏览器支持**

##### window.getComputedStyle

语法： `getComputedStyle(node对象，第二个参数可选（伪对象）).样式名`

只读。

**不兼容IE8以下浏览器**

如果当前元素没有设置该样式，则返回真实值而不是默认值。

##### 自定义一个兼顾object.currentStyle和window.getComputedStyle的方法

```javascript
function getStyle(obj, name) {
  if (window.getComputedStyle) { //注意这里不能直接写getComputedStyle，变量找不到会直接报错，而属性找不到只是返回undefined
    return getComputedStyle(obj, null)[name];
  } else {
    return obj.currentStyle[name];
  }
}
```

##### 其他css样式的操作

* element.clientHeight

  元素的可见高度

* element.clientWidth

  元素的可见宽度

  这两个属性都是不带px的，返回的是一个数字，可以直接进行计算。

  这个高度和宽度是包括内容区和内边距（padding）的

  两个属性都是只读的，不能修改

* element.offsetHeight

* element.offsetWidth

  获取元素的整个高度或宽度，包括内容区、内边距（padding）和边框（border）

* element.offsetParent

  获取当前元素的**定位**父元素

  会获取到离当前元素最近的开启了定位的祖先元素，直到返回body

* element.offsetLeft

* element.offsetTop

  返回当前元素与其**定位父元素**的水平偏移量或垂直偏移量

* element.scrollHeight

* element.scrollWidth

  获取元素整个滚动区域的高度和宽度

* element.scrollLeft

* element.scrollTop

  获取水平滚动条滚动的距离

  获取垂直滚动条滚动的距离

  当`scrollHeight - scrollTop === clientHeight` 时，说明垂直滚动条滚动到底了。

  当`scrollWidth - scrollLeft === clientWidth` 时，说明垂直滚动条滚动到底了。

* 练习：只有看完协议内容，才能勾选和注册

  监听协议容器的onscroll事件，然后判断是否滚动条滚动到底，再讲input元素的disabled设置为true

#### 事件

##### 事件对象

当事件的响应函数被触发时，浏览器每一次都会将一个事件对象作为实参传递给响应函数。

事件对象中封装了所有跟当前事件相关的一切信息。

继承了event对象接口的各种事件对象：https://developer.mozilla.org/en-US/docs/Web/API/Event#interfaces_based_on_event

> 注意：在IE8及以下版本的浏览器中，响应函数不会被传递事件对象。它们是将事件对象作为window对象的属性保存的。
>
> 为解决事件对象的兼容性问题：
>
> event = event || window.event;

* clientX, clientY

  获取鼠标指针在当前可见窗口的水平和垂直坐标

* pageX, pageY

  获取鼠标指针相对于当前页面的坐标。

  > 注意：这两个属性不兼容IE8
  >
  > 如果需要兼容IE8，则考虑通过scrollTop和clientY 或者scrollLeft和clientX的组合去计算

##### 练习：div随鼠标移动

left top这种css属性只对开启了定位的元素有用。（position属性）

注意：

Chrome等浏览器认滚动条是属于body的，可以通过body.scrollTop来获取

Firefox等浏览器认滚动条是属于html的，可以通过documentElement.scrollTop来获取

所以为了兼容各个浏览器，我们需要：

`document.body.scrollTop || document.documentElement.scrollTop ` 来获取scrollTop

 #####  事件的冒泡

就是时间的向上传导。当后代元素上的时间被触发时，其<u>祖先元素</u>的<u>相同事件</u>也会被触发。

在开发中的大部分情况中，冒泡都是有用的。（比如我们之前的div随鼠标移动，如果进入其他div但是事件没有被冒泡的话，则document的响应函数将无法被触发）

如果不希望发生事件冒泡，则可以通过事件对象来取消冒泡：

`event.cancelBubble = true;`

##### 事件的委派

将事件统一绑定给共同的祖先元素，这样当后代元素上的事件触发时，会一直冒泡到祖先元素，然后在祖先元素的响应函数中来处理事件。

利用了事件冒泡，通过委派可以减少事件绑定的次数，提高程序性能。

* event.target

  触发此事件的元素。（事件的目标节点）

通过event.target来判断是否需要在祖先元素中对该事件进行处理。

##### 事件的绑定

* 使用`对象.事件=函数`

  它只能同时为一个元素的一个事件绑定一个响应函数。

  如果绑定多次时，后面的绑定会覆盖之前的绑定。

* 使用 `对象.addEventListener()`

  参数：

  * 事件的字符串，不要加“on”
  * 回调函数，当事件触发时该函数会被调用
  * 是否在<u>捕获阶段</u>触发事件，是一个boolean值。默认为false

  使用该方法为元素绑定响应事件时，可以同时为一个事件绑定多个响应函数，且按照绑定的顺序执行响应函数。

  该方法不兼容IE8及以下浏览器

* 使用 `对象.attachEvent()`

  参数：

  * 事件的字符串，要加上“on”
  * 回调函数

  使用该方法为元素绑定响应事件时，可以同时为一个事件绑定多个响应函数，但是，按照绑定的顺序的**相反顺序**执行响应函数。

  支持IE5-10浏览器

**问题：如何自定义一个函数，使事件的绑定兼容所有浏览器？**

1. 事件的字符串有区别
2. 调用响应函数的顺序不同（所以不能拆分顺序有关的代码到不同的响应函数中）
3. 回调函数中的this有区别
   * addEventListener的this是被绑定的对象
   * attachEvent的this指向window

```javascript
function bind(obj, eventStr, callback) {
  if (obj.addEventListener) {
    obj.addEventListener(eventStr, callback, false);
  } else {
    obj.attachEvent('on' + eventStr, function() {
      callback.call(obj);
    });
  }
}
```

##### 事件的传播

网景(NetScape)和微软有不同的理解：

* 微软

  事件应该从内向外传播 --> 事件应该在冒泡阶段执行

* 网景

  时间应该从外向内传播。当事件触发时，应该先从触发事件的元素的最外层的祖先元素开始执行响应函数，然后再向内传播给后代

W3C综合了以上两种方案，将事件传播分成三个阶段：

1. 捕获阶段

   在捕获阶段时，从最外层的祖先元素，向目标元素进行事件的捕获，但**默认此时不会触发事件**

2. 目标阶段

   事件捕获到目标元素，捕获结束，开始在目标元素上触发事件

3. 冒泡阶段

   事件从目标元素向祖先元素传递，依次出发祖先元素的事件

如果希望在捕获阶段就触发事件，可以将addEventListener的第三个参数设为true （一般情况不会）

IE8及以下浏览器没有捕获阶段。

##### 拖拽事件

组合onmousedown, onmousemove, onmouseup

练习：拖拽一个div

```javascript
const box = document.getElementById('box');

box.onmousedown = function(event) {
	// 专门为IE8及以下所写，将所有其他鼠标点击行为捕获到box上，这样就算全选的时候，也不会使其他内容被拖拽，算是对浏览器默认行为的变相取消。但是一定要记得release，不然之后的其他所有鼠标行为都会再次触发box.onmousedown
	box.setCapture && box.setCapture();

	// 计算鼠标点击方块时与方块之间的水平和垂直偏移量，此偏移量需要在重新计算方位位置时减去。（维持方块和鼠标的偏移量
  event = event || window.event;
	const ol = event.clientX - box.offsetLeft;
  const ot = event.clientY - box.offsetTop;
  
	document.onmousemove = function(event) {
  	event = event || window.event;
  	// 根据鼠标的坐标来确定方块的实时坐标
  	const left = event.clientX - ol;
    const top = event.clientY - ot;
    
    box.style.left = left + 'px';
    box.style.top = top + 'px';
	}
  
  document.onmouseup = function() {
  	// 需要取消这些document的事件绑定，不然在拖拽行为结束之后，会影响其他鼠标行为
  	document.onmousemove = null;
    document.onmouseup = null;
    // 需要在鼠标松开时，需要release capture(IE8需要）
    box.releaseCapture && box.releaseCapture();
  }
  
  // 取消浏览器的默认行为（拖拽一个网页内容时，浏览器会默认去搜索引擎搜索内容）
  // 但是IE8及以下不支持，需要使用setCapture和releaseCapture来帮助实现
  return false;
}
```

有一组拖放事件的API，drag and drop

https://developer.mozilla.org/en-US/docs/Web/API/HTML_Drag_and_Drop_API

##### 滚轮事件

鼠标滚轮滚动的事件

老师课上讲的onmousewheel 已经不建议使用了： https://developer.mozilla.org/en-US/docs/Web/API/GlobalEventHandlers/onmousewheel

现在推荐使用标准的wheel事件：https://developer.mozilla.org/en-US/docs/Web/API/Element/wheel_event

此方法不兼容IE8，所以还是需要使用onmousewheel （但是我觉得已经不太有人还在用IE8了吧。。。）

* event.deltaX

  horizontal scroll amount

* event.deltaY

  vertical scroll amount

  往上滚 大于 0

  往下滚 小于 0

练习：向上滚动box变短，向下滚动box变长

```javascript
const box = document.getElementById('box');

box.onwheel = function(event) {
	if (event.deltaY > 0) {
  	box.style.height = box.clientHeight + 10 + 'px';
  } else {
  	box.style.height = box.clientHeight - 10 + 'px';
  }
  
  return false;
}
```

> 如果事件用onxxx绑定，可以使用return false进行默认行为的取消；
>
> 如果事件是用addEventListener绑定的，只能使用event.preventDefault()来取消浏览器的默认行为

对比另一个类似的onscroll事件： https://developer.mozilla.org/en-US/docs/Web/API/GlobalEventHandlers/onscroll

##### 键盘事件

键盘事件一般都会绑定给一些可以获取焦点的对象或者是document

https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent

* onkeydown

  按键被按下

  会连续出发

  第一次和第二次间隔比较长，后面的触发会非常快，这种设计是为了防止误操作的发生。

  在文本框中输入内容是onkeydown的默认行为

  > ^^ 可以利用上面这个特性，来限制文本框中能输入的内容！那些不能被输入的内容 return false

* onkeyup

  按键松开

  不会连续触发

键盘事件的属性：

* keyCode(已经过时)

  按键的编码

* altKey

* ctrlKey

* metaKey

* shiftKey

  用于判断alt或control或meta或shift键是否跟其他某个键同时被按下

很多跟按键值相关的属性已经过时（比如keyCode，char，charCode，which），需要使用**event.key**来代替！

可以在这里测试key的值：https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent/key#result

```javascript
window.addEventListener("keydown", function (event) {
  if (event.defaultPrevented) {
    return; // Do nothing if the event was already processed
  }

  switch (event.key) {
    case "Down": // IE/Edge specific value
    case "ArrowDown":
      // Do something for "down arrow" key press.
      break;
    case "Up": // IE/Edge specific value
    case "ArrowUp":
      // Do something for "up arrow" key press.
      break;
    case "Left": // IE/Edge specific value
    case "ArrowLeft":
      // Do something for "left arrow" key press.
      break;
    case "Right": // IE/Edge specific value
    case "ArrowRight":
      // Do something for "right arrow" key press.
      break;
    case "Enter":
      // Do something for "enter" or "return" key press.
      break;
    case "Esc": // IE/Edge specific value
    case "Escape":
      // Do something for "esc" key press.
      break;
    default:
      return; // Quit when this doesn't handle the key event.
  }

  // Cancel the default action to avoid it being handled twice
  event.preventDefault();
}, true);
```

### BOM -- browser object model

浏览器对象模型

可以通过js来操作浏览器

BOM对象：

* Window
  * https://developer.mozilla.org/en-US/docs/Web/API/Window
  * 代表整个浏览器的窗口，同时window也是网页中的全局对象
* Navigator
  * 代表当前浏览器的信息，通过该对象来识别不同浏览器
* Location
  * 代表当前浏览器的地址栏信息，通过location可以获取地址栏信息，或者操作浏览器跳转页面
* History
  * 代表浏览器的历史记录，可以通过该对象来操作浏览器的历史记录
  * 由于隐私问题，该对象不能获取到具体的历史记录，只能操作浏览器向前或者向后翻页
  * 只在单次session有效
* Screen
  * 代表用户的屏幕信息。可以获取用户的显示器相关的信息

这些BOM对象都是作为window对象的属性保存的，可以通过window来使用，或者直接使用。

window.navigator, window.location, window.history, window.screen

##### navigator

https://developer.mozilla.org/en-US/docs/Web/API/Navigator

由于历史原因，大部分属性都已经不能帮助我们识别浏览器了。deprecated

一般我们只会使用userAgent来识别浏览器。

* userAgent

  是一个字符串，包含有用来描述浏览器信息的内容。

  如果不能使用userAgent来判断，还可以通过一些浏览器独有的对象来帮助判断浏览器类型。

  但是IE11(Edge)很tricky，它已经从userAgent抹去了msie的标识，并且如果你去检查`!!window.ActiveObject`，它会返回false来迷惑你。

```javascript
if (/firefox/i.test(ua)) {
  // 火狐
} else if (/chrome/i.test(ua)) {
  // chrome
} else if (/msie/i.test(ua)) {
  // IE10及以下浏览器
} else if ('ActiveObject' in window) {
  // IE11 即 Edge
}
```

##### history

https://developer.mozilla.org/en-US/docs/Web/API/History

https://developer.mozilla.org/en-US/docs/Web/API/History_API

* length

  the number of elements in the session history, including the currently loaded page

  历史堆栈中页面的数量

  第一个页面为1

* back()

* forward()

* go(integer)

  向前或向后跳转n个页面

* pushState(), replaceState()

  https://developer.mozilla.org/en-US/docs/Web/API/History_API/Working_with_the_History_API

  HTML5开始，提供了对history栈中内容的操作。

  添加和修改历史记录条目。这些方法通常与window.onpopstate 配合使用。

##### location

https://developer.mozilla.org/en-US/docs/Web/API/Location

直接给location赋值，页面会自动跳转并且产生相应的历史记录。

* assign()

  与直接给location赋值相同

* reload()

  用于重新加载当前页面，相当于刷新按钮

  如果传入true作为参数，则会强制清空缓存刷新页面（所有input的值都清空）

* replace()

  使用一个新页面替换当前页面

  不会产生历史记录

#### 定时器

##### 定时调用

* setInterval

  可以每隔一段时间执行一次某个函数

  返回值是一个number，作为定时器的唯一标识

* clearInterval

  关闭定时调用的定时器

  需要传入定时器的唯一标识作为参数

  即使传入的值不是有效的（比如undefined）也不会报错，只是不做任何事情

在开启定时器之前，需要将当前元素上的定时器关闭。否则可能出现问题。

##### 解决之前键盘控制div移动的问题

之前在学习onKeydown的时候，我们能实现：按下上下左右键能控制div的移动，并且按下ctrl键能够加速。但是问题是：一直按着方向键的时候，第一次事件和第二次事件的延迟是肉眼可见的。（是by design的）

为解决这个问题，我们应该将div移动的那部分代码放到一个很快的定时调用中，然后onkeydown只用来控制速度和方向。这样连续onkeydown事件的一开始的卡顿问题就能得到解决。

```javascript
let dir, speed = 10;

setInterval(function() {
  switch(dir) {
    // 这里根据dir的值，去更改div的坐标已达到移动的效果
    // speed用来体现速度
  }
}, 30);

document.onKeyDown = function(e) {
  if (e.ctrlKey) {
    speed = 100;
  } else {
    speed = 10;
  }
  
  switch(e.key) {
      // 根据e.key的值，去设置dir
  }
}

document.onKeyUp = function() {
  dir = 0; // 该dir是定时调用中switch语句中不包含的值，所以div的坐标不再改变
}
```

##### 延时调用

* setTimeout

  延时调用一个函数，不是马上执行，而且在一段时间后执行，并且只会调用一次。

* clearTimeout

  关闭一个延时调用

> 延时调用和定时调用实际上是可以互相代替的，在开发中我们根据需求去选择使用哪个

##### 定时器的应用

使用js实现一个div的动画：

* 点击button能使一个div往右移动，到达终点时停止
* 点击另一个button能使该div往左移动，达到出发点停止

整合一个函数，move(对象，目标，速度)来实现 --> 速度为正值，通过currentstyle和target的关系来判断是在向左还是向右移动。

但是！！如果有多个div需要使用我们整合的函数的话，就会出问题。因为我们的timer现在存在全局变量中！

> timer变量不能存在全局变量中，必须是跟你要控制的那个元素存在一起（比如存为要控制的元素对象的属性，obj.timer

**实现更多功能**：

比如要向上或向下移动

比如要变长或变短（水平/垂直）

我们需要更新move函数以能为更多功能所复用： `move(obj, attr, target, speed, callback)`

```javascript
function move(obj, attr, target, speed, callback) {
  clearInterval(obj.timer);
  
  if (current > target) {
    speed = -speed; // 如果现在的值大于target说明正在往负方向运行
  }
  
  obj.timer = setInterval(function() {
    const current = parseInt(getStyle(obj, attr)); // getStyle是我们之前定义的函数，为了获取对象的当前样式
  	const newValue = current + speed;
    
    // 已经达到或超过目标值（因为interval就算再小，也是interval）
    if ((speed > 0 && newValue >= target) || (speed < 0 && newValue <= target)) {
      newValue = target;
    }
    
    obj.style[attr] = newValue + 'px';
    
    if (newValue === target) {
      clearInterval(obj.timer);
      
      callback && callback();
    }
  }, 30); // speed和30之前的combination还有待改进，可以指定30为单位，speed是在该单位下的速度
}
```

#### 实现轮播图（当面试题实现一下

本质是控制一个长图的样式的left，父元素的overflow是hidden

 点击page indicator时，要切换到那张图片

不操作时，应该定时放下一张图片

使用之前写的move函数来进行长图的移动效果

* 当移动到最后一张时，move到第一张图片的时间太长了：

  **解决办法：**偷天换日

  我们在长图的最后面再加一张与第一张图片一样的图片，然后当从最后一张图片要转回第一张时，它会先move到最最后一张（跟第一张一样的图），然后设置style等于0 --> 利用视觉效果，实现一种直接从最后一样转回第一张且时间跟之前的转动一样的效果且没有反着转的情况。

* 当按下按钮时，可能会跟自动转动的定时器产生collision

  按下按钮时需要取消定时器，然后在完成move效果后，再开启定时器。（传回调函数给move）

#### 类的操作 class

通过style属性来修改元素的样式，有这些问题：

* 每修改一个样式，浏览器就要重新渲染一次页面，执行性能比较差
* 当需要修改多个样式时，也不太方便

所以其实比较少通过这种方式去修改样式

我们可以通过修改元素的**class属性**来间接地修改样式。除了改善上述问题，还进一步分离了表现和行为。

为此，我们可以定义一些好用的函数：

```javascript
function addClass(obj, className) {
  if (!hasClass(obj, className)) {
    obj.className += ' ' + className;
  }
}

function hasClass(obj, className) {
  const regex = new RegExp('\\b' + className + '\\b');
  
  return regex.test(obj.className);
}

function removeClass(obj, className) {
  const regex = new RegExp('\\b' + className + '\\b');
  
  regex.replace(obj.className, '');
}

function toggleClass(obj, className) {
  if (hasClass(obj, className)) {
    removeClass(obj, className);
  } else {
    addClass(obj, className)
  }
}
```

#### 二级菜单实现（当面试题实现一下

或只实现一下js部分的代码

控制height去实现隐藏二级菜单，放在一个collapsed的class下。

* 绑定单击响应事件

* 实现过渡效果  --> 还是借助我们之前写过的move函数 

  先获得添加collapsed类前后的高度，然后<u>重置为原先的高度</u>，然后借助move函数去实现过渡的动画

  由于move函数是修改内联样式，我们需要在动画结束后删除内联样式，不然会无法展开（因为内联样式优先级更高）

* 关闭之前展开的菜单也需要有过渡效果不然很突兀

### JSON

* JS中的对象只有js自己认识，但是网页开发中往往需要前后端交互，我们需要一种数据交互的形式
* JSON就是一个特殊格式的字符串，可以被任意语言**识别**，并且可以**转换**为任意语言中的对象
* JSON在开发中主要用来进行数据的交互

JSON概念

* JavaScript object notation

* 格式跟js一样，只不过JSON字符串中的属性名必须要双引号

* 分类

  * 对象 {}
  * 数组 []

* 允许的值

  * 字符串
  * 数值
  * 布尔值
  * null
  * 普通数组（不能有函数）
  * 数组

* 将JSON字符串转换为js对象

  在js中提供了一个工具类叫 `JSON` ，可以帮我们将json转换为js对象，也可以将js对象转换为一个json字符串

  * JSON.parse()

    参数为需要转换的json字符串

  * JSON.stringify()

    参数为需要转换的js对象

但是JSON这个工具类在IE7及以下浏览器中不存在，所以如果真的需要兼容IE7的话，我们需要引入一个外部文件，来构建这个JSON类。

补充一个**eval**函数：

* 能执行一段js代码的字符串
* 如果字符串中有{}并且我们不希望eval把它解释成代码块的意思的话，我们需要在{}外面再加一对括号()进行区分
* 功能很强大，但是我们建议不要使用。性能较差；有安全隐患（最主要的原因）。







## JS高级

### 数据类型

#### 分类

* 基本数据类型
  * number
  * boolean
  * string
  * undefined
  * null
* 对象（引用）类型
  * 对象
  * 数组
  * 函数

#### 判断

* typeof

  返回的是字符串

  可以判断undefined

  但是不能判断null 因为 typeof null --> "object"

  typeof 一个array 返回的是 "object"

  typeof 一个函数 返回的是 "function"

* instanceof

  判断对象的具体类型

  `A instance of B`

  B 应该是一个构造函数， A 是不是 B 的实例对象

  返回一个boolean

* ===

#### 相关问题

1. undefined与null的区别？

   定义了，一个未赋值，一个赋值为null

2. 什么时候给变量赋值为null？

   赋值为null一般是为了表明之后会被赋值为一个对象；或者为了释放对象所占的内存空间。

3. 严格区分变量类型与数据类型？（其实我个人觉得根本没要这么细节）

   数据类型：

   * 基本类型
   * 对象类型

   变量类型：（变量本身没有类型，指的是变量内存值的类型）

   * 基本类型 --> 保存的是数据值
   * 引用类型 --> 保存的是地址值

### 数据，变量，内存

* 数据

  存储在内存中的代表特定信息的东西，本质上是0101

* 变量

  可变化的量。由变量名和变量值组成

  变量名用来找对应的内存，变量值就是内存中保存的数据

* 内存

  内存条通电后产生的可储存数据的空间。

  内存分类：

  * 栈

    全局变量和局部变量

  * 堆

    对象

三者关系：内存用来存储数据的空间；变量是内存的标识。

#### 相关问题

1. 关于赋值和内存的问题

   var a = xxx， a内存中保存的是什么？

   如果xxx是基本数据

   如果xxx是对象

   如果xxx是变量

2. 关于引用变量赋值的问题

   n个引用变量指向了同一个对象？

   某个变量赋值了另一个对象？

3. js在调用函数时，传递变量参数时，是值传递还是引用传递

   理解1：值传递（可能基础数据，也可能是地址值）

   理解2：值传递，或者引用传递（地址值）

4. js引擎如何管理内存

   * 内存生命周期
   * 释放内存
     * 局部变量在函数执行完后自动释放
     * 对象在成为垃圾对象后，会在后面的某个时刻被垃圾回收器释放

### 对象

1. 什么是对象？

   * 多个数据的封装体
   * 一个对象代表现实中一个事物

2. 为什么要用对象？

   统一管理多个数据

3. 对象的组成

   属性：属性名 + 属性值

   方法：一种特别的属性，属性值是一个函数

4. 如果访问对象内部数据？

   obj.属性名

   obj[属性名]  --> 更通用

   什么时候必须要使用中括号那种形式呢？

   * 属性名包含特殊字符，-  空格等
   * 属性名不确定，是变量

### 函数

函数是js语言中最复杂的内容，可变化的东西太多。

#### 基本概念

1. 什么是函数？

   实现特定功能的n条语句的封装体

   只有函数是可以被调用的，被执行的。

2. 为什么要用函数？

   提高代码复用（封装）

   便于阅读交流

3. 函数是如何定义的？

   * 函数声明

     `function fn() {}`

   * 表达式声明

     `var fn = function() {}`

   区别在于变量提升（用var声明的变量）以及函数提升（提前被定义好）

4. 如何调用（执行）函数？
   1. 直接调用
   2. 通过对象调用
   3. new调用
   4. call或者apply调用（提供this上下文对象）--> 可以让一个函数成为任意对象的方法进行调用

#### 回调函数

1. 什么函数才是回调函数？
   1. 自定义的
   2. 自己没有调用
   3. 但也执行了
2. 常见的回调函数
   * dom事件回调函数
   * 定时器回调函数
   * ajax请求回调函数
   * 生命周期回调函数

#### IIFE

Immediately-invoked Function Expression

其实就是匿名函数自调用

作用：

* 隐藏实现（体现闭包）
* 不会污染外部（全局）命名空间
* 可以用来编写js模块

#### 函数中的this

1. this是什么？

   任何函数本质上都是通过某个对象来调用的，如果没有指定，就是window

   所有函数内部都有一个变量this

   它的值就是调用函数的当前对象。

2. 如何确定this的值
   * test() --> window
   * p.test() --> p
   * new Person() --> 新创建的对象
   * test.call(obj) --> obj

```javascript
function Person(name) {
  console.log(this);
  this.name = name;
  this.showName = function() {
    console.log(this);
  };
  this.setName = function(name) {
    this.name = name;
    console.log(this);
  }
}

Person('name'); // this --> window
const p = new Person('name'); // this --> p

p.showName() // this --> p

const obj = {};
p.showName.call(obj); // this --> obj

const fun = p.showName;
fun(); // this --> window

function f1() {
  function f2() {
    console.log(this);
  }
  
  f2();
}
f1(); // this --> window
```

### 函数高级

#### 原型与原型链

##### 函数的prototype

1. 函数的prototype属性

   每个函数都有一个prototype属性，它默认指向一个Object空对象（空对象指的是没有自己定义的属性和方法） --> 原型对象

   原型对象中有一个属性constructor，指向函数对象。

   prototype和constructor互相引用

   也就是说 `Date.prototype.constructor === Date`

2. 给原型对象添加属性（一般都是方法）

   函数（构造函数）的所有实例对象自动拥有原型对象中的属性（方法）

   ```javascript
   const Fun = function() {
     
   };
   Fun.prototype.test = function() {
     console.log('this is test');
   };
   
   const fn = new Fun();
   fn.test();
   ```

##### 显式原型和隐式原型

1. 每个函数对象都有一个prototype，即显式原型

2. 每个实例对象都有一个`__proto__`， 即隐式原型

3. 对象的隐式原型的值为对应的构造函数的显式原型的值

4. 内存结构如图所示：

   ```javascript
   function Fn() {};
   const fn = new Fn();
   
   console.log(Fn.prototype === fn.__proto); // true
   Fn.prototype.test = function() {
     console.log('this is test function');
   };
   fn.test(); // this is test function
   ```

   ![Screen Shot 2021-10-04 at 12.47.47 AM](/Users/hualinfeng/Library/Application Support/typora-user-images/Screen Shot 2021-10-04 at 12.47.47 AM.png)

5. 总结：
   * 函数对象的prototype属性是在定义函数时自动添加的，默认值是一个空对象
   * 实例对象的`__proto__` 是在创建对象时自动添加的，默认值为构造函数的prototype
   * 程序员能直接操作prototype 但是不能直接操作`__proto__` (es6之前)

##### 原型链

注意console log一个引用变量时，它输出的不是即时值，而是一个最新的值。（因为本质存的其实是一个地址值）

**原型链解析：（见下图解）**

访问一个对象的属性（方法）时，会先在自身属性查找，如果没有则会沿着`__proto__`这条链向上查找，找到则返回，如果直到Object.prototype也还没找到则返回undefined

所以原型链的别名又叫做**隐式原型链**，因为它依赖于`__proto__`

![Screen Shot 2021-10-04 at 12.19.26 PM](/Users/hualinfeng/Library/Application Support/typora-user-images/Screen Shot 2021-10-04 at 12.19.26 PM.png)

以下我们分析**两个比较特殊的函数**，**function Object(), function Function()**

下面三条信息可由下图体现出来：

![Screen Shot 2021-10-04 at 12.54.31 PM](/Users/hualinfeng/Desktop/Screen Shot 2021-10-04 at 12.54.31 PM.png)

1. **函数对象的显式原型**指向的对象，默认是空的**Object的实例对象**。（**但是Object自己除外**）

   `function Object()` 

   ```javascript
   Fn.prototype instanceof Object; // true
   Object.prototype instanceof Object; // false  --> 唯一的例外，因为这将是终点
   Function.prototype instanceof Object; // true
   Function.prototype.__proto__ === Object.prototype; // true，所以Function实例（也就是说任意函数）也能根据隐式原型链一直追溯到null --> 上图中我们自己加的那条__proto__的线非常关键，条条大路通Object.prototype
   ```

2. 所有函数都是Function的**实例** （包括Function自己）

   ```javascript
   fn.__proto__ === Function.prototype // true
   Object.__proto__ === Function.prototype // true
   Function.__proto__ === Function.prototype // true

3. Object的原型对象是原型链的**尽头**

   ```javascript
   console.log(Object.prototype.__proto__); // null
   ```

**原型链 -- 属性问题**

1. 读取对象的属性值，会去原型链中查找
2. 设置/修改对象的属性值时，不会查找原型链。如果当前对象没有此属性，直接添加该属性并赋值
3. 方法一般定义在原型对象中；函数一般通过构造函数定义在实例对象本身上。

```javascript
function Person(name) {
  this.name = name;
}
Person.prototype.setName = function(name) {
  this.name = name;
};

const p1 = new Person('apple');
const p2 = new Person('banana');

p1.setName('a');
p2.setName('b');

console.log(p1.__proto__ === p2.__proto__); // true
```

##### 探索instanceof

A instanceof B

如果B函数的显式原型对象在A对象的原型链上，则返回true；否则返回false

* 案例分析：

  ```javascript
  function Foo() {};
  const f1 = new Foo();
  f1 instanceof Foo; // true
  f1 instanceof Object; // true
  
  Object instanceof Function; // true
  Object instanceof Object; // true  Object.__proto --> Function.prototype, Function.prototype.__proto --> Object.prototype
  Function instanceof Function; // true
  Function instanceof Object; // true
  Object instanceof Foo; // false
  ```

看instanceof的结果其实就是跟着原型链画图。

##### 面试题

1. ```javascript
   function A() {}
   A.prototype.n = 1;
   const b = new A();
   A.prototype = {
     n: 2,
     m: 3
   };
   const c = new A();
   console.log(b.n, b.m, c.n, c.m); // 1 undefined 2 3
   ```

2. ```javascript
   const F = function(){};
   Object.prototype.a = function() {
     console.log('aaaa');
   };
   Function.prototype.b = function() {
     console.log('bbbb');
   };
   const f = new F();
   f.a(); // aaaa
   f.b(); // f.b is not a function
   F.a(); // aaaa
   F.b(); // bbbb
   ```

#### 执行上下文和执行上下文栈

##### 变量提升和函数提升

变量提升仅跟var有关

1. 变量声明提升

   通过var定义的变量，在定义语句前就能访问到，只不过值为undefined

2. 函数声明提升

   通过function声明的函数，之前就可以调用

```javascript
console.log(a); //undefined
var a = 1;
test(); // undefined
function test() {
  console.log(a);
  var a = 2;
}
test2(); // test2 is not a function！！！！ 此时test2是undefined
var test2 = function(){};
```

#####  执行上下文 execution context

1. 代码分类：
   * 全局代码
   * 函数（局部）代码
2. 全局执行上下文
   * 在执行全局代码前，将window确定为全局执行上下文
   * 对全局数据进行预处理
     * var定义的全局变量 ==> undefined，添加为window属性
     * function声明的全局函数 --> 赋值，添加为window方法
     * this = window
   * 开始执行全局代码
3. 函数执行上下文
   1. 在调用函数，准备执行函数体之前，创建对应的函数执行上下文对象（其实这个对象并不是完全真实的对象，表示着栈内存里的一片封闭的区域，每执行一次函数都会开辟一片内存空间给它，函数执行完毕后那片内存会被释放）
   2. 对局部数据进行预处理
      * 形参 --> 实参 --> 添加为执行上下文对象的属性
      * arguments --> 实参列表 --> 添加为执行上下文对象的属性
      * var定义的局部变量 --> undefined --> 添加为执行上下文对象的属性
      * function声明的函数  --> 赋值 --> 添加为执行上下文对象的方法
      * this --> 赋值 （调用函数的对象）
   3. 开始执行函数体

##### 执行上下文栈

1. 在全局代码执行前，JS引擎就会创建一个栈来管理所有的执行上下文对象
2. 在全局执行上下文window确定后，将其加入栈中（压栈） --> window一直处于栈底
3. 在调用执行函数，函数执行上下文创建后，将其添加到栈中
4. 在当前函数执行完后，将栈顶对象移除（出栈） --> 当前正在执行的函数的执行上下文对象一直在栈顶
5. 当所有代码执行完之后，栈中只剩下window

##### 面试题

1. 依次输出什么？
2. 整个过程产生了几个执行上下文？--> 5

```javascript
console.log(i);
var i = 1;
foo(1);

function foo(i) {
  if (i === 4) {
    return;
  }
  console.log('begin: ', i);
  foo(i + 1);
  console.log('end: ', i);
}

console.log('ge: ', i);
```

3. ```javascript
   function a() {}
   var a;
   console.log(typeof a); // function
   // 先执行变量提升，后执行函数提升
   ```

4. ```javascript
   if (!(b in window)) {
     var b = 1;
   }
   console.log(b); // undefined
   ```

5. ```javascript
   var c = 1;
   function c(c) {
     console.log(c);
   }
   c(2); // c is not a function --> 因为变量提升在代码执行之前
   ```

#### 作用域与作用域链

##### 作用域

1. 理解

   静态的，在代码编写好的时候就已经确定

2. 分类

   全局作用域

   函数作用域

   **块作用域（es6之后）**

3. 作用

   隔离变量，不用作用域下同名变量不会有冲突

##### 作用域与执行上下文

1. 创建的时间不同
   * 全局作用域外，每个函数都会创建自己的作用域，作用域是在函数定义时就确定了，不是函数调用时
   * 全局执行上下文是在全局作用域确定后，js代码马上执行前创建
   * 函数执行上下文是在调用函数时，函数体代码执行前创建

2. 静态 vs 动态（会随着函数的调用创建和自动释放）
3. 联系
   * 执行上下文对象是从属于所在的作用域的（作用域链查找）
   * 全局上下文环境 --》 全局作用域
   * 函数上下文 --》 对应的函数作用域

##### 作用域链

先在当前作用域下的执行上下文查找 --》 上一级作用域的执行上下文 --》 。。。 --》 全局作用域（其实就是全局执行上下文）

弄不清楚可以先画出各个作用域，找出作用域的上下级关系，再去看执行上下文

##### 面试题

```javascript
var x = 10;
function fn() {
  console.log(x);
}
function show(f) {
  var x = 20;
  f();
}
show(fn); // 10
```

```javascript
var fn = function() {
  console.log(fn);
};
fn();

var obj = {
  fn2: function() {
    console.log(fn2)
  }
};
obj.fn2(); // fn2 is not a function --> 应该改成 console.log(this.fn2)
```

#### 闭包

##### 由一个常见的循环遍历加监听的例子引入

```javascript
var btns = xxx;

for (var i = 0, i < btns.length; i++) {
  var btn = btns[i];
  
  btn.onclick = function() {
    console.log(i);
  }
}
console.log(i);
// 每个button的单击响应都是一样的数字
```

原因：i不是定义在函数里的，所以是全局变量，onclick也不是马上执行，等执行的时候，i已经变成了btns.length

解决方法： **闭包** 或者 使用let来进行变量的命名（使用let声明迭代变量时，js引擎会在后台为每一个迭代循环声明一个新的迭代变量，所以其实setTimeout里的那个i都不是同一个变量实例）

```javascript
var btns = xxx;

for (var i = 0, i < btns.length; i++) {
  function(i) { // 此i非彼i
    var btn = btns[i];
  
    btn.onclick = function() {
      console.log(i);
    }
  }(i);
}
console.log(i);
```

##### 闭包理解

利用Chrome的Dev tool可以清楚看到闭包。

1. 如何产生闭包？

   当一个函数嵌套的内部函数引用了外部函数的变量或函数时，就产生了闭包

2. 闭包是什么？

   * 理解一： 闭包是嵌套的内部函数
   * 理解二：闭包是嵌套的内部函数中包含引用外部函数的变量或函数的对象！（从Chrome dev来看，我觉得是这种。在嵌套函数被执行时，以closure的形式存在于执行上下文对象中；在执行函数定义时，也能看到附着于嵌套函数上）

3. 产生闭包的条件

   * 函数嵌套
   * 内部函数引用了外部函数的数据
   * 执行了内部函数的定义（不一定要被执行才产生闭包）

##### 常见的闭包

1. 将函数作为另一个函数的返回值

   ```javascript
   function fn1() {
     var a = 2;
     
     function fn2() {
       a++;
       console.log(a);
     }
     
     return fn2;
   }
   
   var f = fn1();
   f(); //3
   f(); //4
   // 整个过程因为外部函数只执行了一次，所以产生了一个闭包
   ```

2. 将函数作为实参传给另一个函数调用

   ```javascript
   function showDelay(msg, time) {
     setTimeout(function() {
       alert(msg);
     }, time);
   }
   
   showDelay('hello', 1000);
   ```

##### 闭包的作用

1. 函数内部的参数在函数执行完之后，依然存活在内存中（延长了局部变量的生命周期）
2. 让函数外部能操作函数内部的数据（通过暴露产生的闭包的函数给外部使用）

问题：

1. 函数执行完之后，函数内部声明的局部变量是否还存在？

   除了存在于闭包中的变量（且拥有该闭包的函数对象没有被GC处理掉），其他都不存在

2. 在函数外部能直接访问函数内部的数据吗？

   不能！但是可以通过闭包来让外部操作内部数据。（不是随心所欲的操作）

```javascript
function fn() {
    var a = 2;
    
    function fn1() {
        a++;
        return a;
    }
    
    function fn2() {
        a--;
        return a;
    }
    
    return {fn1, fn2};
}

const x = fn();
// fn函数执行后，a变量依然存在（由于闭包），变量fn1和fn2已经释放
// 且x.fn1 和 x.fn2 操作的是同一个变量a
```

##### 闭包的生命周期

* 产生

  在嵌套内部函数定义执行时产生（不是调用时）

  比如上面那个代码，在刚开始执行fn时，fn1和fn2函数被声明定义时（函数声明提前），闭包就已经被产生

* 死亡

  在嵌套的内部函数对象成为垃圾对象时

  比如上面那个代码的 x.fn1 = undefined时，x.fn1里的闭包被释放

##### 闭包的应用--自定义js模块

js模块：

* 具有特定功能的js文件
* 将所有数据和功能都封装在一个函数内部（数据私有化，封装）
* 只向外暴露一个包含一些方法的对象或函数
* 模块的使用者，只能（只需要）通过暴露的对象或函数来调用方法来实现对应的功能

1. 可以直接return {method1: xxx, method2:xxxxx}
2. 也可以利用IIFE直接将模块绑定到window对象上。比如window.myModole = {method1: xxx}

##### 闭包的缺点及解决 -- 内存溢出和泄露

缺点：

函数执行完后，函数内部的局部变量没有释放，占用内存时间会变长。容易造成内存泄露。

解决：

能不用闭包就不用

及时释放内存 --》 让拥有闭包的内存函数对象在使用完之后成为垃圾对象（赋值为null或undefined）

补充：

* **内存溢出**

  一种程序运行出现的错误

  当程序运行需要的内存超过了剩余的内存时，就会抛出内存溢出的错误

* **内存泄露**

  占用的内存没有被及时释放

  内存泄露积累多了就容易导致内存溢出

  常见的内存泄露：

  * 意外的全局变量

    ```javascript
    function fn() {
      a = 1;
      console.log(1);
    }
    ```

  * 没有及时清理的定时器或回调函数

  * 闭包（没有及时释放内部函数）

##### 面试题

```javascript
var name = 'Window';
var object1 = {
  name: 'object1',
  getName: function() {
    return function() {
      return this.name;
    };
  }
}
var object2 = {
  name: 'object2',
  getName: function() {
    var that = this;
    return function() {
      return that.name;
    };
  }
}

object1.getName(); // Window
object2.getName(); // object2
```

还有一道终极闭包面试题！！（林锋也是全对）

```javascript
function fun(n, o) {
  console.log(o);
  
  return {
    fun: function(m) {
      return fun(m, n);
    }
  };
}

var a = fun(0); // undefined
a.fun(1); // 0
a.fun(2); // 0
a.fun(3); // 0
var b = fun(0).fun(1).fun(2).fun(3); // undefined 0 1 2
var c = fun(0).fun(1); // undefined 0
c.fun(2); // 1
c.fun(3); // 1
```

### 面向对象高级

#### 对象创建模式

* Object构造函数模式
  * 先创建空对象，再动态添加属性和方法
  * 适用场景：初始数据不确定
  * 问题：代码很多
* 对象字面量模式
  * 使用{}创建对象，同时指定属性和方法
  *  适用：初始时对象内部数据是确定的
  * 问题：如果创建对象很多的话，有重复代码
* 工厂模式
  * 通过工厂函数动态创建对象并返回
  * 适用：要创建多个对象
  * 问题：对象没有一个具体的类型，都是Object类型
* 自定义构造函数
  * 自定义构造函数，通过new创建对象
  * 适用：需要创建多个类型确定的对象
  * 问题：相同的方法会创建不同的函数对象，浪费内存
* 构造函数+原型
  * 自定义构造函数，属性在构造函数中初始化，方法添加到构造函数的原型对象上
  * 适用：需要创建多个类型确定的对象

#### 继承模式

##### 原型链继承

图解：最关键的一步就是**令子类型的原型为父类型的实例对象**（图中红色过程）

![Screen Shot 2021-10-08 at 4.48.20 PM](/Users/hualinfeng/Library/Application Support/typora-user-images/Screen Shot 2021-10-08 at 4.48.20 PM.png)

以上代码还有个错误的地方：

sub.constructor现在指向了错误的地方，现在指向了父类函数对象

所以我们要在完成继承那一步时还要纠正这个问题：

```javascript
Sub.prototype = new Supper();
Sub.prototype.constructor = Sub;
```

##### 借用构造函数继承（假继承）

```javascript
function Person(name, age) {
  this.name = name;
  this.age = age;
}
function Student(name, age, price) {
  Person.call(this, name, age);
  this.price = price;
}
```

##### 组合继承

原型链继承 + 借用构造函数继承  ==》 应该用这种方式来实现继承

1. 利用原构造函数初始化相同的属性

### 线程机制与事件机制

#### 进程与线程

* 进程process

  程序的一次执行，占有一片独有的内存空间

  可以通过资源管理器看到

  有的程序是多进程的（比如Chrome），有的是单进程的。

* 线程thread

  * 是进程内的一个独立执行单元
  * 是程序执行的一个完成流程
  * 是CPU的最小的调度单元

  有的进程是多线程，有的是单线程

* 相关知识

  * 应用程序必须运行在某个进程的某个线程上
  * 一个进程中至少有一个运行的线程：主线程，进程启动后自动创建
  * 一个进程中可以同时运行多个线程，多线程运行
  * 一个进程内的数据可以供其中的多个线程直接共享
  * 多个进程之间的数据是不能直接共享的
  * 线程池thread pool：保存多个线程对象的容器，实现线程对象的反复利用

* 相关问题

  * 比较多线程和单线程
    * 多线程
      * 优点：能有效提升cpu的利用率
      * 缺点：
        * 创建多线程开销
        * 线程间切换开销
        * 死锁与状态同步问题
    * 单线程
      * 优点：顺序编程，简单易懂
      * 缺点：效率低
  * JS是单线程运行的，但H5中的web workers可以多线程运行
  * 浏览器是多线程运行的。
  * 有些浏览器是多进程的（Chrome和新版IE），有些浏览器是单进程的（Firefox和旧版IE）。

#### 浏览器内核

**支撑浏览器运行的最核心的程序**

不同浏览器可能内核不同：https://zhuanlan.zhihu.com/p/99777087

内核有很多模块组成

* **主线程**
  * **js引擎模块**：负责js程序的编译和运行
  * html/css 文档解析模块： 负责页面文本的解析
  * DOM/CSS模块：负责dom/css在内存中的相关处理
  * 布局和渲染模块：负责页面的布局和效果的绘制（根据内存中的对象）
* **分线程**
  * 定时器模块：负责定时器的<u>管理</u>
  * DOM事件响应模块：负责事件的<u>管理</u>
  * 网络请求模块：负责ajax请求

#### 定时器引发的思考

1. 定时器真的是定时执行吗？

   定时器并不能保证真的定时执行！

   一般会延迟一点点（可以接受）；

   但是也有可能会延迟很长时间（不能接受）！ --> 比如在定时器设定后，做了一个很长时间的任务

2. 定时器的回调函数是在分线程执行的吗？

   是在主线程执行的。JS是单线程的，所有js程序都是在主线程执行的。

3. 定时器是如何实现的？

   事件循环模型

#### JS是单线程执行的

1. 如何证明js是单线程执行的？

   setTimeout的回调函数是在主线程执行的。

   定时器回调函数只有在运行栈中的代码全部执行完之后才有可能执行。

   ```javascript
   setTimeout(function() {
     alert('0000000')
   }, 0); // 不是立即执行的
   
   console.log('alert开始前');
   alert('------')
   console.log('alert开始后');
   ```

2. 为什么js要用单线程模式？

   与用途有关。

   js作为浏览器脚本语言，主要用途是与用户互动以及操作DOM

   这决定了它只能是单线程，否则会带来很复杂的同步问题（比如说多个线程一起操作同一个DOM node）

3. 代码的分类：

   1. 初始化代码
   2. 回调代码

4. js引擎执行代码的基本流程

   1. 执行初始化代码：包含一些特别的代码
      * 设置定时器
      * 绑定事件监听
      * 发送ajax请求
   2. 后面在某个时刻才会执行回调函数。 --> 回调函数是**异步**执行的。

#### 浏览器的事件循环模型

1. 代码分类：
   * 初始化执行代码（同步代码），包含设置定时器，绑定dom事件监听，发送ajax请求等
   * 回调执行函数（异步代码）：处理回调逻辑
2. 模型的两个重要组成部分：
   * **事件管理模块**
   * **回调队列**
3. 模型的运转流程：
   1. 执行初始化代码，将事件回调函数交给对应的<u>管理模块（这些模块在分线程上运行）</u>管理。
   2. 当时间发生时，管理模块会将回调函数及其数据添加到回调队列中
   3. 只有当初始化代码执行完之后，才会遍历读取回调队列中的回调函数执行

事件循环模型见下图：

![Screen Shot 2021-10-10 at 1.45.14 AM](/Users/hualinfeng/Library/Application Support/typora-user-images/Screen Shot 2021-10-10 at 1.45.14 AM.png)

相关概念：

1. 执行栈（execution stack），所有的代码都在此空间中执行
2. 浏览器内核（注意主线程和分线程的模块）
3. 任务队列(task queue)，消息队列(message queue)，事件队列(event queue) --> 指的都是callback queue回调队列，存放的都是待执行的回调函数
4. <u>**事件轮询(event loop)**</u>：从callback queue里循环取出回调函数放入执行栈中处理（一个接一个）
5. 事件驱动模型：event-driven interaction model
6. 请求响应模型：request-response model

#### H5 Web worker多线程

##### 介绍

Web Workers 是HTML5提供的一个JavaScript多线程解决方案。

我们可以将一些大计算量的代码交给web worker运行而不冻结用户界面。 （比如利用递归去解决斐波那契数列的计算）

但是！**子线程完全受主线程控制，且不得操作DOM**，所以这个新标准并**没有改变JavaScript单线程的本质**。

##### 使用

其实我觉得有点像自己去写一个类似处理定时器管理的模块。

onmessage， postMessage， terminate

1. 创建在分线程执行的js文件

   ```javascript
   // worker.js
   function fibo(number) {
     return number <= 2 ? 1 : fibo(number - 1) + fibo(number - 2);
   }
   
   var onmessage = function(event) { // 不能用函数声明，因为其实这个onmessage是赋给全局上下文的
     var number = event.data; // 通过event.data得到发来的数据
     
     postMessage(fibo(number)); // 将计算好的数据发送回主线程，该方法来自于全局上下文this: DedicatedWorkerGlobalScope
   }
   // worker的全局上下文this不再是window也无法访问到document，所以无法对DOM node进行操作
   // 能用console因为console不是window的，而且每个浏览器实现的
   // worker文件中的上下文this： https://developer.mozilla.org/en-US/docs/Web/API/DedicatedWorkerGlobalScope
   ```

   

2. 在主线程的js中发消息并设置回调

   ```javascript
   btn.onclick = function() {
     var number = input.value;
     // 创建worker对象
     var worker = new Worker('worker.js');
     // 绑定接收消息的监听（分线程发回的消息）
     worker.onmessage = function(event) {
       alert(event.data);
     }
     // 向分线程发送消息
     worker.postMessage(number);
   }
   ```

##### 图解

![Screen Shot 2021-10-11 at 12.16.05 AM](/Users/hualinfeng/Library/Application Support/typora-user-images/Screen Shot 2021-10-11 at 12.16.05 AM.png)

##### 不足（不确定现在还是不是不足了）

1. 相对较慢，因为有线程切换开销等
2. 不能跨域加载JS
3. worker内代码不能访问DOM（不能更新UI）
4. 不是每个浏览器都支持这个新特性























































