Normally you should be able to install the latest version of pandoc on a debian/ubuntu machine by doing:

```
cabal update
cabal install
cabal install haskell-platform
cabal install pandoc
```

If you get the following error

```
cabal: dependencies conflict: base-3.0.3.2 requires syb ==0.1.0.2
however
syb-0.1.0.2 was excluded because json-0.5 requires syb >=0.3.3
```

then the workaround is:

```
cabal install json-0.4.4
cabal install pandoc
```

This workaround should no longer be needed once debian/ubuntu updates to ghc 7.x.
