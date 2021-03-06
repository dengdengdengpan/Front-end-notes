# Interview Answer

### 面试技巧

- **遇到比较抽象的题目就具体化，通过举例；遇到比较抽象的题目就具体化，最后通过阐述拔高**
- 抽象题目搜知乎；具体代码题目搜 Stackoverflow 或者博客
- 【XXX 的原理】这种题目一般是说源代码的思路

## HTML
#### 1.怎么理解 HTML 语义化

- HTML 语义化是什么
  - **HTML 语义化是指在开发中应使用正确的 HTML 标签来表达正确的意图，从而让页面具有良好的结构和含义**
- 举例 
  - 比如在开发时，段落就用 `<p>` 标签，边栏就用 `<aside>` 标签，导航栏就用 `<nav>` 标签，页脚就是用 `<footer>` 标签，这样页面具有良好的结构
  - 再比如说，一个提交按钮应该使用 `<button>` 标签而不是 `<div>` 标签，这样做的好处是 `<button>` 标签不仅提供了按钮样式，还提供了键盘的可访问性，比如：使用 tab 健切换按钮，使用回车键点击按钮
- 优点
  - **更便于开发**：语义化的 HTML 标签具有更高的可读性，便于理解和后期维护
  - **更利于SEO**：相比非语义化的 `<div>` 标签，搜索引擎更重视 `<h1>`、`<a>` 标签里的关键字，使用语义化的 HTML 可使网页具有更高的搜索排名，更容易被用户搜到
  - **提升用户体验**：表单中，`<label`> 标签清晰地与表单标签相关联，点击 `<label>` 标签就可使相关联的表单标签获取焦点；`<img>` 标签中添加描述图片内容的 `alt` 属性，再这样当图片无法显示时浏览器会显示 `alt` 属性的内容，用户也能知道图片的大概内容
  - **更便于其它设备解析**：语义化的 HTML 具有良好的结构和含义，它使页面更加友好，便于屏幕阅读器、盲人阅读器等设备解析
  - **更加适配移动端**：语义化的 HTML 文件比非语义化的 HTML 文件更轻便，并且更易于响应式开发

### CSS

#### 1.怎样在文档中应用 CSS

- **外部样式表**：将 CSS 规则写在单独的 `.css` 文件中，并从 HTML 的 `<link`> 元素引入它的情况

  ```html
  <!DOCTYPE html>
  <html>
    <head>
      <meta charset="UTF-8">
      <link ref="stylesheet" href="./index.css">	
    </head>
  </html>
  ```

- **内部样式表**：将 CSS 规则写在 HTML 文件 `<head>` 标签下的 `<style`> 标签中

  ```html
  <!DOCTYPE html>
  <html>
    <head>
      <meta charset="UTF-8">
      <style>
        body {
          margin: 0;
          font-size: 14px;
        }
      </style>
    </head>
  </html>
  ```

- **内联样式**：将 CSS 规则写在 HTML 元素的 `style` 属性中，其特点是每个 CSS 规则只影响一个元素

  ```html
  <!DOCTYPE html>
  <html>
    <head>
      <meta charset="UTF-8">
    </head>
    <body>
      <div style="padding: 10px; color: red; font-weight: bold;"></div>
    </body>
  </html>
  ```

#### 2.有哪些选择器？以及选择器的优先级怎么确定？

- 选择器有：

  - **通用选择器**，用 `*` 表示所有元素

  - **元素选择器**

  - **类选择器**

  - **ID 选择器**

  - **关系选择器**

    ```html
    <!DOCTYPE html>
    <html>
      <head>
       <meta charset="UTF-8">
       <style>
         // 后代选择器
         ul li {}
         // 直接后代选择器
         ul > li {}
         // 相邻兄弟选择器
         li + div {}
         // 通用兄弟选择器
         .active ~ span {} 
       </style>
      </head>
      <body>
        <ul>
          <li></li>
          <div></div>
          <li class="active"></li>
          <span></span>
          <li></li>
          <li></li>
          <span></span>
        </ul>
      </body>
    </html>
    ```

  - **属性选择器**，通过已经存在的属性名和属性值匹配元素

    ```css
    /* 匹配存在 title 属性的 <a> 元素 */
    a[title] {}
    
    /* 匹配存在 href 属性且属性值为 https://baidu.com 的 <a> 元素 */
    a[href="https://baidu.com"] {}
    
    /* 匹配存在 href 属性且属性值包含"example"的 <a> 元素 */
    a[href*="example"] {}
    
    /* 匹配存在 href 属性且属性值开头是".test"的 <a> 元素 */
    a[href^=".test"] {}
    
    /* 匹配存在 href 属性且属性值结尾是".org"的 <a> 元素 */
    a[href$=".org"] {}
    
    /* 匹配存在 class 属性且属性值包含以空格分隔的"logo"的 <a>元素 */
    a[class~="logo"] {}
    ```

  - **伪类选择器**，CSS 伪类是加在选择器后面用来指定元素状态的关键字，常用的伪类有：`:link`、`:visited`、`:hover`、`:focus`、`:active`、`:checked`、`:disabled`、`:first-child`、`:last-child`、`:nth-child`、`:not`

  - **伪元素选择器**，常用的伪元素选择器有：`::after`、`::before`、`::first-line`、`::first-letter`、`::selection`

- 优先级是什么？

  浏览器通过优先级来判断哪些属性值与一个元素最为相关，从而在该元素上应用这些属性值。优先级是基于不同种类选择器组成的规则

- 优先级是怎么确定的？

  - **选择器越具体优先级越高**，具体到计算层面，一个选择器的优先级表述为 0，0，0，0，可以将它构成个十百千的四位数，其中：
    - **千位**：**声明在内联样式中则该位得一分**，由于内联样式没有选择器，所以其总得分为 1000
    - **百位**：存在**ID 选择器**则该位得一分
    - **十位**：存在**类选择器、伪类选择器、属性选择器**则该位得一分
    - **个位**：存在**元素选择器、伪元素选择器**则该位得一分
    - **通配选择器(\*)具有 0 优先级，这与根本没有优先级有区别**
    - **组合符(+、>、~、' ')、否定伪类(:not)对优先级没有影响，但是 :not()括号内声明的选择器会影响优先级**
  - **排在后面的选择器覆盖前面的**
  - **带有 !import 规则的优先级最高**，但是要少用。追问：怎么少用？
    - 可以更好地利用 CSS 及联属性
    - 使 CSS 选择器变得更加具体，从而获得更高的优先级

#### 3.两种盒模型

#### 4.BFC 是什么

#### 5.什么是 flex 布局，以及常用的属性

#### 6.什么是 grid 布局，以及常用的属性

#### 7.清除浮动

#### 8.居中问题

- 水平居中
- 垂直居中
- 水平垂直居中

#### 9.列出 display 属性的全部值

#### 10.定位

#### 11.响应式设计和自适应设计有什么不同





