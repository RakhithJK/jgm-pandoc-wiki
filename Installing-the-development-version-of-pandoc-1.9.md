If you want to live dangerously, you can install the still-in-progress pandoc 1.9 from github:

    git clone git://github.com/jgm/pandoc
    cd pandoc
    git submodule update --init  # to update the templates submodule
    cabal install -ftests        # -fhighlighting no longer needed
    cabal test

