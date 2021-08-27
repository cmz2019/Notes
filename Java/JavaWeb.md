# HTML

页面由三部分内容组成，分别是内容（结构）、表现、行为。

+ 内容（结构），是我们在页面中可以看到的数据，我们称之为内容。一般使用 html 技术来展示。
+ 表现，指的是这些内容在页面上的展示形式。比如说，布局，颜色，大小等等。一般使用 CSS 技术实现。
+ 行为，指的是页面中元素与输入设备交互的响应。一般使用 JavaScript 技术实现。

## HTML简介

Hyper Text Markup Language （超文本标记语言），简写：HTML

HTML 通过标签来标记要显示的网页中的各个部分。网页文件本身是一种文本文件，通过在文本文件中添加标记符，可以告诉浏览器如何显示其中的内容（如：文字如何处理，画 面如何安排，图片如何显示等）。

注：Java 文件是需要先编译，再由 java 虚拟机跑起来。但 HTML 文件不需要编译，直接由浏览器进行解析执行。

HTML 的书写规范如下：

```html
<html>						<!-- 表示整个html页面的开始 -->
    <head>					<!-- 头信息，一般包含三部分内容，title标签、css样式、js代码 -->
        <title>标题</title>  <!-- 标题-->
    </head>
    <body>					<!-- body是页面的主体内容 -->
        页面主题内容
    </body>
</html>						<!-- 表示整个html页面的结束 -->
```

## HTML标签介绍

1. 标签的格式：==<标签名>封装的数据</标签名>==
2. 标签名大小写不敏感。 
3. 标签拥有自己的属性。分为两个属性：
   + 基本属性：`bgcolor="red"` 可以修改简单的样式效果。
   + 事件属性：`onclick="alert('你好！');"` 可以直接设置事件响应后的代码。
4. 标签又分为，单标签和双标签。
   + 单标签格式：==<标签名/>==   br表示换行，hr表示水平线
   + 双标签格式：==<标签名>...封装的数据...</标签名>==

<img src="https://gitee.com/cmz2000/album/raw/master/image/image-20210805195230932.png" alt="image-20210805195230932" style="zoom:50%;" />

### 标签的语法

```html
<!-- 标签不能交叉嵌套 -->
正确：<div><span>早安</span></div>
错误：<div><span>早安</div></span>

<!-- 标签必须正确闭合 -->
<!-- i.有文本内容的标签： -->
正确：<div>早安</div>
错误：<div>早安

<!-- ii.没有文本内容的标签： -->
正确：<br />
错误：<br >

<!-- 属性必须有值，属性值必须加引号 -->
正确：<font color="blue">早安</font>
错误：<font color=blue>早安</font>
错误：<font color>早安</font>

<!-- 注释不能嵌套 -->
正确：<!-- 注释内容 -->
错误：<!-- 注释内容 <!-- 注释内容 --> -->
```

注意：html 代码不是很严谨。有时候标签不闭合，也不会报错，浏览器会自动修复。

## 常用标签

