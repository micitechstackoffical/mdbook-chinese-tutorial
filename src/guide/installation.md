# 软件安装

安装mdBook命令行工具有多种方法。选择以下任意一种最适合您的方法。如果您正在为自动部署安装mdBook，请查看[持续集成]  一章，了解有关如何安装的更多示例。

[持续集成]: ../continuous-integration.md

## 二进制文件

可执行的二进制文件可在[GitHub发布页面][Releases]上下载。选择您的平台（Windows、macOS或Linux）下载二进制文件并解压。解压后有一个“mdbook”的可执行文件，可以运行它来构建电子书。

为了方便执行，可以将该二进制文件的路径添加到 `PATH`.

[releases]: https://github.com/rust-lang/mdBook/releases

## 使用Rust从源代码进行构建

要从源代码构建“mdbook”可执行文件，首先需要安装Rust和Cargo。按照[Rust安装页]上的说明进行操作。  
mdBook当前至少需要Rust版本1.60。

一旦安装了Rust，下面的命令可以用来构建和安装mdBook：

```sh
cargo install mdbook
```

该命令会自动从 [crates.io] 下载mdBook, 构建并安装到 Cargo的全局二进制目录下 (默认是`~/.cargo/bin/`).

运行 `cargo uninstall mdbook` 进行卸载.

[Rust安装页]: https://www.rust-lang.org/tools/install
[crates.io]: https://crates.io/

### 安装最新的master版本

发布到crates.io的版本通常会落后于GitHub上托管的版本。如果您需要最新版本，可以基于master分支自己构建mdBook的最新版本。使用Cargo工具会非常简单。


```sh
cargo install --git https://github.com/rust-lang/mdBook.git mdbook
```

同理, 确保Cargo的bin目录添加到`PATH`.

如果您想修改mdBook， 查看 [Contributing Guide] 获得更多信息.

[Contributing Guide]: https://github.com/rust-lang/mdBook/blob/master/CONTRIBUTING.md
