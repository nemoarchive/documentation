# Overview

The NeMO Archive exposes a read-only REST API for users that wish to create scripts and automate the retrieval of the status of their data submissions. Since the API is RESTful, any modern programming language that has a suitable HTTP library should be able to easily obtain the data. Results are returned in JSON format to make the parsing of the data easier. In the examples provided here, we will be using the fairly ubiquitous `curl` command-line utility to demonstrate the operation of the submission status API.

# Obtaining a Token

Before one can use the API, one must first be able to authenticate to it, so that the server can identify the user. To obtain a token, a JSON Web Token (JWT) to be precise, a user must POST their NeMO submission credentials to the NeMO submission API with an HTTP POST. This is the same set of credentials that NeMO data submitters use to transfer their files via aspera. Example: 

`$ curl -X POST https://nemoarchive.org/api/login -d '{"username": "USERNAME", "password": "PASSWORD"}'`

where "USERNAME" and "PASSWORD" are replaced with the user's actual username and password.

> **Note:** We do not recommend putting the actual password in the terminal as most shells will save it in the shell's history. We encourage the use of environment variables and other techniques (beyond the scope of this document) to avoid the password being exposed.

## Login Failure

In the case of an authentication failure, the above command will simply display the following message:

`{"message":"Login failed."}`

## Sucessful login

If the credentials are accepted, a JSON message is returned that has the following structure:

```
{
  "message": "Successful login.",
  "jwt": "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX",
  "email": "jdoe@example.com",
  "first": "John",
  "last": "Doe",
  "expireAt": 12345678901
}
```

The value of the 'jwt' property should be noted, as it is this token that must be specified for future interactions with the API. The "expireAt" property contains the expiration time, after which, a new token will need to be obtained. Tokens have a lifetime of 24 hours.

# Retrieving Submission Status

Once a valid token has been generated, one can use it to retrieve one's submission history and status by issuing a GET request with it. The token must be specified with an HTTP "Authorization" header. Example:

`$ curl -X GET https://nemoarchive.org/api/submission -H "Authorization: Bearer XXXXXXXXXXXXXXXXXXX"`

In this example, one would replace the repeated X characters with the actual JWT token that was previously obtained. Do not share your token. Treat it as a sensitive piece of information, just as your password.

# Interpreting the results
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

The "total" property indicates how many submissions the API is aware of for the authenticated user.

Each "RESULT" in the list of results is itself a JSON object containing the details of the submission, and the result of each step of the ingest process.

```
{
  "id": "ABC1234",
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

The "id" property is simply the randomly generated identifier for the submission. All submissions are guaranteed to have a globally unique identifier among all submissions. The "creation" property will have the UTC timestamp of when the submission was initiated, which is the when the manifest was submitted via the nemoarchive.org website. The "complete" property will indicate whether the submission is considered complete or not. This will only be true if all the steps have completed, or if there was a failure. The "success" property indicates whether the submission successfully completed. If the submission is still progressing, or if there are any failures, the value will be false. The "policy" property, if set, will show whether the manifest indicated the presence of open data, restricted data, or embargoed data. If there was an error in the manifest validation step, the policy may not have been determined, and the property may not be present.

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

# Paging Through Results

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
