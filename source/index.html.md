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
Handling privacy requests via email can be onerous and difficult to automate. The Opt-Out Machine provides API access to organizations that wish to create automation or integration with existing installed solutions.

### How it works
The Opt-Out Machine will still send an email message for each request. Included in the headers of the email message is a unique identifier field for the request under the comments header: `oom-request-id`. This identifier also appears at the bottom of the footer, where it is more easily human-readable.

The API can be used for fulfillment of requests. All of the information contained in the email can also be retrieved from the API. This enables systems to gather the request ID from the email message, then separately use a programmatic approach to collect the rest of the details without the need to parse them out of an email. Later, updates around acknowledgement and fulfillment of requests can be automated and sent to the API instead of corresponding via email.

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
You must replace <code>meowmeowmeow</code> with your API key.
</aside>

# Requests
## Get all Requests by date

```shell
curl "https://app.knownprivacy.com/api/v1/requests/20221216?page=2" \
  -H "Authorization: meowmeowmeow"
```

> The above command returns JSON structured like this:

```json
{
  "data": [
    {
      "file_count": 0,
      "id": "j4l35j4K",
      "request_types": ["access", "opt-out"],
      "opt_out_completed": false,
      "state": "active",
      "submitted_at": "2022-12-16T23:13:37.920Z",
      "acknowledged_at": "2022-12-16T23:20:01.842Z",
      "person": {
        "name": "Isabel Laurent",
        "dob": "1950-11-06",
        "email": "isabellaurent@yahoo.com",
        "email_addresses": ["il@gmail.com"],
        "phone": "+19712708285",
        "address": "72 County Road 10",
        "address2": null,
        "city": "Sunnyville",
        "state": "NY",
        "postal_code": "04822",
        "identity_verification_proof": {
          "url": "https://app.knownprivacy.com/documents/23fcfb9a-0d8a-4685-86db-7cedbbac49aa",
          "access_code": "RM6_CmfO",
          "valid_until": "2023-02-02T23:13:37.920Z",
        }
      }
    },
    {
      "file_count": 0,
      "id": "46D4gad4",
      "request_types": ["access", "opt-out"],
      "opt_out_completed": false,
      "state": "active",
      "submitted_at": "2022-12-16T23:13:37.920Z",
      "acknowledged_at": null,
      "person": {
        "name": "James Hunt",
        "dob": "1984-01-11",
        "email": "jameshunt@gmail.com",
        "email_addresses": [],
        "phone": "+13035551212",
        "address": "35 Oak Ct",
        "address2": null,
        "city": "Concord",
        "state": "NH",
        "postal_code": "10292",
        "identity_verification_proof": {
          "url": "https://app.knownprivacy.com/documents/23fcfb9a-0d8a-4685-86db-7cedbbac49aa",
          "access_code": "2Mh_Y9bb",
          "valid_until": "2023-02-02T23:13:37.920Z",
        }
      }
    }
  ],
  "page_number": 2,
  "total_entries": 351,
  "total_pages": 8
}
```

This endpoint retrieves all requests that were sent on a given date.

### HTTP Request

`GET https://app.knownprivacy.com/api/v1/requests/<date>`

### Query Parameters

Parameter | Required | Example | Description
--------- | ------- | ------- | -----------
date | true | 20221219 | Indicates which day of requests to retrieve.
page | false | 2 | If excluded, the result will include the first 50 records.


## Get a Specific Request

```shell
curl "https://app.knownprivacy.com/api/v1/requests/j4l35j4K" \
  -H "Authorization: meowmeowmeow"
```

> The above command returns JSON structured like this:

```json
{
  "data": {
    "file_count": 0,
    "id": "j4l35j4K",
    "request_types": ["access", "opt-out"],
    "opt_out_completed": false,
    "state": "active",
    "submitted_at": "2022-12-16T23:13:37.920Z",
    "acknowledged_at": "2022-12-16T23:20:01.842Z",
    "person": {
      "name": "Isabel Laurent",
      "dob": "1950-11-06",
      "email": "isabellaurent@yahoo.com",
      "email_addresses": ["il@gmail.com"],
      "phone": "+19712708285",
      "address": "72 County Road 10",
      "address2": null,
      "city": "Sunnyville",
      "state": "NY",
      "postal_code": "04822",
      "identity_verification_proof": {
        "url": "https://app.knownprivacy.com/documents/23fcfb9a-0d8a-4685-86db-7cedbbac49aa",
        "access_code": "RM6_CmfO",
        "valid_until": "2023-02-02T23:13:37.920Z",
      }
    }
  }
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
curl "https://app.knownprivacy.com/api/v1/ack" \
  -X POST \
  -H "Content-Type: application/json" \
  -H "Authorization: meowmeowmeow"
  -d '{"request_id": "j4l35j4K"}'
```

