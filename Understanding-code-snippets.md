Used libraries from cabal desc: 
* syb, containers, mtl, 
* filepath, temporary, directory, network, HTTP, zlib, zip-archive,  
* parsec, 
* xml, tagsoup, json, texmath, highlighting-kate, blaze-html, blaze-markup,     
* text, bytestring, base64-bytestring, 
* time, old-locale, data-default, process, random,   
* internal: citeproc-hs, pandoc-types, ...

According to the package reverse dependencies [reverse dependencies](http://packdeps.haskellers.com/reverse) list, syb, containers, mtl, network, time, bytestring are widely used, among others

in /pandoc.hs

`(ReqArg
  (\arg opt -> return opt { optReader = map toLower arg })
  "FORMAT")`

"FORMAT" isn't really documented in System.Console.GetOpt, but when looking at source of that module, you see that it is used in the usage message, when describing how to pass the argument for the given option. 

in System.Console.GetOpt, second example uses: 

`foldl (flip id) defaultOptions o`

flip id is confusing because I thought id takes one parameter... this [thread](http://www.haskell.org/pipermail/beginners/2011-March/006477.html) explains it.. The heart of it seems to thinking about (a->b)-> (a->b) as equivalent to (a->b)->a ->b, the latter allowing for flip to change parameters such that you have a->(a->b) ->b. 

in /pandoc.hs

`liftM (map UTF8.decodeArg) getArgs`

liftM takes a regular function a->b and makes its a function over monads, in this case IO monads, so (map UTF8.decodeArg) becomes IO [String] -> IO [String]

in /pandoc.hs

`tp <.> format`



in pandoc.hs
`
E.catch                                                                                                       (readDataFileUTF8 datadir ("templates" </> tp'))
   (\e' -> let _ = (e' ::E.SomeException) in throwIO e')      
`