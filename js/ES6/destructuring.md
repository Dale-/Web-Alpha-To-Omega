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
### 函数参数的解构
