# Accessing Controlled Access Data

In addition to the public data available via the NeMO HTTP Browser and the NeMO Data Portal, the archives also host a number of restricted, or controlled access, datasets. Here you will find available datasets, instructions for requesting access, and preliminary download account instructions.

## Table of Contents
1. [Restricted Datasets](#restricted-datasets)
2. [Obtaining NDA Approval](#obtaining-nda-approval)
3. [Accessing Data through Aspera](#accessing-data-through-aspera)
4. [Accessing Data through Google Cloud Platform - Coming Soon](#accessing-data-through-google-cloud-platform)
&nbsp;  
&nbsp;  
&nbsp;  
&nbsp;  
&nbsp;  
&nbsp;  
## Restricted Datasets
The following datasets will be the initial sets made available for controlled access. Additional datasets will be added as we receive Institutional Certification documents. If you think your dataset is missing, please contact us via the [NeMO Website feedback form](https://nemoarchive.org/contact.php). 

| Dataset | PI | Description | Funding Source | Data Use Limitations |
|---------|----|-------------|----------------|----------------------|
|Shen_3D_epigenome | Yin Shen | Cell type-specific 3D epigenomes in the developing human cortex: Here, we analyze cis-regulatory chromatin interactions, open chromatin peaks, and transcriptomes for radial glia, intermediate progenitor cells, excitatory neurons, and interneurons isolated from mid-gestational human cortex samples. |     | Limited by terms of the Data Use Certification,  requestor must provide documentation of local IRB approval     |
|kriegstein_sc_10x| Arnold Kriegstein | A Cellular Resolution Census of the Developing Human Brain. This project aims to create a spatiotemporal single cell resolution map of the developing human neocortex to establish how many distinct cell types are present and to unravel their complex developmental history. | BICCN | Limited to health, medical, biomedical purposes, use of data is limited to not-for-profit organizations.   |
|lein_lein_pseq_tx | Ed Lein | A multimodal atlas of human brain cell types: Transcriptomic profiles of individual human cortical neurons were assayed by the SMART-seq v4 single nucleus RNA-seq method following acute brain slice patch-clamp electrophysiology recordings and nucleus extraction—i.e. the Patch-seq method.  Surgically-resected brain tissue specimens were collected with patient consent and in the course of neurosurgical procedures for tumor removal or for treatment of epilepsy.  De-identified patient metadata were also collected and accompany the dataset comprised of 318 human cortical neurons from the temporal>frontal>parietal lobes of 56 unique donors. | BICCN | Limited to neurological disorders |   
&nbsp;  
&nbsp;    
&nbsp;  
&nbsp;  
&nbsp;  
&nbsp;  
## Obtaining NDA Approval

Permissions for restricted data access at NeMO are being facilitated by the [NIMH Data Archive (NDA)](https://nda.nih.gov/). NDA and NeMO are working together to ensure a smooth process. NDA provides an [SOP for institutionally sponsored data access requests here](https://nda.nih.gov/about/standard-operating-procedures.html#sop4), however this page outlines the steps required for **NeMO-specific** restricted data access. Users having previously been granted dbGap access for NeMO associated datasets will be contacted individually by NeMO, and can proceed to [creation of an institutional google account](#accessing-data-through-google-cloud-platform). 

### 1) Create an NDA account
[Create an NDA account here](https://nda.nih.gov/user/create_account.html). **NDA ACCOUNT REQUESTS MUST BE MADE USING AN INSTITUTIONAL EMAIL ADDRESS**. Account requests made from a personal account will not be honored by NeMO and will therefore greatly slow down the process of accessing data. 

<img src="https://github.com/nemoarchive/documentation/blob/master/images/NDA_requestacct.png" alt="NDA request account" width=35% align=center> <img src="https://github.com/nemoarchive/documentation/blob/master/images/nda_createacct.png" alt="NDA create account" width=35% align=center>

### 2) Identify Datasets through the NDA Dashboard
Log in to the [NDA Permissions Dashboard](https://nda.nih.gov/user/dashboard/data_permissions.html). Here you can identify relevant BRAIN/NeMO Data Archives dataset(s) within the NDA Controlled Access Permission Group table of the dashboard. To the right is an 'Actions' dropdown. Select "Request Access".  
**You must work at a research institution that has an active [Federal-Wide Assurance](https://www.hhs.gov/ohrp/federalwide-assurances-fwas.html) in order to initiate a data access request.** 

<img src="https://github.com/nemoarchive/documentation/blob/master/images/nda_permission_grps.jpeg" alt="NDA permissions groups">

### 3) Data Access Request Tool   
This will open the Data Access Request Tool where you will enter a) the title of your research and Research Data Use Statement, b) the Signing Official at your research institution, and c) contact information for you and your collaborators. Please carefully review the following screenshots with directions for properly filling out all sections of the request tool.

#### Request Access Instructions Page:
<img src="https://github.com/nemoarchive/documentation/blob/master/images/NDA/req_access.jpeg" alt="NDA request access">

#### Research Data Use Statement:  
Data access requests for controlled access permission groups should include a Research Data Use Statement that appropriately addresses consent-based data use limitations for that permission group.  All BRAIN/NeMO datasets are listed in the NDA Controlled Access Permission Group table.  Look at the “Data Use Limitations” field to determine if there are consent-based data use limitations to which authorized researchers must adhere.  

<img src="https://github.com/nemoarchive/documentation/blob/master/images/NDA/req_details.jpeg" alt="NDA request access details">

#### Authorized Research Institute:  
**You must work at a research institution that has an active [Federal-Wide Assurance](https://www.hhs.gov/ohrp/federalwide-assurances-fwas.html) in order to initiate a data access request.** The signing official(s) associated with your institution will automatically appear as a selectable option.  

<img src="https://github.com/nemoarchive/documentation/blob/master/images/NDA/auth_inst.jpeg" alt="NDA authorized institute">

#### Other Access Recipients:
**Each data access application should be restricted to users from a single institution.** If you have collaborators at other organizations, they must submit a separate data access application, **WITH THE FOLLOWING IMPORTANT EXCEPTION:**  
**Contact information MUST include the following NeMO account in order to ensure that NeMO receives notification of your access approval:** Heather Creasy, hhuot@som.umaryland.edu. You can add this by using the 'Search for users outside your institution' function.  

<img src="https://github.com/nemoarchive/documentation/blob/master/images/NDA/other_recip.jpeg" alt="NDA other access recipients">

### 4) Download Data Use Certificate (DUC)
Download and sign the Data Use Certification PDF from the Data Access Request Tool and complete with signatures by both investigator and institutional Signing Official. [Contact the NDA Help Desk](mailto:ndahelp@mail.nih.gov) if you need assistance identifying Signing Officials at your research institution.  

<img src="https://github.com/nemoarchive/documentation/blob/master/images/NDA/nda_agreement.jpeg" alt="NDA agreement">  
<img src="https://github.com/nemoarchive/documentation/blob/master/images/NDA/DUC_saved.jpg" alt="DUC saved">

### 5) Upload signed DUC 
Log into the NDA Permissions Dashboard and upload the signed DUC on the “Active Requests” section on the top of the NDA Permissions Dashboard.  

<img src="https://github.com/nemoarchive/documentation/blob/master/images/NDA/NDA_active_request.jpg" alt="Active requests">  
<img src="https://github.com/nemoarchive/documentation/blob/master/images/NDA/NDA_Upload_DUC.jpg" alt="Upload DUC">  
<img src="https://github.com/nemoarchive/documentation/blob/master/images/NDA/NAR_upload_DUC_agreemt.jpg" alt="Upload DUC agreement">

### 6)	Your data access request will be reviewed by an NIH Data Access Committee (DAC).  

### 7) Access request decision
NDA will inform investigators and NeMO of a final access decision, at which time NeMO will reach out to all investigators and collaborators included on the access request. It is critical that the NeMO account indicated above be included in your original DUC submission in order for NeMO to receive notification of your request approval. NeMO will grant data access to investigator for ***one year***, after which investigators will need to reapply for access using the process described above.  
&nbsp;  
&nbsp;  
&nbsp;  
&nbsp;  
&nbsp;  
&nbsp;  
## Accessing Data through Aspera

Consented data will soon be available from NeMO through Google Cloud Platform (gcp), see instructions below. However, in the interim, we are providing data through Aspera as a service to those who have already been granted access. 

Complete Aspera instructions are available under [NeMO Resources](https://nemoarchive.org/resources/aspera/). However, here we provide all the steps you will need to download restricted datasets. If you already have Aspera CLI instaklled, skip to step 3.

### 1) Downloading Aspera Command Line Interface (CLI)

Go to the [IBM Aspera website](https://www.ibm.com/products/aspera/downloads). Scroll to Featured Client Software, and select the Aspera Command Line Interface Client > Download now. Select the most recent release for your operating system. 

At this point, you will either need to log into your IBMid account, or set up a free account if you have not previously done so. 

Choose your method of download. Make sure that the box next to "Include prerequisites and co-requisite fixes" IS checked. Follow download instructions. 

### 2) Installing Aspera CLI

Run the installation script:
`$ sh aspera-cli-x.x.x.xxx.xxxxxxx-mac-xx.x-64-release.sh`

The script places the Aspera CLI in the $HOME/Applications/Aspera CLI directory. To install the Aspera CLI in your PATH, run the following command:
`$ export PATH=~/Applications/Aspera\ CLI/bin:$PATH`

Once installed, you should have the ascp utility available to you.


&nbsp;  
&nbsp;  
&nbsp;  
&nbsp;  
&nbsp;  
&nbsp;  
## Accessing Data through Google Cloud Platform

### 1) Creating an institutional Google account

Consented data will be available from NeMO through Google Cloud Platform (gcp). ***Please read the following carefully:***

Any consented user wishing to download data must create a google account at [myaccount.google.com/](https://myaccount.google.com/), **USING THE SAME INSTITUTIONAL EMAIL ADDRESS USED FOR NDA ACCOUNT CREATION**. 

If you are **not** currently logged in as a google user, select "Create a Google Account" at the bottom of the screen. If you **are** currently logged in as a google user, you can select "Use Another Account" and then "Create Account". When creating an account, you should see an option to use a current email address instead of creating an @gmail.com address. Be sure to select this and sign up with your institutional email address. Following remaining prompts to create your account.

<img src="https://github.com/nemoarchive/documentation/blob/master/images/google_newuser.png" alt="google new user account set up" width=35% align=center> <img src="https://github.com/nemoarchive/documentation/blob/master/images/google_currentuser.png" alt="google current user new account set up" width=35% align=center>
<img src="https://github.com/nemoarchive/documentation/blob/master/images/google_curremail.png" alt="google account add email" width=35% align=center> <img src="https://github.com/nemoarchive/documentation/blob/master/images/google_instacct.png" alt="google account institutional email" width=35% align=center>


### 2) Downloading Data

Documentation on accessing data through google cloud platform is coming soon.

