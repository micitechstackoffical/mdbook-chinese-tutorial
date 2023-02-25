# mdBook特定功能

## 隐藏代码行

mdBook中有一个功能，它允许您通过在代码行前面加上`#`来隐藏代码行，[就像使用Rustdoc一样][rustdoc-hide]。这目前只适用于Rust语言代码块。

[rustdoc-hide]: https://doc.rust-lang.org/stable/rustdoc/documentation-tests.html#hiding-portions-of-the-example

```bash
# fn main() {
    let x = 5;
    let y = 6;

    println!("{}", x + y);
# }
```

效果如下：

```rust
# fn main() {
    let x = 5;
    let y = 6;

    println!("{}", x + y);
# }
```

代码块有一个眼球图标（<i class="fa fa-eye"></i>），用于切换隐藏代码行的可见性。

## Rust训练场

Rust语言代码块将自动有一个执行按钮（<i class="fa fa-play"></i>），该按钮将执行代码并在代码块的正下方显示输出。
这通过将代码发送到[Rust Playground]来实现。

```rust
println!("Hello, World!");
```

如果没有`main`函数，那么代码将自动封装一个。

如果您希望禁用代码块的播放按钮，可以在代码块上添加`noplayground`选项，如下所示：

~~~markdown
```rust,noplayground
let mut name = String::new();
std::io::stdin().read_line(&mut name).expect("failed to read line");
println!("Hello {}!", name);
```
~~~

或者，如果您希望禁用书本中所有代码块的播放按钮，可以像这样将配置写入`book.toml`。

```toml
[output.html.playground]
runnable = false
```

## Rust代码库属性


Rust代码块中可以包含其他属性，这些属性添加到`编程语言`之后,使用逗号、空格或制表符分隔。例如：

~~~markdown
```rust,ignore
# This example won't be tested.
panic!("oops!");
```
~~~

这些在使用[`mdbook test`]测试Rust示例时尤为重要。
这些属性与[rustdoc attributes]使用相同的属性，并添加了一些其他属性：

* `editable` — 启用[editor]。
* `noplayground` — 删除执行按钮，但仍可以进行测试。
* `mdbook-runnable` — 强制显示执行按钮。主要用于不需要测试但需要运行的代码示例，与`ignore` 属性结合使用。
* `ignore` — 不会执行测试，也不用显示执行按钮，但仍然需要高亮Rust语法。
* `should_panic` — 执行时，应该会产生异常。
* `no_run` — 代码在测试时编译，但不运行。执行按钮也不显示。
* `compile_fail` — 代码应该无法编译。
* `edition2015`, `edition2018`, `edition2021` — 强制使用特定的Rust版本。请参见[`rust.edition`]以进行全局设置。

[`mdbook test`]: ../cli/test.md
[rustdoc attributes]: https://doc.rust-lang.org/rustdoc/documentation-tests.html#attributes
[editor]: theme/editor.md
[`rust.edition`]: configuration/general.md#rust选项

## 引入文件

使用以下语法，可以将文件引入到书本中：

```hbs
\{{#include file.rs}}
```

文件的路径必须是当前源文件的相对路径。

mdBook会将包含的文件解释为Markdown。由于include命令通常用于插入代码片段和示例，因此您通常需要用 ```` ``` ````来封装，以显示文件内容而不解释它们。

````hbs
```
\{{#include file.rs}}
```
````

## 引入部分文件
Often you only need a specific part of the file, e.g. relevant lines for an
example. We support four different modes of partial includes:
通常，您只需要文件的特定部分，例如示例中的相关行。我们支持四种不同的引入模式，包括：

```hbs
\{{#include file.rs:2}}
\{{#include file.rs::10}}
\{{#include file.rs:2:}}
\{{#include file.rs:2:10}}
```

第一个命令只包含`file.rs`文件的第2行。
第二个命令只包含前10行，从11行到结尾会被忽略。
第三个命令包含从第2行到结尾，第1行会被忽略。
第四个命令只包含`file.rs`文件的第2行到第10行。

为了避免在修改包含的文件时导致书本出错，还可以使用锚点而不是行号来引入特定的部分。锚点是一对匹配的代码行。以正则表达式`ANCHOR:\s*[\w_-]+`开始，同样，以`ANCHOR_END:\s*[\w_-]+`结束。您可以在任何类型的注释行中放置锚点。

假如要包含以下文件:
```rs
/* ANCHOR: all */

// ANCHOR: component
struct Paddle {
    hello: f32,
}
// ANCHOR_END: component

////////// ANCHOR: system
impl System for MySystem { ... }
////////// ANCHOR_END: system

/* ANCHOR_END: all */
```

在书中，您需要这么引入：
````hbs
这是一个component:
```rust,no_run,noplayground
\{{#include file.rs:component}}
```

这是一个system:
```rust,no_run,noplayground
\{{#include file.rs:system}}
```

这是整个文件.
```rust,no_run,noplayground
\{{#include file.rs:all}}
```
````

锚点内包含锚点模式的行将被忽略。

## 引入一个文件，但隐藏除特定行外的所有行

`rustdoc_include`助手用于引入外部Rust文件中的完整示例代码，但最初仅以与`include`相同的方式显示代码行或用锚点的方式指定的代码行。

不在行号范围内或锚点之间的行仍将被引入在内，但它们将以`#`开头。通过这种方式，读者可以展开代码段以查看完整的示例，Rustdoc将在您运行`mdbook test`时使用完整的示例。

例如，考虑一个名为`file.rs` 的文件，其中包含以下Rust程序：

```rust
fn main() {
    let x = add_one(2);
    assert_eq!(x, 3);
}

fn add_one(num: i32) -> i32 {
    num + 1
}
```

通过使用以下语法，我们可以包含一个最初只显示第2行的代码段：

````hbs
要调用`add_one`函数，我们向它传递一个`i32`，并将返回的值绑定到`x`：

```rust
\{{#rustdoc_include file.rs:2}}
```
````

这与我们手动插入代码并使用`#`隐藏除第2行以外的所有内容一样：

````hbs
要调用`add_one`函数，我们向它传递一个`i32`，并将返回的值绑定到`x`：

```rust
# fn main() {
    let x = add_one(2);
#     assert_eq!(x, 3);
# }
#
# fn add_one(num: i32) -> i32 {
#     num + 1
# }
```
````

也就是说，它看起来像这样（单击"展开"图标查看文件的其余部分）：

```rust
# fn main() {
    let x = add_one(2);
#     assert_eq!(x, 3);
# }
#
# fn add_one(num: i32) -> i32 {
#     num + 1
# }
```

## 插入可执行的Rust的文件

使用以下语法，您可以将可运行的Rust文件插入书本中：

```hbs
\{{#playground file.rs}}
```

Rust文件的路径是与当前源文件的相对路径。


当单击`play`时，代码片段将被发送到[Rust Playground]进行编译和运行。结果被发送回并直接显示在代码下面。

以下是呈现的代码片段的样子：

{{#playground example.rs}}

在文件名之后传递的任何值都将作为代码块的属性。例如，`\{{#playground example.rs editable}}`将创建如下代码块：

~~~markdown
```rust,editable
# Contents of example.rs here.
```
~~~

`editable`属性将启用[editor]，如[Rust代码库属性](#rust代码库属性)所述。

[Rust Playground]: https://play.rust-lang.org/

## 控制页面 \<title\>

章节可以设置一个与目录中不一样的\<title\>，方法是在页面顶部附近包含`\{{#title ...}}`。

```hbs
\{{#title My Title}}
```
