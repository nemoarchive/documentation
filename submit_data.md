# Submitting Data to NeMO

## This documentation & described processes are under development

Data submitted to NeMO falls into three categories:
* *Public* - data to be immediately distributed openly and freely to the wider research community,
* *Embargo* - data to be held back, or embargoed, until a specific date, at which point it will be released openly and freely to the wider research community,
* *Restricted* - Controlled access data to be distributed only to an approved group of users due to consent restrictions, e.g. human data. Often restricted datasets contain a combination of private (raw reads, alignments) and public (counts, peaks) datatypes.  In such instances, **users will submit all data to the Restricted area** and we will work with submitters to obtain consent for releasing any files publicly.


### In this document:
1. [Requesting a NeMO Aspera account](#requesting-a-nemo-aspera-account)
2. [Submitting a File Manifest](#submitting-a-file-manifest)
 * format
 * validation
3. [Submitting data](#)
 * currently accepted data types
 * Aspera 
4. [Processing workflow](#processing-workflow)
5. [Weekly summary files](#weekly-summary-files)


#### Requesting a NeMO Aspera Account
New data submitters can register for an account at [nemoarchive.org/register.php](https://nemoarchive.org/register.php). Upon doing so, please send an email to [nemo@som.umaryland.edu](mailto:nemo@som.umaryland.edu) with the following information:
* requested username
* grant under which data was generated
* PI of lab in which data was generated
* whether data is public, for embargo or private*

*Private data submitters are asked to request account usernames with an appended "-restricted", e.g. `doe-restricted`. If you will be submitting both public and private data, you will need to create two accounts, one without and one with the "-restricted" tag.

Someone from the NeMO team will follow up with you regarding any other questions about your data or the submission process.


#### Submitting a File Manifest

(insert submission flowchart here)

All submissions to NeMO Archives, whether public or private, begin with upload of a file manifest including MD5 checksums, through the (NeMO web form coming soon).  
Because we do not dictate file naming conventions, and are no longer defining the specific directory structure that your data submission must contain, it is necessary for us to collect some basic information in order to process your data properly for release. This is not a comprehensive metadata collection and should be easy to populate. 

Detection and validation of a properly formatted manifest will trigger a message providing the Aspera submission path for your data, see below.


##### Manifest format
[Download the file manifest template](./manifest_template.txt)
[Download this Excel-friendly manifest file for further explanation of each field, and a complete list of controlled vocabularies](coming soon)

All manifest files **must** contain the following fields:
 * File name
 * Sample ID
 * Program (e.g. BICCN) - if you are unsure of the proper program designation, please reach out to nemo@som.umaryland.edu
 * Sub-program (e.g. U19 Zeng) - for this most part, this will correspond to the name of the P.I. or the project
 * Lab - P.I. under of the lab in which data was generated
 * Species
 * Modality
 * Sequencing Technology
 * Subspecimen type
 * File types
 * Access
 * Checksums - described below
 * BRAIN initiative only fields (optional):
   - BCDC Project name
   - BCDC data collection
 * Controlled access only fields (optional):
   - Usage (e.g. Limited/GRU/NA)
   - Institutional Certification ID
   - Donor id/name
   - Tissue provider

The manifest file must contain **one row, including an MD5 checksum, for every file included in the submission**. If you're submitting a tarball, the manifest should contain one row for every file within the tarball. For submission of multiple, or chunked, tarballs, there is no need for a manifest per tarball, all component files can be included in a single manifest.
The first column should contain the file name only, no path information. If populating the manifest in Excel or Numbers, please be sure to save as a comma or tab delimited file. The manifest file can have any prefix, however the base filename must be "manifest.[extension]. Only the following file extensions will be accepted: .txt, .tsv, .csv. Failure to name your manifest file accordingly will prevent detection of your manifest, delaying validation.

##### Manifest validation

Upon upload, file manifests will be validated immediately. Thus process checking for: 
presence of checksums and all other required fields
controlled vocabularies
detection of novel file extensions
checks for presence of all components of various bundle types (documentation coming)

Validation results will be made available right on the submission page, and sent via email.

Validation failure any row(s) will result in creation of a log file detailing errors. Once errors are corrected, an updated validation file is submitted in the same way and undergoes the same validation.
Validation success will result in notification via the browser and email, of the Aspera path to which you will submit your data. Be sure to submit to the provided path, any other submissions will be ignored and routinely deleted.


#### Submitting Data

##### Currently accepted data types
NeMO accepts both raw and derived data types of the file extensions [listed here](./file_extensions.md). If you plan to submit data extensions not currently on this list, please let us know ahead of time to avoid processing delays, by emailing nemo@som.umaryland.edu.

Depending on the size of your submission, it may be preferable to submit data files individually or within a tarball. Either is fine. We don't pre-define any specific directory structure for your submission, however we ask that you **do not** submit tarballs within a tarball, as that will delay processing. Files will be reorganized during ingest based on metadata provided in the manifest.


#### Uploading using Aspera
In order to submit data to NeMO, you will first need to download and install the IBM Aspera Command Line Interface, available from the [IBM website](https://www.ibm.com/products/aspera/downloads). You will find the download link under `Featured client software` > `IBM Aspera Command Line Interface`. Select the fix for your particular operating system. You will need to create an IBM account if you do not already have one. Follow installation instructions.

Once installed, you should have the `ascp` utility available to you. Have your Nemo Aspera credentials handy, as commands to initiate an upload will result in a prompt for your password.

Uploading with the Aspera client uses the following syntax:

```
$ ascp [-l <Maximum download speed>] [-k 2] [-m <Minimum download speed>] [-Q] [-T] \
<path to local file or directory> <username>@aspera.nemoarchive.org \
    /provided/path/to/NeMO/directory/
```

All parameters between the [ ... ] brackets are optional parameters described here:

```
l - Allows for the user to set a maximum upload/download speed that aspera should attempt to stay at or below for the duration of the transfer. A speed in Megabits must be provided with this flag.
k - Allow for resumable data transmission in case an interruption occurs.
m - Allows for the user to set a minimum upload/download sped that aspera should attempt to stay at or above for the duration of the transfer. A speed in Megabits must be provided with this flag.
Q - Turns adaptive rate on. Adaptive rate controls the speed of aspera with a goal of not dominating the bandwidth available. Very useful on busy networks that may have other transfers ongoing.
T - Turns encryption off. Turning encryption off will allow for a maximum throughput transfer but should not be provided if data being uploaded is sensitive.
```

In the following example, `my_data.tar.gz` is being transferred to NeMO:

```
$ ascp -l 100M -k 2 -QT /home/user/my_data/my_data.tar.gz user123@aspera.nemoarchive.org:/v34kltf7/
```
where `v34kltf7` is the directory name provided upon manifest validation. Uploading a directory of files would not require any changes as Aspera will recognize that a folder is being transferred and will recursively step through ensuring all files found in the directory are transferred.

```
$ ascp -l 100M -k 2 -QT /home/user/my_data/ user123@aspera.nemoarchive.org:/v34kltf7/
```


It is not possible to list, delete or move data using the Aspera CLI.
--- Subsequently uploading a file of the same name to the same Aspera path will trigger an overwrite of the original file. <-- we need rules on how long the aspera path will be maintained, in case someone wants to overwrite a submission? 
--- Submitters can use their NeMO Aspera credentials to view their pre-processed submitted data at https://aspera.nemoarchive.org. <- will we still provide access to the preprocessed data?
--- put in place a mechanism for deleting/replacing data?


#### Processing workflow
Our processing workflow is shown here:

<img alt="submit_data-8a030ed9.png" src="images/submit_data-8a030ed9.png" width="" height="" > (to be updated)

Once a manifest is validated...
1) begin regular scans for data in the aspera submission path
2) once data is detected, scan for upload completion. 

for each file in manifest, file status: not submitted -> transferring -> submitted -> valid --> processed --> released
                                               ^-------------------------------------invalid

--- reach out to submitter if aspx files persist and/or looks like upload failed

3) once all files submitted status, extract tarballs if neccessary
4) Validation: 
 -- Ensure all ingest contents are in tarball (via preview before extracting).
 -- MD5 matches for each file.  
 -- Generate log & delete files. Email contact to resubmit only problematic files

5) Version if necessary
6) Files passing checks are moved to the Processed area, and are reorganized based on information in the submitted manifest into the directory described [here](data_model documentation coming soon). 
7) bundle files together based on the rules described (coming soon) in the Release area
8) Data are now available for viewing via the NeMO Data Browser. Email sent confirming successful processing
9) Write new identifiers to mysql and (if possible) neo4j

**For restricted data**, the validation and ingest process is the same. Please refer to the [Restricted Data Release documentation](./release_restricted.md) for steps required to make restricted datasets available to approved users or the public, as applicable.



#### Weekly Summary files
Registration for a NeMO Aspera account will also trigger an invitation to join the NeMO Archive groups.io main group and appropriate grant-specific subgroups. As a member of these groups, submitters will receive weekly change logs for their incoming and release areas, and, if relevant, restricted incoming and restricted processed areas. In addition, the NeMO team will send a quarterly manifest of all public and restricted files in NeMO Archive.

These groups are also used for quarterly update reminders and other important information. If you would like to be added or to add someone else from your group to groups.io, please email nemo@som.umaryland.edu.
