# css规范 （自己保留文档）
######  本规范遵循BEM规范，参考地址：https://en.bem.info/methodology/quick-start/
######  示例代码来自腾讯weiui开源项目
以下做BEM规范的概要说明：

#### BEM
- Block
- Element
- Modifier

#### Block
块状域block-name,独立实体，有单独的意义。命名需要具有意义。
如空间位置划分的header,footer,section。具有独立功能的,nav，menu, container 等具有独立意义的实体。
- 要点1：he block shouldn't influence its environment, meaning you shouldn't set the external geometry (margin) or positioning for the block.（官方原文）
###### 大意：模块不应该影响当前它所处的环境，意味着不能使用margin或者position等会影响环境的属性
- 要点2：You also shouldn't use CSS tag or ID selectors when using BEM.
###### 大意：在使用BEM时不能使用CSS标签或者ID选择器

#### Element
归属于block作用域下的block__element,没有独立的意义，仅仅代表了block-name之下的子集。
如 :
 ```
 header__title,nav__list,nav__item
 ```
#### Modifier
`表示block-name，或者block__element的既定状态或者行为。`
`如：nav__list_selected, nav__list-item_active`

#### 在选择器中是由以下三种符号来表示扩展关系：
    -  中划线：仅仅作为词组之间的连接符号，表示块或者子元素的多单词连接符号。block-name
    __ 单下划线：双下划线用来连接块和子元素。block-name__element
    _  单下划线：描述一个块或者子元素的状态 block__element_modifier

引入实例来进行具体使用说明
```html

<div class="page">   
    <div class="page__hd">
        <h1 class="page__title">Button</h1> 
        <p class="page__desc">按钮</p>
    </div>
</div>
```
##### 上面的例子中创建了块page,以及三个子集hd,title,desc。我们注意到：元素的集合是 page > page\__hd > page\__title三层结构，既然 page\__hd是page\__title的父级那么为什么不命名为page\__hd\__title?
原文：

- Elements can be nested inside each other.

- You can have any number of nesting levels.

- An element is always part of a block, not another element. This means that element names can't define a hierarchy such as block\__elem1\__elem2.

##### 引入BEM规范中的嵌套关系原则：
1. 元素能够在任意的彼此嵌套
2. 可以嵌套任意层级
3. 元素一直只能归属于block的一部分，不能是另一个元素。这就意味着不能被命名为block\__name1\__name2

我们接着看一个完整的示例：
```html
<div class="page">
    <div class="page__hd">
        <h1 class="page__title">Button</h1>
        <p class="page__desc">按钮</p>
    </div>
    <div class="page__bd page__bd_spacing">
        <div class="button-sp-area">
        <a href="javascript:;" class="weui-btn weui-btn_primary">页面主操作</a>
		<a href="javascript:;" class="weui-btn weui-btn_primary weui-btn_loading"><i class="weui-loading"></i>页面主操作</a>
        <a href="javascript:;" class="weui-btn weui-btn_disabled weui-btn_primary">页面主操作</a>
        <a href="javascript:;" class="weui-btn weui-btn_default">页面次要操作</a>
		<a href="javascript:;" class="weui-btn weui-btn_default weui-btn_loading"><i class="weui-loading"></i>页面次操作</a>
        <a href="javascript:;" class="weui-btn weui-btn_disabled weui-btn_default">页面次要操作</a>
        <a href="javascript:;" class="weui-btn weui-btn_warn">警告类操作</a>
		<a href="javascript:;" class="weui-btn weui-btn_warn weui-btn_loading"><i class="weui-loading"></i>警告类操作</a>
        <a href="javascript:;" class="weui-btn weui-btn_disabled weui-btn_warn">警告类操作</a>
        </div>


        <div class="button-sp-area cell">
          <a href="javascript:;" class="weui-btn_cell weui-btn_cell-default">普通行按钮</a>
          <a href="javascript:;" class="weui-btn_cell weui-btn_cell-primary">强调行按钮</a>
          <a href="javascript:;" class="weui-btn_cell weui-btn_cell-primary">
            强调行按钮
          </a>
          <a href="javascript:;" class="weui-btn_cell weui-btn_cell-warn">警告行按钮</a>
        </div>

        <div class="button-sp-area">
            <a href="javascript:;" class="weui-btn weui-btn_mini weui-btn_primary">按钮</a>
            <a href="javascript:;" class="weui-btn weui-btn_mini weui-btn_default">按钮</a>
            <a href="javascript:;" class="weui-btn weui-btn_mini weui-btn_warn">按钮</a>
        </div>
    </div>
    <div class="page__ft">
        <a href="javascript:home()"><img src="./images/icon_footer_link.png" /></a>
    </div>
</div>
```
##### 示例中我们能找到的block: page/button-sp-area/weui-btn/cell/page\__ft/weui-loading。
##### 以及element: page__title/page__desc/weui-btn
##### 还有modifier: weui-btn_mini/weui-btn_cell-primary/weui-btn_disabled
我们并没有去调试，也没有去看结构，单单看命名我们就能把整个css结构化，并且能够铺平。严格遵守BEM规范不会出现两层以上的结构，并且复杂的命名，可以大大的规避变量的污染，每一个block都有自己的一个命名空间。

