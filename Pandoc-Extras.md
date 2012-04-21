## Editors

- [pandoc-mode for emacs], by Joost Kremers
- [vim syntax file for pandoc], courtesy of tao_zhyn
- [vim bundle for pandoc], courtesy of David Sanson, wunki, clvv, fmoralesc
- [TextMate bundle for pandoc], courtesy of David Sanson
- [Tool for using pandoc from Notepad++], courtesy of Ted Lilley
- [The Zim desktop wiki] can export content to markdown using pandoc extensions (need zim version 0.55 and up)

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
- [a build configuration for Pandoc that produces a standalone C-callable
  system library](http://github.com/toyvo/libpandoc/tree/master),
  by Anton Tayanovskyy.


## Illustrative Pandoc / markdown2pdf templates

- Kieran Healy keeps his `~/.pandoc/templates` directory on github: [kjhealy / pandoc-templates](/kjhealy/pandoc-templates).  Note that it calls a [special .sty file](https://github.com/kjhealy/latex-custom-kjh/tree/master/needs-memoir) from [kjhealy / latex-custom-kjh](/kjhealy/latex-custom-kjh) . Similarly see:
- [dsanson / pandoc-templates](/dsanson/pandoc-templates)
- [smile / pandoc-templates](/smile/pandoc-templates) - a German language article template.
- [claes / pandoc-templates](/claes/pandoc-templates) is a good illustration of a template with many user-defined variables, e.g for margins, language, papersize, orientation, etc. Because this makes command line specification a bit unwieldy, he includes [simple shell script](/claes/pandoc-templates/blob/master/md2pdf) with unused options commented out to instruct markdown2pdf how to fill in all the blanks.
- This wiki is developing a collection of [[User Contributed Templates]] for purposes of illustration; contribute your own, or if you keep pandoc templates under revision control, link them here. 

## Doc processing tools using Pandoc

- [Gouda](http://www.unexpected-vortices.com/sw/gouda/docs/) helps authors start, maintain, and publish documentation projects. It's focus is simplicity, ease of use, and encouraging prospective doc contributors.

- [SPAB](http://www.howtoselfpublishabook.org/self-publish-a-book/) a very simple windows GUI that uses Pandoc and a couple other open source tools to produce a .mobi, .epub, .doc, and .pdf ready for the most popular self-publishing services.

- [Pandoc Droplets and Services](https://github.com/dsanson/Pandoc-Droplets-and-Services) is a collection of simple and easily customizable Automator drag-and-drop applets and service menu items that use pandoc to produce pdf, docx, odt, and dzslides output on OS X.

## German Introduction / Deutsche Einführung

Eine kurze Einführung (16 Seiten) zu Markdown und Pandoc.

Der Text beschreibt die Auszeichnungssprache Markdown und die Benutzung des Programms Pandoc unter Berücksichtigung der Eigenheiten der deutschen Sprache.
Er geht auf die Erzeugung von eBüchern (ePub und Kindle) sowie den Export nach OpenOffice und MS Office ein.
Außerdem bietet er Vorlagen (templates) zur Verwendung mit LaTeX.

Das komplette Projekt gibt es auf [https://github.com/AKielhorn/Markdown-Intro].

Das PDF Dokument kann man unter [http://dl.dropbox.com/u/6568507/Ziele-md.pdf] herunterladen, das dazugehörige Begleitmaterial sowie die Vorlagen gibt es als Zip archiv unter [http://dl.dropbox.com/u/6568507/Ziele.zip].

## Official documents and books

- [Using pandoc for IETF RFC creation], courtesy of Miek Gieben
- [Preserving old books by rewriting them in markdown], one example of several etexts at [Project Gutenberg] originally transcribed in markdown


[an even simpler route]: http://randomdeterminism.wordpress.com/2011/01/09/markdowntopdf/
[filter module]: https://github.com/adityam/filter
[vim syntax file for pandoc]: http://www.vim.org/scripts/script.php?script_id=2389
[vim bundle for pandoc]: https://github.com/vim-pandoc/vim-pandoc
[TextMate bundle for pandoc]: http://github.com/dsanson/Pandoc.tmbundle
[jsMath]: http://www.math.union.edu/~dpvc/jsMath/
[WordPress EasyFilter]: http://assorted.sourceforge.net/wp-easy-filter/
[Haskell platform]: http://hackage.haskell.org/platform/
[Tool for using pandoc from Notepad++]: http://www.binaryphile.com/clip-pandoc.aspx
[pandoc-mode for emacs]:  http://joostkremers.fastmail.fm/pandoc-mode.html
[gitit]: http://gitit.net
[pandoc-iki]: https://github.com/dubiousjim/pandoc-iki
[ikiwiki]:  http://ikiwiki.info/
[Bash and zsh command-line completion for pandoc]: https://github.com/dsanson/pandoc-completion
[Using pandoc for IETF RFC creation]: https://github.com/miekg/pandoc2rfc
[Preserving old books by rewriting them in markdown]: https://github.com/rwst/book-curie-radio-de
[Project Gutenberg]: http://www.gutenberg.org
[The Zim desktop wiki]: http://zim-wiki.org