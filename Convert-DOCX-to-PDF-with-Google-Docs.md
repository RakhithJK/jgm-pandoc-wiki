This may be old news to some, but I can't remember having seen it, so I make a post for the record.

I just discovered that you can convert a PDF to Markdown (or any other format Pandoc supports) by uploading it to Google Drive, opening it in Google Docs and downloading it from there as DOCX, then converting the DOCX to Markdown with Pandoc. The result is quite good!

The steps:

1.  Log into <https://drive.google.com> in a web browser.

2.  Select the menu \[My Drive\] -> \[Upload files\] in the top bar.

    More recently there is a "button" \[+ New\] in the top left corner. Click on it and select \[File upload\] in the menu which appears.

3.  At least on my system a file dialog opens. Browse to the PDF file; select it; click \[Open\].

4.  (If this doesn't work try step 5.)

    1.  The file appears in the "Quick access" field just below the top bar. You may need to refresh a couple of times.
    2.  Right-click the file thumbnail; choose \[Open with\] -> \[Google Docs\].

5.  If step 4 doesn't work (the PDF file doesn't appear in the quick access field):

    1.  Start typing the PDF file name in the \[Search Drive\] box at the top.
    2.  Click on the file in the menu which appears.
    3.  The file opens in the Drive PDF viewer.
    4.  At the top there is a menu \[Open with Google Docs\]. Click on it and select Google Docs.

    Or look up the file in the file list and follow 4.ii. (Hard when there are lots of files in the list!)

6.  You should now find yourself in the Google Docs document view.

7.  In the \[File\] menu choose \[Download as\] -> \[Microsoft Word (.docx)\].

8.  Save the DOCX file to disk and convert it with Pandoc the same as you would any DOCX
    file, or edit it with Word/LibreOffice if you are of that persuasion.

Basic formatting --- paragraphs, bold, italics --- works very well. Some more advanced formatting is more or less broken:

-   Tables become ordinary text, not very well lined up.
-   Nested lists are flattened.
-   Small caps text disappears entirely! If you have access to the original LaTeX file I suggest putting this in your preamble:

    ``` latex
    \renewcommand\textsc[1]{\textbf{\textit{#1}}}
    ```

    or if bold italics actually occur in your document this:

    ``` latex
    \usepackage{textcase}
    \renewcommand\textsc[1]{\textbf{\textit{\MakeTextUppercase{#1}}}}
    ```

    Ugly as hell but sequences of uppercase bold italics are unlikely to actually occur in a document and are relatively easy to find and replace with something better in a "word processor" or in a text editor after conversion from DOCX to some sensible format with Pandoc.

    If you post-edit in a "WP" you may try `(x)color` and something like `\renewcommand\textsc[1]{\textcolor{red}{#1}}` instead. That may be hard to find *with* the "WP" but is relatively easy to find *in* the "WP" for a human eye.

You may want to correct these things in the "word processor" but my definite preference is to convert the DOCX file to Pandoc's extended Markdown with Pandoc, fix things up and then convert (back) to DOCX. You can then also apply your own custom named styles for things like color.

It still says "For best results, do not make changes to this file other than modifying the styles used by pandoc" but that is just what you want to do if you are using custom styles, including adding your own! BTW you may want to avoid non-ASCII and non-alphanumeric characters in your custom style names so that you don't need to quote your `custom-style` attribute values!

Speaking of small caps it has its official Pandoc syntax: `[small caps text]{.smallcaps}`, but that is far too verbose by Markdown standards! ;-) I usually overload Pandoc's generally useless strikeout syntax so that I can type `~~small caps text~~` with this Pandoc Lua filter:

```lua
function Strikeout (elem)
    return pandoc.SmallCaps(elem.content)
end
```

I hope this is of use to someone!
