# 对象扩展

## 属性的简洁表示法

ES6允许直接写入变量和函数，作为对象的属性和方法。这样书写更加简洁。

```javascript
var foo = 'bar';
var baz = {foo};
baz // {foo: "bar"}

// 等同于
var baz = {foo: foo};
```

除了属性可以简写，方法也可以简写

```javascript
var o = {
    method() {
        return "Hello";
    }
};

// 等同于

var o = {
    method: function() {
        return "Hello";
    }
};
```

适用于函数返回值，写起来会非常简洁和方便

```javascript
function getPoint() {
    var x = 1;
    var y = 10;
    return {x, y};
}

getPoint() // {x: 1, y: 10}
```

在使用Common JS中，输出的代码非常适合使用这种简洁写法

```javascript
var ms = {};

function getItem(key) {
    return key in ms ? ms[key] : null;
}

function setItem(key, value) {
    ms[key] = value;
}

function clear() {
    ms = {};
}

module.exports = { getItem, setItem, clear }
// 等同于
module.exports = {
    getItem: getItem,
    setItem: setItem,
    clear: clear
}
```

## 属性名表达式

JavaScript语言定义对象的属性，有如下种方法。

```javascript
obj.foo = true; // 直接使用标识符作为属性名

obj['a' + 'bc'] = 123; // 使用表达式作为属性名
```

ES6允许字面量定义对象时，用方法二作为对象的属性名，即将表达式放在括号内

```javascript
let propKey = 'foo';

let obj = {
    [propKey]: true,
    ['a' + 'bc']: 123
}
```

这里再举一个例子：

```javascript
var lastWord = 'last word';

var a = {
    'first word': 'hello',
    [lastWord]: 'world'
};

a['first Word'] // hello
a[lastWord] // World
a['last word'] // World
```

注意：属性名表达式如果是一个对象，默认情况下会自动将对象转换为字符串。因此属性名表达式和简洁表示法不可以同时使用。一定要注意。

## 方法的`name`属性

函数的`name`属性返回函数名。对象方法也是函数，因此也有`name`属性。

```javascript
var person = {
    sayName() {
        console.log(this.name);
    },
    // get为取值函数，存值用set
    get firstName() {
        return "Nicholas";
    }
};

person.sayName.name // sayName
person.firstName.name // get firstName
```

有两个特例：如果是`bind`函数，函数名返回`bound` + 函数名称；如果是`function`关键字构造的函数（匿名函数），`name`属性值返回`anonymous`。

## `Object.is()`

该方法用来比较两个值是否严格相等，与严格比较运算符`===`的作用一致。不同之处有两个：+0不等于-0；另一个是NaN等于自身。

```javascript
+0 === -0 // true
NaN === NaN // false

Object.is(+0, -0); // false
Object.is(NaN, NaN) // true
```

## `Object.assign()`

### 基本用法

用于将对象合并，将源对象的所有可枚举属性赋值到目标对象。如果目标对象与源对象由同名属性，或多个源对象由同名属性，则后面的属性会覆盖前面的属性。

```javascript
var target = { a: 1 };

var source1 = { b: 2 };
var source2 = { c: 3 };

Object.assign(target, source1, source2);
target // { a: 1, b: 2, c: 3 }
```

如果只有一个参数，会直接返回该参数；如果该参数不是对象，会先转换为对象，然后返回。由于`undefined`和`null`无法转换为对象，所以将它们作为参数传入会报错。

```javascript
var obj = { a: 1};
Object.assign(obj) === obj // true

typeof Object.assign(2) // object

Object.assign(undefined) // error
Object.assign(null) // error
```

如果`undefined`和`null`出现在非首参数，首先会尝试着转换为对象，如果转换失败会直接跳过。这意味着只要首参数可以转换为对象，就不会报错。

```javascript
let obj = { a: 1};

Object.assign(a, undefined) === obj // true
Object.assign(a, null) === obj // true
```

其他类型的值中，只有字符串会以数组的形式拷贝至对象中，其他值都不会产生效果；

```javascript
var v1 = 'abc';
var v2 = true;
var v3 = 10;

var obj = Object.assign({}, v1, v2, v3);
console.log(obj); // {'0': 'a', '1': 'b', '2': 'c'}
```

`Object.assign()`拷贝的属性是有限制的，只拷贝源对象的自身属性（不拷贝继承属性），也不拷贝不可枚举的属性（`enumerable: false`）。

```javascript
Object.assign({b: 'c'}, Object.defineProperty({}, 'invisible', {
    	enumerable: false,
    	value: 'hello'
	})
)
// {b: 'c'}
```

值得注意的是：`Object.assign()`执行的是浅拷贝，而不是深拷贝。如果源对象某个属性的值是对象，那么目标对象拷贝得到的仅仅是这个对象的引用。

```javascript
var obj1 = {a: {b: 1}};
var obj2 = Object.assign({}, obj1);

obj1.a.b = 2;
obj2.a.b // 2
```



## 属性的可枚举性

## 属性的遍历

## `__proto__`属性，`Objects.setPrototypeOf()`, `Object.getPrototypeOf()`

## `Object.values()`, `Object.entries()`

## 对象的扩展运算符

## `Object.getOwnPropertyDescriptors()`