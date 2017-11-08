If you want to live dangerously, you can try the unofficial [pandoc-nightly builds](https://github.com/pandoc-extras/pandoc-nightly), or install the still-in-progress pandoc from GitHub.  You'll need git and [stack](https://github.com/commercialhaskell/stack/releases).  Grab the stack binary that is appropriate for your platform and put it somewhere in your path.

Then:

    git clone https://github.com/jgm/pandoc
    cd pandoc
    stack setup # only first time
    stack install

Depending on your needs, you'll have to do the same for the following repositories:

    https://github.com/jgm/pandoc-types
    https://github.com/jgm/texmath
    https://github.com/jgm/pandoc-citeproc
    https://github.com/jgm/cmark-hs
    https://github.com/jgm/zip-archive

The `stack install` command will let you know where it put the binaries (pandoc and pandoc-citeproc).  You may have to add this location (`~/.local/bin`, on linux and OSX systems) to your path.
