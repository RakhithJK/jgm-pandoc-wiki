-   [From Markdown, To Markdown](#from-markdown-to-markdown)
    -   [Cleanup](#cleanup)
    -   [TOC generation](#toc-generation)
    -   [Math in Pure Markdown](#math-in-pure-markdown)
    -   [Convert Between the 4 Table Syntaxes in Pandoc](#convert-between-the-4-table-syntaxes-in-pandoc)
    -   [Repeated Footnotes Anchors and Headers Across Multiple Files](#repeated-footnotes-anchors-and-headers-across-multiple-files)
-   [Template Snippet](#template-snippet)

Here’s some tricks that is allowed by pandoc but not obvious at first sight.

# From Markdown, To Markdown

using `pandoc -f markdown... -t markdown...` can have surprisingly useful applications:

## Cleanup

As shown in issue [\#2814](https://github.com/jgm/pandoc/issues/2814), it can be used to clean up / normalize your markdown file.

## TOC generation

*e.g.* you have a long markdown file in GitHub and want to have a TOC, you can use `pandoc -t markdown_github --toc -o example-with-toc.md example.md`

## Math in Pure Markdown

The manual said:

> Note: the `--webtex` option will affect Markdown output as well as HTML.

This can be used to put math in pure markdown. *e.g.* you want to put math directly in the README.md in GitHub.

For example, in the `temp.md`:

``` {.markdown}
# Important Discovery!

$1+2\neq3!$

Try it!
```

Run this:

``` {.bash}
pandoc --atx-headers --webtex=https://latex.codecogs.com/png.latex? -s -o temp-codecogs.md temp.md
```

Then the output becomes:

> # Important Discovery!
>
> ![1+2\\neq3!](https://latex.codecogs.com/png.latex?1%2B2%5Cneq3%21 "1+2\neq3!")
>
> Try it!

## Convert Between the 4 Table Syntaxes in Pandoc

Say, in your source markdown file `pipe.md`:

``` {.markdown}
| testing     | pandoc            | tables  |
|-------------|-------------------|---------|
| simple cell | no multiline cell | and     |
| so          | on                | no list |
```

In command line,

``` {.bash}
pandoc -t markdown-simple_tables-multiline_tables-pipe_tables -s -o grid.md pipe.md
```

In the output `grid.md`:

``` {.markdown}
+--------------------------+--------------------------+--------------------------+
| testing                  | pandoc                   | tables                   |
+==========================+==========================+==========================+
| simple cell              | no multiline cell        | and                      |
+--------------------------+--------------------------+--------------------------+
| so                       | on                       | no list                  |
+--------------------------+--------------------------+--------------------------+
```

## Repeated Footnotes Anchors and Headers Across Multiple Files

If you use auto-identifiers for the headers, and there are different headers of the same name across different folders, you’d want to catenate them together, and pandoc do this for you:

``` {.bash}
pandoc file1.md file2.md ...
```

But if there are repeated footnotes anchors on both files, you need `--file-scope`, which will parsed each files individually (so the footnotes anchors are “local” to the individual file):

``` {.bash}
pandoc file1.md file2.md --file-scope ...
```

What if the 2 files has both problems? i.e. headers of same name (hence the same id by the auto-identifier) and footnotes of the same anchors appears across the files. Either approach gives you problem.

In this case, you can use “to markdown from markdown” to write an intermediate markdown file using `--file-scope`, which handles the colliding footnote anchor for you, and then generate from that intermediate markdown file, the auto-identifiers will handle the headers for you:

``` {.bash}
pandoc --file-scope -o intermediate.md file1.md file2.md
pandoc intermediate.md ...
```

# Template Snippet

If you wrote a template snippet that do not form a complete template. The `-H`, `-B`, or `-A` option would not help because pandoc would put your snippet as is and wouldn’t process it as a template. *i.e.* The snippet is included after the template is processed.

A trick mentioned by [@cagix](<https://github.com/cagix>) in [jgm/pandoc-templates\#220](https://github.com/jgm/pandoc-templates/issues/220) is this:

``` {.bash}
pandoc --template=template_snippet document.md -o processed_snippet
pandoc -H processed_snippet document.md -o document.pdf
```

The first line will process your template snippet according to the properties of the document, but since your snippet (probably) do not have `$body$`, the body would not be in the output. Now the snippet is processed and can then be included through `-H` as is in the 2nd line.
