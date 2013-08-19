## Editors

- [pandoc-mode for emacs], by Joost Kremers
- [vim syntax file for pandoc], courtesy of tao_zhyn
- [vim bundle for pandoc], courtesy of Felipe Morales (fmoralesc), David Sanson (dsanson)
- [vim HTML viewer use pandoc], by lambdalisue
- [TextMate bundle for pandoc], courtesy of David Sanson
- [Tool for using pandoc from Notepad++], courtesy of Ted Lilley
- [The Zim desktop wiki] can export content to markdown using pandoc extensions (need zim version 0.55 and up)
- [Texts], Markdown WYSIWYM editor, is integrated with Pandoc.
- [Scripts for using pandoc with BBEdit and TextWrangler], courtesy of John Gardner.
- [Mac OS X Services](https://github.com/mb21/Pandoc-Mac-OS-X-Services) to invoke pandoc from any text editor with the opened file as input.

## Shell Completion

- [Bash and zsh command-line completion for pandoc], courtesy of David Sanson

## Blogs

- [WordPress EasyFilter], by Yang Zhang, makes it easy to use pandoc
  with WordPress blogs.

## Wikis

- [gitit], a pandoc-based wiki that stores pages in a git (or
  darcs or mercurial) repository.
- [pandoc-iki], by Jim Pryor, a plugin to use pandoc as a
  markdown handler with [ikiwiki].

## Static website generators

- [yst](https://github.com/jgm/yst)
- [Hakyll](http://jaspervdj.be/hakyll/)
- A [bash shell script](https://github.com/wcaleb/website) by Caleb McDaniel to generate a site using pandoc

## Serving `.md` and `.markdown` files with apache

- [apache-pandoc](https://github.com/chdemko/apache-pandoc)

## Using pandoc with ConTeXt

- [filter module] by adityam, which allows you to use
  `\startmarkdown` and `\stopmarkdown` directly in ConTeXt.
- [an even simpler route] from markdown to PDF, via ConTeXt.

## Programming

- [Pyandoc](https://github.com/kennethreitz/pyandoc), a python wrapper
  for pandoc by Kenneth Reitz.
- [pandoc-ruby](https://github.com/alphabetum/pandoc-ruby), a ruby wrapper
  for pandoc by alphabetum.
- [Pandoku](https://github.com/lunant/pandoku), another Ruby wrapper
  for Pandoc by Hong Minhee.
- [libpandoc, a C interface to the pandoc library](http://github.com/ShabbyX/libpandoc/tree/master),
  by Anton Tayanovskyy, updated for pandoc 1.11.1 by Shahbaz Youssefi.
- [pander](https://github.com/Rapporter/pander), an [R](http://www.r-project.org/) wrapper
  for pandoc by Gergely Daróczi.

## Preprocessors

- [Using GPP as a preprocessor](http://randomdeterminism.wordpress.com/2012/06/01/how-i-stopped-worring-and-started-using-markdown-like-tex/) to get TeX-like macro features in Markdown.
- [pandoc-gpp](http://dloureiro.github.io/pandoc-gpp) by David Loureiro is a wrapper around pandoc and gpp in order to provide some macros for extra markup not available in markdown and its extensions.

## Integration with Reference Managers

- [BibDesk Export Templates](https://github.com/dsanson/bibdesk-pandoc-export-templates): drag and drop pandoc-style citations from BibDesk into your document; use pandoc to export formatted reference lists from BibDesk.

## Illustrative Pandoc templates

- Kieran Healy keeps his `~/.pandoc/templates` directory on github: [kjhealy / pandoc-templates](/kjhealy/pandoc-templates).  Note that it calls a [special .sty file](https://github.com/kjhealy/latex-custom-kjh/tree/master/needs-memoir) from [kjhealy / latex-custom-kjh](/kjhealy/latex-custom-kjh) . Similarly see:
- [dsanson / pandoc-templates](/dsanson/pandoc-templates)
- [smile / pandoc-templates](/timheil/pandoc-templates) - a German language article template.
- [claes / pandoc-templates](/claes/pandoc-templates) is a good illustration of a template with many user-defined variables, e.g for margins, language, papersize, orientation, etc. Because this makes command line specification a bit unwieldy, he includes [simple shell script](/claes/pandoc-templates/blob/master/md2pdf) with unused options commented out to instruct markdown2pdf how to fill in all the blanks.
- [wcaleb / pandoc-templates](/wcaleb/pandoc-templates)
- This wiki is developing a collection of [[User Contributed Templates]] for purposes of illustration; contribute your own, or if you keep pandoc templates under revision control, link them here.
- [md2latex](https://bitbucket.org/zuline/md2latex) is a collection of templates and scripts which are designed to allow you to write in APA 6 style using Markdown and to convert to APA 6 Style in LaTeX.

## Doc processing tools using Pandoc

- [Gouda](http://www.unexpected-vortices.com/sw/gouda/) is a Perl script which uses Pandoc to stitch multiple docs together as chapters into an easily-navigable whole. It provides an easy way to get started writing multi-page/chapter documentation.

- Use [Rippledoc](https://github.com/uvtc/rippledoc) to "ripple through" nested subdirectories of pandoc-markdown formatted notes, generating html from them, as well as generating a table of contents for easy navigation.

- [SPAB](http://www.howtoselfpublishabook.org/self-publish-a-book/) a very simple windows GUI that uses Pandoc and a couple other open source tools to produce a .mobi, .epub, .doc, and .pdf ready for the most popular self-publishing services.

- [Pandoc Droplets and Services](https://github.com/dsanson/Pandoc-Droplets-and-Services) is a collection of simple and easily customizable Automator drag-and-drop applets and service menu items that use pandoc to produce pdf, docx, odt, and dzslides output on OS X.

- [Markdown menus](http://www.terminally-incoherent.com/blog/wp-content/uploads/2012/05/markdown-menus.zip), by Luke Maciak, provides contextual menus on Windows for creating PDFs and Word documents from markdown files. Double-click on the reg file to install.

- [PandocMarkdownTools](https://bitbucket.org/zuline/pandocmarkdowntools) is a small collection of scripts that creates a simple menu structure for conversion of Markdown docs using Pandoc. There are also scripts to simplify creation of Pandoc tables in Markdown and to automate replacement of document markers with the table text.

- [tpl4md](https://github.com/dloureiro/tpl4md) by David Loureiro provides markdown templates for widely used documents such as simple pdf documents, complex pdf documents, letters, invoices, orders, or even slides. The goal is to be able to focus on the content that will be written in markdown. The remaining elements are handle by pandoc, latex etc.

- [pandoc-watch](https://github.com/dloureiro/pandoc-watch) by David Loureiro is a simple watcher that call the pandoc command with the provided arguments when a file/folder modification is detected.

## German Introduction / Deutsche Einführung

Eine kurze Einführung (16 Seiten) zu Markdown und Pandoc.

Der Text beschreibt die Auszeichnungssprache Markdown und die Benutzung des Programms Pandoc unter Berücksichtigung der Eigenheiten der deutschen Sprache.
Er geht auf die Erzeugung von eBüchern (ePub und Kindle) sowie den Export nach OpenOffice und MS Office ein.
Außerdem bietet er Vorlagen (templates) zur Verwendung mit LaTeX.

Das komplette Projekt gibt es auf [https://github.com/AKielhorn/Markdown-Intro].

Das PDF Dokument kann man unter [http://dl.dropbox.com/u/6568507/Ziele-md.pdf] herunterladen, das dazugehörige Begleitmaterial sowie die Vorlagen gibt es als Zip archiv unter [http://dl.dropbox.com/u/6568507/Ziele.zip].

## Chinese Introduction / 中文翻譯

[Pandoc’s Markdown 語法中文翻譯](http://pages.tzengyuxio.me/pandoc/)

這份中文語法文件翻譯自 [Pandoc - Pandoc User’s Guide][userguide] 中的 "Pandoc's markdown" 一節。

你可以看看[這份文件的原始檔][source]、產生文件[所使用的 HTML 範本][template]，以及[轉換時的命令參數][script]。

[userguide]: http://johnmacfarlane.net/pandoc/README.html#pandocs-markdown
[source]: https://raw.github.com/tzengyuxio/pages/gh-pages/pandoc/pandoc.markdown
[template]: https://github.com/tzengyuxio/pages/blob/gh-pages/pandoc/pm-template.html5
[script]: https://github.com/tzengyuxio/pages/blob/gh-pages/pandoc/convert.sh

## Official documents and books

- [Using pandoc for IETF RFC creation], courtesy of Miek Gieben
- [Preserving old books by rewriting them in markdown], one example of several etexts at [Project Gutenberg] originally transcribed in markdown


[an even simpler route]: http://randomdeterminism.wordpress.com/2011/01/09/markdowntopdf/
[filter module]: https://github.com/adityam/filter
[vim syntax file for pandoc]: http://www.vim.org/scripts/script.php?script_id=2389
[vim bundle for pandoc]: https://github.com/vim-pandoc/vim-pandoc
[vim HTML viewer use pandoc]: https://github.com/lambdalisue/shareboard.vim
[TextMate bundle for pandoc]: http://github.com/dsanson/Pandoc.tmbundle
[jsMath]: http://www.math.union.edu/~dpvc/jsMath/
[WordPress EasyFilter]: http://assorted.sourceforge.net/wp-easy-filter/
[Haskell platform]: http://hackage.haskell.org/platform/
[Tool for using pandoc from Notepad++]: https://bitbucket.org/lilleyt/clip-pandoc
[pandoc-mode for emacs]: http://joostkremers.github.com/pandoc-mode/
[gitit]: http://gitit.net
[pandoc-iki]: https://github.com/dubiousjim/pandoc-iki
[ikiwiki]:  http://ikiwiki.info/
[Bash and zsh command-line completion for pandoc]: https://github.com/dsanson/pandoc-completion
[Using pandoc for IETF RFC creation]: https://github.com/miekg/pandoc2rfc
[Preserving old books by rewriting them in markdown]: https://github.com/rwst/book-curie-radio-de
[Project Gutenberg]: http://www.gutenberg.org
[The Zim desktop wiki]: http://zim-wiki.org
[Texts]: http:/www.texts.io/
[Scripts for using pandoc with BBEdit and TextWrangler]: https://github.com/jrgcmu/BBpandoc