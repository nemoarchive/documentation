# NeMO Assets API and Landing Pages
NeMO API  and landing pages enable users to access and download data related to grant, project, subject, samples (includes libraries and aliquots), collection (includes publication) and files.
Currently, all API endpoints and landing pages necessitate the use of NeMO identifiers for metadata entities to retrieve information. NeMO plans to facilitate data retrieval utilizing NIMP’s NHASH IDs soon. To access public landing pages and API data at https://assets.nemoarchive.org, login credentials are not needed. Only publically accessible metadata is shown on landing pages and APIs. 


NeMO Identifier API: REST API to retrieve information about the data being represented by a NeMO identifier.
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
#### Collection:
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

#### File

Example: nemo:fil-bkygq2h - BICAN fastq file

**Landing page:** `https://assets.nemoarchive.org/file/nemo:fil-bkygq2h`	

**API endpoint:** `https://assets.nemoarchive.org/api/file/nemo:fil-bkygq2h`	

NeMO Identifiers for some files:

nemo:aln-00i6u7v (BICCN alignment bam.tar file)

nemo:seq-mxsv0g2 (BICCN fastq.tar file)

nemo:alc-hmf2jpb (BICCN alignment bam component file)

#### Sample
The metadata for various samples such as ROIs, tissues, dissociated_cell_samples, enriched_cell_samples, barcoded_cell_samples, amplified cDNA, libraries and library_aliquots can be retrieved using sample endpoint.

Example: nemo:lib-v2x1q6r - BICAN library_aliquot

**Landing page:** 
`https://assets.nemoarchive.org/sample/nemo:lib-v2x1q6r`	

**API endpoint:** 
`https://assets.nemoarchive.org/api/sample/nemo:lib-v2x1q6r`	

NeMO Identifiers for some samples:

nemo:smp-xgkecu9 (BICCN sample)

nemo:smp-qo4j90y (BICCN sample)


#### Subject

Example: nemo:sbj-dq4ms3e (BICCN)

**Landing page:** `https://assets.nemoarchive.org/subject/nemo:sbj-dq4ms3e`	
**API endpoint:** `https://assets.nemoarchive.org/api/subject/nemo:sbj-dq4ms3e`	

NeMO Identifiers for some subjects:
nemo:sbj-njhfvw6

nemo:sbj-asemfa0

nemo:sbj-3e6ik4e


#### Project
Example: nemo:std-zn3fi33 (BICCN project: zeng_sc_10x_proj)	

**Landing page:** `https://assets.nemoarchive.org/project/nemo:std-zn3fi33`		
**API endpoint:** `https://assets.nemoarchive.org/api/project/nemo:std-zn3fi33`	

NeMO Identifiers for some projects:

nemo:std-hoxfi7n

nemo:std-5jvcwm1

nemo:std-27tq25v
	
#### Grant
Example: nemo:grn-37wairu	(BICCN grant)

**Landing page:** `https://assets.nemoarchive.org/grant/nemo:grn-37wairu`		
**API endpoint:** `https://assets.nemoarchive.org/api/grant/nemo:grn-37wairu`		



### Use Cases:

Retrieving the files associated with a BICAN dynamic collection (eg: 10x v3.1 RNA Seq), followed by the extraction of metadata (including the NHASH ID) for the parent library aliquots corresponding to those files.
NeMO will provide the dynamic collection identifier to BCDC prior to a Rapid Release. BCDC will then use the collection API endpoint to retrieve the NeMO identifier for the most recent static child collection associated with the latest Rapid Release. Although the current example of the dynamic collection does not yet include a child static collection, it is expected that there will be one static collection for each Rapid Release moving forward. The following is the flow of steps for retrieving the files for a collection:



Query collection API endpoint using dynamic collection identifier - nemo:col-rmf5gdy
**API endpoint: ** `https://assets.nemoarchive.org/api/collection/nemo:col-rmf5gdy`		
**Landing page: ** `https://assets.nemoarchive.org/collection/nemo:col-rmf5gdy`	

Retrieve the recent static child collection API endpoint URL associated with the latest Rapid Release under the 'child_collections' field in the API output from the query above (currently the static child collection is not available)
      	  ↓

Retrieve the metadata associated with the child collection using the API endpoint URL from above (currently the static child collection is not available)
       	 ↓

Query the child collection identifier using a paginated file endpoint.
This will provide the file API endpoint URLs for all the files associated with this collection (for example purposes using the dynamic collection identifier in the paginated endpoint query).
https://assets.nemoarchive.org/api/collection/nemo:col-rmf5gdy/files?page=1&page_size=100	
 	↓

