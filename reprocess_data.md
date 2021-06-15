Reprocessing data for comunity use

NeMO Archive is actively systematically reprocessing BICCN single cell raw data and making the resulting counts-level files availble to the community. We are using standard pipelines identified as industry standards and/or curated by the BICCN analysis working group. 

List of pipelines used: 

Cellranger 5 for 10X scRNAseq

Human example call:

cellranger count --id=10X243-8 --fastqs=/local/scratch/bherb/cellrangerTest/10X243-8 --sample=10X243-8 --localcores=24 --localmem=40 --transcriptome=refdata-gex-GRCh38-2020-A --description=10X243-8_intron --include-introns

Mouse example call:

cellranger count --id=pBICCNsMMrBSL8iF005d190521 --fastqs=/local/scratch/bherb/cellrangerTest/pBICCNsMMrBSL8iF005d190521  --sample=pBICCNsMMrBSL8iF005d190521 --localcores=24 --localmem=40 --transcriptome=refdata-gex-mm10-2020-A --description=pBICCNsMMrBSL8iF005d190521_intron --include-introns
