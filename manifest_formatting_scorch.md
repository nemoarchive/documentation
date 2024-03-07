# SCORCH Manifest Formatting
To ensure files are associated with the correct metadata, SCORCH manifest submissions consist of several parts:
- Subject metadata
- Sample metadata
- Library information
- File-level information

Currently, SCORCH manifests are submitted as an Excel document (.xlsx) with four tabs (future versions of the SCORCH manifest will use JSON format). 
Each tab contains one level of metadata and blank templates for human and nonhuman submissions can be found here ([human](), [nonhuman]()). 
Each row in the spreadsheet is a single record and required metadata fields are bolded. You can also descriptions (and controlled vocabularies, if applicable) for each field [here](). 

## Cohort/project information
To ensure that files associated with a subject are assigned to the correct data access groups, every subject in SCORCH will be assigned to a cohort. 
For human subjects, these cohorts match the restrictions from your approved Institutional Consent (IC) forms (i.e. samples from one tissue bank or consent group will be a part of a different cohort than samples from a second tissue bank).
For nonhuman subjects, every subject from within a grant will be a part of the same cohort as they all have the same use restrictions. 
The list of current SCORCH cohorts can be found [here](), if you don't see a matching cohort for the data you are submitting, please use the form [here](https://github.com/nemoarchive/helpdesk/issues/new/choose).

To better organize data within the consortium, SCORCH files submitted to NeMO will all be a part of a project (i.e. a subgrouping of data from a single grant with a similar modality). 
By default, SCORCH project names have 3 parts: scorch_, the contact PIs last name, and the type of data that makes up that project (e.g. multimodal or transcriptome). If you would like changes to your project names or need a new project added, please use the form [here](https://github.com/nemoarchive/helpdesk/issues/new/choose)

The current list of SCORCH projects can be found [here]()
# Subject level information
## Human subject information
We are including much more subject level metadata information in the human manifests than in previous versions. For convenience, the NeMO data center will work with tissue banks directly to collect and format human metadata information for the SCORCH consortium. 
To receive access to the pre-formatted SCORCH subject metadata to put in your manifest, contact Joe Receveur (jreceveur@som.umaryland.edu). If you are planning on submitting tissue from subjects not from NNTC or Dr. Mash/NSU please let us know in advance: the tissue bank, subject IDs, and a contact individual at the tissue bank.

## Nonhuman subject information
Descriptions and controlled vocabularies for nonhuman subject level metadata fields can be found [here](). If you need additional controlled vocabulary terms added, please use the form [here](https://github.com/nemoarchive/helpdesk/issues/new/choose)


# Sample level information
At the sample level (i.e. a piece of biological tissue), we collect information about: the subject associated with the sample, the anatomical site from which the sample was collected, any applicable information about parent samples/composite sample, and the type of sample.
For anatomical site, we are asking all SCORCH groups to provide UBERON IDs. To lookup the ontology ID associated with a particular anatomical site you can use the lookup [here](https://www.ebi.ac.uk/ols4/ontologies/uberon).

# Library level information
To maintain a uniform convention across NeMO for what constitutes a library, libraries are treated as arising from a single technique. For example, a piece of biologial tissue (i.e. a sample) used for two different techniques would have two libraries associated with that sample. 

To account for resequencing of the same libraries (e.g. a top-up sequencing or other occurence where the same library was sequenced twice at different times) we use the concept of library aliquot. 
As defined for NeMO Archive, a library aliquot consists of one loading of a library onto a sequencer. 
While the same library can be sequenced repeatedly at different times, each instance would be a seperate aliquot. 
For example: A library is prepared and sequenced once, that data would be associated with the first library aliquot (e.g. LIB001_1). 
Upon analysis, it was discovered that the sequencing depth was too low so that same library preperation was sequenced a second time and would be associated with another aliquot (e.g. LIB001_2).

# File level information
File level metadata includes information about the individual files to be submitted. If a file is present in the manifest, it must be uploaded during the submission process or ingest will fail. 

**Note for techniques**: Manifest validation checks that for a given technique, we receive all the necessiary files. For example if a technique is selected for which we expect a forward read, reverse read, and an index, and those data subtypes are not included for a particular library aliquot, the validation will fail. 
## Example manifests
