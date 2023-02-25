# completions命令

`completions`命令用于shell中的自动补全。当在shell中输入 `mdbook`时，
可以按下shell的自动完成键（通常是Tab键），可以显示有效选项，或完成部分输入。

`completions` 首先需要安装到shell中:

```bash
mdbook completions bash > ~/.local/share/bash-completion/completions/mdbook
```

该命令为给定的shell打印`completion` 脚本。
运行`mdbook completions --help` 查看支持的shell列表。

将`completions`存放在什么位置取决于您使用哪个shell和操作系统。有关如何放置脚本的更多信息，请参阅shell文档。