If you want to live dangerously, you can install the still-in-progress pandoc 1.10 from github:

    git clone git://github.com/jgm/pandoc
    cd pandoc
    cabal update
    git submodule update --init  # to update the templates submodule
    cabal install cabal-dev
    make prep
    make install
    cabal-dev/bin/pandoc --version

