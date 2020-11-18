# 函数的扩展

## 函数参数的默认值

ES6以前函数参数是不能有默认值的，而ES6中对这个规则进行了修改了，函数参数也可以带默认参数。即直接写在参数定义的后面。

```javascript
function log(x, y = 'World') {
    console.log(x, y);
}

log('Hello') // Hello World
log('Hello', 'China') // Hello China
log('Hello', '') // Hello
```

这样的写法有两个好处：一个是阅读代码的人可以立刻意识到哪些参数是可以省略的，不需要查看函数体；其次有利于代码将来的优化。即便在未来的版本对外接口中，彻底拿掉这个参数也不会导致原有代码无法运行。

### 结合解构赋值

参数默认值当然可以使用前面章节提到的解构特性进行赋值了。

```javascript
function foo({x, y = 5}) {
    console.log(x, y);
}

foo({}) // undefined, 5
foo({x: 1}) // 1, 5
foo({x: 1, y: 2}) // 1, 2
foo() // Error
```

### 参数默认值的位置

一般情况下定义了默认值的参数应该是函数的尾部，这样比较容易看出来，如果是非尾部的参数设置默认值，实际上该参数是没法省略的。

```javascript
function (x = 1, y) {
    return [x, y];
}

f() // [1, undefined]
f(2) // [2, undefined]
f(, 1) // Error
f(undefined, 1) // [1, 1]

function (x, y = 5, z) {
    return [x, y, z];
}
f() // [undefined, 5, undefined]
f(1) // [1, 5, undefined]
f(1, ,2) // Error
f(1, undefinied, 2) // [1, 5, 2]
```

### length属性

函数的length属性将返回没有指定默认值的参数个数。也就是说：指定了默认值后，length属性将失真；

```javascript
(function (a) {}).length // 1
(function (a = 5) {}).length // 0
(function (a, b, c = 5) {}).length // 2
```

如果设置了默认值的参数不是尾参数，那么length属性也不再记入后面的参数了。

```javascript
(function (a = 0, b, c) {}).length // 0
(function (a, b = 1, c) {}).length // 1
```

### 作用域

如果参数默认值是一个变量，则该变量所处的作用域与其他变量的作用域规则是一样的。即先是当前函数的作用域，然后才是全局作用域。

```javascript
var x = 1;

function f(x, y = x) {
    console.log(y);
}

f(2) // 2
```

如果参数的默认值是一个函数，该函数的作用域是其声明时所在的作用域

```javascript
let foo = 'outer';

// func为一个默认匿名函数，返回值为变量foo
function bar(func = x => foo) {
    let foo = 'inner';
    console.log(func());
}

bar(); // outer
```

### 默认参数的用途一目了然

利用默认参数值，可以指定某一个参数不得省略，一旦省略就抛出一个错误；如果想省略这个参数，只要设置成`undefined`就行了。

```javascript
function throwIfMissing() {
    throw new Error('Missing parameters');
}

function foo(mustBeProvided = throwIfMissing()) {
    return mustBeProvided;
}

foo(); // Error: Missing Parameters
```

### rest参数

顾名思义：就是入参的个数是不确定的，跟Java中的多参数语法类似。这里就不再赘述了。

```javascript
function add(...values) {
    let sum = 0;
    
    for (var val of values) {
        sum += val;
    }
    
    return sum;
}

add(1, 2, 3, 4) // 10
```

## 扩展运算符

扩展运算符为三个点`...`，是将一个数组转化为用逗号分隔的参数序列，是rest参数的逆运算。

```javascript
console.log(...[1, 2, 3]) // 1 2 3

console.log(1, ...[2, 3, 4], 5) // 1 2 3 4 5

[...document.querySelectorAll('div')] // [<div>, <div>, <div>]
```

该运算符主要用于函数调用

```javascript
function push(array, ...items) {
    array.push(...items);
}

function add(x, y) {
    return x + y;
}

var numbers = [4, 38];
add(...numbers) // 42
```

### 扩展运算符的应用

#### （1）合并数组

```javascript
var arr1 = ['a', 'b'];
var arr2 = ['c'];
var arr3 = ['e', 'f'];

[...arr1, ...arr2, ...arr3] // ['a', 'b', 'c', 'e', 'f']
```

#### （2） 与解构赋值结合

```javascript
a = list[0], rest = list.slice(1)

[a, ...rest] = list

const [first, ...rest] = [1, 2, 3, 4, 5];
first // 1
rest // [2, 3, 4, 5]

const [first, ...rest] = []
first // undefined
rest // []

const [first, ...rest] = ['foo']
first // foo
rest // []
```

#### （3）函数返回值

JavaScript的函数只能返回一个值，如果需要返回多个值，只能返回数组或对象。扩展运算符提供了解决这个问题的一种办法：

```javascript
var dateFields = readDateFields(database);
var d = new Date(...dateFields);
```

#### （4）字符串

可以将字符串转换为真正的数组。因为扩展运算符可以很好地识别Unicode字符，因此最好都用扩展运算符；

```javascript
[...'hello']
// ['h', 'e', 'l', 'l', 'o']
```

#### （5）实现了Iterator接口的对象

任何Iterator接口的对象，都可以用扩展运算符转换为真正的数组，之前已经提过了，这里不再赘述

#### （6）Map和Set结构、Generator函数

承接第（5）点，Map和Set也部署了Iterator接口，所以也可以使用扩展运算符，Generator函数运行后返回的是一个遍历器对象，因此也可以使用扩展运算符。

```javascript
var go = function* () {
    yield 1;
    yield 2;
    yield 3;
};
[...go()] // [1, 2, 3]
```

## 严格模式

## 箭头函数

## 绑定this

## 尾调用优化

## 函数参数的尾逗号