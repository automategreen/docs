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

You will need to generate an authorization token to use the API. This allows your API requests to securely access your devices. You can either generate a token through an HTTPS API request or create one on the Web App (User -> Tokens -> Generate New Token).

In the request, you provide your email, password and expires. Expires is the number of seconds you want the token to be valid. Expires defaults to two week. The examples sets expires to a year.

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

All Automate Green API requests require a valid token. There are three ways to send your access token in a request.

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

> Example Request:

```sh
curl -X POST https://api.automategreen.com/v1/devices \
     -d "sn=PC-ZXCVBNMASDF-001" \
     -d "country=US" \
     -H "Authorization: Bearer $TOKEN"
```

> Response:

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

#### HTTP Request

`POST /v1/devices`

#### Body Parameters

| Parameter | Type   | Description                                                         |
| --------- | ------ | ------------------------------------------------------------------- |
| sn        | string | Serial number of Power Controller to add                            |
| country   | string | The country in which the Power Controller's SIM should be activated |

## List Power Controller Devices

> Example Request:

```shell
curl https://api.automategreen.com/v1/devices \
-H "Authorization: Bearer $TOKEN"
```

> Response:

```json
{
  "devices": [
    {
      "device": {
        "id": "6ItNKFLm_mS",
        "address": "1234567890qwertyuiopasdf",
        "sn": "PC-ZXCVBNMASDF-001",
        "country": "US",
        "type": { ... },
        "status": { ... },
        "info": { ... }
      }
    }
  ]
}
```

List the Power Controllers for your account. This request will include all devices for your account not just the Power Controllers.

#### HTTP Request

`GET /v1/devices`

## Get a Device

> Example Request:

```shell
 curl https://api.automategreen.com/v1/devices \
-H "Authorization: Bearer $TOKEN"
```

> Response:

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
        "current": 0,
        "voltage": 122.2,
        "power": 0,
        "temperature": 99,
        "frequency": 60.01,
        "flow": null,
        "on": false,
        "toggleIn": 0,
        "toggleAt": 0,
        "firmware": 80
      },
      "date": "2018-09-26T13:03:27.000Z"
    },
    "info": {
      "ffr": {
        "enabled": false,
        "tripThreshold": 59.7,
        "returnThreshold": 60,
        "periods": 10,
        "maxBackOff": 10
      },
      "capacity": {
        "enabled": false,
        "lossPerLiter": 0,
        "lossPerIdleMinute": 0,
        "gainPerWattHour": 0,
        "maxCapacity": 0,
        "minCapacity": 0
      },
      "flowmeter": {
        "type": "none",
        "pipeOuterDiameter": 22.2,
        "pipeWallThickness": 1.1,
        "pipeType": "copper"
      },
      "powerLimit": {
        "endsAt": 0,
        "power": 0.25
      }
    }
  }
}
```

Get the details for a single Power Controller. The status is the last known status for the Power Controller. The info is the current known configuration on the controller.

#### HTTP Request

`GET /v1/devices/$DEVICE_ID`

# Refresh Status

#### HTTP Request

`POST /v1/devices/$DEVICE_ID/status`

#### Query parameters

| Parameter | Type    | Description                                       |
| --------- | ------- | ------------------------------------------------- |
| async     | boolean | Update the devices asynchronously (default=false) |

<aside class="notice">
When <code>async=true</code> the API response will be a 202 with no body. The device's response will be stored in `function` status. The request to status the device will persist even if the devices is unreachable. The device's status will be updated it is connected.
</aside>

## Refresh the Device Status

> Example Request:

```sh
curl -X POST https://api.automategreen.com/v1/devices/$DEVICE_ID/status \
     -H "Authorization: Bearer $TOKEN" \
     -H "Content-Type: application/json"
