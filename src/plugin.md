# 第三方插件

mdBook可以通过外部命令进行扩展。有关扩展mdBook的详细信息，请参阅[mdBook指南](https://doc.mici.tech/mdbook-chinese-tutorial/for_developers/index.html) 。以下是社区开发的用于扩展mdBook的插件列表。如果您是插件的作者，请随时将其添加到此列表中！

## 预处理器

- [`mdbook-katex`](https://github.com/lzanini/mdbook-katex): 将LaTex方程渲染到HTML的预处理器。
- [`mdbook-classy`](https://github.com/Reviewable/mdbook-classy): 为段落添加对kramdown样式CSS类注释的支持。
- [`mdbook-plantuml`](https://github.com/sytsereitsma/mdbook-plantuml): 将[PlantUML](https://plantuml.com/) 代码块渲染成图像的渲染器。
- [`mdbook-toc`](https://github.com/badboy/mdbook-toc): 添加内联目录
- [`mdbook-pagetoc`](https://github.com/slowsage/mdbook-pagetoc): 添加侧边栏目录，[`mdBook-pagetoc`](https://github.com/JorelAli/mdBook-pagetoc) 的Rust封装.
- [`mdbook-svgbob`](https://github.com/fzzr-/mdbook-svgbob): 将ASCII图转换为SVG。
- [`mdbook-mermaid`](https://github.com/badboy/mdbook-mermaid): 使用[mermaid.js](https://mermaid-js.github.io/mermaid/#/) 从文本创建图表。
- [`mdbook-admonish`](https://github.com/tommilligan/mdbook-admonish): 基于[mkdocs-material](https://squidfunk.github.io/mkdocs-material/reference/admonitions/) 实现，添加对Material Design规范的支持。
- [`mdbook-morsels`](https://github.com/ang-zeyu/morsels): 客户端搜索解决方案。
- [`mdbook-graphviz`](https://github.com/dylanowen/mdbook-graphviz): 使用Graphviz渲染图形
- [`mdbook-tera`](https://github.com/avitex/mdbook-tera): Tera预处理器，允许基于模板的渲染
- [`mdbook-template`](https://github.com/sgoudham/mdbook-template): 一种预处理器，允许使用动态参数重新使用模板文件。
- [`mdbook-footnote`](https://github.com/daviddrysdale/mdbook-footnote): 添加对脚注的支持。
- [`mdbook-indexing`](https://github.com/daviddrysdale/mdbook-indexing): 添加对索引生成的支持。
- [`mdbook-cmdrun`](https://github.com/FauconFan/mdbook-cmdrun): 运行任意命令并替换markdown文件中这些命令的stdout的预处理器。
- [`mdbook-haskell`](https://github.com/j-rockel/mdbook-haskell): 引入Haskell源代码。
- [`mdbook-i18n`](https://github.com/funkill/mdbook-i18n): 增强多语言版本。
- [`mdbook-keeper`](https://github.com/tfpk/mdbook-keeper): 更好的mdbook测试支持——添加第三方crates并在构建期间进行测试。
- [`mdbook-yml-header`](https://github.com/dvogt23/mdbook-yml-header): 清理文件顶部的yml-header，如[obsidian](https://obsidian.md/)
- [`mdbook-private`](https://github.com/RealAtix/mdbook-private): 允许定义、隐藏或保留私有部分的预处理器。

## 渲染器

* [`mdbook-pdf`](https://github.com/HollowMan6/mdbook-pdf): 
基于[headless_chrome](https://github.com/atroche/rust-headless-chrome) 和 [Chrome DevTools Protocol](https://chromedevtools.github.io/devtools-protocol/tot/Page/#method-printToPDF) 生成PDF。
* [`mdbook-linkcheck`](https://github.com/Michael-F-Bryan/mdbook-linkcheck): 检查断开的链接。
* [`mdbook-epub`](https://github.com/Michael-F-Bryan/mdbook-epub): 实验性EPUB生成器.
* [`mdbook-man`](https://github.com/vv9k/mdbook-man): 生成man页面.
* [`mdbook-test`](https://github.com/Michael-F-Bryan/mdbook-test): 通过[rust-skeptic](https://github.com/budziq/rust-skeptic) 运行该书内容的程序，验证所有内容是否正确编译和运行（类似于`rustdoc --test`）

## 其他

* [`mdBook-pagetoc`](https://github.com/JorelAli/mdBook-pagetoc): 添加侧边栏目录。