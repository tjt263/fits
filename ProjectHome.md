# The new FITS website is at http://fitstool.org. Please update your bookmarks. #


## File Information Tool Set (FITS) ##

---

### What is it? ###
The File Information Tool Set (FITS) identifies, validates, and extracts technical metadata for various file formats.  It wraps several third-party open source tools, normalizes and consolidates their output, and reports any errors. FITS was created by the Harvard University Library [Office for Information Systems](http://hul.harvard.edu/ois/) for use in its [Digital Repository Service (DRS)](http://hul.harvard.edu/ois/systems/drs/).

Source code is now stored in Github: https://github.com/harvard-lts/fits

The current tools used are:
  * [Jhove](http://hul.harvard.edu/jhove/) ([LGPL version 2.1 or any later version](http://www.gnu.org/licenses/licenses.html#LGPL))
  * [Exiftool](http://www.sno.phy.queensu.ca/~phil/exiftool/) ([GPL version 1 or any later version](http://www.gnu.org/copyleft/gpl.html); or [the artistic license](http://www.opensource.org/licenses/artistic-license-1.0.php))
  * [National Library of New Zealand Metadata Extractor](http://meta-extractor.sourceforge.net/) ([Apache Public License version 2](http://www.opensource.org/licenses/apache2.0.php))
  * [DROID](http://droid.sourceforge.net/) ([BSD (new version)](http://www.opensource.org/licenses/bsd-license.php))
  * [FFIdent](http://web.archive.org/web/20061106114156/http://schmidt.devlib.org/ffident/index.html) ([LGPL](http://www.gnu.org/licenses/licenses.html#LGPL))
    * Note that the live site for ffident which was `http://schmidt.devlib.org/ffident/index.html` seems to have disappeared - we are now linking to Internet Archive's version of the ffident website.
  * [File Utility](http://unixhelp.ed.ac.uk/CGI/man-cgi?file) ([windows](http://gnuwin32.sourceforge.net/)) ([revised BSD](http://www.opensource.org/licenses/bsd-license.php))

**The source code for each of the above tools is available on their websites.** The source code for FITS is included in the downloadable ZIP file as well as the source tab above.

FITS also supplies two original tools: FileInfo and XmlMetadata.

In addition, FITS includes the following open source libraries:
  * [JDOM](http://www.jdom.org/) ([Apache-like license, modified Apache version 1.1](http://code.google.com/p/fits/wiki/jdom_license))
  * [staxmate](http://staxmate.codehaus.org/) ([BSD (new version)](http://www.opensource.org/licenses/bsd-license.php))
  * [stax2](http://docs.codehaus.org/display/WSTX/StAX2) ([LGPL version 2.1](http://www.gnu.org/licenses/licenses.html#LGPL))
  * [Woodstox](http://docs.codehaus.org/display/WSTX/Home) ([LGPL version 2.1](http://www.gnu.org/licenses/licenses.html#LGPL))
  * [xercesImpl](http://xerces.apache.org/xerces2-j/) ([Apache Public License version 2](http://www.opensource.org/licenses/apache2.0.php))
  * [xml-apis](http://xml.apache.org/commons/) ([Apache Public License version 2](http://www.opensource.org/licenses/apache2.0.php))
  * [xmlunit](http://xmlunit.sourceforge.net/) ([BSD (new version)](http://www.opensource.org/licenses/bsd-license.php))

**The source code for each of the above libraries is available on their websites.**

To get started see the [FITS User Guide](http://code.google.com/p/fits/wiki/user_guide).

### Changes: ###

**Note that starting with release 0.3.0 Java 1.6 is required.

---**

  * [Version 0.6.2](http://fits.googlecode.com/files/fits-0.6.2.zip) (3/18/13)

-Initial merge and commit from openfits

-Updated Exiftool to 9.06

-Improved video support with Exiftool

-Fixed ordering of File utility command line options

-New configuration option to limit the max number of threads

-Fixed bug in how the include-ext and exclude-ext lists were being processed

-Added a new option to enable output of statistics

-Initial experimental support for Apache Tika - disabled by default.


---


  * [Version 0.6.1](http://fits.googlecode.com/files/fits-0.6.1.zip) (4/25/12)

- Fixed [issue 27](https://code.google.com/p/fits/issues/detail?id=27): Fits with -r overwrites output files with same filename but different path http://code.google.com/p/fits/issues/detail?id=27

- Added new output option -xc.  This will output converted standard output format (MIX, TextMD, DocumentMD, AES57) within the FITS XML `<fits>/<metadata>/<xxxx>/<standard>` element, where `<xxxx>` is either `<image>`, `<text>`, `<document>`, `<audio>`, or `<video>`.

- Added eml to the Jhove exclude-ext list

- Improved FitsOutput API


---


  * Version 0.6.0 (10/25/11)

> - Performance enhancement to make all tools run in parallel in individual threads

> - Added the ADL tool for the identification of Audio Decision List files

> - Added options to process directories of files.  When -i is set to a directory -o must also be set to a directory. The new -r option will process  directories recursively when -i is a directory.

> - Added `<filepath>` element to FileInfo tool, updated fits\_output.xsd

> - Fixed bug in normalizing Exiftool format output for TIFFs. "TIFF EXIF" is not output as "Tagged Image File Format"

> - Fix to use `<fileinfo>/<created>` value for `<mix:GeneralCaptureInformation>/<mix:dateTimeCreated>`  when using -x switch


---


  * Version 0.5.0 (02/24/11)

> - Improved support for audio formats

> - Ability to convert audio metadata to the AES audioObject schema using the -x option

> - Now ising NLNZ VERSION element instead of VERSION-NAME for MP3 files

> - Fixed incorrect JP2 version reported by Exiftool

> - Better identification of JP2 and JPX images (OIS [bug 2876](https://code.google.com/p/fits/issues/detail?id=876))

> - Updated File Utility XSLT to improve format output (OIS [bug 2897](https://code.google.com/p/fits/issues/detail?id=897))

> - Fixed DROID format output for SVG files (OIS [bug 2968](https://code.google.com/p/fits/issues/detail?id=968))

> - Added ability to extract DTD references to the XmlMetadata tool from XML DOCTYPE declaration (OIS [bug 2967](https://code.google.com/p/fits/issues/detail?id=967))

> - Changed file utility call to use --mime instead of -i.  File 5.03 on OS X uses -I rather than -i to print the mime type and mime encoding.  --mime works on Linux and OS X.

> - Tweaked Jhove XSLT conversion of image height and width elements for files containing multiple images. When multiple height or width values are found only the first one is used.

> - Improved identification of EXIF and JFIF JPEGs.


---


  * Version 0.4.2 (07/06/10)
> - Fixed an issue with the `<identification>` element status attribute. If multiple tools agree with the identified format and there are no other identity conflicts then the attribute is omitted.  The SINGLE\_RESULT status attribute is used only if a single tool successfully identifies the file. Thanks to Swithun Crowe for the patch.

> - Added -e cdf parameter to the Windows File Utility call to prevent crashes when processing files that produce a large amount of output.


---


  * Version 0.4.1 (06/30/10)
> - Improved handling of File Utility output for text files

> - Added 'Postscript' as a more specific format of 'Plain text' to fits\_format\_tree.xml

> - Added 'JPEG File Interchange Format' as a more specific format of 'Raw JPEG Stream' to fits\_format\_tree.xml


---


  * Version 0.4 (6/28/10)
> - Fixed Jhove colorspace output, mapping "Greyscale" to "Grayscale"

> - Updated DROID signature file to V35, made configurable in xml/fits.xml

> - Updated OTS-Schemas.jar fixing several issues with MIX output conversion

> - Fixed Exiftool primary chromaticities output

> - Fixed Exiftool and Jhove orientation output.

> - Updated File Utility to support version 5.03

> - Changed format tree `<branch format="text">` to `<branch format="Plain text">`

> - Added Hypertext Markup Language as a child branch of Plain text in the format tree

> - Standard XML output (-x) is now pretty printed


---


  * Version 0.3.2 (02/18/10)]
> - Fixed identification of PDF/A and PDF/X formats

> - Added Ant build files to release ([issue #8](https://code.google.com/p/fits/issues/detail?id=#8))

> - Added patch from beerchr1 to tweak video output and support flash video ([issue #9](https://code.google.com/p/fits/issues/detail?id=#9))


---

  * Version 0.3.1 (02/01/10)
> - Fixed error when trying to convert FITS output to a standard metadata schema for
> a file that does not have sufficient output that can be converted ([issue #6](https://code.google.com/p/fits/issues/detail?id=#6))

> - Fixed bugs in FitsOutput

> - Added patch from beerchr1 to add basic video support ([issue #7](https://code.google.com/p/fits/issues/detail?id=#7))



---

  * Version 0.3.0 (01/14/10)
> - Added API support for converting output to MIX, TextMD and DocumentMD

> - Added command line support (-x) for outputting MIX, TextMD or DocumentMD

> - Updated NLNZ, Jhove, and Exiftool output values to be compatible with MIX

> - Added API feature ([issue #4](https://code.google.com/p/fits/issues/detail?id=#4)) to disable individual tools

> - Updated Jhove to version 1.5

> - Using byteoffset=true option for Jhove TIFF module

> - Fixed [issue #3](https://code.google.com/p/fits/issues/detail?id=#3): using relative path instead of $FITS\_HOME/path in xml/nlnz/config.xml & in XSLT when for xml/fits\_output.xsd

> - Fixed [issue #5](https://code.google.com/p/fits/issues/detail?id=#5): FITS 0.2.6 output file showing FITS version as 0.2.5


---

  * Version 0.2.6
> - Fixed bug that was causing external identification information to be dropped from the output.
  * Version 0.2.5
> - initial release