# Overview

The NeMO Archive exposes a RESTful API for users that wish to create code to make submissions and retrieve the status of their past submissions. Since the API is RESTful, any modern programming language that has a suitable HTTP library should be able to easily obtain the data. Results are returned in JSON format to make the parsing of the data easier. In the examples provided here, we will be using the fairly ubiquitous `curl` command-line utility to demonstrate the operation of the submission status API. Requests are currently rate limited to 3 requests per second (subject to change).

- [Retrieving Status for a Submission](#retrieving-status-for-a-submission)
- [Retrieving Submission History](#retrieving-submission-history)
  - [Interpreting the Results](#interpreting-the-results)
  - [Paging Through Results](#paging-through-results)
- [Obtaining Extended Results For Other Users' Submissions](#obtaining-extended-results-for-other-users-submissions)
- [Searching Submissions](#searching-submissions)
  - [Search with Substrings](#search-with-substrings)
  - [Pagination Through Search Results](#pagination-through-search-results)
- [Making a Submission Through the API](#making-a-submission-through-the-api)
- [Submission Webhooks](#submission-webhooks)
  - [Notification Document Fields](#notification-document-fields)
  - [Authentication Styles](#authentication-styles)
  - [Additional Headers](#additional-headers)
  - [Callback Payload](#callback-payload)
  - [Callback Delivery](#callback-delivery)
- [Dry Runs](#dry-runs)
  - [Dry Runs and Webhooks](#dry-runs-and-webhooks)

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

## Searching Submissions

The NeMO submissions API also has a search feature to search through the submissions that one has access to using various fields to narrow the search results down. This feature is particularly useful if one has a large number of submissions in the NeMO archive and is interested in obtaining the data using various criteria such as those submissions made before or after a particular date, having a particular policy (open data / restricted data), or having a certain status ("complete", "in-progress", "error"). The search feature can also be used to obtain data about submissions if the submission ID is no longer available or unknown.

`$ curl -X POST -H "Authorization: Bearer XXXXXXXXXXXXXXXXXXX" https://nemoarchive.org/api/submission/search -d '{SEARCH_DOC}'`

where {SEARCH_DOC} is a small JSON document that can contain one or more of the fields as shown below:

```
{
  "id": "XXXXXXX",
  "submitter: "user",
  "start-date": "2023-01-01",
  "end-date": "2023-03-31",
  "policy": "open",
  "status": [ "complete" ]
}
```

In this example, a fully completed search document has been used that would translate into a search for submissions with the ID of "XXXXXXX", submitted by user "user", submitted between "2023-01-01" and "2023-03-31" (first quarter of 2023), with an "open" data policy (no "restricted" data), and having fully completed the ingest process (no "in-progress" or "error" submissions should be returned). With such a search document, there can only be 1 result returned because the submission ID, which is globally unique across all NeMO submissions is fully specified. However, if that submission were to not match because of other reasons (status, dates, etc), then no result would be returned.

A more permissive search can be performed by excluding some to the fields above. For example:

```
{
  "start-date": "2023-01-01",
  "end-date": "2023-03-31"
}
```

Return submissions made between 2023-01-01 and 2023-03-31 regardless of status or policy, etc.

### Search with Substrings

Some of the fields of the search document support searching for submissions with only partial data, such as substring. For example, a complete 7 character ID is not necessary to find the submission by ID. If the ID is known to have the letter "x" in it, then a search document like the one below can be used to retrieve it:

```
{
  "id": "x"
}
```

For users with elevated permissions, search results can be refined in combination with substrings of submitter usernames. For example, the document below would retrieve submission data for submissions with "x" in the submission ID and made by submitters with "jo" in their username.

```
{
  "id": "x",
  "submitter": "jo"
}
```

### Pagination Through Search Results

The search feature may not yield ALL the search results in a single response. In such a case, the user will be required to retrieve the next "page" of results by specifying the page number in the URL:

Just as in the earlier documentation for pagination, the response document will specify what page of results it contains. If a page is requested that is out of the range of possible pages for the given query, only the last page is returned. Similarly, if no page is specified, or if an invalid page is specified, only 1 is returned.

```
$ curl -X POST -H "Authorization: Bearer XXXXXXXXXXXXXXXXXXX" https://nemoarchive.org/api/submission/search?page=3 -d '{SEARCH_DOC}'

{
  "total": 121,
  "results": [
    RESULT1,
    RESULT2,
    ...
  ]
  "page": 3
}
```

## Making a Submission Through the API

One can also submit a manifest through the API instead of going through the NeMO Archive website. Submitting the file still requires authentication with a JWT as shown above, but it can be done with any suitable tool like curl, or language, that supports a multipart file upload. Examples:

`$ curl -X POST -H "Authorization: Bearer XXXXXXXXXXXXXXXXXXX" https://nemoarchive.org/api/submission -F manifest=@/path/to/file.csv`

where /path/to/file.csv is your local path to the manifest to be submitted.

If one is using python with the requests module, the code might look similar to this:

```python
submission_endpoint = "https://nemoarchive.org/api/submission"
auth_header = {'Authorization': 'Bearer {}'.format(token)}

filename = "/path/to/my/manifest.tsv"
original_name = filename.split("/")[-1]
manifest_data = {'manifest': (original_name, open(filename, 'rb'))}

response = requests.post(submission_endpoint, files=manifest_data, headers=auth_header)
```

A successful manifest submission will produce a response having the following structure:

```
{
  "result": "success",
  "id": "XXXXXXX"
}
```

where the "id" property will contain the generated id for the submission.

## Submission Webhooks

Rather than polling the submission status endpoints, submitters can ask NeMO to
call a URL of their own as a submission progresses through the ingest process.
To do so, include a `notification` field alongside the `manifest` file when
creating the submission. The value is a JSON document describing the callback
URL, the HTTP method to use, and how NeMO should authenticate to it.

A callback is issued at each of the six steps of the ingest process, on both
success and failure:

| Step | `data.step.name` | Issued when |
| --- | --- | --- |
| 1 | `manifest_validated` | The submitted manifest has been validated. |
| 2 | `upload_complete` | The submitter's data files have finished uploading. |
| 3 | `qc_complete` | The submission has completed quality control. |
| 4 | `https_release` | The files have been released for HTTPS download. |
| 5 | `gcp_release` | The files have been released to Google Cloud Storage. |
| 6 | `portal_release` | The files have been released to the NeMO Portal. |

Endpoints should key off the `data.step.name` field to determine which step a
given callback refers to, rather than assuming a particular step or ordering. A
submission that fails at some step will not produce callbacks for the steps
after it.

`$ curl -X POST -H "Authorization: Bearer XXXXXXXXXXXXXXXXXXX" https://nemoarchive.org/api/submission -F manifest=@/path/to/file.tsv -F 'notification={"url":"https://example.org/hooks/nemo","method":"POST","auth_type":"bearer_token","auth_details":{"token":"YOUR_TOKEN"}}'`

With python and the requests module:

```python
import json

notification = {
    "url": "https://example.org/hooks/nemo",
    "method": "POST",
    "auth_type": "bearer_token",
    "auth_details": {"token": "YOUR_TOKEN"},
}

manifest_data = {'manifest': (original_name, open(filename, 'rb'))}
form_data = {'notification': json.dumps(notification)}

response = requests.post(
    submission_endpoint, files=manifest_data, data=form_data, headers=auth_header
)
```

The notification document is optional. If it is supplied but is not valid JSON,
or does not conform to the schema described below, the submission is rejected
with a 422 status and the following response:

```
{
  "message": "The 'notification' document was invalid."
}
```

### Notification Document Fields

| Field | Required | Description |
| --- | --- | --- |
| `url` | Yes | The callback URL. Must begin with `https://` (plain HTTP is not accepted) and be at most 512 characters. |
| `method` | Yes | The HTTP method NeMO will use for the callback. One of `GET`, `POST`, or `PUT`. |
| `auth_type` | Yes | How NeMO should authenticate to your endpoint. One of `none`, `secret_token`, `basic`, `bearer_token`, or `api_key`. |
| `auth_details` | Yes | An object whose shape depends on `auth_type`, as described below. Must be `null` when `auth_type` is `none`. |
| `headers` | No | An object of additional HTTP headers to send with the callback. Both keys and values must be strings. |

No other fields are permitted; including any unrecognized field will cause the
document to be rejected.

Note that `auth_details` is always required, even when no authentication is
being used. When `auth_type` is `none`, the field must be present and explicitly
set to `null`.

### Authentication Styles

**`none`** — no authentication is performed. `auth_details` must be `null`.

```json
{
  "url": "https://example.org/hooks/nemo",
  "method": "POST",
  "auth_type": "none",
  "auth_details": null
}
```

**`basic`** — HTTP Basic authentication. Both `username` and `password` are
required.

```json
{
  "url": "https://example.org/hooks/nemo",
  "method": "POST",
  "auth_type": "basic",
  "auth_details": {
    "username": "nemo",
    "password": "SECRET"
  }
}
```

**`bearer_token`** — the token is presented as a bearer token, typically a JWT.

```json
{
  "url": "https://example.org/hooks/nemo",
  "method": "POST",
  "auth_type": "bearer_token",
  "auth_details": {
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
  }
}
```

**`secret_token`** — the token is sent in an HTTP header. `token` is required.
The optional `header_name` selects the header the token is placed in; if it is
omitted, the token is sent in an `X-Secret-Token` header.

```json
{
  "url": "https://example.org/hooks/nemo",
  "method": "POST",
  "auth_type": "secret_token",
  "auth_details": {
    "header_name": "X-My-Token",
    "token": "SECRET"
  }
}
```

**`api_key`** — the token is sent in an HTTP header. `token` is required. The
optional `header_name` selects the header the token is placed in; if it is
omitted, the token is sent in an `X-Api-Key` header.

```json
{
  "url": "https://example.org/hooks/nemo",
  "method": "POST",
  "auth_type": "api_key",
  "auth_details": {
    "header_name": "X-API-Key",
    "token": "SECRET"
  }
}
```

The `secret_token` and `api_key` types behave identically apart from the header
used when `header_name` is omitted.

### Additional Headers

The optional `headers` field can be used to send extra headers with each
callback, which is useful for routing or tagging the request on your side.

```json
{
  "url": "https://example.org/hooks/nemo",
  "method": "POST",
  "auth_type": "none",
  "auth_details": null,
  "headers": {
    "X-Project": "my-project",
    "X-Environment": "production"
  }
}
```

### Callback Payload

For `POST` and `PUT` callbacks, NeMO sends a JSON document as the request body,
with a `Content-Type` of `application/json`. For `GET` callbacks the same data
is URL-encoded into the query string instead.

```json
{
  "event_id": "458ba3d8-c379-4af1-b231-ba7159ce6222",
  "data": {
    "step": {
      "name": "manifest_validated",
      "status": "success",
      "step_number": 1,
      "total_steps": 6,
      "details": [
        "Manifest validated"
      ]
    },
    "submission_id": "abc1234",
    "submission_status": "in_progress",
    "submitter": {
      "username": "test_user",
      "email": "test@example.com",
      "first": "FirstName",
      "last": "LastName"
    }
  },
  "timestamp": "2026-09-04T04:55:43Z"
}
```

| Field | Description |
| --- | --- |
| `event_id` | A UUID that is unique to this delivery. Because a callback may be retried, the same event may be delivered more than once with the same `event_id`; it can be used to recognize and discard duplicates. |
| `timestamp` | The UTC time the payload was generated, in `YYYY-MM-DDTHH:MM:SSZ` format. |
| `data.submission_id` | The 7 character submission ID. |
| `data.submission_status` | The overall status of the submission: `in_progress`, `errored`, or `complete`. |
| `data.step` | The ingest step this callback is reporting on. |
| `data.step.name` | Which ingest step this callback reports on. See the table of the six steps [above](#submission-webhooks). |
| `data.step.status` | Either `success` or `error`. |
| `data.step.step_number` | The ordinal position of this step in the ingest process, from 1 to 6. |
| `data.step.total_steps` | The total number of ingest steps, currently 6. |
| `data.step.details` | A non-empty list of human readable messages about the step. On an error, this contains the validation error messages. |
| `data.submitter` | The submitter's `username`, `email`, `first`, and `last` name. |

A `submission_status` of `errored` means the submission has failed at the step
described in `data.step` and will not proceed. A status of `complete` is only
sent when the final step has succeeded.

### Callback Delivery

NeMO expects your endpoint to respond with a 2xx HTTP status code. Any other
status code, or a connection failure or timeout, is treated as a failed
delivery.

Each callback is attempted up to 5 times. Retries use an exponential backoff,
waiting 1, 2, 4, and then 8 seconds between successive attempts. Each individual
attempt has a 10 second timeout. If all 5 attempts fail, the callback is
abandoned and is not retried again later, though the ingest of the submission
itself is unaffected and continues normally.

Because a delivery may be retried after your endpoint has already processed it
(for example, if your response was slow enough to time out), your endpoint
should be idempotent. The `event_id` field can be used to recognize a repeated
delivery.

## Dry Runs

Data submitters may wish to test a manifest that they have prepared for errors before actually submitting it to NeMO. The submission API allows users to submit "dry runs." A key difference between a dry run and an actual submission is that a dry run does not trigger the complete ingest process. It simply checks that the submitted manifest file is well formed, that it has the correct number of columns, and that the columns that must adhere to controlled vocabularies are not using any invalid terms. The deeper quality control measures that happen during the QC step of the ingest process are not performed. When the validation is complete the submitter of the dry run is informed of the outcome of the dry run via email, but the generated ID for the submission will NOT appear in the user's submission history in the dashboard.

To submit a manifest as a dry run, simply add "dryrun=y" to the submission creation endpoint as a query string parameter.

Example:

```python
submission_endpoint = "https://nemoarchive.org/api/submission?dryrun=y"
```

### Dry Runs and Webhooks

A [notification document](#submission-webhooks) submitted with a dry run is
validated, and an invalid one will cause the dry run to be rejected, but it is
not registered and no callbacks will be issued. Dry runs are therefore a
convenient way to check that your notification document is well formed, but
they cannot be used to test that your endpoint receives callbacks correctly.
