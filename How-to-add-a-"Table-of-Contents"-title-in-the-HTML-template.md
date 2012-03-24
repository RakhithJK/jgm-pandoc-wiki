> Using the --toc option when generating HTML generates
> the table of content without the phrase "Table of
> content". Is there a way to add such phrase before
> the table of content?

Allow me to point out that it's good that the TOC title
is not inserted by default in the standard template,
because the exact phrase needed depends on the language
of the document! If you want to insert a TOC title into
your default template and ever write documents in
different languages I'd recommend you to define a
variable to set the actual phrase.

In a terminal do

    pandoc -D html >$HOME/.pandoc/templates/default.html

or on Windows

    pandoc -D html >C:\Documents And Settings\USERNAME\Application Data\pandoc\templates\default.html

Then go to where this 'default.html' is and edit it.
Search for '$toc$' and change the lines around it

    $if(toc)$
    <div id="$idprefix$TOC">
    $toc$
    </div>
    $endif$

into

    $if(toc)$
    <div id="$idprefix$TOC">
    $if(toctitle)$
    <h2 id="$idprefix$toctitle">$toctitle$</h2>
    $endif$
    $toc$
    </div>
    $endif$

or if you want the toc title to always be there, and
only sometimes to be in a different language:

    $if(toc)$
    <div id="$idprefix$TOC">
    <h2 id="$idprefix$toctitle">$if(toctitle)$$toctitle$$else$Table of Contents$endif$</h2>
    $toc$
    </div>
    $endif$

with your most used language's translation of "Table of Contents".

Then you can use

    pandoc -s --toc -V toctitle:'Table des matières' 

or whatever. This is especially useful if a language
uses different forms of the phrase like "Table of
contents" or "Contents" depending on the style of the
document.

Since we included that 'id="$idprefix$toctitle"'
you can also style it with CSS if you want to.
