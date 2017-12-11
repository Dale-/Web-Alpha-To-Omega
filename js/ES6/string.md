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
