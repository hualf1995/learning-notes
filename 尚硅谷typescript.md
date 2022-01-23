### TypeScript

* 简介
  * 是什么？
    * 以js为基础构建的语言
    * 一个js的超集
    * 可以在任何支持js的平台执行
    * ts拓展了js，并添加了类型
    * ts不能被js解析器直接执行，需要通过编译转换成js
  * 增加了什么？
    * 类型
    * 支持es的新特性
    * 添加了es不具备的新特性
    * 丰富的配置选项
    * 强大的开发工具
* 开发环境搭建
  * 下载并安装nodejs
  * 用npm全局安装typescript --> ts的编译器
  * tsc xx.ts  --> 编译xx.ts文件 --> 得到一个可以在浏览器内运行的xx.js文件

#### 基础类型

##### 类型声明

* `let a: number;`

  * 声明一个变量a，并且指定类型为number

* 可以通过后续的配置，改变出错后还能成功编译的情况

* ts可以通过配置，选择编译成什么版本的js --> 兼容性

* 一般来说我们声明变量后会直接赋值： `let a: boolean = true`

  * 如果声明和赋值同时进行的话，可以不需要再声明类型。ts已经能够自动检测变量类型。（之后再给a赋其他类型的值会报错）

* 类型声明的优势在定义函数时更明显

  ```typescript
  function sum(a: number, b: number): number {
  	return a + b;
  }
  // 如果是js的话，是不考虑参数类型和数量的；而在ts中会对参数的类型以及个数做检测，类型不对或者参数数量少或者多都会报错
  // ts还可以声明返回值的类型
  ```

##### ts中的类型

* number

* boolean

* string

* 字面量

  * `let a: 10`
  * 引入或 --> `|`
    * `let a: 'male'|'false'`
    * 更多用在多种类型（联合类型） --> `let a: string|boolean`

* any --> 任意类型(显式)

* unknow 未知类型的值

  * any不仅自己是任意类型，还能赋值给任意变量

  * unknown是一种**类型安全的any**，不能直接赋值给其他变量

  * 如果一定要将unknown赋值给其他变量呢？

    * 做一个类型判断

      * ```typescript
        let a: unknown;
        let b:string = 'hello';
        
        a = 'test';
        
        if (typeof a === 'string') {
          b = a;
        }
        ```

    * 类型断言

      * 用于告诉编译器变量的类型

      * ```typescript
        let a: unknown;
        let b:string = 'hello';
        
        a = 'test';
        /**
        * 两种语法
        * 第一种：变量 as 类型
        * 第二种：<类型>变量
        */
        b = a as string;
        b = <string>a;
        ```

* void

  * 空值，以函数为例就是没有返回值（返回值是undefined/null）

* never

  * 没有值，不能是任何值（对比void）

  * 表示永远不会返回结果

    * ```typescript
      function fn(): never {
        throw new Error('error!!!');
      }
      // 这种函数报错之后程序不再进行，也不会有任何结果
      ```

* object

  * 一般不直接用object去type，没有实际意义

  * `{}`

  * ```typescript
    let a: {name: string};
    // 定义了a是一个有name属性的对象
    // {}中可以指定对象包含哪些属性
    let b: {name: string, age?: number};
    // 语法： {属性名：属性值类型，属性名：属性值类型，....}
    // 在属性名后面加上？表示可选 --> 变量b是一个有name属性，但可能有age属性的对象
    // 如果对象可能还有其他属性，但不可能一个个用?去表示
    let c: {name: string, [whateverName: string]: string};
    // 表示除了name属性，还可能有其他属性值为string的属性
    ```

* function

  * 一般也不会直接用Function去type

  * 设置函数结构的类型声明： `(形参：类型，形参：类型，...) => 返回值类型`

  * ```typescript
    let a: (x: number, y: number) => number;
    // 形参类型错误，返回值类型错误，多参数或者少参数 都会报错
    ```

* array

  * 两种方式： `string[]` 和 `Array<string>` --> `类型[]` 和 `Array<类型>`

* tuple 元组

  * 固定长度的数组
  * `let a: [string, number]` --> 语法是 [类型，类型，....]

* enum 枚举

  * ```typescript
    enum Gender{
      male,
      female
    } // 如果没有给值，默认会从0开始赋值；如果male = 2，但是female没给值，会自动赋3给female
    
    let a:{name: string, gender: Gender};
    ```

