# JavaScript 基础教程

> 适合零基础学习者。课件按照“基础语法 → 流程控制 → 数组与函数 → 对象 → DOM → BOM → 本地存储”的顺序编排。
>
> 除涉及网页操作的示例外，大部分 JavaScript 代码都可以直接在浏览器控制台中运行。按 `F12`，打开“控制台（Console）”即可练习。

---

## 1. JavaScript 简介   

JavaScript 最初由网景公司开发，是一种广泛应用于网页的脚本语言。它可以响应用户操作、修改页面内容、处理表单数据，让静态网页具备动态交互能力。

目前，几乎所有现代浏览器都内置了 JavaScript 引擎。JavaScript 不仅可以在浏览器中运行，也可以通过 Node.js 在服务器端运行。本课件主要学习浏览器中的 JavaScript。

### 1.1 JavaScript 能做什么

- 点击按钮后显示或隐藏内容。
- 检查用户输入是否符合要求。
- 动态修改页面文字、图片和样式。
- 制作轮播图、倒计时、随机点名等交互功能。
- 向服务器请求数据并更新页面。

---

## 2. JavaScript 的组成部分

浏览器中的 JavaScript 主要由三部分组成。

| 组成部分 | 作用 |
| --- | --- |
| ECMAScript | JavaScript 的核心语法，规定变量、数据类型、运算符、函数、对象等基本规则 |
| DOM（Document Object Model） | 文档对象模型，用于访问和修改 HTML 文档中的节点 |
| BOM（Browser Object Model） | 浏览器对象模型，用于操作浏览器窗口、地址、历史记录、定时器等 |

简单来说：

- ECMAScript 解决“代码怎么写”的问题。
- DOM 解决“怎么操作网页内容”的问题。
- BOM 解决“怎么操作浏览器”的问题。

---

## 3. JavaScript 的特点

### 3.1 解释性

JavaScript 源代码由浏览器中的 JavaScript 引擎执行，开发者通常不需要像使用传统编译型语言那样手动完成编译步骤。

### 3.2 动态性

JavaScript 可以响应点击、输入、鼠标移动等事件，不需要重新向服务器请求完整页面，就能改变网页内容。

### 3.3 跨平台性

JavaScript 主要依赖浏览器运行。只要浏览器支持相应的 JavaScript 标准，同一份代码通常就能在不同操作系统中运行。

### 3.4 基于对象

JavaScript 提供了字符串、数组、日期等内置对象，也允许开发者创建自定义对象。对象把相关数据和操作组织在一起，方便完成具体功能。

---

## 4. JavaScript 的使用方式

### 4.1 行内式

行内式是把 JavaScript 代码直接写在 HTML 标签中。

```html
<button type="button" onclick="alert('Hello JavaScript')">点一下</button>
```

也可以通过 `javascript:` 协议执行简单代码：

```html
<a href="javascript:alert('Hello JavaScript')">点一下试试</a>
```

行内式将 HTML 与 JavaScript 混在一起，不利于维护，实际项目中尽量少用。

### 4.2 内嵌式

内嵌式是把 JavaScript 代码写在 `<script>` 标签中。

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <title>内嵌式 JavaScript</title>
</head>
<body>
  <script>
    alert('Hello JavaScript');
  </script>
</body>
</html>
```

`<script>` 可以放在 `<head>` 或 `<body>` 中。如果代码需要操作页面元素，应确保元素已经加载，可以把脚本放在 `</body>` 前，或者使用窗口加载事件。

### 4.3 外链式

外链式是把 JavaScript 写在独立的 `.js` 文件中，再通过 `src` 属性引入。

`index.html`：

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <title>外链式 JavaScript</title>
</head>
<body>
  <script src="./my.js"></script>
</body>
</html>
```

`my.js`：

```javascript
alert('Hello JavaScript');
```

外链式可以让 HTML 与 JavaScript 分离，便于代码复用和维护，是实际开发中常用的方式。

---

## 5. JavaScript 中的输入和输出

### 5.1 输入：prompt()

`prompt()` 用于弹出输入框，返回用户输入的字符串。如果用户点击“取消”，则返回 `null`。

```javascript
const age = window.prompt('请输入您的年龄');
console.log(age);
```

即使用户输入的是数字，**`prompt()` 返回的仍然是字符串**，需要时可转换为数字。

### 5.2 输出：alert()

`alert()` 用于弹出警告框。

```javascript
window.alert('Hello JavaScript');
```

### 5.3 输出：console.log()

`console.log()` 用于在浏览器控制台输出信息，是学习和调试时最常用的输出方式。

```javascript
console.log('Hello JavaScript');
console.log(10 + 20);
```

### 5.4 输出：document.write()

`document.write()` 可以向文档输出内容。

```javascript
document.write('Hello JavaScript');
```

页面加载完成后调用 `document.write()` 可能覆盖原有文档，因此它主要用于早期教学和简单演示，实际开发中通常使用 DOM 方法更新页面。

5.5字符串格式化输出

```js
num=123
alert(`较大的数字是${num}`)  //这种一定要用反引号
alert('较大的数字是'+num)  //可以直接用加号链接
```



---

## 6. JavaScript 注释

注释不会作为程序逻辑执行，主要用于说明代码。

### 6.1 单行注释

```javascript
// 声明一个变量，保存用户年龄
const age = 18;
```

### 6.2 多行注释

```javascript
/*
  以下代码用于计算两个数字的和：
  1. 声明两个数字
  2. 计算结果
  3. 输出结果
*/
const num1 = 10;
const num2 = 20;
console.log(num1 + num2);
```

---

## 7. JavaScript 变量

### 7.1 什么是变量

变量可以理解为存放数据的容器。从计算机角度看，变量对应内存中的一段存储空间，程序通过变量名读取或修改其中的数据。

### 7.2 声明和赋值

参考课件中的早期语法使用 `var` 声明变量：

```javascript
var age;
age = 18;
```

也可以在声明时直接赋值：

```javascript
var age = 18;
```

现代 JavaScript 还提供了 `let` 和 `const`：ES6语法

```javascript
let score = 90;       // 后续可以重新赋值,但不能重新声明
const pi = 3.14159;   // 常量，不能重新赋值,,初始化的时候必须赋值 
```

实际开发中通常优先使用 `const`；确定需要重新赋值时使用 `let`。学习作用域和变量提升时，仍需理解 `var` 的行为。

### 7.3 变量命名规则

1. 变量名可以由字母、数字、下划线 `_` 和美元符号 `$` 组成。
2. 不能以数字开头。
3. 不能使用 `var`、`for`、`while` 等关键字或保留字。
4. 严格区分大小写，`age` 与 `Age` 是两个不同的变量。
5. 变量名应见名知意，推荐使用小驼峰命名法，例如 `userName`。

```javascript
// 合法
let userName = '张三';
let age1 = 18;
let $price = 99;
let _count = 2;

// 不合法：不能以数字开头，也不能使用关键字
// let 1age = 18;
// let for = 10;
```

### 7.4 案例：输出个人信息

```javascript
const name = '张三';
const gender = '男';
const age = 20;
const education = '本科';
const major = '计算机科学与技术';
const email = 'zhangsan@example.com';
const salary = 8000;
const phone = '13800000000';

console.log(name, gender, age, education, major, email, salary, phone);
```

### 7.5 案例：交换两个变量的值 

使用临时变量：

```javascript
let a = 10;
let b = 20;
let temp = a;

a = b;
b = temp;

console.log(a, b); // 20 10
```

---

## 8. JavaScript 数据类型

JavaScript 的数据类型分为**原始类型（简单类型）**和**引用类型（复杂类型）**。原始类型共有 7 种：`number`、`string`、`boolean`、`undefined`、`null`、`bigint`、`symbol`。引用类型主要是 `object`，数组、函数和日期等也属于对象。

### 8.1 数据类型分类

| 分类 | 数据类型 | 主要用途 |
| --- | --- | --- |
| 原始类型 | `number` | 表示整数、小数以及特殊数值 |
| 原始类型 | `string` | 表示文本 |
| 原始类型 | `boolean` | 表示真或假 |
| 原始类型 | `undefined` | 表示变量已声明但没有赋值 |
| 原始类型 | `null` | 表示开发者主动设置的空值 |
| 原始类型 | `bigint` | 表示超出 `number` 安全整数范围的大整数 |
| 原始类型 | `symbol` | 创建独一无二的标识符 |
| 引用类型 | `object` | 保存一组相关数据或功能，包括普通对象、数组、函数、日期等 |

### 8.2 原始类型

#### 1. number：数值型

`number` 可以表示整数和小数，常用于年龄、价格、数量、分数等数据。

```javascript
const age = 18;
const price = 9.9;

console.log(age);          // 18
console.log(price);        // 9.9
console.log(typeof age);   // number
console.log(typeof price); // number
```

`number` 还包含几个特殊数值：

```javascript
console.log(1 / 0);          // Infinity，表示无穷大
console.log(-1 / 0);         // -Infinity，表示负无穷大
console.log(Number('abc'));  // NaN，表示无法得到有效数字
console.log(typeof NaN);     // number，NaN 也属于数值型,not a number
```

#### 2. string：字符串型

字符串用于表示姓名、地址、提示文字等文本，可以使用单引号、双引号或反引号包裹。

```javascript
const name = '张三';
const message = "欢迎来到 JavaScript 课堂";
const greeting = `你好，${name}`;

console.log(message);          // 欢迎来到 JavaScript 课堂
console.log(greeting);         // 你好，张三
console.log(typeof name);      // string
```

#### 3. boolean：布尔型

布尔型只有 `true` 和 `false` 两个值，常用于表示“是否登录”“是否选中”以及条件判断的结果。

```javascript
const isLogin = true;
const isAdult = 18 >= 18;

console.log(isLogin);        // true
console.log(isAdult);        // true
console.log(typeof isLogin); // boolean
```

#### 4. undefined：未定义

变量声明后没有赋值时，默认值通常是 `undefined`，表示“目前还没有值”。

```javascript
let username;

console.log(username);        // undefined
console.log(typeof username); // undefined
```

#### 5. null：空值

`null` 表示开发者主动设置的空值，常用于表达“当前没有选中的对象”或“暂时没有数据”。

```javascript
let selectedUser = null;

console.log(selectedUser);        // null
console.log(typeof selectedUser); // object，这是 JavaScript 的历史遗留行为 
```

`undefined` 与 `null` 的区别：

- `undefined`：通常表示变量已经声明，但还没有赋值。
- `null`：通常表示开发者明确地把变量设置为空。

#### 6. bigint：大整数型

`number` 能安全表示的整数范围有限。需要表示超出安全整数范围的整数时，可以使用 `bigint`，在整数末尾添加 `n`。

```javascript
const bigId = 9007199254740993n;
const result = bigId + 1n;

console.log(result);        // 9007199254740994n最后一位加1
console.log(typeof bigId);  // bigint
```

`bigint` 不能直接与 `number` 混合运算，需要先把它们转换为同一种类型：

```javascript
// console.log(1n + 1);   // 报错：不能混合 BigInt 和其他类型
console.log(1n + BigInt(1)); // 2n
```

#### 7. symbol：唯一标识符

`symbol` 用于创建独一无二的值。即使两个 `Symbol` 的描述完全相同，它们也不相等。

```javascript
const id1 = Symbol('userId');
const id2 = Symbol('userId');

console.log(id1 === id2); // false
console.log(typeof id1);  // symbol
```

### 8.3 引用类型

**引用类型**：变量里**不存真正的数据本身**，存的是一个「地址（引用）」，这个地址指向堆内存里的对象。

引用类型可以保存更复杂的数据结构。普通对象、数组、函数和日期对象都属于这一大类。

#### 1. 普通对象(Py的字典)

对象使用“键: 值”的形式保存一组相关数据，适合描述学生、商品、用户等具体事物。

```javascript
const student = {
  name: '张三',
  age: 20,
  major: '计算机科学与技术'
};

console.log(student.name); // 张三
console.log(student.age);  // 20
console.log(student[1]);  //undefined,对象通过键取值,没有下标
console.log(typeof student); // object
console.log(Array.isArray(student));//false,不是数组
```

#### 2. 数组array(Py的列表)

数组使用下标保存一组有序数据，适合保存成绩、姓名列表、商品列表等。

```javascript
const scores = [80, 90, 100];

console.log(scores[0]);       // 80
console.log(scores.length);   // 3
console.log(typeof scores);   // object
console.log(Array.isArray(scores)); // true
```

#### 3. 函数

函数是一段可以被调用的代码，也是一种特殊的对象。

```javascript
function sayHello() {  //function加函数名
  let coruse = window.prompt('请输入课程名称');
  console.log('你好',coruse);  //功能代码
}

sayHello();               // 你好，JavaScript
console.log(typeof sayHello); // function
```



#### 4. 日期对象

日期对象用于保存和处理日期、时间。

```javascript
const now = new Date();

console.log(now);            // 当前日期和时间
console.log(now.getFullYear()); // 当前年份
console.log(typeof now);     // object
```



#### 5.复杂的对象

```js
//复杂的对象
let student = {
   name: '张三',
   age: 20,
   major: '计算机科学与技术',
   scores:[80,90,100],
   a:{
    b:300,
    c:{
      d:40
    }
   },
   sayHello:function(){
    console.log('你好，我是student下面的js');
   }
}
student.sayHello();

console.log(student.scores[2]);  //100
console.log(student.a.b);  //300
```

```apl
这段代码是 JavaScript 里非常典型的“复杂对象（Nested Object）”，它把真实世界中一个“学生”具备的静态数据和动态行为全部打包在了一个变量里。

你可以把这个 student 对象想象成一个带有很多抽屉的档案柜。我们把它一层一层拆开看：

1. 静态数据（属性：Property）
这些是描述学生的基本特征，通过键值对（Key: Value）存储。
基础数据类型：
name: '张三' （字符串）
age: 20 （数字）
major: '计算机科学与技术'

数组类型：
scores: [80, 90, 100] （这个学生的三门课成绩）

嵌套对象（对象里套对象）：
a: { b: 300, c: { d: 40 } }

这是一种嵌套结构，就像档案柜里的“A抽屉”里有个“C文件夹”，里面还有一张写着 40 的纸。如果你想拿到这个 40，你得这么写：student.a.c.d。

2. 动态行为（方法：Method）
sayHello: function() { ... }

当对象的属性值是一个函数时，我们就不再叫它“属性”了，而是叫它“方法（Method）”。

这代表这个学生不仅能存数据，自己还会“说话”。

3. 最后一句调用：student.sayHello();
这句话的意思是：“让 student 这个对象，执行它自己身上的 sayHello 方法。”

浏览器执行这句代码时，会在控制台打印出：'你好，我是student下面的js'。
```



### 8.4 动态数据类型

JavaScript 变量本身不固定为某一种类型，变量保存什么值，就可以通过该值判断当前类型。

```javascript
let value = '张三';
console.log(typeof value); // string

value = 25;
console.log(typeof value); // number

value = true;
console.log(typeof value); // boolean
```

### 8.5 使用 typeof 判断类型

`typeof` 会返回一个表示数据类型的字符串。

