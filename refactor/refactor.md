## [a, b].indexOf() 重构 ( value === a || value === b)

```javascript
  if ( value === STATUS.NONE || value === STATUS.PROCESS) {
    return true;
  }
```

```javascript
  if ( [STATUS.NONE, STATUS.PROCESS].indexOf(value) >= 0) {
    return true;
  }
```
