FITS is currently configured to use a set of 8 tools for identifying, validating, and extracting technical metadata.


# Jhove #

Capabilities: Identifies, extracts technical metadata, and validates files.

Details: Jhove is written in Java. The FITS tool wrapper uses the provided API.  The Jhove XML output is converted to FITS XML using XSLT. xml/jhove/jhove\_xslt\_map.xml is used to determine which XSLT to apply for the given identified format.

Formats Supported: jpg,tiff,jp2,gif,wave,aiff,xml,html,ascii,utf-8,pdf.

Notes:
  * For JP2 files the JHOVE output element Transformation indicates whether the compression is lossy or lossless. The transformation values are described in Table A-20 of the JPEG2000 part 1 specification. A value of 0 maps to the 9-7 irreversible (lossy) filter. A value of 1 maps to 5-3 reversible (lossless) filter. This JHOVE element is used by FITS when it outputs the compressionScheme in the image metadata, writing it as JPEG 2000 Lossy or JPEG 2000 Lossless.
  * JHOVE does not validate the codestream but it checks the file structure.

# Exiftool #

Capabilities: Identifies and extracts technical metadata.

Details: Exiftool is written in Perl.  A windows executable is also provided.  The Exiftool tool wrapper detects the operating system type and calls the appropriate version of the tool.  The tab-delimited output is captured, converted to a simple XML structure, and then converted to FITS XML using xslt.  xml/exiftool/exiftool\_xslt\_map.xml is used to determine which XSLT to apply for the given identified format.

Formats Supported: jpg,tiff,jp2,gif,bmp,png,psd,dng,wav,mp3,mp4,m4a,aiff,rm,ogg,flac,xml,html,pdf,doc

# National Library of New Zealand Metadata Extractor (NLNZ) #

Capabilities: Identifies and extracts technical metadata.

Details: The FITS NLNZ tool wrapper uses the provided Java API. The NLNZ native XML output is converted to FITS XML using XSLT. xml/nlnz/fits/nlnz\_xslt\_map.xml is used to determine which XSLT to apply to the given identified format.

Formats Supported: jpg,tiff,gif,bmp,wav,mp3,xml,html,pdf,doc,wordperfect,msworks,odt

# File Utility #

Capabilities: Identifies files.

Details: File Utility is usually bundled with Linux, UNIX and OS X.  The GnuWin32 port is provided for use on Windows. Due to variations in versions this may cause different output when run on different platforms.  File Utility is called in its default mode (no arguments), and also with -i to determine the MIME type. The output is converted into a simple XML document and then converted to FITS XML using xml/fileutility/fileutility\_to\_fits.xslt.

Formats Supported: many

# DROID #

Capabilities: Identifies files.

Details: Droid is written in Java. The FITS tool wrapper uses the provided API. The output is converted into a simple XML document and then converted to FITS XML using xml/droid/droid\_to\_fits.xslt. The Droid configuration file and signature file are located in the tools/droid directory.

Formats Supported: Listed in Droid signature file

# FFIdent #

Capabilities: Identifies files.

Details: FFIdent is written in Java.  The FITS tool wrapper uses the provided API.  Output is converted into a simple XML document and then converted to FITS XML using xml/ffident/ffident\_to\_fits.xslt.

Formats Supported: Listed in the configuration file tools/ffident/formats.txt

# FileInfo #

Capabilities: Extracts technical metadata.

Details: FileInfo creates FITS XML natively.  It determines basic file information like file name, size, file system last modified date, and md5 checksums. It uses the fast md5 jar from http://www.twmacinta.com/myjava/fast_md5.php.

Formats Supported: Any

# XmlMetadata #

Capabilities: Identifies and extracts technical metadata.

Details: XmlMetadata creates FITS XML natively.  Its sole purpose is to identify XML and parse out the default namespace and schema location. This is used for FITS text metadata.

Formats Supported: XML