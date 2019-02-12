## 前言

React使用几种巧妙的技术来最大限度地减少更新UI所需的昂贵的 DOM 操作的数量, 今天我们通过对React渲染机制的回顾，从一些性能检测工具入手，介绍一些优化React应用性能的办法。

## React渲染机制

React不直接操作真实DOM，它在内部维护了一套快速相应的虚拟DOM(本篇文章简称为VDOM)，`render`方法返回一个VDOM的描述，React会在reconsilation之后最小化的进行VDOM的更新，最终patch到真实的DOM。

![](/source/img/javascript/react-update.png)

上图官网给出的React组件渲染机制描述图

根据渲染流程，首先会判断shouldComponentUpdate(SCU)是否需要更新。如果需要更新，则调用组件的render生成新的虚拟DOM，然后再与旧的虚拟DOM对比(vDOMEq)，如果对比一致就不更新，如果对比不同，则根据最小粒度改变去更新DOM；如果SCU不需要更新，则直接保持不变，同时其子元素也保持不变。

* C1根节点，绿色SCU (true)，表示需要更新，然后vDOMEq红色，表示虚拟DOM不一致，需要更新。
* C2节点，红色SCU (false)，表示不需要更新，所以C4,C5均不再进行检查
* C3节点同C1，需要更新
* C6节点，绿色SCU (true)，表示需要更新，然后vDOMEq红色，表示虚拟DOM不一致，更新DOM。
* C7节点同C2
* C8节点，绿色SCU (true)，表示需要更新，然后vDOMEq绿色，表示虚拟DOM一致，不更新DOM。

我们再来看看 触发组件更新的流程图

![](/source/img/javascript/react-render-flow.png)

通过上述流程图，再对比渲染机制描述图我们可以发现，如果进行性能优化，关键在：

* shouldComponentUpdate阶段，如果props与state与上一次相同，这个时候可以中断后续的生成虚拟DOM以及DOM DIFF的过程。
* 提高DOM DIFF的效率

## 性能检测工具
### Why-did-you-update

why-did-you-update是一个可以检测到潜在不必要的组件渲染的库。当发生比必要的更新时，它可以在控制台上打印出可能的可以避免多次 re-render 的操作。

它的原理是，通过比较每次 render 时组件的 prevProps 和 this.props，这里的比较是深比较，对 plain object 递归比较，对同为函数的相同属性只通过比较函数名来判断是否相同。

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

React 官方文档里推荐的性能检测方法，是对 Chrome Devtool 的加强，可以将 Devtool 中 JS 部分的火焰图细分为组件各个声明周期的时间，在目前的开发模式下，不用输入 ?rect_perf 也已经开启了这个功能，生产模式下的页面仍然需要加上这个命令。

> 使用方法

* 打开你的应用程序并添加查询字符串?react_perf 。例如: http://localhost:3000
* 打开Chrome DevTools, 选择Performance，点击Record。
* 执行你想要分析的动作。
* 停止记录。
* React事件将会被归类在 **User Timing** 标签下。

![](/source/img/javascript/react_perf.png)

这些数字是相对的，因此组件在生产版本中会运行更快*。然而，这也能够帮助你了解何时会有无关的组件被错误的更新，以及你的组件更新的深度和频率。


> Performance的四个窗格

![](/source/img/javascript/timeline-annotated.png)

* Controls
* Overview: 从FPS、CPU、NET三方面进行性能汇总。
* 火焰图
* 记录详细信息: 在火焰图中选择事件时，Details窗格会显示与事件相关的其他信息。


## 优化方法
### PureComponent
> Component的默认渲染行为

在React Component的生命周期中，又一个`shouldComponentUpdate`方法，这个方法的默认返回值为`true`。

这意味着Component不会比较对当前和下一个状态下的`props`和`state`, 因此每当`shouldComponentUpdate`被调用时，组件默认会重新渲染。从而导致组件因为不相关数据的改变导致重绘，这极大的降低了React的渲染效率。

> PureComponent

