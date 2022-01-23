## JavaScript精粹

### 基本语法补充

#### comments

两种。一种是 `//` 另一种是 `/*  */`  line-ending 和 block comments

obsolete comments（过时的comments）还不如不写comments

block comments 在从comment out代码的时候，不一定是安全的，比如正则表达式也有 */ 这种。所以推荐用 `//`

#### 命名

不能是保留单词，比如 boolean这种

letter， digit，_

#### Numbers

64-bit floating point，same as Java's double

没有单独的integer类型

可以表示整数、小数、指数 `1e2`

`NaN` is a number value --> result of 不能计算出正常结果的操作

`NaN` 不等于任何值，包括自己。 可以用`isNaN()` 去检查是不是

`Infinity` 表示所有大于1.79769313486231570e+308的数

有专门用来处理数字的内置方法。存放在`Math` 对象下，比如`Math.floor(3.4)`

#### Strings

单引号或者双引号均可

Backslash(`\`) is the escape character. 逃逸（转义）字符

所有字符都是16bits wide （因为写js的时候，Unicode是16-bit character set）

JS没有char这种类型

转义字符

* 可以把一些不允许的字符插入字符串  或者将一些字符用数字的形式表现出来
  * （比如", ', \\, /, ）
  * \b 空格
  * \f  formfeed 跳页
  * \n 换行
  * \r  这是一个control字符（browser会忽略control字符，所以在浏览器上看不出效果）--> 把光标移到该行最开始的地方
  * \t  tab
  * \u + 4位十六进制数字

Immutable! 只会新建字符串，不会改变字符串

拥有相同字符且相同字符顺序的两个字符串是相同的， 可以用 `===`

字符串也有一些内置的方法，比如`'cat'.toUpperCase()`

#### Statements 语句

The `switch, while, for, and do` statements are allowed to have an optional `label` prefix that interacts with the break statement.

return语句如果没有返回值，则返回值是undefined

> expression
>
> disruptive:  throw({name, message}), break, return
>
> try
>
> if
>
> switch
>
> while
>
> for
>
> do

Falsy value: false, null, undefined, '', 0, NaN

For循环：

* For(initilization; exit condition; increment/decrement) {code block}
* For(myvar in obj)  --> usually necessary to test `obj.hasOwnProperty(myvar)` 确定不是来自于原型链

#### Expressions 表达式

什么样的是语句，什么样的是表达式？

? Ternary operator

操作符优先级

* Refinement and invocation  --> . [] ()
* unary operators --> delete new typeof + - !
* multiplication, division, modulo   * / %
* Addtion, subtraction + -
* ineuality >= <= > <
* equality === !===
* and &&
* or ||
* ternary ?:

`typeof` 可能的返回值： number, string, boolean, undefined, function, object.

值得注意的是： `typeof []` 和 `typeof null` 都返回`object`

`!`用于取反

`+` 用于 加法 或者 拼接(concatenates)  如果是加法的话，需要确保操作值都是numbers

`/`  --> 与java中不同，因为没有int等数据类型

`&&` 如果左边的值是falsy的，则返回左边的值；否则返回右边的值

`||` 如果左边的值是truthy的，则返回左边的值；否则返回右边的值

`Invocation` 引发函数的执行  --> 函数后面的圆括号 -->  (params)

`refinements` 对象或数组的存取。用于指定一个对象或者数组的属性或者元素。

#### Literals 字面量

number

string

object

array

function

Regex (regular expressions)

#### Functions

### 第三章 Objects

除了number，string，boolean，null， undefined，其他都是objects

object能比较容易用来表示树或者图的结构。

JavaScript includes a **prototype** linkage feature that allows one object to inherit the properties of another. When used well, this can reduce object initialization time and memory consumption.

属性的名字可以是任何的string，包括空string

当属性名字是**合法的js name**并且不是保留词，引号可以省去。（比如`first-name` 需要引号但是 `first_name` 可以不需要引号）

#### 检索

可以用[string expression] 去检索，比如 `a["b"]`

当这个string expression是一个常量，并且是合法的js name，并且不是保留字时，可以使用 `.` 去检索，比如 `a.b`

如果检索的属性不存在，会返回undefined；如果试图去检索一个undefined，会throw TypeError

* The || operator can be used to fill in default values
* && operator can be used to guard from TypeError

#### 更新

同样可以用 `[]` 或者 `.` 的方式去赋值。（替换或者添加）

#### 引用

**Objects are passed by reference. (never copied)**

#### 原型 prototype

每一个对象都连接到一个原型对象，并且从原型对象继承属性。 所有通过对象字面量创建的对象，都连接于js标准的原型对象：`Object.prototype`。

当你创建新对象时，可以选择某个对象作为它的原型。

原型连接在更新时，是不起作用的。也就是说当我们在对一个对象进行更新时，不会触及到该对象的原型。

**原型连接只有在检索(retrieval)值的时候才被用到**。当我们尝试去获取对象的某个属性值时，如果该对象没有此属性名，那么js会试着顺着原型链去原型对象寻找，直到Object.prototype，如果没找到的话，则结果是undefined。这个过程称为**委托**(delegation)。

原型关系是一种动态关系。如果我们添加一个新的属性到原型中，该属性会立即对所有基于该原型创建的对象可见。

#### 反射 reflection

检查对象并确定对象有什么属性是很容易的。

typeof 操作符对确定属性的类型很有帮助。

注意的是，原型链中的任何属性也会产生一个值。

有两种方法处理这些undesirable的属性：

* 让程序去查找并剔除函数值。做反射的目标是数据，因为需要意识到有些值可能是函数。
* 使用`hasOwnPropert`， 判断是否为对象独有的属性，不会检查原型链

#### 枚举 Enumeration

for in语句可以用来遍历对象中的所有属性名，包括函数和在原型中的属性。最常用的过滤器是 hasOwnProperty 和 typeof 的组合来排除。

使用for in语句的遍历，顺序是不确定的。如果需要有正确的顺序，需要创建一个数组去存正确顺序的属性名，然后使用for语句去遍历。

#### 删除 Delete

delete可以用来删除对象的属性，此操作不会触及原型链中的任何对象。删除操作可能会让来自原型链中的属性浮现出来。

`delete obj.property`

#### 减小全局变量污染（es5）

最小化使用全局变量的一个方法是在应用是只创建唯一一个全局变量。把它变成一个应用的容器，其他的全局变量都可以存放在一个名称空间下。（也可以使用闭包来进行信息的隐藏，这是另一种有效减少全局污染的办法）

### 第四章 函数 Functions

函数是js的基础单元模块，用于代码复用、信息隐藏和组合调用。函数用于指定对象的行为。（js里的function功能非常强大！）

#### 函数对象 Function Objects

函数就是对象。函数对象连接到Function.prototype (该原型对象本身连接到Object.prototype)。 每个函数在创建时还有两个附加的隐藏属性：the function's context(函数的上下文) and the code that implements the function's behavior. 

每个函数对象在创建时，也随带有一个prototype的属性。它的值是一个拥有constructor属性且值即为该函数的对象。这和隐藏连接到Function.prototype完全不同。会在继承中揭示。

因为函数是对象，所以它可以向任何其他值一样被使用。（被存放在变量、对象、数组中，可以被当做参数传递，可以被返回。）函数可以拥有方法。

与众不同之处： 它可以被调用 (invoked)

#### 函数字面量 Function literal

保留字function + 函数名（若忽略则被认为是匿名函数） + 参数 + 主体

函数字面量可以出现在任何允许表达式出现的地方。

函数可以被定义在其他函数中。一个内部的函数不仅可以访问自己的参数和变量，也能访问它被嵌套在其中的那个函数的参数和变量。通过函数字面量创造的函数对象包含一个连到外部上下文的连接，这被称为**闭包**。

#### 调用 Invocation

调用一个函数将暂停当前函数的执行，传递控制权和参数给新函数。除了声明时定义的形参，函数还将接收两个附加参数：**this** 和 **arguments**

参数this的值取决于调用的模式(invocation pattern)。在js中，一种有四种调用模式：方法调用、函数调用、构造器调用、apply调用。不同模式在如何初始化关键参数this上存在差异。

调用运算符：产生函数值的任意表达式后面的一对圆括号。（可以没有参数被传递）

每个表达式产生一个参数值；实参和形参个数不匹配时不会报错，多的参数会被忽略，少的参数都被替换成undefined；不会对参数类型进行检查；任何类型的值都可以被传递给参数。

* 方法调用(method)

  当一个函数被保存为一个对象的一个属性时，我们称它为方法。当一个方法被调用时，this被绑定到该对象。（通过对象去调用方法）  This very late binding makes functions that use this highly reusable.

  Methods that get their object context from **this** are called public methods.

* 函数调用(function)

  以此模式调用时，this被绑定到**全局对象**。这是语言设计上的一个错误！

  错误的后果是，不能利用内部函数来帮助对象工作，因为内部函数的this被绑定了错误的值。解决方案：在对象方法中定义一个变量且赋值为this(这个this是正确的this)，然后在内部函数中，使用这个变量访问到this；还有bind，apply等方法

  ```javascript
  myObj.double = function() {
    var that = this;
   	var helper = function() {
      // 因为这个helper函数将会以函数调用模式被调用，所以不能用this去访问myObj
      that.value = 2 * that.value;
    };
    helper(); // 函数调用模式
  };
  
  myObj.double();
  ```

* 构造器调用模式(constructor)

  js是一门基于原型继承的语言。对象可以直接从另外的对象继承属性。该语言是class-free的。js（本身对其原型的本质也不自信）提供了一种和基于类的语言类似的对象构建语法。(worst of both worlds)

  如果在一个函数前加上`new`来调用，那么将创建一个隐藏**连接到该函数的prototype成员**的对象(hidden link to the value of the function’s **prototype** member)。同时`this`将会被绑定到那个新对象上。`new` 前缀也会改变return语句的行为。

  这些设计跟`new`关键词一起被调用的函数被称为 构造器函数。我们约定俗成把构造器函数的变量名**以大写字母开头**。 

  之后会介绍比这种形式更好的替代方法。

  ```javascript
  var Quo = function(status) {
    this.status = status;
  }
  Quo.prototype.getStatus = function() {
    return this.status;
  }
  
  var example = new Quo('hello');
  console.log(example.getStatus()); // hello
  ```

* Apply 调用模式

  因为js是一门functional object-oriented语言，所以functions是可以有method的。

  `apply`可以用来指定this的值来调用函数。

  ```javascript
  var myObj = {
    status: 'xixi'
  }; // 这个对象是没有getStatus方法的，但是我们可以调用别人的方法
  
  Quo.prototype.getStatus.apply(myObj, []);
  // 第一个参数是调用方法时，指定的this内容；第二个参数是传给被调用函数的参数数组
  ```

#### 参数 Arguments

在一个函数被调用时，会自动得到一个`arguments`参数。通过这个参数，可以得到被调用时，传入的所有参数。这使得写一个不指定参数长度的函数是可能的。

```javascript
var sum = function() {
  var i, res = 0
  for (i = 0; i < arguments.length; i++) {
    res =+ arguments[i];
  }
  
  return res;
}
```

这不是一个特别有用的模式。在第六章Arrays，会知道如何给一个数组加一个类似的方法。

因为设计缺陷，arguments不是一个数组，只是一个类数组对象。它有length属性，但不能使用所有array的方法。

#### Return

当一个函数被调用时，它从第一行开始执行，直到遇到结束函数体的花括号。结束调用后，将控制权还给调用该函数的地方。

`return`可以用来提早结束函数的调用。一个函数always会返回一个值，如果没有指定return值，则返回undefined

如果一个函数被加上`new`前缀进行调用，并且返回值不是一个对象，则返回this (创建的新对象)

#### Exceptions

异常处理机制。异常是干扰程序的非正常的事故（但不一定是出乎意料的）。

`throw `语句中断函数的执行。它应该抛出一个exception对象，该对象包含可识别异常类型的name属性和一个描述性的message属性。也可以添加其他的属性。

try-catch语句：如果在try代码块内抛出了异常，控制权会跳转到catch从句。

一个try语句，只会有一个将捕获所有异常的catch代码块。所以可能需要检查异常对象的name属性来确定异常的类型。

#### 给类型增加方法

js允许给语言的基本类型增加方法。比如可以通过给Object.prototype添加方法来使得该方法对所有对象可用。

* 通过给Function.prototype添加如下method方法，可以直接用method增加方法属性，不需要键入prototype

```javascript
// In this book, we use method method to define new methods.
Function.prototype.method = function (name, func) {
  // 只有在确定没有该方法时才能添加
  if (!this.prototype[name]) {
    this.prototype[name] = func;
  	return this;
  }
};

const Car = function(model) {
    this.model = model;
}
Car.method('drive', function() {console.log('sss')});
```

* 给Number增加一个取整的方法

  ```javascript
  Number.method('integer', function() {
    return Math[this < 0 ? 'ceiling' : 'floor'](this);
  })
  ```

* 给String增加一个移除开头和结尾的空白

  ```javascript
  String.method('trim', function() {
    return this.replace(/^\s+|\s+$/g, '');
  })
  ```

通过给基本类型增加方法，我们可以大大提高语言的表现力。因为js原型继承的动态本质，新的方法立刻被赋予到所有的值（对象实例）上，即使对象实例是在方法被创建之前就创建好了。

for in语句用在原型上时表现很糟糕。我们可以使用hasOwnProperty去筛选出继承而来的属性，或者可以查找特定的类型。

#### Recursion 递归

直接或间接调用自身。将问题分解为一组相似的子问题，每一个都用一个寻常解去解决。（调用自身去解决它的子问题）

可以非常高效地操作树结构。比如浏览器端的DOM

```javascript
// Define a walk_the_DOM function that visits every
// node of the tree in HTML source order, starting
// from some given node. It invokes a function,
// passing it each node in turn. walk_the_DOM calls
// itself to process each of the child nodes.
var walk_the_DOM = function walk(node, func) {
  func(node);
  node = node.firstChild;
  while (node) {
    walk(node, func);
    node = node.nextSibling;
  }
};
```

一些语言提供了尾递归优化，如果一个函数返回自身递归调用的结果，那么调用的过程会被替换成一个循环，可以显著提高速度。但是js当前并不支持尾递归的优化。深度递归的函数可能会因为返回堆栈溢出而运行失败。（爆栈）

#### Scope 作用域

该书中讲述的是es5的情况，在es6下，let和const可以解决var定义变量的很多问题。

var是function scope，函数的参数和变量不能在函数体外被访问到，但是在函数体内任意位置定义的变量在该函数中的任何地方可见。--> 最好就是在函数体顶部声明所有在该函数中需要的变量。

block scope：在一个代码块中定义的变量在代码块外是不可见的，定义在代码块中的变量在代码块执行结束后会被释放掉。 --> 推荐尽可能迟地申明变量。

es6的let变量能弥补以下缺陷：

* var定义的变量没有块作用域。
* var定义的全局变量会自动添加全局window对象的属性。
* var定义的变量会提前装载。

举一个例子：遍历array时定义的那个index

#### Closure 闭包

作用域的好处是内部函数能访问定义它们的外部函数的参数和变量（除了this和arguments），这是一个好事。

一个更有趣的情形是内部函数拥有比它外部函数更长的生命周期。

我们之前以对象字面量去创建了一个拥有value和increment方法的对象，但是如果我们想保护value不被非法更改的话，我们需要引入闭包的概念。

```javascript
var myObj = function() {
  var value = 0;
  
  return {
    incre: function(inc) {
      value += typeof inc === 'number' ? inc : 1;
    },
    getValue: function() {
      return value;
    }
  }
}();
// myObj 能得到两个方法，但是由于函数的作用域原因，外部是无法直接访问value的
// myObj的两个方法依然继续享有访问value变量的权利
```

另一种构造函数的例子：

```javascript
var quo = function(status) {
  return {
    get_status: function() {
      return status;
    }
  };
};

var myQuo = quo('hello');
console.log(myQuo.get_status());

// 即使quo已经返回了，但是get_status依然能访问status，并且是该参数本身而不是拷贝。
```

**函数可以访问它*被创建时*所处的上下文环境，这被称为闭包。**

对比下面错误和正确的例子，理解内部函数能访问外部函数的实际变量而无需复制：

```javascript
// 错误示范
var add_handlers = function(notes) {
  var i;
  for (i = 0; i < nodes.length; i++) {
    nodes[i].onclick = function(e) {
      alert(i);
    };
  }
  // 最后每个对话框的数字都是节点个数，因为handler被创建的时候，访问的都是i这个实参，最终i=nodes.length
};

// 正确示范
var add_handlers = function(notes) {
  var i;
  for (i = 0; i < nodes.length; i++) {
    nodes[i].onclick = function(x) {
      // x 不是 i本身，是一个拷贝，所以不存在上面那个问题
      return function(e) {
        alert(x);
      }
    }(i)
  }
};
// That function will return an event handler function that is bound to the value of i that was passed in, not to the i defined in add_handlers.
```

#### Callbacks 回调

函数可以让不连续事件的处理变得更容易。

比如在网络请求时，需要异步请求而避免客户端的堵塞，这个时候需要提供一个当服务器响应达到时被调用的回调函数。

#### Module 模块

我们可以使用函数和闭包来构造模块。模块是一个提供接口却隐藏状态和实现的函数或对象。通过使用函数去产生模块，几乎可以完全摒弃全局变量的使用。

```javascript
var helper = function() {
  var obj = {
    quot: '"',
    lt: '<',
    'gt': '>'
  };
  
  return function() {
    // 此处写具体函数的实现
  };
}();
// 注意这里是直接执行，以返回我们真正需要的那个函数。
// 既避免了全局变量的使用（闭包的价值），又不需要在每次执行函数的时候去创建字面量对象。
```

模块模式（module pattern）利用了函数作用域闭包来建立那些绑定和私有的关系。(create relations that are binding and private)

一般形式：一个定义了私有变量和函数的函数；利用闭包创建可以访问私有变量和函数的特权函数；最后返回这个特权函数或者把它们保存到一个accessible的地方。

Use of the module pattern can eliminate the use of global variables. It promotes information hiding and other good design practices. It is very effective in encapsulat- ing applications and other singletons.

模块模式还能用来创建安全的对象。

```javascript
var serial_maker = function() {
  var prefix = '';
  var seq = 0;
  
  return {
    set_prefix: function(p) {
      prefix = String(p);
    },
    set_seq: function(s) {
      seq = s;
    },
    gensym: function() {
      var result = prefix + seq;
      seq += 1;
      return result;
    }
  }
};

var seger = serial_maker();
// 这个函数没有使用this或者that，所以there is no way to compromise the seqer对象
// 只有通过setter才能去改变私有变量
// 如果把seger.gensym 当做参数传给第三方函数，那个函数也能用它产生期待的结果，但依然不能修改prefix或seq
```

#### 级联 Cascade

一些设置或修改对象某个状态值的函数一般没有返回值。如果我们把这种没有返回值的方法，**返回this**，就可以启用级联。级联可以产生出具备**很强表现力的接口**。它也能帮忙控制那种构造试图一次做太多事的接口的趋势（而是分成多个简单的接口，利用级联来一个个调用）。

Promise

getElement 

#### 套用 Curry

curry方法通过创建一个保存着原始函数和被套用的参数的闭包来工作。它返回另一个函数，该函数被调用时，会返回调用原始函数的结果，并传递调用curry时的参数加上当前调用的参数的所有参数。

```javascript
// 这里用了之前写过的给Function原型增加的method方法
Function.method('curry', function() {
  var slice = Array.prototype.slice;
  // 注意arguments是一个类数组对象，需要用slice转化成数组才能concat
  var args = slice.apply(arguments), that = this;
  return function() {
    return that.apply(null, args.concat(slice.apply(arguments)));
  }
})
```

#### 记忆 Memoization

函数可以用对象去记住先前操作的结果，从而避免无谓的运算。这种优化成为记忆。js的对象和数组都能用来实现这种优化。

例子：斐波那切数列计算

```javascript
// 一般化后的形式，memo储存数据，fundamental是计算方法
var memoizer = function(memo, fundamental) {
  var shell = function(n) {
    var result = memo[n];
    if (typeof result !== 'number') {
      result = fundamental(shell, n);
      memo[n] = result;
    }
    return result;
  }
  return shell;
}

var fibonacci = memoizer([0, 1], function(shell, n) {
  return shell(n-1) + shell(n-2);
});

var factorial = memoizer([1, 1], function(shell, n) {
  return n * shell(n-1);
});
```

### 第五章 继承 Inheritance （先跳过）

js是一门弱类型语言，从不需要类型转换。对象的起源是无关紧要的。对于一个对象来说，重要的是它能做什么，而不是它从哪里来的。

js有很多继承模式，它能模拟那些基于类的模式，也支持其他更具表现力的模式。

在基于类的语言中，对象是类的实例，并且类可以从另一个类继承。js是一门基于原型的语言，这意味着**对象直接从其他对象继承**。

#### 伪类 pseudoclassical

### 第六章 数组 Arrays

数组是一段线性分配的内存，它通过整数去计算偏移并访问其中的元素。数组可以是很快的数据结构。

**但是！**js没有像这种数组一样的数据结构。js提供了一种**拥有一些类数组特性的对象**。它把数组的下标转成字符串，用其作为属性。它明显比一个真正的数组慢，但也可以更方便地使用。它的检索和更新方式与对象一样，除了有一个可以用整数作为属性名的特性外。数组有它们自己的字面量格式，也有一套非常有用的内置方法。

数组继承自Array.Prototype 所以继承了大量的有用的方法。还有一个length属性。并且允许数组包含任意混合类型的值。

#### length

* 没有上界（下标必须是0 - 2^32 - 1的整数）
* 即使用大于当前length的数字作为下标去保存一个元素，length将增大来容纳新元素。并不会发生数组边界错误。
* 它是最大整数属性名加1，并不一定等于数组里的元素个数
* [] 后缀下标运算符 将它的表达式转换成一个字符串，如果该表达式有toString方法，就使用该方法产生的值
* 可以直接设置length的值。设置大了无须分配更多空间；设置小了，会把下标大于等于新length的属性删除
* a[a.length] = '' 和 a.push('') 做的事情一样

#### 删除 delete

由于js的数组其实就是对象，所以可以直接用delete来删除元素。

`delete numbers[2]` 

但是该方法会在下标为2的地方留下一个洞，此时的numbers[2]会返回undefined，但是通常我们希望后面的元素能往前移，填补这个洞

**splice**

js为我们提供了一个splice方法，可以对数组进行改造（根据传入的参数，可以删除元素，删除并替换成其他元素，增加元素等）这是一个可以灵活使用的方法。

因为被删除属性后面的每个属性都需要被移除，并且以一个新的key重新插入，所以对于大型数组来说可能效率不高。

语法： `array.splice(start[, deleteCount[, item1[, item2[, ...]]]])`

* start指定修改开始的位置。如果超出数组长度，则从末尾开始添加内容；如果是负数，则从数组末位开始往前数 `-n = array.length - n`
* deleteCount 可选，表示要移除数组元素的个数。如果大于start后元素的总数，则移除start之后所有元素；如果省略，则移除start之后所有元素；如果是0或者负数，则不移除元素，这种情况下至少应该添加一个新元素。
* item1, item2...  可选，表示从start位置开始，要添加数组的元素。如果不指定，则splice只删除数组元素。

#### 枚举 enumeration

因为js数组其实就是对象，所以可以用for in语句来遍历一个数组的所有属性。但是for in语句不能保证属性的顺序，而且可能存在从原型链中得到意外属性的问题。

所幸的是，常规的for循环避免了这个问题。

#### 容易混淆的（以下陈述都是正确的）

当属性名是小而连续的整数时，应该使用数组；否则应该使用对象。

typeof的返回值都是object

js在区分数组和对象上没有一个好的机制，所以我们需要定义自己的is_array函数（es6中已经有了isArray方法）

方法一：

```javascript
function is_array(value) {
  return value && typeof value === 'object' && value.constructor === Array;
}
```

方法二： （方法一在识别从不同的窗口window或frame里构造的数组时会失败）所以想要准确地检测那些外部的数组，我们需要方法二这种更多的检测。

```javascript
function is_array(value) {
  return value && typeof value === 'object' &&
    typeof value.length === 'number' &&
    typeof value.splice === 'function' &&
    !(value.propertyIsEnumerable('length')); //Object.prototype.propertyIsEnumerable()
}
```

#### 方法 methods

js给数组提供了一套内置的方法。同时，也可以通过Array.prototype对方法进行扩充。

因为数组其实就是对象，所以我们可以直接给一个单独的数组添加方法。并且因为添加的方法，属性不是整数，所以也不会改变length属性。

#### 维度 dimension

js的数组通常不会初始化。

我们可以准备Array.dim方法来帮忙实现初始化。

```javascript
Array.dim = function(dimension, initial) {
  const res = [];
  
  for (const i = 0; i < dimension, i++) {
    res[i] = initial;
  }
  
  return res;
}

const myArray = Array.dim(10, 0);
```

js没有多维数组，但是支持数组元素是数组，所以第二维的数组需要自己创建。

```javascript
const matrix = [
  [1,2,3],
  [4,5,6],
  [7,8,9]
];
```

同理对于二维数组，我们也可以写一个方法去初始化。

Array.matrix = function(x,y,initial) {xxx}

Array.identity = function(n,initial) {xxx}

### 第七章 正则表达式 regular expression

正则表达式是一门简单语言的语法规范。它以方法的形式被用于对字符串中的信息进行查找、替换和提取操作。可处理正则表达式的方法有`regexp.exec`, `regexp.test`, `string.match`, `string.replace`, `string.search` 和 `string.split`

通常来说，在js中正则表达式相较于等效的字符串运算有显著的性能优势。js的正则表达式难以分段阅读，因为它们不支持注释和空白。所以阅读和理解以及维护有些困难。

js程序中，正则表达式必须写在一行中，空白是至关重要的。

#### 说明一个例子

`var parse_url = /^(?:([A-Za-z]+):)?(\/{0,3})([0-9.\-A-Za-z]+)(?::(\d+))?(?:\/([^?#]*))?(?:\?([^#]*))?(?:#(.*))?$/;`

```javascript
var url = "http://www.ora.com:80/goodparts?q#fragment";
var result = parse_url.exec(url);
// result[0] 为 The full string of characters matched
// result[1], result[2]...   The parenthesized substring matches，就是下面说的获取型分组
// 获取型分组如果在一个可选的分组中，并且该分组没有被匹配到的话，保存为undefined
var names = ['url', 'scheme', 'slash', 'host', 'port',
             'path', 'query', 'hash'];
var blanks = '       '; // 为了好看
var i;
for (i = 0; i < names.length; i += 1) {
  document.writeln(names[i] + ':' + blanks.substring(names[i].length), result[i]);
}

url:    http://www.ora.com:80/goodparts?q#fragment
scheme: http
slash:  //
host:   www.ora.com
port:   80
path:   goodparts
query:  q
hash:   fragment
```

* `^` 指示一个字符串的开始， 在例子里为了防止exec跳过不像URL的前缀：

* `(?:([A-Za-z]+):)?` 

  该因子匹配一个协议名，但仅当它之后跟随一个`:` 时才匹配。

  `(?:........) `表示一个非捕获型分组(noncapturing group)，即使匹配也不会存入result数组中。

  后缀的`?` 表示这个分组是可选的。它表示重复0或1次。

  `([A-Za-z]+)`

  `(.......)` 表示一个捕获型分组(capturing group)。一个捕获型分组将复制它所匹配的文本，并将其放入result数组中。第n个捕获型分组所匹配的文本拷贝将出现在result[n]中。

  `[.....] ` 表示一个字符类(character class)。 `A-Za-z` 表示包含26个大小写字母的字符类，连字符hyphen表示范围。 后缀的`+`表示这个字符类将被匹配1次或多次。

* `(\/{0,3})`

  `\/` 用于匹配 `/`， 这里用反斜杠进行转义，就不会被错误解释为该正则表达式的结束符。 后缀 `{0, 3}` 表示将会被匹配0-3次。

* `([0-9.\-A-Za-z]+)`

  它将用于匹配一个主机名 host name，由一个或多个 数字、字母、`.` 或者 `-` 组成。 `\-` 表示匹配`-`， 避免与表示范围的连字符混淆

* `(?::(\d+))?`

  该可选因子匹配一个端口号，它是一个由 `:` 和一个以上的数组组成的序列。 `\d` 表示一个数字字符。

* `(?:\/([^?#]*))?`

  该可选因子匹配路径。该分组以一个 `/` 开始，之后的字符类 `[^?#]` ，开头的`^` 是指这个字符类包含除了`?` 和 `#` 的其它字符。后缀的 `*` 表示这个字符类将被匹配0次或以上。

* `(?:\?([^#]*))?`

  用于匹配参数。 该分组以 `?` 开始，后面跟着0个以上的非`#` 字符。

* `(?:#(.*))?`

  用于匹配hash值。该分组以 `#` 开始，后面跟着0个以上的任意字符（除了结束符）。`.` 表示任意字符

* 末尾的`$`

  结束符。表示这个字符串的结束。确信这个字符串尾部没有其他更多的内容。

js语言处理器之间有非常高度的兼容性。js中least portable(可移植的)的部分就是正则表达式的实现。结构复杂的正则表达式很有可能导致移植性问题。嵌套的正则表达式也能导致极恶劣的性能问题。所以简单就是最好的策略。

#### 另一个例子

匹配数字的正则表达式

数字可能由一个整数部分，加上一个可选的减号、一个可选的小数部分和一个可选的指数部分组成：

` var parse_number = /^-?\d+(?:\.\d*)?(?:e[+\-]?\d+)?$/i;`

解析：

我们又使用了 `/^$/` 的结构来定义了一个正则表达式。它将导致字符串中的所有字符都要针对这个正则表达式进行匹配。

如果我们忽略了 `^`  和 `$` ，那么只要字符串中有一个数字，就可以匹配。

都不忽略时，需要整个字符串的内容仅为一个数字时，才能匹配。

如果只包含 `^` 则需要字符串以一个数字开头才能匹配。

如果只包含 `$`  则需要字符串以一个数字结尾才能匹配。

i 标识： 规定匹配字母时忽略大小写。-->  其实也可以把e因子的判断写成 `[Ee]` 或`(?:E|e)`， 但是还是以简单为原则

`-?`  表示 减号可选（不需要转义符因为不在字符类里，不会产生歧义）

`\d+` 表示匹配一个以上的数字。 同 `[0-9]+`

`(?:\.\d*)?` ： 匹配小数部分（可选的非捕获型分组）。以`.` 为开始，跟着0个以上的数字。（需要对`.`进行转义，以防对任意字符的混淆）

`(?:e[+\-]?\d+)?` ： 匹配指数部分。将匹配 e或E，以及一个可选的`+`或者`-`号，以及一个以上的数字。

####  结构 construction

有三种方式创建一个RegExp对象：

* 正则表达式字面量

  包围在一对斜杠中。

  有三个标志能设置，分别是i, g, m

  i: Insensitive (ignore character case)

  g: Global (match multiple times; the precise meaning of this varies with the method)

  m: Multiline (^ and $ can match line-ending characters)

  `var my_regexp = /"(?:\\.|[^\\\"])*"/g;`

  正则表达式为常量时，使用字面量形式。（比如有一个正则表达式在循环里使用时，不会每次迭代都重新编译正则表达式）

  **用字面量创建的RegExp对象，共享同一个单实例：**

  ```javascript
  function test() {
    return /a/gi;
  }
  
  var a = test();
  var b = test();
  // a 和 b 是同一个对象
  ```

* RegExp 构造器

  这个构造器接受一个字符串，并把它编译为一个RegExp对象。

  创建这个字符串时需要小心，因为反斜杠在正则表达式和字符串字面量中有一些不同的含义。

  When using the constructor function, the normal string escape rules (**preceding special characters with `\` when included in a *string***) are necessary.  在字符串字面量里，要对特殊字符进行转义。比如下面的引号如果不转义的话，就直接跟开始的引号匹配了。

  通常需要双写反斜杠以及对引号进行转义。

  `var my_regexp = new RegExp("\"(?:\\.|[^\\\\\\\"])*\"", 'g');`

  构造器创建模式适用于正则表达式必须在runtime动态产生的情形。可能正则表达式模式会变，或者可能需要从其他资源获取（比如用户输入）

* 还是RegExp构造器，但是第一个参数为正则表达式字面量。 （ES6开始有的）

  `let re = new RegExp(/ab+c/, 'i')`

RegExp对象的属性：

global： 标志g是否被使用

ignoreCase： 标志i是否被使用

lastIndex： 下一次exec匹配开始的索引，初始值为0

multiline： 表示m是否被使用

source： 正则表达式源代码文本

#### 元素 Elements

* 正则表达式选择 regexp choice

  包含一个或多个正则表达式序列，这些序列被 `|` 隔开。只要任意一个序列符合匹配条件，那这个选择就被匹配。

* 正则表达式序列 regexp sequence

  包含一个或多个正则表达式因子。每个因子能选择是否跟随一个量词，决定这个因子被允许出现的次数。默认匹配一次。

* 正则表达式因子 regexp factor

  可以是一个字符、一个由圆括号包围的组、一个字符类或者是一个转义序列。

  除了控制字符和特殊字符以外，所有字符都被按照字面处理。

  > /  \  [  ]  (  )  {  }  ?  +  *  |  .  ^  $
  >
  > 如果希望这些字符也能按字面去匹配，必须要用\前缀进行转义。
  >
  > 当你存在疑惑时，可以给任何特殊字符都添加一个\ 来使其字面化。
  >
  > \ 不能使字母或数字字面化

  一个未被转义的 `.` 将匹配除了行结束符以外的任何字符。

  当lastIndex属性值为0时，一个未转义的`^` 将匹配该文本的开始。当指定了m标识时，它也能匹配行结束符。`\n`

  一个未转义的 `$` 将匹配该文本的结束。当指定了m标识时，它也能匹配行结束符。`\n`

* 正则表达式转义

  与在字符串中一样， `\f` 是换页符，`\n`是换行符，`\r` 是回车符，`\t` 是tab符， `u` 允许指定一个Unicode字符来表示一个十六进制的常量。

  但在正则表达式因子中， `\b`  不是backspace

  \大写字母  是  \小写字母的 取反

  `\d` 等同于 `[0-9]` 匹配一个数字        `\D` 与其相反，表示 `[^0-9]`

  `\s`  是Unicode空白符的partial set     --> 等同于 `[\f\n\r\t\u000B\u0020\u00A0\u2028\u2029]`

  `\w` 等同于 `[0-9A-Z_a-z]` 本应该是表示出现在单词中的字符。但对于真正的语言来说，需要制定自己的类。一个简单的字母类是 `A-Za-z\u00C0-\u1FFF\u2800-\uFFFD`。 它包含所有的Unicode字母，也包括成千上万的非字母的字符。

  `\b` 是一个字边界(word-boundary)标志。但由于它是使用 `\w` 去寻找边界，所以对于多语言应用来说不是很有用。

  `\1` 是指向分组1所捕获到的文本的一个引用，所以它能被再次匹配。

  比如：寻找重复出现的单词。

  ```javascript
  var doubled_words =
           /[A-Za-z\u00C0-\u1FFF\u2800-\uFFFD'\-]+\s+\1/gi;
  ```

* 正则表达式分组

  * 捕获型 capturing

    一个被包围在圆括号(parentheses)中的正则表达式选择。任何匹配这个分组的字符将被捕获。每个捕获型分组都被制定了一个数字，第一个被捕获的是分组1，以此类推。

  * 非捕获型分组

    有一个 `(?:` 前缀。仅做简单的匹配，不会捕获匹配文本，有微弱的性能优势。非捕获型分组不会干扰捕获型分组的编号。

  * positive lookahead和negative lookahead都不是好的特性。

* 正则表达式类

  是一种指定一组字符的便捷方式。

  比如匹配一个元音字母，我们可以写 `(?:a|e|i|o|u)`， 但也可以简写成一个类 `[aeiou]`

  两个比较方便的地方：

  * 能指定字符范围

    比如一个表示32个ASCII的特殊字符：

    ```javascript
    (?:!|"|#|\$|%|&|'|\(|\)|\*|\+|,|-|\.|\/|:|;|<|=|>|@|\[|\\|]|\^|_|` |\{|\||\}|~)
    ```

    可以简写成：

    ```javascript
    [!-\/:-@\[-`{-~]
    ```

  * 类的求反

    只需在`[` 后紧跟一个`^` ，就表示这个类将排除后面这些字符。

* 正则表达式类转义

  字符类内部的转义规则和正则表达式因子稍有不同。在字符类内部：

  `[\b]` 是退格符

  `-  /  [  \  ]  ^ ` 需要被转义。

* 正则表达式量词

  正则表达式因子可以用一个正则表达式量词来决定这个因子应该被匹配的次数。

  包围在一对花括号中的数字就是这个量词

  `/www/` 相等于 `/w{3}/`

  `{3, 6}` --> 3 times to 6 times

  `{3, }` --> 3 times or more

  `?`  --> `{0, 1}`

  `*`  --> `{0, }`

  `+` --> `{1, }`

  如果只有一个量词，则趋向于贪婪性匹配，即匹配尽可能多的重复直至达到上线；如果这个量词后面还有一个额外的 `?` 则趋向于懒惰式匹配，即试图匹配尽可能少的必要重复。一般情况最好坚持使用贪婪性匹配。greedy matching and lazy matching

### 第八章 方法 methods

#### Array

* array.concat(item...)

  返回一个新数组，不影响原数组。

  参数可以是一个或多个，如果参数是一个数组，则**每个元素**都会被分别添加

  ```javascript
  const a = [1,2,3];
  const b = a.concat([4,5,6], 7, 8);
  ```

* array.join(seperator)

  把一个array构造成一个字符串，并用seperator位分隔符连接起来。 seperator默认为`,`

  用该方法连接大量片段，通常比直接用 `+` 要快

* array.pop()

  pop和push可以使array像stack一样工作。pop方法移除array的最后一个元素并返回该元素。 如果array为空，则返回undefined

  pop操作可以这样实现：

  ```javascript
  Array.prototype.pop = function() {
    return this.splice(this.length - 1, 1)[0];
  };

* array.push(item...)

  将一个或多个参数添加到array的尾部，会修改原数组。

  如果item是一个数组，则会将该数组**作为单个元素**整个添加到array中。

  push的实现：

  ```javascript
  Array.prototype.push = function() {
    this.splice.apply(
    	this,
      [this.length, 0].concat(Array.prototype.slice.apply(arguments))
    );
    return this.length;
  };
  // 也不一定需要使用apply
  ```

* array.reverse()

  反转array的元素顺序，会修改原数组。

* array.shift()

  移除array的第一个元素并返回该元素。如果array是空数组，则返回undefined。通常shift比pop慢得多。

  shift的实现：

  ```javascript
  Array.prototype.pop = function() {
    return this.splice(0, 1)[0];
  };

* array.unshift(item...)

  将一个或多个参数添加到array的头部，会修改原数组。

  如果item是一个数组，则会将该数组**作为单个元素**整个添加到array中。

  实现：

  ```javascript
  Array.prototype.unshift = function() {
    this.splice.apply(
    	this,
      [0, 0].concat(Array.prototype.slice.apply(arguments))
    );
    return this.length;
  };
  // 也不一定需要使用apply
  ```

* array.slice(start, end)

  将复制array的一段，start是开始复制的索引，end可选，默认为array.length

  范围是 [start, end)

  如果参数是负数，那实际上是array.length与其相加的结果。（试图成为非负数）

* array.sort(sortFunction)

  对array的内容进行排序

  它不能正确地给一组数字排序。因为js的默认比较函数假定所有要被排序的元素是字符串。

  所以我们需要自己定义比较函数来替代默认的比较函数。比较函数应该接收两个参数，如果这两个参数相等，则返回0；如果第一个参数应该排在前面，则返回一个负数；如果第二个参数应该排在前面，则返回一个正数。（比较的内容是自定义的，主要看返回值是0，正数还是负数，负数靠前，正数靠后）

  我们也可以对简单值进行排序（非对象数组），也可以对对象进行排序。

  sort方法是不稳定的。（排序算法的稳定性是指排序后，数组中的相等值的相对位置没有发生改变）

  所以我们有时候需要基于多个键值(key)进行排序，比如当名字排序一样时，比较年龄，如果都一样再考虑比较成绩等等。

* array.splice(start, deleteCount, item....)

  这个函数功能较为强大也比较灵活。可以用于对数组的改造。

  start是改造（移除元素）的开始位置，deleteCount 是需要移除元素的个数， 如果有额外的参数，都将直接被插入到移除元素的位置上。它返回一个包含被移除的元素的数组。

  最主要的用处就是从一个数组中删除元素。

  deleteCount为0时，考虑成为数组添加元素。

  实现函数略复杂，需要对start后的数组元素进行重新索引。

  ```javascript
  Array.prototype.splice = function(start, deleteCount) {
     var max = Math.max,
         min = Math.min,
         delta,
         element,
         insertCount = max(arguments.length - 2, 0),
         k = 0,
         len = this.length,
         new_len,
         result = [],
         shift_count;
    		 start = start || 0;
         if (start < 0) {
           start += len;
         }
        start = max(min(start, len), 0);
        deleteCount = max(min(typeof deleteCount === 'number' ?
                              deleteCount : len, len - start), 0);
        delta = insertCount - deleteCount;
        new_len = len + delta;
        while (k < deleteCount) {
          element = this[start + k];
          if (element !== undefined) {
            result[k] = element;
          }
          k += 1; }
        shift_count = len - start - deleteCount;
        if (delta < 0) {
          k = start + insertCount;
          while (shift_count) {
            this[k] = this[k - delta];
            k += 1;
            shift_count -= 1;
          }
          this.length = new_len;
        } else if (delta > 0) {
          k = 1;
          while (shift_count) {
            this[new_len - k] = this[len - k];
            k += 1;
            shift_count -= 1;
          } }
        for (k = 0; k < insertCount; k += 1) {
          this[start + k] = arguments[k + 2];
        }
        return result;
  }
  ```

#### Function

* function.apply(thisArg, argArray)

  apply 方法调用function ，传递一个将被绑定到this上的对象，和一个可选的参数数组。

#### Number

* number.toExponential(fractionDigits)

  将这个number转换成一个指数形式的字符串。fractionDigits控制小数点后的位数，值必须在0-20之间。

* number.toFixed(fractionDigits)

  将这个number转换成一个十进制数形式的字符串。fractionDigits控制小数点后的位数，值必须在0-20之间。 默认为0

* number.toPrecision(precision)

  将这个number转换成一个十进制数形式的字符串。precision控制有效数字的位数。值必须在0-21之间

* number.toString(radix)

  转换成一个字符串，可选参数radix控制基数。值必须是2-36之间，默认为10. 最常用的是整数，但其实可以使用任意数字。

  最普通的情况下，也可以使用 `String(number)`

#### Object

* object.hasOwnProperty(name)

  如果这个object包含一个名为name的属性，则返回true

  原型链中的同名属性**不会**被检查

  这个方法对name就是hasOwnProperty时不起作用，此时返回false

#### RegExp

* regexp.exec(string)

  exec是使用正则表达式最强大的方法（也是最慢的）

  如果string成功匹配了regexp，会返回一个数组。下标为0的将包含正则表达式匹配的子字符串，其余下标为n的都对应分组n捕获的文本。如果匹配失败，则返回null

  如果带了g标志，就略复杂一些。查找不是从字符串的起始位置开始，是从 regexp.lastIndex位置开始。如果匹配成功，regexp.lastIndex会被设置为该匹配后的第一个字符的位置。不成功的匹配会重置regexp.lastIndex为0

  这就允许通过一个循环中调用exec去查询一个匹配模式在一个字符串中发生了几次。需要注意两件事情：如果你提前退出了这个循环，再次进入这个循环前必须将regexp.lastIndex重置为0；`^` 因子仅仅匹配regexp.lastIndex为0的情况。

* regexp.test(string)

  test是使用正则表达式最简单的方法（也是最快的）

  如果该regexp匹配string，则返回true；否则返回false

  不要对该方法使用g标志

#### String

* string.charAt(pos)

  js没有字符类型，所以返回的是一个字符串。

  pos如果小于0或者大于等于length，返回空字符串。

* string.charCodeAt(pos)

  方法同charAt，但是是以整数形式表示的字符码位。

  pos如果小于0或者大于等于length，返回NaN

* string.concat(string...)

  用来和其他字符串连接在一起构造新的字符串。很少使用因为 + 更方便。

* string.indexOf(searchStirng, position)

  从position位置开始在string中查找字符串searchStirng

  如果找到则返回第一个匹配字符的位置；否则返回-1

  position可选，默认为0

* string.lastIndexOf(searchString, position)

  与indexOf类似，但是是从该字符串的末尾开始查找

* string.localeCompare(that)

  比较两个字符串。如何比较字符串的规则并没有详细的说明。

  如果比that小，则返回负数；相等则返回0

* **string.match(regexp)**

  该方法匹配一个字符串和一个正则表达式。它根据g标识来决定如何进行匹配。

  如果没有g标识，则调用string.match(regexp) 的结果与调用 regexp.exec(string) 的结果相容。

  如果有g标识，那么它返回一个包含除捕获分组之外的所有匹配的数组。

* **string.replace(searchValue, replaceValue)**

  replace方法对string进行查找和替换的操作，并返回一个新的字符串。

  参数searchValue可以是一个字符串或者一个正则表达式对象。

  * 如果searchValue是一个字符串

    那么searchValue只会在**第一次出现**的地方被替换

  * 如果searchValue是一个正则表达式对象

    如果带有g标志，则将替换所有匹配之处；如果没有带g标志，仅替换第一个匹配之处。

  参数replaceValue可以是一个字符串或者一个函数

  * 如果replaceValue是一个字符串

    * 没有字符`$` ，直接替换

    * 字符`$`有特别的含义：

      * `$$`

         `$`

      * `$&`

        整个匹配的文本

      * `$number`

        第number个分组捕获的文本

      * $`

        匹配之前的文本

      * `$'`

        匹配之后的文本

      ```javascript
      var oldareacode = /\((\d{3})\)/g;
      var p = '(555)666-1212'.replace(oldareacode, '$1-'); // $1 表示的是555
      // p is '555-555-1212'
      // 也可以尝试play around其他几种含义
      ```

  * 如果replaceValue是一个函数

    该方法对每个匹配依次调用这个函数，并且该函数返回的字符串将被用作替换文本。

    传递给这个函数的第一个参数是整个被匹配的文本，第二个参数是分组1捕获的文本，以此类推（跟regexp的exec参数相同）

    举例说明：

    ```javascript
    String.prototype.entityify = function() {
      var character = {
        '<' : '&lt;',
        '>' : '&gt;',
        '&' : '&amp;',
        '"' : '&quot;'
      };
      
      // Return the string.entityify method, which
      // returns the result of calling the replace method.
      // Its replaceValue function returns the result of
      // looking a character up in an object. This use of
      // an object usually outperforms switch statements.
      return function() {
        return this.replace(/[<>&"]/g, function (c) {
          return character[c];
        });
      };
    }();
    
    console.log("<&>".entityify()); // &lt;&amp;&gt;
    ```

* string.search(regexp)

  该方法和indexOf类似，只是它接收的是一个正则表达式对象。如果找到匹配的则返回**第一个匹配的首字符**位置；如果没有找到匹配的则返回-1

  此方法忽略`g` 标志，并且没有position参数。

* string.slice(start, end)

  赋值string的一部分来构造一个新的字符串，end参数可选，范围是[start, end)

* string.split(separator, limit)

  该方法把字符串分割成片段来创建一个字符串数组。可选参数limit可以限制被分割片段的数量。

  separator可以是一个<u>字符串或正则表达式</u>

  如果separator是一个空字符串，则返回一个单字符的数组；否则该方法会在string中查找所有separator出现的地方，分隔符两边的每个单元文本都会被赋值到该数组中。

  该方法忽略g标志。

  使用正则表达式时，需要注意：

  * 来自分组捕获的文本将会被包含在被分割后的数组中。

    ```javascript
    var text = 'last,  first  ,middle';
    var d = text.split(/\s*,\s*/);
    // d is [
    // 'last',
    // 'first',
    //    'middle'
    // ]
    var e = text.split(/\s*(,)\s*/);
    // e is [
    // 'last',
    // ',',
    // 'first',
    // ',',
    //    'middle'
    // ]
    ```

  * 有一些js的实现（不同的系统），在输出数组中会禁止空字符串

    ```javascript
    var c = '|a|b|c|'.split('|');
    // c is ['', 'a', 'b', 'c', '']
    
    var f = '|a|b|c|'.split(/\|/);
    // f is ['a', 'b', 'c'] on some systems, and
    // f is ['', 'a', 'b', 'c', ''] on others
    ```

* string.substring(start, end)

  跟slice方法一样，但是不能处理负数参数。请使用slice函数代替它

* string.toLocaleLowerCase( ) 和 string.toLocaleUpperCase( )

  使用本地化的规则把这个string的所有字母转换成小写或者大写

  主要用在土耳其语上

* string.toLowerCase( ) 和 string.toUpperCase( )

  将string的所有字母都转化成小写或者大写格式

* String.fromCharCode(char...)

  注意这是string类的static方法

  从一串数字中返回一个字符串

  ```javascript
  var a = String.fromCharCode(67, 97, 116);
  // a is 'Cat'
  ```

### 第九章 代码风格

I indented the contents of blocks and object literals four spaces. I placed a space between if and ( so that the if didn’t look like a function invocation. Only in invo- cations do I make ( adjacent with the preceding symbol. I put spaces around all infix operators except for . and [ , which do not get spaces because they have higher prece- dence. I use a space after every comma and colon.

I put at most one statement on a line. Multiple statements on a line can be misread. If a statement doesn’t fit on a line, I will break it after a comma or a binary operator. That gives more protection against copy/paste errors that are masked by semicolon insertion.  I indent the remainder of the statement an extra four spaces, or eight spaces if four would be ambiguous (such as a line break in the condition part of an if statement).

I always use blocks with structured statements such as if and while because it is less error prone. A pair of braces is really cheap protection against bugs that can be expensive to find.

I always use the K&R style, putting the { at the end of a line instead of the front, because it avoids a horrible design blunder in JavaScript’s return statement.

I included some comments. I like to put comments in my programs to leave informa- tion that will be read at a later time by people (possibly myself) who will need to understand what I was thinking. Sometimes I think about comments as a time machine that I use to send important messages to future me.

I struggle to keep comments up-to-date. Erroneous comments can make programs even harder to read and understand. I can’t afford that.

I tried to not waste your time with useless comments.

In JavaScript, I prefer to use line comments. I reserve block comments for formal documentation and for commenting out.

I prefer to make the structure of my programs self-illuminating, eliminating the need for comments. I am not always successful, so while my programs are awaiting perfec- tion, I am writing comments.

ES5以及之前：The convention that variables should be declared at their first use is really bad advice in JavaScript. JavaScript has function scope, but not block scope, so I declare all of my variables at the begin- ning of each function. JavaScript allows variables to be declared after they are used. That feels like a mistake to me, and I don’t want to write programs that look like mistakes. I want my mistakes to stand out. 

Similarly, I never use an assignment expression in the condition part of an `if`

I never allow switch cases to fall through to the next case.

I use a single global variable to contain an application or library. Every object has its own namespace, so it is easy to use objects to organize my code. Use of closure provides further information hiding, increasing the strength of my modules.



