| 表达式 | 返回结果 |
| --- | --- |
| `typeof 20` | `'number'` |
| `typeof '张三'` | `'string'` |
| `typeof true` | `'boolean'` |
| `typeof undefined` | `'undefined'` |
| `typeof null` | `'object'`，历史遗留行为 |
| `typeof 10n` | `'bigint'` |
| `typeof Symbol()` | `'symbol'` |
| `typeof { name: '张三' }` | `'object'` |
| `typeof [80, 90]` | `'object'` |
| `typeof new Date()` | `'object'` |
| `typeof function () {}` | `'function'` |

```javascript
console.log(typeof 20);            // number
console.log(typeof '张三');        // string
console.log(typeof true);          // boolean
console.log(typeof undefined);     // undefined
console.log(typeof null);          // object
console.log(typeof 10n);           // bigint
console.log(typeof Symbol());      // symbol
console.log(typeof {});            // object
console.log(typeof []);            // object
console.log(typeof function () {});// function
```

`typeof` 不能区分数组、普通对象和日期对象。判断数组时，可以使用 `Array.isArray()`：

```javascript
console.log(Array.isArray([80, 90])); // true
console.log(Array.isArray({ name: '张三' })); // false
```

### 8.6 数据类型转换

#### 1. 其他类型转换为字符串

```javascript
const num = 10;
const flag = true;

console.log(num.toString());  // '10'
console.log(String(num));     // '10'
console.log(num + '');        // '10'，利用 + 进行隐式转换
console.log(flag.toString()); // 'true'
```

#### 2. 其他类型转换为数字

`parseInt()` 用于提取整数，不会四舍五入：

```javascript
console.log(parseInt('20px')); // 20
console.log(parseInt('12.8')); // 12
```

`parseFloat()` 用于提取小数：

```javascript
console.log(parseFloat('12.5px')); // 12.5
```

`Number()` 用于完整转换：

```javascript
const price = '120';
const result = Number(price);

console.log(result);        // 120
console.log(typeof result); // number
```

也可以使用 `-`、`*`、`/` 触发隐式数值转换：

```javascript
const num = '10';
const result = num - 0;

console.log(result);        // 10
console.log(typeof result); // number
```

不能转换为有效数字时，结果是 `NaN`：

```javascript
console.log(Number('abc')); // NaN
```



#### 3. 其他类型转换为布尔值

以下值转换为布尔值时是 `false`，称为假值：

```javascript
console.log(Boolean(false));     // false
console.log(Boolean(''));        // false，空字符串
console.log(Boolean(0));         // false
console.log(Boolean(0n));        // false
console.log(Boolean(NaN));       // false
console.log(Boolean(null));      // false
console.log(Boolean(undefined)); // false
```

除假值外，其他值通常转换为 `true`：

```javascript
console.log(Boolean('hello')); // true，非空字符串
console.log(Boolean(1));       // true，非零数字
console.log(Boolean([]));      // true，空数组也是真值
console.log(Boolean({}));      // true，空对象也是真值
```

---

## 9. JavaScript 运算符

### 9.1 算术运算符

| 运算符 | 描述 |
| --- | --- |
| `+` | 加法 |
| `-` | 减法 |
| `*` | 乘法 |
| `/` | 除法 |
| `%` | 取余，即求除法后的余数 |
| `++` | 自增 1 |
| `--` | 自减 1 |

```javascript
const x = 7;
const y = 8;
console.log(x + y); // 15

console.log(3 / 2); // 1.5
console.log(3 % 2); // 1
```

#### 前置自增与后置自增

后置 `++`：先把原值参与当前表达式，再让变量自身加 1。

```javascript
let a = 1;
const b = a++;

console.log(a); // 2
console.log(b); // 1
```

前置 `++`：先让变量自身加 1，再把新值参与当前表达式。

```javascript
let a = 1;
const b = ++a;

console.log(a); // 2
console.log(b); // 2
```

`--` 的规则与 `++` 相同，只是改为减 1。

### 9.2 赋值运算符

| 运算符 | 示例 | 等同于 |
| --- | --- | --- |
| `=` | `x = y` | `x = y` |
| `+=` | `x += y` | `x = x + y` |
| `-=` | `x -= y` | `x = x - y` |
| `*=` | `x *= y` | `x = x * y` |
| `/=` | `x /= y` | `x = x / y` |
| `%=` | `x %= y` | `x = x % y` |

```javascript
let x = 2;
let y = 3;

x += 2;
y -= 1;

console.log(x); // 4
console.log(y); // 2
```

### 9.3 比较运算符

| 运算符 | 描述 |
| --- | --- |
| `==` | 值相等即可，比如1='1' 允许必要的类型转换 |
| `===` | 严格相等，值和类型都必须相同 |
| `!=` | 不相等 |
| `!==` | 严格不相等 |
| `>` | 大于 |
| `<` | 小于 |
| `>=` | 大于或等于 |
| `<=` | 小于或等于 |

```javascript
const a = 1;
const b = '1';

console.log(a == b);  // true
console.log(a === b); // false
console.log(a != b);  // false
console.log(a !== b); // true
```

实际开发中建议优先使用 `===` 和 `!==`，避免隐式类型转换造成意外结果。

#### 三元运算符

语法：`条件表达式 ? 条件为真时的值 : 条件为假时的值`

```javascript
const a = 1;
const b = 2;
const result = a > b ? 'a 大于 b' : 'a 小于或等于 b';

console.log(result);
```

### 9.4 逻辑运算符

| 运算符 | 描述 |
| --- | --- |
| `&&` | 逻辑与，两个条件都为真时才为真 |
| `||` | 逻辑或，至少一个条件为真时就为真 |
| `!` | 逻辑非，对布尔值取反 |

```javascript
const b = true;
const c = false;

console.log(b && c); // false
console.log(b || c); // true
console.log(!b);     // false
```

### 9.5 类型运算符

| 运算符 | 描述 |
| --- | --- |
| `typeof` | 返回一个表示值类型的字符串 |
| `instanceof` | 判断对象是否为某个构造函数的实例 |

```javascript
console.log(typeof 123); // number

let arr = [1, 2, 3];
let obj = { name: '张三' };
let date = new Date();

console.log(arr instanceof Array);   // true （arr 是不是数组老爸生的？是的）
console.log(arr instanceof Object);  // true （在 JS 里，数组其实也是对象的一种）
console.log(obj instanceof Array);   // false （普通对象不是数组生的）
console.log(date instanceof Date);   // true （日期对象确实是 Date 生的）
```

## 10. JavaScript 流程控制

流程控制用于规定代码按照什么顺序执行，主要包括顺序结构、分支结构和循环结构。

### 10.1 顺序结构

代码自上而下依次执行。

```javascript
console.log('第一步');
console.log('第二步');
console.log('第三步');
```

### 10.2 分支结构

分支结构根据条件选择不同代码路径。

#### 1. if 单分支

```javascript
const age = 20;

if (age >= 18) {
  console.log('已经成年');
}
```

#### 2. if...else 双分支

```javascript
const age = 16;

if (age >= 18) {
  console.log('已经成年');
} else {
  console.log('尚未成年');
}
```

#### 3. if...else if...else 多分支

```javascript
const score = 85;

if (score >= 90) {
  console.log('优秀');
} else if (score >= 80) {
  console.log('良好');
} else if (score >= 60) {
  console.log('及格');
} else {
  console.log('不及格');
}
```

#### 4. switch 分支

适合根据同一个表达式的多个明确取值执行不同代码。

```javascript
const day = 3;

switch (day) {
  case 1:
    console.log('星期一：学习变量');
    break;
  case 2:
    console.log('星期二：学习数据类型');
    break;
  case 3:
    console.log('星期三：学习运算符');
    break;
  default:
    console.log('复习本周内容');
}
```

注意事项：

1. `switch` 中的表达式通常是一个变量。
2. 表达式与 `case` 的值使用严格相等 `===` 进行匹配。
3. 当前 `case` 没有 `break` 时，会继续执行后面的 `case`，这种现象称为“case 穿透”。
4. `default` 用于处理所有 `case` 都没有匹配的情况。

### 10.3 循环结构

循环用于重复执行某段代码。例如，打印 100 次 `hello`，不需要手写 100 行输出语句。

#### 1. for 循环

`for` 循环通常用于已知循环次数的场景。

```javascript
for (let i = 1; i <= 100; i++) {//起始变量,循环条件,变量修改
  console.log('hello');
}
```

语法：

```javascript
for (初始化变量; 条件表达式; 操作表达式) {
  // 需要重复执行的代码
}
```

执行过程：

1. 执行初始化变量，只执行一次。
2. 判断条件表达式。
3. 条件为真，执行循环体。
4. 执行操作表达式，更新计数器。
5. 再次判断条件；条件为假时结束循环。

#### 2. 断点调试

在 Chrome 等浏览器中可以按以下步骤调试：

1. 按 `F12` 打开开发者工具。

2. 进入 `Sources`（源代码）面板。

3. 找到需要暂停的 JavaScript 代码行，点击行号设置断点。

4. 刷新页面，让代码执行到断点位置。

5. 使用调试按钮观察执行过程：
   - `Step over`：执行当前行，不进入函数内部。
   
   - `Step into`：进入当前行调用的函数内部。
   
   - `Step out`：跳出当前函数。
   
     

#### 3. while 循环

`while` 循环通常用于不确定具体循环次数，但知道结束条件的场景。

```javascript
let i = 1;

while (i <= 5) {  //循环条件
  console.log(i);
  i++;
}
```

#### 4. do...while 循环

`do...while` 会先执行一次循环体，再判断条件，因此循环体至少执行一次。

```javascript
let i = 1;

do {
  console.log(i);
  i++;
} while (i <= 5);
```

#### 5. break：终止循环

`break` 用于结束离它最近的整个循环。

```javascript
for (let i = 0; i < 10; i++) {
  if (i === 3) {
    break;
  }
  console.log(i);
}
```

输出：`0、1、2`。

在嵌套循环中，`break` 只结束当前所在的那一层循环：

```javascript
for (let j = 0; j < 6; j++) {
  for (let i = 0; i < 5; i++) {
    if (i === 3) {
      break;
    }
    console.log(`j=${j}, i=${i}`);
  }
}
```

#### 6. continue：跳过本次循环

`continue` 用于结束当前这一次循环，直接进入下一次循环。

```javascript
for (let i = 0; i < 5; i++) {
  if (i === 3) {
    continue;
  }
  console.log(i);
}
```

输出：`0、1、2、4`。

### 10.4 分支结构练习

1. 用户输入一个数字，判断它是奇数还是偶数；如果输入的不是有效数字，也要给出提示。
2. 用户输入两个数字，输出较大的数字。
3. 根据分数输出成绩等级。
4. 用户输入一个年份，判断该年份有多少天。
5. 制定学习计划表，输入星期几，提示当天要学习的课程。
6. 出租车起步价为 7 元（2 公里以内），超过 2 公里的部分按每公里 3 元计算，根据路程计算费用。
7. 判断闰年：能被 4 整除且不能被 100 整除，或者能被 400 整除。
8. 根据性别和体重计算输血量：女性体重不超过 50 kg 时为 200 ml，否则为 250 ml；男性不超过 60 kg 时为 250 ml，否则为 300 ml。
9. 气温高于 26℃ 时开启制冷，低于 10℃ 时开启制热，其余情况开启送风模式。
10. 居民用电按三个梯度收费：150 千瓦时及以下部分每千瓦时 0.4463 元；151～400 千瓦时部分每千瓦时 0.4663 元；401 千瓦时以上部分每千瓦时 0.5663 元。输入用电量，计算总费用。
11. A 地到 B 地的硬座和硬卧价格分别为 100 元和 190 元。未成年人身高不足 120 cm 免票；120 cm（含）～150 cm（不含）半票；150 cm 及以上全票。未成年人购买卧铺时，只减免硬座票价的一半。输入年龄、身高和票的类型，输出票价。
12. 输出两个数字中的最大值。
13. 输入一元二次方程的系数，输出方程的实数解；没有实数解时给出提示。

### 10.5 循环结构练习

1. 分别计算 100 以内所有偶数与奇数的和。
2. 输出九九乘法表。
3. 计算 100 以内所有能被 3 整除的数字之和。
4. 计算 1～20 的平均值。
5. 甲、乙、丙、丁共加工 370 个零件。如果甲多加工 10 个、乙少加工 20 个、丙的数量乘以 2、丁的数量除以 2，四人的数量相等。求四人各自加工多少个零件。
6. 鸡兔同笼，共 50 只、160 只脚，求鸡和兔各有多少只。
7. 输出 100 以内的素数。素数是只能被 1 和自身整除且大于 1 的整数。
8. 分别统计 20 以内偶数和奇数的个数。
9. 输出 1000 以内的水仙花数。三位数 `abc` 如果满足 `a³ + b³ + c³ = abc`，就是水仙花数。

---

## 11. JavaScript 数组

Python的列表

数组可以使用一个变量保存多个值，并通过下标访问每个值。数组下标从 `0` 开始。

```javascript
const cars = ['Saab', 'Volvo', 'BMW'];

console.log(cars[0]); // Saab
console.log(cars[1]); // Volvo
```

### 11.1 创建数组

#### 1. 先创建空数组，再添加元素

```javascript
const cars = [];
cars[0] = 'Saab';
cars[1] = 'Volvo';
cars[2] = 'BMW';
```

早期写法也可以使用 `new Array()`：

```javascript
const cars = new Array();
cars[0] = 'Saab';
cars[1] = 'Volvo';
cars[2] = 'BMW';
```

#### 2. 创建指定长度的数组

```javascript
const list = new Array(3);
console.log(list.length); // 3
```

这里的 `3` 是数组长度，不是数组可容纳元素的上限。此时数组中有 3 个空位，并不等于 `[undefined, undefined, undefined]`。

#### 3. 创建数组并赋值

```javascript
const cars = new Array('Saab', 'Volvo', 'BMW');
```

#### 4. 数组字面量

```javascript
const cars = ['Saab', 'Volvo', 'BMW'];
```

字面量写法更简洁，也最常用。

### 11.2 访问和修改数组元素

```javascript
const cars = ['Saab', 'Volvo', 'BMW'];

const firstCar = cars[0];
console.log(firstCar); // Saab

cars[0] = 'Opel';  //修改数据
console.log(cars); // ['Opel', 'Volvo', 'BMW']
```

### 11.3 遍历数组

四种方法

```javascript
//1. 老大哥：经典 for 循环
const array = [23, 12, 5, 19, 3];
for (let i = 0; i < array.length; i++) {
  console.log(array[i]);
}

//优点：控制力极强。你可以倒着遍历（i--），可以跳着遍历（i += 2），随时用 break 退出整个循环，用 continue 跳过本次循环。
//缺点：代码写起来比较繁琐，可读性一般。
```

```js
//2. 优雅姐：forEach
const array = [23, 12, 5, 19, 3];
array.forEach((item, index) => {
    console.log(item, index);
});

//优点：非常优雅，可以直接同时拿到“值（item）”和“下标（index）一定是这个位置”。
//缺点：无法中途停下。你在里面写 break 或 return 都是无效的，它一旦启动，不把数组跑完绝不罢休。
```

```js
//3. 新星子：for...of（ES6 引入）
const array = [23, 12, 5, 19, 3];
for (var item of array) {   // for of打印的是每一项
    console.log(item); 
}

//优点：代码极简，直接拿值。而且它非常灵活，支持 break（终止循环）和 continue（跳过本次）。除了数组，它还能遍历字符串、Map、Set 等。

//缺点：不能直接拿到下标（如果非要下标，得写成 array.entries()）。
```

