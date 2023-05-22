# CSS3 新特性

* **CSS3 选择器**
* **CSS3 边框与圆角**
* **CSS3 背景与渐变**
* **CSS3 过渡**
* **CSS3 变换**
* **CSS3 动画**

## CSS3 选择器

### 基本选择器

* 子选择器

* 相邻兄弟选择器
* 通用兄弟选择器
* 群组选择器

```css
/* 子选择器 */
h1 > strong {}
/* 相邻兄弟选择器 */
h2 + p {}
/* 通用兄弟选择器 */
h2 ~ p {} /* h2 后面所有 p 元素全变 */
/* 群组选择器 */
.div1, .div2, .div3 {}
```

### 属性选择器

* **element[attribute]**
  * 为带有 attribute 属性的元素设置样式
* **element[attribute='value']**
  * 为 attribute='value' 属性的元素设置样式
* **element[attribute~='value']**
  * 选择 attribute 属性值包含**单词** value 的元素 并设置样式
* **element[attribute\*='value']**
  * 选择 attribute 属性值包含值 value 的元素设置样式
* **element[attribute^='value']**
  * 选择attribute属性值是以 value **开头**的元素
* **element[attribute$='value']**
  * 选择 attribute 属性值是以 value **结尾**的元素

```html
<div class="abcd icon0">hello</div>
<div class="icon1" disabled>hello</div>
<div class="icon2">hello</div>
<div class="icon3">hello</div>
<div class="icon4">hello</div>
<div class="icon5 icon">hello</div>

<style>
  div[disabled] {/* 选中 icon1 */}
  div[class='icon1'] {/* 选中 icon1 */}
  div[class*='icon'] {/* 选中所有包含 icon 的元素：icon1,icon2,icon3,icon4,icon5*/}
  div[class~='icon'] {/* 选中包含 icon 单词的元素：icon5*/}
  div[class^='ab'] {/* 选中 icon0 */}
  div[class$='4'] {/* 选中 icon4 */}
</style>
```

### **伪类选择器**

* 锚点伪类
  * `:link`
  * `:visited`
* 用户行为伪类
  * `:hover`
  * `:active`
  * `:focus`
* 目标伪类
  * `:target`
* checked 状态伪类
  * `:checked`
* 结构性伪类
  * `:first-child/:last-child`
  * `:nth-child(n)`
  * `:nth-last-child(n)`
  * `:nth-of-type(n)`
  * `:nth-last-of-type`
  * `:first-of-type/:last-of-type`
  * `:only-child`
  * `:only-of-type`
  * `:empty`
* 否定伪类
  * `:not`

### 伪元素

* **element::first-line**
  * 对元素的第一行文本进行设置，只能用于**块级元素**
* **element::first-letter**
  * 用于向文本的首字母设置特殊样式，只能用于**块级元素**
* **element::before**
  * 在元素的内容前面插入新内容，常与content配合使用
* **element::after**
  * 在元素的内容后面插入新内容，常与content配合使用
* **element::selection**
  * 用于设置浏览器中选中文本后的背景色与前景色

## CSS3 边框与圆角

### CSS3 圆角 border-radius

定义：可以为元素添加圆角边框（块元素，行内块元素，行内元素）

* border-top-left-radius 左上角
* border-top-right-radius 右上角
* border-bottom-right-radius 右下角
* border-bottom-left-radius 左下角
* border-radius 复合属性

复合属性的属性值：

* 四个值：左上角 右上角 右下角 左下角
* 三个值：左上角 右上角和左下角 右下角
* 一个值：4个角都生效

圆形与椭圆：

* 一旦使用百分比，参照的是元素本身的高度与宽度

* 当拿 50% 时，宽等于高为圆形 宽不等于高为椭圆形

### 盒阴影 box-shadow

定义：可以控制一个或多个下拉阴影的框

语法：`box-shadow: 水平方向的偏移量 垂直方向的偏移量 模糊程度 扩展程度 颜色 是否具有内阴影;`

偏移量：

```
把元素左上角（0，0）作为基准点，找水

平方向和垂直方向的偏移量

水平： 正值 --- 右 ，负值 --- 左

垂直： 正值 --- 下 ，负值 --- 上
```

模糊程度：

```text
边界模糊，但是边界线未动
由边界线向外模糊多少像素
```

扩展程度：

```text
盒子阴影，上下左右都向外扩展多少像素
```

是否具有内阴影：

```text
inset(默认没有，也就是默认是外阴影)
加上inset,盒子的阴影为内阴影
扩展程度可为负值，但是模糊程度不可以  
```

## CSS3 背景与渐变

### CSS3 背景

* **background-image**
* **background-cilp**
* **background-origin**
* **background-repeat**
* **background-size**
* **复合属性: background**

### CSS3 渐变

