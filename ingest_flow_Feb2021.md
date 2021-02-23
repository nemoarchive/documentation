# New Ingest Flow

## Manifest submission

1. User submits manifest via a web-form (Flask?)
   1. Can accept either CSV or TSV file
   2. Open-access and controlled datasets should be treated the same.  Check "access" field.
   3. Ensure open-access and controlled (restricted) data is not in the same manifest.
2. Validation checks for each row in manifest file and give error message for each row that validation fails.
   1. Ensure the required metadata is present and controlled vocabulary is used where specified
   2. Ensure all submitted files cam be assigned a file type given the file type patterns in FileEntity.py
   3. Ensure all bundled component contents are accounted for.  Make note of missing ones.
   4. In cases where we ingest directories the md5 will be all contents in the directory
      1. We will not advertise this feature but handle on a case-by-case basis.  This will allow us to develop this later on.
   5. If failure, send email to user (provided in web-form) if validated or not.  Give saved log file of errors.
3. When validation succeeds, will tell users in web form response what Aspera command to use to upload their other files
   1. Aspera path will also be sent to email in web-form.
   2. Directory path will be built based on metadata passed in.
   3. Aspera path will be randomized (like python tempfile/tempdir commands)
4. This directory is used to create an "incoming path" that will be appended to each row of the manifest file which will be written to an "aspera-ready" text file in a specific area.  A script will crawl this directory area and process each text file

## Aspera monitoring

1. Cron launches script every 30 minutes or so.
2. Script will crawl through all "aspera-ready" text files in a "aspera ready" directory on a periodic basis.
   1. If no manifests are present, then exit.
3. When a new manifest is detected in the directory, things start to happen.
   1. A .flag file is added to note the manifest is currently in use so we do not double-process it.
4. For each row in text file the script will open the directory and check for the .aspx file for each entry
   1. If directory is empty, the files have not begun transfer through aspera so do nothing
   2. If .aspx file is present, we change the state of the file_entity from NOT SUBMITTED to TRANSFERRING, but do nothing else.
      1. If .aspx file is longer than a day old, email contact about potential issue with transfer (as a warning).  May be false alarm with a large file.
   3. Keep checking every 10 minutes or so.
   4. If file is present and .aspx is missing, assume this file transferred.  Set file entity state to SUBMITTED
5. If all ingestable files from manifest are present and .aspx file is missing for each file, then assume transfer finished.
   1. Does not apply in tarball.  We'll check during validation.
6. If tarball, extract
7. Validation
   1. Any file validation failures change file entity state to INVALID
   2. Validation successes change file entity state to VALID
   3. Ensure all ingest contents are in tarball (via preview before extracting).
   4. MD5 matches for each file.  Make note of which files do not match.  Wipe these files.
   5. If directory is to be ingested, ensure that the empty flag file exists within the directory.
      1. Worry about this later.
   6. If validation fails, keep "aspera-ready" manifest file where it is and treat as if directory uploading is not finished (since files will be wiped)
      1. This will involve reverting state of invalid file from INVALID to NOT SUBMITTED
   7. Email contacts sent to web-form submitter of all validation failures (kept by python logging module)
8. Once validation succeeds
   1. Determine if version needs to be incremented based on release area
   2. Move to processed
      1. Change file state to PROCESSED
   3. Copy to release or bundle to release
      1. Change file state to RELEASED
   4. Write new identifiers to mysql and (if possible) neo4j
   5. Move "aspera-ready" manifest file to "finished" directory (for tracking purposes).
      1. Remove .flag file for file

## Periodically

1. Remove non-ingestable files from an Aspera area so they are not just taking up space (wipe the directory?)

## Misc

* File types will be handle via a class rather than by a dictionary, which is a change from the old ingest
