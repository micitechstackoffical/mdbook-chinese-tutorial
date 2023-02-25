# 创建一本电子书

一旦已经安装了 `mdbook` 命令行工具, 你可以使用它创建和渲染电子书。

## 初始化电子书

`mdbook init`命令将创建一个新目录，其中包含一本空的电子书，供您开始使用。
可以为其指定要创建的目录的名称：

```sh
mdbook init my-first-book
```

在生成本书之前，它会问几个问题。
```
Do you want a .gitignore to be created? (y/n)
是否创建.gitignore文件？
What title would you like to give the book?
本书的名称是什么？
2023-02-21 18:20:11 [INFO] (mdbook::book::init): Creating a new book with stub content

All done, no errors...

```
回答完问题后，您可以将当前目录更改为新书的目录：

```sh
cd my-first-book
```

生成一本书有几种方法，但最简单的方法之一是使用`serve`命令，该命令将构建该书并启动本地Web服务器：

```sh
mdbook serve --open
```

`--open`选项将打开默认web浏览器以查看该书。
即使在编辑书的内容时，也可以让服务器保持运行状态，`mdbook`将自动重建*并*自动刷新web浏览器。

查看[命令行工具](../cli/index.html)获取更多关于`mdbook`命令和选项的信息.

## 书的结构

一本书是由几个文件构建的，这些文件定义了书的设置和布局。

### `book.toml`

在书的根目录中，有一个`book.toml`文件，其中包含用于描述如何构建书的设置。
这个文件是用 [TOML标记语言](https://toml.io/) 编写的。
通常默认设置已经能够满足要求。
如果您有兴趣探索mdBook提供的更多功能和选项，请查看[配置章节]（../format/Configuration/index.html）以了解更多详细信息。

一个非常基本的 `book.toml` 可以这么简单：

```toml
[book]
title = "My First Book"
```

### `SUMMARY.md`

本书的下一个主要部分是位于 `src/SUMMARY.md` 的摘要文件。此文件包含本书中所有章节的列表。在查看章节之前，必须将其添加到此列表中。

下面是一个包含几个章节的基本摘要文件：

```md
# Summary

[Introduction](README.md)

- [My First Chapter](my-first-chapter.md)
- [Nested example](nested/README.md)
    - [Sub-chapter](nested/sub-chapter.md)
```

可以尝试在编辑器中打开 `src/SUMMARY.md` 并添加一些章节。如何章节不存在, `mdbook` 会自动创建。

有关摘要文件的其他更多选项的信息, 查看 [Summary章节](../format/summary.md)。

### 源文件

书的内容都存放在 `src` 目录中。每个章节都是一个单独的Markdown文件。通常，每一章节都以一级标题开头，作为章节标题。

```md
# My First Chapter

Fill out your content here.
```

文件的布局由作者决定。文件的组织结构与生成的HTML文件相对应，因此请记住，文件组织结构是每个章节URL的一部分。

当 `mdbook serve` 命令运行时，您可以打开任何章节文件并开始编辑它们。每次保存文件时，`mdbook`都会重建书本并刷新web浏览器。

查看[Markdown章节](../format/Markdown.md)，了解有关设置章节内容格式的更多信息。

`src` 目录中的所有其他文件都将包含在输出中。因此，如果您有图像或其他静态文件，只需将它们包含在 `src` 目录中的某个位置。

## 发布电子书

一旦编写完成该书，你可能想把它放在某个地方让别人看。首先是构建并输出该书。可以在`book.toml`文件所在目录中执行`mdbook build`命令完成：

```sh
mdbook build
```

这将生成一个名为`book`的目录，其中包含书本的HTML内容。然后，您可以将此目录放置在任何web服务器上以托管它。

有关发布和部署的更多信息，请查看[持续集成章节](../continuous-indexntegration.md)了解更多信息。