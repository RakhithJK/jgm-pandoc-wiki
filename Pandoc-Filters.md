Pandoc provides an interface for users to write programs (known as filters) which act on the intermediate AST. A tutorial about filters can be found [here](http://johnmacfarlane.net/pandoc/scripting.html).

This page collects together third party filters which can be used to add functionality to pandoc.

## Writing Filters

Filters can be written in the following languages:

| language	| link	| description	|  
|  ------	| ------	| ------	|  
| Python	| [pandocfilters](https://github.com/jgm/pandocfilters)	| a library for writing pandoc filters in python.	| 
| PHP	| [pandocfilters-php](https://github.com/vinai/pandocfilters-php)	| a port of the python [pandocfilters](https://github.com/jgm/pandocfilters) module to PHP to make writing filters in PHP easier.	| 
| Node.js	| [pandoc-filter-node](https://github.com/mvhenderson/pandoc-filter-node)	| a Node.js module for writing pandoc filters in JavaScript.	|  
| Perl	| [Pandoc::Elements](https://metacpan.org/release/Pandoc-Elements)	| a CPAN module for writing pandoc filters in Perl.	| 
| Groovy	| [groovy-pandoc](https://github.com/dfrommi/groovy-pandoc)	| a library for writing Pandoc filters in Groovy.	| 

Other tools:

- [vimhl](https://github.com/lyokha/vim-publish-helper), a vim plugin that makes **vim syntax highlighting** engine available in pandoc.
- [pandoc-jats](https://github.com/mfenner/pandoc-jats), a **Lua custom writer** for Pandoc generating **JATS XML**.

## Written Filters

The following is a list of some known 3rd party filters:

- Images related:
	- [pandoc-svg](https://gist.github.com/jeromerobert/3996eca3acd12e4c3d40), a pandoc filter to convert svg files to pdf by Jerome Robert.
	- [diagrams-pandoc](http://hackage.haskell.org/package/diagrams-pandoc) for inserting images expressed in the Haskell [diagrams](http://projects.haskell.org/diagrams/) DSL.
	- [mermaid-pandoc](https://github.com/raghur/mermaid-filter) for inserting images expressed in [mermaid](http://knsv.github.io/mermaid/) syntax
- Numbering related:
	- [Numerical reference to sections](https://gist.github.com/jkr/bcfacbfdcf4cc4bafcf6), using a specified sign (by default `#`) in internal links. Metadata can configure special sign and whether links should be preserved or converted to plain text.
	- [pandoc-fignos](https://github.com/tomduck/pandoc-fignos), for numbering figures and figure references.
	- [pandoc-eqnos](https://github.com/tomduck/pandoc-eqnos), for numbering equations and equation references.
	- [pandoc-tablenos](https://github.com/tomduck/pandoc-tablenos), for numbering tables and table references.
	- [pandoc-crossref](https://github.com/lierdakil/pandoc-crossref), for numbering and cross-referencing figures, equations and tables
	- [pandoc-numbering](https://github.com/chdemko/pandoc-numbering), for numbering and cross-referencing any kinds of things such as examples, theorems, exercises and so on
	- [pandoc-listof](https://github.com/chdemko/pandoc-listof), for creating lists of any kinds (can be used efficiently with [pandoc-numbering](https://github.com/chdemko/pandoc-numbering))
	- [pandoc-amsthm](https://github.com/ickc/pandoc-amsthm): a pandoc amsthm package to define the use of amsthm through YAML front matter, target at HTML and LaTeX outputs. For HTML, CSS counter is used and defined in a template (by the YAML variables). For LaTeX amsthm package is used and defined in a template (by the YAML variables). [latexdivs.py](https://github.com/ickc/pandocfilters/blob/master/examples/latexdivs.py) is required too.
- LaTeX related:
	- [Using markdown inside raw latex commands](https://gist.github.com/mpickering/f1718fcdc4c56273ed52)
	- [pandoc-latex-environment](https://github.com/chdemko/pandoc-latex-environment), for adding LaTeX environement on specific HTML `div` tags
	- [latexdivs.py](https://github.com/ickc/pandocfilters/blob/master/examples/latexdivs.py): define a syntax to turn any native pandoc Divs into a LaTeX environment: if `latex="true"` is in the attribute of the Div, the first class is used to define the LaTeX environment.
- RAW related:
	- [Pandoc filter to insert arbitrary raw output markup as Code/CodeBlocks with an attribute raw=<outputformat>.](https://gist.github.com/bpj/e6e53cbe679d3ec77e25): Pandoc filter to insert arbitrary raw output markup as Code/CodeBlocks with an attribute `raw=<outputformat>`.
	- [Include Files](http://pandoc.org/scripting.html#include-files): finds all the inline code blocks with attribute include, and replaces their contents with the contents of the file given
- Tables related:
	- [pandoc-csv2table](https://github.com/baig/pandoc-csv2table) for including referenced csv files in markdown as markdown rendered tables.
	- [pandoc-placetable](https://github.com/mb21/pandoc-placetable) lightweight implementation of the idea behind the above `pandoc-csv2table` (e.g. doesn't necessarily require pandoc as a cabal dependency)
	- Creating a [link table](http://stackoverflow.com/questions/26406816/pandoc-is-there-a-way-to-include-an-appendix-of-links-in-a-pdf-from-markdown/26415375#26415375) at the end of your document.
- Others:
	- [Adding support](https://gist.github.com/mpickering/8bc9bb34c4e9b076b107) for indexing with the syntax ``(# term, subterm)`` in html and latex
	- [Adding non-breaking spaces inside a URL to preserve formatting](https://gist.github.com/mpickering/fdc747b9c8306659cb43)