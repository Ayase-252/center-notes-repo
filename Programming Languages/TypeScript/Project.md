# Project

TypeScript提供了一些项目组织的语言特性：

- 编译上下文（Compilation Context）；
- 声明空间（Declaration Spaces）；
- 模块



## 编译上下文

编译上下文是一组TypeScript将会解析与分析的文件，得到的信息用来判断有效性。



除了关于文件的信息，编译上下文还包含着一些正在使用的_编译器选项_。用`tsconfig.json`文件是定义编译上下文的好方法。





## 声明空间

在TypeScript里有两种声明空间：_变量_声明空间和_类型_声明空间。



### 类型声明空间

类型声明空间包含着可以用来作为类型注解的东西，像：

```typescript
class Foo {}
interface Bar {}
type Bas = {}
```

这意味着你现在可以使用`Foo`、`Bar`、`Bas`作为类型注解；



注意，尽管现在有了`interface Bar`，但是你**不能够将它用作一个变量**。因为它不属于_变量声明空间_。

```typescript
interface Bar {}
const bar = Bar //'Bar' only refers to a type, but is being used as a value here.
```



### 变量声明空间

变量声明空间包含着可以用来作为变量的东西。



同理，变量声明空间内的符号也不能用作类型注解：

```typescript
var foo = 123;
var bar: foo; // ERROR: "cannot find name 'foo'
```



## 命名空间

有的时候，在JavaScript中会有这样的用法：

```ts
(function(something) {
   something.foo = 123;
})(something || (something = {}))

console.log(something); // {foo:123}

(function(something) {
   something.bar = 456;
})(something || (something = {}))

console.log(something); // {foo:123, bar:456}
```

这种用法保证了内部向`something`添加属性的函数不会泄漏到全局作用域。在有文件模组之后，这个问题被解决了。但是这个模式在逻辑上组织一些函数仍然是非常有用地，因此TypeScript提供了`namespace`关键字指定一个命名空间：

```typescript
namespace Utility {
    export function log(msg) {
        console.log(msg);
    }
    export function error(msg) {
        console.error(msg);
    }
}

// usage
Utility.log('Call me');
Utility.error('maybe!');
```

它生成的代码：

```javascript
(function (Utility) {

// Add stuff to Utility

})(Utility || (Utility = {}));
```

对于大多数项目，推荐使用外部模组，仅使用`namespace`制作demo与移植JavaScript代码。