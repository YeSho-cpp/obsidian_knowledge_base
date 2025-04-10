# 认识CSS

* CSS表示层叠样式表（Cascading Style Sheet，简称：CSS，又称为又称串样式列表、级联样式表、串接样式表、阶层式样式表）
  是为网页添加<font color=#ff0000>样式的代码</font>。

![image-20230201230429728](https://img1.imgtp.com/2023/02/02/DOav5nDE.png)

## CSS的历史

* 早期的网页都是通过HTML来编写的，但是我们希望HTML页面可以更加丰富:
  * 这个时候就增加了很多<font color=#ff0000>具备特殊样式的元素</font>：比如i、strong、del等等；
  * 后来也有不同的浏览器实现各自的样式语言，但是没有统一的规划；
  * 1994年，哈肯·维姆·莱和伯特·波斯**合作设计CSS**，在1996年的时候发布了CSS1；
  * 直到1997年初，W3C组织才专门成立了CSS的工作组，1998年5月发布了CSS2；
  * 在2006~2009非常流行 <font color=#ff0000>“DIV+CSS”布局</font>的方式来替代所有的html标签；
  * 从CSS3开始，所有的CSS分成<font color=#ff0000>了不同的模块（modules）</font>，每一个“modules”都有于CSS2中额外增加的功能，以及向后
  兼容。
  * 直到2011年6月7日，<font color=#ff0000>CSS 3 Color Module</font>终于发布为W3C Recommendation。
* 总结：CSS的出现是**为了美化HTML的**，并且让结构（HTML）与样式（CSS）分离；
  * <font color=#ff0000>美化方式一</font>：为HTML**添加各种各样的样式**，比如颜色、字体、大小、下划线等等；
  * <font color=#ff0000>美化方式二</font>：对HTML进行布局，按照某种结构显示（CSS进行布局 – 浮动、flex、grid）；

## CSS如何编写呢？

* CSS这么重要，那么它的语法规则是怎么样的呢？
* <img src="https://article.biliimg.com/bfs/article/2e9ec1d3f9f412709c40aff0ba234140fc14bb69.png" alt="image-20230201230503323" style="zoom:33%;" />

* 声明（Declaration）一个单独的CSS规则，如 color: red; 用来指定添加的CSS样式。
  * <font color=#ff0000>属性名（Property name）</font>：要添加的css规则的名称；
  * <font color=#ff0000>属性值（Property value）</font>：要添加的css规则的值；

## CSS样式应用到元素

* CSS提供了3种方法，可以将CSS样式应用到元素上：

### 内联样式（inline style）

* 内联样式表存在于HTML元素的<font color=#ff0000>style属性</font>之中。

	![image-20230201231632547](https://s1.vika.cn/space/2023/02/02/a4c18896659841fcb9a2731af60b946e)

* CSS样式之间用分号`;`隔开，建议每条CSS样式后面都加上分号 
* 
*  很多资料不推荐这种写法：
	- 在<font color=#ff0000>原生的HTML编写</font>过程中确实这种写法是不推荐的
	- 在<font color=#ff0000>Vue的template中</font>某些动态的样式是会使用内联样式的；


### 内部样式表（internal style sheet）
- 将CSS放在HTML文件`<head>元素里的<style>`元素之中。
- ![image.png](https://article.biliimg.com/bfs/article/b48dc2dda299d7268a3ff27a58a72b8b78e50ec9.png )
- 在Vue的开发过程中，**每个组件也会有一个style元素，和内部样式表非常的相似（原理并不相同）；**
### 外部样式表（external style sheet）
- 外部样式表（external style sheet）是将css编写一个独立的文件中，并且`通过<link>元素`引入进来；
* **使用外部样式表主要分成两个步骤**： 
	* 第一步：将css样式在一个独立的css文件中编写（后缀名为.css）； 
	* 第二步：通过`<link>`元素引入进来； 
	* ![image.png](https://article.biliimg.com/bfs/article/6703d8b4dfbb57ee8c8c8d43fd769850f47d5282.png)
* link元素的作用,用来**引入资源** `<!-- 关系：样式表 地址URL -->`
### @import
- 可以在style元素或者CSS文件中使用@import导入其他==多个==的CSS文件
- ![image.png](https://article.biliimg.com/bfs/article/50d6ec7110fcf4095edf923a3bfa1d40bac4a799.png)
📝 这样批量导入后可以，多个css在外部样式只需要写一次


## CSS的注释 

- CSS代码也可以添加注释来方便阅读： 
- CSS的注释和HTML的注释是不一样的； 
- <font color=#ff0000> /* 注释内容 */</font>
- ![image.png](https://article.biliimg.com/bfs/article/dcab885a96e4808f794c6cb5f4299e71721e87d6.png)



## 必须掌握的CSS属性
- 必须掌握的CSS属性
	- 在开发中90+%的时间写的都是这些属性;
	[[CSS样式规则顺序|CSS样式规则顺序]] 
	[[CSS常见属性分类]] 

## CSS属性的官方文栏
CSS官方文档地址
https://www.w3.org/TR/?tag=cSs
CSS推荐文档地址:
[CSS 参考 - CSS：层叠样式表 | MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Reference#%E5%85%B3%E9%94%AE%E5%AD%97%E7%B4%A2%E5%BC%95)
## 目前需要掌握的CSS属性
- 要想深刻理解所有常用CSS属性，最好先学会以下几个最基础最常用的CSS属性
- <font color=#ff0000>font-size:</font>文字大小
- <font color=#ff0000>color:</font>==前景色==(文字颜色)
- <font color=#ff0000>background-color:</font>背景色
- <font color=#ff0000>width:</font>宽度
- <font color=#ff0000>height:</font>高度
## CSS颜色的表示方法
- 在CSS中，颜色,有以下几种表示方法:
- 颜色关键字(color keywords) :
	- 口是不区分大小写的标识符，它表示一个具体的颜色;
	- 可以表示哪些颜色呢?
	- [`<color>` - CSS：层叠样式表 | MDN](https://developer.mozilla.org/zhCN/docs/Web/CSS/color_value#%E8%AF%AD%E6%B3%95)
- RGB颜色:
	- RGB是一种色彩空间，通过R (red，红色)、G (green，绿色)、B (blue，蓝色)三原色来组成了不同的颜色;
	- 也就是通过调整这三个颜色不同的比例，可以组合成其他的颜色;
	- RGB各个原色的取值范围是0~255;
	RGB的表示方法
- RGB颜色可以通过以#为前缀的十六进制字符和函数(rgb()、rgba()）标记表示。
- 方式一:十六进制符号:`#RRGGBB[AA]`
	- R(红)、G(绿)、B(蓝)和A (alpha）是十六进制字符(O-9、A-F);A是可选的。
		- √比如，#ffO000等价于#ff0OO0ff;
	- 后两位 `AA` 就表示 alpha 通道的值代表==透明度==，同样使用一个两位十六进制数来表示。其中，`00` 表示完全透明，`FF` 表示完全不透明，取值范围为 `00` 到 `FF`。
- 方式二:十六进制符号:`#RGB[A]`
	- R(红)、G(绿)、B(蓝）和A(alpha)是十六进制字符（0-9、A-F) ;
	- 三位数符号（#RGB）是六位数形式(#RRGGBB)的减缩版。
		- √比如，#fO9和#ff0099表示同一颜色。
	- 四位数符号（#RGBA）是八位数形式(#RRGGBBAA)的减缩版。
		- √比如，#Of38和#0Off3388表示相同颜色。
- 方式三:函数符: `rgb[a](R,G,B[，A])`
	- R(红)、G(绿)、B(蓝)可以是`<number>`(数字)，或者`<percentage>`(百分比)，255相当于100%。
	- A (alpha)可以是0到1之间的数字，或者百分比，数字1相当于100%(完全不透明)。
	- `transparent`代表透明就是rgba(0,0,0,0)。
	```css
	background-color: rgba(255, 0, 0, 0.5);
	<!-- 这里的 `rgba` 函数中传入了四个参数，分别是红、绿、蓝和 alpha 值。由于我们需要的是红色，因此红色通道的参数值为 `255`，其他通道的参数值为 `0`。而由于需要半透明的效果，因此将 alpha 值设置为 `0.5` 
	-->
	```


# CSS属性-文本
## text-decoration
- **text-decoration用于设置文字的==装饰线**==
	- decoration是装饰/装饰品的意思;
- text-decoration有如下常见取值:
	- <font color=#ff0000>none:</font>无任何装饰线
	- √可以去除a元素默认的下划线
	- <font color=#ff0000>underline:</font>下划线
	- <font color=#ff0000>overline:</font>上划线
	- <font color=#ff0000>line-through:</font>中划线（删除线)
- a元素有下划线的本质是被添加了text-decoration属性
## text-transform
- **text-transform用于设置文字的==大小写转换**==
	- Transform单词是使变形/变换(形变);
- text-transform有几个常见的值:
	- <font color=#ff0000>capitalize:</font>(使...首字母大写，资本化的意思)将每个单词的首字符变为大写
	- <font color=#ff0000>uppercase:</font>(大写字母)将每个单词的所有字符变为大写
	- <font color=#ff0000>lowercase:</font> (小写字母)将每个单词的所有字符变为小写
	- <font color=#ff0000>none:</font>没有任何影响
- 实际开发中用JavaScript代码转化的更多
## text-indent
- text-indent`!用于设置第一行内容的缩进
- 使用任何长度单位，包括像素（px）、百分比（%）<font color=#ff0000>基于父级元素</font>、ems等。
- text-indent: 2em;刚好是缩进2个文字
- 不适用于<font color=#ff0000>行内非替换元素</font>
#css技巧 
	一般很多门户网站会对logo包裹一个`h1`元素，`h1->a->bg`,这样有利于[[前端html#SEO|SEO优化]],  因为是h1所以a里面要有文字，为了让文字隐藏起来，用`text-indent：-9999px`这样一个大的值，让文字消失。
<font color=#ff0000>rems单位介绍:</font>
>rem是CSS3中引入的一个相对单位，其大小是相对于==根元素==（即html元素）的字体大小而定。因此，如果根元素的字体大小为16px，那么1rem就等同于16px；如果根元素的字体大小为10px，那么1rem就等同于10px。
>
使用rem作为长度单位的好处在于它可以根据根元素的字体大小进行缩放，从而实现响应式布局。与em不同的是，它==不会受到父元素字体==大小的影响，计算更加准确。因此，在开发移动端网页时，可以使用rem替代像素和其他绝对单位，从而使页面在不同屏幕尺寸下更加美观。



## text-align（重要)
- text-align:直接翻译过来设置文本的==对齐方式==;
- MDN:定义行内内容（例如文字)如何相对它的块父元素对齐;
- 常用的值
- <font color=#ff0000>left:</font>左对齐
- <font color=#ff0000>right:</font>右对齐
- <font color=#ff0000>center:</font>正中间显示
- <font color=#ff0000>justify:</font>两端对齐
- W3C中的解释:
	> This shorthand property sets the _ 'text-align-all'and_'text-align-last'_ properties and describes how the inline-level content of a block is aligned along the inline axis if the content does not completely fill

其中W3C的解释最准确就是：text-align,用于设置==行内元素内部==相对于容器的文本对齐方式。
<font color=#ff0000>justify:</font>两端对齐：
	其中，当设置justify值时，每一行的==左右两端都会尽可能地平均分布==，使得整个块看起来更加整齐美观。需要注意的是，如果某一行单词太少而无法完全填满这一行，浏览器就会自动调整单词之间的间距，以使每一行达到相同的宽度。text-align-last属性则用于指定最后一行文本的对齐方式。

## word/letter-spacing
- `letter-spacing :
	- 用于设置字母间距
- word-spacing
	- 设置单词之间的间距
- 默认是0，可以设置为负数
# CSS属性-字体
## font-size
- font-size决定文字的大小
- 常用的设置
	- <font color=#ff0000>具体数值＋单位</font>
		- 比如100px
		- 也可以使用em单位(不推荐): 1em代表100%，2em代表200%，0.5em代表50%
	- <font color=#ff0000>百分比</font>
	- 基于父元素的font-size计算，比如50%表示等于父元素font-size的一半

## font-family
- font-family用于设置文字的字体名称
- 可以设置<font color=#ff0000>1个或者多个</font>字体名称;
- 浏览器会选择列表中第一个该计算机上有安装的字体;
- 或者是通过`@font-face`指定的可以直接下载的字体。
	- `@font-face` 是 CSS 中用来定义自定义字体的规则，可以让网页设计者使用自定义的字体而不必依赖于用户计算机中已经安装好的字体。通过 `@font-face` 规则，设计者可以将自定义字体文件（例如 TrueType 字体或 OpenType 字体）嵌入到网页中，从而实现在浏览器中加载和使用这些字体。
	```css
		@font-face {
		  font-family: 'FontName'; /* 自定义字体名称 */
		  src: url('fontname.ttf'); /* 自定义字体文件的地址 */
		  }
	```
	- 需要注意的是，在使用 `@font-face` 规则时，需要同时提供多种字体格式（.eot、.ttf、.woff、.svg等），以保证在不同的浏览器和操作系统上都能够正确地加载和显示字体。此外，考虑到版权和网络流量等问题，也应该谨慎选择合适的自定义字体，遵守有关规定。
- 淘宝使用的字体:
	- ![image.png](https://article.biliimg.com/bfs/article/7070ea1fb364ed579bf912dea2d281af0ebe57a7.png)
	浏览器是通过调用操作系统的字体来进行完成的
	<img src="https://article.biliimg.com/bfs/article/7d1d99e678d3e77b9496ec33d40c1e8e050ed05a.png" alt="image.png" style="zoom:50%;" />

## font-weight
- font-weight用于设置文字的**粗细**(重量)常见的取值:
	- <font color=#ff0000>100 |200 |300| 400 | 500 | 600 | 700 | 800 |900:</font>每一个数字表示一个重量
	- `normal`:等于400
	- `bold`:等于700
- strong、b、h1~h6等标签的font-weight默认就是bold
## font-style(一般)
- font-style用于设置文字的常规、斜体显示
	- `normal`:常规显示
	- `italic(斜体)`:用该==字体的斜体==显示(字体一般有专门的斜体样式)
	- `oblique(倾斜)`︰文本倾斜显示(**仅仅是让文字倾斜**)
- <img src="https://article.biliimg.com/bfs/article/89ce88798598b547c28c5b59de0a4fd3bc69bb87.png" alt="image.png" style="zoom:67%;" />

- `em、i、cite、address、var、dfn`等元素的font-style默认就是italic

## font-variant(了解)
- font-variant可以影响小写字母的显示形式
	- variant是变形的意思;
- 可以设置的值如下
	- `normal`:常规显示
	- `small-caps`:将小写字母替换为缩小过的大写字母-==就是小写字母变大写，但还是小写字母的高度==

## line-height
- line-height用于设置文本的行高
- 行高可以先简单理解为<font color=#ff0000>一行文字所占据的高度</font>
- 文本的行高把**当前的盒子撑**起来了
<img src="https://article.biliimg.com/bfs/article/66e4ec927d0d1ffdd3501863d30ea542f7ffddfc.png" alt="image.png" style="zoom: 50%;" />
- 为什么文本需要行高?
	- 通过设置行高，可以让页面的文本更加**舒适易读**，同时为页面布局提供更多的灵活性和自由度，从而实现更好的用户体验。
- 行高的严格定义是:两行==文字基线==(baseline)之间的间距
- 基线(baseline):与小写字母x最底部对齐的线
- <img src="https://article.biliimg.com/bfs/article/6595afcbffdf7c33951bc014141a95233c917004.png" alt="image.png" style="zoom:50%;" />
从上图我们可以看到`line-height`-文本的高度=行距 
行距是要上下平分的，当准备让一个内容在==一个容器==居中显示时，如果该内容是==文本==，可以使用line-height
**注意区分height和line-height的区别**
- height:元素的整体高度
- line-height:元素中每一行文字所占据的高度
- 应用实例:假设div中只有一行文字，如何让这行文字在div内部垂直居中
	- 让line-height等同于height
	- <img src="https://article.biliimg.com/bfs/article/4ffc32cf579bdf268f294a5d5c3378b793b2efa0.png" alt="image.png" style="zoom:50%;" />
✍️重点
line-height作用于<font color=#ff0000>块级元素</font>会拉伸整体的高度，而<font color=#ff0000>行内非替换元素</font>不会。
## font缩写属性

font是一个缩写属性
- font 属性可以用来作为font-style, font-variant, font-weight, font-size, line-height和 font-family 属性的简写;
- `font:font-style font-variant font-weight font-size/line-height font-family`
- 规则:
	- `font-style、font-variant、font-weight`可以随意调换顺序，也可以省略
	- `/line-height`可以省略，如果不省略，**必须**跟在font-size后面
	- `font-size、font-family`不可以调换顺序，不可以省略
- 行高可以跟字体大小连写数值可以是倍数，这个倍数是字体大小的倍数 12px/1.5
- ![image.png](https://article.biliimg.com/bfs/article/e617d69d35c17cde8e601ad04faeab33b0653b97.png)

# CSS常见选择器
- 开发中经常需要找到特定的网页元素进行设置样式
	- 思考:如何找到特定的那个元素?
- 什么是CSS选择器
	- 按照一定的规则选出**符合条件**的元素，为之添加CSS样式
- `* `通配符，选择所有的选择器，在选择器**逗号**代表并列
- 一般清除浏览器的默认属性，采用并列
	```css
	/*·更推荐的做法-*/
	body, p,div,h2, span {
	margin: 0;
	padding: 0;
	} //将这些浏览默认的边距什么的清空
	```
## 基础选择器
目标:理解选择器的作用，能够使用基础选择器在HTML中选择元素
### 标签选择器
- 结构:<font color=#ff0000>标签名</font>{ css属性名:属性值;}
- 作用:通过标签名，找到页面中所有这类标签，设置样式》
- 注意点:
	1. 标签选择器选择的是一类标签，而不是单独某一个
	2. 标签选择器无论嵌套关系有多深，都能找到对应的标签
- <img src="https://article.biliimg.com/bfs/article/e6d142e07d6d2705e9001ab3ecc2339b0626350f.png" alt="image.png" style="zoom:33%;" />

### 类选择器
- 结构:`.类名{ css属性名:属性值;}`
- 作用:通过类名，找到页面中所有带有这个类名的标签，设置样式
- 注意点:
	1. 所有标签上都有class属性，class属性的属性值称为类名(类似于名字)
	2. 类名可以由数字、字母、下划线、中划线组成，但不能以数字或者中划线开头
	3. 一个标签可以同时有多个类名，类名之间以空格隔开
	4. 类名可以重复，一个类选择器可以同时选中多个标签 
- <img src="https://article.biliimg.com/bfs/article/ebde42125119bd2a86fcb2692c8a09bebbd797ec.png" alt="image.png" style="zoom:33%;" />
✍️注意：类选择器可以加上类型名，例如上图的one类可以改为`div.one`影响使用类`one`的`<div>`元素。
### id选择器
- 结构:`#id属性值{ css属性名:属性值;}`
- 作用:通过id属性值，找到页面中带有这个id属性值的标签，设置样式
- 注意点:
	1. 所有标签上都有id属性
	2. id属性值类似于身份证号码，在一个页面中是唯一的，不可重复的!
	3. 一个标签上只能有一个id属性值 
	4. 一个id选择器只能选中一个标签
	<img src="https://article.biliimg.com/bfs/article/ac174458ff848550e78ac9d382f83804a2e5cf7e.png" alt="image.png" style="zoom:33%;" />

### 补充:类与id的区别
class类名与id属性值的区别
- `.class类名`相当于姓名，可以重复，一个标签可以同时有多个class类名，中间用空格间隔. id属性值相当于身份证号码，不可重复，一个标签只能有一个id属性值 
- 实际开发的情况
	- 类选择器用的最多
	- id一般配合==js==使用，除非特殊情况，否则不要使用id设置样式
	- 实际开发中会遇到冗余代码的抽取(可以将一些公共的代码抽取到一个公共的类中去)
## 属性选择器
- 拥有某一个属性`[att]`
- 属性等于某个值`[att=val]`
- ![image.png](https://article.biliimg.com/bfs/article/81648a05a9d015df86f23d8ea9d762ea4d1163e9.png)
- 其他了解的(不用记)
	1. `[attribute]：`选取具有指定属性的元素。
	2. `[attr*=val]:`属性值包含某一个值val;
	3. `[attr^=val]:`属性值以val开头;
	4. `[attr$=val]:`属性值以val结尾;
	5. `[attr|=val]:`属性值等于val或者以val开头后面紧跟连接符-;
	6. `[attr~=val]:`属性值包含val,如果有其他值必须以空格和val分割;
	例如，对于下面这个HTML代码：

```html
<a href="https://www.example.com" title="example">Link</a>
```

要选择所有具有title属性的a元素，可以使用以下CSS规则：

```css
a[title] {
  color: blue;
}
```

要同时选择href属性值为“https://www.example.com”和title属性值为“example”的a元素，可以使用以下CSS规则：

```css
a[href="https://www.example.com"][title="example"] {
  text-decoration: none;
}
```

属性选择器是一种强大的选择器，可以使得样式更加精细化，从而增强了网页设计的可扩展性。
## 后代选择器
- 后代选择器一:所有的后代(直接/间接的后代)
	- 选择器之间以==空格==分割
	![image.png](https://article.biliimg.com/bfs/article/6df65aa979d7856158354b72d00264da583821e3.png)

- 后代选择器二:直接子代选择器(必须是直接自带)
	- 选择器之间以`>`分割;
	![image.png](https://article.biliimg.com/bfs/article/5fa3494c5190a8b0ba48a039e5e176d1c0b9880d.png)

## 兄弟选择器 
- 兄弟选择器一:相邻兄弟选择器
	- 使用符号`＋`连接
	![image.png](https://article.biliimg.com/bfs/article/898853cccc23075730433fd41fa3cafcbfd744c6.png)

- 兄弟选择器二:普遍兄弟选择器~
	- 使用符号`～`连接
	![image.png](https://article.biliimg.com/bfs/article/d0d60e84a9e4e7cc2581978922e91d21ff58df93.png)

## 选择器组
1. 交集选择器:需要同时符合两个选择器条件(两个选择器==紧密连接==)
	- 在开发中通常为了**精准的选择**某一个元素;
	![image.png](https://article.biliimg.com/bfs/article/da76a835fcad320b2763a4889973f0988b63e1d4.png)

2. 并集选择器:符合一个选择器条件即可(两个选择器以`,`号分割)
	- 在开发中通常为了给**多个元素设置相同**的样式;
	![image.png](https://article.biliimg.com/bfs/article/032fdbe0e7498017aec6e14632b0c1f1642a5366.png)

## 伪类选择器 
- 什么是伪类呢?
	- Pseudo-classes:翻译过来是伪类;
	- 伪类是**选择器**的一种，它用于选择**处于特定状态**的元素;
- 比如我们经常会实现的:当手指放在一个元素上时，显示另外一个颜色;
- ![image.png](https://article.biliimg.com/bfs/article/858754a3ede81b44468d8bc1721bc570ed35390f.png)
### 常见的伪类
1. 动态伪类(dynamic pseudo-classes)
	- `:link、:visited、:hover、:active、:focus`
2. 目标伪类(target pseudo-classes)
	- `:target`
	- `:target` 伪类可以匹配 URL 中的[[前端html#^miaodian|锚点元素]]，例如 `#section1`。当你点击文档中的链接时，浏览器会将 URL 改变为包含了所点击的锚点的地址。此时，`:target` 伪类就会匹配到当前文档中与锚点相符的元素，从而可以对其进行样式设置。
1. 语言伪类(language pseudo-classes)
	- `:lang( )`
2. 元素状态伪类(Ul element states pseudo-classes)
	- `:enabled、:disabled、:checked`
3. 结构伪类(structural pseudo-classes)(后续学习)
	- `:nth-child()、:nth-last-child()、:nth-of-type()、:nth-last-of-type()`
	- `:first-child、:last-child、:first-of-type、:last-of-type`
	- `:root、:only-child、:only-of-type、:empty`
4. 否定伪类(negation pseudo-classes)(后续学习)
	- `:not()`
	所有的伪类: [伪类](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Pseudo-classes)

### 动态伪类
- 使用举例
	- `a:link`未访问的链接
	- `a:visited`已访问的链接
	- <font color=#ff0000>a:hover</font> 鼠标挪动到链接上(重要)
	- `a:active`激活的链接（鼠标在链接上长按住未松开)
- 使用注意
	- `:hover`必须放在:link和:visited后面才能完全生效
	- :active必须放在:hover后面才能完全生效
	- 所以建议的编写顺序是:link、:visited、:hover、:active
- 除了a元素,:hover、:active也能用在其他元素上 
- :focus指当前拥有**输入焦点的元素**（能接收键盘输入)
	- 口文本输入框一聚焦后，背景就会变红色
- 因为链接a元素可以**被键盘的Tab键选中聚焦，所以:focus也适用于a元素**
- **动态伪类编写顺序建议为**
	- `:link、:visited、:focus、:hover、:active`
- 直接给a元素设置样式，相当于给a元素的所有动态伪类都设置了
- 口相当于`a:link、a:visited、a:hover、a:active、a:focus的color`都是red

## 伪元素

**常用的伪元素有**
	- :first-line、:ifirst-line
	- :first-letter、:first-letter
	- :before、<font color=#ff0000>::before</font>
	- :after、<font color=#ff0000>::after</font>
- 为了区分伪元素和伪类，建议伪元素使用2个冒号，比如::first-line
- :first-line可以针对==首行文本==设置属性
- :first-letter可以针对==首字母==设置属性
- ![image.png](https://article.biliimg.com/bfs/article/41372b5681acc91dc7dc2e03bf9bf6f5be7f2a6c.png)
- ::before和after用来在一个元素的**内容之前或之后插入其他内容**(可以是文字、图片)
	- 常通过`content`属性来为一个元素添加修饰性的内容。
	- ![image.png](https://article.biliimg.com/bfs/article/c6c4869cdc33284407b7c03891edd66e6c2352f0.png)
	- 📝使用伪元素的过程中，不要将content**省略**,before和after默认是==行内级==元素。
	伪类跟伪元素区别:
	伪类和伪元素是 CSS 中两个不同的概念，它们用于在 HTML 元素渲染时添加特定样式，但它们所针对的对象却有所不同。
-  伪类：用来描述元素的特殊状态（如 `:hover`、`:focus` 等），此时元素处于文档树的特定位置或者与用户交互时呈现出的特殊状态。伪类以一个冒号 `:` 开头。
-  伪元素：用来创建一些**不在文档树**中的==元素==，并为其添加样式，比如可以使用 `::before` 和 `::after` 来在元素的前面或后面插入内容。伪元素以两个冒号 `::` 开头（在旧版规范中也可以使用一个冒号 `:`）。


# CSS继承-层叠-元素类型
## CSS属性的继承
- CSS的某些属性具有继承性(Inherited):
	- 如果一个属性**具备**继承性,那么在该元素上设置后，它的后代元素都可以继承这个属性;
	- 当然如果后代元素自己有设置该属性,那么**优先使用**后代元素自己的属性(不管继承过来的属性权重多高);
- 如何知道一个属性是否具有继承性呢?
	- 常见的`font-size/font-family/font-weight/line-height/color/text-align`都具有继承性;
	- 这些不用刻意去记，用的多自然就记住了;
- 另外要多学会查阅文档,文档中每个属性都有标明其继承性的:
- ![image.png](https://article.biliimg.com/bfs/article/bb7691e9961f7e6672cb4d5153e8e0257a8aae64.png)
	✍️注意(了解):
	继承过来的是计算值, 而不是设置值
```css
 .test
    {
		font-size: 2em; 
		/* 32px */
    }
```
```html
<div class="test">
        我是好人
        <div class="box">我是好人</div>
</div>
```
这里继承的是32px计算值，而不是父元素的单个字的两倍

**常见的继承属性有哪些呢?(不用记)**
![image.png](https://article.biliimg.com/bfs/article/844ee329d31dc03e36d9fb5bddf6a3ad135b7c1e.png)

## CSS属性的层叠
### 属性的层叠
- CSS的翻译是层叠样式表,什么是**层叠**呢?
	- 对于一个元素来说,相同一个属性我们可以通过不同的选择器给它进行多次设置;
	- 那么属性会被一层层覆盖上去;
	- 但是最终**只有一个会生效**;
- **那么多个样式属性覆盖上去,哪一个会生效呢?**
	- 判断一:选择器的权重,权重大的生效,**根据权重**可以判断出优先级;
	- 判断二:**先后顺序**,权重相同时,后面设置的生效;
- **那么如何知道元素的权重呢?**
- 优先级比较公式是怎样的?
	- **继承<通配符选择器<标签选择器<类选择器<id选择器<行内样式<!important**
- !important能提升继承的优先级吗?
	- 不能
### 权重叠加计算
场景:如果是复合选择器，此时需要通过权重叠加计算方法，判断最终哪个选择器优先级最高会生效>权重叠加计算公式:(每一级之间不存在进位)
![image.png](https://article.biliimg.com/bfs/article/1f4341825f024bfbdd81186cfcd1bf52a590939d.png)

- 比较规则:
	1. 先比较第一级数字，如果比较出来了，之后的统统不看
	2. 如果第一级数字相同，此时再去比较第二级数字，如果比较出来了，之后的统统不看
	3. 
	4. 如果最终所有数字都相同，表示优先级相同，则比较叠性（准写在下面，谁说了算!)
	注意点: **!important如果不是继承，则权重最高，天下第一!**
## CSS属性的类型
- 在前面我们会经常提到div是**块级元素**会独占一行, span是**行内级元素**会在同一行显示.
	- 到底什么是**块级元素**,什么是**行内级元素**呢?
- HTML定义元素类型的思路:
	- HTML元素有很多，比如h元素/p元素/div元素/span元素/img元素/a元素等等;
	- 当我们把这个元素放到页面上时,这个元素到底占据页面中一行多大的空间呢?
		- √为什么我们这里只说一行呢?因为==垂直方向的高度通常是内容决定的==;
	- 比如一个`h1`元素的标题,我们必然是希望它独占一行的,其他的内容不应该和我的标题放在一起;
	- 比如一个`p`元素的段落,必然也应该独占一行，其他的内容不应该和我的段落放在一起;
	- 而类似于`img/span/a`元素,通常是对内容的某一个细节的特殊描述，没有必要独占一行;
- 所以,为了区分哪些元素需要独占一行，哪些元素不需要独占一行， HTML将元素区分(本质是通过CSS的)成了两类:
	- **块级元素**(block-level elements):独占**父元素**的一行
	- **行内级元素**(inline-level elements):多个行内级元素可以在父元素的同一行中显示
	**通过CSS修改元素类型**
- 前面我们说过,事实上元素没有本质的区别:
	- div是块级元素, span是行内级元素;
	- div之所以是块级元素仅仅是因为浏览器默认**设置了display属性**而已;
	- ![image.png](https://article.biliimg.com/bfs/article/c13777afb75855a380ba81255ff4007d8de4fd94.png)

可以通过display来改变元素的特性呢?
### display属性
CSS中有个display属性，能修改元素的显示类型，有4个常用值
- `block`:让元素显示为块级元素
- `inline`:让元素显示为行内级元素
- `inline-block`:让元素同时具备行内级、块级元素的特征,注意inline-block是
- `none`:隐藏元素
### display值的特性
- `block元素:`
	1. 独占父元素的一行可以，width由父元素决定
	2. 随意设置宽高
	3. 高度默认由**内容**决定
- `inline-block元素:`
	1. 跟其他行内级元素在同一行显示
	2. 可以随意设置宽高，但是默认width是**auto**,由内容决定
	3. 可以这样理解
		- √对外来说，它是一个行内级元素
		- √对内来说，它是一个块级元素
- `inline:`
	1. 跟其他行内级元素在同一行显示;
	2. 不可以随意设置宽高;
	3. 宽高都由**内容**决定;
	4. 默认具有**字符间距**（character spacing）的效果，可以通过不换行和设置父元素字体为0，子元素单独设置取消。
	✍️注意：
	特殊情况，p元素不能包含其他块级元素
	行内级元素（比如`a、span、strong`等)一般情况下，只能包含行内级元素
	img是行内替换元素(`inline-replaced`)
>替换元素（Replaced Element）是指在渲染过程中，浏览器根据元素的标签和属性，用另外一种方式来显示内容的元素。常见的替换元素包括：`<img>`、`<video>`、`<audio>`、`<iframe>` 等。

替换元素的特征：
- 内容不受CSS影响。
- 根据属性来确定其内容等（比如src属性，就会从src对应的URL加载对应的内容）。
- 有自己的尺寸（用于图片等元素的占位符）。
- 可以通过JS控制相关行为。

>而行内替换元素则是指那些可以直接放入文本流中并参与布局的替换元素。这些元素默认情况下都是行内级别的，并且宽高由内容本身决定。常见的行内替换元素除了上述提到的 `<img>` 外，还包括 `<input>`、`<textarea>`、`<select>` 等。


✍️需要特别注意的是，
	行内替换元素虽然可以放在行内或块级元素中，但如果放置在非替换元素内，其默认高度由父级元素指定的 `line-height `属性决定。


## 元素隐藏的方法
- **方法一: display设置为none**
	- 元素不显示出来,并且也不占据位置,==不占据任何空间==(和不存在一样)
- **方法二: visibility设置为hidden**
	- 设置为hidden，虽然元素不可见,但是会占据元素应该==占据的空间==;
	- 默认为visible，元素是可见的;
- **方法三: rgba设置颜色,将a的值设置为0**
	- rgba的a设置的是alpha值,可以设置透明度,==不影响子元素==;
- **方法四: opacity设置透明度,设置为0**
	- 设置整个元素的透明度,==会影响所有的子元素==;
## overflow属性

**overflow用于控制内容溢出时的行为**
- `visible`:溢出的内容照样可见
- `hidden`:溢出的内容直接裁剪 
- `scroll`:溢出的内容被裁剪，但可以通过滚动机制查看
	- 会一直显示滚动条区域，滚动条区域占用的空间属于width、height
- `auto`:自动根据内容是否溢出来决定是否提供滚动机制
	📢扩展
	当一个元素内部的子元素都是浮动元素时，可能会导致其父级元素无法正确地计算高度，从而无法正常显示其背景色、边框等样式。此时，可以将父元素的 `overflow` 属性设置为 `hidden`，这将创建一个新的==块级格式化上下文==，并使得父元素可以自适应其包含的所有浮动元素的高度。换句话说，父元素的 `overflow`属性将被用来清除浮动
# CSS盒子模型
## 认识盒子模型
HTML每个元素都是盒子
事实上，我们可以把HTML每一个元素看出一个个的盒子:
![image.png](https://article.biliimg.com/bfs/article/b488855a37fe1bc92301ecc070680a28a3fde178.png)
### 盒子模型(Box Model)
- HTML中的每一个**元素**都可以看做是一个盒子，如右下图所示，可以具备这4个属性
- ![image.png](https://article.biliimg.com/bfs/article/7673c1221ca18c600738697c6a30b9370582e1f1.png)
- 内容(content)
	- 元素的内容width/height
- 内边距(padding)
	- **元素和内容**之间的间距
- 边框(border)
	- 元素自己的边框
- 外边距(margin)
	- **元素和其他元素**之间的间距(一般用于兄弟之间)

### 盒子模型的四边
因为盒子有四边,所以`margin/padding/border`都包括`top/right/bottom/left`四个边:
![image.png](https://article.biliimg.com/bfs/article/f38b75992b7a6de6633c55856328bef44f0b4638.png)
在浏览器的开发工具中
![image.png](https://article.biliimg.com/bfs/article/ce113f0eda40026907fd3fbb002fe839ab6233a0.png)

## 内容width/height
- **设置内容是通过宽度和高度设置的:** 
	- 宽度设置: width
	- 高度设置: height   
- 注意:对于==行内级非替换元素==来说,设置==宽高是无效的==!
- 另外我们还可以设置如下属性:
	- `min-width:`最小宽度，无论内容多少，宽度都大于或等于min-width
	- `max-width:`最大宽度，无论内容多少，宽度都小于或等于max-width
	- 移动端适配时，可以设置最大宽度和最小宽度 
- 下面两个属性不常用:
	- `min-height:`最小高度，无论内容多少，高度都大于或等于min-height
	- `max-height:`最大高度，无论内容多少，高度都小于或等于max-height
	✍️注意： 
	width: auto；
		1. 行内非替换元素 -> width：包裹内容
		2. 块级元素 -> width:包含块(父级元素)的宽度
		3. 绝对父级元素 -> width：包裹内容
## 内边距padding 
- **padding属性**用于设置盒子的内边距,通常用于设置**边框和内容之间的间距**;
- padding包括四个方向,所以有如下的取值:
	- `padding-top:`上内边距
	- `padding-right:`右内边距
	- `padding-bottom:`下内边距
	- `padding-left:`左内边距
- padding单独编写是一个缩写属性:
- `padding-top. padding-right、padding-bottom、padding-left`的简写
- padding缩写属性是从零点钟方向开始,沿着顺时针转动的,也就是上右下左;
**padding的其他值**
|padding值个数| padding例子 | 代表的含义 |
|:-:|:-:|:-:|
| 4 | padding: 10px 20px 30px 40px; | top: 10px, right: 20px, bottom: 30px, left: 40px; |
| 3 | padding: 10px 20px 30px; | top: 10px, right: 20px, bottom: 30px, left: 20px; |
| 2 | padding: 10px 20px; | top: 10px, right: 20px, bottom: 10px, left: 20px; |
| 1 | padding: 10px; | top: 10px, right: 10px, bottom: 10px, left: 10px; |

## 边框/圆角border
border用于设置盒子的边框:
![image.png](https://article.biliimg.com/bfs/article/e796b9158882639206929e8d516a6b28dcb90cec.png)

- 边框相对于content/padding/margin来说特殊一些:
	- 口边框具备<font color=#ff0000>宽度</font>width;
	- 边框具备<font color=#ff0000>样式</font>style;
	- 边框具备<font color=#ff0000>颜色</font>color;
### 设置边框的方式
- **边框宽度**
	- `border-top-width、border-right-width、border-bottom-width、border-left-width`
	- `border-width`是上面4个属性的简写属性
- **边框颜色**
	- `border-top-color、border-right-color、border-bottom-color、border-left-color`
	- `border-color`是上面4个属性的简写属性
- **边框样式**
	- `border-top-style、border-right-style、 border-bottom-style、border-left-style`
	- `border-style`是上面4个属性的简写属性
### 边框的样式设置值
- **边框的样式有很多，我们可以了解如下的几个:**
	- `groove:`凹槽,沟槽,边框看上去好象是雕刻在画布之内
	- `ridge:`山脊，和grove相反，边框看上去好象是从画布中凸出来
	![image.png](https://article.biliimg.com/bfs/article/84a1bf9078505939f8735f9fdeb09e5e8f2c7f16.png)

**同时设置的方式**
- 如果我们相对某一边同时设置宽度样式颜色,可以进行如下设置:
	- `border-top`
	- `border-right`
	- `border-bottom`
	- `border-left`
	- `border:` 统—设置4个方向的边框
- 边框颜色、宽度、样式的编写顺序任意
	![image.png](https://article.biliimg.com/bfs/article/3b731322fb8820612e03d802e87e6fce3795f689.png)
### 圆角-border-radius
- border-radius用于设置盒子的圆角
- ![image.png](https://article.biliimg.com/bfs/article/8f2710a6249258a532ea4335b7e281411ace608c.png)

- border-radius常见的值:
	- `数值`:通常用来设置小的圆角,比如6px;
	- `百分比`:通常用来设置一定的弧度或者圆形,这个%是`border-box`的百分比
	**border-radius事实上是一个缩写属性:**
- 将这四个属性border-top-left-radius、border-top-right-radius、border-bottom-right-radius，和border-bottom-left-radius简写为一个属性。
- 开发中比较少见一个个圆角设置;
- 如果一个元素是正方形，设置border-radius大于或等于50%时，就会变成一个圆 
- ![image.png](https://article.biliimg.com/bfs/article/1b4f21d5cd10f91061867323ba6001a8922a6704.png)
📢扩展：
`box-sizing` 是 CSS 盒模型的一个属性，它用来指定元素的盒模型如何计算尺寸。该属性有三个可能的值： 
- `content-box`：默认值，表示元素的 `width` 和 `height` 只包含内容（content），不包括内边距（padding）和边框（border）；
- `border-box`：表示元素的 `width` 和 `height` 包括了内容（content）、内边距（padding）和边框（border）；
- `padding-box`：表示元素的 `width` 和 `height` 包括了内容（content）和内边距（padding），但不包括边框（border）。

## 外边距margin

- margin属性用于设置盒子的外边距,通常用于<font color=#ff0000>元素和元素之间的间距</font>;
- margin包括四个方向,所以有如下的取值:
	- `margin-top:` 上内边距
	- `margin-right:`右内边距
	- `margin-bottom:`下内边距
	- `margin-left:`左内边距
- margin单独编写是一个缩写属性:
	- `margin-top、margin-right、margin-bottom、margin-left`的简写属性
	- margin缩写属性是从零点钟方向开始,沿着顺时针转动的,也就是上右下左;
- margin也并非必须是四个值,也可以有其他值;
### 上下margin的传递
- **margin-top传递**
	- 如果<font color=#ff0000>块级元素</font>的顶部线和<font color=#ff0000>父元素</font>的顶部线重叠，那么这个块级元素的`margin-top`值会传递给父元素
- **margin-bottom传递**
	- 如果块级元素的底部线和父元素的底部线<font color=#ff0000>重写</font>，并且<font color=#ff0000>父元素的高度是auto</font>，那么这个块级元素的`margin-bottom`值会传递给父元素
- **如何防止出现传递问题?**
	1. 给父元素设置`padding-top\padding-bottom`
	2. 给父元素设置`border`
	3. 触发BFC:<font color=#ff0000>设置overflow为auto</font>
		- overflow：auto触发了盒子的BFC----给当前的盒子建立一个独立的空间，就不受外界的影响
- 建议
	- margin一般是用来设置兄弟元素之间的间距
	- padding一般是用来设置父子元素之间的间距
### 上下margin的折叠
- 垂直方向上相邻的2个margin (margin-top、margin-bottom)有可能会合并为1个margin，这种现象叫做**collapse (折叠)**
- **水平方向上**的margin (margin-left、margin-right)永远不会collapse
- 折叠后最终值的计算规则
	- 两个值进行比较，取==较大==的值
- 如何防止margin collapse?
	- 只设置其中一个元素的margin
	**上下margin折叠的情况**
- <font color=#ff0000>两个兄弟块级元素之间</font>上下margin的折叠
- <font color=#ff0000>父子块级元素之间</font>margin的折叠
- ![image.png](https://article.biliimg.com/bfs/article/4eb6d7bd0579c38f7f3c9fb9a97d3fe9b5970bcd.png)
	📢扩展为什么margin：0,auto 可以让块级元素居中 
	块级元素block box
	**block box width = width + padding + border + margin**
	块级元素一定要占据父元素的一行，根据优先级width先生效，width不够，那么magrgin-right会来凑，margin-left:auto和margin-right:auto会自动平均分配 
## 外轮廓– outline
- outline表示元素的<font color=#ff0000>外轮廓</font>
	- **不占用空间**
	- 默认显示在border的外面
- outline相关属性有
	- `outline-width`: 外轮廓的宽度
	- `outline-style`:取值跟border的样式一样，比如solid、dotted等
	- `outline-color`:外轮廓的颜色
	- `outline`: outline-width、outline-style、outline-color的简写属性，跟border用法类似
- 应用实例
	- 去除a元素、input元素的focus轮廓效果

## 盒子阴影
### box-shadow
- box-shadow属性可以设置一个或者多个阴影
	- 每个阴影用`<shadow>`表示
	- 多个阴影之间用逗号`,`隔开，从前到后**叠加**
- `<shadow>`的常见格式如下
	- ![image.png](https://article.biliimg.com/bfs/article/d88cf748d534500d78302230c34e2523f8e4f52c.png)
	- 第1个`<length>` : `offset-x`，水平方向的偏移，正数往右偏移
	- 第2个`<length>` : `offset-y`,垂直方向的偏移，正数往下偏移
	- 第3个`<length>` : `blur-radius`,模糊半径
	- 第4个`<length>` : `spread-radius`,延伸半径
	- `<color>`:阴影的颜色，如果没有设置，就跟随color属性的颜色
	- `inset`:外框阴影变成内框阴影

我们可以通过一个网站测试盒子的阴影:
	[Box Shadow CSS Generator | 𝗧𝗛𝗘 𝗕𝗘𝗦𝗧 𝗢𝗡𝗟𝗜𝗡𝗘 𝗖𝗦𝗦 𝗚𝗘𝗡𝗘𝗥𝗔𝗧𝗢𝗥](https://html-css-js.com/css/generator/box-shadow/)
	![image.png](https://article.biliimg.com/bfs/article/62f363ddff1c4cb505e2da6e9ee75233bf49f262.png)

### text-shadow
- text-shadow用法类似于box-shadow，用于给文字添加阴影效果
- `<shadow>`的常见格式如下
	- ![image.png](https://article.biliimg.com/bfs/article/ed9bb6443f3e8de5f19cb834e4afce4cfe1ae847.png)
	- 相当于box-shadow，它没有spread-radius的值;
- 我们可以通过一个网站测试文字的阴影:
	[Box Shadow CSS Generator | 𝗧𝗛𝗘 𝗕𝗘𝗦𝗧 𝗢𝗡𝗟𝗜𝗡𝗘 𝗖𝗦𝗦 𝗚𝗘𝗡𝗘𝗥𝗔𝗧𝗢𝗥](https://html-css-js.com/css/generator/box-shadow/)
	![image.png](https://article.biliimg.com/bfs/article/d5da3b4c2c5a338b27fd6b78616befe2c3b5934d.png)
## 行内非替换元素的注意事项
- **以下属性对行内级非替换元素不起作用**
	- width、height、margin-top、margin-bottom
- **以下属性对行内级非替换元素的效果比较特殊**
	- padding-top、padding-bottom、上下方向的border
	- 上下会被撑起来，但是不占据空间
```css
/*·内边距: padding· */
/*·特殊:上下会被撑起来，但是不占据空间*//* padding:.5epx;*/
/*·边框: border·*/
/*-特殊:上下会被撑起来，但是不占据空间*//*border:· 50px solid orange; */
/*.外边距: margin· */
/*·特殊:·上下的margin是不生效的*/
```

扩展📢
- 当我们的盒子中的文本超过盒子的宽度，想要用(...)省略，可以采用下面3个步骤：
	1. `white-space:nowrap` ➡️当设置为 `nowrap` 时，文本不会换行，只在一行上显示。这可以防止文本因为太长而被自动分割到新行。
	2. `overflow:hidden`➡️文本长度超过了父元素指定的宽度，就会将超出部分隐藏，不会被显示出来。
	3. `text-overflow:ellipsis`➡️如果文本长度超过了父元素指定的宽度，并且又被隐藏了，就会在文本末尾添加省略号（...）
- 实现在网页上显示==特定行数==的文本，而不会占满整个页面或父元素的高度。
1. `display: -webkit-box` 是一个旧的布局模式，可用于实现基于 Flexbox 等理念的自适应布局。它是 Webkit 内核的私有 CSS 样式。
2. `-webkit-line-clamp` 属性被用于限制在一个块元素中显示的文本的行数，并通过在超出指定数量的行数后将其截断为省略号来实现。
3. `-webkit-box-orient` 属性可以用于设置弹性容器的主轴方向。默认值为水平方向（横向排列），当设为 `vertical` 时，则按垂直方向（纵向排列）堆叠弹性子元素。

# CSS设置背景

## background-image
- `background-image`用于设置元素的背景图片
	- 会**盖**在(不是覆盖)background-color的上面
- 如果设置了多张图片
	- 设置的第一张图片将显示在最上面，其他图片**按顺序层叠**在下面
- **注意:如果设置了背景图片后，元素没有具体的宽高，背景图片是不会显示出来的**
## background-repeat
- background-repeat用于设置背景图片是否要平铺
- 常见的设值有
	- `repeat`:平铺
	- `no-repeat`:不平铺
	- `repeat-x`:只在水平方向平铺
	- `repeat-y`:只在垂直平方向平铺 
- ![image.png](https://article.biliimg.com/bfs/article/f7fd1c0503a7eeec338bf2ee3d77a6747a9da16b.png)

## background-size
- **background-size**用于设置背景图片的大小
	- `auto`:默认值,以背景图本身大小显示
	- `cover`:缩放背景图，以完全覆盖铺满元素,可能背景图片部分看不见
	- `contain`:缩放背景图，宽度或者高度铺满元素，但是图片保持宽高比
	- `<percentage>`:百分比，相对于背景区(background positioning area)
	- `length`:具体的大小，比如100px
- ![image.png](https://article.biliimg.com/bfs/article/25eca6a198493aa3a85aeddc9f2bfd78b48f1a4b.png)

## background-position
- background-position用于设置背景图片在水平、垂直方向上的具体位置
	- `background-position`:水平方向位置 垂直方向位置 
	- 可以设置==具体的数值==比如20px 30px;
	- 水平方向还可以设值:`left、center、right`
	- 垂直方向还可以设值: `top、center、bottom`
	- 如果只设置了1个方向，**另一个方向默认是center**
	- ![image.png](https://article.biliimg.com/bfs/article/8db1ca2bcc4b19b461399a360071ae78499acab1.png)
	👩‍🔧应用：
	我们可以用`background-position：center center`,达到拖动浏览器滚动条，而背景图片一直在中心区域。

## background-attachment
**background-attachment决定背景图像的位置是在视口内固定，或者随着包含它的区块滚动。**
- 可以设置以下3个值
	- `scroll`: 此关键属性值表示背景相对于元素本身固定，而不是随着它的内容滚动
	- `local`:此关键属性值表示背景相对于元素的内容固定。如果一个元素拥有滚动机制，背景将会随着元素的内容滚动.
	- `fixed`:此关键属性值表示背景相对于视口固定。即使一个元素拥有滚动机制，背景也不会随着元素的内容滚动。
## background
**background是一系列背景相关属性的简写属性**
- 常用格式是
- ![image.png](https://article.biliimg.com/bfs/article/4dda37b47f1302b5059b1634616a8741000dfe36.png)
- background-size可以省略，如果不省略，`/background-size`必须紧跟在`background-position`的后面其他属性也都可以省略，而且顺序任意

### background-image和img对比
- 利用background-image和img都能够实现显示图片的需求，在开发中该如何选择?
|  |`img`| `background-image` |
|:-:|:-:|:-:|
|性质|HTML元素|CSS样式|
| 图片是否占用空间 | 是 | 否 |
| 浏览器右键直接查看地址 | 是 | 否 |
| 支持CSS Sprite | 是 | 是 |
| 更有可能被搜索引擎收录 | 是（结合`alt`属性） | 否 |

**总结**
- img，作为网页内容的重要组成部分，比如广告图片、LOGO图片、文章配图、产品图片
- background-image，可有可无。有，能让网页更加美观。无，也不影响用户获取完整的网页内容信息

# Emmet和结构伪类
## Emmet
### 认识Emmet
- **Emmet(前身为zen Coding)是一个能大幅度<font color=#ff0000>提高前端开发效</font>率的一个工具.**
- 在前端开发的过程中，一大部分的工作是写HTML、CSs代码,如果手动来编写效果会非常低.
- VsCode内置了<font color=#ff0000>Emmet语法</font>,在后缀为.html/.css中输入缩写后按Tab/Enter键即会自动<font color=#ff0000>生成相应代码</font>
- !和html:5可以快速生成完整结构的html5代码
- ![image.png](https://article.biliimg.com/bfs/article/d31c948e81c95a473f1ebff58493fbcb2fd959a3.png)
### 常见Emmet语法
#### `>`(子代)和`+`(兄弟)
- `div>ul>li`
![image.png](https://article.biliimg.com/bfs/article/fcbd0dd4376eb62716d1557bd8028a1fba7b9ab7.png)

- `div+div>p>span+i`
![image.png](https://article.biliimg.com/bfs/article/0dbdf75dca37edce6807345f207403ef53b42c53.png)

- `div+p+ul>li`
![image.png](https://article.biliimg.com/bfs/article/9fc398950d07366ae0393d56425e802bcb447de7.png)
#### `*（多个）和^（上一级）`
- `ul>li*5`
![image.png](https://article.biliimg.com/bfs/article/60810c3f6093414170e70fcbc20a2ea9cce97d17.png)

- `div+div>p>span^h1`
![image.png](https://article.biliimg.com/bfs/article/b022f4193d65efc2515746d5859720a975eda97c.png)
#### ()（分组）
- `div>(header>ul>li*2>a)+footer>p`
![image.png](https://article.biliimg.com/bfs/article/e7d6c600a1b8b3b4f151803f44e64ba569d10dc1.png)

#### 属性(id属性、class属性、普通属性) {}（内容）
- `div#header+div#main>.container>a[href]`
![image.png](https://article.biliimg.com/bfs/article/b31e8551fb5658b85e44be532ffd2eda6f4db908.png)

- `a[href="http://www.baidu.com"]{百度一下}`
![image.png](https://article.biliimg.com/bfs/article/4c4082500c713a8fa5423ec162d18befc018c1fb.png)
#### $（数字）
- `ul>li.item$*5`
![image.png](https://article.biliimg.com/bfs/article/d56d0655574149c661c90bcc8854dd425c317734.png)
#### 隐式标签
![image.png](https://article.biliimg.com/bfs/article/5ed90c0c046938e6a1bb6d8ac4b30eb35fcdba12.png)
- `ul>.item*3
![image.png](https://article.biliimg.com/bfs/article/baf813734f26d8e973b6d9a5a3a9b84e2ff3171a.png)

#### CSS Emmet
1. `w100` 
2. `w20+h30+m40+p50`
3. `m20-30-40-50`
	![image.png](https://article.biliimg.com/bfs/article/91d0bd1667a1e51b866643445e9c31b18df5a5a8.png)
1. dib
2. bd1#cs 
![image.png](https://article.biliimg.com/bfs/article/bcad28c180d3615f03b13c128c437bb5d9431495.png)
## 结构伪类
### 常见的结构伪类
#### 结构伪类 -`:nth-child`
- `:nth-child(1)`
	- 是父元素中的第1个子元素
- `:nth-child(2n)`
	- n代表任意正整数和O
	- 是父元素中的第偶数个子元素（第2、4、6、8.……..个)
	- 跟:`nth-child(even)`同义
- `:nth-child(2n + 1)`
	- n代表任意正整数和O
	- 是父元素中的第奇数个子元素(第1、3、5、7..….个)
	- 跟`:nth-child(odd)`同义
- `:nth-child(-n + 2)`
	- 代表前2个子元素
#### 结构伪类- `:nth-last-child( )`
- :`nth-last-child()`的语法跟`:nth-child()`类似，不同点是`:nth-last-child()`从最后一个子元素开始往前计数
	- `:nth-last-child(1)`，代表倒数第一个子元素
	- `:nth-last-child(-n + 2)`，代表最后2个子元素
- `:nth-of-type()`用法跟`:nth-child()`类似
	- 不同点是`:nth-of-type()`计数时只计算==同种类型的元素==
- `:nth-last-of-type()`用法跟`:nth-of-type()`类似
	- 不同点是`:nth-last-of-type)`从最后一个这种类型的子元素开始往前计数
#### 结构伪类- `:nth-of-type()、:nth-last-of-type()`
**其他常见的伪类(了解):**
- `:first-child`，等同于:nth-child(1)
- `:last-child`，等同于:nth-last-child(1)
- `:first-of-type`，等同于:nth-of-type(1)
- `:last-of-type`，等同于:nth-last-of-type(1)
- `:only-child`，是父元素中唯一的子元素
- `:only-of-type`，是父元素中唯一的这种类型的子元素
**下面的伪类偶尔会使用:**
- `:root`，根元素，就是HTML元素
- `:empty`代表里面完全空白的元素
### 否定伪类的使用
- :not()的格式是:not(x)
	- x是一个==简单选择器==
	- 元素选择器、通用选择器、属性选择器、类选择器、id选择器、伪类（除否定伪类)
- :not(x)<font color=#ff0000>表示除x以外的元素</font>

# 额外知识补充
## border图形
**border主要是用来给盒子增加边框的,但是在开发中我们也可以利用边框的特性来实现一些形状:**
![image.png](https://article.biliimg.com/bfs/article/ea53b894166e25cb284ec226e5f75bfef18ab1f0.png)

- 假如我们将border宽度设置成50会是什么效果呢?
	- 如果我们进一步,将另外三边的颜色去除呢?
	- 如果我们将这个盒子旋转呢?
- 所以利用border或者CSS的特性我们可以做出很多图形:
	- [The Shapes of CSS | CSS-Tricks - CSS-Trick](https://css-tricks.com/the-shapes-of-css/#top-of-site)
## Web网络字体
- **在之前我们有设置过页面使用的字体: font-family**
	- 我们需要提供**一个或多个**字体种类名称，浏览器会在**列表中搜寻**，直到找到它所运行的系统上可用的字体。
	- 这样的方式完全没有问题，但是对于传统的web开发人员来说，字体选择是有限的;
	- 这就是所谓的`Web-safe`字体;
	- 并且这种默认可选的字体并不能进行一些**定制化**的需求;
- **比如下面的字体样式，系统的字体肯定是不能实现的**
![image.png](https://article.biliimg.com/bfs/article/76f25e81bb2721bce9107af5732ba2c5afe1b3bc.png)

- 那么我们是否依然可以在网页中使用这些字体呢?使用`Web Fonts`即可.
### Web fonts的工作原理
- 首先,我们需要通过一些渠道**获取到希望使用的字体**(不是开发来做的事情):
	- 对于某些收费的字体,我们需要获取到对应的授权;
	- 对于某些公司定制的字体,需要设计人员来设计;
	- 对于某些免费的字体，我们需要获取到对应的字体文件;
- 其次,在我们的CSS代码当中使用该字体(重要):
	- 具体的过程看后面的操作流程;
- 最后，在**部署静态资源**时,将`HTML/CSS/JavaScript/Font`一起部署在静态服务器中;
- 用户的角度:
	- 浏览器一个网页时，因为代码中有引入字体文件,字体文件会被一起<font color=#ff0000>下载</font>下来;
	- 浏览器会根据使用的字体在下载的字体文件中<font color=#ff0000>查找、解析、使用</font>对应的字体;
	- 在浏览器中使用对应的字体显示内容;
### web-fonts的兼容性
- 我们刚才使用的字体文件是`.ttf`,它是`TrueType`字体.
	- 在开发中某些浏览器可能不支持该字体，所以为了浏览器的兼容性问题,我们需要有对应其他格式的字体;
- TrueType字体:拓展名是.ttf
	- `OpenType/TrueType`字体:拓展名是`.ttf、.otf`,建立在TrueType字体之上
	- `Embedded OpenType`字体:拓展名是`.eot`,OpenType字体的压缩版
	- `SVG`字体:拓展名是.`svg、.svgz`
	- `WOFF`表示Web Open Font Format web开放字体:拓展名是`.woff`，建立在TrueType字体之上
- 这里我们提供一个网站来生产对应的字体文件:
[在线字体编辑器-JSON在线编辑器](https://font.qqe2.com/)
### web fonts兼容性写法
- 如果我们具备很强的兼容性,那么可以如下格式编写:
```css
@font-face {
	font-family: "hyfont03";
	src: url("./fonts2/AaJianHaoTi.eot");
	src: ur1("./fonts2/AaJianHaoTi.eot?#iefix" ) format("embedded-opentype"),
	url("./fonts2/AaJianHaoTi.woff") format("woff""),
	url("./fonts2/AaJianHaoTi.ttf") format( "truetype"),
	url("./fonts2/AaJianHaoTi.svg#uxfonteditor") format( "svg");
}
```
- 这被称为"bulletproof @font-face syntax(刀枪不入的@font-face语法)
	- 这是`Paul lrish`早期的一篇文章提及后@font-face开始流行起来
- `src`用于指定字体资源
	- `url`指定资源的路径
	- `format`用于帮助浏览器快速识别字体的格式;
## Web字体图标
### 认识字体图标
- 思考:字体可以设计成各式各样的形状，那么能不能把字体直接设计成图标的样子呢?
	- 当然可以，这个就叫做<font color=#ff0000>字体图标</font>。
- 字体图标的好处:
	- 放大<font color=#ff0000>不会失真</font>
	- 可以任意<font color=#ff0000>切换颜色</font>
	- 用到很多个图标时，文件相对图片较小
- 字体图标的使用:
	- 口登录阿里icons(https://www.iconfont.cn/)
	- 下载代码，并且拷贝到项目中
- **将字体文件和默认的css文件导入到项目中**
### 字体图标的使用
- 字体图标的使用步骤:
	- 第一步:通过link引入iconfont.css文件
	- 第二步:使用字体图标
- **使用字体图标常见的有两种方式:**
	- 方式一:通过对应字体图标的<font color=#ff0000>Unicode</font>来显示代码;
	- 方式二:利用已经编写好的<font color=#ff0000>class</font>,直接使用即可;
	- ![image.png](https://article.biliimg.com/bfs/article/5db7947e4eef1416656c554299770acd845a2238.png)

## CSS精灵图
### 认识精灵图CSS Sprite
- **什么是CSS Sprite**
	- 是一种CSS图像<font color=#ff0000>合成技术</font>，将各种小图片合并到一张图片上，然后利用CSS的<font color=#ff0000>背景定位</font>来显示对应的图片部分
	- 有人翻译为: CSS雪碧、CSS精灵
- **使用CSS Sprite的好处**
	1. 减少网页的<font color=#ff0000>http请求数量，加快网页响应速度，减轻服务器压力</font>
	2. 减小<font color=#ff0000>图片总大小</font>
	3. 解决了图片<font color=#ff0000>命名的困扰</font>，只需要针对—张集合的图片命名
- **Sprite图片制作（雪碧图、精灵图)**
	- 方法1: Photoshop，设计人员提供
	- 方法2: https://www.toptal.com/developers/css/sprite-generator
### 精灵图的使用
- 精灵图如何使用呢?
	- 精灵图的原理是通过只显示图片的<font color=#ff0000>很小一部分来展示</font>的;
- 通常使用背景:
	1. 设置对应元素的宽度和高度
	2. 设置精灵图作为背景图片
	3. 调整背景图片的<font color=#ff0000>位置来展示</font>
- 如何获取精灵图的位置
http://www.spritecow.com/
![image.png](https://article.biliimg.com/bfs/article/0bf6c0ed6d4a847a494e1f7fc324d88d6fdfa3fd.png)

## cursor属性
- cursor可以设置<font color=#ff0000>鼠标指针(光标)</font>在元素上面时的显示样式
- cursor常见的设值有
	- `auto:`浏览器根据<font color=#ff0000>上下文决定</font>指针的显示样式，比如根据文本和非文本切换指针样式
	- `default:`由<font color=#ff0000>操作系统决定</font>，一般就是一个小箭头
	- `pointer`:一只小手，鼠标指针挪动到链接上面默认就是这个样式
	- `text`:一条竖线，鼠标指针挪动到文本输入框上面默认就是这个样式
	- `none`:没有任何指针显示在元素上面
# CSS元素定位
## 标准流布局
- 默认情况下，元素都是按照`normal flow`（标准流、常规流、正常流、文档流【document flow】）进行
- 从左到右、从上到下按顺序摆放好
- 默认情况下，互相之间不存在<font color=#ff0000>层叠</font>现象
- ![image.png](https://article.biliimg.com/bfs/article/55851d90e5eac2f58c5d9298cdbfe1ace6241d5c.png)
```html
<body>
	<span>span1</span>
	<img src="images/ cube.jpg" alt="">
	<span style="display: inline-block" >span2</span>
	<div>div</div>
	<p>p</p>
	<span style="display: inline-block" >span</span>
	<strong>strong</strong>
	<h1>h1</h1>
	<span>span3</span>
	<span style="display: inline-block " >span4</span>
	<span>span5</span>
</body>
```

### margin-padding 位置调整的缺点
- **在标准流中，可以使用margin、padding对元素进行定位**
	- 其中margin还可以设置负数
- 比较明显的缺点是
	- 设置一个元素的margin或者padding，通常会<font color=#ff0000>影响到标准流中其他元素的定位效果</font>
	- 不便于<font color=#ff0000>实现元素层叠的效果</font>
- **如果我们希望一个元素可以跳出标准量,单独的对<font color=#ff0000>某个元素进行定位</font>呢?**
	- 我们可以通过position属性来进行设置;
### 认识元素的定位
- 定位允许您从正常的文档流布局中<font color=#ff0000>取出元素</font>，并使它们具有不同的行为:
		- 例如放在另一个元素的上面;
		- 或者始终保持在浏览器视窗内的同一位置;
- 定位在开发中非常常见:
![image.png](https://article.biliimg.com/bfs/article/c4a97a43f39c5e141b610a30d8d5a3c5f01a81ff.png)

### 认识position属性
- 利用position可以对元素进行定位，常用取值有5个:
![image.png](https://article.biliimg.com/bfs/article/51cac52c3bcef9f352b306cda9c9e377255d1d3c.png)

- 默认值:
	- `static`:默认值,静态定位
- 使用下面的值,可以让元素变成**定位元素**(positioned element)
	- `relative`:相对定位
	- `absolute`: 绝对定位
	- `fixed`:固定定位
	- `sticky`:粘性定位
### 静态定位- static
**position属性的默认值**
- 元素按照<font color=#ff0000>normal flow</font>布局
- `left 、right、top、bottom`没有任何作用
## 相对定位
- 元素按照<font color=#ff0000>normal flow</font>布局
- 可以通过`left、right、top.bottom`进行定位
	- 定位参照对象是==元素自己原来==的位置
- left、right、top、bottom用来设置元素的具体位置，对元素的作用如下图所示
- 相对定位的应用场景
	- 在==不影响其他元素位置==的前提下，对当前元素位置进行微调
	![image.png](https://article.biliimg.com/bfs/article/4948068aab7dc4d3213b3df64db900b9b839c685.png)

## 固定定位
### 固定定位- fixed
- 元素<font color=#ff0000>脱离normal flow</font>(脱离标准流、脱标)
- 可以通过`left、right、top、bottom`进行定位
- 定位参照对象是视口(viewport)--是浏览器窗口的大小。
- 当画布滚动时，固定不动
### 画布和视口
- **视口(Viewport)**
	- 文档的可视区域
	- 如右图<font color=#ff0000>红框</font>所示
	![image.png](https://article.biliimg.com/bfs/article/7267d873021c8207ef575f40cc3ad87259b6c972.png)

- **画布(Canvas)**
	- 用于渲染文档的区域
	- 文档内容超出视口范围，可以通过滚动查看
	- 如右图<font color=#ff0000>黑框</font>所示
- **宽高对比**
- <font color=#ff0000>画布>=视口</font>
## 绝对定位
### 绝对定位- absolute
- 元素<font color=#ff0000>脱离normal flow</font>(脱离标准流、脱标)
- 可以通过left、right、top.bottom进行定位
	- 定位参照对象是<font color=#ff0000>最邻近的定位祖先元素</font>
	- 如果<font color=#ff0000>找不到这样的祖先元素，参照对象是视口</font>
- 定位元素(positioned element)
	- position值不为`static`的元素
	- 也就是position值为`relative、absolute、fixed`的元素
### 子绝父相
- 在绝大数情况下，子元素的绝对定位都是相对于<font color=#ff0000>父元素进行定位</font>
- 如果希望子元素相对于父元素进行定位，又不希望父元素脱标，常用解决方案是:
	- 父元素设置`position: relative`(让父元素成为定位元素，而且父元素不脱离标准流)
	- 子元素设置`position: absolute`
	- 简称为<font color=#ff0000>“子绝父相</font>”
### 将position设置为absolute/fixed元素的特点(一)
- **可以==随意设置宽高==**
- **宽高默认==由内容决定==**
- **不再受标准流的约束**
	- 不再严格按照从上到下、从左到右排布
	- ==不再严格区分块级(block)、行内级(inline)，行内块级(inline-block)的很多特性都会消失==
- **不再给父元素汇报宽高数据**
- **脱标元素内部默认还是按照标准流布局**
### 将position设置为absolute/fixed元素的特点(二)
- **绝对定位元素(absolutely positioned element)**
	- position值为`absolute`或者`fixed`的元素
- 对于绝对定位元素来说
	- <font color=#ff0000>定位参照对象的宽度= left + right + margin-left + margin-right +绝对定位元素的实际占用宽度</font>
	- <font color=#ff0000>定位参照对象的高度= top + bottom + margin-top + margin-bottom +绝对定位元素的实际占用高度</font>
#css小技巧
- **如果希望绝对定位元素的<font color=#ff0000>宽高和定位参照对象一样</font>，可以给绝对定位元素设置以下属性**
	- `left: 0、right: 0、top: 0、bottom: O、margin:0`
- 如果希望绝对定位元素在定位参照对象中<font color=#ff0000>居中显示</font>，可以给绝对定位元素设置以下属性
	- `left: 0、right: 0、top: 0、bottom: 0、margin: auto`
	- 另外，还得设置**具体的宽高值**（宽高小于定位参照对象的宽高)
## 粘性定位
- 另外还有一个定位的值是`position: sticky`，比起其他定位值要新一些.
	- sticky是一个大家期待已久的属性;
	- 可以看做是<font color=#ff0000>相对定位和固定(绝对)定位的结合体</font>;
	- 它允许被定位的元素表现得像相对定位一样，直到它滚动到某个阈值点;当达到这个阈值点时,就会变成固定(绝对)定位;
	- **意思就是滚动条滚动到它定位的距离后就从相对定位变为绝对定位，这是为了在滚动页面时可以一直看到某些元素**，
	- ![image.png](https://article.biliimg.com/bfs/article/6a1c58a68c8868aeb06c4507746e87b312fda451.png)
- sticky是相对于最近的<font color=#ff0000>滚动祖先包含滚动视口的</font>(the nearest ancestor scroll container’ s scrollport )这里的祖先不一定时视口，overflow:scroll也会有滚动

### **position值对比**

| 类型      | 脱离标准流 |定位元素| 绝对定位元素 |
|:-:|:-:|:-:|:-:|
|`static`| ×          | ×        | ×            |
|`relative`| ×          | √        | ×            |
|`absolute`| √          | √        | √            |
|`fixed`| √          | √        | √            |

## z-index
- z-index属性用来设置定位元素的<font color=#ff0000>层叠顺序</font>(==仅对定位元素有效==)
	- 取值可以是正整数、负整数、0
- 比较原则
	- 如果是<font color=#ff0000>兄弟关系</font>
		- z-index越大，层叠在越上面
		- z-index相等，写在后面的那个元素层叠在上面
	- 如果不是兄弟关系
		- 各自<font color=#ff0000>从元素自己以及祖先元素中，找出最邻近的2个定位元素进行比较</font>
		- 而且<font color=#ff0000>这2个定位元素必须有设置z-index的具体数值</font>

# CSS元素浮动
## 认识浮动
- float属性可以指定一个元素应<font color=#ff0000>沿其容器</font>的<font color=#ff0000>左侧</font>或<font color=#ff0000>右侧</font>放置，允许文本和内联元素环绕它。
	- float属性最初只用于在一段文本内<font color=#ff0000>浮动图像</font>,<font color=#ff0000>实现文字环绕的效果</font>;
	- 但是早期的CSS标准中并没有提供好的<font color=#ff0000>左右布局方案</font>，因此在一段时间里面它成为<font color=#ff0000>网页多列布局</font>的最常用工具;
- **绝对定位、浮动都会让元素脱离标准流，以达到灵活布局的效果**
- **可以通过float属性让元素产生浮动效果，float的常用取值**
	- `none`: 不浮动，默认值
	- `left`:向左浮动
	- `right`:向右浮动
## 浮动的规则
### 规则一
- **元素一旦浮动后，脱离标准流**
	- 朝着向左或向右方向移动，直到<font color=#ff0000>自己的边界紧贴着包含块</font>（一般是父元素）或者<font color=#ff0000>其他浮动元素的边界</font>为止
	- <font color=#ff0000>定位元素会层叠在浮动元素上面</font>
	- ![image.png](https://article.biliimg.com/bfs/article/ffd6494735d651e4ded360c686977885f3d2b59a.png)
如下代码p元素会盖在 `div` 元素上方
```html
<!DOCTYPE html>
<html>
<head>
    <style>
        div {
            background-color: red;
            width: 100px;
            height: 100px;
            float: left;
        }
        p {
            background-color: green;
            margin-left: 120px;
        }
    </style>
</head>
<body>
    <div></div>
    <p>This is a paragraph.</p> 
</body>
</html>
```

### 规则二
**浮动元素之间不能层叠**
- 如果一个元素浮动，另一个浮动元素已经在那个位置了，后浮动的元素将紧贴着前一个浮动元素（左浮找左浮，右浮找右浮)
- 如果水平方向剩余的空间不够显示浮动元素，浮动元素将向下移动，直到有充足的空间为止
### 规则三

- 浮动元素不能与行内级内容层叠，行内级内容将会被浮动元素推出
	- 浮动元素只能在当前行进行浮动。
	- 口比如行内级元素、inline-block元素、块级元素的文字内容
	- ![image.png](https://article.biliimg.com/bfs/article/0fd17b600bd0e1e91722dd57e89c7d846cd370d2.png)
### 规则四
1. 当元素应用了浮动属性（float: left/right），会导致该元素从文档流中出去，并且之后的元素可以环绕着它，这种现象被称作“浮动”。
2. 在该情况下，如果一个元素同时应用了`display: inline-block`和`float: left/right`，那么inline-block的声明会被==忽略==。
3. 实际上，将一个行内元素设置为浮动后，并不会使它成为块级元素。而是会产生==类似行内块元素==的表现。
### 规则五
浮动的元素会脱离标准流，如果它有父元素->就==不会给父元素汇报高度==
## 浮动的案例
#css技巧 
父级盒子宽度=子盒子+子盒子ml+子盒子mr
我们可以利用这个定理解决margin-right:10超出部分自动往下排的情况
我们在想要排列的浮动盒子添加一个父元素，设置父元素的mr=-10，那么子元素宽度总和就是扩充10px，默认的子元素宽度总和是auto，也就是占据父元素一整行 
```html
.item
    {
       float: left;
       width: 290px;
       margin-top: 10px;
       margin-right: 10px;
       background-color: red;
    }
 .wrap
    {
      margin-right: -10px;
    }
 <div class="content">
    <div class="wrap">
      <div class="item left l1"></div>
      <div class="item left l2"></div>
      <div class="item right"></div>
      <div class="item right"></div>
      <div class="item right"></div>
      <div class="item right"></div>
      <div class="item right"></div>
      <div class="item right"></div>
      <div class="item right"></div>
      <div class="item right"></div>
    </div> 
```
为什么做边框麻烦?
1. 边框给谁加？
2. 加上边框之后，宽度如何计算
## 清除浮动
### 浮动的问题 – 高度塌陷
- 由于浮动元素脱离了标准流，变成了脱标元素，所以<font color=#ff0000>不再向父元素汇报高度</font>
	- 父元素<font color=#ff0000>计算总高度</font>时，就<font color=#ff0000>不会计算浮动子元素的高度</font>，导致了<font color=#ff0000>高度坍塌</font>的问题
- **解决父元素高度坍塌问题的过程，一般叫做清浮动（清理浮动、清除浮动)**
- **清浮动的目的是**
	- 让<font color=#ff0000>父元素计算总高度的时候</font>，把<font color=#ff0000>浮动子元素的高度算进去</font>
### CSS属性- clear
clear属性是做什么的呢?
clear属性可以指定一个元素是否必须==移动(清除浮动后)到在它之前所有的浮动元素下面;==
clear的常用取值
- `left`:要求元素的顶部低于之前生成的所有<font color=#ff0000>左浮动元素</font>的底部
- `right`:要求元素的顶部低于之前生成的所有<font color=#ff0000>右浮动元素</font>的底部
- `both`:要求元素的顶部低于之前生成的所有浮动元素的底部
- `none`:默认值，无特殊要求
那么我们可以利用这个特性来清除浮动.
### 清除浮动的方法
- **事实上我们有很多方法可以清除浮动**
- 方法一:给父元素设置固定高度
	- 扩展性不好（不推荐)
- 方法二:在父元素最后增加一个<font color=#ff0000>空的块级子元素</font>，并且让它设置`clear: both`
	- 会增加很多<font color=#ff0000>无意义的空标签</font>，维护麻烦
	- 违反了<font color=#ff0000>结构与样式分离</font>的原则(不推荐)
	```css
	.line {
	      clear: both;
	    }
	....
	<div class="line"></div> 
	```
- 方法三:给父元素添加一个伪元素
	- 推荐;
	- 编写好后可以轻松实现清除浮动;
	- 给父元素增加`::after`伪元素
		- 纯CSS样式解决，结构与样式分离(推荐)

		```css
		.clear-fix::after
		{
			content:"";
			display:block;
			clear:both;
			visibility:hidden; /* 浏览器兼容性*/
			height:0;  /* 浏览器兼容性*/
		}
		```
## 布局方案对比

|定位方案| 布局方式 |
|:-:|:-:|
|`normal Flow(标准流)`|垂直布局|
|`absolute Positioning(绝对定位)`| 层叠布局 |
|`float(浮动)` |水平布局|
# flex布局
## 认识flex布局
- Flexbox翻译为弹性盒子:
	- 弹性盒子是一种用于<font color=#ff0000>按行</font>或<font color=#ff0000>按列</font>布局元素的==一维布局==方法;
	- 元素<font color=#ff0000>可以膨胀</font>以填充额外的空间,<font color=#ff0000>收缩</font>以适应更小的空间;
	- 通常我们使用Flexbox来进行布局的方案称之为`flex布局`(`flex layout`);
- flex布局是目前web开发中使用最多的布局方案:
	- flex布局(Flexible布局，弹性布局);
	- 目前特别在移动端可以说已经完全普及;
	- 在PC端也几乎已经完全普及和使用，只有非常少数的网站依然在用浮动来布局;
- 为什么需要flex布局呢?
	- 长久以来，CSS布局中唯一可靠且跨浏览器兼容的布局工具只有`floats`和`positioning`
	- 但是这两种方法本身存在很大的局限性并且他们用于布局实在是无奈之举;
### 原先的布局存在的痛点
- 原来的布局存在哪些痛点呢?举例说明:
	- 比如在父内容里面**垂直居中**一个块内容。
	- 比如使容器的所有子项<font color=#ff0000>等分</font>可用宽度/高度，而不管有多少宽度/高度可用。
	- 比如使<font color=#ff0000>多列布局</font>中的所有列采用相同的高度，即使它们包含的内容量不同。
## flex布局的理解
### flex布局的重要概念
- 两个重要的概念:
	- 开启了flex布局的元素叫`flex container`
	- `flex container` 里面的直接子元素叫做`flex item`
	- ![image.png](https://article.biliimg.com/bfs/article/c9dd52493f75fca22ed2d2e64b4fbcc7f7d6706e.png)
- 当`flex container`中的子元素变成了`flex item`时,具备一下特点:
	- `flex item`的布局将<font color=#ff0000>受flex container属性的设置来进行控制和布局</font>;
	- `flex item`不再严格==区分块级元素和行内级元素==,但是`flex container`还是要区分的
	- `flex item`默认情况下是包裹内容的,但是可以**设置宽度和高度**,相当于可以设置宽高可变的行内元素;
- 设置display属性为flex或者inline-flex可以成为flex container
	- `flex`: `flex container` 以 <font color=#ff0000>block-level</font>形式存在
	- `inline-flex`: `flex container` 以 <font color=#ff0000>inline-level</font>形式存在
### flex布局的模型
![image.png](https://article.biliimg.com/bfs/article/b7d43cc014b7dc2e87fc289131469bf5f615e1a2.png)
flex的布局都是依靠`main axis`(主轴)和`cross axi`s(交叉轴)来管理布局的，而且轴有开始位置(start)和结束位置(end)，方便后面调节felx item的<font color=#ff0000>对齐</font>和<font color=#ff0000>排列</font>
### flex相关的属性
- 应用在flex container 上的CSS属性
	- `flex-flow`
	- `flex-direction`
	- `flex-wrap`
	- `flex-flow`
	- `justify-content`
	- `align-items`
	- `align-content`
- 应用在flex items 上的CSS属性
	- `flex-grow`
	- `flex-basis`
	- `flex-shrink`
	- `order`
	- `align-self`
	- `flex`
## flex-container属性
### flex-direction
- flex items默认都是沿着`main axis`(主轴)从main start开始往main end方向排布
	- `flex-direction`决定了`main axis`的方向，有4个取值
	- row（默认值按行水平方向)、row-reverse(将主轴的方向颠倒)、column(按列垂直方向)、column-reverse
	- ![image.png](https://article.biliimg.com/bfs/article/cb464f03c2a99546e347d0f74058c31d3ddd75ee.png)
### flex-wrap
- `flex-wrap`决定了`flex container`是<font color=#ff0000>单行还是多行</font>
	- `nowrap`(默认)︰单行
	- `wrap`:多行(cross start开始在左上角，end在右先角)
	- `wrap-reverse`: 多行（对比 wrap,cross start 与cross end相反)
	- ![image.png](https://article.biliimg.com/bfs/article/5c36b02eae8487474e1ed8a93265069cb8082e90.png)
### flex-flow
`flex-flow`属性是flex-direction和flex-wrap的简写。口顺序任何,并且都可以省略;
### justify-content
**justify-content决定了`flex items`在`main axis `上的对齐方式**
- `flex-start`(默认值)︰与main start对齐
	- ![image.png](https://article.biliimg.com/bfs/article/05222cd49f40fa625eadd1efe5fea83b2418b85d.png)
- `flex-end`: 与main end对齐
	- ![image.png](https://article.biliimg.com/bfs/article/8a0905997bdf36e7d772c29d35efba85cfbf8e2d.png)
- `center`:居中对齐(不对两边靠齐)
	- ![image.png](https://article.biliimg.com/bfs/article/bed73ed11fcb9994ba5b0bbf7c49d94198ec408e.png)
- `space-between`:
	- flex items之间的<font color=#ff0000>距离相等</font>
	- 与main start、main end<font color=#ff0000>两端对齐</font>
	- ![image.png](https://article.biliimg.com/bfs/article/bcb8e69d1909f850b3e72e2592a27b22f431d555.png)
- `space-around`:
	- flex items之间的<font color=#ff0000>距离相等</font>
	- flex items 与 main start、main end之间的距离是flex items 之间<font color=#ff0000>距离的一半</font>
	- ![image.png](https://article.biliimg.com/bfs/article/7f441923b9e2d25dc8c4ecbb0088702118e37e43.png)
- `space-evenly`:
	- flex items之间的<font color=#ff0000>距离相等</font>
	- flex items 与 main start、main end之间的距离<font color=#ff0000>等于</font>flex items之间的距离
	- ![image.png](https://article.biliimg.com/bfs/article/2f5bcf837c8334a1bd42c791ca72ca653faf9518.png)


### align-item
- align-items决定了`flex items`在 `cross axis` 上的对齐方式
	- `normal`:在弹性布局中，效果和stretch一样(<font color=#ff0000>默认值</font>)
	- `stretch`:当`flex items`在`cross axis`方向的size为<font color=#ff0000>auto</font>时，会自动拉伸至填充`flex container`
		```css
		  .item4
		    {
		      height: auto;
		    }
		```
		![image.png](https://article.biliimg.com/bfs/article/f050c7aae4bd9fc54e2606d3a486d48bc6031800.png)
	- `flex-start`: 与cross start对齐
	- `flex-end`: 与cross end 对齐
	- `center`:居中对齐
	- `baseline`:与基准线对齐
	- ![image.png](https://article.biliimg.com/bfs/article/f40ae6ae5ef758f34f9f3588b7cd101802050f50.png)
### align-content
**align-content 决定了==多行==flex items在`cross axis `上的对齐方式，用法与justify-content类似**
- `stretch`(默认值)∶与align-items的 stretch类似
	- 📢 这就是flex每行直接的有很大的间距的原因
	- 使用align-content: stretch;会将整个行沿`cross axis `方向拉伸到与父容器相同的高度。
	- 这意味着它将**增加多行选项之间的间距**，以确保他们填满整个父容器的高度。
	- 但是，它**不会更改单个子元素的高度**，因此单个子元素仍将保持其原始高度。
	- ![image.png](https://article.biliimg.com/bfs/article/d53b9abc18dde3915cd27600e9088e9512b49e38.png)

- `flex-start`: 与cross start对齐
	- ![image.png](https://article.biliimg.com/bfs/article/b38f6e8071d2480f74bf4df1a9c9ea2f130c8ea7.png)
- `flex-end`: 与cross end对齐
	- ![image.png](https://article.biliimg.com/bfs/article/5a0ffa358f52c9334b2fd3a21ead09af293cd8c2.png)

- `center`:居中对齐
	- ![image.png](https://article.biliimg.com/bfs/article/17f399f901fa61bbb5f84d3244ecfe89ccf009ab.png)
- `space-between`:
	- flex items 之间的距离相等
	- 与cross start、cross end两端对齐
	- ![image.png](https://article.biliimg.com/bfs/article/f6846b260308085eba60018bb8c9459823f495c6.png)

- `space-around`:
	- flex items 之间的距离相等
	- flex items与cross start、cross end 之间的距离是 flex items之间距离的一半
	- ![image.png](https://article.biliimg.com/bfs/article/917d7b8dd4dcd14dd4d0153973eeeb71b95d41e2.png)
- `space-evenly`:
	- flex items之间的距离相等
	- flex items 与cross start、cross end 之间的距离等于flex items之间的距离
## flex-item属性
### flex-item属性- order
- `order`决定了`flex items`的排布顺序
	- 可以设置<font color=#ff0000>任意整数</font>（正整数、负整数、O)，**值越小就越排在前面**
	- 默认值是О
- ![image.png](https://article.biliimg.com/bfs/article/91fc787de1c6ac8166d2b4beb03eda945aa53d69.png)
### align-self
- `flex items` 可以通过`align-self`覆盖`flex container` 设置的**align-items**
	- `auto`(默认值)∶遵从flex container的 align-items设置
	- `stretch、flex-start、flex-end、center、baseline`，效果跟align-items 一致
	- ![image.png](https://article.biliimg.com/bfs/article/c77c5bc57cd9497d7a399d8591c89d1cc077b9bf.png)
### flex-grow
- flex-grow决定了当`flex container`在 `main axis`方向上有剩余size时flex items 如何扩展(拉伸/成长)
	- 可以设置任意<font color=#ff0000>非负数字</font>(正小数、正整数、0)，默认值是0
- 如果所有flex items的flex-grow总和sum<font color=#ff0000>超过1</font>，每个flex item 扩展的size为
	- flex container的剩余`size * flex-grow / sum`
	- ![image.png](https://article.biliimg.com/bfs/article/ce6108ff81da13bbab33267d624661a5c554595c.png)

- flex items扩展后的最终size不能超过**max-width`\`max-height**
### flex-shrink
- `flex-shrink`决定了当flex items在 `main axis`方向上超过了`flex container`的 <font color=#ff0000>size</font>时 flex items 如何收缩(缩小)
	- 可以设置任意<font color=#ff0000>非负数字</font>（正小数、正整数、0)，<font color=#ff0000>默认值是1</font>

- 如果所有flex items的flex-shrink总和超过1，每个flex item 收缩的size为
	- flex items超出 flex container的 size`*`收缩比例/所有flex items的收缩比例之和
- **flex items收缩后的最终size 不能小于min-width`\`min-height**

### flex-basis
- `flex-basis` 用来设置`flex items`在`main axis`方向上的`base size`
- `auto(默认值)、具体的宽度数值（ 100px)`
- flex-basis属性用于设置项目在主轴方向上的<font color=#ff0000>基准</font>大小，
- 类似于width属性。但与width不同，flex-basis可以<font color=#ff0000>自动调整以适应其内容</font>，而不会<font color=#ff0000>覆盖</font>自动调整宽度大小的功能。
- 决定flex items最终base size的因素，从优先级高到低
	- max-width`\`max-height`\`min-width`\`min-height
	- flex-basis
	- width`\`height
	- 内容本身的size
### flex缩写属性
- flex是flex-grow || flex-shrink || flex-basis的简写,flex属性可以指定1个，2个或3个值
	- ![image.png](https://article.biliimg.com/bfs/article/8061d209314aa45119df0241f693899ea0756ea4.png)

```css
/* flex-grow flex-shrink flex-basis */
/* none 0 0 auto */
/* auto 1 1 auto */
 flex:auto/none;
```

#css技巧
### 如下布局如何解决对其问题

![image.png](https://article.biliimg.com/bfs/article/a74f56b2d66d0a44607e179e7be1761a868fb66c.png)


- 在平常我们做设计时，不会设置父元素的高度，多行`justify-content: space-between`会造成上面的问题，但我们想要把的不是这种。
	- 解决方法1：通过计算，设置margin
	- 解决方法2：设置没有高度的行内元素块填充例如`<i>`
		```html
		<html>
		<head>
		  <style>
		    .container
		    {
		      width: 500px;
		      background-color:orange;
		      display: flex;
		      flex-wrap: wrap;
		      justify-content: space-between;
		    }
		    .item
		    {
		      width: 110px;
		      height: 110px;
		    }
		    .container>i
		    {
		      width: 110px;
		    }
		  </style>
		</head>
		<body>
		  <div class="container">
		    <div class="item item1">1</div>
		    <div class="item item2">2</div>
		    <div class="item item3">3</div>
		    <div class="item item1">4</div>
		    <div class="item item2">5</div>
		    <div class="item item3">6</div>
		    <div class="item item1">7</div>
		    <i></i><i></i>  <!--这里添加无用的i块 -->
		  </div>
		  <script src="./js/itemRandomColor.js"></script>
		</body>
		</html>
		```
		- 添加`<i>`的个数是<font color=#ff0000>列数-2</font>
		![image.png](https://article.biliimg.com/bfs/article/0dfb71390cb6a7bf0ed6257c6479897145dd2508.png)
