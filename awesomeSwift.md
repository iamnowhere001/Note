# Swift 是一门独断的语言。

关于“正确的” `Swift` 编码方法，作为本书作者，我们有着坚定的自己的看法。你会在本书中看到很多这方面的内容，有时候我们会把这些看法作为事实来对待。但是，归根结底，这只是我们的看法，你完全可以反对我们的观点。

`Swift` 还是一门年轻的语言，许多事情还未成定局。更糟糕的是，很多博客或者文章是不正确的，或者已经过时 (包括我们曾经写过的一些内容，特别是早期就完成了的内容)。不论你在读什么资料，最重要的事情是你应当亲自尝试，去检验它们的行为，并且去体会这些用法。带着批判的眼光去审视和思考，并且警惕那些已经过时的信息。


# Swift 风格指南

* 对于命名，在使用时能清晰表意是最重要的。因为 API 被使用的次数要远远多于被声明的次数，所以我们应当从使用者的角度来考虑它们的名字。尽快熟悉 Swift API 设计准则，并且在你自己的代码中坚持使用这些准则。
* 简洁经常有助于代码清晰，但是简洁本身不应该独自成为我们编码的目标。
* 务必为函数添加文档注释 —— 特别是泛型函数。
* 类型使用大写字母开头，函数、变量和枚举成员使用小写字母开头，两者都使用驼峰式命名法。
* 使用类型推断。省略掉显而易见的类型会有助于提高可读性。
* 如果存在歧义或者在进行定义的时候不要使用类型推断。(比如 func 就需要显示地指定返回类型)
* 优先选择结构体，只在确定需要使用类特有的特性或者是引用语言时才使用类。
* 除非你的设计就是希望某个类被继承使用，否则都应该将它们标记为 final。
* 除非一个闭包后面立即跟随有左括号，否则都应该使用尾随闭包(trailing closure)的语法。
* 使用 guard 来提早退出方法。
* 避免对可选值进行强制解包和隐式强制解包。它们偶尔有用，但是经常需要使用它们的话往往意味着其他不妥的地方。
* 不要写重复的代码。如果你发现你写了好几次类似的代码片段的话，试着将它们提取到一个函数里，并且考虑将这个函数转化为协议扩展的可能性。
* 试着去使用 map 和 reduce, 但这不是强制的。当合适的时候吗，使用 for 循环也无可厚非。高阶函数的意义是让代码可读性更高。但是如果使用 reduce 的场景难以理解的话，强行使用往往事与愿违，这种时候简单的 for 循环可能会更清晰。
* 试着去使用不可变值：除非你需要改变某个值，否则都应该使用 let 来声明变量。不过如果能让代码更加清晰高效的话，可以把它带来的副作用进行隔离。
* Swift 的泛型可能会导致非常长的函数签名。坏消息是我们现在除了将函数声明强制写成几行外，对此并没有什么好办法。我们会在示例代码中在这点上保持一贯性，这样你能看到我们是如何处理这个问题的。
* 除非你确实需要，否则不要使用 self.。在闭包表达式中，使用 self 是一个清晰的信号，表明闭包将会捕获 self。
* 尽可能地对现有的类型和协议进行扩展，而不是写一些全局函数。这有助于提高可读性，让别人更容易发现你的代码。

