# 配置预处理器

预处理器是一种扩展，可以在原始Markdown源文件发送到渲染器之前对其进行修改。

默认情况下，会包含以下预处理器：

- `links`: 在章节中使用 `{{ #playground }}`, `{{ #include }}`, 和 `{{ #rustdoc_include }}` 引入其他外部文件。查看 [引入文件] 获取更多信息。
- `index`: 将所有名为`README.md`的章节文件转换为`index.md`。也就是说，所有`README.md`文件都将被渲染到`index.html`中。

内置的预处理器通过设置[`build.use-default-preprocessors`]选项禁用。

社区开发了几个预处理器。有关可用的预处理器列表，请参见[第三方插件]的wiki页面。

有关如何创建新的预处理器的信息，请参阅[新建预处理器]章节。

[引入文件]: ../mdbook.md#引入文件
[`build.use-default-preprocessors`]: ./general.md#build选项
[第三方插件]: https://github.com/rust-lang/mdBook/wiki/Third-party-plugins
[Preprocessors for Developers]: ../../for_developers/preprocessors.md

## 自定义预处理器配置

预处理器可以通过在 `book.toml` 中包含`preprocessor`表以及预处理器的名称来添加。
例如，如果您有一个名为`mdbook-example`的预处理器，则可以这样添加：

```toml
[preprocessor.example]
```

通过这样设置，mdBook将执行`mdbook-example`预处理器。

此表可以包括预处理器的其他配置项。
例如，`example`预处理器需要一些额外的配置选项：

```toml
[preprocessor.example]
some-extra-feature = true
```

## 将预处理器锁定到渲染器

可以通过以下方式显式指定该预处理器只能在特定渲染器下运行。

```toml
[preprocessor.example]
renderers = ["html"]  # example预处理器只能运行在HTML渲染器下
```

## 执行自己的命令

默认情况下，当您将`[processor.foo]`配置项添加到`book.toml`文件时，
`mdbook`将尝试调用`mdbook-foo`可执行程序。如果要使用不同的程序名或传入命令行参数，可以通过添加`command`字段覆盖。

```toml
[preprocessor.random]
command = "python random.py"
```

## 执行顺序

预处理器的执行顺序可以通过 `before` 和 `after` 字段控制。
例如，假设您希望`linenos`预处理器处理已经`{{#include}}`的行；
然后您希望它在内置的`links`预处理器之后运行，您可以使用`before` 或 `after`字段：


```toml
[preprocessor.linenos]
after = [ "links" ]
```

or

```toml
[preprocessor.links]
before = [ "linenos" ]
```

虽然是多余的，但也可以在同一个配置文件中指定上述两项。

通过`before` 和 `after`指定的具有相同优先级的预处理器将会按名称排序。
任何无限循环都将被检测到并产生错误。