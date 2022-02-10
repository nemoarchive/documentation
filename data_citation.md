# Data Citation

Storing and sharing data via NeMO Archive facilitates data tracking and citation by providing unique dataset identifiers. Unique identifiers align with FAIR Principles, as described in [Wilkinson et al. 2016](https://www.nature.com/articles/sdata201618), a growing effort to "improve the findability, accessibility, interoperability, and reuse of digital assets."


NeMO archive staff work with manuscript authors to organize and release the relevant data set(s) on our website, to make them  available to reviewers and readers via a [NeMO identifiers](https://assets.nemoarchive.org/) landing page. Data files are organized in [BDBags](https://bd2k.ini.usc.edu/tools/bdbag/), or Big Data Bags. A top level BDBag containing all manuscript-associated data can itself contain multiple smaller BDBags corresponding to raw versus processed (analysis) data, or to multiple modalities within the dataset. Each of these individual BDBags has its own unique identifier and its own landing page. This allows interested community users to download whatever granularity of your published dataset they wish, whether that's the entire dataset, or a smaller subset of the data. This is depicted here:


See screenshots from an actual BICCN dataset further down this page.


### Requesting a BDBag and Unique Identifier
Requesting a unique identifier for your dataset involves the following steps:

1) Submit your data to NeMO [as described here](https://github.com/nemoarchive/documentation/blob/master/submit_data.md), or [browse the NeMO archives](https://github.com/nemoarchive/documentation/blob/master/browse_portal.md) to ensure that your data has previously been submitted.
2) Reach out to NeMO Archives staff through our [Contact page](https://nemoarchive.org/contact.php)
3) Download and fill out the [metadata spreadsheet](https://docs.google.com/spreadsheets/d/1s7ZJaC9kYyyDnkYShRImmFXV11yxAO70mCV2ZhD1OvM/edit?usp=sharing). A NeMO Archives staff member will ask you to send this for accurate creation of your landing page. Complete and accurate metadata enhances data reuse and searchability.
4) NeMO Archives staff will provide a unique identifier(s) for citation in your manuscript, see next section.


### Citing your dataset

The NeMO Archives in conjunction with the BICCN endorse the following citation format:
The data analyzed in this study were produced through the Brain Initiative Cell Census Network (BICCN:RRID:SCR_015820) and deposited in the NEMO Archive (RRID:SCR_002001) under identifier `NeMO unique identifier` accessible at `NeMO identifier landing page URL`

Additional Resources are available here:
* [BICCN guide to data and resource citation](https://docs.google.com/document/d/10OCVrKwzPCgdEcmxF1Bf0hX02XpqjbZS3a-z37BbnWk/edit#heading=h.qur7g0r306i0)

More questions? [Contact the NeMO Archives team](https://nemoarchive.org/contact.php).


### Citing someone else's data

If you have used data downloaded from the NeMO Archives in your own publication, we ask that you reference the dataset via it's unique identifier.

### Example Dataset

The following is an example BDBag from the preprint [An integrated transcriptomic and epigenomic atlas of mouse primary motor cortex cell types](https://www.biorxiv.org/content/10.1101/2020.02.29.970558v2.full). This paper includes multiple data modalities. Working with the authors, we organized the data into 9 datasets and associated analysis files, accessible from [https://assets.nemoarchive.org/dat-ch1nqb7](https://assets.nemoarchive.org/dat-ch1nqb7).

![Landing_Page](https://user-images.githubusercontent.com/13540148/79372611-f3421a80-7f23-11ea-97fe-b73812746743.png)



The main landing page provides information about the manuscript and an overall snapshot of the data. The Dataset Collection URL allows interested users to download the entire bolus of data included in the publication. This downloads as a BDBag, which must be opened and explored using [BDBag software](https://bd2k.ini.usc.edu/tools/bdbag/). BDBags and BDBag manipulation are [described in more detail here](to be added).

In the upper right corner  are tabs for `raw` and `analysis` files. Clicking the `raw` tab takes the user to the following page, organized by modality and data generation lab.

![Raw_landing_page](https://user-images.githubusercontent.com/13540148/79373826-f984c680-7f24-11ea-9f6c-ccb510980232.png)

Each entry on this page can be expanded to show the unique identifier for that subset of data, a short description and a source data URL link to a manifest file listing the contents of the dataset.

![dropdown](https://user-images.githubusercontent.com/13540148/79374549-64ce9880-7f25-11ea-8e0d-365683b09f4a.png)


Clicking on the "Identifier" link takes the user to a new landing page for that data subset only. Here, users can download the data subset via the Dataset Collection URL, or the source data URL manifest file. The latter facilitates data download using the [HTTP Data Browser](https://github.com/nemoarchive/documentation/blob/master/browse_http.md).

The Dataset Collection URL downloads data in the form of a BDBag, which must be opened and explored using [BDBag software](https://bd2k.ini.usc.edu/tools/bdbag/). BDBags and BDBag manipulation are [described in more detail here](to be added).



![Directory Example](https://user-images.githubusercontent.com/13540148/79375405-9300a800-7f26-11ea-9d41-3d2844d27065.png)
