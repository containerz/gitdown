
<center>
<img src="https://github.com/imatix/gitdown/raw/master/images/README_1.png" alt="1">
</center>

<A name="toc1-17" title="Welcome To Gitdown" />
Welcome To Gitdown
==================

Gitdown is a simple tool for writing documentation hosted on a github repository.  It uses [ditaa][] to convert ascii diagrams into images, and produces [markdown][] documents that can be uploaded to your repository along with code.  I made Gitdown so that we could write technical white papers and user guides as plain text (including diagrams) and publish them with a single "git push" command.  Gitdown is a simpler version of the tool we use to maintain the [ØMQ][zeromq] [Guide][zguide].

Gitdown is written and maintained by Pieter Hintjens.  Please use the issue [tracker][] for all comments and errata.  This document was published on Wednesday March, 2016 at  1:10:39, and generated by the magic of Gitdown from README.txt.

This is version 2011.03.24 of Gitdown. Changelog:

* 2013.02.05: Gyepi Sam changed pull command to make chunk specification optional.
* 2010.03.24: added .pull command to include chunks from other files.
* 2010.10.11: don't do symbol substitution in code blocks.
* 2010.10.09: added .toc token to generate table of contents.

<A name="toc2-32" title="Contents" />
Contents
--------

*  <a href="#toc2-38">License</a>
*  <a href="#toc2-47">Installation and Use</a>
*  <a href="#toc2-81">Installation in Cygwin</a>
*  <a href="#toc2-105">How it Works</a>
*  <a href="#toc2-160">Gitdown Syntax Summary</a>
*  <a href="#toc2-226">Markdown Syntax Summary</a>

<A name="toc2-38" title="License" />
License
-------

Copyright (c) 2010-2011 Pieter Hintjens
Copyright (c) 1996-2011 iMatix Corporation

This is free software; you can redistribute it and/or modify it under the terms of the GNU General Public License as published by the Free Software Foundation; either version 3 of the License, or (at your option) any later version.

<A name="toc2-47" title="Installation and Use" />
Installation and Use
--------------------

