---
title: Power Controller API Reference

includes:
  - errors

search: true
---

# Introduction

This document is the Automate Green API interface specification for the Power Controller.

# Authentication

## Generate Token

> To generate an auth token, use this code:

```shell
curl https://api.automategreen.com/v1/signin \
-d "user[email]=user@example.com" \
-d "user[password]=mypassword" \
-d "expires=31536000"
```

> You can pass the email and password parameters as either standard form data (above) or as JSON (below):

```shell
curl https://api.automategreen.com/v1/signin \
-d '{"user":{"email":"user@example.com","password":"mypassword"}}' \
-H "Content-Type: application/json"
```

> The response is a JSON with a token:

```json
{
    "message": "Sign in successful",
    "token": "vgdBwL149wfE8upzCY4PiaQOQZsrcIAV4fC1.Zl3vKu5bKhkdBJoCBbJ4ujqMtpvgdBwL149wfE8upzCY4PiaQOQZsrcIAV4fC1xZl3vKu5b.hkdBJoCBbJ4ujqMtpvgdBwL149wfE8upzCY4PiaQOQZ",
    "user": {
        "email": "user@example.com",
        "name": "Example User"
    }
}
```

> To make the token easier to work with, set an environment variable with the token.

```shell
export TOKEN="vgdBwL149wfE8upzCY4PiaQOQZsrcIAV4fC1.Zl3vKu5bKhkdBJoCBbJ4ujqMtpvgdBwL149wfE8upzCY4PiaQOQZsrcIAV4fC1xZl3vKu5b.hkdBJoCBbJ4ujqMtpvgdBwL149wfE8upzCY4PiaQOQZ"
```


You will need to generate an authorization token to use the API.  This allows your API requests to securely access your devices.  You can either generate a token through an HTTPS API request or create one on the Web App (User -> Tokens -> Generate New Token).

In the request, you provide your email, password and expires. Expires is the number of seconds you want the token to be valid. Expires defaults to two week.  The examples sets expires to a year.


## Using Your Token

> Token in the HTTP Authorization header:

```shell
curl https://api.automategreen.com/...
-H "Authorization: Bearer $TOKEN"
```

> Token in the URL query string:

```shell
curl https://api.automategreen.com/.../?access_token=$TOKEN
```

> Token in the request body:

```shell
curl https://api.automategreen.com/...
-d "access_token=$TOKEN"
```

All Automate Green API requests require a valid token.  There are three ways to send your access token in a request.

- HTTP Authorization header (always works)

`Authorization: Bearer $TOKEN`

- URL query string (only works with GET requests)

`?access_token=$TOKEN`

- Request body (only works for POST & PUT when body is URL-encoded)

`access_token=$TOKEN`


<aside class="notice">
You must replace <code>$TOKEN</code> with your API token.
</aside>

# Power Controller Devices

## Add Power Controller Device

```sh
curl -s -X POST https://api.automategreen.com/v1/devices \
     -d "sn=PC-ZXCVBNMASDF-001" \
     -d "country=US" \
     -H "Authorization: Bearer $TOKEN"
```

> Response

```json
{
  "device": {
    "id": "6ItNKFLm_mS",
    "address": "1234567890qwertyuiopasdf",
    "sn": "PC-ZXCVBNMASDF-001",
    "country": "US",
    "type": {
      "id": "power-controller-v10",
      "profile": "power-controller",
      "desc": "Power Controller"
    },
    "status": {
      "state": "Initializing"
    }
  }
}
```

To add a Power Controller the serial number and country must be provided in the API body.

### HTTP Request
`POST /v1/devices`


### Body Parameters

Parameter | Type | Description
--------- | ------- | -----------
sn | string | Serial number of Power Controller to add
country | string | The country in which the Power Controller's SIM should be activated

## List Power Controller Devices

```shell
 curl https://api.automategreen.com/v1/devices \
-H "Authorization: Bearer $TOKEN"
```

> Response

```json
{
  "devices": [
    {
      "device": {
        "id": "6ItNKFLm_mS",
        "address": "1234567890qwertyuiopasdf",
        "sn": "PC-ZXCVBNMASDF-001",
        "country": "US",
        "type": {
          "id": "power-controller-v10",
          "profile": "power-controller",
          "desc": "Power Controller"
        },
        "status": {
          "state": "Active",
          "info": {
            "duration": 900,
            "current": 10,
            "voltage": 238.35,
            "power": 590.59,
            "temperature": 92.7,
            "frequency": 60.01,
            "flow": null,
            "on": true,
            "toggleIn": 0
          },
          "date": "2018-05-08T15:03:15.000Z"
        }
      }
    }
  ]
}
```

List the Power Controllers for your account.  This request will include all devices for your account not just the Power Controllers.

### HTTP Request
`GET /v1/devices`

## Get a Device

```shell
 curl https://api.automategreen.com/v1/devices \
-H "Authorization: Bearer $TOKEN"
```

> Response

```json
{
  "device": {
    "id": "6ItNKFLm_mS",
    "address": "1234567890qwertyuiopasdf",
    "sn": "PC-ZXCVBNMASDF-001",
    "country": "US",
    "type": {
      "id": "power-controller-v10",
      "profile": "power-controller",
      "desc": "Power Controller"
    },
    "status": {
      "state": "Active",
      "info": {
        "duration": 900,
        "current": 10,
        "voltage": 238.35,
        "power": 590.59,
        "temperature": 92.7,
        "frequency": 60.01,
        "flow": null,
        "on": true,
        "toggleIn": 0
      },
      "date": "2018-05-08T15:03:15.000Z"
    }
  }
}
```
Get the details for a single Power Controller.  The status is the last known status for the Power Controller.

### HTTP Request
`GET /v1/devices/$DEVICE_ID`

# Sending Commands

### HTTP Request
`POST /v1/devices/$DEVICE_ID/command`



# Configuring

### HTTP Request
`PUT /v1/devices/$DEVICE_ID`


# Statuses

## List Statuses

```
GET /v1/statuses?device=$DEVICE_ID
GET /v1/statuses?device=$DEVICE_ID&date=$DATE
GET /v1/statuses?device=$DEVICE_ID&date[start]=$START_DATE&date[end]=$END_DATE
```
```sh
curl -g https://api.automategreen.com/v1/statuses?device=bksxGL4wMx&date[start]=2015-08-10T09:30&date[end]=2015-08-15 \
-H "Authorization: Bearer $TOKEN"
```

Returns a list of device statuses for the provided date range.

<aside class="notice">
The date range for a single request is limited to 10 days.
</aside>

### HTTP Request
`GET /v1/statuses`


### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
device | **REQUIRED** | Device for which to get statuses
date[start] | Yesterday | Return statuses starting from this date
date[end] | Now | Return statuses before this date
date | Yesterday | Simplified date returning all status from this date to now


<aside class="notice">
Dates must be in ISO 8601 formated strings.
</aside>