* * 引入了 **&** 符号

    * 表示 且， 需要同时满足
    * 一般用于对象的类型： `let a: {name: string} & {age: number}` 
    * 变量a必须满足既有name又要有age

  * 引入 类型的别名

    * 为了简化类型声明 -->  type xx = 类型声明;

    * ```typescript
      type myType = 1 | 2| 3| 4| 5;
      
      let a: myType;
      let b: myType;
      let c: myType; // 不需要再多次声明相同的类型
      ```

#### ts编译选项

为了使ts开发更优雅，自动化编译

* 自动编译文件 --> watch mode

  * tsc xxx.ts -w

* 自动编译整个项目

  * 需要有配置文件 tsconfig.json 然后就可以通过`tsc -w`自动编译整个项目


##### tsconfig.json --> [reference](https://www.typescriptlang.org/tsconfig)

* 是ts编译器的配置文件，ts编译器会根据它的信息来进行编译

  * ```json
    {}
    ```

* include

  * 定义希望被编译文件所在的目录

  * 默认值：所有文件

  * 举例：  "include":["src/++/+", "tests/++/+"]

    所有src目录和tests目录下的文件都会被编译

  * ** 表示 任意目录， * 表示任意文件

* exclude

  * 定义需要排除在外的目录

  * 默认值：["node_modules", "bower_components", "jspm_packages"]

  * 举例： "exclude": ["./src/hello/++/*"]

    src下hello目录下的文件都不会被编译

* extends

  * 定义被继承的配置文件

  * 举例： "extends": "./configs/base"

    当前配置文件中会自动包含config目录下base.json中的所有配置信息

* files

  * 指定被编译文件的列表，只有需要编译的文件少时才会用到

    ```json
    "files": [
        "core.ts",
        "sys.ts",
        "types.ts",
        "scanner.ts",
        "parser.ts",
        "utilities.ts",
        "binder.ts",
        "checker.ts",
        "tsc.ts"
    ]
    ```

