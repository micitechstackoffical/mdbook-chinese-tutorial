# 常用配置

您可以在***book.toml***文件中配置本书的参数。

下面是一个***book.toml***文件的示例：

```toml
[book]
title = "Example book"
authors = ["John Doe"]
description = "The example book covers examples."

[rust]
edition = "2018"

[build]
build-dir = "my-example-book"
create-missing = false

[preprocessor.index]

[preprocessor.links]

[output.html]
additional-css = ["custom.css"]

[output.html.search]
limit-results = 15
```

## 支持的配置项

需要注意的是，配置中指定的**任何**相对路径是以配置文件所在书本的根目录为准的相对路径。

### 常用配置项

下面关于你的书的一般信息。

- **title:** 本书的标题
- **authors:** 本书的作者
- **description:** 本书的描述，作为元信息添加到每个页面的`<head>`中
- **src:** 默认情况下，源目录位于根文件夹下名为`src`的目录中。但这可以通过配置文件中的`src`键进行配置。
- **language:** 本书的主要语言，用作HTML中的语言属性`<html lang=“en”>`。

**book.toml**
```toml
[book]
title = "Example book"
authors = ["John Doe", "Jane Doe"]
description = "The example book covers examples."
src = "my-src"  # the source files will be found in `root/my-src` instead of `root/src`
language = "en"
```

### Rust选项

Rust语言的选项，用于执行测试和练习。

```toml
[rust]
edition = "2015"   # the default edition for code blocks
```

- **edition**: 
  用于指定代码片段的Rust版本。默认是"2015"。可以使用`edition2015`，`edition2018` 或 `edition2021`注释控制各个代码块，例如：

  ~~~text
  ```rust,edition2015
  // This only works in 2015.
  let try = true;
  ```
  ~~~

### Build选项

这将控制书籍的生成过程。

```toml
[build]
build-dir = "book"                # the directory where the output is placed
create-missing = true             # whether or not to create missing pages
use-default-preprocessors = true  # use the default preprocessors
extra-watch-dirs = []             # directories to watch for triggering builds
```

- **build-dir:** 用于存放生成书籍的目录。默认情况下，是根目录中的`book/`。可以通过命令行参数`--dest-dir` 进行覆盖.
- **create-missing:** 默认情况下，`SUMMARY.md`中指定的任何缺失的文件都将在构建书本时创建（即`create-missing = true`）。如果这里是`false`，在生成过程中如果没有文件将会退出。
- **use-default-preprocessors:** 通过将此选项设置为“false”，禁用（`links` & `index`）的默认预处理器。

  如果您通过配置表声明了相同的或其他预处理器，它们将运行。

  - 为了清楚起见，在没有预处理器配置的情况下，默认的`links` & `index`将运行。
  - 设置 `use-default-preprocessors = false` 将会禁用默认的预处理器。
  - 例如，添加 `[preprocessor.links]` 将确保`links` 运行，不管配置项`use-default-preprocessors` 如何设置。
- **extra-watch-dirs**: `watch`和`serve`命令中用于监视的路径列表。当这些目录下的文件发生变更后将触发重建。如果您的书籍依赖于除`src`目录之外的文件，则非常有用。