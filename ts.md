# implements和extends的区别
- 实现只能是class使用，主要是实现接口或者抽象类定义好的属性和方法
- 继承主要是继承子类已经定义好的属性和方法，并且进行扩展，而实现需要自己实现具体的方法内部逻辑

# const和readonly的区别
- const主要用来描述值，readonly主要用来描述类型
- const定义的变量在定义初期就要赋初值，后续不可再重新赋值，而readonly在class中定义阶段可以不赋初值，在构造函数内部还可以进行赋值

# 枚举和常量枚举的区别
- 常量枚举在编译阶段就会被删除，而直接把具体的值内联到使用的地方，避免额外的性能开销
- 枚举在编译阶段会生成一个对象，并且枚举可以使用计算属性，而常量枚举因为编译阶段会被删除所以不支持计算属性

# TypeScript 中 any、never、unknown、null & undefined 和 void 有什么区别？
any: 动态的变量类型（失去了类型检查的作用）。
never: 永不存在的值的类型。例如：never 类型是那些总是会抛出异常或根本就不会有返回值的函数表达式或箭头函数表达式的返回值类型，又或者是类型交叉，不可能存在的类型，例如string和number交叉就是never。
unknown: 任何类型的值都可以赋给 unknown 类型，但是 unknown 类型的值只能赋给 unknown 本身和 any 类型，代表未知类型，使用的时候必须对类型进行相应判断才能正常使用，更加安全。
null & undefined: 默认情况下 null 和 undefined 是所有类型的子类型。 就是说你可以把 null 和  undefined 赋值给 number 类型的变量。当你指定了 --strictNullChecks 标记，null 和 undefined 只能赋值给 void 和它们各自。
void: 没有任何类型。例如：一个函数如果没有返回值，那么返回值可以定义为void。


# ts的模块加载机制
https://ts.yayujs.com/reference/ModuleResolution.html#classic


