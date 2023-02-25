# 编辑器

除了提供可运行的代码片段外，mdBook还允许对其进行编辑。为了启用可编辑代码块，需要将以下内容添加到***book.toml***中：

```toml
[output.html.playground]
editable = true
```

如果要使特定代码块可编辑，需要添加`editable`属性：

~~~markdown
```rust,editable
fn main() {
    let number = 5;
    print!("{}", number);
}
```
~~~

效果如下：

```rust,editable
fn main() {
    let number = 5;
    print!("{}", number);
}
```

可以看到，在编辑框里新增一个`撤销更改`的按钮。

## 自定义编辑器

默认情况下，编辑器是[Ace](https://ace.c9.io/) 编辑器，但如果需要，可以通过提供不同的文件夹来覆盖该功能：

```toml
[output.html.playground]
editable = true
editor = "/path/to/editor"
```

注意，为了使编辑器的变更生效，需要重写`theme`文件夹中的`book.js`，因为它与默认Ace编辑器有一些耦合。
