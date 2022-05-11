---
title: JavaScript 学习笔记
tags: [JavaScript, notes, 学习笔记]
date: 2022-03-01 17:51:16
categories: notes
---

## 前言

2022.02.28 是我从上家公司离职的日子，心中虽还有些许不舍，但更多的还是轻松和惬意。虽然对自己的技术还是比较自信，但是面对即将踏上新的征程，不得不承认的是在生理上自己还是有些焦虑，导致晚上经常失眠，脑海中一遍遍想象着即将到来的面试场景；

在家复习了整整三周，然后又看了三天面试题，基本上都是 997；本文主要记录在家复习 JavaScript 基础所做的相应笔记；对应项目地址：<https://github.com/Coley48/javascript>

<!-- more -->

### 项目搭建

1. 安装 webpack

```sh
npm i -D webpack webpack-dev-server webpack-cli
```

2. 配置 webpack

在根目录下添加`webpack.dev.js`和`webpack.prod.js`

`webpack.dev.js`用于开发环境，`webpack.prod.js`用于生产环境；

```sh
# 生产环境中使用 babel-loader；
npm i -D @babel/core @babel/preset-env babel-loader
npm i -D @babel/plugin-transform-runtime # async/await

# 根据需要安装其他 loader 和 plugin；
npm i -D style-loader css-loader html-webpack-plugin
```

- [html-webpack-plugin](https://www.npmjs.com/package/html-webpack-plugin)

3. 配置 babel

> `Babel` 是一个 JavaScript 编译器，主要用于将采用 `ECMAScript 2015+` 语法编写的代码转换为向后兼容的 JavaScript 语法，以便能够运行在当前和旧版本的浏览器或其他环境中；

- [babel配置教程](https://www.babeljs.cn/setup#installation)

4. 最后在 `package.json` 中添加运行脚本

```json
"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "serve": "webpack server --config webpack.dev.js",
    "build": "webpack --config webpack.prod.js"
},
```

### JavaScript 基础

**什么是 JavaScript ？**

> JavaScript 最初被创建的目的是“使网页更生动”；这种编程语言写出来的程序被称为脚本，它们可以被直接写在网页的 `HTML` 中，在页面加载的时候自动执行；脚本被以纯文本的形式提供和执行，它们不需要特殊的准备或编译即可运行；

JavaScript 引擎工作基本原理：引擎解析脚本，然后将脚本编译转化为机器语言，最后执行机器语言；

**浏览器中的 JavaScript 限制**

为了用户的（信息）安全，在浏览器中的 JavaScript 的能力是受限的，目的是防止恶意网页获取用户私人信息或损害用户数据；网页中的 JavaScript 不能读、写、复制和执行硬盘上的任意文件，不同的标签页/窗口之间通常互不了解；

**JavaScript 手册**

- [JavaScript MDN 中文参考手册](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference)
- [JavaScript 兼容性表](https://caniuse.com/)
<!-- 
#### script 标签

应该尽量将较复杂的脚本存放在单独的文件中，使用独立文件的好处是浏览器会下载它，然后将它保存到浏览器的缓存中；之后，其他页面想要相同的脚本就会从缓存中获取，所以文件实际上只会下载一次，这可以节省流量，并使得页面（加载）更快；

> Note: 如果设置了 `src` 特性，`script` 标签内容将会被忽略；

JavaScript 代码以分号结尾，注意添加完整分号，以下代码会报错；

```js
alert("Hello")

// Uncaught TypeError: Cannot read properties of undefined (reading '2')
[1, 2].forEach(alert);
``` -->

#### 变量

1. 必须仅包含字母，数字，符号 $ 和 _；
2. 首字符必须非数字；
3. 通常使用驼峰命名规则；
4. 对大小写敏感；
5. 不能使用[保留字](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Lexical_grammar#%E5%85%B3%E9%94%AE%E5%AD%97)命名；
6. 允许非英文字母，但不推荐；

#### 数据类型

在 JavaScript 中有 8 种基本的数据类型（译注：7 种原始类型和 1 种引用类型）；JavaScript，被称为“动态类型”（dynamically typed）的编程语言，意思是虽然编程语言中有不同的数据类型，但是你定义的变量并不会在定义后，被限制为某一数据类型；

- Number 用于任何类型的数字：整数或浮点数，在 ±(2^53-1) 范围内的整数；

```js
// Infinity 代表数学概念中的 无穷大 ∞；是一个比任何数字都大的特殊值；
console.log(1 / 0 === Infinity); // true

// NaN 代表一个计算错误；它是一个不正确的或者一个未定义的数学操作所得到的结果；
// 任何对 NaN 的进一步数学运算都会返回 NaN，且 NaN 不等于任何值，包括自己；
console.log('a' / 2 === NaN); // false
```

- BigInt 用于任意长度的整数；

Number 类型无法表示大于 (253-1)（即 9007199254740991），或小于 -(253-1) 的整数；目前 IE 还未支持该类型；

```js
// 尾部的 "n" 表示这是一个 BigInt 类型
const bigInt = 1234567890123456789012345678901234567890n;
```

- String 用于字符串：一个字符串可以包含 0 个或多个字符，所以没有单独的单字符类型；

双引号和单引号都是“简单”引用，在 JavaScript 中两者几乎没有什么差别；反引号是功能扩展引号，它们允许我们通过将变量和表达式包装在 ${…} 中，来将它们嵌入到字符串中；

```js
let str = "Hello";
let str2 = 'Single quotes are ok too';
let phrase = `can embed another ${str}`; // 模板字符串
```

- Boolean 用于 true 和 false；表示逻辑类型；
- null 用于未知的值，只有一个 null 值的独立类型；
- undefined 用于未定义的值，只有一个 undefined 值的独立类型，代表未被赋值；

> Note: 不建议显示地将 `undefined` 赋值给变量，通常，使用 `null` 将一个“空”或者“未知”的值写入变量中，而 `undefined` 则保留作为未进行初始化的事物的默认初始值；

> Tips: `void` 并不改变表达式的结果，只是让表达式不返回值；因此可以用 `void 0` 来获得 `undefined`；

- Symbol 用于唯一的标识符；
- Object 用于更复杂的数据结构；

当我们想要分别处理不同类型值的时候，或者想快速进行数据类型检验时，可以使用 typeof 运算符返回参数的类型；

```js
// 这是官方承认的 typeof 的错误，这个问题来自于 JavaScript 语言的早期阶段，并为了兼容性而保留了下来；
typeof null // "object"

// 在 JavaScript 语言中没有一个特别的 “function” 类型，函数隶属于 object 类型，但是 typeof 会对函数区分对待，并返回 "function"；
typeof alert // "function"
```
   
#### 类型转换

- 字符串转换：转换发生在输出内容的时候，也可以通过 String(value) 进行显式转换；
- 数字型转换：转换发生在进行算术操作时，也可以通过 Number(value) 进行显式转换；

| 值 | 转换 |
|:----|:----|
| undefined | NaN |
| null | 0 |
| true / false | 1 / 0 |
| string | 忽略字符串两端的空白，按原样读取，空字符串变成 0，转换出错则输出 NaN |

- 布尔型转换：转换发生在进行逻辑操作时，也可以通过 Boolean(value) 进行显式转换；

| 值 | 转换 |
|:----|:----|
| 0, null, undefined, NaN, "" | false |
| 其他值 | true |

#### 基础运算符，数学

**赋值运算符**

赋值语句 x = value 将值 value 写入 x 然后返回 x；

```js
let a = 1;
let b = 2;

let c = 3 - (a = b + 1); // a: 3, c: 0 慎用！
```

链式赋值从右到左进行计算，但可读性不高；

**自增 / 自减**

++, --，又分为前置形式和后置形式，前置形式返回一个新的值，但后置返回原来的值（做加法/减法之前的值）；

[JavaScript 运算符优先级表](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Operator_Precedence)

#### 值的比较

在比较字符串的大小时，JavaScript 会使用“字典（dictionary）”或“词典（lexicographical）”顺序进行判定，换言之，字符串是按字符（母）逐个进行比较的；

字符串的比较算法：
1. 首先比较两个字符串的首位字符大小；
2. 如果一方字符较大（或较小），则该字符串大于（或小于）另一个字符串，算法结束；
3. 否则，如果两个字符串的首位字符相等，则继续取出两个字符串各自的后一位字符进行比较；
4. 重复上述步骤进行比较，直到比较完成某字符串的所有字符为止；
5. 如果两个字符串的字符同时用完，那么则判定它们相等，否则未结束（还有未比较的字符）的字符串更大；

```js
alert( 'Z' > 'A' ); // true
alert( 'Glow' > 'Glee' ); // true
alert( 'Bee' > 'Be' ); // true
```

当对不同类型的值进行比较时，JavaScript 会首先将其转化为数字（number）再判定大小；

```js
// 当使用严格等于 === 时，不相等，因为它们属于不同的类型；
alert( null === undefined ); // false

// 当使用非严格等于 == 时，相等，仅仅等于对方而不等于其他任何的值；
alert( null == undefined ); // true

// 当使用数学式或其他比较方法 < > <= >= 时，null/undefined 会被转化为数字：null 被转化为 0，undefined 被转化为 NaN；
alert( null > undefined ); // false

// 相等性检查 == 和普通比较符 > < >= <= 的代码逻辑是相互独立的
alert( null > 0 );  // false
alert( null == 0 ); // false
alert( null >= 0 ); // true

// undefined 和 null 在相等性检查 == 中不会进行任何的类型转换；
// 它们有自己独立的比较规则，所以除了它们之间互等外，不会等于任何其他的值
alert( undefined > 0 ); // false
alert( undefined < 0 ); // false
alert( undefined == 0 ); // false
```

#### 逻辑运算符 if 和 ?:

- 或运算符 || 返回的值是操作数的初始形式，不会做布尔转换；

执行过程：

1. 从左到右依次计算操作数；
2. 将每一个操作数转化为布尔值；若结果为 true，则返回该操作数的初始值；
3. 如果所有的操作数都被计算过（转换结果都是 false），则返回最后一个操作数；

常见用法：
1. 获取变量列表或者表达式中的第一个真值；
2. 短路求值（Short-circuit evaluation）；

- 与运算符 && 返回的值是第一个假值的初始形式；
- 非运算符 ! 返回操作数的取反布尔值；

> Tips: 两个非运算 `!!` 有时候用来将某个值转化为布尔类型：

#### 空值合并运算符 ??

`a ?? b` 表示如果第一个参数不是 null/undefined，则 ?? 返回第一个参数，否则，返回第二个参数；


```js
// ?? 等价写法：
result = (a !== null && a !== undefined) ? a : b;
```

与 || 比较：

- || 返回第一个真值；
- ?? 返回第一个已定义的值；
- 优先级相同；

出于安全原因，JavaScript 禁止将 ?? 运算符与 && 和 || 运算符一起使用，除非使用括号明确指定了优先级；

#### 循环

跳出多层循环：

```js
outer: for (let i = 0; i < 3; i++) {

    for (let j = 0; j < 3; j++) {

      let input = prompt(`Value at coords (${i},${j})`, '');
  
      // 如果是空字符串或被取消，则中断并跳出这两个循环
      if (!input) break outer; // (*)
  
      // 用得到的值做些事……
    }
}
```

> Tips: `break` 指令必须在代码块内，从技术上讲，任何被标记的代码块都有效；

#### 函数

在 JavaScript 中，函数是一种特殊的值，函数定义有函数声明和函数表达式两种；二者差别主要在，函数表达式是在代码执行到达时被创建，并且仅从那一刻起可用；而函数声明则不同，在函数声明被定义之前，它就可以被调用；

函数就是行为（action），所以它们的名字通常是动词，它应该简短且尽可能准确地描述函数的作用，这样读代码的人就能清楚地知道这个函数的功能；一种普遍的做法是用动词前缀来开始一个函数，这个前缀模糊地描述了这个行为；

空值的 return 或没有 return 的函数返回值为 undefined；

一个函数应该只包含函数名所指定的功能，而不是做更多与函数名无关的功能；两个独立的行为通常需要两个函数，即使它们通常被一起调用；在这种情况下，我们可以创建第三个函数来调用这两个函数；

#### 在浏览器中调试

> Tips: 在控制台中要插入多行代码，按 `Shift+Enter` 来进行换行；

- “下一步（Step）”：运行下一条指令，快捷键 F9；
- “跨步（Step over）”：运行下一条指令，但 不会进入到一个函数中，快捷键 F10；
- “步入（Step into）”，快捷键 F11；
- “步出（Step out）”：继续执行到当前函数的末尾，快捷键 Shift+F11；

> Tips: 在代码中的某一行上右键，在显示的关联菜单中点击一个非常有用的名为 `Continue to here` 的选项；

#### 注释

- 需要注释：

1. 整体架构，高层次的观点；
2. 函数的用法；
3. 重要又不是很明显的解决方案；
  
- 避免注释：

1. 描述“代码如何工作”和“代码做了什么”；
2. 已经足够简单或有很好自描述性的代码；

#### 忍者代码

1. 不要为了简洁而降低代码可读性；
1. 不用一个字母的变量和单词缩写；
2. 不用无意义的变量名；
3. 避免同义词命名变量或函数；
4. 不在函数或循环中替换变量值；
5. 不在变量名前添加下划线；
6. 避免使用和外部变量同名的变量名；
7. 除了主要任务之外，避免给函数添加一个其他行为；
8. 避免返回非标准结果的函数；
9. 避免将多个功能合并在一个函数中；

#### 测试

前端测试主要分为 3 种：单元测试（Unit Test）、集成测试（Integration Test）、UI 测试（UI Test）；

- 单元测试是最容易实现的：代码中多个组件共用的工具类库、多个组件共用的子组件等；
- 集成测试通常被应用在：耦合度较高的函数/组件、经过二次封装的函数/组件、多个函数/组件组合而成的函数/组件等；
- UI 测试只是对于前端的测试，是脱离真实后端环境的，仅仅只是将前端放在真实环境中运行，而后端和数据都应该使用 Mock 的；

适合引入自动化测试的场景：

- 公共库类的开发维护；
- 中长期项目的迭代/重构；
- 引用了不可控的第三方依赖；


当通过手动重新运行来测试代码时，很容易漏掉一些东西；自动化测试意味着测试是独立于代码的；它们以各种方式运行我们的函数，并将结果与预期结果进行比较；

BDD（行为驱动开发）规范先行，实现在后，包含了三部分内容：测试、文档和示例；

安装单元测试库 Jest：
```sh
npm i -D jest
```

- [试试前端自动化测试！（基础篇）](https://juejin.cn/post/6844904194600599560)
- [Jest 测试框架中文文档](https://jestjs.io/zh-Hans/)
- [Vue.js Jest 单元测试](https://alexjover.com/blog/write-the-first-vue-js-component-unit-test-in-jest/)

#### 普通对象

**对象属性**

> Tips: 对象属性可以用 `delete` 操作符移除；

属性名可以是任何字符串或者 symbol，包括使用保留字；包含空格，以数字开头，或包含特殊字符（除 $ 和 _ 以外）的属性名需要加引号；

**计算属性**

在对象字面量中使用方括号定义动态属性名，括号中可以使用变量或更复杂的表达式；

```js
let fruit = prompt("Which fruit to buy?", "apple");

let bag = {
  [fruit]: 5, // 属性名是从 fruit 变量中得到的
};

alert( bag.apple ); // 5 如果 fruit="apple"
```


**in 操作符**

```js
let user = { name: "John", age: 30 };

alert( "age" in user ); // true
alert( "blabla" in user ); // false
```

遍历对象：for(let key in obj) 循环；

> Tips: 对象中，整数属性会被进行排序，其他属性则按照创建的顺序显示；整数属性指的是一个可以在不做任何更改的情况下与一个整数进行相互转换的字符串；

#### 对象引用和复制

> Tips: 赋值了对象的变量存储的不是对象本身，而是该对象“在内存中的地址”；换句话说就是对该对象的“引用”；

> Tips: JavaScript 变量复制，原始类型可类比为“搬家”，对象类型类比为“配钥匙”；

使用 `Object.assign(dest, [src1, src2, src3...])` 方法拷贝对象；

- 第一个参数 dest 是指目标对象；
- 一个或多个源对象 src1, ..., srcN；
- 该方法将所有源对象的属性拷贝到目标对象 dest 中；
- 调用结果返回 dest；
- 如果被拷贝的属性的属性名已经存在，则会被覆盖；

或使用 Spread 语法拷贝对象；`clone = { ...user }`

当对象中存在有其他对象的引用时，可以使用递归循环深拷贝，也可以直接使用 Lodash 定义好的方法 [_.cloneDeep(obj)](https://lodash.com/docs#cloneDeep)；

```js
function cloneDeep(object) {
  if (typeof object !== "object" || object === null) {
    return object;
  }

  const copy = {};
  for (const key in object) {
    if (Object.hasOwnProperty.call(object, key)) {
      copy[key] = cloneDeep(object[key]);
    }
  }
  return copy;
}
```

#### 垃圾回收

JavaScript 中主要的内存管理概念是可达性（Reachability），及存储在内存中并以某种方式可访问或可用的值；

固有的可达值的基本集合，也成为根（roots），包括：

1. 当前执行的函数，它的局部变量和参数；
2. 当前嵌套调用链上的其他函数、它们的局部变量和参数；
3. 全局变量；
4. 还有一些内部的根；

如果一个值可以通过引用或引用链从根访问任何其他值，则认为该值是可达的；

垃圾回收的基本算法被称为 “mark-and-sweep”，即标记清除；现代浏览器均使用此机制，老浏览器使用引用计数机制；标记算法类似于广度优先遍历；

优化建议：

- 分代收集（Generational collection）：对象被分成两组：“新的”和“旧的”；许多对象出现，完成它们的工作并很快死去，它们可以很快被清理；那些长期存活的对象会变得“老旧”，而且被检查的频次也会减少；
- 增量收集（Incremental collection）：如果有许多对象，并且我们试图一次遍历并标记整个对象集，则可能需要一些时间，并在执行过程中带来明显的延迟，所以引擎试图将垃圾收集工作分成几部分来做；然后将这几部分会逐一进行处理，这需要它们之间有额外的标记来追踪变化，但是这样会有许多微小的延迟而不是一个大的延迟；
- 闲时收集（Idle-time collection）：垃圾收集器只会在 CPU 空闲时尝试运行，以减少可能对代码执行的影响；

- [JavaScript 内存泄漏教程](https://www.ruanyifeng.com/blog/2017/04/memory-leak.html)
- [内存泄漏（memory leak）](https://www.wolai.com/mary/qwwq3geBqBcXs2cygGY3Lx)
- [JavaScript 内存泄露的4种方式及如何避免](https://blog.csdn.net/qappleh/article/details/80337630)

#### 对象方法，this

JavaScript 中的 this 可以用于任何函数，即使它不是对象的方法；this 的值是在代码运行时计算出来的，它取决于代码上下文；

在普通函数中的 this，严格模式下的值为 undefined，非严格模式的情况下，this 将会是全局对象（浏览器为 window）；箭头函数没有自己的 this，其 this 值取决于外部“正常的”函数；

#### 构造器和操作符 new

从技术上讲，构造函数也是一个常规函数；但一般以大写字母开头并只用 new 操作符执行；当一个函数被使用 new 操作符执行时，它按照以下步骤：

1. 一个新的空对象被创建并分配给 this；
2. 函数体执行，通常它会修改 this，为其添加新的属性；
3. 返回 this 的值；

```js
function User(name) {
  // this = {};（隐式创建）

  // 添加属性到 this
  this.name = name;
  this.isAdmin = false;

  // return this;（隐式返回）
}
```

> Tips: 创建单个复杂对象的代码，可以将它们封装在一个立即调用的构造函数中 `new function() { … }`；

```js
// 创建一个函数并立即使用 new 调用它
let user = new function() {
  this.name = "John";
  this.isAdmin = false;

  // 函数定义
  this.sayHi = function() {
    alert( "My name is: " + this.name );
  };

  // 用于用户创建的其他代码
  // 也许是复杂的逻辑和语句、局部变量等
};
```

> 在一个函数内部，我们可以使用 `new.target` 属性来检查它是否被使用 `new` 进行调用了；对于常规调用，它为 `undefined`，对于使用 `new` 的调用，则等于该函数；

```js
function User() {
  alert(new.target);
}

User(); // undefined

new User(); // function User { ... }

// 常规模式重定向构造器模式
function User(name) {
  if (!new.target) { // 没有通过 new 运行
    return new User(name); // 添加 new
  }

  this.name = name;
}

let john = User("John"); // 将调用重定向
alert(john.name); // John
```

通常，构造器没有 return 语句；它们的任务是将所有必要的东西写入 this，并自动转换为结果；

当构造器中有 return，若返回值为一个对象，则会返回该对象并覆盖默认的 this；若返回值为原始类型或为空时，则会被忽略，仍然返回默认的 this；

> Tips: 如果没有参数，我们可以省略 `new` 后的括号；

#### 可选链 ?.

如果可选链 ?. 前面的值为 undefined 或者 null，它会停止运算并返回 undefined；

```js
// 可选链
let user = {};

console.log(user?.name?.first);
user.admin?.();
user?.["key"];
```

> Note: `?.` 前的变量必须已声明，且只将 `?.` 使用在一些东西可以不存在的地方，不要过度使用可选链；

可选链 ?. 不是一个运算符，而是一个特殊的语法结构；它还可以与函数和方括号一起使用；

1. ?.() 用于调用一个可能不存在的函数；
2. ?.[] 用于访问一个可能不存在的属性；

> Tips: 删除一个可能不存在的属性，`delete user?.name`；可以使用 ?. 来安全地读取或删除，但不能写入；

#### Symbol 类型

规范中，对象的属性键只能是字符串类型或者 Symbol 类型，Symbol 值表示唯一的标识符；

使用 Symbol() 来创建这种类型的值，创建时可以给 Symbol 一个描述（也称为 Symbol 名），这在代码调试时非常有用；可以通过 symbol.description 属性获取 Symbol 的描述；

```js
// id 是 symbol 的一个实例化对象
let id = Symbol();

// id 是描述为 "id" 的 Symbol
let id2 = Symbol("id");

console.log(id2.description) // id
```

Symbol 保证是唯一的；即使我们创建了许多具有相同描述的 Symbol，它们的值也是不同；描述只是一个标签，不影响任何东西；

```js
let id1 = Symbol("id");
let id2 = Symbol("id");

console.warn(id1 == id2); // false
```

> Note: `Symbol` 不会被自动转换为字符串；只能手动调用 `toString()`；

- “隐藏”属性

Symbol 允许我们创建对象的“隐藏”属性，代码的任何其他部分都不能意外访问或重写这些属性；

```js
let user = {};
let id = Symbol("id");
user[id] = 1;
```
> Note: 在对象字面量中使用 `Symbol`，需要使用方括号；

```js
let id = Symbol();

let user = {
  name: "Coley",
  [id]: 12  
}
```

Symbol 属性不参与 for..in 循环，Object.keys(user) 也会忽略 Symbol；这是一般“隐藏符号属性”原则的一部分；但 Object.assign 会同时复制字符串和 symbol 属性；

从技术上讲，内建方法 Object.getOwnPropertySymbols(obj) 允许我们获取所有的 Symbol；还有一个 Reflect.ownKeys(obj) 方法可以返回一个对象的所有键，包括 Symbol；

**全局 symbol**

调用 Symbol.for(key) 方法创建或查询，该方法会先检查全局注册表，如果有一个描述为 key 的 Symbol，则返回该 Symbol，否则将创建一个新 Symbol，并通过给定的 key 将其存储在全局注册表中；

或者调用 Symbol.keyFor(sym) 方法，通过全局 Symbol 返回一个名字；如果查找的 Symbol 不是全局的则会返回 undefined；

```js
// 从全局注册表中读取
let id = Symbol.for("id"); // 如果该 Symbol 不存在，则创建它

// 再次读取（可能是在代码中的另一个位置）
let idAgain = Symbol.for("id");

// 相同的 Symbol
alert( id === idAgain ); // true

Symbol.keyFor(id); // id
```

除此之外，JavaScript 内部还有许多系统 Symbol，可以用来微调对象的各个方面，以此改变一些内建行为，这些 Symbol 被列在了 [Symbol 表](https://tc39.github.io/ecma262/#sec-well-known-symbols) 的规范中；


#### 对象——原始值转换

JavaScript 不允许自定义运算符对对象的处理方式；因此在对对象进行运算时，对象会被自动转换为原始值，并会得到一个原始值的结果；

所有的对象在布尔上下文（context）中均为 true，因此对于对象，不存在 boolean 转换；

- "string" hint

对象到字符串的转换，当我们对期望一个字符串的对象执行操作时，如 “alert”；

```js
// 输出
alert(obj); // [object Object]

// 将对象作为属性键
anotherObj[obj] = 123; // {[object Object]: 123}
```

- "number" hint

对象到数字的转换，例如当我们进行数学运算时；

```js
// 显式转换
let num = Number(obj); // NaN

// 数学运算（除了二元加法）
let n = +obj; // NaN
let delta = new Date - new Date; // 0

// 小于/大于的比较
let greater = user1 > user2; // false
```

- "default" hint

在少数情况下发生，当运算符“不确定”期望值的类型时；

```js
let total = obj + user; // [object Object][object Object]

console.log(user == 1); // false
```

> Note: 像 `<` 和 `>` 这样的小于/大于比较运算符，也可以同时用于字符串和数字；不过，它们使用 `number hint`，而不是 `default`，这是历史原因；

JavaScript 转换算法：

1. 调用 obj[Symbol.toPrimitive](hint)，带有 symbol 键 Symbol.toPrimitive（系统 symbol）的方法，如果这个方法存在的话；
2. 否则，如果 hint 是 "string"，尝试 obj.toString() 和 obj.valueOf()，对于字符串转换，优先 toString；
3. 否则，如果 hint 是 "number" 或 "default"，尝试 obj.valueOf() 和 obj.toString()，对于数学运算，优先 valueOf；


**Symbol.toPrimitive**

```js
obj[Symbol.toPrimitive] = function(hint) {
  // 这里是将此对象转换为原始值的代码
  // 它必须返回一个原始值
  // hint = "string"、"number" 或 "default" 中的一个
}

// 在对象原型上添加
Object.prototype[Symbol.toPrimitive] = function () {
  return "string";
};

let o = {};

console.warn("" + o); // string

// 另一个例子
let user = {
  name: "John",
  money: 1000,

  [Symbol.toPrimitive](hint) {
    alert(`hint: ${hint}`);
    return hint == "string" ? `{name: "${this.name}"}` : this.money;
  }
};

// 转换演示：
alert(user); // hint: string -> {name: "John"}
alert(+user); // hint: number -> 1000
alert(user + 500); // hint: default -> 1500
```

**toString 和 valueOf**

还可以使用 toString 和 valueOf 方法，但必须返回一个原始值，如果 toString 或 valueOf 返回了一个对象，那么返回值会被忽略；

默认情况下，普通对象具有 toString 和 valueOf 方法：toString 方法返回一个字符串 "[object Object]"；valueOf 方法返回对象自身；

```js

// 另一个例子
let user = {
  name: "John",
  money: 1000,

  // 对于 hint="string"
  toString() {
    return `{name: "${this.name}"}`;
  },

  // 对于 hint="number" 或 "default"
  valueOf() {
    return this.money;
  }
};
```

> Tips: 通常我们希望有一个“全能”的地方来处理所有原始转换；这时，我们可以只实现 `toString`；如果没有 `Symbol.toPrimitive` 和 `valueOf`，`toString` 将处理所有原始转换；

> Note: 三种方式转换可以返回任何原始类型；但由于历史原因，如果 `toString` 或 `valueOf` 返回一个对象，则不会出现 `error`，但是这种值会被忽略；

如果将对象作为参数传递，会先被转换为原始值，如果生成的原始值的类型不正确，则继续进行转换；

```js
let obj = {
  // toString 在没有其他方法的情况下处理所有转换
  toString() {
    return "2";
  }
};

alert(obj * 2); // 4，对象被转换为原始值字符串 "2"，之后它被乘法转换为数字 2
```

#### 原始类型的方法

JavaScript 允许我们像使用对象一样使用原始类型，如在字符串字面值上调用方法；

```js
// 首先创建一个包含字符串字面值的特殊对象，
// 然后调用 toUpperCase 方法，之后特殊对象被销毁，只留下原始值；
console.log("string".toUpperCase()); // STRING
```

对象包装器：String、Number、Boolean、Symbol 和 BigInt；

> Note: 不要使用 `new` 操作符调用包装器，因为包装器会返回一个对象，可能会导致判断失效；

> Note: 特殊的原始类型 `null` 和 `undefined` 是例外，它们没有对应的“对象包装器”，也没有提供任何方法；从某种意义上说，它们是“最原始的”；

原始类型不是对象，不能存储额外的数据

```js
let str = "string";

str.test = 1;

console.warn(str.test); // 非严格模式 undefined
console.warn(str.test); // 严格模式 Uncaught TypeError: Cannot create property 'test' on string 'string'
```

#### 数字类型

JavaScript 中的常规数字以 64 位的格式 IEEE-754 存储，也被称为“双精度浮点数”；在内部，所以有 64 位可以存储一个数字：其中 52 位被用于存储这些数字，其中 11 位用于存储小数点的位置（对于整数，它们为零），而 1 位用于符号；因此所表示的范围介于 ±(2^53-1) 之间；

> Tips: 定义数字时，可以使用下划线间隔数字以增强可读性：`let billion = 1_000_000_000;`，JavaScript 引擎会直接忽略数字之间的 `_`；

科学计数法，在 JavaScript 中，可以通过在数字后面附加字母 "e" 并指定零的个数来缩短数字；

十六进制数字在 JavaScript 中被广泛用于表示颜色，编码字符以及其他许多东西；前缀为 0x，然后是数字；二进制和八进制数字系统很少使用，但也支持使用 0b 和 0o 前缀；

使用 parseInt 和 toString(base) 两个方法可以对进制进行转换；

toString 参数 base 的范围可以从 2 到 36，默认情况下是 10；parseInt 方法将源数按指定进制解析为十进制数；

```js
let number = 200;

console.log(number.toString(16)); // "c8"

console.log(parseInt("c8", 16)); // 200
```

> Tips: 将一个较长的数字标识符转换成较短的时候，可以使用 toString(36)；

> Tips: 使用两个点来调用一个方法，JavaScript 语法隐含了第一个点之后的部分为小数部分；若数字后有两个点，那么 JavaScript 就知道小数部分为空，现在使用该方法 `123456..toString(36)`，或直接为数字添加一对括号 `(123456).toString(36)`；


**舍入**

1. Math.floor 向下取整；
2. Math.ceil 向上取整；
3. Math.round 四舍五入；
4. Math.trunc 截断小数点部分（IE 不支持）；
5. Math.prototype.toFixed(n) 保留 n 位小数，返回字符串；

**精度丢失**

```js
alert( 0.1 + 0.2 == 0.3 ); // false

// Hello！我是一个会自我增加的数字！
alert( 9999999999999999 ); // 显示 10000000000000000
// 超过 2^53 次方

// 精度损失可能会导致数字的增加和减小，在这种特殊的情况下，数字变小了一点，就会向下舍入；
alert( 6.35.toFixed(1) ); // 6.3

// 偶发的无限循环
let i = 0;
while (i != 10) {
  i += 0.2;
}
```

解决方案：

```js
// 使用 toPrecision 凑整并 parseFloat 转成数字后再显示
function strip(num, precision = 12) {
  return +parseFloat(num.toPrecision(precision));
}

// 判断是否在误差范围
function isEqual(arg1, arg2){
  return Math.abs(arg1 - arg2) < Number.EPSILON;
}
```

> Note: 数字内部表示的另一个有趣结果是存在两个零：`0` 和 `-0`；因为在存储时，使用一位来存储符号，不过运算符将它们视为相同的值；

[JavaScript 浮点数陷阱及解法](https://github.com/camsong/blog/issues/9)

**isFinite 和 isNaN**

- isNaN(value) 将其参数转换为数字，然后测试它是否为 NaN；值 “NaN” 是独一无二的，它不等于任何东西，包括它自身，任何不能被转换为数值的的值都会返回 true；
- Number.isNaN(value) 首先判断传入参数是否为数字，如果是数字再继续判断是否为 NaN ，不会进行数据类型的转换，这种方法对于 NaN 的判断更为准确；
- isFinite(value) 将其参数转换为数字，如果是常规数字，则返回 true，而不是 NaN/Infinity/-Infinity；因此该方法可以用于验证字符串值是否为常规数字；

> Note: 在所有数字函数中，包括 `isFinite`，空字符串或仅有空格的字符串均被视为 `0`；

**Object.is**

当内部算法需要比较两个值是否完全相同时，它使用 Object.is（内部称为 SameValue），这种比较方式经常被用在 JavaScript 规范中；

```js
// 适用于 NaN
Object.is(NaN，NaN) // true
NaN === NaN // false

// 比较 0 和 -0，内部符号位不同
Object.is(0，-0) // false
0 === -0 // true
```

除了以上两种特殊情况，其他情况等价于 ===；

**parseInt 和 parseFloat**

使用加号 + 或 Number() 的数字转换是严格的，如果一个值不完全是一个数字，就会失败并返回一个 NaN；

parseInt 和 parseFloat 可以从字符串中“读取”数字，直到无法读取为止；如果发生 error，则返回收集到的数字；函数 parseInt 返回一个整数，而 parseFloat 返回一个浮点数；当没有数字可读时会返回 NaN；

> Tips: `parseInt(str, radix)` 函数具有可选的第二个参数，它指定了数字系统的基数，因此 `parseInt` 还可以解析十六进制数字、二进制数字等的字符串：

#### 字符串

字符串的内部格式始终是 UTF-16，它不依赖于页面编码；

**转义字符**

| 字符 | 描述 |
|:-----|:-----|
| \n | 换行 |
| \r | 在 Windows 中，两个字符 \r\n 的组合代表一个换行 |
| \', \" | 引号 |
| \\ | 反斜线 |
| \t | 制表符 |
| \b, \f, \v | 退格，换页，垂直标签 |
| \xXX | 具有给定十六进制 Unicode XX 的 Unicode 字符 |
| \uXXXX | 以 UTF-16 编码的十六进制代码 XXXX 的 Unicode 字符 |
| \u{X…XXXXXX} |（1 到 6 个十六进制字符）	具有给定 UTF-32 编码的 Unicode 符号 |

> Note: 反斜杠 `\` 在 JavaScript 中用于正确读取字符串，然后消失，内存中的字符串没有 `\`；

**访问字符**

要获取在 pos 位置的一个字符，可以使用方括号 [pos] 或者调用 str.charAt(pos) 方法；区别是如果没有找到字符，[] 返回 undefined，而 charAt 返回一个空字符串；

> Tips: 通过字符串的 `length` 属性可以获取字符串的长度；

```js
let str = `Hello`;

alert( str[1000] ); // undefined
alert( str.charAt(1000) ); // ''（空字符串）

// 使用 for ... of 遍历字符串
for (let char of "Hello") {
  alert(char); // H,e,l,l,o（char 变为 "H"，然后是 "e"，然后是 "l" 等）
}

str[0] = 'h'; // Uncaught TypeError: Cannot assign to read only property '0' of string 'string'
```

**查找子字符串**

使用 str.indexOf(substr, pos) 或 str.lastIndexOf(substr, pos) 方法，从给定位置 pos 开始，在 str 中查找 substr，如果没有找到，则返回 -1，否则返回匹配成功的位置；区别是从字符串的首尾两端开始；

> Tips: 可以使用按位取反 `~`，将数字转换为 `32-bit` 整数（如果存在小数部分，则删除小数部分），然后对其二进制表示形式中的所有位均取反；对于 `32-bit` 整数，`~n` 等于 `-(n+1)`；

```js
let str = "Widget";

// 未查到是 indexOf 返回 -1，取反后值为 0；
if (~str.indexOf("Widget")) {
  alert( 'Found it!' ); // 正常运行
}
```

str.includes(substr, pos) 根据 str 中是否包含 substr 来返回 true/false，也可以使用第二个可选参数指定开始搜索的起始位置；

方法 str.startsWith 和 str.endsWith 判断字符串是否以某子字符串开头或结尾；

**获取子字符串**

str.slice(start [, end]) 返回字符串从 start 到（但不包括）end 的部分；如果没有第二个参数，slice 会一直运行到字符串末尾；start/end 也可以为负值，表示起始位置从字符串结尾计算；

```js
let str = "stringify";
alert( str.slice(0, 5) ); // 'strin'
alert( str.slice(0, 1) ); // 's'
alert( str.slice(2) ); // 'ringify'
alert( str.slice(-4, -1) ); // 'gif'
```

str.substring(start [, end]) 返回字符串在 start 和 end 之间 的部分，与 slice 几乎相同，但允许 start 大于 end（两个值会被交换）；当遇到负数时，会被视为 0；

```js
let str = "stringify";
alert( str.substring(2, 6) ); // "ring"
alert( str.substring(6, 2) ); // "ring"
alert( str.substring(6, 0) ); // "string"
alert( str.substring(6, -1) ); // "string"
```

str.substr(start [, length]) 返回字符串从 start 开始的给定 length 的部分；若第一个参数为负数，则从结尾算起；

```js
let str = "stringify";
alert( str.substr(2, 4) ); // 'ring'
alert( str.substr(-4, 2) ); // 'gi'
```

> Note: `str.substr` 不在 JavaScript 核心规范中的描述，因此，非浏览器环境可能无法支持它；

**比较字符串**

> Note: 小写字母总是大于大写字母，带变音符号的字母存在“乱序”的情况；

所有的字符串都使用 UTF-16 编码，即每个字符都有对应的数字代码；

str.codePointAt(pos) 返回在 pos 位置的字符代码；
String.fromCodePoint(code) 通过数字 code 创建字符；

```js
alert( "z".codePointAt(0) ); // 122
alert( "Z".codePointAt(0) ); // 90

alert( String.fromCodePoint(90) ); // Z

// 输出 65..220 的字符
let str = '';

for (let i = 65; i <= 220; i++) {
  str += String.fromCodePoint(i);
}
alert( str );
// ABCDEFGHIJKLMNOPQRSTUVWXYZ[\]^_`abcdefghijklmnopqrstuvwxyz{|}~¡¢£¤¥¦§¨©ª«¬­®¯°±²³´µ¶·¸¹º»¼½¾¿ÀÁÂÃÄÅÆÇÈÉÊËÌÍÎÏÐÑÒÓÔÕÖ×ØÙÚÛÜ
```

一种特殊的方法来比较不同语言的字符串，调用 str.localeCompare(str2) 会根据语言规则返回一个整数，这个整数能指示字符串 str 在排序顺序中排在字符串 str2 前面 `-`、相同 `0`、后面 `+`；

```js
// 首字母大写
function initialFirst(str) {
  if (typeof str !== "string") return "";

  return str.charAt(0).toUpperCase() + str.slice(1);
}
```

#### 数组

数组是一种特殊的对象，它们扩展了对象，提供了特殊的方法来处理有序的数据集合以及 length 属性；JavaScript 引擎尝试把这些元素一个接一个地存储在连续的内存区域；

```js
// 创建空数组：
let arr = new Array();
let arr = [];

// 会创建一个给定长度的数组，但不含有任何项
let arr = new Array(2); // [empty x 2] 
let arr = new Array(item1, item2, ...);
```

**pop/push, shift/unshift 方法**

pop 取出并返回数组的最后一个元素；push 在数组末端添加元素；shift 取出数组的第一个元素并返回它；unshift 在数组的首端添加元素；其中，push 和 unshift 方法都可以一次添加多个元素；

> Note: `push`/`pop` 方法运行的比较快，而 `shift`/`unshift` 比较慢，是因为后二者会移动数组里所有元素；数组里的元素越多，移动它们就要花越多的时间，也就意味着越多的内存操作；

**遍历数组**

可以使用普通 for 循环或者 for..of，但 for..of 遍历数组只能获取元素值，不能获取当前元素的索引；

> Note: `for..in` 循环会遍历所有属性，不仅仅是这些数字属性；`for..in` 循环适用于普通对象，并且做了对应的优化，但是不适用于数组，因此速度要慢 `10-100` 倍；

**数组 length**

当我们修改数组的时候，length 属性会自动更新，准确来说，它实际上不是数组里元素的个数，而是最大的数字索引值加一；

> Tips: `length` 属性是可写的，清空数组最简单的方法就是：`arr.length = 0`，但该过程是不可逆的；


> Note: 如果使用 `==` 来比较数组，除非比较的是两个引用同一数组的变量，否则它们永远不相等；

```js
// 最大子数组之和
function getMaxSubSum(arr) {
  let maxSum = 0;
  let partialSum = 0;

  for (let item of arr) {
    partialSum += item; // 将其加到 partialSum
    maxSum = Math.max(maxSum, partialSum); // 记住最大值
    if (partialSum < 0) partialSum = 0; // 如果是负数就置为 0
  }

  return maxSum;
}
```

#### 数组方法

**splice**

arr.splice(start[, deleteCount, elem1, ..., elemN])，从索引 start 开始修改 arr，start 可以为负数：删除 deleteCount 个元素并在当前位置插入 elem1, ..., elemN，最后返回已被删除元素的数组；

```js
let arr = [1, 2, 3, 4, 5];

// 删除第一个元素
console.log(arr.splice(0, 1), arr); // [1] [2, 3, 4, 5]
// 删除最后一个元素
console.log(arr.splice(-1, 1), arr); // [5] [2, 3, 4]
// 插入元素
console.log(arr.splice(2, 0, 100), arr); // [] [2, 3, 100, 4] 
```

> Tips: 将 `deleteCount` 设置为 `0`，`splice` 方法就能够插入元素而不用删除任何元素；

**slice**

arr.slice([start], [end]) 返回一个新数组，将所有从索引 start 到 end（不包括 end）的数组项复制到一个新的数组；start 和 end 都可以是负数，在这种情况下，从末尾计算索；

> Tips: 可以不带参数地调用：`arr.slice()` 会创建一个 `arr` 的副本；进行不影响原始数组的进一步转换；

```js
let arr = ["t", "e", "s", "t"];

alert( arr.slice(1, 3) ); // e,s（复制从位置 1 到位置 3 的元素）
alert( arr.slice(-2) ); // s,t（复制从位置 -2 到尾端的元素）
```

**concat**

arr.concat(arg1, arg2...) 创建一个新数组，其中包含来自于其他数组和其他项的值；

> Note: `concat` 只复制数组中的元素，即使是一个类数组的对象，也会被当作一个整体；除非类数组的对象具有 `Symbol.isConcatSpreadable` 属性，那么它就会被 `concat` 当作一个数组来处理；

```js
let arr = [1, 2];

let arrayLike = {
  0: "something",
  length: 1
};

let arrayLike2 = {
  0: "something",
  [Symbol.isConcatSpreadable]: true,
  length: 1
};

alert( arr.concat([3, 4], 5) ); // 1,2,3,4,5
alert( arr.concat(arrayLike, arrayLike2) ); // 1,2,3,4,5,[object Object],something
```

**forEach**

arr.forEach 方法允许为数组的每个元素都运行一个函数；且该函数的结果（如果有返回）会被抛弃和忽略；

**indexOf/lastIndexOf 和 includes**

arr.indexOf(item, from)/arr.lastIndexOf(item, from) 都从索引 from 开始搜索 item，如果找到则返回索引，否则返回 -1；后者从右向左搜索；arr.includes(item, from) 从索引 from 开始搜索 item，如果找到返回 true，否则返回 false；

> Tips: 这些方法使用的是严格相等 `===` 比较，`includes` 的一个非常小的差别是它能正确处理NaN，而 `indexOf/lastIndexOf` 则不能；

```js
let arr2 = [1, 2, NaN, null, undefined];

console.warn(arr2.indexOf(NaN)); // -1
console.warn(arr2.indexOf(null)); // 3
console.warn(arr2.lastIndexOf(undefined)); // 4

console.warn(arr2.includes(NaN)); // true
console.warn(arr2.includes(null)); // true
console.warn(arr2.includes(undefined)); // true
```

**find/findIndex 和 filter**

在对象数组中查找具有特定条件的对象，可以使用 find/findIndex 和 filter 方法；

arr.find((item, index, array) => {...}) 依次对数组中的每个元素调用一个回调函数，其中 item 是元素，index 是它的索引，array 是数组本身；如果回调函数返回 true，则返回 item 并停止迭代，如果返回假值（falsy），则返回 undefined；

arr.findIndex 与 arr.find 的区别只有返回值，前者返回元素索引，后者返回元素本身；

arr.filter 方法返回所有匹配（使回调函数返回 true）元素组成的数组；


**map 和 some/every**

arr.map((item, index, array) => { ... }) 对数组的每个元素都调用函数，并返回一个新的结果数组；

arr.some(fn)/arr.every(fn) 用于检查数组，对数组的每个元素调用函数 fn；如果任何/所有结果为 true，则返回 true，否则返回 false；且存在短路效应；

**sort**

arr.sort 方法对数组进行原位（in-place） 排序，并更改元素的顺序，原位是指在此数组内，而非生成一个新数组；

> Note: 元素默认情况下被按字符串进行排序；所有元素都被转换为字符串，然后进行比较，对于字符串，按照词典顺序进行排序；

arr.sort(fn) 方法实现了通用的排序算法，在内部大多数情况下都是经过快速排序或 Timsort 算法优化的；比较函数 fn 可以返回任何数字，实际上，比较函数只需要返回一个正数表示“大于”，一个负数表示“小于”；

```js
// 基本使用
let arr = [1, -2, 15, 2, 0, 8];

arr.sort((a, b) => a - b); // [-2, 0, 1, 2, 8, 15]
```

快速排序是对冒泡排序的一种改进，由 C.A.R.Hoare（Charles Antony Richard Hoare，东尼·霍尔）在 1962 年提出；这种算法实际上是一种分治法思想，也就是分而治之，把问题分为一个个的小部分来分别解决，再把结果组合起来；

```js
// 快速排序 js 实现
function QuickSort(arr, start, end) {
  if (start > end) {
    return;
  }

  let i = start;
  let j = end;
  let key = arr[i];

  while (i < j) {
    // 往前找小于的数
    while (i < j && arr[j] > key) {
      j--;
    }
    if (i < j) {
      arr[i++] = arr[j];
    }
    // 往后找大于的数
    while (i < j && arr[i] < key) {
      i++;
    }
    if (i < j) {
      arr[j--] = arr[i];
    }
  }

  arr[i] = key;
  QuickSort(arr, start, i - 1);
  QuickSort(arr, i + 1, end);
}

let arr = [72, 6, 57, 88, 60, 42, 83, 73, 48, 85];
QuickSort(arr, 0, arr.length - 1);
console.log(arr); // [6, 42, 48, 57, 60, 72, 73, 83, 85, 88]
```
- [快速排序算法详解（原理、实现和时间复杂度）](http://data.biancheng.net/view/117.html)
- [Timsort原理学习](https://sikasjc.github.io/2018/07/25/timsort/)
- [Getting things sorted in V8](https://v8.dev/blog/array-sort#timsort)

**reverse**

arr.reverse 方法用于颠倒 arr 中元素的顺序，同时也会返回颠倒后的数组 arr；

**split 和 join**

str.split(delim[, limit]) 方法通过给定的分隔符 delim 将字符串分割成一个数组；split 还有一个可选的第二个数字参数，可以限制数组长度，忽略额外的元素；

> Tips: 调用带有空字符串参数的 `split('')`，会将字符串拆分为字母数组；如果一个元素为 `undefined` 或 `null`，它会被转换为空字符串；

arr.join(glue) 与 split 相反，该方法会创建并返回一个由连接符连接的数组元素的字符串；

**reduce/reduceRight**

arr.reduce((accumulator, item, index, array) => { ... }, [initial]) 函数一个接一个地应用于所有数组元素，并将其结果“搬运”到下一个调用，其中 accumulator 是上一个函数调用的结果，第一次等于 initial（若存在）；arr.reduceRight 和 arr.reduce 方法的功能一样，只是遍历为从右到左；

**fill 和 copyWithin**

arr.fill(value, start, end) 从索引 start 到 end，用重复的 value 填充数组；

arr.copyWithin(target, start, end) 将从位置 start 到 end 的所有元素复制到自身的 target 位置（覆盖现有元素）；

```js
let arr = [1, 2, 3, 4, 5];

let result = arr.reduce((sum, current) => sum + current, 0);

alert(result); // 15
```

如果没有初始值，那么 reduce 会将数组的第一个元素作为初始值，并从第二个元素开始迭代；但如果数组为空，那么在没有初始值的情况下调用 reduce 会导致错误；所以建议始终指定初始值；

**Array.isArray**

因为数组是基于对象的，不构成单独的语言类型，所以 typeof 不能帮助从数组中区分出普通对象；但可以使用 Array.isArray() 方法判断；

```js
alert(typeof {}); // object
alert(typeof []); // object

alert(Array.isArray({})); // false
alert(Array.isArray([])); // true
```

**thisArg**

几乎所有调用函数的数组方法，都接受一个可选的附加参数 thisArg；当数组方法的回调函数为普通函数时，该参数值为 this；当使用对象方法作为数组方法的回调函数时，通过该参数将对象上下文语境传入；（不常用）

```js
let army = {
  minAge: 18,
  maxAge: 27,
  canJoin(user) {
    return user.age > 20;
  },
};
let users = [{ age: 16 }, { age: 20 }, { age: 23 }, { age: 30 }];

let soldiers = users.filter(army.canJoin, army);
console.log(soldiers); // [{age: 23}, {age: 30}]
```

**其他方法**

arr.flat(depth)/arr.flatMap(fn) 可以从多维数组创建一个新的扁平数组；

Array.of(element0[, element1[, …[, elementN]]]) 基于可变数量的参数创建一个新的 Array 实例，而不需要考虑参数的数量或类型；

```js
// Fisher-Yates shuffle 随机排列数组
// 思路：逆向遍历数组，并将每个元素与其前面的随机的一个元素互换位置
function shuffle(array) {
  for (let i = array.length - 1; i > 0; i--) {
    // 从 0 到 i 的随机索引
    let j = Math.floor(Math.random() * (i + 1)); 

    // 解构交换元素 array[i] 和 array[j]
    [array[i], array[j]] = [array[j], array[i]];
  }
}
```

#### 可迭代对象

可迭代（Iterable）对象是数组的泛化，这个概念是说任何对象都可以被定制为可在 for..of 循环中使用的对象；数组和字符串是使用最广泛的内建可迭代对象；

为了让对象可迭代，需要为对象添加一个名为 Symbol.iterator 的方法，该方法是一个专门用于使对象可迭代的内建 symbol；

for..of 循环：

1. 首先会调用 Symbol.iterator 方法（没有就报错）；
2. 这个方法必须返回一个迭代器（iterator）即一个有 next 方法的对象；
3. 此后，for..of 仅适用于这个被返回的对象；
4. 然后通过调用返回对象的 next() 方法 for..of 循环取得下一个数值；
5. next() 方法返回的结果的格式必须是 { done: Boolean, value: any }；
6. 当 done = true 时，表示循环结束，否则 value 是下一个值；

```js
// 自定义可迭代对象 range
let range = {
  from: 1,
  to: 5,

  [Symbol.iterator]() {
    // 返回一个带有 next 方法的对象
    return {
      current: this.from,
      last: this.to,

      // 在每轮 for..of 循环中调用
      next() {
        // 固定返回一个 { done, value } 对象
        return this.current < this.last
          ? { done: false, value: this.current++ }
          : { done: true }; // 当 done 为 true 时，结束迭代
      },
    };
  },
};

for (const it of range) {
  console.warn(it); // 1, 2, 3, 4
}
```

> Note: 注意可迭代对象的核心功能：关注点分离；`range` 自身没有 `next()` 方法，相反，是通过调用 `range[Symbol.iterator]()` 创建了另一个对象，即所谓的“迭代器”对象，并且它的 `next` 会为迭代生成值；

从技术上说，我们可以将它们合并，并使用 range 自身作为迭代器来简化代码，但现在不可能同时在对象上运行两个 for..of 循环：它们共享迭代状态，因为只有一个迭代器，即对象本身；

```js
let range = {
  from: 1,
  to: 5,

  [Symbol.iterator]() {
    this.current = this.from;
    return this;
  },

  next() {
    return this.current < this.to
      ? { done: false, value: this.current++ }
      : { done: true };
  }
};
```

> Tips: 无穷迭代器，`next` 没有什么限制，它可以返回越来越多的值，这是正常的，可以通过 `break` 跳出 `for..of` 循环；

**可迭代和类数组**

Iterable 如上所述，是实现了 Symbol.iterator 方法的对象；Array-like 是有索引和 length 属性的对象，所以它们看起来很像数组；但是一个可迭代对象也许不是类数组对象，反之亦然，类数组对象可能不可迭代

> Note: 字符串即是可迭代的（`for..of` 对它们有效），又是类数组的（它们有数值索引和 `length` 属性）；

**Array.from**

全局方法 Array.from 可以接受一个可迭代或类数组的值，并从中获取一个“真正的”数组；Array.from(obj[, mapFn, thisArg]) 完整语法还允许我们提供一个可选的“映射（mapping）”函数；可选的第二个参数 mapFn 可以是一个函数，该函数会在对象中的元素被添加到数组前，被应用于每个元素，此外 thisArg 允许我们为该函数设置 this；

```js
let arr = Array.from(range);
alert(arr); // 1,2,3,4,5 （数组的 toString 转化方法生效）

// 求每个数的平方
let arr = Array.from(range, num => num * num);
alert(arr); // 1,4,9,16,25
```

> Tips: 基于 `Array.from` 创建代理感知（surrogate-aware）的 `slice` 方法，即能够处理 `UTF-16` 扩展字符的 `slice` 方法；

```js
function slice(str, start, end) {
  return Array.from(str).slice(start, end).join('');
}

let str = '𝒳😂𩷶';
alert( slice(str, 1, 3) ); // 😂𩷶

// 原生方法不支持识别代理对（译注：UTF-16 扩展字符）
alert( str.slice(1, 3) ); // 乱码（两个不同 UTF-16 扩展字符碎片拼接的结果）
```

> Tips: 在规范中，大多数内建方法都假设它们需要处理的是可迭代对象或者类数组对象，而不是“真正的”数组，因为这样抽象度更高；

#### Map and Set（映射和集合）

**Map**

Map 是一个带键的数据项的集合，就像一个 Object 一样；但 Map 允许任何类型的键（key），包括对象；

| 方法 | 描述 |
|:-----|:-----|
| new Map() | 创建 map |
| map.set(key, value) | 根据键存储值，并返回 map 本身，因此可以链式调用 |
| map.get(key) | 根据键来返回值，如果 map 中不存在对应的 key，则返回 undefined |
| map.has(key) | 如果 key 存在则返回 true，否则返回 false |
| map.delete(key) | 删除指定键的值 |
| map.clear() | 清空 map |
| map.size | 返回当前元素个数 |

> Note: 虽然 `map[key]` 也有效，例如我们可以设置 `map[key] = 2`，这样会将 `map` 视为 JavaScript 的普通对象 `plain object`，因此它暗含了所有相应的限制（仅支持 `string`/`symbol` 键等）

> Note: `Map` 使用 `SameValueZero` 算法来比较键是否相等，它和严格等于 `===` 差不多，但区别是 `NaN` 被看成是等于 `NaN`，所以 `NaN` 也可以被用作键；

**Map 迭代**

| 方法 | 描述 |
|:-----|:-----|
| map.keys() | 返回所有键的迭代器 |
| map.values() | 返回所有值的迭代器 |
| map.entries() | 返回所有实体 [key, value] 的迭代器 |
| map.forEach((value, key, map) => {}) | 与 Array 的类似 |

for..of 在遍历 map 时，默认情况下使用的是 map.entries 方法；

```js
// 初始化空 Map
let map = new Map();

map.set("1", "str1").set(1, "num1").set(true, "bool1");

for (const value of map) {
  console.log(value); // ['1', 'str1'] [1, 'num1'] [true, 'bool1']
}

// 带键值对的二维数组初始化 Map
let map = new Map([
  ['1',  'str1'],
  [1,    'num1'],
  [true, 'bool1']
]);

// 可迭代对象初始化 Map，iter 返回键值对的数组
let map = new Map(iter);
```

**Map 与对象的转换**

Object.entries(obj) 该方法返回对象的键/值对数组，该数组格式完全按照 Map 所需的格式；因此可以根据一个已有的普通对象（plain object）来创建一个 Map；

```js
let obj = {
  name: "John",
  age: 30
};

let map = new Map(Object.entries(obj));
```

而 Object.fromEntries 作用是给定一个具有 [key, value] 键值对的数组，它会根据给定数组创建一个对象；

```js
let map = new Map();
map.set('banana', 1);
map.set('orange', 2);
map.set('meat', 4);

// 创建一个普通对象
let obj = Object.fromEntries(map.entries()); 
let obj = Object.fromEntries(map); // 效果同上，解释如下
```

> Tips: `Object.fromEntries` 期望得到一个可迭代对象作为参数，而不一定是数组，并且 `map` 的标准迭代会返回跟 `map.entries()` 一样的键/值对；

**Set**

Set 是一个特殊的类型集合，没有键，只有值的集合，且它的每一个值只能出现一次；

| 方法 | 描述 |
|:-----|:-----|
| new Set(iterable) | 创建一个 set，参数是一个 iterable 对象 |
| set.add(value) | 添加一个值，返回 set 本身 |
| set.delete(value) | 删除值，如果 value 存在则返回 true ，否则返回 false |
| set.has(value) | 如果 value 在 set 中，返回 true，否则返回 false |
| set.clear() | 清空 set |
| set.size | 返回元素个数 |

Set 的主要特点是，重复使用同一个值调用 set.add(value) 并不会发生什么改变，这就是 Set 里面的每一个值只出现一次的原因；

```js
// 数组去重
Array.from(new Set([1, 2, 3, 3, 5])); // [1, 2, 3, 5]
```

**Set 迭代**

| 方法 | 描述 |
|:-----|:-----|
| set.keys() | 返回包含所有值的迭代器 |
| set.values() | 同 keys，为了兼容 Map |
| set.entries() | 返回所有实体 [value, value] 的迭代器，为了兼容 Map |
| set.forEach((value, valueAgain, set) => {}) | 与 Array 的类似，也为了兼容 Map |

> Tips: 在 `Map` 和 `Set` 中迭代总是按照值插入的顺序进行的，所以我们不能说这些集合是无序的，但是我们不能对元素进行重新排序，也不能直接按其编号来获取元素；

#### WeakMap and WeakSet

**WeakMap**

WeakMap 和 Map 的第一个不同点就是，WeakMap 的键必须是对象，不能是原始值；

WeakMap 不支持迭代以及 keys()，values() 和 entries() 方法，所以没有办法获取 WeakMap 的所有键或值；只有 get(key)、set(key, value)、delete(key)、has(key) 方法；

> Note: `WeakMap` 不会阻止垃圾回收机制对作为键的对象（key object）的回收，在从技术的角度并不能准确知道 何时会被回收，这些都是由 JavaScript 引擎决定的；因此，暂不支持访问 `WeakMap` 的所有键/值的方法；

> Tips: `WeakMap` 的主要应用场景是额外数据的存储；这些数据放到 `WeakMap` 中，并使用第三方对象作为这些数据的键，那么当该对象被垃圾回收机制回收后，这些数据也会被自动清除；

> Tips: `WeakMap` 也可以用于缓存函数的结果，以便将来对同一个对象的调用可以重用这个结果；

```js
// cache.js
let cache = new WeakMap();

// 计算并记结果
function process(obj) {
  if (!cache.has(obj)) {
    let result = /* calculate the result for */ obj;

    cache.set(obj, result);
  }

  return cache.get(obj);
}

// main.js
let obj = {/* some object */};

let result1 = process(obj);
let result2 = process(obj);

// 不需要这个对象时：
obj = null;
```
**WeakSet**

与 Set 类似，但是我们只能向 WeakSet 添加对象（而不能是原始值）；且对象只有在其它某个（些）地方能被访问的时候，才能留在 set 中；WeakSet 支持 add，has 和 delete 方法，但不支持 size 和 keys()，并且不可迭代；

> Tips: `WeakSet` 同 `WeakMap` 的主要优点是对对象是弱引用，所以被它们引用的对象很容易地被垃圾收集器移除；

#### Object.keys，values，entries

对于普通对象：

Object.keys(obj) 返回一个包含该对象所有的键的数组；
Object.values(obj) 返回一个包含该对象所有的值的数组；
Object.entries(obj) 返回一个包含该对象所有 [key, value] 键值对的数组；

> Note: `Object.*` 方法返回的是“真正的”数组对象，而不只是一个可迭代项，这与 `Map` 有区别，这主要是历史原因；`Object.*` 还会忽略 `symbol` 属性；

```js
// 转换对象
let prices = {
  banana: 1,
  orange: 2,
  meat: 4,
};

let doublePrices = Object.fromEntries(
  // 将价格转换为数组，将每个键/值对映射为另一对
  // 然后通过 fromEntries 再将结果转换为对象
  Object.entries(prices).map(entry => [entry[0], entry[1] * 2])
);
alert(doublePrices.meat); // 8

// 也可以是一个类似于 map.entries() 的迭代器
Object.fromEntries(rangeEntries); // {1: 1, 2: 1, 3: 1, 4: 1}
```

```js
// 计算属性之和
function sumSalaries(salaries) {
  return Object.values(salaries).reduce((a, b) => a + b, 0);
}
```

#### 解构赋值

JavaScript 中最常用的两种数据结构是 Object 和 Array；对象是通过键来存储数据项的单个实体，数组将数据收集到一个有序的集合；

解构赋值是通过将结构中的各元素复制到变量中来达到“解构”的目的，但结构本身是没有被修改的；本质上，解构赋值其实是一种用于对在 = 右侧的值上调用 for..of 并进行赋值的操作的语法糖；

**数组解构**

1. 数组中不想要的元素也可以通过添加额外的逗号来把它丢弃；
2. 等号右侧可以是任何可迭代对象，因为在内部，结构赋值是通过迭代右侧的值来完成工作的；
3. 赋值给等号左侧的任何内容，也可以是一个对象属性；
4. 可以与 .entries() 方法进行循环操作；
5. 可以用于交换变量；
6. 等号左侧可以收集剩余数组项；
7. 如果数组比左边的变量列表短，缺少的值被认为是 undefined；
8. 可在等号左侧指定默认值，默认值可以是更加复杂的表达式甚至可以是函数调用；
9. 

```js
// 基本用法
let arr = ["John", "Smith"];
let [firstName, surname] = arr;

// 配合 split 使用
let [firstName, surname] = "John Smith".split(' ');

// 忽略使用逗号的元素，不需要第二个元素，后面剩余值被忽略
let [firstName, , title] = ["Julius", "Caesar", "Consul", "of the Roman Republic"];

// 等号右侧可以是任何可迭代对象
let [a, b, c] = "abc"; // ["a", "b", "c"]
let [one, two, three] = new Set([1, 2, 3]);

// 循环遍历键—值对
let user = {
  name: "John",
  age: 30
};

for (let [key, value] of Object.entries(user)) {
  alert(`${key}:${value}`); // name:John, then age:30
}

// 交换变量
[guest, admin] = [admin, guest];

// 收集剩余项，rest 的值就是数组中剩下的元素组成的数组
let [name1, name2, ...rest] = ["Julius", "Caesar", "Consul", "Republic"];

// 这里也不会出现报错，缺省值为 undefined
let [firstName, surname] = [];

// 指定未赋值的变量默认值
let [name = "Guest", surname = "Anonymous"] = ["Julius"];
// 表达式或函数只会在这个变量未被赋值的时候才会被计算
let [name = prompt('name?'), surname = prompt('surname?')] = ["Julius"];
```

**对象解构**

1. 可以使用 : 指定属性和变量之间的映射关系；
2. 可以使用 = 设置默认值；
3. 默认值可以是任意表达式或函数调用，且只会在未提供对应的值时才会被计算/调用；
4. 可以只提取所需的内容；
5. 可以提取剩余属性的，并存为对象；

```js
let options = {
  title: "Menu",
  width: 100,
  height: 200
};

// 基本使用，且变量的顺序并不重要
let { title, width, height } = options;

// 指定属性和变量之间的映射关系
let { width: w, height: h, title } = options;

// 使用 "=" 设置默认值
let { width = 100, height = 200, title } = options;

// 默认值可以是任意表达式甚至可以是函数调用
let { width = prompt("width?"), title = prompt("title?") } = options;

// 同时指定映射关系和默认值
let { width: w = 100, height: h = 200, title } = options;

// 只提取所需的内容
let { title } = options;

// rest 存有剩余属性的对象
let { title, ...rest } = options;
```

> Note: 不使用 `let` 时可能存在 JavaScript 引擎把 `{...}` 当作代码块处理；因此需要用括号将解构赋值语句包裹起来；

```js
let title, width, height;

// Uncaught SyntaxError: Unexpected token '='
{title, width, height} = {title: "Menu", width: 200, height: 100};

// 添加括号后，正常运行
({title, width, height} = {title: "Menu", width: 200, height: 100});
```

> Tips: 如果一个对象或数组嵌套了其他的对象和数组，可以在等号左侧使用更复杂的模式来提取更深层的数据，也称嵌套结构；

> Tips: 可以把所有参数当作一个对象来传递，然后函数马上把这个对象解构成多个变量；然后通过指定空对象 `{}` 为整个参数对象的默认值，实现让所有的参数都使用默认值；

```js
function myFunc({ title = "Untitled", width: w = 100, height: h = 200 } = {}) {
  alert( `${title} ${w} ${h}` ); // My Menu 100 200
}

// 另一种默认 options 参数
let defaultOptions = { width: 100, height: 100 };

function myFunc(options) {
  let computedOptions = { ...defaultOptions, ...options };
  console.log(computedOptions);
}
```

#### 日期和时间

内建对象：日期（Date）用于存储日期和时间，并提供了日期/时间的管理方法；

**创建方法**：

1. new Date() 不带参数，创建当前日期和时间的 Date 对象；
2. new Date(milliseconds) 传入的整数参数代表自 1970-01-01 00:00:00 以来经过的毫秒数，该整数被称为时间戳，时间戳也可以为负；
3. new Date(datestring) 若只有一个字符串参数，则会被自动解析；该算法与 Date.parse 所使用的算法相同；
4. new Date(year, month, date, hours, minutes, seconds, ms) 使用当前时区中的给定组件创建日期，只有前两个参数是必须的；year 必须是四位数，month 是 0 到 11 的整数，date 是当月的具体某一天，缺失则默认 1，hours/minutes/seconds/ms 缺失则均为默认值 0；

```js
// new Date()
new Date(); // Fri Mar 04 2022 08:57:13 GMT+0800 (中国标准时间)

// new Date(milliseconds)
let Jan01_1970 = new Date(0); // 0 表示 01.01.1970 UTC+0
let Dec31_1969 = new Date(-24 * 3600 * 1000); // 负时间戳

// new Date(datestring)
new Date("2017-01-26");
// 该时间未被设定，因此被假定为格林尼治标准时间（GMT）的午夜
// 并会根据代码运行时的时区进行调整，因此可能得到以下结果
// Thu Jan 26 2017 08:00:00 GMT+0800 (中国标准时间)
// Thu Jan 26 2017 11:00:00 GMT+1100 (Australian Eastern Daylight Time)
// Wed Jan 25 2017 16:00:00 GMT-0800 (Pacific Standard Time)

// new Date(year, month, date, hours, minutes, seconds, ms)
new Date(2011, 0, 1, 0, 0, 0, 0); // 1 Jan 2011, 00:00:00
new Date(2011, 0, 1, 2, 3, 4, 567); // 1.01.2011, 02:03:04.567
```

**访问日期**

1. getFullYear() 获取年份（4 位数）；
2. getMonth() 获取月份，从 0 到 11；
3. getDate() 获取当月的具体日期，从 1 到 31；
4. getHours()，getMinutes()，getSeconds()，getMilliseconds() 获取相应的时间组件；
5. getDay() 获取一周中的第几天，从 0（星期日）到 6（星期六），第一天始终是星期日；
6. getTime() 返回日期的时间戳，从 1970-1-1 00:00:00 UTC+0 到现在所经过的毫秒数；
7. getTimezoneOffset() 返回 UTC 与本地时区之间的时差，以分钟为单位；

getTimezoneOffset()
返回 UTC 与本地时区之间的时差，以分钟为单位

> Note: 很多 JavaScript 引擎都实现了一个非标准化的方法 `getYear()`，不推荐使用这个方法，直接使用 `getFullYear()`；

> Tips: `get*` （除上述最后两个） 这类方法都是基于当地时区的；可以在 "get" 之后插入 "UTC"，使用 `getUTC*` 以获取与当地时区的 UTC 对应项；

> Tips: `new Date().getUTCHours() - new Date().getHours()` 可得到当地时区相对于 UTC 的偏移；UTC 也即非夏令时的伦敦时间；

**设置日期**

1. setFullYear(year, [month], [date])
2. setMonth(month, [date])
3. setDate(date)
4. setHours(hour, [min], [sec], [ms])
5. setMinutes(min, [sec], [ms])
6. setSeconds(sec, [ms])
7. setMilliseconds(ms)
8. setTime(milliseconds)

以上方法除了 setTime() 都有 UTC 变体；

> Tips: 自动校准 是 `Date` 对象的一个非常方便的特性，当设置超范围的数值，会被自动校准；

```js
// 自动考虑闰年情况
let date = new Date(2016, 1, 28);
date.setDate(date.getDate() + 2); // 2016.03.01

let date = new Date(2016, 0, 2); // 2016.01.02
// 设置为当月的第一天
date.setDate(1); // 2016.01.01
// 天数最小可以设置为 1，所以这里设置的是上一月的最后一天
date.setDate(0); // 2015.12.31
```

> Tips: 利用 Date 对象转数字可快速获取时间戳；`+new Date()` 即当前时间戳；此外日期可以相减，相减的结果是以毫秒为单位时间差；

```js
console.log(+new Date()); // 1646360860388

let date = new Date(2022, 1, 28);
console.log(new Date() - date); // 31919237496
```

> Tips: 当只需要时间戳而不需要创建日期对象时，可以使用 Date.now() 获取当前时间戳；

```js
// 测量时间间隔
let start = Date.now(); // 从 1 Jan 1970 至今的时间戳

for (let i = 0; i < 100000; i++) {
  let doSomething = i * i * i;
}

let end = Date.now(); // 完成

alert( `The loop took ${end - start} ms` ); // 相减的是时间戳，而不是日期
```

> Tips: 直接使用日期对象相减得到时间差比使用 `getTime()` 方法慢，因为 JavaScript 引擎还要做类型转换的额外工作；

为了得到更加可靠的度量，整个度量测试包应该重新运行多次，现代的 JavaScript 引擎的先进优化策略只对执行很多次的 “hot code” 有效，即对于执行很少次数的代码没有必要优化；

```js
// 这是一个测试用例
function diffSubtract(date1, date2) {
  return date2 - date1;
}

function diffGetTime(date1, date2) {
  return date2.getTime() - date1.getTime();
}

function bench(f) {
  let date1 = new Date(0);
  let date2 = new Date();

  let start = Date.now();
  for (let i = 0; i < 100000; i++) f(date1, date2);
  return Date.now() - start;
}

let time1 = 0;
let time2 = 0;

// 在主循环中增加“升温”环节
bench(diffSubtract);
bench(diffGetTime);

// 开始度量，交替运行 10 次
for (let i = 0; i < 10; i++) {
  time1 += bench(diffSubtract);
  time2 += bench(diffGetTime);
}

alert( 'Total time for diffSubtract: ' + time1 );
alert( 'Total time for diffGetTime: ' + time2 );
```

**Date.parse**

Date.parse(str) 方法可以从一个字符串中读取日期；字符串的格式应该为：YYYY-MM-DDTHH:mm:ss.sssZ，其中：

1. YYYY-MM-DD 日期：年-月-日；
2. 字符 "T" 是一个分隔符；
3. HH:mm:ss.sss 时间：小时，分钟，秒，毫秒；
4. 可选字符 'Z' 为 +-hh:mm 格式的时区，单个字符 Z 代表 UTC+0 时区；

Date.parse(str) 调用会解析给定格式的字符串，并返回时间戳，如果给定字符串的格式不正确，则返回 NaN；

```js
let ms = Date.parse('2012-01-26T13:51:50.417-07:00'); // 1327611110417
let date = new Date( Date.parse('2012-01-26T13:51:50.417-07:00') );
```

**其他例子**

```js
// 获取某月的最后一天
function getLastDayOfMonth(year, month) {
  return new Date(year, month + 1, 0).getDate();
}

// 计算今天过去了多少秒
function getSecondsToday() {
  return Math.floor(Date.now() % (24 * 3600 * 1000) / 1000);
}
```

**相关拓展**

> 1884 年，华盛顿的国际经度会议规定，将全球按经线从东到西划分为 24 个时区，其中东、西各 12 个时区，同时规定英国（格林尼治天文台旧址）为零时区，这就诞生了第一个世界时：格林尼治标准时间（Greenwich Mean Time，缩写为GMT，又称格林尼治平时）；中国跨 5 个时区，实际上只用东八时区的标准时即北京时间为准；

> 1972 年诞生了：协调世界时（Universal Time Coordinated，缩写为UTC）；UTC 是当前的世界标准时间；UTC 与 GMT 基本上等同，误差不超过 0.9 秒；UTC 的标准格式为 2019-11-11T00:00:00.000Z，T 代表使用 UTC 时间，Z 是 UTC 偏移量，表示 UTC 时间与本地时的差别、即时差，可用以下形式表示: ±[hh]:[mm]、±[hh][mm]、±[hh]，例如北京时间比 GMT 要早 8 小时，写作 2019-11-11T08:00:00.000+0800

> Unix 时间(Unix Time)，也叫做 POSIX 时间或纪元时间(Epoch Time)，是用来记录时间的流逝，所以也常被叫做时间戳，定义为从 1970-01-01T00:00:00 开始流逝的秒数，不考虑闰秒；之后的时间是正数，之前的是负数；

[时区是怎么划分的？世界各时区的时间如何统一表达？GMT、UTC、UNIX有什么区别？](https://blog.csdn.net/zgdwxp/article/details/102728563)

#### JSON 方法，toJSON

JSON（JavaScript Object Notation）是表示值和对象的通用格式；

**JSON.stringify**

该方法将对象转换为 JSON；得到的 json 字符串是一个被称为 JSON 编码（JSON-encoded） 或 序列化（serialized） 或 字符串化（stringified） 或 编组化（marshalled） 的对象；

JSON 中没有单引号或反引号，JSON 格式字符串都使用双引号，包括对象属性名称也强制使用双引号；

JSON 支持以下数据类型：
- Objects { ... }
- Arrays [ ... ]
- strings
- numbers
- boolean true/false
- null

JSON 是语言无关的纯数据规范，因此一些特定于 JavaScript 的对象属性会被 JSON.stringify 跳过；
- 函数属性（方法）
- Symbol 类型的键和值
- 存储 undefined 的属性

```js
let user = {
  sayHi() { // 被忽略
    alert("Hello");
  },
  [Symbol("id")]: 123, // 被忽略
  something: undefined // 被忽略
};

alert( JSON.stringify(user) ); // {}（空对象）
```

> Note: `JSON.strinify` 支持嵌套对象转换，并且可以自动对其进行转换，但有重要的限制：不得有循环引用；

JSON.stringify 完整语法是 JSON.stringify(value[, replacer, space])；其中 value 是要编码的值，replacer 是要编码的属性数组或映射函数 function(key, value)，space 是用于格式化的空格数量，或者字符串占位符；

```js
let room = {
  number: 23
};

let meetup = {
  title: "Conference",
  participants: [{name: "John"}, {name: "Alice"}],
  place: room // meetup 引用了 room
};

room.occupiedBy = meetup; // room 引用了 meetup

// 属性数组
console.log( JSON.stringify(meetup, ['title', 'participants']) );
// {"title":"Conference","participants":[{},{}]}

// 改造 Object.prototype.toString 方法方便查看 replacer 调用过程
Object.prototype.toString = function () {
  return "[" + Object.keys(this).join(", ") + "]";
};

// 映射函数
console.log("result:", JSON.stringify(meetup, function replacer(key, value) {
    console.log(`${key}: ${value}`);
    return key == "occupiedBy" ? undefined : value;
  })
);
// 根据输出结果可以看出 stringify 是递归调用的；
// 第一个调用是使用特殊的“包装对象”制作的：{"": meetup}，
// 该键为空，值是整个目标对象；
/*
: [title, participants, place]
title: Conference
participants: [name],[name]
0: [name]
name: John
1: [name]
name: Alice
place: [number, occupiedBy]
number: 23
occupiedBy: [title, participants, place]
result: {"title":"Conference","participants":[{"name":"John"},{"name":"Alice"}],"place":{"number":23}}
*/
```

**自定义 “toJSON”**

像 toString 进行字符串转换，对象也可以提供 toJSON 方法来进行 JSON 转换，如果可用，JSON.stringify 会自动调用它；

> Tips: 所有日期都有一个内建的 `toJSON` 方法来返回 UTC 格式类型的字符串；

```js
JSON.stringify({date: new Date}) 
// '{"date":"2022-03-04T04:05:05.467Z"}'
```

> Tips: `toJSON` 既可以用于直接调用 `JSON.stringify` 也可以用于当对象嵌套在另一个编码对象中时；

**JSON.parse**

该方法用于解码 JSON 字符串，完整语法：JSON.parse(str, [reviver])；其中 str 是要解析的 JSON reviver 是可选的函数 function(key,value) 该函数将为每个 (key, value) 对调用，并可以对值进行转换；

> Note: JSON 不支持注释，向 JSON 添加注释无效；

```js
let str = '{"title":"Conference","date":"2017-11-30T12:00:00.000Z"}';

let meetup = JSON.parse(str, function(key, value) {
  if (key == 'date') return new Date(value);
  return value;
});

alert( meetup.date.getDate() ); // 正常运行
```

排除反向引用

```js
let room = {
  number: 23
};

let meetup = {
  title: "Conference",
  occupiedBy: [{name: "John"}, {name: "Alice"}],
  place: room
};

// 循环引用
room.occupiedBy = meetup;
meetup.self = meetup;

// 排除第一个调用时 value 是 meetup 的情况
alert( JSON.stringify(meetup, function replacer(key, value) {
  return (key != "" && value == meetup) ? undefined : value;
}));

/*
{
  "title":"Conference",
  "occupiedBy":[{"name":"John"},{"name":"Alice"}],
  "place":{"number":23}
}
*/
```

#### 递归和堆栈

递归是一种编程模式，在一个任务可以自然地拆分成多个相同类型但更简单的任务的情况下非常有用；简单来说，函数会调用自身就是所谓的递归；

```js
// 这是一个递归例子
function pow(x, n) {
  return (n == 1) ? x : (x * pow(x, n - 1));
}
```

最大的嵌套调用次数（包括首次）被称为递归深度，最大递归深度受限于 JavaScript 引擎；通常，引擎在最大迭代深度为 10000 及以下时是可靠的；

在底层，有关正在运行的函数的执行过程的相关信息被存储在其执行上下文中；执行上下文是一个内部数据结构，它包含有关函数执行时的详细细节：当前控制流所在的位置（代码位置），当前的变量，this 的值，以及其它的一些内部细节；

当一个函数进行嵌套调用时，当前函数被暂停，关联的执行上下文被一个叫做执行上下文堆栈的特殊数据结构保存；然后开始执行嵌套调用，嵌套调用结束后，从堆栈中恢复之前的执行上下文，并从停止的位置恢复外部函数；因此，递归深度等于堆栈中上下文的最大数量；

> Note: 任何递归都可以用循环来重写，通常循环变体更有效，且更节省内存；

> Tips: 一些引擎支持“尾调用（tail call）”优化：如果递归调用是函数中的最后一个调用，那么外部的函数就不再需要恢复执行，因此引擎也就不再需要记住他的执行上下文；

```js
// 数组转单链表
function arrayToList(arr, index = 0) {
  if (index < arr.length) {
    return {
      value: arr[index],
      next: arrayToList(arr, index + 1),
    };
  } else {
    return {
      value: arr[index],
      next: null,
    };
  }
}

let arr = [1, 2, 3, 4];
let list = arrayToList(arr);

/*{
  value: 1,
  next: {
    value: 2,
    next: {
      value: 3,
      next: {
        value: 4,
        next: null
      }
    }
  }
}*/

// 斐波那契
// 递归版本，受限于递归深度，n 较大时，计算时间很长；
function Fibonacci(n) {
  return n > 1 ? Fibonacci(n - 1) + Fibonacci(n - 2) : n;
}

// 改进版本 1，使用数组存储中间计算值，避免重复计算；
let record = [0, 1, 1];

function Fibonacci(n) {
  if (n < record.length) {
    return record[n];
  } else {
    return (record[n] = Fibonacci(n - 1) + Fibonacci(n - 2));
  }
}

// 改进版本 2，使用对象存储中间计算值，并用闭包封装；
export function Fibonacci(n) {
  let caches = {};

  function _fib(n) {
    if (n < 3) return 1;
    return n in caches ? caches[n] : (caches[n] = _fib(n - 1) + _fib(n - 2));
  }

  return _fib(n);
}

// 改进版本 3，使用变量存储前两个值，循环计算，速度最快；
export function Fibonacci(n) {
  if (n < 3) {
    return 1;
  }
  let lastOne = 1;
  let lastTwo = 1;

  for (let i = 3; i < n; i++) {
    [lastTwo, lastOne] = [lastOne, lastOne + lastTwo];
  }

  return lastOne + lastTwo;
}
```

#### Rest 参数与 Spread 语法

在 JavaScript 中，无论函数是如何定义的，你都可以使用任意数量的参数调用函数，未使用的参数会被函数忽略掉；

**Rest 参数**

Rest 参数可以通过使用三个点 ... 并在后面跟着包含剩余参数的数组名称，来将它们包含在函数定义中，这些点的字面意思是“将剩余参数收集到一个数组中”；

```js
// 一个任意个数参数的累加函数
function sumAll(...args) { // 数组名为 args
  let sum = 0;

  for (let arg of args) sum += arg;

  return sum;
}
```

> Note: `Rest` 参数必须放到参数列表的末尾，因为放在其他参数前面或中间就失去了其意义；

**“arguments” 变量**

函数中还有一个名为 arguments 的特殊的类数组对象，该对象按参数索引包含所有参数；arguments 也是可迭代对象；

> Note: 箭头函数没有 `arguments` 对象；

**Spread 语法**

当在函数调用中使用 spread 语法 ...arr 时，它会把可迭代对象 arr “展开”到参数列表中；spread 语法也可以与常规值结合使用；

```js
let arr = [1, -2, 3, 4];
let arr2 = [8, 3, -8, 1];
let obj = { a: 1, b: 2, c: 3 };

// 基本用法
Math.max(1, ...arr, 2, ...arr2, 25);
let merged = [0, ...arr, 2, ...arr2];

// 字符串转数组
[..."hello"]; // ['h', 'e', 'l', 'l', 'o']

// 创建新对象或数组
let objCopy = { ...obj };
let arrCopy = [...arr];
```

> Tips: `Spread` 语法只适用于可迭代对象；而 `Array.from` 既可以用于类数组对象也可用于可迭代对象，因此，对于转换为数组的任务，`Array.from` 往往更通用；

> Tips: `Spread` 语法可以实现 `Object.assign()` 一样的浅拷贝效果，且更简介；

<!-- Object.assign会调用[[Setter]]，Spread语法不会，待验证 -->

#### 变量作用域，闭包

**变量**

如果在代码块 {...} 内声明（使用 let 或 const）了一个变量，那么这个变量只在该代码块内可见；该规则也适用于 if，for 和 while 等，以及函数体中；ES6 之前没有块级作用域；

在 JavaScript 中，每个运行的函数，代码块 {...} 以及整个脚本，都有一个被称为词法环境（Lexical Environment） 的内部（隐藏）的关联对象；

其中，词法环境对象由两部分组成：一个存储所有局部变量作为其属性（包括一些其他信息，如 this 的值）的对象，也称为环境记录（Environment Record）；另一个是对外部词法环境的引用，与外部代码相关联；全局词法环境没有外部引用（其引用为 null）；

> Note: “词法环境”是一个规范对象（specification object）：它仅仅是存在于编程语言规范中的“理论上”存在的，用于描述事物如何运作的对象，无法在代码中获取该对象并直接对其进行操作；

> Note: 一个“变量”只是环境记录这个特殊的内部对象的一个属性；“获取或修改变量”意味着“获取或修改词法环境的一个属性”；

> Environment Record is an abstract class with three concrete subclasses: declarative Environment Record, object Environment Record, and global Environment Record. Function Environment Records and module Environment Records are subclasses of declarative Environment Record.
> Every Environment Record has an [[OuterEnv]] field, which is either null or a reference to an outer Environment Record. 
> A var statement declares variables that are scoped to the running execution context's VariableEnvironment. Within the scope of any VariableEnvironment a common BindingIdentifier may appear in more than one VariableDeclaration but those declarations collectively define only one variable.

> A function Environment Record is a declarative Environment Record that is used to represent the top-level scope of a function and, if the function is not an ArrowFunction, provides a this binding. [[ThisBindingStatus]] filed value should be lexical, initialized, or uninitialized, If the value is lexical, this is an ArrowFunction and does not have a local this value.

> The value of the Function component of the running execution context is also called the `active function object`.

> The LexicalEnvironment and VariableEnvironment components of an execution context are always Environment Records.

执行上下文主要包括，Realm、code evaluation state、Function、ScriptOrModule 和词法环境、变量环境（var 关键字声明的变量）、私有环境（包含 class 私有变量）；

**函数声明**

函数其实也是一个值，就像变量一样，不同之处在于函数声明的初始化会被立即完成（函数声明提升），当创建了一个词法环境时，函数声明会立即变为即用型函数，因此可以在（函数声明）的定义之前调用函数声明；正常来说，这种行为仅适用于函数声明，而不适用于将函数分配给变量的函数表达式；

**内部和外部词法环境**

当代码要访问一个变量时首先会搜索内部词法环境，然后搜索外部环境，然后搜索更外部的环境，以此类推，直到全局词法环境；这也是内部变量覆盖外部同名变量的原因；

如果在任何地方都找不到这个变量，那么在严格模式下就会报错，在非严格模式下，为了向下兼容，给未定义的变量赋值会创建一个全局变量；

函数将从内到外依次在对应的词法环境中寻找目标变量，它使用最新的值；旧变量值不会保存在任何地方；当一个函数想要一个变量时，它会从自己的词法环境或外部词法环境中获取当前值；

**返回函数**

所有的函数在“诞生”时都会记住创建它们的词法环境，从技术上讲，所有函数都有名为 [[Environment]] 的隐藏属性，该属性保存了对创建该函数的词法环境的引用；

> Tips: 函数记住它创建于何处的方式，与函数被在哪儿调用无关；`[[Environment]]` 引用在函数创建时被设置并永久保存，并且不会改变；只有遇到调用函数时，函数词法环境才会被创建，这也是函数中 this 是动态绑定的原因；

```js
// 解释阶段
// 1. 脚本开始执行，创建全局执行上下文；
// 2. 创建一个全局词法环境填充所有（let、const、function）声明的变量（未初始化）以及函数；
// 3. 创建一个全局变量环境，填充所有 var 关键字定义的变量，并初始化为 undefined
// 4. 将外部环境置为 null；

// 执行阶段
// 5. 此时，先定义 counter，值为 undefined；
// 6. 然后执行 makeCounter 后，为 counter 赋值；
let counter = makeCounter();


// 7. 创建新的执行上下文函数，创建函数的词法环境，并存储这个调用的局部变量和参数；
// 8. 同时将外部环境指向全局词法环境；
function makeCounter() {
  let count = 0;

  // 9. 返回时，创建一个嵌套函数，并将嵌套函数的外部词法环境指向 makeCounter；
  return function() {
    // 11. 执行 count++ 时，从自身词法环境中查找 count，然后查找 makeCounter 词法环境中的变量，完成修改后返回；
    return count++;
  }
}

// 10. 执行 counter 函数，创建一个新的词法环境，并从 [[Environment]] 中获取其外部词法环境引用；
counter();
```

闭包是指内部函数总是可以访问其所在的外部函数中声明的变量和参数，即使在其外部函数被返回（寿命终结）了之后；

> Note: 在 JavaScript 中，所有函数都是天生闭包的（除 "new Function" 语法）；也就是说：JavaScript 中的函数会自动通过隐藏的 `[[Environment]]` 属性记住创建它们的位置，所以它们都可以访问外部变量；

通常，函数调用完成后，会将词法环境和其中的所有变量从内存中删除，但当有一个嵌套函数在外部函数结束后仍可达，则它将具有引用词法环境的 [[Environment]] 属性；因此词法环境仍会被保留在内存；

**实践**

> Note: 理论上当函数可达时，它外部的所有变量也都将存在；但在实际中，JavaScript 引擎会试图优化它，它们会分析变量的使用情况，如果从代码中可以明显看出有未使用的外部变量，那么就会将其删除；

> Note: 在 `V8`（Chrome，Edge，Opera）中的一个重要的副作用是，此类变量在调试中将不可用（未定义），或者得到一个同名的外部变量；

```js
// “不存在”的变量和“未初始化”的变量之间的特殊差异
let x = 1;

function func() {
  // 一个变量从技术的角度来讲是存在的，但是在 let 之前还不能使用，该区域也称死区
  console.log(x); // ReferenceError: Cannot access 'x' before initialization
  let x = 2;
}

func();
```

> Note: `AO` 类似于是函数被调用时创建的一个特殊 `VO`，它在 `VO` 的基础上添加了实际调用函数时传入的参数和 `arguments` 对象，还有添加 `this` 对象；`VO` 和 `AO` 被创建时会先后执行著名的函数声明提升和变量声明提升，提升上来的变量和函数挂载到 `VO / AO` 对象的上，其实是作为它的属性存在的；

> Tips: 在调试函数时，`Scope` 的最上层是 `Local`，也就是当前执行上下文的变量对象，下面就是函数的`[[Scopes]]` 属性里保存的父级层级链；点击 `Call Stack` 栏中的函数，还可以切换当前执行上下文，观察下面 `Scope` 的变化；

> Tips: 使用 `console.dir` 可以打印出函数的 `length`、`name`，甚至作用域链 [[Scopes]] 等隐藏属性；严格模式下无法获取 `caller`，`callee`，`arguments` 等属性；

- [JavaScript 现代教程-变量作用域，闭包文档](https://zh.javascript.info/closure)
- [深入 js——作用域链](https://juejin.cn/post/6844904050824052744)
- [JS 规范中的 Execution Context 和 Scope 概念有什么区别？](https://www.zhihu.com/question/51336888)
- [了解 JS 中的ECStack、EC、VO 和 AO](https://zhuanlan.zhihu.com/p/311196297)
- [JS夯实之执行上下文与词法环境](https://juejin.cn/post/6844904145372053511)
- [Lexical environment and function scope](https://stackoverflow.com/questions/12599965/lexical-environment-and-function-scope)
- [Executable Code and Execution Contexts](https://tc39.es/ecma262/#sec-executable-code-and-execution-contexts)

#### 旧时的 var

在 JavaScript 中，有三种声明变量的方式：let，const（现代方式），var（过去留下来的方式）；

**var 没有块级作用域**

用 var 声明的变量，不是函数作用域就是全局作用域；它们在代码块外也是可见的也就是说，var 声明的变量只有函数作用域和全局作用域，没有块级作用域；

这就导致 if 语句或循环语句外可以访问在 {...} 中使用 var 定义的变量；var 穿透了 if，for 和其它代码块，这是因为在早期的 JavaScript 中，块没有词法环境，而 var 就是这个时期的代表之一；

**var 允许重新声明**

使用 var，可以重复声明一个变量，不管多少次都行；如果对一个已经声明的变量使用 var，这条新的声明语句会被忽略：

**var 提升变量声明**

var 声明的变量会在函数开头被定义，与它在代码中定义的位置无关；声明被提升，但是赋值不会；

```js
function sayHi() {
  // phrase 实际声明处

  alert(phrase); // undefined

  // phrase 赋值
  phrase = "Hello";

  // 代码块会被忽略，phrase 仍然会被声明
  if (false) {
    var phrase;
  }
}
sayHi();
```

**立即执行函数**

在之前，JavaScript 中只有 var 这一种声明变量的方式，并且这种方式声明的变量没有块级作用域，于是就有了一种模仿块级作用域的方法，这种方法被称为“立即调用函数表达式”（immediately-invoked function expressions，IIFE）；

通常需要使用圆括号把该函数表达式包起来，以告诉 JavaScript，这个函数是在另一个表达式的上下文中创建的，因此它是一个函数表达式：它不需要函数名，可以立即调用；

```js
// 创建 IIFE 的方法
(function() {
  alert("Parentheses around the function");
})();

(function() {
  alert("Parentheses around the whole thing");
}());

!function() {
  alert("Bitwise NOT operator starts the expression");
}();

+function() {
  alert("Unary plus starts the expression");
}();

~(function () {
  alert("Unary plus starts the expression");
})();
```

#### 全局对象

全局对象提供可在任何地方使用的变量和函数，且其所有属性都可以被直接访问；默认情况下，这些全局变量内建于语言或环境中；在浏览器中，它的名字是 “window”，对 Node.js 而言，它的名字是 “global”，其它环境可能用的是别的名字；

> Tips: 最近，`globalThis` 被作为全局对象的标准名称加入到了 JavaScript 中，所有环境都应该支持该名称，所有主流浏览器都支持它；

> Note: 在浏览器中，使用 `var` 声明的全局函数和变量会成为全局对象的属性；

如果一个值非常重要，且需要在全局范围内可用，可以直接将其作为属性写入；但一般不建议使用全局变量，全局变量应尽可能的少；与使用外部变量或全局变量相比，函数获取“输入”变量并产生特定“输出”的代码设计更加清晰，不易出错且更易于测试；

另外，一般还使用全局对象来测试对现代语言功能的支持；对于没有某些现代功能的旧版浏览器，可以创建 “polyfills”：添加环境不支持但在现代标准中存在的功能；

#### 函数对象，NFE

在 JavaScript 中，函数就是对象，可以把函数理解成可被调用的“行为对象（action object）”；我们不仅可以调用它们，还能把它们当作对象来处理：增/删属性，按引用传递等；

**name 属性**

一个函数的名字可以通过属性 “name” 来访问；名称赋值的逻辑很智能，即使是函数表达式，被创建时没有名字，名称赋值的逻辑也能给它赋予一个正确的名字，然后进行赋值；

> Note: 规范中把这种特性叫做上下文命名，如果函数自己没有提供，那么在赋值中，会根据上下文来推测一个；但当 JavaScript 引擎无法推测名字时，属性 name 就会是空；

```js
let sayHi = function() {
  alert("Hi");
};
alert(sayHi.name); // sayHi

// 对象中的方法也有 name 属性
let user = {

  sayHi() {
    // ...
  },
}
alert(user.sayHi.name); // sayHi

// 在数组中创建的匿名函数没有名称
let funcArr = [function () {}, function func() {}];

// 引擎无法设置正确的名字，所以没有值
console.warn(funcArr[0].name); // <空字符串>
console.warn(funcArr[1].name); // func
```

**length 属性**

length 是函数的另一个内建属性，它返回函数入参的个数；

> Tips: rest 参数不参与计数，属性 length 有时在操作其它函数的函数中用于做内省/运行时检查（introspection）；

```js
// 根据函数参数个数的不同做不同处理，也属于多态的一种
function func(...handlers) {
  for (const handler of handlers) {
    console.log(handler.length); // 1 0

    if (handler.length == 0) {
      // do something
      continue;
    }

    handler(1);
  }
}

func((param) => param, (...rest) => rest);
```

**自定义属性**

在函数内可以使用函数名为函数添加自定义的属性；可以函数当作对象，在函数里存储属性，这对函数的执行没有任何影响；但变量不是函数属性，反之亦然；

> Tips: 函数属性有时会用来替代闭包；区别在于，闭包的属性外部无法访问；

```js
function makeCounter() {

  function counter() {
    return counter.count++;
  };

  counter.count = 0;
  counter.set = value => count = value;
  counter.decrease = () => count--;

  return counter;
}

let counter = makeCounter();
alert( counter() ); // 0
alert( counter() ); // 1
```

**命名函数表达式**

命名函数表达式（NFE，Named Function Expression），指带有名字的函数表达式的术语；但它仍是一个函数表达式，不会成为一个函数声明；NFE 在函数内部可以应用自己，且该函数名在函数外部不可见；

```js
let sayHi = function func(who) {
  if (who) {
    alert(`Hello, ${who}`);
  } else {
    func("Guest"); // 使用 func 再次调用函数自身
  }
};

sayHi(); // Hello, Guest
func(); // Error, func is not defined（在函数外不可见）
```

> jQuery，lodash 等很多 JavaScript 库都充分利用了这个功能，实际上，这么做是为了减少对全局空间的污染，这样一个库就只会有一个全局变量，也降低了命名冲突的可能性；

```js
// 任意数量的括号求和
function sum(param) {
  sum.count = 0;

  _add.toString = function () {
    return sum.count;
  };

  function _add(p) {
    sum.count += p;
    return _add;
  }

  return _add(param);
}

console.log(sum(3)(4)(5)); // 12

// 升级版
function sum2(a) {
  const next = (b) => sum(a + b);
  next.valueOf = () => a;
  return next;
}
```

#### new Function 语法

还有一种很少见的函数创建方法；使用 Function 创建函数：`new Function ([arg1, arg2, ...argN], functionBody);`，由于历史原因，参数也可以按逗号分隔符的形式给出，这种方法实际上是通过运行时通过参数传递过来的字符串创建的；

另外，如果使用 new Function 创建一个函数，那么该函数的 [[Environment]] 并不指向当前的词法环境，而是指向全局环境；

#### 调度：setTimeout 和 setInterval

> Note: 这两个方法并不在 JavaScript 的规范中，但是大多数运行环境都有内建的调度程序，并且提供了这些方法；目前来讲，所有浏览器以及 Node.js 都支持这两个方法；

**setTimeout**

setTimeout 允许我们将函数推迟到一段时间间隔之后再执行；`let timerId = setTimeout(func|code, [delay], [arg1], [arg2], ...)`，setTimeout 在调用时会返回一个“定时器标识符（timer identifier）”，可以将它传入 clearTimeout 方法来取消执行；

| 参数 | 说明 |
| :----- | :----- |
| func\|code | 要执行的函数或代码字符串，一般传入的都是函数；由于历史原因，支持传入代码字符串 |
| delay | 执行前的延时，以毫秒为单位，默认值是 0 |
| arg1，arg2… | 要传入被执行函数（或代码字符串）的参数列表（IE9 以下不支持） |

```js
function sayHi(phrase, who) {
  alert( phrase + ', ' + who );
}

let timerId = setTimeout(sayHi, 1000, "Hello", "John"); // Hello, John

// 如果第一个参数位传入的是字符串，JavaScript 会自动为其创建一个函数
setTimeout("alert('Hello')", 1000); // 不建议使用，可用箭头函数代替

// 取消定时器
clearTimeout(timerId);
```

> Note: 在浏览器中，定时器标识符是一个数字；在 `Node.js` 中返回的是一个定时器对象，这个对象包含一系列方法；

> Tips: `setTimeout` 有一个特殊用法：`setTimeout(func, 0)`，或者仅仅是 `setTimeout(func)`；这样调度可以让 `func` 尽快执行，该函数被调度在当前脚本执行完成“之后”立即执行；

> Note: 浏览器环境下，零延时实际上不为零，嵌套定时器的运行频率是受限制的；根据 HTML5 标准 所讲：“经过 `5` 重嵌套定时器之后，时间间隔被强制设定为至少 `4` 毫秒”；

**setInterval**

setInterval 允许我们重复运行一个函数，从一段时间间隔之后开始运行，之后以该时间间隔连续重复运行该函数；该方法与 setTimeout 用法相同，所有参数的意义也相同；使用 clearInterval(timerId) 方法清除；

> Note: 在大多数浏览器中，包括 Chrome 和 Firefox，在显示 `alert/confirm/prompt` 弹窗时，内部的定时器仍旧会继续“嘀嗒”；因此如果未及时关闭弹窗，可能会出现关闭弹窗后立即弹出新的弹窗；

使用 setInterval 时，函数的实际调用间隔要比代码中设定的时间间隔要短；极端情况下，如果函数执行时间远大于预设时间间隔段时，JavaScript 引擎会等待函数执行完成，然后检查调度程序之后，立即再次执行函数；

> Tips: 相较而言，嵌套的 `setTimeout` 能够精确地设置两次执行之间的延时，而 `setInterval` 却不能；并且嵌套的 `setTimeout` 要比 `setInterval` 灵活得多；采用这种方式可以根据当前执行结果来调度下一次调用，因此下一次调用可以与当前这一次不同；

> Tips: 当一个函数传入 `setInterval/setTimeout` 时，将为其创建一个内部引用，并保存在调度程序中；这样，即使这个函数没有其他引用，也能防止垃圾回收器（GC）将其回收；同时该函数引用的外部变量也会被保留，因此，当我们不再需要调度函数时，最好取消它，即使这是个（占用内存）很小的函数；

```js
// 任何 setTimeout 都只会在当前代码执行完毕之后才会执行
let i = 0;

setTimeout(() => alert(i), 100); // 100000000

// 假设这段代码的运行时间 >100ms
for(let j = 0; j < 100000000; j++) {
  i++;
}
```

> Note: `setTimeout()` 和 `setInterval()` 共用一个编号池，技术上，`clearTimeout()` 和 `clearInterval()` 可以互换；但是，为了避免混淆，不要混用取消定时函数；

- [window.setTimeout MDN 中文参考文档](https://developer.mozilla.org/zh-CN/docs/Web/API/setTimeout)

#### 装饰器模式和转发，call/apply

**装饰器**

装饰器（decorator）：一个特殊的函数，可以被看作是可以添加到函数的 “features” 或 “aspects”，可以添加一个或添加多个装饰器；装饰器是可重用的，逻辑独立的；

```js
function slow(x) {
  // 这里可能会有重负载的 CPU 密集型工作
  alert(`Called with ${x}`);
  return x;
}

function cachingDecorator(func) {
  let cache = new Map();

  return function(x) {
    if (cache.has(x)) {    // 如果缓存中有对应的结果
      return cache.get(x); // 从缓存中读取结果
    }

    // let result = func(x);
    let result = func.call(this, x);  // 否则就调用 func

    cache.set(x, result);  // 然后将结果缓存（记住）下来
    return result;
  };
}

let newSlow = cachingDecorator(slow);
```

特殊的内建函数方法 func.call(context, arg1, arg2, ...)，它允许调用一个显式设置 this 的函数；另一个内建方法 func.apply(context, args)，它与 func.call 区别在第二个参数使用类数组对象 args 作为参数列表（arguments）；

> Note: `func.call` 在 `func` 函数运行时使用的 `this` 值可能不是该方法看到的实际值：如果这个函数处于非严格模式下，则指定为 `null` 或 `undefined`（缺省时） 时会自动替换为指向全局对象，原始值会被包装；

> Note: `Spread` 语法 `...` 允许将可迭代对象 `args` 作为列表传递给 `func.call`，而 `func.apply` 只接受类数组 `args`；

> Tips: 即可迭代又是类数组的对象，使用 call 或 apply 均可，但是 apply 可能会更快，因为大多数 JavaScript 引擎在内部对其进行了优化；

- [Function.prototype.call MDN 中文参考文档](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/call)

**呼叫转移**

将所有参数连同上下文一起传递给另一个函数被称为“呼叫转移（call forwarding）”；

```js
// 一个呼叫转移的最简单形式
let wrapper = function() {
  return func.apply(this, arguments);
};
```

**方法借用**

方法借用（method borrowing）：从一个对象中获取一个方法，并在另一个对象的上下文中“调用”它；

```js
function hash() {
  alert( [].join.call(arguments) ); // 1,2
}

hash(1, 2);
```

> Tips: 采用数组方法并将它们应用于参数 `arguments` 是很常见的，另一种方法是使用 Rest 参数对象，该对象是一个真正的数组；


**防抖和节流**

debounce 会在“冷却（cooldown）”期后运行函数一次，适用于处理最终结果；throttle 运行函数的频率不会大于所给定的时间 ms 毫秒，适用于不应该经常进行的定期更新；

```js
// 防抖装饰器
function debounce(func, ms) {
  let timer = null;

  return function () {
    clearTimeout(timer); // 刷新计时器
    timer = setTimeout(() => func.apply(this, arguments), ms);
  };
}

// 节流装饰器
function throttle(func, ms) {
  let timer = null;
  return function () {
    if (timer) return; // 忽略执行
    timer = setTimeout(() => {
      func.apply(this, arguments);
      timer = null;
    }, ms);
  };
}
```

#### 函数绑定

浏览器中的 setTimeout 方法有些特殊：它为函数调用设定了 this=window（对于 Node.js，this 则会变为计时器（timer）对象；即使是在严格模式下，setTimeout() 的回调函数里面的 this 仍然默认指向 window 对象，并不是 undefined；

函数提供了一个内建方法 bind，可以绑定 this；func.bind(context) 的结果是一个特殊的类似于函数的“外来对象（exotic object）”，它可以像函数一样被调用，并且透明地（transparently）将调用传递给 func 并设定 this=context；

```js
let user = {
  firstName: "John",
  sayHi() {
    alert(`Hello, ${this.firstName}!`);
  }
};

// 可以在没有对象（译注：与对象分离）的情况下运行它
sayHi(); // Hello, John!

// 还是会使用预先绑定（pre-bound）的值，该值是对旧的 user 对象的引用
setTimeout(user.sayHi.bind(user), 1000); // Hello, John!

// 调用了最新的 sayHi 方法；
setTimeout(() => user.sayHi(), 1000); // Another user in setTimeout!

// user 的值在不到 1 秒内发生了改变
user = {
  sayHi() { alert("Another user in setTimeout!"); }
};
```

> Tips: 如果一个对象有很多方法，并且我们都打算将它们都传递出去，那么我们可以在一个循环中完成所有方法的绑定；或者也可以使用 lodash 中的 `_.bindAll(object, methodNames)` 方法；

此外，func.bind(context, [arg1], [arg2], ...) 不仅可以绑定 this，还可以绑定参数（arguments）；可以通过绑定先有函数的一些参数来创建一个新函数，被称为偏函数应用程序（partial function application）；

使用偏函数可以创建一个具有可读性高的名字（double，triple）的独立函数；另一方面，当我们有一个非常通用的函数，并希望有一个通用型更低的该函数的变体时，偏函数会非常有用；lodash 库有现成的 _.partial 实现；

> Note: 绑定函数的上下文是硬绑定（hard-fixed）的，无法再修改它；因此被绑定函数内部的 this 指向，或者对象的属性值会被固定，即会使用预先绑定（pre-bound）的值；

> Note: 一个函数不能被重绑定（re-bound），二次绑定的值无效；在非严格模式下，ES5 标准会将值为 `null` 或 `undefined` 的 `this` 绑定到全局对象，也就是 `this=window`；

```js
// 一个偏函数的例子
function mul(a, b) {
  return a * b;
}

let double = mul.bind(null, 2); // 第一个参数必填
double(3); // 6

// 偏函数装饰器
function partial(func, ...argsBound) {
  return function(...args) {
    // 调用时，获取当前 this，即不关心绑定对象；
    return func.call(this, ...argsBound, ...args);
  }
}

// 二次绑定
function f() {
  alert(this.name);
}

f = f.bind( {name: "John"} ).bind( {name: "Pete"} );

f(); // John
```

#### 深入理解箭头函数

箭头函数是针对那些没有自己的“上下文”，但在当前上下文中起作用的短代码的；箭头函数具有以下特点：

- 没有 this；
- 没有 arguments；
- 不能使用 new 进行调用；
- 也没有 super；

> Note: 箭头函数不具有 `this` 自然也就意味着另一个限制：箭头函数不能用作构造器（constructor），因此不能用 `new` 调用它们；

> Note: 箭头函数 `=>` 没有创建任何绑定，箭头函数只是没有 `this`；`this` 的查找与常规变量的搜索方式完全相同：在外部词法环境中查找；ES6 中的箭头函数并不会创建其自身的执行上下文，所以箭头函数中的 this 取决于它的外部函数；箭头函数内部的 this 会指向声明箭头函数时所在作用域的 this；

> Note: In modules, like `<script type="module">` or `node`, the value of `this` is always `undefined` in the global context. Modules are implicitly in `strict mode`.

**this 绑定**

在绝大多数情况下，函数的调用方式决定了 this 的值（运行时绑定），this 不能在执行期间被赋值，并且在每次函数被调用时 this 的值也可能会不同；在不同情况下，this 值的表现有所不同；

> A regular declarative Environment Record (i.e., one that is neither a function Environment Record nor a module Environment Record) does not provide a this binding.
> Function Environment Records if envRec.[[ThisBindingStatus]] is not lexical, then Return envRec.[[ThisValue]].
> Object Environment Records do not provide a this binding. 
> Module Environment Records always provide a this binding.
> Global Environment Record Return envRec.[[GlobalThisValue]].

规范环境记录中的 this：

1. 全局环境记录，返回 envRec.[[GlobalThisValue]]；
2. 对象环境记录，没有 this；
3. 声明环境记录，
   - 常规声明环境记录，没有 this；
   - 函数环境记录，非箭头函数返回 envRec.[[ThisValue]]；
   - 模块环境记录，返回 undefined；

| 环境 | 非严格模式 | 严格模式 |
| :----- | :----- | :----- |
| 全局上下文 | 全局对象 | 全局对象 |
| 函数上下文 | 全局对象 | undefined |
| 对象上下文 | 调用的对象 | 调用的对象 |
| 模块上下文 | 自动开启严格模式 | 在浏览器端 ES6 模块（`<script type="module">`）中为 undefined，在 Node CommonJS 模块中为当前模块 |
| bind, call, apply | 传入的 thisArg，非对象 thisArg 会被转为对象 | 传入的 thisArg，不会进行对象转换 |
| 箭头函数 | 从创建的执行上下文获取，顶级为全局变量 | 从创建的执行上下文获取，顶级为 undefined |
| eval | 直接调用同箭头函数；间接调用为全局对象 | 直接调用同箭头函数，间接调用为全局对象 |
| setTimeout, setInterval | 将普通函数的 this 绑定为 window 或 Timeout 对象 | 同非严格模式 |

```js
// 全局环境中
// 无论是否在严格模式下，在任何函数体外部的 this 都指向全局对象
console.log(this === globalThis); // true
"use strict";console.log(this === globalThis); // true

// 函数环境中，this 的值取决于函数被调用的方式
// 1. 直接调用
function func() {
  console.log(this); // window / global
}
func();
"use strict";function func() {
  console.log(this); // undefined
}
func();
// 2. 作为对象方法调用，无关严格模式与否
let user = {
  say() {
    console.log(this);
  }
}
user.say(); // user

// 函数绑定和转移 bind, call, apply
// 如果 thisArg 不是对象，则会被尝试转换为对象；null 和 undefined 被转换为全局对象；
let obj = { name: "obj" };
user.say.bind()(); // window / global
user.say.bind(null)(); // window / global
user.say.bind("str")(); // String {'str'} / [String: 'str']
user.say.bind(obj); // { name: "obj" }
user.say.call(); // window / global
user.say.call(null); // window / global
user.say.call(1234); // Number {1234} / [Number: 1234]
user.say.call(obj); // { name: "obj" }
"use strict";
user.say.bind()(); // undefined
user.say.bind(null)(); // null
user.say.bind("str")(); // str
user.say.bind(obj); // { name: "obj" }
user.say.call(); // undefined
user.say.call(null); // null
user.say.call(1234); // 1234
user.say.call(obj); // { name: "obj" }

// 箭头函数中
// this 与封闭词法环境的 this 保持一致；在全局代码中，它将被设置为全局对象；
let fn = () => { console.log(this) }
// return by function
let returnFunc = function() {
  return () => { console.log(this) };
}
// inside function
let outerFunc = function() {
  let _f = () => { console.log(this) };
  _f();
}
// inside object
const obj = {
  myFun: () => console.log(this),
};
// class
class MyCls{
  arrow = () => console.log(this === MyCls, this instanceof MyCls)
  static staticArrow = () => console.log(this === MyCls, this instanceof MyCls)
}

fn(); // window / global
returnFunc()(); // window / global
outerFunc(); // window / global
obj.myFun(); // window / global

"use strict";
fn(); // window / global
returnFunc()(); // undefined
outerFunc(); // undefined
obj.myFun(); // window / global
// class auto use strict mode
new MyCls().arrow(); // false true (instance)
MyCls.staticArrow(); // true false (MyCls)

// Eval
// 1. 直接调用，eval 代码段中的 this 取决于当前执行上下文
eval("console.log(this)"); // window / global
(eval)("console.log(this)"); // window / global
eval("!(() => {console.log(this)})()"); // window / global
eval("!function() {console.log(this)}()"); // window / global
function evalOuter() {
  eval("console.log(this)");
}
evalOuter(); // window / global
let obj = {
  evalOuter,
};
obj.evalOuter(); // obj

"use strict";
// 其余例子结果相同，略
function evalOuter() {
  eval("console.log(this)");
}
evalOuter(); // undefined
let obj = {
  evalOuter,
};
obj.evalOuter(); // obj

// 2. 间接调用，eval 代码段中的 this 指向全局执行上下文，严格模式同
eval?.("console.log(this)") // window / global
window.eval("console.log(this)"); // window
global.eval("console.log(this)"); // global
eval.call(null, "console.log(this)"); // window / global
eval.bind(null, "console.log(this)")(); // window / global
let originalEval = (code) => eval(code);
originalEval("console.log(this)"); // window / global

// setTimeout, setInterval
// 将传入的普通函数 this 设为全局对象，对箭头函数或 bind 生成的函数无效
function func() {
  console.log(this);
}
let arrowFunc = () => console.log(this);
let obj = {
  func() {
    console.log(this);
  },
  arrowFunc : () => console.log(this);
}

setTimeout(func, 100); // window / Timeout
setTimeout(arrowFunc, 100); // window / global
setTimeout(obj.func, 100); // window / Timeout
setTimeout(obj.arrowFunc, 100); // window / global
setTimeout(() => {func(); obj.func()}, 100); // window + obj / global
setTimeout(() => {arrowFunc(); obj.arrowFunc();}, 100); // window + window / global + global
setTimeout(func.bind(null), 100); // window + window / global + global

"use strict";
setTimeout(() => {func(); obj.func()}, 100); // undefined + obj / undefined + obj
setTimeout(() => {arrowFunc(); obj.arrowFunc();}, 100); // window + window / global + global

// 引用丢失
let obj = {
  refFunc(){
    console.log(this);
  }
}
const g = (f) => f();
const h = obj.refFunc;
const j = () => obj.refFunc;
g(obj.refFunc); // No base ref.
h(); // No base ref.
j()(); // No base ref.
(0, obj.refFunc)(); // Another common pattern to remove the base ref.
```

- [ES6 箭头函数的 this 指向详解](https://zhuanlan.zhihu.com/p/57204184)
- [箭头函数和 this 指向](https://www.jianshu.com/p/59be0fc4f4a6)
- [浅谈箭头函数和 setTimeout 中的 this](https://cnblogs.com/jeodeng/p/10658590.html)
- [JavaScript 的 this 原理](http://www.ruanyifeng.com/blog/2018/06/javascript-this.html)
- [How does the "this" keyword work?](https://stackoverflow.com/questions/3127429/how-does-the-this-keyword-work?rq=1)
- [this MDN 中文参考文档](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/this)
- [eval() MDN 中文参考文档](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/eval)
- [window.setTimeout MDN 中文参考文档](https://developer.mozilla.org/zh-CN/docs/Web/API/setTimeout)

#### 属性标志和属性描述符

对象属性（properties），除 value 外，还有三个特殊的特性（attributes），也就是所谓的“标志”；默认都为 true；

- writable 表示是否值可以被修改；
- enumerable 表示是否会被在循环中列出；
- configurable 表示是否此特性可以被删除，这些属性也可以被修改；

**writable**

通过更改 writable 标志来把对象属性设置为只读，不能被重新赋值；

> Note: 在非严格模式下，在对不可写的属性等进行写入操作时，不会出现错误，但是操作仍然不会成功；因为在非严格模式下，违反标志的行为（flag-violating action）只会被默默地忽略掉；

**enumerable**

通过更改 enumerable 标志来把对象属性设置为不可枚举，不会显示在 for..in 中；也会被 Object.keys 排除；

**configurable**

通过设置 configurable 标志把对象属性标志设置为不能被删除（delete），它的特性（attribute）不能被修改；

configurable: false 防止更改和删除属性标志，但是允许更改对象的值，不可配置标志（configurable:false）有时会预设在内建对象和属性中；

> Note: 对于不可配置的属性，我们可以将 `writable: true` 更改为  `false`，从而防止其值被修改（以添加另一层保护），但无法反向行之；

**Object.getOwnPropertyDescriptor / Object.getOwnPropertyDescriptors**

可以通过 Object.getOwnPropertyDescriptor(obj, propertyName) 方法获取，返回一个“属性描述符”对象：包含值和所有的标志；

或者可以使用 Object.getOwnPropertyDescriptors(obj) 方法一次获取对象所有包含 symbol 类型的和不可枚举的属性在内的属性描述符；

**Object.defineProperty / Object.defineProperties**

可以使用 Object.defineProperty(obj, propertyName, descriptor) 方法修改标志；如果该属性存在，defineProperty 会更新其标志，否则它会使用给定的值和标志创建属性；在这种情况下，如果没有提供标志，则会假定它是 false；

或者使用 Object.defineProperties(obj, descriptors) 方法同时定义多个属性；

> Tips: 克隆对象的“标志感知”方式，但无法克隆原型链：`let clone = Object.defineProperties({}, Object.getOwnPropertyDescriptors(obj))`，

```js
let user = {
  name: "Coley48",
  toString() {
    return this.name;
  },
};

// 常规方式定义的属性，标志默认全为 true
Object.getOwnPropertyDescriptor(user, "name");
// {value: 'Coley48', writable: true, enumerable: true, configurable: true}

// 更改旧属性
Object.defineProperty(user, "toString", {
  // 只更新 enumerable 标志
  enumerable: false,
});

// 跳过不可枚举的属性
for (let key in user) console.warn(key); // name
Object.keys(user); // ['name']

// 定义新属性
Object.defineProperty(user, "important", {
  // value 默认 undefined
  // enumerable 未显式给出，默认为 false
  writable: false,
  configurable: false,
});

// 更改不可配置属性 Cannot redefine property: important
Object.defineProperty(user, "important", {
  writable: true,
  configurable: false,
});

// 获取全部属性的标志对象
let descriptors = Object.getOwnPropertyDescriptors(user);

console.log(descriptors);
```

**全局封闭对象**

属性描述符在单个属性的级别上工作，以下方法可以限制整个对象访问，但在实际开发中较少用到；

- Object.preventExtensions(obj) 禁止向对象添加新属性
- Object.seal(obj) 禁止添加/删除属性；为所有现有的属性设置 configurable: false；
- Object.freeze(obj) 禁止添加/删除/更改属性；为所有现有的属性设置 configurable: false, writable: false；
- Object.isExtensible(obj) 如果添加属性被禁止，则返回 false，否则返回 true；
- Object.isSealed(obj) 如果添加/删除属性被禁止，并且所有现有的属性都具有 configurable: false则返回 true；
- Object.isFrozen(obj) 如果添加/删除/更改属性被禁止，并且所有当前属性都是 configurable: false, writable: false，则返回 true；

#### 属性的 getter 和 setter

对象属性分为两种，一种是数据属性，另一种是访问器属性（accessor properties）；本质上是用于获取和设置值的函数，但从外部代码来看就像常规属性；

**getter 和 setter**

访问器属性由 “getter” 和 “setter” 方法表示，在对象字面量中，它们用 get 和 set 表示；

从外表看，访问器属性看起来就像一个普通属性，这就是访问器属性的设计思想；不以函数的方式调用 getter / setter 属性，当读取 / 设置属性时，getter / setter 会在幕后运行；

> Getters 给你一种方法来定义一个对象的属性，但是在访问它们之前不会计算属性的值。 getter 延迟计算值的成本，直到需要此值，如果不需要，您就不用支付成本；

> 一种额外的优化技术是用智能(或称记忆化)getters 延迟属性值的计算并将其缓存以备以后访问。该值是在第一次调用getter 时计算的，然后被缓存，因此后续访问返回缓存值而不重新计算它。这在以下情况下很有用：

**访问器描述符**

访问器属性的描述符与数据属性的不同，没有 value 和 writable，但是有 get 和 set 函数；

- get 一个没有参数的函数，在读取属性时工作；
- set 带有一个参数的函数，当属性被设置时调用；
- enumerable 与数据属性的相同；
- configurable 与数据属性的相同；

> Note: 一个属性要么是访问器（具有 `get/set` 方法），要么是数据属性（具有 `value`），但不能两者都是；如果试图在同一个描述符中同时提供 `get` 和 `value`，则会出现错误；

访问器的一大用途是，它们允许随时通过使用 getter 和 setter 替换“正常的”数据属性，来控制和调整这些属性的行为；例如为旧代码添加一个 getter 以实现兼容；

```js
let user = {
  _name: "John",
};

Object.defineProperty(user, 'name', {
  get() {
    return this._name;
  },

  set(value) {
    set name(value) {
    if (value.length < 4) {
      alert("Name is too short, need at least 4 characters");
      return;
    }
    this._name = value;
  }
  }
});

for(let key in user) alert(key); // _name

alert(user.name); // John
user.name = ""; // alert ...
```

- [getter MDN 中文参考文档](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Functions/get)

#### 原型继承

在 JavaScript 中，对象有一个特殊的隐藏属性 [[Prototype]]（如规范中所命名的），它要么为 null，要么就是对另一个对象的引用，该对象被称为“原型”；

当从 object 中读取一个缺失的属性时，JavaScript 会自动从原型中获取该属性；在编程中，这被称为“原型继承”（Prototypal inheritance）；

原型链有两个限制：一是引用不能形成闭环，否则 JavaScript 会抛出错误；另一个是忽略对象类型，null 之外的其他类型；

> Note: `__proto__` 是 `[[Prototype]]` 因历史原因保留下来的 `getter / setter`；虽然已经过时，但实际上，包括服务端在内的所有环境都支持它，因此使用它是非常安全的；现代编程语言建议应该使用函数 `Object.getPrototypeOf/Object.setPrototypeOf` 来取代 `__proto__` 去 `get/set` 原型；

原型仅用于读取属性，对于写入/删除操作可以直接在对象上进行；访问器（accessor）属性有点特殊，因为分配（assignment）操作是由 setter 函数处理的；因此，写入此类属性实际上与调用函数相同；

```js
let user = {
  name: "John",
  surname: "Smith",

  set fullName(value) {
    [this.name, this.surname] = value.split(" ");
  },

  get fullName() {
    return `${this.name} ${this.surname}`;
  }
};

let admin = {
  __proto__: user,
  isAdmin: true
};

alert(admin.fullName); // John Smith (*)

// setter triggers!
// admin.fullName = "Alice Cooper"; // (**)
admin.name = "Coley48";

alert(admin.fullName); // Coley48 Cooper，admin 的内容被修改了
alert(user.fullName);  // John Smith，user 的内容被保护了

// for..in
for (const key in admin) {
  console.log(key); // isAdmin, surname, name, fullName

  // 过滤继承属性
  if (Object.hasOwnProperty.call(admin, key)) {
    console.warn(key); // isAdmin, name
  }
}

// Object.keys 只返回自己的 key
alert(Object.keys(admin)); // ['isAdmin', 'name']

delete admin.fullName // 无效，无法通过 admin 删除原型中的属性
delete user.fullName // user，admin 中 fullName 均被删除
```

无论在一个对象还是在原型中获取到的方法；在一个方法调用中，this 始终是点符号 . 前面的对象；因此可以实现不同对象共享方法，并且私有状态；

> Tips: `obj.hasOwnProperty(key)` 方法可以判断 `key` 是否是 `obj` 具有自己的（非继承的）属性；一般使用转发调用方式 `Object.hasOwnProperty.call(obj, key)`；

> Note: `for..in` 循环也会迭代继承的属性；而几乎所有键/值获取方法，例如 `Object.keys` 和 `Object.values` 等，都会忽略继承的属性；它们只会对对象自身进行操作，不考虑继承自原型的属性；

> 在现代引擎中，从性能的角度来看，从对象还是从原型链获取属性都是没区别的；引擎会记住在哪里找到的该属性，并在下一次请求中重用它；并且一旦有内容更改，它们就会自动更新内部缓存；

#### F.prototype

如果 F.prototype 是一个对象或者 null（赋以其他值会被忽略），那么 new 操作符会使用它为新对象设置 [[Prototype]]；

> Note: 如果在创建之后，F.prototype 属性有了变化（F.prototype = <another object>），那么通过 new F 创建的新对象也将随之拥有新的对象作为 [[Prototype]]，但已经存在的对象将保持旧有的值；

每个函数都有 prototype 属性，即使我们没有提供它；默认的 prototype 是一个只有属性 constructor 的对象，属性 constructor 指向函数自身；可以使用 constructor 属性来创建一个新对象，该对象使用与现有对象相同的构造器；

```js
// 默认构造器指向自身
function F() {}

F.prototype // {constructor: ƒ}
F.prototype.constructor === F // true
F.prototype.constructor.name // F

let f = new F();
let ff = new f.constructor();

// 对原型相关操作的结果
function Rabbit() {}
Rabbit.prototype = {
  eats: true,
};

// 引用了上面的 prototype
let rabbit1 = new Rabbit();

Rabbit.prototype = {
  eats: false,
};

// 引用了新定义的的 prototype
let rabbit2 = new Rabbit();
// 然后删除新定义的 prototype 的 eats
delete Rabbit.prototype.eats; 

alert(rabbit1.eats); // 从之前引用的 prototype 取值 true
alert(rabbit2.eats); // 从新的 prototype 取值 undefined
```

> Note: 为了确保正确的 `constructor`，我们可以选择添加/删除属性到默认 `prototype`，而不是将其整个覆盖；

#### 原生的原型

所有的内建对象都遵循相同的模式（pattern）：方法都存储在 prototype 中，同时对象本身只存储数据；

**Object 和 Object.prototype**

Object 是 JavaScript 的一种数据类型，它用于存储各种键值集合和更复杂的实体；Objects 可以通过 Object() 构造函数或者使用对象字面量的方式创建；Object 构造函数为给定的参数创建一个包装类对象（object wrapper），具体有以下情况：

- 如果给定值是 null 或 undefined，将会创建并返回一个空对象；
- 如果传进去的是一个基本类型的值，则会构造其包装类型的对象；
- 如果传进去的是引用类型的值，仍然会返回这个值，经他们复制的变量保有和源对象相同的引用地址；
- 当以非构造函数形式被调用时，Object 的行为等同于 new Object()，返回一个空对象；

> Note: 按照规范，所有的内建原型顶端都是 `Object.prototype`，这就是为什么说“一切都从对象继承而来”；`Object.prototype` 上方的链中没有更多的 `[[Prototype]]`；

```js
"".__proto__.__proto__.__proto__; // String Object null
Object.prototype.__proto__; // null
```

- [Object MDN 中文参考文档](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object)

**基础类型**

如果试图访问基本数据类型的属性，那么临时包装器对象将会通过内建的构造器 String、Number 和 Boolean 被创建，提供给我们操作字符串、数字和布尔值的方法然后消失；

> Note: null 和 undefined 比较特殊，它们没有对象包装器，所以它们没有方法和属性，并且它们也没有相应的原型；

在现代编程中，只有一种情况下允许修改原生原型，那就是 polyfilling；但因为原型是全局的，所以很容易造成冲突；

**借用原型方法**

除了通过复制借用原生原型的方法之外，还可以通过将 `obj.__proto__` 设置为对应内置对象的 prototype，然后可以使用该原型下的所有方法；

```js
let obj = {
  0: "Hello",
  1: "world!",
  length: 2,
};

// 只复制某个方法
obj.join = Array.prototype.join;
// 或者指定原型
obj.__proto__ = Array.prototype;

alert( obj.join(',') ); // Hello,world!

// 包装器 + prototype 原型方法
Function.prototype.defer = function (ms) {
  let context = this;

  function func() {
    console.log(this, arguments); // undefined [1, 2]
    setTimeout(() => {
      context.apply(this, arguments);
    }, ms);
  }

  return func;

  // 或使用箭头函数 
  // return (...args) => {
  //   setTimeout(this.apply(this, args), ms);
  // };
};

function f(a, b) {
  console.log(a + b);
}

f.defer(1000)(1, 2); // 1 秒后显示 3
```

#### 原型方法，没有 `__proto__` 的对象

JavaScript 规范中规定，proto 必须仅在浏览器环境下才能得到支持，因此 `__proto__` 被认为是过时且不推荐使用的（deprecated）；应该使用以下现代方法代替 `__proto__`：

- Object.create(proto, [descriptors]) 利用给定的 proto 作为 [[Prototype]] 和可选的属性描述来创建一个空对象；
- Object.getPrototypeOf(obj) 返回对象 obj 的 [[Prototype]]；
- Object.setPrototypeOf(obj, proto) 将对象 obj 的 [[Prototype]] 设置为 proto；

使用 Object.create 来实现比复制 for..in 循环中的属性更强大的对象克隆方式；以对 obj 进行真正准确地拷贝，包括所有的属性：可枚举和不可枚举的，数据属性和 setters/getters —— 包括所有内容，并带有正确的 [[Prototype]]；

```js
let clone = Object.create(Object.getPrototypeOf(obj), Object.getOwnPropertyDescriptors(obj));
```

> Note: JavaScript 引擎对此进行了高度优化，用 `Object.setPrototypeOf` 或对 `obj.__proto__` 赋值，“即时”更改原型是一个非常缓慢的操作，因为它破坏了对象属性访问操作的内部优化；

**very plain object**

在对象中 `__proto__` 属性很特别：它必须是对象或者 null，其他原始类型的赋值会被忽略；而使用 Object.create(null) 创建了一个空对象，这个对象没有原型（[[Prototype]] 是 null），所以 `__proto__` 没有继承 Object.prototype 的 getter/setter，因此可以被赋以任何原始类型值；这样的对象称为 “very plain” 或 “pure dictionary” 对象；

**其他静态方法**

- Object.keys(obj) / Object.values(obj) / Object.entries(obj) 返回一个可枚举的由自身的字符串属性名/值/键值对组成的数组；
- Object.getOwnPropertySymbols(obj) 返回一个由自身所有的 symbol 类型的键组成的数组；
- Object.getOwnPropertyNames(obj) 返回一个由自身所有的字符串键组成的数组；
- Reflect.ownKeys(obj) 返回一个由自身所有键组成的数组；

```js
function Rabbit(name) {
  this.name = name;
}
Rabbit.prototype.sayHi = function() {
  alert(this.name);
};

let rabbit = new Rabbit("Rabbit");

rabbit.sayHi(); // Rabbit
Rabbit.prototype.sayHi(); // undefined
Object.getPrototypeOf(rabbit).sayHi(); // undefined
rabbit.__proto__.sayHi(); // undefined
```

#### Class 基本语法

在现代 JavaScript 中，还有一个更高级的“类（class）”构造方式；而 new 操作符会自动调用 constructor() 方法，因此我们可以在 constructor() 中初始化对象；

```js
// 类的定义实际上声明了一个函数，然后存储类中的方法
class User {
  constructor(name) {
    this.name = name;
  }

  sayHi() {
    console.log(this.name);
  }

  // getter
  get name() {
    return this._name;
  }

  // setter
  set name(value) {
    if (value.length < 4) {
      alert("Name is too short.");
      return;
    }
    this._name = value;
  }
}

// 用法：
let user = new User("John");
user.sayHi();

// 类其实是一个 constructor 方法
console.log(typeof User); // function
console.log(User.prototype.constructor.name); // User
console.log(Object.getOwnPropertyNames(User.prototype)); // ['constructor', 'sayHi']

// class 类与 function 类的差异
User(); // Cannot call a class as a function
// 打印 User 方法的标识符对象
Object.getOwnPropertyDescriptors(User.prototype);
```

class 不是语法糖，class 与直接使用 function 定义的类有很大差异：

1. 通过 class 创建的函数具有特殊的内部属性标记 [[IsClassConstructor]]: true；
2. 类方法不可枚举，类定义将 "prototype" 中的所有方法的 enumerable 标志设置为 false；
3. 类总是使用 use strict；在类构造中的所有代码都将自动进入严格模式；

<!-- class类是方法，原型是对象；构造器是方法类；原型对象用于存储实例方法和属性，class类用于存储静态方法和属性； -->

**类表达式**

就像函数一样，类可以在另外一个表达式中被定义，被传递，被返回，被赋值等；类似于命名函数表达式（Named Function Expressions），类表达式也可以有一个名字，如果类表达式有名字，那么该名字仅在类内部可见；

同对象字面量，类可能包括 getters/setters，计算属性（computed properties）等；

“类字段”是一种允许添加任何属性的语法，与类方法不同，类字段在每个独立对象中被设好，而不是设在 User.prototype；类字段还可以解决丢失 this 的问题；

```js
class Button {
  constructor(value) {
    this.value = value;
  }
  click = () => {
    alert(this.value);
  }
}

let button = new Button("hello");

setTimeout(button.click, 1000); // hello
```

#### 类继承

类继承是一个类扩展另一个类的一种方式，可以在现有功能之上创建新功能；

在内部，关键字 extends 使用了旧的原型机制进行工作；它将子类的 prototype.[[Prototype]] 设置为父类的 prototype；

> Tips: 类语法不仅允许指定一个类，在 `extends` 后可以指定任意表达式，例如一个生成父类的函数调用；

```js
function f(phrase) {
  return class {
    sayHi() { alert(phrase); }
  };
}

class User extends f("Hello") {}

new User().sayHi(); // Hello
```

在子类中可以重写父类同名方法，然后可以使用 super 关键字调用父类方法；箭头函数没有 super，有则会从外部获取；

根据规范，如果一个类扩展了另一个类并且没有 constructor，那么将生成一个只调用 super 的“空” constructor；继承类的 constructor 必须调用 super(...)，并且 (!) 一定要在使用 this 之前调用；

> 在 JavaScript 中，继承类（所谓的“派生构造器”，英文为 “derived constructor”）的构造函数与其他函数之间是有区别的；派生构造器具有特殊的内部属性 [[ConstructorKind]]:"derived"；当通过 new 执行一个常规函数时，它将创建一个空对象，并将这个空对象赋值给 this；但是当继承的 constructor 执行时，它不会执行此操作，而是期望父类的 constructor 来完成这项工作；

```js
class Animal {
  constructor(name) {
    this.speed = 0;
    this.name = name;
  }
}

class Rabbit extends Animal {

  constructor(name, earLength) {
    // super(name);
    this.speed = 0;
    this.name = name;
    this.earLength = earLength;
  }
}

let rabbit = new Rabbit("White Rabbit", 10); // Error: this is not defined.
```

字段初始化的顺序是基类（还未继承任何东西的那种），在构造函数调用前初始化；派生类，在 super() 后立刻初始化；

父类构造器总是会使用它自己字段的值，而不是被子类重写的那一个类字段；可以通过使用方法或者 getter/setter 替代类字段，来修复这个问题；

JavaScript 为函数添加了一个特殊的内部属性：[[HomeObject]]；当一个函数被定义为类或者对象方法时，它的 [[HomeObject]] 属性就成为了该对象；[[HomeObject]] 不能被更改，所以这个绑定是永久的，[[HomeObject]] 是为类和普通对象中的方法定义的，但是对于对象而言，方法必须确切指定为 method()，而不是 "method: function()"；

```js

let animal = {
  name: "Animal",
  eat() {
    // animal.eat.[[HomeObject]] == animal
    console.warn(`${this.name} eats.`);
  },
  jump: function () {
    console.log(`${this.name} jump.`);
  },
};

let rabbit = {
  __proto__: animal,
  name: "Rabbit",
  eat() {
    // rabbit.eat.[[HomeObject]] == rabbit
    super.eat();
  },
  jump: function () {
    // super.jump(); // 'super' is only allowed in object methods and classes.
  },
};

let longEar = {
  __proto__: rabbit,
  name: "Long Ear",
  eat() {
    // longEar.eat.[[HomeObject]] == longEar
    super.eat();
  },
};

// 正确执行
longEar.eat(); // Long Ear eats.
// rabbit.jump();
```

#### 静态属性和静态方法

可以把一个方法赋值给类的函数本身，而不是赋给它的 "prototype"，这样的方法被称为 静态的（static）；静态方法和属性可以被继承；

```js
class User {
  static staticProperty = 10;

  static staticMethod() {
    alert(this === User);
  }
}
User.staticMethod(); // true

// 结果相同
class User { }
User.staticMethod = function() {
  alert(this === User);
};
User.staticProperty = 10;
User.staticMethod(); // true

// extends 创建两个 [[prototype]]，一个是类本身的，另一个是类原型的
class VipUser extends User {}

console.warn(VipUser.__proto__ === User); // true
console.warn(VipUser.prototype.__proto__ === User.prototype); // true
```

#### 私有的和受保护的属性和方法

在面向对象的编程中，属性和方法分为两组：

- 内部接口：可以通过该类的其他方法访问，但不能从外部访问的方法和属性；
- 外部接口：也可以从类的外部访问的方法和属性；

为了隐藏内部接口，我们使用受保护的或私有的属性；
- 受保护的字段以 _ 开头，这是一个众所周知的约定，不是在语言级别强制执行的；
- 私有字段以 # 开头，JavaScript 确保我们只能从类的内部访问它们；

```js
// 定义私有变量
class User {
  _name = "any";
  #age = 18;
  static #secret = 10;
  static get secret() {
    return this.#secret;
  }
  #whisper() {
    console.log(111);
  }
}
console.log(User.secret); // 10
// console.log(User.#secret); // Property '#secret' is not accessible outside class 'User' because it has a private identifier
```

#### 扩展内建类

通常使用 extends 继承内建类，可以扩展内建类的一些方法，或者可以在内建类的原型对象上添加自定义的字段以实现扩展；

> Note: 内建类相互间不继承静态方法；

#### 类检查：instanceof

instanceof 操作符用于检查一个对象是否属于某个特定的 class，通常，instanceof 在检查中会将原型链考虑在内；

```js
class Rabbit {}
let rabbit = new Rabbit();
console.log( rabbit instanceof Rabbit ); // true

// 这里是构造函数，而不是 class
function Rabbit() {}
console.log( new Rabbit() instanceof Rabbit ); // true

let arr = [1, 2, 3];
arr instanceof Array; // true
arr instanceof Object; // true
({}) instanceof Object;
```

**Symbol.hasInstance**

自定义 instanceof，可以在静态方法 Symbol.hasInstance 中设置自定义逻辑；

```js
// 设置 instanceOf 检查
// 并假设具有 canEat 属性的都是 animal
class Animal {
  static [Symbol.hasInstance](obj) {
    return true
  }
}

let obj = { canEat: true };

alert(obj instanceof Animal); // true：Animal[Symbol.hasInstance](obj) 被调用
```

**Object.prototype.isPrototypeOf()**

另一个方法：objA.isPrototypeOf(objB) 判断 objA 处在 objB 的原型链中，返回布尔值；但如果中途修改了原型，则之前创建的实例无法被正确判断；

```js
let obj = {};
Object.prototype.isPrototypeOf(obj); // true

// 改变原型指向
obj.__proto__ = null;
Object.prototype.isPrototypeOf(obj); // false
```

[Object.prototype.isPrototypeOf() MDN 中文参考文档](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/object/isPrototypeOf)

**Object.prototype.toString()**

使用 Object.prototype.toString 方法来可以揭示类型：按照规范，内建的 toString 方法可以被从对象中提取出来，并在任何其他值的上下文中执行，其结果取决于该值；如果我们想要获取内建对象的类型，并希望把该信息以字符串的形式返回，而不只是检查类型的话，我们可以用 {}.toString.call 替代 instanceof；

```js
// 方便起见，将 toString 方法复制到一个变量中
let objectToString = Object.prototype.toString;

let arr = [];
alert( objectToString.call(arr) ); // [object Array]
```

对象的 toString 方法可以使用特殊的对象属性 Symbol.toStringTag 被自定义；

```js
let user = {
  [Symbol.toStringTag]: "User"
};

alert( {}.toString.call(user) ); // [object User]

// 特定于环境的对象和类的 toStringTag：
alert( window[Symbol.toStringTag]); // Window
alert( XMLHttpRequest.prototype[Symbol.toStringTag] ); // XMLHttpRequest

alert( {}.toString.call(window) ); // [object Window]
alert( {}.toString.call(new XMLHttpRequest()) ); // [object XMLHttpRequest]
```

| 类型检查 | 用于 |	返回值 |
| :----- | :----- | :----- |
| typeof | 原始数据类型 |	string |
| instanceof | 对象 |	true/false |
| {}.toString	| 原始数据类型，内建对象，包含 Symbol.toStringTag 属性的对象 | string |

#### Mixin 模式

Mixin 是一个通用的面向对象编程术语：一个包含可被其他类使用而无需继承的方法的类；Mixin 提供了实现特定行为的方法，但是我们不单独使用它，而是使用它来将这些行为添加到其他类中；

```js
let sayHiBase = {
  say() {
    console.log(1);
  }
}

let sayHiMixin = {
  // 设置原型
  __proto__: sayHiBase,

  say() {
    super.say();
  },
  sayHi() {
    alert(`Hello ${this.name}`);
  },
  sayBye() {
    alert(`Bye ${this.name}`);
  }
};
class Person {}
class User extends Person {
  constructor(name) {
    super();
    this.name = name;
  }
}

// mixin
Object.assign(User.prototype, sayHiMixin);
new User('Coley').sayHi(); // coley
new User('Hush').say(); // 1
```

> Note: 在 `sayHiMixin` 内部对父类方法 `super.say()` 的调用会在 `mixin` 的原型中查找方法，而不是在 class 中查找；因为方法 `say` 最初是在 `sayHiMixin` 中创建的，因此，即使复制了它们，但是 `say` 的 `[[HomeObject]]` 内部属性仍引用的是 sayHiMixin；当 `super` 在 `[[HomeObject]].[[Prototype]]` 中寻找父方法时，意味着它搜索的是 `sayHiMixin.[[Prototype]]`，而不是 `User.[[Prototype]]`；

```js
let eventMixin = {
  /**
   * 订阅事件，用法：
   *  menu.on('select', function(item) { ... }
  */
  on(eventName, handler) {
    if (!this._eventHandlers) this._eventHandlers = {};
    if (!this._eventHandlers[eventName]) {
      this._eventHandlers[eventName] = [];
    }
    this._eventHandlers[eventName].push(handler);
  },

  /**
   * 取消订阅，用法：
   *  menu.off('select', handler)
   */
  off(eventName, handler) {
    let handlers = this._eventHandlers?.[eventName];
    if (!handlers) return;
    for (let i = 0; i < handlers.length; i++) {
      if (handlers[i] === handler) {
        handlers.splice(i--, 1);
      }
    }
  },

  /**
   * 生成具有给定名称和数据的事件
   *  this.trigger('select', data1, data2);
   */
  trigger(eventName, ...args) {
    if (!this._eventHandlers?.[eventName]) {
      return; // 该事件名称没有对应的事件处理程序（handler）
    }

    // 调用事件处理程序（handler）
    this._eventHandlers[eventName].forEach(handler => handler.apply(this, args));
  }
};
```

- [JavaScript 现代教程-Mixin 模式](https://zh.javascript.info/mixins)

#### 错误处理，try...catch

try...catch 仅对运行时的 error 有效；这类错误被称为“运行时的错误（runtime errors）”，有时被称为“异常（exceptions）”；

如果在“计划的（scheduled）”代码中发生异常，例如在 setTimeout 中，则 try...catch 不会捕获到异常：为了捕获到计划的（scheduled）函数中的异常，那么 try...catch 必须在这个函数内；

```js
setTimeout(function() {
  try {
    noSuchVariable; // try...catch 处理 error 了！
  } catch {
    alert( "error is caught here!" );
  }
}, 1000);
```

对于所有内建的 error，error 对象具有两个主要属性和其他非标准的属性：

- name：Error 名称；
- message：关于 error 的详细文字描述；
- stack：当前的调用栈：用于调试目的的一个字符串，其中包含有关导致 error 的嵌套调用序列的信息；

使用 throw <error object> 会生成一个 error 对象；技术上讲，我们可以将任何东西用作 error 对象。甚至可以是一个原始类型数据，例如数字或字符串，但最好使用对象，最好使用具有 name 和 message 属性的对象（某种程度上保持与内建 error 的兼容性

```js
// 完整语法
try {
  //  ... 尝试执行的代码 ...
} catch (err) {
  //  ... 处理 error ...
} finally {
  //  ... 总是会执行的代码 ...
}
```

> Note: `finally` 子句适用于 `try...catch` 的任何出口，这包括显式的 `return`；

规范中没有全局 catch 的相关内容，但是代码的执行环境一般会提供这种机制；Node.JS 有 process.on("uncaughtException")；在浏览器中，可以将一个函数赋值给特殊的 window.onerror 属性，该函数将在发生未捕获的 error 时执行；

```js
window.onerror = function(message, url, line, col, error) {
  // message：Error 信息；
  // url；发生 error 的脚本的 URL；
  // line，col：发生 error 处的代码的行号和列号；
  // error：Error 对象；
};
```

全局错误处理程序 window.onerror 的作用通常不是恢复脚本的执行，如果发生编程错误，那这几乎是不可能的，它的作用是将错误信息发送给开发者；

#### 自定义 Error，扩展 Error

JavaScript 允许将 throw 与任何参数一起使用，所以从技术上讲，我们自定义的 error 不需要从 Error 中继承。但是，如果我们继承，那么就可以使用 obj instanceof Error 来识别 error 对象；

```js
class MyError extends Error {
  constructor(message) {
    super(message);
    this.name = this.constructor.name;
  }
}

class ValidationError extends MyError { }

class PropertyRequiredError extends ValidationError {
  constructor(property) {
    super("No property: " + property);
    this.property = property;
  }
}

// name 是对的
alert( new PropertyRequiredError("field").name ); // PropertyRequiredError
```

包装异常是一项广泛应用的技术：用于处理低级别异常并创建高级别 error 而不是各种低级别 error 的函数，低级别异常有时会成为该对象的属性；

#### 回调简介

异步执行某项功能的函数应该提供一个 callback 参数用于在相应事件完成时调用，这被称为“基于回调”的异步编程风格；

```js
// 动态加载脚本
function loadScript(src, callback) {
  // 创建一个 <script> 标签，并将其附加到页面
  // 这将使得具有给定 src 的脚本开始加载，并在加载完成后运行
  let script = document.createElement('script');
  script.src = src;
  script.onload = () => callback(script);
  script.onerror = () => callback(new Error(`Script load error for ${src}`));
  document.head.append(script);
}
```

#### Promise

传递给 new Promise 的函数被称为 executor，当 new Promise 被创建，executor 会自动运行；它的参数 resolve 和 reject 是由 JavaScript 自身提供的回调；
- resolve(value) 如果任务成功完成并带有结果 value；
- reject(error) 如果出现了 error，error 即为 error 对象；

由 new Promise 构造器返回的 promise 对象具有以下内部属性：
- state 最初是 "pending"，然后在 resolve 被调用时变为 "fulfilled"，或者在 reject 被调用时变为 "rejected"；
- result 最初是 undefined，然后在 resolve(value) 被调用时变为 value，或者在 reject(error) 被调用时变为 error；

Promise 可以通过使用 .then、.catch 和 .finally 方法接收处理结果：
- .then 的第一个参数是一个函数，该函数将在 promise resolved 后运行并接收结果；.then 的第二个参数也是一个函数，该函数将在 promise rejected 后运行并接收 error；
- .catch(f) 调用是 .then(null, f) 的完全的模拟，它只是一个简写形式；如果错误在前面以及被处理，该部分不会执行；
- .finally(f) 处理程序（handler）没有参数，在某种意义上，f 总是在 promise 被 settled 时运行：即 promise 被 resolve 或 reject；finally 处理程序将结果和 error 传递给下一个处理程序；

```js
new Promise(function (resolve, reject) {
        // executor
        setTimeout(() => {
            resolve("Done.");
            reject(new Error("Whoops!"));
        }, 1000);
    }).then(
          (result) => {
              console.log(result); // Done.
              throw new Error("Unexpected error.");
          },
          (error) => {
              console.warn(error); // Whoops!
          }
      ).catch((err) => {
          // 可以接收之前 .then 中的错误
          console.warn(err); // Unexpected error.
      }).finally(() => {
          console.log("Finished.");
      });
```

> Note: `executor` 只能调用一个 `resolve` 或一个 `reject`，任何状态的更改都是最终的；所有其他的再对 `resolve` 和 `reject` 的调用都会被忽略；此外，直接受一个参数，多余的参数也会被忽略；

> Note: 通常以 `Error` 对象 `reject`；实际上，`executor` 中还可以立即调用 `resolve` 或 `reject`；

> Tips: 如果 `promise` 为 `pending` 状态，`.then/catch/finally` 处理程序（handler）将等待它。否则，如果 `promise` 已经是 `settled` 状态，它们就会立即运行；

```js
function delay(ms) {
  return new Promise(resolve => setTimeout(resolve, ms));
}
delay(3000).then(() => alert('runs after 3 seconds'));
```

#### Promise 链

每个对 .then 的调用都会返回了一个新的 promise，因此我们可以在其之上调用下一个 .then；形成一个 .then 处理程序（handler）链，也即 Promise 链；

同时 .then(handler) 中所使用的处理程序（handler）可以创建并返回一个 promise，其后的处理程序（handler）将等待它 settled 后再获得其结果（result）；

```js
new Promise((resolve, reject) => {
  setTimeout(() => resolve(1), 1000);
}).then((result) => {
  alert(result); // 1
  return new Promise((resolve, reject) => { // (*)
    setTimeout(() => resolve(result * 2), 1000);
  });
}).then((result) => {
  alert(result); // 2
  return result * 2;
});
```

> Note: 确切地说，处理程序（handler）返回的不完全是一个 `promise`，而是返回的被称为 `thenable` 对象 — 一个具有方法 `.then` 的任意对象，它会被当做一个 `promise` 来对待；

```js
class Thenable {
  constructor(num) {
    this.num = num;
  }
  // 实现 .then 方法
  then(resolve, reject) {
    alert(resolve); // function() { native code }
    // 1 秒后使用 this.num*2 进行 resolve
    setTimeout(() => resolve(this.num * 2), 1000); // (**)
  }
}
```

#### 使用 promise 进行错误处理

Promise 的执行者（executor）和 promise 的处理程序（handler）周围有一个“隐式的 try..catch”；如果发生异常，就会被捕获，并被视为 rejection 进行处理；控制权移交至最近的 error 处理程序（handler）；

在浏览器中，我们可以使用 unhandledrejection 事件来捕获这类 error；这个事件是 HTML 标准 的一部分；

```js
window.addEventListener('unhandledrejection', function(event) {
  // 这个事件对象有两个特殊的属性：
  alert(event.promise); // [object Promise] - 生成该全局 error 的 promise
  alert(event.reason); // Error: Whoops! - 未处理的 error 对象
});

new Promise(function() {
  throw new Error("Whoops!");
}); // 没有用来处理 error 的 catch
```

有一个浏览器技巧，是从 finally 返回零延时（zero-timeout）的 promise；这是因为一些浏览器（比如 Chrome）需要“一点时间”的 promise 处理程序来绘制文档的更改；因此它确保在进入链下一步之前，指示在视觉上是停止的；

函数代码周围有个“隐式的 try..catch”，所以，所有同步错误都会得到处理，但是无法捕获异步的错误；try...catch 是同步工作的；

```js
new Promise(function (resolve, reject) {
    // throw new Error("Sync error.") // 可以捕获
    setTimeout(() => {
        // reject(new Error("No")); // 可以处理
        // throw new Error("Async error."); // 无法捕获
    }, 2000);
}).catch(alert);
```

#### Promise API

在 Promise 类中，有 6 种静态方法；

**Promise.all**

并行执行多个 promise，并等待所有 promise 都准备就绪；Promise.all 接受一个 promise 数组作为参数（从技术上讲，它可以是任何可迭代对象，但通常是一个数组）并返回一个新的 promise；

结果数组中元素的顺序与其在源 promise 中的顺序相同，即使第一个 promise 花费了最长的时间才 resolve，但它仍是结果数组中的第一个；

```js
Promise.all([
    new Promise((resolve) => setTimeout(() => resolve(1), 3000)), // 1
    new Promise((resolve) => setTimeout(() => resolve(2), 2000)), // 2
    new Promise((resolve) => setTimeout(() => resolve(3), 1000)) // 3
]).then((value) => {
    console.log(value);
}); // 1,2,3 当上面这些 promise 准备好时：每个 promise 都贡献了数组中的一个元素
```

> Tips: 一个常见的技巧是，将一个任务数据数组映射（map）到一个 `promise` 数组，然后将其包装到 `Promise.all`；

如果任意一个 promise 被 reject，由 Promise.all 返回的 promise 就会立即 reject，并且带有的就是这个 error；Promise.all 也会立即被 reject，完全忽略列表中其他的 promise，它们的结果也被忽略；

Promise.all(iterable) 允许在 iterable 中使用 non-promise 的“常规”值；常规值将被“按原样”传递给结果数组；

**Promise.allSettled**

Promise.allSettled 等待所有的 promise 都被 settle，无论结果如何；结果数组有两种数据：

- {status:"fulfilled", value:result} 对于成功的响应；
- {status:"rejected", reason:error} 对于 error；

**Promise.race**

与 Promise.all 类似，但只等待第一个 settled 的 promise 并获取其结果（或 error）；第一个 settled 的 promise “赢得了比赛”之后，所有进一步的 result/error 都会被忽略；

**Promise.any**

与 Promise.race 类似，区别在于 Promise.any 只等待第一个 fulfilled 的 promise，并将这个 fulfilled 的 promise 返回；如果给出的 promise 都 rejected，那么则返回 rejected 的 promise 和 AggregateError（一个特殊的 error 对象），在其 errors 属性中存储着所有 promise error；

**Promise.resolve**

Promise.resolve(value) 用结果 value 创建一个 resolved 的 promise；当可以直接从缓存中获取了当前操作的结果 value，但是期望返回的是一个 promise 时，可以使用 Promise.resolve(value) 将 value “封装”进 promise，以满足期望返回一个 promise 的这个需求；

**Promise.reject**

与 Promise.resolve 类似 Promise.reject(error) 用 error 创建一个 rejected 的 promise；

#### Promisification

promisify(f)：接受一个需要被 promisify 的函数 f，并返回一个包装（wrapper）函数；

```js
function promisify(f) {
  return function (...args) { // 返回一个包装函数（wrapper-function） (*)
    return new Promise((resolve, reject) => {
      function callback(err, result) { // 我们对 f 的自定义的回调 (**)
        if (err) {
          reject(err);
        } else {
          resolve(result);
        }
      }

      args.push(callback); // 将我们的自定义的回调附加到 f 参数（arguments）的末尾

      f.call(this, ...args); // 调用原始的函数
    });
  };
}

// 用法：
let loadScriptPromise = promisify(loadScript);
loadScriptPromise(...).then(...);
```

#### 微任务（Microtask）

Promise 的处理程序（handlers）.then、.catch 和 .finally 都是异步的；而异步任务需要适当的管理，为此，ECMA 标准规定了一个内部队列 PromiseJobs，通常被称为“微任务队列（microtask queue）”（V8 术语）；

如规范中所述：队列（queue）是先进先出的：首先进入队列的任务会首先运行；同时，只有在 JavaScript 引擎中没有其它任务在运行时，才开始执行任务队列中的任务；

如果一个 promise 的 error 未被在微任务队列的末尾进行处理，则会出现“未处理的 rejection”；

```js
let promise = Promise.reject(new Error("Promise Failed!"));
setTimeout(() => promise.catch(err => alert('caught')), 1000);

// Error: Promise Failed!
window.addEventListener('unhandledrejection', event => alert(event.reason));
```

#### async/await

async 函数总是返回一个 promise，其他值将自动被包装在一个 resolved 的 promise 中；

```js
async function f() {
  return 1;
}

f().then(alert); // 1
```

await 的关键词，只在 async 函数内工作；await 让 JavaScript 引擎等待直到 promise 完成（settle）并返回结果；

await 实际上会暂停函数的执行，直到 promise 状态变为 settled，然后以 promise 的结果继续执行；这个行为不会耗费任何 CPU 资源，因为 JavaScript 引擎可以同时处理其他任务：执行其他脚本，处理事件等；

> Tips: 在现代浏览器中，当我们处于一个 `module` 中时，那么在顶层使用 `await` 也是被允许的；实测只在 `<script type="module">` 标签中可用；或者可以使用立即执行函数；

```js
(async () => {
  let response = await fetch('/article/promise-chaining/user.json');
  let user = await response.json();
  ...
})();
```

await 允许使用 thenable 对象；如果 await 接收了一个非 promise 的 Thenable 对象，它就会调用这个 .then 方法，并将内建的函数 resolve 和 reject 作为参数传入（就像它对待一个常规的 Promise executor 时一样）；然后 await 等待直到这两个函数中的某个被调用，然后使用得到的结果继续执行后续任务；

```js
class Thenable {
  constructor(num) {
    this.num = num;
  }
  then(resolve, reject) {
    alert(resolve);
    // 1000ms 后使用 this.num*2 进行 resolve
    setTimeout(() => resolve(this.num * 2), 1000); // (*)
  }
}

async function f() {
  // 等待 1 秒，之后 result 变为 2
  let result = await new Thenable(1);
  alert(result);
}

f();
```

如果一个 promise 正常 resolve，await promise 返回的就是其结果。但是如果 promise 被 reject，它将 throw 这个 error；这时可以在 async 函数中用 try..catch 来捕获上面提到的那个 error，与常规的 throw 使用的是一样的方式；

```js
async function f() {
  await Promise.reject(new Error("Whoops!"));
  // 等价于 throw new Error("Whoops!");

  try {
    let response = await fetch('http://no-such-url');
  } catch(err) {
    alert(err); // TypeError: failed to fetch
  }
}
```

> Tips: `async/await` 可以和 `Promise.all` 一起使用；

#### Generator

 Generator 可以按需一个接一个地返回（“yield”）多个值；它们可与 iterable 完美配合使用，从而可以轻松地创建数据流；在此类函数被调用时，它不会运行其代码，而是返回一个被称为 “generator object” 的特殊对象，来管理执行流程；

 ```js
function* generateSequence() {
  yield 1;
  yield 2;
  return 3;
}

let generator = generateSequence();
console.log(generator); // Generator {_invoke: ƒ}

let one = generator.next();
let two = generator.next();
let three = generator.next();
console.log(one, two, three);
// {value: 1, done: false} {value: 2, done: false} {value: 3, done: true}
 ```

 一个 generator 的主要方法就是 next()，当 next 被调用时，它会恢复上图所示的运行，执行直到最近的 yield <value> 语句（value 可以被省略，默认为 undefined），然后函数执行暂停，并将产出的（yielded）值返回到外部代码；

Generator 是可迭代的，但当 done: true 时，for..of 循环会忽略最后一个 value；因此，如果我们想要通过 for..of 循环显示所有的结果，必须使用 yield 返回它们；

```js
function* generateSequence() {
  yield 1;
  yield 2;
  yield 3;
}

let generator = generateSequence();

for(let value of generator) {
  alert(value); // 1，然后是 2，然后是 3
}

let sequence = [0, ...generateSequence()];
alert(sequence); // 0, 1, 2, 3
```

可以通过提供一个 generator 函数作为 Symbol.iterator，来使用 generator 进行迭代；

```js
let range = {
  from: 1,
  to: 5,

  // [Symbol.iterator]: function*() 的简写形式
  *[Symbol.iterator]() {
    for(let value = this.from; value <= this.to; value++) {
      yield value;
    }
  }
};

alert( [...range] ); // 1,2,3,4,5
```

对于 generator 而言，我们可以使用 yield* 这个特殊的语法来将一个 generator “嵌入”（组合）到另一个 generator 中；yield* 指令将执行委托给另一个 generator，这个术语意味着 yield* gen 在 generator gen 上进行迭代，并将其产出（yield）的值透明地（transparently）转发到外部；执行结果与我们内联嵌套 generator 中的代码获得的结果相同；

```js
function* generateSequence(start, end) {
  for (let i = start; i <= end; i++) yield i;
}

function* generatePasswordCodes() {

  // 0..9
  yield* generateSequence(48, 57);

  // A..Z
  yield* generateSequence(65, 90);

  // a..z
  yield* generateSequence(97, 122);

}

let str = '';

for(let code of generatePasswordCodes()) {
  str += String.fromCharCode(code);
}

alert(str); // 0..9A..Za..z
```

与常规函数不同，generator 和调用 generator 的代码可以通过在 next/yield 中传递值来交换结果；yield 不仅可以向外返回结果，而且还可以将外部的值传递到 generator 内；调用 generator.next(arg)，就能将参数 arg 传递到 generator 内部，这个 arg 参数会变成 yield 的结果；

此外，调用 generator.throw(err) 向 yield 传递一个 error，在这种情况下，err 将被抛到对应的 yield 所在的那一行；如果我们没有在那里捕获这个 error，那么，通常，它会掉入外部调用代码（如果有），如果在外部也没有被捕获，则会杀死脚本；

还可以通过 generator.return(value) 完成 generator 的执行并返回给定的 value；如果我们在已完成的 generator 上再次使用 generator.return()，它将再次返回该值；通常只用于在特定条件下停止 generator；

```js
function* gen() {
  // 向外部代码传递一个问题并等待答案
  let result = yield "2 + 2 = ?"; // (*)

  alert(result);
}

let generator = gen();

let question = generator.next().value; // <-- yield 返回的 value


// 将结果传递到 generator 中
generator.next(4);
// 抛出错误
generator.throw(new Error("The answer is not found in my database"));
// 主动返回
g.return('foo'); // { value: "foo", done: true }
```

#### 异步迭代和 generator

异步迭代允许我们对按需通过异步请求而得到的数据进行迭代：

- 使用 Symbol.asyncIterator 取代 Symbol.iterator；next() 方法；
- 应该返回一个 promise（带有下一个值，并且状态为 fulfilled），关键字 async 也可以实现这一点，我们可以简单地使用 async next()；
- 我们应该使用 for await (let item of iterable) 循环来迭代这样的对象；

> Note: 需要常规的同步 `iterator` 的功能，无法与异步 `iterator` 一起使用，因为它期望找到 `Symbol.iterator`，而不是 `Symbol.asyncIterator`；因此 `Spread` 语法 `...` 和 `for..of` 无法异步工作；

```js
// 异步 generator
async function* generateSequence(start, end) {
  for (let i = start; i <= end; i++) {
    // 可以使用 await 了！
    await new Promise(resolve => setTimeout(resolve, 1000));
    yield i;
  }
}

(async () => {
  let generator = generateSequence(1, 5);
  for await (let value of generator) {
    alert(value); // 1，然后 2，然后 3，然后 4，然后 5（在每个 alert 之间有延迟）
  }
})();
```

常规的 generator 可用作 Symbol.iterator 以使迭代代码更短，异步 generator 可用作 Symbol.asyncIterator 来实现异步迭代；

```js
let range = {
  from: 1,
  to: 5,
  // 这一行等价于 [Symbol.asyncIterator]: async function*() {
  async *[Symbol.asyncIterator]() {
    for(let value = this.from; value <= this.to; value++) {
      // 在 value 之间暂停一会儿，等待一些东西
      await new Promise(resolve => setTimeout(resolve, 1000));
      yield value;
    }
  }
};

(async () => {
  for await (let value of range) {
    alert(value); // 1，然后 2，然后 3，然后 4，然后 5
  }
})();
```

#### 模块 Module

简单讲，一个模块（module）就是一个文件，一个脚本就是一个模块；一个模块可以包含用于特定目的的类或函数库；

- AMD 最古老的模块系统之一，最初由 require.js 库实现；
- CommonJS 为 Node.js 服务器创建的模块系统；
- UMD 另外一个模块系统，建议作为通用的模块系统，它与 AMD 和 CommonJS 都兼容；

语言级的模块系统在 2015 年的时候出现在了标准（ES6）中，此后逐渐发展，现在已经得到了所有主流浏览器和 Node.js 的支持；

模块可以相互加载，并可以使用特殊的指令 export 和 import 来交换功能，从另一个模块调用一个模块的函数；

- export 关键字标记了可以从当前模块外部访问的变量和函数；
- import 关键字允许从其他模块导入功能；

> Note: 由于模块支持特殊的关键字和功能，因此我们必须通过使用 `<script type="module">` 特性（attribute）来告诉浏览器，此脚本应该被当作模块（module）来对待；同时模块只通过 `HTTP(s)` 工作，而非本地；

对浏览器和服务端的 JavaScript 来说都有效的模块**核心功能**：

- 模块始终在严格模式下运行，始终使用 "use strict"；
- 每个模块都有自己的顶级作用域（top-level scope）；一个模块中的顶级作用域变量和函数在其他脚本中是不可见的；模块应该 export 想要被外部访问的内容，并 import 所需要的内容；
- 模块代码仅在第一次导入时被解析，如果同一个模块被导入到多个其他位置，那么它的代码只会执行一次，即在第一次被导入时，然后将其导出（export）的内容提供给进一步的导入（importer）；
- import.meta 对象包含关于当前模块的信息，其内容取决于其所在的环境；在浏览器环境中，它包含当前脚本的 URL，或者如果它是在 HTML 中的话，则包含当前页面的 URL；
- 在一个模块中，顶级 this 是 undefined；非模块脚本的顶级 this 是全局对象；

> Note: 在浏览器中，可以通过将变量显式地分配给 `window` 的一个属性，使其成为窗口级别的全局变量，无论脚本是否带有 `type="module"`；但对于模块，应该使用导入/导出而不是依赖全局变量；

> Note: 顶层模块代码应该用于初始化，创建模块特定的内部数据结构；如果需要多次调用某些东西，则应该将其以函数的形式导出；

只对浏览器中拥有 type="module" 标识脚本有效的**特定功能**：

- 模块脚本总是被延迟的，与 defer 特性对外部脚本和内联脚本的影响相同；
- 对于非模块脚本，async 特性（attribute）仅适用于外部脚本；异步脚本会在准备好后立即运行，独立于其他脚本或 HTML 文档；对于模块脚本，它也适用于内联脚本；
- 具有 type="module" 的外部脚本（external script），相同 src 的外部脚本仅运行一次；且从另一个源获取的脚本，远程服务器必须提供表示允许获取的 header Access-Control-Allow-Origin；
- 在浏览器中，import 必须给出相对或绝对的 URL 路径，import 中不允许没有任何路径的模块，这种模块被称为“裸（bare）”模块；
- 旧时的浏览器不理解 type="module"，未知类型的脚本会被忽略；

> Note: 下载外部模块脚本 `<script type="module" src="...">` 不会阻塞 `HTML` 的处理，它们会与其他资源并行加载；模块脚本会等到 `HTML` 文档完全准备就绪（即使它们很小并且比 `HTML` 加载速度更快），然后才会运行；

> Note: 使用 nomodule 特性来提供一个后备：`<script nomodule>...</script>`；

- [Cannot use import statement outside a module](https://www.jianshu.com/p/60a8a74f5eee)
- [CommonJS和ES6模块的区别](https://juejin.cn/post/6844904067651600391)

#### 导出和导入

通常要明确列出需要导入的内容：

- 打包工具中优化器（optimizer）就会从打包好的代码中删除那些未被使用的函数，从而使构建更小，这就是所谓的摇树（tree-shaking）；
- 导入的显式列表可以更好地概述代码结构：使用的内容和位置；使得代码支持重构，并且重构起来更容易；
- 明确列出要导入的内容会使得名称较短；

模块提供了一个特殊的默认导出 export default 语法，以使“一个模块只做一件事”的方式看起来更好；每个文件最多只能有一个默认的导出，因此导出的实体可能没有名称；

| 命名的导出 | 默认的导出 |
| :----- | :----- |
| export class User {...} |	export default class User {...} |
| import {User} from ... | import User from ... |

从技术上讲，我们可以在一个模块中同时有默认的导出和命名的导出，但是通常不会混合使用它们，模块要么是命名的导出要么是默认的导出；

在某些情况下，default 关键词被用于引用默认的导出；默认引入可以使用不同的名称来导入相同的内容；

“重新导出（Re-export）”语法 export ... from ... 允许导入内容，并立即将其导出；export ... from 与 import/export 相比的显著区别是重新导出的模块在当前文件中不可用；

> Note: 重新导出时，默认导出需要单独处理；

> Note: 在代码块 `{...}` 中的 `import/export` 语句无效；

导出方式：

```js
// utils.js
// 导出数组
export let months = ['Jan', 'Feb', 'Mar','Apr', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'];

// 导出 const 声明的变量
export const MODULES_YEAR = 2015;

// 导出类
export class User {
  constructor(name) {
    this.name = name;
  }
}

// 或者统一导出
export { months, MODULES_YEAR, User }; // 导出变量列表

// 导出为其他名字
export { sayHi as hi, sayBye as bye };

// 默认导出
export default sayHi;
export { sayHi as default };

// 重新导出
export { sayHi } from './say.js'; // 重新导出 sayHi
export { default as User } from './user.js'; // 重新导出 default
// 效果同，区别是在当前模块下重新导出无法使用
import User from './user.js';
export { User as default };

// 重新导出含有默认导出的模块
export * from './user.js'; // 重新导出命名的导出
export { default } from './user.js'; // 重新导出默认的导出
```

导入方式：

```js
// index.js
import { months, User } from './utils.js'

// 将所有内容导入为一个对象
import * as say from './utils.js';

// 用 as 让导入具有不同的名字
import { sayHi as hi, sayBye as bye } from './say.js';

// 默认导入
import { default as User, sayHi } from './user.js';

// 有默认导入时导入为对象
import * as user from './user.js';
let User = user.default; // 默认的导出

// 导入模块（其代码，并运行），但不要将其任何导出赋值给变量
import "module";
```

**动态导入**

import(module) 表达式加载模块并返回一个 promise，该 promise resolve 为一个包含其所有导出的模块对象，可以在代码中的任意位置调用这个表达式；

```js
import(modulePath)
  .then(obj => {})
  .catch(err => err);

// 使用 await
let module = await import(modulePath);

// 含默认导出
let obj = await import('./say.js');
let say = obj.default;
```

> Note: 动态导入在常规脚本中工作时，它们不需要 script type="module"；import 不是一个函数，只是一种特殊语法，恰好使用了括号；

#### Proxy 和 Reflect

一个 Proxy 对象包装另一个对象并拦截诸如读取/写入属性和其他操作，可以选择自行处理它们，或者透明地允许该对象处理它们；

```js
let proxy = new Proxy(target, handler);
```

- target 是要包装的对象，可以是任何东西，包括函数；
- handler 代理配置：带有“捕捉器”（“traps”，即拦截操作的方法）的对象；

对 proxy 进行操作，如果在 handler 中存在相应的捕捉器，则它将运行，并且 Proxy 有机会对其进行处理，否则将直接对 target 进行处理；Proxy 是一种特殊的“奇异对象（exotic object）”，它没有自己的属性；如果 handler 为空，则透明地将操作转发给 target；

对于对象的大多数操作，JavaScript 规范中有一个所谓的“内部方法”，它描述了最底层的工作方式，对于每个内部方法，此表中都有一个捕捉器，Proxy 捕捉器会拦截这些方法的调用；

| 内部方法 |	Handler 方法 |	何时触发 |
| :----- | :----- | :----- |
| [[Get]] | get	| 读取属性 |
| [[Set]] | set	| 写入属性 |
| [[HasProperty]] | has |	in 操作符 |
| [[Delete]] | deleteProperty |	delete 操作符 |
| [[Call]] | apply | 函数调用 |
| [[Construct]] | construct |	new 操作符|
| [[GetPrototypeOf]] | getPrototypeOf |	Object.getPrototypeOf |
| [[SetPrototypeOf]] | setPrototypeOf	| Object.setPrototypeOf |
| [[IsExtensible]] | isExtensible |	Object.isExtensible |
| [[PreventExtensions]] | preventExtensions |	Object.preventExtensions |
| [[DefineOwnProperty]] | defineProperty | Object.defineProperty, Object.defineProperties |
| [[GetOwnProperty]] | getOwnPropertyDescriptor |	Object.getOwnPropertyDescriptor, for..in, Object.keys/values/entries |
| [[OwnPropertyKeys]] | ownKeys |	Object.getOwnPropertyNames, Object.getOwnPropertySymbols, for..in, Object.keys/values/entries |

**get 捕获器**

要拦截读取操作，可以添加 get(target, property, receiver) 捕获器；

- target 是目标对象，该对象被作为第一个参数传递给 new Proxy；
- property 目标属性名；
- receiver 如果目标属性是一个 getter 访问器属性，则 receiver 就是本次读取属性所在的 this 对象，通常是 proxy 对象本身；

```js
let numbers = [0, 1, 2];

numbers = new Proxy(numbers, {
  get(target, prop) {
    if (prop in target) {
      return target[prop];
    } else {
      return 0; // 默认值
    }
  }
});

alert( numbers[1] ); // 1
alert( numbers[123] ); // 0（没有这个数组项）
```

> Note: 代理应该在所有地方都完全替代目标对象，目标对象被代理后，任何人都不应该再引用目标对象；

**set 捕获器**

set(target, property, value, receiver)，当写入属性时 set 捕捉器被触发，其中 value 为目标属性的值；如果写入操作（setting）成功，set 捕捉器应该返回 true，否则返回 false（触发 TypeError）；

```js
let onlyNumbers = [];

onlyNumbers = new Proxy(onlyNumbers, {
    set(target, prop, val) {
        // 拦截写入属性操作
        if (typeof val == "number") {
            target[prop] = val;
            return true;
        } else {
            return false;
        }
    }
});

onlyNumbers.push(1); // 添加成功
onlyNumbers.push(2); // 添加成功
console.log("Length is: " + onlyNumbers.length); // 2

// TypeError: 'set' on proxy: trap returned falsish for property '2
onlyNumbers.push("test");
```

**ownKeys 和 getOwnPropertyDescriptor 捕获器**

Object.keys，for..in 循环和大多数其他遍历对象属性的方法都使用内部方法 [[OwnPropertyKeys]]（由 ownKeys 捕捉器拦截) 来获取属性列表；

- Object.getOwnPropertyNames(obj) 返回非 Symbol 键；
- Object.getOwnPropertySymbols(obj) 返回 Symbol 键；
- Object.keys/values() 返回带有 enumerable 标志的非 Symbol 键/值；
- for..in 循环遍历所有带有 enumerable 标志的非 Symbol 键，以及原型对象的键；

```js
let user = { name: "Coley", age: 18 };

user = new Proxy(user, {
    // 使用 ownKeys 捕捉器拦截 for..in 对 user 的遍历
    // 一旦要获取属性列表就会被调用，可以返回不相干的属性
    ownKeys(target) {
        // return Object.keys(target).filter(key => !key.startsWith('_'));
        return ["a", "b", "c"];
        // 返回没有的属性，其描述符为空，没有 enumerable 标志，会被略过，需要 getOwnPropertyDescriptor 返回 enumerable 为 true；
    },

    getOwnPropertyDescriptor(target, prop) {
        // 被每个属性调用
        return {
            enumerable: true, // false 屏蔽，使属性不可迭代
            configurable: true
            /* ...其他标志，可能是 "value:..." */
        };
    }
});

console.log(Object.keys(user)); // a, b, c
```

**deleteProperty 捕获器**

```js
let user = {
    _password: "***",
    checkPassword(value) {
        //对象方法必须能读取 _password
        return value === this._password;
    }
};

user = new Proxy(user, {
    get(target, prop) {
        if (prop.startsWith("_")) {
            throw new Error("Access denied");
        }
        let value = target[prop];
        // 内部方法可以读取内部属性
        return typeof value === "function" ? value.bind(target) : value;
    },
    deleteProperty(target, prop) {
        // 拦截属性删除
        if (prop.startsWith("_")) {
            throw new Error("Access denied");
        } else {
            delete target[prop];
            return true;
        }
    },
    // ...
});

// "get" 不允许读取 _password
try {
    console.log(user._password); // Error: Access denied
} catch (e) {
    console.log(e.message);
}

// "deleteProperty" 不允许删除 _password
try {
    delete user._password; // Error: Access denied
} catch (e) {
    console.log(e.message);
}
```

**has 捕获器**

has 捕捉器会拦截 in 调用，因此可以实现 in 操作符来检查一个数字是否在 range 范围内；

```js
let range = {
    start: 1,
    end: 10
};

range = new Proxy(range, {
    has(target, prop) {
        return prop >= target.start && prop <= target.end;
    }
});

console.log(5 in range); // true
console.log(50 in range); // false
```

**apply 捕获器**

apply(target, thisArg, args) 捕捉器能使代理以函数的方式被调用；

- target 是目标对象（函数也是对象）；
- thisArg 是 this 的值；
- args 是参数列表；

普通的包装函数不会转发属性读取/写入操作或者任何其他操作；进行包装后，就失去了对原始函数属性的访问，例如 name，length 和其他属性；使用 Proxy 可以将所有操作都能被转发到原始函数；

```js
function delay(f, ms) {
    return new Proxy(f, {
        apply(target, thisArg, args) {
            setTimeout(() => target.apply(thisArg, args), ms);
        }
    });
}

function sayHi(user) {
    console.log(`Hello, ${user}!`);
}

sayHi = delay(sayHi, 3000);

console.log(sayHi.length); // 1 (*) proxy 将“获取 length”的操作转发给目标对象

sayHi("John"); // Hello, John!（3 秒后）
```

Reflect 是一个内建对象，可简化 Proxy 的创建；Reflect 对象使调用这些内部方法成为了可能，它的方法是内部方法的最小包装；

尤其是，Reflect 允许我们将操作符（new，delete，……）作为函数（Reflect.construct，Reflect.deleteProperty，……）执行调用；

此外，对于每个可被 Proxy 捕获的内部方法，在 Reflect 中都有一个对应的方法，其名称和参数与 Proxy 捕捉器相同；所以，如果一个捕捉器想要将调用转发给对象，则只需使用相同的参数调用 `Reflect.<method>` 就足够了；

```js
let user = { age: 0 };

Reflect.set(user, "name", "John");
console.log(user.name); // John

Reflect.deleteProperty(user, "no-such-prop");
Reflect.deleteProperty(user, "name");
console.log(user); // {}

user = new Proxy(user, {
    get(target, prop, receiver) {
        console.warn(`GET ${prop}`);
        return Reflect.get(target, prop, receiver); // (1)
    },
    set(target, prop, val, receiver) {
        console.warn(`SET ${prop}=${val}`);
        return Reflect.set(target, prop, val, receiver); // (2)
    }
});

let name = user.name; // 显示 "GET name"
user.name = "Pete"; // 显示 "SET name=Pete"
```

当存在原型继承时，特别是访问继承对象的 get 访问器属性时，需要用到 receiver 参数，保证将正确的 this 传递给 getter；

```js
let user = {
  _name: "Guest",
  get name() {
    return this._name;
  }
};

let userProxy = new Proxy(user, {
  get(target, prop, receiver) { // receiver = admin
    return Reflect.get(target, prop, receiver); // (*)
    // 或者 return Reflect.get(...arguments);
  }
});

let admin = {
  __proto__: userProxy,
  _name: "Admin"
};

alert(admin.name); // Admin
```

Proxy 存在一些局限性：
- 内建对象具有“内部插槽”，对这些对象的访问无法被代理；
- 私有类字段也是如此，因为它们也是在内部使用插槽实现的；因此，代理方法的调用必须具有目标对象作为 this 才能访问它们；
- 对象的严格相等性检查 === 无法被拦截；
- 性能：基准测试（benchmark）取决于引擎，但通常使用最简单的代理访问属性所需的时间也要长几倍；


```js
// 使用了“内部插槽”存储数据，而不通过 [[Get]]/[[Set]] 内部方法
let map = new Map();

let proxy = new Proxy(map, {
  get(target, prop, receiver) {
    let value = Reflect.get(...arguments);
    // 会将将原始对象暴露给该方法，可能使其进一步传递并破坏其他代理功能；
    return typeof value == 'function' ? value.bind(target) : value;
  }
});

proxy.set('test', 1);
alert(proxy.get('test')); // 1（工作了！）
```

> Note: 出于历史原因，内建 `Array` 没有使用内部插槽，所以，代理数组时没有这种问题；

一个可撤销的代理是可以被禁用的代理，`let {proxy, revoke} = Proxy.revocable(target, handler)`；该调用返回一个带有 proxy 和 revoke 函数的对象以将其禁用；这样的代理会将操作转发给对象，并且我们可以随时将其禁用；

```js
let object = {
  data: "Valuable data"
};

let {proxy, revoke} = Proxy.revocable(object, {});

// 将 proxy 传递到其他某处，而不是对象...
alert(proxy.data); // Valuable data

// 稍后，在我们的代码中
revoke();

// proxy 不再工作（revoked）
alert(proxy.data); // Error
```

```js
let arr = [..."abcde"];

arr = new Proxy(arr, {
    get(target, prop, receiver) {
        if (prop < 0) {
            prop = +prop + target.length;
        }
        return Reflect.get(target, prop, receiver);
    }
});
```

- [Proxy MDN 中文参考文档](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Proxy)
- [JavaScript 现代教程-Proxy 和 Reflect](https://zh.javascript.info/proxy)

#### eval

调用 eval(code) 会运行代码字符串，并返回最后一条语句的结果；

严格模式下，eval 有属于自己的词法环境，因此我们不能从外部访问在 eval 中声明的函数和变量；

```js
// script 中
console.log(eval("this")); // window
eval("console.log(this); let x = 6"); // window

let obj = {
    func() {
        console.log(eval("this")); // obj
        eval("console.log(this); let x = 8;"); // obj

        console.log(window.eval("this")); // window
        window.eval("console.log(this); let x = 10;"); // window

        console.log(x); // ReferenceError: x is not defined
    }
}

obj.func();
console.log(x); // ReferenceError: x is not defined
```

不建议使用 eval：

- 如果 eval 中的代码没有使用外部变量，请以 window.eval(...) 的形式调用 eval；
- 如果 eval 中的代码需要访问局部变量，我们可以使用 new Function 替代 eval，并将它们作为参数传递；

```js
let f = new Function('a', 'alert(a)');

f(5); // 5
```

#### 柯里化（Currying）

柯里化（Currying）是一种关于函数的高阶技术，柯里化是一种函数的转换，它是指将一个函数从可调用的 f(a, b, c) 转换为可调用的 f(a)(b)(c)；柯里化不会调用函数，它只是对函数进行转换；

柯里化更高级的实现，如 lodash 库的 [_.curry](https://lodash.com/docs#curry)，会返回一个包装器，该包装器允许函数被正常调用或者以偏函数（partial）的方式调用；

```js
// 高级柯里化实现
function curry(func) {
  return function curried(...args) {
    if (args.length >= func.length) {
      return func.apply(this, args);
    } else {
      return function(...args2) {
        return curried.apply(this, args.concat(args2));
      }
    }
  };
}

function sum(a, b, c) {
  return a + b + c;
}
let curriedSum = curry(sum);

alert( curriedSum(1, 2, 3) ); // 6，仍然可以被正常调用
alert( curriedSum(1)(2,3) ); // 6，对第一个参数的柯里化
alert( curriedSum(1)(2)(3) ); // 6，全柯里化
```

#### Reference Type

为确保 obj.methed() 调用正常运行，JavaScript 中点 '.' 返回的不是一个函数，而是一个特殊的 Reference Type 的值；

Reference Type 是 ECMA 中的一个“规范类型”，被用在 JavaScript 语言内部，因此不能直接使用它；

Reference Type 的值是一个三个值的组合 (base, name, strict)，其中：

- base 是对象；
- name 是属性名；
- strict 在 use strict 模式下为 true；

当 () 被在 Reference Type 上调用时，它们会接收到关于对象和对象的方法的完整信息，然后可以设置正确的 this；Reference Type 是一个特殊的“中间人”内部类型，目的是从 . 传递信息给 () 调用；

任何例如赋值 func = obj.func 等其他的操作，都会将 Reference Type 作为一个整体丢弃掉，而会取 obj.func（一个函数）的值并继续传递，所以任何后续操作都“丢失”了 this；

this 的值仅在函数直接被通过点符号 obj.method() 或方括号 obj['method']() 语法（此处它们作用相同）调用时才被正确传递；除了方法调用之外的任何操作（如赋值 = 或 ||），都会把它转换为一个不包含允许设置 this 信息的普通值；

#### BigInt

BigInt 是一种特殊的数字类型，它提供了对任意长度整数的支持；创建 bigint 的方式有两种：在一个整数字面量后面加 n 或者调用 BigInt 函数，该函数从字符串、数字等中生成 bigint；

```js
const bigint = 1234567890123456789012345678901234567890n;

const sameBigint = BigInt("1234567890123456789012345678901234567890");

const bigintFromNumber = BigInt(10); // 与 10n 相同
```

> Note: 对 `bigint` 的所有操作，返回的结果也是 `bigint`；不可以把 `bigint` 和常规数字类型混合使用；`BigInt` 不支持一元加法；

> Note: 转换操作始终是静默的，绝不会报错，但是如果 `bigint` 太大而数字类型无法容纳，则会截断多余的位，因此我们应该谨慎进行此类转换；

> Note: 由于 `number` 和 `bigint` 属于不同类型，它们可能在进行 `==` 比较时相等，但在进行 `===`（严格相等）比较时不相等；

当在 if 或其他布尔运算中时，bigint 的行为类似于 number，在 if 中，bigint 0n 为假，其他值为 true；

目前并没有一个众所周知的好用的 polyfill，不过，[JSBI](https://github.com/GoogleChromeLabs/jsbi) 库提出了另一种解决方案，该库使用自己的方法实现了大的数字，可以使用它们替代原生的 bigint；

#### 浏览器环境，规格

JavaScript 规范将能运行 JavaScript 的环境称为主机环境，每个环境都提供了特定于平台的功能；

文档对象模型（Document Object Model），简称 DOM，将所有页面内容表示为可以修改的对象；document 对象是页面的主要“入口点”，我们可以使用它来更改或创建页面上的任何内容；

另外也有一份针对 CSS 规则和样式表的、单独的规范 CSS Object Model (CSSOM)，这份规范解释了如何将 CSS 表示为对象，以及如何读写这些对象；

浏览器对象模型（Browser Object Model），简称 BOM，表示由浏览器（主机环境）提供的用于处理文档（document）之外的所有内容的其他对象；

#### DOM 树

根据文档对象模型（DOM），每个 HTML 标签都是一个对象，嵌套的标签是闭合标签的“子标签（children）”，标签内的文本也是一个对象；

标签被称为 元素节点（或者仅仅是元素），并形成了树状结构：`<html>` 在根节点；元素内的文本形成 文本节点，被标记为 `＃text`；一个文本节点只包含一个字符串，它没有子项，并且总是树的叶子；

> Note: 由于历史原因，`<head>` 之前的空格和换行符均被忽略，如果我们在 `</body>` 之后放置一些东西，那么它会被自动移动到 body 内，并处于 body 中的最下方，因为 HTML 规范要求所有内容必须位于 `<body>` 内；

如果浏览器遇到格式不正确的 HTML，它会在形成 DOM 时自动更正它；
- 将文档中纯文本内容包装到 `<html>` 和 `<body>`，并添加`<head>`；
- 自动添加关闭标签；
- 自动为 `<table>` 创建缺失的 `<tbody>`；

> Note: `HTML` 中的所有内容，包括注释，都会成为 `DOM` 的一部分，甚至 `HTML` 开头的 `<!DOCTYPE...>` 指令也是一个 `DOM` 节点；

常用的4中 DOM 节点：
- document：DOM 的“入口点”；
- 元素节点：HTML 标签，树构建块；
- 文本节点：包含文本；
- 注释：有时我们可以将一些信息放入其中，它不会显示，但 JS 可以从 DOM 中读取它；

> Tips: 浏览器开发者工具中，可以通过 `$0` 来进行操作最后选中的元素，先前选择的是 `$1`，`$2`...以此类推；如果存在引用 `DOM` 节点的变量，那么我们可以在控制台（Console）中使用命令 `inspect(node)`，来在元素（Elements）选项卡中查看它；

- [查看 DOM 结构在线工具](http://software.hixie.ch/utilities/js/live-dom-viewer/)

#### 遍历 DOM

最顶层的 document 节点是 document.documentElement，这是对应 `<html>` 标签的 DOM 节点；document.body 对应 `<body>`，document.head 对应 `<head>`；

**访问子节点**

childNodes 集合列出了所有子节点，包括文本节点和注释节点；childNodes 实际上并不是一个数组，而是一个类数组的可迭代对象，成为 DOM 集合；

> Note: DOM 集合是只读的，且几乎所有的 DOM 集合都是实时的，即它们反映了 DOM 的当前状态；

firstChild 和 lastChild 属性是访问第一个和最后一个子元素的快捷方式，还有一个特别的函数 elem.hasChildNodes() 用于检查节点是否有子节点；

**访问兄弟节点和父节点**

下一个兄弟节点在 nextSibling 属性中，上一个是在 previousSibling 属性中；通过 parentNode 来访问父节点；

**纯元素导航**

但希望操纵的是代表标签的和形成页面结构的元素节点时，使用：

- 使用 children 访问那些作为元素节点的子代的节点；
- firstElementChild，lastElementChild 访问标签子元素；
- previousElementSibling，nextElementSibling 访问标签子元素；
- parentElement 访问标签父节点；

> Note: `parentElement` 属性返回的是“元素类型”的父节点，而 `parentNode` 返回的是“任何类型”的父节点，这些属性通常来说是一样的：它们都是用于获取父节点；除了 `document.documentElement`

```js
alert( document.documentElement.parentNode ); // document
alert( document.documentElement.parentElement ); // null

// 任意节点 elem
while(elem = elem.parentElement) { // 向上，直到 <html>
  alert( elem );
}
```

#### 搜索：getElement*，querySelector*

**getElementById**

如果一个元素有 id 特性（attribute），那我们就可以使用 document.getElementById(id) 方法获取该元素；

> Note: 通过 `id` 命名的全局变量来访问元素，这是规范中考虑到兼容性描述的一种标准；但是这可能会造成命名冲突，不建议使用；

**querySelector 和 querySelectorAll**

elem.querySelector(css) 调用会返回给定 CSS 选择器的第一个元素，elem.querySelectorAll(css)，返回 elem 中与给定 CSS 选择器匹配的所有元素，都支持 css 伪类；

**matches**

elem.matches(css) 不会查找任何内容，它只会检查 elem 是否与给定的 CSS 选择器匹配，它返回 true 或 false；

```js
for (let elem of document.body.children) {
  if (elem.matches('a[href$="zip"]')) {
    alert("The archive reference: " + elem.href );
  }
}
```

**closest**

elem.closest(css) 方法会查找与 CSS 选择器匹配的最近的祖先，包括 elem 本身；

**getElementsBy\***

- elem.getElementsByTagName(tag) 查找具有给定标签的元素，并返回它们的集合，tag 参数也可以是对于任何标签的通配符 `*`；
- elem.getElementsByClassName(className) 返回具有给定CSS类的元素；
- document.getElementsByName(name) 返回在文档范围内具有给定 name 特性的元素；

> Note: 所有的 `getElementsBy*` 方法都会返回一个 实时的（live）集合，这样的集合始终反映的是文档的当前状态，并且在文档发生更改时会“自动更新”；而 `querySelectorAll` 返回的是一个静态的集合；

#### 节点属性：type，tag 和 content

EventTarget 是根的“抽象（abstract）”类，该类的对象从未被创建；它作为一个基础，以便让所有 DOM 节点都支持所谓的“事件（event）”；

Node 也是一个“抽象”类，充当 DOM 节点的基础；它提供了树的核心功能：parentNode，nextSibling，childNodes 等（它们都是 getter）；Node 类的对象从未被创建，但是有一些继承自它的具体的节点类，例如：文本节点的 Text，元素节点的 Element，注释节点的 Comment；

Element 是 DOM 元素的基本类；它提供了元素级的导航（navigation），例如 nextElementSibling，children，以及像 getElementsByTagName 和 querySelector 这样的搜索方法；浏览器中不仅有 HTML，还会有 XML 和 SVG，Element 类充当更多特定类的基本类：SVGElement，XMLElement 和 HTMLElement；

> Note: `document` 是 `HTMLDocument` 类的一个实例；

**nodeType 属性**

nodeType 属性提供了另一种“过时的”用来获取 DOM 节点类型的方法；该属性是一个只读的数值型值：

- 对于元素节点 elem.nodeType == 1；
- 对于文本节点 elem.nodeType == 3；
- 对于 document 对象 elem.nodeType == 9；

**标签：nodeName 和 tagName**

可以从 nodeName 或者 tagName 属性中读取一个 DOM 节点的标签名；tagName 属性仅适用于 Element 节点；nodeName 是为任意 Node 定义的，对于元素，它的意义与 tagName 相同，对于其他节点类型（text，comment 等），它拥有一个对应节点类型的字符串；

```js
alert( document.body.nodeName ); // BODY
alert( document.body.tagName ); // BODY

// for comment
alert( document.body.firstChild.tagName ); // undefined（不是一个元素）
alert( document.body.firstChild.nodeName ); // #comment

// for document
alert( document.tagName ); // undefined（不是一个元素）
alert( document.nodeName ); // #document
```

> 浏览器有两种处理文档（document）的模式：HTML 和 XML；通常，HTML 模式用于网页，只有在浏览器接收到带有 header Content-Type: application/xml+xhtml 的 XML-document 时，XML 模式才会被启用；

**innerHTML：内容**

innerHTML 属性允许将元素中的 HTML 获取为字符串形式；innerHTML 属性仅对元素节点有效；

> Note: 如果 `innerHTML` 将一个 `<script>` 标签插入到 `document` 中，它会成为 `HTML` 的一部分，但是不会执行；

> Note: `innerHTML+=` 会进行完全重写，该操作会先移除旧内容，然后写入新内容，因此其内部的图片和其他资源都将重写加载；同时大多浏览器在 innerHTML 内容改变后会取消文字选中状态；

**outerHTML：元素的完整 HTML**

outerHTML 属性包含了元素的完整 HTML，就像 innerHTML 加上元素本身一样；与 innerHTML 不同，写入 outerHTML 不会改变元素，而是在 DOM 中替换它；

**nodeValue/data：文本节点内容**

对于元素节点以外的其他节点类型，例如文本节点，具有它们的对应项：nodeValue 和 data 属性，这两者在实际使用中几乎相同，只有细微规范上的差异；因此可以将信息或模板说明嵌入到 HTML 中的注释中，然后从 data 属性中读取它，并处理嵌入的指令；

```html
<body>
  Hello
  <!-- Comment -->
  <script>
    let text = document.body.firstChild;
    alert(text.data); // Hello

    let comment = text.nextSibling;
    alert(comment.data); // Comment
  </script>
</body>
```

**textContent：纯文本**

textContent 提供了对元素内的 文本 的访问权限：仅文本，去掉所有 `<tags>`；textContent 允许以安全方式写入文本，所有符号（symbol）均按字面意义处理；

**hidden 属性**

“hidden” 特性（attribute）和 DOM 属性（property）指定元素是否可见，从技术上来说，hidden 与 style="display:none" 做的是相同的事，但 hidden 写法更简洁；

```html
<div>Both divs below are hidden</div>

<div hidden>With the attribute "hidden"</div>

<div id="elem">JavaScript assigned the property "hidden"</div>

<script>
  elem.hidden = true;
</script>
```

#### 特性和属性（Attributes and properties）

当浏览器加载页面时，它会“读取”（或者称之为：“解析”）HTML 并从中生成 DOM 对象；对于元素节点，大多数标准的 HTML 特性（attributes）会自动变成 DOM 对象的属性（properties）；

> Note: `DOM` 节点是常规的 JavaScript 对象，`DOM` 属性和方法的行为就像常规的 Javascript 对象一样；

在 HTML 中，标签可能拥有特性（attributes），当浏览器解析 HTML 文本，并根据标签创建 DOM 对象时，浏览器会辨别 标准的 特性并以此创建 DOM 属性；

如果一个特性不是标准的，那么就没有相对应的 DOM 属性，所有特性都可以通过使用以下方法进行访问：

- elem.hasAttribute(name) 检查特性是否存在；
- elem.getAttribute(name) 获取这个特性值；
- elem.setAttribute(name, value) 设置这个特性值；
- elem.removeAttribute(name) 移除这个特性；

或者也可以使用 elem.attributes 读取所有特性：属于内建 Attr 类的对象的集合，attributes 集合是可迭代对象，该对象将所有元素的特性（标准和非标准的）作为 name 和 value 属性存储在对象中；

```html
<body>
  <div id="elem" about="Elephant"></div>

  <script>
    alert( elem.getAttribute('About') ); // (1) 'Elephant'，读取

    elem.setAttribute('Test', 123); // (2) 写入

    alert( elem.outerHTML ); // (3) 查看特性是否在 HTML 中（在）

    for (let attr of elem.attributes) { // (4) 列出所有
      alert( `${attr.name} = ${attr.value}` );
    }
  </script>
</body>
```

此外 HTML 特性的名字是大小写不敏感的，其次特性的值总是字符串类型的；

当一个标准的特性或者属性被改变，对应的属性或者特性也会自动更新；

> Note: 但也有例外，如 `input.value` 只能从特性同步到属性，可用于用户行为导致 `value` 的更改，然后在这些操作之后，从 `HTML` 的特性中恢复“原始”值；

其他特例：

- input.checked 属性（对于 checkbox 的）是布尔型的；
- style 特性是字符串类型的，但 style 属性是一个对象；
- href DOM 属性一直是一个 完整的 URL，即使该特性包含一个相对路径或者包含一个 #hash；

**非标准的特性，dataset**

非标准的特性常常用于将自定义的数据从 HTML 传递到 JavaScript，或者用于为 JavaScript “标记” HTML 元素；

以 “data-” 开头的特性均被保留供开发者使用，并且它们可在 dataset 属性中使用；多个单词组合的特性，会转成对应的驼峰命名的属性；

```html
<body data-about="Elephants">
<script>
  alert(document.body.dataset.about); // Elephants
</script>
```

#### 修改文档（document）

**创建元素**

- document.createElement(tag) 用给定的标签创建一个新 元素节点（element node）；
- document.createTextNode(text) 用给定的文本创建一个 文本节点；

**插入元素**

字符串被以一种安全的方式插入到页面中，特殊符号都会被作转义处理来保证正确显示；

- node.append(...nodes or strings) 在 node 末尾插入节点或字符串；
- node.prepend(...nodes or strings) 在 node 开头插入节点或字符串；
- node.before(...nodes or strings) 在 node 前面插入节点或字符串；
- node.after(...nodes or strings) 在 node 后面插入节点或字符串；
- node.replaceWith(...nodes or strings) 将 node 替换为给定的节点或字符串；

旧方法都会返回插入/删除的节点：

- parentElem.appendChild(node) 将 node 附加为 parentElem 的最后一个子元素；
- parentElem.insertBefore(node, nextSibling) 在 parentElem 的 nextSibling 前插入 node；
- parentElem.replaceChild(node, oldChild) 将 parentElem 的后代中的 oldChild 替换为 node；
- parentElem.removeChild(node) 从 parentElem 中删除 node（假设 node 为 parentElem 的后代）；

**插入代码块**

elem.insertAdjacentHTML(where, html) 方法第一个参数是代码字（code word），指定相对于 elem 的插入位置，第二个参数是 HTML 字符串，该字符串会被“作为 HTML” 插入；

- "beforebegin" 将 html 插入到 elem 前插入；
- "afterbegin" 将 html 插入到 elem 开头；
- "beforeend" 将 html 插入到 elem 末尾；
- "afterend" 将 html 插入到 elem 后；

elem.insertAdjacentText(where, text) 语法一样，但是将 text 字符串“作为文本”插入而不是作为 HTML；elem.insertAdjacentElement(where, elem) 语法一样，但是插入的是一个元素；

**移除节点**

可以使用 node.remove() 移除一个节点；或者通过插入方法移动节点；

```html
<div id="first">First</div>
<div id="second">Second</div>
<script>
  // 无需调用 remove
  second.after(first); // 获取 #second，并在其后面插入 #first
</script>
```

**克隆节点**

调用 elem.cloneNode(true) 来创建元素的一个“深”克隆，具有所有特性（attribute）和子元素，如果我们调用 elem.cloneNode(false)，那克隆就不包括子元素；

**DocumentFragment**

DocumentFragment 是一个特殊的 DOM 节点，用作来传递节点列表的包装器（wrapper）；

**document.write**

调用 document.write(html) 意味着将 html “就地马上”写入页面，html 字符串可以是动态生成的，所以它很灵活，可以使用 JavaScript 创建一个完整的页面并对其进行写入；

document.write 调用只在页面加载时工作，如果页面加载完成后调用，则现有文档内容将被擦除；

从技术上讲，当在浏览器正在读取（“解析”）传入的 HTML 时调用 document.write 方法来写入一些东西，浏览器会像它本来就在 HTML 文本中那样使用它，因为不涉及 DOM 修改，所以运行起来出奇的快；

```js
// 删除元素内子元素
function clear(elem) {
  while (elem.firstChild) {
    elem.firstChild.remove();
  }
  // 或者
  elem.innerHTML = '';
}
```

> Note: 根据规范，`<table>` 只允许特定于表格的标签，浏览器会把 `<table>` 内的文本添加到了表格前面；

#### 样式和类

通常有两种设置元素样式的方式：

- 在 CSS 中创建一个类，并添加它：`<div class="...">`；
- 将属性直接写入 style：`<div style="...">`；

属性 className 对应于 "class" 特性；如果对 elem.className 进行赋值，它将替换类中的整个字符串；classList 是一个特殊的对象，它具有 add/remove/toggle 单个类的方法；

- elem.classList.add/remove(class) 添加/移除类；
- elem.classList.toggle(class) 如果类不存在就添加类，存在就移除它；
- elem.classList.contains(class) 检查给定类，返回 true/false；

elem.style 属性是一个对象，它对应于 "style" 特性（attribute）中所写的内容，对于多词（multi-word）属性，使用驼峰式 camelCase；

> Note: 像 `-moz-border-radius` 和 `-webkit-border-radius` 这样的浏览器前缀属性，也遵循同样的规则：连字符 `-` 表示大写；

我们使用 style.* 来对各个样式属性进行赋值，使用特殊属性 style.cssText 以字符串的形式设置完整的样式；

#### 计算样式：getComputedStyle

style 属性仅对 "style" 特性（attribute）值起作用，而没有任何 CSS 级联（cascade）；

使用 getComputedStyle(element, [pseudo]) 方法获取元素的计算属性；其中 element 是需要被读取样式值的元素，pseudo 伪元素（如果需要），空字符串或无参数则意味着元素本身，结果是一个具有样式属性解析值的对象；

> Note: 计算 (computed) 样式值是所有 CSS 规则和 CSS 继承都应用后的值，这是 CSS 级联（cascade）的结果；解析 (resolved) 样式值是最终应用于元素的样式值值，浏览器将使用计算（computed）值，并使所有单位均为固定的，且为绝对单位；

> Note: JavaScript 看不到 `:visited` 所应用的样式；此外，CSS 中也有一个限制，即禁止在 `:visited` 中应用更改几何形状的样式，这是为了确保一个不好的页面无法测试链接是否被访问，进而窥探隐私；

#### 元素大小和滚动 

> Note: 一些浏览器（并非全部）通过从内容（上面标记为 “content width”）中获取空间来为滚动条保留空间；

offsetParent 是最接近的 CSS 定位的祖先，或者是 td，th，table，body；

> Note: 有以下几种情况下，`offsetParent` 的值为 `null`：
> - 对于未显示的元素（display:none 或者不在文档中）；
> - 对于 `<body>` 与 `<html>`；
> - 对于带有 position:fixed 的元素；

属性 offsetLeft/offsetTop 提供相对于 offsetParent 左上角的 x/y 坐标；
属性 offsetWidth/offsetHeight 提供了元素的“外部” width/height，包括边框的完整大小；

> Note: 如果一个元素（或其任何祖先）具有 `display:none` 或不在文档中，则所有几何属性均为零（或 offsetParent 为 null）；

属性 clientLeft/clientTop 在元素内部，用于测量内侧与外侧的相对坐标，大多数情况下其数值等于边框宽高，但当滚动条在左侧时，clientLeft 等于左边框宽加滚动条宽度；

属性 clientWidth/clientHeight 包括了 “content width” 和 “padding”，但不包括滚动条宽度；

属性 scrollWidth/scrollHeight 是内容区域的完整内部宽度/高度，包括滚动出的部分；
属性 scrollLeft/scrollTop 是元素的隐藏、滚动部分的 width/height；

> Tips: 大多数几何属性是只读的，但是 `scrollLeft/scrollTop` 是可修改的，并且浏览器会滚动该元素；

> Note: CSS `width/height` 取决于另一个属性：`box-sizing`，它定义了“什么是” CSS 宽度和高度，出于 CSS 的目的而对 `box-sizing` 进行的更改可能会破坏此类 JavaScript 操作，其次，CSS 的 `width/height` 可能是 `auto`，有时滚动条也会造成一定的影响；因此通常不从 CSS 中获取 `width/height`；

> Tips: 可以创建一个带有滚动条的元素，但是没有边框（border）和内边距（padding），其全宽度 `offsetWidth` 和内部内容宽度 `clientWidth` 之间的差值就是滚动条的宽度；

- [JavaScript 现代教程-元素大小和滚动文档](https://zh.javascript.info/size-and-scroll)

#### Window 大小和滚动

可以使用 document.documentElement 的 clientWidth/clientHeight 获取窗口宽高；浏览器也支持像 window.innerWidth/innerHeight 这样的属性，window.innerWidth/innerHeight 包括了滚动条；

> Tips: `window.innerWidth - document.documentElement.clientWidth` 也可获得滚动条宽度；

为了可靠地获得完整的文档高度，我们应该采用以下这些属性的最大值：

```js
let scrollHeight = Math.max(
  document.body.scrollHeight, document.documentElement.scrollHeight,
  document.body.offsetHeight, document.documentElement.offsetHeight,
  document.body.clientHeight, document.documentElement.clientHeight
);

alert('Full document height, with scrolled out part: ' + scrollHeight);
```

使用 window.pageXOffset/pageYOffset 属性获取当前滚动位置；使用特殊方法 window.scrollBy(x,y) 和 window.scrollTo(pageX,pageY) 实现页面滚动；

- 方法 scrollBy(x,y) 将页面滚动至 相对于当前位置的 (x, y) 位置；
- 方法 scrollTo(pageX,pageY) 将页面滚动至 绝对坐标，使得可见部分的左上角具有相对于文档左上角的坐标 (pageX, pageY)；
- 对 elem.scrollIntoView(top) 的调用将滚动页面以使 elem 可见；如果 top=true（默认值），页面滚动，使 elem 出现在窗口顶部，元素的上边缘将与窗口顶部对齐，如果 top=false，页面滚动，使 elem 出现在窗口底部，元素的底部边缘将与窗口底部对齐；

> Tips: 使用 `elem.style.overflow = "hidden"` 禁止滚动；

#### 坐标

大多数 JavaScript 方法处理的是以下两种坐标系中的一个：

- 相对于窗口：类似于 position:fixed，从窗口的顶部/左侧边缘计算得出（clientX/clientY）；
- 相对于文档：与文档根（document root）中的 position:absolute 类似，从文档的顶部/左侧边缘计算得出（pageX/pageY）；

clientX/clientY 窗口相对坐标随着页面滚动会发生变化，因为同一个点越来越靠近窗口左侧/顶部；
pageX/pageY 元素在文档中的相对坐标保持不变，从文档顶部（现在已滚动出去）开始计算；

方法 elem.getBoundingClientRect() 返回最小矩形的窗口坐标，该矩形将 elem 作为内建 DOMRect 类的对象；包括以下属性：

- x/y：矩形原点相对于窗口的 X/Y 坐标；
- width/height：矩形的 width/height（可以为负）；
- top/bottom：顶部/底部矩形边缘的 Y 坐标；
- left/right：左/右矩形边缘的 X 坐标；

> Note: 由于历史原因，IE 浏览器不支持 x/y 属性；

对 document.elementFromPoint(x, y) 的调用会返回在窗口坐标 (x, y) 处嵌套最多（the most nested）的元素；只对在可见区域内的坐标 (x,y) 起作用，对于在窗口之外的坐标，elementFromPoint 返回 null；

```js
let centerX = document.documentElement.clientWidth / 2;
let centerY = document.documentElement.clientHeight / 2;

let elem = document.elementFromPoint(centerX, centerY);
elem.style.background = "red";
```

```js
// 获取元素的文档坐标
function getCoords(elem) {
  let box = elem.getBoundingClientRect();

  return {
    top: box.top + window.pageYOffset,
    right: box.right + window.pageXOffset,
    bottom: box.bottom + window.pageYOffset,
    left: box.left + window.pageXOffset
  };
}
```

#### 事件

当 HTML 的加载和处理均完成，DOM 被完全构建完成时触发 DOMContentLoaded 事件；当一个 CSS 动画完成时会触发 transitionend 事件；

**事件处理程序**

为了对事件作出响应，可以分配一个在事件发生时运行的函数处理程序（handler）；处理程序可以设置在 HTML 中名为 `on<event>` 的特性（attribute）中，也可以使用 DOM 属性（property）`on<event>` 来分配处理程序；

如果一个处理程序是通过 HTML 特性（attribute）分配的，那么随后浏览器读取它，并从特性的内容创建一个新函数，并将这个函数写入 DOM 属性（property）；

每个事件只有一个 `on<event>` 属性，无法分配更多事件处理程序；通过为 `on<event>` 属性赋值 null 可以移除处理程序；当浏览器读取 HTML 特性（attribute）时，浏览器将会使用特性中的内容（函数调用或多个语句）创建一个处理程序；

```js
// js 中方法不加括号
button.onclick = sayThanks;

// html 中方法要加括号
// <input type="button" id="button" onclick="sayThanks()">

button.onclick = function() {
  sayThanks(); // <-- 特性（attribute）中的内容
};
```

> Note: 处理程序中的 this 的值是对应的元素，就是处理程序所在的那个元素；

> 因为特性总是字符串的，函数变成了一个字符串，因此使用 setAttribute 设置处理程序会失效；

使用特殊方法 addEventListener 和 removeEventListener 来分配管理多个处理程序；

element.addEventListener(event, handler[, options]) 参数分别为 event 事件名，handler 处理程序，options 附加可选对象；如果同一事件设置有多个事件处理程序，并通过 addEventListener 分配给了相同的元素，则它们的运行顺序与创建顺序相同；

options 具有以下属性：
- once：如果为 true，那么会在被触发后自动删除监听器；
- capture：事件处理的阶段，true 为捕获阶段，false 为冒泡阶段（默认）；
- passive：如果为 true，那么处理程序将不会调用 preventDefault()；

> Note: 由于历史原因，options 也可以是 false/true，它与 {capture: false/true} 相同；

> Note: `passive: true` 选项告诉浏览器（特别是移动端浏览器），处理程序不会取消默认行为，然后浏览器先处理所有处理程序，再执行执行默认行为以提供最大程度的流畅体验，并通过某种方式处理事件；

element.removeEventListener(event, handler[, options]) 要移除处理程序，我们需要传入与分配的函数完全相同的函数，以及同一阶段；

```js
elem.addEventListener( "click" , () => alert('Thanks!'));
// 无法移除两个不同的函数对象
elem.removeEventListener( "click", () => alert('Thanks!'));
```

> Note: 对于某些事件，只能通过 `addEventListener` 设置处理程序，如 DOMContentLoaded 事件；

**事件对象**

当事件发生时，浏览器会创建一个 event 对象，将详细信息放入其中，并将其作为参数传递给处理程序；

event 对象的一些属性：

- event.type 事件类型
- event.currentTarget 处理事件的元素，同 this；

> Tips: `event` 对象在 HTML 处理程序中也可用，`<input type="button" onclick="alert(event.type)" value="Event type">`；

- [window.onload 和 DOMContentLoaded 的区别](https://www.jianshu.com/p/1a8a7e698447)

#### 冒泡和捕获

当一个事件发生在一个元素上，它会首先运行在该元素上的处理程序，然后运行其父元素上的处理程序，然后一直向上到其他祖先上的处理程序；

通过 event.target 属性，父元素上的处理程序始终可以获取事件实际发生位置的详细信息；event.target 与 event.currentTarget 有区别，event.target 是引发事件的“目标”元素，它在冒泡过程中不会发生变化，event.currentTarget 是“当前”元素，其中有一个当前正在运行的处理程序

冒泡事件从目标元素开始向上冒泡。通常，它会一直上升到 `<html>`，然后再到 document 对象，有些事件甚至会到达 window，它们会调用路径上所有的处理程序；但是任意处理程序都可以决定事件已经被完全处理，并通过调用 event.stopPropagation() 方法停止冒泡；

还有一个 event.stopImmediatePropagation() 方法，可以用于停止冒泡，并阻止当前元素上的处理程序运行，使用该方法之后，其他处理程序就不会被执行；

DOM 事件标准描述了事件传播的 3 个阶段：

- 捕获阶段（Capturing phase）事件（从 Window）向下走近元素；
- 目标阶段（Target phase）事件到达目标元素；
- 冒泡阶段（Bubbling phase）事件从元素上开始冒泡；

事件首先通过祖先链向下到达元素（捕获阶段），然后到达目标（目标阶段），最后上升（冒泡阶段），在途中调用处理程序；

> Tips: 通过属性 `event.eventPhase` 可以获得捕获事件的当前阶段（capturing=1，target=2，bubbling=3）；

#### 事件委托

捕获和冒泡允许我们实现一种被称为事件委托的强大的事件处理模式；如果有许多以类似方式处理的元素，那么就不必为每个元素分配一个处理程序，而是将单个处理程序放在它们的共同祖先上；

还可以使用事件委托将“行为（behavior）”以声明方式添加到具有特殊特性（attribute）和类的元素中，将自定义特性添加到描述其行为的元素，然后用文档范围级的处理程序追踪事件，如果事件发生在具有特定特性的元素上则执行行为（action）；

优点：

- 简化初始化并节省内存：无需添加许多处理程序；
- 更少的代码：添加或移除元素时，无需添加/移除处理程序；
- DOM 修改 ：我们可以使用 innerHTML 等，来批量添加/移除元素；

缺点：
- 事件必须冒泡，有些事件不会冒泡就无法使用事件委托；
- 委托可能会增加 CPU 负载，因为容器级别的处理程序会对容器中任意位置的事件做出反应；

#### 浏览器默认行为

许多事件会自动触发浏览器执行某些行为，如链接点击，表单提交等；可以通过两种方式阻止浏览器默认行为：

1. 使用 event 对象的 event.preventDefault() 方法；
2. 针对 `on<event>` 事件处理程序，返回 false 也同样有效；

> Note: 事件处理程序返回的值通常会被忽略，唯一的例外是从使用 `on<event>` 分配的处理程序中返回的 `return false`；

如果默认行为被阻止，那么 event.defaultPrevented 属性为 true，否则为 false；有时可以使用 event.defaultPrevented 来代替 event.stopPropagation()，来通知其他事件处理程序，该事件已经被处理；

```html
<p>Right-click for the document menu (added a check for event.defaultPrevented)</p>
<button id="elem">Right-click for the button menu</button>

<script>
  elem.oncontextmenu = function(event) {
    event.preventDefault();
    alert("Button context menu");
  };

  document.oncontextmenu = function(event) {
    if (event.defaultPrevented) return;

    event.preventDefault();
    alert("Document context menu");
  };
</script>
```

#### 创建自定义事件

内建事件类形成一个层次结构（hierarchy），类似于 DOM 元素类，根是内建的 Event 类；

通过 new Event(type[, options]) 创建一个事件对象，其中 type 是事件类型，自定义的字符串；options 是一个包含两个布尔值属性的对象：
- bubbles: true/false 如果为 true，那么事件会冒泡，默认 false；
- cancelable: true/false 如果为 true，那么“默认行为”就会被阻止，默认 false；

事件对象被创建后，使用 elem.dispatchEvent(event) 调用在元素上“运行”它；

> Tips: 对于来自真实用户操作的事件，`event.isTrusted` 属性为 `true`，对于脚本生成的事件，`event.isTrusted` 属性为 `false`；

对于自己的全新事件类型，应该使用 new CustomEvent，从技术上讲，CustomEvent 和 Event 一样，除了第二个参数（对象）中，可以为想要与事件一起传递的任何自定义信息添加一个附加的属性 detail；

```js
// 事件附带给处理程序的其他详细信息
elem.addEventListener("hello", function(event) {
  alert(event.detail.name);
});

elem.dispatchEvent(new CustomEvent("hello", {
  detail: { name: "John" }
}));
```

对于新的，自定义的事件，绝对没有默认的浏览器行为，但是分派（dispatch）此类事件的代码可能有自己的计划，触发该事件之后应该做什么，

通过调用 event.preventDefault()，事件处理程序可以发出一个信号，指出这些行为应该被取消；该事件必须具有 cancelable: true 标志，否则 event.preventDefault() 调用将会被忽略；

通常事件是在队列中处理的，但当一个事件是在另一个事件中发起的，例如使用 dispatchEvent，这类事件将会被立即处理，即在新的事件处理程序被调用之后，恢复到当前的事件处理程序；可以通过零延时 setTimeout 使原事件不受其它嵌套事件的影响，优先被处理完毕；

```html
<button id="menu">Menu (click me)</button>

<script>
  menu.onclick = function() {
    alert(1);

    menu.dispatchEvent(new CustomEvent("menu-open", {
      bubbles: true
    }));

    alert(2);
  };

  // 在 1 和 2 之间触发
  document.addEventListener('menu-open', () => alert('nested'));
  // 零延时 setTimeout
  document.addEventListener('menu-open', () => alert('nested'));
</script>
```

#### 鼠标事件

与点击相关的事件始终具有 button 属性，该属性允许获取确切的鼠标按钮；在 mousedown 和 mouseup 事件中则可能需要用到 event.button，因为这两个事件在任何按键上都会触发，所以我们可以使用 button 属性来区分是左键单击还是右键单击；

通常我们不在 click 和 contextmenu 事件中使用这一属性，因为前者只在单击鼠标左键时触发，后者只在单击鼠标右键时触发；

| 鼠标按键状态 |	event.button |
| :----- | :----- |
| 左键 (主要按键)	| 0 |
| 中键 (辅助按键)	| 1 |
| 右键 (次要按键)	| 2 |
| X1键 (后退按键)	| 3 |
| X2键 (前进按键)	| 4 |

> Note: 一些老代码可能会使用 event.which 属性来获得按下的按键，这是一个古老的非标准的方式，左中右键的值分别为1、2、3；

所有的鼠标事件都包含有关按下的组合键的信息：

- shiftKey：Shift；
- altKey：Alt（或对于 Mac 是 Opt）；
- ctrlKey：Ctrl；
- metaKey：对于 Mac 是 Cmd；

> Tips: 在 Mac 上我们通常使用 Cmd 代替 Ctrl，使用 `event.ctrlKey || event.metaKey` 判断；

所有的鼠标事件都提供了两种形式的坐标：

- 相对于窗口的坐标：clientX 和 clientY；
- 相对于文档的坐标：pageX 和 pageY；

双击鼠标会有副作用，在某些界面中可能会出现干扰：它会选择文本；最合理的方式是防止浏览器对 mousedown 进行操作；

```html
<b ondblclick="alert('Click!')" onmousedown="return false">
  Double-click me
</b>
```

如果想禁用选择以保护页面的内容不被复制粘贴，那么可以使用另一个事件：oncopy；

#### 移动鼠标：mouseover/out，mouseenter/leave

当鼠标指针移到某个元素上时，mouseover 事件就会发生，而当鼠标离开该元素时，mouseout 事件就会发生；

在 mouseover 和 mouseout 事件中，有一个 relatedTarget 属性，表示鼠标来自的那个元素或是鼠标移动到的，当前指针位置下的元素；当鼠标从窗口外移入时，其值为 null；

在鼠标快速移动的情况下，中间元素可能会被忽略，但是我们可以肯定一件事：如果鼠标指针“正式地”进入了一个元素（生成了 mouseover 事件），那么一旦它离开，我们就会得到 mouseout；

从父元素转到子元素时，也会触发 mouseover/out 事件，浏览器假定鼠标一次只会位于一个元素上最深的那个；

事件 mouseenter/mouseleave 类似于 mouseover/mouseout，它们在鼠标指针进入/离开元素时触发；但元素内部与后代之间的转换不会产生影响，且事件 mouseenter/mouseleave 不会冒泡；

#### 鼠标拖放事件

```js
// 球体拖动
ball.onmousedown = function(event) {

  let shiftX = event.clientX - ball.getBoundingClientRect().left;
  let shiftY = event.clientY - ball.getBoundingClientRect().top;

  ball.style.position = 'absolute';
  ball.style.zIndex = 1000;
  document.body.append(ball);

  moveAt(event.pageX, event.pageY);

  // 移动现在位于坐标 (pageX, pageY) 上的球
  // 将初始的偏移考虑在内
  function moveAt(pageX, pageY) {
    ball.style.left = pageX - shiftX + 'px';
    ball.style.top = pageY - shiftY + 'px';
  }

  function onMouseMove(event) {
    moveAt(event.pageX, event.pageY);
  }

  // 在 mousemove 事件上移动球
  document.addEventListener('mousemove', onMouseMove);

  // 放下球，并移除不需要的处理程序
  ball.onmouseup = function() {
    document.removeEventListener('mousemove', onMouseMove);
    ball.onmouseup = null;
  };

};

ball.ondragstart = function() {
  return false;
};
```

#### 指针事件

指针事件（Pointer Events）是一种用于处理来自各种输入设备（例如鼠标、触控笔和触摸屏等）的输入信息的现代化解决方案；

> Note: 除非你写的代码需要兼容旧版本的浏览器，例如 IE 10 或 Safari 12 或更低的版本，否则无需继续使用鼠标事件或触摸事件我们可以使用指针事件；

**指针事件类型**

| 指针事件 | 类似的鼠标事件 |
| :----- | :----- |
| pointerdown | mousedown |
| pointerup | mouseup |
| pointermove | mousemove |
| pointerover | mouseover |
| pointerout | mouseout |
| pointerenter | mouseenter |
| pointerleave | mouseleave |
| pointercancel | - |
| gotpointercapture | - |
| lostpointercapture | - |

**指针事件属性**

指针事件具备和鼠标事件完全相同的属性，包括 clientX/Y 和 target 等，以及一些其他属性：
- pointerId 触发当前事件的指针唯一标识符，由浏览器生成的，使能够处理多指针的情况，如多点触控功能；
- pointerType 指针的设备类型，必须为字符串，可以是：“mouse”、“pen” 或 “touch”；
- isPrimary 当指针为首要指针（多点触控时按下的第一根手指）时为 true；

有些指针设备会测量接触面积和点按压力（例如一根手指压在触屏上），对于这种情况可以使用以下属性：
- width 指针（例如手指）接触设备的区域的宽度，对于不支持的设备（如鼠标），这个值总是 1；
- height 指针（例如手指）接触设备的区域的长度，对于不支持的设备，这个值总是 1；
- pressure 触摸压力，是一个介于 0 到 1 之间的浮点数，对于不支持压力检测的设备，这个值总是 0.5（按下时）或 0；
- tangentialPressure 归一化后的切向压力（tangential pressure）；
- tiltX, tiltY, twist 针对触摸笔的几个属性，用于描述笔和屏幕表面的相对位置；

pointercancel 事件将会在一个正处于活跃状态的指针交互由于某些原因被中断时触发；在这个事件之后，该指针就不会继续触发更多事件了；

导致指针中断的可能原因如下：

- 指针设备硬件在物理层面上被禁用；
- 设备方向旋转（例如给平板转了个方向）；
- 浏览器打算自行处理这一交互，比如将其看作是一个专门的鼠标手势或缩放操作等；

阻止原生的拖放操作发生：
1. 对于非触屏设备：在 JS 中 ondragstart 事件处理返回 false；或者 event.preventDefault()；
2. 对于触屏设备：在 CSS 中设置 touch-action: none；

**指针捕获**

指针捕获（Pointer capturing）是针对指针事件的一个特性；elem.setPointerCapture(pointerId) 方法将给定的 pointerId 绑定到 elem；在调用之后，所有具有相同 pointerId 的指针事件都将 elem 作为目标（就像事件发生在 elem 上一样），无论这些 elem 在文档中的实际位置是什么；

绑定会在以下情况下被移除：

- 当 pointerup 或 pointercancel 事件出现时，绑定会被自动地移除；
- 当 elem 被从文档中移除后，绑定会被自动地移除；
- 当 elem.releasePointerCapture(pointerId) 被调用，绑定会被移除；

gotpointercapture 会在一个元素使用 setPointerCapture 来启用捕获后触发；
lostpointercapture 会在捕获被释放后触发：其触发可能是由于 releasePointerCapture 的显式调用，或是 pointerup/pointercancel 事件触发后的自动调用；

#### 键盘：keydown 和 keyup

当一个按键被按下时，会触发 keydown 事件，而当按键被释放时，会触发 keyup 事件；

事件对象的 key 属性允许获取字符，而事件对象的 code 属性则允许获取“物理按键代码”；每个按键的代码都取决于该按键在键盘上的位置，UI 事件代码规范 中描述了按键代码：

- 字符键的代码为 `Key<letter>`：KeyA，KeyB 等；
- 数字键的代码为：`Digit<number>`：Digit0，Digit1 等；
- 特殊按键的代码为按键的名字：Enter，Backspace，Tab 等；

> Tips: 为了可靠地跟踪与受键盘布局影响的字符，使用 `event.key` 可能是一个更好的方式；但为了满足切换了语言的情况下，依赖于它的热键也能正常工作，则使用绑定到物理键位置的 `event.code`；

> Note: 如果按下一个键足够长的时间，它就会开始“自动重复”：`keydown` 会被一次又一次地触发，对于由自动重复触发的事件，`event` 对象的 `event.repeat` 属性被设置为 `true`；

过去曾经有一个 keypress 事件，还有事件对象的 keyCode、charCode 和 which 属性；大多数浏览器对它们都存在兼容性问题；

- [字母数字按键代码W3C规范](https://www.w3.org/TR/uievents-code/#key-alphanumeric-section)

#### 滚动

scroll 事件在 window 和可滚动元素上都可以运行；不能通过在 onscroll 监听器中使用 event.preventDefault() 来阻止滚动，因为它会在滚动发生之后才触发；

但是我们可以在导致滚动的事件上，例如在 pageUp 和 pageDown 的 keydown 事件上，使用 event.preventDefault() 来阻止滚动；

滚动的两个重要特性：
- 滚动是“弹性的”；在某些浏览器/设备中，我们可以在文档的顶端或末端稍微多滚动出一点（超出部分显示的是空白区域，然后文档将自动“弹回”到正常状态）；
- 滚动并不精确；当我们滚动到页面末端时，实际上我们可能距真实的文档末端约 0-50px；

```js
// 判断是否滚动到页面底部
let windowRelativeBottom = document.documentElement.getBoundingClientRect().bottom;

// 如果用户将页面滚动的距离不够远（文档末端距窗口底部 >100px）
if (windowRelativeBottom > document.documentElement.clientHeight + 100) break;
```

```js
// 图片懒加载
/**
 * Tests if the element is visible (within the visible part of the page)
 * It's enough that the top or bottom edge of the element are visible
 */
function isVisible(image) {
  // todo: your code
  let position = image.getBoundingClientRect();
  return position.top < window.pageYOffset + document.documentElement.clientHeight + 30;
}

function showVisible() {
  for (let img of document.querySelectorAll('img')) {
    let realSrc = img.dataset.src;
    if (!realSrc) continue;

    if (isVisible(img)) {
      // disable caching
      // this line should be removed in production code
      realSrc += '?nocache=' + Math.random();

      img.src = realSrc;

      img.dataset.src = '';
    }
  }

}

window.addEventListener('scroll', showVisible);
showVisible();
```

#### 表单属性和方法

文档中的表单是特殊集合 document.forms 的成员，既可以使用名字，也可以使用在文档中的编号来获取表单；在表单中，其中的任何元素都可以通过命名的集合 form.elements 来获取到；

```html
<form name="my">
  <input name="one" value="1">
  <input name="two" value="2">
</form>

<script>
  // 获取表单
  let form = document.forms.my; // <form name="my"> 元素

  // 获取表单中的元素
  let elem = form.elements.one; // <input name="one"> 元素
  // let elem = form.elements["one"];
  // let elem = form.elements[0];

  alert(elem.value); // 1
</script>
```

对于相同 name 字段的 input 单选按钮，form.elements[name] 将会是一个集合；这些导航（navigation）属性并不依赖于标签的结构，所有的控件元素，无论它们在表单中有多深，都可以通过 form.elements 获取到；

一个表单内会有一个或多个 `<fieldset>` 元素，它们也具有 elements 属性，该属性列出了 `<fieldset>` 中的表单控件；

> Tips: `elements` 可以省略，可以通过 `form[index/name]` 来访问元素；

对于任何元素，其对应的表单都可以通过 element.form 访问到；

```html
<form id="form">
  <input type="text" name="login">
</form>

<script>
  // form -> element
  let login = form.login;

  // element -> form
  alert(login.form); // HTMLFormElement
</script>
```

**表单元素**

一个 `<select>` 元素有 3 个重要的属性：
- select.options `<option>` 的子元素的集合；
- select.value 当前所选择的 `<option>` 的 value；
- select.selectedIndex 当前所选择的 `<option>` 的编号；

并提供了三种为 `<select>` 设置 value 的不同方式：
- 找到对应的 `<option>` 元素，并将 option.selected 设置为 true；
- 将 select.value 设置为对应的 value；
- 将 select.selectedIndex 设置为对应 `<option>` 的编号；

如果 `<select>` 具有 multiple 特性（attribute），则允许多选；

在规范中，有一个很好的简短语法可以创建 `<option>` 元素：`new Option(text, value, defaultSelected, selected)`；其中参数：
- text `<option>` 中的文本；
- value `<option>` 的 value；
- defaultSelected 如果为 true，那么 selected HTML-特性（attribute）就会被创建；
- selected 如果为 true，那么这个 `<option>` 就会被选中；

`<option>` 元素具有以下属性：
- option.selected `<option>` 是否被选择；
- option.index `<option>` 在其所属的 `<select>` 中的编号；
- option.text `<option>` 的文本内容（可以被访问者看到）；

#### 聚焦：focus/blur

当用户点击某个元素或使用键盘上的 Tab 键选中时，该元素将会获得聚焦（focus），还有一个 HTML 特性（attribute）autofocus 可以让焦点在网页加载时默认落在一个元素上，此外还有其它途径可以获得焦点；

当元素聚焦时，会触发 focus 事件，当元素失去焦点时，会触发 blur 事件；elem.focus() 和 elem.blur() 方法可以设置和移除元素上的焦点；

无法通过在 onblur 事件处理程序中调用 event.preventDefault() 来“阻止失去焦点”，因为 onblur 事件处理程序是在元素失去焦点 之后 运行的；

> Note: `alert` 会将焦点移至自身，因此会导致元素失去焦点（触发 blur 事件），而当 `alert` 对话框被取消时，焦点又回重新回到原元素上（触发 focus 事件）；如果一个元素被从 DOM 中移除，那么也会导致焦点丢失，如果稍后它被重新插入到 DOM，焦点也不会回到它身上；

任何具有 tabindex 特性的元素，都会变成可聚焦的，该特性的 value 是当使用 Tab（或类似的东西）在元素之间进行切换时，元素的顺序号；可以使用 elem.tabIndex 通过 JavaScript 来添加 tabindex；

特殊值：
- tabindex="0" 会使该元素被与那些不具有 tabindex 的元素放在一起。也就是说，当我们切换元素时，具有 tabindex="0" 的元素将排在那些具有 tabindex ≥ 1 的元素的后面；
- tabindex="-1" 只允许以编程的方式聚焦于元素。Tab 键会忽略这样的元素，但是 elem.focus() 有效；

focus 和 blur 事件不会向上冒泡，但会在捕获阶段向下传播，因此可以在捕获阶段进行事件委托；也可以使用冒泡事件 focusin 和 focusout，但必须使用 elem.addEventListener 来分配它们；

#### 事件：change，input，cut，copy，paste

当元素更改完成时，将触发 change 事件，对于文本输入框，当其失去焦点时，就会触发 change 事件；

每当用户对输入值进行修改后，就会触发 input 事件；与键盘事件不同，只要值改变了，input 事件就会触发，即使那些不涉及键盘行为（action）的值的更改也是如此：使用鼠标粘贴，或者使用语音识别来输入文本；

ClipboardEvent 类，并提供了对剪切/拷贝/粘贴的数据的访问方法；

#### 表单：事件和方法提交

提交表单时，会触发 submit 事件，它通常用于在将表单发送到服务器之前对表单进行校验，或者中止提交，并使用 JavaScript 来处理表单；form.submit() 方法允许从 JavaScript 启动表单发送，我们可以使用此方法动态地创建表单，并将其发送到服务器；

提交表单主要有两种方式：
- 点击 `<input type="submit">` 或 `<input type="image">`；
- 在 input 字段中按下 Enter 键；

> Note: 在输入框中使用 Enter 发送表单时，会在 `<input type="submit">`上触发一次 click 事件；

#### 页面生命周期

**DOMContentLoaded**

浏览器已完全加载 HTML，并构建了 DOM 树，但像 `<img>` 和样式表之类的外部资源可能尚未加载完成；此时处理程序可以查找 DOM 节点，并初始化接口；

DOMContentLoaded 事件发生在 document 对象上，且必须使用 addEventListener 来捕获；

具有 async 特性（attribute）的脚本，以及使用 document.createElement('script') 动态生成并添加到网页的脚本也不会阻塞 DOMContentLoaded；

外部样式表不会影响 DOM，因此 DOMContentLoaded 不会等待它们，但如果在样式后面有一个脚本，那么该脚本必须等待样式表加载完成；

```html
<link type="text/css" rel="stylesheet" href="style.css">
<script>
  // 在样式表加载完成之前，脚本都不会执行
  alert(getComputedStyle(document.body).marginTop);
</script>
```

Firefox，Chrome 和 Opera 都会在 DOMContentLoaded 中自动填充表单，如果 DOMContentLoaded 被需要加载很长时间的脚本延迟触发，那么自动填充也会等待；

**window.load**

当整个页面，包括样式、图片和其他资源被加载完成时，会触发 window 对象上的 load 事件；此时外部资源已加载完成，样式已被应用，图片大小也已知了；

**window.onbeforeunload**

如果访问者触发了离开页面的导航（navigation）或试图关闭窗口，beforeunload 处理程序将要求进行更多确认；

```js
window.onbeforeunload = function() {
  // 在某些旧浏览器中返回非空字符串也被视为取消事件；
  return false;
};
```


**window.unload**

当访问者离开页面时，window 对象上的 unload 事件就会被触发，如果希望通过 unload 事件将数据保存到服务器上，可以使用 navigator.sendBeacon(url, data) 方法；该方法在后台发送数据，转换到另外一个页面不会有延迟：浏览器离开页面，但仍然在执行 sendBeacon；

```js
let analyticsData = { /* 带有收集的数据的对象 */ };

window.addEventListener("unload", function() {
  // 也可以发送表单以及其他格式的数据，数据大小限制在 64kb
  navigator.sendBeacon("/analytics", JSON.stringify(analyticsData));
});
```

**readystatechange 和 document.readyState**

document.readyState 属性可以提供当前加载状态的信息，它有三个值：
- loading —— 文档正在被加载；
- interactive —— 文档被全部读取；
- complete —— 文档被全部读取，并且所有资源（例如图片等）都已加载完成；

readystatechange 事件是跟踪文档加载状态的另一种机制，它会在状态发生改变时触发；

```html
<script>
  log('readyState:' + document.readyState);

  document.addEventListener('readystatechange', () => log('readyState:' + document.readyState));
  document.addEventListener('DOMContentLoaded', () => log('DOMContentLoaded'));

  window.onload = () => log('window onload');
</script>

<iframe src="iframe.html" onload="log('iframe onload')"></iframe>

<img src="http://en.js.cx/clipart/train.gif" id="img">
<script>
  img.onload = () => log('img onload');
</script>
```

包含 `<iframe>`，`<img>` 和记录事件的处理程序的文档完整的事件流：
1. readyState:loading
1. readyState:interactive
2. DOMContentLoaded
3. iframe onload
4. img onload
5. readyState:complete
6. window onload

在 DOMContentLoaded 之前，document.readyState 会立即变成 interactive，它们的意义实际上是相同的；当所有资源（iframe 和 img）都加载完成后，document.readyState 变成 complete；转换到 complete 状态的意义与 window.onload 相同，区别在于 window.onload 始终在所有其他 load 处理程序之后运行；

**脚本：async，defer**

当浏览器加载 HTML 时遇到 `<script>` 标签，浏览器就不能继续构建 DOM，它必须立刻执行此脚本，对于外部脚本也是一样的：浏览器必须等脚本下载完，并执行结束，之后才能继续处理剩余的页面；

通常把脚本放在页面底部，可以访问到页面上面的元素，并且不会阻塞页面显示内容；但是当遇到长的 HTML 文档时，则可能会造成明显的延迟；

**defer**

defer 特性告诉浏览器不要等待脚本，浏览器将继续处理 HTML，构建 DOM。脚本会“在后台”下载，然后等 DOM 构建完成后，脚本才会执行；

具有 defer 特性的脚本不会阻塞页面，总是要等到 DOM 解析完毕，但在 DOMContentLoaded 事件之前执行；

浏览器扫描页面寻找 defer 脚本，然后并行下载它们，以提高性能，且它们保持其相对顺序执行；

**async**

async 脚本就是一个会在加载完成时执行的完全独立的脚本，其他脚本不会等待 async 脚本加载完成，同样，async 脚本也不会等待其他脚本；浏览器也不会因 async 脚本而阻塞（与 defer 类似）；

DOMContentLoaded 和异步脚本不会彼此等待：
- 如果异步脚本在页面完成后才加载完成，则 DOMContentLoaded 可能会发生在异步脚本之前；
- 如果异步脚本很短，或者是从 HTTP 缓存中加载的，则 DOMContentLoaded 也可能发生在异步脚本之后；

**动态脚本**

可以使用 JavaScript 动态地创建一个脚本，并将其附加（append）到文档（document）中，当脚本被附加到文档时，脚本就会立即开始加载；

```js
let script = document.createElement('script');
script.src = "/article/script-async-defer/long.js";
document.body.append(script); // 添加到文档
```

默认情况下，动态脚本的行为是“异步”的，即效果同 async；可以显式地设置 script.async=false，改变这个规则，使执行效果同 defer；

> Tips: 在实际开发中，`defer` 用于需要整个 DOM 的脚本，和/或脚本的相对执行顺序很重要的时候；`async` 用于独立脚本，例如计数器或广告，这些脚本的相对执行顺序无关紧要；

#### 资源加载：onload，onerror

浏览器允许跟踪外部资源的加载，如脚本，iframe，图片等；readystatechange 事件也适用于资源，但很少被使用；

对于加载脚本 onload/onerror 事件仅跟踪加载本身；在脚本处理和执行期间可能发生的 error 超出了这些事件跟踪的范围；也就是说：如果脚本成功加载，则即使脚本中有编程 error，也会触发 onload 事件；

load 和 error 事件也适用于其他资源，基本上（basically）适用于具有外部 src 的任何资源；

大多数资源在被添加到文档中后，便开始加载，但是 `<img>` 是个例外，它要等到获得 src 后才开始加载；对于 `<iframe>` 来说，iframe 加载完成时会触发 iframe.onload 事件，无论是成功加载还是出现 error；

如果我们使用的是来自其他域的脚本，并且该脚本中存在 error，那么我们无法获取 error 的详细信息；

要允许跨源访问，`<script>` 标签需要具有 crossorigin 特性（attribute），并且远程服务器必须提供特殊的 header；这里有三个级别的跨源访问：

- 无 crossorigin 特性 —— 禁止访问；
- crossorigin="anonymous" 如果服务器的响应带有包含 * 或我们的源（origin）的 header Access-Control-Allow-Origin，则允许访问，浏览器不会将授权信息和 cookie 发送到远程服务器；
- crossorigin="use-credentials" 如果服务器发送回带有我们的源的 header Access-Control-Allow-Origin 和 Access-Control-Allow-Credentials: true，则允许访问。浏览器会将授权信息和 cookie 发送到远程服务器；

#### DOM 变动观察器（Mutation observer）

MutationObserver 是一个内建对象，它观察 DOM 元素，并在检测到更改时触发回调；

```js
// 创建一个带有回调函数的观察器
let observer = new MutationObserver(callback);
// 将其附加到一个 DOM 节点
observer.observe(node, config);
```

config 是一个具有布尔选项的对象，该布尔选项表示“将对哪些更改做出反应”：
- childList node 的直接子节点的更改；
- subtree node 的所有后代的更改；
- attributes node 的特性（attribute）；
- attributeFilter 特性名称数组，只观察选定的特性；
- characterData 是否观察 node.data（文本内容）；
- attributeOldValue 如果为 true，则将特性的旧值和新值都传递给回调，否则只传新值（需要 attributes 选项）；
- characterDataOldValue 如果为 true，则将 node.data 的旧值和新值都传递给回调，否则只传新值（需要 characterData 选项）；

在发生任何更改后，将执行“回调”：更改被作为一个 MutationRecord 对象列表传入第一个参数，而观察器自身作为第二个参数，MutationRecord 对象具有以下属性：
- type 变动类型，以下类型之一：
  - "attributes"：特性被修改了；
  - "characterData"：数据被修改了，用于文本节点；
  - "childList"：添加/删除了子元素；
- target 更改发生在何处："attributes" 所在的元素，或 "characterData" 所在的文本节点，或 "childList" 变动所在的元素；
- addedNodes/removedNodes 添加/删除的节点；
- previousSibling/nextSibling 添加/删除的节点的上一个/下一个兄弟节点；
- attributeName/attributeNamespace 被更改的特性的名称/命名空间（用于 XML）；
- oldValue 之前的值，仅适用于特性或文本更改，如果设置了相应选项 attributeOldValue/characterDataOldValue

方法 observer.disconnect() 可以停止观察，但这时观察器可能尚未处理某些更改；observer.takeRecords() 方法可以获取尚未处理的变动记录列表，表中记录的是已经发生，但回调暂未处理的变动；

> Note: 观察器在内部对节点使用弱引用，也就是说，如果一个节点被从 DOM 中移除了，并且该节点变得不可访问，那么它就可以被垃圾回收；

#### 选择（Selection）和范围（Range）

选择的基本概念是 Range：本质上是一对“边界点”：范围起点和范围终点；每个点都被表示为一个带有相对于起点的相对偏移（offset）的父 DOM 节点，如果父节点是元素节点，则偏移量是子节点的编号（子节点数），对于文本节点，则是文本中的位置（字符数）；

使用 new Range() 创建一个范围，Range 是一个无参的构造函数；使用 range.setStart(node, offset) 和 range.setEnd(node, offset) 来设置选择边界；

```html
<p id="p">Example: <i>italic</i> and <b>bold</b></p>

From <input id="start" type="number" value=1> – To <input id="end" type="number" value=4>
<button id="button">Click to select</button>
<script>
  button.onclick = () => {
    let range = new Range();

    range.setStart(p, start.value);
    range.setEnd(p, end.value);

    // 应用选择，后文有解释
    document.getSelection().removeAllRanges();
    document.getSelection().addRange(range);
  };
</script>
```

> Note: 一个范围可能跨越许多不相关的节点，所以不必在 `setStart` 和 `setEnd` 中使用相同的节点，唯一要注意的是终点要在起点之后；

range 对象具有以下属性：
- startContainer，startOffset 起始节点和偏移量；
- endContainer，endOffset 结束节点和偏移量；
- collapsed 布尔值，如果范围在同一点上开始和结束（所以范围内没有内容）则为 true；
- commonAncestorContainer 在范围内的所有节点中最近的共同祖先节点；

Range 方法：

- setStart(node, offset) 将起点设置在：node 中的位置 offset；
- setStartBefore(node) 将起点设置在：node 前面；
- setStartAfter(node) 将起点设置在：node 后面；

- setEnd(node, offset) 将终点设置为：node 中的位置 offset；
- setEndBefore(node) 将终点设置为：node 前面；
- setEndAfter(node) 将终点设置为：node 后面；

- selectNode(node) 设置范围以选择整个 node；
- selectNodeContents(node) 设置范围以选择整个 node 的内容；
- collapse(toStart) 如果 toStart=true 则设置 end=start，否则设置 start=end，从而折叠范围；
- cloneRange() 创建一个具有相同起点/终点的新范围；

- deleteContents() 从文档中删除范围内容；
- extractContents() 从文档中删除范围内容，并将删除的内容作为 DocumentFragment 返回；
- cloneContents() 复制范围内容，并将复制的内容作为 DocumentFragment 返回；
- insertNode(node) 在范围的起始处将 node 插入文档；
- surroundContents(node) 使用 node 将所选范围内容包裹起来，该范围必须包含其中所有元素的开始和结束标签；

文档选择是由 Selection 对象表示的，可通过 window.getSelection() 或 document.getSelection() 来获取；Selection API 规范描述了一个选择可以包括零个或多个范围，但实际上，在除 Firefox 之外的所有浏览器中，范围最多是 1；

选择属性：
- anchorNode 选择的起始节点；
- anchorOffset 选择开始的 anchorNode 中的偏移量；
- focusNode 选择的结束节点；
- focusOffset 选择开始处 focusNode 的偏移量；
- isCollapsed 如果未选择任何内容（空范围）或不存在，则为 true ；
- rangeCount 选择中的范围数，除 Firefox 外，其他浏览器最多为 1；

> Tips: 可以通过 `elem.onselectstart` 跟踪当选择从 elem 上开始的事件，阻止默认行为会使选择无法开始；document.onselectionchange 当选择变动时触发；

获取整个选择：
- 作为文本：只需调用 document.getSelection().toString()；
- 作为 DOM 节点：获取底层的（underlying）范围，并调用它们的 cloneContents() 方法；

选择方法：
- getRangeAt(i) 获取从 0 开始的第 i 个范围，在除 Firefox 之外的所有浏览器中，仅使用 0；
- addRange(range) 将 range 添加到选择中，如果选择已有关联的范围，则除 Firefox 外的所有浏览器都将忽略该调用；
- removeRange(range) 从选择中删除 range；
- removeAllRanges() 删除所有范围；
- empty() removeAllRanges 的别名；

- collapse(node, offset) 用一个新的范围替换选定的范围，该新范围从给定的 node 处开始，到偏移 offset 处结束；
- setPosition(node, offset) collapse 的别名；
- collapseToStart() 折叠（替换为空范围）到选择起点；
- collapseToEnd() 折叠到选择终点；
- extend(node, offset) 将选择的焦点（focus）移到给定的 node，位置偏移 offset；
- setBaseAndExtent(anchorNode, anchorOffset, focusNode, focusOffset) 用给定的起点 anchorNode/anchorOffset 和终点 focusNode/focusOffset 来替换选择范围，选中它们之间的所有内容；
- selectAllChildren(node) 选择 node 的所有子节点；
- deleteFromDocument() 从文档中删除所选择的内容；
- containsNode(node, allowPartialContainment = false) 检查选择中是否包含 node（特别是如果第二个参数是 true 的话）；

> Note: 如果选择已存在，需要先使用 `removeAllRanges()` 将其清空，然后添加范围；否则，除 Firefox 外的所有浏览器都将忽略新范围；

**表单控件中的选择**

诸如 input 和 textarea 等表单元素提供了 专用的选择 API，没有 Selection 或 Range 对象；

- 属性：
  - input.selectionStart 选择的起始位置（可写）；
  - input.selectionEnd 选择的结束位置（可写）；
  - input.selectionDirection 选择方向，其中之一：“forward”，“backward” 或 “none”；
- 事件：
  - input.onselect 当某个东西被选择时触发；
- 方法：
  - input.select() 选择文本控件中的所有内容；
  - input.setSelectionRange(start, end, [direction]) 在给定方向上（可选），从 start 一直选择到 end；input.setRangeText(replacement, [start], [end], [selectionMode]) 用新文本替换范围中的文本；

input.setRangeText() 方法如果提供的话可选参数 start 和 end，则设置范围的起点和终点，否则使用用户的选择，最后一个可选参数 selectionMode 决定替换文本后如何设置选择，可能的值为：
- "select" 将选择新插入的文本；
- "start" 选择范围将在插入的文本之前折叠（光标将在其之前）；
- "end" 选择范围将在插入的文本之后折叠（光标将紧随其后）；
- "preserve" 尝试保留选择，默认值；

> Note: 根据规范，发表单控件内的选择不应该触发 `document.onselectionchange` 事件，因为它与 `document` 选择和范围不相关；

> Tips: 一个重要的边界情况是 selectionStart 和 selectionEnd 彼此相等，也正是光标位置；通过将 selectionStart 和 selectionEnd 设置为相同的值，可以实现移动光标的效果；

```html
<style>
  /* CSS 样式 */
  #el {
    user-select: none;
  }
</style>

<div class="el">Unselectable text.</div>

<script>
// 取消选择默认事件
el.onselectstart = () => false;

// 或者在选择发生后清除选择
document.getSelection().empty() 
</script>
```

#### 事件循环：微任务和宏任务

浏览器中 JavaScript 的执行流程和 Node.js 中的流程都是基于事件循环的；事件循环是一个在 JavaScript 引擎等待任务，执行任务和进入休眠状态等待更多任务这几个状态之间转换的无限循环；

一个任务到来时，引擎可能正处于繁忙状态，那么这个任务就会被排入队列；多个任务组成了一个队列，即所谓的“宏任务队列”（v8 术语）；例如以下宏任务：

- 当外部脚本 `<script src="...">` 加载完成时，任务就是执行它
- 当用户移动鼠标时，任务就是派生出 mousemove 事件和执行处理程序；
- 当安排的（scheduled）setTimeout 时间到达时，任务就是执行其回调；

引擎执行任务时永远不会进行渲染（render），如果任务执行需要很长一段时间也没关系，仅在任务完成后才会绘制对 DOM 的更改；如果一项任务执行花费的时间过长，浏览器将无法执行其他任务，通常浏览器会抛出一个如“页面未响应”之类的警报；

当引擎忙于语法高亮时，它就无法处理其他 DOM 相关的工作，它甚至可能会导致浏览器“中断（hiccup）”甚至“挂起（hang）”一段时间；可以通过将大任务拆分成多个小任务来避免这个问题；对浏览器脚本中的过载型任务进行拆分的另一个好处是，可以显示进度指示；

> Tips: 在事件处理程序中，可能会推迟某些行为，直到事件冒泡并在所有级别上得到处理后；可以通过将该代码包装到零延迟的 `setTimeout` 中来做到这一点；

```js
menu.onclick = function() {
  // 创建一个具有被点击的菜单项的数据的自定义事件
  let customEvent = new CustomEvent("menu-open", {
    bubbles: true
  });

  // 异步分派（dispatch）自定义事件
  setTimeout(() => menu.dispatchEvent(customEvent));
};
```

微任务仅来自于我们的代码，它们通常是由 promise 创建的：对 .then/catch/finally 处理程序的执行会成为微任务；微任务也被用于 await 的“幕后”，因为它是 promise 处理的另一种形式；

还有一个特殊的函数 queueMicrotask(func)，它对 func 进行排队，以在微任务队列中执行；

微任务会在执行任何其他事件处理，或渲染，或执行任何其他宏任务之前完成；如果我们想要异步执行（在当前代码之后）一个函数，但是要在更改被渲染或新事件被处理之前执行，那么我们可以使用 queueMicrotask 来对其进行安排（schedule）；

简化的事件循环算法：
1. 从宏任务队列中出队（dequeue）并执行最早的任务；
2. 执行所有微任务：当微任务队列非空时：出队（dequeue）并执行最早的微任务；
3. 如果有变更，则将变更渲染出来；
4. 如果宏任务队列为空，则休眠直到出现宏任务；
5. 转到步骤 1；

#### 弹窗和 window 的方法

如果弹窗是在用户触发的事件处理程序（如 onclick）之外调用的，大多数浏览器都会阻止此类弹窗；

**window.open**

打开一个弹窗的语法是 window.open(url, name, params)：
- url 要在新窗口中加载的 URL；
- name 新窗口的名称，每个窗口都有一个 window.name，在这里我们可以指定哪个窗口用于弹窗，如果已经有一个这样名字的窗口，将在该窗口打开给定的 URL，否则会打开一个新窗口；
- params 新窗口的配置字符串，用逗号分隔，参数之间不能有空格；

params 的设置项：
- left/top屏幕上窗口的左上角的坐标；这有一个限制：不能将新窗口置于屏幕外；
- width/height 新窗口的宽度和高度；宽度/高度的最小值是有限制的，因此不可能创建一个不可见的窗口；
- menubar（yes/no）显示或隐藏新窗口的浏览器菜单；
- toolbar（yes/no）显示或隐藏新窗口的浏览器导航栏（后退，前进，重新加载等）；
- location（yes/no）显示或隐藏新窗口的 URL 字段。Firefox 和 IE 浏览器不允许默认隐藏它；
- status（yes/no）显示或隐藏状态栏；大多数浏览器都会强制显示它；
- resizable（yes/no）允许禁用新窗口大小调整，不建议使用；
- scrollbars（yes/no）允许禁用新窗口的滚动条，不建议使用；

设置中的省略规则：
- 如果 open 调用中没有第三个参数，或者它是空的，则使用默认的窗口参数；
- 如果这里有一个参数字符串，但是某些 yes/no 功能被省略了，那么被省略的功能则被默认值为 no；
- 如果参数中没有 left/top，那么浏览器会尝试在最后打开的窗口附近打开一个新窗口；
- 如果没有 width/height，那么新窗口的大小将与上次打开的窗口大小相同；

open 调用会返回对新窗口的引用，它可以用来操纵弹窗的属性，更改位置，甚至更多操作；弹窗也可以使用 window.opener 来访问 opener 窗口；除了弹窗之外，对其他所有窗口来说，window.opener 均为 null；

> Note: 只有在窗口是同源的时，窗口才能自由访问彼此的内容；

通过 win.close() 方法关闭一个窗口，使用 win.closed 属性检查一个窗口是否被关闭；

- [JavaScript 现代教程-弹窗和 window 的方法](https://zh.javascript.info/popup-windows)

#### 跨窗口通信

如果两个 URL 具有相同的协议，域和端口，则称它们是“同源”的；“同源（Same Origin）”策略限制了窗口（window）和 frame 之间的相互访问；非同源窗口引用只能更改其 location；

**iframe**

一个 `<iframe>` 标签承载了一个单独的嵌入的窗口，它具有自己的 document 和 window；

- iframe.contentWindow 来获取 `<iframe>` 中的 window；
- iframe.contentDocument 来获取 `<iframe>` 中的 document，是 iframe.contentWindow.document 的简写形式；

> Tips: 通过运行脚本设置拥有二级子域名子窗口的 `document.domain` 属性为主窗口的主域名，可以实现子窗口和主窗口无限制通信；

获取 `<iframe>` 的 window 对象的另一个方式是从命名集合 window.frames 中获取：
- 通过索引获取：window.frames[0] 文档中的第一个 iframe 的 window 对象；
- 通过名称获取：window.frames.iframeName 获取 name="iframeName" 的 iframe 的 window 对象；

多层嵌套 iframe：
- window.frames “子”窗口的集合（用于嵌套的 iframe）；
- window.parent 对“父”（外部）窗口的引用；
- window.top 对最顶级父窗口的引用；

> Tips: 可以使用 `top` 属性来检查当前的文档是否是在 `iframe` 内打开的：`window == window.top`；

sandbox 特性（attribute）允许在 `<iframe>` 中禁止某些特定行为，以防止其执行不被信任的代码；它通过将 iframe 视为非同源的，或者应用其他限制来实现 iframe 的“沙盒化”；

对于 `<iframe sandbox src="...">`，有一个应用于其上的默认的限制集，一个空的 "sandbox" 特性会施加最严格的限制，但是我们用一个以空格分隔的列表，列出要移除的限制；包括以下限制：
- allow-same-origin：默认情况下，"sandbox" 会为 iframe 强制实施“不同来源”的策略；
- allow-top-navigation：允许 iframe 更改 parent.location；
- allow-forms：允许在 iframe 中提交表单；
- allow-scripts：允许在 iframe 中运行脚本；
- allow-popups：允许在 iframe 中使用 window.open 打开弹窗；
- allow-popups：允许在 iframe 中使用 window.open 打开弹窗；

> Note: `sandbox` 特性的目的仅是添加更多限制，它无法移除这些限制；尤其是，如果 `iframe` 来自其他源，则无法放宽同源策略；

postMessage 接口允许窗口之间相互通信，无论它们来自什么源；win.postMessage(data, targetOrigin) 方法，data 是要发送的数据，可以是任何对象，数据会被通过使用“结构化序列化算法（structured serialization algorithm）”进行克隆；targetOrigin 指定目标窗口的源，以便只有来自给定的源的窗口才能获得该消息；

targetOrigin 是一种安全措施，如果目标窗口是非同源的，我们无法在发送方窗口读取它的 location；如果不希望做这个检查，可以将 targetOrigin 设置为 *；

为了接收消息，目标窗口应该在 message 事件上有一个处理程序；当 postMessage 被调用时触发该事件（并且 targetOrigin 检查成功）；回调参数 event 对象具有特殊属性：
- data 从 postMessage 传递来的数据；
- origin 发送方的源；
- source 对发送方窗口的引用，可以通过 source.postMessage(...) 立即回复；

> Note: `message` 事件只能通过 `addEventListener` 分配处理程序；

#### ArrayBuffer，二进制数组

ArrayBuffer 是 JavaScript 中基本的二进制对象，是对固定长度的连续内存空间的引用；

```js
// 分配一个 16 字节的连续内存空间，并用 0 进行预填充
let buffer = new ArrayBuffer(16); 
```

如要操作 ArrayBuffer，需要使用“视图”对象；视图对象本身并不存储任何东西；它是一副“眼镜”，透过它来解释存储在 ArrayBuffer 中的字节；

- Uint8Array 将 ArrayBuffer 中的每个字节视为 0 到 255 之间的单个数字，这称为 “8 位无符号整数”；
- Uint16Array 将每 2 个字节视为一个 0 到 65535 之间的整数，这称为 “16 位无符号整数”；
- Uint32Array 将每 4 个字节视为一个 0 到 4294967295 之间的整数，这称为 “32 位无符号整数”；
- Float64Array 将每 8 个字节视为一个 5.0x10^324 到 1.8x10^308 之间的浮点数；
- Uint8ClampedArray 用于 8 位整数，在赋值时便“固定“其值；
- Int8Array，Int16Array，Int32Array 用于有符号整数（可以为负数）；
- Float32Array，Float64Array 用于 32 位和 64 位的有符号浮点数；

所有这些视图（Uint8Array，Uint32Array 等）的通用术语是 TypedArray，它们都享有同一组方法和属性；构造方式：

```js
// 如果给定的是 ArrayBuffer 参数，则会在其上创建视图
// 给定起始位置 byteOffset（默认为 0）以及 length（默认至 buffer 的末尾），视图仅涵盖 buffer 的一段
// new TypedArray(buffer, [byteOffset], [length]);
let buffer = new ArrayBuffer(16); // 创建一个长度为 16 的 buffer
let view = new Uint32Array(buffer); // 将 buffer 视为一个 32 位整数的序列

// 如果给定的是 Array，或任何类数组对象，则会创建一个相同长度的类型化数组，并复制其内容
// new TypedArray(object);
let arr = new Uint8Array([0, 1, 2, 3]);
alert( arr.length ); // 4，创建了相同长度的二进制数组
alert( arr[1] ); // 1，用给定值填充了 4 个字节（无符号 8 位整数）

// 如果给定的是另一个 TypedArray，也是如此：创建一个相同长度的类型化数组，并复制其内容；
// 如果需要的话，数据在此过程中会被转换为新的类型
// new TypedArray(typedArray);
let arr16 = new Uint16Array([1, 1000]);
let arr8 = new Uint8Array(arr16);

// 对于数字参数 length —— 创建类型化数组以包含这么多元素；
// 它的字节长度将是 length 乘以单个 TypedArray.BYTES_PER_ELEMENT 中的字节数
// new TypedArray(length);
let arr = new Uint16Array(4); // 为 4 个整数创建类型化数组
alert( Uint16Array.BYTES_PER_ELEMENT ); // 每个整数 2 个字节
alert( arr.byteLength ); // 8（字节中的大小）

// 不带参数的情况下，创建长度为零的类型化数组
new TypedArray();
```

可以直接创建一个 TypedArray，而无需提及 ArrayBuffer，除第一种情况（已提供 ArrayBuffer）外，其他所有情况都会自动创建 ArrayBuffer；可以通过 buffer 属性，访问 ArrayBuffer，byteLength 属性获取 ArrayBuffer 的长度；

如果尝试将越界值写入类型化数组，多余的位被切除；Uint8ClampedArray 在这方面比较特殊，它的表现不太一样；对于大于 255 的任何数字，它将保存为 255，对于任何负数，它将保存为 0，此行为对于图像处理很有用；

TypedArray 具有常规的 Array 方法：
- 可以遍历（iterate），map，slice，find 和 reduce 等；
- 没有 splice 和 concat；
- arr.set(fromArr, [offset]) 从 offset（默认为 0）开始，将 fromArr 中的所有元素复制到 arr；
- arr.subarray([begin, end]) 创建一个从 begin 到 end（不包括）相同类型的新视图，不复制任何内容；

DataView 是在 ArrayBuffer 上的一种特殊的超灵活“未类型化”视图；它允许以任何格式访问任何偏移量（offset）的数据；

对于类型化的数组，构造器决定了其格式，整个数组应该是统一的，第 i 个数字是 arr[i]；通过 DataView，我们可以使用 .getUint8(i) 或 .getUint16(i) 之类的方法访问数据；可以在调用方法时选择格式，而不是在构造的时候；

通过调用 new DataView(buffer, [byteOffset], [byteLength]) 创建一个 dataView 视图；

- buffer 底层的 ArrayBuffer；与类型化数组不同，DataView 不会自行创建缓冲区（buffer）；
- byteOffset 视图的起始字节位置（默认为 0）；
- byteLength 视图的字节长度（默认至 buffer 的末尾）；

> Note: 还有另外两个术语，用于对二进制数据进行操作的方法的描述：`ArrayBufferView` 是所有这些视图的总称，`BufferSource` 是 `ArrayBuffer` 和 `ArrayBufferView` 的总称；

#### TextDecoder 和 TextEncoder

内建的 TextDecoder 对象在给定缓冲区（buffer）和编码格式（encoding）的情况下，能够将值读取到实际的 JavaScript 字符串中；

decoder 通过 new TextDecoder([label], [options]) 创建：
- label 编码格式，默认为 utf-8，但同时也支持 big5，windows-1251 等许多其他编码格式；
- options 可选对象；
  - fatal 布尔值，如果为 true 则为无效（不可解码）字符抛出异常，否则（默认）用字符 \uFFFD 替换无效字符 ；
  - ignoreBOM 布尔值，如果为 true 则 BOM（可选的字节顺序 unicode 标记），很少需要使用；

通过 decoder.decode([input], [options]) 方法解码：
- input 要被解码的 BufferSource；
- options 可选对象：
  - stream 对于解码流，为 true，则将传入的数据块（chunk）作为参数重复调用 decoder；

stream 为 true 时，多字节的字符可能偶尔会在块与块之间被分割，这个选项告诉 TextDecoder 记住“未完成”的字符，并在下一个数据块来的时候进行解码；


```js
let uint8Array = new Uint8Array([72, 101, 108, 108, 111]);

alert( new TextDecoder().decode(uint8Array) ); // Hello
```

TextEncoder 做相反的事情：将字符串转换为字节；通过 new TextEncoder() 创建；encoder 有两个方法：

- encode(str) 从字符串返回 Uint8Array；
- encodeInto(str, destination) 将 str 编码到 destination 中，该目标必须为 Uint8Array；

```js
let encoder = new TextEncoder();

let uint8Array = encoder.encode("Hello");
alert(uint8Array); // 72,101,108,108,111
```

#### Blob

arrayBuffer 和视图（view）都是 ECMA 标准的一部分，是 JavaScript 的一部分；浏览器还有一个二进制类型大对象 Blob，Blob 对象表示一个不可变、原始数据的类文件对象，由可选的字符串 type（通常是 MIME 类型）和 blobParts 组成；

new Blob(blobParts, options) 创建一个 Blob；
- blobParts 是 Blob/BufferSource/String 类型的值的数组；
- options 可选对象；
  - type Blob 类型，通常是 MIME 类型；
  - endings 是否转换换行符，使 Blob 对应于当前操作系统的换行符（\r\n 或 \n）；默认为 "transparent"（不转换），或者 "native"（转换）；

可以用 blob.slice([byteStart], [byteEnd], [contentType]) 方法来提取 Blob 片段：
- byteStart 起始字节，默认为 0；
- byteEnd 最后一个字节（专有，默认为最后）；
- contentType 新 blob 的 type，默认与源 blob 相同；

> Note: 无法直接在 `Blob` 中更改数据，但我们可以通过 `slice` 获得 `Blob` 的多个部分，从这些部分创建新的 `Blob` 对象，将它们组成新的 `Blob`；

**Blob 作为 URL**

Blob 可以很容易用作 `<a>`、`<img>` 或其他标签的 URL，来显示它们的内容；

```js
// download 特性（attribute）强制浏览器下载而不是导航
// <a download="hello.txt" href="blob:http://localhost:8000/c2d14ed6-c271-46be-9164-06042e419312">hello.txt</a>
let blob = new Blob(["Hello, world!"], { type: "text/plain" });
let link = document.createElement("a");
link.download = "hello.txt";
link.href = URL.createObjectURL(blob);
link.innerHTML = "hello.txt";

// 添加到 DOM
document.body.append(link);
// 或者模拟点击，自动下载
link.click();
// 从内部映射中移除引用
URL.revokeObjectURL(link.href);
```

URL.createObjectURL 取一个 Blob，并为其创建一个唯一的 URL，形式为 `blob:<origin>/<uuid>`；浏览器内部为每个通过 URL.createObjectURL 生成的 URL 存储了一个 URL -> Blob 映射，生成的 URL（即其链接）仅在当前文档打开的状态下才有效；

> Note: 如果我们创建一个 `URL`，那么即使我们不再需要该 `Blob` 了，它也会被挂在内存中；`URL.revokeObjectURL(url)` 从内部映射中移除引用，因此允许 `Blob` 被删除（如果没有其他引用的话），并释放内存；

**Blob 转为 base64**

URL.createObjectURL 的一个替代方法是，将 Blob 转换为 base64-编码的字符串；这种编码将二进制数据表示为一个由 0 到 64 的 ASCII 码组成的字符串，非常安全且“可读”；

可以在 “data-url” 中使用此编码，“data-url” 的形式为 `data:[<mediatype>][;base64],<data>`，并且可以在任何地方和“常规” url 一样使用这种 url；

```js
let blob = new Blob(['Hello, world!'], {type: 'text/plain'});

// 内建的 FileReader 对象可以将 Blob 转换为 base64；
let reader = new FileReader();
reader.readAsDataURL(blob); 

// 调用 onload
reader.onload = function() {
  link.href = reader.result; // data url
  link.click();
};
```

> Note: `Blob` 转换为 `data url`，当对大的 `Blob` 进行编码时，性能和内存会有损耗；

**Image 转为 blob**

可以创建一个图像（image）的、图像的一部分、或者甚至创建一个页面截图的 Blob；

```js
// 获取任何图像
let img = document.querySelector('img');

// 生成同尺寸的 <canvas>
let canvas = document.createElement('canvas');
canvas.width = img.clientWidth;
canvas.height = img.clientHeight;

let context = canvas.getContext('2d');

// 向其中复制图像（此方法允许剪裁图像）
context.drawImage(img, 0, 0);
// 我们 context.rotate()，并在 canvas 上做很多其他事情

// toBlob 是异步操作，结束后会调用 callback
let blob = await new Promise(resolve => canvasElem.toBlob(resolve, 'image/png'));
  // blob 创建完成，下载它
let link = document.createElement('a');
link.download = 'example.png';

link.href = URL.createObjectURL(blob);
link.click();

// 删除内部 blob 引用，这样浏览器可以从内存中将其清除
URL.revokeObjectURL(link.href);
```

**Blob 转为 arrayBuffer**

Blob 构造器允许从几乎所有东西创建 blob，包括任何 BufferSource；如果需要执行低级别的操作的话，则可以使用 blob.arrayBuffer() 从 blob 中获取最低级别的 arrayBuffer；

```js
// 从 blob 获取 arrayBuffer
const bufferPromise = await blob.arrayBuffer();

// 或者
blob.arrayBuffer().then(buffer => /* 处理 arrayBuffer */);
```

**Blob 转为 stream**

当读取和写入超过 2 GB 的 blob 时，将其转换为 arrayBuffer 的使用对我们来说会更加占用内存；这种情况下，可以直接将 blob 转换为 stream 进行处理；stream 是一种特殊的对象，我们可以从它那里逐部分地读取（或写入）；

```js
// 从 blob 获取可读流（readableStream）
const readableStream = blob.stream();
const stream = readableStream.getReader();

while (true) {
  // 对于每次迭代：data 是下一个 blob 数据片段
  let { done, data } = await stream.read();
  if (done) {
    // 读取完毕，stream 里已经没有数据了
    console.log('all blob processed.');
    break;
  }

  // 对刚从 blob 中读取的数据片段做一些处理
  console.log(data);
}
```

#### File 和 FileReader

File 对象继承自 Blob，并扩展了与文件系统相关的功能；

File 对象可以通古构造器创建 new File(fileParts, fileName, [options])；

- fileParts Blob/BufferSource/String 类型值的数组；
- fileName 文件名字符串；
- options 可选对象，
  - lastModified 最后一次修改的时间戳（整数日期）；

或者通常从 `<input type="file">` 或拖放或其他浏览器接口来获取文件；在这种情况下，file 将从操作系统（OS）获得 this 信息；输入（input）可以选择多个文件，因此 input.files 是一个类数组对象；

```js
    let file = document.createElement("input");
    file.type = "file";
    file.onchange = function (e) {
        console.log(this.files[0]);
    };
    document.body.append(file);
```

FileReader 是一个对象，其唯一目的是从 Blob（因此也从 File）对象中读取数据；但使用事件来传递数据，因为从磁盘读取数据可能比较费时间；

- 创建：
  - new FileReader() 一个无参构造函数；
- 方法：
  - readAsArrayBuffer(blob) 将数据读取为二进制格式的 ArrayBuffer；
  - readAsText(blob, [encoding]) 将数据读取为给定编码（默认为 utf-8 编码）的文本字符串；
  - readAsDataURL(blob) 读取二进制数据，并将其编码为 base64 的 data url；
  - abort() 取消操作；
- 事件：
  - loadstart 开始加载；
  - progress 在读取过程中出现；
  - load 读取完成，没有 error；
  - abort 调用了 abort()；
  - error 出现 error；
  - loadend 读取完成，无论成功还是失败；
- 结果：
  - reader.result 是结果（如果成功）；
  - reader.error 是 error（如果失败）；

```html
<input type="file" onchange="readFile(this)">

<script>
function readFile(input) {
  let file = input.files[0];

  let reader = new FileReader();

  reader.readAsText(file);

  reader.onload = function() {
    console.log(reader.result);
  };

  reader.onerror = function() {
    console.log(reader.error);
  };

}
</script>
```

> Note: 对于 `Web Worker`，还有一种同步的 `FileReader` 变体，称为 `FileReaderSync`；读取方法 `read*` 不会生成事件，而是会像常规函数那样返回一个结果；

#### Fetch

JavaScript 可以将网络请求发送到服务器，并在需要时加载新信息；fetch() 方法是一种现代通用的方法；

- 语法：
  - let promise = fetch(url, [options])
- 参数：
  - url 要访问的 URL；
  - options 可选参数：method，header 等；
- 响应：
  - status HTTP 状态码；
  - ok 布尔值，如果 HTTP 状态码为 200-299，则为 true；
- 方法：
  - response.text() 读取 response，并以文本形式返回 response；
  - response.json() 将 response 解析为 JSON；
  - response.formData() 以 FormData 对象的形式返回 response；
  - response.blob() 以 Blob（具有类型的二进制数据）形式返回 response；
  - response.arrayBuffer() 以 ArrayBuffer（低级别的二进制数据）形式返回 response；

没有 options fetch 就是一个简单的 GET 请求，下载 url 的内容；返回一个该调用代码应该用来获取结果的 promise；

获取响应分为两个阶段：第一阶段，当服务器发送了响应头（response header），fetch 返回的 promise 就使用内建的 Response class 对象来对响应头进行解析；第二阶段，需要使用其他的方法调用以获取 response body；

```js
// await 调用
let response = await fetch(url, options); // 解析 response header
let result = await response.json(); // 将 body 读取为 json
// 或者 promise
fetch(url, options)
  .then(response => response.json())
  .then(result => /* process result */)

// 案例
fetch("/index.html")
    .then((res) => {
        if (res.ok) {
            console.log(res);
        }
        return res;
    }).then((res) => {
        res.text().then((text) => {
            console.log(text);
        });
    });
```

> Note: 如果已经使用了 `response` 读取方法来获取 `response`，那么如果再用另一种 `response` 方法，则不会生效，因为 `body` 内容已经被处理过了；

Response header 位于 response.headers 中的一个类似于 Map 的 header 对象；但不是真正的 Map，只是具有类似的方法，可以按名称（name）获取各个 header，或迭代所有 header；

```js
let response = await fetch('https://api.github.com/repos/javascript-tutorial/en.javascript.info/commits');

// 获取一个 header
alert(response.headers.get('Content-Type')); // application/json; charset=utf-8

// 迭代所有 header
for (let [key, value] of response.headers) {
  alert(`${key} = ${value}`);
}
```

可以使用 headers 选项在 fetch 中设置 request header，它有一个带有输出 header 的对象；但有一些无法设置的 header，参见 [forbidden HTTP headers](https://fetch.spec.whatwg.org/#forbidden-header-name)；

```js
let response = fetch(protectedUrl, {
  headers: {
    Authentication: 'secret'
  }
});
```

通过设置选项中 method 可以创建一个 POST 或者其他类型的请求；此外选项中 body 字段会作为请求体发送，需是以下内容之一：

- 字符串；
- FormData 对象，以 form/multipart 形式发送数据；
- Blob/BufferSource 发送二进制数据；
- URLSearchParams，以 x-www-form-urlencoded 编码形式发送数据，很少使用；

```js
let user = {
  name: 'John',
  surname: 'Smith'
};

let response = await fetch('/article/fetch/post/user', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json;charset=utf-8'
  },
  body: JSON.stringify(user)
});

let result = await response.json();
alert(result.message);
```

> Note: 如果请求的 body 是字符串，则 Content-Type 会默认设置为 text/plain;charset=UTF-8；但要发送 JSON 时，会使用 headers 选项来发送 application/json，这是 JSON 编码的数据的正确的 Content-Type；

#### FormData

FormData 的特殊之处在于网络方法（network methods），例如 fetch 可以接受一个 FormData 对象作为 body；它会被编码并发送出去，带有 Content-Type: multipart/form-data；

- 创建：如果提供了 HTML form 元素，它会自动捕获 form 元素字段
let formData = new FormData([form])
- 方法：
  - formData.append(name, value) 添加具有给定 name 和 value 的表单字段；
  - formData.append(name, blob, fileName) 添加一个文件字段，相当于 `<input type="file">`，第三个参数 fileName 设置文件名；
  - formData.delete(name) 移除带有给定 name 的字段；
  - formData.get(name) 获取带有给定 name 的字段值；
  - formData.has(name) 如果存在带有给定 name 的字段，则返回 true，否则返回 false；
  - formData.set(name, value) 类似于 append，但移除所有具有给定 name 的字段，然后附加一个新字段；
  - formData.set(name, blob, fileName) 类似 append，同名字段值会被新值覆盖；

从技术上来讲，一个表单可以包含多个具有相同 name 的字段，因此，多次调用 append 将会添加多个具有相同名称的字段；而 set 方法确保了只有一个具有这种 name 的字段；

```html
<form id="formElem">
  <input type="text" name="name" value="John">
  <input type="text" name="surname" value="Smith">
  <input type="submit">
</form>

<script>
  formElem.onsubmit = async (e) => {
    e.preventDefault();

    let response = await fetch('/article/formdata/post/user', {
      method: 'POST',
      body: new FormData(formElem)
    });

    let result = await response.json();

    alert(result.message);
  };
</script>
```

表单始终以 Content-Type: multipart/form-data 来发送数据，这个编码允许发送文件；

#### Fetch：下载进度

fetch 方法允许去跟踪下载进度，但到目前为止，fetch 方法无法跟踪上传进度；

要跟踪下载进度，可以使用 response.body 属性，它是一个可以逐块（chunk）提供 body 的特殊的对象 ReadableStream；

```js
// 代替 response.json() 以及其他方法
const reader = response.body.getReader();

// 在 body 下载时，一直为无限循环
while(true) {
  // 当最后一块下载完成时，done 值为 true
  // value 是块字节的 Uint8Array
  const {done, value} = await reader.read();

  if (done) {
    break;
  }

  console.log(`Received ${value.length} bytes`)
}
```

其中 await reader.read() 调用的结果是一个具有两个属性的对象：
- done 当读取完成时为 true，否则为 false；
- value 字节的类型化数组：Uint8Array；

```js
// Step 1：启动 fetch，并获得一个 reader
let response = await fetch('https://api.github.com/repos/javascript-tutorial/en.javascript.info/commits?per_page=100');

const reader = response.body.getReader();

// Step 2：获得总长度（length）
const contentLength = +response.headers.get('Content-Length');

// Step 3：读取数据
let receivedLength = 0; // 当前接收到了这么多字节
let chunks = []; // 接收到的二进制块的数组（包括 body）
while(true) {
  const {done, value} = await reader.read();

  if (done) {
    break;
  }

  chunks.push(value);
  receivedLength += value.length;

  console.log(`Received ${receivedLength} of ${contentLength}`)
}

// Step 4：将块连接到单个 Uint8Array
let chunksAll = new Uint8Array(receivedLength); // (4.1)
let position = 0;
for(let chunk of chunks) {
  chunksAll.set(chunk, position); // (4.2)
  position += chunk.length;
}

// Step 5：解码成字符串
let result = new TextDecoder("utf-8").decode(chunksAll);

// 我们完成啦！
let commits = JSON.parse(result);
alert(commits[0].author.login);
```

#### Fetch：中止

有一个特殊的内建对象：AbortController；它不仅可以中止 fetch，还可以中止其他异步任务；

- 创建：
- let controller = new AbortController();
- 方法：
- abort() 中止异步操作；
- 属性：
- signal 可以在这个属性上设置事件监听器；

```js
let controller = new AbortController();
let signal = controller.signal;

// 可取消的操作这一部分
// 获取 "signal" 对象，
// 并将监听器设置为在 controller.abort() 被调用时触发
signal.addEventListener('abort', () => alert("abort!"));

// 另一部分，取消（在之后的任何时候）：
controller.abort(); // 中止！

// 事件触发，signal.aborted 变为 true
alert(signal.aborted); // true
```

为了能够取消 fetch，请将 AbortController 的 signal 属性作为 fetch 的一个可选参数（option）进行传递；fetch 方法知道如何与 AbortController 一起工作，它会监听 signal 上的 abort 事件；当一个 fetch 被中止，它的 promise 就会以一个 error AbortError reject，因此我们应该对其进行处理

```js
// 1 秒后中止
let controller = new AbortController();
setTimeout(() => controller.abort(), 1000);

try {
  let response = await fetch('/article/fetch-abort/demo/hang', {
    signal: controller.signal
  });
} catch(err) {
  if (err.name == 'AbortError') { // handle abort()
    alert("Aborted!");
  } else {
    throw err;
  }
}
```

AbortController 是可伸缩的，它允许一次取消多个 fetch；如果我们有自己的与 fetch 不同的异步任务，我们可以使用单个 AbortController 中止这些任务以及 fetch；

```js
let urls = [...];
let controller = new AbortController();

let ourJob = new Promise((resolve, reject) => { // 我们的任务
  ...
  controller.signal.addEventListener('abort', reject);
});

let fetchJobs = urls.map(url => fetch(url, { // fetches
  signal: controller.signal
}));

// 等待完成我们的任务和所有 fetch
let results = await Promise.all([...fetchJobs, ourJob]);

// 如果 controller.abort() 被从其他地方调用，
// 它将中止所有 fetch 和 ourJob
```

#### Fetch：跨源请求

可以使用 script 标签实现跨域，也成为 jsonp；

有两种类型的跨源请求：
- 所有其他请求；
- 简单的请求：
  - method 为 GET，POST 或 HEAD 类型；
  - 仅允许自定义这些 header：Accept，Accept-Language，Content-Language，Content-Type 的值为 application/x-www-form-urlencoded，multipart/form-data 或 text/plain；

当尝试发送一个非简单请求时，浏览器会发送一个特殊的“预检（preflight）”请求到服务器，询问服务器，是否接受此类跨源请求，并且，除非服务器明确通过 header 进行确认，否则非简单请求不会被发送；

如果一个请求是跨源的，浏览器始终会向其添加 Origin header；服务器可以检查 Origin，如果同意接受这样的请求，就会在响应中添加一个特殊的 header Access-Control-Allow-Origin；

对于跨源请求，默认情况下，JavaScript 只能访问“简单” response header：Cache-Control，Content-Language，Content-Type，Expires，Last-Modified，Pragma；访问任何其他 response header 都将导致 error；

可能仍然存在有些 Web 服务将非标准方法视为一个信号：“这不是浏览器”；因此，为了避免误解，任何“非标准”请求，浏览器不会立即发出在过去无法完成的这类请求；即在它发送这类请求前，会先发送“预检（preflight）”请求来请求许可；预检请求发生在“幕后”，它对 JavaScript 不可见；

预检请求使用 OPTIONS 方法，它没有 body，但是有两个 header：
- Access-Control-Request-Method header 带有非简单请求的方法；
- Access-Control-Request-Headers header 提供一个以逗号分隔的非简单 HTTP-header 列表；

如果服务器同意处理请求，那么它会进行响应，此响应的状态码应该为 200，没有 body，具有 header：
- Access-Control-Allow-Origin 必须为 * 或进行请求的源才能允许此请求；
- Access-Control-Allow-Methods 必须具有允许的方法；
- Access-Control-Allow-Headers 必须具有一个允许的 header 列表；
- header Access-Control-Max-Age 可以指定缓存此权限的秒数，因此，浏览器不是必须为满足给定权限的后续请求发送预检；

> Note: 服务器不应该忘记在主响应中添加 `Access-Control-Allow-Origin`，成功的预检并不能免除此要求；

默认情况下，由 JavaScript 代码发起的跨源请求不会带来任何凭据，cookies 或者 HTTP 认证（HTTP authentication）；这是因为具有凭据的请求比没有凭据的请求要强大得多，如果被允许，它会使用它们的凭据授予 JavaScript 代表用户行为和访问敏感信息的全部权力；

服务器必须显式地带有允许请求的凭据和附加 header，如果服务器同意接受 带有凭据 的请求，则除了 Access-Control-Allow-Origin 外，服务器还应该在响应中添加 header Access-Control-Allow-Credentials: true；对于具有凭据的请求，禁止 Access-Control-Allow-Origin 使用星号 *；

- [JavaScript 现代教程-Fetch：跨源请求](https://zh.javascript.info/fetch-crossorigin)

#### Fetch API

```js
let promise = fetch(url, {
  method: "GET", // POST，PUT，DELETE，等。
  headers: {
    // 内容类型 header 值通常是自动设置的
    // 取决于 request body
    "Content-Type": "text/plain;charset=UTF-8"
  },
  body: undefined // string，FormData，Blob，BufferSource，或 URLSearchParams
  referrer: "about:client", // 或 "" 以不发送 Referer header，
  // 或者是当前源的 url
  referrerPolicy: "no-referrer-when-downgrade", // no-referrer，origin，same-origin...
  mode: "cors", // same-origin，no-cors
  credentials: "same-origin", // omit，include
  cache: "default", // no-store，reload，no-cache，force-cache，或 only-if-cached
  redirect: "follow", // manual，error
  integrity: "", // 一个 hash，像 "sha256-abcdef1234567890"
  keepalive: false, // true keepalive 选项表示该请求可能会在网页关闭后继续存在
  signal: undefined, // AbortController 来中止请求
  window: window // null
});
```

- [JavaScript 现代教程-长轮询（Long polling）](https://zh.javascript.info/long-polling)

#### URL 对象

内建的 URL 类提供了用于创建和解析 URL 的便捷接口；URL 对象立即允许我们访问其组件，因此这是一个解析 url 的好方法；

- 创建 new URL(url, [base])：
  - url 完整的 URL，或者仅路径（如果设置了 base）；
  - base 可选的 base URL：如果设置了此参数，且参数 url 只有路径，则会根据这个 base 生成 URL；
- 组件：
  - href 是完整的 URL，与 url.toString() 相同；
  - protocol 以冒号字符 : 结尾；
  - search 以问号 ? 开头的一串参数；
  - hash 以哈希字符 # 开头；
- 方法：
  - append(name, value) 按照 name 添加参数；
  - delete(name) 按照 name 移除参数；
  - get(name) 按照 name 获取参数；
  - getAll(name) 获取相同 name 的所有参数；
  - has(name) 按照 name 检查参数是否存在；
  - set(name, value) set/replace 参数；
  - sort() 按 name 对参数进行排序，很少使用；

> Tips: 可以在 `fetch` 或 `XMLHttpRequest` 中使用 `URL` 对象，几乎可以在任何需要 `URL` 字符串的地方都能使用 `URL` 对象，大多数方法都会执行字符串转换，这会将 `URL` 对象转换为具有完整 `URL` 的字符串；

URL 对象会自动处理那些不被允许的字符必须被编码，用其 UTF-8 代码代替；

下面是用于编码/解码 URL 的内建函数：
- encodeURI 编码整个 URL；
- decodeURI 解码为编码前的状态；
- encodeURIComponent 编码 URL 组件，例如搜索参数，或者 hash，或者 pathname；
- decodeURIComponent 解码为编码前的状态；

> Note: `encodeURI` 仅编码 `URL` 中完全禁止的字符；`encodeURIComponent` 也编码这类字符，此外，还编码 `# $ & + , / : ; = ? @` 字符；因此对于一个 `URL` 整体，可以使用 `encodeURI`，对于 `URL` 参数，应该改用 `encodeURIComponent`；

> Note: `encode*` 与 `URL` 在对 IPv6 地址的编码方式不同，`URL` 可以正确编码；

#### XMLHttpRequest

XMLHttpRequest 是一个内建的浏览器对象，它允许使用 JavaScript 发送 HTTP 请求；XMLHttpRequest 有两种执行模式：同步（synchronous）和异步（asynchronous）；

- 创建：let xhr = new XMLHttpRequest();
- 初始化：xhr.open(method, URL, [async, user, password])
  - method HTTP 方法，通常是 "GET" 或 "POST"；
  - URL 要请求的 URL，通常是一个字符串，也可以是 URL 对象；
  - async 如果显式地设置为 false，那么请求将会以同步的方式处理；
  - user，password HTTP 基本身份验证（如果需要的话）的登录名和密码；
- 发送请求：xhr.send([body])；
- 监听响应：
  - load 当请求完成，并且响应已完全下载；
  - error 当无法发出请求，例如网络中断或者无效的 URL；
  - progress 在下载响应期间定期触发，报告已经下载了多少；
- 响应：
  - status HTTP 状态码（一个数字）：如果出现非 HTTP 错误，则为 0；
  - statusText HTTP 状态消息（一个字符串）；
  - response（旧脚本可能是 responseText）：服务器 response body；
- xhr 属性：
  - timeout 超时时间，在给定时间内请求没有成功执行，请求就会被取消，并且触发 timeout 事件；
  - responseType 响应类型，
    - "" 响应格式为字符串（默认）；
    - "text" 响应格式为字符串；
    - "arraybuffer" 响应格式为 ArrayBuffer；
    - "blob" 响应格式为 Blob；
    - "document" 响应格式为 XML document；
    - "json" 响应格式为 JSON（自动解析）；
  - readyState 请求处理状态，XMLHttpRequest 的状态会随着它的处理进度变化而变化；
    - 0 UNSENT 初始状态；
    - 1 OPENED open 被调用；
    - 2 HEADERS_RECEIVED 接收到 response header；
    - 3 LOADING 响应正在被加载，每接收到一个数据包会重复一次该状态；
    - 4 DONE 请求完成；
  - withCredentials true，将 cookie 和 HTTP 授权发送到其他域，默认 false；
  - abort() 方法，终止请求；会触发 abort 事件，且 xhr.status 变为 0；
  - setRequestHeader(name, value) 使用给定的 name 和 value 设置 request header；
  - getResponseHeader(name) 获取具有给定 name 的 header（Set-Cookie 和 Set-Cookie2 除外）；
  - getAllResponseHeaders() 返回除 Set-Cookie 和 Set-Cookie2 外的所有 response header；
- xhr 事件：上传需要专门用于跟踪上传事件的 xhr.upload 对象上监听；
  - readystatechange 事件，跟踪请求处理状态变化；
  - loadstart 请求开始；
  - progress 一个响应数据包到达，此时整个 response body 都在 response 中；
  - abort 调用 xhr.abort() 取消了请求；
  - error 发生连接错误，例如，域错误。不会发生诸如 404 这类的 HTTP 错误；
  - load 请求成功完成；
  - timeout 由于请求超时而取消了该请求（仅发生在设置了 timeout 的情况下）；
  - loadend 在 load，error，timeout 或 abort 之后触发；

```js
// 1. 创建一个 new XMLHttpRequest 对象
let xhr = new XMLHttpRequest();

// 2. 配置它：从 URL /article/.../load GET-request
xhr.open('GET', '/article/xmlhttprequest/example/load');

// 3. 通过网络发送请求
xhr.send();

// 4. 当接收到响应后，将调用此函数
xhr.onload = function() {
  if (xhr.status != 200) { // 分析响应的 HTTP 状态
    alert(`Error ${xhr.status}: ${xhr.statusText}`); // 例如 404: Not Found
  } else { // 显示结果
    alert(`Done, got ${xhr.response.length} bytes`); // response 是服务器响应
  }
};

xhr.onprogress = function(event) {
  if (event.lengthComputable) {
    alert(`Received ${event.loaded} of ${event.total} bytes`);
  } else {
    alert(`Received ${event.loaded} bytes`); // 没有 Content-Length
  }

};

xhr.onerror = function() {
  alert("Request failed");
};

// 旧代码中会使用 onreadystatechange 事件监听器
xhr.onreadystatechange = function() {
  if (xhr.readyState == 3) {
    // 加载中
  }
  if (xhr.readyState == 4) {
    // 请求完成
  }
};
```

如果在 open 方法中将第三个参数 async 设置为 false，那么请求就会以同步的方式进行；JavaScript 执行在 send() 处暂停，并在收到响应后恢复执行；

> Note: `XMLHttpRequest` 的另一个特点是不能撤销 `setRequestHeader`；一旦设置了 `header`，就无法撤销了。其他调用会向 `header` 中添加信息，但不会覆盖它；

```js
// 带有进度指示的文件上传
<input type="file" onchange="upload(this.files[0])">

<script>
function upload(file) {
  let xhr = new XMLHttpRequest();

  // 跟踪上传进度
  xhr.upload.onprogress = function(event) {
    console.log(`Uploaded ${event.loaded} of ${event.total}`);
  };

  // 跟踪完成：无论成功与否
  xhr.onloadend = function() {
    if (xhr.status == 200) {
      console.log("success");
    } else {
      console.log("error " + this.status);
    }
  };

  xhr.open("POST", "/article/xmlhttprequest/post/upload");
  xhr.send(file);
}
</script>
```

- [JavaScript 现代教程-可恢复的文件上传](https://zh.javascript.info/resume-upload)

#### WebSocket

WebSocket 协议提供了一种在浏览器和服务器之间建立持久连接来交换数据的方法，数据可以作为“数据包”在两个方向上传递，而不会断开连接和其他 HTTP 请求；

- 创建：let socket = new WebSocket("ws://javascript.info", ["soap", "wamp"]);
  - 第一个参数为服务器地址；
  - 第二个参数可选的子协议数组；
- 事件：
  - open 连接已建立；
  - message 接收到数据；
  - error WebSocket 错误；
  - close 连接已关闭；
- 请求头：
  - Origin 客户端页面的源，允许服务器决定是否使用 WebSocket 与该网站通信；
  - Connection: Upgrade 表示客户端想要更改协议；
  - Upgrade: websocket 请求的协议是 “websocket”；
  - Sec-WebSocket-Key 浏览器随机生成的安全密钥；
  - Sec-WebSocket-Version WebSocket 协议版本；
- 数据传输：WebSocket 通信由 frames（即数据片段）组成，可以从任何一方发送，并且有以下几种类型；
  - text frames 包含各方发送给彼此的文本数据；
  - binary data frames 包含各方发送给彼此的二进制数据；
  - ping/pong frames 被用于检查从服务器发送的连接，浏览器会自动响应它们；
  - connection close frame 以及其他服务 frames；
- 方法：
  - send(data) 可以发送文本或二进制数据；
  - socket.close([code], [reason]) 可以发送一个带有数字码（numeric code）和文本形式的原因的 “connection close frame”；
- 数字码；
  - 1000 默认，正常关闭，如果没有指明 code 时使用它；
  - 1006 没有办法手动设定这个数字码，表示连接丢失（没有 close frame）；
  - 1001 一方正在离开，例如服务器正在关闭，或者浏览器离开了该页面；
  - 1009 消息太大，无法处理；
  - 1011 服务器上发生意外错误；
- 连接状态：可以通过带有值的 socket.readyState 属性获取；
  - 0 CONNECTING：连接还未建立；
  - 1 OPEN：通信中；
  - 2 CLOSING：连接关闭中；
  - 3 CLOSED：连接已关闭；

> Note: `wss://` 是基于 TLS 的 WebSocket，类似于 HTTPS 是基于 TLS 的 HTTP，传输安全层在发送方对数据进行了加密，在接收方进行解密；因此，数据包是通过代理加密传输的，它们看不到传输的里面的内容，且会让这些数据通过；

> Note: WebSocket 对象是原生支持跨源的，没有特殊的 header 或其他限制；旧的服务器无法处理 WebSocket，因此不存在兼容性问题；如果服务器同意切换为 WebSocket 协议，服务器应该返回响应码 `101`；

在浏览器里，仅直接使用文本或二进制 frames；socket.send(body) 调用允许 body 是字符串或二进制格式，包括 Blob，ArrayBuffer 等；当我们收到数据时，文本总是以字符串形式呈现，对于二进制数据默认为 "blob"，因此二进制数据通常以 Blob 对象呈现，可以通过 socket.binaryType 属性设置；

> Note: 在网速受限的情况下，反复地调用 `socket.send(data)` 时，数据将会缓冲（储存）在内存中，并且只能在网速允许的情况下尽快将数据发送出去；`socket.bufferedAmount` 属性储存目前已缓冲的字节数，等待通过网络发送；

```js
// 一个简单的服务器端 WebSocket
import {
    WebSocketServer,
    WebSocket
} from 'ws';

const wss = new WebSocketServer({
    port: 8081
});

console.log("starting...")

wss.on('connection', function connection(ws) {
    ws.on('message', function incoming(data, isBinary) {
        console.log('received: %s', data);

        if (data == "online") {
            ws.send(wss.clients.size);
        } else {
            wss.clients.forEach(function each(client) {
                if (client !== ws && client.readyState === WebSocket.OPEN) {
                  client.send(data, { binary: isBinary });
                }
            });
        }
    });

    ws.send("update");
});
```

#### Server-Sent Events

Server-Sent Events 规范描述了一个内建的类 EventSource，它能保持与服务器的连接，并允许从中接收事件，与 WebSocket 类似，其连接是持久的；

| WebSocket	| EventSource |
| :----- | :----- |
| 双向：客户端和服务端都能交换消息 | 单向：仅服务端能发送消息 |
| 二进制和文本数据 | 仅文本数据 |
| WebSocket 协议 | 常规 HTTP 协议 |

- 创建：let source = new EventSource(url, [credentials]);
  - 第二个参数只有一个可选项 { withCredentials: true }，它允许发送跨源凭证；
- 事件：默认情况下 EventSource 对象生成三个事件：
  - message 收到消息，可以用 event.data 访问；
  - open 连接已打开；
  - error 无法建立连接，例如，服务器返回 HTTP 500 状态码；
- 属性：
  - lastEventId 最后接收到的 id；重新连接后，浏览器在 header Last-Event-ID 中发送此 id；
  - readyState 连接状态
    - 0 EventSource.CONNECTING 表示连接中或者重连中
    - 1 EventSource.OPEN 表示已连接
    - 2 EventSource.CLOSED 表示连接已关闭
- 响应：服务器发送由 \n\n 分隔的消息，一条消息可以按任何顺序包含一个或多个字段，但是 id: 通常排在最后；
  - data: 消息体（body），一系列多个 data 被解释为单个消息，各个部分之间由 \n 分隔；
  - id: 更新 lastEventId，重连时以 Last-Event-ID 发送此 id；
  - retry: 建议重连的延迟，以 ms 为单位。无法通过 JavaScript 进行设置；
  - event: 事件名，必须在 data: 之前；

服务器响应状态码应该为 200，header 为 Content-Type: text/event-stream，然后保持此连接并以一种特殊的格式写入消息；复杂的消息通常是用 JSON 编码后发送；

EventSource 支持跨源请求，就像 fetch 任何其他网络方法，可以使用任何 URL；远程服务器将会获取到 Origin header，并且必须以 Access-Control-Allow-Origin 响应来处理；

**重新连接**

如果与创建连接的服务器，连接断开则会自动重新连接，每次重新连接之间有一点小的延迟，默认为几秒钟；服务器可以使用 retry: 来设置需要的延迟响应时间（以毫秒为单位）；retry: 既可以与某些数据一起出现，也可以作为独立的消息出现；

如果服务器想要浏览器停止重新连接，那么它应该使用 HTTP 状态码 204 进行响应，如果浏览器想要关闭连接，则应该调用 eventSource.close()；当连接最终被关闭时，就无法“重新打开”它；如果想要再次连接，只需要创建一个新的 EventSource；

如果响应具有不正确的 Content-Type 或者其 HTTP 状态码不是 301，307，200 和 204，则不会进行重新连接；在这种情况下，将会发出 "error" 事件，并且浏览器不会重新连接；

**事件**

服务器可以在事件开始时使用 event: ... 指定另一种类型事件，此外还可以通过 addEventListener 监听自定义事件；

#### Cookie，document.cookie

Cookie 是直接存储在浏览器中的一小串数据，它们是 HTTP 协议的一部分，Cookie 通常是由 Web 服务器使用响应 Set-Cookie HTTP-header 设置的，然后浏览器使用 Cookie HTTP-header 将它们自动添加到（几乎）每个对相同域的请求中；

document.cookie 的值由 name=value 对组成，以 ; 分隔，每一个都是独立的 cookie；对 document.cookie 的写入操作只会更新其中提到的 cookie，而不会涉及其他 cookie；

> Note: 从技术上讲，`cookie` 的名称和值可以是任何字符，为了保持有效的格式，它们应该使用内建的 `encodeURIComponent` 函数对其进行转义；

> Note: 一个 cookie 最大为 4KB，每个域的 `cookie` 总数不得超过 `20+` 左右，具体限制取决于浏览器；

cookie 的其他选项：
- path url 路径前缀，该路径下的页面可以访问该 cookie，必须是绝对路径，默认为当前路径；
- domain 可访问 cookie 的域，无法使 cookie 可以被从另一个二级域访问，但其子域名可以访问；
- expire cookie 的到期日期，那时浏览器会自动删除它，日期必须完全采用 GMT 时区的这种格式；如果将 expires 设置为过去的时间，则 cookie 会被删除；
- max-age expires 的替代选项，具指明 cookie 的过期时间距离当前时间的秒数，如果为 0 或负数，则 cookie 会被删除；
- secure Cookie 应只能被通过 HTTPS 传输，默认情况下，cookie 是基于域的，不区分协议；
- samesite 用于防止 XSRF（跨网站请求伪造）攻击，samesite=strict 或 samesite 采用严格模式，samesite=lax 宽松模式；
- httpOnly 服务器端可以设置该选项禁止任何 JavaScript 访问 cookie；那么 document.cookie 看不到 cookie；

默认情况下，如果一个 cookie 没有设置 expire、max-age 这两个参数中的任何一个，那么在关闭浏览器之后，它就会消失，此类 cookie 被称为 "session cookie”

```js
// 返回具有给定 name 的 cookie，
// 如果没找到，则返回 undefined
function getCookie(name) {
  let matches = document.cookie.match(new RegExp(
    "(?:^|; )" + name.replace(/([\.$?*|{}\(\)\[\]\\\/\+^])/g, '\\$1') + "=([^;]*)"
  ));
  return matches ? decodeURIComponent(matches[1]) : undefined;
}

function setCookie(name, value, options = {}) {

  options = {
    path: '/',
    // 如果需要，可以在这里添加其他默认值
    ...options
  };

  if (options.expires instanceof Date) {
    options.expires = options.expires.toUTCString();
  }

  let updatedCookie = encodeURIComponent(name) + "=" + encodeURIComponent(value);

  for (let optionKey in options) {
    updatedCookie += "; " + optionKey;
    let optionValue = options[optionKey];
    if (optionValue !== true) {
      updatedCookie += "=" + optionValue;
    }
  }

  document.cookie = updatedCookie;
}

function deleteCookie(name) {
  setCookie(name, "", {
    'max-age': -1
  })
}

// 使用范例：
setCookie('user', 'John', {secure: true, 'max-age': 3600});
```

> Note: 如果一个脚本设置了一个 `cookie`，那么无论脚本来自何处这个 `cookie` 都属于当前网页的域；

> Note: 如果 `cookie` 是由用户所访问的页面的域以外的域放置的，则称其为第三方 `cookie`，通常用于跟踪和广告服务；

#### LocalStorage，sessionStorage

Web 存储对象 localStorage 和 sessionStorage 允许我们在浏览器上保存键/值对；

两个存储对象都提供相同的方法和属性：
- setItem(key, value) 存储键/值对；
- getItem(key) 按照键获取值；
- removeItem(key) 删除键及其对应的值；
- clear() 删除所有数据；
- key(index) 获取该索引下的键名；
- length 存储的内容的长度，只读；

localStorage 最主要的特点是：
- 在同源的所有标签页和窗口之间共享数据；
- 数据不会过期，它在浏览器重启甚至系统重启后仍然存在；
- 可以像使用一个普通对象那样，读取/设置键，且存储对象是不可迭代的；
- 键和值都必须是字符串；如果是任何其他类型，会被自动转换为字符串；

sessionStorage 特点：
- sessionStorage 的数据只存在于当前浏览器标签页，具有相同页面的另一个标签页中将会有不同的存储；但在同一标签页下的同源 iframe 之间是共享的；数据在页面刷- 新后仍然保留，但在关闭/重新打开浏览器标签页后不会被保留

> Note: 存储对象是不可迭代的；可以通过 Object.keys() 或者 for..in （需要 hasOwnProperty 检查）遍历；

当 localStorage 或 sessionStorage 中的数据更新后，storage 事件就会触发，但以类对象方式访问时，不会触发该事件；
- key 发生更改的数据的 key（如果调用的是 .clear() 方法，则为 null）；
- oldValue 旧值（如果是新增数据，则为 null）；
- newValue 新值（如果是删除数据，则为 null）；
- url 发生数据更新的文档的 url；
- storageArea 发生数据更新的 localStorage 或 sessionStorage 对象；

该事件会在所有可访问到存储对象的 window 对象上触发，导致当前数据改变的 window 对象除外；如果两个窗口都在监听 window.onstorage 事件，那么每个窗口都会对另一个窗口中发生的更新作出反应；

#### IndexedDB

IndexedDB 是一个浏览器内建的数据库：
- 通过支持多种类型的键，来存储几乎可以是任何类型的值；
- 支撑事务的可靠性；
- 支持键范围查询、索引；
- 和 localStorage 相比，它可以存储更大的数据量；

对于传统的客户端-服务器应用，这些功能通常是没有必要的；IndexedDB 适用于离线应用，可与 ServiceWorkers 和其他技术相结合使用；

创建连接：let openRequest = indexedDB.open(name, version);
- name 字符串，即数据库名称；
- version 一个正整数版本，默认为 1；

其他用法：
- 获取一个 promise 包装器，比如 idb；
- 打开一个数据库：idb.openDb(name, version, onupgradeneeded)
  - 在 onupgradeneeded 处理程序中创建对象存储和索引，或者根据需要执行版本更新；
- 对于请求：
  - 创建事务 db.transaction('books')（如果需要的话，设置 readwrite）；
  - 获取对象存储 transaction.objectStore('books')；
- 按键搜索，可以直接调用对象库上的方法，要按对象字段搜索，需要创建索引；
- 如果内存中容纳不下数据，可以使用光标；

- [JavaScript 现代教程-IndexedDB](https://zh.javascript.info/indexeddb)

#### CSS 动画

CSS 动画可以在不借助 Javascript 的情况下做出一些简单的动画效果；

CSS 提供了四个属性来描述一个过渡：
- transition-property 列举要设置动画的所有属性，all 表示应用在所有属性上，但不是所有的 CSS 属性都可以使用过渡动画；
- transition-duration 允许我们指定动画持续的时间，单位为s 或者 ms；
- transition-timing-function 时间函数描述了动画进程在时间上的分布，如果没有指定时间函数，那么将使用 ease 作为默认值；
- transition-delay 允许设定动画开始前的延迟时间；还可以提供一个负值，使动画从整个过渡的中间时刻开始渲染；

也可以在 transition 中以 property duration timing-function delay 的顺序一次性定义它们，并且可以同时为多个属性设置过渡动画；

**贝塞尔曲线**

时间函数可以用贝塞尔曲线描述，通过设置四个满足以下条件的控制点：
- 第一个应为：(0,0)。
- 最后一个应为：(1,1)。
- 对于中间值，x 必须位于 0..1 之间，y 可以为任意值

CSS 中设置一贝塞尔曲线的语法为：cubic-bezier(x2, y2, x3, y3)，只需要设置第二个和第三个值，因为第一个点固定为 (0,0)，第四个点固定为 (1,1)；

CSS 提供几条内建的曲线：
- linear cubic-bezier(0, 0, 1, 1)
- ease cubic-bezier(0.25, 0.1, 0.25, 1.0)
- ease-in cubic-bezier(0.42, 0, 1.0, 1.0)
- ease-out cubic-bezier(0, 0, 0.58, 1.0)
- ease-in-out cubic-bezier(0.42, 0, 0.58, 1.0)

贝塞尔曲线可以使动画超出其原本的范围；曲线上的控制点的 y 值可以使任意的：不管是负值还是一个很大的值；

**阶跃函数**

时间函数 steps(number of steps[, start/end]) 允许你让动画分段进行，number of steps 表示需要拆分为多少段

start 表示在动画开始时，我们需要立即开始第一段的动画，另一个值 end 表示：改变不应该在最开始的时候发生，而是发生在每一段的最后时刻；

CSS 动画完成后，会触发 transitionend 事件，该事件对象有几个特定的属性：
- event.propertyName ：当前完成动画的属性，这在同时为多个属性加上动画时会很有用；
- event.elapsedTime ：动画完成的时间（按秒计算），不包括 transition-delay；

此外还可以通过 CSS 提供的 @keyframes 规则整合多个简单的动画；

- [贝塞尔曲线在线制作工具](http://cubic-bezier.com/)

#### JavaScript 动画

标准动画时序提供了 requestAnimationFrame 函数；当页面在后台时，根本没有重绘，因此回调将不会运行：动画将被暂停并且不会消耗资源；

- 创建方式： let requestId = requestAnimationFrame(callback)；
- 取消回调：cancelAnimationFrame(requestId)；

这是一个设置大多数动画的 helper 函数 animate：
- duration 动画运行的总毫秒数；
- timing 计算动画进度的函数；获取从 0 到 1 的小数时间，返回动画进度，通常也是从 0 到 1；
- draw 绘制动画的函数；

```js
function animate({timing, draw, duration}) {

  let start = performance.now();

  requestAnimationFrame(function animate(time) {
    // timeFraction 从 0 增加到 1
    let timeFraction = (time - start) / duration;
    if (timeFraction > 1) timeFraction = 1;

    // 计算当前动画状态
    let progress = timing(timeFraction);

    draw(progress); // 绘制

    if (timeFraction < 1) {
      requestAnimationFrame(animate);
    }

  });
}
```

- [JavaScript 现代教程-JS 动画](https://zh.javascript.info/js-animation)

#### Custom elements

Custom elements 有两种：自主自定义标签和自定义内建元素；Custom element 在各浏览器中的兼容性已经非常好了，Edge 支持地相对较差，可以使用 [polyfill](https://github.com/webcomponents/webcomponentsjs)；

**Autonomous custom elements** 

自主自定义标签 “全新的” 元素, 继承自 HTMLElement 抽象类；

```js
class MyElement extends HTMLElement {
  constructor() {
    super();
    // 元素在这里创建
  }

  connectedCallback() {
    // 在元素被添加到文档之后，浏览器会调用这个方法
    //（如果一个元素被反复添加到文档／移除文档，那么这个方法会被多次调用）
  }

  disconnectedCallback() {
    // 在元素从文档移除的时候，浏览器会调用这个方法
    // （如果一个元素被反复添加到文档／移除文档，那么这个方法会被多次调用）
  }

  static get observedAttributes() {
    return [/* 属性数组，这些属性的变化会被监视 */];
  }

  attributeChangedCallback(name, oldValue, newValue) {
    // 当上面数组中的属性发生变化的时候，这个方法会被调用
  }

  adoptedCallback() {
    // 在元素被移动到新的文档的时候，这个方法会被调用
    // （document.adoptNode 会用到, 非常少见）
  }

  // 还可以添加更多的元素方法和属性
}

// 让浏览器知道我们新定义的类是为 <my-element> 服务的
customElements.define("my-element", MyElement);
```
> Note: 为了确保 `custom element` 和内建 HTML 元素之间不会发生命名冲突，`Custom element` 名称必须包括一个短横线 `-`；

如果浏览器在 customElements.define 之前的任何地方见到了 custom element 元素，并不会报错，但会把这个元素当作未知元素，就像任何非标准标签一样；:not(:defined) CSS 选择器可以对这样未定义的元素加上样式；

customElements.get(name) 返回指定 custom element name 的类；
customElements.whenDefined(name) 返回一个 promise，将会在这个具有给定 name 的 custom element 变为已定义状态的时候 resolve（不带值）；

在 HTML 解析器构建 DOM 的时候，会按照先后顺序处理元素，先处理父级元素再处理子元素；

**Customized built-in elements**

自定义内建元素继承内建的 HTML 元素，比如自定义 HTMLButtonElement 等；

```html
<script>
  // 类继承自 HTMLButtonElement
  // 这个按钮在被点击的时候说 "hello"
  class HelloButton extends HTMLButtonElement {
    constructor() {
      super();
      this.addEventListener('click', () => alert("Hello!"));
    }
  }

  // 给 customElements.define 提供定义标签的第三个参数
  customElements.define('hello-button', HelloButton, {extends: 'button'});
</script>

<!-- 插入一个普通的 <button> 标签，但添加 is="hello-button" 到这个元素 -->
<button is="hello-button">Click me</button>

<button is="hello-button" disabled>Disabled</button>
```

#### 影子 DOM

浏览器在内部使用 DOM/CSS 来绘制它们，这个 DOM 结构一般来说对我们是隐藏的，但可以在开发者工具里面看见它，在 #shadow-root 下看到的就是被称为「shadow DOM」的东西；

一个 DOM 元素可以有以下两类 DOM 子树：
- Light tree（光明树）一个常规 DOM 子树，由 HTML 子元素组成；
- Shadow tree（影子树）一个隐藏的 DOM 子树，不在 HTML 中反映，无法被察觉；

如果一个元素同时有以上两种子树，那么浏览器只渲染 shadow tree；影子树可以在自定义元素中被使用，其作用是隐藏组件内部结构和添加只在组件内有效的样式；

```html
<script>
customElements.define('show-hello', class extends HTMLElement {
  connectedCallback() {
    const shadow = this.attachShadow({mode: 'open'});
    shadow.innerHTML = `<p>
      Hello, ${this.getAttribute('name')}
    </p>`;
  }
});
</script>

<show-hello name="John"></show-hello>
```

调用 elem.attachShadow({mode: …}) 可以创建一个 shadow tree：
- mode 选项可以设定封装层级，他必须是以下两个值之一
  - open：shadow root 可以通过 elem.shadowRoot 访问；
  - closed：elem.shadowRoot 永远是 null；

浏览器原生的 shadow tree，是封闭的，没有任何方法可以访问它们；

Shadow DOM 元素对于 light DOM 中的 querySelector 不可见；实际上，Shadow DOM 中的元素可能与 light DOM 中某些元素的 id 冲突，这些元素必须在 shadow tree 中独一无二；Shadow DOM 有自己的样式，外部样式规则在 shadow DOM 中不产生作用；

#### 模板元素

内建的 `<template>` 元素用来存储 HTML 模板。浏览器将忽略它的内容，仅检查语法的有效性，但是我们可以在 JavaScript 中访问和使用它来创建其他元素；

```html
<template id="temp">
  <p>Hello, world.</p>
  <style>
    p { font-weight: bold; }
  </style>
  <script>
    alert("Hello");
  </script>
</template>

<script>
  let elem = document.createElement('div');

  // Clone the template content to reuse it multiple times
  elem.append(temp.content.cloneNode(true));

  document.body.append(elem);
  // Now the script from <template> runs
</script>
```

浏览器认为 `<template>` 的内容“不在文档中”：样式不会被应用，脚本也不会被执行；当内容插入文档时，该内容将变为活动状态（应用样式，运行脚本等）；

模板的 content 属性可看作 DocumentFragment，将它插入时，会被插入的则是其子节点；

#### Shadow DOM 插槽，组成

为了渲染 shadow DOM 中的每一个 `<slot name="...">` 元素，浏览器在 light DOM 中寻找相同名字的 slot="..."，这些元素在插槽内被渲染；最后得到一个扁平化（flattened）DOM；

扁平化 DOM 是通过插入插槽从 shadow DOM 派生出来的，浏览器渲染它并且用于样式继承、事件传播；但是 JavaScript 在扁平前仍按原样看到文档；slot="..." 属性仅仅对 shadow host 的直接子代有效，对于嵌套元素它将被忽略；

```js
<script>
customElements.define('user-card', class extends HTMLElement {
  connectedCallback() {
    this.attachShadow({mode: 'open'});
    this.shadowRoot.innerHTML = `
      <div>Name:
        <slot name="username"></slot>
      </div>
      <div>Birthday:
        <slot name="birthday"></slot>
      </div>
    `;
  }
});
</script>

<user-card>
  <span slot="username">John Smith</span>
  <span slot="birthday">01.01.2001</span>
</user-card>
```

shadow DOM 中第一个没有名字的 `<slot>` 是一个默认插槽，它从 light DOM 中获取没有放置在其他位置的所有节点；

如果添加/删除了插槽元素，浏览器将监视插槽并更新渲染，触发 slotchange 事件；

#### Shadow DOM 样式

:host 选择器允许选择 shadow 宿主（包含 shadow 树的元素）；:host(selector) 还可以添加选择器；

shadow 宿主驻留在 light DOM 中，因此它受到文档 CSS 规则的影响；如果在局部的 :host 和文档中都给一个属性设置样式，那么文档样式优先；

使用 ::slotted(selector) 伪类可以给插槽添加样式，但 ::slotted 选择器不能用于任何插槽中更深层的内容，且只能在 CSS 中使用，不能在 querySelector 中使用；

自定义 CSS 属性存在于所有层次，包括 light DOM 和 shadow DOM；

```html
<style>
  /* 在外部文档中声明此属性 */
  user-card {
    --user-card-field-color: green;
  }
</style>

<template id="tmpl">
  <style>
    .field {
      /* 如果 --user-card-field-color 没有被声明过，则取值为 black */
      color: var(--user-card-field-color, black);
    }
  </style>
  <div class="field">Name: <slot name="username"></slot></div>
  <div class="field">Birthday: <slot name="birthday"></slot></div>
</template>

<script>
customElements.define('user-card', class extends HTMLElement {
  connectedCallback() {
    this.attachShadow({mode: 'open'});
    this.shadowRoot.append(document.getElementById('tmpl').content.cloneNode(true));
  }
});
</script>

<user-card>
  <span slot="username">John Smith</span>
  <span slot="birthday">01.01.2001</span>
</user-card>
```

#### Shadow DOM 和事件

当事件在组件外部捕获时，shadow DOM 中发生的事件将会以 host 元素作为目标，这就是事件重定向；但如果事件发生在 slotted 元素上，实际存在于 light DOM 上，则不会发生重定向；

事件仅仅是在它们的 composed 标志设置为 true 的时候才能通过 shadow DOM 边界；这些事件仅能在同一 DOM 中的元素上捕获：mouseenter，mouseleave（也不冒泡），load，unload，abort，error，select，slotchange；

#### 正则表达式

在 JavaScript 中，正则表达式通过内建的“RegExp”类的对象来实现，并与字符串集成；

正则表达式包含模式和可选的修饰符，使用 RegExp 对象或 // 字面量创建；new RegExp 允许从字符串中动态地构造模式；

```js
let regexp = new RegExp("pattern", "i");

regexp = /pattern/; // 没有修饰符
regexp = /pattern/gmi;
```

字符串 str.search 方法，可以搜索一个正则表达式，并返回第一个索引；

在 JavaScript 中，有 6 个修饰符：
- i 使用此修饰符后，搜索时不区分大小写；
- g 使用此修饰符后，搜索时会查找所有的匹配项，而不只是第一个；
- m 多行模式；
- s 启用 dotall 模式，允许点 . 匹配换行符 \n；
- u 开启完整的 unicode 支持。该修饰符能够修正对于代理对的处理；
- y 粘滞模式；

**字符类**

字符类（Character classes） 是一个特殊的符号，匹配特定集中的任何符号；
- \d 表示数字 digit：从 0 到 9 的字符；
- \s 空格符号 space：包括空格，制表符 \t，换行符 \n 和其他少数稀有字符，例如 \v，\f 和 \r；
- \w 单字字符 word：拉丁字母或数字或下划线 _；
- \D 非数字：除 \d 以外的任何字符；
- \S 非空格符号：除 \s 以外的任何字符；
- \W 非单字字符：除 \w 以外的任何字符；
- . 点是一种特殊字符类，它与 “除换行符之外的任何字符” 匹配，带 s 标志时，匹配包括换行符在内的任何字符；
- \p{Letter} 或 \p{L} 表示任何语言中的一个字母，需要带 u 标志；

**Unicode**

Unicode 全称 Unicode Translation Format，又叫做统一码、万国码、单一码，Unicode 的实现方式（也就是编码方式）有很多种，常见的是 UTF-8、UTF-16、UTF-32 和 USC-2；UTF-8 是使用最广泛的 Unicode 编码方式，它是一种可变长的编码方式，可以是 1—4 个字节不等，它可以完全兼容 ASCII 码的 128 个字符；

默认情况下，正则表达式同样把一个 4 个字节的“长字符”当成一对 2 个字节长的字符；

主要的字符类别和它们对应的子类别：
- 字母（Letter） L:
  - 小写（lowercase） Ll
  - 修饰（modifier） Lm
  - 首字母大写（titlecase） Lt
  - 大写（uppercase） Lu
  - 其它（other） Lo
- 数字（Number） N:
  - 十进制数字（decimal digit） Nd
  - 十六进制 
  - 字母数字（letter number） Nl
  - 其它（other） No
- 标点符号（Punctuation） P:
  - 链接符（connector） Pc
  - 横杠（dash） Pd
  - 起始引用号（initial quote） Pi
  - 结束引用号（final quote） Pf
  - 开（open） Ps
  - 闭（close） Pe
  - 其它（other） Po
- 标记（Mark） M (accents etc):
  - 间隔合并（spacing combining） Mc
  - 封闭（enclosing） Me
  - 非间隔（non-spacing） Mn
- 符号（Symbol） S:
  - 货币（currency） Currency_Symbol/Sc
  - 修饰（modifier） Sk
  - 数学（math） Sm
  - 其它（other） So
- 分隔符（Separator） Z:
  - 行（line） Zl
  - 段落（paragraph） Zp
  - 空格（space） Zs
- 其它（Other） C:
  - 控制符（control） Cc
  - 格式（format） Cf
  - 未分配（not assigned） Cn
  - 私有（private use） Co
  - 代理伪字符（surrogate） Cs
- 书写系统：Script/sc
  - 中文 Han

```js
// 匹配标点
"。".search(/\p{P}/u); // 0

// 匹配
"number: xAF".search(/x\p{Hex_Digit}\p{Hex_Digit}/u); // 8

"你好".search(/\p{sc=Han}/u); // 0
```

> Note: Edge 和 Firefox 尚未实现 Unicode 属性 `p{…}`，可以使用库 [XRegExp](http://xregexp.com)；

**锚点**

插入符号 ^ 匹配文本开头，而美元符号 $ － 则匹配文本末尾，统称为“锚点”；

```js
let goodInput = "12:34";
let badInput = "12:345";

let regexp = /^\d\d:\d\d$/;
alert( regexp.test(goodInput) ); // true
alert( regexp.test(badInput) ); // false
```

对于空字符串 ""，正则表达式引擎将会首先匹配模式 ^（输入开始），匹配成功之后，会紧跟着检查模式 $，也匹配成功，所以空字符串是匹配 ^$ 的；

**多行模式**

通过 flag /.../m 可以开启多行模式；默认情况下，锚符 ^ 仅仅匹配文本的开头，在多行模式下，它匹配行的开头；锚符 $ 也可在多行模式下匹配行的结尾；

```js
let str = `1st place: Winnie
2nd place: Piglet
33rd place: Eeyore`;

alert( str.match(/^\d+/gm) ); // 1, 2, 33
alert( str.match(/\w+\n/gim) ); // Winnie\n,Piglet\n
```

**词边界**

当正则表达式引擎遇到 \b 时，它会检查字符串中的位置是否是词边界：

- 在字符串开头，如果第一个字符是单词字符 \w；
- 在字符串中的两个字符之间，其中一个是单词字符 \w，另一个不是；
- 在字符串末尾，如果最后一个字符是单词字符 \w；

```js
"Hello, Java!".match(/\b\w+\b/g); // ['Hello', 'Java']

alert( "1 23 456 78".match(/\b\d\d\b/g) ); // 23,78
alert( "12,34,56".match(/\b\d\d\b/g) ); // 12,34,56
```
> Note: `\b` 既可以用于单词，也可以用于数字，词边界 `\b` 不适用于非拉丁字母；

**转义字符**

`[ \ ^ $ . | ? * + ( )`，这些字符在正则表达式中有特殊的含义；如果要把特殊字符作为常规字符来使用，只需要在它前面加个反斜杠，这个过程叫转义；

斜杠符号 '/' 并不是一个特殊符号，但是它被用于在 Javascript 中开启和关闭正则匹配：/...pattern.../，所以我们也应该转义它；

```js
alert( "Chapter 5.1".match(/\d\.\d/) ); // 5.1
alert( "1\\2".match(/\\/) ); // '\'

// 斜杠 /
alert( "/".match(/\//) ); // '/'
alert( "/".match(new RegExp("/")) ); // '/'
```

在字符串中的反斜杠表示转义或者类似 \n 这种只能在字符串中使用的特殊字符；其它有些并没有特殊的含义，就像 \d 或者 \z，碰到这种情况的话会把反斜杠移除；所以传递一个字符串（参数）给 new RegExp 时，需要双反斜杠 \\，因为字符串引号会消费其中的一个；

```js
console.log("\d\.\d"); // d.d

let regStr = "\\d\\.\\d";
alert(regStr); // \d\.\d (correct now)

let regexp = new RegExp(regStr);

alert( "Chapter 5.1".match(regexp) ); // 5.1
```

**集合和范围**

在方括号 […] 中的几个字符或者字符类意味着“搜索给定的字符中的任意一个”；集合可以在正则表达式中和其它常规字符一起使用，方括号也可以包含字符范围；[a-z] 会匹配从 a 到 z 范围内的字母；

字符类是某些字符集的简写：
- \d 和 [0-9] 相同；
- \w 和 [a-zA-Z0-9_] 相同；
- \s 和 [\t\n\v\f\r ] 外加少量罕见的 unicode 空格字符相同；

除了普通的范围匹配，还有类似 [^…] 的“排除”范围匹配；通过在匹配查询的开头添加插入符号 ^ 来表示，它会匹配所有除了给定的字符之外的任意字符；且在方括号中的特殊字符不需要转义；

破折号 '-' 在方括号中有特殊含义，但这个含义只有当它位于其它字符之间而不是开头或结尾时才会发生作用，所以我们并不需要转义它；需要匹配 `^` 时，建议转义；

```js
// 并不需要转义
let reg = /[-().^+]/g;
alert( "1 + 2 - 3".match(reg) ); // 匹配 +，-

alert( '𝒳'.match(/[𝒳𝒴]/u) ); // 𝒳

let reg = /\d\d[-:]\d\d/g;
alert( "Breakfast at 09:00. Dinner at 21-30".match(reg) ); // 09:00, 21-30
```

如果集合中有代理对（surrogate pairs），则需要标志 u 以使其正常工作；

**量词**

最明显的量词便是一对引号间的数字：{n, m}；在一个字符（或一个字符类等等）后跟着一个量词，用来指出我们具体需要的数量；

- {n} 表示确切的位数；
- {n, m} 表示 n 到 m  之间的位数；
- {n,} 省略上限，表示查找位数大于或等于 n 的位数；

大多数常用的量词都可以有缩写：
- `+` 代表“一个或多个”，相当于 {1,}；
- ? 代表“零个或一个”，相当于 {0,1}；使得符号变得可选；
- * 代表着“零个或多个”，相当于 {0,}，表示这个字符可以多次出现或不出现；

```js
let str = "Should I write color or colour?";
alert( str.match(/colou?r/g) ); // color, colour

alert( "100 10 1".match(/\d0+/g) ); // 100, 10
alert( "100 10 1".match(/\d0*/g) ); // 100, 10, 1

// 正则表达式“浮点数”（带浮点的数字）：\d+\.\d+
// 正则表达式“打开没有属性的 HTML 标记”，比如 <span> 或 <p>：/<[a-z]+>/i
// 正则表达式“打开没有属性的HTML标记”（改进版）：/<[a-z][a-z0-9]*>/i
// 正则表达式“打开没有属性的HTML标记”：/<\/?[a-z][a-z0-9]*>/i
```

**贪婪量词和惰性量词**

在贪婪模式下（默认情况下），量词都会尽可能地重复多次；正则表达式引擎尝试用 .+ 去获取尽可能多的字符，然后再一步步地筛选它们

通过在量词之后添加一个问号 '?' 来启用懒惰模式，所以匹配模式变为 *? 或 +?，甚至将 '?' 变为 ??；懒惰模式只能够通过带 ? 的量词启用，其它的量词依旧保持贪婪模式；

有时需要运用集合和反向集合来解决懒惰模式下的一些问题；

```js
let str1 = '...<a href="link1" class="wrong">... <p style="" class="doc">...';
let str2 = '...<a href="link1" class="doc">... <a href="link2" class="doc">...';
let reg = /<a href="[^"]*" class="doc">/g;

// Works!
alert( str1.match(reg) ); // 没有匹配项，是正确的
alert( str2.match(reg) ); // <a href="link1" class="doc">, <a href="link2" class="doc">

// 匹配注释
let reg = /<!--[\s\S]*?-->/g;
let str = `... <!-- My -- comment
 test --> ..  <!----> ..
`;
alert( str.match(reg) ); // '<!-- My -- comment \n test -->', '<!---->'

// 匹配标签 
let reg = /<[^<>]+>/g;
let str = '<> <a href="/"> <input type="radio" checked> <b>';
alert( str.match(reg) ); // '<a href="/">', '<input type="radio" checked>', '<b>'
```

- [JavaScript 现代教程-贪婪量词和惰性量词](https://zh.javascript.info/regexp-greedy-and-lazy)

**捕获组**

模式的一部分可以用括号括起来 (...)，这称为“捕获组（capturing group）”；这允许将匹配的一部分作为结果数组中的单独项，如果我们将量词放在括号后，则它将括号视为一个整体；


```js
// 匹配域名
let regexp = /([\w-]+\.)+\w+/g;
alert( "site.com my.site.com".match(regexp) ); // site.com,my.site.com
```

方法 str.match(regexp)，如果 regexp 没有 g 标志，将查找第一个匹配并将它作为一个数组返回；
- 在索引 0 处：完全匹配；
- 在索引 1 处：第一个括号的内容；
- 在索引 2 处：第二个括号的内容；
- ...

括号可以嵌套；在这种情况下，编号也从左到右，零索引始终保持完全匹配；即使组是可选的并且在匹配项中不存在，也存在相应的 result 数组项，并且等于 undefined；

```js
let str = '<h1>Hello, world!</h1>';
let tag = str.match(/<(.*?)>/);

console.log(tag); //  ['<h1>', 'h1', index: 0, input: '<h1>Hello, world!</h1>', groups: undefined]
alert( tag[0] ); // <h1>
alert( tag[1] ); // h1
```

当搜索所有匹配项（标志 g）时，match 方法不会返回组的内容，结果是一个匹配数组，但没有每个匹配项的详细信息；使用方法 str.matchAll(regexp) 进行搜索并获取捕获组的内容；

matchAll 和 match 有三个区别：
- 它返回的不是数组，而是一个可迭代的对象；
- 当标志 g 存在时，它将每个匹配组作为一个数组返回；
- 如果没有匹配项，则不返回 null，而是返回一个空的可迭代对象；

```js
let results = '<h1> <h2>'.matchAll(/<(.*?)>/gi);

// results - is not an array, but an iterable object
alert(results); // [object RegExp String Iterator]

alert(results[0]); // undefined (*)

results = Array.from(results); // let's turn it into array

alert(results[0]); // <h1>,h1 (1st tag)
alert(results[1]); // <h2>,h2 (2nd tag)
```

命名组是通过在开始括号之后立即放置 `?<name>` 来完成的；

```js
let dateRegexp = /(?<year>[0-9]{4})-(?<month>[0-9]{2})-(?<day>[0-9]{2})/g;

let str = "2019-10-30 2020-01-01";

let results = str.matchAll(dateRegexp);

for(let result of results) {
  let {year, month, day} = result.groups;

  alert(`${day}.${month}.${year}`);
  // 第一个 alert：30.10.2019
  // 第二个：01.01.2020
}
```

方法 str.replace(regexp, replacement) 用 replacement 替换 str 中匹配 regexp 的所有捕获组；使用 $n 或者名称 `$<name>`；

```js
let str = "John Bull";
let regexp = /(\w+) (\w+)/;
alert( str.replace(regexp, '$2, $1') ); // Bull, John

let regexp = /(?<year>[0-9]{4})-(?<month>[0-9]{2})-(?<day>[0-9]{2})/g;
let str = "2019-10-30, 2020-01-01";
alert( str.replace(regexp, '$<day>.$<month>.$<year>') );
// 30.10.2019, 01.01.2020
```

可以通过在开头添加 ?: 来排除组；

```js
let str = "Gogogo John!";

// ?: 从捕获组中排除 'go'
let regexp = /(?:go)+ (\w+)/i;

let result = str.match(regexp);

alert( result[0] ); // Gogogo John（完全匹配）
alert( result[1] ); // John
alert( result.length ); // 2（数组中没有更多项）
```

**模式中的反向引用**

我们不仅可以在结果或替换字符串中使用捕获组 (...) 的内容，还可以在模式本身中使用它们；

可以使用 \N 在模式中引用一个组，其中 N 是组号；\1 在模式中进一步的含义是“查找与第一（捕获）分组相同的文本”；

如果正则表达式中有很多括号对（注：捕获组），给它们起个名字方便引用；要引用命名组，我们可以使用：`\k<name>`；

```js
let str = `He said: "She's the one!".`;

let regexp = /(?<quote>['"])(.*?)\k<quote>/g;

alert( str.match(regexp) ); // "She's the one!"
```

**选择（OR）|**

通常用圆括号把模式中的选择部分括起来，像这样 before(XXX|YYY)after；

```js
let reg = /([01]\d|2[0-3]):[0-5]\d/g;

alert("00:00 10:10 23:59 25:99 1:2".match(reg)); // 00:00,10:10,23:59
```

**环视断言**

一般来说，环视断言括号中（前瞻和后瞻的通用名称）的内容不会成为匹配到的一部分结果，如果想要捕捉整个环视表达式或其中的一部分，只需要将其包裹在另加的括号中；

| 模式 | 类型 | 匹配 |
| :----- | :----- | :----- |
| x(?=y) | 前瞻肯定断言 | x ，仅当后面跟着 y |
| x(?!y) | 前瞻否定断言 | x ，仅当后面不跟 y |
| (?<=y)x	| 后瞻肯定断言 | x ，仅当跟在 y 后面 |
| (?<!y)x	| 后瞻否定断言 | x ，仅当不跟在 y 后面 |

```js
let str = "1 turkey costs 30€";
let reg = /\d+(?=(€|kr))/; // €|kr 两边有额外的括号
alert( str.match(reg) ); // 30, €

let str = "1 turkey costs $30";
let reg = /(?<=(\$|£))\d+/;
alert( str.match(reg) ); // 30, $

// 匹配非负数
let regexp = /(?<![-\d])\d+/g;
let str = "0 12 -5 123 -18";
alert( str.match(regexp) ); // 0, 12, 123

// 在 body 后插入，在替换字符串中，$& 表示匹配本身
let str = '...<body style="...">...';
str = str.replace(/(?<=<body.*?>)/, `<h1>Hello</h1>`);
str = str.replace(/<body.*?>/, '$&<h1>Hello</h1>');
```

**灾难性回溯**

解决方式：
- 第一种重写正则表达式，尽可能减少其中排列组合的数量；
- 另一种使用前瞻断言禁止量词的回溯；

```js
alert( "JavaScript".match(/\w+Script/)); // JavaScript
alert( "JavaScript".match(/(?=(\w+))\1Script/)); // null

// 括号被命名为 ?<word>，使用 \k<word> 来引用
let regexp = /^((?=(?<word>\w+))\k<word>\s?)*$/;
let str = "An input string that takes a long time or even makes this regex to hang!";

alert( regexp.test(str) ); // false
alert( regexp.test("A correct string") ); // true
```

**粘性标志 "y"**

y 标志允许在源字符串中的指定位置执行搜索；

方法 regexp.exec(str)：
- 如果 regexp 没有标志 g 或 y，那么这个方法就可以寻找字符串 str 中的第一个匹配，就像 str.match(regexp) 一样；
- 如果有标志 g，那么它就会在字符串 str 中执行搜索，从存储在 regexp.lastIndex 属性中的位置开始，如果发现匹配，则将 regexp.lastIndex 设置为匹配后的索引；
- 如果正则表达式带有标记 y，则搜索将精确地在 regexp.lastIndex 位置执行，不会再继续了；

**正则表达式（RegExp）和字符串（String）的方法**

- str.match(regexp) 方法在字符串 str 中找到匹配 regexp 的字符；
  - 如果 regexp 不带有 g 标记，则以数组的形式返回第一个匹配项，其中包含分组和属性 index（匹配项的位置）、input（输入字符串，等于 str）；
  - 如果 regexp 带有 g 标记，则它将所有匹配项的数组作为字符串返回，而不包含分组和其他详细信息；
  - 如果没有匹配项，则无论是否带有标记 g ，都将返回 null；
- str.matchAll(regexp) 方法 str.matchAll(regexp) 是 str.match “新改进的”变体；
  - 它返回包含匹配项的可迭代对象，而不是数组。可以用 Array.from 从中得到一个常规数组；
  - 每个匹配项均以包含分组的数组形式返回（返回格式与不带 g 标记的 str.match 相同）；
  - 如果没有结果，则返回的不是 null，而是一个空的可迭代对象；
- str.split(regexp|substr, limit) 可以使用正则表达式（或子字符串）作为分隔符来分割字符串；
- str.search(regexp) 方法返回第一个匹配项的位置，如果未找到，则返回 -1；
- str.replace(str|regexp, str|func) 用于搜索和替换的通用方法；
  - 当第一个参数是字符串时，它仅替换第一个匹配项；
  - 第二个参数是一个替代字符串，可以在其中使用特殊字符：
    - $&	插入整个匹配项；
    - $`	在匹配项之前插入字符串的一部分；
    - $'	在匹配项之后插入字符串的一部分；
    - $n	插入第 n 个分组的内容；
    - `$<name>`	插入带有给定 name 的括号内的内容；
    - `$$`	插入字符 $；
  - 第二个参数可以是一个函数：
  - func(match, p1, p2, ..., pn, offset, input, groups) 带参数调用：
    - match 匹配项；
    - p1, p2, ..., pn 分组的内容（如有）；
    - offset 匹配项的位置；
    - input 源字符串；
    - groups 所指定分组的对象；
  - 如果正则表达式中没有括号，则只有 3 个参数：func(str, offset, input)
- regexp.exec(str) 方法返回字符串 str 中的 regexp 匹配项；
- regexp.test(str) 方法查找匹配项，然后返回 true/false 表示是否存在；
  - 如果正则表达式带有标记 g，则 regexp.test 从 regexp.lastIndex 属性中查找，并更新此属性，就像 regexp.exec 一样；
  - 如果我们在不同的源字符串上应用相同的全局表达式，可能会出现错误的结果，因为 regexp.test 的调用会增加 regexp.lastIndex 属性值，因此在另一个字符串中的搜索可能是从非 0 位置开始的

- [JavaScript 现代教程-正则表达式（RegExp）和字符串（String）的方法](https://zh.javascript.info/regexp-methods)