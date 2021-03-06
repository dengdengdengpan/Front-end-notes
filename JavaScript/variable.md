# 变量

### 认识变量

在 JavaScript 中，变量是指**用来保存任意值的命名占位符，变量不是值本身，它仅仅是一个用于存储任意值的容器**。比如，可以把变量想象成一个个装东西的盒子，盒子外边有一个唯一标注盒子名称的贴纸，有如下代码：

```javascript
let a = 'Bob';
let b = true;
let c = 35;
```

想象变量 `a` 是一个贴着“a“贴纸的盒子，盒子里面放着字符串值 `Bob`；变量 `b` 是一个贴着“b“贴纸的盒子，盒子里面放着布尔值 `ture`；变量 `c` 是一个贴着“c“贴纸的盒子，盒子里面放着数值 `35`，如下图：

![容器](./imgs/variable-box.png)

我们可以往盒子里面放入任何值。同时，由于 JavaScript 中的变量具有**松散类型**的特点（即变量的值和值的数据类型在脚本的生命周期内都可以改变），所以可以多次更改盒子里面的值及其类型。比如：

```javascript
var a = 10;
a = 25; // 变量 a 的值从数值 10 改变为数值 25
a = true; // 变量 a 的数据类型从 Number 改变为 Boolean
```

### 变量命名

变量作为标识符之一，其命名需要遵守一定的规则：

- 变量名只能包含字母、数字、下划线（_）或美元符号（$），且不能以数字开头。
- 关键字、保留字、`true`、`false`、`null` 不能用作变量名。
- 由于 JavaScript 使用了 **Unicode** 字符集，所以可以使用 Unicode 中的字母字符。比如，中文也是合法的变量名。

下面是一些变量命名的示例：

```javascript
// 合法的变量命名
let name;
let _this;
let $attr;
let 临时变量; // 中文变量名合法，但不推荐
let Früh; // 德文变量名合法，但不推荐
let имя；// 合法，但不推荐

// 不合法的变量命名
let 9age; // 不能以数字开头
let age*; // 不包含 *
let new; // 不能使用关键字
let enum; //不能使用保留字
let null; // 不能使用 null
```

如果变量命名中包含多个单词，通常采用**驼峰式**命名法，即第一个单词首字母小写，后面每个单词首字母大写：

```javascript
let carType;
let userName;
let deviceStatusList;
```

### 声明变量

在 JavaScript 中使用变量，首先需要创建它——即声明一个变量，而**声明变量的方式有三种：分别是使用 `var`、`let`、`const` 后接变量名的方式**。

#### var

##### 使用方式

使用 `var` 关键字可以声明一个变量，并可选地将其初始化为一个值：

```javascript
var name = 'lufei';
```

上述代码声明了一个名为 `name` 的变量，并且初始化时赋值了字符串值 `lufei`，它的实际步骤是下面这样的：

```javascript
var name; // var 关键字告诉 JS 引擎，要声明一个变量 name
name = 'lufei'; // 通过赋值操作符 = ，变量 name 就保存了将字符串值 lufei
```

如果只是声明变量而没有初始化，则变量会保存一个特殊值 `undefined`。

```javascript
var message;
console.log(message); // undefined
```

使用 `var` 可以多次声明同一个变量，变量的值会取排在后面的：

```javascript
var a = 10;
// ...
var a = 50;
console.log(a); // 50
```

如果需要定义多个变量，可以在一条语句中用逗号分隔每个变量（及可选地初始化）：

```javascript
var a,
    b = 10,
    c = false,
    d = [1, 2, 3];
```

##### 作用域

**使用 `var` 声明的变量的作用域是它当前的执行上下文**：

- 在函数外声明的变量会成为全局变量（会成为全局对象的一个属性），全局变量在当前执行环境下都能被访问。

  ```javascript
  var b = 2;
  console.log(window.b); // 2
  function test() {
    console.log(b);
  }
  test(); // 2
  ```

