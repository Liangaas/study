关于 “button”

在日常编码的过程中，发现有时候使用

```
<input type = "submit">
<input type = "button">
<button></button>
```

查了一下资料，来做一个小结

1. \<input type = "submit">  当用户点击时，会自动提交表单。
2. \<input type = "button">  显示是一个按钮，但是如何没有js的处理，按下去是没有行为的。
3. \<button>\</button>   button元素可能包含内容，文本或图像等内容。还有一些旧版本的浏览器的兼容问题。注意：始终规定type的属性，不同浏览器默认的type属性不一样，Internet Explorer 的默认类型是 "button"，而其他浏览器中（包括 W3C 规范）的默认值是 "submit"。
4. 还有可以用其他标签加上样式，点击事件等伪装成一个按钮。