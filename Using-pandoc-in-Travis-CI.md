It seems the best way to configure Travis CI to build pandoc files is through `language: R` and non-container-based VM images, at least if you need to install python pandoc filters. See [Notes] below. See documentation at: [Building an R Project - Travis CI](https://docs.travis-ci.com/user/languages/r/).

# Notes

- Travis CI's container based is very restricted:
    - if `language: python` is used, then `pip` can be used to install pandocfilters, but `tlmgr` cannot be used.
    - if `language: R` is used, TeXLive, `tlmgr`, `pandoc` are default to it, but `pip` cannot be run without `sudo`^[pip]

[^pip]: See [travis-ci/travis-ci#3563](https://github.com/travis-ci/travis-ci/issues/3563).
