# Install FITS Code #
From this Downloads section of this website, download the file named fits-(version).zip, for example fits-0.2.5.zip. **The FITS source is included in this ZIP file.**

Create a new directory on your computer and unzip the contents of the FITS ZIP file to the directory.

# Running FITS (command-line option) #

The way you run FITS will depend on your operating system. **Note that FITS also has a Java API that should be considered for use when processing a large number of files on an on-going basis.**

## Windows ##
These instructions describe how to run FITS in a Terminal window. There are other ways you could run FITS on Windows, including from the Windows Explorer, and using the Java API.

  1. Open up a command line interface.
    * Click on Start -> Click on Run -> Type in cmd
  1. Navigate to the directory where you installed FITS, for example:
    * cd "Program Files"\fits\fits-0.2.5
  1. Run the script names fits.bat
    * >fits.bat
  1. You should see the FITS usage print out
  1. Run FITS against a file using the -i parameter, for example:
    * >fits.bat -i "..\..\..\Documents and Settings\agoethals\Desktop\9198541.arc.gz"
  1. You should see the FITS XML output to your terminal screen.
  1. Save FITS XML output to a file using the -o parameter, for example:
    * >fits.bat -i "..\..\..\Documents and Settings\agoethals\Desktop\9198541.arc.gz" -o "..\..\..\Documents and Settings\agoethals\temp\fits\_9198541.arc.gz.xml"


## Unix ##
These instructions describe how to run FITS in a terminal window. You could also run FITS on Unix using the Java API.

  1. Open up a terminal window.
  1. Navigate to the directory where you installed FITS
  1. If it not already, make the fits.sh file executable
    * chmod +x fits.sh
  1. Run the script named fits.sh
    * ./fits.sh