## Modifying styles in LibreOffice (and Word)

Since pandoc 1.18 you can apply custom paragraph and character styles in DOCX output (thanks @jkr!) by assigning a [custom-style attribute] to divs or spans in Markdown source. 

In order for your custom styles to appear different from the default style you will have to modify them in the appropriate dialog boxes.  Below are the best hints about this which I could find on Google, and a step-by-step description which I wrote myself. 

In case you wonder LibreOffice handles DOCX documents very well, and Pandoc works well with a [reference docx file](https://pandoc.org/MANUAL.html#option--reference-doc) which has been modified in LibreOffice.  This is good because as of Pandoc 1.19.2.1 DOCX support is superior to ODT support in Pandoc, notably including the *custom-style* attribute feature which doesn't work for ODT. 

### Word

[Here is a general tutorial for older versions of Word](http://shaunakelly.com/word/styles/modifyastyle.html), & official documentation for the more recent Word versions for [Windows](https://support.office.com/en-us/article/Customize-or-create-new-styles-in-Word-d38d6e47-f6fc-48eb-a607-1eb120dec563) and [macOS](https://support.office.com/en-us/article/Customize-styles-in-Word-for-Mac-1ef7d8e1-1506-4b21-9e81-adc5f698f86a). The underlying interface is very similar across different versions of Word.

### LibreOffice

These are the most relevant pages in the LibreOffice online help which I could find.  A more hands-on description can be found below them. 

<https://help.libreoffice.org/Writer/Styles_and_Formatting>

<https://help.libreoffice.org/Writer/Styles_in_Writer>

To change a paragraph style follow these steps:

1.  Choose *View--&gt;Styles and Formatting* or hit the *F11* key. 

2.  In the dialog or sidebar which opens make sure the button in the top panel marked with **¶** (for paragraph styles) or **A** (for character styles) is highlighted. 

3.  In the menu at the bottom of the dialog/sidebar choose *Applied Styles*. 

4.  In the list which appears look up the style you want to modify. 

    If you haven't used a *custom-style* attribute with the desired style name in the Markdown document from which your DOCX document was generated you will need to create the style:

    -   Look up the style which you want to base your style on in the list of styles.  Unless you got anything more special in mind *Default Style* or *Text Body* are good choices for paragraph styles[1] and *Default Style*, *Default Paragraph Font* or *Body Text Char* are good choices for character styles.  Otherwise you may want to switch to *Hierarchical* in the menu at the bottom of the dialog/sidebar and look up the style you want to base your style on. 

    -   Right-click on the name of the style you want to base your style on and choose *New...* in the popup menu.  A dialog with several tabs for different style choices appears. 

    -   In the dialog which opens go to the Organizer tab and enter the name of the new style in the *Name* text box.  It should be exactly the same as the value of the *custom-style* attribute on your divs or spans. 

    -   Jump to point 6. below. :-)

5.  Right-click on the name of the style you want to modify and choose *Modify...* in the popup menu.  A dialog with several tabs for different categories of style choices appears. 

6.  Modify the style to match your requirements.  Note that some settings are in unexpected places.  For example language is set in the *Font* tab while writing direction is set in the *Alignment* tab. 


[1] These are also where you want to go to e.g. change the font globally in your document or *reference-docx*.

  [custom-style attribute]: https://pandoc.org/MANUAL.html#output "Pandoc Manual"
  [pandoc manual page]: http://pandoc.org/MANUAL.html
