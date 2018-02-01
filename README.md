### JS查缺补漏

#### 1. switch...case
- 当case缺少break的时候会发生什么？在遇到满足的选项后会将后续无break的语句全部执行，即使后续的case不满足。

```javascript
const x = 1;

function test(x){
  switch (x) {
    case 1:
      console.log('x 等于1');
    case 2:
      console.log('x 等于2');
    case true:
      console.log('x 等于4');
    default:
      console.log('x 等于其他值');
      break;
  }
}
test(x);

// x 等于1
// x 等于2
// x 等于4
// x 等于其他值
```

- case的判断条件是否会进行转换？不会，校验时会进行类型判断。

```javascript
const x = 4;

function test(x){
  switch (x) {
    case 1:
      console.log('x 等于1');
    case 2:
      console.log('x 等于2');
    case true:
      console.log('x 等于4');
    default:
      console.log('x 等于其他值');
      break;
  }
}
test(x);

// x 等于其他值
```

- switch语句部分和case语句部分，都可以使用表达式。

#### 2. break和continue都可以跳出循环，但是有什么区别？

```javascript
for (var i = 0; i < 5; i++) {
  console.log(i);
  if (i === 3)
    break;
}
// 0
// 1
// 2
// 3
// breack 跳出循环后续循环不再执行

for (var i = 0; i < 5; i++) {
  if (i === 3) continue;
  console.log(i);
}
// 0
// 1
// 2
// 4
// continue跳出本轮循环，开始下一轮循环
```

#### 3. 数值操作
- 正零和负零。


  `正零和负零`几乎没有任何区别，除了在做分母时。
```
(1 / +0) === (1 / -0)

// false
// 除以正零得到+Infinity，除以负零得到-Infinity
```

#### 4. 字符串与数组

- 字符串可以被视为字符数组，可以使用数组的方括号返回某个位置的字符，但如果方括号中的数字超过字符串的长度，或方括号中的不是数字，则返回`undefined`。

```javascript

var world = 'hello';

world[1]; // h;
world[10]; // undefined
world['biu']; // undefined
```

- 字符串使用方括号时无法改变字符串中的单个字符。

```javascript
var world = 'hello';

world[1] = W;
world; // hello

delete s[0];
world; // "hello"
```

#### 5. 对象的坑

- 如果行首是一个大括号，它到底是表达式还是语句？

```
{ hello : 123 }

JavaScript 引擎读到上面这行代码，可能有两种含义。
1. 这是一个表达式，表示一个包含foo属性的对象；
2. 这是一个语句，表示一个代码区块，里面有一个标签foo，指向表达式123。

JavaScript 规定，如果行首是大括号，一律解释为语句（即代码块）。如果要解释为表达式（即对象），必须在大括号前加上圆括号。

eval('{foo: 123}') // 123
eval('({foo: 123})') // {foo: 123}
```

#### 6. 数组的坑
- 数组的实质是对象。
- 数组可以使用`in`来判断当前值是否在数组内。
- for...in不仅会遍历数组所有的数字键，还会遍历非数字键,forEach则不会。
- 数组的空位不影响length属性。
- 数组最后一个成员后面有一个逗号，这不影响length属性的值，与没有这个逗号时效果一样。
- 使用delete命令删除一个数组成员，会形成空位，并且不会影响length属性。


#### 7. 函数的坑
- 函数的name属性返回函数的名字但是下面这样的情况会返回什么？

```javascript
var f2 = function () {};
f2.name // "f2"

var f3 = function myName() {};
f3.name // 'myName'

// PS: 只有在变量的值是一个匿名函数时才是变量名。如果变量的值是一个具名函数，那么name属性返回function关键字之后的那个函数名。
```

- 函数的toString方法返回一个字符串，内容是函数的源码，内部的注释也可以返回。
- 函数执行时所在的作用域，是定义时的作用域，而不是调用时所在的作用域。

```javascript
var x = function () {
  console.log(a);
};

function y(f) {
  var a = 2;
  f();
}

y(x)
// ReferenceError: a is not defined
```

- 函数出现同名参数则取最后出现的那个值。
- 严格模式下，arguments对象是一个只读对象，修改它是无效的，但不会报错。
- 两个连着的自执行函数必须带有分号，否则会报错。

```javascript
// 报错
(function(){ /* code */ }())
(function(){ /* code */ }())
```


#### 8. 错误处理机制

- Error 实例对象的一些属性：
    > message：错误提示信息
    > name：错误名称（非标准属性）
    > stack：错误的堆栈（非标准属性）

 - 原生错误类型
    1. SyntaxError对象是解析代码时发生的语法错误。
        > 触发时机：语法解析阶段
    2. ReferenceError对象是引用一个不存在的变量时发生的错误。
        > 触发时机：1、引用一个不存在的变量时。 2、将一个值分配给无法分配的对象，比如对函数的运行结果或者this赋值。
    3. RangeError对象是一个值超出有效范围时发生的错误。
        > 触发时机：1、数组长度为负数。 2、Number对象的方法参数超出范围。 3、函数堆栈超过最大值。
    4. TypeError对象是变量或参数不是预期类型时发生的错误。
        > 触发时机：对字符串、布尔值、数值等原始类型的值使用new命令。
    5. URIError对象是 URI 相关函数的参数不正确时抛出的错误。
        > 触发时机：主要涉及encodeURI()、decodeURI()、encodeURIComponent()、decodeURIComponent()、escape()和unescape()这六个函数。
    6. eval函数没有被正确执行时，会抛出EvalError错误。

- try…catch...finally和return的执行顺序问题
1. try代码块没有发生错误，而且里面还包括return语句，但是finally代码块依然会执行。注意，只有在其执行完毕后，才会显示return语句的值。
```javascript
function idle(x) {
  try {
    console.log(x);
    return 'result';
  } finally {
    console.log("FINALLY");
  }
}

idle('hello')
// hello
// FINALLY
// "result"
```

2. return语句的执行是排在finally代码之前，只是等finally代码执行完毕后才返回。

```javascript
var count = 0;
function countUp() {
  try {
    return count;
  } finally {
    count++;
  }
}

countUp()
// 0
count
// 1

// return语句的count的值，是在finally代码块运行之前就获取了。
```

3. try...catch...finally与return几者之间的执行顺序。

```javascript
function f() {
  try {
    console.log(0);
    throw 'bug';
  } catch(e) {
    console.log(1);
    return true; // 这句原本会延迟到 finally 代码块结束再执行
    console.log(2); // 不会运行
  } finally {
    console.log(3);
    return false; // 这句会覆盖掉前面那句 return
    console.log(4); // 不会运行
  }

  console.log(5); // 不会运行
}

var result = f();
// 0
// 1
// 3

result
// false
```

4. 如果函数内部执行发生错误但是被catch到后，外部不会发生错误且不会被外部catch捕捉到。




