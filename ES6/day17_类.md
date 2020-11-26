# ES6 Class

## 基本概念

ES6为了使JavaScript更接近C++、Java等面向对象的高级语言，引入了Class的概念。语法角度来说Class仅仅是一个语法糖，其绝大部分功能都是ES5的实现。所以是对编程人员友好而设计的。有个例子能充分说明这个问题。

```javascript
// ES5
function Point(x, y) {
    this.x = x;
    this.y = y;
}

Point.prototype.toString = function() {
    return '(' + this.x + ', ' + this.y + ')';
}

var p = new Point(1, 2);

// ES6 更加符合面向对象语言的习惯
class Point {
    constructor(x, y) {
        this.x = x;
        this.y = y;
    }
    
    toString() {
        return '(' + this.x + ', ' + this.y + ')';
    }
}
var p = new Point(1, 2);
```

可以看出，调用类的实例方法，实际上就是调用对象的原型方法。那么对Object的操作对于Class完全适用。例如我可以使用Object.assign方法给类新增很多方法。

```javascript
class Point {
    constructor() {
        // ...
    }
}

Object.assign(Point.prototype, {
    toString() {},
    toValue() {}
});
```

## constructor方法

对象的构造函数，会在对象创建的时候调用，可以用来初始化对象的属性。除此之外，和C++/Java等语言不同的是，constructor中完全可以指定返回另外一个对象。

```javascript
class Foo {
    constructor() {
        return Object.create(null);
    }
}

new Foo() instanceof Foo // false
```

## 类的实例对象

需要使用`new`关键字来生成。如果忘记使用new关键字而是直接像使用函数那样使用Class会报错。和ES5一样，除非属性显示定义在其本身，否则都是定义在原型上的。

```javascript
//定义类
class Point {

  constructor(x, y) {
    this.x = x;
    this.y = y;
  }

  toString() {
    return '(' + this.x + ', ' + this.y + ')';
  }

}

var point = new Point(2, 3);

point.toString() // (2, 3)

point.hasOwnProperty('x') // true
point.hasOwnProperty('y') // true
point.hasOwnProperty('toString') // false
point.__proto__.hasOwnProperty('toString') // true
```

## 不存在变量提升

Class不存在变量提升，这一点和ES5完全不同：由于ES6不会把类的声明提升到代码头部。必须保证子类在父类之后定义。

## Class表达式

与函数一样，类也可以使用表达式的形式定义。需要注明的是`const`关键字后面的才是真正的类名，而`class`后面的仅仅是内部类名，可以使用`this`关键字指代。

```javascript
const MyClass = class Me {
    getClassName() {
        return Me.name;
    }
};
```

使用Class表达式，可以立即写出执行的Class

```javascript
let person = new class {
    constructor(name) {
        this.name = name;
    }
    
    sayName() {
        console.log(this.name);
    }
}('ZhangSan');

person.sayName(); // 'ZhangSan'
```

## 私有方法