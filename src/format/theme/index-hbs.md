# index.hbs

`index.hbs` 是用于渲染书本的handlers模板。Markdown文件被处理成html，然后注入到该模板中。

如果您想更改书本的布局或样式，需要修改该模板。

## 数据

许多数据通过"上下文"暴露在handlebars模板中。在handlebars模板中，您可以使用

```handlebars
{{name_of_property}}
```

下面是暴露出的属性列表:

- ***language*** 书籍的语言，格式为`en`，如`book.toml`中所指定（如果未指定，则默认为`en`）。例如，要在<code class="language-html">\<html lang="{{ language }}"></code>中使用。
- ***title*** 用于当前页面的标题。等同于`{{ chapter_title }} - {{ book_title }}` ，除非未设置`book_title`，在这种情况下，默认为`chapter_title`。
- ***book_title*** 在`book.toml`里设置的书籍名称
- ***chapter_title*** 在`SUMMARY.md`里罗列的当前章节的名称
- ***path*** 源目录中原始Markdown文件的相对路径
- ***content*** 这是渲染的Markdown内容
- ***path_to_root*** 这是一个从当前文件到根目录的路径。为了保持原始目录结构，在相对链接前面加上`path_to_root`非常有用。
- ***chapters*** 下面这个字典格式的数组
  ```json
  {"section": "1.2.1", "name": "name of this chapter", "path": "dir/markdown.md"}
  ```
  
  包含本书的所有章节。例如，用于构建目录(侧边栏)。

## Handlebars Helpers

除了可以访问的属性之外，还有一些handlebars助手可以使用。

### 1. toc

`toc`助手是这样使用的:

```handlebars
{{#toc}}{{/toc}}
```

并根据你的书的结构输出类似这样的内容

```html
<ul class="chapter">
    <li><a href="link/to/file.html">Some chapter</a></li>
    <li>
        <ul class="section">
            <li><a href="link/to/other_file.html">Some other Chapter</a></li>
        </ul>
    </li>
</ul>
```

如果您想用另一个结构制作toc，您可以访问包含所有数据的章节属性。目前唯一的限制是，您必须使用JavaScript而不是使用handlebars助手。

```html
<script>
var chapters = {{chapters}};
// Processing here
</script>
```

### 2. previous / next

`previous`和`next`助手用于暴露上一章和下一章的链接和名称。

下面这样使用：

```handlebars
{{#previous}}
    <a href="{{link}}" class="nav-chapters previous">
        <i class="fa fa-angle-left"></i>
    </a>
{{/previous}}
```

只有上一章/下一章存在时，才会呈现内部html。当然，内部html可以根据您的喜好进行更改。

------

*如果您想暴露其他属性或助手，请[创建一个新的问题](https://github.com/rust-lang/mdBook/issues)*

