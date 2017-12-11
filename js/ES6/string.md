## 字符串的遍历器接口
```javascript
  for (let codePoint of 'foo') {
    console.log(codePoint)
  }
  // "f"
  // "o"
  // "o"
```

### at()
```javascript
  'abc'.charAt(0) // "a"
  '𠮷'.charAt(0) // "\uD842"

  'abc'.at(0) // "a"
  '𠮷'.at(0) // "𠮷"
```

### includes(), startsWith(), endsWith()

> * includes()：返回布尔值，表示是否找到了参数字符串。

> * startsWith()：返回布尔值，表示参数字符串是否在原字符串的头部。

> * endsWith()：返回布尔值，表示参数字符串是否在原字符串的尾部。

```javascript
  let s = 'Hello world!';

  s.startsWith('Hello') // true
  s.endsWith('!') // true
  s.includes('o') // true
```

* 这三个方法都支持第二个参数，表示开始搜索的位置。

```javascript
  let s = 'Hello world!';

  s.startsWith('world', 6) // true
  s.endsWith('Hello', 5) // true
  s.includes('Hello', 6) // false
```

### repeat()

* repeat方法返回一个新字符串，表示将原字符串重复n次。
```javascript
  'x'.repeat(3) // "xxx"
  'hello'.repeat(2) // "hellohello"
  'na'.repeat(0) // ""
```

* 参数如果是小数，会被取整。
```javascript
  'na'.repeat(2.9) // "nana"
```

* 如果repeat的参数是负数或者Infinity，会报错。
```javascript
  'na'.repeat(Infinity)
  // RangeError
  'na'.repeat(-1)
  // RangeError
```

```javascript
  'na'.repeat(-0.9) // ""
  'na'.repeat(NaN) // ""

  'na'.repeat('na') // ""
  'na'.repeat('3') // "nanana"
```

### padStart()，padEnd()

```javascript
  'x'.padStart(5, 'ab') // 'ababx'
  'x'.padStart(4, 'ab') // 'abax'

  'x'.padEnd(5, 'ab') // 'xabab'
  'x'.padEnd(4, 'ab') // 'xaba'

  'xxx'.padStart(2, 'ab') // 'xxx'
  'xxx'.padEnd(2, 'ab') // 'xxx'
```

### 模板字符串
```javascript
  $('#result').append(
    'There are <b>' + basket.count + '</b> ' +
    'items in your basket, ' +
    '<em>' + basket.onSale +
    '</em> are on sale!'
  );

  $('#result').append(`
    There are <b>${basket.count}</b> items
     in your basket, <em>${basket.onSale}</em>
    are on sale!
  `);
```
