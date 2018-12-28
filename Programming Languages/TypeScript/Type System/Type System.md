# TypeScript Type System



## 基本注解

TypeScript使用后置类型注解语法`:TypeAnnotation`。任何在类型声明空间里的符号都可以用作类型注解。



### 基本类型

JS的基本类型已经内置在类型系统中，这意味着可以使用`string`、`number`、`boolean`开箱即用。



### 数组

TypeScript给数组提供了专门的类型语法。语法基本上是在任何合法的类型注解后加上`[]`。



### 接口

在TypeScript，接口是将多个类型注解组合成单个命名注解的核心方式。

```ts
interface Name {
    first: string;
    second: string;
}

var name: Name;
name = {
    first: 'John',
    second: 'Doe'
};

name = {           // Error : `second` is missing
    first: 'John'
};
name = {           // Error : `second` is the wrong type
    first: 'John',
    second: 1337
};
```



### 行内类型注解

如果不想创建一个新的`interface`，也可以用`:{/*structure*/}`的方式注解任何你想要注解的东西。

```typescript
cosnt name: {
  first: string
  second: string
} = {
  first: 'John',
  second: 'Doe'
}
```

inline类型可以快速地为某个变量提供一个非类型注解(off type annotation)。如果你发现在重复的写同样的inline类型注解时，考虑重构为一个`interface`或者`type alias`。



### 特殊类型

TypeScript中有四种具有特殊意义的类型`any`、`null`、`undefined`、`void`



#### `any`

`any`类型兼容类型系统中所有`any`与所有的类型。这意味着**任何东西可以赋值给它**，以及**它可以赋值给任何东西**。



#### `null`与`undefined`

`null`和`undefined`字面值是被类型系统认为等效`any`类型。因此这些字面值可以被赋给任何其他类型的变量

```typescript
let num: number
let str: string

// These literals can be assigned to anything
num = null
str = undefined
```



#### `:void`

使用`:void`强调函数没有返回类型。



## 泛型

一些方法在编译时无法获知参数的类型，此时可以使用泛型注解。

```typescript
function reverse<T>(items: T[]): T[] {
  // ...
}

let sample = [1, 2, 3]
let reversed = reverse(sample) //类型推断：reverse 为 number[]

reversed[0] = '1' // Type '"1"' is not assignable to type 'number'.ts(2322)
```



## 并类型

有的时候，我们希望某个属性是多个类型中的一种，此时可以使用并类型（在类型注解中用`|`标识，像`string|number`）。



## 交类型

交类型用`&`标识，这意味着对象必须符合交类型中所有子类型的要求。



## 元组类型(Tuple Type)

JavaScript中没有第一类的元组支持。大家一般用数组代替元组。TypeScript扩展数组注解语法支持元组`:[typeOfMember1, typeOfMember2, ...]`。配合TypeScript的数组结构语法，使用元组的体验基本像第一类支持一样使用。

```typescript
let nameNumber: [string, number] = ['Jenny', 8675309]

let [name, num] = nameNumber
```



## 类型别名（Type Alias）

TypeScript提供了方便的语法来为你想要重用的类型注解命名。

```ts
type StrOrNum = string|number;

// Usage: just like any other notation
var sample: StrOrNum;
sample = 123;
sample = '123';

// Just checking
sample = true; // Error!
```



### 与`interface`的区别

不像`interface`，你可以给任何类型注解一个类型假名。在使用到交并类型时十分有用。

`interface`可以让`class`使用`implements`，`extends`语法。类型别名不行。