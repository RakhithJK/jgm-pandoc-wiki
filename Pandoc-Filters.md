Pandoc provides an interface for users to write programs (known as filters) which act on the intermediate AST. A tutorial about filters can be found [here](http://johnmacfarlane.net/pandoc/scripting.html).

This page collects together third party filters which can be used to add functionality to pandoc.

## Writing Filters

Filters can be written in any programming language. [[Pandoc wrappers and interfaces]] are available in the following programming languages to facilitate modification of the AST:

| language	| link	| description	|  
|  ------	| ------	| ------	|  
| Python	| [pandocfilters](https://github.com/jgm/pandocfilters)	| a library for writing pandoc filters in python.	| 
| Python	| [panflute](https://github.com/sergiocorreia/panflute)	| a pythonic alternative to `pandocfilters`, with batteries included.(@jgm recommended this in [pandoc discuss](https://groups.google.com/forum/#!searchin/pandoc-discuss/I$27d$20recommend$20that$20people$20use$20panflute$20instead%7Csort:relevance/pandoc-discuss/wbebx65e1Nk/prx8_jLnAQAJ))	| 
| PHP	| [pandocfilters-php](https://github.com/vinai/pandocfilters-php)	| a port of the python [pandocfilters](https://github.com/jgm/pandocfilters) module to PHP to make writing filters in PHP easier.	| 
| Node.js	| [pandoc-filter-node](https://github.com/mvhenderson/pandoc-filter-node)	| a Node.js module for writing pandoc filters in JavaScript.	|  
| Perl	| [Pandoc::Elements](https://metacpan.org/release/Pandoc-Elements)	| a CPAN module for writing pandoc filters in Perl.	| 
| Groovy	| [groovy-pandoc](https://github.com/dfrommi/groovy-pandoc)	| a library for writing Pandoc filters in Groovy.	| 
| Ruby | [paru](https://heerdebeer.org/Software/markdown/paru/) | a Ruby gem to write pandoc filters in Ruby. |

Other tools:

- [vimhl](https://github.com/lyokha/vim-publish-helper), a vim plugin that makes **vim syntax highlighting** engine available in pandoc.
- [pandoc-jats](https://github.com/mfenner/pandoc-jats), a **Lua custom writer** for Pandoc generating **JATS XML**.

## Written Filters

The following is a list of some known 3rd party filters:

- Images related:
	- [pandoc-svg](https://gist.github.com/jeromerobert/3996eca3acd12e4c3d40), a pandoc filter to convert svg files to pdf by Jerome Robert.
	- [diagrams-pandoc](http://hackage.haskell.org/package/diagrams-pandoc) for inserting images expressed in the Haskell [diagrams](http://projects.haskell.org/diagrams/) DSL.
	- [mermaid-pandoc](https://github.com/raghur/mermaid-filter) for inserting images expressed in [mermaid](http://knsv.github.io/mermaid/) syntax
	- [r-pandoc](https://github.com/cdupont/r-pandoc) for inserting plots expressed in the R language
	- [paru-screenshot.rb](https://github.com/htdebeer/paru-filter-collection#paru-screenshotrb) for automatically taking a screen shot of a web page and including that shot as an image in a markdown file.
- Numbering related:
	- [Numerical reference to sections](https://gist.github.com/jkr/bcfacbfdcf4cc4bafcf6), using a specified sign (by default `#`) in internal links. Metadata can configure special sign and whether links should be preserved or converted to plain text.
	- [pandoc-fignos](https://github.com/tomduck/pandoc-fignos), for numbering figures and figure references.
	- [pandoc-eqnos](https://github.com/tomduck/pandoc-eqnos), for numbering equations and equation references.
	- [pandoc-tablenos](https://github.com/tomduck/pandoc-tablenos), for numbering tables and table references.
	- [pandoc-crossref](https://github.com/lierdakil/pandoc-crossref), for numbering and cross-referencing figures, equations and tables
	- [pandoc-numbering](https://github.com/chdemko/pandoc-numbering), for numbering and cross-referencing any kinds of things such as examples, theorems, exercises and so on
	- [pandoc-listof](https://github.com/chdemko/pandoc-listof), for creating lists of any kinds (**deprecated**)
	- [pandoc-amsthm](https://github.com/ickc/pandoc-amsthm): a pandoc amsthm package to define the use of amsthm through YAML front matter, target at HTML and LaTeX outputs. For HTML, CSS counter is used and defined in a template (by the YAML variables). For LaTeX amsthm package is used and defined in a template (by the YAML variables).
- Math related:
    - [pandoc-mathjax-filter](https://github.com/shreevatsa/pandoc-mathjax-filter) rendering math to SVG using mathjax-node
    - [mathjax-pandoc-filter](https://github.com/lierdakil/mathjax-pandoc-filter) rendering math to SVG using mathjax-node
    - [asciimathml-pandocfilter](https://github.com/yuwash/asciimathml-pandocfilter): to add read support for AsciiMathML syntax through conversion into LaTeX
    - [pandoc-unicode-math](https://github.com/marhop/pandoc-unicode-math) replaces Unicode math symbols and greek letters like ∀, ∈, →, λ, or Ω in math environments by equivalent Latex commands like `\forall`, `\in`, `\rightarrow`, `\lambda`, or `\Omega`.
- LaTeX related:
	- [Using markdown inside raw latex commands](https://gist.github.com/mpickering/f1718fcdc4c56273ed52)
	- [pandoc-latex-environment](https://github.com/chdemko/pandoc-latex-environment), for adding LaTeX environment on specific HTML `div` tags
	- [latexdivs.py](https://github.com/jgm/pandocfilters/blob/master/examples/latexdivs.py): define a syntax to turn any native pandoc Divs into a LaTeX environment: if `latex="true"` is in the attribute of the Div, the first class is used to define the LaTeX environment.
	- [pandoc-latex-tip](https://github.com/chdemko/pandoc-latex-tip), for decorating specific `div` and `span` elements by icons taken from the [Font-Awesome icons collection](http://fontawesome.io/icons/)
	- [pandoc-latex-admonition](https://github.com/chdemko/pandoc-latex-admonition), for decorating specific `div` and `codeblock` elments by admonitions
	- [pandoc-latex-barcode](https://github.com/daamien/pandoc-latex-barcode): insert a barcode or a QR code into a latex/PDF document.
	- [pandoc-latex-fontsize](https://github.com/chdemko/pandoc-latex-fontsize), for specifying LaTeX font size on `span`, `code`, `div` and `codeblock` elements
	- [pandoc-latex-unlisted](https://github.com/chdemko/pandoc-latex-unlisted), for unlisting some specific headers in the table of contents
	- [pandoc-latex-newpage](https://github.com/chdemko/pandoc-latex-newpage), for converting horizontal rules into new page
- RAW related:
	- [Pandoc filter to insert arbitrary raw output markup as Code/CodeBlocks with an attribute raw=<outputformat>.](https://gist.github.com/bpj/e6e53cbe679d3ec77e25): Pandoc filter to insert arbitrary raw output markup as Code/CodeBlocks with an attribute `raw=<outputformat>`.
	- [Include Files](http://pandoc.org/scripting.html#include-files): finds all the inline code blocks with attribute include, and replaces their contents with the contents of the file given
	- [pandoc-doc2tex-filter](https://github.com/kuba-orlik/pandoc-dot2tex-filter) - a filter that converts dot notation to PGF/TikZ graphics for latex/pdf rendering.
	- [HTML comment to LaTeX comment](https://github.com/jgm/pandoc/issues/1926#issuecomment-122308490): a filter that converts HTML comment to LaTeX comments
- Tables related:
	- [pandoc-csv2table](https://github.com/baig/pandoc-csv2table) for including referenced csv files in markdown as markdown rendered tables.
	- [pandoc-placetable](https://github.com/mb21/pandoc-placetable) lightweight implementation of the idea behind the above `pandoc-csv2table` (e.g. doesn't necessarily require pandoc as a cabal dependency)
	- [ickc/pantable: CSV Tables in Markdown: Pandoc Filter for CSV Tables](https://github.com/ickc/pantable): a Python alternatives to the above 2 filters, using panflute, with some enhancements (e.g. auto-width, fractional width, etc.)
	- Creating a [link table](http://stackoverflow.com/questions/26406816/pandoc-is-there-a-way-to-include-an-appendix-of-links-in-a-pdf-from-markdown/26415375#26415375) at the end of your document.
- Text related:
    - [pandoc-abbreviations](https://github.com/scokobro/pandoc-abbreviations) allows the use of arbitrary abbreviations, defined in an abbreviations file or in the source document's YAML header, which are replaced on  processing. Useful for maintaining consistency of terminology etc.
    - [pandoc-mustache](https://github.com/michaelstepner/pandoc-mustache) replaces variables like `{{varname}}` in a pandoc document with their values, which are stored in a separate YAML file.
- Others:
	- [Adding support](https://gist.github.com/mpickering/8bc9bb34c4e9b076b107) for indexing with the syntax ``(# term, subterm)`` in html and latex
	- [Adding non-breaking spaces inside a URL to preserve formatting](https://gist.github.com/mpickering/fdc747b9c8306659cb43)
	- [lablinkfix](https://github.com/klpn/lablinkfix) updates links to the Swedish Labour Movement Archives and Library catalogues.
	- [second-date](https://gist.github.com/7937d04120ac27fcfb1955ae15773b05)  changes `date` metadata to a different strftime format using python's dateutil.
    - [pandoc_abnt](https://github.com/limarka/pandoc_abnt) allow to specify the source of images and tables, and automatically corrects *Alineas* according to Brazilian's standard for Academic writings (ABNT NBR 14724:2011). 