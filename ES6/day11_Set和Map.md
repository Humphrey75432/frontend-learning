# Set和Map

## Set

### 基本用法

类似与数组结构，但是数组中的成员值时唯一不重复的。跟Java中的Set是同样的意思。

```javascript
var s = new Set();

[2, 3, 5, 4, 5, 2, 2].map(x => s.add(x));

for (let i of s) {
  console.log(i);
}
// 2 3 5 4
```

也可以结合扩展运算符写成下面这样：

```javascript
var set = new Set([1,2,3,4,5,2,3]);
[...set] // 1 2 3 4 5
set.size() // 5
```

### 实例的属性和方法

除了构造函数以及返回size的大小，还有下面4个操作的方法：使用基本和Java相同

- add(value)：添加某个值，返回Set结构本身
- delete(value)：删除某个值，返回一个布尔值，表示是否删除成功；
- has(value)：返回一个布尔值，表示该值是否为Set成员
- clear()：清除所有成员，没返回值。

```javascript
var s = new Set();
s.add(1).add(2).add(2);

s.size // 2

s.has(1) // true
s.has(2) // true
s.has(3) // false

s.delete(2);
s.has(2) // false
```

### 遍历操作

遍历器包含下面4个方法，可以用于遍历成员，结合Lambda表示可以有十分简洁的写法，跟Java 8的lambdav哈不多，这里不再赘述。

- keys()：返回键名的遍历器
- values()：返回键值的遍历器
- entries()：返回键值对的遍历器
- forEach()：使用回调函数遍历每个成员

```javascript
let set = new Set(['red', 'green', 'blue']);
for (let item of set.keys()) {
  console.log(item);
}
// red
// green
// blue

for (let item of set.values()) {
  console.log(item);
}
// red
// green
// blue

for (let item of set.entries) {
  console.log(item)
}
// ['red', 'red']
// ['green', 'green']
// ['blue', 'blue']
```

### WeakSet

从名字上看，WeakSet也是Set的一种，但是跟Set有下列几个区别：

- 成员只能是对象，不能是其他类型的值；
- 集合内的对象全都是弱引用，GC不会考虑对WeakSet中的对象的引用，因此WeakSet是不可遍历的。

你可能会问，WeakSet的这种特性有什么用呢？由于不能遍历且内存中的对象随时会丢失，因此十分适合存储DOM节点，而不用担心这些节点从文档移除时引发内存泄露。

```javascript
const foos = new WeakSet();

class foo {
  constructor() {
    foos.add(this);
  }
  
  method() {
    if (!foos.has(this)) {
      throw new TypeError('Foo.prototype.method智能在Foo实例上调用!')
    }
  }
}
```



## Map

### 基本用法

### 实例的属性和方法

### 遍历方法

### 与其他数据结构的互相转换

### WeakMap