```js
//4. 坑货弟：for...in
const array = [23, 12, 5, 19, 3];
for (var i in array) { //for in 打印的是每一项
     console.log(array[i]); 
}
//尽量别用,本来是用来遍历对象的键key的,不建议用来遍历数组
```



### 11.4 添加、删除、截取、合并和拷贝

| 方法 | 作用 | 返回值 | 是否修改原数组 |
| --- | --- | --- | --- |
| `push()` | 在末尾添加一个或多个元素 | 新长度 | 是 |
| `unshift()` | 在开头添加一个或多个元素 | 新长度 | 是 |
| `pop()` | 删除最后一个元素 | 被删除的元素 | 是 |
| `shift()` | 删除第一个元素 | 被删除的元素 | 是 |
| `splice()` | 在指定位置删除或插入元素 | 被删除元素组成的新数组 | 是 |
| `slice()` | 截取数组的一部分 | 截取出的新数组 | 否 |
| `concat()` | 合并数组或值 | 合并后的新数组 | 否 |
| `includes()` | 判断是否存在元素 |  |  |

#### 1. push()：末尾添加

```js
// push()：末尾添加，会改原数组
const array = [1, 2, 3, 4, 5];
//array.push(12) 在执行完“把 12 塞进数组末尾”的动作后，会把这个新数组的总长度（也就是 6）作为返回值吐出来。所以用length接收,后面遇到的同理
const length = array.push(12);

console.log(array);  // [1, 2, 3, 4, 5, 12]
console.log(length); // 6
```

#### 2. unshift()：开头添加

```javascript
// unshift()：开头添加，会改原数组
const array = [1, 2, 3];
const length = array.unshift(0);

console.log(array);  // [0, 1, 2, 3]
console.log(length); // 4
```

#### 3. pop()：删除最后一项

```javascript
const array = [23, 12, 5, 19, 3];
const removed = array.pop(); //这个是将删除的项返回

console.log(removed); // 3
console.log(array);   // [23, 12, 5, 19]
```

#### 4. shift()：删除第一项

```javascript
const array = [23, 12, 5, 19, 3];
const removed = array.shift();//这个是将删除的项返回

console.log(removed); // 23
console.log(array);   // [12, 5, 19, 3]
```

#### 5. splice()：删除或插入

语法：

```javascript
array.splice(start, deleteCount, item1, item2);
```

- `start`：开始位置。
- `deleteCount`：删除的元素个数。
- 后续参数：需要插入的新元素。
- **返回的是被操作的元素**

从指定位置删除到末尾：

```javascript
const array = [1, 2, 3, 4, 5];
const removed = array.splice(2);

console.log(removed); // [3, 4, 5]
console.log(array);   // [1, 2]
```

删除指定数量的元素：

```javascript
const array = [1, 2, 3, 4, 5];
const removed = array.splice(1, 2);

console.log(removed); // [2, 3]
console.log(array);   // [1, 4, 5]
```

删除后插入新元素：

```javascript
const array = [1, 2, 3, 4, 5];
const removed = array.splice(1, 2, 28);

console.log(removed); // [2, 3]
console.log(array);   // [1, 28, 4, 5]
```

#### 6. slice()：截取数组

`slice(start, end)` 包含 `start`，不包含 `end`。省略 `end` 时，会截取到数组末尾。

```javascript
const array = [23, 12, 5, 19, 3];
const result = array.slice(1, 3);//返回的是被截取的部分,原数组不变
console.log(result); // [12, 5]
console.log(array);  // 原数组不变


const arr = [10,20,30,40,50];
var a = arr.slice(1) //不写end默认截到末尾
var b = arr.slice(-2)
console.log(a);  //[20, 30, 40, 50]
console.log(b);  //[40, 50]
console.log(arr);  // 原数组不变
```

```js
// slice() 不传参就是复制一个新数组
const arr = [10,20,30,40,50];
let copy = arr.slice();
console.log(copy); // [10,20,30,40,50]
console.log(arr);
```



#### 7. concat()：合并与浅拷贝

```javascript
const array1 = [1, 2];
const array2 = [3, 4];
const result = array1.concat(array2, 5);
//对一个对象array1,括号里是其他想合并到内容
console.log(result); // [1, 2, 3, 4, 5]
```

不传递参数时，可以得到一个一维数组的浅拷贝：

```javascript
const array = [10, 11, 13, 14];
const copy = array.concat();

copy[0] = 100; //修改新数组的元素

console.log(copy[0]);  // 100,新数组变了
console.log(array[0]); // 10,原数组不变,浅拷贝基本数据类型不变



//浅拷贝后,修改新数组的引用数据类型,原数组也会变
const array = [{ name: '张三', age: 20 }]; 
const array2 = array.concat(); 

// 修改新数组里的对象属性
array2[0].name = '李四';
console.log(array2); // 结果：[{ name: '李四', age: 20 }]
console.log(array);  // 结果：[{ name: '李四', age: 20 }]  😱 原数组也被改了！
```

#### 8.includes():是否存在元素

```js
const arr = [10, 2, 5, 8];

console.log(arr.includes(5));  // true
console.log(arr.includes(99)); // false

// 实战用法（就是你刚才第 4 题写的）：
if (!arr.includes(num)) {
    arr.push(num); // 不包含才添加
}
```

### 11.5 数组的浅拷贝与深拷贝

学习浅拷贝和深拷贝之前，需要先理解两个概念：**栈（Stack）**和**堆（Heap）**。它们可以帮助我们理解：为什么有时修改新变量，旧变量也会跟着改变。

#### 1. 先理解栈和堆

在学习阶段，可以使用下面的简化模型理解 JavaScript 内存：

| 内存区域 | 通俗理解 | 主要保存什么 |
| --- | --- | --- |
| 栈（Stack） | 存放变量当前值的地方 | 原始类型的值，以及指向堆中对象的地址（可以理解成“引用”或“门牌号”） |
| 堆（Heap） | 存放复杂数据的地方 | 对象、数组、函数等引用类型的实际内容 |

例如：

```javascript
// 原始类型-栈  
const age = 18;  
let a = true; 
let b = 'zhangsan'

// 复杂类型-堆  
const student = { name: '张三', age: 18 };  
let arr = [1,2,3] 
```

可以简化理解为：

```text
栈                                  堆
age  ───────────────> 18

student ─────────────> 引用地址 ───> { name: '张三', age: 18 }
```

- `age` 是原始类型，可以直接把变量和值联系起来理解。
- `student` 是对象，变量保存的是对象的地址，通过这个地址找到堆中的实际对象。

> 栈和堆在这里是帮助理解数据关系的简化模型。不同 JavaScript 引擎会有自己的内存优化方式，不要求完全按照这种形式存储。

#### 2. 原始类型赋值：两个变量互不影响

这里赋值不是拷贝

先看原始类型的赋值：

```javascript
let a = 'zhangsan';
let b = a;
console.log(b);//'zhangsan'
b = 'lisi';
console.log(b);//lisi
console.log(a);//zhangsan
```

执行过程：

1. `let  a = 'zhangsan'`：变量 `a` 保存数值 `zhangsan`。
2. `let b = a`：把 `a` 中的值复制给 `b`，此时 `b` 也是 `zhangsan`。
3. `b = lisi`：只修改 `b`，不会修改 `a`。

可以简化理解为：

```text
栈
a ─────> zhangsan
b ─────> lisi
```

所以，原始类型赋值后，两个变量各自保存自己的值，互不影响。

#### 3. 引用类型直接赋值：复制的是地址，不是新数组

再看数组的赋值：

```javascript
let arr1 = [1,2,3];
let arr2 = arr1;
console.log(arr2);//[1,2,3];
arr2.push(4)
console.log(arr2);//[1,2,3,4];
console.log(arr1);//[1,2,3,4];
```

为什么只修改 `arr2`，`arr1` 也发生了变化？

执行过程：

1. `const arr1 = [1, 2, 3 ]`：在堆中创建数组，`arr1` 保存这个数组的地址。
2. `const arr2 = arr1`：把数组的地址复制给 `arr2`，没有在堆中创建新数组。
3. `arr1` 和 `arr2` 指向同一个数组，所以通过 `arr2` 修改数组，使用 `arr1` 也能看到变化。

可以简化理解为：

```text
栈                                  堆
arr1 ─────────────┐
                  ├───────────────> [1, 2, 3]
arr2 ─────────────┘
```

`arr1 === arr2` 返回 `true`，也说明它们指向的是同一个数组。

**注意：`const arr2 = arr1` 只是引用赋值，既不是浅拷贝，也不是深拷贝。**



总结:基本类型（数字、字符串、布尔值）赋值，都是“值传递”。传过去的就是具体的值，各自独立。

引用类型（对象、数组、函数）赋值，都是“引用传递”。传递的是房间的地址，两个人共享同一个房间。



#### 4. 浅拷贝：只复制第一层

浅拷贝会创建一个新的数组或对象，但只复制第一层。如果内部还有对象或数组，嵌套内容仍然共享地址。

常见的浅拷贝方式：

```javascript
const copy1 = original.slice();
const copy2 = original.concat();
```

##### 情况一：一维数组的浅拷贝

```javascript
const original = [10, 20, 30];
const copy = original.slice();

copy[0] = 99;
//这时候两个是独立的,不会互相影响
console.log(original); // [10, 20, 30]
console.log(copy);     // [99, 20, 30]
console.log(original === copy); // false
```

`slice()` 创建了一个新数组：

```text
栈(原始类型的值,地址)                     堆
original ─────────────────────────> 数组 A：[10, 20, 30]
copy ─────────────────────────────> 数组 B：[10, 20, 30]
```

两个数组不是同一个数组，所以修改 `copy[0]` 不会影响 `original[0]`。



##### 情况二：数组中包含对象(二维多层)

```javascript
const original = [10, {name:"张三",a:{
    age:18
}}];
const copy = original.slice();
copy[0]=99;  //修改外层
console.log(copy);  //外层独立
console.log(original);  //不受影响
copy[1].name="李四";  //修改内层
console.log(copy);   //内层对象还是同一个
console.log(original);  //外层不变,内层变化了
```

虽然 `slice()` 创建了新的外层数组，但数组中的对象没有被重新创建：

```text
栈                                  堆
original ─────> 外层数组 A ───────┐
                                  ├────> 同一个对象：{ name: '李四' }
copy ─────────> 外层数组 B ───────┘
```

具体过程：

1. `original` 和 `copy` 是两个不同的外层数组。
2. `original[0]` 和 `copy[0]` 保存的是同一个对象的引用。
3. 修改 `copy[0].name`，实际上就是修改这个共享对象。
4. 因此 `original[0].name` 也会变成 `'李四'`。

这就是浅拷贝的核心：**第一层是新的，嵌套的对象或数组仍然共用。**



#### 5. 深拷贝：每一层都创建独立副本

深拷贝就是**“完完全全的两个独立实体”**。你把新数组砸了、烧了、改得面目全非，原数组连一根头发丝都不会掉。

深拷贝不仅会创建新的外层数组，还会为内部嵌套的对象和数组创建新副本。复制完成后，新旧数据互不影响。

对于只包含 JSON 基础数据的数组，可以使用下面的方式：

```javascript
const original = [
  {
    name: '张三',
    scores: [80, 90]
  }
];

//深拷贝方法
const copy = JSON.parse(JSON.stringify(original));

copy[0].name = '李四';
copy[0].scores[0] = 100;


//完全独立的两个数组
console.log(copy[0].name);          // 李四
console.log(original[0].name);      // 张三
console.log(copy[0].scores[0]);     // 100
console.log(original[0].scores[0]); // 80

console.log(original === copy);                  // false
console.log(original[0] === copy[0]);            // false
console.log(original[0].scores === copy[0].scores); // false
```

深拷贝后的关系可以简化理解为：

```text
original ─────> 外层数组 A ─────> 对象 A ─────> 成绩数组 A

copy ─────────> 外层数组 B ─────> 对象 B ─────> 成绩数组 B
```

外层数组、内部对象、成绩数组都是独立副本，所以修改 `copy` 不会影响 `original`。

`JSON.stringify(original)` 会先把数组转换为 JSON 字符串，`JSON.parse()` 再根据字符串创建全新的数组和对象，因此可以实现这种场景下的深拷贝。

JSON 方式有使用限制：

- 对象中的函数以及值为 `undefined` 的属性会被忽略。
- 数组中的函数或 `undefined` 会被转换成 `null`。
- `Date` 对象会被转换成字符串，不再是原来的日期对象。
- 不能直接处理循环引用，例如对象内部又引用对象自身。

因此，这种写法只适合数字、字符串、布尔值、`null`、普通对象和数组组成的简单 JSON 数据。

#### 6. 三种写法对比

| 写法 | 是否创建新外层数组 | 嵌套对象是否独立 | 修改副本是否可能影响原数据 |
| --- | --- | --- | --- |
| `copy = original` | 否 | 否 | 会，因为两者指向同一个数组 |
| `copy = original.slice()` | 是 | 否 | 修改嵌套对象时会影响原数据 |
| `copy = JSON.parse(JSON.stringify(original))` | 是 | 是 | 对简单 JSON 数据不会影响原数据 |

一句话总结复杂数据类型：

- **直接赋值**：两个变量使用同一个数组。
- **浅拷贝**：外层是新的，里面仍然共用。
- **深拷贝**：外层和嵌套内容都是新的，修改互不影响。



### 11.6 数组的排序和翻转

#### 1. reverse()：翻转数组

```javascript
const array = [10, 11, 13, 14];
array.reverse();

console.log(array); // [14, 13, 11, 10]
```

#### 2. sort()：排序

`sort()` 默认把元素转换为字符串，再按照 **Unicode** 顺序排序，因此直接排序数字可能得到意外结果。

```javascript
const array = [10, 11, 13, 14, 100, 21];
array.sort();

console.log(array); // [10, 100, 11, 13, 14, 21]
```

数字升序排序需要传入比较函数：

```javascript
const array = [10, 11, 13, 14, 100, 21];
array.sort(function (a, b) {
  return a - b;
});

console.log(array); // [10, 11, 13, 14, 21, 100]


//推荐写法(匿名函数)
let e = array.sort((a,b)=>{ //推荐写法
  return a-b  //a-b是从小到大,b-a是从大到小
})
console.log(array);
```

`reverse()` 和 `sort()` 都会修改原数组。



#### 3. 冒泡排序

冒泡排序是一种反复比较**相邻两个元素**的排序方法。下面以升序排列为例，也就是把数字从小到大排列。

##### 第一步：理解基本规则

假设有两个相邻数字：

```text
[10, 2]
```

因为左边的 `10` 大于右边的 `2`，所以交换它们的位置：

```text
[2, 10]
```

冒泡排序就是不断重复这个过程，并遵守以下规则：

1. 每次只比较相邻的两个元素：`array[j]` 和 `array[j + 1]`。
2. 如果左边的数字大于右边的数字，就交换位置。
3. 如果左边的数字小于或等于右边的数字，就不交换。
4. 每完成一轮，当前未排序部分的最大值都会被移动到最右边。
5. 已经排到末尾的数字不再参加后面的比较。

##### 第二步：准备要排序的数组

```javascript
const array = [10, 2, 16, 30, 8];
```

