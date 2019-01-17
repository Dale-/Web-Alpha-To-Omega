## React渲染机制
React不直接操作真实DOM，它在内部维护了一套快速相应的虚拟DOM(本篇文章简称为VDOM)，`render`方法返回一个VDOM的描述，React会在reconsilation之后最小化的进行VDOM的更新，最终patch到真实的DOM。

下面官网给出的React组件渲染机制描述图
![](/source/img/javascript/react-update.png)

![](/source/img/javascript/react-render-flow.png)

通过上述流程图，再对比渲染机制描述图我们可以发现，性能优化的两个着手点：`shouldComponentUpdate`和 `DOM DIFF`的结果。

* shouldComponentUpdate阶段，如果props与state与上一次相同，这个时候可以中断后续的生成虚拟DOM以及DOM DIFF的过程。
* 提高DOM DIFF的效率

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

![](/source/img/javascript/usage-of-why-did-you-update.png)

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

*这些数字是相对的，因此组件在生产版本中会运行更快*。然而，这也能够帮助你了解何时会有无关的组件被错误的更新，以及你的组件更新的深度和频率。

## 优化方法
### PureComponent
> Component的默认渲染行为

在React Component的生命周期中，又一个`shouldComponentUpdate`方法，这个方法的默认返回值为`true`。

这意味着Component不会比较对当前和下一个状态下的`props`和`state`, 因此每当`shouldComponentUpdate`被调用时，组件默认会重新渲染。从而导致组件因为不相关数据的改变导致重绘，这极大的降低了React的渲染效率。

> PureComponent

当组件更新时，如果组件的`prop`和`state`都没发生变化，`render`方法就不会被触发。从而省去了`Virtual DOM`的生成和对比过程，达到了提升渲染效率的目的。

下面是PureCompoent对比`props`和`state`的浅比较
![](/source/img/javascript/shallow-equal.png)

`shallowEqual`会比较`Object.keys(state | props)`的长度是否一致，每一个`key`是否两者都有，并且比对是否为同一个引用。所以`shallowEqual`比较的是第一层的值，而非深层的嵌套数据对比。

> 使用方法

将原组件继承自React.Component,替换为React.PureComponent

![](/source/img/javascript/pure-component.png)

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
