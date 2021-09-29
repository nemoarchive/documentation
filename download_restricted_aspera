NeMO currently provides access to restricted data via Google Cloud Platform. [See documentation here](https://github.com/nemoarchive/documentation/blob/master/download_restricted.md). We provide this page for those legacy users who previously accessed data via Aspera.

### Downloading Data using Aspera

If you **do not** already have Aspera CLI installed, see [download & installation instructions here](https://github.com/nemoarchive/documentation/blob/master/install_aspera.md).

#### Downloading Files
From your command line terminal, the `aspc` utility is used to download encrypted data using the following syntax:

```bash
$ ascp [-l <Maximum download speed>] [-k 2] [-m <Minimum download speed>] [-Q] [-T] \
    <username>@aspera.nemoarchive.org:/<path to desired files on NeMO server> \
    /path/to/local/download/directory
```
All parameters between the [ ... ] brackets are optional parameters that are described below:
* l - Allows for the user to set a maximum download speed that aspera should attempt to stay at or below for the duration of the transfer. A speed in Megabits must be provided with this flag.
* k - Allow for resumable data transmission in case an interruption occurs.
* m - Allows for the user to set a minimum download sped that aspera should attempt to stay at or above for the duration of the transfer. A speed in Megabits must be provided with this flag.
* Q - Turns adaptive rate on. Adaptive rate controls the speed of aspera with a goal of not dominating the bandwidth available. Very useful on busy networks that may have other transfers ongoing.
* T - Turns encryption off. Turning encryption off will allow for a maximum throughput transfer but should not be provided if data being uploaded is sensitive.

Example invocation to download a single file:

```bash
$ ascp -l 100M -k 2 -QT user@aspera.nemoarchive.org:/my_project/my_data.tar.gz /home/user/my_data/
```
In the above example, Aspera will attempt to keep the maximum transfer speed at 100 Megabits per second. Adaptive
rate flow is turned on so that aspera does not monopolize the network bandwidth. The `my_data.tar.gz` file
will be downloaded to the local `/home/user/data` area.

Downloading a directory of files requires no changes in syntax:

```bash
$ ascp -l 100M -k 2 -QT user@aspera.nemoarchive.org:/my_project /home/user/my_data/
```

Upon entering the command to initiate a download, you will be prompted for your password.  


#### Decrypting downloaded data
Here we recommend decryption tools and provide instructions specific to your operating system:

##### Mac (OSX)
1. Install GPGSuite, available at [https://gpgtools.org/](https://gpgtools.org/)
2. From your command line terminal, run the following command, replacing public.key with the key file provided to you via email:
```
gpg --import public.key
```
3. From your command line terminal, run the following command. Note that the file/directory that you want to write to does **not** need to be created beforehand:
```
gpg --output <path/to/file/to/write/to> --decrypt <path/to/encrypted/file>
```

##### Windows
1. Download and install GPG4Win, available at [https://www.gpg4win.org/](https://www.gpg4win.org/)
2. Open Kleopatra (included in GPG4Win)
3. Go to File -> Import Certificate and select the public key file that was emailed to you
4. Exit Kleopatra
5. Right click on the encrypted file and click "Decrypt and Verify" on the next menu
6. You can change the output folder in this menu

##### Linux
1. Install gpg through your distribution's package manager (apt, yum, etc)
2. Execute the following command from the command line, replacing public.key with the key file provided to you via email:
  ```
  gpg --import public.key
  ```
3. Execute the following command from the command line. Note that the file/directory that you want to write to does **not** need to be created beforehand:
```
gpg --output <path/to/file/to/write/to> --decrypt <path/to/encrypted/file>
```

&nbsp;

