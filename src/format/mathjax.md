# MathJax 支持

mdBook通过[MathJax](https://www.mathjax.org/) 对数学方程式提供支持。

要启用MathJax，需要在`book.toml`文件中的`output.html`部分中添加`mathjax-support`键。

```toml
[output.html]
mathjax-support = true
```

>**注意:** MathJax使用的常用分隔符尚不支持。当前不能使用`$$ ... $$`分隔符，`\[ ... \]`分隔符需要一个额外的反斜杠才能生效。希望这一限制将很快解除。

>**Note:** 当在MathJax块中使用`双反斜杠`时(例如：
 `\begin{cases} \frac 1 2 \\ \frac 3 4 \end{cases}`) 你需要添加 _两个额外的_ 反斜杠 (e.g., `\begin{cases} \frac 1 2 \\\\ \frac 3 4 \end{cases}`).


### 内联方程式

内联方程式使用`\\(` 和 `\\)`分隔. 因此，如果渲染下面的方程式 \\( \int x dx = \frac{x^2}{2} + C \\) 应该这么写:
```
\\( \int x dx = \frac{x^2}{2} + C \\)
```

### 块方程式

块方程式使用`\\[` 和 `\\]`分隔. 如果渲染下面的方程式，

\\[ \mu = \frac{1}{N} \sum_{i=0} x_i \\]


应该这么写:

```bash
\\[ \mu = \frac{1}{N} \sum_{i=0} x_i \\]
```
