<!-- TOC depthFrom:1 depthTo:6 withLinks:1 updateOnSave:1 orderedList:0 -->

- [Editors](#editors)
- [Web Services to Process Files by Pandoc](#web-services-to-process-files-by-pandoc)
- [Workflow](#workflow)
	- [Preprocessors](#preprocessors)
 		- [Legacy](#legacy)
 		- [Additional readers](#additional-readers)
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
- [Emacs package for exporting using pandoc](https://github.com/robtillotson/org-pandoc)
- [Scripts for using pandoc with BBEdit and TextWrangler], as well as [Notepad++]
- [pandoc-mode for emacs], by Joost Kremers
- [PanWriter](https://panwriter.com) ([GitHub page](https://github.com/mb21/panwriter/)) – distraction-free markdown editor with pandoc integration and paginated preview
- [Mac OS X Services](https://github.com/mb21/Pandoc-Mac-OS-X-Services) to invoke pandoc from any text editor
- [Tool for using pandoc from Notepad++], courtesy of Ted Lilley
- [R Markdown](http://rmarkdown.rstudio.com), using knitr and pandoc
- [Sublime Text](https://sublime.wbond.net/search/pandoc), a number of plugins available for [Sublime Text](http://www.sublimetext.com/) via the plugin [Package Control](https://sublime.wbond.net/installation). See [Plaintxt Productivity](http://plaintext-productivity.net/2-05-how-to-set-up-sublime-text-for-markdown-export-to-word.html) and [Writing academic papers in Markdown using Pandoc](http://web.archive.org/web/20141014045547/http://blog.cigrainger.com/2014/07/pandoc-markdown.html) for further details.
- [TextMate bundle for pandoc], courtesy of David Sanson (for TextMate 2 only)
- [vim-pandoc project], integration with pandoc and utilities for vim, courtesy of Felipe Morales (fmoralesc) and others. Covers:
    - [vim-pandoc], pandoc execution, folding, navigation, edition, etc.
    - [vim-pandoc-syntax], syntax file.
    - [vim-pandoc-after], integration with third-party plugins (snippets, nrrwrgn).
- [vim syntax file for pandoc], courtesy of tao_zhyn
- [vim HTML viewer use pandoc], by lambdalisue
- [vim HTML viewer use pandoc, konqueror or firefox], by tex
- [vim Unite BibTeX source for pandoc], by Mark Sprevak
- [fzf BibTeX], successor of vim Unite BiBTeX (vim Unite is no longer being maintained)
- [The Zim desktop wiki] can export content to markdown using pandoc extensions (need zim version 0.55 and up)
the opened file as input.
- [pandoc-live](https://github.com/ocharles/pandoc-live) is a little utility can be used to watch a Pandoc document for changes, and display the rendering in real-time in a web browser.
- [docker-pandoc](https://github.com/silvio/docker-pandoc) - use pandoc on hosts without haskel installed (via docker). Brings a inotify based watch tool for nearly live generation of output documents.
- [Writage], Markdown plugin for Microsoft Word, is integrated with Pandoc.
- [Markdown Preview Enhanced], a super powerful markdown extension for Atom and Visual Studio Code. It can use pandoc as the Markdown parser.

# Web Services to Process Files by Pandoc

- [Docverter](http://www.docverter.com)
- [Pandoc online](https://foliovision.com/seo-tools/pandoc-online)
- [Pandoc as a service](https://github.com/mrded/pandoc-as-a-service "GitHub source files for running service locally, by Dmitry Demenchuk. Original online service no longer available"): Serving on localhost. (See also: [WaybackMachine](https://web.archive.org/web/20150919182811/http://pandoc-as-a-service.com/ "Archived copy of the original online service by Dmitry Demenchuk"))
- [Markup.rocks](http://markup.rocks) runs a version of pandoc compiled to javascript via ghcjs. Conversion takes place client-side, in the browser, and a live preview of the result is available.

# Workflow

- [panzer](https://github.com/msprev/panzer), adds the concept of 'styles' to pandoc. Styles control templates, metadata settings, pandoc command line options, and instructions to run filters and pre/postprocessors in a simple, reusable, and recombinable way.
- [panrun](https://github.com/mb21/panrun): minimal script that runs pandoc with the options it finds in the YAML metadata of the input file.
- [kokoi](https://github.com/zeis/kokoi), a configurable markup file watcher, previewer, and converter that uses Pandoc as the default markup processing engine. _kokoi_ watches for changes on the markup files in the directory _kokoi_ is started, and if they change, automatically reprocesses and previews them directly in the browser.
- [pdc](https://github.com/bk/pdc) is a command line wrapper for pandoc which makes it possible to completely control the conversion process from the first YAML meta block in the input document(s), including multiple output formats at the same time, templates, pre- and post-processors, etc.
- [Pandocomatic](https://heerdebeer.org/Software/markdown/pandocomatic/), allows you to configure flexible YAML templates that control Pandoc settings, filters, metadata and pre/postprocessing. Applied to a directory, pandocomatic can act as a static site generator.
- [Pandoc Build Task](https://www.nuget.org/packages/PandocTasks/), a small MSBuild target to transform files with Pandoc. Provided as a Nuget package.
- [pandoc-schemata](https://github.com/rakali/pandoc-schemata), providing JSON Schema files for Pandoc JSON.
- [pandocket](https://github.com/wcaleb/pandocket), 'A python script that looks for special lines in a markdown file and uses those lines to convert, clean up, and insert content from URLs into the file for processing by pandoc'
- [fish-completion-pandoc](https://github.com/dsanson/fish-completion-pandoc), a pandoc completion script for use with the fish shell.
- [Pandoctools](https://github.com/kiwi0fruit/pandoctools) is a set of tools for writing reproducible Markdown reports. They rely on Pandoc and Jupyter kernels. At it's core is a profile manager of text processing pipelines (crossplatform bash scripts) that define chain operations over text (Pandoc filters, any text CLI filters).
- [bookbuildeR](https://github.com/thomasWeise/bookbuildeR) is a tool chain for compiling e-books from pandoc flavored Markdown to pdf, epub, html, and azw3, which also comes as a docker container and can also be used via online continuous integration tools such as Travis CI to automatically compile books whose sources are hosted on GitHub upon commits and automatically publish the results to GitHub pages. It also supports separating book sources in one repository and example program codes into another one, which is suitable for computer science books.
- [GitHub Actions](https://github.com/features/actions) is a workflow automation tool (CI/CD) from GitHub.
  You can run pandoc on GitHub Actions on every push (or other GitHub event) to, for example, convert a source document to another markup and publish the results.
  There are two approaches to make pandoc available on GitHub Actions:
  1. You can [reference container actions](https://help.github.com/en/actions/automating-your-workflow-with-github-actions/configuring-a-workflow#referencing-a-container-on-docker-hub) based on the [official docker pandoc images](https://github.com/pandoc/dockerfiles) in your GitHub Actions workflows.
    To learn how to set this up, see [these examples](https://github.com/maxheld83/pandoc-action-example).
  2. If, instead, you want to install pandoc on the host machine *directly*, you can use thus [setup-pandoc](https://github.com/r-lib/actions/tree/master/setup-pandoc).
    This may take longer and does not offer the encapsulation of the docker images, but makes pandoc available in all steps, or even in different versions for matrix builds.

## Preprocessors

- [PP - A generic Preprocessor (with Pandoc in mind)](http://cdsoft.fr/pp/) lets you embed the following diagrams in Markdown code blocks:
    + [Graphviz](http://www.graphviz.org/) (needs to be installed separately)
    + [PlantUML](http://plantuml.com/) (embedded in pp at compile time)
    + [Ditaa](http://ditaa.sourceforge.net/) (embedded in pp at compile time)
    + [Asymptote](http://asymptote.sourceforge.net/) and [R](https://www.r-project.org/) figures
    
    and offers many powerful features to expand pandoc power:
    
    + built-in macros
    + custom macros definition
    + literate programming
    + usage of [Bash](https://www.gnu.org/software/bash/), [Cmd](https://en.wikipedia.org/wiki/Cmd.exe), [PowerShell](https://en.wikipedia.org/wiki/PowerShell), [Python](https://www.python.org/), [Haskell](https://www.haskell.org/) and [R](https://www.r-project.org/) scripts (inline or external)

- [pancritic - using CriticMarkup with pandoc](https://github.com/ickc/pancritic)

    pancritic parses [CriticMarkup](http://criticmarkup.com/) in pandoc as a wrapper or preprocessor.
    CriticMarkup is an in-place diff format, while `pancritic` applies/resolves the indicated diffs.
    It supports HTML and LaTeX outputs.

### Legacy

- (included in PP) [Using GPP as a preprocessor](https://adityam.github.io/context-blog/post/markdown-with-gpp/) to get TeX-like macro features in Markdown.
- (abandonned since 2013) [pandoc-gpp](https://github.com/dloureiro/pandoc-gpp) by David Loureiro is a wrapper around pandoc and gpp in order to provide some macros for extra markup not available in markdown and its extensions.
- (included in PP) [mddia](https://github.com/nichtich/ditaa-markdown/) lets you embed [ditaa](http://ditaa.sourceforge.net/) ASCII-art diagrams in Markdown code blocks

### Additional Readers

- Perl module [Pod::Simple::Pandoc](https://metacpan.org/pod/Pod::Simple::Pandoc) parses [Plain Old Documentation (POD)](https://en.wikipedia.org/wiki/Plain_Old_Documentation) markup format to Pandoc data model.

## Doc processing tools using Pandoc

- [Rippledoc](http://www.unexpected-vortices.com/sw/rippledoc/index.html) processes .md files into html. It ripples down from the current directory through nested subdirectories processing md files as it goes. It also generates tables of contents and navigation links, stitching together the documents into an easily-navigable whole.

- [SPAB](http://www.storyhack.com/self-publish-a-book/ "Storyhack.com reprint of the Bryce Beattie's original article: SPAB installer not available for download") a very simple windows GUI that uses Pandoc and a couple other open source tools to produce a .mobi, .epub, .doc, and .pdf ready for the most popular self-publishing services. (See also:
 [WaybackMachine](https://web.archive.org/web/20130218031035/http://www.howtoselfpublishabook.org/self-publish-a-book "Archived copy of the original article from 2011"); [_How to Self Publish a Book_](http://www.storyhack.com/how-to-self-publish-a-book/ "Article by Bryce Beattie, 2016").)

- [Pandoc Droplets and Services](https://github.com/dsanson/Pandoc-Droplets-and-Services) is a collection of simple and easily customizable Automator drag-and-drop applets and service menu items that use pandoc to produce pdf, docx, odt, and dzslides output on OS X.

- [Markdown menus](http://www.terminally-incoherent.com/blog/wp-content/uploads/2012/05/markdown-menus.zip), by Luke Maciak, provides contextual menus on Windows for creating PDFs and Word documents from markdown files. Double-click on the reg file to install.

- [PandocMarkdownTools](https://bitbucket.org/zuline/pandocmarkdowntools) is a small collection of scripts that creates a simple menu structure for conversion of Markdown docs using Pandoc. There are also scripts to simplify creation of Pandoc tables in Markdown and to automate replacement of document markers with the table text.

- [tpl4md](https://github.com/dloureiro/tpl4md) by David Loureiro provides markdown templates for widely used documents such as simple pdf documents, complex pdf documents, letters, invoices, orders, or even slides. The goal is to be able to focus on the content that will be written in markdown. The remaining elements are handle by pandoc, latex etc.

- [pandoc-watch](https://github.com/dloureiro/pandoc-watch) by David Loureiro is a simple watcher that call the pandoc command with the provided arguments when a file/folder modification is detected.

- [Uberdoc](https://github.com/sbrosinski/uberdoc) by Stephan Brosinski is a wrapper script for Pandoc which provides a build system to turn a number of markdown files (chapters) into large documents. 

- [Xmindoc](https://github.com/sky-y/xmindoc) is a wrapper script for Pandoc which converts XMind mindmaps to any documents that are available in Pandoc.

- [Octavo](https://github.com/OolonColoophid/octavo) is a wrapper script for Pandoc that helps to render multiple versions of the same markdown file, FTP them to a server, and insert text into the document that refers to these other versions in plain English. It supports special markdown to produce highlight boxes and other features, comes with its own templates (in Tufte style, normal PDF, Word, html, etc.), and can produce spoken audio versions.

- [PanDiff](https://github.com/davidar/pandiff) produces prose diffs for any document format supported by Pandoc, and can output Markdown with CriticMarkup, HTML, PDF, and Word docx with Track Changes

- [Bookmanager](https://pypi.org/project/cyberaide-bookmanager/) creates a publication from a number of distributed sources in github or your filesystem based on markdown files that are conveniently integrated in a table of content using yaml. It is especially useful to create customized books, lecture notes, or handouts and allows for easy customization of content for lecture notes.

- [Commentary](https://github.com/hdb/commentary) is a simple Python wrapper and command line tool that preserves native-style comments between markdown and docx Pandoc conversions. 

### Using pandoc with ConTeXt

- [filter module] by adityam, which allows you to use
  `\startmarkdown` and `\stopmarkdown` directly in ConTeXt.
- [an even simpler route] from markdown to PDF, via ConTeXt.

## GUIs for using Pandoc
- [DocDown](https://github.com/lowercasename/docdown): menubar bar app for Pandoc conversions. limited options, but very accessible for less tech-savvy people.
- [Pandoc Plugin for Obsidian](https://github.com/OliverBalfour/obsidian-pandoc): Wide range of conversions from markdown documents in Obsidian.
- [Pandoc-Suite for Alfred](https://chris-grieser.de/pandoc_alfred): enables one-click-conversion of Markdown Documents with Citations into `.docx`, `.pdf`, `.html`, `.odt`, or `.pptx` with the proper bibliography. Also features a Citation Picker for Pandoc Citations and a Citation Style file search. 


# Tools for Websites

## Blogs

- [WordPress EasyFilter], by Yang Zhang, makes it easy to use pandoc
  with WordPress blogs.

## Wikis

- [gitit], a pandoc-based wiki that stores pages in a git (or
  darcs or mercurial) repository.
- [ikiwiki-pandoc], a feature-rich pandoc plugin for [ikiwiki]. Supports most features of pandoc relevant for a wiki, including almost all textual input formats, several math handling options, as well as citation/bibliography processing via pandoc-citeproc. It also supports several export formats, including pdf, docx, odt, beamer and revealjs. (Ikiwiki-pandoc was originally a fork of the currently unmaintained [pandoc-iki]).

## Static website generators

- [yst](https://github.com/jgm/yst): create static websites from YAML data and string templates, written in Haskell.
- [Hakyll](http://jaspervdj.be/hakyll/): a static website compiler library in Haskell.
- [Website](https://github.com/wcaleb/website): a bash shell script by Caleb McDaniel to generate a site using pandoc.
- Jekyll:
    - [jekyll-pandoc-multiple-formats](https://github.com/fauno/jekyll-pandoc-multiple-formats)
    - [jekyll-pandoc](https://github.com/mfenner/jekyll-pandoc), used for the [Opening Science](https://github.com/openingscience/book) book
    - (Also see [this post](http://drz.ac/2013/01/03/blogging-with-math/) for implementation on OctoPress)
- [Nikola](https://github.com/getnikola/nikola): a Python static site generator supporting many compilers including pandoc.
- [Pandocomatic](https://heerdebeer.org/Software/markdown/pandocomatic/) RubyGem. Can also be used to automate the use of pandoc in a more general way.
- [pdsite](http://pdsite.org): single shell script with no dependencies, runs on Unix-like systems.
- [pdblog](https://pdblog.org): even simpler shell script than pdsite, specifically for blog-like sites.
- [gh-themes-magick](https://github.com/tajmone/gh-themes-magick): publish your GitHub project’s single page website from `master` branch’s `/docs/` folder using GitHub themes, and let pandoc scripts update its contents to mirror the project’s `README.md`.
- [simple-template/pandoc](https://github.com/simple-template/pandoc): A system to generate static websites using Make.
- [_jqt_ template engine](https://fadado.github.io/jqt/): _jqt_ uses [jq](https://stedolan.github.io/jq/) as expression language and _Pandoc_ as _MarkDown_ processor. _jqt_ also implements a preprocessing stage using [gpp](https://logological.org/gpp) for templates, _MarkDown_ documents, JSON data and CSS files.

## Serving markdown files with apache

- [apache-pandoc](https://github.com/chdemko/apache-pandoc)

## Pandoc wrappers and interfaces

See [[Pandoc wrappers and interfaces]]

# Citation

## Integration with Reference Managers

- [BibDesk Export Templates](https://github.com/dsanson/bibdesk-pandoc-export-templates): drag and drop pandoc-style citations from BibDesk into your document; use pandoc to export formatted reference lists from BibDesk.
- [Bookends-Tools](https://github.com/iandol/bookends-tools): Uses [Alfred](https://www.alfredapp.com) to interface [Bookends Reference Manager](https://www.sonnysoftware.com) to 11 different citation tools, several of which focus on Pandoc formatting and workflow.
- [zotxt](https://gitlab.com/egh/zotxt), allowing direct usage of a Zotero library (with [zotxt-emacs](https://gitlab.com/egh/zotxt-emacs) for completion in emacs)
- [Zotero Integration Extension](http://baig.github.io/brackets-zotero/): If you miss a Word like workflow working with Zotero where you can just search your Zotero library and insert citations wherever you want, then this extension provides such functionality in [Brackets](http://brackets.io/) for your markdown documents.
- [Applets for Zotero and Scrivener integration](http://github.com/davepwsmith/zotpick-applescript/) using [Better BibTeX] plugin's new Cite as you Write feature. Use in conjunction with this [workflow for integrating Scrivner, Pandoc, Zotero and Marked 2](http://davepwsmith.github.io/academic-scrivener-howto/)
- [pandoc-wikicite](https://www.npmjs.com/package/pandoc-wikicite) allows to directly reference bibliographic items from Wikidata
- The [Pandoc-Suite for Alfred](https://chris-grieser.de/pandoc_alfred) also includes a citation picker that can insert Pandoc Citations systemwide (using either Alfred or Zotero as GUI.)

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
[fzf BibTeX]: https://github.com/msprev/fzf-bibtex
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
[Better BibTeX]: https://retorque.re/zotero-better-bibtex/
[Using pandoc for IETF RFC creation]: https://github.com/miekg/pandoc2rfc
[Preserving old books by rewriting them in markdown]: https://github.com/rwst/book-curie-radio-de
[Project Gutenberg]: http://www.gutenberg.org
[The Zim desktop wiki]: http://zim-wiki.org
[Scripts for using pandoc with BBEdit and TextWrangler]: https://github.com/jrgcmu/BBpandoc
[Notepad++]: https://gist.github.com/jrgcmu/3ccc11c0412f45f9a6a4
[Writage]: http://www.writage.com/
[Markdown Preview Enhanced]: https://shd101wyy.github.io/markdown-preview-enhanced/
