## React原理
### React 生命周期
### React DIFF DOM
### React render

## 性能检测工具
### Why-did-you-update

why-did-you-update是一个可以检测到潜在不必要的组件渲染的库。当发生比必要的更新时，它会检测到组件的render方法何时被调用，并反应到控制台。


![](/source/img/javascript/why-did-you-update.png)

> Setup

```javascript
    npm install --save why-did-you-update
    or
    yarn add why-did-you-update
```

> Usage

```javascript
import React from 'react';

if (process.env.NODE_ENV !== 'production') {
  const {whyDidYouUpdate} = require('why-did-you-update');
  whyDidYouUpdate(React);
}

```

More Options please click [why-did-you-update](https://github.com/maicki/why-did-you-update)

需要注意的是，要确保`why-did-you-update`在production环境中被禁用，因为它会减慢应用程序的速度。

### 性能时间表
React 15.4.0引入了性能时间轴的功能，可以更直观的了解可视化的组件生命周期。

> 使用方法

* 打开你的应用程序并添加查询字符串?react_perf 。例如: http://localhost:3000
* 打开Chrome DevTools, 选择Performance，点击Record。
* 执行你想要分析的动作。
* 停止记录。
* React事件将会被归类在 **User Timing** 标签下。

![](/source/img/javascript/react_perf.png)

### React.addons.Rerf

## 优化方法
### PureComponent
> Component的默认渲染行为

在React Component的生命周期中，又一个`shouldComponentUpdate`方法，这个方法的默认返回值为`true`。

这意味着Component不会比较对当前和下一个状态下的`props`和`state`, 因此每当`shouldComponentUpdate`被调用时，组件默认会重新渲染。从而导致组件因为不相关数据的改变导致重绘，这极大的降低了React的渲染效率。

> PureComponent

当组件更新时，如果组件的`prop`和`state`都没发生变化，`render`方法就不会被触发。从而省去了`Virtual DOM`的生成和对比过程，达到了提升渲染效率的目的。

下面是PureCompoent对比`props`和`state`的浅比较
```javascript
    if (this._compositeType === CompositeTypes.PureClass) {
    shouldUpdate = !shallowEqual(prevProps, nextProps)
    || !shallowEqual(inst.state, nextState);
    }
```

`shallowEqual`会比较`Object.keys(state | props)`的长度是否一致，每一个`key`是否两者都有，并且比对是否为同一个引用。所以`shallowEqual`比较的是第一层的值，而非深层的嵌套数据对比。

> 使用方法

将原组件继承自React.Component,替换为React.PureComponent
```javascript
    import React { PureComponent, Component } from 'react';

    class Foo extends (PureComponent || Component) {
    //...
    }
```

### 实现 shouldComponentUpdate
我们知道`PureComponent`的`shadowEqual`只会浅检查组件的`props`和`state`,所以嵌套对象和数组是不会被比较的。所以如果是需要深比较，我们也可以使用`shouldComponentUpdate`来手动单个比较是否需要重新渲染。

`shouldComponentUpdate`是`props`或`state`更改后、render前被调用的方法。如果`shouldComponentUpdate`返回`true`，`render`将被调用，如果它返回`false`，则组件不会重新渲染。

```javascript
    shouldComponentUpdate(nextProps, nextState) {
        if (/*do some compare*/) {
            return true;
        }
    }
```

### Immutable

### plugin-transform-react-inline-elements