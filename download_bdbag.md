NeMO archive makes use of BDBags to bundle, organize and share datasets. BDBags have advanced features over traditional methods like tar or zip files to package files in an organized directory structure. Since all files on NeMO archive are accessible by https, the BDBags that we create are actually a collection of URL links back to the data itself that the BDBag software uses to retrieve the actual file. The advantage of this system is that the user can choose which files to download instead of dealing with an unwieldy zip file which could contain Gb's worth of data. When NeMO makes quarterly data releases or when users publish a dataset associated with a paper, these datasets can be organized and shared with BDBags.

Data can be downloaded with BDBags in a variety of ways. The user first has to download the BDBag file, an example from the Mini-atlas brain study is [here](http://data.nemoarchive.org/publication_release/MOp_MiniAtlas_2020/Analysis_10X_cells_v2_AIBS.tgz). Opening this .tgz file results in a folder that contains multiple files that the BDBag software uses and an empty folder named 'data' that initially is empty. When the user downloads data, it populates this 'data' folder. The 'fetch.txt' contains a list of URL links to the files associated with the BDBag. 


**GUI interface software for BDBags**

The software described [here](https://bd2k.ini.usc.edu/tools/bdbag/) allows users to interact with the BDBag much like would access any other file on their personal computer. 

![bdbag_gui](https://bd2k.ini.usc.edu/tools/assets/images/bdbag_gui.png)



**Command-line software for BDBags**

For users who prefer a command-line option and a programatic solution to download files, see this [github page](https://github.com/fair-research/bdbag) to get code for the 'bdbag' program. With this function, a user can download either all or a partial set of files within a BDBag based on file size, file name or by directory. This [guide](https://github.com/fair-research/bdbag/blob/master/doc/cli.md) provides details for downloading specific files, and the function and options below reviews a few of the boolean commands that can be used to select files. 

#### `--fetch-filter <column><operator><value>`
Selectively fetch files where entries in `fetch.txt` match the filter expression `<column><operator><value>` where:
*  `column` is one of the following literal values corresponding to the field names in `fetch.txt`: `url`, `length`, or `filename`
* `<operator>` is one of the following predefined tokens:

	| Operator | Description |
	| --- | --- |
	|==| equal
	|!=| not equal
	|=*| wildcard substring equal
	|!*| wildcard substring not equal
	|^*| wildcard starts with
	|$*| wildcard ends with
	|>| greater than
	|>=| greater than or equal to
	|<| less than
	|<=| less than or equal to
* `value` is a string or integer
