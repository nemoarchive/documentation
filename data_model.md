# Data Model

NeMO data and metadata are organized based on the data model shown below. Each of the properties associated with the entities are governed by metadata schemas that specify constraints on the values in the fields. This is an illustrative model, for specific fields check the metadata section.

![NeMO Data Model](images/nemo_data_portal/nemo-data-model.png)

A snapshot of the `biccn` project area is shown here:

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

Symlinks are used to organize data across these 4 top level directories, therefore the same data is accessible from each entry point, simply organized in different ways. For example, the lab directory data is organized next by modality, while the organism directory is organized next by grant. For the complete data structure, see Data Model (coming soon).
