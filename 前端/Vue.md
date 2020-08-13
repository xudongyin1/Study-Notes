## 1、Vue:概述

Vue 是一套用于构建用户界面的**渐进式JavaScript框架**，开发商：尤雨溪，发布于 2014 年 2 月。与其它大型框架不同的是，Vue 被设计为可以自底向上逐层应用。**Vue的核心库只关注视图层**，不仅易于上手，还便于与第三方库（如：`vue-router：跳转`，`vue-resource：通信`，`vuex：管理`）或既有项目整合。

官网：https://cn.vuejs.org/v2/guide/

 

## 2、前端知识体系

### 2.1、前端三要素

- HTML（结构）：超文本标记语言（Hyper Text Markup Language），决定网页的结构和内容
- CSS（表现）：层叠样式表（Cascading Style Sheets），设定网页的表现样式
- JavaScript（行为）：是一种弱类型脚本语言，其源代码不需要经过编译，而是由浏览器解释运行，用于控制网页的行为

### 2.2、结构层(HTML)

### 2.3、表现层(CSS)

CSS层叠样式表是一门标记语言，并不是编程语言，因此不可以自定义变量，不可以引用等，换句话说就是不具备任何语法支持，它主要缺陷如下:

- 语法不够强大，比如无法嵌套书写，导致模块化开发中需要书写很多重复的选择器;
- 没有变量和合理的样式复用机制，使得逻辑上相关的属性值必须以字面量的形式重复输出，导致难以维护;

这就导致了我们在工作中无端增加了许多工作量。为了解决这个问题，前端开发人员会使用一种称之为 **[ CSS 预处理器]** 的工具，提供CSS缺失的样式层复用机制、减少冗余代码，提高样式代码的可维护性。大大提高了前端在样式.上的开发效率。

#### 什么是CSS预处理器

CSS预处理器定义了一种新的语言，其基本思想是，用一种专门的编程语言，为CSS增加了一些编程的特性，将CSS作为目标生成文件，然后开发者就只要使用这种语言进行CSS的编码工作。转化成通俗易懂的话来说就是“**用一种专门的编程语言，进行Web页面样式设计，再通过编译器转化为正常的CSS文件,以供项目使用**”。

**常用的 CSS 预处理器有**

- SASS：基于Ruby，通过服务端处理，功能强大。解析效率稿。需要学习 Ruby 语言，上手难度高于LESS。
- LESS：基于 Node.JS，通过客户端处理，使用简单。功能比 SASS 简单，解析效率也低于 SASS，但在实际开发中足够了，所以后台人员如果需要的话，建议使用 LESS。

 

### 2.4、行为层（JavaScript）

JavaScript一门弱类型脚本语言，其源代码在发往客户端运行之前不需经过编译，而是将文本格 式的字符代码发送给浏览器由浏览器解释运行。

**Native原生JS开发**

原生JS开发，也就是让我们按照【**ECMAScript**】标准的开发方式，简称是ES,特点是所有浏览器都支持。截止到当前博客发布时间，ES标准已发布如下版本:

- ES3

- ES4 (内部,未正式发布)

- ES5 (全浏览器支持)

- ES6 (常用，当前主流版本: webpack打包成为ES5支持! )

- ES7

- ES8

- ES9 (草案阶段)

  区别就是逐步增加新特性。

**TypeScript微软的标准**

TypeScript是一种由微软开发的自由和开源的编程语言。它是JavaScript的一个超集，而且本质上向这个语言添加了可选的静态类型和基于类的面向对象编程。由安德斯海尔斯伯格（C#、Delphi、TypeScript 之父； .NET 创立者）主导。

该语言的特点就是除了具备 ES 的特性之外还纳入了许多不在标准范围内的新特性，所以会导致很多浏览器不能直接支持 TypeScript 语法，需要编译后（编译成 JS ）才能被浏览器正确执行。

#### JavaScript 框架

- **jQuery**：大家熟知的 JavaScript 框架，优点是简化了DOM操作，缺点是DOM操作太频繁，影响前端性能；在前端眼里使用它仅仅是为了兼容 IE6、7、8;
- **Angular**：Google 收购的前端框架，由一群Java程序员开发，其特点是将后台的MVC模式搬到了前端并增加了**模块化开发**的理念，与微软合作，采用 TypeScript 语法开发；对后台程序员友好，对前端程序员不太友好；最大的缺点是版本迭代不合理（如: 1代 -> 2代，除了名字，基本就是两个东西)
- **React**：Facebook 出品，一款高性能的 JS 前端框架；特点是提出了新概念【**虚拟DOM**】用于减少真实 DOM 操作，在内存中模拟 DOM 操作，有效的提升了前端渲染效率；缺点是使用复杂，因为需要额外学习一门【**JSX**】 语言；
- **Vue**：一款渐进式 JavaScript 框架，所谓渐进式就是逐步实现新特性的意思，如实现模块化开发、路由、状态管理等新特性。**其特点是综合了 Angular（模块化）和 React（虚拟DOM）的优点;**
- **Axios**：前端通信框架；因为 Vue 的边界很明确，就是为了处理 DOM，所以并不具备通信能力，此时就需要额外使用一个通信框架与服务器交互；当然也可以直接选择使用 jQuery 提供的 AJAX 通信功能;

#### UI框架

- Ant-Design：阿里巴巴出品，基于 React 的 UI 框架
- ElementUI、iview、 ice：饿了么出品，基于 Vue 的 UI 框架
- Bootstrap：Twitter 推出的一个用于前端开发的开源工具包
- AmazeUI：又叫“妹子 UI"，一款HTML5跨屏前端框架

#### JavaScript 构建工具

- Babel：JS 编译工具，主要用于浏览器不支持的ES新特性，比如用于编译 TypeScript

- WebPack：模块打包器，主要作用是打包、压缩、合并及按序加载

  **注:以上知识点已将WebApp开发所需技能全部梳理完毕**

 

### 2.5、三端统一

##### 2.5.1、混合开发（Hybrid App）

主要目的是实现一套代码三端统一（PC、Android：.apk、iOS：.ipa）并能够调用到设备底层硬件（如：传感器、GPS、摄像头等），打包方式主要有以下两种：

- 云打包：HBuild -> HBuildX，DCloud 出品；API Cloud
- 本地打包：Cordova（前身是 PhoneGap）

##### 2.5.2、微信小程序

详见微信官网，这里介绍一个方便微信小程序 UI 开发的框架：WeUI

### 2.6、后端技术

前端人员为了方便开发也需要掌握一定的后端技术，但我们 Java 后台人员知道后台知识体系极其庞大复杂,所以为了方便前端人员开发后台应用，就出现了 NodeJS 这样的技术。

NodeJS的作者已经声称放弃 NodeJS （说是架构做的不好再加上笨重的 node_ modules，可能让作者不爽了吧） , 开始开发全新架构的 Deno

既然是后台技术，那肯定也需要框架和项目管理工具，NodeJS 框架及项目管理工具如下：

- Express：NodeJS 框架
- Koa：Express 简化版
- NPM：项目综合管理工具，类似于 Maven
- YARM：NPM 的替代方案，类似于 Maven 和 Gradle 的关系

 

### 2.7、主流前端框架

**Vue.js**

#### 2.7.1、iView

iview 是一个强大的基于 Vue 的 UI 库，有很多实用的基础组件比 elementui 的组件更丰富，主要服务于 PC 界面的中后台产品。使用单文件的 Vue 组件化开发模式基于 npm + webpack + babel 开发，支持 ES2015 高质量、功能丰富友好的 API，自由灵活地使用空间。