数组下标与元素的对应关系如下：

| 下标 | 0 | 1 | 2 | 3 | 4 |
| --- | --- | --- | --- | --- | --- |
| 元素 | 10 | 2 | 16 | 30 | 8 |

数组有 5 个元素，最多需要进行 `5 - 1 = 4` 轮比较。

##### 第三步：进行第 1 轮比较

第 1 轮中，`i = 0`。从数组开头开始，依次比较下标 `0和1`、`1和2`、`2和3`、`3和4`。

初始数组：

```text
[10, 2, 16, 30, 8]
```

| 步骤 | `j` | 比较的两个数 | 是否交换 | 当前数组 |
| --- | --- | --- | --- | --- |
| 1 | 0 | `10 > 2` | 交换 | `[2, 10, 16, 30, 8]` |
| 2 | 1 | `10 < 16` | 不交换 | `[2, 10, 16, 30, 8]` |
| 3 | 2 | `16 < 30` | 不交换 | `[2, 10, 16, 30, 8]` |
| 4 | 3 | `30 > 8` | 交换 | `[2, 10, 16, 8, 30]` |

第 1 轮结束后：

```text
[2, 10, 16, 8, 30]
```

最大值 `30` 已经到达正确位置。下一轮不用再比较最后的 `30`。

##### 第四步：进行第 2 轮比较

第 2 轮中，`i = 1`。只处理前 4 个尚未确定位置的数字：

```text
[2, 10, 16, 8]
```

| 步骤 | `j` | 比较的两个数 | 是否交换 | 当前数组 |
| --- | --- | --- | --- | --- |
| 1 | 0 | `2 < 10` | 不交换 | `[2, 10, 16, 8, 30]` |
| 2 | 1 | `10 < 16` | 不交换 | `[2, 10, 16, 8, 30]` |
| 3 | 2 | `16 > 8` | 交换 | `[2, 10, 8, 16, 30]` |

第 2 轮结束后：

```text
[2, 10, 8, 16, 30]
```

`16` 已经到达正确位置，末尾的 `[16, 30]` 都已经排好。

##### 第五步：进行第 3 轮比较

第 3 轮中，`i = 2`。只处理前 3 个数字：

```text
[2, 10, 8]
```

| 步骤 | `j` | 比较的两个数 | 是否交换 | 当前数组 |
| --- | --- | --- | --- | --- |
| 1 | 0 | `2 < 10` | 不交换 | `[2, 10, 8, 16, 30]` |
| 2 | 1 | `10 > 8` | 交换 | `[2, 8, 10, 16, 30]` |

第 3 轮结束后：

```text
[2, 8, 10, 16, 30]
```

`10` 已经到达正确位置，末尾的 `[10, 16, 30]` 都已经排好。

##### 第六步：进行第 4 轮比较

第 4 轮中，`i = 3`，只剩前两个数字需要比较：

```text
[2, 8]
```

因为 `2 < 8`，所以不交换：

```text
[2, 8, 10, 16, 30]
```

此时所有数字都已按从小到大的顺序排列，排序结束。

##### 第七步：写出完整代码

```javascript
let array = [24,69,80,57,13]

// 外层循环：控制一共进行多少轮比较
for (let i = 0; i < array.length - 1; i++) {
  // 内层循环：控制当前这一轮需要比较哪些相邻元素
  for (let j = 0; j < array.length - i - 1; j++) {
    // 左边的数字大于右边的数字时，交换位置
    if (array[j] > array[j + 1]) {//小于是降序
      [array[j],array[j+1]] = [array[j+1],array[j]]
    }
  }
}

console.log('排序结果：', array);
```

控制台会依次看到：

```text
//上面代码省略了
第 1 轮结束：[2, 10, 16, 8, 30]
第 2 轮结束：[2, 10, 8, 16, 30]
第 3 轮结束：[2, 8, 10, 16, 30]
第 4 轮结束：[2, 8, 10, 16, 30]
排序结果：[2, 8, 10, 16, 30]
```

##### 第八步：理解代码中的关键条件

###### 1. 为什么外层循环是 `i < array.length - 1`？

数组有 `n` 个元素时，最多进行 `n - 1` 轮比较。当前数组有 5 个元素，所以进行 4 轮：

```text
i = 0：第 1 轮
i = 1：第 2 轮
i = 2：第 3 轮
i = 3：第 4 轮
```

当 `i = 4` 时，不满足 `i < 4`，外层循环结束。

###### 2. 为什么内层循环是 `j < array.length - i - 1`？

这个条件有两个作用：

- 每轮都从数组开头开始比较，但不比较末尾已经排好的元素。
- 保证 `j + 1` 不会超出数组下标范围。

当前数组长度为 5，各轮比较次数如下：

| 轮次 | `i` | 内层条件 | `j` 的取值 | 比较次数 |
| --- | --- | --- | --- | --- |
| 第 1 轮 | 0 | `j < 4` | 0、1、2、3 | 4 次 |
| 第 2 轮 | 1 | `j < 3` | 0、1、2 | 3 次 |
| 第 3 轮 | 2 | `j < 2` | 0、1 | 2 次 |
| 第 4 轮 | 3 | `j < 1` | 0 | 1 次 |

可以看到，每完成一轮，下一轮就少比较一次，因为末尾又多了一个已经排好的数字。

###### 3. 为什么交换时需要 `temp`？

交换两个变量时，不能直接写成：

```javascript
array[j] = array[j + 1];
array[j + 1] = array[j];
```

执行第一句后，`array[j]` 原来的值已经被覆盖，第二句就无法取得原来的值了。因此要先用 `temp` 保存原值，再完成交换。

一句话总结冒泡排序：**外层循环控制轮数，内层循环比较相邻数字，左边比右边大就交换，每轮把当前最大值送到末尾。**



#### 4. 选择排序(可跳过)

选择排序是一种每轮先“选出最小值”，再把它放到正确位置的排序方法。下面以升序排列为例，也就是把数字从小到大排列。

##### 第一步：理解选择排序的核心思路

每一轮都做三件事：

1. 把当前未排序区域的第一个元素暂时当作最小值。
2. 向后查找，如果发现更小的值，就记录这个更小值的下标。
3. 查找结束后，把最小值与未排序区域的第一个元素交换位置。

例如：

```text
[10, 2, 16, 30, 8]
```

第一轮找到最小值 `2`，把它与第一个位置的 `10` 交换：

```text
[2, 10, 16, 30, 8]
```

此时 `2` 已经排好。第二轮不再处理 `2`，而是从后面的 `[10, 16, 30, 8]` 中继续找最小值。

##### 第二步：认识代码中的三个变量

| 变量 | 作用 |
| --- | --- |
| `i` | 当前轮次，也是未排序区域的第一个下标 |
| `j` | 向后查找时，正在比较的元素下标 |
| `minIndex` | 当前找到的最小值所在的下标 |

这里保存的是最小值的**下标**，而不只是最小值本身。因为最后交换元素时，必须知道最小值在哪个位置。

##### 第三步：进行第 1 轮选择

初始数组：

```text
[10, 2, 16, 30, 8]
```

第 1 轮中，`i = 0`，先假设下标 `0` 的 `10` 是最小值，所以 `minIndex = 0`。

| 比较过程 | 是否找到更小值 | `minIndex` 的变化 |
| --- | --- | --- |
| 初始值：`10` | 暂时把 `10` 当作最小值 | `minIndex = 0` |
| `2 < 10` | 是，`2` 更小 | `minIndex = 1` |
| `16 < 2` | 否 | 仍为 `1` |
| `30 < 2` | 否 | 仍为 `1` |
| `8 < 2` | 否 | 仍为 `1` |

查找结束后，最小值 `2` 的下标是 `1`，与下标 `0` 的 `10` 交换：

```text
交换前：[10, 2, 16, 30, 8]
交换后：[2, 10, 16, 30, 8]
```

第 1 轮结束，`2` 已经放在正确位置。

##### 第四步：进行第 2 轮选择

第 2 轮中，`i = 1`，只处理尚未排序的部分：

```text
[10, 16, 30, 8]
```

先假设下标 `1` 的 `10` 是最小值，所以 `minIndex = 1`。

| 比较过程 | 是否找到更小值 | `minIndex` 的变化 |
| --- | --- | --- |
| 初始值：`10` | 暂时把 `10` 当作最小值 | `minIndex = 1` |
| `16 < 10` | 否 | 仍为 `1` |
| `30 < 10` | 否 | 仍为 `1` |
| `8 < 10` | 是，`8` 更小 | `minIndex = 4` |

查找结束后，把下标 `4` 的 `8` 与下标 `1` 的 `10` 交换：

```text
交换前：[2, 10, 16, 30, 8]
交换后：[2, 8, 16, 30, 10]
```

第 2 轮结束，`[2, 8]` 已经排好。

##### 第五步：进行第 3 轮选择

第 3 轮中，`i = 2`，只处理：

```text
[16, 30, 10]
```

先假设下标 `2` 的 `16` 是最小值，所以 `minIndex = 2`。

| 比较过程 | 是否找到更小值 | `minIndex` 的变化 |
| --- | --- | --- |
| 初始值：`16` | 暂时把 `16` 当作最小值 | `minIndex = 2` |
| `30 < 16` | 否 | 仍为 `2` |
| `10 < 16` | 是，`10` 更小 | `minIndex = 4` |

把下标 `4` 的 `10` 与下标 `2` 的 `16` 交换：

```text
交换前：[2, 8, 16, 30, 10]
交换后：[2, 8, 10, 30, 16]
```

第 3 轮结束，`[2, 8, 10]` 已经排好。

##### 第六步：进行第 4 轮选择

第 4 轮中，`i = 3`，只剩两个数字需要处理：

```text
[30, 16]
```

先假设下标 `3` 的 `30` 是最小值。比较时发现 `16 < 30`，所以把 `minIndex` 更新为 `4`。

最后交换 `30` 和 `16`：

```text
交换前：[2, 8, 10, 30, 16]
交换后：[2, 8, 10, 16, 30]
```

最后一个数字 `30` 不需要再单独处理，因为前面的数字全部排好后，它自然就在正确位置。至此排序结束。

##### 第七步：写出完整代码

```javascript
const array = [10, 2, 16, 30, 8];

// 外层循环：i 表示未排序区域的第一个下标
for (let i = 0; i < array.length - 1; i++) {
  // 先假设当前第一个元素就是最小值
  let minIndex = i;

  // 从 i 的后面开始，寻找真正的最小值
  for (let j = i + 1; j < array.length; j++) {
    if (array[j] < array[minIndex]) {
      // 只记录更小值的下标，此时不交换
      minIndex = j;
    }
  }

  // 如果最小值不在 i 的位置，就进行交换
  if (minIndex !== i) {
    const temp = array[i];
    array[i] = array[minIndex];
    array[minIndex] = temp;
  }

  console.log(`第 ${i + 1} 轮结束：`, array);
}

console.log('排序结果：', array);
```

控制台会依次看到：

```text
第 1 轮结束：[2, 10, 16, 30, 8]
第 2 轮结束：[2, 8, 16, 30, 10]
第 3 轮结束：[2, 8, 10, 30, 16]
第 4 轮结束：[2, 8, 10, 16, 30]
排序结果：[2, 8, 10, 16, 30]
```

##### 第八步：理解代码中的关键语句

###### 1. 为什么每轮先写 `let minIndex = i`？

因为每轮开始时，先假设“未排序区域的第一个元素就是最小值”，然后再通过内层循环验证有没有比它更小的值。

例如第 2 轮的 `i = 1`，表示从下标 `1` 开始查找，下标 `0` 的 `2` 已经排好，不再参与比较。

###### 2. 为什么内层循环从 `j = i + 1` 开始？

`i` 位置的元素已经作为初始最小值，不需要再和自己比较，所以 `j` 从它的下一个位置开始。

###### 3. 为什么内层循环中只修改 `minIndex`，不交换？

因为此时还不能确定后面的元素中是否会出现更小的值。必须等整轮查找结束后，才能确定真正的最小值。

```javascript
if (array[j] < array[minIndex]) {
  minIndex = j;
}
```

这段代码的意思是：如果当前元素比已记录的最小值还小，就更新最小值的下标。

###### 4. 为什么交换前要判断 `minIndex !== i`？

如果 `minIndex === i`，说明第一个元素本来就是最小值，不需要自己和自己交换。

##### 选择排序与冒泡排序的区别

| 对比项 | 选择排序 | 冒泡排序 |
| --- | --- | --- |
| 每轮目标 | 找到最小值，放到前面 | 通过相邻比较，把较大值逐步移到后面 |
| 内层循环做什么 | 查找并记录最小值下标 | 比较相邻元素，必要时立即交换 |
| 每轮交换次数 | 最多交换一次 | 可能交换多次 |
| 已排序部分 | 从数组前方向后增加 | 从数组后方向前增加 |

一句话总结选择排序：**每轮从未排序部分找出最小值，再与未排序部分的第一个元素交换。**

### 11.7 获取数组长度

`length` 属性返回数组中元素的数量。

```javascript
const array = [10, 11, 13, 14, 100, 21];
console.log(array.length); // 6
```

### 11.8 数组练习

1. 计算数组 `[12, 34, 2, 5, 76]` 中所有数字的和。

2. 去掉数组 `[4, 0, 7, 9, 0, 0, 2, 6, 0, 3, 1, 0]` 中的 `0`，把非零值保存到新数组。

   ```js
   const array = [4, 0, 7, 9, 0, 0, 2, 6, 0, 3, 1, 0]
   var newArr=[]
   for (i=0;i<array.length;i++){
       if(array[i]!=0){
         newArr.push(array[i])
       }else{
         continue
       }
   }
   console.log(newArr)
   ```

3. 求数组 `[12, 34, 2, 5, 76]` 中的最大值。

   ```js
   const array =  [12, 34, 2, 5, 76,123,13,1234,1411561,1213]
   
   var max= array[1]
   for (i=0;i<array.length;i++){
       if (max<array[i]) {
         max = array[i]
       } 
   }
   console.log(max)
   ```

4. 向数组 `[10, 2]` 中添加数字，直到数组长度为 5，且不能出现重复数据。

   ```js
   const array = [10,2]
   
   while(array.length!=5){
       a =  Number(prompt('请输入添加的元素'))
       if(array.includes(a)){
         continue
       }else{
         array.push(a)
       }
   }
   console.log(array)
   ```

   

5. 对数组 `[1, 3, 5, 2, 1, 4, 3, 1, 2, 5, 3, 1, 2, 1]` 去重。

   ```js
   const array = [1, 3, 5, 2, 1, 4, 3, 1, 2, 5, 3, 1, 2, 1]
   
   var newArr = []
   for (i=0;i<array.length;i++){
       if (newArr.includes(array[i])){
         continue
       }else{
         newArr.push(array[i])
       }
   }
   console.log(newArr)
   ```

   

## 12. JavaScript 函数

函数是用于完成特定任务的代码块。函数定义后不会自动执行，只有被调用时才执行。

### 12.1 为什么使用函数

1. 复用代码：同一功能只需编写一次。
2. 减少冗余：避免在多个位置重复相同逻辑。
3. 接收不同参数：同一段逻辑可以处理不同数据。
4. 返回处理结果：其他代码可以继续使用函数的计算结果。

### 12.2 函数的基本语法以及其他写法

