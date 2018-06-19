# css选择器

- **简单选择器（Simple selectors）：**：通过元素类型、`class` 或 `id` 匹配一个或多个元素。
- **属性选择器（Attribute selectors）**：通过 属性 / 属性值 匹配一个或多个元素。
- **伪类（Pseudo-classes）：**：匹配处于确定状态的一个或多个元素，比如被鼠标指针悬停的元素，或当前被选中或未选中的复选框，或元素是DOM树中一父节点的第一个子节点。
- **伪元素（Pseudo-elements）**：匹配处于相关的确定位置的一个或多个元素，例如每个段落的第一个字，或者某个元素之前生成的内容。 
- **组合器（Combinators）**：这里不仅仅是选择器本身，还有以有效的方式组合两个或更多的选择器用于非常特定的选择的方法。例如，你可以只选择divs的直系子节点的段落，或者直接跟在headings后面的段落。
- **多用选择器（Multiple selectors）**：这些也不是单独的选择器；这个思路是将以逗号分隔开的多个选择器放在一个CSS规则下面， 以将一组声明应用于由这些选择器选择的所有元素。

## 简单选择器

1. 类型选择器（元素选择器）

   ```html
   <p>I like red.</p>
   <div>I like blue.</div>
   ```

    ```css
   p{
       color:red;
   }
   div{
       color:blue;
   }
    ```

2. 类选择器

    ```html
   <ul>
       <li class = "first done">First</li>
       <li class = "second done">Second</li>
       <li class = "third">Third</li>
   </ul>
    ```

   ```css
   .first{
       font-weight:bold;
   }
   .done{
       text-decoration:line-through;
   }
   ```

3. ID选择器

   ```html
   <p id = "num"> 12345</p>
   <p id = "str">hello,girl.</p>
   ```

   ```css
   #num{
       font-family:cursive;
   }
   #str{
       font-family:monospace;
       text-transform:uppercase;
   }
   ```

4. 通用选择器

   它允许选择在一个页面中的所有元素。由于给每个元素应用同样的规则几乎没有什么实际价值，更常见的做法是与其他选择器结合使用。

## 属性选择器

1.存在和值选择器

- `[attr]`：该选择器选择包含 attr 属性的所有元素，不论 attr 的值为何。
- `[attr=val]`：该选择器仅选择 attr 属性被赋值为 val 的所有元素。
- `[attr~=val]`：该选择器仅选择具有 attr 属性的元素，选择包含val的所有元素，及时某元素属性还包含有其他属性值。

2. 子串值属性选择器（伪正则选择器）

- `[attr|=val]` : 选择attr属性的值是 `val` 或值以 `val-` 开头的元素（注意，这里的 “-” 不是一个错误，这是用来处理语言编码的）。
- `[attr^=val]` : 选择attr属性的值以 `val` 开头（包括 `val`）的元素。
- `[attr$=val]` : 选择attr属性的值以 `val` 结尾（包括 `val`）的元素。
- `[attr*=val]` : 选择attr属性的值中包含子字符串 `val` 的元素（一个子字符串就是一个字符串的一部分而已，例如，”cat“ 是 字符串 ”caterpillar“ 的子字符串）。

## 伪类

表示某一元素在某一状态下呈现出另一种样式。

:link     未点击的状态

:hover    用户鼠标悬停

:active  点击链接，在抬起前的状态

:visited  已经点击访问过的状态

:not   e.g. 

```
<p class = "fancy">good</p>
<p>hello</p>
/*选择类名不是fancy的<p>元素*/
p:not(.fancy){
    color:green;
}
```

:checked

