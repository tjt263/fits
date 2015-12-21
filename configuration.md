# Configuration File #

The primary configuration file is xml/fits.xml.


---


## Tools ##

Wrapped tools are defined by `<tool>` elements. The order of these elements determines the preference in favoring one tool over another. Tool elements must have a 'class' attribute specifying the full Java classpath to the class that implements the Tool interface. The 'exclude-exts' attribute is optional.  It specifies file extensions that the tool should not process.  This is useful if you know a tool misidentifies or generates inaccurate metadata for specific types of files.

## Output ##

**`<dataConsolidator>`**  Specifies the class to use for consolidating the tool output.
It's possible to use custom logic to control the tool output consolidation processes by creating a class implementing the ToolOutputConsolidator interface.

**`<display-tool-output>`** Option for appending the native output for each tool onto the end of the final consolidated FITS XML output.

**`<report-conflicts>`** Option for controlling the display of conflicts.  If set to true, conflicts will be shown in the final FITS XML output. If set to false, only the output from the most preferred tool (controlled by the ordering of the tool elements) will be displayed.

**`<validate-tool-output>`** Generally this should be set to true.  Setting it to false will disable schema validation of the output from each tool.

**`<internal-output-schema>`** Local reference to the FITS XML schema used for validating tool output.

**`<external-output-schema>`** Remote location of the schema referenced when creating the final consolidated FITS XML.


---


# Mapping File #

The mapping file allows for substituting one value for another on a tool by tool, element by element basis.  Rules are defined in the XML file xml/fits\_xml\_map.xml.

For example, if a tool outputs the value "2" as the sampling frequency unit for an image, but you want to use the text string "inches" instead, you could add an entry to fits\_xml\_map.xml. Mappings are applied automatically when a tool creates its FITS output, prior to output consolidation.  You must specify the tool name, version, and element name that you want mapped.  Currently all mapping related needs are handled in the tool's XSLT.


---


# Format Tree #

Certain formats are a more specific subset of a more general format. The format tree (xml/fits\_format\_tree.xml) specifies these relationships.  During output consolidation the format tree is consulted, and any less specific identities are thrown out. For example, OpenOffice text document formats are ZIP-based. Some tools identify these files as ZIP, and others as ODT.  Any tools identifying the file as a ZIP would be discarded according to the rules set by the format tree.


---


# How to Add a New Format #

Certain tools can identify and extract technical metadata for files (Jhove, Exiftool, NLNZ ME), others can only identify file formats (Droid, FFident, File Utility). The specific formats that can have technical metadata extracted varies depending on the tool.  Jhove and NLNZ ME support a small set of popular formats, while Exiftool supports a wide range.

To add FITS-supported technical metadata for a tool:
  1. Decide if the new format fits into one of the already supported format types: image, text, audio, document
  1. If it does not, add the new metadata type to the fits schemas.
  1. Decide if XSLT will be used to transform the tool output into FITS XML
    * If so, create an XSLT file to transform the technical metadata output into FITS XML.
    * If a XSLT to format mapping file exists, add the new format to it.  For example: xml/exiftool/exiftool\_xslt\_map.xml