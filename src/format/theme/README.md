# 主题

默认渲染器使用[handlebars](http://handlebarsjs.com/) 模板来渲染markdown文件，并附带mdBook二进制文件中包含的默认主题。

主题是完全可定制的，您可以通过在项目根目录中创建一个与`src`同级的`theme`目录，选择性地替换主题中的每个文件。
使用要覆盖的文件的名称创建一个新文件，就会使用该文件而不是默认文件。

以下是您可以覆盖的文件:

- **_index.hbs_** handlebars模板。
- **_head.hbs_** 添加到HTML的`<head>`部分。
- **_header.hbs_** 添加到每个书籍页面顶部的内容。
- **_css/_** 包含书籍样式的CSS文件。
    - **_css/chrome.css_** UI元素。
    - **_css/general.css_** 基本样式。
    - **_css/print.css_** 打印机输出样式。
    - **_css/variables.css_** 其他CSS文件中用到的变量。
- **_book.js_** 用于添加客户端功能，如隐藏/显示侧边栏、更改主题等。
- **_highlight.js_** 用于高亮代码片段的js文件，无需修改。
- **_highlight.css_** 用于高亮代码片段的主题样式文件。
- **_favicon.svg_** 和 **_favicon.png_** 图标文件. SVG版本用于[较新浏览器]。
- **fonts/fonts.css** 用于声明使用的字体. 自定义的字体需要添加到`fonts`目录。

通常，当您想要调整主题时，不需要覆盖所有文件。如果您只需要在样式表中进行更改，那么覆盖所有其他文件是没有意义的。由于自定义文件优先于内置文件，因此即便有缺陷修复或者新的功能也不会更新。

**注意:** 覆盖文件时，可能会破坏某些功能。因此，我建议使用默认主题中的文件作为模板，只添加/修改您需要的内容。您可以使用`mdbook init --theme`将默认主题自动复制到源目录中，只需删除不想覆盖的文件。

`mdbook init --theme`不会创建上面列出的所有文件。有些文件，如`head.hbs`，没有内置的等效文件。如果需要，只需创建文件即可。

如果您完全替换了所有内置主题，请确保在配置中也设置了[`output.html.preferred-dark-theme`]，默认为内置的`navy`主题。

[`output.html.preferred-dark-theme`]: ../configuration/renderers.md#html-renderer-options
[较新浏览器]: https://caniuse.com/#feat=link-icon-svg
