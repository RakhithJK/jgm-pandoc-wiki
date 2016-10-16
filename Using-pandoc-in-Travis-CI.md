# TL; DR

It seems the best way to configure Travis CI to build pandoc files is through `language: R` and container-based VM images. If you need to install python pandoc filters, then use `pip install --user pandocfilters`.

# Notes

If `language: R` is used, TeXLive, `tlmgr`, `pandoc` are default to it.

See documentation at: [Building an R Project - Travis CI](https://docs.travis-ci.com/user/languages/r/).
