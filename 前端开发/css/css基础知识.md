---
title: 【 CSS 】基础知识
date: 2016-12-06 13:37:23
tags: CSS
---
### style 标签写在 body 后与 body 前有什么区别？
页面自上而下加载样式，因此要先加载样式，浏览器一边下载页面元素一边渲染。


### link 和 @import 的区别

外部引用 CSS 文件有两种方式：link 和 @import：
```
<link rel="stylesheet" href="style.css" type="text/css"/>

<style type="text/css">
	@import url("style.css");
</style>
```

两者的区别是：
1. **由来的区别。** link 是 XHTML标签，除了加载 CSS 外，还可以定义 RSS 等其他事务；@import 术语 CSS 范畴，只能用来加载 CSS。
2. **加载顺序的差别。** link 引用的 CSS 会同时被加载，而 @import 引用的 CSS 会等到页面全部下载完再加载。所以浏览 @import 加载 CSS 的页面时开始会没有样式。
3. **兼容性的差别。** link 是 XHTML 标签，没有兼容问题；@import 是在 CSS2.1 提出的，低版本的浏览器不支持。
4. **使用DOM控制样式时的差别。** link 支持使用 JS 控制 DOM 去改变样式，@import 不支持。


### 介绍一下标准的 CSS 的盒子模型？低版本 IE 的盒子模型有什么不同的？

（1）有两种， IE 盒子模型、W3C 盒子模型；
（2）盒模型： 内容(content)、填充(padding)、边界(margin)、 边框(border)；
（3）区别： IE的 content 部分把 border 和 padding 计算了进去;
（4）CSS3 中的 **box-sizing** 属性，指定盒子模型。值为 **content-box** 按照 W3C 盒子模型绘制；值为 **border-box** 按照 IE 盒子模型绘制。
```
box-sizing: border-box;
-moz-box-sizing: border-box;
-webkit-box-sizing: border-box;
```


### CSS选择符有哪些？

#### id选择器（#myid）
#### 类选择器（.myclassname）
#### 标签选择器（div, h1, p）
#### 相邻选择器（h1 + p）
**只匹配一个元素**，考虑层级关系，选择与 h1 同层级相邻的第一个 p 元素（且不包含该 p 元素的子 p 元素），即 h1 与 p 有共同的父元素。

#### 所有相邻子元素（h1 ~ p）
**匹配一层相邻元素**，考虑层级关系，匹配与 h1 通层级的所有 p 元素。

#### 子选择器（ul > li）
**只匹配第一层子元素**，匹配 ul 的第一层子 li 元素。

#### 后代选择器（li a）
无视层级，匹配所有后代 a 元素。

#### 通配符选择器（ * ）
#### 属性选择器（a[rel = "external"]）
1.简单属性选择器
希望选择拥有某个属性的元素，不论该属性的值是什么。例如：a[href][title]
2.根据具体属性值选择
选择拥有特定属性值的元素。例如：a[href="http://www.baidu.com"]
3.根据部分属性值选择
例如：img[title ~= "Figure"]，选择 title 文本中包含 Figure 的所有图像。(更严格，Figure为一个单词，后面有空格)
**^=**：[foo ^= "bar"]，选择 foo 属性值以 bar 开头的所有元素；
**$=**：[foo $= "bar"]，选择 foo 属性值以 bar 结尾的所有元素；
***=**：[foo *= "bar"]，选择 foo 属性值中包含子串 bar 的所有元素。
4.特定属性选择类型
例如：img[src |= figure]，选择 src 属性以 figure 开头。（更严格，figure 为一个单词，后面跟 - ）

#### 伪类选择器（a:hover, li:nth-child）
伪类的实质是把某种幻想类关联到与伪类相关的元素。

1.X:checked，定位被选中的单选框和多选框。例子：input[type=radio]:checked{}
2.X:not(selector)，取反伪类选择器。例子：*:not(p){}，选择所有除段落标签以外的标签。
3.X:nth-child(n)/X:nth-last-child(n)
4.X:nth-of-type(n)/X:nth-last-of-type(n)，X:nth-of-type(n) **无视层级**，选择所有层级中第二个 X 元素修饰。
5.X:first-child/X:last-child
6.X:only-child
7.link-visited-focus-hover-active顺序，LVHA（LVFHA）

#### 伪元素选择器
伪元素是在文档中插入幻想元素，从而得到某种效果。**所有伪元素必须放在出现该伪元素的选择器的最后面。**

1.X:first-letter/X:first-line，**仅适用于块级元素**，设置块级元素的首字母/首行的样式（对可修改样式有要求）。
2.X:before/X:after，在元素的前后插入内容。



