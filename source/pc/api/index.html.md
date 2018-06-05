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

## Refresh Status

### HTTP Request
`POST /v1/devices/$DEVICE_ID/status`

## Refresh the Device Status

```sh
curl -s -X POST https://api.automategreen.com/v1/devices/$DEVICE_ID/status \
     -H "Authorization: Bearer $TOKEN" \
     -H "Content-Type: application/json"
```

Manually request the Power Controller to send an updated status to the cloud.  The status will be based on the averages since the last status.

<aside class="notice">
This API request is not needed for normal operation.  The device will automatically send status updates every 15 minutes.
</aside>

# Sending Commands

### HTTP Request
`POST /v1/devices/$DEVICE_ID/command`

## Turn load on

```sh
curl -s -X POST https://api.automategreen.com/v1/devices/$DEVICE_ID/command \
     -H "Authorization: Bearer $TOKEN" \
     -H "Content-Type: application/json" \
     -d "{\"command\": {\"name\": \"on\"}}"
```

## Turn load off

```sh
curl -s -X POST https://api.automategreen.com/v1/devices/$DEVICE_ID/command \
     -H "Authorization: Bearer $TOKEN" \
     -H "Content-Type: application/json" \
     -d "{\"command\": {\"name\": \"off\"}}"
```

## Turn load on in 60 seconds (delayed on)

```sh
curl -s -X POST https://api.automategreen.com/v1/devices/$DEVICE_ID/command \
     -H "Authorization: Bearer $TOKEN" \
     -H "Content-Type: application/json" \
     -d "{\"command\": {\"name\": \"on\", \"delay\": 60}}"
```

## Turn load off in 60 seconds (delayed off)

```sh
curl -s -X POST https://api.automategreen.com/v1/devices/$DEVICE_ID/command \
     -H "Authorization: Bearer $TOKEN" \
     -H "Content-Type: application/json" \
     -d "{\"command\": {\"name\": \"off\", \"delay\": 60}}"
```

## Turn load on for 60 seconds (toggles off after 60 seconds)

```sh
curl -s -X POST https://api.automategreen.com/v1/devices/$DEVICE_ID/command \
     -H "Authorization: Bearer $TOKEN" \
     -H "Content-Type: application/json" \
     -d "{\"command\": {\"name\": \"on_for\", \"time\": 60}}"
```

## Turn load off for 60 seconds (toggles on after 60 seconds)

```sh
curl -s -X POST https://api.automategreen.com/v1/devices/$DEVICE_ID/command \
     -H "Authorization: Bearer $TOKEN" \
     -H "Content-Type: application/json" \
     -d "{\"command\": {\"name\": \"off_for\", \"time\": 60}}"
```

## Limit power for 1 hour to 50 watt*hours

```sh
curl -s -X POST https://api.automategreen.com/v1/devices/$DEVICE_ID/command \
     -H "Authorization: Bearer $TOKEN" \
     -H "Content-Type: application/json" \
     -d "{\"command\": {\"name\": \"limit_power\", \"power\": 50, \"time\": 3600} }"
```

# Configuring

### HTTP Request
`PUT /v1/devices/$DEVICE_ID`


## Configure FFR

### Enable FFR

```sh
curl -s -X PUT https://api.automategreen.com/v1/devices/$DEVICE_ID \
     -H "Authorization: Bearer $TOKEN" \
     -H "Content-Type: application/json" \
     -d "{\"info\": {\"ffr\":{\"enable\":true,\"tripThreshold\":59.7,\"returnThreshold\":59.8,\"periods\":10,\"delay\":10} } }"
```

### Disable FFR

```sh
curl -s -X PUT https://api.automategreen.com/v1/devices/$DEVICE_ID \
     -H "Authorization: Bearer $TOKEN" \
     -H "Content-Type: application/json" \
     -d "{\"info\": {\"ffr\":{\"enable\":false} } }"
```

## Configure Flowmeter

### No Flowmeter

