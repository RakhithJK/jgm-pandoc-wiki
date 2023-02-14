## Prerequisite
### Install a LaTeX distribution
Make sure first that LaTeX engine with support for xelatex is installed. You can install [Tex Live](https://www.tug.org/texlive/quickinstall.html). On Windows systems, [MikTex](https://miktex.org/) is also an viable option. But TeX Live is prefered.

### Find a valid Chinese font
You can use `fc-list :lang=zh` to find valid Chinese font on your system, an example output is shown below:
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

## Methods

### First way
Use the following command to produce your pdf file (reference [here](http://pandoc.org/faqs.html#i-get-a-blank-document-when-i-try-to-convert-a-markdown-document-in-chinese-to-pdf-using-pandoc--o-test.pdf-test.markdown.)):
```bash
pandoc  srs.md -o srs.pdf --pdf-engine=xelatex -V mainfont='WenQuanYi Micro Hei Mono'
```
Note that in the above command, it should be `mainfont` not `CJKmainfont`.

### Second way
Add the following setting to your markdown file,
```yaml
---
mainfont: WenQuanYi Micro Hei Mono
---
```
Then use `pandoc --pdf-engine=xelatex test.md -o test1.pdf` to generate the pdf file.

### Third way
You can use `ctexart` class and do not need to manually designate a font (the package will do it for you). Add the following setting to your markdown file,
```
---
documentclass:
    - ctexart
---
```
The pdf-generating command is the same as second way.