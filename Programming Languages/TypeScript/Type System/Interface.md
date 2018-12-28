# Interfaces

接口对JS的运行时性能_没有任何影响_。



对于复杂类型，可以使用inline类型或者接口。

```typescript
// Use inline type annotation
declare let myPoint: { x: number; y:number}

// Use interface
interface Point {
  x: number
  y: number
}

declare let myPoint: Point
```

两者在这层意义上是等价的。



但是使用接口允许其他库作者在`myPoint`库的基础上进行扩展。

```typescript
// Lib myPoint.d.ts
interface Point {
  x: number
  y: number
}

declare let myPoint: Point

// Lib extendedLib.d.ts
interface Point {
  z: number
}

// Your code
let myPointer.z // Allowed
```

这是因为在TypeScript中的接口是open ended的。

> 所谓Open ended类似于React.setState一样，多次声明同一个`interface`，后面的声明不会完全替代前面的声明，在前面声明的基础上添加或者覆盖新的属性。
>
> ```typescript
> interface Point {
>     x: number
>     y: number
> }
> 
> interface Point {
>     z: number
> }
> 
> const point: Point = {
>     x: 1,
>     y: 2,
>     z: 3
> } // Allowed
> ```



## 类可以实现接口

如果你想要某个类的实例符合一个对象结构。你可以使用`interface`声明结构，然后用类`implements`这个接口。