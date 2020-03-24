1. 关于盒模型

    标准盒模型：(content) + padding + border + margin

    低版本IE盒模型：(content + padding + border) + margin

    盒模型就是dom元素采用的布局模型，可以采用`box-sizing`进行设置，根据计算宽高的区域可分为：

    1. border-box（这也是早期IE传统盒模型）
    2. padding-box（早期的ff浏览器才支持，现在浏览器都不支持）
    3. content-box（默认值）

2. BFC（Block Format Context）

    即块级格式化上下文，是web页面中的可视化css渲染出来的一部分，是块级盒布局出现的区域，也是浮动层元素进行交互的区域；相似的还有IFC（Inline Format Context）；

    它决定了元素如何对其内容进行定位以及和其它元素之间的关系和相互作用；涉及到可视化布局的时候，BFC就提供了一个环境，html元素在这个环境中按照一定规则进行布局；一个环境中的元素不会影响到其它环境中的布局；比如浮动元素形成一个BFC，其内部子元素的位置受该元素的影响，两个浮动元素之间是互不影响的；可以说BFC就是一个作用范围；

    > **触发BFC的条件**
    >
    > * 根元素或者其它包含它的元素
    > * 浮动元素（即float不为none）
    > * 绝对定位元素（position为absolute或fixed）
    > * 内联块（display: inline-block）
    > * 表格单元格（display: table-cell）
    > * 表格标题（display: table-caption)
    > * overflow不是visible的块元素
    > * 弹性盒子（flex或inline-flex）
    > * column-span: all
    >
    > **BFC的约束规则**
    >
    > * 内部的盒会在垂直方向一个接一个排列
    > * 处于同一个BFC的元素相互影响，有可能会发生外边距重叠
    > * 每个元素的margin-box的左边，紧挨容器块的左边框，即使浮动元素也是如此
    > * BFC的区域不会与float的元素区域重叠
    > * 计算BFC的高度时，浮动子元素也参与计算
    > * BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素，反之亦然
    >
    > **BFC可以解决的问题**
    >
    > * 垂直外边距重叠问题
    >
    > ```css
    > .p {
    >     height: 20px;
    >     margin: 10px;
    > }
    > 
    > .wrap {
    >     overflow: hidden;
    > }
    > ```
    >
    > ```html
    > <div class="p"></div>
    > <div class="wrap">
    >     <div class="p"></div>
    > </div>
    > ```
    >
    > 
    >
    > * 去除浮动
    >
    > ```css
    > .content {
    > 	float: left;
    > }
    > 
    > .wrap {
    >     overflow: hidden;
    > }
    > ```
    >
    > ```html
    > <div class="wrap">
    > 	<div class="content"></div>
    > </div>
    > ```
    >
    > 
    >
    > * 自适应两列布局（float + overflow)
    >
    > ```css
    > body {
    >     width: 300px;
    > }
    > 
    > .aside {
    >     width: 100px;
    >     height: 150px;
    >     float: left;
    >     background-color: green;
    > }
    > 
    > .main {
    >     height: 200px;
    >     background: blue;
    >     overflow: hidden;
    > }
    > ```
    >
    > ```html
    > <body>
    >     <div class="aside"></div>
    >     <div class="main"></div>
    > </body>
    > ```
    >
    > 

3. 关于重绘和回流

    当元素的样式发生变化时，浏览器需要触发更新，重新绘制元素；这个过程中，有两种类型的操作，即重绘和回流；

    > **重绘**
    >
    > 当元素样式的改变不影响布局的时候，浏览器会使用重绘对元素进行更新，此时只需要UI层面的重新绘制像素，因此损耗较少
    >
    > **回流**
    >
    > 当元素的尺寸、结构改变或触发某些属性时，浏览器会重新渲染页面，称为回流；此时，浏览器需要重新经过计算，计算后重新进行页面布局
    >
    > 会触发回流的操作
    >
    > * 页面初次渲染
    > * 浏览器窗口大小改变
    > * 元素尺寸、位置、内容发生改变
    > * 元素字体大小变化
    > * 添加或删除可见的dom元素
    > * 激活CSS伪类
    > * 查询某些属性或者调用某些方法
    >
    > 回流必定触发重绘，重绘不一定触发回流

4. css选择器有哪些

    * id选择器 --- #myId
    * 类选择器 --- .myClassName
    * 标签选择器 --- div、p、input
    * 相邻选择器 --- h1 + p
    * 子选择器 --- ul > li
    * 后代选择器 --- li a
    * 通配符选择器 --- *
    * 属性选择器 --- input[type="text"]
    * 伪类选择器 --- a:hover li:first-child
    * 伪元素选择器 --- .myDiv::before

5. 可继承的属性

    * font-size
    * font-family
    * color

