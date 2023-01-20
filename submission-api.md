# Overview

The NeMO Archive exposes a RESTful API for users that wish to create scripts and automate the retrieval of the status of their data submissions. Since the API is RESTful, any modern programming language that has a suitable HTTP library should be able to easily obtain the data. Results are returned in JSON format to make the parsing of the data easier. In the examples provided here, we will be using the fairly ubiquitous `curl` command-line utility to demonstrate the operation of the submission status API.

- [Retrieving Status for a Submission](#retrieving-status-for-a-submission)
- [Retrieving Submission History](#retrieving-submission-history)
  - [Interpreting the Results](#interpreting-the-results)
  - [Paging Through Results](#paging-through-results)
- [Obtaining Extended Results For Other Users' Submissions](#obtaining-extended-results-for-other-users-submissions)
- [Making a Submission Through the API](#making-a-submission-through-the-api)
- [Dry Runs](#dry-runs)

## Retrieving Status for a Submission

Once a valid [JWT](https://jwt.io) has been obtained as decribed [here](api-logins.md), one can use it to retrieve a submission's history and status by issuing a GET request with it. The token must be specified with an HTTP "Authorization" header. In the following example, the token is shown as a series of "X" characters. This should be replaced with your actual token, which is a sensitive/private piece of information. In addition, the submission ID is shown as a series of "Y" characters. This should be replaced with your specific submission ID.

Example:

`$ curl -X GET -H "Authorization: Bearer XXXXXXXXXXXXXXXXXXX" https://nemoarchive.org/api/submission/YYYYYYY`

```
{
  "id": "YYYYYY",
  "submitter": "username",
  "creation": "2021-01-01T10:00:00Z",
  "complete": true,
  "success": true,
  "steps": [
      STEP1,
      STEP2
      ...
      STEP6
  ],
  "policy": "open"
}
```

The "id" property is simply the randomly generated identifier for the submission. All submissions are guaranteed to have a globally unique identifier among all submissions. The "creation" property will have the UTC timestamp of when the submission was initiated, which is the when the manifest was submitted via the nemoarchive.org website or API. The "complete" property will indicate whether the submission is considered complete or not. This will only be true if all the steps have completed, or if there was a failure. The "success" property indicates whether the submission successfully completed. If the submission is still progressing. If there are any failures, this value will be false. The "policy" property, if set, will show whether the manifest indicated the presence of "open" data, controlled/restricted data, or embargoed data. If there was an error in the manifest validation step, the policy may not have been determined, and the property may not be present.

Each "step" in the "steps" list property, will have the following structure:

```
{
    "name": "portal_release",
    "success": true,
    "date": "2021-01-01T10:00:00Z",
    "msg": "Released files were integrated into the NeMO Portal successfully"
}
```

The "step" property names the ingest step, of which there are 6: manifest_validated, upload_complete, qc_complete, https_release, gcp_release and portal_release.

The "success" property will contain a boolean value, and will indicate whether the step succeeded or failed. The "date" property will contain the UTC timestamp of when the step succeeded (or failed). Finally, the "msg" property will have a message containing some brief message about completion of the status, or why the step failed. Steps that are in-progress and not yet complete will not be included in the "steps" list. Since the ingest process contains 6 steps, a fully complete and successful submission will therefore have 6 objects in the "steps" list property.

## Retrieving Submission History

It's possible to retrieve one's entire submission history and status by issuing a GET request to the /submission endpoint.

Example: 

`$ curl -X GET -H "Authorization: Bearer XXXXXXXXXXXXXXXXXXX" https://nemoarchive.org/api/submission`

```
{
  "total": 12,
  "results": [
    RESULT1,
    RESULT2,
    ...
    RESULT12
  ]
}
```

### Interpreting the Results

The "total" property indicates how many submissions the API is aware of for the authenticated user.

Each "RESULT" in the list of results is itself a JSON object containing the details of the submission, and the result of each step of the ingest process as previously shown.

### Paging Through Results

The submission API paginates submissions 100 at a time. Therefore, if the user has made more than 100 submissions, multiple requests will need to be issued to fetch the entirety of the data. The user will alerted to the need to paginate by the presence of a "next" property in the returned JSON document. When paging through the results, the "page" property will also indicate the page number, or slice of data, that has been retrieved. Example:

```
{
  "total": 121,
  "results": [
    RESULT1,
    RESULT2,
    ...
    RESULT100
  ]
  "page": 1,
  "next": "https://nemoarchive.org/api/submission?page=2"
}
```

## Obtaining Extended Results For Other Users' Submissions

If one is a NeMO submission group leader, or a NeMO system superuser, the API is capable of returning results for your own submissions as well as those made by members of your group. Simply add a "all=y" query parameter to the request URL, and the API will return these extended results to you (if authorized). For example:

`$ curl -X GET -H "Authorization: Bearer XXXXXXXXXXXXXXXXXXX" https://nemoarchive.org/api/submission?all=y`

## Making a Submission Through the API

One can also submit a manifest through the API instead of going through the NeMO Archive website. Submitting the file still requires authentication with a JWT as shown above, but it can be done with any suitable tool like curl, or language, that supports a multipart file upload. Examples:

`$ curl -X POST -H "Authorization: Bearer XXXXXXXXXXXXXXXXXXX" https://nemoarchive.org/api/submission -F manifest=@/path/to/file.csv`

where /path/to/file.csv is your local path to the manifest to be submitted.

If one is using python with the requests module, the code might look similar to this:

```python
submission_endpoint = "https://nemoarchive.org/api/submission"
auth_header = {'Authorization': 'Bearer {}'.format(token)}

fh = open(filename, 'rb')
manifest_data = {'manifest': ('manifest', fh)}

response = requests.post(submission_endpoint, files=manifest_data, headers=auth_header)

fh.close()
```

## Dry Runs

Data submitters may wish to test a manifest that they have prepared for errors before actually submitting it to NeMO. The submission API allows users to submit "dry runs." A key difference between a dry run and an actual submission is that a dry run does not trigger the complete ingest process. It simply checks that the submitted manifest file is well formed, that it has the correct number of columns, and that the columns that must adhere to controlled vocabularies are not using any invalid terms. The deeper quality control measures that happen during the QC step of the ingest process are not performed. When the validation is complete the submitter of the dry run is informed of the outcome of the dry run via email, but the generated ID for the submission will NOT appear in the user's submission history in the dashboard.

To submit a manifest as a dry run, simply add "dryrun=y" to the submission creation endpoint as a query string parameter.

Example:

```python
submission_endpoint = "https://nemoarchive.org/api/submission?dryrun=y"
```
