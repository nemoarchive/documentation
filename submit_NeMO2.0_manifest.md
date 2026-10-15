# Steps for submitting NeMO 2.0 manifest


Below is the step-by-step process for submitting data to NeMO.

## 1. Complete the onboarding process

The submitter will need to complete the following forms:
* [Data Submitter Registration : Submitters Only](https://nemoarchive.org/register)  (please use a username ending with "-restricted", for example, *xxx-restricted*).
* [New NeMO User Registration : All Users](https://docs.google.com/forms/d/e/1FAIpQLSeetSCQl3E1D_nB-p_ZV0S7o30ZbmtrLQWkhk2UlznO1ILHuA/viewform)
  
Once we review and approve the forms, we will configure the account for data submission and notify the submitter when they can proceed with manifest submission.<br><br>

## 2. Prepare the manifest

(Give your best shot in filling the manifest and send the manifest to NeMO. We will review and make changes if needed. We are there to help you!!)

Please DO NOT include files with mixed access in the same manifest (Eg: open, open_embargo, restricted_embargo, restricted). Each submission should have files that belong to the same access.

While the account is being configured, the submitter can prepare the manifest in Excel using the appropriate template:

   * BICAN data: Use the [BICAN template](https://docs.google.com/spreadsheets/d/1lGO950PJRNtadjDJpD3x7zYBU8ehQPjw/edit?usp=sharing&ouid=109292309009829585903&rtpof=true&sd=true)
   * BICCN data: Use the [BICCN template](https://docs.google.com/spreadsheets/d/1RAU22FmMkEodfjFTHAsc9R0LM331f39-/edit?usp=sharing&ouid=109292309009829585903&rtpof=true&sd=true)
  
If you are submitting analysis files, the parent files (FASTQ files, with or without BAM files) must be submitted first.

* If the parent files have already been submitted: You only need to complete the *File* sheet. The *Subject*, *Sample*, and *Library* sheets can be left blank. Be sure to populate the **file_derived_from** column with the parent FASTQ filenames, separated by semicolons.
  
* If the parent files have not been submitted: You will need to submit two manifests
    * One for the **FASTQ** files
    * One for the **analysis** files (BAM files, if applicable, and other derived files)<br><br>

   **For the FASTQ manifest** :
   * Complete the *Subject*, *Sample*, *Library*, and *File* sheets.
   * Fill in the *File Attributes* sheet with a collection short name (lab can suggest a name, we will add it to our CV list prior to manifest submission, same is true with project short name).
   * The *Subject Attributes*, *Sample Attributes*, and *Library Attributes* sheets are optional.<br><br>
 
After the FASTQ manifest has been successfully submitted and the FASTQ files have been uploaded, you can submit the analysis manifest. For the analysis manifest, only the *File* sheet is required.

All controlled vocabulary (CV) terms needed for filling the manifests are listed in CVs sheet. If a term is missing in our CVs, we can add them in our list.

For reference, here is a link to a successfully validated [BICAN analysis manifest from the Cohen lab](https://docs.google.com/spreadsheets/d/1v-dktQZYV7TIuwiFapHQgtHyxJVt5Gon/edit?usp=sharing&ouid=106143818500123084294&rtpof=true&sd=true). In that manifest, the **file_derived_from** field references a text file containing the associated FASTQ filenames for each analysis file, as there were too many filenames to fit in a single cell.<br><br>

## 3. Submit the manifest

After you receive confirmation from us that you can begin submissions: 

* Log in to the [NeMO dashboard](https://nemoarchive.org/login) using your registered username and password
* Click *Dashboard* from the top menu
* Click *Make a Submission*
* Select your manifest
* We recommend selecting the **Dry Run** option first
* Click **Submit** <br><br>

Once validation is complete, you will receive an automated email.
* If there are validation errors in the email, correct the manifest and run another **Dry Run** until it validates successfully.
* Once the manifest passes validation, submit it again without selecting Dry Run.<br><br>

## 4. Upload the files

After the manifest is successfully submitted, you will receive an automated email containing an Aspera upload command.

Before using the command, install the Aspera CLI by following the [Aspera CLI installation](https://nam11.safelinks.protection.outlook.com/?url=https%3A%2F%2Fnemoarchive.org%2Fresources%2Fdata-submission%23uploading-using-aspera&data=05%7C02%7Csnadendla%40som.umaryland.edu%7Cc0466da049094888147d08dee7986b27%7C717009a620de461a88940312a395cac9%7C0%7C0%7C639202839074438397%7CUnknown%7CTWFpbGZsb3d8eyJFbXB0eU1hcGkiOnRydWUsIlYiOiIwLjAuMDAwMCIsIlAiOiJXaW4zMiIsIkFOIjoiTWFpbCIsIldUIjoyfQ%3D%3D%7C0%7C%7C%7C&sdata=mHykstsZ8Un%2F3yStUQReSh%2BpG9WsHEAus20eYrMxD0w%3D&reserved=0) instructions.

Once Aspera CLI is installed, use the command from the email to upload your files. See an example command below.

    ascp -l 1000M -k 2 -QT /home/user/my_data/my_data.fastq.gz user123@aspera.nemoarchive.org:v34kltf7/


## 5. Upload completion

After all the files have been uploaded successfully, upload a 0-byte file named **DONE** (with no file extension). This file signals that the upload is complete and starts the downstream QA process.<br><br>

## 6. NeMO processing

Once your upload is complete, your part is done. The NeMO team will perform QA, process the submission, and prepare the data for release.






