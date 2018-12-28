# Handling Event

在React中与在DOM中处理事件的方式是相似的。注意在**语法**上的区别。

- React事件用camelCase命名法命名，而不是DOM中的全小写命名；
- 在使用JSX时，需要传递一个**函数**作为处理函数，而不是DOM中的传递一个字符串。

另外一点是，在React中，不能在事件处理函数中使用`return false`的方式阻止事件的默认行为。
如果要在React中，阻止事件的默认行为，需要显式地调用`event.preventDefault()`方法。

在React中传入事件处理函数中的事件参数`e`是一个合成事件（synthetic event）。
React根据[W3C Spec DOM Level 3 Event](https://www.w3.org/TR/DOM-Level-3-Events/)定义了这些事件。
因此，开发者无需关心事件的跨浏览器兼容性。