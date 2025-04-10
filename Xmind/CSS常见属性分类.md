
---

mindmap-plugin: basic

---
# CSS常见属性分类
coderwhy

## 《字体》相关属性

### font-family：指定字体

### font-size：字体大小，要注意有绝对大小，相对大小，长度，百分比

### font-style：正常体、斜体、倾斜体

### font-weight：设置粗体

### font-variant：用来将所有字体都变成大写，但是原来是大写的字体呢又要比默认的要大一些

### font：[ [ <'font-style'> || <font-variant-css21> || <'font-weight'> || <'font-stretch'> ]? <'font-size'> [ / <'line-height'> ]? <'font-family'> ]

### line-height：normal/数字/长度/百分比

### text-align: 对齐方式，取值为：left, right, center和justify(两侧对齐)

### text-decoration：设置颜色、位置、样式。分别对应了text-decoration-color，text-decoration-line，text-decoration-style.
常用decoration-line的值：none | [ underline || overline || line-through || blink ] | spelling-error | grammar-error

### text-indent：缩进，段落第一行文本要空多少距离，单位为长度

### text-shadow：阴影设置，none | <shadow-t>#

### text-transform：大小写转换

### text-indent：缩进，段落第一行文本要空多少距离，单位为长度

### text-overflow：文本溢出的截断

### white-space：设置如何处理元素中的 空白，长设置属性normal/nowrap

### vertical-align: 垂直对齐方式(较复杂，听视频讲解)

### word-spacing：word和word之间的间距

### letter-spacing：letter和letter之间的间距

### word-break：文字换行，normal、break-all/keep-all/break-word

### color：字体颜色

### opacity：不透明度（0为透明，1为不透明）

## 《盒子》相关属性

### width：设置盒子宽度

### height：设置盒子高度

### max-width/max-height：最大宽度、高度

### min-width/min-height：最小宽度、高度

### margin、margin-left、margin-right、margin-top、margin-top：外边距

### padding、padding-left、padding-right、padding-top、padding-bottom：内边距

### border、border-width、border-style、border-color：边框

### border-radius：边框圆角

### outline：边框（不占据空间）

### box-shadow：inset? && <length>{2,4} && <color>? 边框阴影

## 《背景》相关属性

### background

- [ <bg-layer> , ]* <final-bg-layer>
- <bg-layer> = <bg-image> || <bg-position> [ / <bg-size> ]? || <repeat-style> || <attachment> || <box> || <box>
- <final-bg-layer> = <'background-color'> || <bg-image> || <bg-position> [ / <bg-size> ]? || <repeat-style> || <attachment> || <box> || <box>

### background-color：背景颜色

### background-image：背景图片

### background-position：背景位置

### background-size：背景大小

### background-repeat：重复次数

### background-attachment：背景图片的位置是否固定

### background-origin：背景图片属性的原点位置的相对区域border-box | padding-box | content-box

### background-clip：背景是否延伸

## 《定位》相关属性

### position：定位（static | relative | absolute | sticky | fixed）

### left、right、top、bottom：设置定位位置

### z-index：设定了一个定位元素及其后代元素或 flex 项目的 z-order

### float：浮动（left | right）

### clear：清除浮动

### display：设置元素的内部、外部显示类型

### visibility：元素显示（visible | hidden | collapse）

### overflow：超出部分的显示（visible | hidden | scroll | auto）

## CSS值规则阅读：
https://developer.mozilla.org/zh-CN/docs/Web/CSS/Value_definition_syntax#hash_mark

*XMind: ZEN - Trial Version*