Pandoc doesn't have a strict roadmap but this page should serve as an overview about major changes to pandoc that are on our radar.


## AST changes

- restructuring readers and writers in typeclasses so that they can be run consistently in either pure or in IO, see [jgms's explanation](https://groups.google.com/forum/#!msg/pandoc-discuss/oA7a_zPzzak/jnlIbaFhCgAJ;context-place=searchin/pandoc-discuss/2.0%7Csort:relevance), also [issue 2930](https://github.com/jgm/pandoc/issues/2930)
- [Attributes for all elements](https://github.com/jgm/pandoc/issues/684)
- changing `String` to `Text` everywhere
- changing JSON serialization format to be more human-readable, less Haskell-ADT-oriented, [see this comment](https://github.com/jgm/pandoc/issues/3211#issuecomment-258783108)
- [a Figure block element](https://github.com/jgm/pandoc/issues/3177), capable of containing multiple images 
- [internal links/references to tables and figures](https://github.com/jgm/pandoc/issues/813)
- [PageBreak element](https://github.com/jgm/pandoc/issues/1934)
- replace Strikeout element with a Deleted element; add an [Inserted element](https://github.com/jgm/pandoc/issues/3035)
- [colspans in tables](https://github.com/jgm/pandoc/issues/1024) (and possibly also rowspan)
- [change Format from String to a sum type](https://github.com/jgm/pandoc/issues/547)


## YAML/command-line metadata handling

- https://github.com/jgm/pandoc/issues/3139
- https://github.com/jgm/pandoc/issues/3115
- https://github.com/jgm/pandoc/issues/3060
- https://github.com/jgm/pandoc/issues/3138
- https://github.com/jgm/pandoc/issues/1960
- https://github.com/jgm/pandoc/issues/2139
