# build命令

build命令用于构建该书籍:

```bash
mdbook build
```

它将尝试解析您的`SUMMARY.md`文件，以了解书籍的目录结构并获取相应的文件。注意`SUMMARY.md`中提到的文件
但不存在的将会被创建。

The rendered output will maintain the same directory structure as the source for
convenience. Large books will therefore remain structured when rendered.
为了方便，构建输出的目录结构将保持与源目录结构相同。大型书籍在渲染时将保持结构化。

#### 指定输出目录

`build`命令可以传入一个目录参数，作为书籍的根目录而不是当前的工作目录。

```bash
mdbook build path/to/book
```

#### --open

当您使用`--open` (`-o`) 标志时，mdbook将在构建后使用默认浏览器打开该书籍。

#### --dest-dir

`--dest-dir` (`-d`)选项用于更改书籍的输出目录。该目录是相对于书本根目录的相对路径。如果
未指定，默认为`book.toml`配置文件中`build.build-dir`项的值或者是`./book`目录。

-------------------

***注意：*** *build命令从源目录复制所有文件（不包括扩展名为“.md”的文件）到构建目录中*