## 使用条件
### 安装一个 LaTeX 发行版
首先你需要确保安装了一个支持 `xelatex` 的 LaTeX 引擎. 你可以安装 [Tex Live](https://www.tug.org/texlive/quickinstall.html). 在 Windows 系统上, [MikTex](https://miktex.org/) 也是一个可行的选择, 但是 TeX Live 更好.
### 找到一个有效的中文字体
你可以使用指令 `fc-list :lang=zh` 来找到一个在你的系统上可用的中文字体, 以下是使用此指令的例子:
```
[~]$ fc-list :lang=zh
/usr/share/fonts/source_han_serif/SourceHanSerifCN-ExtraLight.otf: Source Han Serif CN,思源宋体 CN,Source Han Serif CN ExtraLight,思源宋体 CN ExtraLight:style=ExtraLight,Regular
/usr/share/fonts/source_han_serif/SourceHanSerifCN-Bold.otf: Source Han Serif CN,思源宋体 CN:style=Bold
/usr/share/fonts/wqy-microhei/wqy-microhei.ttc: WenQuanYi Micro Hei Mono,文泉驛等寬微米黑,文泉驿等宽微米黑:style=Regular
/usr/share/fonts/wqy-zenhei/wqy-zenhei.ttc: WenQuanYi Zen Hei Sharp,文泉驛點陣正黑,文泉驿点阵正黑:style=Regular
/usr/share/fonts/source_han_serif/SourceHanSerifCN-SemiBold.otf: Source Han Serif CN,思源宋体 CN,Source Han Serif CN SemiBold,思源宋体 CN SemiBold:style=SemiBold,Regular
/usr/share/fonts/source_han_serif/SourceHanSerifCN-Medium.otf: Source Han Serif CN,思源宋体 CN,Source Han Serif CN Medium,思源宋体 CN Medium:style=Medium,Regular
/usr/share/fonts/wqy-microhei/wqy-microhei.ttc: WenQuanYi Micro Hei,文泉驛微米黑,文泉驿微米黑:style=Regular
/usr/share/fonts/cjkuni-uming/uming.ttc: AR PL UMing TW MBE:style=Light
/usr/share/fonts/source_han_serif/SourceHanSerifCN-Light.otf: Source Han Serif CN,思源宋体 CN,Source Han Serif CN Light,思源宋体 CN Light:style=Light,Regular
/usr/share/fonts/source_han_serif/SourceHanSerifCN-Heavy.otf: Source Han Serif CN,思源宋体 CN,Source Han Serif CN Heavy,思源宋体 CN Heavy:style=Heavy,Regular
/usr/share/fonts/wqy-zenhei/wqy-zenhei.ttc: WenQuanYi Zen Hei Mono,文泉驛等寬正黑,文泉驿等宽正黑:style=Regular
/usr/share/fonts/wqy-zenhei/wqy-zenhei.ttc: WenQuanYi Zen Hei,文泉驛正黑,文泉驿正黑:style=Regular
/usr/share/fonts/cjkuni-uming/uming.ttc: AR PL UMing TW:style=Light
/usr/share/fonts/cjkuni-uming/uming.ttc: AR PL UMing HK:style=Light
/usr/share/fonts/cjkuni-uming/uming.ttc: AR PL UMing CN:style=Light
/usr/share/fonts/source_han_serif/SourceHanSerifCN-Regular.otf: Source Han Serif CN,思源宋体 CN:style=Regular
```
## 在 Linux 系统上制作 PDF 文件

### 方案一
使用以下指令来制作你的 PDF 文件. (参考[这里](http://pandoc.org/faqs.html#i-get-a-blank-document-when-i-try-to-convert-a-markdown-document-in-chinese-to-pdf-using-pandoc--o-test.pdf-test.markdown.)):
```bash
pandoc  srs.md -o srs.pdf --latex-engine=xelatex -V mainfont='WenQuanYi Micro Hei Mono'
```
注意, 在上面的指令参数中应当使用 `mainfont` 而非 `CJKmainfont`.

### 方案二
在你的 Markdown 文档中加入如下配置,
```yaml
---
mainfont: WenQuanYi Micro Hei Mono
---
```
然后使用 `pandoc --latex-engine=xelatex test.md -o test1.pdf` 生成 PDF 文件.

### 方案三
你可以使用 `ctexart` 类而不需要手动指定字体 (宏包将替你执行此操作), 将以下设置添加到markdown文档中,
```
---
documentclass:
    - ctexart
---
```
生成 PDF 文档的指令和方案二是相同的. `pandoc --latex-engine=xelatex test.md -o test1.pdf`.


## 在 Windows 系统上制作 PDF 文件
命令与 Linux 系统大致相同. 在 Windows 上, `--latex-engine` 选项被删除, 使用 `--pdf-engine` 代替.

### 方案一
使用以下指令来生成你的 PDF 文件.
```bash
pandoc  srs.md -o srs.pdf --pdf-engine=xelatex -V CJKmainfont=KaiTi
```
### 方案二
在你的 Markdown 文档中加入如下配置:
```yaml
---
CJKmainfont: KaiTi
---
```
然后使用 `pandoc --pdf-engine=xelatex test.md -o test1.pdf` 生成 PDF 文件.

### 方案三
你可以使用 `ctexart` 类而不需要手动指定字体 (宏包将替你执行此操作), 将以下设置添加到markdown文档中,
```
---
documentclass:
    - ctexart
---
```
生成 PDF 文档的指令和方案二是相同的. `pandoc --latex-engine=xelatex test.md -o test1.pdf`.