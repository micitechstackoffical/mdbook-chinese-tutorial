# watch命令

`watch`命令在您希望每当文件更改后都重新渲染时非常有用。您可以在每次变更文件时重复执行`mdbook build`命令。但使用一次`mdbook watch`将监视文件变化，每当您修改文件时自动触发构建；这包括重新创建已删除但仍在`SUMMARY.md`中的文件！

#### 指定一个目录

`watch`命令可以将目录作为参数，用作书籍的根目录，而不是当前工作目录。

```bash
mdbook watch path/to/book
```

#### --open

当您使用`--open` (`-o`) 标志时，mdbook将在构建后使用默认浏览器打开该书籍。

#### --dest-dir

`--dest-dir` (`-d`)选项用于更改书籍的输出目录。该目录是相对于书本根目录的相对路径。如果
未指定，默认为`book.toml`配置文件中`build.build-dir`项的值或者是`./book`目录。

#### 指定排除模式

`watch` 命令不会自动触发书籍根目录中 `.gitignore` 文件中列出的文件的生成。`.gitignore` 文件包含了[gitignore文档](https://git-scm.com/docs/gitignore) 中描述的文件模式。这对于忽略某些编辑器创建的临时文件非常有用。

_注意: 只有书籍根目录下的 `.gitignore` 文件会被使用. 全局 `$HOME/.gitignore` 或者父目录下的 `.gitignore` 文件不会被使用。_