The code is in the bin subdirectory.  Here is how I install it on a new box:

    sudo cp bin/* /usr/local/bin
    export PERLLIB=$PATH    #   goes into .bash_profile

Gitdown includes Ditaa/0.9 and assumes that Ditaa goes into /usr/local/bin/ditaa0_9.jar.

Another option is to leave the files in the git directory and install a wrapper script. 
Assuming the directory ~/bin exists and is in your path, you can do the following:

    install-wrapper ~/bin

to install the file ~/bin/gitdown, which will set things up to run correctly. 

Gitdown assumes that these tools are already installed on your box, which is easy if you're running Linux:

* *ImageMagick*, specifically `mogrify`.
* *Perl*.
* *Java*, a Java run-time environment is needed to run ditaa.

To use Gitdown, edit a text document much like this README.txt.  Then:

    gitdown myfile.txt
    git add myfile.md images/
    git commit -m "Hey, Gitdown is totally crazy!"
    git push origin master

The images directory holds images for all documents in the current directory.  You can write documents anywhere on the git tree but if they are not at the root you must tell Gitdown how to create a full image path by setting the SUBDIR symbol (see below).


<A name="toc2-81" title="Installation in Cygwin" />
Installation in Cygwin
----------------------

Install apt-cyg - cygwin package manager:

      lynx -source rawgit.com/transcode-open/apt-cyg/master/apt-cyg > apt-cyg
      install apt-cyg /bin

Install ImageMagick:

      apt-cyg install ImageMagick
      
Download and install Java JDK from [here](http://www.oracle.com/technetwork/java/javase/downloads/index.html), then restart your PC for the env vars to reflect

Copy the binaries to /usr/local/bin:

      cp bin/* to /usr/local/bin

Add this to your ~/.bash_profile:

      export PERLLIB=$PATH
    

<A name="toc2-105" title="How it Works" />
How it Works
------------

Gitdown exploits github.com's willingness to serve image blobs and Markdown documents that refer to them.  This means you can publish a document with correctly-formatted links, together with images, in a single Git operation.  It is a neat and comfortable way to work.  Kudos to Stathis Sideris for making Ditaa.  Gitdown also adds a preprocessing layer for symbolic insertion and simple macros like 'table of contents'.  This code was recycled from [htmlpp](http://legacy.imatix.com/html/htmlpp/), a HTML preprocessor that looked a lot like Markdown.

The Gitdown workflow is:

<center>
<img src="https://github.com/imatix/gitdown/raw/master/images/README_2.png" alt="2">
</center>

1. You edit a text file that contains text and diagrams in a single document.
2. You process this document with Gitdown to give a Markdown document plus a number of images in an images subdirectory.
3. You add the output (Markdown and images) to your commit set and push that to the repository.
4. The Markdown document is now readable, with images, via the github.com user interface.

This README acts as an example.

<A name="toc2-160" title="Gitdown Syntax Summary" />
Gitdown Syntax Summary
---------------------

Gitdown is a pre-processor that adds these syntax elements on top of Markdown:

    [diagram]                   Defines a ditaa diagram block
    [/diagram]                  # represents diagram number 1..n

    .- comment                  Comment line
    .set name=value             Sets Gitdown symbol
    .sub oldval=newval          Replaces oldval by newval in every line
    .toc [top]                  Insert table of contents
    .pull srcfile[@tag][,opts]  Pull a chunk of text, or the whole file, from srcfile
    .end                        Everything past this is ignored

    $\(xxx)                     Value of variable, anywhere in text
    $\(xxx?zzz)                 Value of variable, or zzz if undefined
    %\(text?zzz)                Value of environment variable, or zzz if undef
    &abc\(text)                 Intrinsic function with arguments

    time()                      Format current time as hh:mm:ss
    date()                      Return current date value
    date("picture")             Format current date using picture
    date("picture", date, lc)   Format specified date using picture & language
    week_day([date])            Get day of week, 0=Sunday to 6=Saturday
    year_week([date])           Get week of year, 1 is first full week
    julian_date([date])         Get Julian date for date
    lillian_date([date])        Get Lillian date for date
    date_to_days(date)          Convert yyyymmdd to Lillian date
    days_to_date(days)          Convert Lillian date to yyyymmdd
    future_date(days[,date])    Calculate a future date
    past_date(days[,date])      Calculate a past date
    date_diff(date1[,date2])    Calculate date1 - date2
    image_height("image.ext")   Get image height (GIF, JPEG)
    image_width("image.ext")    Get image width (GIF, JPEG)
    file_size("filename",arg)   Get size of file: optional arg K or M
    file_date("filename")       Get date of file
    file_time("filename")       Get time of file as hh:mm:ss
    normalise("filename")       Normalise filename to UNIX format
    system("command")           Call a system utility
    lower("string")             Convert string to lower case
    upper("string")             Convert string to upper case

The top argument for .toc tells it the top header level in the text. Lower levels are shown horizontally.  E.g. this file has level 2 headers in the text and uses `.toc 1` to get these laid-out on a single row.

If the .pull command includes an optional @tag, the named chunk of text is pulled from the source file.
A chunk of text is identified by '@tag' anywhere in the line before the chunk, and any other tag signalling the end. '@end' can be used to close any chunk. Tag names must be alphanumeric.
If @tag is omitted, the entire file is included.

The opts argument for .pull can be: 'code' to indicate the results should be indented 4 spaces.
An opts of 'left' removes any left margin.


These symbols have special meaning:

* GIT defines the root HTTP URL of the git repository.
* BRANCH defines the respository branch, defaults to 'master'.
* SUBDIR defines the sub/dir/ to the current file, if not empty, should end in / if not empty.

These symbols are predefined by gitdown for you:

* INPUT specifies the original input file name.
* SELF specifies the input file name without extension.
* OUTPUT specifies the current output file name,

<A name="toc2-226" title="Markdown Syntax Summary" />
Markdown Syntax Summary
-----------------------

This is an extract of the most useful Markdown syntax.

    # Header 1              or follow by ===== on next line
    ## Header 2             or follow by ----- on next line
    ### Header 3
    #### Header 4

    > Blockquote            can continue over multiple lines
    * List item             can continue over multiple lines
    1. List item            can continue over multiple lines

        code block          indented 4 spaces

    <url>                   automatic link, e.g. <ph@imatix.com>
    ![alt](url)             image link
    [text](url)             inline link
    [text][]                implicit reference link
      [text]: url           defined later in document

    *emphasis*              usually, italics
    **strong**              usually, bold
    `Code`                  fixed-space font

* You can put HTML anywhere in your text.
* Blank lines separate paragraphs.
* For more details see the [markdown][] syntax page.

[zeromq]:       http://www.zeromq.com
[zguide]:       http://zguide.zeromq.org
[tracker]:      https://github.com/imatix/gitdown/issues
[markdown]:     http://daringfireball.net/projects/markdown/syntax
[ditaa]:        http://ditaa.org

_This documentation was generated using [Gitdown](https://github.com/zeromq/gitdown)_