```javascript
function 函数名(参数1, 参数2) {
  // 要执行的代码
}
```

函数名的命名规则与变量名相同，多个参数使用逗号分隔。

```js
//其他写法
// 1. 函数声明式（最传统）普通函数
function aaa(item){
    return item+10
}
aaa(10)
console.log(aaa(10));


// 2. 函数表达式（赋值给变量）,也是匿名函数.
var aaa = function(item){
    return item+10
}
console.log(aaa(10)) //和下面这个不一样的哦

// 3. 箭头函数,类似Python的匿名函数
var aaa = (item) => item+10
console.log(aaa(10));

// 箭头函数（=>）是 ES6 引入的“语法糖”（就是让代码写起来更甜的简写方式）。
// 它就是把 function 关键字换成了 => 放在参数和函数体之间。

// 4. 对象里的普通函数
let b = {
    name: "小明",
    aaa: function () {
        console.log(this.name);
    }
}
b.aaa(); // 输出：小明


// 5. 对象里的箭头函数
// 不懂
let c = {
    name: "小明",
    aaa: () => {
        console.log(this.name);
    }
}
c.aaa(); // 输出：undefined （甚至可能报错）
```



### 12.3 定义和调用函数

```javascript
function add(num1, num2) {
  const result = num1 + num2;
  console.log(result);
}

add(1, 2); // 3
add(10, 20); // 30
```

如果后续还需要使用计算结果，应使用 `return` 返回：

```javascript
function add(num1, num2) {
  return num1 + num2;
}

const result = add(1, 2);
console.log(result + 2); // 5
```

`return` 会结束当前函数的执行，并把结果返回给调用者。

### 12.4 匿名函数

没有名字的函数称为匿名函数。匿名函数通常赋值给变量，或者作为参数传给其他函数。

```javascript
const sayHello = function () {
  console.log('你好');
};

sayHello(); // 你好
```

这里的 `function () {}` 没有函数名，通过变量 `sayHello` 调用。虽然函数本身没有名字，但调用方式和普通函数相同。

**匿名函数最常见的用法是作为回调函数传给其他方法：**

```javascript
const numbers = [1, 2, 3];
//四种写法
//写法一
numbers.forEach(function (item) {
  console.log(item);
});
//写法二
numbers.forEach((item,index,numbers)=>{
   console.log(item,index,numbers);
})
//写法三
numbers.forEach((item)=>{
   console.log(item);
})
//写法四
numbers.forEach(item => console.log(item));
```

`forEach()` 需要一个函数作为参数，这里直接传入一个匿名函数，用完即止，不需要单独命名。

箭头函数是匿名函数的简写形式：

```javascript
const add = (num1, num2) => num1 + num2;

console.log(add(1, 2)); // 3
```

当函数体只有一行 `return` 时，可以省略 `return` 和花括号，写法更简短。

### 12.5 函数传参

#### 1. 形参和实参

定义函数时写的参数称为**形参**，调用函数时传入的值称为**实参**。

```javascript
function add(num1, num2) { // num1、num2 是形参
  return num1 + num2;
}

add(1, 2); // 1、2 是实参
```

调用时，实参会按顺序依次赋值给形参：`num1` 得到 `1`，`num2` 得到 `2`。

#### 2. 参数数量不一致的情况

实参少于形参时，缺少的参数值为 `undefined`：

```javascript
function greet(name, age) {
  console.log(name, age);
}

greet('张三'); // 张三 undefined
```

实参多于形参时，多余的实参不会被使用：

```javascript
function add(num1, num2) {
  return num1 + num2;
}

console.log(add(1, 2, 3)); // 3，第三个参数被忽略
```

#### 3. 默认参数

可以在形参后面写默认值，当没有传入该参数时使用默认值。

```javascript
function greet(name = '游客') {
  console.log(`你好，${name}`);
}

greet('张三'); // 你好，张三
greet();       // 你好，游客
```

#### 4. 剩余参数

不确定要传多少个参数时，可以用 `...` 把多余的实参收集成**数组**。扩展运算符

```javascript
function sum(...args) {  //Python的*args
  let total = 0;
  for (const num of args) {  //遍历数组
    total += num;
  }
  return total;
}
console.log(sum(1, 2, 3)); //不确定多少个参数:6
```

`args` 是一个真正的数组，可以直接遍历。



#### 5. 传递引用类型时要注意

传入原始类型时，函数内部修改形参不会影响外面的变量：

```javascript
function change(num) {
  num = 100;
}

let a = 10;
change(a);
console.log(a); // 10
```

传入对象或数组时，形参和实参指向同一个对象，函数内部的修改会影响外部：

```javascript
function changeArr(arrParam) {
    arrParam.push(4); // 修改了形参指向的那个数组
    console.log('函数内的数组：', arrParam); // [1, 2, 3, 4]
}

let myArr = [1, 2, 3];
changeArr(myArr);
console.log('函数外的数组：', myArr); // [1, 2, 3, 4] 😱 原数组变了！
```

如果不想影响原对象，可以在函数内部先拷贝一份，或者不直接修改传入的对象。

如果不是修改操作而是内部重新赋值则不受影响

```js
function reassignArr(arrParam) {
// 注意:这里不是 push，而是直接给它赋了一个全新的数组！
    arrParam = [9, 9, 9]; 
    console.log('函数内：', arrParam); // [9, 9, 9]
}

let myArr = [1, 2, 3];
reassignArr(myArr);
console.log('函数外：', myArr); // [1, 2, 3] （没有变！）
```



### 12.6 案例：简易计算器

以下代码需要在浏览器中运行：

```javascript
function add(num1, num2) {
  return num1 + num2;
}

function sub(num1, num2) {
  return num1 - num2;
}

function multiply(num1, num2) {
  return num1 * num2;
}

function divide(num1, num2) {
  return num1 / num2;
}

const num1 = Number(prompt('请输入第一个数'));
const num2 = Number(prompt('请输入第二个数'));
const operator = prompt('请输入运算符：+、-、*、/');

if (Number.isNaN(num1) || Number.isNaN(num2)) {
  alert('请输入有效数字');
} else {
  switch (operator) {
    case '+':
      alert(add(num1, num2));
      break;
    case '-':
      alert(sub(num1, num2));
      break;
    case '*':
      alert(multiply(num1, num2));
      break;
    case '/':
      alert(divide(num1, num2));
      break;
    default:
      alert('请输入正确的运算符');
  }
}
```

### 12.7 递归函数

函数在内部调用自身，称为递归。递归必须具有结束条件，否则会不断调用，最终造成栈溢出。

```apl
递归三要素
要素一：明确终止条件(出口)
要素二：提取重复逻辑，缩小规模(递推)
要素三：给出返回值(归)
```

```javascript
function countdown(num) {
  console.log(num);

  if (num === 1) {  //出口
    return;
  }
  return countdown(num - 1);  //递推+归
}

countdown(3); // 依次输出 3、2、1
```

使用递归计算 `1～5` 的和：

```javascript
function getSum(num) {
  console.log(num);
  if (num === 1) {  //出口
    return 1;
  }
  return num + getSum(num - 1);  //递推+归
}
console.log(getSum(5)); // 15
```



### 12.8 闭包

#### 1. 先看问题：不用闭包时遇到的麻烦

假设要做一个计数器，每调用一次函数，数字加 1。

**尝试一：用局部变量**

```javascript
function count() {
  let num = 0;
  num++;
  return num;
}

console.log(count()); // 1
console.log(count()); // 1，希望是 2
console.log(count()); // 1，希望是 3
```

问题：每次调用函数都会重新执行 `let num = 0`，函数执行结束后 `num` 就被销毁，所以结果永远是 `1`，无法累计。

**尝试二：用全局变量**

```javascript
let num = 0;

function count() {
  num++;
  return num;
}

console.log(count()); // 1
console.log(count()); // 2
console.log(count()); // 3
```

累计是成功了，但 `num` 是全局变量，任何人都能直接修改它：

```javascript
num = 1000; // 被意外改动，计数被破坏
```

也就是说：局部变量无法保存状态，全局变量容易被污染。我们需要一种方式，让变量**既能在多次调用间保留，又不对外暴露**。闭包就是解决这个问题的。

#### 2. 用闭包解决

```javascript
function count() {
    let num = 0;
    function inner() {  //外嵌内
        num++;  //内用外
        return num;
    }
    return inner;  //外返内
}
let fn = count();
console.log(fn());  //1
console.log(fn());  //2
console.log(fn());  //3
```

执行过程：

1. `count()` 执行时创建局部变量 `num`。
2. 返回的内部函数使用了 `num`，所以这个变量被保留下来，不会被销毁。
3. 每次调用 `count()` 都是在同一个 `count` 上加 `1`。
4. 外部代码拿不到 `num`，只能通过 `count()` 操作它，相当于一个私有变量。

这就是闭包：**内部函数使用了外层函数的变量，即使外层函数已经执行结束，这个变量仍然可以被访问。**

每次调用 `count()` 都会创建独立的 `num`，多个计数器互不影响：

```javascript
const counterA = count();
const counterB = count();

console.log(counterA()); // 1
console.log(counterA()); // 2
console.log(counterB()); // 1，不受 counterA 影响
```

#### 3. 常见用途

- 保存函数执行后还要继续使用的数据，例如计数器。
- 创建私有变量，避免外部随意修改。
- 作为回调函数时，继续使用创建时的外部数据。

#### 4. 闭包带来的问题以及怎么解决

**问题一：变量一直占用内存**

**解决：用完后把保存闭包的变量置为 `null`**，主动断开引用，让垃圾回收机制可以清理：

```js
function count() {
    let num = 0;
    function inner() {
        num++;
        return num;
    }
    return inner;
}
let fn = count();
console.log(fn());
console.log(fn());
console.log(fn());
fn = null;
```

一句话总结：**闭包就是内部函数记住了外层函数的变量，让数据既能保留下来又不对外暴露；用完后记得置为 `null` 释放内存，循环中避免用 `var` 共享变量。**



### 12.9 函数练习

1. 编写函数，根据用户输入的长度和数值创建数组，并对数组排序。

   ```js
   function createArray(num){
       let array = []
       while(array.length!==num){
           x=Number(prompt('请输入数组元素'))
           array.push(x)
       }
       let newarray = array.sort((a,b)=>{
           return a-b
       })
       return newarray
   }
   a = Number(prompt('请输入数组长度'))
   alert(createArray(a))
   ```

2. 编写函数 `createTable(x, y)`，创建 `x` 行、`y` 列的表格并输出到页面。

3. 使用递归计算 1～5 之间所有整数的和。

   ```js
   function getSum(num){
       if(num===1){
           return 1;
       }
       return num+getSum(num-1)
   }
   console.log(getSum(5))
   ```

4. 编写函数返回斐波那契数列的第 `n` 项。

   ```js
   function fbnq(n){
       if (n===1){return 0}
       if (n===2){return 1}
   
       return fbnq(n-1)+fbnq(n-2)
   }
   
   console.log(fbnq(4))
   ```

5. 将简易计算器中的除法补充为：除数为 0 时给出提示，不执行除法。

   

---

## 13. JavaScript 作用域

作用域是变量或函数可以被访问的范围。参考课件主要介绍全局作用域和函数作用域。

### 13.1 全局作用域 window

在全局使用 `var` 声明的变量属于全局变量，可以在全局和函数内部访问。

```javascript
var a = 10;

function fn() {
  console.log(a);
}

fn(); // 10
console.log(a); // 10
```

### 13.2 函数作用域

在函数内部使用 `var` 声明的变量是局部变量，只能在当前函数内部访问。

```javascript
function fn() {
  var b = 2;
  console.log(b);
}

fn(); // 2
// console.log(b); // 报错：b is not defined
```

函数参数也相当于函数内部的局部变量：

```javascript
function addThree(m) {
  m = m + 3;
  console.log(m);
}

addThree(1); // 4
// console.log(m); // 报错：m is not defined
```

### 13.3 变量提升

JavaScript 执行代码前，会把使用 `var` 声明的变量提升到所在作用域的前面，但赋值仍保留在原位置。

```javascript
console.log(a); // undefined
var a = 10;
```

上面的代码可以理解为：

```javascript
//js编译时的样子
var a;   // 👈 声明被提升到了最前面
console.log(a);// undefined
a = 10;  // 赋值留在了原地
console.log(a); // 10
```

函数内部的局部变量同样会提升：

```javascript
function fn() {
  console.log(b); 
  var b = 10;
}

fn();// undefined
```

如果全局和函数内部存在同名变量，**函数内部优先使用局部变量：**

```javascript
//当 JS 引擎准备执行 fn 这个函数时，
//它不会马上从上往下执行代码。
//它首先会像扫描仪一样快速扫一眼整个函数，
//找到所有用 var 声明的变量。
//它发现了 var n = 5;

var n = 10;  //优先用内部
function fn() {
  console.log(n); // undefined，这里访问的是提升到函数顶部的局部变量 n,先声明了n,但事实没有值
  var n = 5;  赋值留在了原地
  console.log(n); // 5
}

fn();
console.log(n); // 10
```

### 13.4 隐式全局变量

在非严格模式下，给未声明的变量赋值会创建全局变量，容易造成难以排查的问题。

给一个**没有声明过的变量直接赋值**，JS 会自动把它挂载到 `window` 上，变成**全局变量**

```javascript
function fn() {
  var a = b = c = 2;
  // 等价于：var a = 2; b = 2; c = 2;
  // a 是局部变量，b 和 c 在非严格模式下会成为全局变量
}

fn();

console.log(a); // 报错：a is not defined
console.log(b); // 2
console.log(c); // 2
```

不要依赖这种行为。变量应先声明再使用，现代代码可以使用严格模式：

```javascript
'use strict';
```

另外，`let` 和 `const` 具有块级作用域，不采用与 `var` 完全相同的提升规则。学习本节时，应重点掌握 `var` 的函数作用域与变量提升。

---



## 14. JavaScript 自定义对象

对象用于描述一个具体事物，把相关属性和行为组织在一起。例如，一个学生对象可以包含姓名、年龄、性别等属性，也可以包含说话等方法。

### 14.1 使用对象字面量创建对象

```javascript
const student = {
  name: '张三',
  age: 20,
  gender: '男',
  say: ()=>{
     console.log('hello');
  }
};
```

访问属性有两种方式：

```javascript
console.log(student.name);    // 张三
console.log(student['name']); // 张三
```

调用内部函数方法：

```javascript
student.say(); // hello
```

### 14.2 使用 new Object() 创建对象

现在前端开发很少这么写了

```javascript
const student = new Object();//1. 创建一个空对象

student.name = '张三';//2. 动态添加属性（数据）
student.age = 20;
student.say = function () 
  console.log('hello');
};   //3. 动态添加方法（行为/函数）

console.log(student.name);//4. 读取和使用
console.log(student.age);
student.say();
```

### 14.3 使用构造函数创建对象

构造函数用于初始化对象，通常与 `new` 一起使用。它可以把多个对象共有的属性和方法封装起来。

```javascript
function Person(name, age, gender) {
  this.name = name;
  this.age = age;
  this.gender = gender;
  this.say = function () {
    console.log(`大家好，我是${this.name}`);
  };
}

const person = new Person('张三', 25, '男');

console.log(person.age);
person.say();
```

构造函数名通常使用大写字母开头。`new Person()` 会创建一个新对象，并让构造函数中的 `this` 指向这个新对象。

