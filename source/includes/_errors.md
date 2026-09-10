# Errors

The Learn Amp API uses the following error codes:


Error Code | Meaning | Description
---------- | ------- | -----------
400 | Bad Request | Your request is invalid.
401 | Unauthorized | Your API key is wrong.
403 | Forbidden | You do not have sufficient permissions to access the resource.
404 | Not Found | The specified resource could not be found.
406 | Not Acceptable | You requested a format that isn't json.
410 | Gone | The resources has been removed from our servers.
429 | Too Many Requests | You're requesting too many API calls.
500 | Internal Server Error | We had a problem with our server. Try again later.
503 | Service Unavailable | We're temporarily offline for maintenance. Please try again later.

## Validation Errors

> 400 Bad request - two parameters rejected on `POST /v1/users`:

```json
{
    "error": "firstName is missing, role must be one of: viewer, curator, reporter, learning_designer, hr, admin",
    "fullErrors": {
        "firstName": [
            "is missing"
        ],
        "role": [
            "must be one of: viewer, curator, reporter, learning_designer, hr, admin"
        ]
    }
}
```

A `400` caused by a rejected parameter returns both a summary and a per-parameter breakdown:

Field | Description
----- | -----------
`error` | Every problem in one string, joined with commas. Useful for logging.
`fullErrors` | An object keyed by parameter name, each holding an array of that parameter's problems. Use this to attribute a failure to a field.

A parameter can carry more than one problem, and more than one parameter can be rejected in a single response, so treat both the values in `fullErrors` and its keys as lists.

<aside class="notice">Nothing is written when a request fails validation — the whole payload is rejected, not the valid parts of it. Correct the request and send it again.</aside>

Where a parameter accepts a fixed set of values, the message names that set, so you can diagnose a rejected value from the response without consulting this page. The accepted values differ by endpoint — filtering users by role accepts `owner`, for instance, while setting a user's role does not — so trust the message over a list you have cached.

<aside class="warning">Match on the status code and the <code>fullErrors</code> keys, not on message text. The wording is there for a human reading a log, and it changes as validation changes.</aside>
