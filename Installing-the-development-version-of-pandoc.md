If you want to live dangerously, you can install the still-in-progress pandoc (and pandoc-citeproc) from github.  You'll need git and [stack](https://github.com/commercialhaskell/stack/releases).  Grab the stack binary that is appropriate for your platform and put it somewhere in your path.

Then:

    git clone https://github.com/jgm/pandoc
    cd pandoc
    git submodule update --init
    git clone git://github.com/jgm/pandoc-citeproc
    stack install --test --install-ghc --stack-yaml stack.full.yaml

The `stack install` command will let you know where it put the binaries (pandoc and pandoc-citeproc).  You may have to add this location (`~/.local/bin`, on linux and OSX systems) to your path.

