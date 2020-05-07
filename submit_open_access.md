Data submissions to NeMO include the submission of the raw sequence files as well as derived intermediate files such as BAM files, and analyzed results including counts and cluster information. In addition, data submitters also submit metadata associated with the data to BCDC.

Presently data submissions are accomplished by uploading data to the archive using Aspera. See the instructions to install the [Aspera client](Install-Aspera-Client). To upload data to NeMO you will need credentials, which can be obtained by completing the [account request form](https://nemoarchive.org/register.php).

To make the data easily findable by end users we have established a consistent directory structure for the projects and studies in the repository that is shown in the figure below. We request that you please adhere to the same directory structure for your data. Please create this directory structure and place files where necessary before uploading. You do not need to include directories that will be empty.

```
biccn
└── grant
    ├── cemba
    │   ├── callaway
    │   │   └── projection
    │   │       ├── bulk
    │   │       │   ├── processed
    │   │       │   │   ├── align
    │   │       │   │   └── peaks
    │   │       │   └── raw
    │   │       └── sncell
    │   │           ├── processed
    │   │           │   ├── align
    │   │           │   └── peaks
    │   │           └── raw
    │   └── ecker
    │       ├── chromatin
    │       │   └── scell
    │       │       ├── processed
    │       │       │   ├── align
    │       │       │   └── counts
    │       │       └── raw
    │       └── methylation
    │           ├── bulk
    │           │   ├── processed
    │           │   │   ├── align
    │           │   │   └── peaks
    │           │   └── raw
    │           └── scell
    │               ├── processed
    │               │   ├── align
    │               │   ├── allc
    │               │   ├── counts
    │               │   └── peaks
    │               └── raw
    ├── devhu
    │   └── kriegstein
    │       └── transcriptome
    │           ├── scell
    │           │   └── processed
    │           │       └── counts
    │           └── sncell
    │               └── processed
    │                   └── counts
    └── feng
        └── feng
            └── transcriptome
                └── scell
                    └── raw
```

Please substitute your grant abbreviation, and lab name (use grant abbreviation if a single lab is generating data). For instance in the above image <i>cemba</i> is the grant name, and <i>ecker</i> is the lab name.

You may use Aspera to copy the contents of the directory recursively, or alternately and preferably, you can create a submission tar and upload the tar to us.

To verify that the files transferred to us are not corrupted we use MD5 checksums to validate the transfer. So please submit the MD5 checksums with your data uploads. See the [section below](#md5-checksum) for generating the MD5 checksums and the specific file format to submit the MD5 checksums.

For example, for a submission.tar.gz file, the unpacked structure would look something like:
```
submission.tar.gz
├── md5sums.txt
└── my_lab
    └── chromatin
        ├── bulk
        │   ├── processed
        │   │   └── A
        │   └── raw
        │       └── B
        └── sncell
            ├── processed
            │   └── C
            └── raw
                └── D
```
## MD5 Checksum Format <a name="md5-checksum"></a>
The checksum file must be named "md5sums.txt" (all lower case, with Unix line endings). It should be placed in the root of the tarball submission. File format is to be compatible with the standard `md5sum` utility, that is:

```<128-bit hexadecimal md5 checksum><2 spaces><file path relative to tarball root>```

Then the contents of the md5sums.txt checksum file would be in the format below (this example shows dummy checksums and filenames):
```
b71aacdabe99a51474ba84fa03ec61ec  my_lab/chromatin/sncell/processed/C
24c37182df9ead481be538a4dcecf641  my_lab/chromatin/sncell/raw/D
6cbc24a41a4c28ff1eeec14ccf70e3cc  my_lab/chromatin/bulk/processed/A
4253c8c3af0c12a9188f5342841d4ec9  my_lab/chromatin/bulk/raw/B
```

Data submitted to NeMO is placed in a special "incoming" holding area while the data is being reviewed for quality control. While in this area, the submitted data is still browsable using the Aspera web interface. Please point your web browser to [https://aspera.nemoarchive.org](https://aspera.nemoarchive.org). After logging in with your NeMO Archive credentials, you may be prompted to download and install a web browser plugin. Please follow the directions. If you have installed the browser plugin correctly, you will be able to navigate up and down the filesystem structure with the interface that is presented, which should look very similar to the figure below:

![Aspera Web Interface](site_figures/aspera_web_interface.png)

Presently, the Aspera web interface is the only way to browse submitted data before it has been released.
