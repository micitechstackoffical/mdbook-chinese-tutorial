# Markdown

mdBook的[解析器](https://github.com/raphlinus/pulldown-cmark) 遵守 [CommonMark](https://commonmark.org/) 的规范以及下面描述的一些扩展。

你可以快速学习[教程](https://commonmark.org/help/tutorial/)， 或 [在线试用](https://spec.commonmark.org/dingus/) CommonMark。完整的Markdown概述超出了本文档的范围，但下面是一些基础知识的概述。要获得更深入的体验，请查看[Markdown Guide](https://www.markdownguide.org).

## 文本和段落

文本是相对可预测的：

```markdown
Here is a line of text.

This is a new line.
```

会如下显示:

Here is a line of text.

This is a new line.

## 标题

标题使用`#` 标记，且都在一条线上。更多`#`表示更小的标题级别：

```markdown
### A heading 

Some text.

#### A smaller heading 

More text.
```

### A heading 

一些内容...

#### A smaller heading 

更多内容...

## 集合

集合可以是无序和有序的. 有序集合会自动排序:

```markdown
* milk
* eggs
* butter

1. carrots
1. celery
1. radishes
```

* milk
* eggs
* butter

1. carrots
1. celery
1. radishes

## 链接

链接到URL或本地文件很容易：

```markdown
Use [mdBook](https://github.com/rust-lang/mdBook). 

Read about [mdBook](mdBook.md).

A bare url: <https://www.rust-lang.org>.
```

Use [mdBook](https://github.com/rust-lang/mdBook). 

Read about [mdBook](mdBook.md).

A bare url: <https://www.rust-lang.org>.

----

以`.md`结尾的相对链接最终将转换为`.html`扩展名。在书本中尽可能使用`.md`链接。当在mdBook之外查看Markdown文件时，这非常有用，例如在能够自动渲染Markdown的GitHub或GitLab上。


指向`README.md`的链接将转换为`index.html`。
这是因为一些平台(如GitHub)会自动呈现README文件，但web服务器根文件通常为`index.html`。


您可以使用`#`片段链接到各个标题。
例如，`mdbook.md#文本和段落`将链接到上面的[文本和段落](#文本和段落)部分。英文情况下是：`mdbook.md#a-heading`将链接到上面的[A heading](#a-heading)部分。ID是通过转换标题（例如大写字母转换为小写字母）并用破折号替换空格来创建的。您可以单击任何标题并查看浏览器中的URL以查看该标题的ID。

## 图像

引入图像只是引入一个指向它们的链接，就像上面的 _Links_ 部分中那样。以下markdown是引入一个Rust的SVG图标，位于与当前文件同一级别的`images`目录中：

```markdown
![The Rust Logo](images/rust-logo-blk.svg)
```

mdBook将生成以下HTML：

```html
<p><img src="images/rust-logo-blk.svg" alt="The Rust Logo" /></p>
```

当然，它显示的图像是这样的：

![The Rust Logo](images/rust-logo-blk.svg)

## 扩展

mdBook有几个超出标准CommonMark规范的扩展。

### 删除线

文本可以通过在每侧用两个波浪形字符包裹文本，以水平线穿过中心呈现：

```text
An example of ~~strikethrough text~~.
```

效果如下:

> An example of ~~strikethrough text~~.

遵循了 [GitHub Strikethrough extension][strikethrough].

### 脚注

脚注在文本中生成一个小的编号链接，单击该链接时，读者会看到项目底部的脚注文本。脚注标签类似于前面有插入符号的链接引用。脚注文本类似于链接引用定义，文本在标签后面。例子：

```text
This is an example of a footnote[^note].

[^note]: This text is the contents of the footnote, which will be rendered
    towards the bottom.
```

相关如下:

> This is an example of a footnote[^note].
>
> [^note]: This text is the contents of the footnote, which will be rendered
>     towards the bottom.

脚注会根据脚注的书写顺序自动编号。

### 表格

可以使用管道和破折号编写表格，以绘制表格的行和列。这些将被转换为与形状匹配的HTML表格。例子：

```text
| Header1 | Header2 |
|---------|---------|
| abc     | def     |
```

此示例的渲染结果与以下类似：

| Header1 | Header2 |
|---------|---------|
| abc     | def     |


有关所支持的确切语法的详细信息，请参阅[GitHub Tables extension][tables]的规范。

### 任务列表

任务列表可用作已完成项目的检查表。例子：

```md
- [x] Complete task
- [ ] Incomplete task
```

渲染结果如下:

> - [x] Complete task
> - [ ] Incomplete task

有关详细信息，请参阅[任务列表扩展]的规范。

### 智能标点符号

一些ASCII标点符号序列将自动转换为花式Unicode字符：

| ASCII sequence | Unicode |
|----------------|---------|
| `--`           | –       |
| `---`          | —       |
| `...`          | …       |
| `"`            | “ or ”, 依赖上下文 |
| `'`            | ‘ or ’, 依赖上下文 |

因此，无需手动输入这些Unicode字符！

默认情况下禁用此功能。
要启用它，请参阅[`output.html.curly-quotes`]config选项。

[strikethrough]: https://github.github.com/gfm/#strikethrough-extension-
[tables]: https://github.github.com/gfm/#tables-extension-
[任务列表扩展]: https://github.github.com/gfm/#task-list-items-extension-
[`output.html.curly-quotes`]: configuration/renderers.md#html渲染器选项
