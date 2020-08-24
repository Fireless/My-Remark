

# CSS 权威指南笔记

## 一、CSS和样式

### 1.1  元素

- 置换元素：元素内容不由文档内容表示。举例：\<img\>  \<input\>
- 非置换内容：文档内容插入到元素内容中，元素本身存在一定格式，且不会被文档内容的插入破坏。
- 块级元素：默认为一个填满父级元素内容区域的框，存在边界。
- 行内元素：不会打断所在的行，即可以理解为，行内元素可以出现在其他块级元素的内容区域。

### 1.2  CSS如何应用到HTML上

#### 1.2.1  link标签

```html
<link rel="stylesheet" type="text/css" href="sheet1.css" media="all">
<link rel="alternate stylesheet" type="text/css" href="sheet1.css" title="default" media="all">
<link rel="alternate stylesheet" type="text/css" href="sheet1.css" title="big_bang!" media="all">
```

  通过link标签关联样式表，最为常见的一种关联方式，需要注意的是link标签必须放在head元素中才会生效，且外部样式表中不能含有任何文档标记。其中rel属性为relation，若设置为alternate，则该样式为候选样式表，在浏览器中可以灵活的切换。其中title属性即为切换样式表时的名字。title = “default”时为默认样式表，若同时设置多个default，只有第一个会生效。但其余title名若相同，则视为同一个系列的样式。media属性为样式生效的媒体设备，比如显示器，打印预览，etc。

#### 1.2.2  Style元素

直接将样式写在style元素中，适用于单个页面的样式调整。在style属性中也可以通过@import引入外部样式表。

#### 1.2.3 @import命令

@import  url(style.css)

@import命令必须在其他css规则之前，若写在css规则之后，则会失效。

URL 可以是绝对地址也可以是相对地址，也可以使用网络地址，使用在线资源。

#### 1.2.4 HTTP链接

闻所未闻，加在HTTP首部。主要是针对不同系列的浏览器进行样式的区分。

#### 1.2.5 行内样式

在元素内部直接增加style属性，此处不能引用外部样式。操作简单，但会增加项目维护成本，不利于优化。

### 1.3 样式表中的内容

- 标记：样式表不能出现标记，HTML中的注释除外 \<!---->
- 规则结构：h1 {color: red;background:yellow}
- 厂商前缀
- 处理空白：与HTML相似，连续空白会合成一个空白
- CSS注释： /** 这是一个css注释 **/；注意 注释不能嵌套

### 1.4 特性查询

```css
@support (color：black){
	body {color:black;}
}
```

上述查询可以理解为，浏览器是否支持color属性，如果支持则采用下面的样式，如果不支持则忽略。所以特性查询可以很好的用在渐进增强样式。在一个css版本还未完全迭代的时候可以先行使用部分样式，不至于出现因为一个样式没有普及而无法使用该样式的尴尬处境。缺点是，某些浏览器可能支持特性，但存在缺弦，此时样式仍会应用。

## 二、选择器

### 2.1 样式的基本规则

**CSS的核心优势是，可以把文档中的某种类型的元素全部应用相同的规则。**最常见的方式是通过元素选择符选择要操作的DOM元素，元素选择符通常为HTML元素。选择后通过声明来决定要修改成什么样式。多个声明可以写在一起，作为一个声明块。格式为 【属性：值；】 ，如果声明中的属性或者值有误，整个规则都回被忽略。

Tips：CSS中属性的值通常用空格分开，但font属性有一个特别，元素字号和行高需要用 '/'分开，h2 {font: large/150%}.\

### 2.2  群组

多个元素选择符可以写在一起，同时使用同一个声明块。以‘，’ 分隔。h1,h2,h3,h4 {color:blue;}.这种群组的方式会很大程度上提高代码的间接性，但也存在一定问题，当后期需要修改样式是，需要将目标样式从各个群组中剥离出来，才能继续操作，否则会影响到其他DOM节点。同时，如果存在多个声明，需要在声明之后加上“；”，不加分号的部分会被视为一个声明，从而失效。

一个黑科技：浏览器不识别某个DOM元素时可以手动增加,如增加一个<main>元素，运行如下代码时，浏览器会认识到元素main的存在，从而允许选择。

