# HTML5



## 1、什么是HTML？

**HTML（Hyper Text Markup Language）：超文本标记语言**

超文本包括：文字、图片、音频、视频、动画等。



### 1.1、W3C标准

#### 1.1.1、W3C

- **World Wide Web Consortium （万维网联盟）**
- 成立于1994年，Web技术领域最具权威和影响力的**国际中立性技术标准机构**
- http://www.w3.org/
- http://wwwchinaw3c.org/

#### 1.1.2、W3C标准包括

- **结构化标准语言（HTML、XML）**
- **表现标准语言（CSS）**
- **行为标准（DOM、ECMAScript）**



### 1.2、HTML基本结构

![img](https://gitee.com/xudongyin/img/raw/master/img/20200719183411.png)

、等成对出现的标签，分别叫做开放标签和闭合标签，

单独呈现的标签（空元素），如

------

,意为用/来关闭空元素





### 1.3、HTML基本信息

- DOCTYPE声明：告诉浏览器我们使用什么规范
- title标签
- meta标签

```html
<!--告诉浏览器我们使用什么规范-->
<!DOCTYPE html>
<html lang="en">
    <!--head标签代表网页头部-->
    <head>
        <!--meta描述性标签,用来描述网页的基本信息,一般用来做SEO-->
        <meta charset="UTF-8">
        <meta name="keyword" content="第一次写HTML">
        <meta name="description" content="学习">
        <!--title网页标题-->
        <title>我的第一个HTML</title>
    </head>

    <!--body标签代表网页主体-->
    <body>
        我的第一个网页
    </body>
</html>
```



## 2、网页基本标签



### 2.1、标题标签

```html
  <!--标题标签-->
    <h1>一级标签</h1>
    <h2>二级标签</h2>
    <h3>三级标签</h3>
    <h4>四级标签</h4>
    <h5>五级标签</h5>
    <h6>六级标签</h6>
```



### 2.2、段落标签

```html
 <!--段落标签-->
    <p>两只老虎，两只老虎，</p>
    <p>跑得快，跑得快，</p>
    <p>一只没有眼睛，</p>
    <p>一只没有尾巴，</p>
    <p>真奇怪！真奇怪！。</p>
```



### 2.3、换行标签

```html
 <!--换行标签-->
    两只老虎，两只老虎，<br/>
    跑得快，跑得快，<br/>
    一只没有眼睛，<br/>
    一只没有尾巴，<br/>
    真奇怪！真奇怪！。<br/>
```



### 2.4、水平线标签

```html
  <!--水平线标签-->
    <hr/>
```



### 2.5、字体样式标签

```html
<!--粗体斜体-->
    <h1>字体标签</h1>
        粗体：<strong>I Love You</strong>
        斜体：<em>I Love You</em>
```



### 2.6、注释和特殊符号

```html
<!--特殊符号-->
    <!--特殊符号记忆方式： &开头，;结尾-->
    空格：&nbsp;<br/>
    大于：&gt;<br/>
    小于：&lt;<br/>
    版权：&copy;<br/>
```



## 3、图像标签

![img](https://gitee.com/xudongyin/img/raw/master/img/20200719183535.png)

**src：图片地址（必填）**

 **相对路径:../表示上一级目录**

​	**绝对路径**

**alt：图片加载失败时显示的文字（必填）**

**title：鼠标悬停文字**

**width="1920" height="1024"：图片的大小**

```html
<img src="../resources/img/1.jpg" alt="背景" title="悬停文字" width="1920" height="1024">
```



## 4、链接标签

**href：连接地址，跳转的地址（必填）**

**target：表示窗口在哪打开**

 **_blank：在新标签中打开网页**

 **_self：在自己的网页中打开**

```html
<a href="我的第一个网页.html" target="_blank">点击我跳转到我的第一个网页</a>
<a href="http://www.baidu.com/" target="_parent">点击我跳转到百度</a>

<br/>
<p><a href="我的第一个网页.html">
    <img src="../resources/img/1.jpg" alt="点击我跳转到我的第一个网页" title="点击我跳转到我的第一个网页" width="500" height="300">
</a></p>
```

**锚链接**

 1.需要一个锚标记

 2.跳转到标记

```html
<!--使用name作为标记-->
<a name="top">顶部</a>
<a href="#down">回到顶部</a>
···
···
···

<a href="#top">回到顶部</a>

<a name="down">底部</a>
```

**功能性链接**

 邮件链接：mailto：邮箱

 QQ链接：百度QQ推广获取

```html
<a href="mailto:935074243@qq.com">点击</a>

<a target="_blank" href="http://wpa.qq.com/msgrd?v=3&uin=935074243&site=qq&menu=yes">
    <img border="0" src="http://wpa.qq.com/pa?p=2:935074243:53" alt="点击这里给我发消息" title="点击这里给我发消息"/>
</a>
```



## 5、块元素和行内元素

**块元素：**无论内容多少，该元素独占一行，如：p、h1-h6

**行内元素：**内容撑开度，左右都是行内元素的可以排在一行，如：a.strong.em



## 6、列表标签

### 6.1、有序列表

 运用范围：考试、应答

```html
<ol>
    <li>Java</li>
    <li>Python</li>
    <li>C++</li>
    <li>运维</li>
</ol>
```



### 6.2、无序列表

 运用范围：导航、侧边栏

```html
<ul>
    <li>Java</li>
    <li>Python</li>
    <li>C++</li>
    <li>运维</li>
</ul>
```



### 6.3、自定义列表

 dl：标签

 dt：列表名称

 dd：列表内容

 运用范围：网站底部

```html
<dl>
    <dt>科目</dt>
    <dd>Java</dd>
    <dd>Python</dd>
    <dd>C++</dd>
    <dd>运维</dd>

    <dt>位置</dt>
    <dd>西安</dd>
    <dd>甘肃</dd>
    <dd>新疆</dd>
    <dd>宁夏</dd>
</dl>
```



## 7、表格标签

行：tr

列：td

border：分割线

```html
<table border="1px">
    <tr>
        <!--colspan="4":跨列-->
        <td colspan="4">1-1</td>
    </tr>
    <tr>
        <!--rowspan="2"：跨行-->
        <td rowspan="2">2-1</td>
        <td>2-2</td>
        <td>2-3</td>
        <td>2-4</td>
    </tr>
    <tr>
        <td>3-1</td>
        <td>3-2</td>
        <td>3-3</td>
    </tr>

</table>
```



## 8、媒体元素

**src：资源路径**

**controls：控制器，有它才能看到**

**autoplay：自动播放**



### 8.1、视频标签video

```html
<video src="../resources/video/【狂神说Java】HTML09：媒体元素.mp4" controls autoplay></video>
```



### 8.2、音频标签audio

```html
<audio src="../resources/audio/HTML09：媒体元素.m4a" controls autoplay></audio>
```



## 9、页面结构分析

![img](https://gitee.com/xudongyin/img/raw/master/img/20200719183515.png)

```html
<header>
    <h2>网页头部</h2>
</header>

<section>
    <h2>网页主体</h2>
</section>

<footer>
    <h2>网页脚部</h2>
</footer>
```



## 10、iframe内联框架

![img](https://gitee.com/xudongyin/img/raw/master/img/20200719183548.png)

**src：地址**

**width、height:宽度高度**

**name：框架标示名**

```html
<iframe src="" name="hello" width="1000px" height="500px"></iframe>

<a href="https://space.bilibili.com/95256449/" target="hello">点击跳转</a>
```



## 11、表单

![img](https://gitee.com/xudongyin/img/raw/master/img/20200719183543.png)

**action：表单提交的位置，可以是网站，可以是一个请求处理地址**

**method：post，get 提交方式**

​	**get方式提交可以在URL中看到我们提交的信息，不安全，但高效**

​	**post方式提交，比较安全，传输大文件**



### 11.1、表单元素格式

![img](https://gitee.com/xudongyin/img/raw/master/img/20200719183559.png)

```html
<h1>注册</h1>

<form action="我的第一个网页.html" method="get">
    <!--文本输入框：input type="text"
                    value="":默认初始值
                    maxlength="8"：最长能写几个字符
                    size="30"：文本框长度-->
    <p>名字：<input type="text" name="username"></p>

    <!--密码框：input type="password"-->
    <p>密码：<input type="password" name="password"></p>

    <!--单选框标签：input type="radio"
            value：单选框的值
            name：表示组
            checked：默认选中-->
    <p>性别：
        <input type="radio" value="boy" name="sex"/>男
        <input type="radio" value="girl" name="sex"/>女
    </p>

    <!--多选框：input type="checkbox-->
    <p>爱好：
        <input type="checkbox" value="sleep" name="hobby">睡觉
        <input type="checkbox" value="code" name="hobby">敲代码
        <input type="checkbox" value="chat" name="hobby">聊天
        <input type="checkbox" value="game" name="hobby">游戏
    </p>

    <!--按钮
    input type="button"：普通按钮
    input type="image"：图片按钮
    input type="submit"：提交按钮
    input type="reset"：重置按钮
    -->
    <input type="button" name="btn1" value="点击边长">
    <!--<input type="image" src="../resources/img/1.jpg">-->

    <!--下拉框、列表框-->
    <p>国家：
        <select name="列表名称">
            <option value="china">中国</option>
            <option value="us">美国</option>
            <option value="yd">印度</option>
            <option value="rd" selected>瑞士</option>
        </select>
    </p>

    <!--文件域：input type="file" name="files"-->
    <p>
        <input type="file" name="files">
        <input type="button" value="upload">
    </p>

    <!--文本域：textarea cols="50" rows="10"
    cols="50"：行 
    rows="10"：列
    -->
    <p>
        <textarea name="textarea" id="" cols="50" rows="10">文本内容</textarea>
    </p>

    <!--邮件验证：input type="email"-->
    <p>邮箱：
        <input type="email" name="email">
    </p>

    <!--url验证：input type="url"-->
    <p>url：
        <input type="url" name="url">
    </p>

    <!--数字验证：input type="number"-->
    <p>number：
        <input type="number" name="number" max="9" min="0" step="2">
    </p>

    <!--滑块：input type="range" min="0" max="100" step="2"-->
    <p>音量：
        <input type="range" min="0" max="100" step="2" name="voice">
    </p>

    <!--搜索框：input type="search"-->
    <p>搜索：
        <input type="search" name="search">
    </p>

    <p>
        <input type="submit">
        <input type="reset">
    </p>
</form>
```

**隐藏域：hidden**

**只读：readonly**

**禁用：disable**



### 11.2、表单的初级验证

placeholder：提示信息

required：非空判断

pattern：正则表达式

正则表达式速查表：https://www.jb51.net/tools/regexsc.htm

```html
<!--自定义邮箱-->
<p>自定义邮箱：
    <input type="text" name="diymail" pattern="^\w+([-+.]\w+)*@\w+([-.]\w+)*\.\w+([-.]\w+)*$">
</p>
```



## 12、总结

![img](https://gitee.com/xudongyin/img/raw/master/img/20200719183612.png)