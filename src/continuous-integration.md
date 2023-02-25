# 在持续集成中运行`mdbook`

有多种服务，如[GitHub Actions]或[GitLab CI/CD]，可用于自动化测试和部署您的书籍。

下面提供了一些关于如何配置服务以运行mdBook的指南。可以在[Automated Deployment]wiki页面上找到特定的方法。

[GitHub Actions]: https://docs.github.com/en/actions
[GitLab CI/CD]: https://docs.gitlab.com/ee/ci/
[Automated Deployment]: https://github.com/rust-lang/mdBook/wiki/Automated-Deployment

## 安装mdBook

安装mdBook有几种不同的方式。根据自己的需求选择。

### 二进制文件


也许最简单的方法是使用[GitHub Releases page][Releases]上发布的二进制文件。一种简单的方法是使用`curl`命令行工具下载该可执行文件：

```sh
mkdir bin
curl -sSL https://github.com/rust-lang/mdBook/releases/download/v0.4.27/mdbook-v0.4.27-x86_64-unknown-linux-gnu.tar.gz | tar -xz --directory=bin
bin/mdbook build
```

此方法的一些注意事项:

* 这相对较快，不需要处理缓存。
* 这不需要安装Rust。
* 指定特定的URL意味着您必须手动更新脚本以获得新版本。如果您想指定到特定版本，这可能是一个好处。当然，有些用户更喜欢在发布时自动获得更新的版本。
* 您依赖于可用的GitHub CDN。

[releases]: https://github.com/rust-lang/mdBook/releases

### 从源代码构建

从源代码构建需要安装Rust。
有些服务预先安装了Rust，但如果您的服务没有安装，则需要添加安装步骤。

安装Rust后，可以使用`cargo install`来构建和安装mdBook。
我们建议使用[SemVer版本](https://semver.org/lang/zh-CN/) 说明符，以便获得mdBook的最新**非中断**版本。
例如：

```sh
cargo install mdbook --no-default-features --features search --vers "^0.4" --locked
```

这包括几个推荐选项:

* `--no-default-features` — 禁用CI上可能不需要的功能，如`mdbook serve`使用的HTTP服务器。这将大大加快构建时间。
* `--features search` — 禁用默认功能意味着您应该手动启用所需的功能，例如内置的[搜索]功能。
* `--vers "^0.4"` — 这将安装最新版本的`0.4`系列。但是，像`0.5.0`之后的版本将不会安装，因为它们可能会破坏您的构建。如果您已经安装了旧版本，Cargo将自动升级mdBook。
* `--locked` — 这将使用mdBook发布时使用的依赖项。如果没有`--locked`，它将使用所有依赖项的最新版本，其中可能包括自上次发布以来的一些修复，但也可能（很少）导致构建问题。

您可能需要研究缓存选项，因为构建mdBook可能会有些慢。

[搜索]: guide/reading.md#搜索

## 执行测试

每次推送变更或创建PullRequest时，可以使用[`mdbook test`]运行测试。这可以用来验证书本中的Rust代码示例。

这需要安装Rust。有些服务预先安装了Rust，但如果您的服务没有安装，则需要添加安装步骤。

除了确保安装了适当版本的Rust之外，只需要在book目录下执行`mdbook test`。

您可能还想考虑运行其他类型的测试，如[mdbook-linkcheck]，它将检查断开的链接。或者，如果您有自己的样式检查、拼写检查或任何其他测试，最好在CI中运行它们。

[`mdbook test`]: cli/test.md
[mdbook-linkcheck]: https://github.com/Michael-F-Bryan/mdbook-linkcheck#continuous-integration

## 部署

您可能希望自动部署书本。
有些人可能希望在每次推送变更时都这样做，而另一些人可能希望仅在打标特定版本时进行部署。

您还需要了解如何将变更推送到web服务的细节。
例如，[GitHub Pages]只需要将输出提交到特定的git分支。
其他服务可能需要使用SSH之类的工具来连接到远程服务器。

基本要点是，您需要运行`mdbook build`来生成输出，然后将文件（位于`book`目录中）传输到正确的位置。

然后，您需要考虑是否需要清除web服务上的缓存。

有关各种不同服务的示例，请参见[Automated Deployment]wiki页面。

[GitHub Pages]: https://docs.github.com/en/pages

### 404处理

mdBook自动生成一个 404 页面，用于缺失的链接。默认输出是位于书本根目录的名为`404.html`的文件。一些服务如[GitHub Pages]将自动使用此页面作为缺失的链接。
对于其他服务，您可能需要考虑将web服务器配置为使用此页面，因为它将为读者提供返回书本的导航。

如果您的书没有部署在域的根目录下，那么您应该设置[`output.html.site-url`]配置，以便404页面正常工作。
它需要知道书本的部署位置，以便正确加载静态文件（如CSS）。
例如，本指南部署在<https://rust-lang.github.io/mdBook/>，并且`site-url`设置配置如下：

```toml
# book.toml
[output.html]
site-url = "/mdBook/"
```

通过在书本中创建名为`src/404.md` 的文件，可以自定义404页面。
如果要使用其他文件名，可以设置[`output.html.input-404`]为其他文件名。

[`output.html.site-url`]: format/configuration/renderers.md#html-renderer-options
[`output.html.input-404`]: format/configuration/renderers.md#html-renderer-options