- [官网地址](https://iviewui.com/)
- [Github](https://github.com/view-design/ViewUI)
- [iview-admin](https://gitee.com/icarusion/iview-admin)

**备注：属于前端主流框架，选型时可以考虑使用，主要特点是移动端支持较多**

 

#### 2.7.2、 ElementUI

Element 是饿了么前端开源维护的 Vue UI 组件库，组件齐全，基本涵盖后台所需的所有组件，文档讲解详细，例子也很丰富。主要用于开发 PC 端的页面，是一个质量比较高的 Vue UI 组件库。

- [官网地址](https://element.eleme.cn/#/zh-CN)
- Github
- [vue-element-admin](https://github.com/PanJiaChen/vue-element-admin)

**备注：属于前端主流框架，选型时可以考虑使用，主要特点是桌面端支持较多**

 

#### 2.7.3、 ICE

飞冰 是阿里巴巴团队基于 React/Angular/Vue 的中后台应用解决方案，在阿里巴巴内部，已经有270多个来自几乎所有 BU 的项目在使用。飞冰包含了一条从设计端到开发端的完整链路，帮助用户快速搭建属于自己的中后台应用。

- 官网地址
- Github 

**备注:主要组件还是以 React 为主，截止 2019 年 02 月 17 日更新博客前对 Vue 的支持还不太完善，目前尚处于观望阶段**

 

#### 2.7.4、VantUI

Vant UI 是有赞前端团队基于有赞统一的规范实现的 Vue 组件库,提供了一整套 UI 基础组件和业务组件。通过 Vant，可以快速搭建出风格统一的页面， 提升开发效率。

- 官网地址
- Github

 

#### 2.7.5、 AtUI

at-ui是一款基于 Vue 2.x 的前端UI组件库,主要用于快速开发PC网站产品。它提供了一套 npm + webpack + babel 前端开发工作流程，CSS 样式独立，即使采用不同的框架实现都能保持统一的UI风格。

- 官网地址
- Github

 

#### 2.7.6、 CubeUI

cube-ui 是滴滴团队开发的基于 Vue.js 实现的精致移动端组件库。支持按需引入和后编译，轻量灵活；扩展性强，可以方便地基于现有组件实现二次开发。

- 官网地址
- Github

 

#### 混合开发

**Flutter**

Flutter是谷歌的移动端UI框架，可在极短的时间内构建Android 和iOs.上高质量的原生级应用。Flutter 可与现有代码一起工作,它被世界各地的开发者和组织使用,并且Flutter是免费和开源的。

- 官网地址
- Github

**备注: Google出品，主要特点是快速构建原生APP应用程序，如做混合应用该框架为必选框架**

**lonic** lonic 既是一个 CSS 框架也是一个 Javascript UI 库，lonic 是目前最有潜力的一款 HTML5 手机应用开发框架。通过 SASS 构建应用程序，它提供了很多 UI 组件来帮助开发者开发强大的应用。它使用 JavaScript MVVM框架和 AngularJS/Vue 来增强应用。提供数据的双向绑定，使用它成为 Web 和移动开发者的共同选择。

- 官网地址
- 官网文档
- Github

 

#### 微信小程序

**mpvue** mpvue 是美团开发的一个使用 `Vue.js` 开发小程序的前端框架，目前支持微信小程序、百度智能小程序，头条小程序和支付宝小程序。框架基于 `Vue.js` ，修改了的运行时框架 `runt ime` 和代码编译器 `compiler` 实现，使其可运行在小程序环境中，从而为小程序开发引入了 `Vue.js` 开发体验。

- 官网地址
- Github

**备注:完备的Vue开发体验，并且支持多平台的小程序开发，推荐使用**

**WeUl** WeUI 是一套同微信原生视觉体验一致的基础样式库， 由微信官方设计团队为微信内网页和微信小程序量身设计，令用户的使用感知更加统一。包含 button、cell、 dialog、toast、article、 icon 等各式元素。

- 官网地址
- Github

 

## 3、了解前后端分离的演变史

### 3.1、后端为主的 MVC 时代

为了降低开发的复杂度，以后端为出发点,比如：Struts、 SpringMVC 等框架的使用，就是后端的MVC时代；

以 `Spring MVC` 的流程为例：

![img](https://gitee.com/xudongyin/img/raw/master/img/20200719184309.png)

- 发起请求到前端控制器( `DispatcherServlet` )
- 前端控制器请求 `HandlerMapping` 查找 `Handler`, 可以根据 `xml` 配置、注解进行查找
- 处理器映射器 `HandlerMapping` 向前端控制器返回 `Handler`
- 前端控制器调用处理器适配器去执行Handler
- 处理器适配器去执行 `Handler`
- `Handler` 执行完成给适配器返回 `ModelAndView`
- 处理器适配器向前端控制器返回 `ModelAndView` ，`Mode lAndView` 是 `SpringMVC` 框架的一个底层对象，包括 `Model` 和 `View`
- 前端控制器请求视图解析器去进行视图解析，根据逻辑视图名解析成真正的视图( `JSP` )
- 视图解析器向前端控制器返回 `View`
- 前端控制器进行视图渲染，视图渲染将模型数据（在 `ModelAndView` 对象中）填充到 `request` 域
- 前端控制器向用户响应结果

**优点：**

MVC是一个非常好的协作模式，能够有效降低代码的耦合度,从架构上能够让开发者明白代码应该写在哪里。为了让View更纯粹,还可以使用Thymeleaf、Freemarker 等模板引擎，使模板里无法写入Java代码,让前后端分工更加清晰。

**缺点：**

- 前端开发重度依赖开发环境，开发效率低，这种架构下，前后端协作有两种模式：
  - 第一种是前端写 DEMO，写好后，让后端去套模板。好处是 DEMO 可以本地开发，很高效。不足是还需要后端套模板，有可能套错，套完后还需要前端确定，来回沟通调整的成本比较大；
  - 另一种协作模式是前端负责浏览器端的所有开发和服务器端的 View 层模板开发。好处是 UI 相关的代码都是前端去写就好，后端不用太关注，不足就是前端开发重度绑定后端环境，环境成为影响前端开发效率的重要因素。
- 前后端职责纠缠不清：模板弓|擎功能强大，依旧可以通过拿到的上下文变量来实现各种业务逻辑。这样，只要前端弱势一点，往往就会被后端要求在模板层写出不少业务代码。还有一个很大的灰色地带是 `Controller` ，页面路由等功能本应该是前端最关注的，但却是由后端来实现。`Controller` 本身与 `Model` 往往也会纠缠不清，看了让人咬牙的业务代码经常会出现在 `Controller` 层。这些问题不能全归结于程序员的素养，否则 JSP 就够了。
- 对前端发挥的局限性：性能优化如果只在前端做空间非常有限，于是我们经常需要后端合作，但由于后端框架限制，我们很难使用**【Comet】** 、**【BigPipe】** 等技术方案来优化性能。

注：在这期间（2005 年以前），包括早期的 JSP、PHP 可以称之为 Web 1.0 时代。因为时代在变、技术在变、什么都在变（引用扎克伯格的一句话：唯一不变的是变化本身）；一些陈旧的技术对于市场来说早就过时了，比如 JSP。

 

### 3.2、基于 AJAX 带来的 SPA 时代

时间回到 2005 年 `AJAX` （Asynchronous JavaScript And XM，异步JavaScript和XML,老技术新用法）被正式提出并开始使用 `CDN` 作为静态资源存储，于是出现了JavaScript王者归来（在这之前 JS 都是用来在网页上贴狗皮膏药广告的）的 SPA （Single Page Application）单页面应用时代。

![img](https://gitee.com/xudongyin/img/raw/master/img/20200719184316.png)

**优点**

这种模式下，**前后端的分工非常清晰，前后端的关键协作点是AJAX接口**。看起来是如此美妙,但回过头来看看的话，这与 JSP 时代区别不大。复杂度从服务端的 JSP 里移到了浏览器的 JavaScript，浏览器端变得很复杂。类似 Spring MVC，**这个时代开始出现浏览器端的分层架构**：

![img](https://gitee.com/xudongyin/img/raw/master/img/20200719184319.png)

**缺点**

- **前后端接口的约定：**如果后端的接口一塌糊涂,如果后端的业务模型不够稳定,那么前端开发会很痛苦；不少团队也有类似尝试，通过接口规则、接口平台等方式来做。**有了和后端一起沉淀的接口规则，还可以用来模拟数据，使得前后端可以在约定接口后实现高效并行开发**。
- **前端开发的复杂度控制：**SPA 应用大多以功能交互型为主，JavaScript 代码过十万行很正常。大量 JS 代码的组织，与 View 层的绑定等，都不是容易的事情。

 

### 3.3、前端为主的 MV* 时代

此处的 MV* 模式如下：

- MVC （同步通信为主）：Model、View、Controller
- MVP （异步通信为主）： Model、 View、 Presenter
- MVVM （异步通信为主）：Model、 View、 ViewModel

为了降低前端开发复杂度，涌现了大量的前端框架，比如：`AngularJS`、 `React`、 `Vue.js` 、`EmberJS`等，这些框架总的原则是先按类型分层,比如 Templates、Controllers、 Models, 然后再在层内做切分，如下图：

![img](https://gitee.com/xudongyin/img/raw/master/img/20200719184322.png)

**优点**

- **前后端职责很清晰：**前端工作在浏览器端，后端工作在服务器端。清晰的分工，可以让开发并行，测试数据的模拟不难，前端可以本地开发。后端则可以专注于业务逻辑的处理，输出 RESTful 等接口。
- **前端开发的复杂度可控：**前端代码很重，但合理分层，让前端代码能各司其职。这一块蛮有意思的，简单如模板特性的选择，就有很多讲究。并非越强大越好，限制什么，留下哪些自由，代码应该如何组织，所有这一切设计都有很大学问，得花一本书的厚度去说明。
- **部署相对独立：**可以快速改进产品的体验。

**缺点**

- 代码不能复用。比如后端依旧需要对数据做各种校验，校验逻辑无法复用浏览器端的代码。如果可以复用，那么后端的数据校验可以相对简单化。
- 全异步，对 SEO 不利。往往还需要服务端做同步渲染的降级方案。
- 性能并非最佳，特别是移动互联网环境下。
- SPA 不能满足所有需求，依旧存在大量多页面应用。URL Design 需要后端配合，前端无法完全掌握。

 

### 3.4、Node.JS 代理的全栈时代

前端为主的 MV* 模式解决了很多很多问题，但如上所述，依旧存在不少不足之处。随着 Node.JS 的兴起， JavaScript 开始有能力运行在服务端。这意味着可以有一种新的研发模式：

<img src="https://gitee.com/xudongyin/img/raw/master/img/20200719184327.png" alt="img" style="zoom:50%;" />

在这种研发模式下，前后端的职责很清晰。对前端来说，两个UI层各司其职：

- Front-End UI Layer 处理浏览器层的展现逻辑。通过 CSS 渲染样式，通过 JavaScript 添加交互功能，HTML 的生成也可以放在这层,具体看应用场景。
- Back-End UI Layer 处理路由、模板、数据获取、Cookie 等。通过路由,前端终于可以自主把控URL Design，这样无论是单页面应用还是多页面应用，前端都可以自由调控。后端也终于可以摆脱对展现的强关注，转而可以专心于业务逻辑层的开发。

通过 Node，Web Server 层也是 JavaScript 代码，这意味着部分代码可前后复用，需要SEO的场景可以在服务端同步渲染，由于异步请求太多导致的性能问题也可以通过服务端来缓解。前一种模式的不足，通过这种模式几乎都能完美解决掉。

与JSP模式相比,全栈模式看起来是一种回归，也的确是一种向原始开发模式的回归,不过是一种螺旋上升式的回归。

基于NodeJS的全栈模式，依旧面临很多挑战:

- 需要前端对服务端编程有更进一步的认识。比如 TCP/IP 等网络知识的掌握。
- NodeJS 层与 Java 层的高效通信。NodeJS 模式下，都在服务器端，RESTful HTTP 通信未必高效,通过 SOAP 等方式通信更高效。一切需要在验证中前行。
- 对部署、运维层面的熟练了解，需要更多知识点和实操经验。
- 大量历史遗留问题如何过渡。这可能是最大最大的阻力。

**注：为什么说:” 前端想学后台很难，而我们后端程序员学任何东西都很简单“；就是因为后端程序员具备相对完善的知识体系。**

### 3.5、总结

综上所述，模式也好，技术也罢，没有好坏优劣之分，只有适合不适合；前后分离的开发思想主要是基于 `SoC`（关注度分离原则），上面种种模式，都是让前后端的职责更清晰，分工更合理高效。

## 4、Vue：MVVM模式和第一个Vue程序

> 什么是 MVVM

MVVM（Model-View-ViewModel）是一种软件架构设计模式，由微软 WPF（用于替代 WinForm，以前就是用这个技术开发桌面应用程序的）和 Silverlight（类似于 Java Applet，简单点说就是在浏览器上运行的 WPF） 的架构师 Ken Cooper 和 Ted Peters 开发，是一种简化用户界面的**事件驱动编程方式**。由 John Gossman（同样也是 WPF 和 Silverlight 的架构师）于 2005 年在他的博客上发表。

MVVM 源自于经典的 MVC（Model-View-Controller）模式。MVVM 的核心是 ViewModel 层，负责转换 Model 中的数据对象来让数据变得更容易管理和使用，其作用如下：

- 该层向上与视图层进行双向数据绑定
- 向下与 Model 层通过接口请求进行数据交互

![img](https://gitee.com/xudongyin/img/raw/master/img/20200719184337)

MVVM 已经相当成熟了，主要运用但不仅仅在网络应用程序开发中。当下流行的 MVVM 框架有 `Vue.js`，`AngularJS` 等。

### 为什么要使用 MVVM

MVVM 模式和 MVC 模式一样，主要目的是分离视图（View）和模型（Model），有几大好处

- **低耦合**： 视图（View）可以独立于 Model 变化和修改，一个 ViewModel 可以绑定到不同的 View 上，当 View 变化的时候 Model 可以不变，当 Model 变化的时候 View 也可以不变。
- **可复用**： 你可以把一些视图逻辑放在一个 ViewModel 里面，让很多 View 重用这段视图逻辑。
- **独立开发**： 开发人员可以专注于业务逻辑和数据的开发（ViewModel），设计人员可以专注于页面设计。
- **可测试**： 界面素来是比较难于测试的，而现在测试可以针对 ViewModel 来写。

### MVVM 的组成部分

<img src="https://gitee.com/xudongyin/img/raw/master/img/20200719184340" alt="img" style="zoom:67%;" />

#### View

View 是视图层，也就是用户界面。前端主要由 `HTML` 和 `CSS` 来构建，为了更方便地展现 `ViewModel` 或者 `Model` 层的数据，已经产生了各种各样的前后端模板语言，比如 FreeMarker、Thymeleaf 等等，各大 MVVM 框架如 Vue.js，AngularJS，EJS 等也都有自己用来构建用户界面的内置模板语言。

#### Model

Model 是指数据模型，泛指后端进行的各种业务逻辑处理和数据操控，主要围绕数据库系统展开。这里的难点主要在于需要和前端约定统一的接口规则

#### ViewModel

ViewModel 是由前端开发人员组织生成和维护的视图数据层。在这一层，前端开发者对从后端获取的 Model 数据进行转换处理，做二次封装，以生成符合 View 层使用预期的视图数据模型。

需要注意的是 ViewModel 所封装出来的数据模型包括视图的状态和行为两部分，而 Model 层的数据模型是只包含状态的

- 比如页面的这一块展示什么，那一块展示什么这些都属于视图状态（展示）
- 页面加载进来时发生什么，点击这一块发生什么，这一块滚动时发生什么这些都属于视图行为（交互）

视图状态和行为都封装在了 ViewModel 里。这样的封装使得 ViewModel 可以完整地去描述 View 层。由于实现了双向绑定，ViewModel 的内容会实时展现在 View 层，这是激动人心的，因为前端开发者再也不必低效又麻烦地通过操纵 DOM 去更新视图。

MVVM 框架已经把最脏最累的一块做好了，我们开发者只需要处理和维护 ViewModel，更新数据视图就会自动得到相应更新，真正实现**事件驱动编程**。

View 层展现的不是 `Model` 层的数据，而是 `ViewModel` 的数据，由 `ViewModel` 负责与 `Model` 层交互，这就**完全解耦了 View 层和 Model 层，这个解耦是至关重要的，它是前后端分离方案实施的重要一环。**

### Vue

Vue (读音 /vjuː/，类似于 view) 是一套用于构建用户界面的渐进式框架，发布于 2014 年 2 月。与其它大型框架不同的是，Vue 被设计为可以自底向上逐层应用。Vue 的核心库只关注视图层，不仅易于上手，还便于与第三方库（如：vue-router，vue-resource，vuex）或既有项目整合。

> MVVM 模式的实现者

- Model：模型层，在这里表示 JavaScript 对象
- View：视图层，在这里表示 DOM（HTML 操作的元素）
- ViewModel：连接视图和数据的中间件，Vue.js 就是 MVVM 中的 ViewModel 层的实现者

在 MVVM 架构中，是不允许 数据 和 视图 直接通信的，只能通过 ViewModel 来通信，而 ViewModel 就是定义了一个 Observer 观察者

- ViewModel 能够观察到数据的变化，并对视图对应的内容进行更新
- ViewModel 能够监听到视图的变化，并能够通知数据发生改变

至此，我们就明白了，Vue.js 就是一个 MVVM 的实现者，他的核心就是实现了 **DOM 监听** 与 **数据绑定**

> 为什么要使用 Vue.js

- 轻量级，体积小是一个重要指标。Vue.js 压缩后有只有 20多kb （Angular 压缩后 56kb+，React 压缩后 44kb+）
- 移动优先。更适合移动端，比如移动端的 Touch 事件
- 易上手，学习曲线平稳，文档齐全
- 吸取了 Angular（模块化）和 React（虚拟 DOM）的长处，并拥有自己独特的功能，如：计算属性
- 开源，社区活跃度高
- ......

### 第一个Vue程序

【说明】IDEA 可以安装 Vue 的插件！

注意：Vue 不支持 IE8 及以下版本，因为 Vue 使用了 IE8 无法模拟的 ECMAScript 5 特性。但它支持所有兼容 ECMAScript 5 的浏览器。

> 下载地址

- 开发版本

  - 包含完整的警告和调试模式：https://vuejs.org/js/vue.js
  - 删除了警告，30.96KB min + gzip：https://vuejs.org/js/vue.min.js

- CDN

  - <script src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.js"></script>

  - <script src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.min.js"></script>

> 代码编写

Vue.js 的核心是实现了 MVVM 模式，她扮演的角色就是 ViewModel 层，那么所谓的第一个应用程序就是展示她的 数据绑定 功能，操作流程如下：

**1、创建一个 HTML 文件**

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>狂神说</title>
</head>
<body>

</body>
</html>
```

**2、引入 Vue.js**

```html
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.js"></script>
```

**3、创建一个 Vue 的实例**

```html
<script type="text/javascript">
    var vm = new Vue({
        el: '#vue',
        data: {
            message: 'Hello Vue!'
        }
    });
</script>
```

说明:

- `el:'#vue'`：绑定元素的 ID
- `data:{message:'Hello Vue!'}`：数据对象中有一个名为 message 的属性，并设置了初始值 Hello Vue!

**4、将数据绑定到页面元素**

```html
<div id="vue">
    {{message}}
</div>
```

说明：只需要在绑定的元素中使用 双花括号 将 Vue 创建的名为 message 属性包裹起来，即可实现数据绑定功能，也就实现了 ViewModel 层所需的效果，是不是和 EL 表达式非常像？

**完整的 HTML**

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>第一个 Vue 应用程序</title>
</head>
<body>

<!--View-->
<div id="vue">
     {{message}}
</div>

<script src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.js"></script>
<script type="text/javascript">
    // var vm = new Vue({}); //ViewModel
    var vm = new Vue({
        el: '#vue',
        data: { //Model
            message: 'Hello Vue!'
        }
    });
</script>
</body>
</html>
```

**测试**

为了能够更直观的体验 Vue 带来的数据绑定功能，我们需要在浏览器测试一番，操作流程如下：

1、在浏览器上运行第一个 Vue 应用程序，进入 开发者工具

2、在控制台输入 vm.message = 'Hello World' ，然后 回车，你会发现浏览器中显示的内容会直接变成 Hello World

此时就可以在控制台直接输入 vm.message 来修改值，中间是可以省略 data 的，在这个操作中，我并没有主动操作 DOM，就让页面的内容发生了变化，这就是借助了 Vue 的 数据绑定 功能实现的；MVVM 模式中要求 ViewModel 层就是使用 观察者模式 来实现数据的监听与绑定，以做到数据与视图的快速响应。

## 5、Vue：基础语法

### v-bind

我们已经成功创建了第一个 Vue 应用！看起来这跟渲染一个字符串模板非常类似，但是 Vue 在背后做了大量工作。现在数据和 DOM 已经被建立了关联，所有东西都是响应式的。我们在控制台操作对象属性，界面可以实时更新！

我们还可以使用`v-bind`来绑定元素特性!

**上代码：**

```html
<!DOCTYPE html>
<html xmlns:v-bind="http://www.w3.org/1999/xhtml">
<head>
    <meta charset="UTF-8">
    <title>狂神说Java</title>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.js"></script>
</head>
<body>

<div id="app">
  <span v-bind:title="message">
    鼠标悬停几秒钟查看此处动态绑定的提示信息！
  </span>
</div>

<script type="text/javascript">
    var vm = new Vue({
        el: '#app',
        data: {
            message: '页面加载于 ' + new Date().toLocaleString()
        }
    })
</script>
</body>
</html>
```

你看到的 v-bind 等被称为指令。指令带有前缀 v-，以表示它们是 Vue 提供的特殊特性。可能你已经猜到了，它们会在渲染的 DOM 上应用特殊的响应式行为。在这里，该指令的意思是：“将这个元素节点的 title 特性和 Vue 实例的 message 属性保持一致”。

如果你再次打开浏览器的 JavaScript 控制台，输入 vm.message = '新消息'，就会再一次看到这个绑定了 title 特性的 HTML 已经进行了更新。

### v-if,v-else

什么是条件判断语句，就不需要我说明了吧（￣▽￣）,以下两个属性！

- `v-if`
- `v-else`

**上代码：**

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>狂神说Java</title>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.js"></script>
</head>
<body>

<div id="vue">
    <h1 v-if="ok">YES</h1>
    <h1 v-else>NO</h1>
</div>

<script type="text/javascript">
    var vm = new Vue({
        el: '#vue',
        data: {
            ok: true
        }
    });
</script>
</body>
</html>
```

测试：

1. 在浏览器上运行，打开控制台！
2. 在控制台输入 `vm.ok = false` ，然后 回车，你会发现浏览器中显示的内容会直接变成 NO

注：使用 `v-*` 属性绑定数据是不需要 `双花括号` 包裹的

### v-else-if

- v-if
- v-else-if
- v-else

注：`===` 三个等号在 JS 中表示绝对等于（就是数据与类型都要相等）

**上代码：**

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>狂神说Java</title>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.js"></script>
</head>
<body>

<div id="vue">
    <h1 v-if="type === 'A'">A</h1>
    <h1 v-else-if="type === 'B'">B</h1>
    <h1 v-else-if="type === 'C'">C</h1>
    <h1 v-else>who</h1>
</div>

<script type="text/javascript">
    var vm = new Vue({
        el: '#vue',
        data: {
            type: 'A'
        }
    });
</script>
</body>
</html>
```

测试：观察在控制台输入 vm.type = 'B'、'C'、'D' 的变化

### v-for

格式说明：

```html
<div id="vue">
    <li v-for="item in items">
        {{ item.message }}
    </li>
</div>
```

注：`items` 是数组，`item`是数组元素迭代的别名。我们之后学习的Thymeleaf模板引擎的语法和这个十分的相似！

**上代码：**

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>狂神说Java</title>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.js"></script>
</head>
<body>

<div id="vue">
    <li v-for="item in items">
        {{ item.message }}
    </li>
</div>

<script type="text/javascript">
    var vm = new Vue({
        el: '#vue',
        data: {
            //items数组，items在这里只是代号作用，可以改为其他代名词
            items: [
                {message: '狂神说Java'},
                {message: '狂神说前端'}
            ]
        }
    });
</script>
</body>
</html>
```

测试 ：在控制台输入 `vm.items.push({message: '狂神说运维'})` ，尝试追加一条数据，你会发现浏览器中显示的内容会增加一条 `狂神说运维`.

### v-on

`v-on` 监听事件

事件有Vue的事件、和前端页面本身的一些事件！我们这里的`click`是vue的事件，可以绑定到Vue中的`methods`中的方法事件！

**上代码：**

```html
<!DOCTYPE html>
<html xmlns:v-on="">
<head>
    <meta charset="UTF-8">
    <title>狂神说Java</title>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.js"></script>
</head>
<body>

<div id="vue">
    <!--在这里我们使用了 v-on 绑定了 click 事件，并指定了名为 sayHi 的方法-->
    <button v-on:click="sayHi">点我</button>
</div>


<script type="text/javascript">
    var vm = new Vue({
        el: '#vue',
        data: {
            message: 'Hello World'
        },
        // 方法必须定义在 Vue 实例的 methods 对象中
        methods: {
            sayHi: function (event) {
                // `this` 在方法里指向当前 Vue 实例
                alert(this.message);
            }
        }
    });
</script>
</body>
</html>
```

## 6、Vue：表单双绑、组件

> **什么是双向数据绑定**

Vue.js 是一个 MVVM 框架，即数据双向绑定，即当数据发生变化的时候，视图也就发生变化，**当视图发生变化的时候，数据也会跟着同步变化**。这也算是 Vue.js 的精髓之处了。

值得注意的是，我们所说的数据双向绑定，一定是对于 UI 控件来说的，非 UI 控件不会涉及到数据双向绑定。单向数据绑定是使用状态管理工具的前提。如果我们使用 `vuex`，那么数据流也是单项的，这时就会和双向数据绑定有冲突。

> **为什么要实现数据的双向绑定**

在 `Vue.js` 中，如果使用 `vuex`，实际上数据还是单向的，之所以说是数据双向绑定，这是用的 UI 控件来说，对于我们处理表单，Vue.js 的双向数据绑定用起来就特别舒服了。即两者并不互斥，在全局性数据流使用单项，方便跟踪；局部性数据流使用双向，简单易操作。

### 在表单中使用双向数据绑定

你可以用 `v-model` 指令在表单 `<input>`、`<textarea>` 及 `<select>` 元素上创建双向数据绑定。它会根据控件类型自动选取正确的方法来更新元素。尽管有些神奇，但 v-model 本质上不过是语法糖。它负责监听用户的输入事件以更新数据，并对一些极端场景进行一些特殊处理。

**注意：v-model 会忽略所有表单元素的 value、checked、selected 特性的初始值而总是将 Vue 实例的数据作为数据来源。你应该通过 JavaScript 在组件的 data 选项中声明初始值!**

#### 单行文本

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>狂神说Java</title>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.js"></script>
</head>
<body>

<!--我们这里希望，输入框的值和{}取值动态绑定，实时相同，我们就使用v-model绑定message-->
<div id="vue">
    单行文本：<input type="text" v-model="message" value="hello" />&nbsp;&nbsp;单行文本是：{{message}}
</div>

<script type="text/javascript">
    var vm = new Vue({
        el: '#vue',
        data: {
            message: "Hello Vue"
        }
    });
</script>

</body>
</html>
```

#### 多行文本

```html
<div id="vue">
    多行文本：<textarea v-model="message"></textarea>&nbsp;&nbsp;多行文本是：{{message}}
</div>

<script type="text/javascript">
    var vm = new Vue({
        el: '#vue',
        data: {
            message: "Hello Textarea"
        }
    });
</script>
```

#### 单复选框

```html
<div id="vue">
    单复选框：
    <input type="checkbox" id="checkbox" v-model="checked">
    &nbsp;&nbsp;
    <label for="checkbox">{{ checked }}</label>
</div>

<script type="text/javascript">
    var vm = new Vue({
        el: '#vue',
        data: {
            checked: false
        }
    });
</script>
```

#### 多复选框

```html
<div id="vue">
    多复选框：
    <input type="checkbox" id="jack" value="Jack" v-model="checkedNames">
    <label for="jack">Jack</label>
    <input type="checkbox" id="john" value="John" v-model="checkedNames">
    <label for="john">John</label>
    <input type="checkbox" id="mike" value="Mike" v-model="checkedNames">
    <label for="mike">Mike</label>
    <span>选中的值: {{ checkedNames }}</span>
</div>

<script type="text/javascript">
    var vm = new Vue({
        el: '#vue',
        data: {
            checkedNames: []
        }
    });
</script>
```

#### 单选按钮

```html
<div id="vue">
    单选按钮：
    <input type="radio" id="one" value="One" v-model="picked">
    <label for="one">One</label>
    <input type="radio" id="two" value="Two" v-model="picked">
    <label for="two">Two</label>
    <span>选中的值: {{ picked }}</span>
</div>

<script type="text/javascript">
    var vm = new Vue({
        el: '#vue',
        data: {
            picked: ''
        }
    });
</script>
```

#### 下拉框

```html
<div id="vue">
    下拉框：
    <select v-model="selected">
        <option disabled value="">请选择</option>
        <option>A</option>
        <option>B</option>  
<!-- <option selected >B</option> 如果要生效，就得将date里的selected数据改为‘B’ -->
        <option>C</option>
    </select>
    <span>选中的值: {{ selected }}</span>
</div>

<script type="text/javascript">
    var vm = new Vue({
        el: '#vue',
        data: {
            selected: ''
        }
    });
</script>
```

**注意**：如果 `v-model` 表达式的初始值未能匹配任何选项，<select> 元素将被渲染为“未选中”状态。在 iOS 中，这会使用户无法选择第一个选项。因为这样的情况下，iOS 不会触发 change 事件。因此，更推荐像上面这样提供一个值为空的禁用选项。

### 什么是组件

组件是可复用的 `Vue` 实例，说白了就是一组可以重复使用的模板，跟 JSTL 的自定义标签、Thymeleaf 的 `th:fragment `等框架有着异曲同工之妙。通常一个应用会以一棵嵌套的组件树的形式来组织：

![img](https://gitee.com/xudongyin/img/raw/master/img/20200719184415.png)

例如，你可能会有页头、侧边栏、内容区等组件，每个组件又包含了其它的像导航链接、博文之类的组件。

### 第一个 Vue 组件

注意：在实际开发中，我们并不会用以下方式开发组件，而是采用 vue-cli 创建 .vue 模板文件的方式开发，以下方法只是为了让大家理解什么是组件。

**使用 Vue.component() 方法注册组件,格式如下：**

```html
<script type="text/javascript">
    // 先注册组件，组件名不能有大写
    Vue.component('my-component-li', {
        template: '<li>Hello li</li>'
    });

    // 再实例化 Vue
    var vm = new Vue({
        el: '#vue'
    });
</script>

<div id="vue">
    <ul>
        <my-component-li></my-component-li>
    </ul>
</div>
```

说明：

- Vue.component()：注册组件
- my-component-li：自定义组件的名字
- template：组件的模板

### 使用 `props` 属性传递参数

像上面那样用组件没有任何意义，所以我们是需要传递参数到组件的，此时就需要使用 `props` 属性了！

**注意：默认规则下组件名 和 props 属性里的值不能为大写；**

```html
<script type="text/javascript">
    // 先注册组件
    Vue.component('my-component-li', {
        props: ['list'],
        template: '<li>Hello {{list}}</li>'
    });

    // 再实例化 Vue
    var vm = new Vue({
        el: '#vue',
        data: {
            items: ["张三", "李四", "王五"]
        }
    });
</script>

<div id="vue">
    <ul>
        <my-component-li v-for="item in items" v-bind:list="item"></my-component-li>
    </ul>
</div>
```

说明：

- `v-for="item in items"`：遍历 `Vue` 实例中定义的名为 `items` 的数组，并创建同等数量的组件
- `v-bind:list=“item“`：将遍历的 `item` 项绑定到组件中 `props` 定义的名为 `list` 属性上；= 号左边的 list 为 props 定义的属性名，右边的为 `item in items` 中遍历的 item 项的值

## 7、什么是Axios

  Axios是一个开源的可以用在浏览器端和`Node JS`的异步通信框架， 她的主要作用就是实现AJAX异步通信，其功能特点如下：

- 从浏览器中创建`XMLHttpRequests`
- 从node.js创建http请求
- 支持Promise API[JS中链式编程]
- 拦截请求和响应
- 转换请求数据和响应数据
- 取消请求
- 自动转换JSON数据
- 客户端支持防御XSRF(跨站请求伪造)

  GitHub：https://github.com/axios/axios
  中文文档：http://www.axios-js.com/

> 为什么要使用Axios

由于`Vue.js`是一个视图层框架并且作者(尤雨溪) 严格准守SoC(关注度分离原则)所以`Vue.js`并不包含AJAX的通信功能， 为了解决通信问题， 作者单独开发了一个名为`vue-resource`的插件， 不过在进入2.0版本以后停止了对该插件的维护并推荐了`Axios`框架。少用jQuery， 因为它操作Dom太频繁!

### 第一个Axios应用程序

  咱们开发的接口大部分都是采用JSON格式， 可以先在项目里模拟一段JSON数据， 数据内容如下：创建一个名为data.json的文件并填入上面的内容， 放在项目的根目录下

```json
{
  "name": "狂神说Java",
  "url": "https://blog.kuangstudy.com",
  "page": 1,
  "isNonProfit": true,
  "address": {
    "street": "含光门",
    "city": "陕西西安",
    "country": "中国"
  },
  "links": [
    {
      "name": "bilibili",
      "url": "https://space.bilibili.com/95256449"
    },
    {
      "name": "狂神说Java",
      "url": "https://blog.kuangstudy.com"
    },
    {
      "name": "百度",
      "url": "https://www.baidu.com/"
    }
  ]
}
```

  **测试代码**

```html
<!DOCTYPE html>
<html lang="en" xmlns:v-binf="http://www.w3.org/1999/xhtml">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <!--v-cloak 解决闪烁问题-->
    <style>
        [v-cloak]{
            display: none;
        }
    </style>
</head>
<body>
<div id="vue">
    <div>地名：{{info.name}}</div>
    <div>地址：{{info.address.country}}--{{info.address.city}}--{{info.address.street}}</div>
    <div>链接：<a v-bind:href="info.url" target="_blank">{{info.url}}</a> </div>
</div>

<!--引入js文件-->
<script src="../js/vue.js"></script>
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
<script type="text/javascript">
    var vm = new Vue({
        el:"#vue",
        //data：属性，下面这个是函数
        data(){
            return{
                //这里info可以写为其他，这里只是一个引用变量的作用
                info:{
                    name:null,
                    address:{
                        country:null,
                        city:null,
                        street:null
                    },
                    url:null
                }
            }
        },
        mounted(){//钩子函数
            axios
                .get('data.json')//这里是链接，看json文件位置
            //这里response可以写为其他，这里只是一个引用变量的作用
                .then(response=>(this.info=response.data));
        }
    });
</script>

</body>
</html>
```

  说明：

1. 在这里使用了v-bind将a:href的属性值与Vue实例中的数据进行绑定
2. 使用axios框架的get方法请求AJAX并自动将数据封装进了Vue实例的数据对象中
3. 我们在data中的数据结构必须和`Ajax`响应回来的数据格式匹配！

## 8、Vue的生命周期

  官方文档：https://cn.vuejs.org/v2/guide/instance.html  #生命周期图示
  Vue实例有一个完整的生命周期，也就是从开始创建初始化数据、编译模板、挂载DOM、渲染一更新一渲染、卸载等一系列过程，我们称这是Vue的生命周期。通俗说就是Vue实例从创建到销毁的过程，就是生命周期。
  在Vue的整个生命周期中，它提供了一系列的事件，可以让我们在事件触发时注册JS方法，可以让我们用自己注册的JS方法控制整个大局，在这些事件响应方法中的this直接指向的是Vue的实例。
![在这里插入图片描述](https://gitee.com/xudongyin/img/raw/master/img/20200719184423)

## 9、Vue：计算属性、内容分发、自定义事件

### 计算属性

计算属性的重点突出在 `属性` 两个字上（属性是名词），首先它是个 `属性` 其次这个属性有 `计算` 的能力（计算是动词），这里的 `计算` 就是个函数；简单点说，它就是一个能够将计算结果缓存起来的属性（将行为转化成了静态的属性），仅此而已；可以想象为缓存！

**上代码：**

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>狂神说Java</title>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.js"></script>
</head>
<body>

<div id="vue">
    <!--注意，一个是方法，一个是属性-->
    <p>调用当前时间的方法：{{currentTime1()}}</p>
    <p>当前时间的计算属性：{{currentTime2}}</p>
</div>

<script type="text/javascript">
    var vm = new Vue({
        el: '#vue',
        data: {
            message: 'Hello Vue'
        },
        methods: {
            currentTime1: function () {
                return Date.now();
            }
        },
        computed: {
            //currentTime2 ，这是一个属性！不是方法
            currentTime2: function () {
                this.message;
                return Date.now();
            }
        }
    });
</script>
</body>
</html>
```

**注意：methods 和 computed 里的东西不能重名**

**说明：**

- methods：定义方法，调用方法使用 currentTime1()，需要带括号
- computed：定义计算属性，调用属性使用 currentTime2，不需要带括号；this.message 是为了能够让 currentTime2 观察到数据变化而变化
- **如果在方法中的值发生了变化，则缓存就会刷新！如果在控制台使用 `vm.message="qinjiang"`,改变下数据的值，则缓存会被刷新，也就是 currentTime2 的值会刷新！**

**结论：**

调用方法时，每次都需要进行计算，既然有计算过程则必定产生系统开销，那如果这个结果是不经常变化的呢？此时就可以考虑将这个结果缓存起来，采用计算属性可以很方便的做到这一点,**计算属性的主要特性就是为了将不经常变化的计算结果进行缓存，以节约我们的系统开销;**

### 内容分发

在 `Vue.js` 中我们使用 `<slot>` 元素作为承载分发内容的出口，作者称其为 插槽，可以应用在组合组件的场景中;

**测试**

比如准备制作一个待办事项组件（todo），该组件由待办标题（todo-title）和待办内容（todo-items）组成，但这三个组件又是相互独立的，该如何操作呢？

**第一步: 定义一个待办事项的组件**

```html
<div id="vue">
    <todo></todo>
</div>

<script type="text/javascript">
    Vue.component('todo', {
        template: '<div>\
                    <div>待办事项</div>\
                    <ul>\
                        <li>学习狂神说Java</li>\
                    </ul>\
               </div>'
        //这里得外加一个<div>包围，不然<ul>列表会显示不出来
    });
</script>
```

**第二步: 我们需要让,待办事项的标题和值实现动态绑定,怎么做呢? 我们可以留出一个插槽!**

1-将上面的代码留出一个插槽,即 slot

```javascript
Vue.component('todo', {
    template: '<div>\
                    <slot name="todo-title"></slot>\
                    <ul>\
                        <slot name="todo-items"></slot>\
                    </ul>\
               </div>'
});
```

2-定义一个名为 todo-title 的待办标题组件 和 todo-items 的待办内容组件

```javascript
Vue.component('todo-title', {
    props: ['title'],
    template: '<div>{{title}}</div>'
});
//这里的index,就是数组的下标,使用for循环遍历的时候,可以循环出来!
Vue.component('todo-items', {
    props: ['item', 'index'],
    template: '<li>{{index + 1}}. {{item}}</li>'
});
```

3-实例化 Vue 并初始化数据

```javascript
var vm = new Vue({
    el: '#vue',
    data: {
        todoItems: ['狂神说Java', '狂神说运维', '狂神说前端']
    }
});
```

4-将这些值,通过插槽插入

```html
<div id="vue">
    <todo>
        <todo-title slot="todo-title" title="秦老师系列课程"></todo-title>
        <todo-items slot="todo-items" v-for="(item, index) in todoItems" v-bind:item="item" v-bind:index="index" ></todo-items>
    </todo>
</div>
```

说明:我们的 todo-title 和 todo-items 组件分别被分发到了 todo 组件的 todo-title 和 todo-items 插槽中

~~~html
<!DOCTYPE html>
<html lang="en" xmlns:v-bind="http://www.w3.org/1999/xhtml">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="../lib/vue.js"></script>
</head>
<body>

<div id="vue">
   <todo>
       <todo-title slot="title" v-bind:header="head"></todo-title>
       <todo-list slot="list" v-for="(item, index) in items"
                  v-bind:list="item" v-bind:ini="index"></todo-list>          
   </todo>
</div>

<script type="text/javascript">
    Vue.component("todo",{
         template: '<div>\
                      <slot name="title"></slot>\
                      <ul>\
                        <slot name="list"></slot>\
                      </ul>\
                     </div>'
        //这里得外加一个<div>包围，不然<ul>列表会显示不出来
    });
    Vue.component("todo-title",{
        props:['header'],
        template: '<p>{{header}}</p>'
    });
    Vue.component('todo-list',{
        props: ['list','ini'],
        template:'<li>{{list}}---{{ini+1}}</li>'
    });
    var vm = new Vue({
        el: '#vue',
        data: {
            head:'秦老师教程',
            items:['java','linux','mysql']
        }
    });
</script>
</body>
</html>

</body>
</html>
~~~



### 自定义事件

通过以上代码不难发现，数据项在 Vue 的实例中，但删除操作要在组件中完成，那么组件如何才能删除 Vue 实例中的数据呢？此时就涉及到参数传递与事件分发了，Vue 为我们提供了自定义事件的功能很好的帮助我们解决了这个问题；使用 this.$emit('自定义事件名', 参数)，操作过程如下:

1-在vue的实例中,增加了 methods 对象并定义了一个名为 removeTodoItems 的方法

```javascript
    var vm = new Vue({
        el: '#vue',
        data: {
            title: "秦老师系列课程1",
            todoItems: ['狂神说Java', '狂神说运维', '狂神说前端']
        },
        methods: {
            // 该方法可以被模板中自定义事件触发
            removeTodoItems: function (index) {
                console.log("删除 " + this.todoItems[index] + " 成功");
                // splice() 方法向/从数组中添加/删除项目，然后返回被删除的项目，其中 index 为添加/删除项目的位置，1 表示删除的数量
                this.todoItems.splice(index, 1);
            }
        }
    });
```

2-修改 todo-items 待办内容组件的代码,增加一个删除按钮,并且绑定事件!

```javascript
    Vue.component('todo-items', {
        props: ['item', 'index'],
        //v-on:click 缩写 @click
        template: '<li>{{index + 1}}. {{item}}  <button @click="remove_component">删除</button></li>',
        methods: {
            remove_component: function (index) {
       // 这里的 remove 是自定义事件的名称，需要在 HTML 中使用 v-on:remove 的方式指派
                this.$emit('remove', index);
            }
        }
    });
```

3-修改 todo-items 待办内容组件的 HTML 代码,增加一个自定义事件,比如叫 remove,可以和组件的方法绑定,然后绑定到vue的方法中!

```html
<!--增加了 v-on:remove="removeTodoItems(index)" 自定义事件，该事件会调用 Vue 实例中定义的名为 removeTodoItems 的方法
    增加了 :key="index" 不然无法传递 index 参数，v-bind:key="index" 缩写:key="index"
    新版本 :key="index"可不写 -->
<todo-items slot="todo-items" v-for="(item, index) in todoItems"
            v-bind:item="item" v-bind:index="index" :key="index"
            v-on:remove="removeTodoItems(index)"></todo-items>
             <!-- 这里remove相当于点击事件函数名 -->
```

### 逻辑理解

![在这里插入图片描述](https://gitee.com/xudongyin/img/raw/master/img/20200719184432)

## 10、Vue 入门小结

核心 : 数据驱动 , 组件化
优点 : 借鉴了 AngulaJS 的模块化开发 和 React 的虚拟Dom , 虚拟Dom就是把Dom操作放到内存中执行;

常用的属性:

- v-if
- v-else-if
- v-else
- v-for
- v-on 绑定事件 , 简写`@`
- v-model 数据双向绑定
- v-bind 给组件绑定参数,简写 `:`

组件化:

- 组合组件 slot 插槽
- 组件内部绑定事件需要使用到 `this.$emit("事件名",参数)`;
- 计算属性的特色,缓存计算数据

遵循SoC 关注度分离原则,Vue是纯粹的视图框架,并不包含,比如Ajax之类的通信功能,为了解决通信问题,我们需要使用Axios 框架做异步通信;

> 说明

Vue的开发都是要基于NodeJS, 实际开发采用 vue-cli 脚手架开发,vue-router 路由,vuex 做状态管理; Vue UI界面我们一般使用 ElementUI(饿了么出品),或者ICE(阿里巴巴出品!)来快速搭建前端项目~

官网:

- https://element.eleme.cn/#/zh-CN
- https://ice.work/

## 11、第一个vue-cli项目

> **什么是vue-cli**

  vue-cli 官方提供的一个脚手架，用于快速生成一个vue的项目模板；预先定义好的目录结构及基础代码，就好比咱们在创建Maven项目时可以选择创建一个骨架项目，这个估计项目就是脚手架，我们的开发更加的快速；

**项目的功能**

- 统一的目录结构
- 本地调试
- 热部署
- 单元测试
- 集成打包上线

**需要的环境**

- Node.js：http://nodejs.cn/download/
    安装就是无脑的下一步就好，安装在自己的环境目录下
- Git：https://git-scm.com/doenloads
  镜像：https://npm.taobao.org/mirrors/git-for-windows/

  **确认nodejs安装成功：**

- cmd下输入`node -v`，查看是否能够正确打印出版本号即可！

- cmd下输入`npm -v`，查看是否能够正确打印出版本号即可！

  这个npm，就是一个软件包管理工具，就和linux下的apt软件安装差不多！
  **安装Node.js淘宝镜像加速器（cnpm）**
  这样的话，下载会快很多~

```bash
# -g 就是全局安装
npm install cnpm -g

# 或使用如下语句解决npm速度慢的问题
npm install --registry=https://registry.npm.taobao.org
```

  安装的过程可能有点慢~，耐心等待！虽然安装了cnpm，但是尽量少用！
  安装的位置：`C:\Users\administrator\AppData\Roaming\npm`

![在这里插入图片描述](https://gitee.com/xudongyin/img/raw/master/img/20200719184437)
  **安装vue-cli**

```bash
cnpm install vue-cli -g
#测试是否安装成功#查看可以基于哪些模板创建vue应用程序，通常我们选择webpack
vue list
```

![在这里插入图片描述](https://gitee.com/xudongyin/img/raw/master/img/20200719184441)

### 第一个vue-cli应用程序

1.创建一个Vue项目，我们随便建立一个空的文件夹在电脑上，我这里在D盘下新建一个目录

```bash
D:\Project\vue-study;
```

2.创建一个基于webpack模板的vue应用程序

```bash
#1、首先需要进入到对应的目录 cd D:\Project\vue-study
#2、这里的myvue是顶日名称，可以根据自己的需求起名
vue init webpack myvue
```

  一路都选择no即可；
  **说明：**

- Project name：项目名称，默认回车即可
- Project description：项目描述，默认回车即可
- Author：项目作者，默认回车即可
- Install vue-router：是否安装vue-router，选择n不安装（后期需要再手动添加）
- Use ESLint to lint your code:是否使用ESLint做代码检查，选择n不安装（后期需要再手动添加)
- Set up unit tests:单元测试相关，选择n不安装（后期需要再手动添加）
- Setupe2etests with Nightwatch：单元测试相关，选择n不安装（后期需要再手动添加）
- Should we run npm install for you after the,project has been created:创建完成后直接初始化，选择n，我们手动执行；运行结果！

### 初始化并运行

```bash
cd myvue  #进入项目目录
npm install  #安装依赖包
npm run dev  #启动，相当于启动tomcat
```

  执行完成后，目录多了很多依赖

当出现问题时，可以查看提示进行处理如下
![img](https://gitee.com/xudongyin/img/raw/master/img/20200719184450)

### Vue-cli目录结构

![image-20200702144501120](https://gitee.com/xudongyin/img/raw/master/img/20200719184456.png)

- build 和 config：WebPack 配置文件
- node_modules：用于存放 npm install 安装的依赖文件
- src： 项目源码目录
- static：静态资源文件
- .babelrc：Babel 配置文件，主要作用是将 ES6 转换为 ES5
- .editorconfig：编辑器配置
- eslintignore：需要忽略的语法检查配置文件
- .gitignore：git 忽略的配置文件
- .postcssrc.js：css 相关配置文件，其中内部的 module.exports 是 NodeJS 模块化语法
- index.html：首页，仅作为模板页，实际开发时不使用
- package.json：项目的配置文件
  - name：项目名称
  - version：项目版本
  - description：项目描述
  - author：项目作者
  - scripts：封装常用命令
  - dependencies：生产环境依赖
  - devDependencies：开发环境依赖

## 12、webpack使用

> **什么是Webpack**

  本质上， webpack是一个现代JavaScript应用程序的静态模块打包器(module bundler) 。当webpack处理应用程序时， 它会递归地构建一个依赖关系图(dependency graph) ， 其中包含应用程序需要的每个模块， 然后将所有这些模块打包成一个或多个bundle.
  Webpack是当下最热门的前端资源模块化管理和打包工具， 它可以将许多松散耦合的模块按照依赖和规则打包成符合生产环境部署的前端资源。还可以将按需加载的模块进行代码分离，等到实际需要时再异步加载。通过loader转换， 任何形式的资源都可以当做模块， 比如Commons JS、AMD、ES 6、CSS、JSON、Coffee Script、LESS等；
  伴随着移动互联网的大潮， 当今越来越多的网站已经从网页模式进化到了WebApp模式。它们运行在现代浏览器里， 使用HTML 5、CSS 3、ES 6等新的技术来开发丰富的功能， 网页已经不仅仅是完成浏览器的基本需求； WebApp通常是一个SPA(单页面应用) ， 每一个视图通过异步的方式加载，这导致页面初始化和使用过程中会加载越来越多的JS代码，这给前端的开发流程和资源组织带来了巨大挑战。
  前端开发和其他开发工作的主要区别，首先是前端基于多语言、多层次的编码和组织工作，其次前端产品的交付是基于浏览器的，这些资源是通过增量加载的方式运行到浏览器端，如何在开发环境组织好这些碎片化的代码和资源，并且保证他们在浏览器端快速、优雅的加载和更新，就需要一个模块化系统，这个理想中的模块化系统是前端工程师多年来一直探索的难题。

### 模块化的演进

Script标签

```html
	<script src = "module1.js"></script>
	<script src = "module2.js"></script>
	<script src = "module3.js"></script>
```

  这是最原始的JavaScript文件加载方式，如果把每一个文件看做是一个模块，那么他们的接口通常是暴露在全局作用域下，也就是定义在window对象中，不同模块的调用都是一个作用域。
  这种原始的加载方式暴露了一些显而易见的弊端：

- 全局作用域下容易造成变量冲突
- 文件只能按照``的书写顺序进行加载
- 开发人员必须主观解决模块和代码库的依赖关系
- 在大型项目中各种资源难以管理，长期积累的问题导致代码库混乱不堪

#### **CommonsJS**

------

  服务器端的NodeJS遵循CommonsJS规范，该规范核心思想是允许模块通过require方法来同步加载所需依赖的其它模块，然后通过exports或module.exports来导出需要暴露的接口。

```javascript
require("module");
require("../module.js");
export.doStuff = function(){};
module.exports = someValue;
```

  **优点：**

- 服务器端模块便于重用
- NPM中已经有超过45万个可以使用的模块包
- 简单易用

  **缺点：**

- 同步的模块加载方式不适合在浏览器环境中，同步意味着阻塞加载，浏览器资源是异步加载的
- 不能非阻塞的并行加载多个模块

  **实现：**

- 服务端的NodeJS
- Browserify：浏览器端的CommonsJS实现，可以使用NPM的模块，但是编译打包后的文件体积较大
- modules-webmake：类似Browserify，但不如Browserify灵活
- wreq：Browserify的前身

#### **AMD**

------

  Asynchronous Module Definition规范其实主要一个主要接口define(id?,dependencies?,factory);它要在声明模块的时候指定所有的依赖dependencies，并且还要当做形参传到factory中，对于依赖的模块提前执行。

```javascript
define("module",["dep1","dep2"],functian(d1,d2){
	return someExportedValue;
});
require（["module","../file.js"],function(module，file){});
```

  **优点**

- 适合在浏览器环境中异步加载模块
- 可以并行加载多个模块

  **缺点**

- 提高了开发成本，代码的阅读和书写比较困难，模块定义方式的语义不畅
- 不符合通用的模块化思维方式，是一种妥协的实现

实现

- RequireJS
- curl

#### **CMD**

------

  Commons Module Definition规范和AMD很相似，尽保持简单，并与CommonsJS和NodeJS的Modules规范保持了很大的兼容性。

```javascript
define(function(require,exports,module){
	var $=require("jquery");
	var Spinning = require("./spinning");
	exports.doSomething = ...;
	module.exports=...;
});
```

**优点：**

- 依赖就近，延迟执行
- 可以很容易在NodeJS中运行缺点
- 依赖SPM打包，模块的加载逻辑偏重

**实现**

- Sea.js
- coolie

#### ES6模块

------

  EcmaScript 6标准增加了JavaScript语言层面的模块体系定义。ES 6模块的设计思想， 是尽量静态化， 使编译时就能确定模块的依赖关系， 以及输入和输出的变量。Commons JS和AMD模块，都只能在运行时确定这些东西。

```javascript
import "jquery"
export function doStuff(){}
module "localModule"{}
```

  **优点**

- 容易进行静态分析
- 面向未来的Ecma Script标准

  **缺点**

- 原生浏览器端还没有实现该标准
- 全新的命令，新版的Node JS才支持

  **实现**

- Babel

  **大家期望的模块**
  系统可以兼容多种模块风格， 尽量可以利用已有的代码， 不仅仅只是JavaScript模块化， 还有CSS、图片、字体等资源也需要模块化。

### 安装Webpack

  WebPack是一款模块加载器兼打包工具， 它能把各种资源， 如JS、JSX、ES 6、SASS、LESS、图片等都作为模块来处理和使用。
  **安装：**

```bash
npm install webpack -g
npm install webpack-cli -g
```

  测试安装成功

~~~bash
webpack -v
webpack-cli -v
~~~

![在这里插入图片描述](https://gitee.com/xudongyin/img/raw/master/img/20200719184508.png)

### 配置

  创建 `webpack.config.js`配置文件

- entry：入口文件， 指定Web Pack用哪个文件作为项目的入口
- output：输出， 指定WebPack把处理完成的文件放置到指定路径
- module：模块， 用于处理各种类型的文件
- plugins：插件， 如：热更新、代码重用等
- resolve：设置路径指向
- watch：监听， 用于设置文件改动后直接打包

```js
module.exports = {
	entry:"",
	output:{
		path:"",
		filename:""
	},
	module:{
		loaders:[
			{test:/\.js$/,;\loade:""}
		]
	},
	plugins:{},
	resolve:{},
	watch:true
}
```

  直接运行`webpack`命令打包

### 使用webpack

1. 创建项目(普通文件夹可以容纳 js 文件和 html 文件就行)
2. 创建一个名为modules的目录，用于放置JS模块等资源文件
3. 在modules下创建模块文件，如hello.js，用于编写JS模块相关代码

```js
//暴露一个方法：sayHi
exports.sayHi = function(){
    document.write("<div>Hello Webpack</div>");
}
```

   4.在modules下创建一个名为main.js的入口文件，用于打包时设置entry属性

```js
//require 导入一个模块，就可以调用这个模块中的方法了
var hello = require("./hello");
hello.sayHi();
```

   5.在项目目录下创建webpack.config.js配置文件，**使用webpack命令打包**

```js
module.exports = {
	entry:"./modules/main.js",
	output:{
		filename:"./js/bundle.js"
	}
}
```

   6.在项目目录下创建HTML页面，如index.html，导入webpack打包后的JS文件

```html
<!doctype html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>狂神说Java</title>
    </head>
    <body>
        <script src="dist/js/bundle.js"></script>
    </body>
</html>
```

1. 在IDEA控制台中直接执行webpack；如果失败的话，就使用管理员权限运行即可！
2. 运行HTML看效果

  **说明**

```shell
# 参数--watch 用于监听变化，实时更新打包出来的js，实现热部署
webpack --watch
```

## 13、vue-router路由

> **说明**

  学习的时候，尽量的打开官方的文档

  Vue Router是Vue.js官方的路由管理器。它和Vue.js的核心深度集成， 让构建单页面应用变得易如反掌。包含的功能有：

- 嵌套的路由/视图表
- 模块化的、基于组件的路由配置
- 路由参数、查询、通配符
- 基于Vue js过渡系统的视图过渡效果
- 细粒度的导航控制
- 带有自动激活的CSS class的链接
- HTML5 历史模式或hash模式， 在IE 9中自动降级
- 自定义的滚动行为

### 安装

  **基于第一个`vue-cli`进行测试学习； 先查看node modules中是否存在vue-router**
  vue-router是一个插件包， 所以我们还是需要用npm/cnpm来进行安装的。打开命令行工具，**进入你的项目目录**，输入下面命令。

```shell
npm install vue-router --save-dev
```

如果在一个模块化工程中使用它，必须要通过Vue.use()显式声明使用，明确地安装路由功能：

```shell
import Vue from 'vue'
import VueRouter from 'vue-router'

Vue.use(VueRouter);
```

### 测试

1、先删除没有用的东西
2、`components` 目录下存放我们自己编写的组件
3、定义一个`Content.vue` 的组件

```html
<template>
	<div>
		<h1>内容页</h1>
	</div>
</template>

<script>
	export default {
		name:"Content"
	}
</script>
```

定义一个`Main.vue`组件

```html
<template>
    <div>
        <h1>首页</h1>
    </div>
</template>

<script>
    export default {
        name:"Main"
    }
</script>
```

4、安装路由，在src目录下，新建一个文件夹：`router`，专门存放路由，配置路由index.js，如下

```js
import Vue from'vue'
//导入路由插件
import Router from 'vue-router'
//导入上面定义的组件
import Content from '../components/Content'
import Main from '../components/Main'
//安装路由
Vue.use(Router) ;
//配置路由
export default new Router({
	routes:[
		{
			//路由路径
			path:'/content',
			//路由名称
			name:'content',
			//跳转到组件
			component:Content
			},{
			//路由路径
			path:'/main',
			//路由名称
			name:'main',
			//跳转到组件
			component:Mian
			}
		]
	});
```

5、在`main.js`中配置路由

```js
import Vue from 'vue'
import App from './App'

//导入上面创建的路由配置目录，这里router不能有大写
import router from './router'//自动扫描里面的路由配置

//来关闭生产模式下给出的提示
Vue.config.productionTip = false;

new Vue({
	el:"#app",
	//配置路由
	router,
	components:{App},
	template:'<App/>'
});
```

**注意：这里router不能有大写，且不能换别的单词，不然会报错**

6、在`App.vue`中使用路由

```html
<template>
	<div id="app">
		<!--
			router-link：默认会被渲染成一个<a>标签，to属性为指定链接
			router-view：用于渲染路由匹配到的组件
		-->
		<router-link to="/main">首页</router-link>
		<router-link to="/content">内容</router-link>
		<router-view></router-view>
	</div>
</template>

<script>
	export default{
		name:'App'
	}
</script>

<style></style>
```

## 14、实战快速上手

我们采用实战教学模式并结合ElementUI组件库，将所需知识点应用到实际中，以最快速度带领大家掌握Vue的使用；

### 创建工程

 **注意：命令行都要使用管理员模式运行**
1、创建一个名为hello-vue的工程

~~~bash
#1、首先需要进入到对应的目录 cd D:\Project\vue-study
#2、这里的hello-vue是顶日名称，可以根据自己的需求起名
vue init webpack hello-vue
~~~

2、安装依赖， 我们需要安装vue-router、element-ui、sass-loader和node-sass四个插件

```shell
#进入工程目录
cd hello-vue
#安装vue-routern 
npm install vue-router --save-dev
#安装element-ui
npm i element-ui -S
#安装依赖
npm install
# 安装SASS加载器
cnpm install sass-loader node-sass --save-dev
#启功测试
npm run dev
```

3、Npm命令解释：

- `npm install moduleName`：安装模块到项目目录下
- `npm install -g moduleName`：-g的意思是将模块安装到全局，具体安装到磁盘哪个位置要看npm config prefix的位置
- `npm install -save moduleName`：–save的意思是将模块安装到项目目录下，安装的是发布环境中需要用到的module， 并在package文件的dependencies节点写入依赖，-S为该命令的缩写
- `npm install -save-dev moduleName`:–save-dev的意思是将模块安装到项目目录下， 安装的是只有在开发环境下需要用到的module，并在package文件的devDependencies节点写入依赖，-D为该命令的缩写

### 创建登录页面

  把没有用的初始化东西删掉！
  在源码目录中创建如下结构：

- assets：用于存放资源文件
- components：用于存放Vue功能组件
- views：用于存放Vue视图组件
- router：用于存放vue-router配置

<img src="https://gitee.com/xudongyin/img/raw/master/img/20200719184519" alt="在这里插入图片描述" style="zoom:67%;" />

  **创建首页视图，在views目录下创建一个名为Main.vue的视图组件：**

```html
<template>
	<div>首页</div>
</template>
<script>
	export default {
			name:"Main"
	}
</script>
<style scoped>
</style>
```

 创建登录页视图在views目录下创建名为Login.vue的视图组件，其中el-*的元素为ElementUI组件；

```html
<template>
  <div>
    <el-form ref="loginForm" :model="form" :rules="rules" label-width="80px" class="login-box">
      <h3 class="login-title">欢迎登录</h3>
      <el-form-item label="账号" prop="username">
        <el-input type="text" placeholder="请输入账号" v-model="form.username"/>
      </el-form-item>
      <el-form-item label="密码" prop="password">
        <el-input type="password" placeholder="请输入密码" v-model="form.password"/>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" v-on:click="onSubmit('loginForm')">登录</el-button>
      </el-form-item>
    </el-form>

    <el-dialog
      title="温馨提示"
      :visible.sync="dialogVisible"
      width="30%"
      :before-close="handleClose">
      <span>请输入账号和密码</span>
      <span slot="footer" class="dialog-footer">
        <el-button type="primary" @click="dialogVisible = false">确 定</el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>
  export default {
    name: "Login",
    data() {
      return {
        form: {
          username: '',
          password: ''
        },

        // 表单验证，需要在 el-form-item 元素中增加 prop 属性
        rules: {
          username: [
            {required: true, message: '账号不可为空', trigger: 'blur'}
          ],
          password: [
            {required: true, message: '密码不可为空', trigger: 'blur'}
          ]
        },

        // 对话框显示和隐藏
        dialogVisible: false
      }
    },
    methods: {
      onSubmit(formName) {
        // 为表单绑定验证功能
        this.$refs[formName].validate((valid) => {
          if (valid) {
            // 使用 vue-router 路由到指定页面，该方式称之为编程式导航
            this.$router.push("/main");
          } else {
            this.dialogVisible = true;
            return false;
          }
        });
      }
    }
  }
</script>

<style lang="scss" scoped>
  .login-box {
    border: 1px solid #DCDFE6;
    width: 350px;
    margin: 180px auto;
    padding: 35px 35px 15px 35px;
    border-radius: 5px;
    -webkit-border-radius: 5px;
    -moz-border-radius: 5px;
    box-shadow: 0 0 25px #909399;
  }

  .login-title {
    text-align: center;
    margin: 0 auto 40px auto;
    color: #303133;
  }
</style>
```

**elementUI 的 el 标签解释**

|                           标签                           |                     解释                     |
| :------------------------------------------------------: | :------------------------------------------: |
|                          el-col                          |              整体，默认占24栅格              |
|                       el-container                       |                   主题区域                   |
|                        el-tooltip                        |                  提示框信息                  |
|                        el-header                         |                 内容头部区域                 |
|                         el-aside                         |                 左侧内容区域                 |
|                         el-main                          |                 主要内容区域                 |
|                         el-menu                          |                  整个导航栏                  |
|                        el-submenu                        |                单独一个导航栏                |
|                       el-menu-item                       |       单独一个导航栏里面的单独一个栏目       |
|                    el-menu-item-group                    | 一组导航栏 el-date-picker 组件事件格式化方式 |
|                        el-dialog                         |                  弹出对话框                  |
|                         el-table                         |                     表格                     |
|                     el-table-column                      |                    表格列                    |
|                      el-pagination                       |                   新增分页                   |
|                        el-select                         |                    选择框                    |
|                        el-button                         |                     按钮                     |
|                         el-form                          |                   表单提交                   |
|             el-form-item lable = “活动区域”              |                    表单域                    |
| el-input-number : (@change = handlechange – change 事件) |           数字输入框，可以实现加减           |
|                       el-tab-pane                        |              是 el-table 的分页              |




创建路由，在router目录下创建一个名为`index.js`的vue-router路由配置文件

```js
import Vue from 'vue'
import VueRouter from 'vue-router'
import Main from '../views/Main'
import Login from '../views/Login'

Vue.use(VueRouter);

export default new VueRouter({
  routes:[
    {
      path:'/login',
      name:'login',
      component: Login
    },{
    path: '/main',
      name:'main',
      component: Main
    }
  ]
});
```

APP.vue

```html
<template>
  <div id="app">
    <router-view></router-view>
  </div>
</template>

<script>


export default {
  name: 'App',

}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
```

main.js

```js
// The Vue build version to load with the `import` command
// (runtime-only or standalone) has been set in webpack.base.conf with an alias.
import Vue from 'vue'
import App from './App'
import router from "./router"
//导入element-ui和其css
import ElementUI from 'element-ui'
import 'element-ui/lib/theme-chalk/index.css'

Vue.use(router)  //可不写，router不能改，且不能大写
Vue.use(ElementUI)

/* eslint-disable no-new */
new Vue({
  el: '#app',
  router,
    /* element-ui官方规定用法 */
  render:h=>h(App)
})
```

如果出现错误: 可能是因为sass-loader的版本过高导致的编译错误，当前最高版本是8.x，需要退回到7.3.1 ；
去package.json文件里面的 "sass-loader"的版本更换成7.3.1，然后重新`cnpm install`就可以了；

## 15、路由嵌套

嵌套路由又称子路由，在实际应用中，通常由多层嵌套的组件组合而成。同样地，URL 中各段动态路径也按某种结构对应嵌套的各层组件，例如：

~~~bash
/user/foo/profile                     /user/foo/posts
+------------------+                  +-----------------+
| User             |                  | User            |
| +--------------+ |                  | +-------------+ |
| | Profile      | |  +------------>  | | Posts       | |
| |              | |                  | |             | |
| +--------------+ |                  | +-------------+ |
+------------------+                  +-----------------+
~~~

1、用户信息组件，在上面项目基础上，在 views/user 目录下创建一个名为 Profile.vue 的视图组件；

~~~HTML
<template>
    <div>
      个人信息
    </div>
</template>

<script>
    export default {
        name: "UserProfile"
    }
</script>

<style scoped>

</style>

~~~

2、用户列表组件在 views/user 目录下创建一个名为 List.vue 的视图组件；

~~~html
<template>
    <div>
      用户列表
    </div>
</template>

<script>
    export default {
        name: "UserList"
    }
</script>

<style scoped>

</style>
~~~

3、配置嵌套路由修改 router 目录下的 index.js 路由配置文件，代码如下

~~~javascript
import Vue from 'vue'
import Router from 'vue-router'

import Login from "../views/Login"
import Main from '../views/Main'

// 用于嵌套的路由组件
import UserProfile from '../views/user/Profile'
import UserList from '../views/user/List'

Vue.use(Router);

export default new Router({
  routes: [
    {
      // 登录页
      path: '/login',
      name: 'Login',
      component: Login
    },
    {
      // 首页
      path: '/main',
      name: 'Main',
      component: Main,
      // 配置嵌套路由
      children: [
        {path: '/user/profile', component: UserProfile},
        {path: '/user/list', component: UserList},
      ]
    }
  ]
});
~~~

说明：主要在路由配置中增加了 children 数组配置，用于在该组件下设置嵌套路由

4、修改首页视图，我们修改 Main.vue 视图组件，此处使用了 ElementUI 布局容器组件，代码如下：

~~~html
<template>
    <div>
      <el-container>
        <el-aside width="200px">
          <el-menu :default-openeds="['1']">
            <el-submenu index="1">
              <template slot="title"><i class="el-icon-caret-right"></i>用户管理</template>
              <el-menu-item-group>
                <el-menu-item index="1-1">
                  <router-link to="/user/profile">个人信息</router-link>
                </el-menu-item>
                <el-menu-item index="1-2">
                  <router-link to="/user/list">用户列表</router-link>
                </el-menu-item>
              </el-menu-item-group>
            </el-submenu>
            <el-submenu index="2">
              <template slot="title"><i class="el-icon-caret-right"></i>内容管理</template>
              <el-menu-item-group>
                <el-menu-item index="2-1">分类管理</el-menu-item>
                <el-menu-item index="2-2">内容列表</el-menu-item>
              </el-menu-item-group>
            </el-submenu>
          </el-menu>
        </el-aside>

        <el-container>
          <el-header style="text-align: right; font-size: 12px">
            <el-dropdown>
              <i class="el-icon-setting" style="margin-right: 15px"></i>
              <el-dropdown-menu slot="dropdown">
                <el-dropdown-item>个人信息</el-dropdown-item>
                <el-dropdown-item>退出登录</el-dropdown-item>
              </el-dropdown-menu>
            </el-dropdown>
          </el-header>

          <el-main>
            <router-view />
          </el-main>
        </el-container>
      </el-container>
    </div>
</template>

<script>
    export default {
        name: "Main"
    }
</script>

<style scoped lang="scss">
  .el-header {
    background-color: #B3C0D1;
    color: #333;
    line-height: 60px;
  }

  .el-aside {
    color: #333;
  }
</style>
~~~

说明：在元素中配置了用于展示嵌套路由,主要使用个人信息展示嵌套路由内容

### 参数传递

我们经常需要把某种模式匹配到的所有路由，全都映射到同个组件。例如，我们有一个 User 组件，对于所有 ID 各不相同的用户，都要使用这个组件来渲染。此时我们就需要传递参数了；

1、修改路由配置, 主要是在 path 属性中增加了 :id 这样的占位符，以及name属性

~~~javascript
{   
    path: '/user/profile/:id', 
    name:'UserProfile', 
    component: UserProfile
}
~~~

2、传递参数

此时我们将 to 改为了 **:to**，是为了将这一属性当成对象使用，注意 router-link 中的 name 属性名称 一定要和路由中的 name 属性名称匹配，因为这样 Vue 才能找到对应的路由路径；

~~~html
<router-link to="/user/profile">个人信息</router-link><!--原来样式-->
<router-link :to="{name: 'UserProfile', params: {id: 1}}">个人信息</router-link>
~~~

3、接收参数, 在参数所要接送的组件中 **(需要一个标签包围，不然会报错)**

~~~html
<div>{{ $route.params.id }}</div>
~~~

### 使用 props 的方式

1、修改路由配置 , 主要增加了 props: true 属性

~~~JavaScript
{path: '/user/profile/:id', name:'UserProfile', component: UserProfile, props: true}
~~~

2、传递参数和之前一样

~~~html
<router-link :to="{name: 'UserProfile', params: {id: 1}}">个人信息</router-link>
~~~

3、接收参数为目标组件增加 props 属性

~~~html
<template>
  <div>
    个人信息
    {{ id }}
  </div>
</template>

<script>
    export default {
      props: ['id'],
      name: "UserProfile"
    }
</script>

<style scoped>

</style>
~~~



### 组件重定向

重定向的意思大家都明白，但 **Vue 中的重定向是作用在路径不同但组件相同的情况下**，比如：

```javascript
{
  path: '/main',
  name: 'Main',
  component: Main
},
{
  path: '/goHome',
  redirect: '/main'
}
```
说明：这里定义了两个路径，一个是 /main ，一个是 /goHome，其中 /goHome 重定向到了 /main 路径，由此可以看出重定向不需要定义组件；

使用的话，只需要设置对应路径即可；

~~~html
<el-menu-item index="1-3">
    <router-link to="/goHome">回到首页</router-link>
</el-menu-item>
~~~

## 16、路由模式与 404

路由模式有两种

- hash：路径带 # 符号，如 http://localhost/#/login
- history：路径不带 # 符号，如 http://localhost/login

修改路由配置，代码如下：

~~~javascript
export default new Router({
  mode: 'history',
  routes: [
  ]
});
~~~

处理 404 创建一个名为 NotFound.vue 的视图组件，代码如下：

~~~html
<template>
  <div>
    页面不存在，请重试！
  </div>
</template>

<script>
  export default {
    name: "NotFount"
  }
</script>

<style scoped>

</style>
~~~

修改路由配置，代码如下：

~~~JavaScript
import NotFound from '../views/NotFound'
{
   path: '*',
   component: NotFound
}
~~~

### 路由钩子与异步请求

**beforeRouteEnter**：在进入路由前执行
**beforeRouteLeave**：在离开路由前执行

在 Profile.vue 上代码：

~~~JavaScript
export default {
    props: ['id'],
    name: "UserProfile",
    beforeRouteEnter: (to, from, next) => {
        console.log("准备进入个人信息页");
        next();
    },
    beforeRouteLeave: (to, from, next) => {
        console.log("准备离开个人信息页");
        next();
    }
}
~~~

参数说明：

- to：路由将要跳转的路径信息
- from：路径跳转前的路径信息
- next：路由的控制参数
  - next() 跳入下一个页面
  - next(’/path’) 改变路由的跳转方向，使其跳到另一个路由
  - next(false) 返回原来的页面
  - next( (vm)=>{ } ) **仅在 beforeRouteEnter 中可用，vm 是组件实例**

### 在钩子函数中使用异步请求

1、安装 Axios 

~~~bash
cnpm install --save vue-axios
npm install --save axios
~~~

2、main.js引用 Axios

~~~JavaScript
import axios from 'axios'
import VueAxios from 'vue-axios'
//这里两个参数位置不能颠倒
Vue.use(VueAxios, axios)
~~~

3、准备数据 ： 只有我们的 static 目录下的文件是可以被访问到的，所以我们就把静态文件放入该目录下。

~~~java
// 静态数据存放的位置
static/mock/data.json
~~~

4、在 beforeRouteEnter 中进行异步请求

~~~javascript
export default {
    props: ['id'],
    name: "UserProfile",
    beforeRouteEnter: (to, from, next) => {
        console.log("准备进入个人信息页");
        // 注意，一定要在 next 中请求，因为该方法调用时 Vue 实例还没有创建，此时无法获取到 this 对象，在这里使用官方提供的回调函数拿到当前实例
        next(vm => {
            vm.getData();
        });
    },
    beforeRouteLeave: (to, from, next) => {
        console.log("准备离开个人信息页");
        next();
    },
    methods: {
        getData: function () {
            this.axios({
                method: 'get',
                url: 'http://localhost:8080/static/mock/data.json'
            }).then(function (repos) {
                console.log(repos);
            }).catch(function (error) {
                console.log(error);
            });
        }
    }
}
~~~

