## 扩展运算符 (spread)

> 扩展运算符是三个点（...）。它好比rest参数的逆运算，将一个数组转为用逗号分隔的参数序列。

```javascript
  console.log(...[1, 2, 3])
  // 1 2 3

  console.log(1, ...[2, 3, 4], 5)
  // 1 2 3 4 5

  [...document.querySelectorAll('div')]
  // [<div>, <div>, <div>]
```

> 应用

```javascript
  function push(array, ...items) {
    array.push(...items);
  }

  function add(x, y) {
    return x + y;
  }

  const numbers = [4, 38];
  add(...numbers) //42

  function f(v, w, x, y, z) {}
  const args = [0, 1];
  f(-1, ...args, 2, ...[3]);
```

> 如果扩展运算符最后面是一个空数组，则不产生任何效果。

```javascript
  [...[], 1]
  // [1]
```
