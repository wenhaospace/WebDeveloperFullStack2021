# HTML Basis

## 1. 什么是HTML ?

**超文本标记语言**（英语：**H**yper**T**ext **M**arkup **L**anguage，简称：**HTML**）是一种用于创建网页的标准标记语言。HTML是一种基础技术，常与CSS、JavaScript一起被众多网站用于设计网页、网页应用程序以及移动应用程序的用户界面。网页浏览器可以读取HTML文件，并将其渲染成可视化网页。HTML描述了一个网站的结构语义随着线索的呈现，使之成为一种标记语言而非编程语言。

----

## 2. 页面结构

基本结构：

```html
<!DOCTYPE html>
<html>
  <head>
    <title>My Website</title>
  </head>
  <body>
    <!-- This is a comment-->
    <h1>My Website</h1>
    <p>This is my very first website</p>
  </body>
</html>
```

----

## 3. Meta标签

```html
<meta name="desciption" content="This is my website description">
<meta name="keywords" content="web development, web design">
<meta name="robots" content="NOINDEX,NOFOLLOW"><!--避免站点被谷歌，必应展示-->
```

-----

## 4. 基本排版

### 4.1 Headings 标题标签

```html
<!-- Headings -->
  <h1>Heading 1</h1>
  <h2>Heading 2</h2>
  <h3>Heading 3</h3>
  <h4>Heading 4</h4>
  <h5>Heading 5</h5>
  <h6>Heading 6</h6>
```

### 4.2 Paragraph 段落标签

```html
<!-- Paragraph -->
  <p>Lorem, ipsum dolor sit amet consectetur adipisicing elit. Tenetur quo atque dolorem quis doloribus deleniti amet <strong>mollitia excepturi minima</strong>. Ea quam fugiat reprehenderit facilis consequatur consequuntur voluptatem deleniti. Beatae harum vitae corporis quam repellat <em>dolore reiciendis ipsam ex alias libero. In ratione cupiditate cumque, accusamus consequuntur illo accusantium eveniet</em>, numquam beatae rem deserunt sapiente, sed amet <del>voluptatibus</del> corporis voluptates tempore harum repellat nostrum veniam. Neque magni dolorum, quisquam quaerat ipsum qui culpa officiis vitae aliquam. 
```

### 4.3 Line Break 换行标签

```html
<!-- Line Break -->
    <br>
    <br>
  Quae tenetur, impedit dolore ipsa voluptate adipisci earum officia cum suscipit recusandae sit optio. Vitae maiores animi vero
```

### 4.4 Horizontal Rule 水平线标签

```html
<!-- Horizontal Rule -->
  <hr>
  modi vel accusamus consequuntur enim blanditiis voluptates.</p>
```

-----

## 5. 超链接 & 图片

### 5.1 超链接

- External link 外部链接

```html
<!-- External link -->
  <p>
    <a href="http://google.com">Click for Google</a>
  </p>
  <p>
    <a href="http://google.com" target="_blank">Click for Google</a>
  </p>
```

- Internal link 内部链接

```html
<!-- Internal link -->
  <p>
    <a href="/HTMLSANDBOX/04_typography.html">Typography</a>
  </p>
```

### 5.2  图片

- Local image 本地图片

```html
<!-- Local image -->
  <img src="/HTMLSANDBOX/images/sample.jpg" alt="My Image" width="200">
```

- Remote image 远程图片

```html
<!-- Remote image -->
  <img src="https://source.unsplash.com/200x200/?building,water" alt="My Image 2">
```

-----

## 6.  列表 & 表格

### 6.1 列表

- Unordered lists 无序列表

```html
<!-- Unordered lists -->
  <ul>
    <li>Item 1</li>
    <li>Item 2</li>
    <li>Item 3</li>
    <li>Item 4</li>
  </ul>
```

- Ordered lists 有序列表

```html
<!-- Ordered lists -->
  <ol>
    <li>Item 1</li>
    <li>Item 2</li>
    <li>Item 3</li>
    <li>Item 4</li>
  </ol>
```

- Nested lists 嵌套列表

```html
<!-- Nested lists -->
  <ul>
    <li>Item 1</li>
    <li>Item 2
      <ul>
        <li>Nested Item 1</li>
        <li>Nested Item 2</li>
        <li>Nested Item 3</li>
        <li>Nested Item 4</li>
      </ul>
    </li>
    <li>Item 3</li>
    <li>Item 4</li>
  </ul>
```

### 6.2 表格

```html
<!-- Tables -->
  <table>
    <thead>
      <tr>
        <th>First Name</th>  
        <th>Last Name</th>  
        <th>Email</th>  
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>John</td>
        <td>Doe</td>
        <td>jdoe@gmail.com</td>
      </tr>
      <tr>
        <td>Kate</td>
        <td>Smith</td>
        <td>kate@gmail.com</td>
      </tr>
      <tr>
        <td>Jeff</td>
        <td>Wilson</td>
        <td>jeff@gmail.com</td>
      </tr>
    </tbody>
  </table>
```

----

## 7. 表单 & Input

