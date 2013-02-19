If you want to live dangerously, you can install the still-in-progress pandoc 1.11 from github:

    git clone git://github.com/jgm/pandoc
    cd pandoc
    make prep
    make install
    cabal-dev/bin/pandoc --version

Note:  If you have trouble installing cabal-dev on windows, see [here](http://stackoverflow.com/questions/11181428/cannot-install-cabal-dev-in-windows).

