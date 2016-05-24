Segmentation Fault Detector Scripts
-----------------------------------

This project is a set of Unix Bash shell scripts that perform the following functions:

detect-segfaults.bsh
--------------------

This script detects a segmentation fault by running a grep on the 'tail -f' of the MarkLogic ErrorLog.txt. When it finds a Segmentation fault, it inserts a Uri into the following known location:

-   Database = documents

-   doc-uri = /ml\_65574/segfault.xml

The Uri has the following content:

&lt;signal-received&gt;true&lt;/signal-received&gt;

detect-uri.bsh
--------------

This script checks for the existence of a Uri named “/ml\_65574/segfault.xml” in the Documents database. If not found, it loops forever; if found, runs a separate bash script that creates a pstack movie under the /tmp directory of the local host.

The pstack movie is of 2 minutes duration.

send-segfault.bsh
-----------------

This script is used to test the functionality of the above scripts. It identifies the MarkLogic server process and sends it a 'Segmentation Fault' kill signal.

ml-support-dump.sh
------------------

This script runs for 2 minutes and creates a zip file under the following path:

/tmp/$TSTAMP.zip

The zip file contains the following files:

-   iostat.log

-   vmstat.log

-   pstack.log

-   pmap.log

-   pstack-summary.log

-   sar-summary.log

Running the Shell files
-----------------------

To run the project, you should open a couple of shell command windows and run them as follows:

1.  Window \#1: run ‘./detect-segfault.bsh’

2.  Window \#2: run ‘./detect-uri.bsh’

To test the first two scripts, open a third shell window and execute the following command:

> ./send-segfault.bsh

This will cause a Sementation Fault signal to be sent to the MarkLogic process, which will be detected by the *detect-segfault.bsh* shell script. A well-known Uri will be placed in the Documents databse, which will de seen by the *detect-uri.bsh* script and cause it to run the *ml-support-dump* script.

You can find the results of this script on the host machine under the following directory:

/tmp/\[timestamp\].zip

Bugs and Improvements
---------------------

Please file bugs and improvement requests on the project page.
