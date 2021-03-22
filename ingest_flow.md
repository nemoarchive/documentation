# New Ingest Flow - Engineering documentation

## Manifest submission

1. User submits manifest at https://assets.nemarchive.org/manifest_submission
   1. Valid extensions accepted - .csv, .tab, or .tsv
   2. Must also provide contact email and valid Aspera name
2. Manifest file is renamed with a temporary 7-character string to avoid colliding with another manifest submission if simultaneous.  This file is placed in a "submitted_manifest" directory while validation is happening
   1.  A manifest object (Manifest class in manifest.py) is created and a log file is saved with the manifest.  Initial state is "SUBMITTED"
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
   8. If any of the above checks fail for the manifest, file, or bundle, the manifest state is set to "INVALID" and errors are recorded in the Manifest-object's "errors" property. If all checks pass, the manifest status is set to "VALID".
4. If validation fails...
   1. The user is redirected to the index page (same page with the form) and all validation errors are shown.  The user can take this time to correct and re-submit the manifest
5. If validation succeeds...
   1. A random 7-character string is used to create the Aspera submission directory in the Aspera root that the user will place their files in.
      1. The aspera root is dependent on if at least one file is restricted ("controlled") or embargoed.  If any files is restricted, all files will be submitted to the restricted aspera root directory (though the process may be a special case).  If any files are embargoed (and none are restricted), all files will be submitted to the embargo aspera root directory
   2. This aspera dir, along with contact email and the "validated" path for the file is appended to each row in the manifest, and this new manifest is written to the "valid_manifests" area.  The original manifest file is removed from the "submitted_manifests" area to keep the area clean.
   3. The user is sent to the next page, there they are shown an Aspera submission command to use for their submission. This command is also emailed to the contact email provided.

## Ingest Aspera Submissions

1. Cron will launch the "ingest_aspera_submissions.py" script.  Currently no time interval has been set for this.
2. The "valid manifests" directory will be inspected for unprocessed manifests.
   1. A manifest will end in "manifest.tsv"
   2. If a manifest has a ".flag" file associated, that means the manifest is currently being processed by a previous run of "ingest_aspera_submissions.py" and will be ignored.
3. Inside the manifest, the Aspera area the user should have submitted their files is determined.
   1. This area can be either in "public", "embargo", or "restricted" depending on the "Access" field. Please consult "Manifest Submission - 5.1.1" for more information.
4. If a tarball is found in the Aspera submission area, it is extracted.
5. For each row in the manifest
   1. A file entity object is created for each potential file name, file state set to "NOT SUBMITTED", and the expected md5 is recorded.
   2. A check to determine if all files in Aspera are ready for transfer begins
      1. If tarball was present and extracted, was file in tarball?  If so, set file to "SUBMITTED"
      2. If file was transferred via Aspera, is the .aspx file gone?  If so, set file to "SUBMITTED".  If file is present along with .aspx, set state to "TRANSFERRING"
6. If all files have "SUBMITTED" state we proceed, as we assume transfer finished.
   1. If even a single file is not in "SUBMITTED" state, the .flag file for the manifest file is removed and the script exits.  The next cron iteration will attempt to process this manifest again.
7. For each file, validate the observed MD5 against the expected MD5, and set state to "VALID" if md5s match.
   1. If md5 does not match, mark state as "INVALID", and send email about validation errors to contact email found in manifest.
   2. "INVALID" files are also removed from the Aspera submission area, so the user will have to re-upload
   3. The manifest .flag file will be removed so that the manifest can be processed again in a future cron.
8. For each file:
   1. Depending on the type of file, it may be normalized (gzip or gunzip) so that all files copied or in a bundle are in the same compression state.
   2. The validated area path and released area path are both determined
9. For files that can be bundled:
   1. File "component" identifiers are created for each file entity.
   2. A bundle entity is created
   3. The bundle release path is determined.
      1. The bundle version is assigned depending on the presence of existing bundles in that release path.  This version counter is passed to each of the component files that make up the bundle, and their validated filepaths are altered to reflect this.  Files that are copied instead of bundled do not have incremented versions but are overwritten instead.
   4. The bundle file identifier is created.
   5. If this step fails, an email is sent to the NeMO team to look into the issue.
10. All files that were in the manifest are moved to their detemined "validated" area path, and their state set to "VALIDATED".
   1. If this step fails, an email is sent to the NeMO team to look into the issue.
11. All files that are eventually copied to the "release" area are assigned file identifiers.
12. Some information is written to file, which is then passed as input to "bundle_nemo_files.pl".  This script copies files to the release area or bundles them into a tarball using the grid.
   1. If this step fails, an email is sent to the NeMO team to look into the issue.
13. After a brief sleeping period (30 seconds) to let the filesystem sync up, all bundled files are set to "RELEASED".  Copied files are still in "VALIDATED" but could be set to "RELEASED" as a TODO
14. Identifier information for each bundle, component file, and copied file are loaded into MySQL
15. Identifier and release path (as https URI) per file are written to a new manifest, which is placed in the "finished_manifests" area
16. An email is sent to the contact email from the "valid manifest":
   1. Has the "finished_manifest" as an attachment.
   2. Also prints all files in the Aspera submission area that were not ingested due to not being in the manifest file.
17. The manifest from "valid_manifests" area is removed along with the .flag file.

## Periodically

1. Remove non-ingestable files from an Aspera area so they are not just taking up space (wipe the directory?)
   1. We will email the user of their non-ingestible files after successful ingestion to give them time to determine what to do with them (mark as junk, add to new manifest, potentially force us to have a new file entity class or regex pattern)

