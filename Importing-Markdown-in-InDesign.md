Pandoc can convert markdown (or any other of pandoc's input formats) to Adobe InCopy ICML, which is a standalone XML format and a subset of the zipped IDML format ([IDML Documentation as PDF](http://wwwimages.adobe.com/www.adobe.com/content/dam/acom/en/devnet/indesign/sdk/cs6/idml/idml-cookbook.pdf)). InCopy is the companion word-processor to Adobe InDesign.

### Workflow

The workflow is as follows:

1. Export to ICML (and be sure to use the `-s` or `--standalone` flag to get a valid document with the required header), for example:

        pandoc input.md -s -o output.icml

2. In InDesign, choose `File > Place` and select the generated ICML file to integrate it into your layout as a text frame.
3. Style the imported text by changing the generated Character- and Paragraph Styles.

You and your collaborators can keep working on the input file (e.g. in markdown) and periodically regenerate the ICML file by rerunning the command in (1). In InDesign, you can then update the document in the _Links_ panel to have the text changed reflected.

Note that you cannot initially change the imported text in InDesign because it is _locked_. To _check out_ the file, you need to select the text frame and choose `Edit > InCopy > Check Out`. (See [Work with managed files](https://helpx.adobe.com/indesign/using/managed-files.html) for what Adobe has to say about this workflow.) You could now edit text and adjust spacing manually. However, by doing this you loose the ability to seamlessly integrate new changes as outlined in the previous paragaph.

If you expect to receive further text adjusted at a later point in time, it is therefore recommended that you do _not_ check out your documents and instead use [H&J styles](https://indesignsecrets.com/hj-styles-in-indesign.php) and/or [GREP styles](http://www.typophile.com/node/69252) to programatically set up spacing and kerning as desired.

### Custom styles

Since pandoc 2.6, you can use markdown to embed custom paragraph styles and character styles in your document. See [custom styles in the pandoc MANUAL](http://pandoc.org/MANUAL.html#custom-styles).