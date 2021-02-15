# Submitting Data to NeMO

Data submitted to NeMO falls into three categories:
* *Public* - data to be immediately distributed openly and freely to the wider research community,
* *Embargo* - data to be held back, or embargoed, until a specific date, at which point it will be released openly and freely to the wider research community,
* *Restricted* - Controlled access data to be distributed only to an approved group of users due to consent restrictions, e.g. human data. Often restricted datasets contain a combination of private (raw reads, alignments) and public (counts, peaks) datatypes.  In such instances, users will submit **all** data to the Restricted area and we will work with the submitters to obtain consent for releasing any files publicly.

### In this document:
1. [Requesting a NeMO Aspera account](#requesting-a-nemo-aspera-account)
2. [Submission requirements](#submission-requirements)
 * File manifest
 * Checksums
 * Accepted data types
3. [Uploading using Aspera](#uploading-using-aspera)
  * Reviewing submissions
4. [Submission validation](#submission-validation)
5. [Processing workflow](#processing-workflow)
6. Metadata
7. [Weekly summary files](#weekly-summary-files)

&nbsp;
#### Requesting a NeMO Aspera Account
New users can register for an account at [nemoarchive.org/register.php](https://nemoarchive.org/register.php). Upon doing so, please send an email to [nemo@som.umaryland.edu](mailto:nemo@som.umaryland.edu) with the following information:
* requested username
* grant under which data was generated
* PI of lab in which data was generated
* whether data is public, for embargo or private*

*Private data submitters are asked to request account usernames with an appended "-restricted", e.g. `doe-restricted`. If you will be submitting both public and private data, you will need to create two accounts, one without and one with the "-restricted" tag.

Someone from the NeMO team will follow up with you regarding any other questions about your data.


#### Submission requirements
All submissions to NeMO Archives, whether public or private, must contain the following 3 items. Failure to include any one will result in a validation error and your submission will not be processed.
1. a file manifest
2. checksum file
3. Data

Depending on the size of your submission, it may be preferable to submit data files individually or within a tarball. Either is fine.

##### File Manifest
[Download the file manifest template](./manifest_template.txt)

Because we do not dictate file naming conventions, and are no longer defining the specific directory structure that your data submission must contain, it is necessary for us to collect some basic information in order to process your data properly for release. This is **not** a comprehensive metadata collection and should be easy to populate.

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
 * BRAIN initiative only fields:
   - BCDC Project name
   - BCDC data collection
 * Controlled access only fields:
   - Usage (e.g. Limited/GRU/NA)
   - Institutional Certification ID
   - Donor id/name
   - Tissue provider

[Download this Excel-friendly manifest file for further explanation of each field, and a complete list of controlled vocabularies]

The manifest file must contain **one row for every file included in the submission**. If you're submitting a tarball, the manifest should contain one row for every file within the tarball. For submission of multiple, or chunked, tarballs, there is no need for a manifest per tarball, all component files can be included in a single manifest.
The first column should contain the file name only, no path information. If populating the manifest in Excel or Numbers, please be sure to save as a .txt file. Any other file extensions will fail the initial validation step. The manifest file can have any prefix, however the base filename must be "manifest.txt". Failure to name your manifest file accordingly will prevent detection of your submission, delaying validation & processing.

***IMPORTANT!*** *If submitting data in a compressed (e.g. tarball) format, your manifest file must be outside of the tarball in order to be recognized by the validator.*


##### Checksums
We require an MD5 checksum file, compatible with the standard `md5sum` utility, that is:

```
<128-bit hexadecimal md5 checksum><2 spaces><file path relative to tarball root>
```

for example

```
b71aacdabe99a51474ba84fa03ec61ec  my_lab/chromatin/sncell/processed/C
24c37182df9ead481be538a4dcecf641  my_lab/chromatin/sncell/raw/D
```

Please do not provide an individual checksum file for each data file, but rather a single checksum file containing **all** files in the submission. If you're submitting a tarball, the checksum file should contain one row for every file within the tarball. For submission of multiple, or chunked, tarballs, there is no need for a checksum file per tarball, all component files can be included in a single checksum file.
The checksum file can have any prefix, however the base filename must be "md5sums.txt" and the file must contain Unix line endings.

***IMPORTANT!*** *If submitting data in a compressed (e.g. tarball) format, your checksum file must be outside of the tarball in order to be recognized by the validator.*


##### Accepted data types
NeMO accepts both raw and derived data types. The NeMO ingest scripts that launch the processing workflow allow the file extensions [listed here](./file_extensions.md). If you plan to submit data extensions not currently on this list, please let us know ahead of time to avoid processing delays, by emailing nemo@som.umaryland.edu.

&nbsp;

#### Uploading using Aspera
In order to submit to NeMO, you will first need to download and install the IBM Aspera Command Line Interface, available from the [IBM website](https://www.ibm.com/products/aspera/downloads). You will find the download link under `Featured client software` > `IBM Aspera Command Line Interface`. Select the fix for your particular operating system. You will need to create an IBM account if you do not already have one. Follow installation instructions.

Once installed, you should have the `ascp` utility available to you. Have your Nemo Aspera credentials handy, as commands to initiate an upload will result in a prompt for your password.

Uploading with the Aspera client uses the following syntax:

```
$ ascp [-l <Maximum download speed>] [-k 2] [-m <Minimum download speed>] [-Q] [-T] \
<path to local file or directory> <username>@aspera.nemoarchive.org \
    /path/to/NeMO/directory/
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
$ ascp -l 100M -k 2 -QT /home/user/my_data/my_data.tar.gz user123@aspera.nemoarchive.org:/biccn/
```
Uploading a directory of files would not require any changes as Aspera will recognize that a folder is being transferred and will recursively step through ensuring all files found in the directory are transferred.

```
$ ascp -l 100M -k 2 -QT /home/user/my_data/ user123@aspera.nemoarchive.org:/biccn/
```

Public data submitters will be mounted at the root level of the NeMO file system. Within that, data is organized by Project: BICCC, BICCN or Other. For more information on our directory structure, see [HTTP Access](link), Please submit your data to the appropriate project. Any other directory structure pre-organization is optional. Files will be reorganized during ingest based on metadata provided in the manifest.
Restricted data submitters will be mounted in a directory specific to their project, for privacy reasons, so submissions should be made **directly to the mount point**:

```
$ ascp -l 100M -k 2 -QT /home/user/my_private_data/ user123-restricted@aspera.nemoarchive.org:.
```

It is not possible to list, delete or move data using the Aspera CLI. Subsequently uploading a file of the exact same name will trigger an overwrite of the original file. Submitters can use their NeMO Aspera credentials to view their pre-processed submitted data at https://aspera.nemoarchive.org.

&nbsp;
#### Submission validation

Upload of a manifest.txt file will trigger our validation process, which checks for presence of a checksum file and looks for noncanonical file extensions. A validation email will be sent within 24 hours of completion of upload, either confirming successful submission or outlining validation failures. If you have not received a validation email from NeMO 24 hours after submission, please email nemo@som.umaryland.edu, as your manifest file was most likely not detected.

&nbsp;

#### Processing workflow
Our processing workflow is shown here:

<img alt="submit_data-8a030ed9.png" src="images/submit_data-8a030ed9.png" width="" height="" >

&nbsp;
Successful validation of a submission will launch the first step of the ingest process,
which reviews the submission for incomplete uploads, runs checksums locally and compares with submitted checksums, and runs a series of checks for presence of all components of various bundle types (documentation coming).
Files passing checks are moved to the Processed area, and are reorganized based on information in the submitted manifest into the directory described [here](data_model documentation coming soon).

A weekly cron job bundles files together based on the rules described above (coming soon) in the Release area.
Data are now available for viewing via the NeMO Data Browser.
Assuming no validation errors, data submitters should expect no more than one week for their submission to be publicly available.

**For restricted data**, the validation and ingest process is the same. Please refer to the [Restricted Data Release documentation](./release_restricted.md) for steps required to make restricted datasets available to approved users or the public, as applicable.


#### Metadata
Coming soon


#### Weekly Summary files
Registration for a NeMO Aspera account will also trigger an invitation to join the NeMO Archive groups.io main group and appropriate grant-specific subgroups. As a member of these groups, submitters will receive weekly change logs for their incoming and release areas, and, if relevant, restricted incoming and restricted processed areas. In addition, the NeMO team will send a quarterly manifest of all public and restricted files in NeMO Archive.

If you would like to be added or to add someone else from your group to groups.io, please email nemo@som.umaryland.edu.
