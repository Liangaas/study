# DOM编程艺术（2）--前端开发笔记 --网易微专业

1. 属性操作

    HTML attribute -> DOM property

   每个html属性对应相应的DOM对象属性

   ```html
   <div>
   	<label for = "userName">用户名:</label>
       <input id = "userName" type = "text" class = "u-txt">
   </div>
   ```

   | input.    |            |
   | --------- | ---------- |
   | id        | "userName" |
   | type      | "text"     |
   | className | "u-txt"    |

   ps.因为class名字冲突->className

   | .label  |            |
   | ------- | ---------- |
   | htmlFor | "userName" |

   ps. for 名字冲突->htmlFor

   * property accessor

     缺点：通用性（名字异常）差、扩展性差

     优点：取到的是实用对象

     ```js
     //读
     input.className; //"u-txt"
     input["id"];//"userName"
     //写
     input.value = "aaa@163.com";
     input.disable = true;//不可操作，使用于input
     ```

     ```html
     <input class = "u-txt"
            maxlength = "10"
            disabled
            onclick = "showSuggest();">
     ```

     | input.    |                             |          |
     | --------- | --------------------------- | -------- |
     | className | "u-txt"                     | string   |
     | maxLength | 10                          | Number   |
     | disabled  | true                        | Boolean  |
     | onclick   | function onclick(even){...} | Function |

   * getAttribute/setAttribute(属性字符串，不能直接使用，但通用性好)

     var attribute  = element.getAttribute(attributeName);

     element.setAttribute(name,value);

     e.g. input.setAttribute("value","aaa@163.com");

   * dataset 在元素上保存数据

     ```html
     <div id = "user" data-id = "1234" data-account-name = "aa" data-email = "aaa123@163.com" data-mobile = "123">
         aaa
     </div>
     ```

     | div.dataset. |                  |
     | ------------ | ---------------- |
     | id           | "1234"           |
     | accountName  | "aa"             |
     | email        | "aaa123@163.com" |
     | mobile       | "123"            |

2. 样式操作

   CSS -> DOM

   <link rel = "stylesheet" href = "base.css"> ——element.sheet

   <style>

   ​	body {margin:30;}

   ​	p{color :#aaa;line-height:20px}	 

   </style> 								——element.sheet

   <p style = "color:red">paragraph</p>	——element.style

   | 分解                                                 | 对应的层次                                 |
   | ---------------------------------------------------- | ------------------------------------------ |
   | <style>...</style>                                   | element.sheet                              |
   | body {margin:30;}    p{color :#aaa;line-height:20px} | element.sheet.cssRules                     |
   | p{color :#aaa;line-height:20px}                      | element.sheet.cssRules[1]                  |
   | color :#aaa;line-height:20px                         | element.sheet.cssRules[1].style            |
   | p                                                    | element.sheet.cssRules[1].selectorText     |
   | 20px                                                 | element.sheet.cssRules[1].style.lineHeight |

   ps.  style -->CSSStyleDeclaration

   ps.内嵌样式 可以直接用element.style.color就可以获取样式的字体颜色了

   * 更新样式

     * element.style.xxx = xxx；来修改

       element.style.borderColor = 'red';

       element.style.color = 'red';

     * element.style.cssText = 'border-color:red;color:red'

     * 更新class

       在css里面增加一个新的类

       ```
       .invalid{
           border-color :red;
           color:red;
       }
       ```

       ```
       element.className += 'invalid';
       ```

   * 一次性修改很多元素的样式（换肤）--- 更换样式表

     <link rel  = "stylesheet" href = "base.css">

     <link id = "skin" rel  ="stylesheet" href = skin.spring.css>

     ```
     function changeSkin(){
         $('skin').href  ="skin.summer.css";
     }
     ```

   * 获取元素样式

     * element.style 只能获取内嵌样式，获取不一定是实际样式

     * window.getComputedStyle(element[,pseudoElt]);

       e.g. window.getComputedStyle(element).color;//获取颜色

       ps.ie9只能获取currentStyle

