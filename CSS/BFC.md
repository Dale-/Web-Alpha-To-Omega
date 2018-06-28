### BFC (Block Formatting Context)

## 定义
BFC, 全称是block formatting context，它是一个独立封闭的渲染区域，在这个区域内的所有元素，从区域的顶部起，一个接一个地根据自身的布局特性进行排列：在这个区域内的块级元素 ，按从上到下的顺序显示，相邻的块级元素可以使用margin隔离，但在垂直方向上相邻的块级元素会发生margin合并；在这个区域内的inline-level或inline-level-block元素，则按从左到右的顺序显示（W3C组织说BFC内部的元素都是一个接一个地垂直显示，我觉得不是很严格，因为BFC内部也可以容纳inline-level和inline-level-block元素，所以这里我的解释和W3C还是稍微有一些不一样）。具有BFC格式化环境的元素，我们称之为BFC元素，可以说，BFC定义了BFC元素content区域的渲染规则。

## 创建方式
* 根元素或其它包含它的元素
* 浮动元素 (元素的 float 不是 none)
* 绝对定位元素 (元素的 position 为 absolute 或 fixed)
* 内联块 (元素具有 display``: inline-block)
* 表格单元格 (元素具有 display``: table-cell，HTML表格单元格默认属性)
* 表格标题 (元素具有 display``: table-caption, HTML表格标题默认属性)
* overflow 值不为 visible 的块元素
* display``: flow-root
* contain为以下值的元素: layout, content, 或 strict
* 弹性项 (display``: flex 或 inline-flex元素的子元素)
* 网格项 (display``: grid 或 inline-grid 元素的子元素)
* 多列容器 (元素的 column-count 或 column-width 不为 auto， 包括 column-count: 1的元素)
* column-span``: all应当总是会创建一个新的格式化上下文，即便具有 column-span: all 的元素并不被包裹在一个多列容器中。
