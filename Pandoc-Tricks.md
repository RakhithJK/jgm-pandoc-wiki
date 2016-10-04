Here's some tricks that is allowed by pandoc but not obvious at first sight.

# Math in Pure Markdown

The manual said:

>Note: the `--webtex` option will affect Markdown output as well as HTML.

This can be used to put math in pure markdown. *e.g.* you want to put math directly in the README.md in GitHub.

For example, in the `temp.md`:

```markdown
# Important Discovery!

$1+2\neq3!$

Try it!
```

Run this:

```bash
pandoc --atx-headers --webtex=https://latex.codecogs.com/png.latex? -s -o temp-codecogs.md temp.md
```

Then the output becomes:

> # Important Discovery!
> 
> ![1+2\\neq3!](https://latex.codecogs.com/png.latex?1%2B2%5Cneq3%21 "1+2\neq3!")
> 
> Try it!

# Template Snippet

If you wrote a template snippet that do not form a complete template. The `-H`, `-B`, or `-A` option would not help because pandoc would put your snippet as is and wouldn't process it as a template. *i.e.* The snippet is included after the template is processed.

A trick mentioned by @cagix in jgm/pandoc-templates#220 is this:

```bash
pandoc --template=template_snippet document.md -o processed_snippet
pandoc -H processed_snippet document.md -o document.pdf
```

The first line will process your template snippet according to the properties of the document, but since your snippet (probably) do not have `$body$`, the body would not be in the output. Now the snippet is processed and can then be included through `-H` as is in the 2nd line.
