If you want to live dangerously, you can install the still-in-progress pandoc (and pandoc-citeproc) from github.  You'll need git and [stack](https://github.com/commercialhaskell/stack/releases).  Grab the stack binary that is appropriate for your platform and put it somewhere in your path.

Then:

    mkdir pandoc-build
    cd pandoc-build
    git clone https://github.com/jgm/pandoc-types
    git clone https://github.com/jgm/texmath
    git clone https://github.com/jgm/pandoc-citeproc
    git clone https://github.com/jgm/pandoc
    git clone https://github.com/jgm/cmark-hs
    git clone https://github.com/jgm/zip-archive
    cd pandoc
    git submodule update --init
    stack install --test --install-ghc --stack-yaml stack.full.yaml

The `stack install` command will let you know where it put the binaries (pandoc and pandoc-citeproc).  You may have to add this location (`~/.local/bin`, on linux and OSX systems) to your path.

To pull in the latest changes, after you've done this and there have been changes in the repositories:  Visit each repository in pandoc-build (pandoc-types, texmath, pandoc-citeproc, pandoc, zip-archive, cmark-hs) and do `git pull`.  In the pandoc repo, also do `git submodule update` and `stack install --test --stack-yaml stack.full.yaml`.
