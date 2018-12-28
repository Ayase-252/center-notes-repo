## JavaScript

TypeScript是JavaScript的超集，但是TypeScript会尝试保护开发者踩JavaScript一些古怪的坑。



比如：

```typescript
[] + []
// JavaScript: ''
// TypeScript: Error: Operator '+' cannot be applied to types 'undefined[]' and 'undefined[]'.

{} + []
// JavaScript: 0
// TS： Operator '+' cannot be applied to types 'number' and '{}'.ts(2365)

[] + {}
// JS: '[object Object]'
// TS: Operator '+' cannot be applied to types 'number' and '{}'.ts(2365)

{} + {}
// JS: NaN
// TS:

'hello'  - 1
// JS: NaN
// TS: The left-hand side of an arithmetic operation must be of type 'any', 'number', 'bigint' or an enum type.

function add(a, b) {
  return
  // Unreachable code detected.ts(7027)
  	a + b
}
```



本质上，TypeScript是在已知类型信息的情况下检查（lint）JavaScript代码。



## Null and Undefined

### 与JSON与序列化的问题

JSON标准支持编码`null`，但是不支持编码`undefined`。因此值为`null`的属性在JSON序列化的时候**会包含**在结果中。



### 想法

TypeScript团队不使用`null`，没有导致任何问题。 [TypeScript coding guidelines](https://github.com/Microsoft/TypeScript/wiki/Coding-guidelines#null-and-undefined)。

Douglas Crockford认为[`null` is a bad idea](https://www.youtube.com/watch?v=PSGEjv3Tqo0&feature=youtu.be&t=9m21s) ，我们应该只使用`undefined`。

> 使用`null`或者`undefined`是高度有争议性的问题。

NodeJS在回调函数中用`null`表示**目前不可用的东西**。



