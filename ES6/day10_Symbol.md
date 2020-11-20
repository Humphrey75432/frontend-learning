# Symbol

ES5的对象属性名都是字符串，这容易造成属性名的冲突。如果存在一种机制，能够保证每个属性的名字都是独一无二的就可以解决这个问题。因此ES6中引入了Symbol。

ES6引入了一种新的原始数据类型Symbol，表示独一无二的值。可以称之为JavaScript的第七种数据类型。

Symbol是通过symbol函数生成的。也就是说，对象的属性名可以有两种类型，一种是原来就有的字符串，另一种是新增的Symbol类型。凡是属性名属于Symbol的，就是独一无二的，可以保证不会与其他属性名发生冲突。

```javascript
let s = Symbol();
typeof s // symbol
```

`Symbol`函数可以接收一个字符串作为参数，表示对Symbol实例的描述，主要是为了在控制台显示或者是转为字符串的过程中方便区分。

```javascript
var s1 = Symbol('foo');
var s2 = Symbol('bar');

s1 // Symbol(foo)
s2 // Symbol(bar)

s1.toString() // "Symbol(foo)"
s2.toString() // "Symbol(bar)"
```

如果Symbol的参数是一个对象，那么就会调用该对象的`toString()`方法，将其转换为字符串，然后才生成一个Symbol值。

```javascript
const obj = {
    toString() {
        return 'abc';
    }
};

const sym = Symbol(obj);
sym // Symbol(abc)
```

Symbol值不能与其他类型的值进行运算，会报错；但是却可以显式转换为字符串

```javascript
var sym = Symbol('My Symbol');

"your symbol is " + sym // Error

String(sym); // Symbol('My symbol')
sym.toString(); // Symbol(My symbol)
```

## 作为属性名的Symbol

由于每一个Symbol值都是不相等的，这意味着Symbol可以作为标识符，用在对象的属性名，就能保证不会出现同名的属性。可以防止某一个键被不小心改写或覆盖。

```javascript
var mySymbol = Symbol();

var a = {};
a[mySymbol] = 'Hello!';

var a = {
    [mySymbol]: 'Hello'
};

var a = {};
Object.defineProperty(a, mySymbol, { value: 'Hello' });

a[mySymbol] // "Hello"
```

Symbol值作为对象属性名时，不能用作点运算符。

```javascript
var mySymbol = Symbol();
var a = {};

a.mySymbol = 'Hello';
a[mySymbol] // undefined
a['mySymbol'] // Hello
```

Symbol类型还可以用于定义一组常量，保证这组常量的值都是不相等的。

```javascript
log.levels = {
    DEBUG: Symbol('debug'),
    INFO: Symbol('info'),
    WARN: Symbol('warn')
};

log(log.levels.DEBUG, 'debug message');
log(log.levels.INFO, 'info message');
```

还有一个例子：

```javascript
var shapeType = {
    triangle: Symbol()
};

function getArea(shape, options) {
    var area = 0;

    switch(shape) {
        case shapeType.triangle:
            area = .5 * options.width * options.height;
            break;
    }

    return area;
}

getArea(shapeType.triangle, {width: 100, height: 100}); // 5000
```

## 属性名的遍历

Symbol作为属性名，不会出现在`for...in`、`for...of`循环中，也不会被`Object.keys()`、`Object.getOwnPropertyNames()`、`JSON.stringify()`返回。但是，它也不是私有属性，有一个`Object.getOwnPropertySymbols`方法，可以获取指定对象的所有Symbol属性名。

```javascript
var obj = {};
var a = Symbol('a');
var b = Symbol('b');

obj[a] = 'Hello';
obj[b] = 'World';

var objectSymbols = Object.getOwnPropertySymbols(obj);

objSymbols // [Symbol(a), Symbol(b)]
```

由于以Symbol值作为名称的属性，不会被常规方法遍历得到。因此可以利用这个特性，为对象定义一些非私有的、但又希望只用于内部的方法。

```javascript
var size = Symbol('size');

class Collection {
    constructor() {
        this[size] = 0;
    }
    
    add(item) {
        this[this[size]] = item;
        this[size]++;
    }
    
    static sizeOf(instance) {
        return instance[size];
    }
}

var x = new Collection();
Collection.sizeOf(x) // 0

x.add('foo');
Collection.sizeOf(x) // 1

Object.keys(x) // ['0']
Object.getOwnPropertyNames(x) // ['0']
Object.getOwnPropertySymbols(x) // [Symbol(size)]
```

## Symbol.for(), Symbol.keyFor()

有时候希望重新使用同一个Symbol值，`Symbol.for`可以做到这一点。其接受一个字符串为参数，然后搜索有没有已该参数作为名称的Symbol值，有就返回，没有就以这个名称创建一个Symbol值。

```javascript
var s1 = Symbol.for('foo')
var s2 = Symbol.for('foo')
s1 === s2 // true
```