```

> Response:

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
        "current": 0,
        "voltage": 122.2,
        "power": 0,
        "temperature": 99,
        "frequency": 60.01,
        "flow": null,
        "on": false,
        "toggleIn": 0,
        "toggleAt": 0,
        "firmware": 80
      },
      "date": "2018-09-26T13:03:27.000Z"
    },
    "info": {
      "ffr": {
        "enabled": false,
        "tripThreshold": 59.7,
        "returnThreshold": 60,
        "periods": 10,
        "maxBackOff": 10
      },
      "capacity": {
        "enabled": false,
        "lossPerLiter": 0,
        "lossPerIdleMinute": 0,
        "gainPerWattHour": 0,
        "maxCapacity": 0,
        "minCapacity": 0
      },
      "flowmeter": {
        "type": "none",
        "pipeOuterDiameter": 22.2,
        "pipeWallThickness": 1.1,
        "pipeType": "copper"
      },
      "powerLimit": {
        "endsAt": 0,
        "power": 0.25
      }
    }
  }
}
```

Manually request the Power Controller to send an updated status to the cloud. The status will be based on the averages since the last status.

<aside class="notice">
This API request is not needed for normal operation.  The device will automatically send status updates based on the configured sample period.
</aside>

# Sending Commands

#### HTTP Request

`POST /v1/devices/$DEVICE_ID/command`

#### Query parameters

| Parameter | Type    | Description                                       |
| --------- | ------- | ------------------------------------------------- |
| async     | boolean | Update the devices asynchronously (default=false) |

<aside class="notice">
When <code>async=true</code> the API response will be a 202 with no body. The device's response will be stored in `function` status. The command will persist even if the devices is unreachable. The command will be sent once the device is connected.  Only the latest command will be sent.  The persistent behavior can be disabled by setting `persist` to `false` in the command object.
</aside>

## Turn load on

> Example Request:

```sh
curl -X POST https://api.automategreen.com/v1/devices/$DEVICE_ID/command \
     -H "Authorization: Bearer $TOKEN" \
     -H "Content-Type: application/json" \
     -d '{
          "command": {
            "name": "on"
          }
        }'
```

## Turn load on - Asynchronously

> Example Request:

```sh
curl -X POST https://api.automategreen.com/v1/devices/$DEVICE_ID/command?async=true \
     -H "Authorization: Bearer $TOKEN" \
     -H "Content-Type: application/json" \
     -d '{
          "command": {
            "name": "on"
          }
        }'
```

Turns the load on asynchronously and be persistent (make sure it turns on when connected)

## Turn load on - Asynchronously disabling persistent

> Example Request:

```sh
curl -X POST https://api.automategreen.com/v1/devices/$DEVICE_ID/command?async=true \
     -H "Authorization: Bearer $TOKEN" \
     -H "Content-Type: application/json" \
     -d '{
          "command": {
            "name": "on"
            "persist" false
          }
        }'
```

Turns the load on asynchronously but don't persist the command (only tries once )

## Turn load off

> Example Request:

```sh
curl -X POST https://api.automategreen.com/v1/devices/$DEVICE_ID/command \
     -H "Authorization: Bearer $TOKEN" \
     -H "Content-Type: application/json" \
     -d '{
          "command": {
            "name": "off"
          }
        }'
```

Turn the load off now.

## Turn load On In

> Example Request:

```sh
curl -X POST https://api.automategreen.com/v1/devices/$DEVICE_ID/command \
     -H "Authorization: Bearer $TOKEN" \
     -H "Content-Type: application/json" \
     -d '{
          "command": {
            "name": "on",
            "delay": 60
          }
        }'
```

Turn the load on in the future based on the delay (seconds).

## Turn load Off In

> Example Request:

```sh
curl -X POST https://api.automategreen.com/v1/devices/$DEVICE_ID/command \
     -H "Authorization: Bearer $TOKEN" \
     -H "Content-Type: application/json" \
     -d '{
          "command": {
            "name": "off",
            "delay": 60
          }
        }'
```

Turn the load off in the future based on the delay (seconds).

## Turn load On For

> Example Request:

```sh
curl -X POST https://api.automategreen.com/v1/devices/$DEVICE_ID/command \
     -H "Authorization: Bearer $TOKEN" \
     -H "Content-Type: application/json" \
     -d '{
          "command": {
            "name": "on_for",
            "delay": 60
          }
        }'
```

Turn the load on for a period of time based on the delay (seconds).

## Turn load Off For

> Example Request:

```sh
curl -X POST https://api.automategreen.com/v1/devices/$DEVICE_ID/command \
     -H "Authorization: Bearer $TOKEN" \
     -H "Content-Type: application/json" \
     -d '{
          "command": {
            "name": "off_for",
            "delay": 60
          }
        }'
```

