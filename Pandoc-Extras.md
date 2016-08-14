<!-- TOC depthFrom:1 depthTo:6 withLinks:1 updateOnSave:1 orderedList:0 -->

- [Editors](#editors)
- [Web Services to Process Files by Pandoc](#web-services-to-process-files-by-pandoc)
- [Workflow](#workflow)
	- [Preprocessors](#preprocessors)
	- [Doc processing tools using Pandoc](#doc-processing-tools-using-pandoc)
		- [Using pandoc with ConTeXt](#using-pandoc-with-context)
- [Tools for Websites](#tools-for-websites)
	- [Blogs](#blogs)
	- [Wikis](#wikis)
	- [Static website generators](#static-website-generators)
	- [Serving markdown files with apache](#serving-markdown-files-with-apache)
	- [Pandoc wrappers and interfaces](#pandoc-wrappers-and-interfaces)
- [Citation](#citation)
	- [Integration with Reference Managers](#integration-with-reference-managers)
	- [Citation Style Language (CSL) with pandoc](#citation-style-language-csl-with-pandoc)
- [Other Wiki Pages](#other-wiki-pages)
	- [Filters](#filters)
	- [Templates: Illustrative Pandoc Templates](#templates-illustrative-pandoc-templates)
- [Examples of uses of pandoc](#examples-of-uses-of-pandoc)

<!-- /TOC -->
# Editors

- [Atom Packages](https://atom.io/packages/search?q=pandoc)
- [Scripts for using pandoc with BBEdit and TextWrangler], as well as [Notepad++]
- [pandoc-mode for emacs], by Joost Kremers
- [Mac OS X Services](https://github.com/mb21/Pandoc-Mac-OS-X-Services) to invoke pandoc from any text editor
- [Tool for using pandoc from Notepad++], courtesy of Ted Lilley
- [R Markdown](http://rmarkdown.rstudio.com), using knitr and pandoc
- [Sublime Text](https://sublime.wbond.net/search/pandoc), a number of plugins available for [Sublime Text](http://www.sublimetext.com/) via the plugin [Package Control](https://sublime.wbond.net/installation). See [Plaintxt Productivity](http://plaintext-productivity.net/2-05-how-to-set-up-sublime-text-for-markdown-export-to-word.html) and [Writing academic papers in Markdown using Pandoc](http://blog.cigrainger.com/2014/07/pandoc-markdown.html) for further details.
- [TextMate bundle for pandoc], courtesy of David Sanson (for TextMate 2 only)
- [Texts], Markdown WYSIWYM editor, is integrated with Pandoc.
- [vim-pandoc project], integration with pandoc and utilities for vim, courtesy of Felipe Morales (fmoralesc) and others. Covers:
    - [vim-pandoc], pandoc execution, folding, navigation, edition, etc.
    - [vim-pandoc-syntax], syntax file.
    - [vim-pandoc-after], integration with third-party plugins (snippets, nrrwrgn).
- [vim syntax file for pandoc], courtesy of tao_zhyn
- [vim HTML viewer use pandoc], by lambdalisue
- [vim HTML viewer use pandoc, konqueror or firefox], by tex
- [vim Unite BibTeX source for pandoc], by Mark Sprevak
- [The Zim desktop wiki] can export content to markdown using pandoc extensions (need zim version 0.55 and up)
the opened file as input.
- [pandoc-live](https://github.com/ocharles/pandoc-live) is a little utility can be used to watch a Pandoc document for changes, and display the rendering in real-time in a web browser.
- [docker-pandoc](https://github.com/silvio/docker-pandoc) - use pandoc on hosts without haskel installed (via docker). Brings a inotify based watch tool for nearly live generation of output documents.
- [Writage], Markdown plugin for Microsoft Word, is integrated with Pandoc.

# Web Services to Process Files by Pandoc

- [Docverter](http://www.docverter.com)
- [Pandoc online](https://foliovision.com/seo-tools/pandoc-online)
- [Pandoc as a service](http://pandoc-as-a-service.com/)

# Workflow

- [panzer](https://github.com/msprev/panzer), adds the concept of 'styles' to pandoc. Styles control templates, metadata settings, pandoc command line options, and instructions to run filters and pre/postprocessors in a simple, reusable, and recombinable way.
- [kokoi](https://github.com/zeis/kokoi), a configurable markup file watcher, previewer, and converter that uses Pandoc as the default markup processing engine. _kokoi_ watches for changes on the markup files in the directory _kokoi_ is started, and if they change, automatically reprocesses and previews them directly in the browser.
- [Pandoc Build Task](https://www.nuget.org/packages/PandocTasks/), a small MSBuild target to transform files with Pandoc. Provided as a Nuget package.
- [pandoc-schemata](https://github.com/rakali/pandoc-schemata), providing JSON Schema files for Pandoc JSON.
- [pandoc-seed-project](https://github.com/Dashed/pandoc-seed-project), a git repo seed where by you can clone from. It uses [gulp.js](https://github.com/gulpjs/gulp) and some of its plugins to automate task of using pandoc to watch and compile documents from a source directory to a destination directory. Useful for large and complex directory of documents to process through pandoc.
- [grunt-pandoc](https://github.com/Dashed/grunt-pandoc), a makefile-like workflow, that uses gruntjs and nodejs, to watch and automatically compile documents using `pandoc`. Useful for large and complex directory of documents to process through pandoc. **Note:* Author now recommends [pandoc-seed-project](https://github.com/Dashed/pandoc-seed-project).
- [pandocket](https://github.com/wcaleb/pandocket), 'A python script that looks for special lines in a markdown file and uses those lines to convert, clean up, and insert content from URLs into the file for processing by pandoc'

## Preprocessors

- [ickc/pandoc-criticmarkup: using criticmarkup in the pandoc markdown source](https://github.com/ickc/pandoc-criticmarkup): CriticMarkup for pandoc as a preprocessor. It can accept or reject (and optionally overwrite the markdown source), and show the diff in HTML and LaTeX output by chaining it with pandoc.
- [Using GPP as a preprocessor](http://randomdeterminism.wordpress.com/2012/06/01/how-i-stopped-worring-and-started-using-markdown-like-tex/) to get TeX-like macro features in Markdown.
- [pandoc-gpp](https://github.com/dloureiro/pandoc-gpp) by David Loureiro is a wrapper around pandoc and gpp in order to provide some macros for extra markup not available in markdown and its extensions.
- [mddia](https://github.com/nichtich/ditaa-markdown/) lets you embed [ditaa](http://ditaa.sourceforge.net/) ASCII-art diagrams in Markdown code blocks
- [PP - A generic Preprocessor (with Pandoc in mind)](http://cdsoft.fr/pp/) lets you embed the following diagrams in Markdown code blocks:
   - [Graphviz](http://www.graphviz.org/) (needs to be installed separately)
   - [PlantUML](http://plantuml.com/) (embedded in pp at compile time)
   - [Ditaa](http://ditaa.sourceforge.net/) (embedded in pp at compile time)

## Doc processing tools using Pandoc

- [Rippledoc](http://www.unexpected-vortices.com/sw/rippledoc/index.html) processes .md files into html. It ripples down from the current directory through nested subdirectories processing md files as it goes. It also generates tables of contents and navigation links, stitching together the documents into an easily-navigable whole.

- [SPAB](http://www.howtoselfpublishabook.org/self-publish-a-book/) a very simple windows GUI that uses Pandoc and a couple other open source tools to produce a .mobi, .epub, .doc, and .pdf ready for the most popular self-publishing services.

- [Pandoc Droplets and Services](https://github.com/dsanson/Pandoc-Droplets-and-Services) is a collection of simple and easily customizable Automator drag-and-drop applets and service menu items that use pandoc to produce pdf, docx, odt, and dzslides output on OS X.

- [Markdown menus](http://www.terminally-incoherent.com/blog/wp-content/uploads/2012/05/markdown-menus.zip), by Luke Maciak, provides contextual menus on Windows for creating PDFs and Word documents from markdown files. Double-click on the reg file to install.

- [PandocMarkdownTools](https://bitbucket.org/zuline/pandocmarkdowntools) is a small collection of scripts that creates a simple menu structure for conversion of Markdown docs using Pandoc. There are also scripts to simplify creation of Pandoc tables in Markdown and to automate replacement of document markers with the table text.

- [tpl4md](https://github.com/dloureiro/tpl4md) by David Loureiro provides markdown templates for widely used documents such as simple pdf documents, complex pdf documents, letters, invoices, orders, or even slides. The goal is to be able to focus on the content that will be written in markdown. The remaining elements are handle by pandoc, latex etc.

- [pandoc-watch](https://github.com/dloureiro/pandoc-watch) by David Loureiro is a simple watcher that call the pandoc command with the provided arguments when a file/folder modification is detected.

- [Uberdoc](https://github.com/sbrosinski/uberdoc) by Stephan Brosinski is a wrapper script for Pandoc which provides a build system to turn a number of markdown files (chapters) into large documents. 

- [Xmindoc](https://github.com/sky-y/xmindoc) is a wrapper script for Pandoc which converts XMind mindmaps to any documents that are available in Pandoc.

- [pdc](https://github.com/bk/pdc) is a command line wrapper for pandoc which makes it possible to completely control the conversion process from the first YAML meta block in the input document(s), including multiple output formats at the same time, templates, pre- and post-processors, etc.

### Using pandoc with ConTeXt

- [filter module] by adityam, which allows you to use
  `\startmarkdown` and `\stopmarkdown` directly in ConTeXt.
- [an even simpler route] from markdown to PDF, via ConTeXt.


# Tools for Websites

## Blogs

- [WordPress EasyFilter], by Yang Zhang, makes it easy to use pandoc
  with WordPress blogs.

## Wikis

- [gitit], a pandoc-based wiki that stores pages in a git (or
  darcs or mercurial) repository.
- [ikiwiki-pandoc], a feature-rich pandoc plugin for [ikiwiki]. Supports most features of pandoc relevant for a wiki, including almost all textual input formats, several math handling options, as well as citation/bibliography processing via pandoc-citeproc. It also supports several export formats, including pdf, docx, odt, beamer and revealjs. (Ikiwiki-pandoc was originally a fork of the currently unmaintained [pandoc-iki]).

## Static website generators

- [yst](https://github.com/jgm/yst)
- [Hakyll](http://jaspervdj.be/hakyll/)
- A [bash shell script](https://github.com/wcaleb/website) by Caleb McDaniel to generate a site using pandoc
- Jekyll
    - [jekyll-pandoc-multiple-formats](https://github.com/fauno/jekyll-pandoc-multiple-formats)
    - [jekyll-pandoc](https://github.com/mfenner/jekyll-pandoc), used for the [Opening Science](https://github.com/openingscience/book) book
    - (Also see [this post](http://drz.ac/2013/01/03/blogging-with-math/) for implementation on OctoPress)
- [Pandocomatic](https://github.com/htdebeer/pandocomatic)
- [pdsite](http://pdsite.org)

## Serving markdown files with apache

- [apache-pandoc](https://github.com/chdemko/apache-pandoc)

## Pandoc wrappers and interfaces

- [Pandoctor](https://github.com/smargh/alfred_pandoctor), an [Alfred](http://www.alfredapp.com/) workflow that acts as a GUI for pandoc. Allows for one-off conversions as well as templates. Created by Stephen Margheim
- [PanConvert](http://panconvert.sourceforge.net), a cross-platform pandoc gui-wrapper. Allows using pandoc in a gui-environment
- [pypandoc](https://github.com/bebraw/pypandoc), a python wrapper
- [Pyandoc](https://github.com/kennethreitz/pyandoc), a python wrapper
  for pandoc by Kenneth Reitz.
- [pandoc-ruby](https://github.com/alphabetum/pandoc-ruby), a ruby wrapper
  for pandoc by alphabetum.
- [Pandoku](https://github.com/lunant/pandoku), another Ruby wrapper
  for Pandoc by Hong Minhee.
- [Paru](https://heerdebeer.org/Software/markdown/paru/), yet another Ruby wrapper for pandoc by Huub de Beer. Since version 0.1 it supports filters as well.
- [libpandoc, a C interface to the pandoc library](http://github.com/ShabbyX/libpandoc/tree/master),
  by Anton Tayanovskyy, updated for pandoc 1.11.1 by Shahbaz Youssefi.
- [pander](https://github.com/Rapporter/pander), an [R](http://www.r-project.org/) wrapper
  for pandoc by Gergely Daróczi.
- [scala-pandoc](https://github.com/pvorb/scala-pandoc), a Scala/Java wrapper for Pandoc by Paul Vorbach
- [node-pdc](https://github.com/pvorb/node-pdc), a Node.js wrapper for Pandoc by Paul Vorbach
- [groovy-pandoc](https://github.com/dfrommi/groovy-pandoc), a Groovy wrapper for Pandoc by Dennis Frommknecht
- [Universal Document Converter](https://bitbucket.org/leito/universal-document-converter), a Java wrapper for Pandoc by Leonardo S. De Seta

# Citation

## Integration with Reference Managers

- [BibDesk Export Templates](https://github.com/dsanson/bibdesk-pandoc-export-templates): drag and drop pandoc-style citations from BibDesk into your document; use pandoc to export formatted reference lists from BibDesk.
- [zotxt](https://bitbucket.org/egh/zotxt), allowing direct usage of a Zotero library (with [zotxt-emacs](https://bitbucket.org/egh/zotxt-emacs) for completion in emacs)
- [Zotero Integration Extension](http://baig.github.io/brackets-zotero/): If you miss a Word like workflow working with Zotero where you can just search your Zotero library and insert citations wherever you want, then this extension provides such functionality in [Brackets](http://brackets.io/) for your markdown documents.
- [Applets for Zotero and Scrivener integration](http://github.com/davepwsmith/zotpick-applescript/) using [Better BibTeX](https://zotplus.github.io/better-bibtex/) plugin's new [Cite as you Write](https://zotplus.github.io/better-bibtex/cayw.html) feature. Use in conjunction with this [workflow for integrating Scrivner, Pandoc, Zotero and Marked 2](http://davepwsmith.github.io/academic-scrivener-howto/)

## Citation Style Language (CSL) with pandoc

- [Testing and Using CSL Style Files with Pandoc](https://github.com/KurtPfeifle/pandoc-csl-testing) -- a GitHub repository with a shell script, a Markdown file and usage instructions enabling you to create visual representations of the effects that any one of the [more than 1200 available Citation Style Language (CSL) files](http://citationstyles.org/) may have on your output documents.

# Other Wiki Pages

## Filters

See [[Pandoc Filters]]

## Templates: Illustrative Pandoc Templates

- This wiki is developing a collection of [[User Contributed Templates]] for purposes of illustration; contribute your own, or if you keep pandoc templates under revision control, link them here.

# Examples of uses of pandoc

- [Using pandoc for IETF RFC creation], courtesy of Miek Gieben
- [Preserving old books by rewriting them in markdown], one example of several etexts at [Project Gutenberg] originally transcribed in markdown


[an even simpler route]: http://randomdeterminism.wordpress.com/2011/01/09/markdowntopdf/
[filter module]: https://github.com/adityam/filter
[vim syntax file for pandoc]: http://www.vim.org/scripts/script.php?script_id=2389
[vim-pandoc project]: https://github.com/vim-pandoc
[vim-pandoc]: https://github.com/vim-pandoc/vim-pandoc
[vim-pandoc-syntax]: https://github.com/vim-pandoc/vim-pandoc-syntax
[vim-pandoc-after]: https://github.com/vim-pandoc/vim-pandoc-after
[vim HTML viewer use pandoc]: https://github.com/lambdalisue/shareboard.vim
[vim HTML viewer use pandoc, konqueror or firefox]: https://github.com/tex/vimpreviewpandoc
[vim Unite BibTeX source for pandoc]: https://github.com/msprev/unite-bibtex
[TextMate bundle for pandoc]: http://github.com/dsanson/Pandoc.tmbundle
[jsMath]: http://www.math.union.edu/~dpvc/jsMath/
[WordPress EasyFilter]: http://assorted.sourceforge.net/wp-easy-filter/
[Haskell platform]: http://hackage.haskell.org/platform/
[Tool for using pandoc from Notepad++]: https://bitbucket.org/binaryphile/clip-pandoc
[pandoc-mode for emacs]: http://joostkremers.github.com/pandoc-mode/
[gitit]: http://gitit.net
[pandoc-iki]: https://github.com/dubiousjim/pandoc-iki
[ikiwiki]:  http://ikiwiki.info/
[ikiwiki-pandoc]: https://github.com/sciunto-org/ikiwiki-pandoc
[Bash and zsh command-line completion for pandoc]: https://github.com/dsanson/pandoc-completion
[Using pandoc for IETF RFC creation]: https://github.com/miekg/pandoc2rfc
[Preserving old books by rewriting them in markdown]: https://github.com/rwst/book-curie-radio-de
[Project Gutenberg]: http://www.gutenberg.org
[The Zim desktop wiki]: http://zim-wiki.org
[Texts]: http:/www.texts.io/
[Scripts for using pandoc with BBEdit and TextWrangler]: https://github.com/jrgcmu/BBpandoc
[Notepad++]: https://gist.github.com/jrgcmu/3ccc11c0412f45f9a6a4
[Writage]: http://www.writage.com/