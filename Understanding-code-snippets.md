Used libraries from cabal desc: 
* parsec, 
* syb, containers, mtl, 
* filepath, directory, network, HTTP, 
* texmath, xml, citeproc-hs, json, tagsoup, highlighting-kate, blaze-html, blaze-markup    
* extensible-exceptions, 
* pandoc-types, 
* bytestring, text, base64-bytestring,  
* random, data-default, temporary, old-locale, time,   
* zlib, zip-archive, 
* process, 

According to the package reverse dependencies [reverse dependencies](http://packdeps.haskellers.com/reverse) list, syb, containers, mtl, network, time, bytestring are widely used, among others

in /pandoc.hs

`(ReqArg
  (\arg opt -> return opt { optReader = map toLower arg })
  "FORMAT")`

"FORMAT" isn't really documented in System.Console.GetOpt, but when looking at source of that module, you see that it is used in the usage message, when describing how to pass the argument for the given option. 

in System.Console.GetOpt, second example uses: 

`foldl (flip id) defaultOptions o`

flip id is confusing because I thought id takes one parameter... this [thread](http://www.haskell.org/pipermail/beginners/2011-March/006477.html) explains it.. as I understand, we have id:: a -> a, flip (d -> (e -> f)) -> (e -> d -> f), and the data this is being applied to has types defaultOptions:: Options and o:: Options -> Options, so we end with (flip id) :: (Options -> (Options -> Options)) -> Options where id was originally unified to (e -> f) -> (e -> f) and (flip id) ended up with (e -> (e->f) -> f) ...I think...

