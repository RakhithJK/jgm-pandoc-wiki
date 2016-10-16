# TL; DR

It seems the best way to configure Travis CI to build pandoc files is through `language: R` and non-container-based VM images, at least if you need to install python pandoc filters. See [Notes] below. See documentation at: [Building an R Project - Travis CI](https://docs.travis-ci.com/user/languages/r/).

# Notes

- Travis CI's container based is very restricted:
    - if `language: python` is used, then `pip` can be used to install pandocfilters, but `tlmgr` cannot be used.
    - if `language: R` is used, TeXLive, `tlmgr`, `pandoc` are default to it, but `pip` cannot be run without `sudo`^[pip]

To summarize, if you want to use the container based VM, `pip` and `tlmgr` is mutually exclusive:

- If you need `tlmgr` but not pandoc filters, `language: R` will serve you well.

- Not have access to `tlmgr` means you can only use the Ubuntu pre-packaged version of TeXLive (*e.g.* `sudo apt-get install texlve-full`), and the container-based Ubuntu 12.04 shipped with TeXLive 2009, which are *old*[^eg]. If you are fine with that and need pandoc filters, `language: pip` is the way to go.

[^pip]: See [travis-ci/travis-ci#3563](https://github.com/travis-ci/travis-ci/issues/3563).

[^eg]: An example is the Physics package, which is kind of new.
