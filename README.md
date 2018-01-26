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

var world = 'hello';

world[1]; // h;
world[10]; // undefined
world['biu']; // undefined
```

- 字符串使用方括号时无法改变字符串中的单个字符。

```javascript
var world = 'hello';

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