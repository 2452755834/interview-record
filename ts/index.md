# ts面试题

## interface和type的区别

https://xiangshu233.cn/ts%20%E4%B8%AD%20type%20%E4%B8%8E%20interface%20%E7%9A%84%E5%8C%BA%E5%88%AB/#%E6%98%A0%E5%B0%84%E7%B1%BB%E5%9E%8B

- 都可以进行扩展，但是方式不同,接口使用继承，而别名使用交叉类型  
- 接口可以重复声明，会自动进行合并，而type不能重复声明
- 接口可以被类实现 type不行
- 接口主要用于声明对象、函数类型，而type除此之外还能声明基础类型、联合类型、元组等
- type 支持生成映射类型，interface 不支持 即 [k in keyof T]
- type可以搭配typeof使用

## implements和extends的区别

- 实现只能是class使用，主要是实现接口或者抽象类定义好的属性和方法
- 继承主要是继承子类已经定义好的属性和方法，并且进行扩展，而实现需要自己实现具体的方法内部逻辑

## const和readonly的区别

- const主要用来描述值，readonly主要用来描述类型
- const定义的变量在定义初期就要赋初值，后续不可再重新赋值，而readonly在class中定义阶段可以不赋初值，在构造函数内部还可以进行赋值

## 枚举和常量枚举的区别

- 常量枚举在编译阶段就会被删除，而直接把具体的值内联到使用的地方，避免额外的性能开销
- 枚举在编译阶段会生成一个对象，并且枚举可以使用计算属性，而常量枚举因为编译阶段会被删除所以不支持计算属性

## TypeScript 中 any、never、unknown、null & undefined 和 void 有什么区别？

any: 动态的变量类型（失去了类型检查的作用）。
never: 永不存在的值的类型。例如：never 类型是那些总是会抛出异常或根本就不会有返回值的函数表达式或箭头函数表达式的返回值类型，又或者是类型交叉，不可能存在的类型，例如string和number交叉就是never。
unknown: 任何类型的值都可以赋给 unknown 类型，但是 unknown 类型的值只能赋给 unknown 本身和 any 类型，代表未知类型，使用的时候必须对类型进行相应判断才能正常使用，更加安全。
null & undefined: 默认情况下 null 和 undefined 是所有类型的子类型。 就是说你可以把 null 和  undefined 赋值给 number 类型的变量。当你指定了 --strictNullChecks 标记，null 和 undefined 只能赋值给 void 和它们各自。
void: 没有任何类型。例如：一个函数如果没有返回值，那么返回值可以定义为void。

## ts的模块加载机制

https://ts.yayujs.com/reference/ModuleResolution.html#classic

## public、private、protected区别

- public：默认值，可以在类的内部和外部访问
- protected：只允许在类和子类的内部访问，实例不允许访问
- private：只允许在类的内部访问，子类和实例都不能访问

## declare和declare global区别

- 在.d.ts文件中声明declare就是全局声明的变量，只有在模块中declare才是声明的局部变量，这个时候就需要declare global声明全局变量
- 在文件中使用import/export都会默认成为模块

## mixin是什么

mixin其实主要就是调用该函数之后，给传入的构造函数都添加上相同的属性和方法，混入某些能力
- mixin首先接受一个构造函数
- 然后返回一个类并且继承与该构造函数
- 在返回的类内部，添加额外的属性和方法