6. 不可继承的属性

    * border
    * padding
    * margin
    * width
    * height
    * ...

7. 如何计算css优先级

    > 通配符选择器: 0
    >
    > 元素选择器/关系选择器/伪元素选择器: 1
    >
    > class选择符/属性选择器/伪类选择器: 10
    >
    > id选择符: 100
    >
    > 内联样式：1000
    >
    > !important: ∞

    如果优先级相同，则选择最后出现的样式；通过继承获得的样式优先级最低

8. css新增伪类有哪些

    * p:first-of-type 选择属于其父元素的首个元素的元素
    * p:last-of-type 选择属于其父元素的最后一个元素的元素
    * p:only-of-type 选择属于其父元素唯一的元素的元素
    * p:only-child 选择属于其父元素的唯一子元素的元素
    * p:nth-child(2) 选择属于其父元素的第二个子元素的元素
    * :enabled :disabled 表单控件的启用/禁用状态
    * :checked 单选框或复选框被选中

9. 如何居中div

    ```css
    /* 绝对定位加margin: auto */
    .container {
        position: relative;
    }
    
    .content {
        position: absolute;
        top: 0;
        bottom: 0;
        left: 0;
        right: 0;
        margin: auto;
    }
    ```

    ```css
    /* 绝对定位加transfrom */
    /* 如果content宽高固定，也可以使用50%负margin的形式，这里懒得写了 */
    .container {
        position: relative;
    }
    
    .content {
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
    }
    ```

    ```css
    /* flex布局 */
    .container {
        display: flex;
     	align-items: center;
        justify-content: center;
    }
    ```

    ```css
    /* 利用行级元素居中的特性 */
    .container {
        width: 200px;
        height: 200px;
        line-height: 200px;
        text-align: center;
    }
    
    .content {
        width: 100px;
        height: 100px;
        display: inline-block;
        vertical-align: middle;
    }
    ```

10. css3有哪些新特性

    > 支持透明度rgba和opacity
    >
    > background-image、background-origin、background-size、background-repeat
    >
    > word-wrap: break-word
    >
    > 文字阴影及盒阴影
    >
    > font-face 属性，定义自己的字体
    >
    > 圆角border-radius
    >
    > 边框图片border-image
    >
    > flex布局
    >
    > 媒体查询
    >
    > ...

11. 如何理解css3的flex-box

     该布局模型的目是时提供一种更为高效的方式来对容器中的条目进行布局、对齐和空间分配；在传统布局中，block布局是把块在垂直方向上从上到下依次排列；而inline布局则是在水平方向上来排列；弹性盒布局则没有此方向限制，可以由开发人员自由操作；

     flex布局更适用于移动端开发，在andriod和ios上可以完美支持；

12. 如何用css创建一个三角形；

     ```css
     .container {
         width: 0;
         height: 0;
       	border-left: 100px solid green;
       	border-bottom: 100px solid green;
       	border-top: 100px solid transparent;
       	border-right: 100px solid transparent;
     }
     ```

13. 为什么要初始化css样式

     不同浏览器对有些标签的样式默认值时不同的，如果没有初始化样式，往往会出现在不同浏览器之间的页面显示差异；

14. visibility属性的collapse在不同浏览器下有什么区别

     * 在chrome浏览器下，使用collapse和使用hidden没有区别
     * 在ff、opera、ie下，和display: none没有区别（都是旧版浏览器）

15. display: none 和 visibility: hidden区别

     前者不显示对应元素，不在文档流中分配空间（回流+重绘）

     后面隐藏对应元素，在文档布局中仍然保留原来的空间（重绘）

16. position和display、overflow、float这些特性叠加后会怎么样

     display属性规定了元素应该生成的框的类型；position属性规定元素的定位类型；oveflow规定了子元素的显示类型；float是布局方式，定义了元素在哪个方向上浮动；

     类似于优先级机制：position: absolute/fixed优先级最高，有这两个属性时，float不起作用，display值需调整；float或者absolute定位的元素，只能是块级元素或者表格；

17. 如何清除浮动

     * 父div可以手动设置height
     * 浮动元素后可以跟一个空div设置clear: both；
     * 父div可以设置overflow: hidden（生成BFC）

18. 关于浮动

     * 设置浮动之后，该元素的display值自动变为block

19. 媒体查询语法

     ```css
     .container {
         height: 100px;
         background-color: red;
     }
     
     @media screen and (max-width: 800px) {
         .container {
             background-color: blue;
         }
     }
     
     @media screen and (max-width: 500px) {
         .container {
             background-color: green;
         }
     }
     ```

20. css优化方法

     * 避免过度约束
     * 避免后代选择符
     * 避免链式选择符
     * 避免通配符
     * 避免不必要的命名空间
     * 避免不必要的重复
     * 避免!important
     * ...