定义：可以在两个或者多个指定颜色之间显示平移的过渡

* **线性渐变**
  * 定义：是沿着一根轴线改变颜色，从起点到终点进行顺序渐变（从一边拉向另一边）
  * 语法：`background: linear-gradient(方向，开始颜色，结束颜色)`

* **径向渐变**

#### 线性渐变

定义：是沿着一根轴线改变颜色，从起点到终点进行顺序渐变（从一边拉向另一边）

语法：`background: linear-gradient(方向，开始颜色，结束颜色)`

方向介绍：

* 方向从上到下（默认）:

```text
background: linear-gradient(red,blue);
```

* 方向从左到右

```text
background: linear-gradient(to right,red,blue);
```

* 对角

```text
background: linear-gradient(to right bottom,red,blue);
```

* 角度(单位deg)

```text
background: linear-gradient(角度,red,blue);
```

角度说明：0deg 将创建一个从下到上的渐变，90deg将创建一个从左到右的渐变

颜色结点：默认每个颜色均匀分布

```css
/* 从0%到10%，为红色，从10%到20%为红色到蓝色的渐变，从20%到30%为蓝色到绿色的渐变，从30%到40%，为绿色到黄色的渐变,从40%到100%为黄色 */
background: linear-gradient(red 10%,blue 20%,green 30%,yellow 40%);

/*从0%到10%，为红色，从10%到100%为红色到蓝色的渐变*/
background: linear-gradient(red 10%,blue);

/*从0%到30%，为红色到蓝色的渐变*/
background: linear-gradient(red,blue 30%);

/*由透明色变为不透明色*/
background:lineargradient(rgba(255,0,0,0),rgba(255,0,0,1));
```

重复渐变（把元素的整体宽度看成100%）

```css
background: repeating-linear-gradient(90deg,red 0%,blue 20%);
/*或者*/ 
background: repeating-linear-gradient(90deg,red 0%,blue 10%,red 20%);
```

#### 径向渐变

定义：从起点到终点，颜色从内向外进行圆形渐变

语法：`background: radial-gradient(形状尺寸，开始颜色，结束颜色)`

- 形状分类：

circle --- 圆形

ellipse --- 椭圆形

- 尺寸大小：

closest-side最近边

```css
background: radial-gradient(closest-side circle,red , blue);
```

farthest-side 最远边

```css
background: radial-gradient(farthest-side circle,red , blue);
```

closest-corner最近角

```css
background: radial-gradient(closest-corner circle,red , blue);
```

farthest-corner最远角

```css
background: radial-gradient(farthest-corner circle,red , blue);
```

- 颜色结点：

例：

```css
background:radial-gradient(circle,red 50% ,blue 70%);
```

注意：此时的百分比,指的是圆心到元素最远端的距离（角度）

- 重复渐变 ：

示例：

 ```css
 background: repeating-radial-gradient(red 0%,blue 20%);
 
 background: repeating-radial-gradient(red 0%,blue 10%,red 20%);
 ```

## CSS3 过渡

定义：允许css的属性值在一定时间区间内平滑的过渡，在鼠标点击，鼠标滑过或对元素任何改变中触发，并圆滑地以动画形式改变css的属性值。

* **transition-property属性**
  * 指定CSS属性的name，transition效果
* **transition-duration属性**
  * transition效果需要指定多少秒或毫秒才能完成
* **transition-timing-function属性**
  * 指定transition效果的转速曲线
* **transition-delay 属性**
  * 定义transition效果开始的时候
* **transition 复合属性**

transition-timing-function 可选值：

| 值                            | 描述                                                         |
| :---------------------------- | :----------------------------------------------------------- |
| linear                        | 规定以相同速度开始至结束的过渡效果（等于 cubic-bezier(0,0,1,1)）。 |
| ease                          | 规定慢速开始，然后变快，然后慢速结束的过渡效果（cubic-bezier(0.25,0.1,0.25,1)）。 |
| ease-in                       | 规定以慢速开始的过渡效果（等于 cubic-bezier(0.42,0,1,1)）。  |
| ease-out                      | 规定以慢速结束的过渡效果（等于 cubic-bezier(0,0,0.58,1)）。  |
| ease-in-out                   | 规定以慢速开始和结束的过渡效果（等于 cubic-bezier(0.42,0,0.58,1)）。 |
| cubic-bezier(*n*,*n*,*n*,*n*) | 在 cubic-bezier 函数中定义自己的值。可能的值是 0 至 1 之间的数值。 |

语法

```css
transition: property duration timing-function delay;
```

示例

```css
div {
  width:100px;
	height:100px;
	background:red;
  transition: width 2s ease-in-out 2s;
  -webkit-transition: width 2s ease-in-out 2s; /* Safari */
}
div:hover {
	width: 300px;
}
```

