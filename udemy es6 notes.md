### Array Helpers

---

##### forEach

```javascript
const a = [1,2,3];
a.forEach((i, index) => console.log(`The ${index}th item is ${i}`));
```

##### map

```javascript
const a = [1,2,3];
const b = a.map((i, index) => i * index); //[0, 2, 6]
```

##### filter

```javascript
const a = [1,2,3];
const b = [1, undefined, 3];
const c = a.filter(item => item ===1); // [1]
const d = b.filter(Boolean); // [1, 3]
```

##### find

```javascript
const a = [1,2,3];
const b = a.find(item => item === 1); // 1
const c = a.find(item => item === 4); // undefined
```

##### every & some

```javascript
const a = [1,2,3];
const b = a.every(item => item >= 2); // false
const c = a.some(item => item >= 2); // true
```

##### reduce

1. if no initial value, the initial value will be the first element
2. don't forget to return the accumulated value
3. using spread operation in the callback is time consuming. 

```javascript
const a = [1,2,3];
const b = a.reduce((accumulator, currentValue) => accumulator + currentValue, 0); // 6
```

### const & let vs var

---

Block scope vs function scope

const cannot be re-assigned, and must be initialize during declaration.

### template strings

```javascript
const index = 1;
const value = 100;
const str1 = 'The index ' + index + ' is ' + value;
const str2 = `The index ${index} is ${value}`;
```

### arrow functions

```javascript
const a = item => item * 2;
const b = (item, num) => item * num;
const c = (item, num) => {
  const d = item * num;
  console.log(d);
};

// this context may be lost
const obj = {
  theme: 'fruit',
  fruits: ['apple', 'banana'],
  log: function() {
    return this.fruits.map(function(fruit) {
      console.log(`${fruit} is a ${this.theme}`); // wrong!
    })
  }
}
// To solve this problem: we can use bind or reference cache
const obj1 = {
  theme: 'fruit',
  fruits: ['apple', 'banana'],
  log: function() {
    const self = this;
    return this.fruits.map(function(fruit) {
      console.log(`${fruit} is a ${self.theme}`); // ok
    })
  }
}
// in ES6, We can use fat arrow function
const obj2 = {
  theme: 'fruit',
  fruits: ['apple', 'banana'],
  log: function() {
    // automatically set this = obj2
    return this.fruits.map(fruit => {
      console.log(`${fruit} is a ${this.theme}`); // ok
    })
  }
}
```

### Enhanced Object literals

```javascript
const a = 4;
const obj = {
  a, // this is Enhanced Object literals
  b: 3
};
```

### Default function arguments

```javascript
function abc(number = 0) {
  console.log(number);
}
```

### Rest & Spread

```javascript
// rest use case
function addNumbers(...numbers) {
	return numbers.reduce(xxx, 0);
}
addNumbers(1,2,3,4,5,6,7,8);

// spread use case
const a = [1, 2, 3, 4, 5];
const b = [...a, 6, 7]; // 1,2,3,4,5,6,7
```

### Destructuring

* object

  ```javascript
  const file = {
  	type: 'pdf',
    size: 123,
    name: 'name'
  };
  
  const {type} = file; // 'pdf'
  function logFile({type, size, name}) {
    // do something on the file object
  }
  logFile(file);
  ```

* array

  ```javascript
  const companies = [
  	'google',
    'uber',
    'fb'
  ];
  const [name, ...rest] = companies;
  const {length} = companies; // 3
  name; // 'google'
  rest; // ['uber', 'fb']
  ```

* array and object

  ```javascript
  const companies = [
  	{name: 'google', location: 'mtv'},
    {name: 'uber', location: 'sf'}
    {name: 'fb', location: 'mp'}
  ];
  const [{location}] = companies;
  location; // 'mtv'
  
  const Google = {
    locations: ['mtv', 'London']
  };
  const {locations: [loc]} = Google;
  loc; // 'mtv'
  
  // we can also destructure an object inside an object: 
  const a = {
    b: {
      c: 10
    }
  };
  const {b: {c}} = a;
  c; // 10
  ```

