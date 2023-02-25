# 其他渲染器

"渲染器"只是`mdbook`在书籍渲染过程中调用的一个程序。该程序通过`stdin`以JSON格式传递书籍内容和配置信息。一旦渲染器接收到这些信息，它就可以随心所欲地做任何事情。

有关使用渲染器的详细信息，请参见[配置渲染器](../format/configuration/Renderers.md)。

该社区开发了几个渲染器。有关其他渲染器的列表，请参见[第三方插件]wiki页面。

## 设置

本页将以简单的**单词计数程序**的形式指导您创建自己的渲染器。虽然它将用Rust编写，但也可以使用Python或Ruby之类的语言完成。

首先，您需要创建一个新的二进制程序，并添加`mdbook`作为依赖项。

```shell
$ cargo new --bin mdbook-wordcount
$ cd mdbook-wordcount
$ cargo add mdbook
```

当我们的`mdbook-wordcount`插件被调用时，`mdbook` 将通过插件的`stdin`向它发送一个JSON版本的[`RenderContext`]。为了方便起见，有一个[`RenderContext::from_json()`]构造函数，它将加载一个`RenderContext`。

这是渲染器加载该书所需的所有模板。

```rust
// src/main.rs
extern crate mdbook;

use std::io;
use mdbook::renderer::RenderContext;

fn main() {
    let mut stdin = io::stdin();
    let ctx = RenderContext::from_json(&mut stdin).unwrap();
}
```

> **注意:** 
  `RenderContext`包含`version`字段。这使渲染器能够确定它们是否与调用它的`mdbook`版本兼容。此`version`直接来自`mdbook`的`Cargo.toml`中的相应字段。

  建议渲染器使用[`semver`]crate检查此字段，如果可能存在兼容性问题，则发出警告。


## 检查书籍

现在我们的渲染器有一本书，让我们来计算每一章有多少单词！

因为`RenderContext`包含一个[`Book`]字段(`Book`)，而`Book`具有用于迭代`Book`中所有项的[`Book::iter()`]方法，所以这一步与第一步一样简单。

```rust

fn main() {
    let mut stdin = io::stdin();
    let ctx = RenderContext::from_json(&mut stdin).unwrap();

    for item in ctx.book.iter() {
        if let BookItem::Chapter(ref ch) = *item {
            let num_words = count_words(ch);
            println!("{}: {}", ch.name, num_words);
        }
    }
}

fn count_words(ch: &Chapter) -> usize {
    ch.content.split_whitespace().count()
}
```


## 启用该渲染器

现在我们已经具备了基本功能，如果想实际使用它。首先，需要安装程序。

```shell
$ cargo install --path .
```

然后，`cd`到需要计算单词数的书籍下，并更新其`book.toml`文件。

```diff
  [book]
  title = "mdBook Documentation"
  description = "Create book from markdown files. Like Gitbook but implemented in Rust"
  authors = ["Mathieu David", "Michael-F-Bryan"]

+ [output.html]

+ [output.wordcount]
```

当它将一本书加载到内存中时，`mdbook`将检查您的`book.toml`文件，并通过查找所有`output.*`配置项来找出要使用的渲染器。如果没有提供，则返回使用默认的HTML渲染器。

值得注意的是，如果您想要添加自定义渲染器，需要确保添加了HTML渲染器，即使它的配置项是空的。

现在你只需要像正常一样构建你的书，一切都应该*正常工作*。

```shell
$ mdbook build
...
2018-01-16 07:31:15 [INFO] (mdbook::renderer): Invoking the "mdbook-wordcount" renderer
mdBook: 126
Command Line Tool: 224
init: 283
build: 145
watch: 146
serve: 292
test: 139
Format: 30
SUMMARY.md: 259
Configuration: 784
Theme: 304
index.hbs: 447
Syntax highlighting: 314
MathJax Support: 153
Rust code specific features: 148
For Developers: 788
Alternative Backends: 710
Contributors: 85
```

我们之所以不需要指定`wordcount`渲染器的全名/路径，是因为`mdbook`将尝试通过约定*推断*程序的名称。`foo`渲染器的可执行文件通常称为`mdbook-foo`，在`book.toml`中有一个关联的`[output.foo]`条目。要明确告诉`mdbook`要调用什么命令（它可能需要命令行参数或是解释脚本），可以使用`command`字段。

```diff
  [book]
  title = "mdBook Documentation"
  description = "Create book from markdown files. Like Gitbook but implemented in Rust"
  authors = ["Mathieu David", "Michael-F-Bryan"]

  [output.html]

  [output.wordcount]
+ command = "python /path/to/wordcount.py"
```


## 配置

现在想象一下，你不想计算某个章节的字数（可能是生成的文本/代码等）。规范的做法是通过设置`book.toml`配置文件，向`[output.foo]`配置项中添加条目。

`Config`可以大致视为一个嵌套的hashmap，它允许您调用像`get()`这样的方法来访问配置的内容，使用`get_deserialized()`方法来检索值并自动反序列化为任意类型`T`。

为了实现这一点，我们将创建自己的可序列化的`WordcountConfig`结构，该结构将封装此渲染器的所有配置。

首先添加 `serde` 和 `serde_derive` 到 `Cargo.toml`,

```
$ cargo add serde serde_derive
```

然后创建配置数据结构：

