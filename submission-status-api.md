# Overview

The NeMO Archive exposes a read-only REST API for users that wish to create scripts and automate the retrieval of the status of their data submissions. Since the API is RESTful, any modern programming language that has a suitable HTTP library should be able to easily obtain the data. Results are returned in JSON format to make the parsing of the data easier. In the examples provided here, we will be using the fairly ubiquitous `curl` command-line utility to demonstrate the operation of the submission status API.

# Obtaining a Token

Before we can use the API, we must first be able to authenticate to it, so that the server can identify the user. To obtain a token, a JSON Web Token (JWT) to be precise, a user must POST their NeMO submission credentials to the NeMO submission API with an HTTP POST. This is the same set of credentials that NeMO data submitters use to transfer their files via aspera. Example: 

`$ curl -X POST https://nemoarchive.org/api/login.php  -d '{"username": "USERNAME", "password": "PASSWORD"}'`

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

`$ curl -X GET https://nemoarchive.org/api/submission.php -H "Authorization: Bearer XXXXXXXXXXXXXXXXXXX"`

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

# Paging Through Results

The submission API paginates submissions 100 at a time. Therefore, if the user has made more than 100 submissions, multiple requests will need to be issued to fetch the entirety of the data. The user will alerted to the need to paginate by the presence of a "next" key in the returned JSON document. When paging through the results, the "page" property will also indicate the "page", or slice of data, that has been retrieved. Example:

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
  "next": "https://nemoarchive.org/api/submission.php?page=2"
}
```
