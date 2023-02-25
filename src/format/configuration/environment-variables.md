# 环境变量

通过设置相应的环境变量，可以从命令行覆盖所有配置值。由于许多操作系统将环境变量限制为字母数字字符或`_`，因此配置键的格式需要与普通的`foo.bar.baz`格式略有不同。

用于配置的变量以`MDBOOK_`开头。通过删除`MDBOOK_`前缀并将剩下的字符串转换为`短横线格式`。双下划线(`__`)分隔嵌套的键，而单下划线(`_`)替换为短划线(`-`)。

例如:

- `MDBOOK_foo` -> `foo`
- `MDBOOK_FOO` -> `foo`
- `MDBOOK_FOO__BAR` -> `foo.bar`
- `MDBOOK_FOO_BAR` -> `foo-bar`
- `MDBOOK_FOO_bar__baz` -> `foo-bar.baz`

So by setting the `MDBOOK_BOOK__TITLE` environment variable you can override the
book's title without needing to touch your `book.toml`.
因此，通过设置`MDBOOK_BOOK__TITLE`环境变量，您可以覆盖书的标题，而无需修改`book.toml`。

> **注意:** 为了方便设置更复杂的配置项，首先将环境变量的值解析为JSON，如果解析失败，则返回为字符串。
>
> 这意味着，如果您需要，可以在构建书籍时覆盖所有书籍元数据，例如
>
> ```shell
> $ export MDBOOK_BOOK='{"title": "My Awesome Book", "authors": ["Michael-F-Bryan"]}'
> $ mdbook build
> ```

后一种情况可能适用于从脚本或CI调用`mdbook`的情况，在这种情况下，有时无法在构建之前更新`book.toml`。