### 14.4 遍历对象

`for...in` 可以遍历对象的可枚举属性名。

```javascript
const person = {
  name: '张三',
  age: 25,
  gender: '男'
};

for (const key in person) {
  console.log(key);         // 属性名
  console.log(person[key]); // 属性值
}
```

---

## 15. JS 常用内置对象方法

### 15.1 String：字符串对象

字符串具有一系列常用属性和方法。字符串本身不可修改，这些方法会返回新字符串或计算结果，不会直接改动原字符串。

#### 字符串常用方法速查表

| 操作           | 方法或属性                       | 作用                                                 |
| -------------- | -------------------------------- | ---------------------------------------------------- |
| 获取长度       | `length`                         | 返回字符串中 UTF-16 码元的数量，可简单理解为字符长度 |
| 获取指定字符   | `charAt()`                       | 返回指定下标的字符                                   |
| 获取字符编码   | `charCodeAt()`                   | 返回指定下标字符的 Unicode 编码                      |
| 查找位置       | `indexOf()`、`lastIndexOf()`     | 查找文本第一次或最后一次出现的位置                   |
| 搜索字符串     | `search()`                       | 返回匹配位置，支持正则表达式                         |
| 判断是否包含   | `includes()`                     | 判断是否包含指定文本，返回布尔值                     |
| 判断开头和结尾 | `startsWith()`、`endsWith()`     | 判断是否以指定文本开头或结尾                         |
| 截取字符串     | `slice()`、`substring()`         | 截取指定位置的字符串                                 |
| 替换内容       | `replace()`、`replaceAll()`      | 替换第一个匹配项或全部匹配项                         |
| 大小写转换     | `toUpperCase()`、`toLowerCase()` | 转换为大写或小写                                     |
| 拼接字符串     | `concat()`、`+`、模板字符串      | 把多个字符串连接起来                                 |
| 删除空白       | `trim()`                         | 删除字符串两端的空白                                 |
| 分割字符串     | `split()`                        | 按指定分隔符把字符串转换为数组                       |
| 重复字符串     | `repeat()`                       | 将字符串重复指定次数                                 |
| 补齐字符串     | `padStart()`、`padEnd()`         | 在开头或末尾补齐到指定长度                           |

#### 1. length：获取长度

```javascript
const text = 'ABCDEF';
console.log(text.length); // 6
```

#### 2. indexOf() 和 lastIndexOf()

`indexOf()` 返回指定文本第一次出现的位置，`lastIndexOf()` 返回最后一次出现的位置。没有找到时，两者都返回 `-1`。

```javascript
const str = "The full name of China is the People's Republic of China.";

console.log(str.indexOf('China'));     // 第一次出现的位置
console.log(str.lastIndexOf('China')); // 最后一次出现的位置
console.log(str.indexOf('USA'));       // -1
```

两种方法都可以传入第二个参数，指定检索的起始位置：

```javascript
console.log(str.indexOf('China', 18));
console.log(str.lastIndexOf('China', 50));
```

#### 3. search()：搜索字符串

`search()` 返回匹配位置，支持字符串或正则表达式。

```javascript
const str = 'Please locate where locate occurs.';
console.log(str.search('locate')); // 7
```

#### 4. 提取部分字符串

| 方法                    | 第二个参数的含义       | 是否支持负索引        |
| ----------------------- | ---------------------- | --------------------- |
| `slice(start, end)`     | 结束位置，不包含该位置 | 支持                  |
| `substring(start, end)` | 结束位置，不包含该位置 | 不支持，负数按 0 处理 |

`slice()`：

```javascript
const str = 'Apple, Banana, Mango';

console.log(str.slice(7, 13));   // Banana
console.log(str.slice(-13, -7)); // Banana
console.log(str.slice(7));       // Banana, Mango
```

`substring()`：

```javascript
console.log(str.substring(7, 13)); // Banana
console.log(str.substring(7));     // Banana, Mango
```

新代码优先使用 `slice()` 或 `substring()`。

#### 5. replace()：替换内容

```
str.replace(pattern, replacement)
```

默认只替换第一个匹配项：

```javascript
const str = 'Please visit Microsoft and Microsoft!';
const result = str.replace('Microsoft', 'Example');

console.log(result);
```

`replace()` 默认区分大小写：

```javascript
const str = 'Please visit Microsoft!';
console.log(str.replace('MICROSOFT', 'Example')); // 没有匹配，不替换
```

使用正则表达式的 `i` 标志忽略大小写：

```javascript
console.log(str.replace(/MICROSOFT/i, 'Example'));
```

使用 `g` 标志进行全局替换：

```javascript
const str = 'Please visit Microsoft and Microsoft!';
console.log(str.replace(/Microsoft/g, 'Example'));
```

#### 6. 大小写转换

```javascript
const text = 'Hello World!';

console.log(text.toLowerCase()); // hello world!
console.log(text.toUpperCase()); // HELLO WORLD!
```

#### 7. concat()：连接字符串

```javascript
const text1 = 'Hello';
const text2 = 'World';

console.log(text1.concat(' ', text2)); // Hello World
console.log('Hello' + ' ' + 'World');  // Hello World
```

#### 8. trim()：删除两端空白

```javascript
const str = '   Hello World!   ';
console.log(str.trim()); // Hello World!
```

#### 9. 提取指定字符

`charAt()` 返回指定下标的字符：

```javascript
const str = 'HELLO WORLD';
console.log(str.charAt(0)); // H
```

#### 10. split()：把字符串转换为数组

```javascript
const text = 'a,b,c,d,e';
console.log(text.split(',')); // ['a', 'b', 'c', 'd', 'e']

const words = 'hello world javascript';
console.log(words.split(' ')); // ['hello', 'world', 'javascript']

const names = '张三|李四|王五';
console.log(names.split('|')); // ['张三', '李四', '王五']
```

#### 11. includes()：判断是否包含指定文本

`includes()` 返回布尔值，适合判断字符串中是否存在某段内容。它会区分大小写。

```javascript
const message = 'Hello JavaScript';

console.log(message.includes('JavaScript')); // true
console.log(message.includes('Hello'));      // true
console.log(message.includes('java'));       // false，大小写不一致
console.log(message.includes('Python'));     // false
```

#### 12. startsWith() 和 endsWith()：判断开头和结尾

`startsWith()` 判断是否以指定文本开头，`endsWith()` 判断是否以指定文本结尾。

```javascript
const fileName = 'avatar.png';

console.log(fileName.startsWith('avatar')); // true
console.log(fileName.startsWith('user'));   // false
console.log(fileName.endsWith('.png'));     // true
console.log(fileName.endsWith('.jpg'));     // false
```

这两个方法也区分大小写，常用于判断文件后缀、网址前缀等。

#### 13. replaceAll()：替换全部匹配内容

`replace()` 使用普通字符串查找时只替换第一项，`replaceAll()` 可以替换所有匹配项。

```javascript
const str = 'red,blue,red,green';
const result = str.replaceAll('red', 'yellow');

console.log(result); // yellow,blue,yellow,green
console.log(str);    // red,blue,red,green，原字符串不变
```

也可以使用带 `g` 标志的正则表达式完成相同操作：

```javascript
console.log(str.replace(/red/g, 'yellow'));
```

#### 14. repeat()：重复字符串

`repeat(count)` 返回重复 `count` 次后的新字符串。

```javascript
console.log('ab'.repeat(3)); // ababab
console.log('*'.repeat(5));  // *****
console.log('你好'.repeat(2));// 你好你好
```

#### 15. padStart() 和 padEnd()：补齐字符串

`padStart(targetLength, padString)` 在开头补齐，`padEnd(targetLength, padString)` 在末尾补齐。

常见用途是把一位数的小时、分钟或秒补成两位数：

```javascript
const hour = String(8);
const minute = String(5);

console.log(hour.padStart(2, '0'));   // 08
console.log(minute.padStart(2, '0')); // 05
console.log('7'.padEnd(3, '0'));      // 700
```

如果原字符串已经达到指定长度，就不会继续补齐：

```javascript
console.log('12'.padStart(2, '0')); // 12
```

#### 字符串练习

1. 统计字符串 `abcdeereeryhhssqqq` 中出现次数最多的字符及其次数。
2. 去除 `abcdeereeryhhssqqq` 中的重复字符。
3. 编写函数判断字符串是否为回文，例如“上海自来水来自海上”。
4. 编写函数删除字符串中的所有数字。
5. 编写函数翻转字符串。

### 15.2 Math：数学对象

`Math` 提供数学常量和数学计算方法，不需要使用 `new` 创建。

| 方法或属性           | 作用                         | 示例结果                     |
| -------------------- | ---------------------------- | ---------------------------- |
| `Math.PI`            | 圆周率                       | 约 `3.14159`                 |
| `Math.round(x)` 要会 | 四舍五入                     | `Math.round(6.8)` 得到 `7`   |
| `Math.pow(x, y)`     | 计算 `x` 的 `y` 次幂         | `Math.pow(8, 2)` 得到 `64`   |
| `Math.sqrt(x)`       | 计算平方根                   | `Math.sqrt(64)` 得到 `8`     |
| `Math.abs(x)`        | 计算绝对值                   | `Math.abs(-4.7)` 得到 `4.7`  |
| `Math.ceil(x)` 要会  | 向上取整                     | `Math.ceil(6.4)` 得到 `7`    |
| `Math.floor(x)`要会  | 向下取整                     | `Math.floor(2.7)` 得到 `2`   |
| `Math.min(...)`要会  | 求最小值                     | `Math.min(3, 1, 8)` 得到 `1` |
| `Math.max(...)`要会  | 求最大值                     | `Math.max(3, 1, 8)` 得到 `8` |
| `Math.random()`要会  | 返回 `[0, 1)` 范围内的随机数 | 每次可能不同 包含0 不包含1   |

```javascript
console.log(Math.round(2.3)); // 2
console.log(Math.pow(8, 2));  // 64
console.log(Math.sqrt(64));   // 8
console.log(Math.abs(-4.7));  // 4.7
console.log(Math.ceil(6.4));  // 7
console.log(Math.floor(2.7)); // 2
console.log(Math.min(0, 450, 35, 10, -8, -300, -78)); // -300
console.log(Math.max(0, 450, 35, 10, -8, -300, -78)); // 450
```

#### 生成随机整数

```javascript
// 0～9
const num1 = Math.floor(Math.random() * 10);

// 0～10
const num2 = Math.floor(Math.random() * 11);

// 1～10
const num3 = Math.floor(Math.random() * 10) + 1;
```

#### 案例：猜数字小游戏

```javascript
const answer = Math.floor(Math.random() * 10) + 1;
let guess;

while (guess !== answer) {
  const input = prompt('请输入 1～100 之间的整数，点击取消可退出');

  if (input === null) {
    break;
  }

  guess = Number(input);

  if (Number.isNaN(guess)) {
    alert('请输入有效数字');
  } else if (guess > answer) {
    alert('猜大了');
  } else if (guess < answer) {
    alert('猜小了');
  } else {
    alert('恭喜你，猜对了！');
  }
}
```

### 15.3 Date：日期对象

#### 1. 创建日期对象

创建当前日期和时间：

```javascript
const now = new Date();
console.log(now);
```

使用年、月、日、时、分、秒、毫秒创建：

```javascript
const date = new Date(2018, 11, 24, 10, 33, 30, 0);
```

注意：JavaScript 的月份从 `0` 开始，一月是 `0`，十二月是 `11`。

也可以省略后面的时间部分：

```javascript
const date1 = new Date(2018, 11, 24, 10, 33, 30);
const date2 = new Date(2018, 11);
```

不能只写一个年份来表示日期。只有一个数字参数时，该参数会被当作毫秒时间戳：

```javascript
const date = new Date(2018);
```

使用毫秒时间戳创建，起点是 1970 年 1 月 1 日 00:00:00 UTC：

```javascript
const date = new Date(0);
```

使用日期字符串创建：

```javascript
const date = new Date('2018-12-24 10:33:30');
```

#### 2. 日期获取方法

| 方法                | 描述                                 |
| ------------------- | ------------------------------------ |
| `getDate()`         | 获取一个月中的日期，范围 1～31       |
| `getDay()`          | 获取星期，星期日为 0，星期六为 6     |
| `getFullYear()`     | 获取四位年份                         |
| `getHours()`        | 获取小时，范围 0～23                 |
| `getMilliseconds()` | 获取毫秒，范围 0～999                |
| `getMinutes()`      | 获取分钟，范围 0～59                 |
| `getMonth()`        | 获取月份，范围 0～11                 |
| `getSeconds()`      | 获取秒，范围 0～59                   |
| `getTime()`         | 获取从 1970 年 1 月 1 日至今的毫秒数 |

```javascript
const now = new Date();

console.log(now.getFullYear());
console.log(now.getMonth() + 1); // 显示月份时需要加 1
console.log(now.getDate());
console.log(now.getDay());
console.log(now.getHours());
console.log(now.getMinutes());
console.log(now.getSeconds());
```

#### 3. 日期设置方法

设置方法会直接修改原来的日期对象，并返回修改后的毫秒时间戳。

| 方法                            | 参数                             | 参数含义                                 |
| ------------------------------- | -------------------------------- | ---------------------------------------- |
| `setFullYear(year, month, day)` | 必填 `year`，可选 `month`、`day` | 设置年份，可同时设置月份和日期           |
| `setMonth(month, day)`          | 必填 `month`，可选 `day`         | 设置月份，`0` 表示 1 月，`11` 表示 12 月 |
| `setDate(day)`                  | 必填 `day`                       | 设置一个月中的第几天，`1`～`31`          |
| `setHours(hour, min, sec, ms)`  | 必填 `hour`，后三个可选          | 设置小时，可依次设置分、秒、毫秒         |
| `setMinutes(min, sec, ms)`      | 必填 `min`，后两个可选           | 设置分钟，可依次设置秒、毫秒             |
| `setSeconds(sec, ms)`           | 必填 `sec`，`ms` 可选            | 设置秒，可同时设置毫秒                   |
| `setMilliseconds(ms)`           | 必填 `ms`                        | 设置毫秒，`0`～`999`                     |
| `setTime(timestamp)`            | 必填 `timestamp`                 | 用毫秒时间戳设置时间                     |

重点理解两条规则：

1. **月份从 0 开始**：`0` 是 1 月，`11` 是 12 月，和 `getMonth()` 一致。
2. **后面的可选参数不写，就保留原来的值**。

设置年份、月份、日期：

```javascript
const date = new Date('2026-11-08 10:30:00');

date.setFullYear(2030, 0, 1);

console.log(date.getFullYear(), date.getMonth() + 1, date.getDate()); // 2030 1 1
console.log(date.getHours()); // 10，时间未传入，保持不变
```

只改年份，月份和日期保留原值：

```javascript
const date = new Date('2026-11-08 10:30:00');

date.setFullYear(2030);

console.log(date.getFullYear(), date.getMonth() + 1, date.getDate()); // 2030 11 8
```

设置时分秒：

```javascript
const date = new Date('2026-11-08 10:30:00');

date.setHours(20, 15, 30);

console.log(date.getHours(), date.getMinutes(), date.getSeconds()); // 20 15 30
```

用时间戳设置时间：

```javascript
const date = new Date();
date.setTime(1793588411860);

console.log(date.getFullYear()); // 2026
```

