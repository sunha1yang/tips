### JS查缺补漏

#### 1. switch...case
1. 当case缺少break的时候会发生什么？在遇到满足的选项后会将后续无break的语句全部执行，即使后续的case不满足。

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

2. case的判断条件是否会进行转换？不会，校验时会进行类型判断。

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

3. switch语句部分和case语句部分，都可以使用表达式。

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
