The pandoc supplied by Raspbian is rather old (1.9.4.2)
The last version you can build is 1.11.1 but this is still an improvement.
Here is how to do it:

  - You need a Model B with 512 MB Ram
  - You need an 8 GB (or larger) SD Card
  - Use `raspi-confg` to set the Memory split to 16 (You can change back to 64 later)
  - Set swap to 500 MB (`/etc/dphys-swapfile`)
  - `sudo apt-get update && apt-get upgrade`
  - Boot into console mode (no X)
  - If you have any services running (Samba, Hostapd, Apache, ... ) stop them.
  - `sudo apt-get haskell-platform`
  - `cabal update`
  - You may want to edit `.cabal/config` now and set `user-install` to `False`, remove the `--` to uncomment.
    Otherwise the newly build packages are only available for the user who build them.
  - `cabal install pandoc-1.11.1` this will take hours, be warned.
  - You may now want to install texlive, the full version will take up 3.5 GB but the small version should do for pandoc.
