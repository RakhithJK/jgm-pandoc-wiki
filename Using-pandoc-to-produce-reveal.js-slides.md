You can use pandoc to produce beautiful slides using [reveal.js](http://lab.hakim.se/reveal-js/).

# Setup

Download the latest [`reveal.js`](https://github.com/hakimel/reveal.js/archive/master.zip) and place it in a folder called `reveal.js`:

    wget https://github.com/hakimel/reveal.js/archive/master.tar.gz
    tar xzvf master.tar.gz
    mv reveal.js-master reveal.js

You can skip the above step by referencing the reveal.js URL when calling pandoc later. To do that, add the `-V revealjs-url=http://lab.hakim.se/reveal-js` option in your `pandoc` call. Note that in this case, you will require Internet access to show your slides, and that they will not be standalone.

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

    pandoc -t revealjs -s -o myslides.html myslides.md

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