详见 [https://www.w3school.com.cn/tags/index.asp](https://www.w3school.com.cn/tags/index.asp)

### font字体标签

需求：在网页上显示 “我是字体标签”，并修改字体为 宋体，颜色为红色。

```html
<body>
    <!-- 字体标签
        font 标签是字体标签,它可以用来修改文本的字体,颜色,大小(尺寸)
        color 属性修改颜色
        face 属性修改字体
        size 属性修改文本大小
    -->
	<font color="red" face="宋体" size="7">我是字体标签</font>
</body>
```

### 特殊字符

需求：把 \<br\> 换行标签变成文本，转换成字符显示在页面上

```html
<body>
    <!-- 特殊字符
        常用的特殊字符:
        < 对应 &lt;
        > 对应 &gt;
        空格 对应 &nbsp;
    -->
	我是&lt;br&gt;标签<br/>
</body>
```

HTML 中的常用字符实体是不间断空格 `&nbsp;`。

浏览器总是会截短 HTML 页面中的空格。如果在文本中写 10 个空格，在显示该页面之前，浏览器会删除它们中的 9 个。如需在页面中增加空格的数量，需要使用 `&nbsp;` 字符实体。

<img src="https://gitee.com/cmz2000/album/raw/master/image/image-20210805201207573.png" alt="image-20210805201207573" style="zoom: 80%;" />

注意：实体名称对大小写敏感。

### 标题标签

标题标签是 h1 到 h6

```html
<body>
    <!-- 标题标签
        h1 - h6 都是标题标签
        h1 最大
        h6 最小
            align 属性是对齐属性
            left 左对齐(默认)
            center 居中
            right 右对齐
    -->
    <h1 align="left">标题 1</h1>
    <h2 align="center">标题 2</h2>
    <h3 align="right">标题 3</h3>
    <h4>标题 4</h4>
    <h5>标题 5</h5>
    <h6>标题 6</h6>
    <h7>标题 7</h7>  <!-- h7不属于标题范围，无法识别，浏览器只是简单的把文本显示出来 -->
</body>
```

### 超链接

在网页中所有点击之后可以跳转的内容都是超链接

```html
<body>
    <!-- a 标签是 超链接
        href 属性设置连接的地址
        target 属性设置哪个目标进行跳转
            _self 表示当前页面(默认值)
            _blank 表示打开新页面来进行跳转
    -->
    <a href="http://localhost:8080">百度</a><br/>
    <a href="http://localhost:8080" target="_self">百度_self</a><br/>
    <a href="http://localhost:8080" target="_blank">百度_blank</a><br/>
</body>
```

### 列表

常见的是无序列表、有序列表

```html
<body>
    <!--
        ul 是无序列表  ol 是有序列表
        type 属性可以修改列表项前面的符号
        li 是列表项
    -->
    <ul type="none">
        <li>赵四</li>
        <li>刘能</li>
        <li>小沈阳</li>
        <li>宋小宝</li>
    </ul>
</body>
```

### img标签

img 标签可以在 html 页面上显示图片。

需求：使用 img 标签显示一张照片。并修改宽高，和边框属性。

```html
<body>
    <!-- img 标签是图片标签,用来显示图片
        src 属性可以设置图片的路径
        width 属性设置图片的宽度
        height 属性设置图片的高度
        border 属性设置图片边框大小
        alt 属性设置当指定路径找不到图片时,用来代替显示的文本内容

        在 web 中路径分为相对路径和绝对路径两种
        相对路径:
            . 		表示当前文件所在的目录
            .. 		表示当前文件所在的上一级目录
            文件名	  表示当前文件所在目录的文件,相当于 ./文件名 ,其中 ./ 可以省略
        绝对路径:
            正确格式是: http://ip:port/工程名/资源路径
            错误格式是: 盘符:/目录/文件名
    -->
    <img src="1.jpg" width="200" height="260" border="1" alt="美女找不到"/>
    <img src="../2.jpg" width="200" height="260" />
    <img src="../imgs/3.jpg" width="200" height="260" />
    <img src="../imgs/4.jpg" width="200" height="260" />
    <img src="../imgs/5.jpg" width="200" height="260" />
    <img src="../imgs/6.jpg" width="200" height="260" />
</body>
```

### 表格

需求 1：做一个 带表头的，三行，三列的表格，并显示边框
需求 2：修改表格的宽度，高度，表格的对齐方式，单元格间距

```html
<body>
    <!-- table 标签是表格标签
            border 设置表格标签
            width 设置表格宽度
            height 设置表格高度
            align 设置表格相对于页面的对齐方式
            cellspacing 设置单元格间距
        tr 是行标签
		td 是单元格标签
        th 是表头标签
        align 设置单元格文本对齐方式
        b 是加粗标签
		colspan 属性设置跨列
		rowspan 属性设置跨行
    -->
    <table align="center" border="1" width="300" height="300" cellspacing="0">
        <tr>
            <th>1.1</th>
            <th>1.2</th>
            <th>1.3</th>
        </tr>
        <tr>
            <td>2.1</td>
            <td>2.2</td>
            <td>2.3</td>
        </tr>
        <tr>
            <td>3.1</td>
            <td>3.2</td>
            <td>3.3</td>
        </tr>
    </table>
</body>
```

效果如下：

<img src="https://gitee.com/cmz2000/album/raw/master/image/image-20210805214644074.png" alt="image-20210805214644074" style="zoom:67%;" />

### 表单

什么是表单?
表单就是 html 页面中，用来收集用户信息的所有元素集合.然后把这些信息发送给服务器

```html
<body>
    <!--
        form 标签就是表单
            input type=text 是文件输入框, value 设置默认显示内容
            input type=password 是密码输入框, value 设置默认显示内容
            input type=radio 是单选框, name 属性可以对其进行分组, checked="checked"表示默认选中
			input type=checkbox 是复选框, checked="checked"表示默认选中
            input type=reset 是重置按钮, value 属性修改按钮上的文本
            input type=submit 是提交按钮, value 属性修改按钮上的文本
            input type=button 是按钮, value 属性修改按钮上的文本
            input type=file 是文件上传域
            input type=hidden 是隐藏域, 当我们要发送某些信息, 而这些信息, 不需要用户参与, 就可以使用隐藏域(提交的时候同时发送给服务器)
            select 标签是下拉列表框
            option 标签是下拉列表框中的选项, selected="selected"设置默认选中
            textarea 表示多行文本输入框(起始标签和结束标签中的内容是默认值)
                rows 属性设置可以显示几行的高度
                cols 属性设置每行可以显示几个字符宽度
    -->
    <form>
        用户名称：<input type="text" value="默认值"/><br/>
        用户密码：<input type="password" value="abc"/><br/>
        确认密码：<input type="password" value="abc"/><br/>
        性别：<input type="radio" name="sex"/>男
        	<input type="radio" name="sex" checked="checked" />女<br/>
        兴趣爱好：<input type="checkbox" checked="checked" />Java
        		<input type="checkbox" />JavaScript
        		<input type="checkbox" />C++<br/>
        国籍：<select>
                <option>--请选择国籍--</option>
                <option selected="selected">中国</option>
                <option>美国</option>
                <option>小日本</option>
        </select><br/>
        自我评价： <textarea rows="10" cols="20">我才是默认值</textarea><br/>
        <input type="reset" value="abc" />
        <input type="submit"/>
    </form>
</body>
```

效果如下：

<img src="https://gitee.com/cmz2000/album/raw/master/image/image-20210805221020132.png" alt="image-20210805221020132" style="zoom:80%;" />

#### 表单格式化

```html
<body>
    <form>
        <h1 align="center">用户注册</h1>
        <table align="center">
            <tr>
            	<td> 用户名称： </td>
            	<td>
                    <input type="text" value="默认值"/>
            	</td>
            </tr>
            <tr>
                <td> 用户密码： </td>
            	<td>
                    <input type="password" value="abc"/>
                </td>
            </tr>
            <tr>
            	<td>确认密码： </td>
            	<td>
                    <input type="password" value="abc"/>
                </td>
            </tr>
            <tr>
            	<td>性别： </td>
            	<td>
            		<input type="radio" name="sex"/>男
            		<input type="radio" name="sex" checked="checked" />女
            	</td>
            </tr>
            <tr>
            	<td> 兴趣爱好： </td>
            	<td>
            		<input type="checkbox" checked="checked" />Java
            		<input type="checkbox" />JavaScript
            		<input type="checkbox" />C++
            	</td>
            </tr>
            <tr>
            	<td>国籍： </td>
            	<td>
            		<select>
                        <option>--请选择国籍--</option>
                        <option selected="selected">中国</option>
                        <option>美国</option>
                        <option>小日本</option>
            		</select>
            	</td>
            </tr>
            <tr>
                <td>自我评价： </td>
                <td>
                    <textarea rows="10" cols="20">我才是默认值</textarea>
                </td>
            </tr>
            <tr>
                <td><input type="reset" /></td>
                <td align="center"><input type="submit"/></td>
            </tr>
        </table>
    </form>
</body>
```

效果如下：

<img src="https://gitee.com/cmz2000/album/raw/master/image/image-20210805222557046.png" alt="image-20210805222557046" style="zoom: 80%;" />

#### 表单提交

```html
<body>
    <!--
        form 标签是表单标签
            action 属性设置提交的服务器地址
            method 属性设置提交的方式 GET(默认值)或 POST
        表单提交的时候， 数据没有发送给服务器的三种情况：
            1、 表单项没有 name 属性值
            2、 单选、 复选（下拉列表中的 option 标签） 都需要添加 value 属性， 以便发送给服务器
            3、 表单项不在提交的 form 标签中
        GET 请求的特点是：
            1、 浏览器地址栏中的地址是： action 属性[+?+请求参数]
            	请求参数的格式是： name=value&name=value
            2、 不安全
            3、 它有数据长度的限制
        POST 请求的特点是：
            1、 浏览器地址栏中只有 action 属性值
            2、 相对于 GET 请求要安全
            3、 理论上没有数据长度的限制
    -->
    <form action="http://localhost:8080" method="post">
        <input type="hidden" name="action" value="login" />
        <h1 align="center">用户注册</h1>
        <table align="center">
            <tr>
            	<td> 用户名称： </td>
            	<td>
            		<input type="text" name="username" value="默认值"/>
            	</td>
            </tr>
            <tr>
            	<td> 用户密码： </td>
            	<td><input type="password" name="password" value="abc"/></td>
            </tr>
            <tr>
            	<td>性别： </td>
            	<td>
                    <input type="radio" name="sex" value="boy"/>男
                    <input type="radio" name="sex" checked="checked" value="girl" />女
            	</td>
            </tr>
            <tr>
                <td> 兴趣爱好： </td>
                <td><input name="hobby" type="checkbox" checked="checked" value="java"/>Java
                    <input name="hobby" type="checkbox" value="js"/>JavaScript
                    <input name="hobby" type="checkbox" value="cpp"/>C++
            	</td>
            </tr>
            <tr>
                <td>国籍： </td>
                <td>
                    <select name="country">
                        <option value="none">--请选择国籍--</option>
                        <option value="cn" selected="selected">中国</option>
                        <option value="usa">美国</option>
                        <option value="jp">小日本</option>
                    </select>
                </td>
            </tr>
            <tr>
                <td>自我评价： </td>
                <td><textarea name="desc" rows="10" cols="20">我才是默认值</textarea></td>
            </tr>
            <tr>
                <td><input type="reset" /></td>
                <td align="center"><input type="submit"/></td>
            </tr>
        </table>
    </form>
</body>
```

### 其他标签

```html
<body>
    <!--
        div标签, 默认独占一行
        span标签, 它的长度是封装数据的长度
        p段落标签, 默认会在段落的上方或下方各空出一行来（如果已有就不再空）
    -->
    <div>div 标签 1</div>
    <div>div 标签 2</div>
    <span>span 标签 1</span>
    <span>span 标签 2</span>
    <p>p 段落标签 1</p>
    <p>p 段落标签 2</p>
</body>
```

# CSS

CSS 是「层叠样式表单」 。是用于（增强）控制网页样式并允许将样式信息与网页内容分离的一种标记性语言。 

详细文档参考 [https://www.w3school.com.cn/css/index.asp](https://www.w3school.com.cn/css/index.asp)

## CSS语法规则

![image-20210806003753145](https://gitee.com/cmz2000/album/raw/master/image/image-20210806003753145.png)

+ 选择器：浏览器根据 “选择器” 决定受 CSS 样式影响的 HTML 元素（标签）。
+ 属性 (property) 是你要改变的样式名，并且每个属性都有一个值。属性和值被冒号分开，并由花括号包围，这样就组成了一个完整的样式声明（declaration） ，例如：`p {color: blue}`
+ 多个声明：如果要定义不止一个声明，则需要用分号将每个声明分开。虽然最后一条声明的最后可以不加分号（但尽量在每条声明的末尾都加上分号）

例如：

```css
p {
    /* 这是注释 */
    color:red;
    font-size:30px;
}
```

## CSS与HTML的结合方式

需求：分别定义两个 div、span 标签，分别修改每个 div 标签的样式为：边框 1 个像素，实线，红色

### 直接在标签中设置

第一种：在标签的 style 属性上设置 `key:value value;`，修改标签样式。  

```html
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Title</title>
    </head>
    <body>
        <div style="border: 1px solid red;">div 标签 1</div>
        <div style="border: 1px solid red;">div 标签 2</div>
        <span style="border: 1px solid red;">span 标签 1</span>
        <span style="border: 1px solid red;">span 标签 2</span>
    </body>
</html>
```

缺点：

1. 如果标签多了、样式多了，代码量非常庞大。
2. 可读性非常差。
3. Css 代码没什么复用性可言  

### 在head标签中定义

第二种：在 head 标签中，使用 style 标签来定义各种自己需要的 css 样式。

```html
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Title</title>
        <!--style 标签专门用来定义 css 样式代码-->
        <style type="text/css">
            div{
            	border: 1px solid red;
            } 
            span{
            	border: 1px solid red;
            }
        </style>
    </head>
    <body>
        <div>div 标签 1</div>
        <div>div 标签 2</div>
        <span>span 标签 1</span>
        <span>span 标签 2</span>
    </body>
</html>
```

缺点：

1. 只能在同一页面内复用代码，不能在多个页面中复用 css 代码。
2. 维护起来不方便，实际的项目中会有成千上万的页面，要到每个页面中去修改，工作量太大了。

### 独立的css文件

第三种：把 css 样式写成一个单独的 css 文件，再通过 link 标签引入即可复用。

使用 html 的 `<link rel="stylesheet" type="text/css" href="./styles.css" />` 标签 导入 css 样式文件。

css 文件内容：

```css
div{
	border: 1px solid yellow;
}
span{
	border: 1px solid red;
}
```

html 文件代码：

```html
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Title</title>
        <!--link 标签专门用来引入 css 样式代码-->
        <link rel="stylesheet" type="text/css" href="1.css"/>
    </head>
    <body>
        <div>div 标签 1</div>
        <div>div 标签 2</div>
        <span>span 标签 1</span>
        <span>span 标签 2</span>
    </body>
</html>
```

## CSS选择器

### 标签名选择器

标签名选择器的格式是：

```css
标签名{
	属性: 值;
} 
```

标签名选择器，可以决定哪些标签被动的使用这个样式。

需求：在所有 div 标签上修改字体颜色为蓝色，字体大小 30 个像素，边框为 1 像素黄色实线。并且修改所有 span 标签的字体颜色为黄色，字体大小 20 个像素，边框为 5 像素蓝色虚线。

```html
<html>
    <head>
        <meta charset="UTF-8">
        <title>CSS 选择器</title>
        <style type="text/css">
            div{
                border: 1px solid yellow;
                color: blue;
                font-size: 30px;
            } 
            span{
                border: 5px dashed blue;
                color: yellow;
                font-size: 20px;
            }
        </style>
    </head>
    <body>
        <div>div 标签 1</div>
        <div>div 标签 2</div>
        <span>span 标签 1</span>
        <span>span 标签 2</span>
    </body>
</html>
```

### id选择器

id 选择器的格式是：

```css
#id属性值{
	属性: 值;
}
```

id选择器，可以让我们通过 id 属性选择性的去使用这个样式。

需求：分别定义两个 div 标签，第一个 div 标签定义 id 为 id001，然后根据 id 属性定义 css 样式修改字体颜色为蓝色，字体大小 30 个像素，边框为 1 像素黄色实线。第二个 div 标签定义 id 为 id002，然后根据 id 属性定义 css 样式 修改的字体颜色为红色，字体大小 20 个像素，边框为 5 像素蓝色点线。

```html
<html>
    <head>
        <meta charset="UTF-8">
        <title>ID 选择器</title>
        <style type="text/css">
            #id001{
                color: blue;
                font-size: 30px;
                border: 1px yellow solid;
            } 
            #id002{
                color: red;
                font-size: 20px;
                border: 5px blue dotted;
            }
        </style>
    </head>
    <body>
        <div id="id002">div 标签 1</div>
        <div id="id001">div 标签 2</div>
    </body>
</html>
```

### class选择器（类选择器）

class 类型选择器的格式是：

```css
.class 属性值{
	属性: 值;
}
```

class 类型选择器，可以通过 class 属性有效的选择性地去使用这个样式。

需求 1：修改 class 属性值为 class01 的 span 或 div 标签，字体颜色为蓝色，字体大小 30 个像素，边框为 1 像素黄色实线。
需求 2：修改 class 属性值为 class02 的 div 标签，字体颜色为灰色，字体大小 26 个像素，边框为 1 像素红色实线。

```html
<html>
    <head>
        <meta charset="UTF-8">
        <title>class 类型选择器</title>
        <style type="text/css">
        .class01{
            color: blue;
            font-size: 30px;
            border: 1px solid yellow;
        }
        .class02{
            color: grey;
            font-size: 26px;
            border: 1px solid red;
        }
        </style>
    </head>
    <body>
        <div class="class02">div 标签 class01</div>
        <div class="class02">div 标签</div>
        <span class="class02">span 标签 class01</span>
        <span>span 标签 2</span>
    </body>
</html>
```

### 组合选择器

组合选择器的格式是：

```css
选择器1, 选择器2, 选择器n{
	属性: 值;
}
```

组合选择器可以让多个选择器共用同一个 css 样式代码。

需求：修改 class="class01" 的 div 标签 和 id="id01" 所有的 span 标签，字体颜色为蓝色，字体大小 20 个像素，边框为 1 像素黄色实线。

```html
<html>
    <head>
        <meta charset="UTF-8">
        <title>class 类型选择器</title>
        <style type="text/css">
        .class01, #id01{
            color: blue;
            font-size: 20px;
            border: 1px yellow solid;
        }
        </style>
    </head>
    <body>
        <div id="id01">div 标签 class01</div> <br />
        <span >span 标签</span> <br />
        <div>div 标签</div> <br />
        <div>div 标签 id01</div> <br />
    </body>
</html>
```

# JavaScript

JavaScript 语言诞生主要是完成页面的数据验证。因此它运行在客户端，需要运行浏览器来解析执行 JavaScript 代码。

JS 是弱类型， Java 是强类型。

特点：

1. 交互性（它可以做的就是信息的动态交互）
2. 安全性（不允许直接访问本地硬盘）
3. 跨平台性（只要是可以解释 JS 的浏览器都可以执行，和平台无关）

## JavaScript和HTML的结合方式  

### 方式一

只需要在 head 标签中， 或者在 body 标签中， 使用 script 标签 来书写 JavaScript 代码  

```html
<html lang="en">
    <head>
        <meta charset="UTF-8"><title>Title</title>
        <script type="text/javascript">
            // alert 是 JavaScript 语言提供的一个警告框函数。
            // 它可以接收任意类型的参数， 这个参数就是警告框的提示信息
        	alert("hello javaScript!");
        </script>
    </head>
    <body>
    </body>
</html>
```

### 方式二

使用 script 标签引入 单独的 JavaScript 代码文件  

文件目录：

<img src="https://gitee.com/cmz2000/album/raw/master/image/image-20210806125906315.png" alt="image-20210806125906315" style="zoom:67%;" />

```html
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Title</title>
        <!--
            现在需要使用 script 引入外部的 js 文件来执行
            src 属性专门用来引入 js 文件路径（可以是相对路径，也可以是绝对路径）
            script 标签可以用来定义 js 代码，也可以用来引入 js 文件
            但是，两个功能二选一使用，不能同时使用两个功能。
			若要使用两个功能，可以在两个 script 标签中使用
        -->
        <script type="text/javascript" src="1.js"></script>
        <script type="text/javascript">
            alert("hello again");
        </script>
    </head>
    <body>
    </body>
</html>
```

## 变量

JavaScript 的变量类型：

|    类型    |  类型名  |
| :--------: | :------: |
|  数值类型  |  number  |
| 字符串类型 |  string  |
|  对象类型  |  object  |
|  布尔类型  | boolean  |
|  函数类型  | function |

JavaScript 里特殊的值：

|  特殊值   |                            含义                             |
| :-------: | :---------------------------------------------------------: |
| undefined | 未定义，所有 js 变量未赋于初始值的时候，默认值都是undefined |
|   null    |                            空值                             |
|    NaN    |            全称是 Not a Number。非数字，非数值。            |

JS 中的定义变量格式：

```javascript
var 变量名;
var 变量名 = 值;
```

代码示例

```html
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Title</title>
        <script type="text/javascript">
            var i;
            // alert(i); // undefined
            i = 12;
            // typeof()是 JavaScript 语言提供的一个函数。
            // alert( typeof(i) ); // number
            i = "abc";
            // 它可以取变量的数据类型返回
            // alert( typeof(i) ); // string
            var a = 12;
            var b = "abc";
            alert( a * b ); // NaN 是非数字， 非数值。
        </script>
    </head>
    <body>
    </body>
</html>
```

### 关系（比较）运算

等于：`==`，等于是简单的做字面值的比较

全等于：`===`，除了做字面值的比较之外，还会比较两个变量的数据类型

```html
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Title</title>
        <script type="text/javascript">
            var a = "12";
            var b = 12;
            alert( a == b ); // true
            alert( a === b ); // false
        </script>
    </head>
    <body>
    </body>
</html>
```

### 逻辑运算

且运算：`&&`

或运算：`||`

取反运算：`!`

在 JavaScript 语言中，所有的变量，都可以做为一个 boolean 类型的变量去使用。

`0`、`null`、`undefined`、`""`（空串）都认为是 false

```html
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Title</title>
        <script type="text/javascript">
            /* && 且运算。
                有两种情况：
                第一种：当表达式全为真的时候,返回最后一个表达式的值。
                第二种：当表达式中，有一个为假的时候,返回第一个为假的表达式的值
            */
            var a = "abc";
            var b = true;
            var d = false;
            var c = null;
            // alert( a && b );//true
            // alert( b && a );//true
            // alert( a && d ); // false
            // alert( a && c ); // null
            /* || 或运算
                第一种情况：当表达式全为假时，返回最后一个表达式的值
                第二种情况：只要有一个表达式为真，就会把回第一个为真的表达式的值
            */
            // alert( d || c ); // null
            // alert( c|| d ); //false
            // alert( a || c ); //abc
            // alert( b || c ); //true
        </script>
    </head>
    <body>
    </body>
</html>
```

注意：`&&`与运算 和 `||`或运算 有短路。短路就是说，当这个 `&&` 或者 `||`运算有结果了之后，后面的表达式不再执行。

## 数组

JS 中 数组的定义格式：

```javascript
var 数组名 = []; // 空数组
var 数组名 = [1 , ’abc’ , true]; // 定义数组同时赋值元素  
```

代码演示：

```html
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Title</title>
        <script type="text/javascript">
            var arr = []; // 定义一个空数组
            alert( arr.length ); // 0
            arr[0] = 12;
            alert( arr[0] );//12
            alert( arr.length ); // 1
            // javaScript 语言中的数组，只要我们通过数组下标赋值，那么最大的下标值，就会自动的给数组做扩容操作。
            arr[2] = "abc";
            alert(arr.length); //3
            alert(arr[1]);	// undefined
            // 数组的遍历
            for (var i = 0; i < arr.length; i++){
            	alert(arr[i]);
            }
        </script>
    </head>
    <body>
    </body>
</html>
```

## 函数

### 两种定义方式

#### function关键字

使用的格式如下：

```javascript
function 函数名(形参列表){
	函数体
} 
```

在 JavaScript 语言中，如何定义带有返回值的函数？

只需要在函数体内直接使用 return 语句返回值即可。

```html
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Title</title>
        <script type="text/javascript">
            // 定义一个无参函数
            function fun(){
            	alert("无参函数 fun()被调用了");
            } 
            // 函数调用，才会执行
            // fun();
            function fun2(a ,b) {
            	alert("有参函数 fun2()被调用了 a=>" + a + ",b=>"+b);
            } 
            // fun2(12,"abc");
            // 定义带有返回值的函数
            function sum(num1, num2) {
            	var result = num1 + num2;
            	return result;
            } 
            alert( sum(100,50) );
        </script>
    </head>
    <body>
    </body>
</html>
```

#### 第二种方式

```javascript
var 函数名 = function(形参列表) { 函数体 }
```

代码演示：

```html
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Title</title>
        <script type="text/javascript">
            var fun = function () {
            	alert("无参函数");
            } 
            // fun();
            var fun2 = function (a,b) {
            	alert("有参函数 a=" + a + ",b=" + b);
            } 
            // fun2(1,2);
            var fun3 = function (num1,num2) {
            	return num1 + num2;
            }
            alert( fun3(100,200) );
        </script>
    </head>
    <body>
    </body>
</html>
```

==注：在 Java 中函数允许重载，但是在 JS 中函数的重载会直接覆盖掉上一次的定义，即不允许重载。==

代码演示：

```html
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Title</title>
        <script type="text/javascript">
            function fun() {
            	alert("无参函数 fun()");
            } 
            function fun(a,b) {
            	alert("有参函数 fun(a,b)");
            }
            fun();
        </script>
    </head>
    <body>
    </body>
</html>
```

运行结果如下：

![image-20210806133830646](https://gitee.com/cmz2000/album/raw/master/image/image-20210806133830646.png)

### 函数的隐形参数  

隐形参数：就是在 function 函数中不需要定义，但却可以直接用来获取所有参数的变量，我们管它叫隐形参数。

隐形参数特别像 java 的可变长参数：

```java
public void fun( Object... args );
public void fun( Object[] args );	// 可变长参数其实是一个数组
```

那么 js 中的隐形参数也跟 java 的可变长参数一样，操作类似数组。  

代码演示

```html
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Title</title>
        <script type="text/javascript">
            function fun(a) {
                alert( arguments.length );//可看参数个数
                alert( arguments[0] );
                alert( arguments[1] );
                alert( arguments[2] );
                alert("a = " + a);
                for (var i = 0; i < arguments.length; i++){
                	alert( arguments[i] );
            	} 
                alert("无参函数 fun()");
            } 
            // fun(1,"ad",true);
            // 需求: 编写一个函数，用于计算所有参数相加的和并返回
            function sum(num1, num2) {
                var result = 0;
                for (var i = 0; i < arguments.length; i++) {
                    if (typeof(arguments[i]) == "number") {
                    	result += arguments[i];
                    }
            	} 
                return result;
            } 
            alert( sum(1,2,3,4,"abc",5,6,7,8,9) );
        </script>
    </head>
    <body>
    </body>
</html>
```

## 自定义对象

### Object形式

```javascript
// 对象的定义
var 变量名 = new Object(); // 对象实例（空对象）
变量名.属性名 = 值; 		// 定义一个属性
变量名.函数名 = function(){} // 定义一个函数
//对象的访问
变量名.属性;
变量名.函数名();
```

代码演示：

```html
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Title</title>
        <script type="text/javascript">
            var obj = new Object();
            obj.name = "华仔";
            obj.age = 18;
            obj.fun = function () {
            	alert("姓名： " + this.name + " , 年龄： " + this.age);
            } 
            // alert( obj.age );
            obj.fun();
        </script>
    </head>
    <body>
    </body>
</html>
```

### {}花括号形式

```javascript
// 对象的定义
var 变量名 = { // 空对象
    属性名: 值, // 定义一个属性
    属性名: 值, // 定义一个属性
    函数名: function(){} // 定义一个函数
};
// 对象的访问
变量名.属性;
变量名.函数名();
```

代码演示：

```html
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Title</title>
        <script type="text/javascript">
            var obj = {
                name:"华仔",
                age:18,
                fun : function () {
                	alert("姓名： " + this.name + " , 年龄： " + this.age);
                }
            };
            alert(obj.name);
            obj.fun();
        </script>
    </head>
    <body>
    </body>
</html>
```

## js中的事件

什么是事件？ 事件是电脑输入设备与页面进行交互的响应，我们称之为事件。

### 常用的事件

+ onload 加载完成事件：页面加载完成之后，常用于做页面 js 代码初始化操作
+ onclick 单击事件：常用于按钮的点击响应操作。
+ onblur 失去焦点事件：常用用于输入框失去焦点后验证其输入内容是否合法。
+ onchange 内容发生改变事件：常用于下拉列表和输入框内容发生改变后操作
+ onsubmit表单提交事件：常用于表单提交前，验证所有表单项是否合法。

### 事件的注册

什么是事件的注册（绑定）？

其实就是告诉浏览器， 当事件响应后要执行哪些操作代码， 叫事件注册或事件绑定。  

+ 静态注册事件：通过 html 标签的事件属性直接赋于事件响应后的代码，这种方式我们叫静态注册。

+ 动态注册事件：是指先通过 js 代码得到标签的 dom 对象，然后再通过 dom 对象.事件名 = function(){} 这种形式赋于事件响应后的代码，叫动态注册。

动态注册基本步骤：

1. 获取标签对象
2. 标签对象.事件名 = fucntion(){}  

### onload

加载完成事件

```html
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Title</title>
        <script type="text/javascript">
            // onload 事件的方法
            function onloadFun() {
            	alert('静态注册 onload 事件，所有代码');
            }
            // onload 事件动态注册。是固定写法
            window.onload = function () {
            	alert("动态注册的 onload 事件");
            }
        </script>
    </head>
    <!--静态注册 onload 事件
        onload 事件是浏览器解析完页面之后就会自动触发的事件
        <body onload="onloadFun();">
    -->
    <body>
    </body>
</html>
```

### onclick

单击事件

```html
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Title</title>
        <script type="text/javascript">
            function onclickFun() {
            	alert("静态注册 onclick 事件");
            }
            // 动态注册 onclick 事件
            window.onload = function () {
            // 1 获取标签对象
            /*
            * document 是 JavaScript 语言提供的一个对象（文档）
            * getElementById: 通过 id 属性获取标签对象
            **/
            var btnObj = document.getElementById("btn01");
                // alert( btnObj );
                // 2 通过标签对象.事件名 = function(){}
                btnObj.onclick = function () {
                    alert("动态注册的 onclick 事件");
                }
            }
        </script>
    </head>
    <body>
        <!--静态注册 onClick 事件-->
        <button onclick="onclickFun();">按钮 1</button>
        <button id="btn01">按钮 2</button>
    </body>
</html>
```

### onblur

失去焦点事件

```html
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Title</title>
        <script type="text/javascript">
            // 静态注册失去焦点事件
            function onblurFun() {
                // console 是控制台对象，是由 JavaScript 语言提供，专门用来向浏览器的控制器打印输出，用于测试使用
                // log() 是打印的方法
            	console.log("静态注册失去焦点事件");
            }
            // 动态注册 onblur 事件
            window.onload = function () {
            	// 1、获取标签对象
            	var passwordObj = document.getElementById("password");
                // alert(passwordObj);
                // 2、通过标签对象.事件名 = function(){};
                passwordObj.onblur = function () {
                	console.log("动态注册失去焦点事件");
                }
            }
        </script>
    </head>
    <body>
        用户名:<input type="text" onblur="onblurFun();"><br/>
        密码:<input id="password" type="text" ><br/>
    </body>
</html>
```

### onchange

内容发生改变事件

```html
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Title</title>
        <script type="text/javascript">
        function onchangeFun() {
        	alert("女神已经改变了");
        } 
        window.onload = function () {
        	//1 获取标签对象
        	var selObj = document.getElementById("sel01");
            // alert( selObj );
            //2 通过标签对象.事件名 = function(){}
            selObj.onchange = function () {
                alert("男神已经改变了");
            }
        }
        </script>
    </head>
    <body>
        请选择你心中的女神：
        <!--静态注册 onchange 事件-->
        <select onchange="onchangeFun();">
            <option>--女神--</option>
            <option>芳芳</option>
            <option>佳佳</option>
            <option>娘娘</option>
        </select>
        请选择你心中的男神：
        <select id="sel01">
            <option>--男神--</option>
            <option>国哥</option>
            <option>华仔</option>
            <option>富城</option>
        </select>
    </body>
</html>
```

### onsubmit

表单提交事件

```html
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Title</title>
        <script type="text/javascript" >
            // 静态注册表单提交事务
            function onsubmitFun(){
                // 要验证所有表单项是否合法， 如果， 有一个不合法就阻止表单提交
                alert("静态注册表单提交事件----发现不合法");
                return flase;
            }
            // 动态注册表单提交事务
            window.onload = function () {
                //1 获取标签对象
                var formObj = document.getElementById("form01");
                //2 通过标签对象.事件名 = function(){}
                formObj.onsubmit = function () {
                    // 要验证所有表单项是否合法， 如果， 有一个不合法就阻止表单提交
                    alert("动态注册表单提交事件----发现不合法");
                    return false;
                }
            }
        </script>
    </head>
    <body>
        <!--return false 可以阻止表单提交 -->
        <form action="http://localhost:8080" method="get" onsubmit="return onsubmitFun();">
        	<input type="submit" value="静态注册"/>
        </form>
        <form action="http://localhost:8080" id="form01">
        	<input type="submit" value="动态注册"/>
        </form>
    </body>
</html>
```

## DOM模型

DOM 全称是 Document Object Model 文档对象模型。

就是把文档中的标签，属性，文本 转换成为对象来管理。

![image-20210806220151702](https://gitee.com/cmz2000/album/raw/master/image/image-20210806220151702.png)

+ Document 它管理了所有的 HTML 文档内容。
+ document 它是一种树结构的文档，有层级关系。
+ 它让我们把所有的标签都对象化
+ 我们可以通过 document 访问所有的标签对象。  

document 对象中常用方法：

```javascript
// 通过标签的 id 属性查找标签 dom 对象，elementId 是标签的 id 属性值
document.getElementById(elementId);
// 通过标签的 name 属性查找标签 dom 对象，elementName 标签的 name 属性值
document.getElementsByName(elementName);
// 通过标签名查找标签 dom 对象。tagname 是标签名
document.getElementsByTagName(tagName);
// 通过给定的标签名，创建一个标签对象。tagName 是要创建的标签名
document.createElement(tagName);
```

### 节点

节点就是标签对象

#### 方法

通过具体的元素节点调用 `getElementsByTagName()` 方法，获取当前节点的指定标签名孩子节点

`appendChild(ChildNode)` 方法，可以添加一个子节点，ChildNode 是要添加的孩子节点  

#### 属性

+ childNodes属性， 获取当前节点的所有子节点
+ firstChild属性， 获取当前节点的第一个子节点
+ lastChild属性， 获取当前节点的最后一个子节点
+ parentNode属性， 获取当前节点的父节点
+ nextSibling属性， 获取当前节点的下一个节点
+ previousSibling属性， 获取当前节点的上一个节点
+ className用于获取或设置标签的 class 属性值
+ innerHTML属性， 表示获取/设置起始标签和结束标签中的内容
+ innerText属性， 表示获取/设置起始标签和结束标签中的文本  

## JQuery

见 pdf 笔记

# Tomcat

## JavaWeb的概念

JavaWeb 是指所有通过 Java 语言编写的、可以通过浏览器来访问的程序的总称，叫 JavaWeb。

JavaWeb 是基于请求和响应来开发的：

+ 请求是指客户端给服务器发送数据，叫请求 Request
+ 响应是指服务器给客户端回传数据，叫响应 Response
+ 请求和响应是成对出现的，有请求就有响应。

<img src="https://gitee.com/cmz2000/album/raw/master/image/image-20210810010637199.png" alt="image-20210810010637199" style="zoom: 80%;" />

## Web资源的分类

web 资源按实现的技术和呈现的效果的不同，又分为静态资源和动态资源两种。

+ 静态资源：html、css、js、txt、mp4 视频、jpg 图片
+ 动态资源：jsp 页面、Servlet 程序  

## 常用的Web服务器

+ Tomcat：由 Apache 组织提供的一种 Web 服务器， 提供对 jsp 和 Servlet 的支持。 它是一种轻量级的 javaWeb 容器（服务器） ， 也是当前应用最广的 JavaWeb 服务器（免费） 。
+ Jboss：是一个遵从 JavaEE 规范的、 开放源代码的、 纯 Java 的 EJB 服务器， 它支持所有的 JavaEE 规范（免费） 。
+ GlassFish：由 Oracle 公司开发的一款 JavaWeb 服务器， 是一款强健的商业服务器， 达到产品级质量（应用很少） 。
+ Resin：是 CAUCHO 公司的产品， 是一个非常流行的服务器， 对 servlet 和 JSP 提供了良好的支持，性能也比较优良， resin 自身采用 JAVA 语言开发（收费， 应用比较多） 。
+ WebLogic：是 Oracle 公司的产品， 是目前应用最广泛的 Web 服务器， 支持 JavaEE 规范，而且不断的完善以适应新的开发要求， 适合大型项目（收费， 用的不多， 适合大公司） 。  

## Tomcat的使用

### 目录介绍

![image-20210810135433016](https://gitee.com/cmz2000/album/raw/master/image/image-20210810135433016.png)

# Servlet

## 概念和技术

### 什么是Servlet

1. Servlet 是 JavaEE 规范之一，规范就是接口。也就是说 Servlet 是 Interface。
2. Servlet 就 JavaWeb 三大组件之一。 三大组件分别是： Servlet 程序、 Filter 过滤器、 Listener 监听器。
3. Servlet 是运行在服务器上的一个 java 小程序， ==它可以接收客户端发送过来的请求， 并响应数据给客户端==。  

### 手动实现Servlet程序  

1. 编写一个类去实现 Servlet 接口
2. 实现 service 方法，处理请求，并响应数据
3. 到 web.xml 中去配置 servlet 程序的访问地址  

代码示例：

```java
public class HelloServlet implements Servlet {
    /**
    * service 方法是专门用来处理请求和响应的
    * @param servletRequest
    * @param servletResponse
    * @throws ServletException
    * @throws IOException
    */
    @Override
    public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {
        System.out.println("Hello Servlet 被访问了");
    }
}
```

### 访问Servlet程序

![image-20210811004210868](https://gitee.com/cmz2000/album/raw/master/image/image-20210811004210868.png)

### Servlet的生命周期

1. 执行 Servlet 构造器方法
2. 执行 init 初始化方法（第一、二步，是在第一次访问的时候创建 Servlet 程序会调用）。
3. 执行 service 方法（第三步，每次访问都会调用）。
4. 执行 destroy 销毁方法（第四步，在 web 工程停止的时候调用）。  

```java
public class Hello implements Servlet {
    public Hello() {
        System.out.println("构造器方法");
    }

    @Override
    public void init(ServletConfig servletConfig) throws ServletException {
        System.out.println("init方法");
    }

    @Override
    public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {
        System.out.println("Hello被访问了");
        HttpServletRequest httpServletRequest = (HttpServletRequest) servletRequest;
        String method = httpServletRequest.getMethod();
        if ("GET".equals(method)) {
            doGet();
        }
        else if ("POST".equals(method)){
            doPost();
        }
    }

    public void doGet() {
        System.out.println("GET方法");
    }

    public void doPost() {
        System.out.println("POST方法");
    }

    @Override
    public void destroy() {
        System.out.println("销毁方法");
    }
}
```

### HttpServlet

一般在实际项目开发中， 都是使用继承 HttpServlet 类的方式去实现 Servlet 程序。

1. 编写一个类去继承 HttpServlet 类
2. 根据业务需要重写 doGet 或 doPost 方法
3. 到 web.xml 中的配置 Servlet 程序的访问地址  

代码演示：

```java
public class Hello2 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("doGet方法");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("doPost方法");
    }
}
```

### Servlet类的继承体系

![image-20210811150336128](https://gitee.com/cmz2000/album/raw/master/image/image-20210811150336128.png)

## ServletConfig类

ServletConfig 类从类名上来看， 就知道是 Servlet 程序的配置信息类。

Servlet 程序和 ServletConfig 对象都是由 Tomcat 负责创建， 我们负责使用。

Servlet 程序默认是第一次访问的时候创建， ServletConfig 是每个 Servlet 程序创建时， 就创建一个对应的 ServletConfig 对象。

### 作用

ServletConfig 类有三大作用：

1. 可以获取 Servlet 程序的别名 servlet-name 的值
2. 获取初始化参数 init-param
3. 获取 ServletContext 对象

## ServletContext类

### 概念

1. ServletContext 是一个接口， 它表示 Servlet 上下文对象
2. 一个 web 工程， 只有一个 ServletContext 对象实例。
3. ServletContext 对象是一个域对象。
4. ServletContext 是在 web 工程部署启动的时候创建。 在 web 工程停止的时候销毁。

什么是域对象?

域对象，是可以像 Map 一样存取数据的对象，叫域对象。这里的域指的是存取数据的操作范围，整个 web 工程。

|            | 存数据           | 取数据           | 删除数据            |
| ---------- | ---------------- | ---------------- | ------------------- |
| **Map**    | `put()`          | `get()`          | `remove()`          |
| **域对象** | `setAttribute()` | `getAttribute()` | `removeAttribute()` |

### 作用

1. 获取 web.xml 中配置的上下文参数 context-param
2. 获取当前的工程路径， 格式: /工程路径
3. 获取工程部署后在服务器硬盘上的绝对路径
4. 像 Map 一样存取数据  

## HttpServletRequest类

### 作用

每次只要有请求进入 Tomcat 服务器， Tomcat 服务器就会把请求过来的 HTTP 协议信息解析好封装到 Request 对象中。然后传递到 service 方法（ doGet 和 doPost） 中给我们使用。 我们可以通过 HttpServletRequest 对象， 获取到所有请求的信息。  

### 常用方法

![image-20210812203149877](https://gitee.com/cmz2000/album/raw/master/image/image-20210812203149877.png)

### 请求转发

查看 pdf 笔记

## HttpServletResponse类