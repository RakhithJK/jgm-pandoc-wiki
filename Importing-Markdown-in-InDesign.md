Pandoc can convert markdown (or any other of pandoc's input formats) to Adobe InCopy ICML, which is a standalone XML format and a subset of the zipped IDML format ([IDML Documentation as PDF](https://web.archive.org/web/20171018062624/https://www.adobe.com/content/dam/acom/en/devnet/indesign/sdk/cs6/idml/idml-cookbook.pdf)). InCopy is the companion word-processor to Adobe InDesign.

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

Since pandoc 2.6, you can use markdown to embed custom paragraph styles and character styles in your document.

If you define a `div` or `span` with the attribute `custom-style`,
pandoc will apply your specified style to the contained elements. So,
for example using the `bracketed_spans` syntax,

    [Get out]{custom-style="Emphatically"}, he said.

would produce a docx file with "Get out" styled with character
style `Emphatically`. Similarly, using the `fenced_divs` syntax,

    Dickinson starts the poem simply:

    ::: {custom-style="Poetry"}
    A Bird came down the Walk.
    He did not know I saw.
    :::

would style the two contained lines with the `Poetry` paragraph style.

For more info, see [custom styles in the pandoc MANUAL](http://pandoc.org/MANUAL.html#custom-styles).

### List styling

The ICML writer always outputs the first item in a list as a different style than the rest.

![InDesign Paragraph Styles panel showing the "NumList" and "NumList > first" paragraph styles.](https://i.imgur.com/sX8KZFk.png)

You can then edit the paragraph style and have a different "Space Before" for the first item, resulting in a nicely formatted list.

However, if you are using InDesign 2019 or newer, the new "Space Between Paragraphs Using Same Style" option in Paragraph Styles can make the handling of lists much simpler. You can delete the "> first" style, replace it with the standard style when it asks you to, and handle the space before, after and in between the list much more cleanly from a single style.

![The InDesign Paragraph Style Options panel, opened to the "Indents and Spacing" section, with the spacing options highlighted.](https://i.imgur.com/M4KB6wt.png)

