# pdf2xml-viewer - A simple viewer and inspection tool for text boxes in PDF documents

July 2016 / Feb. 2017, Markus Konrad <markus.konrad@wzb.eu> / [Berlin Social Science Center](https://www.wzb.eu/en)

## Introduction

This is a small tool with which it is possible to view and examine individual text boxes in PDF documents. This is
very helpful for analyzing the distribution of texts across a page, especially in the case of
[OCR-processed PDFs](https://en.wikipedia.org/wiki/Optical_character_recognition) (so called "sandwich PDFs") from
which you might want to extract structured information (see 
[pdftabextract](https://github.com/WZBSocialScienceCenter/pdftabextract) for this). With this viewer, you can examine
such PDFs and have a look at the properties of individual text boxes, like position, width, height or font
specification. In combination with pdftabextract, you can view the grids that were generated for the detected columns and rows. [This blog post](https://datascience.blog.wzb.eu/2017/02/16/data-mining-ocr-pdfs-using-pdftabextract-to-liberate-tabular-data-from-scanned-documents/) shows an example usage.

The viewer requires you to convert your PDFs to the *pdf2xml* format. Afterwards
you can start up a local webserver, display this XML file in the viewer (as seen below) and examine the individual
text boxes with your browsers developer console.

![OCR PDF example in the viewer](https://datascience.blog.wzb.eu/wp-content/uploads/10/2017/02/pdf2xml-viewer-page.png)

The created file in pdf2xml format can later also be used to extract structured information, which I explain in my
series of blog posts about [data mining PDFs](https://datascience.blog.wzb.eu/category/pdfs/).

## How to use it

### 1. Convert a PDF to pdf2xml format

At first, you need to convert your PDFs using the **poppler-utils**, a package which is part of most Linux distributions
and is also available for OSX via Homebrew or MacPorts. From this package we need the command `pdftohtml` and can create
an XML file in pdf2xml format in the following way using the Terminal:

```
pdftohtml -c -hidden -xml input.pdf output.xml
```

The arguments *input.pdf* and *output.xml* are your input PDF file and the created XML file in pdf2xml format
respectively. It is important that you specifiy the *-hidden* parameter when you're dealing with OCR-processed
("sandwich") PDFs. You can furthermore add the parameters *-f n* and *-l n* to set only a range of pages to be
converted.

### 2. Start a minimal local webserver to display the text boxes in the PDF with the viewer

Now that you have your file(s) in pdf2xml format, change to the directory where pdf2xml-viewer resides (where it's
*index.html* file is). You should also copy the generated XML files to this location. Now let's start up a minimal
local webserver. This can be done very easily with Python, which is installed on Linux and Mac OSX by default.
You can do so in the Terminal with Python 2.x:

```
python -m SimpleHTTPServer 8080
```

Or with Python 3:
```
python3 -m http.server 8080 --bind 127.0.0.1
```

Now you open your browser and go to the adress http://127.0.0.1:8080. The viewer shows up and you can now enter the
file name of your file to load (it must be relative to the directory in which pdf2xml-viewer resides). If you just
want to see an example, type in *example/ocr-output.pdf.xml* and load this file. Now you browse through the pages of
your PDF document and you'll see the text boxes with red frames. You can further examine these boxes by using your
browser's inspection tools (right click on element and select "Inspect" in Chrome or Firefox) as seen below:

![OCR example in pdf2xml-viewer with browser inspection tools](https://datascience.blog.wzb.eu/wp-content/uploads/10/2016/07/ocr-example-output-devconsole.png)

### 3. Use the advanced features of the viewer

You can load a page grid JSON file that was generated with [pdftabextract](https://github.com/WZBSocialScienceCenter/pdftabextract) (function `common.save_page_grids`):

![Generated page grid viewed in pdf2xml-viewer](https://datascience.blog.wzb.eu/wp-content/uploads/10/2017/02/pdf2xml-viewer-pagegrid.png)

### 4. Extract data from your PDFs

If you want to extract structured data from the PDFs, you should have a look at the
[pdftabextract](https://github.com/WZBSocialScienceCenter/pdftabextract) package.

## Technical details

This viewer uses [d3.js](https://d3js.org) to display the pdf2xml file. I chose this approach because it is the fastest
and simples in order to inspect individual elements of a (OCR-processed) PDF document, without using expensive special
software. Furthermore it allows to add additional features such as displaying overlays of calculated lines or grids.

## License

Apache License 2.0. See LICENSE file.
