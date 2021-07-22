#### 简单的术语熟悉

`color:black;`为一个**声明**，包括一个属性（color）和一个值（black）组成。

``` body{  color:black; font-family:Helvetica;}``` 

为一个**声明块**，声明块前面有一个**选择器** `body`，选择器和声明块一起组成了**规则集**；

#### 常用词汇

vertical：垂直的；

align：校准；使成一条直线；



## 第一章 层叠、优先级和继承

### 层叠的规则

1. 条件

   1. **样式表的来源：** 作者样式表（优先级高）or用户代理样式表（浏览器默认样式）

   2. **选择器的优先级：** 最多的ID **>** ID一样多，最多的类 **>** 都一样多，最多的标签名。伪类选择器（如`:hover`）和属性选择器（如`[type="input"]`）与类选择器优先级相同。通用选择器（*）和组合器（>、+、~）对优先级没有影响。

      `!important`会覆盖内联样式成为最高的优先级，在内联样式中使用`!important`则无法被覆盖，应让选择器优先级尽可能低。

   3. **源码顺序：**例子：`<a>`标签伪类的排列顺序，依次为 `a:link` `a:visited` `a:hover` `a:active`，即是因为源码顺序影响层叠优先级。

2. 概括图

   ![层叠优先级](scr\层叠优先级.png)

3. 层叠值

   作为层叠的结果，应用到一个元素上特定属性的值。

4. 经验法则

   **1、在选择器中不要使用ID选择器；2、不要使用!important**

   不必固守但不应习惯性地使用。

### 继承

1. 如果元素的某个属性没有层叠值，则可能会继承某个祖先元素的值。继承顺着DOM树往下传递
2. 不是所有的属性都能被继承，继承的通常是我们希望被继承的那些。

### 特殊值

有两个特殊值可以赋给任意属性，用于控制层叠。

1. `inherit`用继承值代替另一个层叠值；
2. `initial`将属性重置为默认值。这里值得注意的是initial重置的属性的初始值，不是元素的初始值。`display:initial`等价于`display:inline`，在任何元素都不会等于`display:block`。

### 简写属性

1. `font` 指定了```font-style,font-weight,font-size,font-height,font-family```

   `background`指定了多个背景属性；

   `border`是```border-width,border-style,border-color```的简写属性，这几个属性也是简写属性；

   `border-width`是上、右、下、左边框宽度的简写属性；

   等等

2. 简写属性可以忽略不填一些值，但其初始值仍会默认覆盖原有样式。

3. 简写顺序

   1.  上右下左
   2. 水平、垂直

## 第二章 相对单位

### 前言

1. 无论我们喜欢与否，都得抛弃以前那种固定宽度的栏目设计，考试考虑**响应式**设计。
2. 像素（px）是最常用的绝对长度单位，是一个具有误导性的名称，CSS像素并不严格等于显示器的像素。

### em和rem

1. em是最常见的相对长度单位。1em等于当前元素的字号。浏览器会根据相对单位的值计算出绝对值，称作**计算值**(computed value)。

2. 使用em定义字号：

   当前元素的字号决定了em，那如果声明`font-size:1.2em`会怎样？一个字号显然不可能等于自己的1.2倍。实际上，这个font-size是根据继承的字号来计算的。对于大多数浏览器，默认的字号是16px；

3. em同时用于字号和其他属性

   如果同时用em指定一个元素的字号和其他属性，浏览器会先计算字号，再用这个计算值去算出其余的属性值。

   em会不断继承并重新计算，用em定义字号并多级嵌套时会出现问题。

   *em用在内边距、外边距、元素大小上很好，但用在字号上会很复杂*

4. 使用rem设置字号

   解析HTML文档为DOM，`<html>`元素是其他元素的根节点，使用**伪类选择器(:root)**或者类型选择器**html**（这里不算一个标签）选中它自己。

   `rem`是root em的缩写，是相对于根元素的单位。

5. 一个使用提示

   用rem设置字号，用px设置边框，用em设置其他大部分属性。

6. 停止像素思维

   将根元素的字号设置为0.625em，重置font-size为10px以方便计算，这本质上仍是像素思维。而10px太小，又需要写很多重复代码来覆盖掉它。

