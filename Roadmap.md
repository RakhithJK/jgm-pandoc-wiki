Pandoc doesn't have a strict roadmap but this page should serve as an overview about major changes to pandoc that are under consideration.

See also the [Task list issue](https://github.com/jgm/pandoc/issues/1852).

## AST changes

- [Attributes for all elements](https://github.com/jgm/pandoc/issues/684)
- changing `String` to `Text` everywhere
- [a Figure block element](https://github.com/jgm/pandoc/issues/3177), capable of containing multiple images 
- [short and long captions for tables](https://github.com/jgm/pandoc/issues/2978), figures, [headers](https://github.com/jgm/pandoc/issues/4409)
- [internal links/references to tables and figures](https://github.com/jgm/pandoc/issues/813)
- [PageBreak element](https://github.com/jgm/pandoc/issues/1934)
- replace Strikeout element with a Deleted element; add an [Inserted element](https://github.com/jgm/pandoc/issues/3035)
- [colspans in tables](https://github.com/jgm/pandoc/issues/1024) (and possibly also rowspan)
- [change Format from String to a sum type](https://github.com/jgm/pandoc/issues/547)
- possible [`Underline` element](https://github.com/jgm/pandoc/pull/2270)
- [Endnotes support for Docx, ODT and Muse](https://github.com/jgm/pandoc/pull/4042)

## Filters/JSON

- changing JSON serialization format to be more human-readable, less Haskell-ADT-oriented, [see this comment](https://github.com/jgm/pandoc/issues/3211#issuecomment-258783108)

## YAML/command-line metadata handling

- [Feature request: Ability to specify a YAML metadata file for all reader types](https://github.com/jgm/pandoc/issues/1960)
- [Specify command-line options using YAML metadata](https://github.com/jgm/pandoc/issues/4627)
- [Command-line options --css and --include-in-header override corresponding metadata fields instead of accumulating](https://github.com/jgm/pandoc/issues/3139)
- [Combining multiple metadata blocks](https://github.com/jgm/pandoc/issues/3115)
- [Allow complex metadata fields to be defined on the command line (e.g. using JSON)](https://github.com/jgm/pandoc/issues/3732)
- [Change merge-behaviour of two metadata blocks](https://github.com/jgm/pandoc/issues/4057)

## Other

- [Custom styles in ODT, ICML (and maybe LaTeX and RST) writers](https://github.com/jgm/pandoc/issues/2106) (maybe also [in Docbook](https://github.com/jgm/pandoc/issues/3657))