### CSS优先级算法（特殊性）如何计算？
选择器的特殊性由选择器本身的组件确定，特殊性值表述为 4 个部分，如 0，0，0，0。一个选择器的具体特殊性如下确定：
1.ID属性值，加 0，1，0，0；
2.类属性值、属性选择或伪类，加 0，0，1，0；
3.元素（标签）和伪元素，加 0，0，0，1；
4.结合符和通配符对特殊性没有贡献，但是与根本没有特殊性的**继承来的样式**优先级更高。

注意：1.重要性：在声明的结束分号之前插入 !important 来标志重要声明；
2.内联样式特殊性值为 1，0，0，0；
3.继承的值没有特殊性，甚至连 0 特殊性都没有；
4.按来源排序：创作人员样式 > 读者样式 > 用户代理样式。

总结：重要声明优先级最高（重要性内部优先级按特殊性排序），没有重要声明按特殊性决定优先级，特殊性相同按就近原则。



### CSS中可以和不可以继承的属性
#### 不可以继承的属性
1.盒子模型的属性：width、height、margin、border、padding
2.背景属性：background
3.定位属性：float、position、top、right、bottom、left、min-width、max-height、overflow、z-index
4.其他属性：display、outline

#### 有继承性的属性
1.字体文本属性：font、text-indent、text-align、line-height、word-spacing、letter-spacing、color
2.表格列表布局属性：table-layout、list-style
3.其他属性：visibility、cursor



### 元素竖向的百分比设定是相对于容器的高度吗？

当按照百分比设定一个元素的宽度时，它是相对于父容器的宽度计算的，但是，对于一些表示 **竖向距离的属性**，例如 `padding-top`、`padding-bottom`、`margin-top`、`margin-bottom`等，当按照百分比设定它们时，依据的是父容器的宽度，不是高度。子元素的 `height` 属性设定为百分比时，依据的是父元素的高度。


### 外边距合并的理解
当两个**垂直外边距**相遇时，它们将形成一个边距。
发生垂直外边距合并的情况：
1.当一个元素出现在另一个元素上面时，第一个元素的下外边距与第二个元素的上外边距发生合并；

![合并示例1](/img/css/base/hebing1.gif)

2.当一个元素包含在另一个元素中时（假设没有内边距或边框把外边距分隔开），它们的上/下外边距会发生合并；

![合并示例2](/img/css/base/hebing2.gif)

3.外边距自身会发生合并：假设有一个空元素，有外边距但没有边框或填充，在这种情况下上下外边距会合并。

![合并示例3](/img/css/base/hebing3.gif)

注释：1.**只有普通文档流中块级元素的垂直外边距会发生合并**，行内框、浮动框或决定定位之间的垂直外边距不会合并。
2.设置 overflow 属性的元素和它的子元素不会合并。
3.合并规则：
a.全部为正值时，取最大值；
b.不全是正值，取正值最大值和负值最小值之差（最大正值减去最小负值）；
c.全部为负值时，取最小的负值。

### CSS隐藏元素的几种方法（至少说出三种）
1. Opacity:元素本身依然占据它自己的位置并对网页的布局起作用。它也将响应用户交互;
2. Visibility:与 opacity 唯一不同的是它不会响应任何用户交互。此外，元素在读屏软件中也会被隐藏;
3. Display:display 设为 none 任何对该元素直接打用户交互操作都不可能生效。此外，读屏软件也不会读到元素的内容。这种方式产生的效果就像元素完全不存在;
4. Position:不会影响布局，能让元素保持可以操作;
5. Clip-path:clip-path 属性还没有在 IE 或者 Edge 下被完全支持。如果要在你的 clip-path 中使用外部的 SVG 文件，浏览器支持度还要低;

### CSS清除浮动的几种方法（至少两种）

- 使用带clear属性的空元素
- 使用CSS的overflow属性；
- 使用CSS的:after伪元素；
- 使用邻接元素处理；

### CSS3有哪些新特性？

1. CSS3实现圆角（border-radius:8px），阴影（box-shadow:10px），
2. 对文字加特效（text-shadow、），线性渐变（gradient），旋转（transform）
transform:rotate(9deg) scale(0.85,0.90) translate(0px,-30px) skew(-9deg,0deg);//旋转,缩放,定位,倾斜
3. 增加了更多的CSS选择器 多背景 rgba
4. 在CSS3中唯一引入的伪元素是::selection
5. 媒体查询，多栏布局
6. border-image
7. CSS3中新增了一种盒模型计算方式：box-sizing。盒模型默认的值是content-box, 新增的值是padding-box和border-box，

### position的值， relative和absolute分别是相对于谁进行定位的？

absolute :生成绝对定位的元素， 相对于最近一级的 定位不是 static 的父元素来进行定位。
fixed （老IE不支持）生成绝对定位的元素，通常相对于浏览器窗口或 frame 进行定位。
relative 生成相对定位的元素，相对于其在普通流中的位置进行定位。
static 默认值。没有定位，元素出现在正常的流中
sticky 生成粘性定位的元素，容器的位置根据正常文档流计算得出










































