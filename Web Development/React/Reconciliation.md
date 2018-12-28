# Reconciliation

> [U] the process of making to opposite beliefs, ideas, or situations 
> agree.
>

在不同文档中，Reconciliation被翻译为[协调](https://react.docschina.org/docs/reconciliation.html)、
[一致性比较](https://react.css88.com/docs/reconciliation.html)等等。

Reconciliation是一个diff过程，它能够用最小的代价去更新视图。

## Motivation

当状态或者`props`更新的时候，组件的`render`函数会返回新的元素树。
React需要搞清楚如何高效地更新UI以匹配最新的元素树。然而，
目前[SOTA的算法](https://grfia.dlsi.ua.es/ml/algorithms/references/editsurvey_bille.pdf)复杂度为$O(n^3)$。
这个算法在实际中代价过于昂贵。取而代之，
React在**两个假设**的条件下实现了$O(n)$的启发式算法：

1. 两个不同类型的元素会产生不同的树；
2. 开发者通过`key`prop提示哪些子元素可能是稳定的。

在实际中，这些假设对于几乎所有的实际用例都是成立的。

## Diff算法

在diff两颗树的时候，React首先比较根元素，根据情况的不同，React会做出不同的操作。

### 两个元素类型不同

React会完全摧毁旧元素，然后重新从零开始构造新元素。

当摧毁一颗树的时候，旧DOM节点会被摧毁，对应组件实例会收到`componentWillUnmount()`。
构造新树的时候，新DOM节点会被插入到DOM中，对应组件实例会收到`componentWillMount()`、
然后是`componentDidMount`。任意与旧树关联的状态会丢失。

> 如果根元素的类型不同，则直接丢弃掉所有子树，重新构造新元素。
>

### 相同类型的DOM元素

当比较相同类型的React DOM元素。React会检查两者的属性（attributes），
保留相同的底层DOM节点，只更新变化的属性。

当处理完DOM节点之后，React在子节点上递归进行diff操作。

### 相同类型的组件元素

当组件更新的时候，实例仍然是相同的，因此它的状态可以在各次渲染时维持。
React更新底层组件实例的`props`以匹配新元素。
在底层实例上调用`componentWillReceiveProps()`、`componentWillUpdate()`.

接下来会调用`render()`方法，然后diff算法会在先前结果和新结果之间递归。

### 在子元素上的递归

算法会在子元素递归调用diff，但是会存在一个问题。当子元素的位置发生改变但是内容没发生改变，
naive的实现会使性能更差。

'''html
  <ul>
    <li>Duke</li>
    <li>Villanova</li>
  </ul>

  <ul>
    <li>Connecticut</li>
    <li>Duke</li>
    <li>Villanova</li>
  </ul>
'''

比如，如果逐个比较`<ul>`里面的`<li>`元素的话，React会丢弃掉原`<ul>`中的全部内容。

### keys

为了解决这一个问题，React给元素增加了一个`key`属性。当子元素有`key`属性时，
React会用`key`去匹配原树中的子元素。

```js
<ul>
  <li key="2015">Duke</li>
  <li key="2016">Villanova</li>
</ul>

<ul>
  <li key="2014">Connecticut</li>
  <li key="2015">Duke</li>
  <li key="2016">Villanova</li>
</ul>
```

通过比较，React知道`2014`是新的，`2015`、`2016`没变，只是位置变化了。

`key`不需要全局唯一，只需要在对应的兄弟元素之间唯一就可以。

关于使用数组的`index`作为`key`，当数组中元素不需要重新排序时是可以的。
但是如果需要重新排序的时候，性能就会变差。

## Trade off

Reconciliation算法是React的一个实现细节。开发者会时常细化该算法，以提高性能。

注意到，Reconciliation算法依赖于两个假设。如果这两个假设未能成立，性能会大大下降。

1. 两个不同类型的元素会产生不同的树；
2. 开发者通过`key`prop提示哪些子元素可能是稳定的。