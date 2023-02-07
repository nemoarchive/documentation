# NeMO API Overview
The NeMO Archive makes available a number of application programming interfaces (APIs) to assist users in submitting data as well as retrieving information from data hosted at the archive. Below is an overview of the currently available APIs as well as links to more detailed information about each API. 

## APIs at NeMO: Getting started
Most APIs at the NeMO Archive are secured with the use of JWT (Javascript Web Tokens). The linked document here goes through the steps to obtain a token and important considerations about maintaining the security of tokens ([Obtaining a Javascript token for NeMO](https://github.com/nemoarchive/documentation/blob/master/api-logins.md)). 

## Submission API
The NeMO Archive exposes a RESTful API for users that wish to create code to make submissions and retrieve the status of their past submissions. Since the API is RESTful, any modern programming language that has a suitable HTTP library should be able to easily obtain the data. Results are returned in JSON format to make the parsing of the data easier. 

Additional information and examples:
- [Github link](https://github.com/nemoarchive/documentation/blob/master/submission-api.md)
- **Swagger link?**

## File status API
This API can be used to determine if a particular file has been submitted to NeMO and return the ingest status of that file. File status can be queried with either a NeMO identifier (e.g. nemo:sqc-0000001) or with a file name and submission ID. 

Additional information and examples:

- [Github link](https://github.com/nemoarchive/documentation/blob/master/file-status-api.md)
- [Swagger link](https://app.swaggerhub.com/apis-docs/UMIGS/NeMO_file_status/)

