# JavaScript高级程序设计笔记

## 一、JavaScript简介

###  1.1 JavaScript诞生的原因

随着web日益流行，人们对客户端脚本语言的需求越来越强烈。当时使用的还是“调制解调器”拨号上网的方式上网，速度极低。用户在客户端提交的数据，全都是先提交给服务端，再由服务端计算，最后返回给客户端。设想，如果有一个复杂的表单，对其做的每一个表单验证都需要在服务端处理，那么耗时是很长的。对于用户来说，提交表单数据后，需要等待10s甚至20s来等来服务端的表单验证，大量时间被花费在数据传输上，这样的用户体验是很不友好的。因此JavaScript应运而生，被用来在客户端处理一些数据，进行表单验证等操作，提高了用户体验。

### 1.2 JavaScript实现

#### 1.2.1 ECMAScript

为JS的核心部分，主要规定了语法，类型，语句，关键字，保留字，操作符，对象。目前主要作为JavaScript的一部分用于Web浏览器的前端处理。但Web浏览器只是ECMAScript的宿主之一。服务端的JS平台Node，Adobe Flash都使用了ECMAScript作为核心部分。

#### 1.2.2 DOM

全称为Document Object Model，即文档对象模型。把一个页面映射成多层次的节点结构，这些节点又可以包含数据。因此开发人员可以对这些节点进行增加，查找，删除操作，从而方便的控制页面展示的内容。

```html
<html>
	<head>
		<title> Just a Example </title>
	</head>
	<body>
		<p> Hello world!!!!</p>
	</body>
</html>
```

Dom标准主要规定了：

- DOM Core ：规定如何映射基于XML的文档结构。
- DOM HTML： 在DOM核心上进行了扩展，增加了针对HTML的对象和方法。
- DOM Views：定义了跟踪不同文档视图的接口（比如引用CSS之前和之后的）。
- DOM Events： 定义了事件和事件处理接口。
- DOM Style：定于了基于CSS为元素定义样式的接口。
- DOM Traversal and Range：定义了遍历和操作文档树的接口。

#### 1.2.3  BOM

全称Browser Object Model。指浏览器页面以外的部分，目前还没有比较统一的标准，主要有一下这下功能：

- 移动，缩放，关闭浏览器窗口
- 弹出新浏览器窗口的功能
- 获取Session
- 对Cookie的支持
- 提供用户显示屏分辨率的详细信息的Screen对象
- 提供浏览器详细信息的navigation对象
- 提供所加载页面的详细信息的location对象