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