**参数超出范围时会自动进位**，这一点在实际开发中很有用：

```javascript
const date = new Date('2026-11-08');

date.setMonth(12); // 12 表示第 13 个月，自动进位到下一年 1 月
console.log(date.getFullYear(), date.getMonth() + 1); // 2027 1

date.setDate(0); // 0 表示上个月的最后一天
console.log(date.getMonth() + 1, date.getDate()); // 12 31
```

#### 4. 获取当前时间戳

时间戳是从 1970 年 1 月 1 日 00:00:00 UTC 到现在的毫秒数。以下三种写法结果相同：

```javascript
console.log(new Date().getTime()); // 1793588411860
console.log(new Date().valueOf()); // 1793588411860
console.log(+new Date());          // 1793588411860
```

`+new Date()` 是一元加号把日期对象隐式转换成数字，效果与 `getTime()` 一样。

#### 5. 时间戳转换为时间

把时间戳传给 `new Date()`，就能得到对应的时间对象：

```javascript
const timestamp = 1793588411860;
const date = new Date(timestamp);

console.log(date); // 对应的完整时间
```

如果需要 `年-月-日 时:分:秒` 格式，可以封装成一个函数：

```javascript
function formatTime(timestamp) {
  const date = new Date(timestamp);

  const y = date.getFullYear();
  const m = String(date.getMonth() + 1).padStart(2, '0');
  const d = String(date.getDate()).padStart(2, '0');
  const h = String(date.getHours()).padStart(2, '0');
  const min = String(date.getMinutes()).padStart(2, '0');
  const s = String(date.getSeconds()).padStart(2, '0');

  return `${y}-${m}-${d} ${h}:${min}:${s}`;
}

console.log(formatTime(1793588411860)); // 2026-11-02 11:00:11
console.log(formatTime(Date.now()));     // 当前时间
```

`Date.now()` 是获取当前时间戳的另一种写法，等价于 `new Date().getTime()`。

#### 6. 时间转换为时间戳

把日期字符串或日期对象转成时间戳：

```javascript
console.log(new Date('2026-11-08 10:00:00').getTime()); // 1794103200000
console.log(new Date(2026, 10, 8, 10, 0, 0).getTime()); // 1794103200000
```

注意：`new Date(2026, 10, 8)` 中的月份从 `0` 开始，`10` 表示 11 月；而字符串 `'2026-11-08'` 中的 `11` 就是 11 月。

#### 7. 案例：抢购倒计时

```javascript
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <title>年月日时分秒</title>
</head>
<body>
  <div id="time" style="font-size:24px;color: #f00;"></div>

  <script>
    const timeDom = document.getElementById('time')

    function showTime() {
      const now = new Date()
      const y = now.getFullYear() // 年份，用这个，不要getYear()
      const m = String(now.getMonth() + 1).padStart(2, '0') // 月份从0开始！+1
      const d = String(now.getDate()).padStart(2, '0')

      const h = String(now.getHours()).padStart(2, '0')
      const min = String(now.getMinutes()).padStart(2, '0')
      const s = String(now.getSeconds()).padStart(2, '0')

      timeDom.innerText = `${y}-${m}-${d} ${h}:${min}:${s}`
    }

    showTime()
    setInterval(showTime, 1000)
  </script>
</body>
</html>

```

如果需要让倒计时每秒自动更新，可以结合后面学习的 `setInterval()` 定时器。

---

## 16. JavaScript DOM

DOM（Document Object Model，文档对象模型）把 HTML 文档表示为由节点组成的树状结构。JavaScript 可以通过 DOM 获取、修改、创建和删除页面节点。

### 16.1 HTML DOM 树

根据 HTML DOM 标准，文档中的内容都可以看作节点：

1. 整个文档是文档节点。document
2. 每个 HTML 元素是元素节点。
3. HTML 元素中的文本是文本节点。
4. HTML 元素的属性是属性节点。
5. 注释是注释节点。

### 16.2 获取 HTML 元素

假设页面结构如下：

```html
<h1 id="title" class="heading">JavaScript DOM</h1>
<p class="content">第一段</p>
<p class="content">第二段</p>
```

#### 1. 通过 id 获取

```javascript
const title = document.getElementById('title');
```

#### 2. 通过标签名获取

```javascript
const paragraphs = document.getElementsByTagName('p');
```

#### 3. 通过类名获取

```javascript
const contents = document.getElementsByClassName('content');
```

#### 4. 通过 CSS 选择器获取一个元素

```javascript
const title = document.querySelector('#title');
const firstPar = document.querySelector('.content');
```

#### 5. 通过 CSS 选择器获取全部匹配元素

```javascript
const par = document.querySelectorAll('.content');
```

#### 6. 获取特殊节点

```javascript
console.log(document.body);            // body 元素
console.log(document.documentElement); // html 元素
```

### 16.3 获取和修改节点内容

```html
<p id="message">原来的内容</p>
<input id="username" value="张三">
```

#### 1. innerText

`innerText` 获取或设置文本内容，不解析 HTML 标签。

```javascript
const message = document.querySelector('#message');

console.log(message.innerText);
message.innerText = '<strong>新的文本</strong>';
```

页面会原样显示 `<strong>新的文本</strong>`。

#### 2. innerHTML

`innerHTML` 会解析 HTML 标签。

```javascript
message.innerHTML = '<strong>新的文本</strong>';
```

插入不可信的用户输入时要谨慎使用 `innerHTML`，避免引入安全问题。

#### 3. value

`value` 用于获取或设置表单元素的值。

```javascript
const input = document.querySelector('#username');

console.log(input.value);
input.value = '李四';
```

### 16.4 获取和修改属性

常见属性包括 `href`、`src`、`title`、`alt` 等。

```html
<a id="link" href="https://www.example.com" title="示例网站">访问网站</a>
<img id="logo" src="./old.png" alt="旧图片">
```

```javascript
const link = document.querySelector('#link');
const image = document.querySelector('#logo');

console.log(link.href);
link.title = '新的提示文字';

image.src = './new.png';
image.alt = '新图片';
```

### 16.5 表单元素属性操作

常见属性包括：

- `type`：表单控件类型。
- `value`：表单控件的值。
- `checked`：复选框或单选框是否选中。
- `selected`：下拉选项是否选中。
- `disabled`：是否禁用，`true` 表示禁用。

```html
<input id="agree" type="checkbox">
<button id="submit" type="button">提交</button>
```

```javascript
const checkbox = document.querySelector('#agree');
const button = document.querySelector('#submit');

checkbox.checked = true;
button.disabled = true;
```

### 16.6 修改节点样式

#### 1. 使用 style 修改行内样式

```javascript
const title = document.querySelector('#title');
title.style.color = 'red';
title.style.fontSize = '32px';
```

#### 2. 使用 className 修改类名

```javascript
title.className = 'active';
```

#### 3. 使用 classList 操作类名

```javascript
title.classList.add('active');
title.classList.remove('active');
title.classList.toggle('active');
- 如果它**没有** `active` 类 → 添加 `active`
- 如果它**已经有** `active` 类 → 移除 `active`
```

旧代码中也可能通过 `setAttribute('class', 'active')` 设置类名，通过getAttribute(属性名)来获取属性：

```javascript
title.setAttribute('class', 'active');
```

### 16.7 通过节点关系获取节点

节点之间常见的关系包括父节点、子节点和同胞节点。

```html
<ul id="list">
  <li>第一项</li>
  <li>第二项</li>
</ul>
```

```javascript
const list = document.querySelector('#list');
const firstItem = list.children[0];

console.log(firstItem.parentElement);          // 父元素
console.log(list.children);                   // 子元素集合
console.log(firstItem.nextElementSibling);     // 下一个同胞元素
console.log(list.children[1].previousElementSibling); // 上一个同胞元素
```

### 16.8 创建、添加和删除节点

```html
<ul id="list"></ul>
<button id="remove" type="button">删除第一项</button>
```

```javascript
const list = document.querySelector('#list');

// 创建并添加节点
const item = document.createElement('li');
item.innerText = '新增项目';
list.appendChild(item);

// 删除节点
document.querySelector('#remove').addEventListener('click', function () {
  if (list.firstElementChild) {
    list.removeChild(list.firstElementChild);
  }
});
```

也可以直接调用元素自身的 `remove()` 删除当前元素，不需要先获取父节点：

```html
<ul id="list">
  <li>第一项</li>
  <li>第二项</li>
</ul>
<button id="removeLast" type="button">删除最后一项</button>
```

```javascript
const list = document.querySelector('#list');

document.querySelector('#removeLast').addEventListener('click', function () {
  const lastItem = list.lastElementChild;

  if (lastItem) {
    lastItem.remove(); // 自己删除自己
  }
});
```

两者区别：

| 写法                         | 说明                                 |
| ---------------------------- | ------------------------------------ |
| `父节点.removeChild(子节点)` | 必须通过父节点调用，删除指定的子节点 |
| `子节点.remove()`            | 元素自己调用，直接删除自身，更简洁   |

如果是“删除自己”，例如给每个列表项加一个删除按钮，用 `remove()` 更方便：

```html
<ul id="todo">
  <li>学习 JavaScript <button type="button">删除</button></li>
  <li>练习 DOM 操作 <button type="button">删除</button></li>
</ul>
```

```javascript
document.querySelectorAll('#todo button').forEach(function (button) {
  button.addEventListener('click', function () {
    button.parentElement.remove(); // 删除按钮所在的整个 li
  });
});
```

这里按钮删除的是它的父元素 `li`，所以先通过 `parentElement` 找到父节点，再调用 `remove()`。

### 16.9 元素注册事件

事件处理通常分为三步：

1. 获取事件源，也就是需要响应事件的元素。
2. 注册事件。
3. 编写事件处理逻辑。

常见事件：

| 事件属性      | 描述                             |
| ------------- | -------------------------------- |
| `onchange`    | 表单元素的值改变并提交变化时触发 |
| `onclick`     | 用户点击元素                     |
| `onmouseover` | 鼠标移入元素                     |
| `onmouseout`  | 鼠标移出元素                     |
| `onkeydown`   | 用户按下键盘按键                 |
| `onkeyup`     | 用户松开键盘按键                 |
| `onload`      | 浏览器完成页面加载               |

#### 常见事件小案例

下面用一个完整页面演示这 7 个事件，可以直接保存为 `event.html` 用浏览器打开。

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <title>常见事件演示</title>
</head>
<body>
  <input id="name" placeholder="输入内容后按回车">
  <button id="btn">点击我</button>
  <div id="box" style="width:120px;height:60px;background:#eee;">鼠标移入移出</div>
  <input id="keyInput" placeholder="在这里按键">
  <p id="result"></p>

  <script>
    const nameInput = document.getElementById('name');
    const btn = document.getElementById('btn');
    const box = document.getElementById('box');
    const keyInput = document.getElementById('keyInput');
    const result = document.getElementById('result');

    // onchange：输入框的值改变并失去焦点后触发
    nameInput.onchange = function () {
      result.innerText = 'onchange：' + this.value;
    };

    // onclick：点击按钮
    btn.onclick = function () {
      result.innerText = 'onclick：按钮被点击';
    };

    // onmouseover：鼠标移入，改变背景色
    box.onmouseover = function () {
      this.style.background = 'orange';
    };

    // onmouseout：鼠标移出，恢复背景色
    box.onmouseout = function () {
      this.style.background = '#eee';
    };

    // onkeydown：按下键盘时触发
    keyInput.onkeydown = function (event) {
      result.innerText = 'onkeydown：按下了 ' + event.key;
    };

    // onkeyup：松开键盘时触发
    keyInput.onkeyup = function (event) {
      result.innerText = 'onkeyup：松开了 ' + event.key;
    };

    // onload：页面加载完成后执行
    window.onload = function () {
      console.log('onload：页面加载完成');
    };
  </script>
</body>
</html>
```

各事件的触发时机和案例效果：

| 事件          | 触发时机                   | 案例中的效果             |
| ------------- | -------------------------- | ------------------------ |
| `onchange`    | 输入框的值改变并失去焦点后 | 页面显示输入的内容       |
| `onclick`     | 鼠标点击元素时             | 页面显示“按钮被点击”     |
| `onmouseover` | 鼠标移入元素时             | 方块背景变橙色           |
| `onmouseout`  | 鼠标移出元素时             | 方块背景恢复灰色         |
| `onkeydown`   | 按下键盘按键时             | 显示按下的按键名         |
| `onkeyup`     | 松开键盘按键时             | 显示松开的按键名         |
| `onload`      | 页面所有资源加载完成后     | 控制台输出“页面加载完成” |

注意 `onchange` 和 `oninput` 的区别：`onchange` 要等输入框失去焦点才触发，`oninput` 每输入一个字符就触发。

#### 方式一：事件属性

```javascript
const button = document.querySelector('button');

button.onclick = function () {
  console.log('按钮被点击');
};
```

移除事件：

```javascript
button.onclick = null;
```

#### 方式二：addEventListener()

```javascript
function handleClick() {
  console.log('按钮被点击');
}

button.addEventListener('click', handleClick);
```

移除事件时，事件类型和函数必须与注册时一致：

```javascript
button.removeEventListener('click', handleClick);
```

`addEventListener()` 可以为同一个元素、同一个事件注册多个处理函数，实际开发中更常用。

### 16.10 事件的传播

事件传播通常包括三个阶段：

1. 捕获阶段：`document → html → body → 目标节点`。
2. 目标阶段：事件到达目标节点。
3. 冒泡阶段：`目标节点 → body → html → document`。

`addEventListener()` 的第三个参数可以指定是否在捕获阶段处理事件：

```javascript
element.addEventListener('click', handleClick, true);  // 捕获阶段
element.addEventListener('click', handleClick, false); // 冒泡阶段，默认
```

### 16.11 事件对象

事件处理函数可以接收事件对象，常用属性和方法如下：

| 属性或方法                | 说明                                |
| ------------------------- | ----------------------------------- |
| `event.target`            | 返回实际触发事件的元素              |
| `event.type`              | 返回事件类型，如 `click`，不带 `on` |
| `event.preventDefault()`  | 阻止元素的默认行为                  |
| `event.stopPropagation()` | 阻止事件继续传播                    |

```javascript
const link = document.querySelector('a');