当组件更新时，如果组件的`prop`和`state`都没发生变化，`render`方法就不会被触发。从而省去了`Virtual DOM`的生成和对比过程，达到了提升渲染效率的目的。

下面是PureComponent对比`props`和`state`的浅比较
```JavaScript
  if (this._compositeType === CompositeTypes.PureClass) {
    shouldUpdate = !shallowEqual(prevProps, nextProps) || ! shallowEqual(inst.state, nextState);
  }
```

下面是shallowEqual的源码
```javascript
  'use strict';

  const hasOwnProperty = Object.prototype.hasOwnProperty;

  /**
   * inlined Object.is polyfill to avoid requiring consumers ship their own
   * https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/is
   */
  function is(x: mixed, y: mixed): boolean {
    // SameValue algorithm
    if (x === y) { // Steps 1-5, 7-10
      // Steps 6.b-6.e: +0 != -0
      // Added the nonzero y check to make Flow happy, but it is redundant
      return x !== 0 || y !== 0 || 1 / x === 1 / y;
    } else {
      // Step 6.a: NaN == NaN
      return x !== x && y !== y;
    }
  }
  /**
   * Performs equality by iterating through keys on an object and returning false
   * when any key has values which are not strictly equal between the arguments.
   * Returns true when the values of all keys are strictly equal.
   */
  function shallowEqual(objA: mixed, objB: mixed): boolean {
    if (is(objA, objB)) { // 如果 ===，返回 true
      return true;
    }


    if (typeof objA !== 'object' || objA === null ||
        typeof objB !== 'object' || objB === null) {
      return false;
    }

    const keysA = Object.keys(objA);
    const keysB = Object.keys(objB);

    if (keysA.length !== keysB.length) {
      return false;
    }

    // Test for A's keys different from B.
    for (let i = 0; i < keysA.length; i++) {
      if (
        !hasOwnProperty.call(objB, keysA[i]) || // A B 中包含相同元素且相等，函数也是直接用 === 来比较
        !is(objA[keysA[i]], objB[keysA[i]])
      ) {
        return false;
      }
    }

    return true;
  }
```

`shallowEqual` 会对Object的每个属性，如果是非引用类型，那么可以直接比较判断是否相等。如果是引用类型，则通过比较引用的地址进行严格比较（===）。当 props 和 state 是不可突变数据的时候，可以直接使用 PureComponent。

> 使用方法

将原组件继承自React.Component,替换为React.PureComponent

```JavaScript
  import React, { PureComponent } from React

  class Foo extend PureComponent {
    render() {
      // ...
    }
  }

  export default Foo
```

### ImmutableJS

