You can use pandoc to produce beautiful slides using [reveal.js](https://revealjs.com/).

# Setup

Download the latest supported [`reveal.js`](https://github.com/hakimel/reveal.js) for your version of Pandoc and place it in a folder called `reveal.js`:

Pandoc 2.9.2.2 and later can use reveal.js 4.x:

    wget https://github.com/hakimel/reveal.js/archive/master.tar.gz
    tar -xzvf master.tar.gz
    mv reveal.js-master reveal.js

Pandoc 2.9.2.1 and earlier must use reveal.js 3.x:

    wget https://github.com/hakimel/reveal.js/archive/3.9.2.tar.gz
    tar -xzvf 3.9.2.tar.gz
    mv reveal.js-3.9.2 reveal.js

To provide local reveal assets to the slide deck, add a flag like `-V revealjs-url=./reveal.js` to your pandoc call.

You can skip the above step by referencing the reveal.js URL when calling pandoc later. To do that, add the `-V revealjs-url=https://unpkg.com/reveal.js@3.9.2/` option in your `pandoc` call. **Note:** in this case, you will require Internet access to show your slides, and that they will not be standalone. Use the latest 4.x version if you're using a newer version of Pandoc.

Now, create a file called `myslides.md` with your content. It may optionally include a YAML front matter for title, author, and date:

    ---
    author: John Doe
    title: Demo Slide
    date: June 21, 2017
    ---
    # Foo
    ```python
    print("hello world")
    ```
    # Bar
    * test
    * test

# Creating the Slides

Use this command to produce your slideshow:

    pandoc -t revealjs -s -o myslides.html myslides.md -V revealjs-url=https://unpkg.com/reveal.js@3.9.2/

# Themes

You can use `-V theme=$theme` to set your theme as `$theme`, with the following options:

- `beige`
- `black`
- `blood`
- `league`
- `moon`
- `night`
- `serif`
- `simple`
- `sky`
- `solarized`
- `white`

Here are [some example presentations using reveal.js](https://github.com/hakimel/reveal.js/wiki/Example-Presentations).

# Transitions
You can use `-V transition=$transition` to set your theme as `$transition`, with the following options:

- `cube`