```javascript
document.createElement('main')
```

### 2.3  类选择符和ID选择符

类选择符，ID选择符都可以起到，为指定元素修改样式的作用。其中类选择符class可以同时修改多个样式，还可以通过元素标签进行筛选，p.colorblue {  }表示对含有colorblue类的p标签进行修改。而p.colorbule .fontx {} 则表示对同时具有colorblue和fontx类的p标签进行操作。

ID选择器则由于ID的唯一性，一般只对某一个元素产生影响，但由于有些浏览器并不一定会检查ID的唯一性，所以如果同时存在多个ID，这些ID有可能还是会被全部赋值。在CSS中这没有太大影响，但有多个元素具有同一ID，会对JavaScript操作是产生影响，只会作用于第一个，因为getElementById（）等函数预期只有一个ID属性为选定值。

### 2.4 属性选择符

#### 2.4.1 简单属性选择符

```html
<style>
    a[href] {
        font-weight:bold;
    }
    
    a[href][title]{
        color:red;
    }
</style>
<a href="#" title="Stan's Note" />
<a href="www.fireless7.online" title="Stan" />
<a title='no href' />

```

简单的属性选择器，可以通过属性来选择元素，如上述，第二个选择的就是，同时具有href和title标签的元素。在实际应用中可以作为代码诊断使用，通过标亮颜色，来区分出是否有元素漏写某些属性。

#### 2.4.2 精准的属性选择

```css
p[class="atm"]
p[class="atm ilike"]
```

此种方式选择属性，需要属性和属性值同时满足，且属性值需要完全匹配。如果一个元素的class属性为“atm iLike”，则只会被第二种写法匹配到。第一种写法无法匹配。

#### 2.4.3 根据部分属性值选择

- [foo != "bar"]    选择元素有foo属性，且以bar- 开头
- [foo ~= "bar"]   其值包含bar这个词
- [foo *= "bar"]    包含子串为bar 更多的时候用在 ”https://“ 
- [foo ^= "bar"]   以bar开头
- [foo $= "bar"]   以bar结尾，“.jpg”,".img" 文件选择

#### 2.4.4 不区分大小写的标识符

```css
a[href$=".pdf"  i]
```

加上 后缀 i 之后 ，不再区分pdf的大小写，含有href属性，其值以任意大小写PDF结尾的元素都会被选中。

### 2.5  根据文档结构选择

#### 2.5.1 后代选择符

```css
h1 em {color:gray}
ul ol ul em {color:gray}
```

后代选择符可以理解为包含，只要是后代元素都会被应用，需要区别于子元素。且后代选择符对元素的距离一无所知。

#### 2.5.2 选择子元素

```css
h1 > strong {color:red}
```

只作用于子元素，上述语句表示h1元素的子元素strong字体变为红色，包含在h1内，但不是h1直接子元素的strong不发生变化。 

#### 2.5.3  选择兄弟元素

```css
h1 + p {margin-top: 0;}
h2 ~ol {font-style:italic;}
```

"+"表示选择紧邻的兄弟元素，若之后的兄弟元素之间存在其他元素，其他元素之后的兄弟元素无法生效。

“~”表示选择任意兄弟元素，其间间隔任意其他元素都可以生效。

### 2.6  伪类选择符

#### 2.6.1 拼接伪类

CSS允许将伪类串联起来，以超链接a标签举例,伪类的前后顺序不影响结果

```css
a:link:hover {color:red;}
a:visited:hover {color:red;}
a:link:hover:lang(de) {color:gray;}
```

#### 2.6.2 结构伪类

**伪类始终执行那个所依附的元素**，可以理解为对“：” 之前的元素进行进一步的筛选

1. :root  选择根元素
2. :empty 选择空元素，注意元素内包含文本也视为非空元素
3. :only-child  选择唯一子代
4. :only-of-type  同胞中属于唯一种类的元素，通常可以用来选择img元素
5. :first-child  元素的第一个子代
6. :last-child  元素的最后一个子代
7. :first-of-type  元素的第一个，没有了子代的限制
8. :last-of-type  元素的最后一个
9. :nth-child  选择任意位置子元素 ，例： nth-child(2)  nth-child( 3n +1)，可自行设定规则
10.  :nth-last-child  从后往前
11. :nth-of-type  去除子元素条件

