- [Writing Filters](#writing-filters)
- [Written Filters](#written-filters)
  - [Document (DOCX/ODT) related](#document-docxodt-related)
  - [Images related](#images-related)
  - [Numbering related](#numbering-related)
  - [Math related](#math-related)
  - [LaTeX related](#latex-related)
  - [RAW related](#raw-related)
  - [Tables related](#tables-related)
  - [Text related](#text-related)
  - [Running Code related](#running-code-related)
  - [Others](#others)

Pandoc provides an interface for users to write programs (known as filters) which act on the intermediate AST. For more info see the [filter tutorial](http://pandoc.org/filters.html) and the [Lua filter tutorial](http://pandoc.org/lua-filters.html).

This page collects together third party filters which can be used to add functionality to pandoc.

## Writing Filters

Filters can be written in any programming language. [[Pandoc wrappers and interfaces]] are available in the following programming languages to facilitate modification of the AST:

| language	| link	| description	|  
|  ------	| ------	| ------	|  
| Python	| [pandocfilters](https://github.com/jgm/pandocfilters)	| a library for writing pandoc filters in python.	| 
| Python	| [panflute](https://github.com/sergiocorreia/panflute)	| a pythonic alternative to `pandocfilters`, with batteries included. It reconstructs pandoc AST in an internal panflute AST which makes it more seamless in interacting with the AST. (@jgm recommended this in [pandoc discuss](https://groups.google.com/forum/#!searchin/pandoc-discuss/I$27d$20recommend$20that$20people$20use$20panflute$20instead%7Csort:relevance/pandoc-discuss/wbebx65e1Nk/prx8_jLnAQAJ))	| 
| Python | [pantable](https://github.com/ickc/pantable) | specialized in writing filter for tables based on [panflute](https://github.com/sergiocorreia/panflute), which provides a lossless conversion between an internal structure and panflute AST. |
| PHP	| [pandocfilters-php](https://github.com/vinai/pandocfilters-php)	| a port of the python [pandocfilters](https://github.com/jgm/pandocfilters) module to PHP to make writing filters in PHP easier.	| 
| Node.js	| [pandoc-filter-node](https://github.com/mvhenderson/pandoc-filter-node)	| a Node.js module for writing pandoc filters in JavaScript.	|  
| Perl	| [Pandoc::Elements](https://metacpan.org/release/Pandoc-Elements)	| a CPAN module for writing pandoc filters in Perl.	| 
| Groovy	| [groovy-pandoc](https://github.com/dfrommi/groovy-pandoc)	| a library for writing Pandoc filters in Groovy.	| 
| Ruby | [paru](https://heerdebeer.org/Software/markdown/paru/) | a Ruby gem to write pandoc filters in Ruby. |
| Lua | [pandoc's official documentation](https://pandoc.org/lua-filters.html) | Pandoc includes a [lua](https://www.lua.org/) interpreter by default so is quite lightweight|
| Elixir | [Panpipe](https://github.com/marcelotto/panpipe) | a library for writing pandoc filters in Elixir |
| .NET | [PandocFilters](https://www.nuget.org/packages/PandocFilters/) | a NuGet package for writing Pandoc filters in .NET languages |
| OCaml | [ocaml-pandoc](https://github.com/smimram/ocaml-pandoc) | An [OCaml](https://ocaml.org/) library for writing pandoc filters. |

Other tools:

- [vimhl](https://github.com/lyokha/vim-publish-helper), a vim plugin that makes **vim syntax highlighting** engine available in pandoc.
- [pandoc-jats](https://github.com/mfenner/pandoc-jats), a **Lua custom writer** for Pandoc generating **JATS XML**.
- [2bbcode](https://github.com/lilydjwg/2bbcode), a Lua custom writer for BBCode.
- [pandocmeta.lua](https://github.com/odkr/pandocmeta.lua), a simple Lua package that converts Pandoc metadata types into a, possibly multi-dimensional, table.

## Written Filters

See [github.com/pandoc/lua-filters](https://github.com/pandoc/lua-filters) for some select filters written in Lua. Some other known 3rd party filters:

### Document (DOCX/ODT) related

- Because DOCX and ODT files cannot use templates, we are limited in
  how we can transform metadata into document content. Several
  [paru](https://heerdebeer.org/Software/markdown/paru/) filters can
  help to solve this, given a metadata format involving authors with
  affiliation/correspondence fields and institute information:
  [README](https://github.com/iandol/dotpandoc/tree/master/filters);
  and individual filters:
  [simplifyMetadata](https://github.com/iandol/dotpandoc/blob/master/filters/simplifyMetadata),
  [prependInstitute](https://github.com/iandol/dotpandoc/blob/master/filters/prependInstitute),
  [prependKeywords](https://github.com/iandol/dotpandoc/blob/master/filters/prependKeywords),
  [prependAbstract](https://github.com/iandol/dotpandoc/blob/master/filters/prependAbstract),
  [prependComments](https://github.com/iandol/dotpandoc/blob/master/filters/prependComments)
  --- filters combined:
  [prependAll](https://github.com/iandol/dotpandoc/blob/master/filters/prependAll).

- [pandoc-odt-filters](https://github.com/jzeneto/pandoc-odt-filters): filters that improve ODT output --- creates sequences in image and table captions (for automatic list-of-figures and list-of-tables), corrects links to images and tables, corrects bibliography style, custom styles to headers and spans, better list styles and real smallcaps. Some of the filters are configurable.

### Images related

- [pandoc-svg](https://gist.github.com/jeromerobert/3996eca3acd12e4c3d40),
  a pandoc filter to convert svg files to pdf by Jerome Robert.
- [diagrams-pandoc](http://hackage.haskell.org/package/diagrams-pandoc)
  for inserting images expressed in the Haskell
  [diagrams](http://projects.haskell.org/diagrams/) DSL.
- [mermaid-pandoc](https://github.com/raghur/mermaid-filter) for
  inserting images expressed in
  [mermaid](http://knsv.github.io/mermaid/) syntax
- [r-pandoc](https://github.com/cdupont/r-pandoc) for inserting plots
  expressed in the R language
- [paru-screenshot.rb](https://github.com/htdebeer/paru-filter-collection#paru-screenshotrb)
  for automatically taking a screen shot of a web page and including
  that shot as an image in a markdown file.
- [pandoc-plot](https://github.com/LaurentRDC/pandoc-plot) to generate and embed figures based on code 
  blocks in documents, using a variety of toolkits (e.g. Matplotlib, MATLAB, gnuplot, ggplot2, etc.). Easy 
  integration with Haskell libraries (e.g. Hakyll)

### Numbering related

- [Numerical reference to
  sections](https://gist.github.com/jkr/bcfacbfdcf4cc4bafcf6), using a
  specified sign (by default `#`) in internal links. Metadata can
  configure special sign and whether links should be preserved or
  converted to plain text.
- [pandoc-fignos](https://github.com/tomduck/pandoc-fignos), for
  numbering figures and figure references.
- [pandoc-eqnos](https://github.com/tomduck/pandoc-eqnos), for
  numbering equations and equation references.
- [pandoc-tablenos](https://github.com/tomduck/pandoc-tablenos), for
  numbering tables and table references.
- [pandoc-crossref](https://github.com/lierdakil/pandoc-crossref), for
  numbering and cross-referencing figures, equations and tables
- [pandoc-numbering](https://github.com/chdemko/pandoc-numbering), for
  numbering and cross-referencing any kinds of things such as
  examples, theorems, exercises and so on
- [pandoc-ling](https://github.com/cysouw/pandoc-ling), for formatting, 
  numbering and cross-referencing linguistic examples
- [pandoc-listof](https://github.com/chdemko/pandoc-listof), for
  creating lists of any kinds (**deprecated**)
- [pandoc-amsthm](https://github.com/ickc/pandoc-amsthm): a pandoc
  amsthm package to define the use of amsthm through YAML front
  matter, target at HTML and LaTeX outputs. For HTML, CSS counter is
  used and defined in a template (by the YAML variables). For LaTeX
  amsthm package is used and defined in a template (by the YAML
  variables). -
  [definitionlist-filter.lua](https://gist.github.com/rriemann/2dfa7f4b1147d7f3fad506cf6f863cd7),
  for converting some definition lists to theorem-like (amsthm)
  Environments and some references to cref tags in LaTeX

### Math related

- [mathjax-pandoc-filter](https://github.com/lierdakil/mathjax-pandoc-filter)
  rendering math to SVG using mathjax-node
- [asciimathml-pandocfilter](https://github.com/yuwash/asciimathml-pandocfilter):
  to add read support for AsciiMathML syntax through conversion into
  LaTeX
- [pandoc-unicode-math](https://github.com/marhop/pandoc-unicode-math)
  replaces Unicode math symbols and greek letters like ∀, ∈, →, λ, or
  Ω in math environments by equivalent Latex commands like `\forall`,
  `\in`, `\rightarrow`, `\lambda`, or `\Omega`.
- [SugarTeX](https://github.com/kiwi0fruit/sugartex) is a more
  readable LaTeX language extension and transcompiler to LaTeX. Fast
  Unicode autocomplete in Atom editor via SugarTeX Completions for
  Atom.

### LaTeX related

- [Using markdown inside raw latex
  commands](https://gist.github.com/mpickering/f1718fcdc4c56273ed52)
- [pandoc-latex-environment](https://github.com/chdemko/pandoc-latex-environment),
  for adding LaTeX environment on specific HTML `div` tags
- [latexdivs.py](https://github.com/jgm/pandocfilters/blob/master/examples/latexdivs.py):
  define a syntax to turn any native pandoc Divs into a LaTeX
  environment: if `latex="true"` is in the attribute of the Div, the
  first class is used to define the LaTeX environment.
- [pandoc-latex-tip](https://github.com/chdemko/pandoc-latex-tip), for
  decorating specific `span`, `code`, `div` and `codeblock` elements
  by icons taken from popular icon collections.
- [pandoc-latex-admonition](https://github.com/chdemko/pandoc-latex-admonition),
  for decorating specific `div` and `codeblock` elments by admonitions
- [pandoc-latex-barcode](https://github.com/daamien/pandoc-latex-barcode):
  insert a barcode or a QR code into a latex/PDF document.
- [pandoc-latex-fontsize](https://github.com/chdemko/pandoc-latex-fontsize),
  for specifying LaTeX font size on `span`, `code`, `div` and
  `codeblock` elements
- [pandoc-latex-color](https://github.com/chdemko/pandoc-latex-color),
  for specifying X11 color on `span` and `div` elements
- [pandoc-latex-unlisted](https://github.com/chdemko/pandoc-latex-unlisted),
  for unlisting some specific headers in the table of contents (**deprecated** since it is now in the core of pandoc)
- [pandoc-latex-newpage](https://github.com/chdemko/pandoc-latex-newpage),
  for converting horizontal rules into new page
- [pandoc-latex-french-spaces](https://github.com/chdemko/pandoc-latex-french-spaces),
  for dealing with french spaces around some punctuation marks
- [pandoc-latex-margin](https://github.com/chdemko/pandoc-latex-margin),
  for setting left and right margins on `div` and `codeblock` elements
- [pandoc-beamer-block](https://github.com/chdemko/pandoc-beamer-block),
  for using `block`, `alertblock` and `exampleblock` environment
  defined in beamer.

### RAW related

- [Include Files](http://pandoc.org/scripting.html#include-files):
  finds all the inline code blocks with attribute include, and
  replaces their contents with the contents of the file given
- [code-includes.lua](https://github.com/a-vrma/pandoc-filters#code-includes)
  Include code from source files. Keep your examples and documentation compiled and
  in-sync. Similar to the above except you don't have to install Haskell and you can
  select by line number.
- [pandoc-dot2tex-filter](https://github.com/kuba-orlik/pandoc-dot2tex-filter) -
  a filter that converts dot notation to PGF/TikZ graphics for
  latex/pdf rendering.
- [HTML comment to LaTeX
  comment](https://github.com/jgm/pandoc/issues/1926#issuecomment-122308490):
  a filter that converts HTML comment to LaTeX comments

### Tables related

- [pandoc-csv2table](https://github.com/baig/pandoc-csv2table) for
  including referenced csv files in markdown as markdown rendered
  tables.
- [pandoc-placetable](https://github.com/mb21/pandoc-placetable)
  lightweight implementation of the idea behind the above
  `pandoc-csv2table` (e.g. doesn\'t necessarily require pandoc as a
  cabal dependency)
- [ickc/pantable: CSV Tables in Markdown: Pandoc Filter for CSV
  Tables](https://github.com/ickc/pantable): a Python alternatives to
  the above 2 filters, using panflute, with some enhancements (e.g.
  auto-width, fractional width, etc.)
- Creating a [link
  table](http://stackoverflow.com/questions/26406816/pandoc-is-there-a-way-to-include-an-appendix-of-links-in-a-pdf-from-markdown/26415375#26415375)
  at the end of your document. -
  [pandocsql](https://github.com/alexpdp7/pandocsql) run SQL queries
  on tables, generating other tables

### Text related

- [pandoc-abbreviations](https://github.com/scokobro/pandoc-abbreviations)
  allows the use of arbitrary abbreviations, defined in an
  abbreviations file or in the source document\'s YAML header, which
  are replaced on processing. Useful for maintaining consistency of
  terminology etc.
- [pandoc-acronyms](https://gitlab.com/mirkoboehm/pandoc-acronyms) is
  a filter for managing acronyms. It replaces acronyms like \"FAQ\" at
  first use with the full text \"frequently asked questions (FAQ)\".
  It is installed using
  [pip](https://pypi.org/project/pandoc-acronyms/).
- [count-para.lua](https://github.com/cysouw/count-para) add numbering to 
  paragraphs to allow for detailed citation (in scientific context). Proposal
  to replace page-number referencing, which does not work with adaptive design. 
- [pandoc-lang](https://github.com/davidar/pandoc-lang) automatically
  detects the (natural) language of text, as well as the programming
  language of code blocks
- [pandoc-mustache](https://github.com/michaelstepner/pandoc-mustache)
  replaces variables like `{{varname}}` in a pandoc document with
  their values, which are stored in a separate YAML file.
- [pandoc-quotes.lua](https://github.com/odkr/pandoc-quotes.lua) and
  the older [pandoc-quotes](https://github.com/odkr/pandoc-quotes)
  replace non-typographic, quotation marks with typographic ones for
  languages other than US English.
- [transclude.lua](https://github.com/a-vrma/pandoc-filters#transclude)
  Include content from another file just like in AsciiDoc and ReST.
- [columns](https://github.com/jdutant/columns) Multiple columns support.

### Running Code related

- [R-pandoc](https://github.com/cdupont/R-pandoc) for generating R
  plots
- [filter\_pandoc\_run\_py](https://github.com/caiofcm/filter_pandoc_run_py)
  for executing python codes written in code blocks and also embedding
  print output and pyplot figures
- [pandoc-plot](https://github.com/LaurentRDC/pandoc-plot) to generate
  and embed figures based on code blocks in documents, using a variety
  of toolkits (e.g. Matplotlib, MATLAB, gnuplot, ggplot2, etc.). Easy
  integration with Haskell libraries (e.g.
  [Hakyll](https://jaspervdj.be/hakyll/))
- [Knitty](https://github.com/kiwi0fruit/knitty): is a Pandoc filter
  for reproducible reports via Jupyter and Pandoc (Stitch\'s fork that
  is a Knitr-RMarkdown-like lib). Insert Python code (or other Jupyter
  kernel code) to the Markdown document or write in *plain
  Python/Julia/R/any-kernel-lang* with block-commented Markdown and
  have code\'s results in the Pandoc output document.
- [pandocsql](https://github.com/alexpdp7/pandocsql) which uses an
  in-memory SQLite database. It creates tables from tables in the
  document and executes queries in code blocks, showing the results as
  tables.
- [pannb](https://ickc.github.io/pannb/), a pandoc filter to control the output from ipynb input,
  this includes metadata block, filter out Python code, and converting all raw-blocks to native pandoc AST.
  The 3 can be mixed and matched.

### Citation related

- [pandoc-manubot-cite](https://github.com/manubot/manubot#pandoc-filter)
  allows citing persistent identifiers directly like `@doi:10/c7np` or `@pubmed:29618526`.
  Removes the need for a reference manager by supporting DOIs, PubMed IDs, URLs, ISBNs, Wikidata IDs, and the hundreds of other ID types registered with <https://identifiers.org>.
  Written in Python.
  Available on [PyPI](https://pypi.org/project/manubot/).
- [pandoc-url2cite](https://github.com/phiresky/pandoc-url2cite)
  allows citing certain persistent identifiers directly (URLs, ISBNs, and DOIs). Basically a less opinionated and simpler version of pandoc-manubot-cite.
  Written in TypeScript.
  Available on [npm](https://www.npmjs.com/package/pandoc-url2cite).
- [pandoc-zotxt.lua](https://github.com/odkr/pandoc-zotxt.lua)
  looks up sources for citations in Zotero.


### Others

- [Adding
  support](https://gist.github.com/mpickering/8bc9bb34c4e9b076b107)
  for indexing with the syntax `(# term, subterm)` in html and latex
- [Adding non-breaking spaces inside a URL to preserve
  formatting](https://gist.github.com/mpickering/fdc747b9c8306659cb43)
- [toc-css](https://github.com/cysouw/toc-css) Lua filter changing the appearance of the Pandoc basic HTML table of contents by some CSS and vanilla Javascript.
- [lablinkfix](https://github.com/klpn/lablinkfix) updates links to
  the Swedish Labour Movement Archives and Library catalogues.
- [second-date](https://gist.github.com/7937d04120ac27fcfb1955ae15773b05)
  changes `date` metadata to a different strftime format using
  python\'s dateutil.
- [pandoc\_abnt](https://github.com/limarka/pandoc_abnt) allow to
  specify the source of images and tables, and automatically
  corrects *Alineas* according to Brazilian\'s standard for Academic
  writings (ABNT NBR 14724:2011).
- [pandoc-refheadstyle](https://github.com/odkr/pandoc-refheadstyle)
  sets a custom style for the reference section header; there\'s a
  Lua version, too:
  [pandoc-refheadstyle.lua](https://github.com/odkr/pandoc-refheadstyle.lua)
- [nheengatu](http://joseflavio.com/nheengatu/) provides several
  resources for publishing multimedia content through formats such
  as LaTeX, HTML and EPUB.
- [code-includes.lua](https://github.com/a-vrma/pandoc-filters#code-includes)
  Include code from source files. Keep your examples and documentation compiled and
  in-sync.
