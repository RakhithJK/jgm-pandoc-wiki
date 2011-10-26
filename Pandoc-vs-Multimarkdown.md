This is an evolving document comparing features of Pandoc and
Fletcher Penney's Multimarkdown.

- [Pandoc User's Guide](http://johnmacfarlane.net/pandoc/README.html)
- [Multimarkdown User's Guide](http://fletcher.github.com/peg-multimarkdown/)

## Input formats

| Format            | Pandoc | MMD |
| ------            | ------ | --- |
| markdown          |  yes   | yes |
| reStructuredText  |  yes   | no  |
| Textile           |  yes   | no  |
| HTML              |  yes   | no  |
| LaTeX             |  yes   | no  |

## Output formats

| Format            | Pandoc | MMD |
| ------            | ------ | --- |
| HTML              | yes    | yes |
| LaTeX             | yes    | yes |
| ConTeXt           | yes    | no  |
| markdown          | yes    | no  |
| OPML              | no     | yes |
| OpenDocument XML  | yes    | yes |
| ODT               | yes    | no  |
| Textile           | yes    | no  |
| reStructuredText  | yes    | no  |
| RTF               | yes    | no  |
| DocBook           | yes    | no  |
| Texinfo           | yes    | no  |
| Groff man         | yes    | no  |
| Mediawiki         | yes    | no  |
| Emacs org-mode    | yes    | no  |
| EPUB              | yes    | no  |
| Slidy             | yes    | no  |
| S5                | yes    | no  |


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

### Raw TeX

Pandoc allows raw TeX commands and environments in markdown. These are passed
unchanged to LaTeX and ConTeXt writers, and ignored in other writers.

### Pictures with captions

Pandoc treats an image in a paragraph by itself as a separate figure
with a caption.

### Scripting

Pandoc has a Haskell API for convenient scripting. The AST can be
modified between parsing and writing. For examples, see
[Scripting with pandoc](http://johnmacfarlane.net/pandoc/scripting.html).

## Features in MMD but not pandoc

### Image and link attributes


### Glossary


## Features implemented differently in pandoc and MMD

### Raw HTML

### Anchors and cross-references

### Citations and bibliography

### Definition lists


### Math


### Tables

Pandoc tables are designed to look natural in plain text (but require a monospace font for readability).  Table cells can span multiple lines.  Table cells can contain block-level elements (multiple paragraphs, lists, code blocks).  Row spans and column spans are not currently supported.  Captions are supported.  Cell alignment is determined implicitly, based on the position of the column header.  Cell widths are also determined implicitly, based on the width of the column.

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

MMD tables use `|` characters to indicate columns, so the tables are more readable using a proportional spaced font.  Colons are used to indicate column alignment.  Column spans but not row spans are supported.  Captions are supported. Cells are limited to a single line and cannot contain block-level elements.  Cell widths are not supported.

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

### Metadata

Both pandoc and MMD allow a metadata block at the beginning of the document.  Pandoc only supports title, author, and date (though other metadata can be specified by the command line).  By convention, the first line preceded by `%` is the title, the second (if present) the authors, and the third (if present) the date.  MMD allows arbitrary metadata fields to be specified using a `key : value` format.   Quite a few document features can be controlled using metadata.

MMD does not parse the contents of metadata fields as markdown. Pandoc does, allowing titles and authors to include arbitrary markdown formatting (even footnotes).

An advantage of MMD's system is that arbitrary metadata fields can be specified. A disadvantage is that a document starting with a line containing a colon may be unexpectedly interpreted as beginning with metadata. Try "To be or not to be: that is the question."  Note also that MMD's metadata fields cannot contain blank lines.

Pandoc metadata:

~~~~
% My title with `markdown` *emphasis*
% John MacFarlane
  John Doe
% September 6, 2004
~~~~

MMD metadata:

~~~~
Title:  A New MultiMarkdown Document  
Author: Fletcher T. Penney  
        John Doe  
Date:   July 25, 2005  
~~~~



