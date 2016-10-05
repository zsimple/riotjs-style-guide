# RiotJS风格指南

Opinionated *RiotJS Style Guide* for teams by [De Voorhoede](https://twitter.com/devoorhoede).

[![RiotJS Style Guide badge](https://cdn.rawgit.com/voorhoede/riotjs-style-guide/master/riotjs-style-guide.svg)](https://github.com/voorhoede/riotjs-style-guide)


## 目的

本指南提供了一种统一的方式来组织你的[RiotJS]（http://riotjs.com/）代码。它的好处：

* 开发人员/团队成员了更容易理解和寻找；
* 更容易为IDE所理解的代码；
* 更容易重复使用你创建的组件；
* 更容易缓存和分发代码；

本指南受[AngularJS风格指南]（https://github.com/johnpapa/angular-styleguide）John Papa 的启发。

## 演示

我们的[RiotJS演示（https://github.com/voorhoede/riotjs-demos#riotjs-demos-）是一个伴侣本指南，说明与实际案例的指导方针。

## 目录

* [基于模块的开发](#module-based-development)
* [Tag 模块名称](#tag-module-names)
* [1模块 = 1目录](#1-module--1-directory)
* [使用`* .tag.html`扩展名](#use-taghtml-extension)
* [在 tag 中使用`<script>`标签](#use-script-inside-tag)
* [保持 tag 表达式简单](#keep-tag-expressions-simple)
* [保持 tag 选项简单](#keep-tag-options-primitive)
* [驾驭您的 tag 选项](#harness-your-tag-options)
* [把 this 赋值为 `tag`](#assign-this-to-tag)
* [放置 tag 属性和方法在顶部](#put-tag-properties-and-methods-on-top)
* [避免使用伪 ES6 语法](#avoid-fake-es6-syntax)
* [避免使用 `tag.parent`](#avoid-tagparent)
* [使用`each ...in`语法](#use-each--in-syntax)
* [将样式放入外部文件](#put-styles-in-external-files)
* [使用 tag 的名称作为风格范围](#use-tag-name-as-style-scope)
* [为 tag API 书写文档](#document-your-tag-api)
* [添加 tag 演示](#add-a-tag-demo)
* [检查你的 tag 文件](#lint-your-tag-files)
* [添加徽章到您的项目](#add-badge-to-your-project)


## 基于模块的开发

始终构建您的应用程序出来的小模块，做一件事，把它做好。

模块是应用程序的小的自包含的部分。该RiotJS微架构是专门设计来帮助您创建*视图逻辑模块*，这防暴要求*标记*。

###为什么呢？

小模块更容易学习，理解，维护，重用和调试。无论您和其他开发人员。

＃＃＃ 怎么样？

每个标签骚乱（如任何模块）必须是[首页]（https://addyosmani.com/first/）：* *聚焦（[单一职责（http://en.wikipedia.org/wiki/Single_responsibility_principle）） *独立*，*可重复使用*，*小*和*可测试*。

如果你的模块不太多或变得太大，最多把它分解成更小的模块，其中每个做只是事情。
作为一个经验法则，尽量保持每个标签文件小于100行代码。
此外，还要确保您的标签模块中的隔离工作。例如通过加入一个独立的演示。

** **提示：如果您使用通过一个[AMD]（https://github.com/amdjs/amdjs-api/blob/master/AMD.md）或CommonJS的模块加载，你需要[编译标签`--modular`（`-m`）标志（http://riotjs.com/guide/compiler/#amd-and-commonjs）：
```庆典
＃使AMD和CommonJS的
防暴--modular
```

[↑回到目录]（＃表的，内容）


##标签模块名称

一个标记模块是一个特定类型的模块，包含防暴标签。

每个模块的名称必须是：

* ** **有意义：不overspecific，不是过于抽象。
* ** **短：2或3个字。
* ** ** Pronouncable：我们希望能够谈论他们。

标签模块名称也必须是：

* **自定义元素遵循规范的**：包括一个连字符（https://www.w3.org/TR/custom-elements/#concepts），不要使用保留名称。
* **`应用程式`命名空间**：如果非常通用和否则为1字，以便它可以容易地在其他项目中重复使用。

###为什么呢？

*该名称用于对模块进行通信。所以它必须是短的，有意义的和pronouncable。
*本`tag`元件被插入到DOM。因此，他们需要坚持规范。

＃＃＃ 怎么样？

```HTML
<！ - 推荐 - >
<应用头/>
<用户列表/>
<范围滑块/>

<！ - 避免 - >
<BTN-组/> <！ - 短，但unpronouncable。使用`按钮group`代替 - >
<UI滑块/> <！ - 所有标签都UI元素，所以是没有意义的 - >
<滑块/> <！ - 不是自定义元素遵循规范 - >
```

[↑回到目录]（＃表的，内容）


## 1模块= 1目录

其中捆绑构建模块到一个地方的所有文件。

###为什么呢？

捆绑模块文件（防暴标签，测试，资产，文档等），使他们很容易找到，移动和重复使用。

＃＃＃ 怎么样？

使用模块名作为目录名和文件名前缀。
文件扩展名取决于文件的目的。

```
模块/
└──我，例如/
    ├──我-example.tag.html
    ├──我-example.less
    ├──...
    └──README.md
```

如果你的项目使用嵌套结构，可以嵌套在模块中的一个模块。
例如一个通用的`无线电group`模块可以被直接放置内部“模块/”。而`搜索-filters`可仅使一个`搜索-form`内感，并且可以因此被嵌套：

```
模块/
├──无线电组/
| └──无线电group.tag.html
└──搜索表单/
    ├──搜索form.tag.html
    ├──...
    └──搜索过滤器/
        └──搜索filters.tag.html
```

[↑回到目录]（＃表的，内容）


##使用`* .tag.html`扩展

防暴引入了一个名为* *标签新概念，并建议使用`* .tag`扩展。
然而，在本质上，这些标签包含标记只是自定义元素。因此，你应该**使用`* .tag.html`扩展**。

###为什么呢？

*告诉开发商这不仅仅是HTML，但防暴标签元素。
*提高IDE支持（信号如何解释）。

＃＃＃ 怎么样？

在[浏览器内编辑]的情况下（http://riotjs.com/guide/compiler/#in-browser-compilation）：
```HTML
<SCRIPT SRC =“路径/到/模块/我的，例如：/我-example.tag.html”TYPE =“防暴/标签”> </ SCRIPT>
```

在[预编译]（http://riotjs.com/guide/compiler/#pre-compilation），设置[自定义扩展]的情况下（http://riotjs.com/guide/compiler/#custom-extension ）：
```庆典
防暴--ext tag.html模块/距离/ tags.js
```

如果您使用的[标签WebPACK中加载程序（https://github.com/srackham/tag-loader），[配置装载机（http://webpack.github.io/docs/using-loaders。 HTML＃配置）相匹配的扩展：
```的JavaScript
{测试：/\.tag.html$/，装载机：'标签'}
```

[↑回到目录]（＃表的，内容）


##使用`<script>`标记内

虽然防暴支持写入标签元素中的JavaScript [未经`<SCRIPT>`]（http://riotjs.com/guide/#no-script-tag）
你应该总是**使用`<script>`**围绕脚本。这是更接近Web标准，并防止混乱开发商和集成开发环境。

###为什么呢？

*防止标记被解释为脚本。
*提高IDE支持（信号如何解释）。
*告诉开发商标记的地方停和脚本开始。

＃＃＃ 怎么样？

```HTML
<！ - 推荐 - >
<我的，例如>
<H1>年是{this.year} </ H1>

<SCRIPT>
this.year =（新的Date（））调用getUTCFullYear（）。
</ SCRIPT>
</我的，例如>

<！ - 避免 - >
<我的，例如>
<H1>年是{this.year} </ H1>

this.year =（新的Date（））调用getUTCFullYear（）。
</我的，例如>
```

[↑回到目录]（＃表的，内容）


##保持标签的表达简单

防暴的直列[表情]（http://riotjs.com/guide/#expressions）100％的JavaScript。这使得他们extemely强大，但可能也很复杂。因此，你应该保持**表达标签**简单。

###为什么呢？

*复杂的内联表达式难以阅读。
*内嵌表达式不能elsewehere重用。这可能会导致代码重复和代码腐烂。
*集成开发环境通常不具有表达式语法的支持，让您的IDE无法自动完成或验证。

＃＃＃ 怎么样？

将复杂的表达式标记的方法或标记属性。

```HTML
<！ - 推荐 - >
<我的，例如>
{年（）+' - '+月（）}

<SCRIPT>
常量twoDigits =（NUM）=>（'0'+ NUM）.slice（-2）;
this.month =（）=> twoDigits（（新的Date（））getUTCMonth（）+1）;
this.year =（）=>（新的Date（））调用getUTCFullYear（）。
</ SCRIPT>
</我的，例如>

<！ - 避免 - >
<我的，例如>
{（新的Date（））调用getUTCFullYear（）+' - '+（'0'+（（新的Date（））getUTCMonth（）+ 1））片（-2）}
</我的，例如>
```

[↑回到目录]（＃表的，内容）


##保持标记选项原始

传球选择防暴支持标记使用的标签元素属性的实例。标签实例中这些选项都可以通过`opts`。例如`我-attr`对价值`<我的标签我-ATTR =“{}值”/>`将通过`opts.myAttr`可用`里面我-tag`。

虽然防暴支持通过这些属性通过复杂的JavaScript对象，你应该尽量保持**标签选项原始尽可能**。尽量只使用[JavaScript的原语（https://developer.mozilla.org/en-US/docs/Glossary/Primitive）（字符串，数字，布尔值）和功能。避免复杂的对象。

这一规则的例外是你的应用程序内只能使用对象来解决的情况下（如集合或递归标签）或知名对象（例如，在网上商店的产品）。

###为什么呢？

*通过使用每个选项的属性分别标记有一个明确的和表现的API。
*通过作为选项值我们的标签API是类似于天然HTML（5）元素的API只使用原语和功能。这使得我们自定义元素直接熟悉。
*通过使用每个选项的属性，其他开发人员可以轻松地理解传递给标签实例。
*当传递复杂的对象是看不出来的这实际上是正在使用的自定义标签属性和对象的方法。这使得它很难重构代码，并可能导致代码腐烂。

＃＃＃ 怎么样？

使用每个选项标记属性，具有原始的或功能价值：

```HTML
<！ - 推荐 - >
<范围滑块
值=“[10，20]”
分=“0”
最大=“100”
步=“5”
在幻灯片=“{} updateInputs”
上月底=“{} updateResults”
/>

<！ - 避免 - >
<范围滑块配置=“{complexConfigObject}”>
```
```HTML
<！ - 例外：递归标记，如菜单 - >
<菜单项>
<a href="{ opts.url }"> {} opts.text </A>
<UL如果=“{opts.items}”>
<LI每个=“{在opts.items项目}”>
<菜单项
文本=“{} item.text”
URL =“{} item.url”
项=“{} item.items”/>
</ li>
</ ul>
</菜单项>
```

[↑回到目录]（＃表的，内容）


##驾驭您的标记选项

在防暴您的标记选项是你的API。一个强大的和可预见的API，使您的代码很容易被其他开发者使用。

标签选项通过自定义HTML属性传递。这些属性的值可以是防暴表达式（`ATTR =“{VAR}”`）或普通字符串（`ATTR =“值”`）或完全丢失。你应该充分利用**的标记选项**考虑到这些不同的情况。

##为什么呢？

利用你的标记选项可以确保您的标记功能将始终（防御性编程）。即使当其他开发者以后使用您的代码的方式你有没有想过呢。

＃＃ 怎么样？

*使用默认设置选项值。
*使用类型转换投选项值与预期的类型。
*检查是否选择使用它之前存在。

例如[暴乱的`<TODO>`示例]（http://riotjs.com/guide/#example）可以改进也工作，如果没有'items`提供，通过使用默认：

```的JavaScript
this.items = opts.items || []; //默认为空列表
```
确保不同的使用情况下，所有的工作：
```HTML
<待办事项=“{[{标题：'苹果'}，{标题：”橙子“，做到：真正}]}”> <！ - 使用定列表 - >
<TODO> <！ - 使用空列表 - >
```

该`<范围滑块>`在[标签保持原始的选项（https://github.com/voorhoede/riotjs-style-guide#keep-tag-options-primitive）预计号码`min`，`max`和`step`。使用类型转换：

```的JavaScript
//如果步骤的选择是有效的，用作数字，否则默认为一个。
this.step =！isNaN（编号（opts.step））？号（opts.step）：1;
```
确保不同的使用情况下，所有的工作：
```HTML
<范围滑块> <！ - 使用默认 - >
<范围滑块步=“5”> <！ - “5”转换为数字5 - >
<范围滑块步=“{X}”> <！ - 尝试使用`x` - >
```

该`<范围滑盖>`还支持可选* *`上slide`和`上end`回调。检查选项存在，是预期的格式，在使用它之前：

```的JavaScript
slider.on（'滑'，（数值，手柄）=> {
如果（typeof运算opts.onSlide ==='功能'）{
opts.onSlide（值，手柄）;
}
}
```
确保不同的使用情况下，所有的工作：
```HTML
<范围滑块> <！ - 什么都不做 - >
<范围滑块上滑动=“{} updateInputs”> <！ - 呼吁幻灯片updateInputs - >
<范围滑块上滑动=“无效选项”> <！ - 什么都不做 - >
```

[↑回到目录]（＃表的，内容）


##`分配到this``tag`

在一个防暴标签元素的背景下，'this`绑定到[标签实例]（http://riotjs.com/api/#tag-instance）。
因此，当你需要引用它在不同的情况下，确保`this`可作为`tag`。

###为什么呢？

*通过分配`this`到一个名为变量`tag`的变量告诉开发商它绑定到[标签实例]（http://riotjs.com/api/#tag-instance）无论它的使用。

＃＃＃ 怎么样？

```的JavaScript
/* 推荐的 */
// ES5：`分配给this``tag`变量
var标记=这一点;
window.onresize =功能（）{
    tag.adjust（）;
}

// ES6：`分配给this``tag`不变
常量标签=这一点;
window.onresize =功能（）{
    tag.adjust（）;
}

// ES6：你仍然可以使用`this`脂肪箭头
window.onresize =（）=> {
    this.adjust（）;
}

/ * *避免/
VAR自我=这一点;
VAR _this =这一点;
//等
```

[↑回到目录]（＃表的，内容）


##将代码属性，在上面的方法

骚乱标签元素在里面你通常把它标记首位，其次是它的脚本。
特性和在脚本结合到标签（`this`）方法是在标记直接可用。你应该把在脚本的顶部按字母顺序排列的标签属性和方法。比单行标记方法不再需要在脚本后链接到不同的功能。

###为什么呢？

*放置标签的属性和方法在脚本的顶部，让你瞬间识别标签的部分可以在标记中使用。
*按字母顺序排列的属性和方法，使他们很容易找到。
*通过保持每一个标签的方法或属性声明一个班轮你可以一目了然地获取标签的完整概述。
*通过移动标记方法背后的全部功能了，你最初隐藏实现细节。

＃＃＃ 怎么样？

放在上面的标签属性和方法：

```的JavaScript
/ *建议：按字母顺序排列的属性然后方法* /
var标记=这一点;
tag.text ='';
tag.todos = [];
tag.add =增加;
tag.edit =编辑;
tag.toggle =切换;

函数add（事件）{
    / * ... * /
}

编辑功能（事件）{
    / * ... * /
}

功能切换（事件）{
    / * ... * /
}

/ *忌：不散开标签的属性和方法在脚本* /
var标记=这一点;

tag.todos = [];
tag.add =函数（事件）{
    / * ... * /
}

tag.text ='';
tag.edit =函数（事件）{
    / * ... * /
}

tag.toggle =函数（事件）{
    / * ... * /
}
```
    
也把混入和顶级观测起来：

```的JavaScript
/* 推荐的 */
var标记=这一点;
//按字母顺序排列的属性
//按字母顺序排列的方法
tag.mixin（'someBehaviour'）;
tag.on（'装'，onMount）;
tag.on（'更新'，的onUpdate）;
//等
```

[↑回到目录]（＃表的，内容）


##，避免假ES6语法

防暴支持[速记* ES6像*方法的语法（http://riotjs.com/guide/#tag-syntax）。防暴编译语法速记`方法名（）{}``成= this.methodName功能（）{} .bind（本）`。由于这是不规范的，你应该避免**假ES6方法简写语法**。

###为什么呢？

*假ES6简写语法是非标准的，因此可以混淆开发商。
*标签脚本不是实际ES6类，所以集成开发环境就无法解释假ES6类方法的语法。
*它应始终清楚哪些方法绑定到标签，从而在标记可用。速记语法掩盖写代码是透明的，容易理解的原则。

＃＃＃ 怎么样？

使用`tag.methodName =`而不是魔法`方法名（）{}`的语法：

```的JavaScript
/* 推荐的 */
var标记=这一点;
tag.todos = [];
tag.add =增加;

函数add（）{
    如果（tag.text）{
        tag.todos.push（{标题：tag.text}）;
        tag.text = tag.input.value ='';
    }
}

/ * *避免/
待办事项= [];

加（）{
    如果（this.text）{
        this.todos.push（{标题：this.text}）;
        this.text = this.input.value ='';
    }
}
```

预编译期间[假ES6语法禁用改造（http://riotjs.com/guide/compiler/#no-transformation）由type`设置``到none`：** **提示：

```庆典
防暴--type无
```

[↑回到目录]（＃表的，内容）


##，避免`tag.parent`

防暴支持[嵌套标签（http://riotjs.com/guide/#nested-tags），其中有通过`tag.parent`访问他们的父母背景。访问上下文广告代码模块的外部违反[模块为基础的发展（＃模块为基础开发）的[首页]（https://addyosmani.com/first/）规则。因此，你应该**避免使用`tag.parent` **。

该例外是在[for each循环]匿名子标签（http://riotjs.com/guide/#loops），因为它们是在标签模块中直接定义。

###为什么呢？

*一个标签模块，如任何模块，必须隔离工作。如果一个标签需要访问它的父，这条规则被打破了。
*如果一个标签需要访问到它的父，它不再能够在不同的上下文重用。
*通过访问其母公司的子标签，可在其父修改属性。这可能会导致意外的行为。

＃＃＃ 怎么样？

*从父使用属性表达式子标记传递值。
*父标签使用回调在属性表达式子标记定义的通过方法。

```HTML
<！ - 推荐 - >
<父标签>
<子标签值=“{}值”/> <！ - 通父母的价值孩子 - >
</父标签>

<子标签>
<SPAN> {} opts.value </ SPAN> <！ - 由父母传给使用价值 - >
</子标签>

<！ - 避免 - >
<父标签>
<子标签/>
</父标签>

<子标签>
<SPAN>值：{} parent.value </ SPAN> <！ - 不要这样做！ - >
</子标签>
```
```HTML
<！ - 推荐 - >
<父标签>
<子标签上的事件=“{} methodToCallOnEvent”/> <！ - 使用方法回调 - >
<SCRIPT> this.methodToCallOnEvent =（）=> {/*...*/}; </ SCRIPT>
<父标签>

<子标签>
<按钮的onclick =“{} opts.onEvent”> </按钮> <！ - 由父母传给调用方法 - >
</子标签>

<！ - 避免 - >
<父标签>
<子标签/>
<SCRIPT> this.methodToCallOnEvent =（）=> {/*...*/}; </ SCRIPT>
<父标签>

<子标签>
<按钮的onclick =“{} parent.methodToCallOnEvent”> </按钮> <！ - 不这样做 - >
</子标签>
```
```HTML
<！ - 允许例外 - >
<父标签>
<按钮，每个=“{中的项目项目}”
的onclick =“{} parent.onEvent”> <！ - 没关系，因为按钮是不是暴乱标签 - >
{} item.text
</按钮>
<SCRIPT> this.onEvent =（E）=> {警报（e.item.text）; } </ SCRIPT>
</父标签>
```

[↑回到目录]（＃表的，内容）


##使用`每... in`语法

防暴支持多个符号[循环]（http://riotjs.com/guide/#loops）：在数组项（每``=“{中的项目项目}”）;键，对象值（每`=“{项目中键，值}”`）和速记（`每个=“{}项目”`）符号。这种速记会导致混乱。因此，你应该**使用`各... in`语法**。

###为什么呢？

骚乱的`each`指令遍历每个项目创建一个新的标签实例。当使用速记符号，目前项目的方法和属性都绑定到当前标签实例（本地`this`）。这不是明显在寻找标记，因此可能会混淆其他开发商的时候。因此，你应该**使用`各... in`语法**。

＃＃＃ 怎么样？

使用`每个=“{中的项目项目}”`或`每个=“{项目中键，值}”'，而不是'每个=“{}项目”`语法：

```HTML
<！ - 推荐： - >
<ul>
    <LI每个=“{中的项目项目}”>
      <标签类=“{完成：item.done}”>
<INPUT TYPE =“复选框”选中=“{} item.done”> {} item.title
      </标签>
    </ li>
</ ul>

<！ - 推荐： - >
<ul>
    <LI每个=“{键，项项}”>
      <标签类=“{完成：item.done}”>
<INPUT TYPE =“复选框”选中=“{} item.done”> {}键。 {} item.title
      </标签>
    </ li>
</ ul>

<！ - 忌： - >
<ul>
    <LI每个=“{项目}”>
      <标签类=“{完成：完成}”>
<INPUT TYPE =“复选框”选中=“{}做”> {}称号
      </标签>
    </ li>
</ ul>
```

[↑回到目录]（＃表的，内容）


##将在外部文件中风格

对于开发商的方便，防暴允许你在一个定义标签元素的样式[嵌套`<STYLE>`标记（http://riotjs.com/guide/#tag-styling）。虽然你可以[适用范围]（http://riotjs.com/guide/#scoped-css）这些样式的标记元素，防暴并不提供真正的封装。相反，从骚乱的标签（JavaScript）的提取这些样式，并将其注入到上运行的文件。由于防暴编译嵌套样式JavaScript和不具有真正的封装，您应该把**外部文件中风格**。

###为什么呢？

*外部样式表可以通过浏览器独立骚乱和标记文件进行处理。这意味着样式可以应用到初始的标记，即使发生JavaScript错误或不加载（还）。
*外部样式表可以组合使用预处理器（都不能少，萨斯，PostCSS等），你自己的（现有的）构建工具。
*外部样式表可以缩小的，送达和缓存分开。这样可以提高性能。
*防暴表达式在嵌套的`<STYLE>单曲，所以没有额外的好处，在使用它们的支持。

＃＃＃ 怎么样？

相关的标签和其标记样式，应放置在一个单独的样式表文件旁边的标记文件，其模块目录中：

```
我-例子/
├──我-example.tag.html
。├──我，例如（CSS |少| SCSS）< - 外部旁边标记文件的样式表
└──...
```

[↑回到目录]（＃表的，内容）


##使用标签名，风格范围

防暴标签元素是可以很好地用来作为风格的范围根自定义元素。
可替代地，模块名称可被用作CSS类命名空间。

###为什么呢？

*划定范围的风格标签元素提高可预测性为防止风格标签元素外泄漏。
*为模块目录使用相同的名称，防暴标签和风格的根很容易让开发者了解他们是一起的。

＃＃＃ 怎么样？

使用标签名选择作为孩子的家长选择或命名空间前缀（取决于你的CSS命名策略）。

```CSS
/* 推荐的 */
我-例如{}
我-例如李{}
。我-example__item {}

/ * *避免/
。我的替代{} / *没有范围标记或模块名称* /
。我的父母。我的 - 例如{} / *。我的父母是范围之外，因此不应该在这个文件中*可以使用/
```

注：如果您使用[`数据是=`]（http://riotjs.com/guide/#html-elements-as-tags）（在[v2.3.17]介绍（HTTP：// riotjs。 COM /释放音符/＃行军-9-2016））发起暴动的标签，你可以使用`[数据是=“我-例如”]`为CSS选择器，而不是'。我-example`。


[↑回到目录]（＃表的，内容）


##记录您的标签API

骚乱标记实例是通过使用你的应用程序里的标签元素创建。实例通过其自定义属性进行配置。对于要由其他开发商所使用的标签，这些自定义属性 - 标签的API - 应该在`README.md`文件进行记录。

###为什么呢？

*文档为开发人员提供一个高度概括为一个模块，而无需经过它的所有代码。这使得一个模块更方便和更容易使用。
*标签的API是一套定制属性为其配置的通过。因此，这些是特别感兴趣的其他开发人员只需要消费（而不是开发）的标签。
*文件正式确立的API，并告诉开发商哪个功能修改标签的代码时，以保持向后兼容。
*`README.md`是首先要阅读文档的事实上的标准文件名。代码仓库托管服务（Github上，到位桶，Gitlab等）通过资源目录浏览时显示的README文件的内容，直接。这适用于我们的模块目录为好。
  
＃＃＃ 怎么样？

一个`README.md`文件添加到标签的模块目录：

```
范围滑块/
├──范围，slider.tag.html
├──范围，slider.less
└──README.md
```

在README文件，描述的功能和模块的使用。对于标签模块的最有用的来形容自定义属性，它支持那些有它的API：

```降价
＃范围滑块

##功能

范围滑块允许用户通过在滑块轨道拖动手柄的开始和结束的值都设置一个数值范围。

该模块采用[noUiSlider]（http://refreshless.com/nouislider/）为跨浏览器和触摸支持。

##用法

`<范围滑盖>`支持以下自定义标签属性：

|属性|类型|描述
| --- | --- | ---
| `min` |号码|数，其中范围开始（下限）。
| `max` |号码|数，其中两端的范围（上限）。
| `values` |号码[] *可选* |含开始和结束值数组。例如。 `值=“[10，20]”`。默认为`[opts.min，opts.max]`。
| `step` |数量*可选* |通过数递增/递减值。默认为1。
| `上slide` |功能*可选* |函数调用`（价值观，HANDLE）`当用户拖动开始（`HANDLE == 0`）或结束（`HANDLE == 1`）处理。例如。 `在幻灯片= {updateInputs}`，用`tag.updateInputs =（值，HANDLE）=> {常量的值=值[手柄] }`。
| `上end` |功能*可选* |函数调用`（价值观，HANDLE）`当用户停止拖动一个手柄。

对于自定义外观滑块参见[在noUiSlider文档样式段（http://refreshless.com/nouislider/more/#section-styling）。
```

[↑回到目录]（＃表的，内容）


##添加标记演示

添加`* .demo.html`文件具有不同的配置标记，示出了标签如何使用的演示。

###为什么呢？

*一个标签演示证明了在模块中的隔离工作。
*一个标签演示不必深入到文件或代码之前让开发者预览。
*演示可示出所有的可能的配置和变型的标记可被用。

＃＃＃ 怎么样？

一个`* .demo.html`文件添加到您的模块目录：

```
城市地图/
├──城市map.tag.html
├──城市map.demo.html
├──城市map.css
└──...
```

内部演示文件：

*包括`防暴+ compiler.min.js`在运行时也编译。
*包括标记文件（例如`/城市map.tag.html`）。
*创建`demo`标签（`<收益率/>`）嵌入到您的演示（否则选项属性不支持）。
*了`<演示>`标签内写入演示。
*作为奖励加`唱段，label`s到`<演示>`标签和作风为那些标题栏。
*初始化使用`riot.mount（'示范'，{}）`。

城市tag`模块中`实例演示文件：

```HTML
<！ - 模块/城市地图/城市map.demo.html： - >
<BODY>
    <H1>城市地图演示</ H1>
    
    <演示ARIA标签=“伦敦金融城图”>
        <城市地图位置=“伦敦”/>
    </演示>
    
    <演示ARIA标签=“巴黎城市地图”>
        <城市地图位置=“巴黎”/>
    </演示>
    
    <链接rel =“stylesheet”属性HREF =“./城市map.css”>
    
    <SCRIPT SRC =“路径/要/防暴+ compiler.min.js”> </ SCRIPT>
    <脚本类型=“防暴/标签”SRC =“./城市map.tag.html”> </ SCRIPT>
    <SCRIPT>
        riot.tag（“演示”，“<产量/>'）;
        riot.mount（'示范'，{}）;
    </ SCRIPT>
    
    <STYLE>
    / *添加一个灰色条带'咏叹调，label`作为演示标题* /
    演示：前{
        内容：“演示：”ATTR（ARIA标签）;
显示：块;
        背景：＃F3F5F5;
        填充：.5em;
        明确：两者;
        }
    </样式>
</ BODY>
```
注：这是一个工作理念，但可能是使用编译脚本干净多了。

[↑回到目录]（＃表的，内容）


##皮棉您标记文件

棉短绒提高代码的一致性，并帮助跟踪语法错误。随着一些额外的配置防暴标记文件也可以LINTED。

###为什么呢？

*掉毛标记文件，确保所有开发人员使用相同的代码风格。
*掉毛标记文件，可以跟踪语法错误为时已晚了。

＃＃＃ 怎么样？

为了让棉短绒从你的`* .tag.html`文件解压脚本，[把脚本`<SCRIPT>`标签内（＃的用脚本内标签）和[标签保持简单的表达式（＃保留-tag表达式简单的）（如棉短绒不理解那些）。配置您的棉短绒，使全局变量`riot`和标签`opts`。

#### ESLint

[ESLint]（http://eslint.org/）需要一个额外的[ESLint HTML插件（https://github.com/BenoitZugmeyer/eslint-plugin-html#eslint-plugin-html），以从提取脚本标记文件。

在`模块/ .eslintrc`配置ESLint（这样的IDE都可以把它解释为好）：
```JSON
{
“扩展”：“：建议eslint”
“插件”：“HTML”]
“ENV”：{
“浏览器”：真
}，
“全局”：{
“OPTS”：真实，
“暴动”：真
}
}
```

运行ESLint
```庆典
eslint模块/ ** / *。tag.html
```

#### JSHint

[JSHint]（http://jshint.com/）可以（用`--extract = auto`）解析HTML（使用`--extra-ext`）和提取脚本。

在`模块/ .jshintrc`配置JSHint（这样的IDE都可以把它解释为好）：
```JSON
{
“浏览器”：真实，
“PREDEF”：“OPTS”，“暴动”]
}
```

运行JSHint
```庆典
jshint --config模块/ .jshintrc --extra-EXT = HTML --extract =自动模块/
```
注：JSHint不接受`tag.html`作为扩展名，但只`html`。

[↑回到目录]（＃表的，内容）


##徽章添加到您的项目

您可以使用* RiotJS风格指南徽章*链接到本指南：

[！[RiotJS风格指南徽章]（https://cdn.rawgit.com/voorhoede/riotjs-style-guide/master/riotjs-style-guide.svg）]（https://github.com/voorhoede/riotjs -style-指南）

###为什么呢？

通知其他的开发项目是继RiotJS风格指南。并让他们知道他们可以找到本指南。

＃＃＃ 怎么样？

在项目中包含的徽章。在降价：

```降价
[！[RiotJS风格指南徽章]（https://cdn.rawgit.com/voorhoede/riotjs-style-guide/master/riotjs-style-guide.svg）]（https://github.com/voorhoede/riotjs -style-指南）
```

或HTML：

```HTML
<a href="https://github.com/voorhoede/riotjs-style-guide">
    <IMG ALT =“RiotJS风格指南徽章”
    SRC =“https://cdn.rawgit.com/voorhoede/riotjs-style-guide/master/riotjs-style-guide.svg”>
</A>
```

[↑回到目录]（＃表的，内容）

---

＃＃ 执照

[！[CC0]（http://mirrors.creativecommons.org/presskit/buttons/88x31/svg/cc-zero.svg）]（https://creativecommons.org/publicdomain/zero/1.0/）

[德Voorhoede]（https://twitter.com/devoorhoede）放弃在全球范围内本作品受著作权法在内的所有相关权利的所有权利，在法律允许的范围内。

您可以复制，修改，分发和执行工作，甚至用于商业目的，无需要求授权。
