# Pure Component

为了优化渲染性能，我们会使用shouldComponentUpdate(SCU)钩子提示React该组件及其子组件是否渲染。
除了手写SCU之外，React提供了内置的[`PureComponent`](https://reactjs.org/docs/react-api.html#reactpurecomponent)类。
`PureComponent`会对`props`与`state`进行一次浅比较（shallow comparision）
减少跳过必要更新的概率。

如果React组件的render函数在给定相同的props与state的时候渲染结果相同，就可以使用
`React.PureComponent`提高在某些情形下的性能。

> Shallow Compare会遍历一个`props`与`state`的所有的`key`，
> 当任意属性的值不严格相等的时候返回`true`。


> 这么一看大多数的组件都可以使用`PureComponent`优化。
>

## References
1. [shouldComponentUpdate](https://reactjs.org/docs/react-component.html#shouldcomponentupdate)