#### 命名之后我们引用一个官方示例：
```html
<div class="block">
    <div class="block__elem1">
        <div class="block__elem2">
            <div class="block__elem3"></div>
        </div>
    </div>
</div>
```
我们的逻辑该如何去写CSS改变block__elem3的样式呢？
```css
.block .block__elem1 . block__elem2 .block__elem3 {
	color: #333
}
```
这种写法是耦合非常强的写法，后期如果结构发生多次变化CSS就会变得特别臃肿，维护将会是特别困难。

BEM 法法论中做了如下描述：
- However, this block structure is always represented as a flat list of elements in the BEM methodology。
在BEM方法论中，这样的模块通常被表示为一个并列的元素列表

```css
.block {}
.block__elem1 {}
.block__elem2 {}
.block__elem3 {}
```
这样如果结构发生变化：

```html
<div class="block">
    <div class="block__elem1">
	 <div class="block__elem3"></div>
    </div>
	 <div class="block__elem2">
        </div>
</div>
```
虽然代码结构发生了变化，但是元素的规则和他们的名字仍然保持不变

以上BEM的基本原则就介绍完毕：

------------

## 实践部分
- MemberShip （block的成员）
- Optionality （可选性）

#### MemberShip 
关于模块的成员描述：元素使用输入模块的一部分，不能用在模块之外
```html
<!--正确的，模块的成员只能在模块的作用域下使用-->
<div class="block">
	<div class="block__title">
		
	</div>
	<div class="block__list">
		
	</div>
</div>
<!--错误的，模块的成员使用在了模块之外-->
<div class="block">
	<div class="block__title">
		
	</div>
</div>
<div class="block__list"></div>

```
#### Optionality
关于可选性的描述：元素是可选的模块组件，并不是所有的模块必须有元素
```html
<!-- 'search-form' 模块 -->
<div class="search-form">
    <!-- 'input' 模块 -->
    <input class="input"/>

    <!-- 'button' 模块 -->
    <button class="button"></button>
</div>
```

### Modifier
关于修饰符的描述：如多大，主题是什么，与其他表现上的区别，如何响应用户或者他具有什么样的行为。
- block-name_modifier-name
- block-name__element_modifier-name

#### 关于Modifier类型的定义
- Boolean:
  当修饰符的是否存在是重要的，与他的值无关时使用这种类型的修饰符。
  比如：disabled
  例：block-name__input_disabled
- Key-value
  当修饰符的值是重要的使用键值对类型。
  比如：使用islands主题
  例：menu_theme_islands
###### 修饰符不能单独去使用
  

## 帮助
- 在实际项目中是应该创建模块还是一个元素呢？
应该遵循以下原则：
1. 如果这段代码可能被重用，且不依赖页面上的其他组件，那么应该创建一个模块。
2. 如果这段代码并没有block实体的情况下不能使用，那么应该创建一个元素。

#### BEM解决的问题
组件之间完全解耦，不会造成命名看空间的浪费，如 .list ul li 的写法带来的嵌套风险

#### 性能
BEM 命名会使类名边长，但是经过压缩之后带来的开销科技忽略不计，并且 降低了嵌套维度，带来的性能提高是显而易见的。


