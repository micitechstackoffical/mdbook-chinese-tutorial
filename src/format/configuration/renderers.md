# 配置渲染器

渲染器（也称为“后端”）负责输出书籍。

下面是内置的渲染器:

* [`html`](#html-renderer-options) — 输出HTML格式.如果在`book.toml`中未定义其他`[output]`表，则默认启用此选项。
* [`markdown`](#markdown-renderer) — 这会在运行预处理器后将书作为markdown输出。这对于调试预处理器很有用。

社区已经开发了一些渲染器。有关可用渲染器的列表，请参见[第三方插件]wiki页面。

有关如何创建新渲染器的信息，请参阅[新建渲染器]章节。

[第三方插件]: https://github.com/rust-lang/mdBook/wiki/Third-party-plugins
[新建渲染器]: ../../for_developers/backends.md

## `output`配置项

可以通过在`book.toml`中包含渲染器名称的`output`配置项来添加渲染器。
例如，如果您有一个名为`mdbook-wordcount`的渲染器，通过下面方式添加：

```toml
[output.wordcount]
```

通过如上配置，mdBook将会执行`mdbook-wordcount`渲染器。

还可以包含渲染器的配置项。
例如，如果`wordcount`渲染器需要一些额外的配置选项：
```toml
[output.wordcount]
ignores = ["Example Chapter"]
```

如果定义了任何`[output]`配置项，则默认不会再启用`html`渲染器。
如果您仍希望保持`html`渲染器运行，那么只需将其包含在`book.toml`文件中即可。
例如：

```toml
[book]
title = "My Awesome Book"

[output.wordcount]

[output.html]
```

如果包含多个`output`配置项，则会改变输出目录的布局。
如果只有一个渲染器，输出则直接放置在`book`目录中（请参见[`build.build-dir`]以覆盖此位置）。

如果有多个渲染器，则每个渲染器都放置在`book`下的单独目录中。例如，上面的目录是`book/html`和`book/wordcount`。

[`build.build-dir`]: general.md#build-options

### 自定义渲染器命令

默认情况下，当您将`[output.foo]`配置项添加到`book.toml`文件时，
`mdbook`将尝试调用`mdbook-foo`可执行程序。如果要使用不同的程序名或传入命令行参数，可以通过添加`command`字段覆盖。

```toml
[output.random]
command = "python random.py"
```

### 可选的渲染器

如果启用的渲染器未安装，默认是会抛出错误的。可以通过将渲染器标记为`可选`来改变此行为：

```toml
[output.wordcount]
optional = true
```

这会将错误降级为警告。


## HTML渲染器选项

HTML渲染器有多个选项，详情如下。
它们应该在`book.toml`文件的`[output.html]`配置项中指定。

```toml
# Example book.toml file with all output options.
[book]
title = "Example book"
authors = ["John Doe", "Jane Doe"]
description = "The example book covers examples."

[output.html]
theme = "my-theme"
default-theme = "light"
preferred-dark-theme = "navy"
curly-quotes = true
mathjax-support = false
copy-fonts = true
additional-css = ["custom.css", "custom2.css"]
additional-js = ["custom.js"]
no-section-label = false
git-repository-url = "https://github.com/rust-lang/mdBook"
git-repository-icon = "fa-github"
edit-url-template = "https://github.com/rust-lang/mdBook/edit/master/guide/{path}"
site-url = "/example-book/"
cname = "myproject.rs"
input-404 = "not-found.md"
```

以下配置选项可用：

- **theme:** mdBook带有默认主题和所需的所有资源文件。但是如果设置了此选项，mdBook将有选择地覆盖指定文件夹中的主题文件。
- **default-theme:** 在"更改主题"下拉列表中设置的默认主题颜色，默认是`light`。
- **preferred-dark-theme:** 默认的深色主题。如果浏览器通过['prefers-color-scheme'](https://developer.mozilla.org/en-US/docs/Web/CSS/@media/prefers-color-scheme) CSS媒体查询请求站点的深色版本，则将使用此主题。默认是`navy`。
- **curly-quotes:** 将直引号转换为卷曲引号，代码块和代码跨度中出现的引号除外。默认是`false`。
- **mathjax-support:** 添加对[MathJax](../mathjax.md)的支持,默认是`false`。
- **copy-fonts:** (**Deprecated**) 如果为 `true` (默认), mdBook使用其内置字体，这些字体被复制到输出目录中。
  如果为 `false`, 内置的字体不会被使用。
  此选项已弃用。如果您想定义自己的字体，创建一个`theme/fonts/fonts.css` 文件，并将字体存储在`theme/fonts/`目录中。
- **google-analytics:** 此字段已弃用，在将来的版本中删除。使用`theme/head.hbs`文件添加相应的Google Analytics代码。
- **additional-css:** 如果你想在不覆盖整个样式的情况下稍微改变一下书籍的外观，可以指定一组样式表，将在默认值之后加载，这样可以改变风格
- **additional-js:** 如果您想在不删除当前行为的情况下向书本中添加一些行为，可以指定一组JavaScript文件，这些文件将与默认文件一起加载。
- **no-section-label:** 默认情况下，mdBook会在章节前面添加编号。例如，"1.", "2.1"。将此选项设置为true以禁用
这些编号。默认为`false`。
- **git-repository-url:** 该书的git存储库的url。如果提供了图标链接，将在书的菜单栏中输出。
- **git-repository-icon:** 用于git仓库链接的FontAwesome图标类。默认为`fa-github`，看起来像<i class="fa fa-github"></i>。
如果您没有使用GitHub，可以使用这个图标`fa-code-fork`，它看起来像<i class="fa fa-code-fork"></i>。
- **edit-url-template:** 
  编辑url模板，以"建议编辑"按钮显示（类似于<i class="fa fa-edit"></i>），
  用于直接跳转到编辑当前查看的页面。例如，GitHub项目将其设置为
`https://github.com/<owner>/<repo>/edit/master/{path}`，
Bitbucket项目将其设置为
`https://bitbucket.org/<owner>/<repo>/src/master/{path}?mode=edit`
其中｛path｝将替换为文件在仓库里的全路径。
- **input-404:** 当文件缺失时显示的markdown文件的名称。会生成文件名相同的，以`html`结尾的文件。默认为`404.md`。
- **site-url:** 
  用于托管本书站点的url。这是为了确保404文件中的导航链接和script/css导入可以正常工作，即使在访问
子目录时也是正常的，默认为`/`。如果设置了`site-url`，确保对静态文件资源使用文档的相对链接，这意味着它们不应该以`/`开头。
- **cname:** 
  书籍托管站点的DNS子域名或顶点域名。此字符串将写入站点根目录中名为CNAME的文件，如下GitHub Pages所需（请参见[*管理GitHub页站点的自定义域名*][custom domain]）。

[custom domain]: https://docs.github.com/en/github/working-with-github-pages/managing-a-custom-domain-for-your-github-pages-site

### `[output.html.print]`

`[output.html.print]`配置项提供了控制可打印输出的选项。默认情况下，mdBook将在书的右上角包含一个图标（看起来像<i class="fa fa-print"></i>），该图标将书打印为一页。

```toml
[output.html.print]
enable = true    # include support for printable output
page-break = true # insert page-break after each chapter
```

- **enable:** 启用打印支持。当`false`时，将不会显示打印图标。默认为`true`。
- **page-break** 在章节之间插入分页符。默认为`true`。

### `[output.html.fold]`

`[output.html.fold]`配置项提供了控制导航侧边栏中章节列表折叠的选项。
```toml
[output.html.fold]
enable = false    # whether or not to enable section folding
level = 0         # the depth to start folding
```

- **enable:** 启用章节折叠。当为`false`时，所有章节都打开。默认为`false`。
- **level:** 级别越高，打开的折叠区域越多。当级别为0时，所有折叠都闭合。默认值为`0`。

### `[output.html.playground]`

`[output.html.playground]`配置项提供了控制Rust示例代码块及其与[Rust playground]集成的选项。

[Rust Playground]: https://play.rust-lang.org/

```toml
[output.html.playground]
editable = false         # allows editing the source code
copyable = true          # include the copy button for copying code snippets
copy-js = true           # includes the JavaScript for the code editor
line-numbers = false     # displays line numbers for editable code
runnable = true          # displays a run button for rust code
```

- **editable:** 允许编辑源代码，默认为`false`。
- **copyable:** 在代码片段中显示`拷贝`按钮，默认为`true`。
- **copy-js:** 将编辑器的JavaScript文件复制到输出目录。默认为`true`.
- **line-numbers** 在代码的可编辑部分显示行号。要求`editable`和 `copy-js`都为`true`。默认为`false`。
- **runnable** 显示Rust代码段的`运行`按钮。将其更改为`false`将全局禁用playground上的`运行`功能。默认为`true`。

[Ace]: https://ace.c9.io/

### `[output.html.search]`

`[output.html.search]`配置项提供了控制内置文本[搜索]的选项。mdBook必须在启用`search`功能的情况下编译（默认情况下启用）。

[搜索]: ../../guide/reading.md#search

```toml
[output.html.search]
enable = true            # enables the search feature
limit-results = 30       # maximum number of search results
teaser-word-count = 30   # number of words used for a search result teaser
use-boolean-and = true   # multiple search terms must all match
boost-title = 2          # ranking boost factor for matches in headers
boost-hierarchy = 1      # ranking boost factor for matches in page names
boost-paragraph = 1      # ranking boost factor for matches in text
expand = true            # partial words will match longer terms
heading-split-level = 3  # link results to heading levels
copy-js = true           # include Javascript code for search
```

- **enable:** 开启搜索特性。默认为 `true`。
- **limit-results:** 最大搜索结果数。默认为 `30`。
- **teaser-word-count:** 用于搜索结果摘要的单词数。 默认为 `30`。
- **use-boolean-and:** 定义多个搜索词之间的逻辑关系。如果为`true`，所有搜索词必须出现在每个结果中。默认为`false`。
- **boost-title:** 如果搜索词出现在标题中，则搜索结果得分的提升因子。默认为`2`。
- **boost-hierarchy:** 如果搜索词出现在层次结构中，则搜索结果得分的提升因子。层次结构包含父文档的所有标题和所有父标题。默认值为`1`。
- **boost-paragraph:** 如果搜索词出现在文本中，则搜索结果得分的提升因子。默认值为`1`。
- **expand:** 如果搜索应匹配较长的结果，则为True，例如搜索`micro`应匹配`microwave`。默认为`true`。
- **heading-split-level:** 搜索结果将链接到包含结果的文档部分。文档按此级别或以下的标题划分为多个部分。默认值为`3`。(`###这是一个3级标题`)
- **copy-js:** 将搜索实现的JavaScript文件复制到输出目录。默认为`true`。

### `[output.html.redirect]`

`[output.html.redirect]`配置项提供了一种添加重定向的方法。当移动、重命名或删除页面后需确保链接指向新位置时，这非常有用。

```toml
[output.html.redirect]
"/appendices/bibliography.html" = "https://rustc-dev-guide.rust-lang.org/appendix/bibliography.html"
"/other-installation-methods.html" = "../infra/other-installation-methods.html"
```

该配置项包含多个键值对，其中键是需要重定向的文件路径，该路径是输出目录的绝对路径（例如`/appends/preferences.html`）。
该值是浏览器能够导航的任何有效的URI（例如`https://rust-lang.org/`，`/overview.html`或`../bibliography.html`）。

这将生成一个HTML页面，该页面将自动重定向到给定地址。请注意，源路径不支持`#`锚点重定向。

## Markdown 渲染器

Markdown渲染器将运行预处理器，然后输出生成的Markdown。这对于调试预处理器非常有用，尤其是与`mdbook test`结合使用，以查看`mdbook`传递给`rustdoc`的Markdown。

Markdown渲染器包含在`mdbook`中，但默认情况下禁用。通过向`book.toml`添加一个空配置项来启用它，如下所示：

```toml
[output.markdown]
```

此时没有Markdown渲染器的其他配置选项；仅用于启用还是禁用。

有关如何指定在Markdown渲染器之前应运行哪些预处理器的信息，请参阅[预处理器文档](preprocessors.md)。
