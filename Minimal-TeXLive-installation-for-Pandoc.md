

In the course of setting up Pandoc on a laptop with limited resources I have through trial and error determined the TeXLive packages needed to produce a very simple PDF with Pandoc using `--pdf-engine=pdflatex`, `--pdf-engine=xelatex`, `--pdf-engine=lualatex` or `--pdf-engine=context`.

It turns out that [the list of needed packages in the Pandoc manual](https://pandoc.org/MANUAL.html#creating-a-pdf) is not sufficient, not only because in some cases the names of the .sty files and the TeXLive packages don't coincide. Since I in the end decided to include `collection-context`, `collection-luatex` and `collection-xetex` for good measure there may actually be some duplication in the list below.

Note that when running `install-tl` as described [on the TeXLive quick install page](http://www.tug.org/texlive/quickinstall.html) you should actively select the `basic` scheme and deselect docs and sources in the options if you want a very lean install as I did here.

Note that if you have manually added the path to TeXLive's `bin` directory to your `PATH` variable as is typically the case it will not be in the root's `PATH` when running with `sudo`. The workaround is to use `env` to temporarily set the root's `PATH` to be identical to your `PATH` by running

````sh
sudo env "PATH=$PATH" sh pandoc-texlive.sh
````

The script can be conveniently downloaded by cloning [this gist](https://git.io/fjrJc). Most likely I will be updating it there rather than here!

``````sh
#!/bin/sh

# pandoc-texlive.sh
# TeXLive packages needed to produce PDFs with Pandoc
# using different *TeX `--pdf-engine` options.

tlmgr install \
scheme-basic \
amsfonts \
amsmath \
babel \
biber \
biblatex \
bibtex \
bidi \
booktabs \
collection-context \
collection-luatex \
collection-xetex \
csquotes \
ec \
fancyvrb \
filehook \
fontspec \
footnotehyper \
geometry \
graphics \
hyperref \
ifluatex \
ifxetex \
listings \
lm \
lm-math \
lualatex-math \
luaotfload \
mathspec \
microtype \
natbib \
oberdiek \
parskip \
polyglossia \
setspace \
tools \
ulem \
unicode-math \
upquote \
xcolor \
xurl \
zapfding \
``````
