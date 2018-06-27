# DOM编程艺术（3）--前端开发笔记 --网易微专业

1. DOM事件

   * 事件流

   * 事件注册

   * 事件对象 

   * 事件分类

   * 事件代理

     1. 什么是DOM事件

        * 点击一个DOM元素

        * 键盘按下一个键

        * 输入框输入内容

        * 页面加载完成

           	---都会出发DOM事件

     2. 事件流

        一棵DOM树

     ​      window -> document -> html -> body -> div -> p -> a

     ​	当点击a标签，就会产生DOM click 事件

     ​	事件传递的过程：

        * capture phase 从树的最顶端开始，往下捕获，直到捕获到触发事件的节点的父元素。
        * target phase 事件触发过程
        * bubble phase 从当前节点的父节点开始，冒泡到顶层的windows对象。

     3. 事件注册

        * 事件注册

          eventTarget.addEventListener(type,listener[,useCapture])

          ```js
          var elem = document.ElementById('div');
          //事件触发就调用的函数
          var clickHandler = function(event){
              //TO DO
          }
          //事件处理函数可以是多个
          elem,addEventListener('click',clickHandler,false);
          //另外一种注册方式(这样的事件处理函数只有一个)
          elem.onclick = clickHandler;
          ```

        * 取消事件注册

          eventTarget.removeEventListener(type,listener[,useCapture]);

          ```js
          elem.romoveEventListener('click',clickHandler,false);
          //另一种方式，不建议
          elem.onclick = null;
          ```

        * 事件触发

          eventTarget.dispatchEven(type)

          ```js
          elem.dispatchEvent('click');//触发一个点击事件
          ```

          浏览器兼容（IE678） no capture

          * 事件的注册与取消

            -- attachEvent/detachEvent

          * 事件触发

            -- fireEvent(e)

            ```js
            var addEvent = document.addEventListener ?
                function(elem,type,listener,useCapture){
                    elem.addEventListener(type,listener,useCapture):
                    function(elem,type,listener,useCapture){
                        elem.attachEvent('on'+type,listener);
                    }
                };
            ```

            ```js
            var delEvent = document.removeEventListener ?
                function(elem,type,listener,useCapture){
                    elem.removeEventListener(type,listener,useCapture):
                    function(elem,type,listener,useCapture){
            elem.detachEvent('on' + type,listener);
                    }
                }
            ```

          ----主体都是DOM元素

     4. 事件对象：

        当事件被触发的时候，它会调用事件处理函数，在调用事件处理函数的时候，传入一些信息，这些信息代表当前事件的一些状态，这就是事件对象。当我们调用事件处理函数的时候，引擎会传入一个对象，这个对象就含有一些事件的状态信息，这就是事件对象。

        ```js
        var elem = document.ElementByID('div1');
        var clickHandler = function(event){
            //TO DO
        }
        elem.addEventListener('click',clickHandler,false);
        ```

        event ---事件对象

        在IE的低版本：

        ```js
        var elem = document.ElementById('div1');
        car clickHandler = function(event){
            //有时候放到window.even
            event = event || windows.event;
        }
        addEvent(elem,'click',clickHandler,false);
        ```

        * 属性

          -- type

          -- target(srcElement)

          -- currentTarget 当前处理事件的节点

        * 方法

          -- stopPropagation阻止传播，不让事件再冒泡上去

          阻止这个事件传到父节点

          even.stopPropagation()(W3C)

          even.cancelBubble = true (IE)

          -- preventDefault 阻止默认行为

          浏览器定义的默认行为

          Even.preventDefault(w3c)

          Even.returnValue = false(IE)

          -- stopImmediatePropagation 阻止冒泡（和第一个有不同）

          做了两件事，第一件和第一个方法一样，第二个是阻止当前节点的后续事件，在当前节点注册了两个事件，如果做了阻止，在后面的注册的事件都不会被触发。

     5. 事件分类

        | 类            | 继承于     | 事件                                                         |
        | ------------- | ---------- | ------------------------------------------------------------ |
        | Event         | -          | load unload error select abort                               |
        | UIEvent       | Event      | resize scroll                                                |
        | FocusEvent    | UIEvent    | blur focus focusin focusout                                  |
        | InputEvent    | UIEvent    | beforeinput input                                            |
        | KeyboardEvent | UIEvent    | keydown keyup                                                |
        | MouseEvent    | UIEvent    | click dbclick mousedown mouseenter mouseleave mousemove mouseout mouseover mouseup |
        | WheelEvent    | MouseEvent | wheel                                                        |

        1）MouseEvent

        | 事件类型   | 是否冒泡 | 元素    | 默认事件                   | 元素例子 |
        | ---------- | -------- | ------- | -------------------------- | -------- |
        | click      | yes      | element | focus/activation           | div      |
        | dbclick    | yes      | element | focus/activation select    | div      |
        | mousedown  | yes      | element | drag/srcoll text selection | div      |
        | mouseout   | yes      | element | none                       | div      |
        | mouseover  | yes      | element | none                       | div      |
        | mouseup    | yes      | element | context menu               | div      |
        | mousemove  | yes      | element | none                       | div      |
        | mouseenter | no       | element | none                       | div      |
        | mouseleave | no       | element | none                       | div      |

        mouseenter vs mouseover

        mouseover 移到一个div上，div上有子元素，移动到子元素上，都会触发famouseover。

        mouseenter嵌套的子元素不会触发mouseenter

        MouseEvent的对象

        * 属性

          -- clientX，clientY 鼠标点击的那一点距离浏览器的左边框和上边框距离

          -- screenX，screenY鼠标点击的那一点距离屏幕的左边框和上边框距离

          -- ctrlKey，shiftKey，altKey，metaKey 某个键是否被按下 true/fasle

          -- button(0,1,2) 鼠标的左中右键

        mouseEvent顺序

        * 从元素A上方移过

          -- mousemove ->  mouseover(A) -> mouseenter(A) -> mousemove(A) -> mouseout(A) -> mouseleave(A)

        * 点击元素

          -- mousedown -> [ mousemove] -> mouseup -> click

        例子 — 拖拽div

        ```html
        <div id = "div1"></div>
        ```

        ```css
        <style type = "text/css">
        #div1{
            position:absolute;top:0;left:0;
            border:1px solid #000;
            width:100px;height:100px;
        }
        </style>
        ```

        ```js
        var elem = document.getElementById('div1');
        var clientX,clientY,moving;
        var mouseDownHandler = function(event){
            event = event || windows.event;
            clientX = even.clientX;
            clientY = even.clientY;
            moving = !0;
        }
        var mouseMovingHandler = function(event){
            if(!moving) return;
            event = event || windows.event;
            var newClientX = event.clientX;
            var newClientY = event.clentY;
            var left = parseInt(elem.style.left) || 0;
            var top = parseInt(elem.style.top) ||0;
            elem.style.left = left(newClientX - clientX) +'px';
            elem.style.top = left(newClientY - clientY) +'px';
            clientX = newClientX;
            clientY = newClientY;     
         }
        var mouseUpHandler = function(event){
            moving =!1;
        }
        addEvent(elem,'mousedown',mouseDownHandler);
        addEvent(elem,'mousemove',mouseMoveHandler);
        addEvent(elem,'mouseup',mouseUpHandler);
        ```

     2）wheelEvent

     | 事件类型 | 是否冒泡 | 元素    | 默认事件                | 元素例子 |
     | -------- | -------- | ------- | ----------------------- | -------- |
     | wheel    | yes      | element | scroll or zoom document | div      |

     wheelEvent对象

     * 属性

       -- deltaModel

       -- deltaX

       -- deltaY

       -- deltaZ

     3）FocusEvent

     | 事件类型             | 是否冒泡 | 元素           | 默认事件 | 元素例子     |
     | -------------------- | -------- | -------------- | -------- | ------------ |
     | blur                 | no       | window,element | none     | window,input |
     | focus                | no       | window,element | none     | window,input |
     | focusin即将获得焦点  | no       | window,element | none     | window,input |
     | focusout即将失去焦点 | no       | window,element | none     | window,input |

     * 属性

       -- relatedTarget 

     4） inputEvent

     | 事件类型                          | 是否冒泡 | 元素    | 默认事件           | 元素例子 |
     | --------------------------------- | -------- | ------- | ------------------ | -------- |
     | beforeinput(输入框还没有内容)     | yes      | element | update DOM element | input    |
     | input（输入框显示用户修改的内容） | yes      | element | none               | input    |

     5）KeyboardEvent

     | 事件类型 | 是否冒泡 | 元素    | 默认事件                                 | 元素例子   |
     | -------- | -------- | ------- | ---------------------------------------- | ---------- |
     | keydown  | yes      | element | beforeinput/input focus/blur  activation | div，input |
     | keyup    | yes      | element | none                                     | div，input |

     * 属性

       -- key  （“string”）

       -- code （“string”）

       -- ctrlKey，shiftKey，altKey，metaKey （true/false）

       -- repeat 持续按着一个键 为true

     6) Event

     | 事件类型                 | 是否冒泡 | 元素                    | 默认事件 | 元素例子            |
     | ------------------------ | -------- | ----------------------- | -------- | ------------------- |
     | load(依赖网络加载的事件) | no       | window,document,element | none     | window,image,iframe |
     | unload                   | no       | window,document,element | none     | window              |
     | error（加载错误）        | no       | window,element          | none     | window,image        |
     | select（文本被选择）     | no       | element                 | none     | input,textarea      |
     | abort（退出）            | no       | window,element          | none     | window,image        |

     * window 

       — load 要加载的东西加载完成

       -- unload  

       -- error

       -- abort

     * image

       -- load 图片加载完成

       -- error 比如给图片赋值了错误的Url地址

       -- abort 

       ```html
       <image alt="photo" src = "http://www.163.com/photo.jpg" onerror"this.src= 'http://www.163.com/default.jpg'"/>
       ```

     7) UIEvent

     | 事件类型 | 是否冒泡 | 元素             | 默认事件 | 元素例子       |
     | -------- | -------- | ---------------- | -------- | -------------- |
     | resize   | no       | window,element   | none     | window, iframe |
     | scroll   | no/yes   | document,element | none     | document,div   |

     

     
