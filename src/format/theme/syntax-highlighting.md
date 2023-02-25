# 语法高亮

mdBook使用[Highlight.js](https://highlightjs.org) 用于语法高亮显示的自定义主题。

自动语言检测已关闭，因此需要指定您使用的编程语言，如下所示：

~~~markdown
```rust
fn main() {
    // Some code
}
```
~~~

## 支持的语言

默认情况下支持这些语言，但您可以通过提供自己的`highlight.js` 文件来添加更多语言：

- apache
- armasm
- bash
- c
- coffeescript
- cpp
- csharp
- css
- d
- diff
- go
- handlebars
- haskell
- http
- ini
- java
- javascript
- json
- julia
- kotlin
- less
- lua
- makefile
- markdown
- nginx
- objectivec
- perl
- php
- plaintext
- properties
- python
- r
- ruby
- rust
- scala
- scss
- shell
- sql
- swift
- typescript
- x86asm
- xml
- yaml

## 自定义主题

与主题一样，用于语法高亮显示的文件也可以用自定义的文件覆盖

- ***highlight.js*** 通常，您不必覆盖此文件，除非您希望使用更新的版本。
- ***highlight.css*** highlight.js用于语法高亮显示的样式。

如果你想在`highlight.js`使用另一个主题，请从他们的网站下载，或自己制作，将其重命名为`highlight.css`，并将其放在书本的`theme`文件夹中。

这样就可以使用您自己的主题了，而不是默认主题。

## 隐藏代码行

mdBook中有一个功能，可以通过在代码行前面加上`#`来隐藏代码行。

```bash
# fn main() {
    let x = 5;
    let y = 6;

    println!("{}", x + y);
# }
```

将会渲染成如下代码：

```rust
# fn main() {
    let x = 5;
    let y = 7;

    println!("{}", x + y);
# }
```

**目前，这只适用于用`rust`注释的代码示例。因为它会与某些编程语言的语义发生冲突。未来，我们希望通过`book.toml`使其可配置，以便每个人都能从中受益。**



## 改进默认主题

如果您认为默认主题对于某些语言不太合适，或者想改进它，请[提交新问题](https://github.com/rust-lang/mdBook/issues) 解释你的想法。

您也可以将更新通过Pull-Request提交上来。

总的来说，主题应该是明亮而严肃的，没有太多浮华的色彩。
