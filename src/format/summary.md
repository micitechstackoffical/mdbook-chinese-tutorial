# SUMMARY.md

mdBook使用`摘要文件`来了解要包括哪些章节、它们应该以什么顺序出现、它们的层次结构以及源文件的位置。没有这个文件，就不能生成书籍。

该markdown文件必须命名为`SUMMARY.md`。它的格式非常严格，必须遵循下面描述的结构，以便于解析。
下面未指定的任何元素，无论是格式还是文本，都可能被忽略，或者在构建书籍时可能导致错误。

### 结构

1. ***标题*** - 可选的，但通常以标题开头, 一般是 <code class="language-markdown"># Summary</code>. 这个通常会被解析器忽略,不过没有影响。

   ```markdown
   # Summary
   ```

1. ***前缀章节*** - 在主要编号章节之前，可以添加不加编号的前缀章节。这对于前言、介绍等章节很有用。
然而，也有一些限制。前缀章节不能嵌套；它们都应该在根级别。一旦添加了编号章节，就不能添加前缀章节。

   ```markdown
   [A Prefix Chapter](relative/path/to/markdown.md)

   - [First Chapter](relative/path/to/markdown2.md)
   ```

1. ***模块标题*** - 
可以从逻辑上将书籍分隔成多个不同模块，每个模块可以定义模块标题。模块标题是不可点击的文本。模块标题是可选的，编号的章节可以根据需要分成任意多个模块。

   ```markdown
   # My Part Title

   - [First Chapter](relative/path/to/markdown.md)
   ```

1. ***编号章节*** - 编号章节组成了本书的主要内容，编号章节可以嵌套，从而形成一个良好的层次结构（章节、子章节等）。
   ```markdown
   # Title of Part

   - [First Chapter](relative/path/to/markdown.md)
   - [Second Chapter](relative/path/to/markdown2.md)
      - [Sub Chapter](relative/path/to/markdown3.md)

   # Title of Another Part

   - [Another Chapter](relative/path/to/markdown4.md)
   ```
   编号的章节可以用`-`或`*`表示（不要混用分隔符）。
   
1. ***后缀章节*** - 与前缀章节一样，后缀章节同样没有编号，且位于编号章节之后。
   ```markdown
   - [Last Chapter](relative/path/to/markdown.md)

   [Title of Suffix Chapter](relative/path/to/markdown2.md)
   ```

1. ***草稿章节*** - 草稿章节是没有文件和内容的章节。草稿章节的目的是保留将来待编写的章节，或者仍在规划书的结构以避免创建文件，
从而改变本书的结构。

   草稿章节在HTML中以禁用的链接的形式展现。如您在左侧目录的下一章所见。草稿章节与普通章节一样编写，但不写入文件路径。
   ```markdown
   - [Draft Chapter]()
   ```

1. ***分隔符*** - 
   分隔符可以在任何其他章节之前、中间和之后添加。呈现的结果是在构建的目录中添加一行。分隔符是只包含破折号和至少三个破折号的行：`---`。
   ```markdown
   # My Part Title
   
   [A Prefix Chapter](relative/path/to/markdown.md)

   ---

   - [First Chapter](relative/path/to/markdown2.md)
   ```
  

### 示例

下面是本书`SUMMARY.md` 的源文件，效果如左侧菜单渲染的内容。

```markdown
{{#include ../SUMMARY.md}}
```
