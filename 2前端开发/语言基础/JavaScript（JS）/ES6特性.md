ES6（ECMAScript 2015）是 JavaScript 语言的一次重大更新，引入了许多新的语法特性和功能。以下是一些 ES6 的核心特性及其示

ES6 引入的这些特性大大提升了 JavaScript 的可读性和开发效率，后续版本（ES2016+）在此基础上继续扩展了更多功能（如 `async/await`、`BigInt` 等）。现代项目中，ES6+ 语法已成为主流。

### **1. 箭头函数 (`=>`)**

更简洁的函数语法，且不绑定自己的 `this`。

```javascript
// 传统函数
function sum(a, b) {
  return a + b;
}

// 箭头函数
const sum = (a, b) => a + b;

// 无参数或多个参数需用括号
const greet = () => 'Hello!';
const multiply = (a, b) => a * b;

// 函数体有多行时用大括号，并显式 return
const getFullName = (first, last) => {
  return `${first} ${last}`;
};
```

### **2. 模板字符串 (`${}`)**

简化字符串拼接，支持换行。

```javascript
const name = 'Alice';
const age = 30;

// 传统字符串拼接
const message = 'Hello, ' + name + '! You are ' + age + ' years old.';

// 模板字符串
const message = `Hello, ${name}! You are ${age} years old.`;

// 支持换行
const html = `
  <div>
    <h1>${name}</h1>
  </div>
`;
```

### **3. 解构赋值**

从数组或对象中提取值并赋值给变量。

```javascript
// 数组解构
const numbers = [1, 2, 3];
const [a, b, c] = numbers; // a=1, b=2, c=3

// 对象解构
const person = { name: 'Bob', age: 25 };
const { name, age } = person; // name='Bob', age=25

// 重命名变量
const { name: fullName, age: years } = person; // fullName='Bob', years=25

// 默认值
const { city = 'Unknown' } = person; // city='Unknown'
```

### **4. 扩展运算符 (`...`)**

用于展开数组或对象，或收集剩余参数。

```javascript
// 数组展开
const arr1 = [1, 2];
const arr2 = [...arr1, 3, 4]; // [1, 2, 3, 4]

// 对象展开
const obj1 = { a: 1, b: 2 };
const obj2 = { ...obj1, c: 3 }; // { a: 1, b: 2, c: 3 }

// 剩余参数
const sum = (a, b, ...rest) => {
  return a + b + rest.reduce((acc, num) => acc + num, 0);
};
sum(1, 2, 3, 4); // 10 (1+2+3+4)
```

### **5. `let` 和 `const`**

块级作用域的变量声明。

```javascript
// let 允许重新赋值，但不允许重复声明
let x = 10;
x = 20; // 合法

// const 必须初始化，且不能重新赋值（但对象属性可修改）
const PI = 3.14;
// PI = 3; // 报错

const person = { name: 'Alice' };
person.name = 'Bob'; // 合法（修改对象属性）
// person = {}; // 报错（重新赋值）
```

### **6. 类 (`class`)**

基于原型的继承的语法糖。

```javascript
class Animal {
  constructor(name) {
    this.name = name;
  }

  speak() {
    console.log(`${this.name} makes a sound.`);
  }
}

class Dog extends Animal {
  constructor(name) {
    super(name); // 调用父类构造函数
  }

  speak() {
    console.log(`${this.name} barks.`);
  }
}

const dog = new Dog('Buddy');
dog.speak(); // 输出: "Buddy barks."
```

### **7. Promise**

处理异步操作的对象，避免回调地狱。

```javascript
function fetchData() {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve('Data fetched successfully!');
      // reject(new Error('Failed to fetch data!'));
    }, 1000);
  });
}

// 使用 Promise
fetchData()
  .then(data => console.log(data))
  .catch(error => console.error(error));

// 更简洁的 async/await 语法（ES2017）
async function getData() {
  try {
    const data = await fetchData();
    console.log(data);
  } catch (error) {
    console.error(error);
  }
}
```

### **8. 默认参数值**

为函数参数提供默认值。

```javascript
function greet(name = 'Guest') {
  return `Hello, ${name}!`;
}

greet(); // "Hello, Guest!"
greet('Alice'); // "Hello, Alice!"
```

### **9. 模块 (`import`/`export`)**

支持文件间的模块化导入导出。

```javascript
// math.js（导出）
export const add = (a, b) => a + b;
export const subtract = (a, b) => a - b;

// main.js（导入）
import { add, subtract } from './math.js';
console.log(add(1, 2)); // 3

// 或导入全部作为一个对象
import * as math from './math.js';
console.log(math.subtract(5, 3)); // 2
```

### **10. 其他特性**

* **for...of 循环**：遍历可迭代对象（如数组、字符串）。

  ```javascript
  const arr = [1, 2, 3];
  for (const num of arr) {
    console.log(num);
  }
  ```

* **对象方法简写**：

  ```javascript
  const person = {
    name: 'Alice',
    sayHi() { // 等同于 sayHi: function() {...}
      console.log(`Hi, ${this.name}!`);
    }
  };
  ```

* **Promise.all**：并行处理多个 Promise。

  ```javascript
  Promise.all([promise1, promise2])
    .then(results => console.log(results))
    .catch(error => console.error(error));
  ```

  