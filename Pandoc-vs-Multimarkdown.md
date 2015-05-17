This is an evolving document comparing features of Pandoc (1.13) and
Fletcher Penney's Multimarkdown (version 3).

- [Pandoc User's Guide](http://johnmacfarlane.net/pandoc/README.html)
- [Multimarkdown User's Guide](http://fletcher.github.com/peg-multimarkdown/)

Note: as of v 1.10, pandoc allows limited conversion between markdown dialects.
Not all features are yet supported, and conversions will not be perfect. But they
should help for those who want to migrate.

pandoc &rarr; multimarkdown:

    pandoc -s -t markdown_mmd

multimarkdown &rarr; pandoc:

    pandoc -s -f markdown_mmd -t markdown

## Input formats

| Format            | Pandoc | MMD |
| ------            | ------ | --- |
| markdown          |  ✓     | ✓   |
| reStructuredText  |  ✓     |     |
| Textile           |  ✓     |     |
| HTML              |  ✓     |     |
| LaTeX             |  ✓     |     |
| DocBook           |  ✓     |     |
| MediWiki          |  ✓     |     |
| OPML              |  ✓     |     |
| Emacs org-mode    |  ✓     |     |
| Haddock           |  ✓     |     |
| txt2tags          |  ✓     |     |
| Word docx         |  ✓     |     |
| EPUB              |  ✓     |     |

## Output formats

| Format            | Pandoc | MMD v2 | MMD v3 |
| ------            | ------ | ---    | -----  |
| XHTML 4           | ✓      | ✓      | ✓      |
| HTML 5            | ✓      |        |        |
| LaTeX             | ✓      | ✓      | ✓      |
| ConTeXt           | ✓      |        |        |
| markdown          | ✓      |        |        |
| Word docx         | ✓      |        |         |
| OPML              | ✓      | ✓      | ✓      |
| OpenDocument XML  | ✓      |        | ✓      |
| ODT               | ✓      |        |        |
| Textile           | ✓      |        |        |
| reStructuredText  | ✓      |        |        |
| RTF               | ✓      | ✓      |        |
| DocBook           | ✓      |        |        |
| Texinfo           | ✓      |        |        |
| Groff man         | ✓      |        |        |
| Mediawiki         | ✓      |        |        |
| Emacs org-mode    | ✓      |        |        |
| EPUB v2           | ✓      | ✓      |        |
| EPUB v3           | ✓      | ✓      |        |
| Slidy             | ✓      |        |        |
| S5                | ✓      | ✓      |        |
| dzslides          | ✓      |        |        |
| Beamer            | ✓      |        |        |
| AsciiDoc          | ✓      |        |        |
| FictionBook 2     | ✓      |        |        |
| DokuWiki          | ✓      |        |        |

## Features in pandoc but not MMD

### Templates

Pandoc includes a templating system for standalone documents.
Default templates are included, but users can override them with
custom templates.

### Delimited code blocks

Pandoc supports delimited code blocks, like this:

    ~~~~ {.haskell}
    fibs = 1 : 1 : zipWith (+) (tail fibs) fibs
    ~~~~

or

    ``` haskell
    as = 1 : as
    ```

### Code highlighting

Pandoc highlights code marked with a language in a delimited
code block.  No external program is required.  Over 80 syntaxes are
supported.

### Example lists

Pandoc supports a syntax for running example lists that are incremented
throughout a document:

~~~~
(@one)  My first example will be numbered (1).
(@)  My second example will be numbered (2).

Explanation of example (@one).

(@)  My third example will be numbered (3).
~~~~

### Fancy list numbers

Pandoc allows ordered lists to have different numbering styles
and delimiters; these are recorded and reproduced, where possible,
in the output format.

~~~~
(a) My list
(b) Lowercase letters
    i. Roman sublist
    ii. Next
~~~~

### List start number

In standard markdown the starting number of an ordered list is
ignored, so all lists start with 1. Pandoc allows lists to start
with any number.

### Strikeout

Pandoc supports strikeout `~~like so~~`.

### Superscript

Pandoc supports superscripts:  `mc^2^`.

### Subscript

Pandoc supports subscripts:  `H~2~O`.

### Inline footnotes

Pandoc allows inline footnotes, like `this^[Here's a note.]`.

### Scripting

Pandoc has a Haskell API for convenient scripting. The AST can be
modified between parsing and writing. For examples, see
[Scripting with pandoc](http://johnmacfarlane.net/pandoc/scripting.html).
There is also a python module for writing pandoc filters using python.

## Features in MMD but not pandoc

### Image and link attributes

MMD supports image and link attributes using the following syntax:

~~~~
[image]: http://path.to/image "Image title" width=40px height=400px
[link]:  http://path.to/link.html "Some Link" class=external
         style="border: solid black 1px;"
~~~~

### Glossary

MMD turns specially marked footnotes into glossary entries in
LaTeX:

~~~~
[^glossaryfootnote]: glossary: term (optional sort key)
    The actual definition belongs on a new line, and can continue on
    just as other footnotes.
~~~~

## Features implemented differently in pandoc and MMD

### Raw TeX

Pandoc allows raw TeX commands and environments in markdown. These are passed
unchanged to LaTeX and ConTeXt writers, and ignored in other writers.

MMD allows raw TeX inside HTML comments.  It also supports the LaTeX
`\input` command, while pandoc does not.

### Raw HTML

In pandoc, text within HTML  block tags is parsed as markdown,
unless the `--strict` option is used.  In MMD, it is parsed as markdown
if the `--process-html` option is used, or if the block tag contains
the `markdown` attribute.

### Citations and bibliography

Pandoc has extensive support for automatic citation and bibliography
generation that works in every output format. Many existing bibliography
database formats (including BibTeX, MODS, and EndNote) can be used. Citations
are written in markdown as follows:

~~~~
Blah blah [see @doe99, pp. 33-35; also @smith04, ch. 1].
Blah blah [@doe99, pp. 33-35, 38-39 and *passim*].
Blah blah [@smith04; @doe99].
Smith says blah [-@smith04].
@smith04 [p. 33] says blah.
~~~~

They will be formatted in the output according to a
[CSL]((http://citationstyles.org/) stylesheet specified on the
command line, and a bibliography will be added automatically if the style
calls for it. Many, many styles are available. You can even switch freely
between author-date and footnote styles, and pandoc will do the right thing
with surrounding punctuation.

MMD has much more rudimentary citation support.  Example:

~~~~
This is a statement that should be attributed to
its source[p. 23][#Doe:2006].

And following is the description of the reference to be
used in the bibliography.

[#Doe:2006]: John Doe. *Some Big Fancy Book*.  Vanity Press, 2006.
~~~~

There is no automatic citation/bibliography formatting, unless LaTeX
output is used (in which case `natbib` and `bibtex` are used).

### Math

Pandoc allows inline and display LaTeX math.  `$` delimiters are
used for inline math, and `$$` for display math.  If LaTeX macros
have been defined in the document, they are automatically applied
to all math (and this works even if the output format is not LaTeX).
A variety of HTML output options are possible,  including
direct conversion to MathML, faking it with unicode, raw LaTeX
for use with MathJax, and images.

MMD also allows inline and display LaTeX math, but `\\(` delimiters
are used for inline math, and `\\[` for display math. MathJax is
used in HTML.

### Tables

Pandoc tables are designed to look natural in plain text (but require a monospace font for readability).  Table cells can span multiple lines.  Table cells can contain block-level elements (multiple paragraphs, lists, code blocks).  Row spans and column spans are not currently supported.  Captions are supported.  Cell alignment is determined implicitly, based on the position of the column header.  Relative cell widths in the output format will mirror the relative widths of the columns in the markdown source.

Simple table:

~~~~
  Right     Left     Center     Default
-------     ------ ----------   -------
     12     12        12            12
    123     123       123          123
      1     1          1             1

Table:  Demonstration of simple table syntax.
~~~~

Multiline table:

~~~~
----------- ------- --------------- -------------------------
   First    row                12.0 Example of a row that
                                    spans multiple lines.

  Second    row                 5.0 Here's another one. Note
                                    the blank line between
                                    rows.
-------------------------------------------------------------
~~~~

Grid table (generated using Emacs table mode):

~~~~
+---------------+---------------+--------------------+
| Fruit         | Price         | Advantages         |
+===============+===============+====================+
| Bananas       | $1.34         | - built-in wrapper |
|               |               | - bright color     |
+---------------+---------------+--------------------+
| Oranges       | $2.10         | - cures scurvy     |
|               |               | - tasty            |
+---------------+---------------+--------------------+
~~~~

As of 1.10, pandoc also offers pipe tables as in PHP markdown extra; these can be used when it is inconvenient to line up columns.  Pipe tables look like this:

    | Right | Left | Default | Center |
    |------:|:-----|---------|:------:|
    |   12  |  12  |    12   |    12  |
    |  123  |  123 |   123   |   123  |
    |    1  |    1 |     1   |     1  |

      : Demonstration of simple table syntax.

MMD tables use `|` characters to indicate columns, so the tables are more readable using a proportional spaced font.  Colons are used to indicate column alignment.  Column spans but not row spans are supported.  Captions are supported. Cells are limited to a single line and cannot contain block-level elements.

MMD table:

~~~~
|             |          Grouping           ||
First Header  | Second Header | Third Header |
 ------------ | :-----------: | -----------: |
Content       |          *Long Cell*        ||
Content       |   **Cell**    |         Cell |

New section   |     More      |         Data |
And more      |            And more          |
[Prototype table]
~~~~

Pandoc's pipe tables are similar, but do not have all of
the features of MMD pipe tables (sections, colspan, rowspan, grouping).

### Metadata

Both pandoc and MMD allow a title block at the beginning of the document.  A pandoc title block contains title, author, and date (though other metadata can be specified by the command line).  By convention, the first line preceded by `%` is the title, the second (if present) the authors, and the third (if present) the date.  MMD allows arbitrary metadata fields to be specified using a `key : value` format.   Quite a few document features can be controlled using metadata.

MMD does not parse the contents of metadata fields as markdown. Pandoc does, allowing titles and authors to include arbitrary markdown formatting (even footnotes).

An advantage of MMD's system is that arbitrary metadata fields can be specified. A disadvantage is that a document starting with a line containing a colon may be unexpectedly interpreted as beginning with metadata. Try beginning a document with "This above all: to thine own self be true." Note also that MMD's metadata fields cannot contain blank lines.

As of version 1.12, pandoc allows YAML metadata blocks to be inserted anywhere in a document.  Metadata fields may contain formatted text.

Pandoc title block:

~~~~
% My title with `markdown` *emphasis*
% John MacFarlane
  John Doe
% September 6, 2004
~~~~

Pandoc YAML metadata block:

~~~~
---
title: My title with `markdown` *emphasis*
author:
- John MacFarlane
- John Doe
doi: 10.234234.23424/x
date: September 6, 2004
abstract: |
  A formatted abstract here.

  May contain multiple paragraphs.
...
~~~~

MMD metadata:

~~~~
Title:  A New MultiMarkdown Document  
Author: Fletcher T. Penney  
        John Doe  
Date:   July 25, 2005  
~~~~