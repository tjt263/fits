# Using the API #

```

Fits fits = new Fits("FITS_HOME"); 
FitsOutput fitsOut = fits.examine(input);
fitsOutput.saveToDisk("output/path/to/file");

```

FITS\_HOME is the directory containing the XML and tools directories. It can either be passed into the Fits() constructor, or it can be set as a system environment variable. All required FITS jars need to be on the classpath.  The xml/nlnz directory also has to be added to the classpath.

The FITS XML can be retrieved from FitsOutput as a JDOM object:
`FitsOutput.getFitsXml()`

Several convenience methods are provided. (This is not a complete list).

Test for an element with name:
`boolean hasElement = fitsOutput.hasMetadataElement("name");`

Look up a single element by name:
`FitsMetadataElement element = fitsOutput.getMetadataElement("name");`

Look up a list of all elements by name:
`List<FitsMetadataElement> elements = fitsOutput.getMetadataElements("name");`

Get a list of all elements in the FileInfo section:
`List<FitsMetadataElement> elements = fitsOutput.getFileInfoElements();`

Get a list of all elements in the FileStatus section:
`List<FitsMetadataElement> elements = fitsOutput.getFileStatusElements();`

Get the technical metadata type of the output (image, text, audio, document):
`String type = fitsOutput.getTechMetadataType();`

Get a list of all elements in the technical metadata section:
`List<FitsMetadataElement> elements = fitsOutput.getTechMetadataElements();`

Test for conflicting metadata values by name:
`Boolean hasConflict = hasConflictingMetadataElements("name");`


---


# Adding a Tool #

Any type of tool, whether it's based on Perl, Java, or something else entirely, can be added to FITS.
A tool wrapper must be created that encapsulates the complexities of invoking the tool, capturing the output, and converting it to FITS XML.

For example Exiftool is written in Perl.  The Exiftool tool wrapper checks for the operating system type and whether or not Perl is installed. It then can decide if it should use the standard Perl version of Exiftool or the windows executable.

A tool wrapper must implement the Tool interface.  If the tool depends on a specific operating system, the necessary checks should be made within the tool wrapper to prevent execution on incompatible systems.

It is the responsibility of the tool wrapper to convert the tool output into FITS XML and return a valid ToolOutput object.

For tools that natively return XML, XSLT can be used to convert the output to FITS XML.  For tools that do not return XML, the output can either be a) directly converted to FITS XML, or b) converted to a basic intermediate XML format and then converted using XSLT.

Any new tool wrappers must be added to the xml/fits.xml configuration file.  FITS handles initializing the wrapper and sending the input file to it.

FITS generally prefers to use text labels rather than numeric values for technical metadata.  For example an image's Resolution Unit should be "inches" instead of the value 2.  If a tool prefers to output numeric values then these can be converted either using either the FITS mapping file, or during the conversion process from the native tool output to FITS xml.

The ToolBase abstract class implements the Tool interface and provides methods for applying XSLT transforms, checking for unknown identities and excluded extensions.  The current tool wrappers all extend ToolBase.


---


# Overview of Interfaces #

## Tool ##

The Tool interface defines the following methods:
`ToolOutput extractInfo(File file)` -- Called by FITS to invoke the tool against the input file.  It must return a ToolOutput object containing valid FITS XML

`boolean isIdentityKnown(FileIdentity identity)` -- Logic for determining if the tool was able to identify the input file

`ToolInfo getToolInfo()` -- Returns a ToolInfo object describing the tool.

`Boolean canIdentify()` -- indicates whether or not the tool can identify file formats.  This is important to know for output consolidation purposes.

`void addExcludedExtension()` -- Adds a file extension to specify that FITS should not use this tool wrapper to process files with that extension (set from xml/fits.xml tool definitions)

`boolean hasExcludedExtension(String ext)` -- Checks if the tool can process files having the provided file extension.

## ToolOutputConsolidator ##

The ToolOutputConsolidator interface defines one method:
`FitsOutput processResults(List<ToolOutput> results);`

Classes implementing the ToolOutputConsolidator must accept a list of ToolOutput objects, merge them, and return a FitsOutput object.


---


# Test Suite #

A basic test suite is provided.  The launcher scripts (testoutput.bat, testoutput.sh) can be run in 'create' or 'test' mode.  Running in create mode will call FITS on all files in the tests/input directory, writing the output to tests/output.  Running in 'test' mode will call FITS against all files in tests/input and compare the output to the corresponding output file in tests/output. All structure and element values are compared, but any attribute differences are ignored.  Attributes in the FITS output generally contain timestamps or tool version numbers, which could change if a tool is updated to newer version.

Note that changing the fits.xml configuration or adding a new tool will require running the output test in 'create' mode to rebuild the output files.


---


# UML Diagrams #

## Fits Overview ##
![http://fits.googlecode.com/svn/wiki/fits.png](http://fits.googlecode.com/svn/wiki/fits.png)


---


## Tools ##
![http://fits.googlecode.com/svn/wiki/tools.png](http://fits.googlecode.com/svn/wiki/tools.png)


---


## Consolidation ##
![http://fits.googlecode.com/svn/wiki/consolidation.png](http://fits.googlecode.com/svn/wiki/consolidation.png)


---


## Identity ##
![http://fits.googlecode.com/svn/wiki/identity.png](http://fits.googlecode.com/svn/wiki/identity.png)


---


## Mapping ##
![http://fits.googlecode.com/svn/wiki/mapping.png](http://fits.googlecode.com/svn/wiki/mapping.png)