link.addEventListener('click', function (event) {
  event.preventDefault();
  console.log(event.target);
  console.log(event.type);
});
```

旧式 IE 代码中还可能看到：

- `event.srcElement`：类似 `event.target`。
- `event.cancelBubble`：用于阻止冒泡。
- `event.returnValue`：用于控制默认行为。

现代开发优先使用标准属性和方法。

### 16.12 案例一：轮播图

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <title>优化版原生轮播图</title>
  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }
    .wrap {
      width: 600px;
      height: 300px;
      margin: 30px auto;
      position: relative;
      overflow: hidden;
    }
    .img-box {
      width: 3000px;
      height: 300px;
      display: flex;
      transition: left 0.4s ease;
      position: absolute;
      left: 0;
      top: 0;
    }
    .img-box img {
      width: 600px;
      height: 300px;
      display: block;
    }
    .prev, .next {
      width: 40px;
      height: 60px;
      background: rgba(0,0,0,0.4);
      color: #fff;
      text-align: center;
      line-height: 60px;
      font-size: 24px;
      position: absolute;
      top: 50%;
      transform: translateY(-50%);
      cursor: pointer;
      z-index: 9;
      user-select: none;
    }
    .prev { left: 0; }
    .next { right: 0; }
    .dots {
      position: absolute;
      bottom: 15px;
      left: 50%;
      transform: translateX(-50%);
      display: flex;
      gap: 8px;
    }
    .dot {
      width: 12px;
      height: 12px;
      border-radius: 50%;
      background: rgba(255,255,255,0.5);
      cursor: pointer;
    }
    .dot.active {
      background: #fff;
    }
  </style>
</head>
<body>
  <div class="wrap">
    <div class="img-box">
      <img src="https://picsum.photos/id/1/600/300" alt="">
      <img src="https://picsum.photos/id/2/600/300" alt="">
      <img src="https://picsum.photos/id/3/600/300" alt="">
      <img src="https://picsum.photos/id/4/600/300" alt="">
      <img src="https://picsum.photos/id/5/600/300" alt="">
    </div>
    <div class="prev">&lt;</div>
    <div class="next">&gt;</div>
    <div class="dots"></div>
  </div>

  <script>
    // 获取元素
    const wrap = document.querySelector('.wrap')
    const imgBox = document.querySelector('.img-box')
    const prevBtn = document.querySelector('.prev')
    const nextBtn = document.querySelector('.next')
    const dotsWrap = document.querySelector('.dots')

    // 配置
    const imgWidth = 600
    const total = 5
    let index = 0
    let timer = null

    // 渲染：移动图片 + 更新圆点
    function render() {
      imgBox.style.left = -index * imgWidth + 'px'
      const dots = document.querySelectorAll('.dot')
      dots.forEach(dot => dot.classList.remove('active'))
      dots[index].classList.add('active')
    }

    // 下一张
    function goNext() {
      index++
      if (index >= total) index = 0
      render()
    }

    // 上一张
    function goPrev() {
      index--
      if (index < 0) index = total - 1
      render()
    }

    // 开启自动轮播
    function startAuto() {
      // 先清一遍，防止重复开多个定时器
      clearInterval(timer)
      timer = setInterval(goNext, 2000)
    }

    // 停止自动轮播
    function stopAuto() {
      clearInterval(timer)
    }

    // 生成圆点 + 绑定点击事件（addEventListener）
    for (let i = 0; i < total; i++) {
      const dot = document.createElement('div')
      dot.className = 'dot'
      if (i === 0) dot.classList.add('active')
      dotsWrap.appendChild(dot)

      // 圆点点击
      dot.addEventListener('click', () => {
        index = i
        render()
      })
    }

    // 按钮事件 addEventListener
    nextBtn.addEventListener('click', goNext)
    prevBtn.addEventListener('click', goPrev)

    // 鼠标悬浮事件，同样用 addEventListener
    wrap.addEventListener('mouseenter', stopAuto)
    wrap.addEventListener('mouseleave', startAuto)

    // 初始化启动
    startAuto()
  </script>
</body>
</html>


```

### 16.13 案例二：表单全选与取消全选

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <title>全选与取消全选</title>
</head>
<body>
  <label>
    <input type="checkbox" id="checkAll">
    全选
  </label>
  <hr>
  <input type="checkbox" class="checkItem"><br>
  <input type="checkbox" class="checkItem"><br>
  <input type="checkbox" class="checkItem">

  <script>
    const checkAll = document.querySelector('#checkAll');
    const items = document.querySelectorAll('.checkItem');

    // 点击全选框，同步所有子选项
    checkAll.addEventListener('click', function () {
      for (let i = 0; i < items.length; i++) {
        items[i].checked = this.checked;
      }
    });

    // 点击子选项，检查是否所有子选项都已选中
    for (let i = 0; i < items.length; i++) {
      items[i].addEventListener('click', function () {
        let allChecked = true;

        for (let j = 0; j < items.length; j++) {
          if (!items[j].checked) {
            allChecked = false;
            break;
          }
        }

        checkAll.checked = allChecked;
      });
    }
  </script>
</body>
</html>
```

### 16.14 DOM 综合练习

1. 制作简易图片轮播，点击“上一张”和“下一张”切换图片。
2. 制作随机点名小程序，点击按钮后从姓名数组中随机显示一名学生。
3. 为轮播图小圆点增加鼠标移入事件，使鼠标移入时也能切换选中状态。
4. 制作一个列表，支持动态添加和删除列表项。

---

## 17. JavaScript BOM

BOM（Browser Object Model，浏览器对象模型）提供了与浏览器窗口交互的对象和方法。`window` 是 BOM 的顶层对象，也是浏览器中的全局对象。

在全局作用域中使用 `var` 声明的变量以及使用函数声明创建的函数，会成为 `window` 的属性或方法；使用 `let`、`const` 声明的变量不会成为 `window` 的属性。

### 17.1 window 常见事件

#### 1. 窗口加载事件

`load` 事件在页面及其资源加载完成后触发。

```javascript
window.addEventListener('load', function () {
  const button = document.querySelector('button');

  button.addEventListener('click', function () {
    console.log('按钮被点击');
  });
});
```

早期写法：

```javascript
window.onload = function () {
  console.log('页面加载完成');
};
```

#### 2. 窗口尺寸改变事件

```javascript
window.addEventListener('resize', function () {
  console.log('窗口尺寸改变了');
});
```

也可以写成：

```javascript
window.onresize = function () {
  console.log('窗口尺寸改变了');
};
```

### 17.2 定时器

#### 1. 一次性定时器：setTimeout()

`setTimeout()` 在指定时间后执行一次回调函数，时间单位是毫秒。

```javascript
const timerId = window.setTimeout(function () {
  console.log('一次性定时器');
}, 1000);
```

在回调执行前，可以使用 `clearTimeout()` 取消定时器：

```javascript
window.clearTimeout(timerId);
```

#### 2. 循环定时器：setInterval()    间歇调用

`setInterval()` 每隔指定时间执行一次回调函数。

```javascript
const timerId = window.setInterval(function () {
  console.log('循环定时器');
}, 1000);
```

使用 `clearInterval()` 停止循环定时器：

```javascript
window.clearInterval(timerId);
```

定时器会返回一个标识符，清除定时器时需要传入这个标识符。

#### 3. 案例：动态显示系统时间

```html
<p id="clock"></p>

<script>
  function updateClock() {
    const now = new Date();
    document.querySelector('#clock').innerText = now.toLocaleString();
  }

  updateClock();
  setInterval(updateClock, 1000);
</script>
```

### 17.3 JavaScript 执行机制

JavaScript 的核心执行机制是单线程。单线程意味着主线程中的任务需要依次处理，前一个任务结束后，才能继续执行后一个任务。

为了处理定时器、网络请求等耗时操作，任务可以分为：

- 同步任务：在主线程上按照顺序执行。
- 异步任务：满足触发条件后，其回调进入任务队列，等待主线程空闲时执行。

#### 1. 同步任务

```javascript
console.log(1);
console.log(2);
console.log(3);
```

输出顺序：`1、2、3`。

#### 2. 同步与异步

```javascript
console.log(1);

setTimeout(function () {
  console.log(2);
}, 1000);

console.log(3);
```

输出顺序：`1、3、2`。

原因是同步代码先执行，定时器回调等待约 1 秒后再进入任务队列。

#### 3. 多个定时器

```javascript
console.log(1);

setTimeout(function () {
  console.log(2);
}, 1000);

setTimeout(function () {
  console.log(3);
}, 0);

console.log(4);
```

通常输出顺序：`1、4、3、2`。

虽然 `0` 毫秒定时器写在后面，但它会比 `1000` 毫秒定时器更早满足执行条件。需要注意，`setTimeout()` 的延迟时间表示“至少等待多久”，并不保证到达时间后立即执行。

### 17.4 location 对象

`location` 是 `window` 的属性，用于获取或设置当前页面的地址信息。

#### 1. href：获取或设置完整 URL

```javascript
console.log(location.href);
```

跳转到另一个页面：

```javascript
location.href = 'https://www.example.com';
```

#### 2. search：获取查询参数

假设从 A 页面跳转到 B 页面：

```text
detail.html?name=lisi&age=12
```

在 B 页面中：

```javascript
console.log(location.search); // ?name=lisi&age=12
```

`search` 包含以 `?` 开头的查询字符串。

#### 3. 常见方法

| 方法           | 作用                                           |
| -------------- | ---------------------------------------------- |
| `assign(url)`  | 跳转到新页面，保留当前页面历史记录，可以后退   |
| `replace(url)` | 使用新页面替换当前页面，不保留当前页面历史记录 |
| `reload()`     | 重新加载当前页面                               |

```javascript
// location.assign('detail.html');
// location.replace('detail.html');
// location.reload();
```

### 17.5 navigator 对象

`navigator` 包含浏览器相关信息，例如用户代理字符串：

```javascript
console.log(window.navigator.userAgent);
```

### 17.6 history 对象

`history` 用于操作当前标签页的浏览历史。

| 方法        | 作用                                   |
| ----------- | -------------------------------------- |
| `back()`    | 后退一页                               |
| `forward()` | 前进一页                               |
| `go(n)`     | 前进或后退指定页数，负数后退，正数前进 |

```html
<a href="index.html">跳转主页面</a>
<button id="back" type="button">后退</button>

<script>
  document.querySelector('#back').addEventListener('click', function () {
    history.go(-1);
    // 也可以写成 history.back();
  });
</script>
```

### 17.7 screen 对象

`screen` 对象包含客户端显示屏幕的信息。

```javascript
console.log(screen.width);
console.log(screen.height);
console.log(screen.availWidth);
console.log(screen.availHeight);
```

### 17.8 元素偏移量：offset 系列

`offset` 系列属性返回数字，不带单位，通常用于读取元素的位置和尺寸。

| 属性           | 说明                                     |
| -------------- | ---------------------------------------- |
| `offsetTop`    | 元素顶部距离其 `offsetParent` 顶部的距离 |
| `offsetLeft`   | 元素左侧距离其 `offsetParent` 左侧的距离 |
| `offsetWidth`  | 元素宽度，包含内容、内边距和边框         |
| `offsetHeight` | 元素高度，包含内容、内边距和边框         |
| `offsetParent` | 返回用于定位计算的父级元素               |

这些属性主要用于读取，不用于直接设置元素尺寸。

```javascript
const box = document.querySelector('#box');

console.log(box.offsetTop);
console.log(box.offsetLeft);
console.log(box.offsetWidth);
console.log(box.offsetHeight);
console.log(box.offsetParent);
```

结合鼠标事件中的 `event.pageX`、`event.pageY` 和元素位置，可以计算鼠标在指定区域中的坐标。

### 17.9 元素可视区：client 系列

| 属性           | 说明                                     |
| -------------- | ---------------------------------------- |
| `clientWidth`  | 内容宽度加左右内边距，不包含边框和滚动条 |
| `clientHeight` | 内容高度加上下内边距，不包含边框和滚动条 |
| `clientLeft`   | 元素左边框宽度                           |
| `clientTop`    | 元素上边框宽度                           |

```javascript
console.log(box.clientWidth);
console.log(box.clientHeight);
console.log(box.clientLeft);
console.log(box.clientTop);
```

### 17.10 元素滚动：scroll 系列

| 属性           | 说明                                 |
| -------------- | ------------------------------------ |
| `scrollHeight` | 元素内容的总高度，包括不可见部分     |
| `scrollWidth`  | 元素内容的总宽度，包括不可见部分     |
| `scrollLeft`   | 内容向左滚动的像素数，可以读取或设置 |
| `scrollTop`    | 内容向上滚动的像素数，可以读取或设置 |

```javascript
console.log(box.scrollHeight);
console.log(box.scrollWidth);

box.scrollTop = 100;
box.scrollLeft = 50;
```

### 17.11 BOM 综合练习

1. 动态显示系统时间，每秒更新一次。
2. 编写随机点名小程序：点击“开始”不断切换姓名，点击“结束”停止点名。
3. 使用表格展示学生姓名，并在随机点名时高亮当前姓名。
4. 完善图片轮播，使用 `setInterval()` 自动切换图片。
5. 获取鼠标在矩形区域中的坐标，并实时显示。

---

## 18. 本地存储

浏览器本地存储可以在用户电脑中保存数据。常见方式是 `localStorage` 和 `sessionStorage`。

### 18.1 localStorage

`localStorage` 用于持久化保存数据。除非主动删除、清除浏览器数据或浏览器执行清理，否则数据不会因为关闭页面或浏览器而消失。

传统 cookie 的每条数据通常只有约 4 KB，而 `localStorage` 的容量一般为几 MB，常见浏览器约为 5 MB，具体容量因浏览器而异。

### 18.2 sessionStorage

`sessionStorage` 用于会话级存储：

- 数据只在当前标签页的会话中使用。
- 页面刷新后通常仍然保留。
- 关闭对应标签页后，数据会被清除。
- 不同标签页之间的数据通常相互独立。

### 18.3 常用方法

两种存储对象的方法相同：

| 方法                  | 作用                                |
| --------------------- | ----------------------------------- |
| `setItem(key, value)` | 保存数据                            |
| `getItem(key)`        | 根据键读取数据，不存在时返回 `null` |
| `removeItem(key)`     | 删除指定数据                        |
| `clear()`             | 清空当前存储对象中的所有数据        |

#### localStorage 示例

```javascript
localStorage.setItem('username', '张三');

const username = localStorage.getItem('username');
console.log(username); // 张三

localStorage.removeItem('username');
// localStorage.clear(); // 谨慎使用，会清空所有 localStorage 数据
```

#### sessionStorage 示例

```javascript
sessionStorage.setItem('token', 'temporary-token');
console.log(sessionStorage.getItem('token'));

sessionStorage.removeItem('token');
```

### 18.4 保存对象

本地存储只能直接保存字符串。保存对象或数组时，通常先转换为 JSON 字符串；读取时再解析。

```javascript
const student = {
  name: '张三',
  age: 20
};

localStorage.setItem('student', JSON.stringify(student));

const savedStudent = JSON.parse(localStorage.getItem('student'));
console.log(savedStudent.name); // 张三
```

### 18.5 两种存储方式的区别

| 对比项     | localStorage              | sessionStorage            |
| ---------- | ------------------------- | ------------------------- |
| 生命周期   | 持久保存，主动删除后消失  | 当前标签页会话结束后清除  |
| 页面刷新   | 保留                      | 保留                      |
| 关闭标签页 | 通常保留                  | 通常清除                  |
| 容量       | 通常约 5 MB，因浏览器而异 | 通常约 5 MB，因浏览器而异 |
| 常用场景   | 用户设置、长期保存的偏好  | 当前会话中的临时数据      |

本地存储中的数据可以被用户和脚本访问，不要保存密码、支付信息等敏感数据。

---

## 学习总结

完成本课件后，应能够独立完成以下任务：

1. 使用变量、数据类型和运算符编写基础 JavaScript 程序。
2. 使用分支与循环解决奇偶判断、成绩分级、数字求和等问题。
3. 使用数组和函数组织数据、复用代码。
4. 创建自定义对象，并使用 String、Math、Date 等内置对象。
5. 使用 DOM 修改页面内容、样式、属性并处理用户事件。
6. 使用 BOM 完成定时器、页面跳转、历史记录和动态时间等功能。
7. 使用本地存储保存简单的字符串、数组和对象数据。
