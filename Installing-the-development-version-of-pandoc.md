If you want to live dangerously, you can try the unofficial [pandoc-nightly builds](https://github.com/pandoc-extras/pandoc-nightly), or install the still-in-progress pandoc from GitHub.  You'll need git and [stack](https://github.com/commercialhaskell/stack/releases).  Grab the stack binary that is appropriate for your platform and put it somewhere in your path.

Then:

    git clone https://github.com/jgm/pandoc
    cd pandoc
    stack setup # only first time
    stack install

The `stack install` command will let you know where it put the `pandoc` binary.  You may have to add this location (`~/.local/bin`, on linux and OSX systems) to your path.

For more info, see also [CONTRIBUTING.md](https://github.com/jgm/pandoc/blob/master/CONTRIBUTING.md)