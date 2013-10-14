First, clone a copy of the impress.js repository:

    git clone https://github.com/bartaz/impress.js.git

Next, get the custom template:

    https://gist.github.com/jgm/5665065

Create a file `impress.txt` with these contents:

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

    pandoc --template 5665065/impress-template.html -V impress-url=impress.js -s -t html5 --section-divs -o impress.html impress.txt