Use the file API endpoint URLs from the previous step to retrieve file metadata (using the first file endpoint URL from the previous output as an example). Some of the metadata in the file endpoint include the HTTPS location of the file and nemo identifier for the parent library_aliquot for the file ( example: NY-TX14001-1 - nemo:lib-v2x1q6r)
API endpoint:
https://assets.nemoarchive.org/api/file/nemo:fil-bkygq2h
Landing page:
https://assets.nemoarchive.org/file/nemo:fil-bkygq2h		
↓

Use the sample API endpoint to retrieve metadata associated with the library aliquot retrieved by using the nemo identifier for the aliquot from the step above.
(NHASH ID can be obtained from the “alternate_id” field in sample endpoint)
API endpoint:
https://assets.nemoarchive.org/api/sample/nemo:lib-v2x1q6r	
Landing page: 
https://assets.nemoarchive.org/sample/nemo:lib-v2x1q6r	

Retrieving the files associated with a BICAN dynamic metacollection (eg: multiome), followed by the extraction of metadata (including the NHASH ID) for the parent library aliquots corresponding to those files.
NeMO will provide the dynamic metacollection identifier to BCDC prior to a Rapid Release. BCDC will then use the collection API endpoint to retrieve the API endpoint URLs for the most recent static child metacollection associated with the latest Rapid Release. Although the current example of the dynamic metacollection does not yet include a child static metacollection, it is expected that there will be one static metacollection for each Rapid Release moving forward. The endpoint for the static metacollection will return endpoint URLs for the two child collections containing ATAC and RNA datasets. The following is the flow of steps for retrieving the files for a metacollection:

Query collection API endpoint using dynamic metacollection identifier - nemo:col-4hjv07d	
Landing page: https://assets.nemoarchive.org/collection/nemo:col-4hjv07d	
API : https://assets.nemoarchive.org/api/collection/nemo:col-4hjv07d
                            	       	 ↓
	
Retrieve the most recent static child collection API endpoint URLs for the ATAC and RNA datasets associated with the latest Rapid Release under the 'child_collections' field in the API output from the query above.
https://assets.nemoarchive.org/api/collection/nemo:col-64xrgxd	(RNA)
https://assets.nemoarchive.org/api/collection/nemo:col-5grzfnz	(ATAC)
      			 ↓

Retrieve the metadata associated with each of the child collections using the API endpoint URLs from above.
API endpoints:
https://assets.nemoarchive.org/api/collection/nemo:col-64xrgxd (RNA)	
https://assets.nemoarchive.org/api/collection/nemo:col-5grzfnz (ATAC)	
Landing pages: https://assets.nemoarchive.org/collection/nemo:col-64xrgxd	
https://assets.nemoarchive.org/collection/nemo:col-5grzfnz	
       	 ↓

Query the child collection identifiers using paginated file endpoints.
This will provide the file API endpoint URLs for all the files associated with this collection.
https://assets.nemoarchive.org/api/collection/nemo:col-64xrgxd/files?page=1&page_size=100	(RNA)
https://assets.nemoarchive.org/api/collection/nemo:col-5grzfnz/files?page=1&page_size=100	(ATAC)
 	↓

Use the file API endpoint URLs from the previous step to retrieve file metadata (using first file endpoint URL from the previous output as an example). Some of the metadata in the file endpoint include the HTTPS location of the file and nemo identifier for the parent library_aliquot for the file.
API endpoint:
https://assets.nemoarchive.org/api/file/nemo:fil-0kv21as (RNA)
https://assets.nemoarchive.org/api/file/nemo:fil-25cr79p (ATAC)
Landing pages:
https://assets.nemoarchive.org/file/nemo:fil-0kv21as	
https://assets.nemoarchive.org/file/nemo:fil-25cr79p	
↓

Use the sample API endpoint to retrieve metadata associated with the library aliquot retrieved by using the nemo identifier for the aliquot from the step above.
(NHASH ID can be obtained from the “alternate_id” field in sample endpoint)
API endpoints:
https://assets.nemoarchive.org/api/sample/nemo:lib-tw3jgvv	
(ATAC aliquot NY-AT16001-1)
https://assets.nemoarchive.org/api/sample/nemo:lib-iajck4t	
(RNA aliquot NY-MX12001-1)
Landing pages:
https://assets.nemoarchive.org/sample/nemo:lib-tw3jgvv	
https://assets.nemoarchive.org/sample/nemo:lib-iajck4t	




