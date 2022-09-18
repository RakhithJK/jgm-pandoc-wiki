Note: there is a page about [installing and using Pandoc with Gentoo](https://wiki.gentoo.org/wiki/Pandoc) on their wiki.

***

Q: when convert a markdown to pdf, it says: ' lmodern.sty' not found

A: You should emerge `dev-texlive/texlive-fontsrecommended`


Here is my env running pandoc OK:

```
➜  r2000 git:(master) ✗ eix ghc -I       
[I] dev-lang/ghc
     Available versions:  (~)6.10.4-r1 6.12.3 6.12.3-r2 (~)7.0.4 (~)7.4.1 (~)7.4.1-r1 {bash-completion binary doc ghcbootstrap llvm}
     Installed versions:  7.4.1-r1(10:52:48 PM 07/29/2012)(-binary -doc -ghcbootstrap -llvm)
     Homepage:            http://www.haskell.org/ghc/
     Description:         The Glasgow Haskell Compiler

➜  r2000 git:(master) ✗ eix pandoc -I
[I] app-text/pandoc
     Available versions:  (~)1.8.1.1-r1!t ~1.9.1.2 ~1.9.2 ~1.9.3 (~)1.9.4.1-r2!t {doc highlight hscolour pdf profile test}
     Installed versions:  1.9.4.1-r2!t(05:04:51 PM 08/01/2012)(-doc -hscolour -profile -test)
     Homepage:            http://johnmacfarlane.net/pandoc
     Description:         Conversion between markup formats

[D] dev-haskell/pandoc-types
     Available versions:  (~)1.8 ~1.9.1 {doc hscolour profile}
     Installed versions:  1.9.1(10:39:31 AM 07/30/2012)(-doc -hscolour -profile)
     Homepage:            http://johnmacfarlane.net/pandoc
     Description:         Types for representing a structured document

Found 2 matches.
➜  r2000 git:(master) ✗ eix texlive -I
[I] app-text/texlive-core
     Available versions:  2011-r6 ~2011-r7 ~2012 {X cjk doc source tk xetex}
     Installed versions:  2011-r6(09:18:07 PM 08/01/2012)(X xetex -cjk -doc -source -tk)
     Homepage:            http://tug.org/texlive/
     Description:         A complete TeX distribution

[I] dev-texlive/texlive-basic
     Available versions:  2011-r1 ~2012 {doc source}
     Installed versions:  2011-r1(04:03:02 PM 07/27/2012)(-doc -source)
     Homepage:            http://www.tug.org/texlive/
     Description:         TeXLive Essential programs and files

[I] dev-texlive/texlive-documentation-base
     Available versions:  2011 ~2012 {source}
     Installed versions:  2011(04:02:41 PM 07/27/2012)(-source)
     Homepage:            http://www.tug.org/texlive/
     Description:         TeXLive TeX Live documentation

[I] dev-texlive/texlive-fontsrecommended
     Available versions:  2011 ~2012 {doc source}
     Installed versions:  2011(06:19:48 PM 08/01/2012)(-doc -source)
     Homepage:            http://www.tug.org/texlive/
     Description:         TeXLive Recommended fonts

[I] dev-texlive/texlive-fontutils
     Available versions:  2011 ~2012 {doc source}
     Installed versions:  2011(04:03:12 PM 07/27/2012)(-doc -source)
     Homepage:            http://www.tug.org/texlive/
     Description:         TeXLive Graphics and font utilities

[I] dev-texlive/texlive-genericrecommended
     Available versions:  2011 ~2012 {doc source}
     Installed versions:  2011(09:18:18 PM 08/01/2012)(-doc -source)
     Homepage:            http://www.tug.org/texlive/
     Description:         TeXLive Recommended generic packages

[I] dev-texlive/texlive-latex
     Available versions:  2011 ~2012 {doc source}
     Installed versions:  2011(04:03:28 PM 07/27/2012)(-doc -source)
     Homepage:            http://www.tug.org/texlive/
     Description:         TeXLive Basic LaTeX packages

[I] dev-texlive/texlive-latexextra
     Available versions:  2011-r2 ~2012 {doc source}
     Installed versions:  2011-r2(09:30:40 PM 08/01/2012)(-doc -source)
     Homepage:            http://www.tug.org/texlive/
     Description:         TeXLive LaTeX supplementary packages

[I] dev-texlive/texlive-latexrecommended
     Available versions:  2011 ~2012 {doc source}
     Installed versions:  2011(04:03:53 PM 07/27/2012)(-doc -source)
     Homepage:            http://www.tug.org/texlive/
     Description:         TeXLive LaTeX recommended packages

[I] dev-texlive/texlive-mathextra
     Available versions:  2011 ~2012 ~2012-r1 {doc source}
     Installed versions:  2011(09:18:26 PM 08/01/2012)(-doc -source)
     Homepage:            http://www.tug.org/texlive/
     Description:         TeXLive Advanced math typesetting

[I] dev-texlive/texlive-pictures
     Available versions:  2011 ~2011-r1 ~2012 {doc source}
     Installed versions:  2011(09:18:40 PM 08/01/2012)(-doc -source)
     Homepage:            http://www.tug.org/texlive/
     Description:         TeXLive Graphics packages and programs

[I] dev-texlive/texlive-xetex
     Available versions:  2011 ~2012 {X doc source}
     Installed versions:  2011(09:30:55 PM 08/01/2012)(X -doc -source)
     Homepage:            http://www.tug.org/texlive/
     Description:         TeXLive XeTeX packages

Found 12 matches.
➜  r2000 git:(master) ✗ eix latex -I
[I] dev-texlive/texlive-latex
     Available versions:  2011 ~2012 {doc source}
     Installed versions:  2011(04:03:28 PM 07/27/2012)(-doc -source)
     Homepage:            http://www.tug.org/texlive/
     Description:         TeXLive Basic LaTeX packages

[I] dev-texlive/texlive-latexextra
     Available versions:  2011-r2 ~2012 {doc source}
     Installed versions:  2011-r2(09:30:40 PM 08/01/2012)(-doc -source)
     Homepage:            http://www.tug.org/texlive/
     Description:         TeXLive LaTeX supplementary packages

[I] dev-texlive/texlive-latexrecommended
     Available versions:  2011 ~2012 {doc source}
     Installed versions:  2011(04:03:53 PM 07/27/2012)(-doc -source)
     Homepage:            http://www.tug.org/texlive/
     Description:         TeXLive LaTeX recommended packages

[I] virtual/latex-base
     Available versions:  1.0
     Installed versions:  1.0(04:03:58 PM 07/27/2012)
     Description:         Virtual for basic LaTeX bin
```