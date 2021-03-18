# New Ingest Flow - Engineering documentation

## Manifest submission

1. User submits manifest at https://assets.nemarchive.org/manifest_submission
   1. Valid extensions accepted - .csv, .tab, or .tsv
   2. Must also provide contact email and valid Aspera name
2. Manifest file is renamed with a temporary 7-character string to avoid colliding with another manifest submission if simultaneous.  This file is placed in a "submitted_manifest" directory while validation is happening
   1.  A manifest object (Manifest class in manifest.py) is created and a log file is saved with the manifest
3. Validation checks:
   1. Headers in manifest file match those from manifest template on Github
      1. Currently those are hard-coded into the manifest.py module so if they change in Github they will need to be changed here
   2. Fields that are required are checked to ensure the fields are filled in
   3. All filenames in the manifest must be unique
   4. Fields with controlled vocabulary must use that vocabulary.
      1. Controlled vocabulary Excel file from Github is downloaded and parsed to get the controlled vocab.
   5. For each file, an attempt is made to discern its file type (and make a class-based object from file_entity.py) by pattern-matching the file extension or other relevant parts.
   6. For each file, an attempt is made to build the "validated" filepath where a file will be copied to, based on the vocab-to-dirname mapping schema.
      1. Vocab-to-dirname mapping is currently hardcoded in a function in manifest.py
   7. For each file that can be bundled, an attempt is made to create a bundle object (from bundle_entity.py) amongst file components that constitute it, and a check is made to see if all component files are present
4. If validation fails...
   1. The user is redirected to the index page (same page with the form) and all validation errors are shown.  The user can take this time to correct and re-submit the manifest
5. If validation succeeds...
   1. A random 7-character string is used to create the Aspera submission directory in the Aspera root that the user will place their files in.
      1. The aspera root is dependent on if at least one file is restricted ("controlled") or embargoed.  If any files is restricted, all files will be submitted to the restricted aspera root directory (though the process may be a special case).  If any files are embargoed (and none are restricted), all files will be submitted to the embargo aspera root directory
   2. This aspera dir, along with contact email and the "validated" path for the file is appended to each row in the manifest, and this new manifest is written to the "valid_manifests" area.  The original manifest file is removed from the "submitted_manifests" area to keep the area clean.
   3. The user is sent to the next page, there they are shown an Aspera submission command to use for their submission. This command is also emailed to the contact email provided.

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