Turn the load off for a period of time based on the delay (seconds).

## Turn load On At

> Example Request:

```sh
curl -X POST https://api.automategreen.com/v1/devices/$DEVICE_ID/command \
     -H "Authorization: Bearer $TOKEN" \
     -H "Content-Type: application/json" \
     -d '{
          "command": {
            "name": "on_at",
            "time": 1528225394
          }
        }'
```

Turn the load on at a set time. Time is the number of seconds elapsed since the UNIX epoch.

## Turn load Off At

> Example Request:

```sh
curl -X POST https://api.automategreen.com/v1/devices/$DEVICE_ID/command \
     -H "Authorization: Bearer $TOKEN" \
     -H "Content-Type: application/json" \
     -d '{
          "command": {
            "name": "off_at",
            "time": 1528225394
          }
        }'
```

Turn the load off at a set time. Time is the number of seconds elapsed since the UNIX epoch.

## Limit Power

> Example Request:

```sh
curl -X POST https://api.automategreen.com/v1/devices/$DEVICE_ID/command \
     -H "Authorization: Bearer $TOKEN" \
     -H "Content-Type: application/json" \
     -d '{
          "command": {
            "name": "limit_power",
            "power": 50,
            "time": 3600
          }
        }'
```

Turn on the load and limit the amount of power the Power Controller can use for a set period of time. Power is in Watt\*hours. Time is in seconds.

The load will be turned on when the command is send. The load will be turned off if the limit is reached. The load will turn back on after the period expires. If the limit is not reached during the set period, then the load will not be turned off.

## Reset device

Reset/reboots the controller.

> Example Request:

```sh
curl -X POST https://api.automategreen.com/v1/devices/$DEVICE_ID/command \
     -H "Authorization: Bearer $TOKEN" \
     -H "Content-Type: application/json" \
     -d '{
          "command": {
            "name": "reset"
          }
        }'
```

# Configuring

#### HTTP Request

`PUT /v1/devices/$DEVICE_ID`

#### Query parameters

| Parameter | Type    | Description                                       |
| --------- | ------- | ------------------------------------------------- |
| async     | boolean | Update the devices asynchronously (default=false) |

<aside class="notice">
When <code>async=true</code> the API response will be a 202 with no body. The device's response code will be stored in `function` status. The setting will persist even if the devices is unreachable and be applied once the devices is connected.
</aside>

## Configure FFR

#### Info FRR Attributes

| Attribute       | Type    | Description                                                                 |
| --------------- | ------- | --------------------------------------------------------------------------- |
| enable          | boolean | Enables/Disables the FFR functionality                                      |
| tripThreshold   | number  | The frequency to trigger the FFR (turn)                                     |
| returnThreshold | number  | The frequency to return to service (after random delay)                     |
| periods         | number  | The number of periods (cycles) the threshold is passed before transitioning |
| delay           | number  | The maximum delay for the random return to service.                         |

### Enable FFR

> Example Request:

```sh
curl -X PUT https://api.automategreen.com/v1/devices/$DEVICE_ID \
     -H "Authorization: Bearer $TOKEN" \
     -H "Content-Type: application/json" \
     -d '{
          "info": {
            "ffr": {
              "enable": true,
              "tripThreshold": 59.7,
              "returnThreshold": 59.8,
              "periods": 10,
              "delay": 10
            }
          }
        }'
```

To enable the FFR on a Power Controller, the `ffr` object in the device's `info` is sent.

### Enable FFR - Asynchronously

> Example Request:

```sh
curl -X PUT https://api.automategreen.com/v1/devices/$DEVICE_ID?async=true \
     -H "Authorization: Bearer $TOKEN" \
     -H "Content-Type: application/json" \
     -d '{
          "info": {
            "ffr": {
              "enable": true,
              "tripThreshold": 59.7,
              "returnThreshold": 59.8,
              "periods": 10,
              "delay": 10
            }
          }
        }'
```

The same FFR settings but sent asynchronously. The API will return a 202.

### Disable FFR

> Example Request:

```sh
curl -X PUT https://api.automategreen.com/v1/devices/$DEVICE_ID \
     -H "Authorization: Bearer $TOKEN" \
     -H "Content-Type: application/json" \
     -d '{
          "info": {
            "ffr":{
              "enable": false
            }
          }
        }'
```