### 视口的相对单位

1. 视口：浏览器窗口里网页可见部分的边框区域。*不包括浏览器的地址栏、工具栏、状态栏*。

2. vh、vw、vmin、vmax

3. 使用`calc()`定义字号

   可以对两个及以上的值进行基本运算。支持 + 、- 、× 、÷，**加号和减号两边必须有空白！**

   eg：`:root{ font-size: calc(0.5em + 2vw);}` 0.5em保证了最小字号2vw使字体随视口缩放。

### 无单位的数值和行高

1. `line-height, z-index, font-weight`允许使用无单位的值。
2. 任何长度单位`px, em, rem`可以使用无单位的0；
3. 无单位的0**不能用于角度值和与时间相关的值**；
4. 我们可以用一个无单位的数值给body设置行高，之后就不用修改了。

### 自定义属性（CSS变量）

​	声明一个变量，为它赋一个值，然后在样式表的其他地方引用这个值。

1. 定义一个自定义属性

   `:root{--main-font: Helvetiva;}`定义了一个名为`--main-font`的变量，使用两个连字符`--`与CSS属性区分，后面随意命名。

2. CSS变量的使用

   在`:root`中声明后即可在整个网页中使用，调用函数`var()`使用变量如

   `p {font-family:var(--main-font, sans-serif)}`，函数里接收的除了第一个参数变量外，还可以接收第二个参数**备用值**，若变量未被定义，则使用备用值，但值得注意的是，若`var()`计算出来的变量是非法值，无论有没有备用值，对应的属性都会被设置为其初始值。

3. CSS变量的作用域

   要创建具有全局作用域的变量，在:root 选择器中声明；

   要创建局部作用域的变量，在使用它的选择器中声明。

## 第三章 盒模型

### 元素宽度的问题

1. 调整盒模型：

   `box-sizing`的默认值为`content-box`，将其修改为`border-box`，全局修改的方式如下：

   `*,::before,::after{box-sizing:border-box;}`

### 元素高度的问题

1. 通常避免给元素指定明确的高度，普通文档流为限定的宽度和无限的高度设计的，因此，容器的高度由内容天然决定，而不是容器自己决定。

2. 控制溢出

   **垂直溢出**：当明确设置一个元素的高度时，内容可能会溢出容器。使用`overflow`属性控制溢出行为。

   `visible（默认）`所有内容可见；

   `hidden`溢出内容在内边距边缘被裁剪；

   `scroll`容器出现滚动条；

   `auto`只有内容溢出时容器才会出现滚动条。

   *tip.谨慎使用滚动条*

   **水平溢出**：典型场景：一个很窄的容器放一条很长的URL，溢出规则与垂直方向一致。

   可以单独用`overflow-x`属性单独控制水平方向的溢出。

3. 等高列

   1. 等高列的高度原则：元素自己决定，拓展较矮的高度等于较高的列。
   2. **CSS表格布局**：（大概了解）给容器设置`display:table`，给每一列设置`display:table-cell`。默认`table`不会扩展到100%，要指明容器宽度。另外，外边距不会做用于`table-cell`元素，使用`border-spacing`和负外边距调整列之间的间隔。
   3. **Flexbox**：给容器设置`display:flex`，变成弹性容器，子元素默认等高。

   * *除非别无选择，不要明确设置元素高度，会导致更复杂的情况。*

4. `min-height`和`max-height`

   指定最小和最大高度，类似的是`min-width` 和`max-width`。

5. **垂直居中**

   1. `vertical-align`声明只会影响**行内元素或者`table-cell`元素**，控制着该元素跟同一行内的其他元素的对齐关系。

      对于显示为`table-cell`的元素，`vertical-align`控制了内容在单元格内的对齐，在CSS表格布局中，可以使用`vertical-align`实现垂直居中。

   2. 给容器**上下相等的内边距**，同时**让容器和内容自己决定高度**。

   3. 总结：

      1. 容器为自然高度：设置相等的上下内边距。
      2. 需指定高度或避免使用内边距：`display:table-cell`和`vertical-align:middle`
      3. Flexbox
      4. 只有一行文字：设一个大的行高等于容器高度。若内容不是行内元素，设置为`inline-block`
      5. 内容和高度都知道：绝对定位；
      6. 不知道内部元素的高度：绝对定位结合变形(transform)

