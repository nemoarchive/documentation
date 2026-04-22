# Accessing SCORCH data and metadata

## Access policies
SCORCH consensus processed data is provided under a [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/deed.en) license. The SCORCH consortium makes all effort to ensure files are as accurate as possible but please note that data and derived products may not always be in their final state at the time of release and are provided as-is [more details](https://scorch.igs.umaryland.edu/data-use-disclaimer.php).

## File access
You can find public files and file information via the following:
- On the [SCORCH data portal](https://scorch-portal.nemoarchive.org/)
- Via collection or dataset landing pages e.g. [https://assets.nemoarchive.org/dat-zngw2c8 ](https://assets.nemoarchive.org/dat-zngw2c8)
- At the [http browser](https://data.nemoarchive.org/scorch/)
- On Google Cloud [SCORCH public data](https://console.cloud.google.com/storage/browser/scorch-public)

## Consensus processed data
The SCORCH data center provides consensus processed outputs of all our SCORCH data (including human) which are publicly accessible 
(including for subjects for which the raw data is restricted). The two  pipelines used by SCORCH for consensus processing
are [Optimus](https://broadinstitute.github.io/warp/docs/Pipelines/Optimus_Pipeline/README) and
[Multiome](https://broadinstitute.github.io/warp/docs/Pipelines/Multiome_Pipeline/README). Details of the pipelines and file contents can be found at those links.

To see what consensus processed data is available via open access you can use the link [here](https://scorch-portal.nemoarchive.org/search?facetTab=Samples&filters=%7B%22op%22:%22and%22,%22content%22:%5B%7B%22op%22:%22in%22,%22content%22:%7B%22field%22:%22file.genome_build%22,%22value%22:%5B%22GRCh38-HIV%22,%22GRCm39_EcoHIV%22,%22GCF_003339765.1-103_SIV%22,%22mRatBN7.2_HIV%22%5D%7D%7D%5D%7D) to see the files in the SCORCH data portal.
## Access to restricted raw data
Access to restricted raw data (i.e. human fastq files) is managed by dbGaP.
Current datasets available for dbGaP request:

- [SCORCH: Single Nuclei Transcriptome Profiling in Addiction Circuitry of the HIV+ Brain](https://dbgap.ncbi.nlm.nih.gov/beta/study/phs003988.v1.p1/#study)
- Additional datasets coming soon

## Requesting additional human subject metadata

The data dictionary for fields collect at NeMO for SCORCH data can be found here ([human](), [nonhuman]()). In the case of certain tissue banks such as NNTC, the source tissue bank may have additional metadata of interest. To see the source where a human donor or nonhuman subject was obtained from, you can add the filter subject source to the portal facet display. In particular, human samples obtained from nntc (nntc.org) have extensive metadata available beyond what is displayed at NeMO. Requests for access to this additional metadata can be submitted at [nntc.org](https://nntc.org/form/contact).


## More information/ user help
If you need more information, please use our contact form [here](https://scorch.igs.umaryland.edu/contact.php) to contact the SCORCH Data Coordinating Center.
