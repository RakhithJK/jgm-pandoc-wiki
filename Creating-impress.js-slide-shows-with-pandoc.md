First, clone a copy of the [impress.js](http://impress.js.org/) repository:

    git clone https://github.com/bartaz/impress.js.git

Next, get the custom template:

    wget https://gist.github.com/jgm/5665065/raw/impress-template.html

**Or** use halloleo's custom template which supports standard meta variables like $title$, $css$, etc:

    wget https://gist.githubusercontent.com/halloleo/3177e019f7544f08af467feb8511d90c/raw/5c74a92e1f918cef240ee6ccff3fc9b27ebbd08a/impress-template.html

Create a file `impress.txt` with this content:

```markdown
# {#overview .step data-scale=10}

# Bored? {.step}

Aren't you **bored** with slides?

# {.step data-x=2000}

Their limits are annoying?

# {.step data-rotate=90}

Then you should try **impress.js**

```

Now create your slide show:

    pandoc --template impress-template.html -V impress-url=impress.js -s -t html5 --section-divs -o impress.html impress.txt