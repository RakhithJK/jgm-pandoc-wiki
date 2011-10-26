## Features in pandoc but not MMD

## Features in MMD but not pandoc

## Features implemented differently in pandoc and MMD

### Tables

Pandoc tables are designed to look natural in plain text (but require a monospace font for readability).  Table cells can span multiple lines.  Table cells can contain block-level elements (multiple paragraphs, lists, code blocks).  Row spans and column spans are not currently supported.  Captions are supported.  Cell alignment is determined implicitly, based on the position of the column header.  Cell widths are also determined implicitly, based on the width of the column.

Simple table:

```
  Right     Left     Center     Default
-------     ------ ----------   -------
     12     12        12            12
    123     123       123          123
      1     1          1             1

Table:  Demonstration of simple table syntax.
```

Multiline table:

```
----------- ------- --------------- -------------------------
   First    row                12.0 Example of a row that
                                    spans multiple lines.

  Second    row                 5.0 Here's another one. Note
                                    the blank line between
                                    rows.
-------------------------------------------------------------
```

Grid table (generated using Emacs table mode):

```
+---------------+---------------+--------------------+
| Fruit         | Price         | Advantages         |
+===============+===============+====================+
| Bananas       | $1.34         | - built-in wrapper |
|               |               | - bright color     |
+---------------+---------------+--------------------+
| Oranges       | $2.10         | - cures scurvy     |
|               |               | - tasty            |
+---------------+---------------+--------------------+
```

MMD tables use `|` characters to indicate columns, so the tables are more readable using a proportional spaced font.  Colons are used to indicate column alignment.  Column spans but not row spans are supported.  Captions are supported. Cells are limited to a single line and cannot contain block-level elements.  Cell widths are not supported.

MMD table:

```
|             |          Grouping           ||
First Header  | Second Header | Third Header |
 ------------ | :-----------: | -----------: |
Content       |          *Long Cell*        ||
Content       |   **Cell**    |         Cell |

New section   |     More      |         Data |
And more      |            And more          |
[Prototype table]
```