### 负外边距

1. ![](scr\负外边距.png)

### 外边距折叠

1. 当顶部和底部的外边距相邻时，就会重叠。

   外边距折叠的根本原因：**margin之间直接接触没有阻隔**，解决的方案也是**阻止外边距直接接触**。

2. 防止外边距折叠：

   1. 对容器使用`overflow:auto`(非visible)，防止内部元素的外边距与容器外部的外边距折叠
   2. 在两个外边距之间加上边框或者内边距。（阻止接触）
   3. 容器为浮动元素`float`、内联块`inline-block`、绝对定位和固定定位时，外边距不会折叠。
   4. Flexbox布局中弹性布局内的元素不会发生外边距折叠。
   5. `table-cell`不具备外边距属性，不会折叠。

3. 这些方法很多会改变元素的布局行为，不要轻易使用。

### 容器内的元素间距

1. 使用猫头鹰选择器( \*+\* )以全局设置堆叠元素的外边距。

## 第四章 理解浮动

（暂时不去回顾，需要用的时候拿起来，把Flexbox和别的要用的回顾下）

## 第五章 Flexbox

### Flexbox的原则

1. 给元素添加display:flex ，元素变成**弹性容器(flex container)**，它的直接子元素变成**弹性子元素(flex item)**。

   弹性容器像块元素一样填满可用宽度。

   弹性子元素默认在同一行按照从左到右的顺序排列。高度相等，由它们的内容决定。

2. 子元素按照**主轴**线排列，主轴的方向为主起点到主终点。垂直于主轴的的是**副轴**；

### 弹性子元素的大小

1. flex简写属性：

   flex属性是flex-grow flex-shrink flex-basis 的简写，默认为 1 1 0%。

2. flex-basis:

   定义了元素大小的基本值。可以设置为任意的width值：px、em、百分比。

   初始值为auto，此时若已经设置了width值，则用width值作为flex-basis的值，否则用元素内容自身的大小；如果不是auto，则width属性会被忽略。

3. flex-grow：

   1. 将每个弹性子元素的 flex-basis 值加起来，它们的值若没有填满弹性容器的宽度，多余的留白会按照flex-grow（增长因子）的值分配给每个弹性子元素
   2. flex-grow的值为**非负整数**，就是子元素在增长时的“权重”大小

4. flex-shrink：

   与flex-grow相似，当弹性子元素的 flex-basis 值加起来大于弹性容器可用宽度时，若不用flex-shrink，就会导致溢出。flex-shrink就以 flex-shrink 的值作为溢出内容收缩速度的“权重”

5. 利用flex属性实现多种布局

### 弹性方向

1. flex-direction 即<u>弹性容器的主轴方向</u>属性的初始值 row 控制子元素从左到右的方向排列；

   指定 flex-direction:column 控制子元素从上到下排列。

   还有 flex-direction:row-reverse 和 flex-direction:column-reverse让子元素反方向排列

2. 创建一个**嵌套的弹性盒子**

   ```css
   .column-sidebar{
       flex:1;
       display:flex;
       flex-direction:column;
   }
   .column-sidebar > .tile{
       flex:1;
   }
   ```

   以上代码创建了一个**嵌套的弹性盒子**，内部盒子的弹性方向改为了column，主轴发生了旋转

3. CSS中处理高度的方式与处理宽度的方式本质上不一样，弹性容器会占用100%的可用宽度，而高度则由自身的内容决定，无论主轴方向如何。

### 对齐、间距等细节

**弹性容器**的属性

1. flex-direction

   1. 指定了主轴方向，副轴垂直于主轴
   2. 值：**row**、row-reverse、column、column-reverse 	

2. flex-wrap

   1. 指定了弹性子元素是否会在弹性容器内折行（列）显示
   2. 值：**nowrap**、wrap、wrap-reverse
   3. 启用换行后，子元素将不再根据flex-shrink值收缩，超过弹性容器的子元素会换行显示；如果方向是column或column-reverse，flex-wrap会让子元素换到新的一列显示，但仅限限制了高度的弹性容器