To disable the FFR on a Power Controller, the `ffr` object in the device's `info` is sent with `enable=false`.

## Configure Sample Period

#### Info Sample Period Attributes

| Attribute    | Type   | Description                         |
| ------------ | ------ | ----------------------------------- |
| samplePeriod | number | Number of seconds between samplings |

### Set Sample Period

> Example Request:

```sh
curl -X PUT https://api.automategreen.com/v1/devices/$DEVICE_ID \
     -H "Authorization: Bearer $TOKEN" \
     -H "Content-Type: application/json" \
     -d '{
          "info": {
            "samplePeriod": 300
          }
        }'
```

Setting the sampling period to 5 minutes (300 seconds)

### Set Sample Period - Asynchronously

> Example Request:

```sh
curl -X PUT https://api.automategreen.com/v1/devices/$DEVICE_ID?async=true \
     -H "Authorization: Bearer $TOKEN" \
     -H "Content-Type: application/json" \
     -d '{
          "info": {
            "samplePeriod": 300
          }
        }'
```

Setting the sampling period to 5 minutes (300 seconds) asynchronously.

## Scheduling

To create a schedule an array of scheduled commands should be sent. The maximum number of schedules 39. Each scheduled command has the following attributes:

#### Schedule Attributes

| Attribute | Type   | Description                                                                              |
| --------- | ------ | ---------------------------------------------------------------------------------------- |
| name      | string | name of the command to execute (on, off, or limit_power)                                 |
| when      | number | unix timestamp for when the command should be executed                                   |
| repeat    | number | optional number of seconds that the command should repeat (default: 0 - repeat disabled) |

#### Additional Schedule Attributes - limit_power

| Attribute | Type   | Description                                            |
| --------- | ------ | ------------------------------------------------------ |
| power     | number | number of watts to which to limit power                |
| time      | number | the amount of time in seconds for which to limit power |

<aside class="notice">
All schedules are overwritten each time this API is called.
</aside>

### Set Schedule

> Example Request:

```sh
curl -X PUT https://api.automategreen.com/v1/devices/$DEVICE_ID \
     -H "Authorization: Bearer $TOKEN" \
     -H "Content-Type: application/json" \
     -d '{
          "schedule": [{
            "name": "on",
            "when": 1571076135,
            "repeat": 86400
          },{
            "name": "off",
            "when": 1571076195,
            "repeat": 86400
          },{
            "name": "limit_power",
            "power": 50,
            "time": 3600,
            "when": 1571162475
          }]
        }'
```

Setting up 3 scheduled commands. The on and off commands will repeat every 24 hours. The power_limit command will not repeat.

### Clear Schedule

> Example Request:

```sh
curl -X PUT https://api.automategreen.com/v1/devices/$DEVICE_ID \
     -H "Authorization: Bearer $TOKEN" \
     -H "Content-Type: application/json" \
     -d '{
          "schedule": []
        }'
```

To clear all scheduled commands on a device. Send an empty schedule array.

### Set Schedule - Asynchronously

> Example Request:

```sh
curl -X PUT https://api.automategreen.com/v1/devices/$DEVICE_ID?async=true \
     -H "Authorization: Bearer $TOKEN" \
     -H "Content-Type: application/json" \
     -d '{
          "schedule": [...]
        }'
```

Setting up a schedule asynchronously.

# Statuses

## Status Types

The status objects have had two parameters added to track the type of update and reason for the update. There are three types of status for the power controller: update, relay, trigger.

### Update Status Type

> Example JSON::

```json
{
  "id": "xyz890ABC",
  "deviceId": "abc123def",
  "date": "2018-05-03T12:22:58.807Z",
  "state": "Active",
  "type": "update",
  "reason": "auto",
  "info": {
    "firmware": 10,
    "duration": 900,
    "current": 10.5,
    "voltage": 243.6,
    "power": 639.45,
    "temperature": 75.2,
    "frequency": 60.01,
    "flow": 10.0,
    "on": true,
    "toggleAt": 0
  }
}
```

The update status is sent periodicity (auto) or when a status request is issued via the API (api). The update status will provides the sensor data and current relay state. In addition the time (duration) used to calculate the averages is also provided. For the automatic update statuses, the duration will be 900 seconds (15 minutes). For API status updates, the duration will vary based on the time sense the last update status.

