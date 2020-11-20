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