3. flex-flow

   为flex-direction、flex-wrap的简写属性

4. justify-content

   1. 控制子元素在主轴上的位置（对齐方式）

   2. 值：**flex-start**、flex-end、center、space-between、space-around

   3. 对于space-between和space-around的间距计算：

      间距是在元素的外边距确定之后进行计算的，还要考虑flex-grow的值。

      只要任一子元素的flex-grow不为0，或者任一子元素在主轴方向上的外边距为auto，justify-content就失效了

5. align-items

   1. 控制子元素在副轴上的位置（对齐方式）

   2. 值：flex-start、flex-end、center、**stretch**、baseline

   3. 水平排列下，初始值stretch让所有子元素填充容器的高度，垂直排列下填充容器的宽度。故可以实现等高列。

      <u>助记提示</u>：我们通过“调整”(justify)文字，让其在水平方向的两端均匀分布；而align-items更像vertical-align，让行内元素在垂直方向“对齐”(align)。

6. align-content

   1. 如果开启了flex-wrap，align-content就会控制弹性子元素在副轴上的间距，如果子元素没有换行，则会忽略align-content
   2. 值：flex-start、flex-end、center、stretch、space-between、space-around

**弹性子元素**的属性

1. flex-grow

   整数，增长因子

2. flex-shrink

   整数，收缩因子

3. flex-basis

   元素未受前两者影响时的初始大小

4. flex

   前三者的简写属性

5. align-self

   1. 控制子元素在副轴上的对齐方式，会覆盖容器上的align-items值。如果元素副轴方向上的外边距为auto，则会忽略该属性。
   2. 值：**auto**、flex-start、flex-end、center、stretch、baseline
   3. 与弹性容器的align-items效果相同，但能给弹性子元素单独设定不同的对齐方式

6. order

   值为整数，将弹性子元素从兄弟节点中移动到指定位置，覆盖源码顺序

### 值得注意的地方

1. 通过Flexbugs了解Flexbox的浏览器bug

2. 整页布局

   在一行多列的布局时，可能会发生：在三列布局中，用户先看到两列，然后列的大小改变，出现第三列。

   整页布局采用网格布局以避免这种情况。

## 第六章 网格布局

### 网页布局的新纪元

1. 与Flexbox类似，网格布局也是作用于两级DOM结构。设置为display:grid的元素成为一个**网格容器（grid container）**，它的子元素变成**网格元素（grid items）**。
2. 新的属性和单位
   1. grid-template-columns，grid-template-rows分别定义网格每行每列的大小
   2. grid-gap：每个网格单元之间的间距；如果用两个值，分别指定垂直和水平方向的间距
   3. 新的单位 fr ，是一种分数单位（fraction unnit）；也可以用其他单位混搭px、em、百分比

### 网格剖析

1. 基本概念

   1. **网格线**（grid line），grid-gap位于网格线上
   2. **网格轨道**（grid track），两个相邻网格线之间的空间
   3. **网格单元**（grid cell），网格上的单个空间
   4. **网格区域**（grid area），网格上的<u>矩形区域</u>

   示例：`grid-template-columns:1fr 1fr 1fr`定义三个等宽且垂直的**网格轨道**，同时还定义了四条垂直的**网格线**。
   
2. repeat()函数

   repeat(arg1, arg2)以 参数二 的模式重复 参数一 次

   `grid-template-columns: 1fr repeat(3, 200px 3fr 1fr) 2fr`

   创建一个 1: 200px: 3: 1: 200px: 3: 1: 200px: 3: 1: 2 的11个网格列

   参数二也可以为 auto ，轨道会根据自身内容拓展

3. 网格线的编号

   编号从左上角为1开始递增；负数则从右下角开始从 -1 递减

   **注意：**隐式网格轨道不会改变负数的含义，负的网格线编号依然是从<u>显式网格线</u>的右下角开始的。

   属性：grid-column和grid-row 中用编号指定网格元素的位置

   eg：`grid-column:1/3; grid-row:2/5;`

   这两个属性的值也可以用span指定（例：`grid-row:2/span 3`或`grid-row:span 2`），第一个是从2号网格线开始，延伸三个网格行；第二个延伸两个网格行，但没有指出具体是哪一行，浏览器会根据**布局算法**把元素放到第一处可用空间。

