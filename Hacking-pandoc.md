Complete pandoc consists of several distinct Haskell packages
which you might need to look at:

* [jgm/pandoc](https://github.com/jgm/pandoc) itself
* [jgm/pandoc-templates](https://github.com/jgm/pandoc-templates)
* [jgm/pandoc-types](https://github.com/jgm/pandoc-types)
* [citeproc-hs](http://gorgias.mine.nu/repos/citeproc-hs/)

To understand the pandoc implementation, besides Haskell

* understand how frontends and backends are joined in pandoc
* make yourself familiar with [the Parsec monadic parser combinator library](http://legacy.cs.uu.nl/daan/parsec.html)
