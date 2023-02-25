# clean命令

`clean`命令用于删除生成的book和任何其他的输出物。

```bash
mdbook clean
```

#### 指定一个目录

`clean` 命令可以将目录作为参数，用作书本的根目录，而不是当前工作目录。

```bash
mdbook clean path/to/book
```

#### --dest-dir

`--dest-dir` (`-d`)选项用于更改书籍的输出目录。该目录是相对于书本根目录的相对路径。如果
未指定，默认为`book.toml`配置文件中`build.build-dir`项的值或者是`./book`目录。

```bash
mdbook clean --dest-dir=path/to/book
```

`path/to/book` 可以是绝对或相对路径。