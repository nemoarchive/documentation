# HTTP Data Browsing

An HTTP server-based browser is available at [data.nemoarchive.org/](http://data.nemoarchive.org/). This document describes directory organization within the NeMO HTTP data browser.  
&nbsp;  

<img src="https://github.com/nemoarchive/documentation/blob/master/images/HTTP_browser.png" width=35% align=center>

The top, or root, level of the HTTP browser separates data based on project or release: 
* projects:
 * `biccc/` contains data generated within the BRAIN Initiative Cell Census Consortium, precursor to the BICCN
 * `biccn/` contains data generated as part of the ongoing [BRAIN Initiative Cell Census Network](https://biccn.org/)
 * `other/` In addition to hosting BRAIN Initiative data, the NeMO repository also hosts 'omics data from other neuroscience projects. Contact us if you would like to discuss submission of your dataset(s) to the NeMO Archives.
* releases:
 * `nemo_release/` contains a [bdbag] (link tbd) corresponding to each quarterly NeMO release. NeMO releases are synchronized to the same release schedule as the BCDC release schedule. These bags contain BICCN data only. 
 * `publication_release/` contains one or more [bdbags] (link tbd) corresponding to dataset(s) analyzed for BICCN-associated publications. See Data citation for more information.

Within each project, data is organized by 1) grant, 2) lab where data was generated, 3) organism, or 4) assay type. An example of the biccn project area is shown here:

```
biccn
├── assay
│   ├── chromatin
│   ├── methylation
│   └── transcriptome
├── grant
│   ├── cemba
│   ├── devhu
│   ├── feng
│   ├── huang 
│   ├── lein
│   ├── zeng
│   └── zhang
├── lab
│   ├── anderson
│   ├── arlotta
│   ├── callaway
│   ├── ecker
│   ├── feng
│   ├── kriegstein
│   ├── lein
│   ├── linnarsson
│   ├── macosko
│   ├── regev
│   ├── tolias
│   └── zeng
└── organism
    ├── human
    ├── marmoset
    └── mouse
```

Symlinks are used to organize data across these 4 top level directories, therefore the same data is accessible from each entry point, simply organized in different ways. For example, the lab directory data is organized next by modality, while the organism directory is organized next by grant.

Restricted data is not yet available via the HTTP Browser.

Data can be downloaded from the NeMO HTTP Browser using any tools that support http downloads.


