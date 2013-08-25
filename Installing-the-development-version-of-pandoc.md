If you want to live dangerously, you can install the still-in-progress pandoc from github.  You'll need git and the Haskell Platform.

First, you'll need to install the new pandoc-types from the github repository:

    git clone git://github.com/jgm/pandoc-types
    cd pandoc-types
    cabal update
    cabal install --force
    cd ..

Now get the pandoc source:

    git clone git://github.com/jgm/pandoc
    cd pandoc
    git submodule update --init
    cabal install --enable-tests

## Extra info for Haskell/Ubuntu novices

Here is one route that successfully sets up an environment to develop pandoc on.

For all these commands, after several attempts, I ended up putting them into a script and saving the output, as several earlier attempts, with different combinations of version, gave errors.

1. Set up an Ubuntu 12.10 environment ([you'll have problems if you go for 13.04](http://askubuntu.com/questions/286764/how-to-install-haskell-platform-for-ubuntu-13-04))
1. Run the following commands:

        sudo apt-get update
        sudo apt-get install git
        sudo apt-get install haskell-platform

    The update command is needed. Without this, there are errors about incorrect versions. (I didn't record the messages)

1. Run the commands in the main section of this page.