21. 什么是响应式设计

     响应式设计是一个网站的样式能够兼容多个终端，而不是为每一个终端做一个特定的版本；

     其基本原理是通过媒体查询的形式，检测不同设备的屏幕尺寸从而应用不同的样式；

     页面头部必须有meta声明的viewport

     ```javascript
     <meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1,user-scalable=no">
     ```

22. 理解line-height

     行高是指一行文字的高度，具体说是两行文字间基线的距离；css中起高度作用的是height和line-height，没有定义height属性，最终其表现作用的一定是line-height 属性；

     单行文本居中，设置line-height值与height值一致；

     多行文本居中，需要设置display属性为inline-block;

23. 如何让chrome支持12px以下的字体

     设置 `transform: scale(.8)`缩放字体

24. 如何让页面里的字体变清晰变细

     `-webkit-font-smoothing: antialiased`可以在ios上起到平滑字体的作用，windows下无效；

25. position: fixed 在安卓下无效怎么处理

     携带响应式布局的头

     ```html
     <meta name="viewport" content="width=device-width,initial-scale=1.0,minimum-scale=1.0,maximum-scale=1.0,user-scalable=no">
     ```

26. 如果手动写动画，你认为最小时间间隔是多久，为什么

     多数显示器默认频率是60HZ，所以理论上最小时间间隔为1/60s=16.7ms;

27. 图片格式区别

     > **png**
     >
     > Protable Netword Graphics，一种无损数据压缩格式，压缩比高，色彩好，大部分地方都可以使用，保存透明通道；
     >
     > **jpg**
     >
     > 一种针对相片使用的失真压缩方法，是一种破坏性压缩，在色调及颜色平滑上做得不错，不存储透明通道；
     >
     > **gif**
     >
     > 以8位色重现真色彩的图像，可以保留多帧做动态图，色彩差，压缩性差；
     >
     > **webp**
     >
     > 谷歌推出的一种图片格式，同时提供有损和无损压缩格式，支持透明通道，压缩率只有jpg的2/3，大小比png小45%；缺点是压缩的时间更久，兼容性差；

28. 关于行内元素和块级元素和行级块元素

     > **行内元素**
     >
     > a、em、i、b、span、font、strong、big、input、textarea、img等等
     >
     > **块级元素**
     >
     > div、p、h1~h6、ul、ol、li、table、dd、hr、header、section、aside、footer等等
     >
     > **行级块元素**
     >
     > select、button

29. nth-of-type 和 nth-child 区别

     li:nth-of-type(2) 是指父元素的第二个li元素

     li:nth-child(2) 是指父元素下的第二个元素且必须是li的元素（很容易找不到）

30. css2.0和css3.0区别

     css3加强了css2的功能，增加了许多新的属性和新的标签，并删除了一些冗余的标签，在布局方面减少了代码量，也增加了更多的效果（圆角、动画之类的）还有在盒子模型和列表模块都进行了改进；

31. 如何理解标签语义化

     标签语义化就是指标签的含义；语义化的主要目的就是让大家更直观地认识标签和属性地作用；主要目的是对搜索引擎更加友好，有了良好地结构和语义，我们地网页内容更容易被搜索引擎抓取，更方便网站的推广；而且可维护性高，且容易阅读；

     常见语义化标签

     title、h1~h6、p、ul、li、ol

     header、nav、footer、article、main、section、aside（html5新增）

     等等等等
    
32. 关于伪类选择器和伪元素

     伪类用一个冒号表示，伪元素用两个冒号表示；

     > **伪类选择器**
     >
     > 由于状态是动态变化的，所以一个元素达到一个特定的状态时，它可能得到一个伪类的样式；当状态改变时，它又会失去这个样式；
     >
     > **伪元素选择器**
     >
     > 并不是针对真正的dom元素使用的选择器，而是针对css中已经定义好的伪元素的选择器；

33. css解析顺序

     css是从右往左解析的，因为这样子解析性能最好，从右往左才一开始就可以筛选掉大量的叶子节点；如果从左往右解析的话，那如果碰到不符合规则的子节点，那么又要往父节点方向回溯，回溯操作需要浪费大量的性能，增加css解析时间；

34. 常见浏览器兼容性问题

     > 1. 不同浏览器的默认margin和padding不同 --- 初始化样式
     > 2. img之间有空隙且不同浏览器表现不同 --- 设置img为display: block;

35. link和@import区别

     @import是css提供的语法规则，用来引入外部样式表；link是html提供的标签，不仅可以加载css，还可以定义rss，rel连接属性；

     加载页面时，link标签引入的css被同时加载，@import引入的css将在页面加载完毕后被加载；