#### 2.6.3 动态伪类

##### 2.6.3.1.超链接伪类

- :link 具有超链接，且尚未被访问的元素
- :visit 具有超链接，已经被访问

##### 2.6.3.2.用户操作伪类

- :focus  当前获得输入焦点的元素
- :hover 鼠标指针悬停其上的元素
- :active 由用户输入激活的元素

#### 2.6.4 UI状态伪类

##### 2.6.4.1.启用和禁用的UI元素

```css
:enabled {font-weight:bold;}
:disabled {color:red;}
//通常用于对禁用的元素设置样式,提示用户.
```

##### 2.6.4.2.选择状态

```css
:checked {background-color:silver;}
:interminate {border:3px;}
//可以通过:not(:check)选择 未被选择元素
```

##### 2.6.4.3.默认选项伪类

```css
:default {font-style:italic}
//default指按钮，选择框，选择的初始选择元素，不会随check改变
:default + label{
    font-size:large;
}
//跟上标签一起操作，为常用法，且checked的属性不会覆盖default
```

##### 2.6.4.4 可选性伪类

```html
 <style>
 	input:required{border:1px solid #FFF}
 	input:optional{border:2px solid #DDD}
 </style>
 <input type="text" required>
 <input type="text" >
 <input type="text" required="false">
```

显而易见，required伪类就是选择表单中的必填项，optional选择的选填项。但是在实际测试中发现，第三个input加入了属性 required=“false” 之后还是会被认识是一个必填项，匹配 :require.

##### 2.4.6.5 有效性伪类

```css
input[type="email"]:focus:invalid{
	background-image:url(warning.jpg);
}

input[type="email"]:focus:valid{
 	background-image:url(checkmark.jpg);
}
```

:valid和:invalid 伪类只使用与能检查数据有效性的元素，因此大部分情况是用于表单验证（前端验证）时，对元素进行样式的改变，且通常与:focus伪类配合。

##### 2.4.6.6 范围 伪类

```html
<style>
    input[type="number"]:in-range{ color:red;}
    input[type="number"]:out-of-range{ color:red;}
    input[type="number"]:invalid{ color:blue;}
</style>
<input type="number" min="0" max="100" step="10" value="23">
//范围伪类只对设定了范围的元素生效
```

##### 2.4.6.7 可变性伪类

```css
textarea:read-only{font-style:italic;}
textarea:read-write{opacity:0.5;}
//字面意思分别针对只读和可编元素
```

#### 2.6.5 :target伪类

首先解释URL中的片段标识符(fragment identifier)

www.baidu.com/tr.html#traget-pseudo

上述URL中“#”之后的就是片段标识符，网址解析后，会直接导航到页面中元素ID为target=persudo的元素。

:target伪类就是作用于这类元素

#### 2.6.6 :lang伪类

用来装饰特定语言 :lang(en)

#### 2.6.7  :not 伪类

一个否定伪类，可以对上述所有肯定伪类进行反选，需要注意的是 :not() 括号中只能使用一个选择符，不能使用连接选择符和群组选择符。

### 2.7  伪元素选择符

- :first-letter  首字母
- :first-line  首行
- ::before   选择元素之前
- ::after      选择元素之后

```css
 li:first-child::after{
        content: "Tina";
        font-size: xx-large;
    }
 li:first-child::before{
        content: "**";
        color: red;
        font-weight: bolder;
    }
```

关注content的属性，还可以加文字符号等。其中first-letter和first-line存在限制，只能应用到块级元素上，例如标题和断乱，不能应用到行内元素上，例如超链接。

## 三、特指度和层叠

### 3.1 特指度

选择符的特指度由选择符本身的组成部分决定，一个特指度由四部分组成（0,0,0,0），规则如下

- 选择符中每个ID属性值加0,1,0,0
- 选择符中的每个类属性值，属性选择或者伪类加0,0,1,0
- 选择符中的每个元素和伪元素加0,0,0,1
- 行内样式的特指度为1，0,0,0
- 连接符和通用选择符不增加特指度
- 特指度的比较是从左到右的，根据精确度可以理解为 ID > Class > 元素 、伪元素

