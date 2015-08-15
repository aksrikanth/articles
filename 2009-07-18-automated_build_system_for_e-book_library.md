<!-- Sat 18 Jul 2009 -->

A number of sources including [Project Gutenberg](http://www.gutenberg.org/) and [Google](http://books.google.com/) have started to digitize books that have gone out of copyright.
Unfortunately, these sources either provide unformatted plain text or device specific, closed and often DRM encumbered formats.
Everytime I moved to a different device or computer, I had to download all my e-books in the appropriate format.
Worse, any cleanups and formatting I had done to my earlier copies was lost and I had to go through the same process again.
Tired of this, I decided to create an [automated build process](http://github.com/akincisor/library-build-system/) to generate e-books in different formats.

The first decision I made was that since my main source of books was Project Gutenberg, I should store all the originals in plain ASCII text.
The formatting of this plain text should be such that it could be read directly without need of conversion if necessary.
From this source the system should build all other formats that I might need like PDF, plucker, MobiBook etc.

The clear consequence of this choice is that the e-books could not contain images and complex formatting like tables.
For most e-books that I have, this is not a big problem.
The issue then becomes how to convert these text files to multiple formats that may be necessary for different devices.

The first tool, that I have been using from time to time in the past, was the excellent perl module [txt2html](http://txt2html.sourceforge.net/).
This tool converts simple text files into (X)HTML and is quite sufficient for the kinds of formatting that are required.
I also wrote a small ruby script to generate a table of contents with internal links and insert it into the HTML stream.
I might write in more detail about this later.

The next step of the process is to convert this HTML into useful e-book formats like PDF, plucker, MobiBook and ePub.
For PDF generation I used a combination of html2ps and pd2pdf.
I was pleased to see that this retains the internal links in the document as well.
I used the [plucker-build](http://www.plkr.org/) command that comes packaged with plucker to generate the plucker PDBs.
I then found the most excellent [calibre](http://calibre.kovidgoyal.net/) that converts almost any other format to MobiBook or ePub.

I used [rake](http://rake.rubyforge.org/) to describe the dependencies and to automate the build process.
I found the syntanx and clarity of the rake system much better than that of ordinary make.

All in all this project has been a lot of fun, and I can think of many more tweaks and improvements to the process.
I also learnt a bunch about e-book formats and the power of rake.
