# Welcome to the pandoc wiki!

This is a place for users to share pandoc tips.

## Extensions ##

These extend the capabilities of pandoc:

- [[Pandoc Extras]]: tools built around pandoc
- [[User-contributed templates]]: other templates to build standalone output formats.
- [[Pandoc Filters]]: Pandoc provides an interface for users to write programs (known as filters) which act on the intermediate AST.
- [[Pandoc Boilerplates]]: readymade pandoc presentation style

## Learning Pandoc ##

- General:
	- [[Documentation and Tutorials]]
	- [[Tabular overview: Pandoc's supported Markdown dialects and extensions enabled by default]]
	- [[Hacking Pandoc]]
- Examples:
	- [[Creating impress.js slide shows with pandoc]]
	- [[How to add a "Table of Contents" title in the HTML template]]
- Compiling pandoc:
	- [[Installing the development version of pandoc]]
- Debug LaTeX output examples:
	- [[Pandoc with gentoo]]
	- [[Pandoc with Chinese]](简体中文)
	- [[Trials and Tribulations: How to find correct font names for Pandoc's use with LuaLaTeX?]]
    - [[Minimal TeXLive installation for Pandoc]]


## Comparisons ##

- [[Pandoc vs Multimarkdown]]
- [[Pandoc vs Markdown.pl]]

## Exit Codes ##

based on <src/Text/Pandoc/Error.hs>

3
:   Failing because there were warnings

4
:   Application error

5
:   Error compiling template

6
:   Option error

21
:   Unknown input format

22
:   Unknown output format

23
:   Unsupported file extension

31
:   EPUB subdirectory contains illegal characters

43
:   Error producing PDF

47
:   PDF program not found

61
:   Could not fetch via HTTP

62
:   Should never happen

63
:   Some error

64
:   Parse error

65
:   Parsec error (?)

66
:   Make pdf error

67
:   Syntax map error

83
:   Error running filter

91
:   Loop encountered in expanding macro

92
:   UTF-8 decoding error

93
:   IPython notebook decoding error

97
:   Could not find data file

99
:   Resource not found
