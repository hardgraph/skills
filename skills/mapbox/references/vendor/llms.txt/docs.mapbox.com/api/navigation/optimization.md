# Optimization API v2

> **Note (beta): Public Beta for the Optimization v2 API**
> 
> Mapbox is launching a Beta program for the Optimization v2 API with many new features, including time window constraints, vehicle capacity limits, driver shifts, and pickup and drop off constraints.If you are interested in early access to the Optimization v2 API Beta, [sign up here](https://www.mapbox.com/contact/optimization-v2-api?utm_source=docs&utm_medium=banner).

> **Note: Optimization v1 API**
> 
> This page documents the Optimization v2 API Beta functionality.If you would like to use the Optimization v1 here, [documentation is available here](https://docs.mapbox.com/api/navigation/optimization-v1/).

The Optimization v2 API is an API for calculating efficient plans for vehicles to visit multiple locations. These are commonly known as [vehicle routing problems](https://en.wikipedia.org/wiki/Vehicle_routing_problem).

The Optimization v2 API enables you to submit vehicle routing problems in the form of routing problem documents describing the number of vehicles in your fleet, locations to be visited, and other constraints that are relevant to your real-world problem. The API returns an optimized solution for your routing problem as a solution document that describes a route plan for each vehicle.

> **Related content (playground): [Optimization API v2 playground](https://demos.mapbox.com/optimization-v2-playground/)**
> 
> Describe pickup and drop off locations, specify shipments and vehicles, add all the related constraints - and see the optimized solution on a map.

## Quick summary

1.  `POST` to `/optimized-trips/v2?access_token=YOUR_MAPBOX_TOKEN` to submit a routing problem, and save the `id` from the JSON response.
2.  `GET` from `/optimized-trips/v2/{id}?access_token=YOUR_MAPBOX_TOKEN` to check the status and receive the result when it is complete.
3.  `GET` from `/optimized-trips/v2?access_token=YOUR_MAPBOX_TOKEN` to list all your account's submitted routing problems and their processing statuses.

## Submit a routing problem

**POST** : `https://api.mapbox.com/optimized-trips/v2`

To submit a routing problem, make a HTTP POST request with a `Content-Type` of `application/json`, and [submit the routing problem document JSON](#routing-problem-document) as the POST body.

### Example HTTP request

```http
POST /optimized-trips/v2?access_token=YOUR_MAPBOX_ACCESS_TOKENHTTP/1.1
Host: api.mapbox.com
Content-Type: application/json

{
    "version": 1,
    "vehicles": [...],
    "services": [...]
}
```

### Example cURL request

```bash
$ curl -H 'Content-Type: application/json'
     -d @problem.json
     'https://api.mapbox.com/optimized-trips/v2?access_token=YOUR_MAPBOX_ACCESS_TOKEN'
```

### 202 Accepted

The 202 response to a request is a JSON object that contains the following properties:

| Property | Type | Description |
| --- | --- | --- |
| `id` | `string` | This is a unique ID for the submitted request. Save this value. It is needed to retrieve the status and results of your submission. |
| `status` | `string` | This indicates whether the routing problem submission is `pending`, `processing`, or `complete ("ok")`. `complete ("ok")` indicates a successfully submitted routing problem, not a solved problem. |
| `status_date` | `string` | The date when the status was updated. |

The 202 response body has a `Content-Type` of `application/json`, and looks similar to this:

```json
HTTP/1.1 202 Accepted
Content-Type: application/json

{ 
    "id": "123e4567-e89b-12d3-a456-426655440000",
    "status": "ok"
}
```

The `id` value in this response is a unique ID for the request you submitted. Save this value, then use it to retrieve the status and results of your submission.

> **Note: Optimization API errors**
> 
> To learn about other responses for this API, see [Optimization v2 API errors](#optimization-v2-api-errors).

## Retrieve a solution

**GET** : `https://api.mapbox.com/optimized-trips/v2/{id}`

To retrieve a solution, you must have the `id` generated when the [routing problem was submitted](#submit-a-routing-problem). If you did not save the `id` for your submission, you can [retrieve a list of all your submissions](#list-submissions-and-their-status) to find the `id`.

Refer to the [solution document](#solution-document) section to better understand the solutions that you receive from the Optimization v2 API.

### Example HTTP request

```http
GET /optimized-trips/v2/123e4567-e89b-12d3-a456-426655440000?access_token=YOUR_MAPBOX_ACCESS_TOKEN
Content-Type: application/json
Host: api.mapbox.com
```

### Example cURL request

```bash
$ curl "https://api.mapbox.com/optimized-trips/v2/YOUR_JOB_ID?access_token=YOUR_MAPBOX_ACCESS_TOKEN"
```

### 202 Accepted

No solution is available for your routing problem yet. The request is either waiting to be processed or is processing.

### 200 OK

This response means that the solution has been posted. The body of the HTTP response is the solution JSON document.

```json
{
  "dropped": {
    "services": [],
    "shipments": []
  },
  "routes": [
    {
      "vehicle": "truck-1",
      "stops": [
        {
          "location": "warehouse",
          "eta": "2022-01-01T09:00:00Z",
          "type": "start",
          "wait": 0,
          "odometer": 0
        },
        {
          "location": "stop-3",
          "eta": "2022-01-01T09:01:12Z",
          "type": "dropoff",
          "duration": 0,
          "wait": 0,
          "odometer": 381
        },
        {
          "location": "stop-2",
          "eta": "2022-01-01T09:01:36Z",
          "type": "dropoff",
          "duration": 0,
          "wait": 0,
          "odometer": 487
        },
        {
          "location": "stop-1",
          "eta": "2022-01-01T09:01:53Z",
          "type": "dropoff",
          "duration": 0,
          "wait": 0,
          "odometer": 587
        },
        {
          "location": "warehouse",
          "eta": "2022-01-01T09:02:28Z",
          "type": "end",
          "odometer": 762
        }
      ]
    }
  ]
}
```

## List submissions and their status

**GET** : `https://api.mapbox.com/optimized-trips/v2`

You can fetch a list of all the routing problems you have submitted along with their current processing statuses. To do so, make a HTTP `GET` request against the base URL.

### Example HTTP request

```http
GET /optimized-trips/v2?access_token=YOUR_MAPBOX_ACCESS_TOKEN
Content-Type: application/json
Host: api.mapbox.com
```

### Example cURL request

```bash
$ curl --request GET
  --url 'https://api.mapbox.com/optimized-trips/v2?access_token=YOUR_MAPBOX_ACCESS_TOKEN'
  --header 'Content-Type: application/json'
```

### 200 OK

You will receive a JSON response body that is structured as follows:

```json
[
  { "id": "123e4567-e89b-12d3-a456-426655440000", "status": "processing" },
  { "id": "123e4567-e89b-12d3-a456-426655440001", "status": "complete" }
]
```

The values for `id` are the unique IDs generated each time a routing problem is submitted successfully. Use the `id` to [retrieve a solution](#retrieve-a-solution) for one of your submitted routing problems.

## Routing problem document

Routing problems are JSON documents with a defined schema.

### Conventions

-   All time values are in ISO 8601 format. For example, `2023-07-22T14:29:23.123-0700`. Time zones are supported. Milliseconds are accepted, but ignored.
-   Duration values are in seconds.
-   Distance values are in meters.

### Overview

The root object in the routing problem document contains five elements:

```json
{
    "version": 1,
    "locations": [...],
    "vehicles": [...],
    "services": [...],
    "shipments": [...]
}
```

Each of these root elements is described in a separate section below.

### Version

Required. This object describes the schema version of the routing problem document.  Only the value `1` (as a number) is supported.

### Locations

Required. A location is a physical point in space that needs to be visited by the vehicles in your routing problem. You must refer to a location from `services` or `shipments` for a location to be visited.

| Field | Description |
| --- | --- |
| `name` | Required. The unique name of a location. This name is used by other objects when they refer to a place. |
| `coordinates` | Required. An array of two numbers representing a location in "longitude,latitude" order. |

### Example request object - locations

```json
"locations": [
    {
        "name": "123 Address Street",
        "coordinates": [ -122.1234, 37.812 ],
    }
]
```

### Vehicles

Required. A vehicle is a car, truck, or similar machine that travels between your defined locations. You need to add at least one vehicle to generate a solution.

<table><thead><tr><th>Field</th><th>Description</th></tr></thead><tbody><tr><td><code>name</code></td><td>Required. The unique name of a vehicle. The name is displayed in the solution document.</td></tr><tr><td><code>routing_profile</code></td><td>Optional. Defines which profile to use to calculate travel times/distances for this vehicle. The default value is <code>mapbox/driving</code>.<br><br>Valid values include: <code>mapbox/driving</code>, <code>mapbox/driving-traffic</code>, <code>mapbox/cycling</code>, <code>mapbox/walking</code>.These correspond to the profiles available on a <a href="/navigation/directions/#routing-profiles">Mapbox Directions routing profile</a>.</td></tr><tr><td><code>start_location</code></td><td>Optional. The location where the vehicle should start its route. If not supplied, the vehicle starts at a location that minimizes the route according to the <code>objectives</code> that you set.</td></tr><tr><td><code>end_location</code></td><td>Optional. The location where the vehicle should end its route. If not supplied, the vehicle finishes at a location that minimizes the route according to the <code>objectives</code> that you set.</td></tr><tr><td><code>capacities</code></td><td>Optional. How many units of an item can be carried by a vehicle. You can define as many capacities as needed. When you define shipments, you can describe how many units of each capacity these shipments require. The optimizer ensures that a vehicle's capacity is never exceeded.</td></tr><tr><td><code>capabilities</code></td><td>Optional. The capabilities of a vehicle. When defining shipments, you can require that only vehicles with certain capabilities should serve those locations.</td></tr><tr><td><code>earliest_start</code></td><td>Optional. The earliest time this vehicle can start operating.</td></tr><tr><td><code>latest_end</code></td><td>Optional. The latest time this vehicle can finish operating.</td></tr><tr><td><code>breaks</code></td><td>Optional. Define when mandated breaks for vehicles can begin and end, and how long a break can last.&nbsp; An array of required objects with <code>earliest_start</code> (timestamp string), <code>latest_end</code> (timestamp string), and <code>duration</code> (integer seconds) properties.</td></tr><tr><td><code>loading_policy</code></td><td>Optional. How packages must be loaded/unloaded from the vehicle. For example, packages loaded onto the vehicle first cannot be unloaded until the packages loaded later are unloaded. Valid values include <code>any</code>, <code>fifo</code>, and <code>lifo</code>. FIFO loading is "first in, first out", which means packages picked up first must be dropped off first. LIFO is "last in, first out", which means packages picked up last must be dropped off first.&nbsp; The default value is <code>any</code>, which means there is no constraint on loading/unloading order for the vehicle.</td></tr></tbody></table>

### Example request object - vehicles

```json
"vehicles": [
    {
        "name": "truck-1",
        "routing_profile": "mapbox/driving",
        "start_location": "warehouse",
        "end_location": "warehouse",
        "capacities": {
            "volume": 3000,
            "weight": 1000,
            "boxes": 100
        },
        "capabilities": [
            "ladder",
            "refrigeration"
        ],
        "earliest_start": "2022-05-31T09:00:00Z",
        "latest_end": "2022-05-31T17:00:00Z",
        "breaks": [
            {
                "earliest_start": "2022-05-31T12:00:00Z",
                "latest_end": "2022-05-31T13:00:00Z",
                "duration": 1800
            }
        ]
    }
]
```

### Services

> **Note**
> 
> At least one of `services` or `shipments` is required to submit the routing problem document. If you define `services`, then `shipments` is optional.

Services are locations that need to be visited, but these visits do not consume or relieve any capacity of the vehicle. For example, a plumber performing house calls does not need to drop off or pick up any units.

<table><thead><tr><th>Field</th><th>Description</th></tr></thead><tbody><tr><td><code>name</code></td><td>Required. A unique name for a service. This could be an order ID or identifier generated by an upstream system.</td></tr><tr><td><code>location</code></td><td>Required. The location where a service is to be performed. This value should be the same as the value defined for the <code>name</code> field in <a href="#locations">locations</a>.</td></tr><tr><td><code>duration</code></td><td>Optional. The amount of time (integer seconds) the vehicle needs to stop at a location to complete the service.</td></tr><tr><td><code>requirements</code></td><td>Optional. What capabilities are required at a location. Only vehicles that have these capabilities can do this service. You set the values related to requirements.</td></tr><tr><td><code>service_times</code></td><td>Optional. Define when a service can start and when it must be finished. This is useful when a location is only accessible at certain times, or when an appointment has been arranged. An array of objects with <code>earliest</code> (timestamp string) and <code>latest</code> (timestamp string) required.<br><br>Set <code>type</code> with one of the following values to define how strictly the time range needs to be adhered to: <code>strict</code> (default), <code>soft</code>, <code>soft_start</code>, or <code>soft_end</code>. Setting this value to <code>soft</code> allows the service to occur outside the window, but an increasing penalty is applied the further from the window it gets. <code>soft_end</code> permits deliveries to be late (but not early) and <code>soft_start</code> permits deliveries to be early (but not late).<br><br>If a service has a corresponding <code>duration</code>, your <code>latest</code> time limit should take this into account.&nbsp; For example, if a service takes 60 seconds to complete, and the <code>latest</code> value is <code>19:00:00</code>, the driver has to arrive by 18:59:00 to meet the constraint. Maximum of 1 service time may be supplied.</td></tr></tbody></table>

### Example request objects - services

```json
"services": [
    {
        "name": "work-order-1234",
        "location": "123 Address Street",
        "duration": 300,
        "requirements": [
            "ladder"
        ],
        "service_times": [
            {
                "earliest": "2022-05-31T13:00:00Z",
                "latest": "2022-05-31T14:00:00Z",
                "type": "soft"
            }
        ]
    }
]
```

### Shipments

> **Note**
> 
> At least one of `shipments` or `services` is required to submit the routing problem document. If you define `shipments`, then `services` is optional.

Shipments are the items that need to be picked up at one location and delivered to another. Shipments have dimensions like size, weight, and units), and they consume capacity in the vehicles that carry them.

| Field | Description |
| --- | --- |
| `name` | Required.  A unique name for a shipment. This could be an order ID or identifier generated by an upstream system. |
| `from` | Required. The location where the shipment must be picked up. This value must be the same as a value defined for the `name` field in [locations](#locations). |
| `to` | Required. The location where the location must be delivered. This value must be the same as a value defined for the `name` field in [locations](#locations). |
| `size` | Optional. How many units of capacity a shipment consumes in a vehicle. The units are released at the drop off location. A vehicle must have all the capacity units defined  to do a pickup/drop off. |
| `requirements` | Optional. Capabilities that a vehicle needs to do a pickup/drop off. |
| `pickup_duration` | Optional. How long it takes to do the pickup. The default value is `0`. |
| `dropoff_duration` | Optional. How long it takes to do the drop off. The default value is `0`. |
| `pickup_times` | Optional. Define the time ranges when it is possible to do the pickup. Maximum of 1 pickup time window may be supplied. |
| `dropoff_times` | Optional. Define the time ranges when it is possible to do the drop off. Maximum of 1 drop off time window may be supplied. |

### Example request object - shipments

```json
"shipments": [
{
    "name": "order-1234",
    "from": "warehouse",
    "to": "123 Address Street",
    "size": {
        "weight": 30,
        "volume": 100,
        "boxes": 3
    },
    "requirements": [
        "refrigeration"
    ],
    "pickup_duration": 60,
    "dropoff_duration": 60,
    "pickup_times": [
        {
            "earliest": "2022-05-31T09:15:00Z",
            "latest": "2022-05-31T09:30:00Z",
            "type": "strict"
        }
    ],
    "dropoff_times": [
        {
            "earliest": "2022-05-31T10:15:00Z",
            "latest": "2022-05-31T10:30:00Z",
            "type": "soft_end"
        }
    ]
}]
```

### Options

Optional. This object enables you to choose how to solve your routing problem.

| Field | Description |
| --- | --- |
| `objectives` | Optional. An array of length 1 where can specify one of two routing objectives: minimize the total time traveled by the fleet (`min-total-travel-duration`), or minimize the length of the longest schedule in the fleet (`min-schedule-completion-time`). In practice, `min-schedule-completion-time` also improves shipment delivery times and increases fleet use by splitting routes when extra capacity is available. You may only specify one of these objectives. |

### Example request object - options

```json
"options": {
    "objectives": ["min-schedule-completion-time"]
}
```

## Solution document

### Overview

The root document has two elements: any shipments or services that are not included in the solution, and the routes for each vehicle.

```json
{
    "dropped": {
        "shipments": [...],
        "services": [...]
    },
    "routes": [
        {
            "vehicle": "name",
            "stops": [...]
        },
        ...
    ]
}
```

### Dropped

This property lists shipments and services that are not included in the solution. This can occur when the optimization engine cannot find a solution, but by dropping a shipment or service from consideration, the engine can find a solution. If all shipments and services are solved, these lists are empty.

### Routes

A route is a sequence of stops to be visited by a single vehicle. The `vehicle` property is the name of the vehicle as defined in your routing problem document. The `stops` array describes the list and order of stops that the vehicle for the route should visit.

| Field | Description |
| --- | --- |
| `type` | Required. The kind of stop for each location. Valid values include `start`, `service`, `pickup`, `dropoff`, `break`, and `end`. |
| `location` | Required. The name of the location where a stop occurs. |
| `eta` | Required. The expected time of arrival at a location. |
| `odometer` | Required. How far you should have traveled when you arrive at this stop. Not present on `break` type stops. |
| `wait` | Optional. How long you should wait before proceeding to the next stop. This value is >0 when you are scheduled to arrive too early for the next stop's service time range. Not present on `end` type stops. |
| `duration` | Optional. How long a vehicle needs to stop at a location to complete the service. The duration of a stop is measured in seconds. |

For stops of type `service`, `pickup`, and `dropoff`, there is an extra field called `services`, `pickups`, or `dropoffs`. This field contains an array of the names of the services, pickups, or deliveries to be completed at that location, in the order they should be completed.

### Example stop

```json
{
  "type": "service",
  "services": ["service-1"],
  "location": "location-1",
  "eta": "2022-08-02T09:15:00Z",
  "odometer": 17.2,
  "wait": 0
}
```

### Example solution

```json
{
  "dropped": {
    "services": ["service-3", "service-4"],
    "shipments": []
  },
  "routes": [
    {
      "vehicle": "xyz",
      "stops": [
        {
          "type": "depart",
          "location": "warehouse",
          "eta": "2022-08-02T09:00:00Z",
          "odometer": 0
        },
        {
          "type": "service",
          "location": "location-1",
          "services": ["service-1"],
          "eta": "2022-08-02T14:42:00Z",
          "odometer": 100,
          "wait": 300
        },
        { "type": "break", "eta": "2022-08-02T14:42:00Z", "wait": 3600 },
        {
          "type": "service",
          "services": ["service-2"],
          "location": "location-2",
          "eta": "2022-08-02T15:42:00Z",
          "wait": 0,
          "odometer": 200
        },
        {
          "type": "arrive",
          "location": "warehouse",
          "eta": "2022-08-02T20:24:00Z",
          "odometer": 300
        }
      ]
    }
  ]
}
```

## Optimization v2 API errors

<table><thead><tr><th>Response body code</th><th>HTTP status code</th><th>Description</th></tr></thead><tbody><tr><td><code>OK</code></td><td><code>200</code></td><td>The solution has been posted. The body of the HTTP response is the solution JSON document.</td></tr><tr><td><code>Accepted</code></td><td><code>202</code></td><td>The problem was submitted successfully. The id should be saved so that you can <a href="#retrieve-a-solution">retrieve the solution</a>.</td></tr><tr><td><code>Unauthorized</code></td><td><code>401</code></td><td>YOUR_MAPBOX_ACCESS_TOKEN is not valid. Your account might not have the needed permissions to access the Optimization v2 API.<br><br>If you are interested in early access to the Optimization v2 API Beta, <a href="https://www.mapbox.com/contact/optimization-v2-api?utm_source=docs&amp;utm_medium=banner">sign up here</a>.</td></tr><tr><td><code>Not found</code></td><td><code>404</code></td><td>No submission was found that belongs to the account linked to the access token used. Check the <code>id</code> and your account token before trying again.</td></tr><tr><td><code>Unacceptable</code></td><td><code>422</code></td><td>The request is not formatted properly.</td></tr></tbody></table>

## Optimization v2 API restrictions and limits

-   Maximum of 300 routing problems submitted per minute
-   Maximum of 60 list submissions requests per minute
-   Maximum of 60 solutions status requests per minute
-   Maximum number of 1000 locations per routing problem

If you require a higher rate limit, [contact us](https://www.mapbox.com/contact/sales/).