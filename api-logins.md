# API Logins

Most APIs at the NeMO Archive are secured with the use of [JWT](https://jwt.io) (Javascript Web Tokens). This document describes how users may obtain such a token so that they may use them in programs and scripts effectively and safely.

- [Obtaining a Token](#obtaining-a-token)
- [Login Failure](#login-failure)
- [Successful Login](#successful-login)
- [Token Handling](#token-handling)

## Obtaining a Token

Before one can use the API, one must first be able to authenticate to it, so that the server can identify the user. To obtain a token, a JSON Web Token (JWT) to be precise, a user must POST their NeMO submission credentials to the NeMO submission API with an HTTP POST. This is the same set of credentials that NeMO data submitters use to transfer their files via aspera. Example: 

`$ curl -X POST https://api.nemoarchive.org/v1/login -d '{"username": "USERNAME", "password": "PASSWORD"}'`

where "USERNAME" and "PASSWORD" are replaced with the user's actual username and password.

> **Note:** We do not recommend putting the actual password in the terminal as most shells will save it in the shell's history. We encourage the use of environment variables and other techniques (beyond the scope of this document) to avoid the password being exposed.

## Login Failure

In the case of an authentication failure, the above command will simply display the following message:

`{"message":"Login failed."}`

## Successful Login

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

## Token Handling

The value of the 'jwt' property is the actual JWT token. In this example, it is simply a series of "X" characters and is invalid, but a real JWT would be considerably longer and be composed of what look like random characters. It is this string that must be specified for future interactions with NeMO RESTful APIs that require authentication. The "expireAt" property contains the expiration time, after which, a new token will need to be obtained. Tokens have a lifetime of 24 hours.

Do not share your token! Treat it as a sensitive piece of information, just as your password. Similarly, be careful not to store the token in a file or script, publish it, or commit it to a version control system such as GitHub.
