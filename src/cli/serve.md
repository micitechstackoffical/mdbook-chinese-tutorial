# serve命令

`serve`命令用于启动一个web服务器来实时预览书籍，默认是`localhost:3000` : 

```bash
mdbook serve
```

`serve`命令用于监视书籍`src`目录的变化，重新构建书籍并为每次变更刷新浏览器；这包括重新创建已删除但还保留在`SUMMARY.md`中的文件！这里使用websocket连接触发浏览器刷新。

***注意:*** *`serve`命令用于测试一本书的HTML输出，并不打算作为网站的完整HTTP服务器。*

#### 指定一个目录

`serve` 命令可以传入一个目录参数，作为书籍的根目录而不是当前的工作目录。

```bash
mdbook serve path/to/book
```

### 服务器选项

`serve`服务器的主机名默认是 `localhost`, 端口默认是 `3000`. 这两个选项都可以通过命令行指定:

```bash
mdbook serve path/to/book -p 8000 -n 127.0.0.1 
```

#### --open

当您使用`--open` (`-o`) 标志时，mdbook将在启动服务器后使用默认浏览器打开该书籍。

#### --dest-dir

`--dest-dir` (`-d`)选项用于更改书籍的输出目录。该目录是相对于书本根目录的相对路径。如果
未指定，默认为`book.toml`配置文件中`build.build-dir`项的值或者是`./book`目录。

#### Specify exclude patterns

`serve` 命令不会自动触发书籍根目录中 `.gitignore` 文件中列出的文件的生成。`.gitignore` 文件包含了[gitignore文档](https://git-scm.com/docs/gitignore) 中描述的文件模式。这对于忽略某些编辑器创建的临时文件非常有用。

***注意:*** *只有书籍根目录下的 `.gitignore` 文件会被使用. 全局 `$HOME/.gitignore` 或者父目录下的 `.gitignore` 文件不会被使用。*
