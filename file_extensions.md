The NeMO ingest scripts allow the file extensions listed below. By default we ignore extensions not on this list. If you have an extension not shown here that you would like us to process for release, please let us know as soon as possible, by emailing nemo@som.umaryland.edu.

## Manifest file

* manifest.csv
* manifest.tab
* manifest.tsv
* manifest.txt (as tab-delimited)

This will be converted to TSV no matter which format is chosen for the initial manifest.

## BAM

* .bam
* .bam.bai  (index file)

## BED
* .bed
* .bed.gz

The ingest will gunzip the file if gzipped.  This is to keep consistent with BigBed and BigWig files

## BIGBED

* .bb
* .bigbed
* .bigBed

## BIGWIG

* .bw
* .bigwed
* .bigWig

## CSV

* .csv
* .csv.gz

The ingest will gzip the file if not already gzipped

## FASTQ

* .fastq
* .fastq.gz
* .fq.gz

The ingest will gzip the file(s) not already gzipped

### FASTQ patterns in middle of file name

* \_R1
* .R1
* .read1

* \_R2
* .R2
* .read2

* \_R3
* .R3
* .read3

* \_I1
* .I1

* \_I2
* .I2

## FPKM

* barcodes.fpkm_tracking
* genes.fpkm_tracking

# H5

* .h5

## H5AD

* .h5ad
* .json

## MEX

* .mtx
* .mtx.gz
* .barcodes.tsv
* .barcodes.tsv.gz
* .features.tsv  (One of 'features' or 'genes' must be present)
* .features.tsv.gz
* .genes.tsv
* .genes.tsv.gz

The ingest will gunzip the file(s) if gzipped.  This is to gzip the MEX tarball when ingested
Currently, ingest requires unique files names, so each of the above must be prefixed with a sampleID. This sampleID will be stripped during processing and applied to the mex bundle. In other words, when a user gunzips and untars sampleID.mex.tar.gz, the result the result is a folder named 'sampleID' containing matrix.mtx.gz, features.tsv.gz, and barcodes.tsv.gz files. For barcodes, feature, or genes files that are a part of MEX format, those files should be labeled as mtx format in the file type column of the mainfest.

## SNAP

* .snap

## TAB Analysis

* \_COLmeta_DIMRED\_ in middle of name and .tab at end
* \_ROWmeta_DIMRED\_ in middle of name and .tab at end
* \_DIMREDmeta\_ in middle of name and .tab at end

## TAB Counts

* COLMeta.tab
* ROWMeta.tab
* DataMTX.tab
* EXPMeta.json

## TSV

* .tsv
* .tsv.idx (index file)
* .tsv.tbi (index file)
* .tsv.gz
* .tsv.gz.idx (index file)
* .tsv.gz.tbi (index file)

The ingest will gzip the file(s) if not already gzipped

## The following, if encountered, will be extracted to ingest contents within

* tar.bz2
* tar.gz