* function arugments(don't have to pass in a long list of params)
* easy to handle for looping through arrays for some cases.

### Class

* ES5 inheritance

  ```javascript
  function Car(options) {
  	this.title = options.title;
  }
  Car.prototype.drive = function() {
    return 'vroom';
  };
  
  function Toyota(options) {
    Car.call(this, options); // call the parent's constructor
    this.color = options.color;
  }
  // below two lines to inherit the parent's methods
  Toyota.prototype = Object.create(Car.prototype);
  Toyota.prototype.constructor = Toyota;
  // own methods
  Toyota.prototype.honk = function() {
    return 'beep';
  };
  
  const toyota = new Toyota({title: 'abc', color: 'red'});
  ```

* ES6 class syntax

  ```javascript
  class Car {
    constructor({title}) {
      this.title = title;
    }
    drive() {
      return 'vroom';
    }
  }
  
  class Toyota extends Car {
    constructor(options) {
      super(options); // equal to Car.constructor()
      this.color = options.color;
    }
    honk() {
      return 'beep';
    }
  }
  ```

### Generators

---

* function, can be entered or exited for multiple times. (usefull with **for-of** loop, which is used to iterate over **iterable objects**)

  ```javascript
  function* colors() {
    yield 'red';
    yield 'blue';
    yield 'green';
  } // the return value will be together with the final {done: true}
  const gen = colors();
  gen.next(); // {value: 'red', done: false}
  gen.next();	// {value: 'blue', done: false}
  gen.next(); // {value: 'green', done: false}
  gen.next(); // {done: true}
  
  const myColors = [];
  for (let color of colors()) {
    myColors.push(color);
  }
  console.log(myColors); // ['red', 'blue', 'green']
  ```

*  delegation of generators(yield*)

  ```javascript
  const a = {
    apple: 1
  };
  const b = {
    a,
    banana: 2,
    orange: 3,
    grape: 4
  };
  function* gen1(b) {
    yield b.banana;
    yield b.grape;
    yield* gen2(b.a);
  }
  function* gen2(a) {
    yield a.apple;
  }
  for (let i of gen1(b)) {...} // [2, 4, 1]
  // ugly but it is.
  ```

* Symbol.iterator: tell how to iterate your objects(much clearer then above, do not have to write separate iterator functions)

  ```javascript
  const a = {
    apple: 1,
    [Symbol.iterator]: function* () {
      yield this.apple;
    }
  };
  const b = {
    a,
    banana: 2,
    orange: 3,
    grape: 4,
    [Symbol.iterator]: function* () {
      yield this.banana;
      yield this.grape;
      yield* this.a;
    }
  };
  for (let i of b) {...} // [2, 4, 1]
  ```

* **for-in** loop is used to loop through the properties of an object. But sometimes, we may not want to go through the entire object. In this case, we need the combination of **for-of** loop and **Symbol.iterator with generators** to make the objects iterable.

* Generators with recursions:

  ```javascript
  class Comment {
    constructor(content, children) {
      this.content = content;
      this.children = children; // array of children nodes
    }
    
    *[Symbol.iterator]() {
      yield this.content;
      for (let child of this.children) {
        yield* this.child;
      }
    }
  }
  ```

### Promise

---

* Terminology: unresolved, resolved, rejected, then, catch.

* then and catch also return promises, so they can be chained.

* How to create a promise? How to use then and catch?

  ```javascript
  const resolveCallback = () => {};
  const rejectCallback = () => {};
  const promise = new Promise((resolve, reject) => {
    // do some long-time operation
    if (all the things are good) {
      resolve(resolvedDate);
    }
    if (something was wrong) {
      reject(errorData);
    }
  });
  promise.then(resolveCallback).catch(rejectCallback);
  
  // also other kinds of promise before the es6 native Promise object
  const promise1 = Promise.resolve(3);
  const promise2 = 42;
  const promise3 = new Promise((resolve, reject) => {
    setTimeout(resolve, 100, 'foo');
  });
  
  Promise.all([promise1, promise2, promise3]).then((values) => {
    console.log(values);
  }); // [3, 42, "foo"]
  ```

  More Promise methods: <https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise>

* fetch

  * we need to call response.json to get the real data from reponse. Response the response object from server. (including headers, ok, status, etc.)

  ```javascript
  fetch(url).then(response => response.json()).then(data => console.log(data));
  ```

  * Another short comming: [error handling](https://learnwithparam.com/blog/how-to-handle-fetch-errors/) (not like other ajax utility libraries)
    * It always gets a response, unless there is a **network error**(like wrong domain)
    * All 4xx, 5xx don’t get into catch block
    * We can rectify it by throwing error and allow only response which has status code between 200 and 299(see examples in the above link, also introduce the async/await syntax)
  * Still recommend using axios, superagent or jquery to make HTTP requests.