#### Reason Values

| Reason | Description                                             |
| ------ | ------------------------------------------------------- |
| auto   | The status was sent automatically by the controller     |
| api    | The status was sent because the `status` API was called |

#### Info FRR Attributes

| Attribute   | Type    | Description                                                                       |
| ----------- | ------- | --------------------------------------------------------------------------------- |
| firmware    | number  | The firmware build number of the controller                                       |
| duration    | number  | Duration of the sensor sampling used for averaging (sec)                          |
| current     | number  | Average current (A)                                                               |
| voltage     | number  | Average voltage (V)                                                               |
| power       | number  | Total power used during duration (Wh)                                             |
| temperature | number  | Average abetment temperature (F)                                                  |
| frequency   | number  | Average frequency (Hz)                                                            |
| flow        | number  | Average rate of flow (L/min)                                                      |
| on          | boolean | The state of the load: `true` mean the load is powered                            |
| toggleAt    | number  | UNIX epoch timestamp (seconds) for when to toggle the relay state (0 means never) |

### Relay Status Type

> Example JSON::

```json
{
  "id": "xyz890ABC",
  "deviceId": "abc123def",
  "date": "2018-05-03T12:22:58.807Z",
  "state": "Active",
  "type": "relay",
  "reason": "reboot",
  "info": {
    "firmware": 10,
    "on": true,
    "toggleAt": 0
  }
}
```

The relay status records when the relay was turned on or off and what turned it on or off.

#### Reason Values

| Reason       | Description                                  |
| ------------ | -------------------------------------------- |
| reboot       | Sent when the device boot up                 |
| timer        | A timer (like a delay) cause the relay event |
| ffr          | The FFR logic caused the relay event         |
| ffrr         | The FFR return logic caused the relay event  |
| api          | The API was used to control the relay        |
| manual       | The mode button was pressed                  |
| power_limit  | The power limiting command's threshold       |
| max_capacity | The load capacity reached the max value      |
| min_capacity | The load capacity dropped to the min value   |

#### Info FRR Attributes

| Attribute | Type    | Description                                                                       |
| --------- | ------- | --------------------------------------------------------------------------------- |
| firmware  | number  | The firmware build number of the controller                                       |
| on        | boolean | The state of the load: `true` mean the load is powered                            |
| toggleAt  | number  | UNIX epoch timestamp (seconds) for when to toggle the relay state (0 means never) |

### Trigger Status Type

> Example JSON:

```json
{
  "id": "xyz890ABC",
  "deviceId": "abc123def",
  "date": "2018-05-03T12:22:58.807Z",
  "state": "Active",
  "type": "trigger",
  "reason": "ffr",
  "info": {
    "firmware": 10,
    "frequency": 59.695
  }
}
```

The trigger status records when a trigger was invoked and why it what trigger was invoked.

#### Reason Values

| Reason | Description               |
| ------ | ------------------------- |
| ffr    | FFR was the trigger event |

#### Info FRR Attributes

| Attribute | Type   | Description                                 |
| --------- | ------ | ------------------------------------------- |
| firmware  | number | The firmware build number of the controller |
| frequency | number | Frequency that triggered the FFR (Hz)       |

### Function Status Type

> Example JSON - Active:

```json
{
  "id": "xyz890ABC",
  "deviceId": "abc123def",
  "date": "2018-05-03T12:22:58.807Z",
  "state": "Active",
  "type": "function",
  "reason": "command",
  "info": {
    "update": { "on": true },
    "responseCodes": [102]
  }
}
```

> Example JSON - Unavailable:

```json
{
  "id": "xyz890ABC",
  "deviceId": "abc123def",
  "date": "2018-05-03T12:22:58.807Z",
  "state": "Unavailable",
  "type": "function",
  "reason": "command",
  "info": {
    "update": { "on": true },
    "responseCodes": []
  }
}
```

The function status records when an update sent to a device. If the device is connected and response to the update, the state will be Active. If there is no response from the device, the state will be Unavailable.

#### Reason Values

| Reason       | Description                             |
| ------------ | --------------------------------------- |
| command      | command sent to the device              |
| ffr          | FFR update sent to the device           |
| samplePeriod | sample period update sent to the device |
| status       | status request sent to the device       |
| schedule     | schedule update sent to the device      |

