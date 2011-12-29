If you want to live dangerously, you can install the still-in-progress pandoc 1.9 from github.  But the dependencies are a bit complicated, so here is a guide:

pandoc-types 1.9:

    git clone git://github.com/jgm/pandoc-types
    cd pandoc-types
    cabal install
    cd ..

highlighting-kate 0.4:

    git clone git://github.com/jgm/highlighting-kate
    cd highlighting-kate
    cabal update    # because it needs a recent version of blaze-html
    cabal install
    cd ..

citeproc-hs 0.3.4 -- the version at the official repository lacks two patches that are needed to build citeproc-hs against pandoc-types 1.9, so until they are applied I am temporarily hosting a repository that has them:

    darcs get http://johnmacfarlane.net/repos/citeproc-hs
    cd citeproc-hs
    cabal install

Now we can get the source for pandoc and compile:

    git clone git://github.com/jgm/pandoc
    cd pandoc
    git submodule update --init  # to update the templates submodule
    cabal install -ftests        # -fhighlighting no longer needed
    cabal test

The three citations tests currently fail.  If anything else fails, or if these insntructions don't work, let me (jgm) know.
