Complete pandoc consists of several distinct Haskell packages
which you might need to look at:

* [jgm/pandoc](https://github.com/jgm/pandoc) itself
* [jgm/pandoc-templates](https://github.com/jgm/pandoc-templates)
* [jgm/pandoc-types](https://github.com/jgm/pandoc-types)
* [jgm/citeproc](https://github.com/jgm/citeproc)

To understand the pandoc implementation, besides Haskell

* understand how frontends and backends are joined in pandoc
* make yourself familiar with [the Parsec monadic parser combinator library](http://legacy.cs.uu.nl/daan/parsec.html)
* to get a grip on monads have a look at [Haskell: Understanding_monads](http://en.wikibooks.org/wiki/Haskell/Understanding_monads)

See also:

* [[Installing the development version of pandoc]].
* [[Contributing to pandoc|https://github.com/jgm/pandoc/blob/master/CONTRIBUTING.md]]

[[Understanding code snippets]] - a q & a style explanation of code snippets from an Intermediate functional programmer trying to learn Haskell and how to structure a Haskell project.