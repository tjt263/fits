# Introduction #

The File Information Tool Set (FITS) identifies, validates and extracts technical metadata for a wide range of file formats.  It acts as a wrapper, invoking and managing the output from several other open source tools. Output from these tools are converted into a common format, compared to one another and consolidated into a single XML output file.  FITS is written in Java and is compatible with Java 1.6 or higher.  The external tools currently used are:
  * [Jhove](http://hul.harvard.edu/jhove/)
  * [Exiftool](http://www.sno.phy.queensu.ca/~phil/exiftool/)
  * [National Library of New Zealand Metadata Extractor](http://meta-extractor.sourceforge.net/)
  * [DROID](http://droid.sourceforge.net/)
  * [FFIdent](http://schmidt.devlib.org/ffident/index.html)
  * [File Utility](http://unixhelp.ed.ac.uk/CGI/man-cgi?file) ([windows](http://gnuwin32.sourceforge.net/))


---


# How to Use FITS #

FITS can be used as a command line tool or within other projects using its API.  It relies on an environment variable named FITS\_HOME to find the needed configuration and xsl transforms directories.

## Command Line ##

Windows .bat file and Linux/OS X shell launcher scripts are provided.  These scripts build the necessary Java classpath and set the FITS\_HOME environment variable automatically.

## Command Line Options ##
  * -i The input file you want to examine
  * -o The destination of the output XML file.
  * -r process directories recursively when -i is a directory
  * -h Prints the usage message
  * -v Displays the FITS version number
  * -x convert FITS output to a standard metadata schema
  * -xc output using a standard metadata schema and include FITS xml

If -o is not specified then the output is sent to the console window.

The general syntax is:

`>fits.[bat|sh] -i input_file -o output_file`

## API ##

When using the API the FITS\_HOME environment variable must be passed in with the Fits() constructor. See the Developer Info section.


---


# Overview of FITS Life Cycle #

  1. configuration load
    * FITS\_HOME environment variable set up
    * Fits.xml configuration file loaded
    * Tool wrappers created
    * Output consolidator configured.
  1. for each tool wrapper
    * each tool executed on file creating a ToolOutput object containing a fits xml document
      * If necessary, XSLT is applied to tool output to create the FITS compatible xml
    * FITS mapping file applied (xml/fits\_xml\_map.xml)
  1. consolidation
    * Identities consolidated
      * format tree (xml/fits\_format\_tree.xml) consulted
    * Output from tools unable to identify the file or those who identified a less specific type are thrown out
    * Fileinfo sections merged
    * Filestatus sections merged
    * metadata sections merged
  1. Output
    * The consolidated fits xml file is written to a file or the console
    * If using the API a FitsOutput object is returned


---


# Output Format #

Each tool wrapper must implement the Tool interface and return a ToolOutput object.  ToolOutput must contain a valid FITS XML JDOM object. Each tool's output is validated against the local FITS XML schema when the ToolOutput object is created.  The schema is located in xml/fits\_output.xsd.

During consolidation tool output conflicts are accounted for by adding a status attribute to the element.

After consolidation a single FITS XML file will reference the online schema located at http://hul.harvard.edu/ois/xml/xsd/fits/fits_output.xsd

## Status Attributes ##

If multiple tools disagree on an identity or other metadata values, a status attribute is added to the element with a value of "CONFLICT".  If only a single tool reports an identity or other metadata value a status attribute is added to the element with a value of "SINGLE\_RESULT". If multiple tools agree on a an identity or value, and none disagree, the status attribute is omitted.

## Tool Ordering Preference ##

The ordering preference of the tools in xml/fits.xml determines the ordering of conflicting values.  If the report-conflict configuration option is set to false then only the tool that first reported the element is displayed.  The other conflicting values are discarded.

## Identities and Technical Metadata ##

All tools that agree on an identity are consolidated into a single `<identity>` section. Technical metadata is only output (and a part of the consolidation process) for tools that were able to identify the file and that are listed in the first `<identity>` section. All other output is discarded.

## Tool Output Normalization ##

It’s possible for tools to output conflicting data when they actually mean the same thing.  For example, one tool could report the format of a PNG image as “Portable Network Graphics”, while another may report “PNG”.  A tool could report a sampling frequency unit of “2”, while another may report the text string “inches”.   If left alone, these would cause false positive conflicts to appear in the FITS consolidated output.  These differences are converted in the XSLT that converts the native tool output into FITS XML.  In general FITS prefers text strings to numeric values (“inches” instead of “2”), and complete format names to abbreviations (“Portable Network Graphics” instead of “PNG”).  If new tools or formats are being added to FITS then thorough testing should be done to ensure that any false positive conflicts are resolved.