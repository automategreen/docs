---
title: API Reference

toc_footers:
  - <a href='/sensors/'>Sensor Hub Documentation</a>
  - <a href='https://app.automategreen.com/'>Beta Login</a>

includes:
  - errors

search: true
---

# Introduction

This document is the Automate Green API interface specification.

<aside class="notice">
Please help fix typos and errors by submitting a pull request on <a href="https://github.com/automategreen/docs">GitHub</a>. 
</aside>



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


## Use Token

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

# Gateways

## List Gateways

### HTTP Request
`GET /v1/gateways`


## Get Gateway

### HTTP Request
`GET /v1/gateways/$GATEWAY_ID`


## Update Gateway

### HTTP Request
`PUT /v1/gateways/$GATEWAY_ID`


## Delete Gateway

### HTTP Request
`DELETE /v1/gateways/$GATEWAY_ID`

## Reload Gateway

### HTTP Request
`GET /v1/gateways/$GATEWAY_ID/reload`

# Devices

## List Devices


```shell
 curl https://api.automategreen.com/v1/devices \
-H "Authorization: Bearer $TOKEN"
```


> Response

```json
{
  "devices": [
    {
      "address": "112233",
      "gateway": "aSdFgHjKl",
      "id": "qWeRtYuIoP",
      "info": {
        "ledBrightness": 32,
        "links": [],
        "onLevel": 100,
        "rampRate": 500
      },
      "lastUpdate": "2015-07-17T23:50:45.666Z",
      "name": "Master Bedroom",
      "status": {
        "date": 1436312185098,
        "deviceId": "qWeRtYuIoP",
        "id": "-13SKLb9fg",
        "info": {
          "level": 0
        },
        "state": "Active"
      },
      "type": {
        "description": "Switch",
        "name": "switch",
        "profile": "switch"
      }
    }
  ]
}
```



### HTTP Request
`GET /v1/devices`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
gateway | ALL | Gateway for which to get devices



## Get Device

### HTTP Request
`GET /v1/devices/$DEVICE_ID`


## Command Device

### HTTP Request
`POST /v1/devices/$DEVICE_ID/command`



## Update Device

### HTTP Request
`PUT /v1/devices/$DEVICE_ID`



## Delete Device

### HTTP Request
`DELETE /v1/devices/$DEVICE_ID`


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
Dates should be in ISO 8601 formated strings.
</aside>

# Actions

## List Actions

```sh
curl https://api.automategreen.com/v1/actions \
-H "Authorization: Bearer $TOKEN"
```
```sh
GET /v1/actions
GET /v1/actions?device=$DEVICE_ID
```

### HTTP Request
`GET /v1/actions`


### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
device | all | Device for which to get actions

## Get Action

```sh
curl https://api.automategreen.com/v1/actions/WJNShxzKNl \
-H "Authorization: Bearer $TOKEN"
```

> Response

```json
{
  "action":{
    "id":"WJNShxzKNl",
    "device":"bksxGL4wMx",
    "last":"2015-08-14T19:34:12.228Z",
    "count":1,
    "type":"request",
    "event":"cooling",
    "options":{
      "url":"https://maker.ifttt.com/trigger/bksxGL4wMx.cooling/with/key/bFbh7mj_PeM6ppxW1_Tq7H"
    }
  }
}
```

### HTTP Request
`GET /v1/actions/$ACTION_ID`



## Create Action

```sh
curl https://api.automategreen.com/v1/actions \
-H "Authorization: Bearer $TOKEN" \
-d "device=bksxGL4wMx" \
-d "type=request" \
-d "event=cooling" \
-d "options[url]=https://maker.ifttt.com/trigger/bksxGL4wMx.cooling/with/key/bFbh7mj_PeM6ppxW1_Tq7H"
```

> Response

