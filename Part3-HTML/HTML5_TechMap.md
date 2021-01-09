# HTML5 技术概览

**HTML5** 是定义 [HTML](https://developer.mozilla.org/zh-CN/docs/HTML) 标准的最新的版本。 该术语通过两个不同的概念来表现：

- 它是一个新版本的**HTML**语言，具有新的元素，属性和行为，
- 它有更大的**技术**集，允许构建更多样化和更强大的网站和应用程序。这个集合有时称为HTML5和它的朋友们，不过大多数时候仅缩写为一个词 **HTML5**。

设计为所有Open Web开发人员都可以使用，此引用页面链接到许多关于HTML5技术的资源，根据其功能分为几个组。

- **语义**：能够让你更恰当地描述你的内容是什么。
- **通信**：能够让你和服务器之间通过创新的新技术方法进行通信。
- **离线 & 存储**：能够让网页在客户端本地存储数据以及更高效地离线运行。
- **多媒体**：使 video 和 audio 成为了在所有 Web 中的一等公民。
- **2D/3D 绘图 & 效果**：提供了一个更加分化范围的呈现选择。
- **性能 & 集成**：提供了非常显著的性能优化和更有效的计算机硬件使用。
- **设备访问 Device Access**：能够处理各种输入和输出设备。
- **样式设计**: 让作者们来创作更加复杂的主题吧！

## 1. 语义

### HTML5 中的区块和段落元素

HTML5 中新的区块和段落元素一览:`<section>`,`article`,`<nav>`,`<header>`,`<footer>`,`aside`,和`<hgroup>`

### 使用HTML5的音频和视频

`<audio>`和`<video>` 元素嵌入和允许操作新的多媒体内容。

### 表单的改进

看一下 HTML5 中对 web 表单的改进：[强制校验API](https://developer.mozilla.org/zh-CN/docs/Learn/HTML/Forms/Form_validation)，一些新的属性，一些新的`<input>`元素`type` 属性值 ，新的`<output>`元素。

### `<iframe>`的改进

使用 `sandbox`， `seamless`， 和 `srcdoc` 属性，作者们现在可以精确控制 `iframe`元素的安全级别以及期望的渲染

### MathML

允许直接嵌入数学公式。

### HTML5 兼容的解析器

用于把 HTML5 文档的字节转换成 DOM 的解释器，已经被扩展了，并且现在精确地定义了在所有情况下使用的行为，甚至当碰到无效的 HTML 这种情况。这就导致了 HTML5 兼容的浏览器之间极大的可预测性和互操作性。

----

## 2. 通信

### Web Sockets

允许在页面和服务器之间建立持久连接并通过这种方法来交换非 HTML 数据。

### Server-sent events

允许服务器向客户端推送事件，而不是仅在响应客户端请求时服务器才能发送数据的传统范式。

### WebRTC

这项技术，其中的 RTC 代表的是即时通信，允许连接到其他人，直接在浏览器中控制视频会议，而不需要一个插件或是外部的应用程序。

----

## 3. 离线 & 存储

### 离线资源：应用程序缓存

火狐全面支持 HTML5 离线资源规范。其他大多数针对离线资源仅提供了某种程度上的支持。

### 在线和离线事件

Firefox 3 支持 WHATWG 在线和离线事件，这可以让应用程序和扩展检测是否存在可用的网络连接，以及在连接建立和断开时能感知到。

### WHATWG 客户端会话和持久化存储 (又名 DOM 存储)

客户端会话和持久化存储让 web 应用程序能够在客户端存储结构化数据。

### IndexedDB

是一个为了能够在浏览器中存储大量结构化数据，并且能够在这些数据上使用索引进行高性能检索的 Web 标准。

### 自 web 应用程序中使用文件

对新的 HTML5 文件 API 的支持已经被添加到 Gecko 中，从而使 Web 应用程序可以访问由用户选择的本地文件。这包括使用 [**type**](https://developer.mozilla.org/zh-CN/docs/HTML/Element/Input#attr-type) file 的 `<input>` 元素的新的 [**multiple**](https://developer.mozilla.org/zh-CN/docs/HTML/Element/Input#attr-multiple) 属性针对多文件选择的支持。 还有 [`FileReader`](https://developer.mozilla.org/zh-CN/docs/DOM/FileReader)。

----

## 4. 多媒体

### 使用HTML5的音频和视频

`<audio>`和`<video>` 元素嵌入和允许操作新的多媒体内容。

### WebRTC

这项技术，其中的 RTC 代表的是即时通信，允许连接到其他人，直接在浏览器中控制视频会议，而不需要一个插件或是外部的应用程序。

### 使用 Camera API

允许使用，操作计算机摄像头，并从中存储图像。

### Track 和 WebVTT

`<track>`元素支持字幕和章节。[WebVTT](https://developer.mozilla.org/zh-CN/docs/HTML/WebVTT) 一个文本轨道格式。

----

## 5. 3D图像 & 效果

### HTML5 针对 `<canvas>` 元素的文本 API

HTML5 文本 API 现在由 `<canvas>` 元素支持。

### WebGL

WebGL 通过引入了一套非常地符合 OpenGL ES 2.0 并且可以用在 HTML5 [``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/canvas) 元素中的 API 给 Web 带来了 3D 图像功能。

### SVG

一个基于 XML 的可以直接嵌入到 HTML 中的矢量图像格式。

----

## 6. 性能 & 集成

### Web Workers

能够把 JavaScript 计算委托给后台线程，通过允许这些活动以防止使交互型事件变得缓慢。

### `XMLHttpRequest` Level 2

允许异步读取页面的某些部分，允许其显示动态内容，根据时间和用户行为而有所不同。这是在 [Ajax](https://developer.mozilla.org/zh-CN/docs/AJAX)背后的技术。

### 即时编译的 JavaScript 引擎

新一代的 JavaScript 引擎功能更强大，性能更杰出。

### History API

允许对浏览器历史记录进行操作。这对于那些交互地加载新信息的页面尤其有用。

### contentEditable 属性：把你的网站改变成 wiki !

HTML5 已经把 contentEditable 属性标准化了。了解更多关于这个特性的内容。

### 拖放

HTML5 的拖放 API 能够支持在网站内部和网站之间拖放项目。同时也提供了一个更简单的供扩展和基于 Mozilla 的应用程序使用的 API。

### HTML 中的焦点管理

支持新的 HTML5 `activeElement` 和 `hasFocus` 属性。

### 基于 Web 的协议处理程序

你现在可以使用 `navigator.registerProtocolHandler()` 方法把 web 应用程序注册成一个协议处理程序。

### `requestAnimationFrame`

允许控制动画渲染以获得更优性能。

### 全屏 API

为一个网页或者应用程序控制使用整个屏幕，而不显示浏览器界面。

### 指针锁定 API

允许锁定到内容的指针，这样游戏或者类似的应用程序在指针到达窗口限制时也不会失去焦点。

### 在线和离线事件

为了构建一个良好的具有离线功能的 web 应用程序，你需要知道什么时候你的应用程序确实离线了。顺便提一句，在你的应用程序又再回到在线状态时你也需要知道。

## 7. 设备访问

### 使用 Camera API

允许使用和操作计算机的摄像头，并从中存取照片。

### 触控事件

对用户按下触控屏的事件做出反应的处理程序。

### 使用地理位置定位

让浏览器使用地理位置服务定位用户的位置。

### 检测设备方向

让用户在运行浏览器的设备变更方向时能够得到信息。这可以被用作一种输入设备（例如制作能够对设备位置做出反应的游戏）或者使页面的布局跟屏幕的方向相适应（横向或纵向）。

### 指针锁定 API

允许锁定到内容的指针，这样游戏或者类似的应用程序在指针到达窗口限制时也不会失去焦点。

-----

## 8. 样式

[CSS](https://developer.mozilla.org/zh-CN/docs/CSS) 已经扩展到能够以一个更加复杂的方法给元素设置样式。这通常被称为 [CSS3](https://developer.mozilla.org/zh-CN/docs/CSS/CSS3), 尽管 CSS 已经不再是很难触动的规范，并且不同的模块并不全部位于 level 3：其中一些位于 level 1 而另一些位于 level 4，覆盖了所有中间的层次。

- 新的背景样式特性

  现在可以使用 [`box-shadow`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/box-shadow) 给逻辑框设置一个阴影，而且还可以设置 [多背景](https://developer.mozilla.org/zh-CN/docs/CSS/Multiple_backgrounds)。

- 更精美的边框

  现在不仅可以使用图像来格式化边框，使用 [`border-image`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/border-image) 和它关联的普通属性，而且可以通过 [`border-radius`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/border-radius) 属性来支持圆角边框。

- 为你的样式设置动画

  使用 [CSS Transitions](https://developer.mozilla.org/zh-CN/docs/CSS/Using_CSS_transitions) 以在不同的状态间设置动画，或者使用 [CSS Animations](https://developer.mozilla.org/zh-CN/docs/CSS/Using_CSS_animations) 在页面的某些部分设置动画而不需要一个触发事件，你现在可以在页面中控制移动元素了。

- 排版方面的改进

  作者们如今有更强大的能力来使自己的网页文字达到更佳的排版。他们不但可以控制 [`text-overflow`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/text-overflow) 和 [hyphenation](https://developer.mozilla.org/zh-CN/docs/CSS/hyphens)， 还可以给它设置一个 [阴影](https://developer.mozilla.org/zh-CN/docs/CSS/text-shadow) 或者更精细地控制它的 [decorations](https://developer.mozilla.org/zh-CN/docs/CSS/text-decoration)。感谢新的 [`@font-face`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/@font-face) 规则，现在我们可以下载并应用自定义的字体了。

- 新的展示性布局

  为了提高设计的灵活性，已经有两种新的布局被添加了进来：[CSS 多栏布局](https://developer.mozilla.org/zh-CN/docs/CSS/Using_CSS_multi-column_layouts), 以及 [CSS 灵活方框布局](https://developer.mozilla.org/zh-CN/docs/CSS/Flexbox)。

# 参考文档

https://developer.mozilla.org/zh-CN/docs/Web/Guide/HTML/HTML5