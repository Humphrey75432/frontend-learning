# Generator函数

## 基本概念

Generator函数是ES6提供的一种异步编程解决方案，语法行为与传统函数完全不同。

Generator函数有多种理解角度：从语法上：可以将其理解成状态机，封装了多个内部状态；其次：执行Generator函数会返回一个遍历器对象。

形式上，Generator函数是个普通函数，但是有两个特征。一是`function`关键字和函数名之间多了一个星号；二是函数体内部使用了`yield`语句，定义了不同的内部状态。

```javascript
function* hello() {
    yield 'hello';
    yield 'world';
    return 'ending';
}

var hello = hello();
hello.next() // hello - 1
hello.next() // world - 2
hello.next() // ending - 3
hello.next() //undefined - 4
```

## yield语句

可以看出，只有显式调用next()方法才会继续遍历到下一个状态，所以其实提供了一种可以暂停执行的函数。其中yield就是暂停标识。如果Generator函数不使用yield语句，这时就变成了一个单纯地暂缓执行函数。注意：yield不能用在普通函数中。

```javascript
function* f() {
    console.log('Executed!');
}

var generator = f();
setTimeout(function() {
    generator.next();
}, 2000);
```

## 与Iterator接口的关系

由于Generator函数就是遍历器生成的函数，因此可以把Generator赋值给对象的Symbol.iterator属性，从而使得该对象具有Iterator接口。

```javascript
var myIterable = {};
myIterable[Symbol.iterator] = function* () {
    yield 1;
    yield 2;
    yield 3;
};
[...myIterable] // [1, 2, 3]
```

## next()方法的参数

`yield`语句本身没有返回值，或者说总是返回`undefined`。`next`方法可以带一个参数，该参数就会被当做上一个yield语句的返回值。

```javascript
function* f() {
    for (var i = 0; true; i++) {
        var reset = yield i;
        if (reset) {i = -1;}
    }
}

var g = f();

g.next()
g.next()
g.next(true)
```

上面这个例子有一个很有意思的特性：可以通过在Generator函数运行的不同阶段，从外部向内部注入不同的值，从而来调整函数的行为。