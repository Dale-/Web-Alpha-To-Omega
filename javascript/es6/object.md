## 对象的扩展

### 属性的简洁表示法

```javascript
  const foo = 'bar';
  const baz = {foo};
  baz // {foo: "bar"}

  // 等同于
  const baz = {foo: foo};
```

```javascript
  function f(x, y) {
    return {x, y};
  }

  // 等同于

  function f(x, y) {
    return {x: x, y: y};
  }

  f(1, 2) // Object {x: 1, y: 2}
```

> 除了属性简写，方法也可以简写。

```javascript
  const o = {
    method() {
      return "Hello!";
    }
  };

  // 等同于

  const o = {
    method: function() {
      return "Hello!";
    }
  };
```

> 下面是一个实际的例子。

```javascript
  let birth = '2000/01/01';

  const Person = {

    name: '张三',

    //等同于birth: birth
    birth,

    // 等同于hello: function ()...
    hello() { console.log('我的名字是', this.name); }

  };
```
