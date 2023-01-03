# NeMO Archive File Status API

Users wishing to know if the NeMO Archive has received a file from a submission and what the ingest status of that file is may use the File Status API described here.

- [NeMO Archive File Status API](#nemo-archive-file-status-api)
  - [Retrieving Status with a NeMO Identifier](#retrieving-status-with-a-nemo-identifier)
  - [Retrieving Status with a Submission ID and File Name](#retrieving-status-with-a-submission-id-and-file-name)
  - [Swagger/OpenAPI documentation](#swaggeropenapi-documentation)

## Retrieving Status with a NeMO Identifier

Endpoint:

`https://api.nemoarchive.org/v1/file-status/{identifier}`

The "identifier" is a NeMO Archive generated string that is assigned during the data submissin/ingest process
and uniquely identifies NeMO assets such as files.

Example:

`$ curl -X GET https://api.nemoarchive.org/v1/file-status/nemo:sqc-0000001`

```json
{
  "file_name": "filename.tar.gz",
  "schema": "https://raw.githubusercontent.com/nemoarchive/api-schemas/main/file-status-api/identifier_response.json",
  "status": "COMPLETE",
  "submission_id": "ABCD123",
  "submitter": {
    "username": "jdoe",
    "first": "John",
    "last": "Doe",
    "email": "jdoe@example.com"
  }
}
```

The "submission_id" property identifies which submission the specified file was submitted with, and the "file_name" contains the actual name of the file. The "submitter" property itself contains an object that describes who the NeMO submitter was that submitted the file. The "status" property contains the current status of the file and may take one of the following values:  COMPLETE, FAILED, IN_PROGRESS, and HOLD. Finally, the "schema" property contains a URL to a [JSON-Schema](https://json-schema.org) document that describes the JSON that is returned from calls to this endpoint. The JSON-Schema document is most usefule and relevant to developers wishing to automate interactions with this API.

## Retrieving Status with a Submission ID and File Name

Endpoint:

`https://api.nemoarchive.org/v1/file-status/{submission_id}/{file_name}`

Example:

`$ curl -X GET https://api.nemoarchive.org/v1/file-status/ABCD123/filename.tar.gz`

```json
{
  "identifier": "nemo:sqc-0000001",
  "schema": "https://raw.githubusercontent.com/nemoarchive/api-schemas/main/file-status-api/sub_id_filename_response.json",
  "status": "COMPLETE",
  "submission_id": "ABCD123",
  "submitter": {
    "username": "jdoe",
    "first": "John",
    "last": "Doe",
    "email": "jdoe@example.com"
  }
}
```

The response to this endpoint is very similar to the previous endpoint that requires an "identifier" paramter specified in the URL. However, in the case where the caller does not know what the identifier is, this endpoint may be used instead. It simply requires the submission id in which the file was submitted to the NeMO Archive, and the name of the file in that submission (file names are unique in NeMO submissions). The response will contain the NeMO identifier for the file in the "identifier" property. Please note that the "schema" property in the response points to a different JSON-Schema document since the response is slightly different.

## Swagger/OpenAPI documentation

In addition to this documentation, developers may also consult the [Swagger](https://swagger.io) specification that NeMO maintains [here](https://app.swaggerhub.com/apis/UMIGS/NeMO_file_status/1.0).