```json
{
  "action":{
    "id":"WJNShxzKNl",
    "device":"bksxGL4wMx",
    "type":"request",
    "event":"cooling",
    "options":{
      "url":"https://maker.ifttt.com/trigger/bksxGL4wMx.cooling/with/key/bFbh7mj_PeM6ppxW1_Tq7H"
    }
  }
}
```

### HTTP Request
`POST /v1/actions`

#### Request Arguments

### Body Parameters

Parameter | Type | Description
--------- | ------- | -----------
device | string | ID of device for which to associate the actions
type | string | ID of device for which to associate the actions
event | string | Event that triggers the action
options | object | Options for the action
conditions | [object] | Conditions to validate prior to action
threshold | object | Event trigger threshold
injectStatus | boolean | Inject current device status (request only)
injectDeviceId | boolean | Inject device ID (request only)
deviceToControlId | string | ID of device to control (command only)
disabled | boolean | Is the action disabled

### Types
- request
- command

### Events
#### Light/Switch Events
- **turnOn** - Triggered when switch is turned on
- **turnOff** - Triggered when switch is turned off

#### Thermostat Events
- **heating** - Triggered when thermostat starts heating
- **cooling** - Triggered when thermostat starts cooling
- **off** - Triggered when thermostat stops heating or cooling

#### Door Events
- **opened** - Triggered when contact sensor is opened
- **closed** - Triggered when contact sensor is closed

#### Motion Events
- **motion** - Triggered when motion is detected
- **clear** - Triggered when motion ends

#### Leak Events
- **wet** - Triggered when leak is detected
- **dry** - Triggered when leak is removed

#### Flow Event
- **flow** - Triggered when flow changes.  *Optional supports thresholds*
- **turnOn** - Triggered when flow is detected
- **turnOff** - Triggered when flow stops

#### Temperature Event
- **temp** - Triggered when temperature changes.  *Optional supports thresholds*

#### Current Event
- **current** - Triggered when current changes.  *Optional supports thresholds*
- **turnOn** - Triggered when current is detected
- **turnOff** - Triggered when current stops

#### Light/Photo Event
- **resistance** - Triggered when resistance in photocell changes.  *Optional supports thresholds*
- **dark** - Triggered when resistance goes above light threshold
- **light** - Triggered when resistance goes below light threshold

### Thresholds

Some events support thresholds.  The event will only be triggered if the threshold is reached.

- **goesAbove**
- **goesBelow**
- **isAbove**
- **isBelow**

### Options
#### Request Options
- **url**
- **method**

### Condition objects

> Time of Day Condition

```json
{
  "type": "timeOfDay",
  "start": "hh:mm [AM/PM]",
  "end": "hh:mm [AM/PM]"
}
```


> Day of Week Condition

```json
{
  "type": "dayOfWeek",
  "sunday": true,
  "monday": true,
  "tuesday": true,
  "wednesday": true,
  "thursday": true,
  "friday": true,
  "saturday": true
}
```

The action conditions are passed as an array of objects, `[{...},{...},...]`. All conditions must be true for the action to be triggered. There are three types of conditions.

#### Time of Day

Only trigger the action when between a `start` and `end` time.

#### Day of Week

Each day of the week can be set to a boolean.  `true` for days the action should be triggered.  If a day is omitted, it defaults to `false`.

#### Device status

Only trigger the action when a devices has a current status.


## Update Action

```sh
curl -X PUT https://api.automategreen.com/v1/actions/WJNShxzKNl \
-H "Authorization: Bearer $TOKEN" \
-d "device=bksxGL4wMx" \
-d "type=request" \
-d "event=heating" \
-d "options[url]=https://maker.ifttt.com/trigger/bksxGL4wMx.cooling/with/key/bFbh7mj_PeM6ppxW1_Tq7H"
```

### HTTP Request
`POST /v1/actions/$ACTION_ID`

## Delete Action

```sh
curl -X DELETE https://api.automategreen.com/v1/actions/WJNShxzKNl \
-H "Authorization: Bearer $TOKEN"
```

### HTTP Request
`DELETE /v1/actions/$ACTION_ID`




