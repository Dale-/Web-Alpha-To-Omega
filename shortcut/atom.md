## 快捷键

### 文件
`ctrl-shift-s`  保存所有打开的文件  
`cmd-shift-o`  打开目录  
`cmd-alt-\`   显示或隐藏目录树  
`cmd-t`或`cmd-p` 查找文件  
`cmd-b` 在打开的文件之间切换  
`cmd-up` 移动到文件开始
`cmd-down` 移动到文件结束

<!-- more -->

### Atom大小写转换
`cmd-u` 使当前字符大写

### 折叠
`alt-cmd-[` 折叠
`alt-cmd-]` 展开
`alt-cmd-shift-{` 折叠全部  
`alt-cmd-shift-}` 展开全部

### Markdown预览
`ctrl-shift-M` Markdown预览

### 选取
`cmd-shift-D` 复制一行代码
`ctrl-shift-P`  选取至上一行  
`ctrl-shift-N`  选取至下一样  
`ctrl-shift-B`  选取至前一个字符  
`ctrl-shift-F`  选取至后一个字符  
`alt-shift-B`, `alt-shift-left`  选取至字符开始  
`alt-shift-F`, `alt-shift-right`  选取至字符结束  
`ctrl-shift-E`, `cmd-shift-right`  选取至本行结束  
`ctrl-shift-A`, `cmd-shift-left`  选取至本行开始  
`cmd-shift-up`  选取至文件开始  
`cmd-shift-down`  选取至文件结尾  
`cmd-L`  选取一行，继续按回选取下一行  
`ctrl-shift-W`  选取当前单词  

### 移动
`ctrl-cmd-up` 当前行向上移动
`ctrl-cmd-down` 当前行向下移动

### 查找和替换  
`cmd-F` 在buffer中查找  
`cmd-shift-f` 在整个工程中查找  

### 创建
`a` 快速创建文件(current directory)
`shift-a` 快速创建文件夹(current directory)

---

## 插件

### atom-beautify
> 其他编辑器都支持代码格式化，以便阅读。atom-beautify插件提供类似的功能用于美化代码，可以对html、css、js等文件进行格式美化。使用时需要先选中要美化的代码

### file-icons
> 这个插件可以对不同类型的文件显示不同的文件图标，包含在目录树里面和标签页里面都会显示。

### git-plus
> git的增强插件，实现在Atom界面下完成Git Bash的一般操作

### linter-eslint
> 该插件是用`jshint`来检查代码，想必大家都听说过`jshint`代码检查工具，它有一个配置文件.jshintrc，这个文件告诉`jshint`执行的检查规则。通过`jshint`能发现代码中存在的问题，可以及时避免bug的发生。linter-jshint插件基于atom规则来使用`jshint`，该插件可以在项目根目录下新建一个.jshintrc来告诉检查规则，也可以不用创建此文件来进行代码检查。
注意：linter-jshint是依赖linter插件来使用的，也就是说必须先安装linter插件；因为linter是一个粗糙的检查，有很多针对专门项的代码检查，如linter-csslint、linter-php等等

### pigments
> 当你编写很多`CSS`时，`Pigments`会很方便，能够一目了然根据颜色代码显示颜色，你再不必拷贝粘贴颜色十六进制代码到其他图形编辑工具中以查看其颜色，甚至能够和SaSS变量一起工作，如果你在另外一个`SASS`文件中定义了一个变量的颜色，那么这个变量本身将会以它定义的颜色显示。

### emmet
> 这款插件是用来支持`zend-coding`，Emmet的前身是大名鼎鼎的Zen coding，如果你从事Web前端开发的话，对该插件一定不会陌生。它使用仿CSS选择器的语法来生成代码，大大提高了HTML/CSS代码编写的速度

### js-hyperclick
> 首先安装`hyperclick`, 实现按住command + 单击跳转到函数定义处或引入文件。再安装你所用的x-hyperclick，比如我用js，就安装js-hyperclick，其他语言同理。如果需要支持非本文件内的跳转，需要安装[goto-definition](https://github.com/faceair/atom-goto-definition)
