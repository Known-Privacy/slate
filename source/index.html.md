---
title: Opt-Out Machine API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell

toc_footers:
  - <a href='https://github.com/slatedocs/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true

code_clipboard: true

meta:
  - name: description
    content: Documentation for the Opt-Out Machine API
---

# Introduction
Handling privacy requests via email can be onerous and difficult to automate. The Opt-Out Machine provides API access to organizations that wish to make use of it.

### How it works
The Opt-Out Machine will still send an email message for each request. Included in the headers of the email message is a unique identifier field for the request under the comments header: `oom-request-id`. This identifier also appears at the bottom of the footer, where it is more easily human-readable.

All of the information contained in the email can also be retrieved from the API. This enables systems to simply gather the request ID from the email message, then separately use a programmatic approach to collect the rest of the details without the need to parse it out of an email.

# Authentication

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here" \
  -H "Authorization: meowmeowmeow"
```

> Make sure to replace `meowmeowmeow` with your API key.

Opt-Out Machine uses API keys to allow access to the API. Sandbox and production keys will be delivered manually.

Opt-Out Machine expects for the API key to be included in all API requests to the server in a header that looks like the following:

`Authorization: meowmeowmeow`

<aside class="notice">
You must replace <code>meowmeowmeow</code> with your personal API key.
</aside>

# Requests
## Get a Specific Request

```shell
curl "https://app.knownprivacy.com/api/v1/requests/j4l35j4K" \
  -H "Authorization: meowmeowmeow"
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "name": "Max",
  "breed": "unknown",
  "fluffiness": 5,
  "cuteness": 10
}
```

This endpoint retrieves a specific `request`.

### HTTP Request

`GET https://app.knownprivacy.com/api/v1/requests/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the request to retrieve

## Acknowledge a Specific Request

```shell
curl "https://app.knownprivacy.com/api/v1/requests/j4l35j4K/ack" \
  -X POST \
  -H "Authorization: meowmeowmeow"
```

> The above command returns JSON structured like this:

```json
{
  "success": true
}
```

This endpoint creates an acknowledgement for a specific `request`. This will simply assign an acknowledgement to the request for the purposes of both the customer's information and for the compliance record for the company.

### HTTP Request

`POST https://app.knownprivacy.com/api/v1/requests/<ID>/ack`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the request to acknowledge