## CSS3 变换

定义：让一个元素在一个坐标系统中变形，这个属性包含一系列的变形函数，可以移动，旋转，缩放元素。

语法：`transform：none | <transform-function> 默认值是none`

* **2d变换**
  * **rotate()旋转**
  * **translate()平移**
  * **scale( )缩放**
  * **skew()扭曲/倾斜**
  * **变换基点**
* **3d变换**
  * 开启3d空间transform-style: preserve-3d; 一般给父元素开启
  * 子元素设置3d变换效果
  * rotate
    * rotateX()
    * rotateY()
    * rotateZ()
  * translate
    * translateZ()
  * scale
    * scaleZ()
  * 设置景深：实现近大远小
    * 父元素子元素都可以设置
    * 父元素：perspective: 300px;
    * 子元素：`transform:perspective(300px) translateZ(-200px);`
  * 变换基点
    * 默认值： 50% 50% 0
    * transform-origin: top; 关键字表示 ( 50% 0 0 )
    * 注意：立体3d盒子 Z：只能使用具体的长度，不能使用百分比和关键字
  * 景深中心点：改变观察者视角
    * perspective-origin: top;
    * perspective-origin: top right;
  * 元素背面可见还是不可见
    * backface-visibility:visible ;（默认值：可见）
    * backface-visibility: hidden; 不可见

## CSS3 动画

定义：使元素从一种样式逐渐变化到另外一种样式的效果

* **@keyframes**
  * animationname：必写项，定义动画的名称
  * keyframes-selector：必写项，动画持续时间的百分比 0% - 100%之间， 或者使用form和to关键字也可以设置，form代表0%，to代表100%

语法：

```css
@keyframes animationname {
  keyframes-selector{
		cssStyles;
	}
}
```

* **animation属性**

  * animation-name 属性

    * 语法：`animation-name：keyframename | none`
    * keyframename：指定要绑定到选择器的关键帧的动画名称

  * animation-duration属性

    * 定义：设置对象动画的持续时间
    * 语法：`animation-duration：time`
    * time 指定对象播放完成需要花费的时间，默认值是0

  * animation-timing-function属性

    * 定义：设置对象动画的过渡类型，即规定动画的速度曲线
    * 语法：`animation-timing-function: value;`
    * 参数说明：与 transition-timing-function 属性的参数一样

  * animation-delay属性

    * 定义：设置动画的延迟时间
    * 语法：`animation-delay：time;`
    * 参数说明：可选值，定义动画开始前等待的时间，以秒或者毫秒数计数，默认值是0

  * animation-iteration-count 属性

    * 定义：设置对象动画的循环次数

    * 语法：`animation-iteration-count: infinite | number;`

    * 参数说明：

      number: 数字，其默认值是1

      infinite: 无限循环次数

  * animation-direction属性

    * 定义：设置对象动画是否反向运动

    * 语法：`animation-direction：normal , reverse , alternate , alternate-reverse`

    * 参数说明：

      normal : 正常方向

      reverse :反向运动

      alternate ： 先正常运动在反向运动，并持续交替运行， 需要配合循环属性使用

      alternate-reverse ： 先反向运动在正常运动，并持续交替运行， 需要配合循环属性使用

  * animation-play-state属性

    * 定义：指定对象是否正在运行或已暂停

    * 语法：`animation-play-state：paused | running`

    * 参数说明：

      paused ： 指定暂停动画

      running : 默认值，制定正在运行的动画

  * animation-fill-mode

    * 定义：设置动画结束后的状态

    * none：默认值。不设置对象动画之外的状态，DOM未进行动画前状态

      forwards：设置对象状态为动画结束时的状态，100%或to时，当设置animation-direcdtion为reverse时动画结束后显示为keyframes第一帧

      backwards：设置对象状态为动画开始时的状态,（测试显示DOM未进行动画前状态）

      both：设置对象状态为动画结束或开始的状态，结束时状态优先

  * animation复合属性（不推荐使用 ）

    * 语法：`animation ： name duration timing-function delay interation-count direction play-state`

animation-timing-function 使用名为三次贝塞尔（Cubic Bezier）函数的数学函数，来生成速度曲线。您能够在该函数中使用自己的值，也可以预定义的值：

| 值                    | 描述                                                         |
| --------------------- | ------------------------------------------------------------ |
| linear                | 动画从头到尾的速度是相同的。                                 |
| ease                  | 默认。动画以低速开始，然后加快，在结束前变慢。               |
| ease-in               | 动画以低速开始。                                             |
| ease-out              | 动画以低速结束。                                             |
| ease-in-out           | 动画以低速开始和结束。                                       |
| cubic-bezier(n,n,n,n) | 在 cubic-bezier 函数中自己的值。可能的值是从 0 到 1 的数值。 |
