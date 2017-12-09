## Linux system
The following is about using Chinese with pandoc on Linux platform, including the problem with PDF output, which requires XeLaTeX/LuaLaTeX and custom fonts.

- 首先编译遇到内存溢出问题, 是ghc7.4.1以前有bug,用7.4.1能编过.

- 然后遇到说network依赖的ghc版本不对的问题,pasace 用3 network用2. 这个执行haskell-updater, unmask安装不成功的package就可以.

- 然后遇到latex问题,报错,装了texlive-fontsrecommands也不好使.中文乱码. 为了使之后的编译通过，建议把 ctex 包也安装了。

- 然后原来需要xelatex, 且指定中文字体名:

```bash
pandoc  srs.md -o srs.pdf --latex-engine=xelatex -V CJKmainfont=WenQuanYi\ Micro\ Hei\ Mono
```

## Windows system
On Windows systems, make sure first that LaTeX engine is installed (either TeX Live or MiKTeX is fine). Then you can use two different ways to generate pdf.
### First way
Add the following setting to your markdown file,
```yaml
---
CJKmainfont: KaiTi
---
```
Then use `pandoc --pdf-engine=xelatex test.md -o test1.pdf` to generate the pdf file. Make sure that the value (i.e., `KaiTi`) after `CJKmainfont` is a valid font on your system. Check [here](https://jdhao.github.io/2017/05/13/guide-on-how-to-use-chinese-with-matplotlib/) to see how to find the available Chinese font on your system.

### second way
You can use `ctexart` class and do not need to manually designate a font (the package will do it for you). Add the following setting to your markdown file,
```
---
documentclass:
    - ctexart
---
```
The pdf-generating command is the same as first way.