> *Note: the following instructions were tested on Mac OSX. They should work on Linux too. For Windows you'll have to adapt them as needed.*

***LuaLaTeX makes it easy to use any font installed on your system when using `pandoc` to generate PDFs from Markdown source text.***

***Really easy? Yes... but only if you know how to specify a font name that LuaLaTeX actually understands!***

Consider this: I was interested in all variations of my newly installed FOSS Adobe [*Source Code Pro*](https://github.com/adobe-fonts/source-code-pro) fonts. They are said to have a really good coverage of *box drawing characters*, and I wanted to see that by letting **`pandoc --latex-engine=lualatex`** convert the following code block sample (which I copied from [here](http://adobe-type-tools.github.io/boxDrawing/)) to PDF. This code box is part of my Markdown file *'fun-with-box-drawing.md'*:

```python

        ┌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┐
        ┆Part of    ░▒▓▓▓▓┆
        ┆this stroked ░▒▓▓┆     ████████████▒
        ┆box is shaded.░▒▓┆     █████OK█████▓
        └╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┘     ████████████▓
   ╔═══════════════╗            ▒▓▓▓▓▓▓▓▓▓▓▓▓
   ║Here are some  ║
   ║block elements ║ ┌───────────┐
   ║within a box.  ║ │ ▏▏▏▏▏▏▏ ▁ │
   ╠═══════════════╣ │ ▎▎▎▎▎▎ ▂▁ │
   ╙────┰─────┰────╜ │ ▍▍▍▍▍ ▃▂▁ │
        ┃     ┗━━━━━━┥ ▌▌▌▌ ▄▃▂▁ │
        ┃ → → → → → →│ ▋▋▋ ▅▄▃▂▁ │
        ┃ → → → → → →│ ▊▊ ▆▅▄▃▂▁ │
        ┗━━━━━━━━━━━━┥ ▉ ▇▆▅▄▃▂▁ │
                     │ ██▇▆▅▄▃▂▁ │
                     └───────────┘


```

## Question: Which fontname to use?

Now what is a valid font name that can be given so that LuaLaTeX actually finds it? The ***filenames*** of my installed 'Source Code' fonts are these:

```bash
kp@mbp:~> ls -1 /Library/Fonts/SourceCodePro-*
 /Library/Fonts/SourceCodePro-Black.ttf
 /Library/Fonts/SourceCodePro-Bold.ttf
 /Library/Fonts/SourceCodePro-ExtraLight.ttf
 /Library/Fonts/SourceCodePro-Light.ttf
 /Library/Fonts/SourceCodePro-Regular.ttf
 /Library/Fonts/SourceCodePro-Semibold.ttf
```

I expected that adding **`-V monofont=SourceCodePro-Light`** to the Pandoc command line would work. It didn't. Next, I tried **`-V monofont=SourceCodePro-Light.ttf`**. Still no dice!

Then I tried to find out to the ***PostScript name*** as stored inside the font file itself. This is because a font's filename frequently does not match the PostScript name:

```
kp@mbp:> otfinfo -i /Library/Fonts/SourceCodePro-Light.ttf | head -n 17
 Family:              Source Code Pro Light
 Subfamily:           Regular
 Full name:           Source Code Pro Light
 PostScript name:     SourceCodePro-Light
 Preferred family:    Source Code Pro
 Preferred subfamily: Light
 Version:             Version 1.010;PS Version 1.000;hotconv 1.0.70;makeotf.lib2.5.5900
 Unique ID:           1.010;ADBE;SourceCodePro-Light;ADOBE
 Designer:            Paul D. Hunt
 Manufacturer:        Adobe Systems Incorporated
 Vendor URL:          http://www.adobe.com/type
 Trademark:           Source is a trademark of Adobe Systems Incorporated in the United States and/or other countries.
 Copyright:           Copyright 2010, 2012 Adobe Systems Incorporated. All Rights Reserved.
 License URL:         http://www.adobe.com/type/legal.html
 License Description: Copyright 2010, 2012 Adobe Systems Incorporated (http://www.adobe.com/), with Reserved Font Name 'Source'.
                      All Rights Reserved. Source is a trademark of Adobe Systems Incorporated in the United States and/or other countries.

 This Font Software is licensed under the SIL Open Font License, Version 1.1.

```

Hmmm... in this case it matched. I ***had*** tried the name *SourceCodePro-Light* already. For completeness, despite my extremely low expectation in this case, I also tried `-V monofont="Source Code Pro Light"`. After all, `otfinfo -i` had suggested this as being the *"Full name"* of the font in question... Guess what?

Next, I tried to employ *FontConfig*'s `fc-list` command to see what other fancy font names were available:

```
kp@mbp:~> fc-list | grep -i source | grep -i code
 /Library/Fonts/SourceCodePro-Black.ttf: Source Code Pro,Source Code Pro Black:style=Black,Regular
 /Library/Fonts/SourceCodePro-Light.ttf: Source Code Pro,Source Code Pro Light:style=Light,Regular
 /Library/Fonts/SourceCodePro-ExtraLight.ttf: Source Code Pro,Source Code Pro ExtraLight:style=ExtraLight,Regular
 /Library/Fonts/SourceCodePro-Semibold.ttf: Source Code Pro,Source Code Pro Semibold:style=Semibold,Regular
 /Library/Fonts/SourceCodePro-Bold.ttf: Source Code Pro:style=Bold
 /Library/Fonts/SourceCodePro-Regular.ttf: Source Code Pro:style=Regular
``` 

So **`fc-list`** didn't come up with anything different than `otfinfo` already had.

What now? Some search-engineing brought me a step closer to the solution.


## Solution

Three years ago, TeXExchange users *[Ulrike Fischer](http://tex.stackexchange.com/users/2388/ulrike-fischer)* and *[topskip](http://tex.stackexchange.com/users/243/topskip)* came up with a ~~shell~~ **[script](http://tex.stackexchange.com/a/30656)** that prints a complete list of font names that may be used with LuaLaTeX. 

Unfortunately the script had stopped working with newer versions of LuaLaTeX (such as is included in TeXLive 2014). 

I updated Ulrike's and topskip's script to work with TeXLive 2014:

```lua
#!/usr/bin/env texlua

kpse.set_program_name("listluatexfonts")

cachefile  = kpse.expand_var("$TEXMFVAR")  .. "/luatex-cache/generic/names/luaotfload-names.luc"
fontlist = dofile(cachefile)
assert(fontlist,"Could not load font name database")

local tmp = {}

for _,font in ipairs(fontlist.mappings) do
  tmp[#tmp + 1] = font.fontname
end
table.sort(tmp)

for _,fontname in ipairs(tmp) do
  print(fontname)
end
```

Copy above code, save it as **`listluatexfonts`** somewhere on your $PATH and make it executable (`chmod a+x listluatexfonts`).

If you have recently installed new fonts (or if you never did run a LuaLaTeX command before), then run this command first to update the font cache available to LuaLaTeX:

    luaotfload-tool -u -vvv

This will generate an updated cache of font names as *luaotfload-names.luc* in a path where it is found and can be used by LuaLaTeX.

```bash
kp@mbp:~> luaotfload-tool -u -vvv
 luaotfload | util : Setting log level
 luaotfload | util : Task completed successfully
 luaotfload | db : Updating the font names database.
 luaotfload | db : Font names database loaded
 luaotfload | db : Loading took 18 ms.
 luaotfload | db : Blacklisting 8 files and directories.
 luaotfload | db : Whitelisting 0 files.
 luaotfload | db : Scanning TEXMF fonts...
 luaotfload | db : Initiating scan of 401 directories.
 luaotfload | db : Scanned 1496 files, 0 new.
 luaotfload | db : Scanning OS fonts...
 luaotfload | db : Searching in static system directories...
 luaotfload | db : Scanned 610 files, 0 new.
 luaotfload | db : Scanned 0 font files; 0 new entries.
 luaotfload | db : Creating filename map.
 luaotfload | db : Analyzing families, sizes, and styles.
 luaotfload | db : Ordering design sizes.
 luaotfload | db : Rebuilt in 649 ms.
 luaotfload | db : Font index saved at ...
 luaotfload | db : Gzip: /Users/kp/.texlive2014/texmf-var/luatex-cache/generic/names/luaotfload-names.lua.gz
 luaotfload | db : Byte: /Users/kp/.texlive2014/texmf-var/luatex-cache/generic/names/luaotfload-names.luc
 luaotfload | cache : Lookup cache saved.
 luaotfload | cache : Lookup cache emptied.
 luaotfload | db : Fonts in the database: 1591
 luaotfload | util : Task completed successfully
```

Now run the above script:

    ./listluatexfonts


I did it as **`listluatexfonts | grep -i source`** and saw this list:

```python
sourcecodeproblack
sourcecodeprobold
sourcecodeproextralight
sourcecodeprolight
sourcecodepromedium
sourcecodeproregular
sourcecodeprosemibold
```

Uh-huh!

Now my Pandoc command also worked and it produced the outputs I wanted:

```bash
 for font in $(listluatexfonts | grep -i source | grep -i code); do
   pandoc                                          \
      --highlight-style=espresso                   \
      --latex-engine=lualatex                      \
       -V monofont="${font}"                       \
       -o fun-with-box-drawing---using-${font}.pdf \
        box-drawing-fun-with-adobe-source-code-pro-fonts.md
 done
``` 

It turned out that the box drawing I had used wasn't the best to really highlight the specific differences between the various Source Code font faces. A better one is this:

```noweb
 +×△▲△©━┭╱═══╲┮━©△▲△×+
╔══╩╬╩╡÷╱← 0 →╲÷╞╩╬╩══╗
╚═╗╭╫╮ợ╱┅┅┓┅┏┅┅╲ọ╭╫╮╔═╝
╒═╝╎║┊╱X╭─────╮X╲┊║╎╚═╕
││┐╎║┊╳╓╖ BOX ╥╖╳┊║╎┌││
└││╎║╰─╨╠═════╣╨─╯║╎││┘
│└│╞╝╔╗▞╟╱╲╳╱╲╢▚╔╗╚╡│┘│
╰─╥┧Y║║▞ ┳┳┳┳┳ ▚║║Y┟╥─╯
☐┌╢┗┛╠║▞┍╇╇Ø╇╇┑▚║╣┗┛╟┐☑
┃!╚══╝┎┐│━┵┼┶━│┌┒╚══╝!┃
┃└┐┼┃│┃╷└─────┘╷┃│┃┼┌┘┃
┃♪╿│▼▽┃╰───*───╯┃▽▼│╿√┃
╚╦╧╰─>┡ DRAWING ┩<─╯╧╦╝
╶I┉┉┉┭└┄┄┄┄┄┄┄┄┄┘┮┉┉┉I╴
╭──┬─┴/ SOU┬RCE \┴─┬──╮
┢━━┷━━━━╸CO┼DE╺━━━━┷━━┪
━━━┻╾╮←(┏┑PRO┍┓)→╭╼┻━━━
░▒▓▌─╯┯━┷└───┘┷━┯╰─▐▓▒░
z─────┘────¡────└─────z
```

Here is a side-by-side screenshot of the resulting PDFs produced by `pandoc` for Source Code *Black* and *ExtraLight* font faces:

![Side-by-side screenshot illustrating box drawing character differences for Source Code *Black* and *ExtraLight*](http://i.stack.imgur.com/p65Pi.jpg)