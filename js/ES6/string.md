## 字符的Unicode表示法

```javascript
  "\u0061"
  // "a"

  "\uD842\uDFB7"
  // "𠮷"

  "\u20BB7"
  // "7"
```

ES6对这一点做出了改进，只要将码点放入大括号，就能正确解读该字符。

```javascript
  "\u{20BB7}"
  // "𠮷"

  "\u{41}\u{42}\u{43}"
  // "ABC"

  let hello = 123;
  hell\u{6F} // 123

  '\u{1F680}' === '\uD83D\uDE80'
// true
```

## codePointAt()

## String.fromCodePoint()

## 字符串的遍历器接口