- 在函数内声明的变量会成为包含它的函数的局部变量，该变量只在该函数内部能够被访问。

  ```javascript
  function test() {
    var a = 1; // 局部变量
    console.log(a); 
  }
  test(); // 1
  console.log(a); // Uncaught ReferenceError: a is not defined
  ```

  不过，如果在函数内声明变量时省略 `var` 关键字，并且函数内也无该变量，那么该变量会被隐式地创建为全局变量。

  ```javascript
  // 函数内无该变量
  function test() {
    c = 3;
  }
  test();
  console.log(c); // 3
  console.log(window.c); // 3
  
  // 函数内有该变量
  function foo() {
    var d;
    function bar() {
      d = 2; // d 是外边 foo 函数里的局部变量 d
    }
    bar();
  }
  foo();
  console.log(window.d); // undefined
  ```

##### 变量提升

使用 `var` 声明的变量无论出现在作用域的什么地方，都将在代码被执行前**首先**进行处理，可以形象地将这个过程想象成变量会被"移动"到了各自作用域的最顶端，这个过程被称为**变量提升**。如下代码：

```javascript
console.log(name); // undefined
var name = 'lufei';
```

以上代码没有报错并在控制台打印出了 `undefined`，是因为它们被执行时等价成下面代码：

```javascript
// 假设这里是当前作用域顶部
var name; // 把 var 声明的变量移动到当前作用域顶部
console.log(name); // 执行打印，但此时变量 name 还未进行赋值操作，所以保存的是特殊值 undefined
name = 'lufei'; // 对变量 name 进行赋值操作
```

这样就产生了“变量提升”，但实际上变量在代码中的位置是不会被移动的，所以这里的“提升”并非字面意思。要理解“提升”需要一些背景知识：

> JavaScript 作为一种即时编译型的编程语言，任何的 JavaScript 代码片段在执行前都要进行编译——大部分情况编译就发生在代码执行前的几微秒（甚至更短）的时间内。

因此，**正确的思考思路是，`var` 声明的变量会在代码被执行前提前被编译器处理**。对于 `var name = 'lufei';` 这个语句，JavaScript 会将其看作两个部分：

```javascript
var name;
name = 'lufei';
```

其中，第一部分是在**编译阶段**进行的；第二部分则会被留到原地等待**执行阶段**。即**只有声明本身会被提升，赋值操作或其它运行逻辑会留在原地**，这样就形成了“提升”的效果。

另外值得注意的是，`var` 声明在每个作用域中都会进行提升操作。所以除了前面的全局作用域，在函数产生的作用域中也会对 `var` 声明进行提升：

```javascript
function fn() {
  console.log(name);
  var name = 'lufei';
}
fn();
```

以上代码会被理解成下面的形式：

```javascript
function fn() {
  var name; // 将 var 声明提升到当前函数作用域的最顶部
  console.log(name); // 此时 name 还没有被赋值，所以保存了特殊值 undefined
  name = 'lufei';
}
fn();
```

#### let

##### 使用方式

使用 `let` 可以声明一个拥有**块级作用域**的变量，并可选地是否将其初始化为一个值：

```javascript
let name;
```

上述代码只声明了变量 `name` 没有进行初始化，则变量 `name` 会保存一个特殊值 `undefined`。如果要使用 `let` 声明多个变量，使用逗号分隔（并可选是否进行初始化）：

```javascript
let name = 'lufei',
    age = 18,
    school,
    gender = 'male';
```

##### 不可重复声明

`let` 在一个作用域中重复声明同一个变量会引起 `SyntaxError` 报错：

```javascript
var name = 'lufei';
let name = 'test'; // Uncaught SyntaxError: Identifier 'name' has already been declared

let age = 18;
let age = 22; // Uncaught SyntaxError: Identifier 'age' has already been declared

let x = 1;
switch(x) {
  case 0:
    let foo;
    break;
  case 1:
    let foo; // Uncaught SyntaxError: Identifier 'foo' has already been declared
    break;
  default:
    let bar;
}
```

##### 作用域规则

**`let` 声明的变量的作用域在离其最近的花括号中，并且只能在当前块或子块中可用**，如下代码：

```javascript
function test() {
  let a = 1;
  {
    let a = 6; // 这个块中的变量 a 和在外边的变量 a 是两个不同的变量
    console.log(a); // 6
  }
  console.log(a); // 1
}
```

##### 没有变量提升

使用 `let` 声明的变量不会有提升，所以要先声明后使用，否则会报错：

```javascript

```