PS：注意区别ID选择符和选择id属性的属性选择符之间的区别，增加 ！important之后独立于非重要声明进行特指度比较，所有重要规则一律优先于非重要声明。

### 3.2 继承

继承指某些样式不仅会应用到指定的元素上，还会继承到元素的子代上。例如，h1元素内的文字就会继承h1元素的样式，还有ul，ol中也会继承。需要注意的是继承绝大多数情况只会向下继承，不会向上继承。只有当HTML元素没有设置背景，而body元素设置了背景这种情况下，body元素的背景会向上“继承”给HTMl元素。

还需要注意的一点是，继承的样式无特指度，注意是无特指度，不是零特指度。零特指度是大于无特指的。所以需要慎用通用选择符“**<span style="font-size:30px;">*</span>**” ，他的使用会覆盖掉文档中的继承样式

### 3.3 层叠

当两个元素样式特指度相同时，一般按照先后顺序，靠后的后渲染，最终表现为靠后。还有就是什么的权重，具体如下：

1. 读者提供的样式中以！important 标记的声明
2. 创作人员编写的样式中以！important标记的声明
3. 创作人员编写的常规声明
4. 读者提供的常规声明
5. 用户代理的默认声明

## 四、值和单位

### 4.1 关键字、字符串和其他文本

#### 4.1.1 关键字

​	每个CSS属性都有其允许使用的关键字范围，只有在范围内的关键字才可以生效。同一个关键字在不同属性中可能表达不同的含义。

CSS3定义了几个“全局“关键字，规范中的每个属性都可以使用：inherit,inital 和 ubset

- inheri：:令某一属性强制继承父元素该属性的值
- initial：将属性值设为初始值，意义在于，不同用户代理（可以理解为浏览器）的初始设置可能不同，此时可以自行适配浏览器
- unset： 对继承属性来说，unset  == inherit ；对于非继承属性来说，unset == initial

#### 4.1.2 字符串

 “i like to play with strings."

CSS字符串与其他语言字符串类似，也可以通过反斜杠进行转义，若需要换行则需要使用Unicode字符 \A.

#### 4.1.3 URL

Uniform Resource Locator 统一资源定位符，用于获取Web服务器上的资源。主要需要注意绝对地址和相对地址的区别。

#### 4.1.4 图像

对于浏览器来说，<image> 的值通常可以看成URL值。

#### 4.1.5 标识符

某些属性接受标识符，标识符由用户定义，本身是词，严格区分大小写。

### 4.2 数字和百分数

数字可以为正数，也可以是浮点数，正负都可，具体元素会有不同的范围限制。

在栅格布局中引入了弹性值，即在数字后面跟"fr"。

### 4.3 距离

#### 4.3.1 绝对长度单位

-  英寸(in)
- 厘米（cm）
- 毫米（mm）
- 四分之一毫米（q）
- 点（pt）
- 派卡（pc）
-  像素（px）

#### 4.3.2 相对长度单位

**em和ex**

都会随着元素的Font-size改变，1em等于元素的font-size属性值，所以不同元素的em实际值可能是不同的。1em等于所用字体中小写字母m的宽度，1ex等于所用字体小写字母x的高度。

**rem**

rem跟em很相似，也会随元素font-size的改变而改变，区别在于，rem不是跟随父元素，而是始终跟随根元素也就是html元素的font-size值。可以用来初始化字体，font-size：1rem 就可以把字体设置成根元素一致的大小。

**ch**

可以简单理解为“一个字符”

**vw  vh  vmin vmax**

视区单位，根据浏览器当前窗口变化大小，100vh即视区高度，100vw为视区宽度，vmin为宽度和高度中较小值的1/100,vmax 为宽度高度较大值的1/100。

### 4.4 计算值

CSS允许使用calc（）计算值，但仅限于加减乘除。

p {witdth: calc(90% - 2rem) }

Tips: 运算符左右两侧需要加空格。

### 4.5 颜色

**具名颜色**

直接使用颜色英文来表示

**RGB和RGBa**

rgb（0,0,0）  rgba（0,0,0,1） RGBa比RGB多的一位参数表示颜色的透明度。还可以使用16进制RGB色  #EFEFEF。 