```sh
curl -s -X PUT https://api.automategreen.com/v1/devices/$DEVICE_ID \
     -H "Authorization: Bearer $TOKEN" \
     -H "Content-Type: application/json" \
     -d "{\"info\": {\"flowmeter\":{\"type\":\"none\"} } }"
```

### TUF2000M Clamp-on Flowmeter

```sh
curl -s -X PUT https://api.automategreen.com/v1/devices/$DEVICE_ID \
     -H "Authorization: Bearer $TOKEN" \
     -H "Content-Type: application/json" \
     -d "{\"info\": {\"flowmeter\":{\"type\":\"tuf2000m\", \"pipeOuterDiameter\": 22.225, \"pipeWallThickness\": 0.8128, \"pipeType\": \"copper\"} } }"
```

### EUF4315K Clamp-on Flowmeter

```sh
curl -s -X PUT https://api.automategreen.com/v1/devices/$DEVICE_ID \
     -H "Authorization: Bearer $TOKEN" \
     -H "Content-Type: application/json" \
     -d "{\"info\": {\"flowmeter\":{\"type\":\"euf4315k\"} }"
```

## Configure Load Capacity

### Enable Capacity

```sh
curl -s -X PUT https://api.automategreen.com/v1/devices/$DEVICE_ID \
     -H "Authorization: Bearer $TOKEN" \
     -H "Content-Type: application/json" \
     -d "{\"info\": {\"capacity\":{\"enable\":true, \"maxCapacity\":100, \"minCapacity\":50, \"lossPerLiter\":5.5, \"lossPerIdleMinute\":1.3, \"gainPerWattHour\":2.7} } }"
```

### Disable Capacity

```sh
curl -s -X PUT https://api.automategreen.com/v1/devices/$DEVICE_ID \
     -H "Authorization: Bearer $TOKEN" \
     -H "Content-Type: application/json" \
     -d "{\"info\": {\"capacity\":{\"enable\":false} } }"
```


# Statuses

The status objects have had two parameters added to track the type of update and reason for the update. There are three types of status for the power controller: update, relay, trigger.

### Update Status Type

```json
{
  "id": "xyz890ABC",
  "deviceId": "abc123def",
  "date": "2018-05-03T12:22:58.807Z",
  "state": "Active",
  "type": "update",
  "reason": "auto", // either 'auto' or 'api'
  "info": {
    "duration": 900, // Duration of the sensor sampling used for averaging (sec)
    "current": 10.5, // Average current (A)
    "voltage": 243.6, // Average voltage (V)
    "power": 639.45, // Total power used during duration (Wh)
    "temperature": 75.2, // Average abetment temperature (F)
    "frequency": 60.01, // Average frequency (Hz)
    "flow": 10.0, // Average rate of flow (L/min)
    "on": true, // is the load state
    "toggleIn": 0 // number of seconds until toggling relay state (0 means never)
  }
}
```

The update status is sent periodicity (auto) or when a status request is issued via the API (api). The update status will provides the sensor data and current relay state. In addition the time (duration) used to calculate the averages is also provided. For the automatic update statuses, the duration will be 900 seconds (15 minutes). For API status updates, the duration will vary based on the time sense the last update status.

### Relay Status Type

```json
{
  "id": "xyz890ABC",
  "deviceId": "abc123def",
  "date": "2018-05-03T12:22:58.807Z",
  "state": "Active",
  "type": "relay",
  "reason": "reboot", // either 'reboot', 'timer', 'ffr', 'api', or 'manual'
  "info": {
    "on": true, // is the load state
    "toggleIn": 0 // number of seconds until toggling relay state (0 means never)
  }
}
```

The relay status records when the relay was turned on or off and what turned it on or off.


## Trigger Status Type

```json
{
  "id": "xyz890ABC",
  "deviceId": "abc123def",
  "date": "2018-05-03T12:22:58.807Z",
  "state": "Active",
  "type": "trigger",
  "reason": "ffr", // 'ffr'
  "info": {
    "frequency": 59.68, // Frequency at time of trigger (Hz)
  }
}
```

The trigger status records when a trigger was invoked and why it what trigger was invoked.


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
