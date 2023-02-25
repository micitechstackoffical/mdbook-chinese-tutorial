# 面向开发人员

虽然`mdbook`主要用作命令行工具，但您也可以直接导入底层库并用它来管理书籍。
它还有一个相当灵活的插件机制，如果您需要对书籍进行一些分析或输出不同的格式，您可以创建自定义插件和消费者（通常称为*backends*）。

这里的*面向开发人员*章节将向您展示`mdbook`的更高级用法。

开发人员可以通过以下两种主要方式来介入书籍的构建过程：

- [预处理器](preprocessors.md)
- [其他渲染器](backends.md)


## 构建过程

渲染书本项目的过程需要以下几个步骤。

1. 加载书籍
    - 解析`book.toml`，如果它不存在，则返回到默认的`Config`
    - 将书本章节载入内存
    - 查找应使用的预处理器和渲染器
2. 对于每一个渲染器:
   1. 执行所有的预处理器.
   2. 调用渲染器以呈现处理的结果.


## 将`mdbook`用作一个库

`mdbook`二进制文件只是`mdbook`crate的包装器，其功能以命令行方式对外提供。因此，基于`mdbook`很容易创建自己的程序，添加自己的功能（例如自定义预处理器）或调整构建过程。


了解如何使用`mdbook` crate的最简单方法是查看
[API Docs]。顶层文档解释了如何使用[`MDBook`]类型加载和构建一本书，而[config]模块则很好地解释了配置系统。


[`MDBook`]: https://docs.rs/mdbook/*/mdbook/book/struct.MDBook.html
[API Docs]: https://docs.rs/mdbook/*/mdbook/
[config]: https://docs.rs/mdbook/*/mdbook/config/index.html
