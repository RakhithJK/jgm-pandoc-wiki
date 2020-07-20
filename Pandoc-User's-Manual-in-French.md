Synopsis
========

<span class="nowrap">`pandoc`</span> \[*options*\] \[*input-file*\]…

Description
===========

Pandoc est une bibliothèque [Haskell](https://www.haskell.org/) pour la
conversion d'un format de balisage à un autre, et un outil de ligne de
commande qui utilise cette bibliothèque.

Pandoc peut convertir entre de nombreux formats de balisage et de
traitement de texte, y compris, mais sans s'y limiter, diverses saveurs
de [Markdown](https://daringfireball.net/projects/markdown/),
[HTML](https://www.w3.org/html/),
[LaTeX](https://www.latex-project.org/) et [Word
docx](https://en.wikipedia.org/wiki/Office_Open_XML). Pour les listes
complètes des formats d'entrée et de sortie, voir les options
<a href="#option--from" class="option"><span class="nowrap"><code>--from</code></span></a>
et
<a href="#option--to" class="option"><span class="nowrap"><code>--to</code></span></a>
[ci-dessous](#general-options). Pandoc peut aussi produire du
[PDF](https://www.adobe.com/pdf/) en sortie : voir [Créer un PDF
ci-dessous](#creating-a-pdf).

La version améliorée de Markdown de Pandoc comprend la syntaxe pour [les
tables](#tables), [les listes de definitions](#definition-lists),
[metadata blocks](#metadata-blocks), [notes de bas de page](#footnotes),
[citations](#citations), [math](#math), et bien plus encore. Voir
ci-dessous sous [Pandoc’s Markdown](#pandocs-msarkdown).

Pandoc a une conception modulaire : il se compose d'un ensemble de
lecteurs, qui analysent le texte dans un format donné et produisent une
représentation native du document (un arbre syntaxique abstrait ou AST),
et d'un ensemble d' "auteurs", qui convertissent cette représentation
native en un format cible. Ainsi, l'ajout d'un format d'entrée ou de
sortie ne nécessite que l'ajout d'un lecteur ou d'un rédacteur. Les
utilisateurs peuvent également exécuter des [filtres
pandoc](https://pandoc.org/filters.html) pour modifier l'AST
intermédiaire.

Comme la représentation intermédiaire d'un document de pandoc est moins
expressive que de nombreux formats entre lesquels il est converti, il ne
faut pas s'attendre à des conversions parfaites entre chaque format et
tous les autres. Pandoc tente de préserver les éléments structurels d'un
document, mais pas les détails de formatage tels que la taille des
marges. Et certains éléments du document, tels que les tableaux
complexes, peuvent ne pas correspondre au modèle de document simple de
Pandoc. Alors que les conversions de Pandoc's Markdown vers tous les
formats aspirent à être parfaites, on peut s'attendre à ce que les
conversions de formats plus expressifs que Pandoc's Markdown soient
perdues.

Utiliser pandoc
---------------

Si on ne spécifie pas de *fichiers en entrée*, l'entrée est lue sur
*stdin*. La sortie par défaut est *stdout*. Pour envoyer la sortie vers
un fichier, utiliser l'option
<a href="#option--output" class="option"><span class="nowrap"><code>-o</code></span></a>
:

    pandoc -o output.html input.txt

Par défaut, pandoc produit un fragment de document. Pour produire un
document autonome (par exemple un fichier HTML valide comprenant <span
class="nowrap">`<head>`</span> et <span class="nowrap">`<body>`</span>),
utiliser l'option
<a href="#option--standalone" class="option"><span class="nowrap"><code>-s</code></span></a>
ou
<a href="#option--standalone" class="option"><span class="nowrap"><code>--standalone</code></span></a>
:

    pandoc -s -o output.html input.txt

Pour plus d'informations sur la manière dont les documents autonomes
sont produits, voir [Modèles](#templates) ci-dessous.

Si plusieurs fichiers d'entrée sont donnés, <span
class="nowrap">`pandoc`</span> les concaténera tous (avec des lignes
blanches entre eux) avant de les analyser (utiliser
<a href="#option--file-scope" class="option"><span class="nowrap"><code>--file-scope</code></span></a>
pour analyser les dossiers individuellement).

Spécification de formats
------------------------

Le format de l'entrée et de la sortie peut être spécifié explicitement à
l'aide d'options de ligne de commande. Le format d'entrée peut être
spécifié à l'aide de l'option
<a href="#option--from" class="option"><span class="nowrap"><code>-f/--from</code></span></a>
option, le format de sortie en utilisant l'option
<a href="#option--to" class="option"><span class="nowrap"><code>-t/--to</code></span></a>.
Ainsi, pour convertir <span class="nowrap">`hello.txt`</span> de
Markdown à LaTeX, vous pourriez taper :

    pandoc -f markdown -t latex hello.txt

Pour convertir <span class="nowrap">`hello.html`</span>de  HTML à
Markdown:

    pandoc -f html -t markdown hello.html

Les formats d'entrée et de sortie pris en charge sont énumérés dans les
options [Options](#options) (voir
<a href="#option--from" class="option"><span class="nowrap"><code>-f</code></span></a>
pour les formats d'entrée et
<a href="#option--to" class="option"><span class="nowrap"><code>-t</code></span></a>pour
les  formats en sortie). Vous avez aussi <span
class="nowrap">`pandoc                   --list-input-formats`</span> et
<span
class="nowrap">`pandoc                   --list-output-formats`</span>
pour imprimer les listes des formats pris en charge.

Si le format d'entrée ou de sortie n'est pas spécifié explicitement,
<span class="nowrap">`pandoc`</span> tentera de le deviner à partir des
extensions des noms de fichiers. Ainsi, par exemple,

    pandoc -o hello.tex hello.txt

convertira <span class="nowrap">`hello.txt`</span> de Markdown à LaTeX.
Si aucun fichier de sortie n'est spécifié (de sorte que la sortie passe
à *stdout*), ou si l'extension du fichier de sortie est inconnue, le
format de sortie sera par défaut HTML. Si aucun fichier d'entrée n'est
spécifié (de sorte que l'entrée provient de *stdin*), ou si les
extensions des fichiers d'entrée sont inconnues, le format d'entrée sera
supposé être Markdown.

Codage des caractères
---------------------

Pandoc utilise le codage de caractères UTF-8 pour l'entrée et la sortie.
Si votre codage de caractères local n'est pas UTF-8, vous devez faire
passer l'entrée et la sortie par [<span
class="nowrap">`iconv`</span>](https://www.gnu.org/software/libiconv/):

    iconv -t utf-8 input.txt | pandoc | iconv -f utf-8

Notez que dans certains formats de sortie (tels que HTML, LaTeX,
ConTeXt, RTF, OPML, DocBook et Texinfo), l'information sur le codage des
caractères est incluse dans l'en-tête du document, qui ne sera inclus
que si vous utilisez l'option
<a href="#option--standalone" class="option"><span class="nowrap"><code>-s/--standalone</code></span></a>.

Créer un PDF
------------

Pour produire un PDF, il faut spécifier un fichier de sortie avec une
extension <span class="nowrap">`.pdf`</span> :

    pandoc test.txt -o test.pdf

Par défaut, pandoc utilisera LaTeX pour créer le PDF, ce qui nécessite
l'installation d'un moteur LaTeX (voir
<a href="#option--pdf-engine" class="option"><span class="nowrap"><code>--pdf-engine</code></span></a>ci-dessous).
Sinon, pandoc peut utiliser ConTeXt, roff ms ou HTML comme format
intermédiaire. Pour ce faire, il faut spécifier un fichier de sortie
avec une extension <span class="nowrap">`.pdf`</span>, comme avant, mais
ajouter l'option
<a href="#option--pdf-engine" class="option"><span class="nowrap"><code>--pdf-engine</code></span></a>
ou
<a href="#option--to" class="option"><span class="nowrap"><code>-t                     context</code></span></a>,
<a href="#option--to" class="option"><span class="nowrap"><code>-t html</code></span></a>,
ou
<a href="#option--to" class="option"><span class="nowrap"><code>-t ms</code></span></a>à
la ligne de commande. L'outil utilisé pour générer le PDF à partir du
format intermédiaire peut être spécifié en utilisant
<a href="#option--pdf-engine" class="option"><span class="nowrap"><code>--pdf-engine</code></span></a>.

Vous pouvez contrôler le style PDF à l'aide de variables, en fonction du
format intermédiaire utilisé : voir [variables pour
LaTeX](#variables-for-latex), [variables for
ConTeXt](#variables-for-context), [variables for <span
class="nowrap">`wkhtmltopdf`</span>](#variables-for-wkhtmltopdf),
[variables for ms](#variables-for-ms). Lorsque le HTML est utilisé comme
format intermédiaire, la sortie peut être stylée en utilisant
<a href="#option--css" class="option"><span class="nowrap"><code>--css</code></span></a>.

Pour déboguer la création du PDF, il peut être utile d'examiner la
représentation intermédiaire : au lieu de
<a href="#option--output" class="option"><span class="nowrap"><code>-o test.pdf</code></span></a>,
utilisez par exemple
<a href="#option--standalone" class="option"><span class="nowrap"><code>-s -o test.tex</code></span></a>
pour exporter en LaTeX. Vous pouvez ensuite lancer la commande <span
class="nowrap">`pdflatex                   test.tex`</span>.

Lorsque vous utilisez LaTeX, les paquets suivants doivent être
disponibles (ils sont inclus dans toutes les versions récentes de [TeX
Live](https://www.tug.org/texlive/)): [<span
class="nowrap">`amsfonts`</span>](https://ctan.org/pkg/amsfonts), [<span
class="nowrap">`amsmath`</span>](https://ctan.org/pkg/amsmath), [<span
class="nowrap">`lm`</span>](https://ctan.org/pkg/lm), [<span
class="nowrap">`unicode-math`</span>](https://ctan.org/pkg/unicode-math),
[<span class="nowrap">`ifxetex`</span>](https://ctan.org/pkg/ifxetex),
[<span class="nowrap">`ifluatex`</span>](https://ctan.org/pkg/ifluatex),
[<span class="nowrap">`listings`</span>](https://ctan.org/pkg/listings)
(si l'option
<a href="#option--listings" class="option"><span class="nowrap"><code>--listings</code></span></a>
est utilisée), [<span
class="nowrap">`fancyvrb`</span>](https://ctan.org/pkg/fancyvrb), [<span
class="nowrap">`longtable`</span>](https://ctan.org/pkg/longtable),
[<span class="nowrap">`booktabs`</span>](https://ctan.org/pkg/booktabs),
[<span class="nowrap">`graphicx`</span>](https://ctan.org/pkg/graphicx)
(si le document contient des images), [<span
class="nowrap">`hyperref`</span>](https://ctan.org/pkg/hyperref), [<span
class="nowrap">`xcolor`</span>](https://ctan.org/pkg/xcolor), [<span
class="nowrap">`ulem`</span>](https://ctan.org/pkg/ulem), [<span
class="nowrap">`geometry`</span>](https://ctan.org/pkg/geometry) (avec
la variable <span class="nowrap">`geometry`</span> positionnée), [<span
class="nowrap">`setspace`</span>](https://ctan.org/pkg/setspace) (avec
<span class="nowrap">`linestretch`</span>), et [<span
class="nowrap">`babel`</span>](https://ctan.org/pkg/babel) (avec <span
class="nowrap">`lang`</span>). L'usage de <span
class="nowrap">`xelatex`</span> ou de <span
class="nowrap">`lualatex`</span> comme moteur PDF nécessite [<span
class="nowrap">`fontspec`</span>](https://ctan.org/pkg/fontspec). <span
class="nowrap">`xelatex`</span>utilise [<span
class="nowrap">`polyglossia`</span>](https://ctan.org/pkg/polyglossia)
(avec <span class="nowrap">`lang`</span>), [<span
class="nowrap">`xecjk`</span>](https://ctan.org/pkg/xecjk), et [<span
class="nowrap">`bidi`</span>](https://ctan.org/pkg/bidi) (avec la
variable<span class="nowrap">`dir`</span> positionnée). Si la variable
<span class="nowrap">`mathspec`</span>est positionnée, <span
class="nowrap">`xelatex`</span> utilisera [<span
class="nowrap">`mathspec`</span>](https://ctan.org/pkg/mathspec) au lieu
de [<span
class="nowrap">`unicode-math`</span>](https://ctan.org/pkg/unicode-math).
Les paquages [<span
class="nowrap">`upquote`</span>](https://ctan.org/pkg/upquote)et [<span
class="nowrap">`microtype`</span>](https://ctan.org/pkg/microtype) sont
utilisés si disponibles, et [<span
class="nowrap">`csquotes`</span>](https://ctan.org/pkg/csquotes) sera
utilisé pour la [typographie](#typography) si la variable<span
class="nowrap">`csquotes`</span>ou le champ metadata est positionné à
Vrai. Les paquages [<span
class="nowrap">`natbib`</span>](https://ctan.org/pkg/natbib), [<span
class="nowrap">`biblatex`</span>](https://ctan.org/pkg/biblatex), [<span
class="nowrap">`bibtex`</span>](https://ctan.org/pkg/bibtex), et [<span
class="nowrap">`biber`</span>](https://ctan.org/pkg/biber) peuvent être
utilisés en option pour [le rendu des citations](#citation-rendering).
Les paquets suivants seront utilisés pour améliorer la qualité de la
production s'ils sont présents, mais pandoc n'exige pas leur présence :
[<span class="nowrap">`upquote`</span>](https://ctan.org/pkg/upquote)
(pour des citations directes dans des environnements verbatim), [<span
class="nowrap">`microtype`</span>](https://ctan.org/pkg/microtype) (pour
de meilleurs ajustements d'espacement), [<span
class="nowrap">`parskip`</span>](https://ctan.org/pkg/parskip) (pour de
meilleurs espaces interparagraphe), [<span
class="nowrap">`xurl`</span>](https://ctan.org/pkg/xurl) (pour de
meilleurs sauts de ligne dans les URL), [<span
class="nowrap">`bookmark`</span>](https://ctan.org/pkg/bookmark) (pour
de meilleurs signets PDF), et [<span
class="nowrap">`footnotehyper`</span>](https://ctan.org/pkg/footnotehyper)
ou [<span
class="nowrap">`footnote`</span>](https://ctan.org/pkg/footnote) (pour
permettre les notes de bas de page dans les tableaux).

Lecture sur Internet
--------------------

Au lieu d'un fichier d'entrée, un URI absolu peut être donné. Dans ce
cas, pandoc ira chercher le contenu en utilisant HTTP :

    pandoc -f html -t markdown https://www.fsf.org

Il est possible de fournir une chaîne User-Agent personnalisée ou un
autre en-tête lorsque l'on demande un document à partir d'une URL :

    pandoc -f html -t markdown --request-header User-Agent:"Mozilla/5.0" \
      https://www.fsf.org

Options
=======

Options générales
-----------------

<span id="option--from" class="option-anchor"><span class="nowrap">`-f`</span> *FORMAT*, <span class="nowrap">`-r`</span> *FORMAT*, <span class="nowrap">`--from=`</span>*FORMAT*, <span class="nowrap">`--read=`</span>*FORMAT*</span>  
Specify input format. *FORMAT* can be:

<div id="input-formats">

-   <span class="nowrap">`commonmark`</span>
    ([CommonMark](https://commonmark.org/) Markdown)
-   <span class="nowrap">`creole`</span> ([Creole
    1.0](http://www.wikicreole.org/wiki/Creole1.0))
-   <span class="nowrap">`csv`</span>
    ([CSV](https://tools.ietf.org/html/rfc4180) table)
-   <span class="nowrap">`docbook`</span>
    ([DocBook](https://docbook.org/))
-   <span class="nowrap">`docx`</span> ([Word
    docx](https://en.wikipedia.org/wiki/Office_Open_XML))
-   <span class="nowrap">`dokuwiki`</span> ([DokuWiki
    markup](https://www.dokuwiki.org/dokuwiki))
-   <span class="nowrap">`epub`</span> ([EPUB](http://idpf.org/epub))
-   <span class="nowrap">`fb2`</span>
    ([FictionBook2](http://www.fictionbook.org/index.php/Eng:XML_Schema_Fictionbook_2.1)
    e-book)
-   <span class="nowrap">`gfm`</span> ([GitHub-Flavored
    Markdown](https://help.github.com/articles/github-flavored-markdown/)),
    or the deprecated and less accurate <span
    class="nowrap">`markdown_github`</span>; use [<span
    class="nowrap">`markdown_github`</span>](#markdown-variants) only if
    you need extensions not supported in [<span
    class="nowrap">`gfm`</span>](#markdown-variants).
-   <span class="nowrap">`haddock`</span> ([Haddock
    markup](https://www.haskell.org/haddock/doc/html/ch03s08.html))
-   <span class="nowrap">`html`</span>
    ([HTML](https://www.w3.org/html/))
-   <span class="nowrap">`ipynb`</span> ([Jupyter
    notebook](https://nbformat.readthedocs.io/en/latest/))
-   <span class="nowrap">`jats`</span>
    ([JATS](https://jats.nlm.nih.gov/) XML)
-   <span class="nowrap">`jira`</span>
    ([Jira](https://jira.atlassian.com/secure/WikiRendererHelpAction.jspa?section=all)
    wiki markup)
-   <span class="nowrap">`json`</span> (JSON version of native AST)
-   <span class="nowrap">`latex`</span>
    ([LaTeX](https://www.latex-project.org/))
-   <span class="nowrap">`markdown`</span> ([Pandoc’s
    Markdown](#pandocs-markdown))
-   <span class="nowrap">`markdown_mmd`</span>
    ([MultiMarkdown](https://fletcherpenney.net/multimarkdown/))
-   <span class="nowrap">`markdown_phpextra`</span> ([PHP Markdown
    Extra](https://michelf.ca/projects/php-markdown/extra/))
-   <span class="nowrap">`markdown_strict`</span> (original unextended
    [Markdown](https://daringfireball.net/projects/markdown/))
-   <span class="nowrap">`mediawiki`</span> ([MediaWiki
    markup](https://www.mediawiki.org/wiki/Help:Formatting))
-   <span class="nowrap">`man`</span> ([roff
    man](https://man.cx/groff_man%287%29))
-   <span class="nowrap">`muse`</span>
    ([Muse](https://amusewiki.org/library/manual))
-   <span class="nowrap">`native`</span> (native Haskell)
-   <span class="nowrap">`odt`</span>
    ([ODT](https://en.wikipedia.org/wiki/OpenDocument))
-   <span class="nowrap">`opml`</span>
    ([OPML](http://dev.opml.org/spec2.html))
-   <span class="nowrap">`org`</span> ([Emacs Org
    mode](https://orgmode.org/))
-   <span class="nowrap">`rst`</span>
    ([reStructuredText](https://docutils.sourceforge.io/docs/ref/rst/introduction.html))
-   <span class="nowrap">`t2t`</span>
    ([txt2tags](https://txt2tags.org/))
-   <span class="nowrap">`textile`</span>
    ([Textile](https://www.promptworks.com/textile))
-   <span class="nowrap">`tikiwiki`</span> ([TikiWiki
    markup](https://doc.tiki.org/Wiki-Syntax-Text#The_Markup_Language_Wiki-Syntax))
-   <span class="nowrap">`twiki`</span> ([TWiki
    markup](https://twiki.org/cgi-bin/view/TWiki/TextFormattingRules))
-   <span class="nowrap">`vimwiki`</span>
    ([Vimwiki](https://vimwiki.github.io/))

</div>

Les extensions peuvent être activées ou désactivées individuellement en
annexant <span class="nowrap">`+EXTENSION`</span> ou <span
class="nowrap">`-EXTENSION`</span> au nom du format. Voir ci-dessous
[Extensions](#extensions), pour une liste des extensions et de leurs
noms. Voir
<a href="#option--list-input-formats" class="option"><span class="nowrap"><code>--list-input-formats</code></span></a>
et 
<a href="#option--list-extensions" class="option"><span class="nowrap"><code>--list-extensions</code></span></a>,
ci-dessous.

<span id="option--to" class="option-anchor"><span class="nowrap">`-t`</span> *FORMAT*, <span class="nowrap">`-w`</span> *FORMAT*, <span class="nowrap">`--to=`</span>*FORMAT*, <span class="nowrap">`--write=`</span>*FORMAT*</span>  
Specify output format. *FORMAT* can be:

<div id="output-formats">

-   <span class="nowrap">`asciidoc`</span>
    ([AsciiDoc](https://www.methods.co.nz/asciidoc/)) or <span
    class="nowrap">`asciidoctor`</span>
    ([AsciiDoctor](https://asciidoctor.org/))
-   <span class="nowrap">`beamer`</span> ([LaTeX
    beamer](https://ctan.org/pkg/beamer) slide show)
-   <span class="nowrap">`commonmark`</span>
    ([CommonMark](https://commonmark.org/) Markdown)
-   <span class="nowrap">`context`</span>
    ([ConTeXt](https://www.contextgarden.net/))
-   <span class="nowrap">`docbook`</span> or <span
    class="nowrap">`docbook4`</span> ([DocBook](https://docbook.org/) 4)
-   <span class="nowrap">`docbook5`</span> (DocBook 5)
-   <span class="nowrap">`docx`</span> ([Word
    docx](https://en.wikipedia.org/wiki/Office_Open_XML))
-   <span class="nowrap">`dokuwiki`</span> ([DokuWiki
    markup](https://www.dokuwiki.org/dokuwiki))
-   <span class="nowrap">`epub`</span> or <span
    class="nowrap">`epub3`</span> ([EPUB](http://idpf.org/epub) v3 book)
-   <span class="nowrap">`epub2`</span> (EPUB v2)
-   <span class="nowrap">`fb2`</span>
    ([FictionBook2](http://www.fictionbook.org/index.php/Eng:XML_Schema_Fictionbook_2.1)
    e-book)
-   <span class="nowrap">`gfm`</span> ([GitHub-Flavored
    Markdown](https://help.github.com/articles/github-flavored-markdown/)),
    or the deprecated and less accurate <span
    class="nowrap">`markdown_github`</span>; use [<span
    class="nowrap">`markdown_github`</span>](#markdown-variants) only if
    you need extensions not supported in [<span
    class="nowrap">`gfm`</span>](#markdown-variants).
-   <span class="nowrap">`haddock`</span> ([Haddock
    markup](https://www.haskell.org/haddock/doc/html/ch03s08.html))
-   <span class="nowrap">`html`</span> or <span
    class="nowrap">`html5`</span> ([HTML](https://www.w3.org/html/),
    i.e. [HTML5](https://html.spec.whatwg.org/)/XHTML [polyglot
    markup](https://www.w3.org/TR/html-polyglot/))
-   <span class="nowrap">`html4`</span>
    ([XHTML](https://www.w3.org/TR/xhtml1/) 1.0 Transitional)
-   <span class="nowrap">`icml`</span> ([InDesign
    ICML](https://wwwimages.adobe.com/www.adobe.com/content/dam/acom/en/devnet/indesign/sdk/cs6/idml/idml-cookbook.pdf))
-   <span class="nowrap">`ipynb`</span> ([Jupyter
    notebook](https://nbformat.readthedocs.io/en/latest/))
-   <span class="nowrap">`jats_archiving`</span>
    ([JATS](https://jats.nlm.nih.gov/) XML, Archiving and Interchange
    Tag Set)
-   <span class="nowrap">`jats_articleauthoring`</span>
    ([JATS](https://jats.nlm.nih.gov/) XML, Article Authoring Tag Set)
-   <span class="nowrap">`jats_publishing`</span>
    ([JATS](https://jats.nlm.nih.gov/) XML, Journal Publishing Tag Set)
-   <span class="nowrap">`jats`</span> (alias for <span
    class="nowrap">`jats_archiving`</span>)
-   <span class="nowrap">`jira`</span>
    ([Jira](https://jira.atlassian.com/secure/WikiRendererHelpAction.jspa?section=all)
    wiki markup)
-   <span class="nowrap">`json`</span> (JSON version of native AST)
-   <span class="nowrap">`latex`</span>
    ([LaTeX](https://www.latex-project.org/))
-   <span class="nowrap">`man`</span> ([roff
    man](https://man.cx/groff_man%287%29))
-   <span class="nowrap">`markdown`</span> ([Pandoc’s
    Markdown](#pandocs-markdown))
-   <span class="nowrap">`markdown_mmd`</span>
    ([MultiMarkdown](https://fletcherpenney.net/multimarkdown/))
-   <span class="nowrap">`markdown_phpextra`</span> ([PHP Markdown
    Extra](https://michelf.ca/projects/php-markdown/extra/))
-   <span class="nowrap">`markdown_strict`</span> (original unextended
    [Markdown](https://daringfireball.net/projects/markdown/))
-   <span class="nowrap">`mediawiki`</span> ([MediaWiki
    markup](https://www.mediawiki.org/wiki/Help:Formatting))
-   <span class="nowrap">`ms`</span> ([roff
    ms](https://man.cx/groff_ms%287%29))
-   <span class="nowrap">`muse`</span>
    ([Muse](https://amusewiki.org/library/manual)),
-   <span class="nowrap">`native`</span> (native Haskell),
-   <span class="nowrap">`odt`</span> ([OpenOffice text
    document](https://en.wikipedia.org/wiki/OpenDocument))
-   <span class="nowrap">`opml`</span>
    ([OPML](http://dev.opml.org/spec2.html))
-   <span class="nowrap">`opendocument`</span>
    ([OpenDocument](http://opendocument.xml.org/))
-   <span class="nowrap">`org`</span> ([Emacs Org
    mode](https://orgmode.org/))
-   <span class="nowrap">`pdf`</span>
    ([PDF](https://www.adobe.com/pdf/))
-   <span class="nowrap">`plain`</span> (plain text),
-   <span class="nowrap">`pptx`</span>
    ([PowerPoint](https://en.wikipedia.org/wiki/Microsoft_PowerPoint)
    slide show)
-   <span class="nowrap">`rst`</span>
    ([reStructuredText](https://docutils.sourceforge.io/docs/ref/rst/introduction.html))
-   <span class="nowrap">`rtf`</span> ([Rich Text
    Format](https://en.wikipedia.org/wiki/Rich_Text_Format))
-   <span class="nowrap">`texinfo`</span> ([GNU
    Texinfo](https://www.gnu.org/software/texinfo/))
-   <span class="nowrap">`textile`</span>
    ([Textile](https://www.promptworks.com/textile))
-   <span class="nowrap">`slideous`</span>
    ([Slideous](https://goessner.net/articles/slideous/) HTML and
    JavaScript slide show)
-   <span class="nowrap">`slidy`</span>
    ([Slidy](https://www.w3.org/Talks/Tools/Slidy2/) HTML and JavaScript
    slide show)
-   <span class="nowrap">`dzslides`</span>
    ([DZSlides](http://paulrouget.com/dzslides/) HTML5 + JavaScript
    slide show),
-   <span class="nowrap">`revealjs`</span>
    ([reveal.js](https://revealjs.com/) HTML5 + JavaScript slide show)
-   <span class="nowrap">`s5`</span>
    ([S5](https://meyerweb.com/eric/tools/s5/) HTML and JavaScript slide
    show)
-   <span class="nowrap">`tei`</span> ([TEI
    Simple](https://github.com/TEIC/TEI-Simple))
-   <span class="nowrap">`xwiki`</span> ([XWiki
    markup](https://www.xwiki.org/xwiki/bin/view/Documentation/UserGuide/Features/XWikiSyntax/))
-   <span class="nowrap">`zimwiki`</span> ([ZimWiki
    markup](https://zim-wiki.org/manual/Help/Wiki_Syntax.html))
-   the path of a custom Lua writer, see [Custom
    writers](#custom-writers) below

</div>

Notez que les sorties <span class="nowrap">`odt`</span>, <span
class="nowrap">`docx`</span>, <span class="nowrap">`epub`</span>et, 
<span class="nowrap">`pdf`</span> ne sont pas dirigées vers *stdout* à
moins que vous le forciez avec
<a href="#option--output" class="option"><span class="nowrap"><code>-o                         -</code></span></a>.

Les extensions peuvent être activées ou désactivées individuellement en
ajoutant <span class="nowrap">`+EXTENSION`</span> ou <span
class="nowrap">`-EXTENSION`</span> au nom de format. Voir
[Extensions](#extensions) ci-dessous, pour une liste des extensions et
de leurs noms. Voir
<a href="#option--list-output-formats" class="option"><span class="nowrap"><code>--list-output-formats</code></span></a>
et 
<a href="#option--list-extensions" class="option"><span class="nowrap"><code>--list-extensions</code></span></a>ci-dessous,

<span id="option--output" class="option-anchor"><span class="nowrap">`-o`</span> *FILE*, <span class="nowrap">`--output=`</span>*FILE*</span>  
Ecrire la sortie vers *FILE* au lieu de *stdout*. Si *FILE* est <span
class="nowrap">`-`</span>, la sortie sera *stdout*, même si un format
non textuel (<span class="nowrap">`docx`</span>, <span
class="nowrap">`odt`</span>, <span class="nowrap">`epub2`</span>, <span
class="nowrap">`epub3`</span>) est spécifié.

<span id="option--data-dir" class="option-anchor"><span class="nowrap">`--data-dir=`</span>*DIRECTORY*</span>  
Indiquez le répertoire de données de l'utilisateur pour rechercher des
fichiers de données pandoc. Si cette option n'est pas spécifiée, le
répertoire de données utilisateur par défaut sera utilisé. Sur les
systèmes \*nix et macOS ce sera le sous-répertoire <span
class="nowrap">`pandoc`</span> du répertoire de données XDG (par défaut,
<span class="nowrap">`$HOME/.local/share`</span>, qu'on peut changer en
modifiant la variable d'environnement <span
class="nowrap">`XDG_DATA_HOME`</span>. Si ce répertoire n'existe pas,
<span class="nowrap">`$HOME/.pandoc`</span> sera utilisé (compatibilité
ascendante). Dans Windows, le répertoire de données de l'utilisateur par
défaut est <span
class="nowrap">`C:\Users\USERNAME\AppData\Roaming\pandoc`</span>. Vous
pouvez trouver le répertoire des données de l'utilisateur par défaut sur
votre système en consultant la sortie de <span
class="nowrap">`pandoc --version`</span>. Un répertoire <span
class="nowrap">`reference.odt`</span>, <span
class="nowrap">`reference.docx`</span>, <span
class="nowrap">`epub.css`</span>, <span
class="nowrap">`templates`</span>, <span class="nowrap">`slidy`</span>,
<span class="nowrap">`slideous`</span>ou,  <span
class="nowrap">`s5`</span> placé dans ce répertoire remplacera les
valeurs par défaut de pandoc.

<span id="option--defaults" class="option-anchor"><span class="nowrap">`-d`</span> *FILE*, <span class="nowrap">`--defaults=`</span>*FILE*</span>  
Indiquez un ensemble de paramètres d'options par défaut. FILE est un
fichier YAML dont les champs correspondent aux paramètres d'option de la
ligne de commande. Toutes les options de conversion de documents, y
compris les fichiers d'entrée et de sortie, peuvent être définies à
l'aide d'un fichier par défaut. Le fichier sera d'abord recherché dans
le répertoire de travail, puis dans le sous-répertoire <span
class="nowrap">`defaults`</span> du répertoire des données utilisateur
(voir
<a href="#option--data-dir" class="option"><span class="nowrap"><code>--data-dir</code></span></a>).
L'extension<span class="nowrap">`.yaml`</span> peut être omise. Voir la
section [Default files](#default-files) pour plus d'informations sur le
format du fichier. Les paramètres du fichier par défaut peuvent être
écrasés ou étendus par des options ultérieures sur la ligne de commande.

<span id="option--bash-completion" class="option-anchor"><span class="nowrap">`--bash-completion`</span></span>  
Generate a bash completion script. To enable bash completion with
pandoc, add this to your <span class="nowrap">`.bashrc`</span>:

    eval "$(pandoc --bash-completion)"

<span id="option--verbose" class="option-anchor"><span class="nowrap">`--verbose`</span></span>  
Donne une sortie de débogage bavarde. Actuellement, cela n'a d'effet
qu'avec une sortie PDF.

<span id="option--quiet" class="option-anchor"><span class="nowrap">`--quiet`</span></span>  
Supprimer les messages d'avertissement.

<span id="option--fail-if-warnings" class="option-anchor"><span class="nowrap">`--fail-if-warnings`</span></span>  
Sortir avec le statut d'erreur s'il y a des avertissements.

<span id="option--log" class="option-anchor"><span class="nowrap">`--log=`</span>*FILE*</span>  
Inscrire dans le FICHIER les messages de log au format JSON lisible par
la machine. Tous les messages dépassant le niveau DEBUG seront écrits,
quels que soient les paramètres de verbosité
(<a href="#option--verbose" class="option"><span class="nowrap"><code>--verbose</code></span></a>,
<a href="#option--quiet" class="option"><span class="nowrap"><code>--quiet</code></span></a>).

<span id="option--list-input-formats" class="option-anchor"><span class="nowrap">`--list-input-formats`</span></span>  
Liste les formats d'entrée pris en charge, un par ligne.

<span id="option--list-output-formats" class="option-anchor"><span class="nowrap">`--list-output-formats`</span></span>  
Liste les formats de sortie pris en charge, un par ligne.

<span id="option--list-extensions" class="option-anchor"><span class="nowrap">`--list-extensions`</span>\[<span class="nowrap">`=`</span>*FORMAT*\]</span>  
Liste des extensions prises en charge pour FORMAT, une par ligne,
précédée d'un + ou d'un - indiquant si elle est activée par défaut dans
FORMAT. Si FORMAT n'est pas spécifié, les valeurs par défaut pour le
Markdown de pandoc sont données.

<span id="option--list-highlight-languages" class="option-anchor"><span class="nowrap">`--list-highlight-languages`</span></span>  
Liste les langues prises en charge pour la coloration syntaxique, une
par ligne.

<span id="option--list-highlight-styles" class="option-anchor"><span class="nowrap">`--list-highlight-styles`</span></span>  
Liste les styles pris en charge pour la coloration syntaxique, un par
ligne. Voir
<a href="#option--highlight-style" class="option"><span class="nowrap"><code>--highlight-style</code></span></a>.

<span id="option--version" class="option-anchor"><span class="nowrap">`-v`</span>, <span class="nowrap">`--version`</span></span>  
Imprimer la version.

<span id="option--help" class="option-anchor"><span class="nowrap">`-h`</span>, <span class="nowrap">`--help`</span></span>  
Afficher l'aide.

Options du Lecteur ("Reader")
-----------------------------

<span id="option--shift-heading-level-by" class="option-anchor"><span class="nowrap">`--shift-heading-level-by=`</span>*NUMBER*</span>  
Décalage des niveaux de titres par un nombre entier positif ou négatif.
Par exemple, avec
<a href="#option--shift-heading-level-by" class="option"><span class="nowrap"><code>--shift-heading-level-by=-1</code></span></a>
les rubriques de niveau 2 deviennent des rubriques de niveau 1, et les
rubriques de niveau 3 deviennent des rubriques de niveau 2. Les titres
ne peuvent pas avoir un niveau inférieur à 1, de sorte qu'un titre qui
serait déplacé en dessous du niveau 1 devient un paragraphe ordinaire.
Exception : avec un décalage de -N, un titre de niveau N au début du
document remplace le titre des métadonnées. 
<a href="#option--shift-heading-level-by" class="option"><span class="nowrap"><code>--shift-heading-level-by=-1</code></span></a>
is a good choice when converting HTML or Markdown documents that use an
initial level-1 heading for the document title and level-2+ headings for
sections.
<a href="#option--shift-heading-level-by" class="option"><span class="nowrap"><code>--shift-heading-level-by=1</code></span></a>
may be a good choice for converting Markdown documents that use level-1
headings for sections to HTML, since pandoc uses a level-1 heading to
render the document title.

<span id="option--base-header-level" class="option-anchor"><span class="nowrap">`--base-header-level=`</span>*NUMBER*</span>  
*Deprecated. Use
<a href="#option--shift-heading-level-by" class="option"><span class="nowrap"><code>--shift-heading-level-by</code></span></a>=X
instead, where X = NUMBER - 1.* Specify the base level for headings
(defaults to 1).

<span id="option--strip-empty-paragraphs" class="option-anchor"><span class="nowrap">`--strip-empty-paragraphs`</span></span>  
*Deprecated. Use the <span class="nowrap">`+empty_paragraphs`</span>
extension instead.* Ignore paragraphs with no content. This option is
useful for converting word processing documents where users have used
empty paragraphs to create inter-paragraph space.

<span id="option--indented-code-classes" class="option-anchor"><span class="nowrap">`--indented-code-classes=`</span>*CLASSES*</span>  
Préciser les classes à utiliser pour les blocs de code indentés, par
exemple, <span class="nowrap">`perl,numberLines`</span> ou <span
class="nowrap">`haskell`</span>. Des classes multiples peuvent être
séparées par des espaces ou des virgules.

<span id="option--default-image-extension" class="option-anchor"><span class="nowrap">`--default-image-extension=`</span>*EXTENSION*</span>  
Indiquez une extension par défaut à utiliser lorsque les chemins
d'images/URL n'ont pas d'extension. Cela vous permet d'utiliser la même
source pour les formats qui nécessitent différents types d'images.
Actuellement, cette option ne concerne que les lecteurs Markdown et
LaTeX.

<span id="option--file-scope" class="option-anchor"><span class="nowrap">`--file-scope`</span></span>  
Analyser chaque fichier individuellement avant de les combiner pour des
documents de plusieurs fichiers. Cela permettra aux notes de bas de page
des différents fichiers ayant les mêmes identifiants de fonctionner
comme prévu. Si cette option est activée, les notes de bas de page et
les liens ne fonctionneront pas d'un fichier à l'autre. La lecture de
fichiers binaires (docx, odt, epub) implique
<a href="#option--file-scope" class="option"><span class="nowrap"><code>--file-scope</code></span></a>.

<span id="option--filter" class="option-anchor"><span class="nowrap">`-F`</span> *PROGRAM*, <span class="nowrap">`--filter=`</span>*PROGRAM*</span>  
Spécifier un exécutable à utiliser comme filtre transformant le pandoc
AST après que l'entrée soit analysée et avant que la sortie soit écrite.
L'exécutable doit lire JSON à partir de stdin et écrire JSON dans
stdout. La JSON doit être formatée comme l'entrée et la sortie de la
JSON du pandoc. Le nom du format de sortie sera transmis au filtre en
tant que premier argument. De ce fait,

    pandoc --filter ./caps.py -t latex

est equivalent à

    pandoc -t json | ./caps.py latex | pandoc -f json -t latex

Cette dernière forme peut être utile pour le débogage des filtres.  
  
Les filtres peuvent être écrits dans n'importe quelle langue. <span
class="nowrap">`Text.Pandoc.JSON`</span> exporte <span
class="nowrap">`toJSONFilter`</span> pour faciliter l'écriture des
filtres dans Haskell. Ceux qui préfèrent écrire des filtres en python
peuvent utiliser le module [<span
class="nowrap">`pandocfilters`</span>](https://github.com/jgm/pandocfilters),
installable à partir de PyPI. Il existe également des bibliothèques de
filtres pandoc dans [PHP](https://github.com/vinai/pandocfilters-php),
[perl](https://metacpan.org/pod/Pandoc::Filter)et, 
[JavaScript/node.js](https://github.com/mvhenderson/pandoc-filter-node).

Par ordre de préférence, pandoc cherchera des filtres dans

1.  un chemin complet ou relatif spécifié (exécutable ou non exécutable)

2.  <span class="nowrap">`$DATADIR/filters`</span> (exécutable ou non
    exécutable) où <span class="nowrap">`$DATADIR`</span> est le
    répertoire des données des utilisateurs (voir 
    <a href="#option--data-dir" class="option"><span class="nowrap"><code>--data-dir</code></span></a>ci-dessus,
    ).

3.  <span class="nowrap">`$PATH`</span> (exécutable uniquement)

Les filtres et les Lua-filtres sont appliqués dans l'ordre indiqué sur
la ligne de commande.

<span id="option--lua-filter" class="option-anchor"><span class="nowrap">`-L`</span> *SCRIPT*, <span class="nowrap">`--lua-filter=`</span>*SCRIPT*</span>  
Transformer le document de manière similaire aux filtres JSON (voir
<a href="#option--filter" class="option"><span class="nowrap"><code>--filter</code></span></a>),
mais en utilisant le système de filtrage Lua intégré à pandoc. Le script
Lua donné devrait renvoyer une liste de filtres Lua qui seront appliqués
dans l'ordre. Chaque filtre Lua doit contenir des fonctions de
transformation d'éléments indexées par le nom de l'élément AST sur
lequel la fonction de filtrage doit être appliquée.

Le module Lua de <span class="nowrap">`pandoc`</span> fournit des
fonctions d'aide pour la création d'éléments. Il est toujours chargé
dans l'environnement du script Lua.

Voici un exemple de script Lua pour la macro-expansion :

    function expand_hello_world(inline)
      if inline.c == '{{helloworld}}' then
        return pandoc.Emph{ pandoc.Str "Hello, World" }
      else
        return inline
      end
    end

    return {{Str = expand_hello_world}}

Par ordre de préférence, pandoc cherchera des filtres Lua dans

1.  un chemin complet ou relatif spécifié (exécutable ou non exécutable)

2.  <span class="nowrap">`$DATADIR/filters`</span> (exécutable ou non
    exécutable) où  <span class="nowrap">`$DATADIR`</span> est le
    répertoire des données des utilisateurs (voir 
    <a href="#option--data-dir" class="option"><span class="nowrap"><code>--data-dir</code></span></a>ci-dessus,
    ).

<span id="option--metadata" class="option-anchor"><span class="nowrap">`-M`</span> *KEY*\[<span class="nowrap">`=`</span>*VAL*\], <span class="nowrap">`--metadata=`</span>*KEY*\[<span class="nowrap">`:`</span>*VAL*\]</span>  
Réglez le champ de métadonnées KEY sur la valeur VAL. Une valeur
spécifiée sur la ligne de commande remplace une valeur spécifiée dans le
document à l'aide de [YAML metadata
blocks](#extension-yaml_metadata_block). Les valeurs seront analysées
comme des valeurs booléennes ou des chaînes de caractères YAML. Si
aucune valeur n'est spécifiée, la valeur sera traitée comme booléenne
true. Comme
<a href="#option--variable" class="option"><span class="nowrap"><code>--variable</code></span></a>,
<a href="#option--metadata" class="option"><span class="nowrap"><code>--metadata</code></span></a>
entraîne la définition de variables de modèle. Mais contrairement à 
<a href="#option--variable" class="option"><span class="nowrap"><code>--variable</code></span></a>,
<a href="#option--metadata" class="option"><span class="nowrap"><code>--metadata</code></span></a>
affecte les métadonnées du document sous-jacent (qui est accessible à
partir de filtres et peut être imprimé dans certains formats de sortie)
et les valeurs des métadonnées seront échappées lorsqu'elles seront
insérées dans le modèle.

<span id="option--metadata-file" class="option-anchor"><span class="nowrap">`--metadata-file=`</span>*FILE*</span>  
Lire les meta-données depuis le fichier YAML fourni (ou JSON). Cette
option peut être utilisée avec chaque format d'entrée, mais les balises
de texte dans le fichier YAML seront toujours analysées en tant que
Markdown. En général, l'entrée sera traitée de la même manière que dans
[YAML metadata blocks](#extension-yaml_metadata_block). Cette option
peut être utilisée de manière répétée pour inclure plusieurs fichiers de
métadonnées ; les valeurs des fichiers spécifiés plus tard sur la ligne
de commande seront préférées à celles des fichiers précédents. Les
valeurs des métadonnées spécifiées à l'intérieur du document, ou en
utilisant
<a href="#option--metadata" class="option"><span class="nowrap"><code>-M</code></span></a>,
écrasent les valeurs spécifiées avec cette option.

<span id="option--preserve-tabs" class="option-anchor"><span class="nowrap">`-p`</span>, <span class="nowrap">`--preserve-tabs`</span></span>  
Conservez les tabulations au lieu de les convertir en espaces. (Par
défaut, pandoc convertit les tabulations en espaces avant d'analyser sa
saisie). Notez que cela n'affectera que les tabulations dans les portées
de code littéral et les blocs de code. Les tabulations dans le texte
normal sont toujours traitées comme des espaces.

<span id="option--tab-stop" class="option-anchor"><span class="nowrap">`--tab-stop=`</span>*NUMBER*</span>  
Précisez le nombre d'espaces par tabulation (par défaut, 4).

<span id="option--track-changes" class="option-anchor"><span class="nowrap">`--track-changes=accept`</span>\|<span class="nowrap">`reject`</span>\|<span class="nowrap">`all`</span></span>  
Précise ce qu'il faut faire des insertions, suppressions et commentaires
produits par la fonction "Suivi des modifications" de MS Word. <span
class="nowrap">`accept`</span> (par défaut), insère toutes les
insertions, et ignore toutes les suppressions . <span
class="nowrap">`reject`</span> insère toutes les suppressions et ignore
les insertions. Les deux <span class="nowrap">`accept`</span>et  <span
class="nowrap">`reject`</span> ignorent les commentaires. <span
class="nowrap">`all`</span> met en place des insertions, des
suppressions et des commentaires, enveloppés dans des "span" avec les
classes <span class="nowrap">`insertion`</span>, <span
class="nowrap">`deletion`</span>, <span
class="nowrap">`comment-start`</span>, et <span
class="nowrap">`comment-end`</span>, respectivement. L'auteur et le
moment du changement sont inclus. <span class="nowrap">`all`</span> est
utile pour le script : il n'accepte que les modifications d'un certain
réviseur, par exemple, ou avant une certaine date. Si un paragraphe est
inséré ou supprimé, <span class="nowrap">`track-changes=all`</span>
produces a span with the class <span
class="nowrap">`paragraph-insertion`</span>/<span
class="nowrap">`paragraph-deletion`</span> avant la rupture du
paragraphe concerné. Cette option ne concerne que le lecteur docx.

<span id="option--extract-media" class="option-anchor"><span class="nowrap">`--extract-media=`</span>*DIR*</span>  
Extraire les images et autres supports contenus dans le document source
ou liés à celui-ci vers le chemin DIR, en le créant si nécessaire, et
ajuster les références des images dans le document afin qu'elles
pointent vers les fichiers extraits. Si le format source est un
conteneur binaire (docx, epub ou odt), le média est extrait du conteneur
et les noms de fichiers originaux sont utilisés. Sinon, le support est
lu à partir du système de fichiers ou téléchargé, et les nouveaux noms
de fichiers sont construits sur la base des hachages SHA1 du contenu.

<span id="option--abbreviations" class="option-anchor"><span class="nowrap">`--abbreviations=`</span>*FILE*</span>  
Spécifie un fichier d'abréviations personnalisé, avec des abréviations,
une par ligne. Si cette option n'est pas spécifiée, pandoc lira le
fichier de données <span class="nowrap">`abbreviations`</span> du
répertoire des données de l'utilisateur ou se rabattre sur une valeur
par défaut du système. Pour voir la valeur par défaut du système,
utilisez  <span
class="nowrap">`pandoc                       --print-default-data-file=abbreviations`</span>.
La seule utilisation que fait pandoc de cette liste est dans le lecteur
Markdown. Les chaînes de caractères se terminant par un point qui se
trouvent dans cette liste seront suivies d'un espace insécable, de sorte
que le point ne produira pas d'espace de fin de phrase dans des formats
comme LaTeX.

Options générales d'écriture
----------------------------

<span id="option--standalone" class="option-anchor"><span class="nowrap">`-s`</span>, <span class="nowrap">`--standalone`</span></span>  
Produire une sortie avec un en-tête et un pied de page appropriés (par
exemple un fichier HTML, LaTeX, TEI ou RTF autonome, et non un
fragment). Cette option est définie automatiquement pour les sorties
<span class="nowrap">`pdf`</span>, <span class="nowrap">`epub`</span>,
<span class="nowrap">`epub3`</span>, <span class="nowrap">`fb2`</span>,
<span class="nowrap">`docx`</span>et,  <span
class="nowrap">`odt`</span>. Pour une sortie <span
class="nowrap">`native`</span>, cette option entraîne l'inclusion de
métadonnées ; dans le cas contraire, les métadonnées sont supprimées.

<span id="option--template" class="option-anchor"><span class="nowrap">`--template=`</span>*FILE*\|*URL*</span>  
Utiliser le fichier spécifié comme modèle personnalisé pour le document
généré. Implique
<a href="#option--standalone" class="option"><span class="nowrap"><code>--standalone</code></span></a>.
Voir [Templates](#templates), ci-dessous, pour une description de la
syntaxe du modèle. Si aucune extension n'est spécifiée, une extension
correspondant à l'auteur sera ajoutée, de sorte que
<a href="#option--template" class="option"><span class="nowrap"><code>--template=special</code></span></a>
utilise <span class="nowrap">`special.html`</span>pour les exports 
HTML. Si le modèle n'est pas trouvé, pandoc le recherchera dans le
répertoire <span class="nowrap">`templates`</span> du répertoire des
données utilisateur (voir
<a href="#option--data-dir" class="option"><span class="nowrap"><code>--data-dir</code></span></a>).
Si cette option n'est pas utilisée, un modèle par défaut approprié au
format de sortie sera utilisé (voir
<a href="#option--print-default-template" class="option"><span class="nowrap"><code>-D/--print-default-template</code></span></a>).

<span id="option--variable" class="option-anchor"><span class="nowrap">`-V`</span> *KEY*\[<span class="nowrap">`=`</span>*VAL*\], <span class="nowrap">`--variable=`</span>*KEY*\[<span class="nowrap">`:`</span>*VAL*\]</span>  
Définir la variable de modèle KEY à la valeur VAL lors du rendu du
document en mode autonome. Si aucune VAL n'est spécifiée, la clé recevra
la valeur <span class="nowrap">`true`</span>.

<span id="option--print-default-template" class="option-anchor"><span class="nowrap">`-D`</span> *FORMAT*, <span class="nowrap">`--print-default-template=`</span>*FORMAT*</span>  
Imprimez le modèle par défaut du système pour un FORMAT de sortie. (Voir
<a href="#option--to" class="option"><span class="nowrap"><code>-t</code></span></a>
pour obtenir une liste des FORMATS possibles). Les modèles dans le
répertoire des données utilisateur sont ignorés. Cette option peut être
utilisée avec
<a href="#option--output" class="option"><span class="nowrap"><code>-o</code></span></a>/<a href="#option--output" class="option"><span class="nowrap"><code>--output</code></span></a>
pour rediriger la sortie vers un fichier, mais
<a href="#option--output" class="option"><span class="nowrap"><code>-o</code></span></a>/<a href="#option--output" class="option"><span class="nowrap"><code>--output</code></span></a>
doit venir avant
<a href="#option--print-default-template" class="option"><span class="nowrap"><code>--print-default-template</code></span></a>
sur la ligne de commande.

Notez que certains des modèles par défaut utilisent des modèles
partiels, par exemple <span class="nowrap">`styles.html`</span>. Pour
imprimer les partiels, utilisez
<a href="#option--print-default-data-file" class="option"><span class="nowrap"><code>--print-default-data-file</code></span></a>:
par exemple,
<a href="#option--print-default-data-file" class="option"><span class="nowrap"><code>--print-default-data-file=templates/styles.html</code></span></a>.

<span id="option--print-default-data-file" class="option-anchor"><span class="nowrap">`--print-default-data-file=`</span>*FILE*</span>  
Imprimer un fichier de données par défaut du système. Les fichiers du
répertoire des données utilisateur sont ignorés. Cette option peut être
utilisée avec
<a href="#option--output" class="option"><span class="nowrap"><code>-o</code></span></a>/<a href="#option--output" class="option"><span class="nowrap"><code>--output</code></span></a>
pour rediriger la sortie vers un fichier, mais
<a href="#option--output" class="option"><span class="nowrap"><code>-o</code></span></a>/<a href="#option--output" class="option"><span class="nowrap"><code>--output</code></span></a>
doit venir avant
<a href="#option--print-default-data-file" class="option"><span class="nowrap"><code>--print-default-data-file</code></span></a>
sur la ligne de commande.

<span id="option--eol" class="option-anchor"><span class="nowrap">`--eol=crlf`</span>\|<span class="nowrap">`lf`</span>\|<span class="nowrap">`native`</span></span>  
Spécifier manuellement les fins de ligne : <span
class="nowrap">`crlf`</span> (Windows), <span class="nowrap">`lf`</span>
(macOS/Linux/UNIX), ou <span class="nowrap">`native`</span> (terminaison
de ligne appropriée au système d'exploitation sur lequel le pandoc est
exécuté). La valeur par défaut est  <span
class="nowrap">`native`</span>.

<span id="option--dpi" class="option-anchor"><span class="nowrap">`--dpi`</span>=*NUMBER*</span>  
Indiquez la valeur par défaut des ppp (points par pouce) pour la
conversion des pixels en pouces/centimètres et vice versa.
(Techniquement, le terme correct serait ppi : pixels par pouce.) La
valeur par défaut est 96dpi. Lorsque les images contiennent des
informations sur les ppp en interne, la valeur encodée est utilisée au
lieu de la valeur par défaut spécifiée par cette option.

<span id="option--wrap" class="option-anchor"><span class="nowrap">`--wrap=auto`</span>\|<span class="nowrap">`none`</span>\|<span class="nowrap">`preserve`</span></span>  
Déterminez comment le texte est encapsulé dans la sortie (le code
source, et non la version rendue). Avec <span
class="nowrap">`auto`</span> (valeur par défaut), pandoc tentera de
faire correspondre les lignes à la largeur de colonne spécifiée par
<a href="#option--columns" class="option"><span class="nowrap"><code>--columns</code></span></a>
(default 72). Avec <span class="nowrap">`none`</span>, pandoc ne mettra
pas du tout les lignes à la longueur. Avec <span
class="nowrap">`preserve`</span>, pandoc s'efforcera de préserver la
longueur des lignes du document source (c'est-à-dire que lorsqu'il y a
des nouvelles lignes non sémantiques dans la source, il y aura également
des nouvelles lignes non sémantiques dans la sortie). La mise à la
longueur des automatique des lignes ne fonctionne pas actuellement avec
la sortie HTML. Pour la sortie <span class="nowrap">`ipynb`</span>,
cette option affecte la mise à la longueur des cellules markdown.

<span id="option--columns" class="option-anchor"><span class="nowrap">`--columns=`</span>*NUMBER*</span>  
Précisez la longueur des lignes en caractères. Cela affecte l'habillage
du texte dans le code source généré (voir
<a href="#option--wrap" class="option"><span class="nowrap"><code>--wrap</code></span></a>).
Elle affecte également le calcul de la largeur des colonnes pour les
tableaux en texte clair (voir [Tables](#tables) ci-dessous).

<span id="option--toc" class="option-anchor"><span class="nowrap">`--toc`</span>, <span class="nowrap">`--table-of-contents`</span></span>  
Inclut une table des matières générée automatiquement (ou, dans le cas
de <span class="nowrap">`latex`</span>, <span
class="nowrap">`context`</span>, <span class="nowrap">`docx`</span>,
<span class="nowrap">`odt`</span>, <span
class="nowrap">`opendocument`</span>, <span class="nowrap">`rst`</span>,
ou <span class="nowrap">`ms`</span> une instruction pour en créer une)
dans le document de sortie. Cette option n'a d'effet que si, 
<a href="#option--standalone" class="option"><span class="nowrap"><code>-s/--standalone</code></span></a>
est utilisé, et il n'a aucun effet sur les sorties <span
class="nowrap">`man`</span>, <span class="nowrap">`docbook4`</span>,
<span class="nowrap">`docbook5`</span>, ou <span
class="nowrap">`jats`</span>.

Notez que si vous produisez un PDF via <span class="nowrap">`ms`</span>,
la table des matières apparaîtra au début du document, avant le titre.
Si vous préférez qu'elle soit à la fin du document, utilisez l'option
<a href="#option--pdf-engine-opt" class="option"><span class="nowrap"><code>--pdf-engine-opt=--no-toc-relocation</code></span></a>.

<span id="option--toc-depth" class="option-anchor"><span class="nowrap">`--toc-depth=`</span>*NUMBER*</span>  
Précisez le nombre de niveaux de section à inclure dans la table des
matières. La valeur par défaut est 3 (ce qui signifie que les rubriques
de niveau 1, 2 et 3 figureront dans le sommaire).

<span id="option--strip-comments" class="option-anchor"><span class="nowrap">`--strip-comments`</span></span>  
Supprimez les commentaires HTML dans la source Markdown ou Textile,
plutôt que de les transmettre à la sortie Markdown, Textile ou HTML sous
forme de HTML brut. Cela ne s'applique pas aux commentaires HTML à
l'intérieur des blocs HTML bruts lorsque l'extension <span
class="nowrap">`markdown_in_html_blocks`</span> n'est pas positionnée.

<span id="option--no-highlight" class="option-anchor"><span class="nowrap">`--no-highlight`</span></span>  
Désactive la mise en évidence syntaxique pour les blocs de code et les
alignements, même lorsqu'un attribut de langue est donné.

<span id="option--highlight-style" class="option-anchor"><span class="nowrap">`--highlight-style=`</span>*STYLE*\|*FILE*</span>  
Spécifie le style de coloriage à utiliser dans le code source mis en
évidence. Les options sont <span class="nowrap">`pygments`</span> (par
défaut), <span class="nowrap">`kate`</span>, <span
class="nowrap">`monochrome`</span>, <span
class="nowrap">`breezeDark`</span>, <span
class="nowrap">`espresso`</span>, <span class="nowrap">`zenburn`</span>,
<span class="nowrap">`haddock`</span>, et <span
class="nowrap">`tango`</span>. Pour plus d'informations sur la mise en
évidence de la syntaxe dans le cadre de pandoc, voir [Syntax
highlighting](#syntax-highlighting), ci-dessous. Voir aussi
<a href="#option--list-highlight-styles" class="option"><span class="nowrap"><code>--list-highlight-styles</code></span></a>.

Au lieu d'un nom *STYLE*, un fichier JSON avec l'extension <span
class="nowrap">`.theme`</span> peut être fourni. Il sera analysé comme
un thème de mise en évidence de la syntaxe KDE et (s'il est valide)
utilisé comme style de mise en évidence.  
  
Pour générer la version JSON d'un style existant, utilisez
<a href="#option--print-highlight-style" class="option"><span class="nowrap"><code>--print-highlight-style</code></span></a>.

<span id="option--print-highlight-style" class="option-anchor"><span class="nowrap">`--print-highlight-style=`</span>*STYLE*\|*FILE*</span>  
Imprime une version JSON d'un style de surlignage, qui peut être
modifié, sauvegardé avec une extension<span
class="nowrap">`.theme`</span>, et utilisé avec
<a href="#option--highlight-style" class="option"><span class="nowrap"><code>--highlight-style</code></span></a>.
Cette option peut être utilisée avec
<a href="#option--output" class="option"><span class="nowrap"><code>-o</code></span></a>/<a href="#option--output" class="option"><span class="nowrap"><code>--output</code></span></a>
pour rediriger la sortie vers un fichier, mais
<a href="#option--output" class="option"><span class="nowrap"><code>-o</code></span></a>/<a href="#option--output" class="option"><span class="nowrap"><code>--output</code></span></a>
doit venir avant
<a href="#option--print-highlight-style" class="option"><span class="nowrap"><code>--print-highlight-style</code></span></a>
sur la ligne de commande.

<span id="option--syntax-definition" class="option-anchor"><span class="nowrap">`--syntax-definition=`</span>*FILE*</span>  
Demande à pandoc de charger un fichier de définition de la syntaxe XML
KDE, qui sera utilisé pour la mise en évidence syntaxique des blocs de
code correctement marqués. Ce fichier peut être utilisé pour ajouter la
prise en charge de nouveaux langages ou pour utiliser des définitions
syntaxiques modifiées pour des langages existants. Cette option peut
être répétée pour ajouter plusieurs définitions syntaxiques.

<span id="option--include-in-header" class="option-anchor"><span class="nowrap">`-H`</span> *FILE*, <span class="nowrap">`--include-in-header=`</span>*FILE*\|*URL*</span>  
Inclure le contenu de FILE, mot pour mot, à la fin de l'en-tête. Cela
peut être utilisé, par exemple, pour inclure des CSS ou des JavaScript
spéciaux dans des documents HTML. Cette option peut être utilisée à
plusieurs reprises pour inclure plusieurs fichiers dans l'en-tête. Ils
seront inclus dans l'ordre indiqué. Cela implique
<a href="#option--standalone" class="option"><span class="nowrap"><code>--standalone</code></span></a>.

<span id="option--include-before-body" class="option-anchor"><span class="nowrap">`-B`</span> *FILE*, <span class="nowrap">`--include-before-body=`</span>*FILE*\|*URL*</span>  
Inclure le contenu du DOSSIER, mot pour mot, au début du corps du
document (par exemple, après le tag <span class="nowrap">`<body>`</span>
en HTML, ou la commande <span class="nowrap">`\begin{document}`</span>
de LaTeX). Ceci peut être utilisé pour inclure des barres de navigation
ou des bannières dans les documents HTML. Cette option peut être
utilisée de manière répétée pour inclure plusieurs fichiers. Ils seront
inclus dans l'ordre indiqué. Implique
<a href="#option--standalone" class="option"><span class="nowrap"><code>--standalone</code></span></a>.

<span id="option--include-after-body" class="option-anchor"><span class="nowrap">`-A`</span> *FILE*, <span class="nowrap">`--include-after-body=`</span>*FILE*\|*URL*</span>  
Inclure le contenu du DOSSIER, mot pour mot, à la fin du corps du
document (avant le tag <span class="nowrap">`</body>`</span> de HTML, ou
la commande<span class="nowrap">`\end{document}`</span>de  LaTeX). Cette
option peut être utilisée à plusieurs reprises pour inclure plusieurs
fichiers. Ils seront inclus dans l'ordre indiqué. Implique
<a href="#option--standalone" class="option"><span class="nowrap"><code>--standalone</code></span></a>.

<span id="option--resource-path" class="option-anchor"><span class="nowrap">`--resource-path=`</span>*SEARCHPATH*</span>  
Liste de chemins pour la recherche d'images et d'autres ressources. Les
chemins doivent être séparés par : sur les systèmes Linux, UNIX, et
macOS, et par ; sur Windows. Si
<a href="#option--resource-path" class="option"><span class="nowrap"><code>--resource-path</code></span></a>
n'est pas spécifié, le chemin de ressource par défaut est le répertoire
de travail. Notez que, si
<a href="#option--resource-path" class="option"><span class="nowrap"><code>--resource-path</code></span></a>
est spécifié, le répertoire de travail doit être explicitement
répertorié sinon il ne sera pas recherché. Par exemple :
<a href="#option--resource-path" class="option"><span class="nowrap"><code>--resource-path=.:test</code></span></a>
sera recherché dans le répertoire de travail et le sous-répertoire <span
class="nowrap">`test`</span>, dans cet ordre.

<a href="#option--resource-path" class="option"><span class="nowrap"><code>--resource-path</code></span></a>
n'a d'effet que si (a) le format de sortie intègre des images (par
exemple, <span class="nowrap">`docx`</span>, <span
class="nowrap">`pdf`</span>, ou <span class="nowrap">`html`</span> avec
<a href="#option--self-contained" class="option"><span class="nowrap"><code>--self-contained</code></span></a>)
ou (b) il est utilisé conjointement
avec<a href="#option--extract-media" class="option"><span class="nowrap"><code>--extract-media</code></span></a>.

<span id="option--request-header" class="option-anchor"><span class="nowrap">`--request-header=`</span>*NAME*<span class="nowrap">`:`</span>*VAL*</span>  
Définissez l'en-tête de la requête NAME à la valeur VAL lorsque vous
effectuez des requêtes HTTP (par exemple, lorsqu'une URL est donnée sur
la ligne de commande, ou lorsque les ressources utilisées dans un
document doivent être téléchargées). Si vous êtes derrière un proxy,
vous devez également définir la variable d'environnement <span
class="nowrap">`http_proxy`</span> vers <span
class="nowrap">`http://...`</span>.

Options affectant des exports spécifiques
-----------------------------------------

<span id="option--self-contained" class="option-anchor"><span class="nowrap">`--self-contained`</span></span>  
Produire un fichier HTML autonome sans dépendance externe, en utilisant
<span class="nowrap">`data:`</span> URIs to incorporate the contents of
linked scripts, stylesheets, images, and videos. Implies
<a href="#option--standalone" class="option"><span class="nowrap"><code>--standalone</code></span></a>.
The resulting file should be “self-contained,” in the sense that it
needs no external files and no net access to be displayed properly by a
browser. This option works only with HTML output formats, including
<span class="nowrap">`html4`</span>, <span
class="nowrap">`html5`</span>, <span class="nowrap">`html+lhs`</span>,
<span class="nowrap">`html5+lhs`</span>, <span
class="nowrap">`s5`</span>, <span class="nowrap">`slidy`</span>, <span
class="nowrap">`slideous`</span>, <span
class="nowrap">`dzslides`</span>, and <span
class="nowrap">`revealjs`</span>. Scripts, images, and stylesheets at
absolute URLs will be downloaded; those at relative URLs will be sought
relative to the working directory (if the first source file is local) or
relative to the base URL (if the first source file is remote). Elements
with the attribute <span class="nowrap">`data-external="1"`</span> will
be left alone; the documents they link to will not be incorporated in
the document. Limitation: resources that are loaded dynamically through
JavaScript cannot be incorporated; as a result,
<a href="#option--self-contained" class="option"><span class="nowrap"><code>--self-contained</code></span></a>
does not work with
<a href="#option--mathjax" class="option"><span class="nowrap"><code>--mathjax</code></span></a>,
and some advanced features (e.g. zoom or speaker notes) may not work in
an offline “self-contained” <span class="nowrap">`reveal.js`</span>
slide show.

<span id="option--html-q-tags" class="option-anchor"><span class="nowrap">`--html-q-tags`</span></span>  
Use <span class="nowrap">`<q>`</span> tags for quotes in HTML.

<span id="option--ascii" class="option-anchor"><span class="nowrap">`--ascii`</span></span>  
N'utiliser que des caractères ASCII en sortie. Actuellement pris en
charge pour les formats XML et HTML (qui utilisent des entités au lieu
d'UTF-8 lorsque cette option est sélectionnée), CommonMark, gfm et
Markdown (qui utilisent des entités), roff ms (qui utilisent des
échappements hexadécimaux) et, dans une mesure limitée, LaTeX (qui
utilise des commandes standard pour les caractères accentués lorsque
cela est possible). la sortie roff man utilise ASCII par défaut.

<span id="option--reference-links" class="option-anchor"><span class="nowrap">`--reference-links`</span></span>  
Utilisez des liens de type référence, plutôt que des liens en ligne,
pour rédiger des notes ou des textes restructurés. Par défaut, des liens
en ligne sont utilisés. Le placement des références de liens est affecté
par l' option
<a href="#option--reference-location%20" class="option"><span class="nowrap"><code>--reference-location</code></span></a>.

<span id="option--reference-location " class="option-anchor"><span class="nowrap">`--reference-location                       = block`</span>\|<span class="nowrap">`section`</span>\|<span class="nowrap">`document`</span></span>  
Précisez si les notes de bas de page (et les références, si <span
class="nowrap">`reference-links`</span> est mis) sont placés à la fin du
bloc courant (de niveau supérieur), de la section courante ou du
document. La valeur par défaut est <span
class="nowrap">`document`</span>. Actuellement, il ne concerne que
l'export en Markdown.

<span id="option--atx-headers" class="option-anchor"><span class="nowrap">`--atx-headers`</span></span>  
Utilisez des titres de type ATX dans la sortie de Markdown. La valeur
par défaut est d'utiliser des titres de type setext pour les niveaux 1
et 2, puis des titres ATX. (Note : pour des sorties <span
class="nowrap">`gfm`</span>, les rubriques ATX sont toujours utilisées).
Cette option affecte également les cellules Markdown dans la sortie
<span class="nowrap">`ipynb`</span>.

<span id="option--top-level-division" class="option-anchor"><span class="nowrap">`--top-level-division=[default|section|chapter|part]`</span></span>  
Traitez les titres de haut niveau comme le type de division donné dans
les sorties LaTeX, ConTeXt, DocBook et TEI. L'ordre hiérarchique est le
suivant : partie, chapitre, puis section ; toutes les rubriques sont
décalées de telle sorte que la rubrique de niveau supérieur devienne le
type spécifié. Le comportement par défaut consiste à déterminer le
meilleur type de division par le biais des heuristiques : à moins que
d'autres conditions ne s'appliquent, <span
class="nowrap">`section`</span> est choisie. Lorsque la variable <span
class="nowrap">`documentclass`</span>est <span
class="nowrap">`report`</span>, <span class="nowrap">`book`</span>, ou
<span class="nowrap">`memoir`</span> (à moins que l'option <span
class="nowrap">`article`</span>soit spécifiée), <span
class="nowrap">`chapter`</span> est implicite comme étant le cadre de
cette option. Si  <span class="nowrap">`beamer`</span> est le format de
sortie, spécifiez soit <span class="nowrap">`chapter`</span> ou <span
class="nowrap">`part`</span> fera en sorte que les rubriques de haut
niveau deviennent <span class="nowrap">`\part{..}`</span>, tandis que
les rubriques de second niveau restent par défaut.

<span id="option--number-sections" class="option-anchor"><span class="nowrap">`-N`</span>, <span class="nowrap">`--number-sections`</span></span>  
Numéroter les titres de section en sortie LaTeX, ConTeXt, HTML ou EPUB.
Par défaut, les sections ne sont pas numérotées. Les sections avec
classe <span class="nowrap">`unnumbered`</span> ne seront jamais
numérotées, même si
<a href="#option--number-sections" class="option"><span class="nowrap"><code>--number-sections</code></span></a>
est spécifié.

<span id="option--number-offset" class="option-anchor"><span class="nowrap">`--number-offset=`</span>*NUMBER*\[<span class="nowrap">`,`</span>*NUMBER*<span class="nowrap">`,`</span>*…*\]</span>  
Décalage pour les titres de section dans la sortie HTML (ignoré dans les
autres formats de sortie). Le premier chiffre est ajouté au numéro de la
section pour les titres de premier niveau, le second pour les titres de
second niveau, etc. Ainsi, par exemple, si vous voulez que la première
rubrique de premier niveau de votre document soit numérotée "6",
indiquez
<a href="#option--number-offset" class="option"><span class="nowrap"><code>--number-offset=5</code></span></a>.
If your document starts with a level-2 heading which you want to be
numbered “1.5”, specify
<a href="#option--number-offset" class="option"><span class="nowrap"><code>--number-offset=1,4</code></span></a>.
Offsets are 0 by default. Implies
<a href="#option--number-sections" class="option"><span class="nowrap"><code>--number-sections</code></span></a>.

<span id="option--listings" class="option-anchor"><span class="nowrap">`--listings`</span></span>  
Utiliser le paquage [<span
class="nowrap">`listings`</span>](https://ctan.org/pkg/listings) de
LaTeX. Le paquet ne prend pas en charge le codage multi-octets pour le
code source. Pour gérer l'UTF-8, vous devez utiliser un modèle
personnalisé. Ce problème est entièrement documenté ici : [Encoding
issue with the listings
package](https://en.wikibooks.org/wiki/LaTeX/Source_Code_Listings#Encoding_issue).

<span id="option--incremental" class="option-anchor"><span class="nowrap">`-i`</span>, <span class="nowrap">`--incremental`</span></span>  
Faites en sorte que les éléments de la liste dans les diaporamas
s'affichent progressivement (un par un). Par défaut, les listes
s'affichent toutes en même temps.

<span id="option--slide-level" class="option-anchor"><span class="nowrap">`--slide-level=`</span>*NUMBER*</span>  
Précise que les rubriques ayant le niveau spécifié créent des
diapositives (pour <span class="nowrap">`beamer`</span>, <span
class="nowrap">`s5`</span>, <span class="nowrap">`slidy`</span>, <span
class="nowrap">`slideous`</span>, <span
class="nowrap">`dzslides`</span>). Les titres au-dessus de ce niveau
dans la hiérarchie servent à diviser le diaporama en sections ; les
titres en dessous de ce niveau créent des sous-titres à l'intérieur
d'une diapositive. Notez que le contenu qui n'est pas contenu sous les
titres du niveau de la diapositive n'apparaîtra pas dans le diaporama.
Par défaut, le niveau de la diapositive est défini en fonction du
contenu du document ; voir  
[Structuring the slide show](#structuring-the-slide-show).

<span id="option--section-divs" class="option-anchor"><span class="nowrap">`--section-divs`</span></span>  
Mettre les sections dans des étiquettes <span
class="nowrap">`<section>`</span> (ou <span
class="nowrap">`<div>`</span> pour <span class="nowrap">`html4`</span>),
and attach identifiers to the enclosing <span
class="nowrap">`<section>`</span> (or <span
class="nowrap">`<div>`</span>) rather than the heading itself. See
[Heading identifiers](#heading-identifiers), below.

<span id="option--email-obfuscation" class="option-anchor"><span class="nowrap">`--email-obfuscation=none`</span>\|<span class="nowrap">`javascript`</span>\|<span class="nowrap">`references`</span></span>  
Spécifier une méthode pour cacher les liens <span
class="nowrap">`mailto:`</span>dans les documents HTML. <span
class="nowrap">`none`</span>laisse les liens <span
class="nowrap">`mailto:`</span> tels quels. <span
class="nowrap">`javascript`</span> les cache en utilisant JavaScript.
<span class="nowrap">`references`</span> les occulte en imprimant leurs
lettres sous forme de références de caractères décimaux ou hexadécimaux.
Par défaut, <span class="nowrap">`none`</span>.

<span id="option--id-prefix" class="option-anchor"><span class="nowrap">`--id-prefix=`</span>*STRING*</span>  
Spécifiez un préfixe à ajouter à tous les identificateurs et liens
internes dans la sortie HTML et DocBook, et aux numéros de note de bas
de page dans la sortie Markdown et Haddock. Ceci est utile pour éviter
les doublons d'identificateurs lors de la génération de fragments à
inclure dans d'autres pages.

<span id="option--title-prefix" class="option-anchor"><span class="nowrap">`-T`</span> *STRING*, <span class="nowrap">`--title-prefix=`</span>*STRING*</span>  
Spécifiez STRING comme préfixe au début du titre qui apparaît dans
l'en-tête HTML (mais pas dans le titre tel qu'il apparaît au début du
corps du HTML). Implique
<a href="#option--standalone" class="option"><span class="nowrap"><code>--standalone</code></span></a>.

<span id="option--css" class="option-anchor"><span class="nowrap">`-c`</span> *URL*, <span class="nowrap">`--css=`</span>*URL*</span>  
Lien vers une feuille de style CSS. Cette option peut être utilisée de
manière répétée pour inclure plusieurs fichiers. Ils seront inclus dans
l'ordre indiqué.  
Une feuille de style est nécessaire pour générer l'EPUB. Si aucune
feuille de style n'est fournie en utilisant cette option (ou les champs
metadata <span class="nowrap">`css`</span> ou <span
class="nowrap">`stylesheet`</span>), pandoc va chercher un fichier <span
class="nowrap">`epub.css`</span> dans le répertoire des données des
utilisateurs (voir
<a href="#option--data-dir" class="option"><span class="nowrap"><code>--data-dir</code></span></a>).
If it is not found there, sensible defaults will be used.

<span id="option--reference-doc" class="option-anchor"><span class="nowrap">`--reference-doc=`</span>*FILE*</span>  
Utiliser le fichier spécifié comme référence de style dans la production
d'un fichier docx ou ODT.

Docx  
Pour les meilleurs résultats, le docx de référence doit être une version
modifiée d'un fichier docx produit à l'aide de pandoc. Le contenu du
docx de référence est ignoré, mais ses feuilles de style et les
propriétés du document (y compris les marges, la taille de la page,
l'en-tête et le pied de page) sont utilisées dans le nouveau docx. Si
aucun docx de référence n'est spécifié sur la ligne de commande, pandoc
recherchera un fichier<span class="nowrap">`reference.docx`</span> dans
le répertoire des données des utilisateurs (voir
<a href="#option--data-dir" class="option"><span class="nowrap"><code>--data-dir</code></span></a>).
Si ce n'est pas le cas non plus, des valeurs par défaut raisonnables
seront utilisées.

Pour produire un fichier<span class="nowrap">`reference.docx`</span>
personnalisé, faites d'abord une copie du<span
class="nowrap">`reference.docx`</span>par défaut : <span
class="nowrap">`pandoc -o                           custom-reference.docx --print-default-data-file                           reference.docx`</span>.
Puis ouvrez <span class="nowrap">`custom-reference.docx`</span>dans 
Word, modifiez les styles comme vous le souhaitez, et enregistrez le
fichier. Pour de meilleurs résultats, n'apportez pas de modifications à
ce fichier, à part celles des styles utilisés par pandoc :

Styles de paragraphes :

-   Normal
-   Body Text
-   First Paragraph
-   Compact
-   Title
-   Subtitle
-   Author
-   Date
-   Abstract
-   Bibliography
-   Heading 1
-   Heading 2
-   Heading 3
-   Heading 4
-   Heading 5
-   Heading 6
-   Heading 7
-   Heading 8
-   Heading 9
-   Block Text
-   Footnote Text
-   Definition Term
-   Definition
-   Caption
-   Table Caption
-   Image Caption
-   Figure
-   Captioned Figure
-   TOC Heading

Styles de caractères :

-   Default Paragraph Font
-   Body Text Char
-   Verbatim Char
-   Footnote Reference
-   Hyperlink

Styles de tableaux :

-   Table

ODT  
Pour de meilleurs résultats, le ODT de référence devrait être une
version modifiée d'un ODT produit à l'aide de pandoc. Le contenu de
l'ODT de référence est ignoré, mais ses feuilles de style sont utilisées
dans le nouvel ODT. Si aucun ODT de référence n'est spécifié sur la
ligne de commande, pandoc recherchera un fichier <span
class="nowrap">`reference.odt`</span> dans le répertoire des données des
utilisateurs (voir
<a href="#option--data-dir" class="option"><span class="nowrap"><code>--data-dir</code></span></a>).
Si ce n'est pas le cas non plus, des valeurs par défaut raisonnables
seront utilisées.  
  
Pour produire un <span
class="nowrap">`reference.odt`</span>personnalisé, obtenir d'abord une
copie du fichier <span class="nowrap">`reference.odt`</span>par défaut :
<span
class="nowrap">`pandoc -o                           custom-reference.odt --print-default-data-file                           reference.odt`</span>.
Puis ouvrir <span class="nowrap">`custom-reference.odt`</span> dans
LibreOffice, modifier les styles comme vous le souhaitez, et enregistrer
le fichier.

PowerPoint  
Les modèles inclus dans Microsoft PowerPoint 2013 (avec les extensions
<span class="nowrap">`.pptx`</span> ou <span
class="nowrap">`.potx`</span>) are known to work, as are most templates
derived from these.

L'exigence spécifique est que le modèle commence par les quatre
indications suivantes :

1.  Title Slide
2.  Title and Content
3.  Section Header
4.  Two Content

Tous les modèles inclus dans une récente version de MS PowerPoint
observent ce critère (vous pouvez cliquer sur <span
class="nowrap">`Layout`</span> dans le menu <span
class="nowrap">`Home`</span> pour vérifier).

Vous pouvez aussi modifier le fichier <span
class="nowrap">`reference.pptx`</span>par défaut : d'abord executer
<span
class="nowrap">`pandoc                           -o custom-reference.pptx --print-default-data-file                           reference.pptx`</span>,
puis modifier <span class="nowrap">`custom-reference.pptx`</span>dans 
MS PowerPoint (pandoc will use the first four layout slides, as
mentioned above).

<span id="option--epub-cover-image" class="option-anchor"><span class="nowrap">`--epub-cover-image=`</span>*FILE*</span>  
Use the specified image as the EPUB cover. It is recommended that the
image be less than 1000px in width and height. Note that in a Markdown
source document you can also specify <span
class="nowrap">`cover-image`</span> in a YAML metadata block (see [EPUB
Metadata](#epub-metadata), below).

<span id="option--epub-metadata" class="option-anchor"><span class="nowrap">`--epub-metadata=`</span>*FILE*</span>  
Look in the specified XML file for metadata for the EPUB. The file
should contain a series of [Dublin Core
elements](https://www.dublincore.org/specifications/dublin-core/dces/).
For example:

     <dc:rights>Creative Commons</dc:rights>
     <dc:language>es-AR</dc:language>

By default, pandoc will include the following metadata elements: <span
class="nowrap">`<dc:title>`</span> (from the document title), <span
class="nowrap">`<dc:creator>`</span> (from the document authors), <span
class="nowrap">`<dc:date>`</span> (from the document date, which should
be in [ISO 8601 format](https://www.w3.org/TR/NOTE-datetime)), <span
class="nowrap">`<dc:language>`</span> (from the <span
class="nowrap">`lang`</span> variable, or, if is not set, the locale),
and <span
class="nowrap">`<dc:identifier                       id="BookId">`</span>
(a randomly generated UUID). Any of these may be overridden by elements
in the metadata file.

Note: if the source document is Markdown, a YAML metadata block in the
document can be used instead. See below under [EPUB
Metadata](#epub-metadata).

<span id="option--epub-embed-font" class="option-anchor"><span class="nowrap">`--epub-embed-font=`</span>*FILE*</span>  
Embed the specified font in the EPUB. This option can be repeated to
embed multiple fonts. Wildcards can also be used: for example, <span
class="nowrap">`DejaVuSans-*.ttf`</span>. However, if you use wildcards
on the command line, be sure to escape them or put the whole filename in
single quotes, to prevent them from being interpreted by the shell. To
use the embedded fonts, you will need to add declarations like the
following to your CSS (see
<a href="#option--css" class="option"><span class="nowrap"><code>--css</code></span></a>):

    @font-face {
    font-family: DejaVuSans;
    font-style: normal;
    font-weight: normal;
    src:url("DejaVuSans-Regular.ttf");
    }
    @font-face {
    font-family: DejaVuSans;
    font-style: normal;
    font-weight: bold;
    src:url("DejaVuSans-Bold.ttf");
    }
    @font-face {
    font-family: DejaVuSans;
    font-style: italic;
    font-weight: normal;
    src:url("DejaVuSans-Oblique.ttf");
    }
    @font-face {
    font-family: DejaVuSans;
    font-style: italic;
    font-weight: bold;
    src:url("DejaVuSans-BoldOblique.ttf");
    }
    body { font-family: "DejaVuSans"; }

<span id="option--epub-chapter-level" class="option-anchor"><span class="nowrap">`--epub-chapter-level=`</span>*NUMBER*</span>  
Specify the heading level at which to split the EPUB into separate
“chapter” files. The default is to split into chapters at level-1
headings. This option only affects the internal composition of the EPUB,
not the way chapters and sections are displayed to users. Some readers
may be slow if the chapter files are too large, so for large documents
with few level-1 headings, one might want to use a chapter level of 2
or 3.

<span id="option--epub-subdirectory" class="option-anchor"><span class="nowrap">`--epub-subdirectory=`</span>*DIRNAME*</span>  
Specify the subdirectory in the OCF container that is to hold the
EPUB-specific contents. The default is <span
class="nowrap">`EPUB`</span>. To put the EPUB contents in the top level,
use an empty string.

<span id="option--ipynb-output" class="option-anchor"><span class="nowrap">`--ipynb-output=all|none|best`</span></span>  
Determines how ipynb output cells are treated. <span
class="nowrap">`all`</span> means that all of the data formats included
in the original are preserved. <span class="nowrap">`none`</span> means
that the contents of data cells are omitted. <span
class="nowrap">`best`</span> causes pandoc to try to pick the richest
data block in each output cell that is compatible with the output
format. The default is <span class="nowrap">`best`</span>.

<span id="option--pdf-engine" class="option-anchor"><span class="nowrap">`--pdf-engine=`</span>*PROGRAM*</span>  
Utilisez le moteur spécifié lorsque vous produisez un fichier PDF. Les
valeurs valides sont <span class="nowrap">`pdflatex`</span>, <span
class="nowrap">`lualatex`</span>, <span class="nowrap">`xelatex`</span>,
<span class="nowrap">`latexmk`</span>, <span
class="nowrap">`tectonic`</span>, <span
class="nowrap">`wkhtmltopdf`</span>, <span
class="nowrap">`weasyprint`</span>, <span
class="nowrap">`prince`</span>, <span class="nowrap">`context`</span>et 
<span class="nowrap">`pdfroff`</span>. Si le moteur n'est pas dans votre
PATH, vous pouvez indiquer ici le chemin complet du moteur. Si cette
option n'est pas spécifiée, pandoc utilise les valeurs par défaut
suivantes en fonction du format de sortie spécifié à l'aide de
<a href="#option--to" class="option"><span class="nowrap"><code>-t/--to</code></span></a>:

-   <a href="#option--to" class="option"><span class="nowrap"><code>-t                           latex</code></span></a>
    ou rien : <span class="nowrap">`pdflatex`</span> (autres options:
    <span class="nowrap">`xelatex`</span>, <span
    class="nowrap">`lualatex`</span>, <span
    class="nowrap">`tectonic`</span>, <span
    class="nowrap">`latexmk`</span>)
-   <a href="#option--to" class="option"><span class="nowrap"><code>-t                           context</code></span></a>:
    <span class="nowrap">`context`</span>
-   <a href="#option--to" class="option"><span class="nowrap"><code>-t                           html</code></span></a>:
    <span class="nowrap">`wkhtmltopdf`</span> (autres options : <span
    class="nowrap">`prince`</span>, <span
    class="nowrap">`weasyprint`</span>)
-   <a href="#option--to" class="option"><span class="nowrap"><code>-t                           ms</code></span></a>:
    <span class="nowrap">`pdfroff`</span>

<span id="option--pdf-engine-opt" class="option-anchor"><span class="nowrap">`--pdf-engine-opt=`</span>*STRING*</span>  
Utilisez la chaîne de caractères donnée comme argument de ligne de
commande pour le <span class="nowrap">`pdf-engine`</span>. Par exemple,
pour utiliser un répertoire persistant <span
class="nowrap">`foo`</span>pour les fichiers auxiliaires de <span
class="nowrap">`latexmk`</span>, utiliser
<a href="#option--pdf-engine-opt" class="option"><span class="nowrap"><code>--pdf-engine-opt=-outdir=foo</code></span></a>.
Notez qu'aucun contrôle des options en double n'est effectué.

Citation rendering
------------------

<span id="option--bibliography" class="option-anchor"><span class="nowrap">`--bibliography=`</span>*FILE*</span>  
Mettre le champ <span class="nowrap">`bibliography`</span>dans les
metadonnées du  document à *FILE*, en dérogeant à toute valeur fixée
dans les métadonnées, et traiter les citations en utilisant <span
class="nowrap">`pandoc-citeproc`</span>. (Ceci est équivalent à
<a href="#option--metadata" class="option"><span class="nowrap"><code>--metadata bibliography=FILE --filter                         pandoc-citeproc</code></span></a>.)
Si
<a href="#option--natbib" class="option"><span class="nowrap"><code>--natbib</code></span></a>
ou
<a href="#option--biblatex" class="option"><span class="nowrap"><code>--biblatex</code></span></a>
est également fourni, <span class="nowrap">`pandoc-citeproc`</span>
n'est pas utilisé, ce qui équivaut à
<a href="#option--metadata" class="option"><span class="nowrap"><code>--metadata                         bibliography=FILE</code></span></a>.
Si vous fournissez cet argument plusieurs fois, chaque FICHIER sera
ajouté à la bibliographie.

<span id="option--csl" class="option-anchor"><span class="nowrap">`--csl=`</span>*FILE*</span>  
Mettre le champ <span class="nowrap">`csl`</span> des métadonnées du
document à *FILE*, en supplantant toute valeur fixée dans les
métadonnées. (Cela équivaut à
<a href="#option--metadata" class="option"><span class="nowrap"><code>--metadata                         csl=FILE</code></span></a>.)
Cette option n'est pertinente qu'avec <span
class="nowrap">`pandoc-citeproc`</span>.

<span id="option--citation-abbreviations" class="option-anchor"><span class="nowrap">`--citation-abbreviations=`</span>*FILE*</span>  
Mettre le champ <span class="nowrap">`citation-abbreviations`</span> des
metadonnées du document à *FILE*, en supplantant toute valeur fixée dans
les métadonnées. (Cela équivaut à
<a href="#option--metadata" class="option"><span class="nowrap"><code>--metadata                         citation-abbreviations=FILE</code></span></a>.)
Cette option n'est pertinente qu'avec <span
class="nowrap">`pandoc-citeproc`</span>.

<span id="option--natbib" class="option-anchor"><span class="nowrap">`--natbib`</span></span>  
Utilise [<span
class="nowrap">`natbib`</span>](https://ctan.org/pkg/natbib) pour les
citations dans la production LaTeX. Cette option n'est pas à utiliser
avec le filtre <span class="nowrap">`pandoc-citeproc`</span> ou avec une
sortie PDF. Il est destiné à être utilisé pour produire un fichier LaTeX
qui peut être traité avec [<span
class="nowrap">`bibtex`</span>](https://ctan.org/pkg/bibtex).

<span id="option--biblatex" class="option-anchor"><span class="nowrap">`--biblatex`</span></span>  
Utiliser [<span
class="nowrap">`biblatex`</span>](https://ctan.org/pkg/biblatex)pour les
citations dans la sortie Latex. Cette option n'est pas à utiliser avec
le filtre <span class="nowrap">`pandoc-citeproc`</span> ou avec la
sortie PDF. Il est destiné à être utilisé pour produire un fichier LaTeX
qui peut être traité avec [<span
class="nowrap">`bibtex`</span>](https://ctan.org/pkg/bibtex) or [<span
class="nowrap">`biber`</span>](https://ctan.org/pkg/biber).

Math rendering in HTML
----------------------

The default is to render TeX math as far as possible using Unicode
characters. Formulas are put inside a <span class="nowrap">`span`</span>
with <span class="nowrap">`class="math"`</span>, so that they may be
styled differently from the surrounding text if needed. However, this
gives acceptable results only for basic math, usually you will want to
use
<a href="#option--mathjax" class="option"><span class="nowrap"><code>--mathjax</code></span></a>
or another of the following options.

<span id="option--mathjax" class="option-anchor"><span class="nowrap">`--mathjax`</span>\[<span class="nowrap">`=`</span>*URL*\]</span>  
Use [MathJax](https://www.mathjax.org/) to display embedded TeX math in
HTML output. TeX math will be put between <span
class="nowrap">`\(...\)`</span> (for inline math) or <span
class="nowrap">`\[...\]`</span> (for display math) and wrapped in <span
class="nowrap">`<span>`</span> tags with class <span
class="nowrap">`math`</span>. Then the MathJax JavaScript will render
it. The *URL* should point to the <span
class="nowrap">`MathJax.js`</span> load script. If a *URL* is not
provided, a link to the Cloudflare CDN will be inserted.

<span id="option--mathml" class="option-anchor"><span class="nowrap">`--mathml`</span></span>  
Convert TeX math to [MathML](https://www.w3.org/Math/) (in <span
class="nowrap">`epub3`</span>, <span class="nowrap">`docbook4`</span>,
<span class="nowrap">`docbook5`</span>, <span
class="nowrap">`jats`</span>, <span class="nowrap">`html4`</span> and
<span class="nowrap">`html5`</span>). This is the default in <span
class="nowrap">`odt`</span> output. Note that currently only Firefox and
Safari (and select e-book readers) natively support MathML.

<span id="option--webtex" class="option-anchor"><span class="nowrap">`--webtex`</span>\[<span class="nowrap">`=`</span>*URL*\]</span>  
Convert TeX formulas to <span class="nowrap">`<img>`</span> tags that
link to an external script that converts formulas to images. The formula
will be URL-encoded and concatenated with the URL provided. For SVG
images you can for example use
<a href="#option--webtex" class="option"><span class="nowrap"><code>--webtex                         https://latex.codecogs.com/svg.latex?</code></span></a>.
If no URL is specified, the CodeCogs URL generating PNGs will be used
(<span class="nowrap">`https://latex.codecogs.com/png.latex?`</span>).
Note: the
<a href="#option--webtex" class="option"><span class="nowrap"><code>--webtex</code></span></a>
option will affect Markdown output as well as HTML, which is useful if
you’re targeting a version of Markdown without native math support.

<span id="option--katex" class="option-anchor"><span class="nowrap">`--katex`</span>\[<span class="nowrap">`=`</span>*URL*\]</span>  
Use [KaTeX](https://github.com/Khan/KaTeX) to display embedded TeX math
in HTML output. The *URL* is the base URL for the KaTeX library. That
directory should contain a <span class="nowrap">`katex.min.js`</span>
and a <span class="nowrap">`katex.min.css`</span> file. If a *URL* is
not provided, a link to the KaTeX CDN will be inserted.

<span id="option--gladtex" class="option-anchor"><span class="nowrap">`--gladtex`</span></span>  
Enclose TeX math in <span class="nowrap">`<eq>`</span> tags in HTML
output. The resulting HTML can then be processed by
[GladTeX](https://humenda.github.io/GladTeX/) to produce images of the
typeset formulas and an HTML file with links to these images. So, the
procedure is:

    pandoc -s --gladtex input.md -o myfile.htex
    gladtex -d myfile-images myfile.htex
    # produces myfile.html and images in myfile-images

Options for wrapper scripts
---------------------------

<span id="option--dump-args" class="option-anchor"><span class="nowrap">`--dump-args`</span></span>  
Print information about command-line arguments to *stdout*, then exit.
This option is intended primarily for use in wrapper scripts. The first
line of output contains the name of the output file specified with the
<a href="#option--output" class="option"><span class="nowrap"><code>-o</code></span></a>
option, or <span class="nowrap">`-`</span> (for *stdout*) if no output
file was specified. The remaining lines contain the command-line
arguments, one per line, in the order they appear. These do not include
regular pandoc options and their arguments, but do include any options
appearing after a <span class="nowrap">`--`</span> separator at the end
of the line.

<span id="option--ignore-args" class="option-anchor"><span class="nowrap">`--ignore-args`</span></span>  
Ignore command-line arguments (for use in wrapper scripts). Regular
pandoc options are not ignored. Thus, for example,

    pandoc --ignore-args -o foo.html -s foo.txt -- -e latin1

is equivalent to

    pandoc -o foo.html -s

Exit codes
==========

If pandoc completes successfully, it will return exit code 0. Nonzero
exit codes have the following meanings:

| Code | Error                           |
|-----:|:--------------------------------|
|    3 | PandocFailOnWarningError        |
|    4 | PandocAppError                  |
|    5 | PandocTemplateError             |
|    6 | PandocOptionError               |
|   21 | PandocUnknownReaderError        |
|   22 | PandocUnknownWriterError        |
|   23 | PandocUnsupportedExtensionError |
|   31 | PandocEpubSubdirectoryError     |
|   43 | PandocPDFError                  |
|   47 | PandocPDFProgramNotFoundError   |
|   61 | PandocHttpError                 |
|   62 | PandocShouldNeverHappenError    |
|   63 | PandocSomeError                 |
|   64 | PandocParseError                |
|   65 | PandocParsecError               |
|   66 | PandocMakePDFError              |
|   67 | PandocSyntaxMapError            |
|   83 | PandocFilterError               |
|   91 | PandocMacroLoop                 |
|   92 | PandocUTF8DecodingError         |
|   93 | PandocIpynbDecodingError        |
|   97 | PandocCouldNotFindDataFileError |
|   99 | PandocResourceNotFound          |

Default files
=============

L'option
<a href="#option--defaults" class="option"><span class="nowrap"><code>--defaults</code></span></a>
peut être utilisée pour spécifier un ensemble d'options. Voici un
exemple de fichier de valeurs par défaut qui montre tous les champs qui
peuvent être utilisés :

<div id="cb19" class="sourceCode">

    from: markdown+emoji
    # reader: may be used instead of from:
    to: html5
    # writer: may be used instead of to:

    # leave blank for output to stdout:
    output-file:
    # leave blank for input from stdin, use [] for no input:
    input-files:
    - preface.md
    - content.md
    # or you may use input-file: with a single value

    template: letter
    standalone: true
    self-contained: false

    # note that structured variables may be specified:
    variables:Il est destiné à être utilisé pour produire un fichier LaTeX qui peut être traité avec
      documentclass: book
      classoption:
        - twosides
        - draft

    # metadata values specified here are parsed as literal
    # string text, not markdown:
    metadata:
      author:
      - Sam Smith
      - Julie Liu
    metadata-files:
    - boilerplate.yaml
    # or you may use metadata-file: with a single value

    # Note that these take files, not their contents:
    include-before-body: []
    include-after-body: []
    include-in-header: []
    resource-path: ["."]

    # filters will be assumed to be Lua filters if they have
    # the .lua extension, and json filters otherwise.  But
    # the filter type can also be specified explicitly, as shown:
    filters:
    - pandoc-citeproc
    - wordcount.lua
    - type: json
      path: foo.lua

    file-scope: false

    data-dir:

    # ERROR, WARNING, or INFO
    verbosity: INFO
    log-file: log.json

    # citeproc, natbib, or biblatex
    cite-method: citeproc
    # part, chapter, section, or default:
    top-level-division: chapter
    abbreviations:

    pdf-engine: pdflatex
    pdf-engine-opts:
    - "-shell-escape"
    # you may also use pdf-engine-opt: with a single option
    # pdf-engine-opt: "-shell-escape"

    # auto, preserve, or none
    wrap: auto
    columns: 78
    dpi: 72

    extract-media: mediadir

    table-of-contents: true
    toc-depth: 2
    number-sections: false
    # a list of offsets at each heading level
    number-offset: [0,0,0,0,0,0]
    # toc: may also be used instead of table-of-contents:
    shift-heading-level-by: 1
    section-divs: true
    identifier-prefix: foo
    title-prefix: ""
    strip-empty-paragraphs: true
    # lf, crlf, or native
    eol: lf
    strip-comments: false
    indented-code-classes: []
    ascii: true
    default-image-extension: ".jpg"

    # either a style name of a style definition file:
    highlight-style: pygments
    syntax-definitions:
    - c.xml
    # or you may use syntax-definition: with a single value
    listings: false

    reference-doc: myref.docx

    # method is plain, webtex, gladtex, mathml, mathjax, katex
    # you may specify a url with webtex, mathjax, katex
    html-math-method:
      method: mathjax
      url: "https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js"
    # none, references, or javascript
    email-obfuscation: javascript

    tab-stop: 8
    preserve-tabs: true

    incremental: false
    slide-level: 2

    epub-subdirectory: EPUB
    epub-metadata: meta.xml
    epub-fonts:
    - foobar.otf
    epub-chapter-level: 1
    epub-cover-image: cover.jpg

    reference-links: true
    # block, section, or document
    reference-location: block
    atx-headers: false

    # accept, reject, or all
    track-changes: accept

    html-q-tags: false
    css:
    - site.css

    # none, all, or best
    ipynb-output: best

    # A list of two-element lists
    request-headers:
    - ["User-Agent", "Mozilla/5.0"]

    fail-if-warnings: false
    dump-args: false
    ignore-args: false
    trace: false

</div>

Les champs qui sont omis n'auront que leurs valeurs par défaut
habituelles. Ainsi, un fichier par défaut peut être aussi simple qu'une
ligne :

<div id="cb20" class="sourceCode">

    verbosity: INFO

</div>

Les fichiers Default peuvent être placés dans le sous-dossier <span
class="nowrap">`defaults`</span> du répertoire des données d'utilisateur
et utilisées à partir de n'importe quel répertoire. Par exemple, on
pourrait créer un fichier spécifiant les valeurs par défaut pour
l'écriture de lettres, le sauvegarder sous <span
class="nowrap">`letter.yaml`</span> dans le sous-répertoire <span
class="nowrap">`defaults`</span> du répertoire des données de
l'utilisateur, puis invoquer ces valeurs par défaut à partir de tout
répertoire utilisant <span
class="nowrap">`pandoc                   --defaults letter`</span> ou
<span class="nowrap">`pandoc                   -dletter`</span>.

Lorsque plusieurs fichiers default sont utilisés, leur contenu sera
combiné.

Notez que, lorsque les arguments de la ligne de commande peuvent être
répétés
(<a href="#option--metadata-file" class="option"><span class="nowrap"><code>--metadata-file</code></span></a>,
<a href="#option--css" class="option"><span class="nowrap"><code>--css</code></span></a>,
<a href="#option--include-in-header" class="option"><span class="nowrap"><code>--include-in-header</code></span></a>,
<a href="#option--include-before-body" class="option"><span class="nowrap"><code>--include-before-body</code></span></a>,
<a href="#option--include-after-body" class="option"><span class="nowrap"><code>--include-after-body</code></span></a>,
<a href="#option--variable" class="option"><span class="nowrap"><code>--variable</code></span></a>,
<a href="#option--metadata" class="option"><span class="nowrap"><code>--metadata</code></span></a>,
<a href="#option--syntax-definition" class="option"><span class="nowrap"><code>--syntax-definition</code></span></a>),
les valeurs spécifiées sur la ligne de commande se combineront avec les
valeurs spécifiées dans le fichier Default, plutôt que de les remplacer.

Modèles
=======

Lorsque l'option
<a href="#option--standalone" class="option"><span class="nowrap"><code>-s/--standalone</code></span></a>
est utilisée, pandoc utilise un modèle pour ajouter les en-têtes et les
pieds de page nécessaires à un document autonome. Pour voir le modèle
utilisé par défaut, il suffit de taper

    pandoc -D *FORMAT*

où FORMAT est le nom du format de sortie. Un modèle personnalisé peut
être spécifié à l'aide de l'option
<a href="#option--template" class="option"><span class="nowrap"><code>--template</code></span></a>.
Vous pouvez également remplacer les modèles par défaut du système pour
un format de sortie donné FORMAT en mettant un fichier <span
class="nowrap">`templates/default.*FORMAT*`</span> dans le répertoire
des données des utilisateurs (voir
<a href="#option--data-dir" class="option"><span class="nowrap"><code>--data-dir</code></span></a>,
ci-dessus). *Exceptions :*

-   Pour une sortie <span class="nowrap">`odt`</span>, personnalisez le
    modèle <span class="nowrap">`default.opendocument`</span>.
-   Pour une sortie <span class="nowrap">`pdf`</span>, personnalisez le
    modèle <span class="nowrap">``</span><span
    class="nowrap">`default.latex`</span>(ou le modèle <span
    class="nowrap">`default.context`</span>, si vous utilisez
    <a href="#option--to" class="option"><span class="nowrap"><code>-t                       context</code></span></a>,
    ou le modèle <span class="nowrap">`default.ms`</span>, si vous
    utilisez
    <a href="#option--to" class="option"><span class="nowrap"><code>-t                       ms</code></span></a>,
    ou le modèle <span class="nowrap">`default.html`</span> si vous
    utilisez<a href="#option--to" class="option"><span class="nowrap"><code>-t                       html</code></span></a>).
-   <span class="nowrap">`docx`</span> et <span
    class="nowrap">`pptx`</span> n'ont pas de modèle (cependant, vous
    pouvez
    utiliser<a href="#option--reference-doc" class="option"><span class="nowrap"><code>--reference-doc</code></span></a>
    pour personnaliser la sortie).

Les modèles contiennent des variables, qui permettent d'inclure des
informations arbitraires à n'importe quel moment du fichier. Ils peuvent
être définis en ligne de commande à l'aide de l'option
<a href="#option--variable" class="option"><span class="nowrap"><code>-V/--variable</code></span></a>.
Si une variable n'est pas définie, pandoc cherchera la clé dans les
métadonnées du document, qui peuvent être définies en utilisant soit les
blocs de métadonnées [YAML](#extension-yaml_metadata_block) ou avec
l'option<a href="#option--metadata" class="option"><span class="nowrap"><code>-M/--metadata</code></span></a>.
En outre, certaines variables sont dotées de valeurs par défaut par
pandoc. Voir [Variables](#variables) ci-dessous pour une liste des
variables utilisées dans les modèles par défaut de pandoc.

Si vous utilisez des modèles personnalisés, vous devrez peut-être les
réviser au fur et à mesure de l'évolution de pandoc. Nous vous
recommandons de suivre les modifications apportées aux modèles par
défaut et de modifier vos modèles personnalisés en conséquence. Une
façon simple de procéder consiste à créer une variante ("fork") du dépôt
[pandoc-templates](https://github.com/jgm/pandoc-templates) et fusionner
les changements après chaque libération de pandoc.

Syntaxe des modèles
-------------------

### Commentaires

Tout ce qui se trouve entre la séquence <span
class="nowrap">`$--`</span> et la fin de la ligne sera traitée comme un
commentaire et sera omise de la sortie.

### Délimiteurs

Pour marquer les variables et les structures de contrôle dans le modèle,
soit <span class="nowrap">`$`</span>…<span class="nowrap">`$`</span>
soit <span class="nowrap">`${`</span>…<span class="nowrap">`}`</span>
peuvent être utilisés comme délimiteurs. Les styles peuvent également
être mélangés dans le même modèle, mais les délimiteurs d'ouverture et
de fermeture doivent correspondre dans chaque cas. Le premier délimiteur
peut être suivi d'un ou plusieurs espaces ou tabulations, qui seront
ignorés. Le délimiteur de fermeture peut être suivi d'un ou plusieurs
espaces ou tabulations, qui seront ignorés.  
  
Pour inclure littérallement un <span class="nowrap">`$`</span> dans le
document, écrire <span class="nowrap">`$$`</span>.

### Variables interpolées

Un logement pour une variable interpolée est un nom de variable entouré
de délimiteurs correspondants. Les noms de variables doivent commencer
par une lettre et peuvent contenir des lettres, des chiffres, <span
class="nowrap">`_`</span>, <span class="nowrap">`-`</span>, et <span
class="nowrap">`.`</span>. Les mots-clés <span
class="nowrap">`it`</span>, <span class="nowrap">`if`</span>, <span
class="nowrap">`else`</span>, <span class="nowrap">`endif`</span>, <span
class="nowrap">`for`</span>, <span class="nowrap">`sep`</span>, et <span
class="nowrap">`endfor`</span> ne peuvent pas être utilisés comme noms
de variables. Exemples :

    $foo$
    $foo.bar.baz$
    $foo_bar.baz-bim$
    $ foo $
    ${foo}
    ${foo.bar.baz}
    ${foo_bar.baz-bim}
    ${ foo }

Les noms de variables avec des points sont utilisés pour obtenir des
valeurs de variables structurées. Ainsi, par exemple, <span
class="nowrap">`employee.salary`</span> restituera la valeur du champ
salaire <span class="nowrap">`salary`</span>de l'objet qui est la valeur
du champ<span class="nowrap">`employee`</span>.

-   Si la valeur de la variable est une valeur simple, elle sera rendue
    textuellement. (Notez qu'aucun échappement n'est effectué ;
    l'hypothèse est que le programme appelant échappera les chaînes de
    caractères de manière appropriée pour le format de sortie).
-   Si la valeur est une liste, les valeurs seront concaténées.
-   If the value is a map, the string <span class="nowrap">`true`</span>
    will be rendered.
-   Toute autre valeur sera rendue comme la chaîne vide.

### Conditions

Une condition commence par <span class="nowrap">`if(variable)`</span>
(inclus dans les délimiteurs correspondants) et se termine par <span
class="nowrap">`endif`</span> (inclus dans les délimiteurs
correspondants). Il peut éventuellement contenir un <span
class="nowrap">`else`</span> (inclus dans les délimiteurs
correspondants). La section<span class="nowrap">`if`</span>est utilisée
si <span class="nowrap">`variable`</span>a une valeur non vide, sinon la
section<span class="nowrap">`else`</span>est utilisée (si présente).
Exemples :

    $if(foo)$bar$endif$

    $if(foo)$
      $foo$
    $endif$

    $if(foo)$
    part one
    $else$
    part two
    $endif$

    ${if(foo)}bar${endif}

    ${if(foo)}
      ${foo}
    ${endif}

    ${if(foo)}
    ${ foo.bar }
    ${else}
    no foo!
    ${endif}

The keyword <span class="nowrap">`elseif`</span> may be used to simplify
complex nested conditionals:

    $if(foo)$
    XXX
    $elseif(bar)$
    YYY
    $else$
    ZZZ
    $endif$

### Boucles for

Une boucle for commence par <span class="nowrap">`for(variable)`</span>
(inclus dans les délimiteurs correspondants) et se termine par <span
class="nowrap">`endfor`</span>(inclus dans les délimiteurs
correspondants).

-   Si <span class="nowrap">`variable`</span> est un tableau, le contenu
    de la boucle sera évalué à plusieurs reprises, avec <span
    class="nowrap">`variable`</span> étant réglé sur chaque valeur du
    tableau à tour de rôle, et concaténé.
-   If <span class="nowrap">`variable`</span> is a map, the material
    inside will be set to the map.
-   Si la valeur de la variable associée n'est pas un tableau ou une
    carte, une seule itération sera effectuée sur sa valeur.

Exemples :

    $for(foo)$$foo$$sep$, $endfor$

    $for(foo)$
      - $foo.last$, $foo.first$
    $endfor$

    ${ for(foo.bar) }
      - ${ foo.bar.last }, ${ foo.bar.first }
    ${ endfor }

    $for(mymap)$
    $it.name$: $it.office$
    $endfor$

Vous pouvez éventuellement spécifier un séparateur entre des valeurs
consécutives en utilisant <span class="nowrap">`sep`</span>  (inclus
dans les délimiteurs correspondants). Le contenu entre <span
class="nowrap">`sep`</span> et le <span class="nowrap">`endfor`</span>
est le séparateu .

    ${ for(foo) }${ foo }${ sep }, ${ endfor }

Au lieu d'utiliser <span class="nowrap">`variable`</span>dans la boucle
, the special anaphoric keyword <span class="nowrap">`it`</span> may be
used.

    ${ for(foo.bar) }
      - ${ it.last }, ${ it.first }
    ${ endfor }

### Partials

Les parties (sous-modèles stockés dans différents fichiers) peuvent être
incluses en utilisant la syntaxe

    ${ boilerplate() }

Les parties seront recherchées dans le répertoire contenant le modèle
principal, et seront supposées avoir la même extension que le modèle
principal si elles n'ont pas d'extension explicite. (Si les fichiers
partiels ne se trouvent pas ici, ils seront également recherchés dans le
sous-répertoire <span class="nowrap">`templates`</span>du répertoire des
données de l'utilisateur.)

Partials may optionally be applied to variables using a colon:

    ${ date:fancy() }

    ${ articles:bibentry() }

Si <span class="nowrap">`articles`</span> est un tableau, ceci va itérer
sur ses valeurs, en appliquant la partie <span
class="nowrap">`bibentry()`</span> à chacun. Le deuxième exemple
ci-dessus est donc équivalent à

    ${ for(articles) }
    ${ it:bibentry() }
    ${ endfor }

Notez que le mot-clé anaphorique <span class="nowrap">`it`</span> doit
être utilisé lors de l'itération sur les parties. Dans les exemples
ci-dessus, les parties <span class="nowrap">`bibentry`</span> devrait
contenir <span class="nowrap">`it.title`</span> (et ainsi de suite) au
lieu de <span class="nowrap">`articles.title`</span>.

Final newlines are omitted from included partials.

Partials may include other partials.

A separator between values of an array may be specified in square
brackets, immediately after the variable name or partial:

    ${months[, ]}$

    ${articles:bibentry()[; ]$

The separator in this case is literal and (unlike with <span
class="nowrap">`sep`</span> in an explicit <span
class="nowrap">`for`</span> loop) cannot contain interpolated variables
or other template directives.

### Nesting

Pour vous assurer que le contenu est "imbriqué", c'est-à-dire que les
lignes suivantes sont indentées, utilisez la directive ^ :

    $item.number$  $^$$item.description$ ($item.price$)

In this example, if <span class="nowrap">`item.description`</span> has
multiple lines, they will all be indented to line up with the first
line:

    00123  A fine bottle of 18-year old
           Oban whiskey. ($148)

To nest multiple lines to the same level, align them with the <span
class="nowrap">`^`</span> directive in the template. For example:

    $item.number$  $^$$item.description$ ($item.price$)
                   (Available til $item.sellby$.)

will produce

    00123  A fine bottle of 18-year old
           Oban whiskey. ($148)
           (Available til March 30, 2020.)

If a variable occurs by itself on a line, preceded by whitespace and not
followed by further text or directives on the same line, and the
variable’s value contains multiple lines, it will be nested
automatically.

### Breakable spaces

Normalement, les espaces dans le modèle lui-même (par opposition aux
valeurs des variables interpolées) ne sont pas sécables, mais ils
peuvent être rendus sécables dans une partie du modèle en utilisant le
mot-clé \~ (terminé par un autre \~).

    $~$This long line may break if the document is rendered
    with a short line length.$~$

### Pipes

Un tuyau ("pipe") transforme la valeur d'une variable ou d'une partie de
celle-ci. Les tuyaux sont spécifiés à l'aide d'une barre oblique (<span
class="nowrap">`/`</span>) entre le nom de la variable (ou partiel) et
le nom du pipe. Exemple :

    $for(name)$
    $name/uppercase$
    $endfor$

    $for(metadata/pairs)$
    - $it.key$: $it.value$
    $endfor$

    $employee:name()/uppercase$

Pipes may be chained:

    $for(employees/pairs)$
    $it.key/alpha/uppercase$. $it.name$
    $endfor$

Some pipes take parameters:

    |----------------------|------------|
    $for(employee)$
    $it.name.first/uppercase/left 20 "| "$$it.name.salary/right 10 " | " " |"$
    $endfor$
    |----------------------|------------|

Actuellement, les pipes suivants sont prédéfinis :

-   <span class="nowrap">`pairs`</span>: Convertit une carte ou un
    tableau en un tableau de cartes, chacune avec les champs <span
    class="nowrap">`key`</span>et <span class="nowrap">`value`</span>.
    Si la valeur initiale était un tableau, <span
    class="nowrap">`key`</span> sera l'index du tableau, commençant par
    1 .

-   <span class="nowrap">`uppercase`</span>: Convertit le texte en
    majuscules.

-   <span class="nowrap">`lowercase`</span>: Convertit le texte en
    minuscules.

-   <span class="nowrap">`length`</span>: Retourne la longueur de la
    valeur : nombre de caractères pour une valeur textuelle, nombre
    d'éléments pour une carte ou un tableau.

-   <span class="nowrap">`reverse`</span>: Inverse une valeur textuelle
    ou un tableau, et n'a aucun effet sur les autres valeurs.

-   <span class="nowrap">`chomp`</span>: Removes trailing newlines (and
    breakable space).

-   <span class="nowrap">`nowrap`</span>: Disables line wrapping on
    breakable spaces.

-   <span class="nowrap">`alpha`</span>: Converts textual values that
    can be read as an integer into lowercase alphabetic characters <span
    class="nowrap">`a..z`</span> (mod 26). This can be used to get
    lettered enumeration from array indices. To get uppercase letters,
    chain with <span class="nowrap">`uppercase`</span>.

-   <span class="nowrap">`roman`</span>: Converts textual values that
    can be read as an integer into lowercase roman numerials. This can
    be used to get lettered enumeration from array indices. To get
    uppercase roman, chain with <span class="nowrap">`uppercase`</span>.

-   <span class="nowrap">`left n "leftborder" "rightborder"`</span>:
    Renders a textual value in a block of width <span
    class="nowrap">`n`</span>, aligned to the left, with an optional
    left and right border. Has no effect on other values. This can be
    used to align material in tables. Widths are positive integers
    indicating the number of characters. Borders are strings inside
    double quotes; literal <span class="nowrap">`"`</span> and <span
    class="nowrap">`\`</span> characters must be backslash-escaped.

-   <span class="nowrap">`right n "leftborder" "rightborder"`</span>:
    Renders a textual value in a block of width <span
    class="nowrap">`n`</span>, aligned to the right, and has no effect
    on other values.

-   <span
    class="nowrap">`center n "leftborder"                       "rightborder"`</span>:
    Renders a textual value in a block of width <span
    class="nowrap">`n`</span>, aligned to the center, and has no effect
    on other values.

Variables
---------

### Variables de métadonnées

<span class="nowrap">`title`</span>, <span class="nowrap">`author`</span>, <span class="nowrap">`date`</span>  
permettent d'identifier les aspects fondamentaux du document. Inclus
dans les métadonnées PDF par le biais de LaTeX et ConTeXt. Ils peuvent
être définis au moyen d'un [pandoc title
block](#extension-pandoc_title_block), qui permet des auteurs multiples,
ou par le biais d'un [YAML metadata
block](#extension-yaml_metadata_block):

    ---
    author:
    - Aristotle
    - Peter Abelard
    ...

Notez que si vous souhaitez simplement définir des métadonnées PDF ou
HTML, sans inclure un bloc titre dans le document lui-même, vous pouvez
définir les variables <span class="nowrap">`title-meta`</span>, <span
class="nowrap">`author-meta`</span>, et <span
class="nowrap">`date-meta`</span>. (Par défaut, elles sont définies
automatiquement, sur la base <span class="nowrap">`title`</span>, <span
class="nowrap">`author`</span>, et <span class="nowrap">`date`</span>.)

<span class="nowrap">`subtitle`</span>  
sous-titre du document, inclus dans les documents HTML, EPUB, LaTeX,
ConTeXt et docx

<span class="nowrap">`abstract`</span>  
résumé du document, inclus dans les documents LaTeX, ConTeXt, AsciiDoc
et docx

<span class="nowrap">`keywords`</span>  
liste des mots clés à inclure dans les métadonnées HTML, PDF, ODT, pptx,
docx et AsciiDoc ; répéter comme pour <span
class="nowrap">`author`</span>ci-dessus, 

<span class="nowrap">`subject`</span>  
sujet du document, inclus dans les métadonnées ODT, PDF, docx et pptx

<span class="nowrap">`description`</span>  
description du document, incluse dans les métadonnées ODT, docx et pptx.
Certaines applications les présentent sous forme de la métadonnée <span
class="nowrap">`Comments`</span>.

<span class="nowrap">`category`</span>  
catégorie de document, incluse dans les métadonnées docx et pptx

En outre, toute métadonnée de chaîne de niveau racine, non incluse dans
les métadonnées ODT, docx ou pptx, est ajoutée comme propriété
personnalisée. Le bloc de métadonnées YAML suivant, par exemple :

    ---
    title:  'This is the title'
    subtitle: "This is the subtitle"
    author:
    - Author One
    - Author Two
    description: |
        This is a long
        description.

        It consists of two paragraphs
    ...

inclura <span class="nowrap">`title`</span>, <span
class="nowrap">`author`</span> et <span
class="nowrap">`description`</span> en tant que propriétés de document
standard et <span class="nowrap">`subtitle`</span> comme une propriété
personnalisée lors de la conversion en docx, ODT ou pptx.

### Variables linguistiques

<span class="nowrap">`lang`</span>  
identifies the main language of the document using IETF language tags
(following the [BCP 47](https://tools.ietf.org/html/bcp47) standard),
such as <span class="nowrap">`en`</span> or <span
class="nowrap">`en-GB`</span>. The [Language subtag
lookup](https://r12a.github.io/app-subtags/) tool can look up or verify
these tags. This affects most formats, and controls hyphenation in PDF
output when using LaTeX (through [<span
class="nowrap">`babel`</span>](https://ctan.org/pkg/babel) and [<span
class="nowrap">`polyglossia`</span>](https://ctan.org/pkg/polyglossia))
or ConTeXt.

Use native pandoc [Divs and Spans](#divs-and-spans) with the <span
class="nowrap">`lang`</span> attribute to switch the language:

    ---
    lang: en-GB
    ...

    Text in the main document language (British English).

    ::: {lang=fr-CA}
    > Cette citation est écrite en français canadien.
    :::

    More text in English. ['Zitat auf Deutsch.']{lang=de}

<span class="nowrap">`dir`</span>  
the base script direction, either <span class="nowrap">`rtl`</span>
(right-to-left) or <span class="nowrap">`ltr`</span> (left-to-right).

For bidirectional documents, native pandoc <span
class="nowrap">`span`</span>s and <span class="nowrap">`div`</span>s
with the <span class="nowrap">`dir`</span> attribute (value <span
class="nowrap">`rtl`</span> or <span class="nowrap">`ltr`</span>) can be
used to override the base direction in some output formats. This may not
always be necessary if the final renderer (e.g. the browser, when
generating HTML) supports the [Unicode Bidirectional
Algorithm](https://www.w3.org/International/articles/inline-bidi-markup/uba-basics).

When using LaTeX for bidirectional documents, only the <span
class="nowrap">`xelatex`</span> engine is fully supported (use
<a href="#option--pdf-engine" class="option"><span class="nowrap"><code>--pdf-engine=xelatex</code></span></a>).

### Variables for HTML math

<span class="nowrap">`classoption`</span>  
when using [KaTeX](#option--katex), you can render display math
equations flush left using [YAML metadata](#layout) or with
<a href="#option--metadata" class="option"><span class="nowrap"><code>-M classoption=fleqn</code></span></a>.

### Variables for HTML slides

These affect HTML output when [producing slide shows with
pandoc](#producing-slide-shows-with-pandoc).

All [reveal.js configuration
options](https://github.com/hakimel/reveal.js#configuration) are
available as variables. To turn off boolean flags that default to true
in reveal.js, use <span class="nowrap">`0`</span>.

<span class="nowrap">`revealjs-url`</span>  
base URL for reveal.js documents (defaults to <span
class="nowrap">`reveal.js`</span>)

<span class="nowrap">`s5-url`</span>  
base URL for S5 documents (defaults to <span
class="nowrap">`s5/default`</span>)

<span class="nowrap">`slidy-url`</span>  
base URL for Slidy documents (defaults to <span
class="nowrap">`https://www.w3.org/Talks/Tools/Slidy2`</span>)

<span class="nowrap">`slideous-url`</span>  
base URL for Slideous documents (defaults to <span
class="nowrap">`slideous`</span>)

<span class="nowrap">`title-slide-attributes`</span>  
additional attributes for the title slide of reveal.js slide shows. See
[background in reveal.js and
beamer](#background-in-reveal.js-and-beamer) for an example.

### Variables for Beamer slides

These variables change the appearance of PDF slides using [<span
class="nowrap">`beamer`</span>](https://ctan.org/pkg/beamer).

<span class="nowrap">`aspectratio`</span>  
slide aspect ratio (<span class="nowrap">`43`</span> for 4:3
\[default\], <span class="nowrap">`169`</span> for 16:9, <span
class="nowrap">`1610`</span> for 16:10, <span
class="nowrap">`149`</span> for 14:9, <span class="nowrap">`141`</span>
for 1.41:1, <span class="nowrap">`54`</span> for 5:4, <span
class="nowrap">`32`</span> for 3:2)

<span class="nowrap">`beamerarticle`</span>  
produce an article from Beamer slides

<span class="nowrap">`beameroption`</span>  
add extra beamer option with <span
class="nowrap">`\setbeameroption{}`</span>

<span class="nowrap">`institute`</span>  
author affiliations: can be a list when there are multiple authors

<span class="nowrap">`logo`</span>  
logo image for slides

<span class="nowrap">`navigation`</span>  
controls navigation symbols (default is <span
class="nowrap">`empty`</span> for no navigation symbols; other valid
values are <span class="nowrap">`frame`</span>, <span
class="nowrap">`vertical`</span>, and <span
class="nowrap">`horizontal`</span>)

<span class="nowrap">`section-titles`</span>  
enables “title pages” for new sections (default is true)

<span class="nowrap">`theme`</span>, <span class="nowrap">`colortheme`</span>, <span class="nowrap">`fonttheme`</span>, <span class="nowrap">`innertheme`</span>, <span class="nowrap">`outertheme`</span>  
beamer themes

<span class="nowrap">`themeoptions`</span>  
options for LaTeX beamer themes (a list).

<span class="nowrap">`titlegraphic`</span>  
image for title slide

### Variables for PowerPoint

These variables control the visual aspects of a slide show that are not
easily controlled via templates.

<span class="nowrap">`monofont`</span>  
font to use for code.

### Variables pour LaTeX

Pandoc utilise ces variables pour [créer un PDF](#creating-a-pdf) avec
un moteur LaTeX.

#### Mise en page

<span class="nowrap">`block-headings`</span>  
faire des <span class="nowrap">`\paragraph`</span> et <span
class="nowrap">`\subparagraph`</span> (fourth- and fifth-level headings,
or fifth- and sixth-level with book classes) autonome plutôt que de
dépendre des autres niveaux ; nécessite un formatage supplémentaire pour
distinguer <span class="nowrap">`\subsubsection`</span> (third- or
fourth-level headings). Au lieu d'utiliser cette option,
[KOMA-Script](https://ctan.org/pkg/koma-script) peut ajuster les
rubriques de manière plus approfondie:

    ---
    documentclass: scrartcl
    header-includes: |
      \RedeclareSectionCommand[
        beforeskip=-10pt plus -2pt minus -1pt,
        afterskip=1sp plus -1sp minus 1sp,
        font=\normalfont\itshape]{paragraph}
      \RedeclareSectionCommand[
        beforeskip=-10pt plus -2pt minus -1pt,
        afterskip=1sp plus -1sp minus 1sp,
        font=\normalfont\scshape,
        indent=0pt]{subparagraph}
    ...

<span class="nowrap">`classoption`</span>  
option for document class, e.g. <span class="nowrap">`oneside`</span>;
repeat for multiple options:

    ---
    classoption:
    - twocolumn
    - landscape
    ...

<span class="nowrap">`documentclass`</span>  
document class: usually one of the standard classes, [<span
class="nowrap">`article`</span>](https://ctan.org/pkg/article), [<span
class="nowrap">`book`</span>](https://ctan.org/pkg/book), and [<span
class="nowrap">`report`</span>](https://ctan.org/pkg/report); the
[KOMA-Script](https://ctan.org/pkg/koma-script) equivalents, <span
class="nowrap">`scrartcl`</span>, <span class="nowrap">`scrbook`</span>,
and <span class="nowrap">`scrreprt`</span>, which default to smaller
margins; or [<span
class="nowrap">`memoir`</span>](https://ctan.org/pkg/memoir)

<span class="nowrap">`geometry`</span>  
option for [<span
class="nowrap">`geometry`</span>](https://ctan.org/pkg/geometry)
package, e.g. <span class="nowrap">`margin=1in`</span>; repeat for
multiple options:

    ---
    geometry:
    - top=30mm
    - left=20mm
    - heightrounded
    ...

<span class="nowrap">`hyperrefoptions`</span>  
option for [<span
class="nowrap">`hyperref`</span>](https://ctan.org/pkg/hyperref)
package, e.g. <span class="nowrap">`linktoc=all`</span>; repeat for
multiple options:

    ---
    hyperrefoptions:
    - linktoc=all
    - pdfwindowui
    - pdfpagemode=FullScreen
    ...

<span class="nowrap">`indent`</span>  
uses document class settings for indentation (the default LaTeX template
otherwise removes indentation and adds space between paragraphs)

<span class="nowrap">`linestretch`</span>  
adjusts line spacing using the [<span
class="nowrap">`setspace`</span>](https://ctan.org/pkg/setspace)
package, e.g. <span class="nowrap">`1.25`</span>, <span
class="nowrap">`1.5`</span>

<span class="nowrap">`margin-left`</span>, <span class="nowrap">`margin-right`</span>, <span class="nowrap">`margin-top`</span>, <span class="nowrap">`margin-bottom`</span>  
sets margins if <span class="nowrap">`geometry`</span> is not used
(otherwise <span class="nowrap">`geometry`</span> overrides these)

<span class="nowrap">`pagestyle`</span>  
control <span class="nowrap">`\pagestyle{}`</span>: the default article
class supports <span class="nowrap">`plain`</span> (default), <span
class="nowrap">`empty`</span> (no running heads or page numbers), and
<span class="nowrap">`headings`</span> (section titles in running heads)

<span class="nowrap">`papersize`</span>  
paper size, e.g. <span class="nowrap">`letter`</span>, <span
class="nowrap">`a4`</span>

<span class="nowrap">`secnumdepth`</span>  
numbering depth for sections (with
<a href="#option--number-sections" class="option"><span class="nowrap"><code>--number-sections</code></span></a>
option or <span class="nowrap">`numbersections`</span> variable)

#### Fonts

<span class="nowrap">`fontenc`</span>  
allows font encoding to be specified through <span
class="nowrap">`fontenc`</span> package (with <span
class="nowrap">`pdflatex`</span>); default is <span
class="nowrap">`T1`</span> (see [LaTeX font encodings
guide](https://ctan.org/pkg/encguide))

<span class="nowrap">`fontfamily`</span>  
font package for use with <span class="nowrap">`pdflatex`</span>: [TeX
Live](https://www.tug.org/texlive/) includes many options, documented in
the [LaTeX Font Catalogue](https://tug.org/FontCatalogue/). The default
is [Latin Modern](https://ctan.org/pkg/lm).

<span class="nowrap">`fontfamilyoptions`</span>  
options for package used as <span class="nowrap">`fontfamily`</span>;
repeat for multiple options. For example, to use the Libertine font with
proportional lowercase (old-style) figures through the [<span
class="nowrap">`libertinus`</span>](https://ctan.org/pkg/libertinus)
package:

    ---
    fontfamily: libertinus
    fontfamilyoptions:
    - osf
    - p
    ...

<span class="nowrap">`fontsize`</span>  
font size for body text. The standard classes allow 10pt, 11pt, and
12pt. To use another size, set <span
class="nowrap">`documentclass`</span> to one of the
[KOMA-Script](https://ctan.org/pkg/koma-script) classes, such as <span
class="nowrap">`scrartcl`</span> or <span
class="nowrap">`scrbook`</span>.

<span class="nowrap">`mainfont`</span>, <span class="nowrap">`sansfont`</span>, <span class="nowrap">`monofont`</span>, <span class="nowrap">`mathfont`</span>, <span class="nowrap">`CJKmainfont`</span>  
font families for use with <span class="nowrap">`xelatex`</span> or
<span class="nowrap">`lualatex`</span>: take the name of any system
font, using the [<span
class="nowrap">`fontspec`</span>](https://ctan.org/pkg/fontspec)
package. <span class="nowrap">`CJKmainfont`</span> uses the [<span
class="nowrap">`xecjk`</span>](https://ctan.org/pkg/xecjk) package.

<span class="nowrap">`mainfontoptions`</span>, <span class="nowrap">`sansfontoptions`</span>, <span class="nowrap">`monofontoptions`</span>, <span class="nowrap">`mathfontoptions`</span>, <span class="nowrap">`CJKoptions`</span>  
options to use with <span class="nowrap">`mainfont`</span>, <span
class="nowrap">`sansfont`</span>, <span
class="nowrap">`monofont`</span>, <span
class="nowrap">`mathfont`</span>, <span
class="nowrap">`CJKmainfont`</span> in <span
class="nowrap">`xelatex`</span> and <span
class="nowrap">`lualatex`</span>. Allow for any choices available
through [<span
class="nowrap">`fontspec`</span>](https://ctan.org/pkg/fontspec); repeat
for multiple options. For example, to use the [TeX
Gyre](http://www.gust.org.pl/projects/e-foundry/tex-gyre) version of
Palatino with lowercase figures:

    ---
    mainfont: TeX Gyre Pagella
    mainfontoptions:
    - Numbers=Lowercase
    - Numbers=Proportional
    ...

<span class="nowrap">`microtypeoptions`</span>  
options to pass to the microtype package

#### Links

<span class="nowrap">`colorlinks`</span>  
add color to link text; automatically enabled if any of <span
class="nowrap">`linkcolor`</span>, <span
class="nowrap">`filecolor`</span>, <span
class="nowrap">`citecolor`</span>, <span
class="nowrap">`urlcolor`</span>, or <span
class="nowrap">`toccolor`</span> are set

<span class="nowrap">`linkcolor`</span>, <span class="nowrap">`filecolor`</span>, <span class="nowrap">`citecolor`</span>, <span class="nowrap">`urlcolor`</span>, <span class="nowrap">`toccolor`</span>  
color for internal links, external links, citation links, linked URLs,
and links in table of contents, respectively: uses options allowed by
[<span class="nowrap">`xcolor`</span>](https://ctan.org/pkg/xcolor),
including the <span class="nowrap">`dvipsnames`</span>, <span
class="nowrap">`svgnames`</span>, and <span
class="nowrap">`x11names`</span> lists

<span class="nowrap">`links-as-notes`</span>  
causes links to be printed as footnotes

#### Front matter

<span class="nowrap">`lof`</span>, <span class="nowrap">`lot`</span>  
include list of figures, list of tables

<span class="nowrap">`thanks`</span>  
contents of acknowledgments footnote after document title

<span class="nowrap">`toc`</span>  
include table of contents (can also be set using
<a href="#option--toc" class="option"><span class="nowrap"><code>--toc/--table-of-contents</code></span></a>)

<span class="nowrap">`toc-depth`</span>  
level of section to include in table of contents

#### BibLaTeX Bibliographies

These variables function when using BibLaTeX for [citation
rendering](#citation-rendering).

<span class="nowrap">`biblatexoptions`</span>  
list of options for biblatex

<span class="nowrap">`biblio-style`</span>  
bibliography style, when used with
<a href="#option--natbib" class="option"><span class="nowrap"><code>--natbib</code></span></a>
and
<a href="#option--biblatex" class="option"><span class="nowrap"><code>--biblatex</code></span></a>.

<span class="nowrap">`biblio-title`</span>  
bibliography title, when used with
<a href="#option--natbib" class="option"><span class="nowrap"><code>--natbib</code></span></a>
and
<a href="#option--biblatex" class="option"><span class="nowrap"><code>--biblatex</code></span></a>.

<span class="nowrap">`bibliography`</span>  
bibliography to use for resolving references

<span class="nowrap">`natbiboptions`</span>  
list of options for natbib

### Variables for ConTeXt

Pandoc uses these variables when [creating a PDF](#creating-a-pdf) with
ConTeXt.

<span class="nowrap">`fontsize`</span>  
font size for body text (e.g. <span class="nowrap">`10pt`</span>, <span
class="nowrap">`12pt`</span>)

<span class="nowrap">`headertext`</span>, <span class="nowrap">`footertext`</span>  
text to be placed in running header or footer (see [ConTeXt Headers and
Footers](https://wiki.contextgarden.net/Headers_and_Footers)); repeat up
to four times for different placement

<span class="nowrap">`indenting`</span>  
controls indentation of paragraphs, e.g. <span
class="nowrap">`yes,small,next`</span> (see [ConTeXt
Indentation](https://wiki.contextgarden.net/Indentation)); repeat for
multiple options

<span class="nowrap">`interlinespace`</span>  
adjusts line spacing, e.g. <span class="nowrap">`4ex`</span> (using
[<span
class="nowrap">`setupinterlinespace`</span>](https://wiki.contextgarden.net/Command/setupinterlinespace));
repeat for multiple options

<span class="nowrap">`layout`</span>  
options for page margins and text arrangement (see [ConTeXt
Layout](https://wiki.contextgarden.net/Layout)); repeat for multiple
options

<span class="nowrap">`linkcolor`</span>, <span class="nowrap">`contrastcolor`</span>  
color for links outside and inside a page, e.g. <span
class="nowrap">`red`</span>, <span class="nowrap">`blue`</span> (see
[ConTeXt Color](https://wiki.contextgarden.net/Color))

<span class="nowrap">`linkstyle`</span>  
typeface style for links, e.g. <span class="nowrap">`normal`</span>,
<span class="nowrap">`bold`</span>, <span
class="nowrap">`slanted`</span>, <span
class="nowrap">`boldslanted`</span>, <span class="nowrap">`type`</span>,
<span class="nowrap">`cap`</span>, <span class="nowrap">`small`</span>

<span class="nowrap">`lof`</span>, <span class="nowrap">`lot`</span>  
include list of figures, list of tables

<span class="nowrap">`mainfont`</span>, <span class="nowrap">`sansfont`</span>, <span class="nowrap">`monofont`</span>, <span class="nowrap">`mathfont`</span>  
font families: take the name of any system font (see [ConTeXt Font
Switching](https://wiki.contextgarden.net/Font_Switching))

<span class="nowrap">`margin-left`</span>, <span class="nowrap">`margin-right`</span>, <span class="nowrap">`margin-top`</span>, <span class="nowrap">`margin-bottom`</span>  
sets margins, if <span class="nowrap">`layout`</span> is not used
(otherwise <span class="nowrap">`layout`</span> overrides these)

<span class="nowrap">`pagenumbering`</span>  
page number style and location (using [<span
class="nowrap">`setuppagenumbering`</span>](https://wiki.contextgarden.net/Command/setuppagenumbering));
repeat for multiple options

<span class="nowrap">`papersize`</span>  
paper size, e.g. <span class="nowrap">`letter`</span>, <span
class="nowrap">`A4`</span>, <span class="nowrap">`landscape`</span> (see
[ConTeXt Paper Setup](https://wiki.contextgarden.net/PaperSetup));
repeat for multiple options

<span class="nowrap">`pdfa`</span>  
adds to the preamble the setup necessary to generate PDF/A of the type
specified, e.g. <span class="nowrap">`1a:2005`</span>, <span
class="nowrap">`2a`</span>. If no type is specified (i.e. the value is
set to True, by
e.g. <a href="#option--metadata" class="option"><span class="nowrap"><code>--metadata=pdfa</code></span></a>
or <span class="nowrap">`pdfa: true`</span> in a YAML metadata block),
<span class="nowrap">`1b:2005`</span> will be used as default, for
reasons of backwards compatibility. Using
<a href="#option--variable" class="option"><span class="nowrap"><code>--variable=pdfa</code></span></a>
without specified value is not supported. To successfully generate PDF/A
the required ICC color profiles have to be available and the content and
all included files (such as images) have to be standard conforming. The
ICC profiles and output intent may be specified using the variables
<span class="nowrap">`pdfaiccprofile`</span> and <span
class="nowrap">`pdfaintent`</span>. See also [ConTeXt
PDFA](https://wiki.contextgarden.net/PDF/A) for more details.

<span class="nowrap">`pdfaiccprofile`</span>  
when used in conjunction with <span class="nowrap">`pdfa`</span>,
specifies the ICC profile to use in the PDF, e.g. <span
class="nowrap">`default.cmyk`</span>. If left unspecified, <span
class="nowrap">`sRGB.icc`</span> is used as default. May be repeated to
include multiple profiles. Note that the profiles have to be available
on the system. They can be obtained from [ConTeXt ICC
Profiles](https://wiki.contextgarden.net/PDFX#ICC_profiles).

<span class="nowrap">`pdfaintent`</span>  
when used in conjunction with <span class="nowrap">`pdfa`</span>,
specifies the output intent for the colors, e.g. <span
class="nowrap">`ISO                     coated v2 300\letterpercent\space (ECI)`</span>
If left unspecified, <span class="nowrap">`sRGB IEC61966-2.1`</span> is
used as default.

<span class="nowrap">`toc`</span>  
include table of contents (can also be set using
<a href="#option--toc" class="option"><span class="nowrap"><code>--toc/--table-of-contents</code></span></a>)

<span class="nowrap">`whitespace`</span>  
spacing between paragraphs, e.g. <span class="nowrap">`none`</span>,
<span class="nowrap">`small`</span> (using [<span
class="nowrap">`setupwhitespace`</span>](https://wiki.contextgarden.net/Command/setupwhitespace))

<span class="nowrap">`includesource`</span>  
include all source documents as file attachments in the PDF file

### Variables for <span class="nowrap">`wkhtmltopdf`</span>

Pandoc uses these variables when [creating a PDF](#creating-a-pdf) with
[<span class="nowrap">`wkhtmltopdf`</span>](https://wkhtmltopdf.org/).
The
<a href="#option--css" class="option"><span class="nowrap"><code>--css</code></span></a>
option also affects the output.

<span class="nowrap">`footer-html`</span>, <span class="nowrap">`header-html`</span>  
add information to the header and footer

<span class="nowrap">`margin-left`</span>, <span class="nowrap">`margin-right`</span>, <span class="nowrap">`margin-top`</span>, <span class="nowrap">`margin-bottom`</span>  
set the page margins

<span class="nowrap">`papersize`</span>  
sets the PDF paper size

### Variables for man pages

<span class="nowrap">`adjusting`</span>  
adjusts text to left (<span class="nowrap">`l`</span>), right (<span
class="nowrap">`r`</span>), center (<span class="nowrap">`c`</span>), or
both (<span class="nowrap">`b`</span>) margins

<span class="nowrap">`footer`</span>  
footer in man pages

<span class="nowrap">`header`</span>  
header in man pages

<span class="nowrap">`hyphenate`</span>  
if <span class="nowrap">`true`</span> (the default), hyphenation will be
used

<span class="nowrap">`section`</span>  
section number in man pages

### Variables for ms

<span class="nowrap">`fontfamily`</span>  
font family (e.g. <span class="nowrap">`T`</span> or <span
class="nowrap">`P`</span>)

<span class="nowrap">`indent`</span>  
paragraph indent (e.g. <span class="nowrap">`2m`</span>)

<span class="nowrap">`lineheight`</span>  
line height (e.g. <span class="nowrap">`12p`</span>)

<span class="nowrap">`pointsize`</span>  
point size (e.g. <span class="nowrap">`10p`</span>)

### Variables set automatically

Pandoc sets these variables automatically in response to
[options](#options) or document contents; users can also modify them.
These vary depending on the output format, and include the following:

<span class="nowrap">`body`</span>  
body of document

<span class="nowrap">`date-meta`</span>  
the <span class="nowrap">`date`</span> variable converted to ISO 8601
YYYY-MM-DD, included in all HTML based formats (dzslides, epub, html,
html4, html5, revealjs, s5, slideous, slidy). The recognized formats for
<span class="nowrap">`date`</span> are: <span
class="nowrap">`mm/dd/yyyy`</span>, <span
class="nowrap">`mm/dd/yy`</span>, <span
class="nowrap">`yyyy-mm-dd`</span> (ISO 8601), <span
class="nowrap">`dd MM yyyy`</span> (e.g. either <span
class="nowrap">`02 Apr 2018`</span> or <span
class="nowrap">`02 April 2018`</span>), <span
class="nowrap">`MM dd, yyyy`</span> (e.g. <span
class="nowrap">`Apr. 02, 2018`</span> or <span
class="nowrap">`April                     02, 2018),`</span>yyyy\[mm\[dd\]\]\]<span
class="nowrap">`(e.g.`</span>20180402, <span
class="nowrap">`201804`</span> or <span class="nowrap">`2018`</span>).

<span class="nowrap">`header-includes`</span>  
contents specified by
<a href="#option--include-in-header" class="option"><span class="nowrap"><code>-H/--include-in-header</code></span></a>
(may have multiple values)

<span class="nowrap">`include-before`</span>  
contents specified by
<a href="#option--include-before-body" class="option"><span class="nowrap"><code>-B/--include-before-body</code></span></a>
(may have multiple values)

<span class="nowrap">`include-after`</span>  
contents specified by
<a href="#option--include-after-body" class="option"><span class="nowrap"><code>-A/--include-after-body</code></span></a>
(may have multiple values)

<span class="nowrap">`meta-json`</span>  
JSON representation of all of the document’s metadata. Field values are
transformed to the selected output format.

<span class="nowrap">`numbersections`</span>  
non-null value if
<a href="#option--number-sections" class="option"><span class="nowrap"><code>-N/--number-sections</code></span></a>
was specified

<span class="nowrap">`sourcefile`</span>, <span class="nowrap">`outputfile`</span>  
les noms de fichiers source et destination, tels qu'ils sont indiqués
sur la ligne de commande. <span class="nowrap">`sourcefile`</span> peut
également être une liste si la saisie provient de plusieurs fichiers, ou
vide si la saisie provient de stdin. Vous pouvez utiliser l'extrait
suivant dans votre modèle pour les distinguer :

    $if(sourcefile)$
    $for(sourcefile)$
    $sourcefile$
    $endfor$
    $else$
    (stdin)
    $endif$

Similarly, <span class="nowrap">`outputfile`</span> can be <span
class="nowrap">`-`</span> if output goes to the terminal.

If you need absolute paths, use e.g. <span
class="nowrap">`$curdir$/$sourcefile$`</span>.

<span class="nowrap">`curdir`</span>  
working directory from which pandoc is run.

<span class="nowrap">`toc`</span>  
non-null value if
<a href="#option--toc" class="option"><span class="nowrap"><code>--toc/--table-of-contents</code></span></a>
was specified

<span class="nowrap">`toc-title`</span>  
title of table of contents (works only with EPUB, HTML, opendocument,
odt, docx, pptx, beamer, LaTeX)

Extensions
==========

Le comportement de certains lecteurs et "writers" peut être ajusté en
activant ou en désactivant diverses extensions.  
  
Une extension peut être activée en ajoutant <span
class="nowrap">`+EXTENSION`</span> au nom du format et désactivé en
ajoutant  <span class="nowrap">`-EXTENSION`</span>Par exemple,
<a href="#option--from" class="option"><span class="nowrap"><code>--from                   markdown_strict+footnotes</code></span></a>
est du Markdown strict avec les notes de bas de page activées, tandis
que
<a href="#option--from" class="option"><span class="nowrap"><code>--from                   markdown-footnotes-pipe_tables</code></span></a>est
le Markdown de pandoc sans les notes de bas de page, ni les
"pipe-tables".

Le lecteur et le créateur Markdown sont de loin ceux qui utilisent le
plus les extensions. Les extensions qu'ils utilisent seuls sont donc
couvertes dans la section [Pandoc’s Markdown](#pandocs-markdown)
ci-dessous  (Voir [Markdown variants](#markdown-variants) pour <span
class="nowrap">`commonmark`</span> et <span
class="nowrap">`gfm`</span>.)  Dans ce qui suit, les extensions qui
fonctionnent également pour d'autres formats sont couvertes.  
  
Notez que les extensions Markdown ajoutées au format <span
class="nowrap">`ipynb`</span>affectent les cellules Markdown des
notebooks Jupiler (comme les options en ligne de commande telles que
<a href="#option--atx-headers" class="option"><span class="nowrap"><code>--atx-headers</code></span></a>).

Typographie
-----------

#### Extension: <span class="nowrap">`smart`</span>

Interpret straight quotes as curly quotes, <span
class="nowrap">`---`</span> as em-dashes, <span
class="nowrap">`--`</span> as en-dashes, and <span
class="nowrap">`...`</span> as ellipses. Nonbreaking spaces are inserted
after certain abbreviations, such as “Mr.”

Cette extension peut être activée/désactivée pour les formats suivants :

input formats  
<span class="nowrap">`markdown`</span>, <span
class="nowrap">`commonmark`</span>, <span class="nowrap">`latex`</span>,
<span class="nowrap">`mediawiki`</span>, <span
class="nowrap">`org`</span>, <span class="nowrap">`rst`</span>, <span
class="nowrap">`twiki`</span>

output formats  
<span class="nowrap">`markdown`</span>, <span
class="nowrap">`latex`</span>, <span class="nowrap">`context`</span>,
<span class="nowrap">`rst`</span>

enabled by default in  
<span class="nowrap">`markdown`</span>, <span
class="nowrap">`latex`</span>, <span class="nowrap">`context`</span>
(both input and output)

Note: Si vous *convertissez en* Markdown, alors l' extension <span
class="nowrap">`smart`</span>a l'effet inverse : what would have been
curly quotes comes out straight.

In LaTeX, <span class="nowrap">`smart`</span> means to use the standard
TeX ligatures for quotation marks (<span
class="nowrap">``` `` ```</span> and <span class="nowrap">`''`</span>
for double quotes, <span class="nowrap">`` ` ``</span> and <span
class="nowrap">`'`</span> for single quotes) and dashes (<span
class="nowrap">`--`</span> for en-dash and <span
class="nowrap">`---`</span> for em-dash). If <span
class="nowrap">`smart`</span> is disabled, then in reading LaTeX pandoc
will parse these characters literally. In writing LaTeX, enabling <span
class="nowrap">`smart`</span> tells pandoc to use the ligatures when
possible; if <span class="nowrap">`smart`</span> is disabled pandoc will
use unicode quotation mark and dash characters.

Headings and sections
---------------------

#### Extension: <span class="nowrap">`auto_identifiers`</span>

Une rubrique sans identifiant explicitement spécifié se verra
automatiquement attribuer un identifiant unique basé sur le texte de la
rubrique.  
  
Cette extension peut être activée/désactivée pour les formats suivants :

input formats  
<span class="nowrap">`markdown`</span>, <span
class="nowrap">`latex`</span>, <span class="nowrap">`rst`</span>, <span
class="nowrap">`mediawiki`</span>, <span class="nowrap">`textile`</span>

output formats  
<span class="nowrap">`markdown`</span>, <span
class="nowrap">`muse`</span>

enabled by default in  
<span class="nowrap">`markdown`</span>, <span
class="nowrap">`muse`</span>

L'algorithme par défaut utilisé pour dériver l'identifiant à partir du
texte de l'en-tête est :

-   Supprimer tout formatage, liens, etc..
-   Supprimer toutes les notes de bas de page.
-   Supprimer tous les caractères non alphanumériques, à l'exception des
    traits de soulignement, des traits d'union et des points.
-   Remplacer tous les espaces et les nouvelles lignes par des traits
    d'union.
-   Convertir tous les caractères alphabétiques en minuscules.
-   Supprimer tout jusqu'à la première lettre (les identifiants ne
    peuvent pas commencer par un chiffre ou un signe de ponctuation).
-   S'il ne reste rien après cela, utilisez l'identifiant <span
    class="nowrap">`section`</span>.

Thus, for example,

| Heading                                                                           | Identifier                                                |
|:----------------------------------------------------------------------------------|:----------------------------------------------------------|
| <span class="nowrap">`Heading                         identifiers in HTML`</span> | <span class="nowrap">`heading-identifiers-in-html`</span> |
| <span class="nowrap">`Maître                         d'hôtel`</span>              | <span class="nowrap">`maître-dhôtel`</span>               |
| <span class="nowrap">`*Dogs*?--in                         *my* house?`</span>     | <span class="nowrap">`dogs--in-my-house`</span>           |
| <span class="nowrap">`[HTML],                         [S5], or [RTF]?`</span>     | <span class="nowrap">`html-s5-or-rtf`</span>              |
| <span class="nowrap">`3.                         Applications`</span>             | <span class="nowrap">`applications`</span>                |
| <span class="nowrap">`33`</span>                                                  | <span class="nowrap">`section`</span>                     |

Ces règles devraient, dans la plupart des cas, permettre de déterminer
l'identifiant à partir du texte de l'en-tête. L'exception est lorsque
plusieurs rubriques ont le même texte ; dans ce cas, la première
obtiendra un identifiant comme décrit ci-dessus ; la seconde obtiendra
le même identifiant en y suffixant <span class="nowrap">`-1`</span>; le
troisième avec <span class="nowrap">`-2`</span>; etc...

(Toutefois, un algorithme différent est utilisé si <span
class="nowrap">`gfm_auto_identifiers`</span> est activée ; voir
ci-dessous .)

Ces identificateurs sont utilisés pour fournir des cibles de liens dans
la table des matières générée par l'option
<a href="#option--toc" class="option"><span class="nowrap"><code>--toc|--table-of-contents</code></span></a>.
Ils permettent également de fournir facilement des liens d'une section
d'un document à une autre. Un lien vers cette section, par exemple,
pourrait ressembler à ceci :

    See the section on
    [heading identifiers](#heading-identifiers-in-html-latex-and-context).

Notez toutefois que cette méthode de fourniture de liens vers les
sections ne fonctionne qu'aux formats HTML, LaTeX et ConTeXt.

Si
l'option<a href="#option--section-divs" class="option"><span class="nowrap"><code>--section-divs</code></span></a>est
spécifiée, alors chaque section sera incluse dans une <span
class="nowrap">`section`</span> (ou une <span
class="nowrap">`div`</span>, si <span class="nowrap">`html4`</span> a
été spécifié), et l'identifiant sera joint comme étiquette de <span
class="nowrap">`<section>`</span> (ou <span
class="nowrap">`<div>`</span>) plutôt que l'en-tête lui-même. Cela
permet de manipuler des sections entières à l'aide de JavaScript ou de
les traiter différemment en CSS.

#### Extension: <span class="nowrap">`ascii_identifiers`</span>

Causes the identifiers produced by <span
class="nowrap">`auto_identifiers`</span> to be pure ASCII. Accents are
stripped off of accented Latin letters, and non-Latin letters are
omitted.

#### Extension: <span class="nowrap">`gfm_auto_identifiers`</span>

Change l'algorithme utilisé par <span
class="nowrap">`auto_identifiers`</span> pour correspondre à la méthode
GitHub. Les espaces sont convertis en tirets (<span
class="nowrap">`-`</span>), des caractères majuscules aux caractères
minuscules, et des caractères de ponctuation autres que <span
class="nowrap">`-`</span> et <span class="nowrap">`_`</span> sont
enlevés. Les emojis sont remplacés par leur nom.

Math Input
----------

Les extensions [<span
class="nowrap">`tex_math_dollars`</span>](#extension-tex_math_dollars),
[<span
class="nowrap">`tex_math_single_backslash`</span>](#extension-tex_math_single_backslash)et, 
[<span
class="nowrap">`tex_math_double_backslash`</span>](#extension-tex_math_double_backslash)
sont décrits dans la section consacrée à Markdown Pandoc.  
  
Cependant, ils peuvent également être utilisés avec une entrée HTML.
C'est pratique pour lire des pages web formatées avec MathJax, par
exemple.

Raw HTML/TeX
------------

The following extensions (especially how they affect Markdown
input/output) are also described in more detail in their respective
sections of [Pandoc’s Markdown](#pandocs-markdown).

#### Extension: <span class="nowrap">`raw_html`</span>

When converting from HTML, parse elements to raw HTML which are not
representable in pandoc’s AST. By default, this is disabled for HTML
input.

#### Extension: <span class="nowrap">`raw_tex`</span>

Allows raw LaTeX, TeX, and ConTeXt to be included in a document.

This extension can be enabled/disabled for the following formats (in
addition to <span class="nowrap">`markdown`</span>):

input formats  
<span class="nowrap">`latex`</span>, <span class="nowrap">`org`</span>,
<span class="nowrap">`textile`</span>, <span
class="nowrap">`html`</span> (environments, <span
class="nowrap">`\ref`</span>, and <span class="nowrap">`\eqref`</span>
only), <span class="nowrap">`ipynb`</span>

output formats  
<span class="nowrap">`textile`</span>, <span
class="nowrap">`commonmark`</span>

Note: as applied to <span class="nowrap">`ipynb`</span>, <span
class="nowrap">`raw_html`</span> and <span
class="nowrap">`raw_tex`</span> affect not only raw TeX in markdown
cells, but data with mime type <span class="nowrap">`text/html`</span>
in output cells. Since the <span class="nowrap">`ipynb`</span> reader
attempts to preserve the richest possible outputs when several options
are given, you will get best results if you disable <span
class="nowrap">`raw_html`</span> and <span
class="nowrap">`raw_tex`</span> when converting to formats like <span
class="nowrap">`docx`</span> which don’t allow raw <span
class="nowrap">`html`</span> or <span class="nowrap">`tex`</span>.

#### Extension: <span class="nowrap">`native_divs`</span>

This extension is enabled by default for HTML input. This means that
<span class="nowrap">`div`</span>s are parsed to pandoc native elements.
(Alternatively, you can parse them to raw HTML using
<a href="#option--from" class="option"><span class="nowrap"><code>-f                     html-native_divs+raw_html</code></span></a>.)

When converting HTML to Markdown, for example, you may want to drop all
<span class="nowrap">`div`</span>s and <span
class="nowrap">`span`</span>s:

    pandoc -f html-native_divs-native_spans -t markdown

#### Extension: <span class="nowrap">`native_spans`</span>

Analogous to <span class="nowrap">`native_divs`</span> above.

Literate Haskell support
------------------------

#### Extension: <span class="nowrap">`literate_haskell`</span>

Treat the document as literate Haskell source.

This extension can be enabled/disabled for the following formats:

input formats  
<span class="nowrap">`markdown`</span>, <span
class="nowrap">`rst`</span>, <span class="nowrap">`latex`</span>

output formats  
<span class="nowrap">`markdown`</span>, <span
class="nowrap">`rst`</span>, <span class="nowrap">`latex`</span>, <span
class="nowrap">`html`</span>

If you append <span class="nowrap">`+lhs`</span> (or <span
class="nowrap">`+literate_haskell`</span>) to one of the formats above,
pandoc will treat the document as literate Haskell source. This means
that

-   In Markdown input, “bird track” sections will be parsed as Haskell
    code rather than block quotations. Text between <span
    class="nowrap">`\begin{code}`</span> and <span
    class="nowrap">`\end{code}`</span> will also be treated as Haskell
    code. For ATX-style headings the character ‘=’ will be used instead
    of ‘\#’.

-   In Markdown output, code blocks with classes <span
    class="nowrap">`haskell`</span> and <span
    class="nowrap">`literate`</span> will be rendered using bird tracks,
    and block quotations will be indented one space, so they will not be
    treated as Haskell code. In addition, headings will be rendered
    setext-style (with underlines) rather than ATX-style (with ‘\#’
    characters). (This is because ghc treats ‘\#’ characters in column 1
    as introducing line numbers.)

-   In restructured text input, “bird track” sections will be parsed as
    Haskell code.

-   In restructured text output, code blocks with class <span
    class="nowrap">`haskell`</span> will be rendered using bird tracks.

-   In LaTeX input, text in <span class="nowrap">`code`</span>
    environments will be parsed as Haskell code.

-   In LaTeX output, code blocks with class <span
    class="nowrap">`haskell`</span> will be rendered inside <span
    class="nowrap">`code`</span> environments.

-   In HTML output, code blocks with class <span
    class="nowrap">`haskell`</span> will be rendered with class <span
    class="nowrap">`literatehaskell`</span> and bird tracks.

Examples:

    pandoc -f markdown+lhs -t html

reads literate Haskell source formatted with Markdown conventions and
writes ordinary HTML (without bird tracks).

    pandoc -f markdown+lhs -t html+lhs

writes HTML with the Haskell code in bird tracks, so it can be copied
and pasted as literate Haskell source.

Note that GHC expects the bird tracks in the first column, so indented
literate code blocks (e.g. inside an itemized environment) will not be
picked up by the Haskell compiler.

Other extensions
----------------

#### Extension: <span class="nowrap">`empty_paragraphs`</span>

Allows empty paragraphs. By default empty paragraphs are omitted.

This extension can be enabled/disabled for the following formats:

input formats  
<span class="nowrap">`docx`</span>, <span class="nowrap">`html`</span>

output formats  
<span class="nowrap">`docx`</span>, <span class="nowrap">`odt`</span>,
<span class="nowrap">`opendocument`</span>, <span
class="nowrap">`html`</span>

#### Extension: <span class="nowrap">`native_numbering`</span>

Enables native numbering of figures and tables. Enumeration starts at 1.

This extension can be enabled/disabled for the following formats:

output formats  
<span class="nowrap">`odt`</span>, <span
class="nowrap">`opendocument`</span>

#### Extension: <span class="nowrap">`styles`</span>

When converting from docx, read all docx styles as divs (for paragraph
styles) and spans (for character styles) regardless of whether pandoc
understands the meaning of these styles. This can be used with [docx
custom styles](#custom-styles). Disabled by default.

input formats  
<span class="nowrap">`docx`</span>

#### Extension: <span class="nowrap">`amuse`</span>

In the <span class="nowrap">`muse`</span> input format, this enables
Text::Amuse extensions to Emacs Muse markup.

#### Extension: <span class="nowrap">`citations`</span>

Some aspects of [Pandoc’s Markdown citation syntax](#citations) are also
accepted in <span class="nowrap">`org`</span> input.

#### Extension: <span class="nowrap">`ntb`</span>

In the <span class="nowrap">`context`</span> output format this enables
the use of [Natural Tables
(TABLE)](https://wiki.contextgarden.net/TABLE) instead of the default
[Extreme Tables (xtables)](https://wiki.contextgarden.net/xtables).
Natural tables allow more fine-grained global customization but come at
a performance penalty compared to extreme tables.

Pandoc’s Markdown
=================

Pandoc comprend une version étendue et légèrement révisée de la syntaxe
du [Markdown](https://daringfireball.net/projects/markdown/) de John
Gruber. Ce document explique la syntaxe, en notant les différences par
rapport au Markdown standard. Sauf indication contraire, ces différences
peuvent être supprimées en utilisant le format <span
class="nowrap">`markdown_strict`</span> au lieu de <span
class="nowrap">`markdown`</span>. Des extensions peuvent être activées
ou désactivées pour spécifier le comportement de manière plus
granulaire. Elles sont décrites ci-après. Voir aussi
[Extensions](#extensions) ci-dessus, pour les extensions qui
fonctionnent également sur d'autres formats.

Philosophie
-----------

Markdown est conçu pour être facile à écrire et, plus important encore,
facile à lire :

> Un document au format Markdown doit pouvoir être publié tel quel, en
> texte brut, sans avoir l'air d'avoir été marqué avec des balises ou
> des instructions de formatage. – [John
> Gruber](https://daringfireball.net/projects/markdown/syntax#philosophy)

Ce principe a guidé les décisions de pandoc pour trouver la syntaxe des
tableaux, notes de bas de page et autres extensions.  
  
Il y a toutefois un point sur lequel les objectifs de pandoc diffèrent
des objectifs initiaux de Markdown. Alors que Markdown a été conçu à
l'origine avec la génération HTML à l'esprit, pandoc est conçu pour de
multiples formats de sortie. Ainsi, alors que pandoc permet
l'intégration de HTML brut, il le décourage et fournit d'autres moyens,
non HTML, de représenter les éléments importants d'un document comme les
listes de définitions, les tableaux, les mathématiques et les notes de
bas de page.

Paragraphes
-----------

Un paragraphe est une ou plusieurs lignes de texte suivies d'une ou
plusieurs lignes blanches. Les nouvelles lignes sont traitées comme des
espaces, de sorte que vous pouvez refondre vos paragraphes comme vous le
souhaitez. Si vous avez besoin d'un retour à la ligne, mettez deux
espaces ou plus à la fin d'une ligne.

#### Extension: <span class="nowrap">`escaped_line_breaks`</span>

Une barre oblique inversée suivie d'une nouvelle ligne est également une
rupture de ligne. Remarque : dans les cellules de tableaux à lignes
multiples et de grilles, c'est la seule façon de créer un saut de ligne,
car les espaces de fin de ligne dans les cellules sont ignorés.

Headings
--------

Il existe deux types de rubriques : Setext et ATX.

### Setext-style headings

Un en-tête de type setext est une ligne de texte "soulignée" avec une
rangée de signes <span class="nowrap">`=`</span> (pour un titre de
niveau 1) ou de signes <span class="nowrap">`-`</span> (pour un titre de
niveau 2) :

    A level-one heading
    ===================

    A level-two heading
    -------------------

Le texte de l'en-tête peut contenir un formatage en ligne, tel que
l'accentuation (voir [Inline formatting](#inline-formatting),
ci-dessous).

### ATX-style headings

Une rubrique de type ATX se compose de un à six signes <span
class="nowrap">`#`</span> et une ligne de texte, éventuellement suivie
d'un nombre quelconque de signes <span class="nowrap">`#`</span>. Le
nombre de signes <span class="nowrap">`#`</span> en début de ligne donne
le niveau de titre :

    ## Un titre de niveau 2

    ### Un titre de niveau 3 ###

Comme pour les titres de type setext, le texte du titre peut contenir un
formatage :

    # Un titre de niveau 1 avec [lien url](/url) et *emphase*

#### Extension: <span class="nowrap">`blank_before_header`</span>

La syntaxe standard de Markdown ne nécessite pas de ligne blanche avant
un titre. Pandoc l'exige (sauf, bien sûr, au début du document). La
raison de cette exigence est qu'il est trop facile pour un <span
class="nowrap">`#`</span> de se retrouver accidentellement au début
d'une ligne (peut-être à cause d'un retour à la ligne). Prenons un
exemple :

    I like several of their flavors of ice cream:
    #22, for example, and #5.

#### Extension: <span class="nowrap">`space_in_atx_header`</span>

De nombreuses mises en œuvre de Markdown ne nécessitent pas d'espace
entre <span class="nowrap">`#`</span>ouvrant d'une en-tête ATX et le
texte de l'en-tête, de sorte que <span
class="nowrap">`#5                   bolt`</span> et <span
class="nowrap">`#hashtag`</span> sont considérés comme des en-têtes.
Avec cette extension, pandoc nécessite l'espace.

### Heading identifiers

Voir aussi l' [extension](#extension-auto_identifiers) [<span
class="nowrap">`auto_identifiers`</span>](#extension-auto_identifiers)
ci-dessus.

#### Extension: <span class="nowrap">`header_attributes`</span>

Les titres peuvent se voir attribuer des attributs en utilisant cette
syntaxe à la fin de la ligne contenant le texte du titre :

    {#identifier .class .class key=value key=value}

Ainsi, par exemple, les rubriques suivantes se verront toutes attribuer
l'identifiant <span class="nowrap">`foo`</span>:

    # My heading {#foo}

    ## My heading ##    {#foo}

    My other heading   {#foo}
    ---------------

(Cette syntaxe est compatible avec [PHP Markdown
Extra](https://michelf.ca/projects/php-markdown/extra/).)

Notez que bien que cette syntaxe permette l'attribution de classes et
d'attributs de clés/valeurs, les auteurs n'utilisent généralement pas
toutes ces informations. Les identificateurs, les classes et les
attributs clé/valeur sont utilisés dans les formats HTML et HTML-based
tels que EPUB et slidy. Les identificateurs sont utilisés pour les
étiquettes et les ancres de liens dans les rédacteurs LaTeX, ConTeXt,
Textile, Jira markup et AsciiDoc.  
  
Les titres avec la classe <span class="nowrap">`unnumbered`</span> ne
seront pas numérotés, même si
<a href="#option--number-sections" class="option"><span class="nowrap"><code>--number-sections</code></span></a>
est spécifié. Un tiret simple (<span class="nowrap">`-`</span> ) dans un
contexte d'attributs est équivalent à<span
class="nowrap">`.unnumbered`</span> et préférable dans les documents non
anglais. Donc,

    # My heading {-}

est identique à

    # My heading {.unnumbered}

Si la classe <span class="nowrap">`unlisted`</span> est présente avec
<span class="nowrap">`unnumbered`</span>, le titre ne sera pas inclus
dans une table des matières. (Actuellement, cette fonctionnalité n'est
mise en œuvre que pour certains formats : ceux basés sur LaTeX et HTML,
PowerPoint et RTF).

#### Extension: <span class="nowrap">`implicit_header_references`</span>

Pandoc se comporte comme si des liens de référence avaient été définis
pour chaque rubrique. Ainsi, pour établir un lien avec une rubrique

    # Heading identifiers in HTML

vous pouvez simplement écrire

    [Heading identifiers in HTML]

ou

    [Heading identifiers in HTML][]

ou

    [the section on heading identifiers][heading identifiers in
    HTML]

au lieu de donner l'identifiant explicitement :

    [Heading identifiers in HTML](#heading-identifiers-in-html)

Si plusieurs rubriques ont un texte identique, la référence
correspondante ne sera liée qu'à la première, et vous devrez utiliser
des liens explicites pour établir des liens avec les autres, comme
décrit ci-dessus.  
  
Comme les liens de référence réguliers, ces références ne sont pas
sensibles à la casse.  
  
Les définitions des références de liens explicites ont toujours la
priorité sur les références implicites des rubriques. Ainsi, dans
l'exemple suivant, le lien pointera vers <span
class="nowrap">`bar`</span>, pas vers <span
class="nowrap">`#foo`</span>:

    # Foo

    [foo]: bar

    Voir [foo]

Citations en bloc
-----------------

Markdown utilise les conventions du courrier électronique pour citer des
blocs de texte. Un bloc de citation est un ou plusieurs paragraphes ou
autres éléments de bloc (tels que des listes ou des titres), chaque
ligne étant précédée d'un caractère <span class="nowrap">`>`</span>et
d'une espace optionnelle . (Le <span class="nowrap">`>`</span> doit
démarrer à la marge gauche, mais il ne doit pas être indenté de plus de
trois espaces.)

    > This is a block quote. This
    > paragraph has two lines.
    >
    > 1. This is a list inside a block quote.
    > 2. Second item.

Une forme "paresseuse", qui nécessite le caractère <span
class="nowrap">`>`</span>uniquement sur la première ligne de chaque
bloc, est également autorisée :

    > This is a block quote. This
    paragraph has two lines.

    > 1. This is a list inside a block quote.
    2. Second item.

Parmi les éléments du bloc qui peuvent être contenus dans une citation
en bloc, on trouve d'autres citations en bloc. C'est-à-dire que les
citations en bloc peuvent être imbriquées :

    > This is a block quote.
    >
    > > A block quote within a block quote.

Si le caractère <span class="nowrap">`>`</span> est suivi d'un espace
facultatif, cet espace sera considéré comme faisant partie du guillemet
et non de l'indentation du contenu. Ainsi, pour mettre un bloc de code
indenté dans un bloc de guillemets, vous avez besoin de cinq espaces
après le <span class="nowrap">`>`</span>:

    >     code

#### Extension: <span class="nowrap">`blank_before_blockquote`</span>

La syntaxe standard de Markdown ne nécessite pas de ligne blanche avant
une citation bloc. Pandoc l'exige (sauf, bien sûr, au début du
document). La raison de cette exigence est qu'il est trop facile pour un
<span class="nowrap">`>`</span> de se retrouver au début d'une ligne par
accident (peut-être à cause de l'enroulement de la ligne). Ainsi, à
moins que le format <span class="nowrap">`markdown_strict`</span> soit
utilisé, ce qui suit ne produit pas une citation en bloc imbriquée dans
pandoc :

    > This is a block quote.
    >> Nested.

Verbatim (code) blocks
----------------------

### Blocs de code indentés

Un bloc de texte indenté de quatre espaces (ou une tabulation) est
traité comme du texte textuel : c'est-à-dire que les caractères spéciaux
ne déclenchent pas de formatage particulier, et que tous les espaces et
sauts de ligne sont préservés. Par exemple, les caractères spéciaux ne
déclenchent pas de formatage spécial et tous les espaces et sauts de
ligne sont conservés,

        if (a > 3) {
          moveShip(5 * gravity, DOWN);
        }

L'indentation initiale (quatre espaces ou une tabulation) n'est pas
considérée comme faisant partie du texte intégral et est supprimée dans
la sortie.  
  
Note : les lignes vierges dans le texte du compte rendu ne doivent pas
nécessairement commencer par quatre espaces.

### Blocs de code clôturés

#### Extension: <span class="nowrap">`fenced_code_blocks`</span>

En plus des blocs de code standard indentés, pandoc prend en charge les
blocs de code clôturés. Ceux-ci commencent par une rangée de trois
tildes ou plus (<span class="nowrap">`~`</span>) et se terminent par une
rangée de tildes qui doit être au moins aussi longue que la rangée de
départ. Tout ce qui se trouve entre ces lignes est traité comme un code.
Aucune indentation n'est nécessaire :

    ~~~~~~~
    if (a > 3) {
      moveShip(5 * gravity, DOWN);
    }
    ~~~~~~~

Comme les blocs de code ordinaires, les blocs de code clôturés doivent
être séparés du texte environnant par des lignes blanches.  
  
Si le code lui-même contient une rangée de tildes ou de bâtons
"backticks", il suffit d'utiliser une rangée plus longue de tildes ou de
bâtons au début et à la fin :

    ~~~~~~~~~~~~~~~~
    ~~~~~~~~~~
    code including tildes
    ~~~~~~~~~~
    ~~~~~~~~~~~~~~~~

#### Extension: <span class="nowrap">`backtick_code_blocks`</span>

Identique à <span class="nowrap">`fenced_code_blocks`</span>, mais
utilise des "backticks" (<span class="nowrap">`` ` ``</span>) au lieu de
tildes (<span class="nowrap">`~`</span>).

#### Extension: <span class="nowrap">`fenced_code_attributes`</span>

Vous pouvez, si vous le souhaitez, attacher des attributs à un bloc de
code clôturé ou à un bloc de code backtick en utilisant cette syntaxe :

    ~~~~ {#mycode .haskell .numberLines startFrom="100"}
    qsort []     = []
    qsort (x:xs) = qsort (filter (< x) xs) ++ [x] ++
                   qsort (filter (>= x) xs)
    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Ici <span class="nowrap">`mycode`</span> est un identificateur, <span
class="nowrap">`haskell`</span> et <span
class="nowrap">`numberLines`</span> sontet des classes,  <span
class="nowrap">`startFrom`</span> est un attribut de valeur <span
class="nowrap">`100`</span>. Certains formats de sortie peuvent utiliser
ces informations pour effectuer une mise en évidence syntaxique.
Actuellement, les seuls formats de sortie qui utilisent ces informations
sont HTML, LaTeX, Docx, Ms et PowerPoint. Si la coloration syntaxique
est prise en charge pour votre format de sortie et votre langue, le bloc
de code ci-dessus apparaîtra en surbrillance, avec des lignes
numérotées. (Pour savoir quelles langues sont prises en charge, tapez
<span
class="nowrap">`pandoc                   --list-highlight-languages`</span>.)
Sinon, le bloc de code ci-dessus apparaîtra comme suit :

    <pre id="mycode" class="haskell numberLines" startFrom="100">
      <code>
      ...
      </code>
    </pre>

La classe <span class="nowrap">`numberLines`</span> (ou <span
class="nowrap">`number-lines`</span>) fera en sorte que les lignes du
bloc de code soient numérotées, en commençant par <span
class="nowrap">`1`</span> ou par la valeur de l'attribut <span
class="nowrap">`startFrom`</span>. La classe <span
class="nowrap">`lineAnchors`</span> (ou <span
class="nowrap">`line-anchors`</span>) fera en sorte que les lignes
soient des ancres cliquables dans la sortie HTML.

Un formulaire de raccourci peut également être utilisé pour spécifier la
langue du bloc de code :

    ```haskell
    qsort [] = []
    ```

Ce qui est équivalent à :

    ``` {.haskell}
    qsort [] = []
    ```

Si l'extension <span class="nowrap">`fenced_code_attributes`</span> est
désactivée, mais l'entrée contient un ou plusieurs attributs de classe
pour le bloc de code, le premier attribut de classe sera imprimé après
la clôture ouvrante comme un mot vide.  
  
Pour éviter toute mise en évidence, utilisez la fonction
<a href="#option--no-highlight" class="option"><span class="nowrap"><code>--no-highlight</code></span></a>
flag. To set the highlighting style, use
<a href="#option--highlight-style" class="option"><span class="nowrap"><code>--highlight-style</code></span></a>.
Pour plus d'informations sur la mise en évidence, voir [Syntax
highlighting](#syntax-highlighting), ci-dessous.

Blocs Ligne
-----------

#### Extension: <span class="nowrap">`line_blocks`</span>

Un bloc de lignes est une séquence de lignes commençant par une barre
verticale (<span class="nowrap">`|`</span>) suivie d'un espace. La
division en lignes sera préservée dans la sortie, tout comme les espaces
de tête de ligne ; sinon, les lignes seront formatées en Markdown. Ceci
est utile pour les vers et les adresses :

    | The limerick packs laughs anatomical
    | In space that is quite economical.
    |    But the good ones I've seen
    |    So seldom are clean
    | And the clean ones so seldom are comical

    | 200 Main St.
    | Berkeley, CA 94718

Les lignes peuvent être forcées en fin de ligne si nécessaire, mais la
ligne de continuation doit commencer par un espace.

    | The Right Honorable Most Venerable and Righteous Samuel L.
      Constable, Jr.
    | 200 Main St.
    | Berkeley, CA 94718

Cette syntaxe est empruntée à
[reStructuredText](https://docutils.sourceforge.io/docs/ref/rst/introduction.html).

Listes
------

### Listes à puces

Une liste à puces est une liste d'éléments d'une liste à puces. Une
liste à puces commence par une puce (<span class="nowrap">`*`</span>,
<span class="nowrap">`+`</span>, ou <span class="nowrap">`-`</span>).
Voici un exemple simple :

    * one
    * two
    * three

Il en résultera une liste "compacte". Si vous voulez une liste "libre",
dans laquelle chaque élément est formaté en paragraphe, mettez des
espaces entre les éléments :

    * one

    * two

    * three

Les puces n'ont pas besoin d'être alignées avec la marge de gauche ;
elles peuvent être indentées d'un, deux ou trois espaces. La puce doit
être suivie d'un espace.  
  
Les éléments de la liste ont une meilleure apparence si les lignes
suivantes affleurent avec la première ligne (après la puce) :

    * here is my first
      list item.
    * and my second.

Mais Markdown permet aussi un format "paresseux" :

    * here is my first
    list item.
    * and my second.

### Block content in list items

Un élément de liste peut contenir plusieurs paragraphes et d'autres
contenus au niveau des blocs. Toutefois, les paragraphes suivants
doivent être précédés d'une ligne blanche et être en retrait pour
s'aligner sur le premier contenu non spatial après le marqueur de liste.

      * First paragraph.

        Continued.

      * Second paragraph. With a code block, which must be indented
        eight spaces:

            { code }

Exception : si le marqueur de liste est suivi d'un bloc de code indenté,
qui doit commencer 5 espaces après le marqueur de liste, alors les
paragraphes suivants doivent commencer deux colonnes après le dernier
caractère du marqueur de liste :

    *     code

      continuation paragraph

Les éléments de la liste peuvent inclure d'autres listes. Dans ce cas,
la ligne blanche précédente est facultative. La liste imbriquée doit
être mise en retrait pour s'aligner sur le premier caractère non espace
après le marqueur de liste de l'élément de liste qui la contient.

    * fruits
      + apples
        - macintosh
        - red delicious
      + pears
      + peaches
    * vegetables
      + broccoli
      + chard

Comme indiqué ci-dessus, Markdown vous permet d'écrire les éléments de
la liste "paresseusement", au lieu d'indenter les lignes de
continuation. Toutefois, si un élément de liste comporte plusieurs
paragraphes ou d'autres blocs, la première ligne de chacun doit être
mise en retrait.

    + A lazy, lazy, list
    item.

    + Another one; this looks
    bad but is legal.

        Second paragraph of second
    list item.

### Listes ordonnées

Les listes ordonnées fonctionnent comme les listes à puces, sauf que les
éléments commencent par des énumérateurs plutôt que par des puces.  
  
Dans le Markdown standard, les énumérateurs sont des nombres décimaux
suivis d'un point et d'un espace. Les nombres eux-mêmes sont ignorés, il
n'y a donc pas de différence entre cette liste :

    1.  one
    2.  two
    3.  three

et celle-là :

    5.  one
    7.  two
    1.  three

#### Extension: <span class="nowrap">`fancy_lists`</span>

Contrairement au système standard de Markdown, pandoc permet de marquer
les articles d'une liste ordonnée avec des lettres majuscules et
minuscules et des chiffres romains, en plus des chiffres arabes. Les
marques de liste peuvent être mises entre parenthèses ou suivies d'une
seule parenthèse droite ou d'un point. Ils doivent être séparés du texte
qui suit par au moins un espace et, si le marqueur de liste est une
lettre majuscule avec un point, par au moins deux
espaces.<a href="#fn1" id="fnref1" class="footnote-ref"><sup>1</sup></a>

L' extension <span class="nowrap">`fancy_lists`</span> autorise
également ‘<span class="nowrap">`#`</span>’ à être utilisé comme
marqueur de liste ordonnée à la place d'un chiffre :

    #. one
    #. two

#### Extension: <span class="nowrap">`startnum`</span>

Pandoc fait également attention au type de marqueur de liste utilisé, et
au numéro de départ, et ces deux éléments sont préservés dans la mesure
du possible dans le format de sortie. Ainsi, ce qui suit donne une liste
avec des chiffres suivis d'une seule parenthèse, commençant par 9, et
une sous-liste avec des chiffres romains minuscules :

     9)  Ninth
    10)  Tenth
    11)  Eleventh
           i. subone
          ii. subtwo
         iii. subthree

Pandoc commencera une nouvelle liste chaque fois qu'un type différent de
marqueur de liste sera utilisé. Ainsi, trois listes seront créées :

    (2) Two
    (5) Three
    1.  Four
    *   Five

Si des marqueurs de liste par défaut sont souhaités, utilisez <span
class="nowrap">`#.`</span>:

    #.  one
    #.  two
    #.  three

#### Extension: <span class="nowrap">`task_lists`</span>

Pandoc prend en charge les listes de tâches, en utilisant la syntaxe de
GitHub-Flavored Markdown.

    - [ ] an unchecked task list item
    - [x] checked item

### Listes de définition

#### Extension: <span class="nowrap">`definition_lists`</span>

Pandoc prend en charge les listes de définition, en utilisant la syntaxe
de PHP Markdown Extra avec certaines
extensions.<a href="#fn2" id="fnref2" class="footnote-ref"><sup>2</sup></a>

    Term 1

    :   Definition 1

    Term 2 with *inline markup*

    :   Definition 2

            { some code, part of Definition 2 }

        Third paragraph of definition 2.

Chaque terme doit tenir sur une ligne, qui peut éventuellement être
suivie d'une ligne blanche, et doit être suivi d'une ou plusieurs
définitions. Une définition commence par un deux-points ou un tilde, qui
peuvent être indentés d'un ou deux espaces.  
  
Un terme peut avoir plusieurs définitions, et chaque définition peut
être constituée d'un ou plusieurs éléments de bloc (paragraphe, bloc de
code, liste, etc.), chacun étant indenté de quatre espaces ou d'un
tabulateur. Le corps de la définition (y compris la première ligne, à
l'exception du deux-points ou du tilde) doit être indenté de quatre
espaces. Cependant, comme pour les autres listes Markdown, vous pouvez
"paresseusement" omettre l'indentation, sauf au début d'un paragraphe ou
d'un autre élément de bloc :

    Term 1

    :   Definition
    with lazy continuation.

        Second paragraph of the definition.

Si vous laissez un espace avant la définition (comme dans l'exemple
ci-dessus), le texte de la définition sera traité comme un paragraphe.
Dans certains formats de sortie, cela signifie un plus grand espacement
entre les paires terme/définition. Pour une liste de définitions plus
compacte, omettez l'espace avant la définition :

    Term 1
      ~ Definition 1

    Term 2
      ~ Definition 2a
      ~ Definition 2b

Notez que l'espace entre les éléments d'une liste de définitions est
obligatoire. (Une variante qui assouplit cette exigence, mais qui
interdit l'enroulement dur "paresseux", peut être activée avec <span
class="nowrap">`compact_definition_lists`</span>: voir [Non-pandoc
extensions](#non-pandoc-extensions), ci-dessous.)

### Listes d'exemples numéroté

#### Extension: <span class="nowrap">`example_lists`</span>

Le marqueur de listes spécial <span class="nowrap">`@`</span> peut être
utilisé pour des exemples numérotés de manière séquentielle. Le premier
élément de la liste avec un marqueur <span class="nowrap">`@`</span>
sera numéroté ‘1’, le suivant ‘2’, etc.., tout au long du document. Il
n'est pas nécessaire que les exemples numérotés figurent dans une liste
unique ; chaque nouvelle liste utilisant <span class="nowrap">`@`</span>
reprendra là où la dernière s'est arrêtée. Ainsi, par exemple :

    (@)  My first example will be numbered (1).
    (@)  My second example will be numbered (2).

    Explanation of examples.

    (@)  My third example will be numbered (3).

Les exemples numérotés peuvent être étiquetés et mentionnés ailleurs
dans le document :

    (@good)  This is a good example.

    As (@good) illustrates, ...

L'étiquette peut être une chaîne de caractères alphanumériques, un trait
de soulignement ou un trait d'union.  
  
Note : les paragraphes de suite dans les listes d'exemples doivent
toujours être indentés de quatre espaces, quelle que soit la longueur du
marqueur de liste. Autrement dit, les listes d'exemples se comportent
toujours comme si l'extension<span
class="nowrap">`four_space_rule`</span> est utilisée. En effet, les
étiquettes d'exemple ont tendance à être longues et il serait difficile
d'indenter le contenu jusqu'au premier caractère différent d'une espace
après l'étiquette.

### Compact and loose lists

Pandoc se comporte différemment de <span
class="nowrap">`Markdown.pl`</span> sur certains "cas marginaux"
impliquant des listes. Considérez cette source :

    +   First
    +   Second:
        -   Fee
        -   Fie
        -   Foe

    +   Third

Pandoc transforme cela en une "liste compacte" (sans tags <span
class="nowrap">`<p>`</span> autour de “First”, “Second”, ou “Third”),
alors que Markdown ajoute les étiquettes <span
class="nowrap">`<p>`</span> autour de “Second” et “Third” (mais pas
“First”), à cause de l'espace vide autour de "Third". Pandoc suit une
règle simple : si le texte est suivi d'une ligne blanche, il est traité
comme un paragraphe. Puisque "Second" est suivi d'une liste, et non
d'une ligne blanche, il n'est pas traité comme un paragraphe. Le fait
que la liste soit suivie d'une ligne blanche n'a pas d'importance. (Note
: Pandoc fonctionne de cette manière même lorsque le format <span
class="nowrap">`markdown_strict`</span> est specifié. Ce comportement
est conforme à la description syntaxique officielle de Markdown, même
s'il est différent de celui de <span
class="nowrap">`Markdown.pl`</span>.)

### Terminer une liste

Que faire si vous voulez mettre un bloc de code indenté après une liste
?

    -   item one
    -   item two

        { my code block }

Des problèmes ! Ici, pandoc (comme d'autres implémentations de Markdown)
traitera <span class="nowrap">`{ my code block }`</span> comme deuxième
paragraphe du point 2, et non comme bloc de code.  
  
Pour "couper" la liste après le point deux, vous pouvez insérer un
contenu non indenté, comme un commentaire HTML, qui ne produira pas de
sortie visible dans un format quelconque :

    -   item one
    -   item two

    <!-- end of list -->

        { my code block }

Vous pouvez utiliser la même astuce si vous voulez deux listes
consécutives au lieu d'une seule grande liste :

    1.  one
    2.  two
    3.  three

    <!-- -->

    1.  uno
    2.  dos
    3.  tres

Horizontal rules
----------------

Une ligne contenant une rangée de trois ou plus de caractères <span
class="nowrap">`*`</span>, <span class="nowrap">`-`</span>, ou <span
class="nowrap">`_`</span> (éventuellement séparées par des espaces)
produit une règle horizontale :

    *  *  *  *

    ---------------

Tableaux
--------

Quatre types de tableaux peuvent être utilisés. Les trois premiers types
supposent l'utilisation d'une police à largeur fixe, telle que Courier.
Le quatrième type peut être utilisé avec des polices à espacement
proportionnel, car il ne nécessite pas d'aligner les colonnes.

#### Extension: <span class="nowrap">`table_captions`</span>

Une légende peut éventuellement être fournie avec les 4 types de
tableaux (comme illustré dans les exemples ci-dessous). Une légende est
un paragraphe qui commence par la chaîne de caractères <span
class="nowrap">`Table:`</span> (ou simplement <span
class="nowrap">`:`</span>), qui sera supprimée. Elle peut apparaître
avant ou après le tableau.

#### Extension: <span class="nowrap">`simple_tables`</span>

De tableaux simples ressemblent à ceci :

      Right     Left     Center     Default
    -------     ------ ----------   -------
         12     12        12            12
        123     123       123          123
          1     1          1             1

    Table:  Demonstration of simple table syntax.

Les lignes d'en-tête et les lignes de tableau doivent tenir sur une
seule ligne. L'alignement des colonnes est déterminé par la position du
texte de l'en-tête par rapport à la ligne en pointillés qui se trouve en
dessous
:<a href="#fn3" id="fnref3" class="footnote-ref"><sup>3</sup></a>

-   Si la ligne pointillée affleure le texte de l'en-tête sur le côté
    droit mais le dépasse sur la gauche, la colonne est alignée à
    droite.
-   Si la ligne pointillée affleure le texte de l'en-tête sur le côté
    gauche mais le dépasse sur la droite, la colonne est alignée à
    gauche.
-   Si la ligne pointillée dépasse le texte de l'en-tête des deux côtés,
    la colonne est centrée.
-   Si la ligne pointillée est au ras du texte de l'en-tête des deux
    côtés, l'alignement par défaut est utilisé (dans la plupart des cas,
    il sera à gauche).

Le tableau doit se terminer par une ligne blanche, ou une ligne de
tirets suivie d'une ligne blanche.  
  
La ligne d'en-tête de la colonne peut être omise, à condition qu'une
ligne de tirets soit utilisée pour terminer le tableau. Par exemple, la
ligne d'en-tête de la colonne peut être omise, à condition qu'une ligne
pointillée soit utilisée pour terminer le tableau :

    -------     ------ ----------   -------
         12     12        12             12
        123     123       123           123
          1     1          1              1
    -------     ------ ----------   -------

Lorsque la ligne d'en-tête est omise, l'alignement des colonnes est
déterminé sur la base de la première ligne du corps du tableau. Ainsi,
dans les tableaux ci-dessus, les colonnes seraient respectivement
alignées à droite, à gauche, au centre et à droite.

#### Extension: <span class="nowrap">`multiline_tables`</span>

Les tableaux multilignes permettent aux lignes d'en-tête et de tableau
de couvrir plusieurs lignes de texte (mais les cellules qui couvrent
plusieurs colonnes ou lignes du tableau ne sont pas prises en charge).
Voici un exemple :

    -------------------------------------------------------------
     Centered   Default           Right Left
      Header    Aligned         Aligned Aligned
    ----------- ------- --------------- -------------------------
       First    row                12.0 Example of a row that
                                        spans multiple lines.

      Second    row                 5.0 Here's another one. Note
                                        the blank line between
                                        rows.
    -------------------------------------------------------------

    Table: Here's the caption. It, too, may span
    multiple lines.

Ceux-ci fonctionnent comme de simples tableaux, mais avec les
différences suivantes :

-   Ils doivent commencer par une rangée de tirets, avant le texte de
    l'en-tête (sauf si la rangée de l'en-tête est omise).
-   Ils doivent se terminer par une rangée de tirets, puis par une ligne
    blanche.
-   Les lignes doivent être séparées par des lignes blanches.

Dans les tableaux multilignes, l'analyseur de tableau fait attention à
la largeur des colonnes, et les auteurs essaient de reproduire ces
largeurs relatives dans le résultat. Ainsi, si vous trouvez qu'une des
colonnes est trop étroite dans la sortie, essayez de l'élargir dans la
source Markdown.

L'en-tête peut être omis dans les tableaux multilignes ainsi que dans
les tableaux simples :

    ----------- ------- --------------- -------------------------
       First    row                12.0 Example of a row that
                                        spans multiple lines.

      Second    row                 5.0 Here's another one. Note
                                        the blank line between
                                        rows.
    ----------- ------- --------------- -------------------------

    : Here's a multiline table without a header.

Il est possible qu'un tableau multiligne ne comporte qu'une seule ligne,
mais celle-ci doit être suivie d'une ligne blanche (et ensuite de la
ligne de tirets qui termine le tableau), ou le tableau peut être
interprété comme un tableau simple.

#### Extension: <span class="nowrap">`grid_tables`</span>

Les tableaux quadrillés ressemblent à ceci :

    : Sample grid table.

    +---------------+---------------+--------------------+
    | Fruit         | Price         | Advantages         |
    +===============+===============+====================+
    | Bananas       | $1.34         | - built-in wrapper |
    |               |               | - bright color     |
    +---------------+---------------+--------------------+
    | Oranges       | $2.10         | - cures scurvy     |
    |               |               | - tasty            |
    +---------------+---------------+--------------------+

La rangée de <span class="nowrap">`=`</span>sépare l'en-tête du corps de
la table, et peut être omise pour une table sans en-tête. Les cellules
des tableaux à grille peuvent contenir des éléments de bloc arbitraires
(plusieurs paragraphes, blocs de code, listes, etc.). Les cellules qui
couvrent plusieurs colonnes ou lignes ne sont pas prises en charge. Les
tableaux de grille peuvent être créés facilement en utilisant le mode
tableau d'Emacs (<span class="nowrap">`M-x table-insert`</span>).

Les alignements peuvent être spécifiés comme dans les tableaux "pipe",
en plaçant des deux-points aux limites de la ligne de séparation après
l'en-tête :

    +---------------+---------------+--------------------+
    | Right         | Left          | Centered           |
    +==============:+:==============+:==================:+
    | Bananas       | $1.34         | built-in wrapper   |
    +---------------+---------------+--------------------+

Pour les tableaux sans en-tête, les deux-points vont plutôt sur la ligne
supérieure :

    +--------------:+:--------------+:------------------:+
    | Right         | Left          | Centered           |
    +---------------+---------------+--------------------+

##### Limitations des tableaux quadrillages

Pandoc ne prend pas en charge les tableaux à grille avec des étendues de
lignes ou de colonnes. Cela signifie que ni le nombre variable de
colonnes sur les lignes ni le nombre variable de lignes sur les colonnes
ne sont pris en charge par Pandoc. Toutes les grilles doivent avoir le
même nombre de colonnes dans chaque ligne et le même nombre de lignes
dans chaque colonne. Par exemple, les exemples Docutils [grid
tables](https://docutils.sourceforge.io/docs/ref/rst/restructuredtext.html#grid-tables)
ne donnera pas le résultat escompté avec Pandoc.

#### Extension: <span class="nowrap">`pipe_tables`</span>

Les tableaux pipe ressemblent à ceci :

    | Right | Left | Default | Center |
    |------:|:-----|---------|:------:|
    |   12  |  12  |    12   |    12  |
    |  123  |  123 |   123   |   123  |
    |    1  |    1 |     1   |     1  |

      : Demonstration of pipe table syntax.

La syntaxe est identique aux tableaux [PHP Markdown Extra
tables](https://michelf.ca/projects/php-markdown/extra/#table). Les
caractères de début et de fin de *pipe* sont facultatifs, mais les
*pipes* sont obligatoires entre toutes les colonnes. Les deux points
indiquent l'alignement des colonnes comme indiqué. L'en-tête ne peut pas
être omis. Pour simuler un tableau sans en-tête, il faut inclure un
en-tête avec des cellules vides.

Comme les *pipes* indiquent les limites des colonnes, il n'est pas
nécessaire d'aligner les colonnes verticalement, comme dans l'exemple
ci-dessus. Il s'agit donc d'un tableau à *pipe* parfaitement légal (bien
que laid) :

    fruit| price
    -----|-----:
    apple|2.05
    pear|1.37
    orange|3.09

Les cellules des tableaux *pipe* ne peuvent pas contenir d'éléments de
bloc comme des paragraphes et des listes, et ne peuvent pas s'étendre
sur plusieurs lignes. Si un tableau *pipe* contient une ligne dont le
contenu imprimable est plus large que la largeur de la colonne (voir
<a href="#option--columns" class="option"><span class="nowrap"><code>--columns</code></span></a>),
le tableau prendra alors toute la largeur du texte et le contenu des
cellules s'enveloppera, la largeur relative des cellules étant
déterminée par le nombre de tirets dans la ligne séparant l'en-tête du
tableau du corps du tableau. (Par exemple <span
class="nowrap">`---|-`</span> rendrait la première colonne 3/4 et la
deuxième colonne 1/4 de la largeur du texte intégral). En revanche, si
aucune ligne n'est plus large que la largeur de la colonne, le contenu
des cellules ne sera pas "enveloppé", et les cellules seront
dimensionnées en fonction de leur contenu.

Note : pandoc reconnaît également les tableaux de pipe de la forme
suivante, tels qu'ils peuvent être produits par le mode orgtbl d'Emacs :

    | One | Two   |
    |-----+-------|
    | my  | table |
    | is  | nice  |

La différence est l'usage de <span class="nowrap">`+`</span> au lieu de
<span class="nowrap">`|`</span>. Les autres fonctions de orgtbl ne sont
pas prises en charge. En particulier, pour obtenir un alignement des
colonnes sans défaut, vous devrez ajouter des deux-points comme
ci-dessus.

Blocs de métadonnées
--------------------

#### Extension: <span class="nowrap">`pandoc_title_block`</span>

Si le fichier commence par un bloc titre

    % title
    % author(s) (separated by semicolons)
    % date

il sera analysé comme une information bibliographique, et non comme un
texte ordinaire. (Il sera utilisé, par exemple, dans le titre d'une
sortie autonome LaTeX ou HTML). Le bloc peut contenir uniquement un
titre, un titre et un auteur, ou les trois éléments. Si vous voulez
inclure un auteur mais pas de titre, ou un titre et une date mais pas
d'auteur, vous avez besoin d'une ligne blanche :

    %
    % Author

    % My title
    %
    % June 15, 2006

Le titre peut occuper plusieurs lignes, mais les lignes de continuation
doivent commencer par un espace avant, donc :

    % My title
      on multiple lines

Si un document a plusieurs auteurs, les auteurs peuvent être placés sur
des lignes séparées avec un espace avant, ou séparés par des
points-virgules, ou les deux. Ainsi, tous les éléments suivants sont
équivalents :

    % Author One
      Author Two

    % Author One; Author Two

    % Author One;
      Author Two

La date doit tenir sur une ligne.  
  
Les trois champs de métadonnées peuvent contenir un formatage standard
en ligne (italique, liens, notes de bas de page, etc.).

Les blocs de titres seront toujours analysés, mais ils n'affecteront la
sortie que lorsque l'option
<a href="#option--standalone" class="option"><span class="nowrap"><code>--standalone</code></span></a>
(<a href="#option--standalone" class="option"><span class="nowrap"><code>-s</code></span></a>
est choisie) . En sortie HTML, les titres apparaîtront deux fois : une
fois dans l'en-tête du document - c'est le titre qui apparaîtra en haut
de la fenêtre dans un navigateur - et une fois au début du corps du
document. Le titre dans l'en-tête du document peut comporter un préfixe
facultatif (option
<a href="#option--title-prefix" class="option"><span class="nowrap"><code>--title-prefix</code></span></a>
ou
<a href="#option--title-prefix" class="option"><span class="nowrap"><code>-T</code></span></a>).
Le titre dans le corps apparaît comme un élément H1 avec la classe
"title", il peut donc être supprimé ou reformaté avec le CSS. Si un
préfixe de titre est spécifié avec
<a href="#option--title-prefix" class="option"><span class="nowrap"><code>-T</code></span></a>
et qu'aucun bloc titre n'apparaît dans le document, le préfixe du titre
sera utilisé seul comme titre HTML.  
  
L'auteur Page de manuel extrait de la ligne de titre un titre, un numéro
de section de la page de manuel et d'autres informations d'en-tête et de
pied de page. Le titre est supposé être le premier mot de la ligne de
titre, qui peut éventuellement se terminer par un numéro de section (à
un seul chiffre) entre parenthèses. (Il ne doit pas y avoir d'espace
entre le titre et les parenthèses.) Tout ce qui suit est supposé être un
texte supplémentaire de pied de page et d'en-tête. Un caractère de pipe
unique (<span class="nowrap">`|`</span>) devrait être utilisé pour
séparer le texte du pied de page du texte de l'en-tête. Ainsi,

    % PANDOC(1)

donnera une page de manuel avec le titre <span
class="nowrap">`PANDOC`</span>et la  section 1.

    % PANDOC(1) Pandoc User Manuals

aura également "manuels d'utilisation Pandoc" dans le bas de page.

    % PANDOC(1) Pandoc User Manuals | Version 4.0

aura également "Version 4.0" dans l'en-tête.

#### Extension: <span class="nowrap">`yaml_metadata_block`</span>

Un bloc de métadonnées
[YAML](https://yaml.org/spec/1.2/spec.html "YAML v1.2 Spec") est un
objet YAML valide, délimité par une ligne de 3 tirets (<span
class="nowrap">`---`</span>) au-dessus et une ligne de 3 tirets
en-dessous (<span class="nowrap">`---`</span>) or 3 points (<span
class="nowrap">`...`</span>) en-dessous. Un bloc de métadonnées YAML
peut se trouver n'importe où dans le document, mais s'il ne se trouve
pas au début, il doit être précédé d'une ligne blanche. (Notez qu'en
raison de la façon dont pandoc concatène les fichiers d'entrée lorsque
plusieurs sont fournis, vous pouvez également conserver les métadonnées
dans un fichier YAML séparé et le transmettre à pandoc en tant
qu'argument, avec vos fichiers Markdown :

    pandoc chap1.md chap2.md chap3.md metadata.yaml -s -o book.html

Assurez-vous simplement que le fichier YAML commence par <span
class="nowrap">`---`</span> et termine avec <span
class="nowrap">`---`</span> ou <span class="nowrap">`...`</span>.) Vous
pouvez également utiliser l' option
<a href="#option--metadata-file" class="option"><span class="nowrap"><code>--metadata-file</code></span></a>.
Cependant, cette approche ne permet pas de référencer le contenu (comme
les notes de bas de page) du document principal de saisie de Markdown.

Les métadonnées seront extraites des champs de l'objet YAML et ajoutées
à toutes les métadonnées de document existantes. Les métadonnées peuvent
contenir des listes et des objets (imbriqués arbitrairement), mais
toutes les chaînes scalaires ("*string scalars*") seront interprétées
comme des Markdown. Les champs dont les noms se terminent par un trait
de soulignement seront ignorés par pandoc. (Ils peuvent se voir
attribuer un rôle par des processeurs externes.) Les noms de champs ne
doivent pas être interprétables comme des numéros YAML ou des valeurs
booléennes (donc, par exemple, <span class="nowrap">`yes`</span>, <span
class="nowrap">`True`</span>, et <span class="nowrap">`15`</span> ne
peuvent pas être utilisés comme noms de champs).

Un document peut contenir plusieurs blocs de métadonnées. Si deux blocs
de métadonnées tentent de définir le même champ, la valeur du deuxième
bloc sera prise.  
  
Lorsque pandoc est utilisé avec
<a href="#option--to" class="option"><span class="nowrap"><code>-t markdown</code></span></a>
pour créer un document Markdown, un bloc de métadonnées YAML ne sera
produit que si l'option
<a href="#option--standalone" class="option"><span class="nowrap"><code>-s/--standalone</code></span></a>
est utilisée. Toutes les métadonnées apparaîtront en un seul bloc au
début du document.

Il est à noter que les règles d'échappement de
[YAML](https://yaml.org/spec/1.2/spec.html "YAML v1.2 Spec") doivent
être respectées. Ainsi, par exemple, si un titre contient un
deux-points, il doit être cité. Le caractère pipe (<span
class="nowrap">`|`</span>) peut être utilisé pour commencer un bloc
indenté qui sera interprété littéralement, sans qu'il soit nécessaire de
s'échapper. Ce formulaire est nécessaire lorsque le champ contient des
lignes vierges ou un formatage au niveau du bloc :

    ---
    title:  'This is the title: it contains a colon'
    author:
    - Author One
    - Author Two
    keywords: [nothing, nothingness]
    abstract: |
      This is the abstract.

      It consists of two paragraphs.
    ...

Les variables du modèle seront définies automatiquement à partir des
métadonnées. Ainsi, par exemple, en écrivant du HTML, la variable <span
class="nowrap">`abstract`</span> sera réglé sur l'équivalent HTML du
Markdown dans le champ <span class="nowrap">`abstract`</span> :

    <p>This is the abstract.</p>
    <p>It consists of two paragraphs.</p>

Les variables peuvent contenir des structures YAML arbitraires, mais le
modèle doit correspondre à cette structure. La  variable <span
class="nowrap">`author`</span>dans les modèles par défaut s'attend à une
simple liste ou chaîne de caractères, mais peut être modifié pour
supporter des structures plus compliquées. La combinaison suivante, par
exemple, ajouterait une affiliation à l'auteur si elle est donnée :

    ---
    title: The document title
    author:
    - name: Author One
      affiliation: University of Somewhere
    - name: Author Two
      affiliation: University of Nowhere
    ...

Pour utiliser les auteurs structurés dans l'exemple ci-dessus, vous
auriez besoin d'un modèle personnalisé :

    $for(author)$
    $if(author.name)$
    $author.name$$if(author.affiliation)$ ($author.affiliation$)$endif$
    $else$
    $author$
    $endif$
    $endfor$

Le contenu brut à inclure dans l'en-tête du document peut être spécifié
en utilisant <span class="nowrap">`header-includes`</span>; cependant,
il est important de marquer ce contenu comme un code brut pour un format
de sortie particulier, en utilisant l'extension [<span
class="nowrap">`raw_attribute`</span>
extension](#extension-raw_attribute)), sinon elle sera interprétée comme
un markdown. Par exemple :

    header-includes:
    - |
      ```{=latex}
      \let\oldsection\section
      \renewcommand{\section}[1]{\clearpage\oldsection{#1}}
      ```

Les échappements de backslash (*touche oblique inverse*)
--------------------------------------------------------

#### Extension: <span class="nowrap">`all_symbols_escapable`</span>

Sauf à l'intérieur d'un bloc de code ou d'un code en ligne, toute
ponctuation ou tout caractère d'espacement précédé d'une barre oblique
inversée sera traité littéralement, même s'il indique normalement un
formatage. Ainsi, par exemple, si l'on écrit

    *\*hello\**

on aura

    <em>*hello*</em>

au lieu de

    <strong>hello</strong>

Cette règle est plus facile à retenir que la règle standard de Markdown,
qui ne permet d'échapper que les caractères suivants

    \`*_{}[]()>#+-.!

(Toutefois si le format <span class="nowrap">`markdown_strict`</span>
est utilisé, la règle standard de Markdown sera utilisée).  
  
Un espace en retrait est analysé comme un espace insécable. Dans la
sortie TeX, il apparaîtra comme <span class="nowrap">`~`</span>. En
sortie HTML et XML, il apparaîtra sous la forme d'un caractère espace
insécable unicode (notez qu'il semblera donc en fait "invisible" dans la
source HTML générée ; vous pouvez toujours utiliser l'option en ligne de
commande
<a href="#option--ascii" class="option"><span class="nowrap"><code>--ascii</code></span></a>
pour la faire apparaître comme une entité explicite).

Un saut deligne échappé par une barre oblique inversée (c'est-à-dire une
barre oblique inversée en fin de ligne) est analysé comme un saut de
ligne. Il apparaîtra dans la sortie TeX comme <span
class="nowrap">`\\`</span> et en HTML comme <span
class="nowrap">`<br />`</span> C'est une bonne alternative à la méthode
"invisible" de Markdown qui consiste à indiquer les ruptures de ligne en
utilisant deux espaces à la fin d'une ligne.  
  
Les échappements par barre oblique inversée ne fonctionnent pas dans les
contextes textuels (ou "*verbatim*").

Formatage en ligne
------------------

### Emphasis

To *emphasize* some text, surround it with <span
class="nowrap">`*`</span>s or <span class="nowrap">`_`</span>, like
this:

    This text is _emphasized with underscores_, and this
    is *emphasized with asterisks*.

Double <span class="nowrap">`*`</span> or <span
class="nowrap">`_`</span> produces **strong emphasis**:

    This is **strong emphasis** and __with underscores__.

A <span class="nowrap">`*`</span> or <span class="nowrap">`_`</span>
character surrounded by spaces, or backslash-escaped, will not trigger
emphasis:

    This is * not emphasized *, and \*neither is this\*.

#### Extension: <span class="nowrap">`intraword_underscores`</span>

Comme <span class="nowrap">`_`</span> est parfois utilisé à l'intérieur
de mots et d'identifiants, pandoc n'interprète pas un <span
class="nowrap">`_`</span> entouré de caractères alphanumériques comme
marqueur d'accentuation. Si vous souhaitez mettre l'accent sur une
partie seulement d'un mot, utilisez <span class="nowrap">`*`</span>:

    feas*ible*, not feas*able*.

### Rayer du texte

#### Extension: <span class="nowrap">`strikeout`</span>

Pour rayer une section de texte d'une ligne horizontale, commencez et
terminez-la par <span class="nowrap">`~~`</span>. Ainsi, par exemple,

    This ~~is deleted text.~~

### Exposants et indices

#### Extension: <span class="nowrap">`superscript`</span>, <span class="nowrap">`subscript`</span>

Les exposants peuvent être écrits en entourant le texte par les
caractères <span class="nowrap">`^`</span>; les indices peuvent être
écrits en entourant le texte par les caractères <span
class="nowrap">`~`</span>. Par exemple,

    H~2~O is a liquid.  2^10^ is 1024.

Le texte entre <span class="nowrap">`^...^`</span> ou <span
class="nowrap">`~...~`</span> ne peut pas contenir d'espaces ou de
nouvelles lignes. Si le texte en exposant ou en abrégé contient des
espaces, ces derniers doivent être séparés par des barres obliques
inverses. (Ceci afin d'éviter les mises en exposant et en indice
accidentelles par l'utilisation ordinaire de <span
class="nowrap">`~`</span>et  <span class="nowrap">`^`</span>, et aussi
de mauvaises interactions avec les notes de bas de page). Ainsi, si vous
voulez la lettre P avec "un chat" en indice, utilisez <span
class="nowrap">`P~a\                   cat~`</span>et pas,  <span
class="nowrap">`P~a                   cat~`</span>.

### Verbatim

Pour faire une courte séquence de texte mot à mot, mettez-la à
l'intérieur des backticks (Fr : AltGr-7) :

    What is the difference between `>>=` and `>>`?

Si le texte verbatim comprend une backtick, utilisez des backtick
doubles :

    Here is a literal backtick `` ` ``.

(Les espaces après les baguettes d'ouverture et avant les baguettes de
fermeture seront ignorés).  
  
La règle générale est qu'un texte verbatim commence par une série de
backticks consécutives (éventuellement suivie d'un espace) et se termine
par une série du même nombre de baguettes (éventuellement précédée d'un
espace).  
  
Il est à noter que les backslash-escapes (et autres constructions
Markdown) ne fonctionnent pas dans les contextes verbatim :

    This is a backslash followed by an asterisk: `\*`.

#### Extension: <span class="nowrap">`inline_code_attributes`</span>

Les attributs peuvent être joints au texte verbatim, tout comme pour
[fenced code blocks](#fenced-code-blocks):

    `<$>`{.haskell}

### Petites capitales

To write small caps, use the <span class="nowrap">`smallcaps`</span>
class:

    [Small caps]{.smallcaps}

Or, without the <span class="nowrap">`bracketed_spans`</span> extension:

    <span class="smallcaps">Small caps</span>

For compatibility with other Markdown flavors, CSS is also supported:

    <span style="font-variant:small-caps;">Small caps</span>

Cela fonctionnera dans tous les formats de sortie qui supportent les
petites capitales.

Maths
-----

#### Extension: <span class="nowrap">`tex_math_dollars`</span>

Tout ce qui se situe entre deux caractères <span
class="nowrap">`$`</span> sera traité comme des mathématiques TeX.
Le<span class="nowrap">`$`</span> ouvrant doit avoir un caractère
non-espace immédiatement à sa droite, tandis que le<span
class="nowrap">`$`</span> fermant doit avoir un caractère non-espace
immédiatement à sa gauche, et ne doit pas être suivi immédiatement d'un
chiffre. Ainsi, <span class="nowrap">`$20,000 and $30,000`</span> ne
sera pas analysé comme des maths. Si, pour une raison quelconque, vous
devez joindre les caractères <span class="nowrap">`$`</span> dans du
texte au sens littéral, faites-les précéder d'une barre oblique inversée
et ils ne seront pas interprêtés comme délimiteurs mathématiques.

For display math, use <span class="nowrap">`$$`</span> delimiters. (In
this case, the delimiters may be separated from the formula by
whitespace.)

Les mathématiques TeX seront imprimées dans tous les formats de sortie.
La manière dont il est rendu dépend du format de sortie :

LaTeX  
It will appear verbatim surrounded by <span
class="nowrap">`\(...\)`</span> (for inline math) or <span
class="nowrap">`\[...\]`</span> (for display math).

Markdown, Emacs Org mode, ConTeXt, ZimWiki  
It will appear verbatim surrounded by <span
class="nowrap">`$...$`</span> (for inline math) or <span
class="nowrap">`$$...$$`</span> (for display math).

XWiki  
It will appear verbatim surrounded by <span
class="nowrap">`{{formula}}..{{/formula}}`</span>.

reStructuredText  
It will be rendered using an [interpreted text role <span
class="nowrap">`:math:`</span>](https://docutils.sourceforge.io/docs/ref/rst/roles.html#math).

AsciiDoc  
For AsciiDoc output format
(<a href="#option--to" class="option"><span class="nowrap"><code>-t asciidoc</code></span></a>)
it will appear verbatim surrounded by <span
class="nowrap">`latexmath:[$...$]`</span> (for inline math) or <span
class="nowrap">`[latexmath]++++\[...\]+++`</span> (for display math).
For AsciiDoctor output format
(<a href="#option--to" class="option"><span class="nowrap"><code>-t asciidoctor</code></span></a>)
the LaTex delimiters (<span class="nowrap">`$..$`</span> and <span
class="nowrap">`\[..\]`</span>) are omitted.

Texinfo  
It will be rendered inside a <span class="nowrap">`@math`</span>
command.

roff man, Jira markup  
It will be rendered verbatim without <span class="nowrap">`$`</span>’s.

MediaWiki, DokuWiki  
It will be rendered inside <span class="nowrap">`<math>`</span> tags.

Textile  
It will be rendered inside <span
class="nowrap">`<span                     class="math">`</span> tags.

RTF, OpenDocument  
It will be rendered, if possible, using Unicode characters, and will
otherwise appear verbatim.

ODT  
It will be rendered, if possible, using MathML.

DocBook  
If the
<a href="#option--mathml" class="option"><span class="nowrap"><code>--mathml</code></span></a>
flag is used, it will be rendered using MathML in an <span
class="nowrap">`inlineequation`</span> or <span
class="nowrap">`informalequation`</span> tag. Otherwise it will be
rendered, if possible, using Unicode characters.

Docx  
It will be rendered using OMML math markup.

FictionBook2  
If the
<a href="#option--webtex" class="option"><span class="nowrap"><code>--webtex</code></span></a>
option is used, formulas are rendered as images using CodeCogs or other
compatible web service, downloaded and embedded in the e-book.
Otherwise, they will appear verbatim.

HTML, Slidy, DZSlides, S5, EPUB  
The way math is rendered in HTML will depend on the command-line options
selected. Therefore see [Math rendering in
HTML](#math-rendering-in-html) above.

HTML brut
---------

#### Extension: <span class="nowrap">`raw_html`</span>

Markdown vous permet d'insérer du HTML brut (ou DocBook) n'importe où
dans un document (sauf dans les contextes verbatim, où <span
class="nowrap">`<`</span>, <span class="nowrap">`>`</span>, et <span
class="nowrap">`&`</span> sont interprétés littéralement).
(Techniquement, il ne s'agit pas d'une extension, puisque la norme
Markdown le permet, mais il a été fait une extension pour qu'il puisse
être désactivé si désiré).  
  
Le HTML brut est transmis tel quel en HTML, S5, Slidy, Slideous,
DZSlides, EPUB, Markdown, CommonMark, Emacs Org mode, et Textile output,
et supprimé dans les autres formats.  
  
Pour une façon plus explicite d'inclure du HTML brut dans un document
Markdown, voir l'extension [<span class="nowrap">`raw_attribute`</span>
extension](#extension-raw_attribute).

Dans le format CommonMark, si <span class="nowrap">`raw_html`</span> est
activée, les exposants, les indices, les biffures et les petites
capitales seront représentés en HTML. Dans le cas contraire, les repères
(*fallbacks*) en texte clair seront utilisés. Notez que même si  <span
class="nowrap">`raw_html`</span> est désactivé, les tableaux seront
rendus avec la syntaxe HTML s'ils ne peuvent pas utiliser la syntaxe du
pipe.

#### Extension: <span class="nowrap">`markdown_in_html_blocks`</span>

Le Markdown standard vous permet d'inclure des "blocs" HTML : des blocs
de HTML entre des balises symétriques qui sont séparées du texte
environnant par des lignes blanches, et dont le début et la fin se
trouvent dans la marge gauche. À l'intérieur de ces blocs, tout est
interprété comme du HTML, et non du Markdown ; donc (par exemple), <span
class="nowrap">`*`</span>ne signifie pas "mise en emphase".

Pandoc se comporte de cette façon lorsque le format <span
class="nowrap">`markdown_strict`</span>est utilisé ; mais par défaut,
pandoc interprète le matériel entre les balises de bloc HTML comme étant
du Markdown. Ainsi, par exemple, pandoc transformera

    <table>
    <tr>
    <td>*one*</td>
    <td>[a link](https://google.com)</td>
    </tr>
    </table>

en

    <table>
    <tr>
    <td><em>one</em></td>
    <td><a href="https://google.com">a link</a></td>
    </tr>
    </table>

alors que <span class="nowrap">`Markdown.pl`</span> le conservera tel
quel.

Il y a une exception à cette règle : le texte entre les balises <span
class="nowrap">`<script>`</span>et  <span
class="nowrap">`<style>`</span> n'est pas interprétée comme du
Markdown.  
  
Cet écart par rapport au standard Markdown devrait faciliter le mélange
de Markdown avec des éléments de bloc HTML. Par exemple, on peut
entourer un bloc de texte Markdown avec des balises <span
class="nowrap">`<div>`</span> sans empêcher qu'il soit interprété comme
un Markdown.

#### Extension: <span class="nowrap">`native_divs`</span>

Utilise les blocs pandoc natifs <span class="nowrap">`Div`</span>pour du
contenu entre balises <span class="nowrap">`<div>`</span>. Pour
l'essentiel, cela devrait donner le même résultat que <span
class="nowrap">`markdown_in_html_blocks`</span>, mais il est plus facile
d'écrire des filtres pandoc pour manipuler des groupes de blocs.

#### Extension: <span class="nowrap">`native_spans`</span>

Utilise les blocs pandoc <span class="nowrap">`Span`</span> natifs pour
le contenu entre balises <span class="nowrap">`<span>`</span>. Pour
l'essentiel, cela devrait donner le même résultat que <span
class="nowrap">`raw_html`</span>, mais il est plus facile d'écrire des
filtres pandoc pour manipuler des groupes d'en ligne ("*inlines*").

#### Extension: <span class="nowrap">`raw_tex`</span>

En plus du HTML brut, pandoc permet d'inclure du LaTeX, TeX et ConTeXt
brut dans un document. Les commandes TeX en ligne seront préservées et
transmises telles quelles aux auteurs LaTeX et ConTeXt. Ainsi, par
exemple, vous pouvez utiliser LaTeX pour inclure des citations BibTeX :

    This result was proved in \cite{jones.1967}.

Notez que dans les environnements LaTeX, comme

    \begin{tabular}{|l|l|}\hline
    Age & Frequency \\ \hline
    18--25  & 15 \\
    26--35  & 33 \\
    36--45  & 22 \\ \hline
    \end{tabular}

le matériel entre les balises de début et de fin sera interprété comme
du LaTeX brut, et non comme du Markdown.  
  
Pour une manière plus explicite et plus souple d'inclure du TeX brut
dans un document Markdown, voir l'extension [<span
class="nowrap">`raw_attribute`</span>
extension](#extension-raw_attribute).

Le LaTeX en ligne est ignoré dans les formats de sortie autres que
Markdown, LaTeX, le mode Emacs Org et ConTeXt.

### Generic raw attribute

#### Extension: <span class="nowrap">`raw_attribute`</span>

Inline spans and fenced code blocks with a special kind of attribute
will be parsed as raw content with the designated format. For example,
the following produces a raw roff <span class="nowrap">`ms`</span>
block:

    ```{=ms}
    .MYMACRO
    blah blah
    ```

And the following produces a raw <span class="nowrap">`html`</span>
inline element:

    This is `<a>html</a>`{=html}

This can be useful to insert raw xml into <span
class="nowrap">`docx`</span> documents, e.g. a pagebreak:

    ```{=openxml}
    <w:p>
      <w:r>
        <w:br w:type="page"/>
      </w:r>
    </w:p>
    ```

The format name should match the target format name (see
<a href="#option--to" class="option"><span class="nowrap"><code>-t/--to</code></span></a>,
above, for a list, or use <span
class="nowrap">`pandoc                   --list-output-formats`</span>).
Use <span class="nowrap">`openxml`</span> for <span
class="nowrap">`docx`</span> output, <span
class="nowrap">`opendocument`</span> for <span
class="nowrap">`odt`</span> output, <span class="nowrap">`html5`</span>
for <span class="nowrap">`epub3`</span> output, <span
class="nowrap">`html4`</span> for <span class="nowrap">`epub2`</span>
output, and <span class="nowrap">`latex`</span>, <span
class="nowrap">`beamer`</span>, <span class="nowrap">`ms`</span>, or
<span class="nowrap">`html5`</span> for <span
class="nowrap">`pdf`</span> output (depending on what you use for
<a href="#option--pdf-engine" class="option"><span class="nowrap"><code>--pdf-engine</code></span></a>).

This extension presupposes that the relevant kind of inline code or
fenced code block is enabled. Thus, for example, to use a raw attribute
with a backtick code block, <span
class="nowrap">`backtick_code_blocks`</span> must be enabled.

The raw attribute cannot be combined with regular attributes.

LaTeX macros
------------

#### Extension: <span class="nowrap">`latex_macros`</span>

Lorsque cette extension est activée, pandoc analyse les définitions de
macros LaTeX et applique les macros résultantes à tous les calculs LaTeX
et LaTeX brut. Ainsi, par exemple, ce qui suit fonctionnera dans tous
les formats de sortie, pas seulement LaTeX :

    \newcommand{\tuple}[1]{\langle #1 \rangle}

    $\tuple{a, b, c}$

Note that LaTeX macros will not be applied if they occur inside a raw
span or block marked with the [<span
class="nowrap">`raw_attribute`</span>
extension](#extension-raw_attribute).

Lorsque <span class="nowrap">`latex_macros`</span> est désactivé, le
LaTeX brut et les mathématiques n'auront pas de macros appliquées. Cette
approche est généralement préférable lorsque vous visez le LaTeX ou le
PDF.  
  
Les définitions de macros en LaTeX ne seront transmises en tant que
LaTeX brut que si <span class="nowrap">`latex_macros`</span> n'est pas
activée. Les définitions des macros dans la source Markdown (ou d'autres
formats permettant <span class="nowrap">`raw_tex`</span>) sera transmise
indépendamment du fait que <span class="nowrap">`latex_macros`</span>est
activé .

Liens
-----

Markdown permet de spécifier les liens de plusieurs façons.

### Automatic links

Si vous indiquez une URL ou une adresse électronique entre crochets,
elle deviendra un lien :

    <https://google.com>
    <sam@green.eggs.ham>

### Liens en ligne

Un lien en ligne se compose du texte du lien entre crochets, suivi de
l'URL entre parenthèses. (Facultativement, l'URL peut être suivie d'un
titre de lien, entre guillemets).

    This is an [inline link](/url), and here's [one with
    a title](https://fsf.org "click here for a good time!").

Il ne peut y avoir aucun espace entre la partie entre crochets et la
partie entre parenthèses. Le texte du lien peut contenir une mise en
forme (comme l'accentuation), mais pas le titre.  
  
Les adresses électroniques dans les liens en ligne ne sont pas
autodétectées, elles doivent donc être préfixées par <span
class="nowrap">`mailto`</span>:

    [Write me!](mailto:sam@green.eggs.ham)

### Liens vers des références

Un lien de référence *explicite* comporte deux parties, le lien lui-même
et la définition du lien, qui peut se trouver ailleurs dans le document
(soit avant, soit après le lien).  
  
Le lien se compose du texte du lien entre crochets, suivi d'une
étiquette entre crochets. (Il ne peut y avoir d'espace entre les deux,
sauf si l'extension <span class="nowrap">`spaced_reference_links`</span>
est validée.) La définition du lien se compose de l'étiquette entre
crochets, suivie des deux points et d'un espace, puis de l'URL et,
éventuellement (après un espace), d'un titre de lien entre guillemets ou
entre parenthèses. L'étiquette ne doit pas pouvoir être analysée comme
une citation (en supposant que l'extension <span
class="nowrap">`citations`</span> est activée): Les citations ont la
priorité sur les étiquettes de liens.  
  
Voici quelques exemples :

    [my label 1]: /foo/bar.html  "My title, optional"
    [my label 2]: /foo
    [my label 3]: https://fsf.org (The free software foundation)
    [my label 4]: /bar#special  'A title in single quotes'

L'URL peut éventuellement être entourée des signes &lt;&gt; :

    [my label 5]: <http://foo.bar.baz>

Le titre peut aller sur la ligne suivante

    [my label 3]: https://fsf.org
      "The free software foundation"

Notez que les étiquettes de liens ne sont pas sensibles à la casse.
Donc, cela va fonctionner :

    Here is [my link][FOO]

    [Foo]: /bar/baz

Dans un lien de référence implicite, la deuxième paire de crochets est
vide :

    See [my website][].

    [my website]: http://foo.bar.baz

Note: Dans <span class="nowrap">`Markdown.pl`</span> et la plupart des
autres implémentations de Markdown, les définitions de liens de
référence ne peuvent pas se produire dans des constructions imbriquées
telles que des éléments de liste ou des citations en bloc. Pandoc lève
cette apparente restriction arbitraire. Ce qui suit est donc très bien
dans le cadre de pandoc, mais pas dans la plupart des autres mises en
œuvre :

    > My block [quote].
    >
    > [quote]: /foo

#### Extension: <span class="nowrap">`shortcut_reference_links`</span>

Dans un lien de référence de raccourci, la deuxième paire de crochets
peut être entièrement omise :

    See [my website].

    [my website]: http://foo.bar.baz

### Liens internes

Pour établir un lien avec une autre section du même document, utilisez
l'identifiant généré automatiquement (voir [Heading
identifiers](#heading-identifiers)). Par exemple:

    See the [Introduction](#introduction).

ou

    See the [Introduction].

    [Introduction]: #introduction

Les liens internes sont actuellement pris en charge pour les formats
HTML (y compris les diaporamas HTML et EPUB), LaTeX et ConTeXt.

Images
------

Un lien immédiatement précédé d'un <span class="nowrap">`!`</span> sera
traitée comme une image. Le texte du lien sera utilisé comme texte
alternatif de l'image :

    ![la lune](lalune.jpg "Voyage to the moon")

    ![movie reel]

    [movie reel]: movie.gif

#### Extension: <span class="nowrap">`implicit_figures`</span>

Une image avec un texte alt non vide, se produisant par lui-même dans un
paragraphe, sera rendue comme une figure avec une légende. Le texte alt
de l'image sera utilisé comme légende.

    ![This is the caption](/url/of/image.png)

La manière dont cela est rendu dépend du format de sortie. Certains
formats de sortie (par exemple, RTF) ne permettent pas encore d'obtenir
des chiffres. Dans ces formats, vous obtiendrez simplement une image
dans un paragraphe, sans légende.  
  
Si vous voulez juste une image en ligne régulière, assurez-vous que ce
n'est pas la seule chose dans le paragraphe. Une façon de le faire est
d'insérer un espace insécable après l'image :

    ![This image won't be a figure](/url/of/image.png)\

Notez que dans les diaporamas de reveal.js, une image dans un paragraphe
à elle seule qui a la classe <span class="nowrap">`stretch`</span>
remplira l'écran, et les balises de légende et de figure seront omises.

#### Extension: <span class="nowrap">`link_attributes`</span>

Des attributs peuvent être définis sur les liens et les images :

    An inline ![image](foo.jpg){#id .class width=30 height=20px}
    and a reference ![image][ref] with attributes.

    [ref]: foo.jpg "optional title" {#id .class key=val key2="val 2"}

(Cette syntaxe est compatible avec [PHP Markdown
Extra](https://michelf.ca/projects/php-markdown/extra/) lorsque seuls
<span class="nowrap">`#id`</span>et  <span
class="nowrap">`.class`</span> sont utilisés.)

Pour HTML et EPUB, tous les attributs connus de HTML5 sauf <span
class="nowrap">`width`</span>et  <span class="nowrap">`height`</span>
(mais incluant <span class="nowrap">`srcset`</span> et <span
class="nowrap">`sizes`</span>) sont passés tels quels. Les attributs
inconnus sont transmis en tant qu'attributs personnalisés, avec <span
class="nowrap">`data-`</span> prepended. Les autres auteurs ignorent les
attributs qui ne sont pas spécifiquement pris en charge par leur format
de sortie.  
  
Les attributs <span class="nowrap">`width`</span> et <span
class="nowrap">`height`</span>sur les images sont traités spécialement .
Lorsqu'elle est utilisée sans unité, l'unité est supposée être le pixel.
Toutefois, les identificateurs d'unité suivants peuvent être utilisés :
<span class="nowrap">`px`</span>, <span class="nowrap">`cm`</span>,
<span class="nowrap">`mm`</span>, <span class="nowrap">`in`</span>,
<span class="nowrap">`inch`</span> et <span class="nowrap">`%`</span>.
Il ne doit y avoir aucun espace entre le nombre et l'unité. Par exemple
:

    ![](file.jpg){ width=50% }

-   Les dimensions sont converties en pouces pour une sortie dans des
    formats de page comme LaTeX. Les dimensions sont converties en
    pixels pour une sortie au format HTML. Utilisez l'option
    <a href="#option--dpi" class="option"><span class="nowrap"><code>--dpi</code></span></a>
    spécifier le nombre de pixels par pouce. La valeur par défaut est de
    96 dpi.
-   L'unité <span class="nowrap">`%`</span> est généralement relative à
    un certain espace disponible. Par exemple, l'exemple ci-dessus se
    traduira par ce qui suit.
    -   HTML: <span
        class="nowrap">`<img href="file.jpg"                         style="width: 50%;" />`</span>
    -   LaTeX: <span
        class="nowrap">`\includegraphics[width=0.5\textwidth,height=\textheight]{file.jpg}`</span>
        (Si vous utilisez un modèle personnalisé, vous devez configurer
        <span class="nowrap">`graphicx`</span> as in the default
        template.)
    -   ConTeXt: <span
        class="nowrap">`\externalfigure[file.jpg][width=0.5\textwidth]`</span>
-   Some output formats have a notion of a class
    ([ConTeXt](https://wiki.contextgarden.net/Using_Graphics#Multiple_Image_Settings))
    or a unique identifier (LaTeX <span
    class="nowrap">`\caption`</span>), or both (HTML).
-   When no <span class="nowrap">`width`</span> or <span
    class="nowrap">`height`</span> attributes are specified, the
    fallback is to look at the image resolution and the dpi metadata
    embedded in the image file.

Divs and Spans
--------------

En utilisant les extensions <span class="nowrap">`native_divs`</span>et
<span class="nowrap">`native_spans`</span> (voir
[ci-dessus](#extension-native_divs)), la syntaxe HTML peut être utilisé
dans le cadre de Markdown pour créer des éléments <span
class="nowrap">`Div`</span> et <span class="nowrap">`Span`</span> natifs
dans l'AST pandoc (en opposition au HTML brut). Toutefois, il existe
également une syntaxe plus agréable :

#### Extension: <span class="nowrap">`fenced_divs`</span>

Allow special fenced syntax for native <span class="nowrap">`Div`</span>
blocks. A Div starts with a fence containing at least three consecutive
colons plus some attributes. The attributes may optionally be followed
by another string of consecutive colons. The attribute syntax is exactly
as in fenced code blocks (see [Extension: <span
class="nowrap">`fenced_code_attributes`</span>](#extension-fenced_code_attributes)).
As with fenced code blocks, one can use either attributes in curly
braces or a single unbraced word, which will be treated as a class name.
The Div ends with another line containing a string of at least three
consecutive colons. The fenced Div should be separated by blank lines
from preceding and following blocks.

Example:

    ::::: {#special .sidebar}
    Here is a paragraph.

    And another.
    :::::

Fenced divs can be nested. Opening fences are distinguished because they
*must* have attributes:

    ::: Warning ::::::
    This is a warning.

    ::: Danger
    This is a warning within a warning.
    :::
    ::::::::::::::::::

Fences without attributes are always closing fences. Unlike with fenced
code blocks, the number of colons in the closing fence need not match
the number in the opening fence. However, it can be helpful for visual
clarity to use fences of different lengths to distinguish nested divs
from their parents.

#### Extension: <span class="nowrap">`bracketed_spans`</span>

Une séquence d'inlines entre crochets, comme on le ferait pour commencer
un lien, sera traitée comme un <span class="nowrap">`Span`</span> avec
des attributs si elle est suivie immédiatement par des attributs :

    [This is *some text*]{.class key="val"}

Footnotes
---------

#### Extension: <span class="nowrap">`footnotes`</span>

Le Markdown de Pandoc permet d'ajouter des notes de bas de page, en
utilisant la syntaxe suivante :

    Here is a footnote reference,[^1] and another.[^longnote]

    [^1]: Here is the footnote.

    [^longnote]: Here's one with multiple blocks.

        Subsequent paragraphs are indented to show that they
    belong to the previous footnote.

            { some.code }

        The whole paragraph can be indented, or just the first
        line.  In this way, multi-paragraph footnotes work like
        multi-paragraph list items.

    This paragraph won't be part of the note, because it
    isn't indented.

Les identificateurs dans les références de notes de bas de page ne
doivent pas contenir d'espaces, de tabulations ou de nouvelles lignes.
Ces identificateurs sont utilisés uniquement pour corréler la référence
de la note de bas de page avec la note elle-même ; dans la sortie, les
notes de bas de page seront numérotées séquentiellement.  
  
Les notes de bas de page elles-mêmes ne doivent pas être placées à la
fin du document. Ils peuvent apparaître n'importe où sauf à l'intérieur
d'autres éléments de bloc (listes, blocs de citations, tableaux, etc.).
Chaque note de bas de page doit être séparée du contenu environnant (y
compris les autres notes de bas de page) par des lignes blanches.

#### Extension: <span class="nowrap">`inline_notes`</span>

Les notes de bas de page en ligne sont également autorisées (bien que,
contrairement aux notes normales, elles ne puissent pas contenir
plusieurs paragraphes). La syntaxe est la suivante :

    Here is an inline note.^[Inlines notes are easier to write, since
    you don't have to pick an identifier and move down to type the
    note.]

Les notes de bas de page en ligne et régulières peuvent être mélangées
librement.

Citations
---------

#### Extension: <span class="nowrap">`citations`</span>

En utilisant un filtre externe, <span
class="nowrap">`pandoc-citeproc`</span> pandoc peut générer
automatiquement des citations et une bibliographie dans un certain
nombre de styles. L'utilisation de base est,

    pandoc --filter pandoc-citeproc myinput.txt

Pour utiliser cette fonction, vous devrez spécifier un fichier
bibliographique à l'aide du champ de métadonnée <span
class="nowrap">`bibliography`</span>dans une section de métadonnées 
YAML, ou l'argument en ligne de commande
<a href="#option--bibliography" class="option"><span class="nowrap"><code>--bibliography</code></span></a>.
Vous pouvez fournir plusieurs arguments
<a href="#option--bibliography" class="option"><span class="nowrap"><code>--bibliography</code></span></a>ou
définir un champ de métadonnées  <span
class="nowrap">`bibliography`</span> dans le tableau YAML, si vous
souhaitez utiliser plusieurs fichiers bibliographiques. La bibliographie
peut avoir n'importe lequel de ces formats :

| Format      | File extension |
|:------------|----------------|
| BibLaTeX    | .bib           |
| BibTeX      | .bibtex        |
| Copac       | .copac         |
| CSL JSON    | .json          |
| CSL YAML    | .yaml          |
| EndNote     | .enl           |
| EndNote XML | .xml           |
| ISI         | .wos           |
| MEDLINE     | .medline       |
| MODS        | .mods          |
| RIS         | .ris           |

Notez que <span class="nowrap">`.bib`</span> peut être utilisé avec les
fichiers BibTeX et BibLaTeX ; utilisez <span
class="nowrap">`.bibtex`</span>pour  forcer BibTeX.

Notez que <span class="nowrap">`pandoc-citeproc --bib2json`</span>et 
<span class="nowrap">`pandoc-citeproc --bib2yaml`</span> peuvent
produire des fichiers <span class="nowrap">`.json`</span> et <span
class="nowrap">`.yaml`</span> depuis n'importe lequel des fichiers
supportés.

Balises intra-champ : dans les bases de données BibTeX et BibLaTeX,
pandoc-citeproc analyse un sous-ensemble du balisage LaTeX ; dans les
bases de données CSL YAML, pandoc Markdown ; et dans les bases de
données CSL JSON, un balisage
[HTML-like](https://docs.citationstyles.org/en/1.0/release-notes.html#rich-text-markup-within-fields)
:

<span class="nowrap">`<i>...</i>`</span>  
italics

<span class="nowrap">`<b>...</b>`</span>  
bold

<span class="nowrap">`<span                     style="font-variant:small-caps;">...</span>`</span> or <span class="nowrap">`<sc>...</sc>`</span>  
small capitals

<span class="nowrap">`<sub>...</sub>`</span>  
subscript

<span class="nowrap">`<sup>...</sup>`</span>  
superscript

<span class="nowrap">`<span                     class="nocase">...</span>`</span>  
prevent a phrase from being capitalized as title case

<span class="nowrap">`pandoc-citeproc -j`</span> et <span
class="nowrap">`-y`</span> convertissent autant que possible les formats
CSL JSON et CSL YAML.

Au lieu de spécifier un fichier bibliographique en utilisant
<a href="#option--bibliography" class="option"><span class="nowrap"><code>--bibliography</code></span></a>
ou le champ de métadonnées YAML <span
class="nowrap">`bibliography`</span> vous pouvez inclure les données de
citation directement dans le champ <span
class="nowrap">`references`</span> des métadonnées YAML du document. Le
champ doit contenir un tableau de références codées en YAML, par exemple
:

    ---
    references:
    - type: article-journal
      id: WatsonCrick1953
      author:
      - family: Watson
        given: J. D.
      - family: Crick
        given: F. H. C.
      issued:
        date-parts:
        - - 1953
          - 4
          - 25
      title: 'Molecular structure of nucleic acids: a structure for deoxyribose
        nucleic acid'
      title-short: Molecular structure of nucleic acids
      container-title: Nature
      volume: 171
      issue: 4356
      page: 737-738
      DOI: 10.1038/171737a0
      URL: https://www.nature.com/articles/171737a0
      language: en-GB
    ...

(<span class="nowrap">`pandoc-citeproc --bib2yaml`</span> peut les
produire à partir d'un fichier bibliographique dans l'un des formats
pris en charge .)

Citations and references can be formatted using any style supported by
the [Citation Style Language](https://citationstyles.org/), listed in
the [Zotero Style Repository](https://www.zotero.org/styles). These
files are specified using the
<a href="#option--csl" class="option"><span class="nowrap"><code>--csl</code></span></a>
option or the <span class="nowrap">`csl`</span> metadata field. By
default, <span class="nowrap">`pandoc-citeproc`</span> will use the
[Chicago Manual of Style](https://chicagomanualofstyle.org/) author-date
format. The CSL project provides further information on [finding and
editing styles](https://citationstyles.org/authors/).

To make your citations hyperlinks to the corresponding bibliography
entries, add <span
class="nowrap">`link-citations:                   true`</span> to your
YAML metadata.

Citations go inside square brackets and are separated by semicolons.
Each citation must have a key, composed of ‘@’ + the citation identifier
from the database, and may optionally have a prefix, a locator, and a
suffix. The citation key must begin with a letter, digit, or <span
class="nowrap">`_`</span>, and may contain alphanumerics, <span
class="nowrap">`_`</span>, and internal punctuation characters (<span
class="nowrap">`:.#$%&-+?<>~/`</span>). Here are some examples:

    Blah blah [see @doe99, pp. 33-35; also @smith04, chap. 1].

    Blah blah [@doe99, pp. 33-35, 38-39 and *passim*].

    Blah blah [@smith04; @doe99].

<span class="nowrap">`pandoc-citeproc`</span> detects locator terms in
the [CSL locale
files](https://github.com/citation-style-language/locales). Either
abbreviated or unabbreviated forms are accepted. In the <span
class="nowrap">`en-US`</span> locale, locator terms can be written in
either singular or plural forms, as <span class="nowrap">`book`</span>,
<span class="nowrap">`bk.`</span>/<span class="nowrap">`bks.`</span>;
<span class="nowrap">`chapter`</span>, <span
class="nowrap">`chap.`</span>/<span class="nowrap">`chaps.`</span>;
<span class="nowrap">`column`</span>, <span
class="nowrap">`col.`</span>/<span class="nowrap">`cols.`</span>; <span
class="nowrap">`figure`</span>, <span class="nowrap">`fig.`</span>/<span
class="nowrap">`figs.`</span>; <span class="nowrap">`folio`</span>,
<span class="nowrap">`fol.`</span>/<span class="nowrap">`fols.`</span>;
<span class="nowrap">`number`</span>, <span
class="nowrap">`no.`</span>/<span class="nowrap">`nos.`</span>; <span
class="nowrap">`line`</span>, <span class="nowrap">`l.`</span>/<span
class="nowrap">`ll.`</span>; <span class="nowrap">`note`</span>, <span
class="nowrap">`n.`</span>/<span class="nowrap">`nn.`</span>; <span
class="nowrap">`opus`</span>, <span class="nowrap">`op.`</span>/<span
class="nowrap">`opp.`</span>; <span class="nowrap">`page`</span>, <span
class="nowrap">`p.`</span>/<span class="nowrap">`pp.`</span>; <span
class="nowrap">`paragraph`</span>, <span
class="nowrap">`para.`</span>/<span class="nowrap">`paras.`</span>;
<span class="nowrap">`part`</span>, <span
class="nowrap">`pt.`</span>/<span class="nowrap">`pts.`</span>; <span
class="nowrap">`section`</span>, <span
class="nowrap">`sec.`</span>/<span class="nowrap">`secs.`</span>; <span
class="nowrap">`sub                   verbo`</span>, <span
class="nowrap">`s.v.`</span>/<span class="nowrap">`s.vv.`</span>; <span
class="nowrap">`verse`</span>, <span class="nowrap">`v.`</span>/<span
class="nowrap">`vv.`</span>; <span class="nowrap">`volume`</span>, <span
class="nowrap">`vol.`</span>/<span class="nowrap">`vols.`</span>; <span
class="nowrap">`¶`</span>/<span class="nowrap">`¶¶`</span>; <span
class="nowrap">`§`</span>/<span class="nowrap">`§§`</span>. If no
locator term is used, “page” is assumed.

<span class="nowrap">`pandoc-citeproc`</span> will use heuristics to
distinguish the locator from the suffix. In complex cases, the locator
can be enclosed in curly braces (using <span
class="nowrap">`pandoc-citeproc`</span> 0.15 and higher only):

    [@smith{ii, A, D-Z}, with a suffix]
    [@smith, {pp. iv, vi-xi, (xv)-(xvii)} with suffix here]

A minus sign (<span class="nowrap">`-`</span>) before the <span
class="nowrap">`@`</span> will suppress mention of the author in the
citation. This can be useful when the author is already mentioned in the
text:

    Smith says blah [-@smith04].

You can also write an in-text citation, as follows:

    @smith04 says blah.

    @smith04 [p. 33] says blah.

If the style calls for a list of works cited, it will be placed in a div
with id <span class="nowrap">`refs`</span>, if one exists:

    ::: {#refs}
    :::

Otherwise, it will be placed at the end of the document. Generation of
the bibliography can be suppressed by setting <span
class="nowrap">`suppress-bibliography: true`</span> in the YAML
metadata.

If you wish the bibliography to have a section heading, you can set
<span class="nowrap">`reference-section-title`</span> in the metadata,
or put the heading at the beginning of the div with id <span
class="nowrap">`refs`</span> (if you are using it) or at the end of your
document:

    last paragraph...

    # References

The bibliography will be inserted after this heading. Note that the
<span class="nowrap">`unnumbered`</span> class will be added to this
heading, so that the section will not be numbered.

If you want to include items in the bibliography without actually citing
them in the body text, you can define a dummy <span
class="nowrap">`nocite`</span> metadata field and put the citations
there:

    ---
    nocite: |
      @item1, @item2
    ...

    @item3

In this example, the document will contain a citation for <span
class="nowrap">`item3`</span> only, but the bibliography will contain
entries for <span class="nowrap">`item1`</span>, <span
class="nowrap">`item2`</span>, and <span class="nowrap">`item3`</span>.

It is possible to create a bibliography with all the citations, whether
or not they appear in the document, by using a wildcard:

    ---
    nocite: |
      @*
    ...

For LaTeX output, you can also use [<span
class="nowrap">`natbib`</span>](https://ctan.org/pkg/natbib) or [<span
class="nowrap">`biblatex`</span>](https://ctan.org/pkg/biblatex) to
render the bibliography. In order to do so, specify bibliography files
as outlined above, and add
<a href="#option--natbib" class="option"><span class="nowrap"><code>--natbib</code></span></a>
or
<a href="#option--biblatex" class="option"><span class="nowrap"><code>--biblatex</code></span></a>
argument to <span class="nowrap">`pandoc`</span> invocation. Bear in
mind that bibliography files have to be in respective format (either
BibTeX or BibLaTeX).

For more information, see the [pandoc-citeproc man
page](https://github.com/jgm/pandoc-citeproc/blob/master/man/pandoc-citeproc.1.md).

Non-pandoc extensions
---------------------

Les extensions syntaxiques Markdown suivantes ne sont pas activées par
défaut dans pandoc, mais peuvent être activées en ajoutant <span
class="nowrap">`+EXTENSION`</span> au nom du format, où <span
class="nowrap">`EXTENSION`</span> est le nom de l'extension. Ainsi, par
example, <span class="nowrap">`markdown+hard_line_breaks`</span> is
Markdown with hard line breaks.

#### Extension: <span class="nowrap">`old_dashes`</span>

Selects the pandoc &lt;= 1.8.2.1 behavior for parsing smart dashes:
<span class="nowrap">`-`</span> before a numeral is an en-dash, and
<span class="nowrap">`--`</span> is an em-dash. This option only has an
effect if <span class="nowrap">`smart`</span> is enabled. It is selected
automatically for <span class="nowrap">`textile`</span> input.

#### Extension: <span class="nowrap">`angle_brackets_escapable`</span>

Allow <span class="nowrap">`<`</span> and <span
class="nowrap">`>`</span> to be backslash-escaped, as they can be in
GitHub flavored Markdown but not original Markdown. This is implied by
pandoc’s default <span class="nowrap">`all_symbols_escapable`</span>.

#### Extension: <span class="nowrap">`lists_without_preceding_blankline`</span>

Permettre à une liste de se produire juste après un paragraphe, sans
espace vide intermédiaire.

#### Extension: <span class="nowrap">`four_space_rule`</span>

Sélectionne le comportement pandoc &lt;= 2.0 pour l'analyse des listes,
de sorte que quatre espaces d'indentation sont nécessaires pour les
paragraphes de continuation des éléments de la liste.

#### Extension: <span class="nowrap">`spaced_reference_links`</span>

Permettre des espaces entre les deux composantes d'un lien de référence,
par exemple,

    [foo] [bar].

#### Extension: <span class="nowrap">`hard_line_breaks`</span>

Toutes les nouvelles lignes d'un paragraphe doivent être interprétées
comme des sauts de ligne au lieu d'espaces.

#### Extension: <span class="nowrap">`ignore_line_breaks`</span>

Il fait en sorte que les nouvelles lignes à l'intérieur d'un paragraphe
soient ignorées, plutôt que d'être traitées comme des espaces ou des
sauts de ligne. Cette option est destinée aux langues d'Asie de l'Est où
les espaces ne sont pas utilisés entre les mots, mais où le texte est
divisé en lignes pour plus de lisibilité.

#### Extension: <span class="nowrap">`east_asian_line_breaks`</span>

Fait en sorte que les nouveaux traits à l'intérieur d'un paragraphe
soient ignorés, plutôt que d'être traités comme des espaces ou des sauts
de ligne, lorsqu'ils se produisent entre deux caractères larges d'Asie
de l'Est. C'est un meilleur choix que <span
class="nowrap">`ignore_line_breaks`</span> pour les textes qui
comprennent un mélange de caractères larges d'Asie de l'Est et d'autres
caractères.

#### Extension: <span class="nowrap">`emoji`</span>

Analyse les émojis textuels comme <span
class="nowrap">`:smile:`</span>en tant qu'émoticônes Unicode.

#### Extension: <span class="nowrap">`tex_math_single_backslash`</span>

Tout ce qui se trouve entre <span class="nowrap">`\(`</span>et  <span
class="nowrap">`\)`</span> doit être interprété comme des calculs de TeX
en ligne, et tout ce qui se trouve entre <span
class="nowrap">`\[`</span>et  <span class="nowrap">`\]`</span> doit être
interprété comme des calculs de TeX en affichage. Note: un inconvénient
de cette extension est qu'elle empêche l'échappement de <span
class="nowrap">`(`</span> et <span class="nowrap">`[`</span>.

#### Extension: <span class="nowrap">`tex_math_double_backslash`</span>

Tout ce qui se situe entre <span class="nowrap">`\\(`</span>et  <span
class="nowrap">`\\)`</span> doit être interprété comme un calcul TeX en
ligne, et tout ce qui se situe entre <span
class="nowrap">`\\[`</span>et  <span class="nowrap">`\\]`</span> doit
être interprété comme un affichage TeX math.

#### Extension: <span class="nowrap">`markdown_attribute`</span>

Par défaut, pandoc interprète le matériel à l'intérieur des balises de
niveau bloc comme étant du Markdown. Cette extension modifie le
comportement de sorte que Markdown n'est analysé à l'intérieur des
balises de niveau bloc que si les balises ont l'attribut <span
class="nowrap">`markdown=1`</span>.

#### Extension: <span class="nowrap">`mmd_title_block`</span>

Permet un bloc titre de style
[MultiMarkdown](https://fletcherpenney.net/multimarkdown/) en haut du
document, par exemple:

    Title:   My title
    Author:  John Doe
    Date:    September 1, 2008
    Comment: This is a sample mmd title block, with
             a field spanning multiple lines.

Voir la documentation MultiMarkdown pour plus de détails. Si les
fonctions <span class="nowrap">`pandoc_title_block`</span>ou  <span
class="nowrap">`yaml_metadata_block`</span>sont activées, elles auront
la priorité sur  <span class="nowrap">`mmd_title_block`</span>.

#### Extension: <span class="nowrap">`abbreviations`</span>

Parses PHP Markdown Extra abbreviation keys, like

    *[HTML]: Hypertext Markup Language

Notez que le modèle de document pandoc ne supporte pas les abréviations,
donc si cette extension est activée, les clés d'abréviation sont
simplement ignorées (au lieu d'être analysées comme des paragraphes).

#### Extension: <span class="nowrap">`autolink_bare_uris`</span>

Transforme tous les URI absolus en liens, même s'ils ne sont pas
entourés d'accolades <span class="nowrap">`<...>`</span>.

#### Extension: <span class="nowrap">`mmd_link_attributes`</span>

Analyse les attributs de valeur clé de style multimarkdown sur les
références de liens et d'images. Cette extension ne doit pas être
confondue avec l'extension [<span
class="nowrap">`link_attributes`</span>](#extension-link_attributes).

    This is a reference ![image][ref] with multimarkdown attributes.

    [ref]: https://path.to/image "Image title" width=20px height=30px
           id=myId class="myClass1 myClass2"

#### Extension: <span class="nowrap">`mmd_header_identifiers`</span>

Analyse les identificateurs des titres de style "multimarkdown" (entre
crochets, après le titre mais avant tout <span
class="nowrap">`#`</span>de suivi dans un titre ATX).

#### Extension: <span class="nowrap">`compact_definition_lists`</span>

Activates the definition list syntax of pandoc 1.12.x and earlier. This
syntax differs from the one described above under [Definition
lists](#definition-lists) in several respects:

-   No blank line is required between consecutive items of the
    definition list.
-   To get a “tight” or “compact” list, omit space between consecutive
    items; the space between a term and its definition does not affect
    anything.
-   Lazy wrapping of paragraphs is not allowed: the entire definition
    must be indented four
    spaces.<a href="#fn4" id="fnref4" class="footnote-ref"><sup>4</sup></a>

#### Extension: <span class="nowrap">`gutenberg`</span>

Use [Project Gutenberg](https://www.gutenberg.org/) conventions for
<span class="nowrap">`plain`</span> output: all-caps for strong
emphasis, surround by underscores for regular emphasis, add extra blank
space around headings.

Markdown variants
-----------------

In addition to pandoc’s extended Markdown, the following Markdown
variants are supported:

<span class="nowrap">`markdown_phpextra`</span> (PHP Markdown Extra)  
<span class="nowrap">`footnotes`</span>, <span
class="nowrap">`pipe_tables`</span>, <span
class="nowrap">`raw_html`</span>, <span
class="nowrap">`markdown_attribute`</span>, <span
class="nowrap">`fenced_code_blocks`</span>, <span
class="nowrap">`definition_lists`</span>, <span
class="nowrap">`intraword_underscores`</span>, <span
class="nowrap">`header_attributes`</span>, <span
class="nowrap">`link_attributes`</span>, <span
class="nowrap">`abbreviations`</span>, <span
class="nowrap">`shortcut_reference_links`</span>, <span
class="nowrap">`spaced_reference_links`</span>.

<span class="nowrap">`markdown_github`</span> (deprecated GitHub-Flavored Markdown)  
<span class="nowrap">`pipe_tables`</span>, <span
class="nowrap">`raw_html`</span>, <span
class="nowrap">`fenced_code_blocks`</span>, <span
class="nowrap">`auto_identifiers`</span>, <span
class="nowrap">`gfm_auto_identifiers`</span>, <span
class="nowrap">`backtick_code_blocks`</span>, <span
class="nowrap">`autolink_bare_uris`</span>, <span
class="nowrap">`space_in_atx_header`</span>, <span
class="nowrap">`intraword_underscores`</span>, <span
class="nowrap">`strikeout`</span>, <span
class="nowrap">`task_lists`</span>, <span class="nowrap">`emoji`</span>,
<span class="nowrap">`shortcut_reference_links`</span>, <span
class="nowrap">`angle_brackets_escapable`</span>, <span
class="nowrap">`lists_without_preceding_blankline`</span>.

<span class="nowrap">`markdown_mmd`</span> (MultiMarkdown)  
<span class="nowrap">`pipe_tables`</span>, <span
class="nowrap">`raw_html`</span>, <span
class="nowrap">`markdown_attribute`</span>, <span
class="nowrap">`mmd_link_attributes`</span>, <span
class="nowrap">`tex_math_double_backslash`</span>, <span
class="nowrap">`intraword_underscores`</span>, <span
class="nowrap">`mmd_title_block`</span>, <span
class="nowrap">`footnotes`</span>, <span
class="nowrap">`definition_lists`</span>, <span
class="nowrap">`all_symbols_escapable`</span>, <span
class="nowrap">`implicit_header_references`</span>, <span
class="nowrap">`auto_identifiers`</span>, <span
class="nowrap">`mmd_header_identifiers`</span>, <span
class="nowrap">`shortcut_reference_links`</span>, <span
class="nowrap">`implicit_figures`</span>, <span
class="nowrap">`superscript`</span>, <span
class="nowrap">`subscript`</span>, <span
class="nowrap">`backtick_code_blocks`</span>, <span
class="nowrap">`spaced_reference_links`</span>, <span
class="nowrap">`raw_attribute`</span>.

<span class="nowrap">`markdown_strict`</span> (Markdown.pl)  
<span class="nowrap">`raw_html`</span>, <span
class="nowrap">`shortcut_reference_links`</span>, <span
class="nowrap">`spaced_reference_links`</span>.

We also support <span class="nowrap">`commonmark`</span> and <span
class="nowrap">`gfm`</span> (GitHub-Flavored Markdown, which is
implemented as a set of extensions on <span
class="nowrap">`commonmark`</span>).

Note, however, that <span class="nowrap">`commonmark`</span> and <span
class="nowrap">`gfm`</span> have limited support for extensions. Only
those listed below (and <span class="nowrap">`smart`</span>, <span
class="nowrap">`raw_tex`</span>, and <span
class="nowrap">`hard_line_breaks`</span>) will work. The extensions can,
however, all be individually disabled. Also, <span
class="nowrap">`raw_tex`</span> only affects <span
class="nowrap">`gfm`</span> output, not input.

<span class="nowrap">`gfm`</span> (GitHub-Flavored Markdown)  
<span class="nowrap">`pipe_tables`</span>, <span
class="nowrap">`raw_html`</span>, <span
class="nowrap">`fenced_code_blocks`</span>, <span
class="nowrap">`auto_identifiers`</span>, <span
class="nowrap">`gfm_auto_identifiers`</span>, <span
class="nowrap">`backtick_code_blocks`</span>, <span
class="nowrap">`autolink_bare_uris`</span>, <span
class="nowrap">`space_in_atx_header`</span>, <span
class="nowrap">`intraword_underscores`</span>, <span
class="nowrap">`strikeout`</span>, <span
class="nowrap">`task_lists`</span>, <span class="nowrap">`emoji`</span>,
<span class="nowrap">`shortcut_reference_links`</span>, <span
class="nowrap">`angle_brackets_escapable`</span>, <span
class="nowrap">`lists_without_preceding_blankline`</span>.

Producing slide shows with pandoc
=================================

You can use pandoc to produce an HTML + JavaScript slide presentation
that can be viewed via a web browser. There are five ways to do this,
using [S5](https://meyerweb.com/eric/tools/s5/),
[DZSlides](http://paulrouget.com/dzslides/),
[Slidy](https://www.w3.org/Talks/Tools/Slidy2/),
[Slideous](https://goessner.net/articles/slideous/), or
[reveal.js](https://revealjs.com/). You can also produce a PDF slide
show using LaTeX [<span
class="nowrap">`beamer`</span>](https://ctan.org/pkg/beamer), or slides
shows in Microsoft
[PowerPoint](https://en.wikipedia.org/wiki/Microsoft_PowerPoint) format.

Here’s the Markdown source for a simple slide show, <span
class="nowrap">`habits.txt`</span>:

    % Habits
    % John Doe
    % March 22, 2005

    # In the morning

    ## Getting up

    - Turn off alarm
    - Get out of bed

    ## Breakfast

    - Eat eggs
    - Drink coffee

    # In the evening

    ## Dinner

    - Eat spaghetti
    - Drink wine

    ------------------

    ![picture of spaghetti](images/spaghetti.jpg)

    ## Going to sleep

    - Get in bed
    - Count sheep

To produce an HTML/JavaScript slide show, simply type

    pandoc -t FORMAT -s habits.txt -o habits.html

where <span class="nowrap">`FORMAT`</span> is either <span
class="nowrap">`s5`</span>, <span class="nowrap">`slidy`</span>, <span
class="nowrap">`slideous`</span>, <span
class="nowrap">`dzslides`</span>, or <span
class="nowrap">`revealjs`</span>.

For Slidy, Slideous, reveal.js, and S5, the file produced by pandoc with
the
<a href="#option--standalone" class="option"><span class="nowrap"><code>-s/--standalone</code></span></a>
option embeds a link to JavaScript and CSS files, which are assumed to
be available at the relative path <span
class="nowrap">`s5/default`</span> (for S5), <span
class="nowrap">`slideous`</span> (for Slideous), <span
class="nowrap">`reveal.js`</span> (for reveal.js), or at the Slidy
website at <span class="nowrap">`w3.org`</span> (for Slidy). (These
paths can be changed by setting the <span
class="nowrap">`slidy-url`</span>, <span
class="nowrap">`slideous-url`</span>, <span
class="nowrap">`revealjs-url`</span>, or <span
class="nowrap">`s5-url`</span> variables; see [Variables for HTML
slides](#variables-for-html-slides), above.) For DZSlides, the
(relatively short) JavaScript and CSS are included in the file by
default.

With all HTML slide formats, the
<a href="#option--self-contained" class="option"><span class="nowrap"><code>--self-contained</code></span></a>
option can be used to produce a single file that contains all of the
data necessary to display the slide show, including linked scripts,
stylesheets, images, and videos.

To produce a PDF slide show using beamer, type

    pandoc -t beamer habits.txt -o habits.pdf

Note that a reveal.js slide show can also be converted to a PDF by
printing it to a file from the browser.

To produce a Powerpoint slide show, type

    pandoc habits.txt -o habits.pptx

Structuring the slide show
--------------------------

Par défaut, le niveau de la diapositive est le niveau de titre le plus
élevé dans la hiérarchie qui est suivi immédiatement par le contenu, et
non par un autre titre, quelque part dans le document. Dans l'exemple
ci-dessus, les titres de niveau 1 sont toujours suivis par des titres de
niveau 2, qui sont suivis par le contenu, de sorte que le niveau de la
diapositive est de 2. Ce mode de fonctionnement peut être inhibé en
utilisant l'option
<a href="#option--slide-level" class="option"><span class="nowrap"><code>--slide-level</code></span></a>.

Le document est découpé en diapositives selon les règles suivantes :

-   Une règle horizontale lance toujours une nouvelle diapositive.

-   Un titre au niveau de la diapositive commence toujours une nouvelle
    diapositive.

-   Les titres *en dessous* du niveau de la diapositive dans la
    hiérarchie créent des titres *à l'intérieur* d'une diapositive.

-   Les titres *au-dessus* du niveau de la diapositive dans la
    hiérarchie créent des "diapositives de titre", qui contiennent
    simplement le titre de la section et aident à diviser le diaporama
    en sections. Le contenu qui ne figure pas sous ces rubriques sera
    inclus sur la diapositive titre (pour les diaporamas HTML) ou sur
    une diapositive suivante portant le même titre (pour les
    projecteurs).

-   Une page de titre est construite automatiquement à partir du bloc
    titre du document, s'il existe. (Dans le cas de beamer, cela peut
    être désactivé en commentant certaines lignes du modèle par défaut).

Ces règles sont conçues pour prendre en charge de nombreux styles
différents de diaporama. Si vous ne vous souciez pas de structurer vos
diapositives en sections et sous-sections, vous pouvez simplement
utiliser des titres de niveau 1 pour toutes les diapositives. (Dans ce
cas, le niveau 1 sera le niveau des diapositives.) Mais vous pouvez
également structurer le diaporama en sections, comme dans l'exemple
ci-dessus.

Note : dans les diaporamas de reveal.js, si le niveau de la diapositive
est 2, une mise en page bidimensionnelle sera produite, les titres de
niveau 1 se développant horizontalement et les titres de niveau 2 se
développant verticalement. Il n'est pas recommandé d'utiliser un
emboîtement plus profond des niveaux de section avec reveal.js.

Incremental lists
-----------------

Par défaut, ces auteurs produisent des listes qui affichent "tout en
même temps". Si vous souhaitez que vos listes s'affichent de manière
progressive (un élément à la fois), utilisez l'option
<a href="#option--incremental" class="option"><span class="nowrap"><code>-i</code></span></a>.
Si vous voulez qu'une liste particulière s'écarte de la valeur par
défaut, mettez-la dans un bloc <span class="nowrap">`div`</span> avec
classe <span class="nowrap">`incremental`</span> ou <span
class="nowrap">`nonincremental`</span>. Ainsi, par exemple, en utilisant
la syntaxe <span class="nowrap">`fenced                   div`</span>,
ce qui suit serait incrémentiel indépendamment de la valeur par défaut
du document :

    ::: incremental

    - Eat spaghetti
    - Drink wine

    :::

or

    ::: nonincremental

    - Eat spaghetti
    - Drink wine

    :::

Bien que l'utilisation de divs <span
class="nowrap">`incremental`</span>et  <span
class="nowrap">`nonincremental`</span> soit la méthode recommandée pour
établir des listes incrémentielles au cas par cas, une méthode plus
ancienne est également prise en charge : le fait de mettre des listes à
l'intérieur d'un bloc de guillemets s'écartera de la valeur par défaut
du document (c'est-à-dire qu'il s'affichera de manière incrémentielle
sans l'option
<a href="#option--incremental" class="option"><span class="nowrap"><code>-i</code></span></a>et
tout à la fois avec l'option
<a href="#option--incremental" class="option"><span class="nowrap"><code>-i</code></span></a>)
:

    > - Eat spaghetti
    > - Drink wine

Les deux méthodes permettent de mélanger des listes incrémentielles et
non incrémentielles dans un seul document.

Remarque : ni l'optionni aucune des méthodes décrites ici ne fonctionne
actuellement pour la sortie PowerPoint.
<a href="#option--incremental" class="option"><span class="nowrap"><code>-i/--incremental</code></span></a>
option nor any of the methods described here currently works for
PowerPoint output.

Inserting pauses
----------------

Vous pouvez ajouter des "pauses" à l'intérieur d'une diapositive en
incluant un paragraphe contenant trois points, séparés par des espaces :

    # Slide with a pause

    content before the pause

    . . .

    content after the pause

Note : cette fonctionnalité n'est pas encore implémentée pour les
sorties PowerPoint.

Styling the slides
------------------

Vous pouvez modifier le style des diapositives HTML en mettant des
fichiers CSS personnalisés dans <span
class="nowrap">`$DATADIR/s5/default`</span> (pour S5), <span
class="nowrap">`$DATADIR/slidy`</span> (pour Slidy), ou <span
class="nowrap">`$DATADIR/slideous`</span> (pour Slideous), où <span
class="nowrap">`$DATADIR`</span> est le répertoire des données de
l'utilisateur (voir
<a href="#option--data-dir" class="option"><span class="nowrap"><code>--data-dir</code></span></a>,
ci-dessus). Les originaux peuvent être trouvés dans le répertoire des
données du système de pandoc (généralement <span
class="nowrap">`$CABALDIR/pandoc-VERSION/s5/default`</span>Pandoc y
recherchera les fichiers qu'il ne trouve pas dans le répertoire des
données de l'utilisateur). .

Pour les dzslides, le CSS est inclus dans le fichier HTML lui-même, et
peut y être modifié.

Toutes les [reveal.js configuration
options](https://github.com/hakimel/reveal.js#configuration) peuvent
être fixées par des variables. Par exemple, les thèmes peuvent être
utilisés en définissant la variable <span class="nowrap">`theme`</span>:

    -V theme=moon

Ou vous pouvez spécifier une feuille de style personnalisée en utilisant
l'option
<a href="#option--css" class="option"><span class="nowrap"><code>--css</code></span></a>.

Pour styliser les diapositives de beamer, vous pouvez spécifier un <span
class="nowrap">`theme`</span>, <span class="nowrap">`colortheme`</span>,
<span class="nowrap">`fonttheme`</span>, <span
class="nowrap">`innertheme`</span>, et <span
class="nowrap">`outertheme`</span>, en utilisant l'option
<a href="#option--variable" class="option"><span class="nowrap"><code>-V</code></span></a>:

    pandoc -t beamer habits.txt -V theme:Warsaw -o habits.pdf

Notez que les attributs d'en-tête se transforment en attributs de
diapositive (sur un <span class="nowrap">`<div>`</span> ou <span
class="nowrap">`<section>`</span>) dans les formats de diapositive HTML,
ce qui vous permet de styliser des diapositives individuelles. Dans
beamer, le seul attribut de titre qui affecte les diapositives est la
classe <span class="nowrap">`allowframebreaks`</span>qui définit
l'option  <span class="nowrap">`allowframebreaks`</span>, entraînant la
création de plusieurs diapositives si le contenu dépasse le cadre. Ceci
est particulièrement recommandé pour les bibliographies :

    # References {.allowframebreaks}

Speaker notes
-------------

Les notes des intervenants sont prises en charge dans reveal.js et la
sortie PowerPoint (pptx). Vous pouvez ainsi ajouter des notes à votre
document Markdown :

    ::: notes

    This is my note.

    - It can contain Markdown
    - like this list

    :::

Pour afficher la fenêtre des notes dans reveal.js, appuyez sur <span
class="nowrap">`s`</span> pendant la présentation. Les notes des
orateurs en PowerPoint seront disponibles, comme d'habitude, sous forme
de documents à distribuer et de présentation.

Les notes ne sont pas encore prises en charge pour les autres formats de
diapositives, mais les notes n'apparaîtront pas sur les diapositives
elles-mêmes.

Columns
-------

Pour placer des documents dans des colonnes côte à côte, vous pouvez
utiliser un conteneur div natif avec la classe <span
class="nowrap">`columns`</span>, contenant deux ou plusieurs conteneurs
div avec la classe <span class="nowrap">`column`</span> et un attribut
<span class="nowrap">`width`</span> :

    :::::::::::::: {.columns}
    ::: {.column width="40%"}
    contents...
    :::
    ::: {.column width="60%"}
    contents...
    :::
    ::::::::::::::

Frame attributes in beamer
--------------------------

Il est parfois nécessaire d'ajouter l'option LaTeX <span
class="nowrap">`[fragile]`</span> à un cadre dans beamer (par exemple,
lors de l'utilisation de l'environnement <span
class="nowrap">`minted`</span> environment). On peut y parvenir en
ajoutant la classe <span class="nowrap">`fragile`</span> au titre
introduisant la diapositive :

    # Fragile slide {.fragile}

Tous les autres attributs de frame décrits dans la section 8.1 du guide
de l'utilisateur du Beamer peuvent également être utilisés : <span
class="nowrap">`allowdisplaybreaks`</span>, <span
class="nowrap">`allowframebreaks`</span>, <span
class="nowrap">`b`</span>, <span class="nowrap">`c`</span>, <span
class="nowrap">`t`</span>, <span class="nowrap">`environment`</span>,
<span class="nowrap">`label`</span>, <span
class="nowrap">`plain`</span>, <span class="nowrap">`shrink`</span>,
<span class="nowrap">`standout`</span>, <span
class="nowrap">`noframenumbering`</span>.

Background in reveal.js and beamer
----------------------------------

Background images can be added to self-contained reveal.js slideshows
and to beamer slideshows.

For the same image on every slide, use the configuration option <span
class="nowrap">`background-image`</span> either in the YAML metadata
block or as a command-line variable. (There are no other options in
beamer and the rest of this section concerns reveal.js slideshows.)

For reveal.js, you can instead use the reveal.js-native option <span
class="nowrap">`parallaxBackgroundImage`</span>. You can also set <span
class="nowrap">`parallaxBackgroundHorizontal`</span> and <span
class="nowrap">`parallaxBackgroundVertical`</span> the same way and must
also set <span class="nowrap">`parallaxBackgroundSize`</span> to have
your values take effect.

To set an image for a particular reveal.js slide, add <span
class="nowrap">`{data-background-image="/path/to/image"}`</span> to the
first slide-level heading on the slide (which may even be empty).

In reveal.js’s overview mode, the parallaxBackgroundImage will show up
only on the first slide.

Other reveal.js background settings also work on individual slides,
including <span class="nowrap">`data-background-size`</span>, <span
class="nowrap">`data-background-repeat`</span>, <span
class="nowrap">`data-background-color`</span>, <span
class="nowrap">`data-transition`</span>, and <span
class="nowrap">`data-transition-speed`</span>.

To add a background image to the automatically generated title slide,
use the <span class="nowrap">`title-slide-attributes`</span> variable in
the YAML metadata block. It must contain a map of attribute names and
values.

See the [reveal.js
documentation](https://github.com/hakimel/reveal.js#slide-backgrounds)
for more details.

For example in reveal.js:

    ---
    title: My Slideshow
    parallaxBackgroundImage: /path/to/my/background_image.png
    title-slide-attributes:
        data-background-image: /path/to/title_image.png
        data-background-size: contain
    ---

    ## Slide One

    Slide 1 has background_image.png as its background.

    ## {data-background-image="/path/to/special_image.jpg"}

    Slide 2 has a special image for its background, even though the heading has no content.

Creating EPUBs with pandoc
==========================

EPUB Metadata
-------------

EPUB metadata may be specified using the
<a href="#option--epub-metadata" class="option"><span class="nowrap"><code>--epub-metadata</code></span></a>
option, but if the source document is Markdown, it is better to use a
[YAML metadata block](#extension-yaml_metadata_block). Here is an
example:

    ---
    title:
    - type: main
      text: My Book
    - type: subtitle
      text: An investigation of metadata
    creator:
    - role: author
      text: John Smith
    - role: editor
      text: Sarah Jones
    identifier:
    - scheme: DOI
      text: doi:10.234234.234/33
    publisher:  My Press
    rights: © 2007 John Smith, CC BY-NC
    ibooks:
      version: 1.3.4
    ...

The following fields are recognized:

<span class="nowrap">`identifier`</span>  
Either a string value or an object with fields <span
class="nowrap">`text`</span> and <span class="nowrap">`scheme`</span>.
Valid values for <span class="nowrap">`scheme`</span> are <span
class="nowrap">`ISBN-10`</span>, <span class="nowrap">`GTIN-13`</span>,
<span class="nowrap">`UPC`</span>, <span
class="nowrap">`ISMN-10`</span>, <span class="nowrap">`DOI`</span>,
<span class="nowrap">`LCCN`</span>, <span
class="nowrap">`GTIN-14`</span>, <span class="nowrap">`ISBN-13`</span>,
<span class="nowrap">`Legal deposit number`</span>, <span
class="nowrap">`URN`</span>, <span class="nowrap">`OCLC`</span>, <span
class="nowrap">`ISMN-13`</span>, <span class="nowrap">`ISBN-A`</span>,
<span class="nowrap">`JP`</span>, <span class="nowrap">`OLCC`</span>.

<span class="nowrap">`title`</span>  
Either a string value, or an object with fields <span
class="nowrap">`file-as`</span> and <span class="nowrap">`type`</span>,
or a list of such objects. Valid values for <span
class="nowrap">`type`</span> are <span class="nowrap">`main`</span>,
<span class="nowrap">`subtitle`</span>, <span
class="nowrap">`short`</span>, <span class="nowrap">`collection`</span>,
<span class="nowrap">`edition`</span>, <span
class="nowrap">`extended`</span>.

<span class="nowrap">`creator`</span>  
Either a string value, or an object with fields <span
class="nowrap">`role`</span>, <span class="nowrap">`file-as`</span>, and
<span class="nowrap">`text`</span>, or a list of such objects. Valid
values for <span class="nowrap">`role`</span> are [MARC
relators](https://loc.gov/marc/relators/relaterm.html), but pandoc will
attempt to translate the human-readable versions (like “author” and
“editor”) to the appropriate marc relators.

<span class="nowrap">`contributor`</span>  
Same format as <span class="nowrap">`creator`</span>.

<span class="nowrap">`date`</span>  
A string value in <span class="nowrap">`YYYY-MM-DD`</span> format. (Only
the year is necessary.) Pandoc will attempt to convert other common date
formats.

<span class="nowrap">`lang`</span> (or legacy: <span class="nowrap">`language`</span>)  
A string value in [BCP 47](https://tools.ietf.org/html/bcp47) format.
Pandoc will default to the local language if nothing is specified.

<span class="nowrap">`subject`</span>  
A string value or a list of such values.

<span class="nowrap">`description`</span>  
A string value.

<span class="nowrap">`type`</span>  
A string value.

<span class="nowrap">`format`</span>  
A string value.

<span class="nowrap">`relation`</span>  
A string value.

<span class="nowrap">`coverage`</span>  
A string value.

<span class="nowrap">`rights`</span>  
A string value.

<span class="nowrap">`cover-image`</span>  
A string value (path to cover image).

<span class="nowrap">`css`</span> (or legacy: <span class="nowrap">`stylesheet`</span>)  
A string value (path to CSS stylesheet).

<span class="nowrap">`page-progression-direction`</span>  
Either <span class="nowrap">`ltr`</span> or <span
class="nowrap">`rtl`</span>. Specifies the <span
class="nowrap">`page-progression-direction`</span> attribute for the
[<span class="nowrap">`spine`</span>
element](http://idpf.org/epub/301/spec/epub-publications.html#sec-spine-elem).

<span class="nowrap">`ibooks`</span>  
iBooks-specific metadata, with the following fields:

-   <span class="nowrap">`version`</span>: (string)
-   <span class="nowrap">`specified-fonts`</span>: <span
    class="nowrap">`true`</span>\|<span class="nowrap">`false`</span>
    (default <span class="nowrap">`false`</span>)
-   <span class="nowrap">`ipad-orientation-lock`</span>: <span
    class="nowrap">`portrait-only`</span>\|<span
    class="nowrap">`landscape-only`</span>
-   <span class="nowrap">`iphone-orientation-lock`</span>: <span
    class="nowrap">`portrait-only`</span>\|<span
    class="nowrap">`landscape-only`</span>
-   <span class="nowrap">`binding`</span>: <span
    class="nowrap">`true`</span>\|<span class="nowrap">`false`</span>
    (default <span class="nowrap">`true`</span>)
-   <span class="nowrap">`scroll-axis`</span>: <span
    class="nowrap">`vertical`</span>\|<span
    class="nowrap">`horizontal`</span>\|<span
    class="nowrap">`default`</span>

The <span class="nowrap">`epub:type`</span> attribute
-----------------------------------------------------

For <span class="nowrap">`epub3`</span> output, you can mark up the
heading that corresponds to an EPUB chapter using the [<span
class="nowrap">`epub:type`</span>
attribute](http://www.idpf.org/epub/31/spec/epub-contentdocs.html#sec-epub-type-attribute).
For example, to set the attribute to the value <span
class="nowrap">`prologue`</span>, use this markdown:

    # My chapter {epub:type=prologue}

Which will result in:

    <body epub:type="frontmatter">
      <section epub:type="prologue">
        <h1>My chapter</h1>

Pandoc will output <span
class="nowrap">`<body                   epub:type="bodymatter">`</span>,
unless you use one of the following values, in which case either <span
class="nowrap">`frontmatter`</span> or <span
class="nowrap">`backmatter`</span> will be output.

| <span class="nowrap">`epub:type`</span> of first section | <span class="nowrap">`epub:type`</span> of body |
|----------------------------------------------------------|-------------------------------------------------|
| prologue                                                 | frontmatter                                     |
| abstract                                                 | frontmatter                                     |
| acknowledgments                                          | frontmatter                                     |
| copyright-page                                           | frontmatter                                     |
| dedication                                               | frontmatter                                     |
| credits                                                  | frontmatter                                     |
| keywords                                                 | frontmatter                                     |
| imprint                                                  | frontmatter                                     |
| contributors                                             | frontmatter                                     |
| other-credits                                            | frontmatter                                     |
| errata                                                   | frontmatter                                     |
| revision-history                                         | frontmatter                                     |
| titlepage                                                | frontmatter                                     |
| halftitlepage                                            | frontmatter                                     |
| seriespage                                               | frontmatter                                     |
| foreword                                                 | frontmatter                                     |
| preface                                                  | frontmatter                                     |
| seriespage                                               | frontmatter                                     |
| titlepage                                                | frontmatter                                     |
| appendix                                                 | backmatter                                      |
| colophon                                                 | backmatter                                      |
| bibliography                                             | backmatter                                      |
| index                                                    | backmatter                                      |

Linked media
------------

Par défaut, pandoc téléchargera les médias référencés à partir de tout
élément <span class="nowrap">`<img>`</span>, <span
class="nowrap">`<audio>`</span>, <span class="nowrap">`<video>`</span>
ou <span class="nowrap">`<source>`</span>présent dans l'EPUB généré, et
les inclura dans le conteneur EPUB, ce qui donnera un EPUB complètement
autonome. Si vous souhaitez plutôt créer des liens vers des ressources
médiatiques externes, utilisez du HTML brut dans votre source et
ajoutez  <span class="nowrap">`data-external="1"`</span> à la balise
avec l'attribut <span class="nowrap">`src`</span>Par exemple:

    <audio controls="1">
      <source src="https://example.com/music/toccata.mp3"
              data-external="1" type="audio/mpeg">
      </source>
    </audio>

Creating Jupyter notebooks with pandoc
======================================

When creating a [Jupyter
notebook](https://nbformat.readthedocs.io/en/latest/), pandoc will try
to infer the notebook structure. Code blocks with the class <span
class="nowrap">`code`</span> will be taken as code cells, and
intervening content will be taken as Markdown cells. Attachments will
automatically be created for images in Markdown cells. Metadata will be
taken from the <span class="nowrap">`jupyter`</span> metadata field. For
example:

    ---
    title: My notebook
    jupyter:
      nbformat: 4
      nbformat_minor: 5
      kernelspec:
         display_name: Python 2
         language: python
         name: python2
      language_info:
         codemirror_mode:
           name: ipython
           version: 2
         file_extension: ".py"
         mimetype: "text/x-python"
         name: "python"
         nbconvert_exporter: "python"
         pygments_lexer: "ipython2"
         version: "2.7.15"
    ---

    # Lorem ipsum

    **Lorem ipsum** dolor sit amet, consectetur adipiscing elit. Nunc luctus
    bibendum felis dictum sodales.

    ``` code
    print("hello")
    ```

    ## Pyout

    ``` code
    from IPython.display import HTML
    HTML("""
    <script>
    console.log("hello");
    </script>
    <b>HTML</b>
    """)
    ```

    ## Image

    This image ![image](myimage.png) will be
    included as a cell attachment.

If you want to add cell attributes, group cells differently, or add
output to code cells, then you need to include divs to indicate the
structure. You can use either [fenced divs](#extension-fenced_divs) or
[native divs](#extension-native_divs) for this. Here is an example:

    :::::: {.cell .markdown}
    # Lorem

    **Lorem ipsum** dolor sit amet, consectetur adipiscing elit. Nunc luctus
    bibendum felis dictum sodales.
    ::::::

    :::::: {.cell .code execution_count=1}
    ``` {.python}
    print("hello")
    ```

    ::: {.output .stream .stdout}
    ```
    hello
    ```
    :::
    ::::::

    :::::: {.cell .code execution_count=2}
    ``` {.python}
    from IPython.display import HTML
    HTML("""
    <script>
    console.log("hello");
    </script>
    <b>HTML</b>
    """)
    ```

    ::: {.output .execute_result execution_count=2}
    ```{=html}
    <script>
    console.log("hello");
    </script>
    <b>HTML</b>
    hello
    ```
    :::
    ::::::

If you include raw HTML or TeX in an output cell, use the \[raw
attribute\]\[Extension: <span
class="nowrap">`fenced_attribute`</span>\], as shown in the last cell of
the example above. Although pandoc can process “bare” raw HTML and TeX,
the result is often interspersed raw elements and normal textual
elements, and in an output cell pandoc expects a single, connected raw
block. To avoid using raw HTML or TeX except when marked explicitly
using raw attributes, we recommend specifying the extensions <span
class="nowrap">`-raw_html-raw_tex+raw_attribute`</span> when translating
between Markdown and ipynb notebooks.

Note that options and extensions that affect reading and writing of
Markdown will also affect Markdown cells in ipynb notebooks. For
example,
<a href="#option--wrap" class="option"><span class="nowrap"><code>--wrap=preserve</code></span></a>
will preserve soft line breaks in Markdown cells;
<a href="#option--atx-headers" class="option"><span class="nowrap"><code>--atx-headers</code></span></a>
will cause ATX-style headings to be used; and
<a href="#option--preserve-tabs" class="option"><span class="nowrap"><code>--preserve-tabs</code></span></a>
will prevent tabs from being turned to spaces.

Syntax highlighting
===================

Pandoc will automatically highlight syntax in [fenced code
blocks](#fenced-code-blocks) that are marked with a language name. The
Haskell library [skylighting](https://github.com/jgm/skylighting) is
used for highlighting. Currently highlighting is supported only for
HTML, EPUB, Docx, Ms, and LaTeX/PDF output. To see a list of language
names that pandoc will recognize, type <span
class="nowrap">`pandoc                   --list-highlight-languages`</span>.

The color scheme can be selected using the
<a href="#option--highlight-style" class="option"><span class="nowrap"><code>--highlight-style</code></span></a>
option. The default color scheme is <span
class="nowrap">`pygments`</span>, which imitates the default color
scheme used by the Python library pygments (though pygments is not
actually used to do the highlighting). To see a list of highlight
styles, type <span
class="nowrap">`pandoc                   --list-highlight-styles`</span>.

If you are not satisfied with the predefined styles, you can use
<a href="#option--print-highlight-style" class="option"><span class="nowrap"><code>--print-highlight-style</code></span></a>
to generate a JSON <span class="nowrap">`.theme`</span> file which can
be modified and used as the argument to
<a href="#option--highlight-style" class="option"><span class="nowrap"><code>--highlight-style</code></span></a>.
To get a JSON version of the <span class="nowrap">`pygments`</span>
style, for example:

    pandoc --print-highlight-style pygments > my.theme

Then edit <span class="nowrap">`my.theme`</span> and use it like this:

    pandoc --highlight-style my.theme

If you are not satisfied with the built-in highlighting, or you want
highlight a language that isn’t supported, you can use the
<a href="#option--syntax-definition" class="option"><span class="nowrap"><code>--syntax-definition</code></span></a>
option to load a [KDE-style XML syntax definition
file](https://docs.kde.org/stable5/en/applications/katepart/highlight.html).
Before writing your own, have a look at KDE’s [repository of syntax
definitions](https://github.com/KDE/syntax-highlighting/tree/master/data/syntax).

To disable highlighting, use the
<a href="#option--no-highlight" class="option"><span class="nowrap"><code>--no-highlight</code></span></a>
option.

Custom Styles
=============

Custom styles can be used in the docx and ICML formats.

Output
------

By default, pandoc’s docx and ICML output applies a predefined set of
styles for blocks such as paragraphs and block quotes, and uses largely
default formatting (italics, bold) for inlines. This will work for most
purposes, especially alongside a <span
class="nowrap">`reference.docx`</span> file. However, if you need to
apply your own styles to blocks, or match a preexisting set of styles,
pandoc allows you to define custom styles for blocks and text using
<span class="nowrap">`div`</span>s and <span
class="nowrap">`span`</span>s, respectively.

If you define a <span class="nowrap">`div`</span> or <span
class="nowrap">`span`</span> with the attribute <span
class="nowrap">`custom-style`</span>, pandoc will apply your specified
style to the contained elements (with the exception of elements whose
function depends on a style, like headings, code blocks, block quotes,
or links). So, for example, using the <span
class="nowrap">`bracketed_spans`</span> syntax,

    [Get out]{custom-style="Emphatically"}, he said.

would produce a docx file with “Get out” styled with character style
<span class="nowrap">`Emphatically`</span>. Similarly, using the <span
class="nowrap">`fenced_divs`</span> syntax,

    Dickinson starts the poem simply:

    ::: {custom-style="Poetry"}
    | A Bird came down the Walk---
    | He did not know I saw---
    :::

would style the two contained lines with the <span
class="nowrap">`Poetry`</span> paragraph style.

For docx output, styles will be defined in the output file as inheriting
from normal text, if the styles are not yet in your reference.docx. If
they are already defined, pandoc will not alter the definition.

This feature allows for greatest customization in conjunction with
[pandoc filters](https://pandoc.org/filters.html). If you want all
paragraphs after block quotes to be indented, you can write a filter to
apply the styles necessary. If you want all italics to be transformed to
the <span class="nowrap">`Emphasis`</span> character style (perhaps to
change their color), you can write a filter which will transform all
italicized inlines to inlines within an <span
class="nowrap">`Emphasis`</span> custom-style <span
class="nowrap">`span`</span>.

For docx output, you don’t need to enable any extensions for custom
styles to work.

Input
-----

The docx reader, by default, only reads those styles that it can convert
into pandoc elements, either by direct conversion or interpreting the
derivation of the input document’s styles.

By enabling the [<span class="nowrap">`styles`</span>
extension](#ext-styles) in the docx reader
(<a href="#option--from" class="option"><span class="nowrap"><code>-f docx+styles</code></span></a>),
you can produce output that maintains the styles of the input document,
using the <span class="nowrap">`custom-style`</span> class. Paragraph
styles are interpreted as divs, while character styles are interpreted
as spans.

For example, using the <span
class="nowrap">`custom-style-reference.docx`</span> file in the test
directory, we have the following different outputs:

Without the <span class="nowrap">`+styles`</span> extension:

    $ pandoc test/docx/custom-style-reference.docx -f docx -t markdown
    This is some text.

    This is text with an *emphasized* text style. And this is text with a
    **strengthened** text style.

    > Here is a styled paragraph that inherits from Block Text.

And with the extension:

    $ pandoc test/docx/custom-style-reference.docx -f docx+styles -t markdown

    ::: {custom-style="First Paragraph"}
    This is some text.
    :::

    ::: {custom-style="Body Text"}
    This is text with an [emphasized]{custom-style="Emphatic"} text style.
    And this is text with a [strengthened]{custom-style="Strengthened"}
    text style.
    :::

    ::: {custom-style="My Block Style"}
    > Here is a styled paragraph that inherits from Block Text.
    :::

With these custom styles, you can use your input document as a
reference-doc while creating docx output (see below), and maintain the
same styles in your input and output files.

Custom writers
==============

Pandoc peut être étendu avec des auteurs personnalisés écrits en
[Lua](https://www.lua.org/). (Pandoc comprend un interprète Lua, il
n'est donc pas nécessaire d'installer Lua séparément)

Pour utiliser un auteur personnalisé, il suffit de spécifier le chemin
d'accès au script Lua à la place du format de sortie. Par exemple :

    pandoc -t data/sample.lua

Pour créer un auteur personnalisé, il faut écrire une fonction Lua pour
chaque élément possible d'un document pandoc. Pour obtenir un exemple
documenté que vous pouvez modifier en fonction de vos besoins, faites

    pandoc --print-default-data-file sample.lua

Notez que les auteurs d'articles personnalisés n'ont pas de modèle par
défaut. Si vous souhaitez utiliser
<a href="#option--standalone" class="option"><span class="nowrap"><code>--standalone</code></span></a>
avec un auteur personnalisé, vous devrez spécifier un modèle
manuellement en utilisant
<a href="#option--template" class="option"><span class="nowrap"><code>--template</code></span></a>
ou ajouter un nouveau modèle par défaut avec le nom <span
class="nowrap">`default.NAME_OF_CUSTOM_WRITER.lua`</span> au
sous-répertoire <span class="nowrap">`templates`</span> de votre
répertoire de données utilisateur (voir [Templates](#templates)).

A note on security
==================

If you use pandoc to convert user-contributed content in a web
application, here are some things to keep in mind:

1.  Although pandoc itself will not create or modify any files other
    than those you explicitly ask it create (with the exception of
    temporary files used in producing PDFs), a filter or custom writer
    could in principle do anything on your file system. Please audit
    filters and custom writers very carefully before using them.

2.  If your application uses pandoc as a Haskell library (rather than
    shelling out to the executable), it is possible to use it in a mode
    that fully isolates pandoc from your file system, by running the
    pandoc operations in the <span class="nowrap">`PandocPure`</span>
    monad. See the document [Using the pandoc
    API](https://pandoc.org/using-the-pandoc-api.html) for more details.

3.  Pandoc’s parsers can exhibit pathological performance on some corner
    cases. It is wise to put any pandoc operations under a timeout, to
    avoid DOS attacks that exploit these issues. If you are using the
    pandoc executable, you can add the command line options <span
    class="nowrap">`+RTS -M512M -RTS`</span> (for example) to limit the
    heap size to 512MB.

4.  The HTML generated by pandoc is not guaranteed to be safe. If <span
    class="nowrap">`raw_html`</span> is enabled for the Markdown input,
    users can inject arbitrary HTML. Even if <span
    class="nowrap">`raw_html`</span> is disabled, users can include
    dangerous content in attributes for headings, spans, and code
    blocks. To be safe, you should run all the generated HTML through an
    HTML sanitizer.

Authors
=======

Copyright 2006–2020 John MacFarlane (jgm@berkeley.edu). Released under
the
[GPL](https://www.gnu.org/copyleft/gpl.html "GNU General Public License"),
version 2 or greater. This software carries no warranty of any kind.
(See COPYRIGHT for full copyright and warranty notices.) For a full list
of contributors, see the file AUTHORS.md in the pandoc source code.

<div class="section footnotes" role="doc-endnotes">

------------------------------------------------------------------------

1.  <div id="fn1">

    The point of this rule is to ensure that normal paragraphs starting
    with people’s initials, like

        B. Russell was an English philosopher.

    do not get treated as list items.

    This rule will not prevent

        (C) 2007 Joe Smith

    from being interpreted as a list item. In this case, a backslash
    escape can be used:

        (C\) 2007 Joe Smith

    <a href="#fnref1" class="footnote-back">↩︎</a>

    </div>

2.  <div id="fn2">

    I have been influenced by the suggestions of [David
    Wheeler](https://justatheory.com/2009/02/modest-markdown-proposal/).<a href="#fnref2" class="footnote-back">↩︎</a>

    </div>

3.  <div id="fn3">

    This scheme is due to Michel Fortin, who proposed it on the
    [Markdown discussion
    list](http://six.pairlist.net/pipermail/markdown-discuss/2005-March/001097.html).<a href="#fnref3" class="footnote-back">↩︎</a>

    </div>

4.  <div id="fn4">

    To see why laziness is incompatible with relaxing the requirement of
    a blank line between items, consider the following example:

        bar
        :    definition
        foo
        :    definition

    Is this a single list item with two definitions of “bar,” the first
    of which is lazily wrapped, or two list items? To remove the
    ambiguity we must either disallow lazy wrapping or require a blank
    line between list
    items.<a href="#fnref4" class="footnote-back">↩︎</a>

    </div>

</div>

</div>

</div>

</div>

</div>
