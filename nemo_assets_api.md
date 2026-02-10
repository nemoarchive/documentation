# NeMO Assets API for retrieval of metadata
NeMO API  and landing pages enable users to access and download data related to grant, project, subject, samples (includes libraries and aliquots), collection (includes publication) and files.
Currently, all API endpoints and landing pages necessitate the use of NeMO identifiers for metadata entities to retrieve information. NeMO plans to facilitate data retrieval utilizing NIMP’s NHASH IDs soon. To access public landing pages and API data at https://assets.nemoarchive.org, login credentials are not needed. Only publically accessible metadata is shown on landing pages and APIs. 

[Swagger Documentation](https://app.swaggerhub.com/apis/UMIGS/Identifiers/1.2)

### Metadata entities
Identifiers at NeMO are divided into several groups to help users determine what an identifier is referring to. Current entities include:
`collection`, `file`, `library`, `sample`, `subject`, `project`, and `grant`.
### API URL:
`https://assets.nemoarchive.org/api/<metadata_entity>/<nemo_identifier>`

Replace <metadata_entity> with one of the metadata entities listed above.
Replace <nemo_identifier> with a valid nemo identifier for the metadata entity of interest.

### Landing page URL:
`https://assets.nemoarchive.org<metadata_entity>/<nemo_identifier>`

Replace <metadata_entity> with one of the metadata entities listed above.
Replace <nemo_identifier> with a valid nemo identifier for the metadata entity of interest.

### Examples of usage:
### Collection:
The collection landing page provides metadata about the datasets included in the collection. The page doesn’t display all files included in the collection, for display of all files please use the pagenated API end point.

Example: nemo:col-rmf5gdy - BICAN transcriptome collection

**Landing page:**
`https://assets.nemoarchive.org/collection/nemo:col-rmf5gdy`

**API endpoint:** 
`https://assets.nemoarchive.org/api/collection/nemo:col-rmf5gdy`	

**Paginated file endpoint for collection** (provides file endpoint URLs for the files associated with the collection)**:**
`https://assets.nemoarchive.org/api/collection/nemo:col-rmf5gdy/files?page=1&page_size=100`

NeMO Identifiers for some example collections: 

nemo:col-4hjv07d (BICAN dynamic multiome metacollection)

nemo:dat-ek5dbmu (BICCN publication collection)

nemo:col-2j1kset (BICCN collection)

### File

Example: nemo:fil-bkygq2h - BICAN fastq file

**Landing page:** `https://assets.nemoarchive.org/file/nemo:fil-bkygq2h`	

**API endpoint:** `https://assets.nemoarchive.org/api/file/nemo:fil-bkygq2h`	

NeMO Identifiers for some files:

nemo:aln-00i6u7v (BICCN alignment bam.tar file)

nemo:seq-mxsv0g2 (BICCN fastq.tar file)

nemo:alc-hmf2jpb (BICCN alignment bam component file)

### Sample
The metadata for various samples such as ROIs, tissues, dissociated_cell_samples, enriched_cell_samples, barcoded_cell_samples, amplified cDNA, libraries and library_aliquots can be retrieved using sample endpoint.

Example: nemo:lib-v2x1q6r - BICAN library_aliquot

**Landing page:** 
`https://assets.nemoarchive.org/sample/nemo:lib-v2x1q6r`	

**API endpoint:** 
`https://assets.nemoarchive.org/api/sample/nemo:lib-v2x1q6r`	

NeMO Identifiers for some samples:

nemo:smp-xgkecu9 (BICCN sample)

nemo:smp-qo4j90y (BICCN sample)


### Subject

Example: nemo:sbj-dq4ms3e (BICCN)

**Landing page:** `https://assets.nemoarchive.org/subject/nemo:sbj-dq4ms3e`	

**API endpoint:** `https://assets.nemoarchive.org/api/subject/nemo:sbj-dq4ms3e`	

NeMO Identifiers for some subjects:
nemo:sbj-njhfvw6

nemo:sbj-asemfa0

nemo:sbj-3e6ik4e


### Project
Example: nemo:std-zn3fi33 (BICCN project: zeng_sc_10x_proj)	

**Landing page:** `https://assets.nemoarchive.org/project/nemo:std-zn3fi33`	

**API endpoint:** `https://assets.nemoarchive.org/api/project/nemo:std-zn3fi33`	

NeMO Identifiers for some projects:

nemo:std-hoxfi7n

nemo:std-5jvcwm1

nemo:std-27tq25v
	
### Grant
Example: nemo:grn-37wairu	(BICCN grant)

**Landing page:** `https://assets.nemoarchive.org/grant/nemo:grn-37wairu`	

**API endpoint:** `https://assets.nemoarchive.org/api/grant/nemo:grn-37wairu`		




## Use Cases for using APIs

Use case 1: Retrieving files associated with the latest snapshot collection within a cumulative collection (eg: 10x v3.1 RNA Seq), followed by extraction of metadata (including the aliquot NHASH ID) for the files.

NeMO will provide the cumulative collection identifier to BCDC prior to a Rapid Release. BCDC will then use the collection API endpoint to retrieve NeMO identifier for snapshot collection associated with the latest Rapid Release. The following are the steps for retrieving the files for the snapshot collection, a child of cumulative collection.



Query collection API endpoint using cumulative collection identifier - nemo:col-afddrzj

**API endpoint:** `https://assets.nemoarchive.org/api/collection/nemo:col-afddrzj`	

**Landing page:** `https://assets.nemoarchive.org/collection/nemo:col-afddrzj`	

      	               ↓

Retrieve the recent static snapshot child collection API endpoint URL associated with the latest Rapid Release under the 'child_collections' field in the API output from the query above

**API endpoint:** `https://assets.nemoarchive.org/api/collection/nemo:col-7x7snh7`
 
       	              ↓

Query the child snapshot collection identifier using a paginated file endpoint.
This will provide the file API endpoint URLs for all the files associated with this collection.
`https://assets.nemoarchive.org/api/collection/nemo:col-7x7snh7/files?page=1&page_size=100`	
 	     
		              ↓

Use the file API endpoint URLs from the previous step to retrieve file metadata (using the first file endpoint URL from the previous output as an example). Some of the metadata in the file endpoint include the HTTPS location of the file, file nemo identifier and NHASH id for the parent library_aliquot ( example: name -NY-TX14001-1; nemo identifier -nemo:lib-v2x1q6r; NHASH id -LA-JFFKEH205984JOILGE819914)

**API endpoint:**

`https://assets.nemoarchive.org/api/file/nemo:fil-bkygq2h`

**Landing page:**

`https://assets.nemoarchive.org/file/nemo:fil-bkygq2h`		

------------

Use case 2: Retrieving files associated with a latest meta-snapshot collection from a meta-cumulative collection (eg: multiome) followed by extraction of metadata (including the aliquot NHASH ID) for the files.

NeMO will provide the meta-cumulative collection identifier to BCDC prior to a Rapid Release. BCDC will then use the collection API endpoint to retrieve the API endpoint URL for the meta-snapshot collection associated with the latest Rapid Release. The endpoint for the  meta-snapshot collection will return endpoint URLs for the two child snapshot collections containing ATAC and RNA datasets. The following are the steps for retrieving the files for ATAC and RNA datasets.

Query collection API endpoint using dynamic meta-cumulative collection identifier - nemo:col-iefmnby

**Landing page:** `https://assets.nemoarchive.org/collection/nemo:col-iefmnby`	

**API:** `https://assets.nemoarchive.org/api/collection/nemo:col-iefmnby`

     	            	 ↓
						 
Retrieve the most recent child static meta-snapshot collection API endpoint URLs from the latest Rapid Release under 'child_collections'.
`https://assets.nemoarchive.org/api/collection/nemo:col-myr9nn1`

                         ↓
	
Retrieve the snapshot collection API endpoint URLs for the ATAC and RNA datasets under 'child collections'
`https://assets.nemoarchive.org/api/collection/nemo:col-0onu0di`	(ATAC)

`https://assets.nemoarchive.org/api/collection/nemo:col-o29aa39`	(RNA)

      			        ↓

Query the snapshot collection identifiers using paginated file endpoints.
This will provide the file API endpoint URLs for all the files associated with this collection.

`https://assets.nemoarchive.org/api/collection/nemo:col-o29aa39/files?page=1&page_size=100`	(RNA)

`https://assets.nemoarchive.org/api/collection/nemo:col-0onu0di/files?page=1&page_size=100`	(ATAC)

 	                    ↓

Use the file API endpoint URLs from the previous step to retrieve file metadata (using the first file endpoint URL from the previous output as an example). Some of the metadata in the file endpoint include the HTTPS location of the file, file nemo identifier and NHASH id for the parent library_aliquot.
API endpoint:

`https://assets.nemoarchive.org/api/file/nemo:fil-0kv21as` (RNA)

`https://assets.nemoarchive.org/api/file/nemo:fil-25cr79p` (ATAC)

**Landing pages:**

`https://assets.nemoarchive.org/file/nemo:fil-0kv21as`	

`https://assets.nemoarchive.org/file/nemo:fil-25cr79p`	
	




