The following is about using Chinese with pandoc on Linux platform, including the problem with PDF output, which requires XeLaTeX/LuaLaTeX and custom fonts.

- 首先编译遇到内存溢出问题, 是ghc7.4.1以前有bug,用7.4.1能编过.

- 然后遇到说network依赖的ghc版本不对的问题,pasace 用3 network用2. 这个执行haskell-updater, unmask安装不成功的package就可以.

- 然后遇到latex问题,报错,装了texlive-fontsrecommands也不好使.中文乱码.

- 然后原来需要xelatex, 且指定中文字体名:

```bash
pandoc  srs.md -o srs.pdf --latex-engine=xelatex -V mainfont=WenQuanYi\ Micro\ Hei\ Mono
```
