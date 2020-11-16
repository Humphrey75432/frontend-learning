# 正则表达式

## 基本概念

用于匹配字符串中字符组合的模式，在JavaScript中正则表达式也是对象。模式匹配常用在`RegExp` `exec` `test`方法，以及`String`的`match`、`matchAll`、`replace`、`search`和`split`方法。

## 创建语法

使用下面两种方法都可以创建：

```javascript
// method 1
var re = /abc/ig; // 其中ig为正则对象修饰符

// method 2
var re = new RegExp(/abc/ig); // 返回一个原有正则表达式的拷贝
```

除此之外ES6还提供了带两个参数的`RegExp`函数，但是在ES5这种写法是非法的。ES6中则放款了此项限制：

```javascript
var regex = new RegExp(/xyz/, 'i'); // ES5中报错、但是在ES6中是允许的
// regex "i" 第二个参数为指定修饰符，返回的正则表达式对象会忽略原有的修饰符，只使用新指定的修饰符
```

## ES6新增功能：`u`修饰符

`U字符`为Unicode字符的意思，可以识别码点超过`0xFFFF`的字符。前一章节也讲过ES6加入了Unicode字符表示。因此正则也是可以支持Unicode的模式识别的。例子如下：

```javascript
/\u{61}/.test('a') // false
/\u{61}/u.test('a') // true
/\u{20BB7}/u.test('𠮷') // true
```

### 预定义模式

另外`U`字符也会影响预定义模式，能否正确识别大于`0xFFFF`的Unicode字符：

```javascript
function codePointLength(text) {
    var result = text.match(/[\s\S]/gu);
    return result ? result.length : 0;
}

var s = '\u{20BB7}\u{20BB7}';

s.length // 4
codePointLength(s) // 2
```

## ES6新增功能：`y`修饰符

`y`字符也称为粘连字符，其作用于`g`修饰符类似。后一次匹配都从上一次匹配成功的下一个位置开始。不同之处在于`g`修饰符只要剩余位置中存在匹配即可，而`y`修饰符确保匹配必须从剩余的第一个位置开始；例子如下：

```javascript
var s = 'aaa_aa_a';
var r1 = /a+/g;
var r2 = /a+/y;

r1.exec(s);
[ 'aaa', index: 0, input: 'aaa_aa_a', groups: undefined ]

r2.exec(s);
[ 'aaa', index: 0, input: 'aaa_aa_a', groups: undefined ]

r1.exec(s);
[ 'aa', index: 4, input: 'aaa_aa_a', groups: undefined ]

r2.exec(s);
null

r1.exec(s);
[ 'a', index: 7, input: 'aaa_aa_a', groups: undefined ]

r2.exec(s);
[ 'aaa', index: 0, input: 'aaa_aa_a', groups: undefined ]

r1.exec(s);
null

r2.exec(s);
null
```

### 例子：`g`修饰符和`y`修饰符的区别

使用`g`修饰符和`lastIndex`来验证运行方式

```javascript
const REGEX = /a/g;

REGEX.lastIndex = 2; // 默认从2开始匹配（数组下标2）
const match = REGEX.exec('xaya');

match.index // 3 也就是指定位置向后搜索字符'a'

REGEX.lastIndex // 4 从下一次匹配的位置开始
REGEX.exec('xaya') // null
```

使用`y`修饰符和`lastIndex`来验证运行方式

```javascript
const REGEX = /a/y;

REGEX.lastIndex = 2;
REGEX.exec('xaya') // null 不是粘连，因为位置2指向的是字符'y'

REGEX.lastIndex = 3;

const match = REGEX.exec('xaxa');
match.index // 3
REGEX.lastIndex // 4
```

### sticky属性

ES6正则对象多了`sticky`属性，表示是否设置了`y`修饰符。

```javascript
var r = /hello\d/y;
r.sticky // true
```

### flags属性

ES6为正则表达式新增了`flags`属性，会返回正则表达式的修饰符。

```javascript
/abc/ig.source // "abc"

/abc/ig.flags // "gi"
```

## ES6新增功能：`s`修饰符

### dotAll模式：即点代表一切字符

正则表达式中`.`是个特殊字符，可以用于表示任意的单个字符。但是它排除了行终止符。而有时候我们希望匹配的是任意单个字符，因此，提案中引入了`/s`修饰符，使得`.`可以匹配任意单个字符。

```javascript
/foo.bar/s.test('foo\nbar') // true
```

