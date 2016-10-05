# RiotJS风格指南

Opinionated *RiotJS Style Guide* for teams by [De Voorhoede](https://twitter.com/devoorhoede).

[![RiotJS Style Guide badge](https://cdn.rawgit.com/voorhoede/riotjs-style-guide/master/riotjs-style-guide.svg)](https://github.com/voorhoede/riotjs-style-guide)


## 目的

本指南提供了一种统一的方式来组织你的[RiotJS](http://riotjs.com/)代码。它的好处：

* 开发人员/团队成员了更容易理解和寻找；
* 更容易为IDE所理解的代码；
* 更容易重复使用你创建的组件；
* 更容易缓存和分发代码；

本指南受[AngularJS风格指南](https://github.com/johnpapa/angular-styleguide)John Papa 的启发。

## 演示

我们的[RiotJS演示](https://github.com/voorhoede/riotjs-demos#riotjs-demos-)是一个伴侣本指南，说明与实际案例的指导方针。

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

始终让你的应用程序构建在小模块之上，每个模块做一件事，把它做好。

模块是应用程序的小的自包含的部分。RiotJS 微架构是专门设计来帮助您创建 *视图逻辑模块*，Riot 称之为 *tag*。

### 为什么呢？

小模块更容易学习，理解，维护，重用和调试。无论是您还是其他开发人员。

### 怎么做？

每个Tag（就好像任何模块）必须遵循[FIRST](https://addyosmani.com/first/)原则：
*Focused*（[单一职责](http://en.wikipedia.org/wiki/Single_responsibility_principle)
*Independent*(独立)
*Reusable*(可重复使用)
*Small*
*Testable*(可测试)

如果你的模块做了很多或变得太大，把它分解成更小的模块。
作为一个经验法则，尽量保持每个 Tag 文件小于100行代码。
此外，还要确保您的Tag模块可以独立运作，例如通过加入一个独立的演示。

**提示**：如果您使用通过一个[AMD](https://github.com/amdjs/amdjs-api/blob/master/AMD.md)或CommonJS的模块加载，你需要[使用`--modular`(`-m`）编译标签](http://riotjs.com/guide/compiler/#amd-and-commonjs)：
```bash
# enable AMD and CommonJS
riot --modular
```

[↑回到目录](#table-of-contents)


## Tag 模块名称

一个 Tag 模块是一个特定类型的模块，包含一个 Riot tag。

每个模块的名称必须是：

* **有意义**：不过于特殊，也不是过于抽象。
* **短**：2或3个单词。
* **能读出来**：我们希望能够谈论它们。

Tag 模块名称也必须是：

* **自定义元素遵循规范**：[包括一个连字符](https://www.w3.org/TR/custom-elements/#concepts)，不要使用保留词。
* **`app-`命名空间**：如果非常通用那么再来一个单词，以便它可以容易地在其他项目中重复使用。

### 为什么呢？

* 该名称用于对模块进行通信。所以它必须是短的，有意义的和并且可读。
* 本`tag`元件被插入到DOM。因此，他们需要坚守规范。

### 怎么做？

```HTML
<!-- 推荐 -->
<app-header />
<user-list />
<range-slider />

<!-- 避免 -->
<btn-group /> <!-- short, but unpronouncable. use `button-group` instead -->
<ui-slider /> <!-- all tags are ui elements, so is meaningless -->
<slider /> <!-- not custom element spec compliant -->
```

[↑回到目录](#table-of-contents)


## 1模块 = 1目录

把构筑一个模块所用的所有文件放到一个地方。

### 为什么呢？

打包 Tag 文件（Tags，测试，静态文件，文档等）时，就很容易找到，移动和重复使用。

### 怎么做？

使用模块名作为目录名和文件名。
文件扩展名取决于文件的目的。

```
modules/
└── my-example/
    ├── my-example.tag.html
    ├── my-example.less
    ├── ...
    └── README.md
```

如果你的项目使用嵌套结构，可以嵌套在模块中的一个模块。
例如一个通用的`radio-group`模块可以被直接放置内部 "modules/"。而`search-filters`仅在一个`search-form`内才有意义，因此可以如下嵌套：

```
modules/
├── radio-group/
|   └── radio-group.tag.html
└── search-form/
    ├── search-form.tag.html
    ├── ...
    └── search-filters/
        └── search-filters.tag.html
``` 

[↑回到目录](#table-of-contents)


## 使用`* .tag.html` 后缀名

Riot 引入了一个名为*tags*的概念，并建议使用`* .tag`扩展。
然而，在本质上，这些标签包含标记只是自定义元素。因此，你应该**使用`*.tag.html`扩展**。

### 为什么呢？

* 告诉其他开发者这不仅仅是HTML，还是 Riot Tag 元素。
* 提高IDE支持。

### 怎么做？

在[浏览器模式](http://riotjs.com/guide/compiler/#in-browser-compilation)：
```html
<script src="path/to/modules/my-example/my-example.tag.html" type="riot/tag"></script>
```

在[预编译模式](http://riotjs.com/guide/compiler/#pre-compilation)，设置[自定义扩展](http://riotjs.com/guide/compiler/#custom-extension)：
```bash
riot --ext tag.html modules/ dist/tags.js
```

如果您使用的[WebPACK tag loader](https://github.com/srackham/tag-loader)，[配置 Loader](http://webpack.github.io/docs/using-loaders):
```javascript
{ test: /\.tag.html$/, loader: 'tag' }
```

[↑回到目录](#table-of-contents)


## 在 Tag 内使用`<script>`标记

虽然 Riot 支持写入标签元素中的JavaScript [省略`<SCRIPT>`](http://riotjs.com/guide/#no-script-tag)
你应该总是**使用`<script>`**包裹脚本。这是更接近Web标准，并能防止开发人员和IDE混乱。

### 为什么呢？

* 防止标记被解释为脚本。
* 提高IDE支持。
* 告诉其他开发者脚本开始和结束的地方。

### 怎么做？

```html
<!-- recommended -->
<my-example>
	<h1>The year is { this.year }</h1>
	
	<script>
		this.year = (new Date()).getUTCFullYear();
	</script>
</my-example>

<!-- avoid -->
<my-example>
	<h1>The year is { this.year }</h1>
	
	this.year = (new Date()).getUTCFullYear();
</my-example>
```

[↑回到目录](#table-of-contents)


## 保持 Tag 表达式简单

Riot 的 行内[表达式](http://riotjs.com/guide/#expressions)是纯粹的JavaScript。这使得他们非常强大，但可能也很复杂。因此，你应该保持**表达标签简单**。

### 为什么呢？

* 复杂的内联表达式难以阅读。
* 内嵌表达式不能在其他地方服用。这可能会导致代码重复和代码腐烂。
* 集成开发环境通常不具有表达式语法的支持，您的IDE无法自动完成验证。

### 怎么做？

将复杂的表达式表示为 tag 的法或属性。

```html
<!-- recommended -->
<my-example>
	{ year() + '-' + month() }
	
	<script>
		const twoDigits = (num) => ('0' + num).slice(-2);
		this.month = () => twoDigits((new Date()).getUTCMonth() +1);
		this.year  = () => (new Date()).getUTCFullYear();
	</script>
</my-example>

<!-- avoid -->
<my-example>
	{ (new Date()).getUTCFullYear() + '-' + ('0' + ((new Date()).getUTCMonth()+1)).slice(-2) }
</my-example>
```

[↑回到目录](#table-of-contents)


##保持 tag 的选项简单

Riot 支持 Tag 使用的标签元素属性。标签实例中这些选项都可以通过`opts`访问。例如`<my-tag my-attr="{某个值}" />`将可以通过`opts.myAttr`访问。

虽然 Riot 通过这些属性可以支持复杂的JavaScript对象，你还是应该尽量保持**标签选项尽可能简单**。尽量只使用[JavaScript的原始数据类型](https://developer.mozilla.org/en-US/docs/Glossary/Primitive)（字符串，数字，布尔值）和函数，避免复杂的对象。

这一规则的例外是你的应用程序内只能使用对象（如集合或递归标签）来解决的情况下，或一些非常明显的对象（例如，在网上商店的product）。

### 为什么呢？

* 通过使用每个选项单独标记有助于构建一个明确和清晰的API。
* 这样的选项值使我们的 tag API 类似于天然HTML（5）元素，这使得我们自定义元素直接且亲切。
* 通过使用每个选项的属性，其他开发人员可以轻松地理解传递给标签实例。
* 当传递复杂的对象使，很难看出当前正在使用哪个属性和方法。这使得重构代码困难，并可能导致代码腐烂。

### 怎么做？

使用每个选项标记属性，具有原始的值和函数：

```html
<!-- recommended -->
<range-slider
	values="[10, 20]"
	min="0"
	max="100"
	step="5"
	on-slide="{ updateInputs }"
	on-end="{ updateResults }"
	/>
	
<!-- avoid -->
<range-slider config="{ complexConfigObject }">
```
```html
<!-- exception: recursive tag, like menu item -->
<menu-item>
	<a href="{ opts.url }">{ opts.text }</a>
	<ul if="{ opts.items }">
		<li each="{ item in opts.items }">
			<menu-item 
				text="{ item.text }" 
				url="{ item.url }" 
				items="{ item.items }" />
		</li>
	</ul>
</menu-item>
```

[↑回到目录](#table-of-contents)


## 管理好您的 tag 选项

在 Riot 您的 tag 选项就是你的API。一个强大的和可预见的API，使您的代码很容易被其他开发者使用。

tag 选项通过自定义HTML属性传递。这些属性的值可以是 Riot 表达式（`attr="{ var }"`）或普通字符串（`attr="value"`）或完全丢失。你应该充分利用**管理 tag 选项**考虑到这些不同的情况。

### 为什么呢？

管理好你的 tag 选项可以确保您的 tag 功能将始终可用（防御性编程）。即使当其他开发者以后使用您有没有想过的调用方式。

### 怎么做？

* 使用默认设置选项值。
* 使用类型转换投选项值与预期的类型。
* 在使用前检查是否存在。

例如[Riot 的 `<todo>`示例](http://riotjs.com/guide/#example)可以改进改进，如果没有提供`items`，使用默认的：

```javascript
this.items = opts.items || []; // default to empty list
```
Ensuring different use cases all work:
```html
<todo items="{ [{ title:'Apples' }, { title:'Oranges', done:true }] }"> <!-- uses given list -->
<todo> <!-- uses empty list -->
```

`<range-slider>`在[使用原始的选项]（https://github.com/voorhoede/riotjs-style-guide#keep-tag-options-primitive）预计号码`min`，`max`和`step`。使用类型转换：

```javascript
// if step option is valid, use as number otherwise default to one. 
this.step = !isNaN(Number(opts.step)) ? Number(opts.step) : 1;
```
确保各种情况可用
```html
<range-slider> <!-- uses default -->
<range-slider step="5"> <!-- converts "5" to number 5 -->
<range-slider step="{ x }"> <!-- tries to use `x` -->
```

`<range-slider>` 还接收一个*可选的* `on-slide` 和 `on-end` 回调。在使用前检查是否存在和合法：

```javascript
slider.on('slide', (values, handle) => {
	if (typeof opts.onSlide === 'function') {
		opts.onSlide(values, handle);
	}
}
```
确保各种情况下可用
```html
<range-slider> <!-- does nothing -->
<range-slider on-slide="{ updateInputs }"> <!-- calls updateInputs on slide -->
<range-slider on-slide="invalid option"> <!-- does nothing -->
```

[↑回到目录](#table-of-contents)


## 将`this`赋值给`tag`

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