#### Info FRR Attributes

| Attribute     | Type            | Description                        |
| ------------- | --------------- | ---------------------------------- |
| update        | object          | The update that was sent           |
| responseCodes | array of number | the response codes from the device |

## List Statuses


`GET /v1/statuses?device=$DEVICE_ID`
`GET /v1/statuses?device=$DEVICE_ID&date=$DATE`
`GET /v1/statuses?device=$DEVICE_ID&date[start]=$START_DATE&date[end]=$END_DATE`

```sh
curl -g https://api.automategreen.com/v1/statuses?device=bksxGL4wMx&date[start]=2015-08-10T09:30&date[end]=2015-08-15 \
-H "Authorization: Bearer $TOKEN"
```

Returns a list of device statuses for the provided date range.

<aside class="notice">
The date range for a single request is limited to 10 days.
</aside>

#### HTTP Request

`GET /v1/statuses`

#### Query Parameters

| Parameter   | Default     | Description                                                |
| ----------- | ----------- | ---------------------------------------------------------- |
| device      | ALL Devices | Device for which to get statuses (if omitted all devices)  |
| date[start] | Yesterday   | Return statuses starting from this date                    |
| date[end]   | Now         | Return statuses before this date                           |
| date        | Yesterday   | Simplified date returning all status from this date to now |
| raw         | false       | If true, return raw DB object                              |

<aside class="notice">
Dates must be in ISO 8601 formatted strings.
</aside>

<aside class="notice">
If device is omitted statuses for all devices for the time period are return.  This is recommend for only short periods of time since results can be to large.
</aside>

# Response Codes

If the controller is online and receiving commands, the following codes will be contained in the API response body. The HTTP status code will be 200. If the controller is not responding, the HTTP status code will be 503.

| Success Codes | Meaning                                             |
| ------------- | --------------------------------------------------- |
| 10            | Triggered the controller to reset                   |
| 100           | Refreshed the sensors status                        |
| 101           | Refreshed the relays status                         |
| 102           | Turned on the relay                                 |
| 103           | Turned off the relay                                |
| 104           | Turned on the relay for period                      |
| 105           | Turned off the relay for period                     |
| 106           | Turn on the relay at timestamp                      |
| 107           | Turn off the relay at timestamp                     |
| 108           | FFR has been enabled and configured                 |
| 109           | FFR has been disabled                               |
| 110           | Flowmeter configured to none                        |
| 111           | Flowmeter configured for EUF4315K                   |
| 112           | Flowmeter configured for TUF2000M                   |
| 113           | Capacity set point has been disabled                |
| 114           | Capacity set point has been enabled and configured  |
| 115           | Limit power has been configured                     |
| 116           | Keep alive configured,                              |
| 117           | Schedule added,                                     |
| 118           | Schedule removed,                                   |
| 119           | Schedules cleared,                                  |
| 120           | Sample period configured,                           |
| 121           | Schedules disabled,                                 |
| 122           | Limit power disabled,                               |
| 1001          | FFR unchanged (no command sent to device)           |
| 1002          | Sample period unchanged (no command sent to device) |

| Error Codes | Meaning                                                         |
| ----------- | --------------------------------------------------------------- |
| -999        | Trying to configure an unsupported sensor type                  |
| -998        | Capacity set point trip threshold is to low                     |
| -997        | Capacity set point return threshold is less than trip threshold |
| -996        | FFR period is to short                                          |
| -995        | FFR period is to long                                           |
| -994        | FFR delay is negative                                           |
| -993        | Relay config is not supported                                   |
| -992        | Invalid relay command                                           |
| -991        | Pipe type is invalid                                            |
| -990        | Pipe outer diameter is invalid                                  |
| -989        | Pipe wall is invalid                                            |
| -988        | Capacity set point minimum value is invalid                     |
| -987        | Capacity set point maximum value is invalid                     |
| -986        | Limit power value is invalid                                    |
| -985        | Keep alive setting is invalid                                   |
| -985        | Unknown schedule type                                           |
| -985        | Invalid schedule index                                          |
| -985        | Invalid schedule config                                         |
| -985        | Exceed max number schedules                                     |
| -985        | Sample period is invalid                                        |
