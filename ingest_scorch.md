# SCORCH Data Ingestion Overview
The SCORCH consortium has implemented consortium-wide metadata standards for incoming data which results in the ingest process being different than a standard data upload. Submission of SCORCH files consists of 4 parts:
- [Initial formatting validation](https://github.com/nemoarchive/documentation/blob/master/ingest_scorch.md#initial-formatting-validation)
- [File upload](https://github.com/nemoarchive/documentation/blob/master/ingest_scorch.md#file-upload)
- [Validation](https://github.com/nemoarchive/documentation/blob/master/ingest_scorch.md#validation)
- [Data release](https://github.com/nemoarchive/documentation/blob/master/ingest_scorch.md#release)

To contact the NeMO team about any questions or issues, please use the form [here](https://github.com/nemoarchive/helpdesk/issues/new/choose).

## Initial formatting validation
  To ensure files are associated with the correct metadata, SCORCH manifest submissions consist of:
  - Subject metadata
  - Sample metadata
  - Library information
  - File-level information

At this step, users submit a formatted manifest ([Formatting a SCORCH manifest](https://github.com/nemoarchive/documentation/blob/master/manifest_formatting_scorch.md)), either by logging into [nemoarchive.org](https://nemoarchive.org) and using the 'Make a submission' option on your dashboard or using an API ([NeMO API details](https://nemoarchive.org/resources/nemo-api-overview.php)).
The formatting validation can be run as a 'dry run' where the uploaded manifest is validated for correctness but the submission process does not continue (i.e. no Aspera upload path is given). This is useful for testing new submissions so that any manifest failures do not show up in your dashboard. 
Every submission made without the dry run checkbox selected will be displayed in your user dashboard. 

<p align="center">
<img width="380" align="center" alt="NeMO manifest submission page with option for dry-run" src="https://github.com/nemoarchive/documentation/assets/28451557/15dff9be-8579-487d-a993-d6abf8068386">
</p>

Once the manifest is uploaded and submitted, you will receive an email shortly with the results of your validation check (this can take up to 5 minutes). Note: this email will be sent to the email address associated with you account. 
In that email, you will either see that your manifest was successfully validated for format or receive a list of errors that need to be corrected before the manifest is resubmited. 
If your submission is not a dry run, you will also receive an Aspera command and location to submit your files (seven character string, this is your submission ID).

## File upload
Once you have received a message that your manifest was successfully validated for format, the files can be submitted via Aspera. In your validation email, you will receive an Aspera command to be used for upload 
(e.g. ascp -l 1000M -k 2 -QT user@aspera.nemoarchive.org:89qteg4/). Note: You will need to add in the path where the files are to be uploaded from to this command (e.g. ascp -l 1000M -k 2 -QT SCORCH/FilesForUpload/*.fastq user@aspera.nemoarchive.org:89qteg4/).

**Files must be submitted in a flattened format (i.e. no directory structure present in the upload). Files in subdirectories will not be found by the ingest process and you will receive an error saying files are missing.**

### Once all files are uploaded
To let us know that your upload is complete, submit a single empty file named 'DONE' using the same Aspera command as used to submit your files (running touch DONE on Linux is an easy way to make this file). 

**Do not submit an empty file named DONE until your file uploads are complete!!**
Once the DONE file is found within your directory, automated validation of your files begins (Note: it will take up to an hour after the DONE file is added for the change in status to be reflected on your Dashboard). 
If the DONE file is not added, you will receive a reminder email 15 days after manifest validation, at 30 days your submission will error out as inactive.

## Validation
During the validation/quality control step, NeMO Archive checks that: 

1) All files present in the manifest are in your upload directory
2) Checksums of files match those present in the manifest
3) Subject and file data use/access restrictions match those expected for a given project/data type

If an error is found at this step, you will receive an automated email from nemo@som.umaryland.org or one of our analysts with instructions for correcting your submission. 

## Release
Following validation, the next step in the process is data release. The exact locations of released files will depend on the data use/access restrictions but for open data (i.e. with no release limitations) files are made available in the following locations:
1) Through the data portal/s for open data
2) Via dbGaP for restricted human data

**Note for SCORCH**: NeMO Archive currently maintains two data portals which host SCORCH data. The [Public SCORCH portal](https://scorch-portal.nemoarchive.org/) will contain only public SCORCH data (and is accessible to users from outside the consortium).The [Internal SCORCH data portal](https://private-scorch-portal.nemoarchive.org/) is for SCORCH consortium members only (requires log in credentials) and will contain public data as well as data and metadata currently under embargo prior to public release.

Once files reach the release step, they are considered fully ingested to NeMO. At this point you will receive a receipt with your file information as well as persistent NeMO identifiers for each file submitted. 

