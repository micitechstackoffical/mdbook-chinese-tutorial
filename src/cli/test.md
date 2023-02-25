# test命令

写书时，有时需要自动化一些测试。例如[Rust编程书](https://doc.rust-lang.org/stable/book/) 用了很多可能过时的代码示例。因此能够自动测试这些代码示例就显得非常重要。

mdBook支持`test`命令，该命令将运行书中的所有可用测试。在目前，只支持`rustdoc`测试，在未来会支持其他的。

#### 禁用代码块上的测试

`rustdoc`不会测试包含`ignore`属性的代码块：

    ```rust,ignore
    fn main() {}
    ```

`rustdoc`不会测试指定非`Rust`编程语言的代码块：

    ```markdown
    **Foo**: _bar_
    ```

`rustdoc` *会* 测试没有指定任何编程语言的代码块：

    ```
    This is going to cause an error!
    ```

#### 指定一个目录

`test`命令可以将目录作为参数，用作书籍的根目录，而不是当前工作目录。


```bash
mdbook test path/to/book
```

#### --library-path

`--library-path` (`-L`)选项用于向库搜索路径中添加目录，`rustdoc`在构建和测试示例时会用到该库搜索路径。多个目录可以使用多个选项添加(`-L foo -L bar`)或逗号分隔列表(`-L foo,bar`)。该路径应指向`Cargo`
[生成缓存](https://doc.rust-lang.org/cargo/guide/build-cache.html) `deps`目录,该目录包含项目的构建输出。例如，如果Rust项目的书籍在名为`my-book`目录中，以下命令将在运行`test`时包含`crate`的依赖项：

```shell
mdbook test my-book -L target/debug/deps/
```

查看`rustdoc`命令行[documentation](https://doc.rust-lang.org/rustdoc/command-line-arguments.html#-l--library-path-where-to-look-for-dependencies)
获取更多信息.

#### --dest-dir

`--dest-dir` (`-d`)选项用于更改书籍的输出目录。该目录是相对于书本根目录的相对路径。如果
未指定，默认为`book.toml`配置文件中`build.build-dir`项的值或者是`./book`目录。

#### --chapter

`--chapter` (`-c`)选项允许您使用章节名称或章节的相对路径测试特定的章节。