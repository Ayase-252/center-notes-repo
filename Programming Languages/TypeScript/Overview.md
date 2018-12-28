# TypeScript

TypeScript是微软开发的一种JavaScript方言。是现有JavaScript的超集。

TypeScript需要通过编译器`tsc`编译成JavaScript代码运行。



[TOC]



## Why TypeScript

TypeScript有两个主要目标：

- 给JavaScript提供一个_可选的类型系统_；
- 为现在的JavaScript引擎提供未来JavaScript标准中计划支持的特性；

###  类型系统

为什么要向JavaScript中添加类型？

- 增加在开发者代码重构时的敏捷性：_比起在运行时出BUG，不如由编译器抓出错误_；
- 类型本身就是最好的一种文档形式：_函数声明是定理，函数体就是证明_；



但是严格执行类型对于一些应用来说过于正式了。为了降低使用类型的门槛，TypeScript具有以下特性。

#### JavaScript是有效的TypeScript

TypeScript本身就是JavaScript的超集，现有的JavaScript代码可以直接在TypeScript中使用；

#### 类型可以是隐式的

TypeScript会尝试从现有代码中推断出一些类型信息；

#### 类型可以是显式的

可以使用类型注解（annotation）：

- 帮助编译器，更重要地、帮助下一位读代码的开发者读懂代码；
- 确认编译器看到的就是你想的

TypeScript使用其他_可选_类型注解语言中（ActionScript，F#）流行地后置类型类型注解语法：

```typescript
let foo：number = 123
```

#### 类型是结构化的

TypeScript的类型系统是结构化的（Structural），即鸭子类型（duck typing）是第一类语言结构（language construct）。

> 鸭子类型并不关注对象的“类型”是否相同，而关注对象的“行为”是否和规定的类型的行为是否相同。

#### 类型错误不会阻止TypeScript编译为有效的JavaScript代码

为了从JavaScript迁移到TypeScript的方便，即便在代码中有编译错误，TypeScript仍然会尽可能将代码编译为有效的JavaScript。

因此开发者可以渐进地把项目从JavaScript升级到TypeScript。

#### Type is ambinent

TypeScript一个主要的设计目标就是兼容现有的JavaScript库。TypeScript通过_声明_的方式实现了这个目标。

### 未来的ES特性

TypeScript为只支持ES5的JS引擎提供了很多ES6特性。

