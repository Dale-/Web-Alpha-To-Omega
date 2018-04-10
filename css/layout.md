## 前端布局方案

* 传统布局方案（借助浮动、定位等手段）
* flex布局方案
* grid布局方案

### 布局基础要点

> CSS标准盒模型

一个web页面是由众多html元素拼凑而成的，而每一个html元素，都被解析为一个矩形盒，而CSS盒模型就是这种矩形盒的解构模型。CSS盒模型，它由内到外、被四条边界Content edge、Padding edge、Border edge和Margin edge划分为四个区域：Content area、Padding area、Border area和Margin area，在形状上，Content area（又称content-box）是实心矩形，其余是空心环形（空心部分是Content area）。

![](/source/img/css/layout/css-model.png)

此外，每个区域都有其特定的作用：Content area，是当前元素用来容纳所有子孙元素；Padding area，是当前元素用来隔离自身和子孙元素；Border area是当前元素用来显示自身的轮廓；Margin area，是当前元素用来隔离自身和相邻元素。理解每个区域的作用和职责至关重要，有助于我们写出优雅、清晰的布局代码。

> box-sizing (CSS3属性)

### 1. box-sizing的作用