* compilerOptions

  * 编译选项是配置文件中非常重要也比较复杂的配置选项

  * 项目选项

    * target

      * 设置ts代码编译的目标版本
      * ES3（默认）、ES5、ES6/ES2015、ES7/ES2016、ES2017、ES2018、ES2019、ES2020、ESNext

      ```json
      "compilerOptions": {
          "target": "ES6"
      }
      //所编写的ts代码将会被编译为ES6版本的js代码
      ```

    * lib

      * 指定代码运行时所包含的库
      * 一般不需要配置这个，默认为浏览器中运行所需的库
      * ES5、ES6/ES2015、ES7/ES2016、ES2017、ES2018、ES2019、ES2020、ESNext、DOM、WebWorker、ScriptHost ......

    * module

      * 设置编译后代码使用的模块化系统
      * CommonJS、UMD、AMD、System、ES2015、ES2020、ESNext、None...

      ```json
      "compilerOptions": {
          "module": "CommonJS"
      }
      ```

    * outDir

      * 编译后文件的所在目录
      * 默认情况下，编译后的js文件会和ts文件位于相同的目录，设置outDir后可以改变编译后文件的位置

      ```json
      "compilerOptions": {
          "outDir": "./dist"
      }
      // 设置后编译后的js文件将会生成到dist目录
      ```

    * outFile (一般由打包工具比如webpack来做)

      * 将所有的文件编译为一个js文件
      * 默认会将所有的编写在全局作用域中的代码合并为一个js文件，如果module配置为None、System或AMD则会将模块一起合并到文件之中

      ```json
      "compilerOptions": {
          "outFile": "./dist/app.js"
      }
      ```

    * rootDir (好像课上没讲)

      * 指定代码的根目录，默认情况下编译后文件的目录结构会以最长的公共目录为根目录，通过rootDir可以手动指定根目录

      ```json
      "compilerOptions": {
          "rootDir": "./src"
      }
      ```

    * allowJs

      * 是否对js文件编译（默认为false）

    * checkJs

      * 是否对js文件进行检查（默认为false）

    * removeComments

      * 是否删除注释（默认为false）

    * noEmit

      * 不生成编译后的文件（默认为false）

    * noEmitOnError

      * 当有错误时不生成编译后的文件（默认为false）
      * 更严格的编译

    接下来是语法检查的相关属性

    * strict
      * 严格检查的总开关，默认值为false，建议设为true开启所有的严格检查
    * alwaysStrict
      * 以严格模式编译代码
    * noImplicitAny
      * 禁止隐式的any类型
    * noImplicitThis
      * 禁止类型不明确的this
    * strictNullChecks
      * 严格的空值检查
      * 比如document.getxxx有可能返回null，需要严格检查

    。。。。 --> [更多](https://github.com/JasonkayZK/typescript_learn/tree/2-compile-options)

#### 使用webpack打包ts代码 --> 最好知道每一步在干什么

* 初始化项目

  * 进入根目录，执行`npm init -y`， 创建package.json文件（配置文件）

* 安装依赖（构建工具

  `npm i -D webpack webpack-cli typescript ts-loader`

  * webpack：构建工具webpack
  * webpack-cli：webpack的命令行工具
  * typescript：ts编译器
  * ts-loader：ts加载器，用于在webpack中编译ts文件

* 配置webpack

  * 根目录下创建`webpack.config.js`

  ```javascript
  const path = require("path"); //帮助拼接路径
  
  // webpack中的所有配置信息
  module.exports = {
    // 入口文件（主文件，从哪里开始执行）
    entry: "./src/index.ts",
    
    // 指定打包文件所在的目录
    output: {
      // 指定打包文件的目录
      path: path.resolve(__dirname, "dist"), // ./src/dist
      // 打包后的文件
      filename: "bundle.js"
    },
    
   	// 指定webpack打包时使用的模块
    module = {
    	// 指定要加载的规则
    	rules: [
    		{
    			// test指定的是规则生效的文件
    			test: /\.ts*/,
    			// use指定使用的loader
    			use: 'ts-loader',
    			// 要排除的文件
    			exclude: /node_modules/
  			}
  		]
  	}
  };
  ```

* 配置ts编译选项  `tsconfig.json`

  ```json
  {
     "compilerOptions": {
         "target": "ES2015",
         "module": "ES2015",
         "strict": true
     }
  }
  ```

* 修改package.json 增加命令

  ```json
  {
     ...
     "scripts": {
         "test": "echo \"Error: no test specified\" && exit 1",
         "build": "webpack"
     },
     ...
  }
  ```

* 引入html-webpack-plugin插件

  * webpack中html插件，用来自动创建html文件并且引入相关的资源
  * 需要在`webpack.config.js`中的plugins中引入

  ```javascript
  ...
  const HtmlWebpackPlugin = require("html-webpack-plugin");
  
  module.exports = {
    ...
    
    // 配置webpack插件
    plugins: [
      new HtmlWebpackPlugin({
        title: '用来定义title',
        template: 'html模板的路径，为该插件创建的html提供模板'
      })
    ]
  };
  ```

* 引入webpack-dev-server

  * webpack的开发服务器
  * 在项目里安装一个内置的服务器，根据项目的更新自动刷新

  `npm install webpack-dev-server`

  另外需要在package.json里去配置命令

  `”start": "webpack serve --open chrome.exe"` npm start即可开启一个本地服务器来使用我们的应用并在chrome中打开

* 引入clean-webpack-plugin插件

  * webpack中的清除插件，每次构建都会先清除目录（防止有旧的文件存在）

  ```javascript
  ...
  const {CleanWebpackPlugin} = require("clean-webpack-plugin");
  
  module.exports = {
    ...
    
    // 配置webpack插件
    plugins: [
      new CleanWebpackPlugin(),
      new HtmlWebpackPlugin({
        title: '用来定义title',
        template: 'html模板的路径，为该插件创建的html提供模板'
      })
    ]
  };
  ```

* 为解决模块引入问题，还需要在webpack配置文件中配置resolve

  ```javascript
  ...
  
  module.exports = {
    ...
    
    // 用来设置引用模块（哪些可以被当做模板引用）
    resolve: {
    	extensions: ['.ts', '.js']
    }
  };
  ```

* 以上为一些必要的配置，下面考虑兼容性等其他问题

* Babel

  虽然TS在编译时也支持代码转换，但是只支持简单的代码转换；

  对于例如：Promise等ES6特性，TS无法直接转换，这时还要用到babel来做转换；

  * 安装依赖
    * `npm i -D @babel/core @babel/preset-env babel-loader core-js`
    * @babel/core： babel的核心工具
    * @babel/preset-env：babel的预定义环境（根据不同浏览器的环境进行转换）
    * @babel-loader：babel在webpack中的加载器
    * core-js：模拟js运行环境的代码，用来使老版本的浏览器支持新版ES语法（比如promise， corejs会提供一个自己写的promise）
  * 在webpack文件中进行配置

  ```javascript
  ...
  module.exports = {
    // ...
    module: {
      rules: [
        {
          test: /\.ts$/,
  				use: [ //当有多个加载器使用时，可以写成array
            // 加载器从后往前使用，所以将ts-loader写在下面，先进行ts编译成js，再被babel处理。
            {
              // 指定加载器
              loader: "babel-loader",
              // 设置babel
              options: {
                // 设置预定义的环境
                presets: [
                  // 第一个环境
                  [
                    // 指定环境的插件
                    "@babel/preset-env",
                    // 配置信息
                    {
                      // 要兼容的目标浏览器
                      targets: {
                        "chrome": "58",
                        "ie": "11"
                      },
                      // 使用的corejs的版本
                      "core.js": "3",
                      // 使用corejs的方式
                      useBuiltIns: "usage" // 按需下载
                    }
                  ]
                ]
              }
            }, // 较为复杂的加载器的配置用object
            "ts-loader"
          ]
        }
      ]
    }
  };
  ```

  * 即使如此，依然可能在ie中报错，因为webpack在打包后，会自带一个箭头函数包裹起来。需要进行配置告诉webpack不能使用箭头函数。

    ```javascript
    ...
    output: {
      ...
      environment: {
        arrowFunction: false // 关闭webpack的箭头函数，可选
      }
    }
    ...
    ```

#### 面向对象（其实很多内容java里学过，封装继承多态，还有类、抽象类、接口）

* 简而言之，所有操作都是通过对象来完成的。
* 对象是什么？

  * 计算机程序的本质就是对现实事物的抽象。
  * 一个事物，抽象到程序里，就成了一个对象。
  * 数据在对象里叫做属性；功能在对象里叫做方法。所以程序中的一切皆为对象。

##### 类class

* 要有对象就要先定义类。类可以理解为对象的模型。根据类可以创建指定类型的对象。

* 如何定义类？

  ```typescript
  class Person{
    // 属性
    // 实例属性，需要通过创建的对象去访问
    // const p = new Person(); p.name;
    name: string = 'abc';
    // 静态属性，需要通过类直接调用
    // Person.age
    static age: number = 18;
    // 还可以添加另一个关键字 readonly 只读
    
    // 方法(也有实例方法和静态方法)
    hello() {
      console.log('xxx');
    }
  }
  ```

* 构造函数和this

  需要在创建对象时，初始化实例的属性

  new对象的时候，相当于在调用constructor

  在**实例方法**中，this就表示当前的实例（调用方法的实例对象）

  在构造函数中，可以通过this向新建的对象中添加属性

  ```typescript
  class Dog{
  	name: string;
    age: number;
    
    constructor(name: string, age: number) {
      this.name = name;
      this.age = age;
    }
    bark() {
      console.log(this);
    }
  }
  
  const dog = new Dog('xx', 3);
  dog.bark();
  ```

##### 继承

* OCP 开闭原则
  
  * 软件中的对象应该对于扩展是开放的，但是对于修改是封闭的
  
* extends关键词

* 继承后子类拥有父类所有的属性和方法

* 可以在子类中新写只属于子类的方法；也可以重写父类中的方法(override)来实现子类特有的业务逻辑

* 可以在子类中添加新的属性

  * super关键词

    * 在子类中，super表示该子类的父类

  * 在子类中写构造函数，需要去调用父类的构造函数

    ```typescript
    class Animal{
        name: string;
    
        constructor(name: string) {
            this.name = name;
        }
    }
    
    class Dog extends Animal{
      age: number;
      
      constructor(name: string, age: number) {
        super(name); // 通过super调用父类的构造函数，此处相当于 this.name = name
        this.age = age;
      }
    }
    ```

##### 抽象类

* 以abstract开头的class是抽象类
  * 不能用来创建对象
  * 专门来用被继承的类
* 抽象方法
  * 只能定义在抽象类中
  * 没有具体实现的方法体（但是需要定义输入输出）
  * 在继承该抽象类的子类中，必须重写该抽象方法

```typescript
abstract class Animal {
  name: string;
  age: number;
  
  constructor(name: string, age: number) {
    this.name = name;
    this.age = age;
  }
  
  abstract bark(): void;
}
class Dog extends Animal {
  bark() {
    console.log(this);
  }
}
```

##### 接口 interface

* 用来定义一个类的结构，一个类应该包含哪些属性和方法(implements)

  * 用来限制一个类的结构，对象只有包含接口中定义的所有属性和方法时才能匹配接口

  * 接口中的所有方法和属性都是没有实值的，接口中的所有方法都是抽象方法

  * ```typescript
    interface MyInterface{
      name: string;
      fn(): void;
    }
    
    class MyClass implements MyInterface{
      name: string;
      
      constructor(name: string) {
        this.name = name;
      }
      
      fn() {
        console.log('ss');
      }
    }
    ```

* 也可以当成类型声明去使用（比如Object其实本身也是一种类）

  * ```typescript
    type myType = {
      name: string;
      age: number;
    };
    
    //之后就不能再声明myType这种类型了
    
    interface myInterface{
      name: string;
      age: number
    }
    interface myInterface{
      gender: string;
    }
    // 接口用作类型声明的时候，可以多次定义，最终myInterface是个merge的结果
    ```

##### 封装

* 默认情况下，对象的属性是可以任意的修改的。为了数据的安全性，我们可以限制属性的访问权限。

* 三种修饰符

  * public，是默认值。可以在任意位置访问或修改属性
  * protected，只能在类或者该类的子类中访问或修改
  * private，只能在类中访问或修改属性

* getter setter

  ```typescript
  class Person {
    private _name: string;
    private _age: number;
    constructor(name: string, age: number) {
      this._name = name;
      this._age = age;
    }
    get name() {
      return this._name;
    }
    set name(value: string) {
      this._name = value;
    }
  }
  
  const per = new Person('abc', 123);
  // 看上去是在访问或修改属性，实际上是调用了对应的getter和setter
  // 可以在getter或setter里添加逻辑判断，防止数据被随意修改
  console.log(per.name);
  per.name = 'def';
  console.log(per.name);
  ```

* 构造函数的简写方式

  ```typescript
  // 以下两种方式效果相同
  class Person1 {
    private _name: string;
    private _age: number;
    constructor(name: string, age: number) {
      this._name = name;
      this._age = age;
    }
  }
  
  class Person2 {
    constructor(private name: string, private age: number) {
    }
  }
  ```

##### 泛型 (Generics)

* 在定义函数或类时，如果遇到类型不明确（返回值、参数、属性的类型不能确定）的情况，可以考虑使用泛型。（而不是any）

```typescript
function fn1<T>(name: T): T {
  return name;
}
fn1('abc'); // TS的自动推断可以帮助判断类型，不过还是推荐明确写出泛型的类型
fn1<number>(123); // 手动指定泛型的类型 --> 推荐

// 可以指定多个泛型
function fn2<T, K>(name: T, age: K): T {
  console.log(age);
  return name;
}
fn2<string, number>('abc', 123);

// 可以对泛型进行约束  extends关键词
// 使用T extends MyInter表示泛型T必须是MyInter的子类，不一定非要使用接口。类和抽象类同样适用。
interface MyInter {
  length: number;
}
function fn3<T extends MyInter>(input: T): number {
  return input.length;
}
fn3<string>('abc');
fn3<Array<number>>([1,2,3]);
fn3({length: 3});
```

* 泛型类

```typescript
class MyClass<T> {
  name: T;
  constructor(name: T) {
    this.name = name;
  }
}
const a = new MyClass<string>('abc');
const b = new MyClass<number>(123);
```

#### 项目练习：贪吃蛇

##### 项目搭建

* 在之前讲的webpack基础上进行
* 配置css的部分（使webpack能对css进行处理）
  *  style-loader, css-loader, less, less-loader；（以less为例）
  * 我们也需要跟babel类似的工具，对css进行兼容性的处理。
    * postcss；
    * postcss-loader；
    * postcss-preset-env；

```javascript
// webpack.config.js
// 引入一个包
const path = require('path');
// 引入html插件
const HTMLWebpackPlugin = require('html-webpack-plugin');
// 引入clean插件
const {CleanWebpackPlugin} = require('clean-webpack-plugin');

// webpack中的所有的配置信息都应该写在module.exports中
module.exports = {
    // 指定入口文件
    entry: "./src/index.ts",

    // 指定打包文件所在目录
    output: {
        // 指定打包文件的目录
        path: path.resolve(__dirname, 'dist'),
        // 打包后文件的文件
        filename: "bundle.js",
        environment: {
          	// 告诉webpack不使用箭头
            arrowFunction: false,
            // 不使用const,此时兼容IE 10
            const: false
        }
    },

    // 指定webpack打包时要使用模块
    module: {
        // 指定要加载的规则
        rules: [
            {
                // test指定的是规则生效的文件
                test: /\.ts$/,
                // 要使用的loader
              	// 比较简单的直接写loader名字即可，复杂的加载器需要写成对象进行配置
                use: [// use里的加载器从下往上执行，先ts到js，在处理js兼容性
                    // 配置babel
                    {
                        // 指定加载器
                        loader: "babel-loader",
                        // 设置babel
                        options: {
                            // 设置预定义的环境
                            presets: [
                                [
                                    // 指定环境的插件
                                    "@babel/preset-env",
                                    // 配置信息
                                    {
                                        // 要兼容的目标浏览器
                                        targets: {
                                            "chrome": "58",
                                            "ie": "11"
                                        },
                                        // 指定corejs的版本
                                        "corejs": "3",
                                        // 使用corejs的方式 "usage" 表示按需加载
                                        "useBuiltIns": "usage"
                                    }
                                ]
                            ]
                        }
                    },
                    'ts-loader'
                ],
                // 要排除的文件
                exclude: /node-modules/
            },

            // 设置less文件的处理
            {
                test: /\.less$/,
                use: [// use里的加载器从下往上执行
                    "style-loader",
                    "css-loader",

                    // 引入postcss
                    // 类似于babel，把css语法转换兼容旧版浏览器的语法
                    {
                        loader: "postcss-loader",
                        options: {
                            postcssOptions: {
                                plugins: [
                                    [
                                        // 浏览器兼容插件
                                        "postcss-preset-env",
                                        {
                                            // 每个浏览器最新两个版本
                                            browsers: 'last 2 versions'
                                        }
                                    ]
                                ]
                            }
                        }
                    },
                    "less-loader"
                ]
            }
        ]
    },

    // 配置Webpack插件
    plugins: [
        new CleanWebpackPlugin(),
        new HTMLWebpackPlugin({
            // title: "这是一个自定义的title"
            template: "./src/index.html"
        }),
    ],

    // 用来设置引用模块
    resolve: {
        extensions: ['.ts', '.js']
    }

};
```

##### 项目界面（静态html）

* 最外面的页面
* 游戏舞台
  * 蛇
  * 食物
* 记分牌

```html
<!DOCTYPE html>
<html lang="zh">
  <head>
      <meta charset="UTF-8">
      <title>贪吃蛇</title>
  </head>
  <body>
      <!--创建游戏的主容器-->
      <div id="main">
          <!--设置游戏的舞台-->
          <div id="stage">
              <!--设置蛇-->
              <div id="snake">
                  <!--snake内部的div 表示蛇的各部分-->
                  <div></div>
              </div>

              <!--设置食物-->
              <div id="food">
                  <!--添加四个小div 来设置食物的样式-->
                  <div></div>
                  <div></div>
                  <div></div>
                  <div></div>
              </div>
          </div>

          <!--设置游戏的积分牌-->
          <div id="score-panel">
              <div>
                  Score:<span id="score">0</span>
              </div>
              <div>
                  Level:<span id="level">1</span>
              </div>
          </div>
      </div>
  </body>
</html>
```

```less
// 设置变量
@bg-color: #b7d4a8;

// 清除默认样式
* {
  margin: 0;
  padding: 0;
  // 改变盒子模型的计算方式
  box-sizing: border-box;
}

body {
  font: bold 20px "Courier";
}

//设置主窗口的样式
#main {
  width: 360px;
  height: 420px;
  background-color: @bg-color;
  // 设置居中
  margin: 100px auto;
  // 设置边框
  border: 10px solid black;
  // 设置圆角
  border-radius: 40px;

  // 开启弹性盒模型
  display: flex;
  // 设置主轴的方向
  flex-flow: column;
  // 设置侧轴的对齐方式
  align-items: center;
  // 设置主轴的对齐方式
  justify-content: space-around;

  // 游戏舞台
  #stage {
    width: 304px;
    height: 304px;
    border: 2px solid black;
    // 开启相对定位
    position: relative;

    // 设置蛇的样式
    #snake {
      & > div {
        width: 10px;
        height: 10px;
        background-color: #000;
        // 蛇的边框颜色和背景颜色相同
        border: 1px solid @bg-color;
        // 开启绝对定位
        position: absolute;
      }
    }

    // 设置食物
    #food {
      width: 10px;
      height: 10px;
      position: absolute;
      // 初始化一个位置
      left: 40px;
      top: 100px;

      // 开启弹性盒
      display: flex;
      // 设置横轴为主轴，wrap表示会自动换行
      flex-flow: row wrap;

      // 设置主轴和侧轴的空白空间分配到元素之间
      justify-content: space-between;
      align-content: space-between;

      & > div {
        width: 4px;
        height: 4px;
        background-color: black;

        // 使四个div旋转45度
        transform: rotate(45deg);
      }
    }
  }

  // 记分牌
  #score-panel {
    width: 300px;
    display: flex;
    // 设置主轴对齐方式
    justify-content: space-between;
  }
}
```

**align-content** determines the spacing between lines

**align-items** determines how the items **as a whole** are aligned within the container.

When there is only one line, **align-content** has no effect

##### Food类

```typescript
// 定义食物类Food
class Food {
    // 定义一个属性表示食物所对应的元素
    private element: HTMLElement;

    constructor() {
        // 获取页面中的food元素并将其赋值给element
        // 末尾加上叹号，表示id为food的元素必定存在（非空）
        this.element = document.getElementById('food')!;
    }

    // 定义一个获取食物X轴坐标的方法
    get X() {
        return this.element.offsetLeft;
    }

    // 定义一个获取食物Y轴坐标的方法
    get Y() {
        return this.element.offsetTop;
    }

    // 修改食物的位置
    change() {
        // 生成一个随机的位置
        // 食物的位置最小是0 最大是290
        // 蛇移动一次就是一格，一格的大小就是10，所以就要求食物的坐标必须是整10
        let top = Math.round(Math.random() * 29) * 10;
        let left = Math.round(Math.random() * 29) * 10;

        this.element.style.left = left + 'px';
        this.element.style.top = top + 'px';
    }
}
export default Food;
```

##### ScorePanel

```typescript
class ScorePanel {
  score: 0;
  level: 1;
  
  scoreEle: HTMLElement;
  levelEle: HTMLElement;
  maxLevel: number;
  upgradeBaseScore: number;
  
  constructor(maxLevel: number = 10, upgradeBaseScore: number = 10) {
    this.scoreEle = document.getElementById('score')!;
    this.levelEle = document.getElementById('level')!;
    this.maxLevel = maxLevel;
    this.upgradeBaseScore = upgradeBaseScore;
  }
  
  addScore() {
    this.scoreEle.innerHTML = ++this.score + '';
    
    if (this.score % this.upgradeBaseScore === 0) {
      this.levelUp();
    }
  }
  
  levelUp() {
    if (this.level < this.maxLevel) {
      this.levelEle.innerHTML = ++this.level + '';
    }
  }
}

export default ScorePanel;
```

##### Snake

```typescript
class Snake {
  container: HTMLElement;
	head: HTMLElement;
  bodies: HTMLCollection; // 是即使更新的；当其所包含的文档结构发生改变时，它会自动更新。
  
  constructor() {
    this.container = document.getElementById('snake')!;
    this.head = document.querySelector('#snake > div') as HTMLElement;
    this.bodies = document.getElementsByTagName('div');//不用querySelectorAll是因为返回的是nodelist，不是HTMLCollection
  }
  
  get X() {
    return this.head.offsetLeft;
  }
  
  get Y() {
    return this.head.offsetTop;
  }
  
  set X(value: number) {
    // 如果新值和旧值相同，则直接返回不再修改
    if (this.X === value) {
      return;
    }
    
    // X的值的合法范围0-290之间
    if (value < 0 || value > 290) {
      // 进入判断说明蛇撞墙了
      throw new Error('蛇撞墙了！');
    }
    
    // 禁止调头
    // 如果下一个蛇头的水平坐标与第二节蛇身相同，则说明是调头行为
    if (this.bodies[1] && (this.bodies[1] as HTMLElement).offsetLeft === value) {
      // 如果发生了调头，则忽略调头，修正value的值，使其继续往之前的方向走
      // 以下代码是在修正value的值
      if (value > this.X) {
        // 如果新值value大于旧值X，则说明蛇想要向右走（此时发生调头），应该使蛇继续向左走
        value = this.X - 10;
      } else {
        value = this.X + 10;
      }
    }
    
    // 移动身体
    this.moveBody();
    this.head.style.left = value + 'px';
    // 检查有没有撞到自己
    this.checkHeadBody();
  }
  
  set Y(value: number) {
    // 如果新值和旧值相同，则直接返回不再修改
    if (this.Y === value) {
      return;
    }
    
    // Y的值的合法范围0-290之间
    if (value < 0 || value > 290) {
      // 进入判断说明蛇撞墙了，抛出一个异常
      throw new Error('蛇撞墙了！');
    }
    
    // 禁止调头
    // 如果下一个蛇头的垂直坐标与第二节蛇身相同，则说明是调头行为
    if (this.bodies[1] && (this.bodies[1] as HTMLElement).offsetTop === value) {
      // 如果发生了调头，则忽略调头，修正value的值，使其继续往之前的方向走
      // 以下代码是在修正value的值
      if (value > this.Y) {
        // 如果新值value大于旧值X，则说明蛇想要向下走（此时发生调头），应该使蛇继续向上走
        value = this.X - 10;
      } else {
        value = this.X + 10;
      }
    }
    
    // 移动身体
    this.moveBody();
    this.head.style.top = value + 'px';
    // 检查有没有撞到自己
    this.checkHeadBody();
  }
  
  addBody() {
    // 向element中添加一个div
    // beforeend：结束标签之前的位置
    this.container.insertAdjacentHTML('beforeend', '<div></div>');
  }
  
  // 移动身体（不包括蛇头）
  moveBody() {
     /*
        *   将后边的身体设置为前边身体的位置
        *       举例子：
        *           第4节 = 第3节的位置
        *           第3节 = 第2节的位置
        *           第2节 = 蛇头的位置
        */
    // 遍历获取所有的身体
    for (let i = this.bodies.length - 1; i > 0; i--) {
      // 获取前边身体的位置
      let X = (this.bodies[i - 1] as HTMLElement).offsetLeft;
      let Y = (this.bodies[i - 1] as HTMLElement).offsetTop;

      // 将值设置到当前身体上
      (this.bodies[i] as HTMLElement).style.left = X + 'px';
      (this.bodies[i] as HTMLElement).style.top = Y + 'px';
    }
  }
  
  // 检查蛇头是否撞到身体的方法
  checkHeadBody() {
    // 获取所有的身体，检查其是否和蛇头的坐标发生重叠
    for (let i = 1; i < this.bodies.length; i++) {
      let bd = this.bodies[i] as HTMLElement;
      if (this.X === bd.offsetLeft && this.Y === bd.offsetTop) {
        // 进入判断说明蛇头撞到了身体，游戏结束
        throw new Error('撞到自己了！');
      }
    }
  }
}

export default Snake;
```

##### GameControl 游戏的主控制类

```typescript
import Food from './Food';
import ScorePanel from './ScorePanel';
import Snake from 'Snake';

class GameControl {
  snake: Snake;
  scorePanel: ScorePanel;
  food: Food;
  
  direction: string = ''; // 创建一个属性来存储蛇的移动方向（也就是按键的方向）
  isLive: boolean = true; // 创建一个属性用来记录游戏是否结束
  
  constructor() {
    this.snake = new Snake();
    this.food = new Food();
    this.scorePanel = new ScorePanel();
    this.init();
  }
  // 游戏的初始化方法，调用后游戏即开始
  init() {
    document.addEventListener('keydown', this.keydownHandler.bind(this));
    
    this.run();
  }
  
  keydownHandler() {
    // 需要检查event.key的值是否合法（用户是否按了正确的按键）
    // 修改direction属性
    this.direction = event.key;
  }
  
  run() {
     /*
        *   根据方向（this.direction）来使蛇的位置改变
        *       向上 top  减少
        *       向下 top  增加
        *       向左 left 减少
        *       向右 left 增加
        */
    // 获取蛇现在坐标
    let X = this.snake.X;
    let Y = this.snake.Y;

    // 根据按键方向来计算X值和Y值（未更新）
    switch (this.direction) {
      case "ArrowUp":
      case "Up":
        // 向上移动 top 减少
        Y -= 10;
        break;
      case "ArrowDown":
      case "Down":
        // 向下移动 top 增加
        Y += 10;
        break;
      case "ArrowLeft":
      case "Left":
        // 向左移动 left 减少
        X -= 10;
        break;
      case "ArrowRight":
      case "Right":
        // 向右移动 left 增加
        X += 10;
        break;
    }
    
    this.checkEat(X, Y);
    
    //修改蛇的X和Y值
    try {
      this.snake.X = X;
      this.snake.Y = Y;
    } catch (e) {
      // 进入到catch，说明出现了异常，游戏结束，弹出一个提示信息
      alert(e.message + ' GAME OVER!');
      // 将isLive设置为false
      this.isLive = false;
    }

    // 开启一个定时调用（定时器调用自身）
    // 会再次创建一个定时器
    this.isLive && setTimeout(this.run.bind(this), 300 - (this.scorePanel.level - 1) * 30);
  }
  
  // 检查蛇是否吃到食物
  checkEat(X: number, Y: number) {
    if (X === this.food.X && Y === this.food.Y) {
      // 食物的位置要进行重置
      this.food.change();
      // 分数增加
      this.scorePanel.addScore();
      // 蛇要增加一节
      this.snake.addBody();
    }
  }
}
```