> The above command returns JSON structured like this:

```json
{
  "data": {
    "success": true
  }
}
```

This endpoint creates an acknowledgement for a specific `request`. This will simply assign an acknowledgement to the request for the purposes of both the customer's information and for the company's compliance record.

### HTTP Request

`POST https://app.knownprivacy.com/api/v1/requests/ack`

### Parameters

Parameter | Description
--------- | -----------
ID | The ID of the request to acknowledge




## Update a Specific Request

```shell
curl "https://app.knownprivacy.com/api/v1/requests/j4l35j4K" \
  -X PATCH \
  -H "Content-Type: application/json" \
  -H "Authorization: meowmeowmeow"
  -d '{"opt_out_completed": "true", "state": "fulfilled_with_data", "comments": "Please find personal data file sent separately."}'
```

> The above command returns JSON structured like this:

```json
{
  "data": {
    "file_count": 0,
    "id": "j4l35j4K",
    "request_types": ["access", "opt-out"],
    "opt_out_completed": true,
    "state": "fulfilled_with_data",
    "submitted_at": "2022-12-16T23:13:37.920Z",
    "acknowledged_at": "2022-12-16T23:20:01.842Z",
    "person": {
      "name": "Isabel Laurent",
      "dob": "1950-11-06",
      "email": "isabellaurent@yahoo.com",
      "email_addresses": ["il@gmail.com"],
      "phone": "+19712708285",
      "address": "72 County Road 10",
      "address2": null,
      "city": "Sunnyville",
      "state": "NY",
      "postal_code": "04822",
      "identity_verification_proof": {
        "url": "https://app.knownprivacy.com/documents/23fcfb9a-0d8a-4685-86db-7cedbbac49aa",
        "access_code": "RM6_CmfO",
        "valid_until": "2023-02-02T23:13:37.920Z",
      }
    }
  }
}
```

This endpoint updates a specific `request`. Fields that may be updated include `opt_out_completed` and `state`.

### HTTP Request

`PUT/PATCH https://app.knownprivacy.com/api/v1/requests/<ID>`

### Parameters

Parameter | Description
--------- | -----------
opt_out_completed | Boolean string indicating whether an opt-out was fulfilled.
comments | Comments or follow up questions regarding the request. May be up to 250 characters in length.
state | The status of the request. Values may include: `failed`, `failed_on_id_verification`, `failed_on_legality`, `fulfilled`, `fulfilled_no_record`, `fulfilled_with_data`


## Update Multiple Requests (bulk update)

```shell
curl "https://app.knownprivacy.com/api/v1/requests" \
  -X PATCH \
  -H "Content-Type: application/json" \
  -H "Authorization: meowmeowmeow"
  -d '[
      {"id": "j4l35j4K", "opt_out_completed": "true", "state": "fulfilled_with_data"},
      {"id": "46m4b74f", "opt_out_completed": "true", "state": "fulfilled_with_data"}
  ]'
```

> The above command returns JSON structured like this:

```json
{
  "data": [
    {"id": "j4l35j4K", "update_received": "true", "errors": []},
    {"id": "46m4b74f", "update_received": "true", "errors": []}
  ]
}
```

This endpoint accepts updates for multiple `request` records at once. Fields that may be updated for each request include `opt_out_completed` and `state`.

### HTTP Request

`PUT/PATCH https://app.knownprivacy.com/api/v1/requests`

### Item Parameters

Parameter | Description
--------- | -----------
id | Request ID to be updated.
opt_out_completed | Boolean string indicating whether an opt-out was fulfilled.
comments | Comments or follow up questions regarding the request. May be up to 250 characters in length.
state | The status of the request. Values may include: `failed`, `failed_on_id_verification`, `failed_on_legality`, `fulfilled`, `fulfilled_no_record`, `fulfilled_with_data`


# Files
## Upload a file for a given request

```shell
curl "https://app.knownprivacy.com/api/v1/files" \
  -X POST \
  -H "Content-Type: application/json" \
  -H "Authorization: meowmeowmeow"
  -d '{"request_id": "j4l35j4K"}'
```

> The above command returns JSON structured like this:

```json
{
  "data": {
    "success": true
  }
}
```

This endpoint enables data uploads pertaining to a particular "right-to-know" or "access" request.

### HTTP Request

`POST https://app.knownprivacy.com/api/v1/files`

### Parameters

Parameter | Description
--------- | -----------
request_id | The ID of the request to associate with the file
file | The data payload to be sent