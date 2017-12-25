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

## Array.form()

> `Array.from` 方法用于将两类对象转为真正的数组：类似数组的对象(array-like object) 和 可遍历(iterable) 的对象(包括ES6新增的数据结构SET和MAP)。

```javascript
  let arrayLike = {
    '0': 'a',
    '1': 'b',
    '2': 'c',
    length: 3
  };

  // ES5的写法
  var array1 = [].slice.call(arrayLike);
  // ['a', 'b', 'c']

  // ES6的写法
  let array2 = Array.from(arrayLike);
  // ['a', 'b', 'c']

```

## Array.of()

> `Array.of`方法用于将一组值，转换为数组。

```javascript
  Array.of(3, 1, 8) // [3, 1, 8]
  Array.of(3) // [3]
  Array.of(3).length // 1
```

## 数组实例的 copyWithin()

## 数组实例的 find() 和 findIndex()

## 数组实例的fill()

## 数组实例的 entries(), keys() 和 values()

## 数组实例的 includes()

## 数组的空位            
