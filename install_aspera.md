# Download and Install Aspera Command Line Interface (CLI)

Aspera's fasp transfer technology eliminates the fundamental bottlenecks of conventional file transfer
technologies such as FTP, HTTP, and Windows CIFS, and speeds transfers over public and private IP networks.
In addition, users have extraordinary control over individual transfer rates and bandwidth sharing, and full
visibility into bandwidth utilization.

### Download
From the [IBM Aspera website](https://www.ibm.com/products/aspera/downloads),
Scroll to Developer Resources, and select the IBM Aspera Command Line Interface Client > Getting started. Installation instructions can be found here [Aspera CLI Install](https://github.com/IBM/aspera-cli#installation). 


While the method at the link above is the preferred install method, a second method of installing ascp is below:

1. Go to the Aspera website and download/install the Web connect browser plugin [Plugin](https://www.ibm.com/aspera/connect/)
2. Depending on your operating system, you’ll find the ascp binary file at:
* Windows: <INSTALLDIR>\Aspera\Aspera Connect\bin (e.g. C:\Program Files\Aspera...)
* Mac: /Applications/Aspera\ Connect.app/Contents/Resources
* Linux: /home/<USERNAME>/.aspera/connect/bin/ (e.g. /home/user/.aspera...)
3. cd to the directory above that matches your system, you should see ‘ascp’ in the directory
4. Run  export so you can call ascp from elsewhere (e.g for a mac, change path as needed):
export PATH=/Applications/Aspera\ Connect.app/Contents/Resources/:$PATH 