```rust
extern crate serde;
#[macro_use]
extern crate serde_derive;

...

#[derive(Debug, Default, Serialize, Deserialize)]
#[serde(default, rename_all = "kebab-case")]
pub struct WordcountConfig {
  pub ignores: Vec<String>,
}
```

现在我们只需要从`RenderContext`中反序列化`WordcountConfig`，然后添加一个检查以确保跳过忽略的章节。

```diff
  fn main() {
      let mut stdin = io::stdin();
      let ctx = RenderContext::from_json(&mut stdin).unwrap();
+     let cfg: WordcountConfig = ctx.config
+         .get_deserialized("output.wordcount")
+         .unwrap_or_default();

      for item in ctx.book.iter() {
          if let BookItem::Chapter(ref ch) = *item {
+             if cfg.ignores.contains(&ch.name) {
+                 continue;
+             }
+
              let num_words = count_words(ch);
              println!("{}: {}", ch.name, num_words);
          }
      }
  }
```


## 输出和异常


当构建一本书时，将字数打印到终端是很好的，但将它们输出到某个文件中也是一个好主意。`mdbook`应该通过[`RenderContext`]中的`destination`字段告诉渲染器将任何输出放在哪里。

```diff
+ use std::fs::{self, File};
+ use std::io::{self, Write};
- use std::io;
  use mdbook::renderer::RenderContext;
  use mdbook::book::{BookItem, Chapter};

  fn main() {
    ...

+     let _ = fs::create_dir_all(&ctx.destination);
+     let mut f = File::create(ctx.destination.join("wordcounts.txt")).unwrap();
+
      for item in ctx.book.iter() {
          if let BookItem::Chapter(ref ch) = *item {
              ...

              let num_words = count_words(ch);
              println!("{}: {}", ch.name, num_words);
+             writeln!(f, "{}: {}", ch.name, num_words).unwrap();
          }
      }
  }
```

> **注意:** 无法保证目标目录存在或为空（`mdbook`可能会保留前面的内容，让渲染器进行缓存），因此最好使用`fs::create_dir_all()`创建它。
>
> 如果目标目录已经存在，不要假设它是空的。为了允许渲染器缓存以前运行的结果，`mdbook`可能会在目录中保留旧内容。

处理一本书时总是有可能发生错误（只需查看我们已经编写的所有`unwrap()`），因此`mdbook`会将非零退出代码解释为渲染失败。

例如，如果我们想确保所有章节都有*偶数*的单词，如果遇到奇数则出错，那么您可以这样做：

```diff
+ use std::process;
  ...

  fn main() {
      ...

      for item in ctx.book.iter() {
          if let BookItem::Chapter(ref ch) = *item {
              ...

              let num_words = count_words(ch);
              println!("{}: {}", ch.name, num_words);
              writeln!(f, "{}: {}", ch.name, num_words).unwrap();

+             if cfg.deny_odds && num_words % 2 == 1 {
+               eprintln!("{} has an odd number of words!", ch.name);
+               process::exit(1);
              }
          }
      }
  }

  #[derive(Debug, Default, Serialize, Deserialize)]
  #[serde(default, rename_all = "kebab-case")]
  pub struct WordcountConfig {
      pub ignores: Vec<String>,
+     pub deny_odds: bool,
  }
```

现在，我们重新安装该渲染器并构建一本书，

```shell
$ cargo install --path . --force
$ mdbook build /path/to/book
...
2018-01-16 21:21:39 [INFO] (mdbook::renderer): Invoking the "wordcount" renderer
mdBook: 126
Command Line Tool: 224
init: 283
init has an odd number of words!
2018-01-16 21:21:39 [ERROR] (mdbook::renderer): Renderer exited with non-zero return code.
2018-01-16 21:21:39 [ERROR] (mdbook::utils): Error: Rendering failed
2018-01-16 21:21:39 [ERROR] (mdbook::utils):    Caused By: The "mdbook-wordcount" renderer failed
```

您可能已经注意到的，插件子流程的输出会立即传递给用户。建议插件遵循"沉默规则"，仅在必要时生成输出（例如生成错误或警告）。

所有环境变量都传递到渲染器，允许您使用通常的`RUST_LOG`来控制日志记录的详细程度。

## 后续

虽然有点简单，但希望这个示例足以说明如何为`mdbook`创建一个其他渲染器。如果您觉得它缺少了一些东西，请在[问题跟踪器]中创建一个问题，以便我们可以改进用户指南。

本章开头提到的现有渲染器应该是如何实现的一个很好的例子，所以请随意浏览源代码或提问。


[第三方插件]: https://github.com/rust-lang/mdBook/wiki/Third-party-plugins
[`RenderContext`]: https://docs.rs/mdbook/*/mdbook/renderer/struct.RenderContext.html
[`RenderContext::from_json()`]: https://docs.rs/mdbook/*/mdbook/renderer/struct.RenderContext.html#method.from_json
[`semver`]: https://crates.io/crates/semver
[`Book`]: https://docs.rs/mdbook/*/mdbook/book/struct.Book.html
[`Book::iter()`]: https://docs.rs/mdbook/*/mdbook/book/struct.Book.html#method.iter
[`Config`]: https://docs.rs/mdbook/*/mdbook/config/struct.Config.html
[问题跟踪器]: https://github.com/rust-lang/mdBook/issues