我们也可以在`shouldComponentUpdate()`中使用深拷贝来避免无必要的 render()，但deepCopy 和 deepCompare 一般都是非常耗性能的。FaceBook官方提出的不可变数据[ImmutableJS](https://facebook.github.io/immutable-js/)，使用structural sharing解决了复杂数据在 deepCopy 和 deepCompare过程中性能损耗的问题。

ImmutableJS拥有 Persistent Data Structure（持久化数据结构）与 structural sharing（结构共享）的特点。持久化数据结构保证数据一旦创建就不能修改，使用旧数据创建新数据时，旧数据也不会改变。而结构共享是指没有改变的数据共用一个引用，这样既减少了深拷贝的性能消耗们也减少了内存开销。

![](/source/img/javascript/immutable-structural-sharing.png)

左边红色节点表示变化的值，右边生成新值改变了红色节点到根节点路径之间的所有节点，也就是所有绿色节点的值，其他使用它的地方并不会受到影响，而且蓝色节点还是和旧值共享的。即如果对象树中一个节点发生变化，只修改这个节点和受它影响的父节点，其它节点则进行共享。

`ImmutableJS` 提供了大量的方法去更新、删除、添加数据，极大的方便了我们操纵数据。除此之外，还提供了原生类型与 `ImmutableJS` 类型判断与转换方法 

```javascript
import { Map, fromJS, isImmutable  } from "immutable";
const map1 = Map({ a: 'a', b: 'b', c: 'c' });
const map2 = map1.set('b', 'bb');
map1 !== map2; // true
map1.get('b'); // b
map2.get('b'); // bb
map1.get('a') === map2.get('a'); // true

const object = fromJS({
  a: 'a',
  b: [1, 2, 3, 4]
}); // 支持混合类型
isImmutable(obj); // true
obj.size(); // 2
const obj1 = obj.toJS(); // 转换成原生 `js` 类型
```


Immutable 则提供了简洁高效的判断数据是否变化的方法，只需 === 和 is 比较就能知道是否需要执行 render()，而这个操作成本很低，所以可以极大提高性能。我们可以使用is来控制 shouldComponentUpdate：

```javascript
import { is } from 'immutable';

shouldComponentUpdate: (nextProps = {}, nextState = {}) => {
  const thisProps = this.props || {}, thisState = this.state || {};

  if (Object.keys(thisProps).length !== Object.keys(nextProps).length ||
      Object.keys(thisState).length !== Object.keys(nextState).length) {
    return true;
  }

  for (const key in nextProps) {
    if (!is(thisProps[key], nextProps[key])) {
      return true;
    }
  }

  for (const key in nextState) {
    if (thisState[key] !== nextState[key] || !is(thisState[key], nextState[key])) {
      return true;
    }
  }
  return false;
}
```


### 实现 shouldComponentUpdate
我们知道`PureComponent`的`shadowEqual`只会浅检查组件的`props`和`state`,所以嵌套对象和数组是不会被比较的。所以如果是需要深比较，我们也可以使用`shouldComponentUpdate`来手动单个比较是否需要重新渲染。

`shouldComponentUpdate`是`props`或`state`更改后、render前被调用的方法。如果`shouldComponentUpdate`返回`true`，`render`将被调用，如果它返回`false`，则组件不会重新渲染。

所以在一些情况下，你可以通过重写这个生命周期函数`shouldComponentUpdate`来提升渲染效率。这个函数默认返回`true`。如果你知道在某些情况下你的组件不需要更新，你可以在`shouldComponentUpdate`内返回`false`来跳过整个渲染进程。

```javascript
  shouldComponetUpdate(nextProps, nextState) {
    if (/* do some compare*/) {
        reture true;
    }  
  }
```

### plugin-transform-react-inline-elements
[(Reference)Optimizing Compiler: Inline React Elements](https://github.com/facebook/react/issues/3228)

在production环境，优化`React.createElement` function为`babelHelpers.jsx`

> In

```javascript
  <Baz foo="bar" key="1"></Baz>
```

> Out

```javascript
  babelHelpers.jsx(Baz, {
      foo: "bar"
  }, "1");

  /**
  * Instead of
  */

  React.createElement(Baz, {
    foo: "bar",
    key: "1",
  });

```

### 使用稳定的 `key`

> 使用稳定的 key, 对子组件进行唯一性识别，准确知道要操作的自组件，提高DOM DIFF的效率

* 唯一的: 元素的 key 在它的兄弟元素中应该是唯一的（而非全局唯一）
* 稳定的: 元素的 key 不应随时间、页面刷新或者元素重新排序而变

当使用数组中的索引作为 key时，若元素没有重排，该方法效果不错。但是如果其中的item被删除或移动导致重排,则整个key 就失去了与原项的对应关系，加大了diff的开销。

### 无状态组件

React官方在0.14版本中加入了`无状态组件`, 这种组件没有状态，没有生命周期，接受props渲染生成DOM结构。无状态组件非常简单，开销很低，如果一个组件不需要管理state只是纯展示，那么就可以定义成无状态组件。

```javascript
const Component (props) => (
    <div>
        {props.text}
        ...
    </div>
)
```

因为无状态组件只是函数，没有实例返回，所以没有refs属性。


## 参考
* [Tool: Why Did You Update](https://github.com/maicki/why-did-you-update)
  
* [React 渲染机制](https://react.docschina.org/docs/optimizing-performance.html)

* [React 性能优化](https://reactjs.org/docs/optimizing-performance.html)

* [ImmutableJS](https://facebook.github.io/immutable-js/)
