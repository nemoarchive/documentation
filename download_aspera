# Downloading files with the Aspera Client

Aspera's fasp transfer technology eliminates the fundamental bottlenecks of conventional file transfer technologies such as FTP, HTTP, and Windows CIFS, and speeds transfers over public and private IP networks. In addition, users have extraordinary control over individual transfer rates and bandwidth sharing, and full visibility into bandwidth utilization.

## Installing Aspera

Please go to the Aspera website and download the Aspera CLI Client. Once installed, you should have the ascp utility available to you.

## Transferring Files To/From the Aspera Server

Once the Aspera client has been successfully installed, transfers to and from the NeMO Aspera server can be executed with your NeMO username and password.

## Downloading Files

Commands to initiate a download result in a prompt for your password.

Downloading with the Aspera client uses the following syntax

```bash
ascp [-l <Maximum download speed>] [-k 2] [-m <Minimum download speed>] [-Q] [-T] \
    <username>@aspera.nemoarchive.org:/<path to desired files on NeMO server> \
    /path/to/local/download/directory
```

All parameters between the [ ... ] brackets are optional parameters that are described below:

* l - Allows for the user to set a maximum download speed that aspera should attempt to stay at or below for the duration of the transfer. A speed in Megabits must be provided with this flag.
* k - Allow for resumable data transmission in case an interruption occurs.
* m - Allows for the user to set a minimum download sped that aspera should attempt to stay at or above for the duration of the transfer. A speed in Megabits must be provided with this flag.
* Q - Turns adaptive rate on. Adaptive rate controls the speed of aspera with a goal of not dominating the bandwidth available. Very useful on busy networks that may have other transfers ongoing.
* T - Turns encryption off. Turning encryption off will allow for a maximum throughput transfer but should not be provided if data being uploaded is sensitive.

An example invocation of a file being downloaded from the NeMO server can be found below:

```bash
ascp -l 100M -k 2 -QT user@aspera.nemoarchive.org:/my_project/my_data.tar.gz /home/user/my_data/
```

In this case, Aspera will attempt to keep the maximum transfer speed at 100 Megabits per second and would not encrypt the data being sent to the NeMO server. Adaptive rate flow is turned on so that aspera does not monopolize the network bandwidth. The my_data.tar.gz file will be downloaded to the local /home/user/data folder with the execution of this command.

Downloading a directory of files requires no changes in syntax:

```bash
ascp -l 100M -k 2 -QT user@aspera.nemoarchive.org:/my_project /home/user/my_data/
```

## Uploading Files

Commands to initiate an upload will result in a prompt for your password.

Uploading files can be achieved using the same download syntax described above:

```bash
ascp [-l <Maximum download speed>] [-k 2] [-m <Minimum download speed>] [-Q] [-T] \
    <path to file or directory>user@aspera.nemoarchive.org \
    /path/to/desired/directory/
```

The only change here is supplying a file prior to providing user@aspera.nemoarchive.org:

```bash
ascp -l 100M -k 2 -QT /home/user/my_data/my_data.tar.gz user@aspera.nemoarchive.org:/my_project/
```

Here the my_data.tar.gz file would be uploaded to the my_project folder on the NeMO server. Uploading a directory of files would not require any changes as aspera will recognize a folder is being transferred and will recursively step through ensuring that all files found in the directory are transferred.

```bash
ascp -l 100M -k 2 -QT /home/user/my_data/ user@aspera.nemoarchive.org:/my_project/
```