```html
<form action="process.php">
    <!-- Text -->
    <div>
      <label for="name">Name</label><br>
      <input type="text" name="name" id="name" value="John Doe">
    </div>
    <!-- Email -->
    <div>
      <label for="email">Email</label><br>
      <input type="email" name="email" id="email" placeholder="Enter an Email">
    </div>
    <!-- Textarea -->
    <div>
      <label for="message">Message</label><br>
      <textarea name="message" id="message" cols="30" rows="10"></textarea>
    </div>
    <!-- Select -->
    <div>
      <label for="sex">Sex</label>
      <select name="sex" id="sex">
        <option value="male">Male</option>
        <option value="female" selected>Female</option>
        <option value="other">Other</option>
      </select>
    </div>
    <!-- Number -->
    <div>
      <label for="age">Age</label><br>
      <input type="number" name="age" id="age">
    </div>
    <!-- Date -->
    <div>
      <label for="birthdate">Birthdate</label><br>
      <input type="date" name="birthdate" id="birthdate">
    </div>
    <!-- Radio -->
    <div>
      <label for="membership">Membership</label>
      <input type="radio" name="membership" id="membership" value="simple">Simple
      <input type="radio" name="membership" id="membership" value="standerd" checked>Standerd
      <input type="radio" name="membership" id="membership" value="super">Super
    </div>
    <!-- Checkbox -->
    <div>
      <label for="likes">I Like...</label>
      <input type="checkbox" name="likes" id="likes" value="Bike">Bike
      <input type="checkbox" name="likes" id="likes" value="Car" checked>Car
      <input type="checkbox" name="likes" id="likes" value="boat">Boat
    </div>
    <!-- Input Submit -->
    <input type="submit" value="Submit">
    <!-- Button Submit -->
    <!-- <button type="submit">Submit</button> -->
    <!-- Button Reset -->
    <button type="reset">Reset</button>
  </form>
```

----

## 8. Block & Inline

## 9. Div, Span, ID, Class

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Div, span, id and class</title>
  <style>
    .card{
      border:1px solid #ccc;
      background: #f4f4f4;
      padding: 20px;
      margin-bottom: 10px;
    }

    .enhance{
      color: yellow;
      background-color: black;
    }
  </style>
</head>
<body>
  <div id="main-header" class="card">
    <h1>My Website</h1>
    <p>A site about me</p>
  </div>
  <ul id="main-nav">
    <li><a href="#">Home</a></li>
    <li><a href="#">About</a></li>
    <li><a href="#">Contact</a></li>
  </ul>
  <div id="about" class="card">
    <h3>About</h3>
    <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Autem quas dolor <span class="enhance">soluta ullam laudantium molestias, vel exercitationem odit in dicta accusantium illo</span> soluta ullam laudantium molestias, vel exercitationem odit in dicta accusantium illo maiores, quos ea vero dolore? Quod, earum officiis.</p>
  </div>

  <div id="contact" class="card">
    <h3>Contact Me</h3>
    <ul>
      <li>Address:50 main st,Boston MA</li>
      <li>Phone:5555555</li>
      <li>Email:me@something.com</li>
    </ul>
  </div>

  <div id="footer">
    <p>Copyright &copy; 2020</p>
  </div>
</body>
</html>
```

-----

## 10. HTML 实体标签

- Non breaking space 空格

```html
<!-- Non breaking space -->
  <p>Hello, my name is &nbsp;&nbsp; Brad</p>
```

- Greater than and less than 大于和小于

```html
<!-- Greater than and less than -->
  <p>5 &gt; 2</p>
  <p>5 &#62; 2</p>
  <p>1 &lt; 2</p>
  <p>1 &#60; 2</p>
```

- Copyright 版权

```html
<!-- Copyright -->
  <p>&copy;</p>
  <p>&reg;</p>
```

- Currency 货币

```html
<!-- Currency -->
  <p>&cent;</p>
  <p>&pound;</p>
  <p>&yen;</p>
  <p>&euro;</p>
```

- Suits 黑桃，红桃，梅花，方块

```html
  <!-- Suits -->
  <p>&spades;</p>
  <p>&clubs;</p>
  <p>&hearts;</p>
  <p>&diams;</p>
```

- Computer code 计算机代码

```html
<!-- Computer code -->
  <code>
    &lt;?php echo 'helo'?&gt;
  </code>
  <p>Save the file by pressing <kbd>Ctrl + S</kbd></p>
```

----

## 11. HTML5 语义化标签

- 头部标签 

```html
<!-- header -->
  <header id="header" class="hf">
    <h1>My WebSite</h1>
    <p>Just Another WebSite</p>
  </header>
```

- 主体标签

```html
<!-- main -->
  <main id="main">
  <!-- 主体内容 -->
  </main>
```

- 部分标签

```html
<section>
</section>
```

- 文章标签

```html
<article id="article">
  <h3>Article Two</h3>
  <p>Lorem ipsum dolor sit amet consectetur, adipisicing elit. Quibusdam enim eveniet iure omnis voluptatibus officiis totam porro quos, a libero blanditiis minus inventore repellat numquam nihil? Pariatur excepturi iure odio!</p>
</article>
```

- 侧边栏标签

```html
<aside id="main-right" class="card">
      <h3>Navigation</h3>
      
</aside>
```

- 导航标签

```html
<nav>
    <ul style="list-style: none;">
      <li><a href="#">Home</a></li>
      <li><a href="#">About</a></li>
      <li><a href="#">Contact</a></li>
    </ul>
</nav>
```

- 尾部标签

```html
<footer id="footer" class="hf">
    <p style="text-align: center;">Copyright &copy; 2020</p>
  </footer>
```



# 参考



