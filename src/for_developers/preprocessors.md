# 预处理器

*预处理器*在这本书被渲染之前就被加载，允许您改变这本书。可能的场景包括：

- 创建自定义助手 `\{{#include /path/to/file.md}}`
- 在基于Latex样式的表达式 (`$$ \frac{1}{3} $$`) 中替换为mathjax。

有关使用预处理器的详细信息，请参阅[配置预处理器]（../format/configuration/processors.md）。

## MDBook中的钩子

MDBook使用一种相当简单的机制来发现第三方插件。
将一个新配置项添加到`book.toml`（例如，`[preprocessor.foo]`用来添加`foo`预处理器），然后`mdbook`将尝试在构建过程中调用`mdbook-foo`程序。

一旦定义了预处理器并开始构建过程，mdBook就会执行两次`preprocessor.foo.command`键中定义的命令。
第一次运行预处理器以确定它是否支持给定的渲染器。
mdBook向进程传递两个参数：第一个参数是字符串`supports`，第二个参数是渲染器名称。
如果预处理器支持给定的渲染器，则以状态代码0退出，如果不支持，则返回非零退出代码。

如果预处理器支持渲染器，那么mdbook将再次运行它，将JSON数据传递到stdin。
JSON由`[context, book]`数组组成，其中`context`是序列化对象[`PreprocessorContext`]，`book`是包含书籍内容的[`Book`]对象。

预处理器将进行了任何修改后的[`Book`]对象以JSON格式返回到stdout。

最简单方法是创建自己的`Preprocessor`（例如在`lib.rs`中），然后创建一个shell二进制文件
将输入转换为正确的`Preprocessor`方法。为了方便起见，`examples/`目录中有[一个无运算预处理器示例]，可以很容易地适用于其他预处理器。

<details>
<summary>一个无运算预处理器示例</summary>

```rust
// nop-preprocessors.rs

{{#include ../examples/nop-preprocessor.rs}}
```
</details>

## 实现预处理器的提示

通过引入`mdbook`库，预处理器可以访问现有的处理书籍的基本能力。

例如，自定义预处理器可以使用
[`CmdPreprocessor::parse_input()`] 函数反序列化JSON并写入`stdin`。通过[`Book::for_each_mut()`]对`Book`的每一章节进行适当的改变，然后用`serde_json`crate写入到 `stdout`。

章节可以直接访问（通过递归遍历章节），也可以通过`Book::for_each_mut()`方法访问。

`chapter.content`只是一个markdown格式的字符串。虽然完全可以使用正则表达式或手动查找和替换，但您希望将输入处理成对计算机友好的结果。[`pulldown-cmark`][pc] crate实现了基于事件的高质量的Markdown解析器，[`pulldown-cmark-to-cmark`][pctc]crate则将事件转换回Markdown文本。

下面的代码块显示了如何删除markdown中的所有`emphasis`，而不会意外破坏文档。

```rust
fn remove_emphasis(
    num_removed_items: &mut usize,
    chapter: &mut Chapter,
) -> Result<String> {
    let mut buf = String::with_capacity(chapter.content.len());

    let events = Parser::new(&chapter.content).filter(|e| {
        let should_keep = match *e {
            Event::Start(Tag::Emphasis)
            | Event::Start(Tag::Strong)
            | Event::End(Tag::Emphasis)
            | Event::End(Tag::Strong) => false,
            _ => true,
        };
        if !should_keep {
            *num_removed_items += 1;
        }
        should_keep
    });

    cmark(events, &mut buf, None).map(|_| buf).map_err(|err| {
        Error::from(format!("Markdown serialization failed: {}", err))
    })
}
```

对于其他所有内容，请查看[完整示例][example]。

## 用不同的语言实现预处理器

mdBook利用`stdin`和`stdout`与预处理器进行通信，这使得用Rust以外的语言实现它们变得很容易。
下面的代码展示了如何在Python中实现一个简单的预处理器，它将修改第一章的内容。
下面的示例遵循上面显示的配置，`preprocessor.foo.command`实际上指向Python脚本。

```python
import json
import sys


if __name__ == '__main__':
    if len(sys.argv) > 1: # we check if we received any argument
        if sys.argv[1] == "supports": 
            # then we are good to return an exit status code of 0, since the other argument will just be the renderer's name
            sys.exit(0)

    # load both the context and the book representations from stdin
    context, book = json.load(sys.stdin)
    # and now, we can just modify the content of the first chapter
    book['sections'][0]['Chapter']['content'] = '# Hello'
    # we are done with the book's modification, we can just print it to stdout, 
    print(json.dumps(book))
```



[preprocessor-docs]: https://docs.rs/mdbook/latest/mdbook/preprocess/trait.Preprocessor.html
[pc]: https://crates.io/crates/pulldown-cmark
[pctc]: https://crates.io/crates/pulldown-cmark-to-cmark
[example]: https://github.com/rust-lang/mdBook/blob/master/examples/nop-preprocessor.rs
[一个无运算预处理器示例]: https://github.com/rust-lang/mdBook/blob/master/examples/nop-preprocessor.rs
[`CmdPreprocessor::parse_input()`]: https://docs.rs/mdbook/latest/mdbook/preprocess/trait.Preprocessor.html#method.parse_input
[`Book::for_each_mut()`]: https://docs.rs/mdbook/latest/mdbook/book/struct.Book.html#method.for_each_mut
[`PreprocessorContext`]: https://docs.rs/mdbook/latest/mdbook/preprocess/struct.PreprocessorContext.html
[`Book`]: https://docs.rs/mdbook/latest/mdbook/book/struct.Book.html
