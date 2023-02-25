# init命令

每一本新书都有相同的最小模板。为了这个目的，mdBook包含一个 `init` 命令。

`init`命令的用法如下:

```bash
mdbook init
```

首次使用 `init` 命令时，会初始化一些文件和目录:
```bash
book-test/
├── book
└── src
    ├── chapter_1.md
    └── SUMMARY.md
```

- `src` 目录是使用markdown编写书籍的地方. 它包含所有源文件、配置文件等。

- `book` 目录是输出书籍的地方. 该输出目录上传到服务器后供他人观看。

- `SUMMARY.md` 是书籍的框架, 在[另一章节](../format/summary.md)中进行了更详细的讨论。

#### 提示: 从SUMMARY.md生成章节

当 `SUMMARY.md` 文件已经存在时，`init`命令将首先解析它并根据`SUMMARY.md`中使用的路径生成丢失的文件。
这样可以让你思考和创建该书籍的整个结构，然后让mdBook生成它。

#### 指定一个目录

`init` 命令可以将目录作为参数，用作书籍的根目录而不是当前的工作目录。

```bash
mdbook init path/to/book
```

#### --theme

使用 `--theme` 标志时，默认主题将被复制到src下名为`theme`的目录，以便您可以修改它。

主题被选择性覆盖，这意味着如果您不想覆盖特定文件，只需删除它，将使用默认文件。

#### --title
 
指定书本的标题。如果未提供，会以交互式提示的方式询问标题内容。

```bash
mdbook init --title="my amazing book"
```

#### --ignore

创建一个 `.gitignore` 文件，该文件配置为忽略[构建]书本时创建的`book`目录。如果未提供，则会出现一个交互式提示，询问是否应创建它。

[building]: build.md
