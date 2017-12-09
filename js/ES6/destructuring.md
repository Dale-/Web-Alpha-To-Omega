## 变量的解构赋值

### 数组的解构

> 基本用法

```javascript
  let [foo, [[bar], baz]] = [1, [[2], 3]];
  foo // 1
  bar // 2
  baz // 3

  let [ , , third] = ["foo", "bar", "baz"];
  third // "baz"

  let [x, , y] = [1, 2, 3];
  x // 1
  y // 3

  let [head, ...tail] = [1, 2, 3, 4];
  head // 1
  tail // [2, 3, 4]

  let [x, y, ...z] = ['a'];
  x // "a"
  y // undef
```

* 如果解构不成功，变量的值就等于undefined
```javascript
  let [foo] = [];
  let [bar, foo] = [1];
```
以上两种情况都属于解构不成功，foo的值都会等于undefined。

* 报错
```javascript
  let [foo] = 1;
  let [foo] = false;
  let [foo] = NaN;
  let [foo] = undefined;
  let [foo] = null;
  let [foo] = {};
```
上面的语句都会报错，因为等号右边的值，要么转为对象以后不具备 Iterator 接口（前五个表达式），要么本身就不具备 Iterator 接口（最后一个表达式）。

> 默认值

```javascript
  let [foo = true] = [];
  foo // true

  let [x, y = 'b'] = ['a']; // x='a', y='b'
  let [x, y = 'b'] = ['a', undefined]; // x='a', y='b'
```

* 注意，ES6 内部使用严格相等运算符（===），判断一个位置是否有值。所以，如果一个数组成员不严格等于undefined，默认值是不会生效的。
```javascript
  let [x = 1] = [undefined];
  x // 1

  let [x = 1] = [null];
  x // null
```

* 默认值可以引用解构赋值的其他变量，但该变量必须已经声明。
```javascript
  let [x = 1, y = x] = [];     // x=1; y=1
  let [x = 1, y = x] = [2];    // x=2; y=2
  let [x = 1, y = x] = [1, 2]; // x=1; y=2
  let [x = y, y = 1] = [];     // ReferenceError
```

### 对象的解构
```javascript
  let { foo, bar } = { foo: "aaa", bar: "bbb" };
  foo // "aaa"
  bar // "bbb"
```

* 与数组一样，解构也可以用于嵌套结构的对象。
```javascript
  let obj = {
    p: [
      'Hello',
      { y: 'World' }
    ]
  };

  let { p: [x, { y }] } = obj;
  x // "Hello"
  y // "World"
```

* 对象的解构也可以指定默认值。
```javascript
  var {x = 3} = {};
  x // 3

  var {x, y = 5} = {x: 1};
  x // 1
  y // 5

  var {x: y = 3} = {};
  y // 3

  var {x: y = 3} = {x: 5};
  y // 5

  var { message: msg = 'Something went wrong' } = {};
  msg // "Something went wrong"
```

* 默认值生效的条件是，对象的属性值严格等于undefined。
```javascript
  var {x = 3} = {x: undefined};
  x // 3

  var {x = 3} = {x: null};
  x // null
```

### 字符串的解构
```javascript
  const [a, b, c, d, e] = 'hello';
  a // "h"
  b // "e"
  c // "l"
  d // "l"
  e // "o"
```

* 类似数组的对象都有一个length属性，因此还可以对这个属性解构赋值。
```javascript
  let {length : len} = 'hello';
  len // 5
```

### 数值和布尔的解构

* 解构赋值时，如果等号右边是数值和布尔值，则会先转为对象。

```javascript
  let {toString: s} = 123;
  s === Number.prototype.toString // true

  let {toString: s} = true;
  s === Boolean.prototype.toString // true
```

```javascript
  let { prop: x } = undefined; // TypeError
  let { prop: y } = null; // TypeError
```

### 函数参数的解构

```javascript
  function add([x, y]){
    return x + y;
  }

  add([1, 2]); // 3

  [[1, 2], [3, 4]].map(([a, b]) => a + b);
  // [ 3, 7 ]

```

* 函数参数的解构也可以使用默认值。
```javascript
  function move({x = 0, y = 0} = {}) {
    return [x, y];
  }

  move({x: 3, y: 8}); // [3, 8]
  move({x: 3}); // [3, 0]
  move({}); // [0, 0]
  move(); // [0, 0]
```

上面代码中，函数move的参数是一个对象，通过对这个对象进行解构，得到变量x和y的值。如果解构失败，x和y等于默认值。

```javascript
  function move({x, y} = { x: 0, y: 0 }) {
    return [x, y];
  }

  move({x: 3, y: 8}); // [3, 8]
  move({x: 3}); // [3, undefined]
  move({}); // [undefined, undefined]
  move(); // [0, 0]
```

上面代码是为函数move的参数指定默认值，而不是为变量x和y指定默认值，所以会得到与前一种写法不同的结果。

## 用途
### 交换变量的值
```javascript
  let x = 1;
  let y = 2;

  [x, y] = [y, x];
```

### 从函数返回多个值
```javascript
  // 返回一个数组

  function example() {
    return [1, 2, 3];
  }
  let [a, b, c] = example();

  // 返回一个对象

  function example() {
    return {
      foo: 1,
      bar: 2
    };
  }
  let { foo, bar } = example();
```

### 函数参数的定义
* 解构赋值可以方便地将一组参数与变量名对应起来。
```javascript
  // 参数是一组有次序的值
  function f([x, y, z]) { ... }
  f([1, 2, 3]);

  // 参数是一组无次序的值
  function f({x, y, z}) { ... }
  f({z: 3, y: 2, x: 1});
```
