# TS

## 一、TS简介、对比JS

TS（TypeScript）和 JS（JavaScript）有以下主要区别：

1. **类型系统**

   * JS

     ：是动态类型语言，变量类型在运行时确定，无需预先声明。

     ```javascript
     let num = 10;
     num = "hello"; // 合法，JS允许变量类型动态变化
     ```

   * TS

     ：是静态类型语言，需声明变量类型，类型错误会在编译时检查。

     ```typescript
     let num: number = 10;
     num = "hello"; // 报错，TypeScript不允许将字符串赋值给number类型
     ```

2. **编译与运行**

   * **JS**：直接解释执行，无需编译。
   * **TS**：需编译为 JS 才能运行，编译过程会检查类型错误。

3. **语法扩展**

   * TS

     ：提供接口（interface）、枚举（enum）、类（class）等面向对象特性。

     ```typescript
     interface Person {
       name: string;
       age: number;
     }
     
     const person: Person = { name: "Alice", age: 30 };
     ```

   * **JS**：需通过原型链或构造函数模拟类似功能。

4. **代码维护性**

   * **TS**：类型注解使代码更清晰，便于团队协作和大型项目维护。
   * **JS**：灵活性高，但复杂项目中可能导致类型相关的运行时错误。

5. **生态系统支持**

   * **TS**：主流框架（如 React、Vue）均支持 TS，大型项目中应用广泛。
   * **JS**：所有浏览器和 Node.js 直接支持，无需额外编译。

6. **错误检测**

   * **TS**：编译时发现类型错误，减少运行时崩溃。
   * **JS**：错误需在运行时才能发现。

7. **适用场景**

   * **TS**：适合大型、长期维护的项目，或需要强类型约束的场景。
   * **JS**：适合快速原型开发、小型项目或对灵活性要求高的场景。

总结：TS 通过类型系统增强了代码可靠性和可维护性，而 JS 则以灵活性和无需编译的特性见长。



## 学习TS

学习 TypeScript（TS）可以按照以下步骤系统进行，结合基础概念、实践项目和进阶技巧：

### **1. 前置知识准备**

确保熟悉 JavaScript（ES6+）基础：

* 变量声明（`let`/`const`）、箭头函数、解构赋值
* 类、继承、Promise、async/await
* 模块化（`import`/`export`）

### **2. 环境搭建**

安装 TypeScript 编译器：

```bash
npm install -g typescript
```

创建`tsconfig.json`配置文件：

```bash
npx tsc --init
```

### **3. 核心概念学习**

#### **3.1 基础类型**

```typescript
let num: number = 10;
let name: string = "Alice";
let isDone: boolean = false;
let anyValue: any = "可以是任意类型";
let arr: number[] = [1, 2, 3];
let tuple: [string, number] = ["hello", 100]; // 元组
```

#### **3.2 接口（Interface）**

定义对象结构：

```typescript
interface User {
  name: string;
  age?: number; // 可选属性
  readonly id: number; // 只读属性
}

const user: User = { name: "Bob", id: 1 };
```

#### **3.3 类（Class）**

支持面向对象编程：

```typescript
class Animal {
  protected name: string; // 受保护属性，子类可访问
  constructor(name: string) {
    this.name = name;
  }
  speak(): string {
    return `Hello, I'm ${this.name}`;
  }
}

class Dog extends Animal {
  bark() {
    return "Woof!";
  }
}
```

#### **3.4 函数类型**

```typescript
function add(a: number, b: number): number {
  return a + b;
}

// 可选参数
function greet(name?: string): string {
  return name ? `Hi ${name}` : "Hi";
}
```

#### **3.5 泛型（Generics）**

创建可复用的组件：

```typescript
function identity<T>(arg: T): T {
  return arg;
}

let output = identity<string>("myString"); // 指定类型
let output2 = identity(100); // 类型推断
```

#### **3.6 枚举（Enum）**

```typescript
enum Color {
  Red,
  Green,
  Blue,
}

let c: Color = Color.Green; // 值为1（默认从0开始）
```

### **4. 实践项目**

通过小项目巩固知识：

#### **4.1 简单工具函数库**

```typescript
// 数组去重函数
function unique<T>(arr: T[]): T[] {
  return [...new Set(arr)];
}

// 使用：
const numbers = [1, 2, 2, 3];
const uniqueNumbers = unique(numbers); // 类型自动推断为number[]
```

#### **4.2 前端组件（以 React 为例）**

```tsx
interface ButtonProps {
  text: string;
  onClick: () => void;
  disabled?: boolean;
}

const Button: React.FC<ButtonProps> = ({ text, onClick, disabled = false }) => {
  return (
    <button onClick={onClick} disabled={disabled}>
      {text}
    </button>
  );
};
```

### **5. 进阶技巧**

#### **5.1 类型守卫**

```typescript
function printValue(x: string | number) {
  if (typeof x === "string") {
    console.log(x.toUpperCase()); // 类型缩小为string
  } else {
    console.log(x.toFixed(2)); // 类型缩小为number
  }
}
```

#### **5.2 映射类型**

```typescript
interface User {
  name: string;
  age: number;
}

// 创建只读版本
type ReadonlyUser = Readonly<User>;
```

#### **5.3 装饰器（Decorators）**

```typescript
function log(target: any, propertyKey: string, descriptor: PropertyDescriptor) {
  const originalMethod = descriptor.value;
  descriptor.value = function (...args: any[]) {
    console.log(`Calling ${propertyKey} with args: ${JSON.stringify(args)}`);
    return originalMethod.apply(this, args);
  };
}

class Calculator {
  @log
  add(a: number, b: number) {
    return a + b;
  }
}
```

### **6. 学习资源推荐**

#### **官方文档**

* [TypeScript 官方文档](https://www.typescriptlang.org/docs/)
* [TypeScript 入门教程](https://www.typescriptlang.org/docs/handbook/typescript-in-5-minutes.html)

#### **实战项目**

* 用 TS 重构现有 JS 项目
* 参与开源 TS 项目（如 Discord.js、Vue3）

### **7. 常见错误处理**

* **类型不匹配**：检查变量类型和函数返回值类型。
* **未定义类型**：使用`unknown`代替`any`，避免类型丢失。
* **类型断言**：谨慎使用`as`，优先通过类型守卫解决问题。