4. 与Flexbox配合

   1. 二者是互补的，尽管功能有重叠的地方；
   2. Flexbox本质是一维的，适合用在相似的元素组成的行（列）上，网格是二维的，旨在解决一个轨道的元素跟另一个轨道的元素对齐的问题；
   3. Flexbox是以内容为切入点由内向外工作的，网格是以布局为切入点从外向内工作的。
   4. 实践中，网格通常更适合用于整体的布局，而Flexbox用于对网格区域内特定元素布局。

### 代替语法

1. 网格线命名

   eg：`grid-template-columns:[start] 2fr [center] 1fr [end];`这个时候网格线的名称就可以代替编号了。

   eg：`grid-row:row 3/span 2;`从第三条名称为 row 的网格线开始，延伸两个网格行的范围。

   eg：`grid-template-columns:repeat(3,[col] 1fr 1fr)`结合 repeat 重复的例子

2. 网格区域命名

   1. ASCII art 语法的网格区域命名

      ```css
      grid-template-area:"title title""nav nav""main aside1""main aside2" 
      header{
          grid-area:title;
      }
      nav{
          grid-area:nav;	/*将每个网格元素放到一个命名的网格区域*/
      }
      .main{
          grid-area:main;
      }
      .sidebar-top{
          grid-area:aside1;
      }
      .sidebar-bodttom{
          grid-area:aside2;
      }
      ```

      **警告：**每个命名的网格区域必须组成一个<u>矩形</u>

      **Tip：**可以用句点 ( **.** )作为名称，这样创建一个空的网格单元。

### 显式和隐式网格

1. 使用 grid-template-× 属性定义网格轨道时，创建的是**显式网格**，但是网格元素可以放在显式轨道之外，这时会自动创建隐式轨道以拓展网格，直到包含这些元素。

2. 隐式网格轨道默认大小为auto，会自动扩展。

   可以指定 grid-auto-columns 和 grid-auto-rows ，为隐式网格轨道指定一个大小（例：`grid-auto-columns:1fr`）

3. 例子：`repeat(auto-fill,minmax(200px,1fr));`

   repeat()里的auto-fill，只要网格放得下，浏览器就会尽可能多的生产轨道，且不与minmax()冲突

   若用 auto-fit 替代 auto-fill ，可以让非空的网格轨道扩展，填满可用空间。

4. grid-auto-flow 属性

   控制布局算法的行为，初始值为 row ，优先放到网格行里，只有行满了，才会到下一列；值为 column 时同理。

   加上关键字 dense （例：`grid-auto-flow：column dense`，就会紧凑地填满网格的空白。

### 特性查询

1. @support (display:grid) { …… }

   如果浏览器能够理解括号里的声明，就会使用大括号里的样式规则，否则不使用。

   可以用于页面的退化。

   可以结合运算符 and or not 使用。

### 对齐

1. 三个调整属性

   justify-content、justify-items、justify-self

2. 三个对齐属性

   align-content、align-items、align-self

   **注：**网格对齐目前暂不做详细了解应用。只在需要时查询使用。

## 定位和层叠上下文

### 概要

1. position属性的初始值static；如果把它改成其他值，元素就被**定位**了，如果是静态定位，我们就说它**未被定位**

### 固定定位

1. position：fixed

   搭配四种属性：top、right、bottom、left

### 绝对定位

1. position：absolute

2. 如果祖先元素都没有定位，那么绝对定位的元素会基于**初始包含块**（initial containing block）来定位。

   初始包含块跟视口一样大，固定在网页的顶部。

3. 定位伪元素

   伪元素表现得像子元素一样，所以定位的按钮就成为其伪元素的包含块。

### 相对定位

1. position：relative

2. 对于相对定位relative的元素，top、right、bottom、left只能让元素在各方向移动，top和bottom不能一起用，left和right同理。

3. 通过position：relative改变元素位置并不常用，常见用法：

   给它里面需要绝对定位的元素创建一个包含块。

4. 创建一个CSS三角形

   ```css
   div::after{
       content: "";
       border:solid 1em;
       border-color: transparent transparent black transparent;
   }
   ```

   这里伪元素没有内容，也就没有宽高，不管border的宽度设置多少，都会形成一个三角形。

### 层叠上下文

1. 浏览器将HTML解析为DOM的时候还创建了另一个树形结构，**渲染树**。它代表每个元素的样式和位置，还有浏览器**绘制**元素的顺序。后绘制的元素会出现在先绘制的元素前面。

2. 

   1. 元素在HTML里的顺序决定了绘制的顺序。
   2. 定位元素时，所有的定位元素会出现在非定位元素前面。

3. 用 z-index 控制层叠顺序

   z-index的值为任意整数，值越高越在前面

   注意：

   1. z-index只在定位元素上生效；

   		2. 给一个定位元素加上 z-index 可以创建层叠上下文。

4. 理解层叠上下文

   1. 给一个定位元素加上 z-index 的时候，它就会成为一个新的层叠上下文的根。所有的后代元素就是这个层叠上下文的一部分。
   2. 层叠上下文之外的元素无法叠放在层叠上下文的两个元素之间。

5. 其他创建层叠上下文的方式

   小于1的opacity属性；transform、filter属性；文档根节点也会创建一个顶级的层叠上下文。

6. 层叠上下文内的顺序

   从后往前

   1. 层叠上下文的跟
   2. z-index为负的定位元素（以及其子元素）
   3. 非定位元素
   4. z-index为auto的元素（以及其子元素）
   5. z-index为正的元素（以及其子元素）

7. 用变量设置 z-index 让层叠更清晰

### 粘性定位

1. position：sticky

   粘性元素永远不会超过父元素的范围。

2. 使用粘性定位要注意浏览器的兼容性。

## 响应式设计

### 原则一：移动优先

1. 在构建桌面版前要先构建移动端布局。之后再“渐进增强”。

2. **重点**：做响应式设计时，要确保html包含所有内容。不同屏幕尺寸可以用不同的CSS，但必须是同一份HTML。

3. 断点

   一个特殊的临界值。屏幕尺寸达到这个值时，网页的样式会发生改变。

4. 屏幕阅读器将某些HTML5元素作为里程碑，要将元素放到适合的标签里。

5. 为移动端的响应式设计添加视口 meta 标签

   告诉移动设备，网页已经适配了小屏设备。否则，移动浏览器会假定网页不是响应式的。

   ```html
   <meta name="viewport" content="width=device-width,initial-scale=1">
   ```

### 原则二：媒体查询

1. @media 媒体查询

   ```css
   @media (min-width:560px){
   	.title>h1{
   	font-size:2.25em;}
   }
   ```

   @media规则会对条件进行检查，只有满足所有条件时，才会将这些样式应用到页面上。

2. 在媒体查询中断点推荐使用 **em单位**。

3. 媒体查询的类型

   查询方式：

   1. 同时满足多个条件，用 and: `@media (min-width:20em) and (max-width:35em){...}`
   2. 满足多个条件之一，用逗号 , : `@media (max-width:20em) , (min-width:35em) {...}`

   查询类型：

   1. min-width、max-width等

      统称为**媒体特征**

   2. 媒体类型

      常见的两种**媒体类型**为 screen 和 print（打印样式） 

### 原则三：流式布局

使用的容器随视口宽度而变化，主页面容器通常不会有明确宽度。

1. 在主容器中，任何列都可以用百分比来定义宽度。Flexbox布局也可以。

2. 整本书中几乎只用了流式布局。

3. 处理表格：

   一个方法：将表格强制显示为一个普通的块级元素。

### 响应式图片

1. 不同视口大小使用不同的图片。

2. 使用srcset提供对应的图片

3. **提示**：图片作为流式布局的一部分，请始终确保它不会超过容器的宽度。

   一劳永逸的方法：`img{ max-width: 100%;}` ，这个确实要多注意。