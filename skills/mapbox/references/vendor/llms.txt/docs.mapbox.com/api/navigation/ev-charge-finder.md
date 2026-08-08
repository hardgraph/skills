# EV Charge Finder API

> **Note (beta): Private Preview for the EV Charge Finder API**
> 
> For pricing and to sign up for EV Charge Finder API, [sign up here](https://www.mapbox.com/contact/electric-vehicle). Note that the features and functionality of the API are subject to change.

The **EV Charge Finder API** searches for EV charge points within a specific area. The API provides various filtering options (such as connector types) to match different charging needs. The response is structured according to the industry-standard [OCPI v2.2.1 Locations module](https://github.com/ocpi/ocpi/blob/2.2.1/mod_locations.asciidoc), with additional extensions by Mapbox.

> **Related content (playground): [EV Charge Finder API playground](https://docs.mapbox.com/playground/ev-charge-finder/)**
> 
> Experiment with EV Charge Finder API queries and display charging stations from the API response on a map.

## Search for EV Charge Points

Search for charging stations within a specified geographic area, with options to filter results by charge point operator, connector types, availability, charging speeds, and other key attributes.

**GET** : `https://api.mapbox.com/ev/v1/locations?access_token={access_token}&latitude={latitude}&longitude={longitude}&distance={distance}`

### Required Parameters

| Parameter | Type | Description |
| --- | --- | --- |
| `access_token` | `string` | A valid Mapbox [access token](https://docs.mapbox.com/api/guides/#access-tokens-and-token-scopes). |
| `latitude` | `float` | The latitude of the location around which charge points are searched. The latitude must be a number between `-90.0` and `90.0`. |
| `longitude` | `float` | The longitude of the location around which charge points are searched. The longitude must be a number between `-180.0` and `180.0`. |
| `distance` | `float` | The maximum distance in kilometers around the latitude, longitude to search for charge points. You can specify a distance of up to 100 kilometers. The default value is 10 kilometers. |

### Optional Parameters

| Parameter | Type | Description |
| --- | --- | --- |
| `limit` | `integer` | An optional limit on the number of charge points to be provided in the response. You can specify a limit up to 100. The default value is 20. |
| `connector_types` | `string` | An optional comma-delimited list of the OCPI v2.2.1 [`ConnectorType`](#ocpi-connectortype) to be included in the response. By default all connector types will be included in the response. |
| `operators` | `string` | An optional comma-delimited list of the charge point operators to be included in the response. By default all operators will be included in the response. To get the current list of operators, refer to the [Retrieve list of Charge Point Operators](#retrieve-operators) section. |
| `exclude_operators` | `string` | An optional comma-delimited list of the charge point operators (CPO) to be excluded from the response. To get the current list of operators, refer to the [Retrieve list of Charge Point Operators](#retrieve-operators) section. |
| `min_charging_power` | `float` | An optional value in watts that sets the lower limit for the charging power supported by EVSEs at a charge point. If `max_charging_power` is provided, the default value is 0. |
| `max_charging_power` | `float` | An optional value in watts that sets the upper limit for the charging power supported by EVSEs at a charge point. If `min_charging_power` is provided, The default value is 500000. |
| `availability` | `string` | An optional filter on the charge points availability, reflected by the status of their EVSEs. The value of this optional parameter must be a valid [Status](#ocpi-status) (for example, "AVAILABLE" will return all charge points that have an EVSE with the status AVAILABLE). |
| `amenities` | `string` | Only include locations in the response that provide all specified amenities or have them nearby.
| Possible values | Description |
| --- | --- |
| `hotel` | A hotel. |
| `restaurant` | A restaurant. |
| `cafe` | A cafe. |
| `shopping_mall` | A shopping mall. |
| `supermarket` | A supermarket. |
| `sports` | Gyms, fields, etc. |
| `recreation_center` | A recreation area. |
| `park` | A park or nature reserve. |
| `nature_reserve` | A park or nature reserve. |
| `museum` | A museum. |
| `bus_stop` | A bus stop. |
| `bus_station` | A bus stop. |
| `taxi` | A taxi stand. |
| `railway_station` | A railway station. |
| `airport` | An airport. |
| `parking_lot` | A parking lot. |
| `gas_station` | A gas station. |
| `dog_park` | A dog park. |
| `bike_sharing` | A bike/e-bike/e-scooter sharing location. |
| `carpool_parking` | A carpool parking. |
| `tram_stop` | A tram stop. |
| `metro_station` | A metro station. |
| `wifi` | Wifi available. |
| `roof` | The charging station is protected by a roof. |
| `restroom` | Restrooms available. |
| `light` | The charging station is well-lit. |
| `trailer_friendly` | The charging location is suitable for trailers. |

 |
| `exclude` | `string` | A comma-delimited list of attributes. Charge points that have one or more of the specified attributes will be excluded from the response.

| Possible values | Description |
| --- | --- |
| `tesla_exclusive` | Charge point that can only be used by Tesla cars. |

 |
| `payment_methods` | `string` | An optional comma-delimited list of payment methods. When present, only locations supporting any of the provided payment methods will be included in the response.

| Possible values | Description |
| --- | --- |
| `ad-hoc` | Credit and debit card payments. |

 |
| `opening_times` | `string` | An optional filter for opening times. When present, only locations open during the given hours will be included in the response. If not present, all otherwise relevant locations will be included.

| Possible values | Description |
| --- | --- |
| `twentyfourseven` | Open 24/7. |

 |
| `eta_type` | `string` | An optional parameter to enable route-based ETA and distance calculation in the response. The only allowed value is `navigation`. When used, the response includes route-based `distance` and `eta` values calculated using driving traffic conditions at the time of the request. If not provided, the API returns straight-line distance only and the `eta` field is not included in the response. Enabling ETA calculations will introduce additional latency and incur extra costs, as each result with ETA will be billed according to the Mapbox Matrix API pricing. |
| `origin_latitude` | `float` | An optional parameter to specify the latitude of the origin location used for ETA and distance calculation. Must be a number between `-90.0` and `90.0`. This parameter requires `eta_type=navigation`. Must be specified together with `origin_longitude`. If both `origin_latitude` and `origin_longitude` are not provided, the required `latitude` and `longitude` parameters are used as the origin. |
| `origin_longitude` | `float` | An optional parameter to specify the longitude of the origin location used for ETA and distance calculation. Must be a number between `-180.0` and `180.0`. This parameter requires `eta_type=navigation`. Must be specified together with `origin_latitude`. If both `origin_latitude` and `origin_longitude` are not provided, the required `latitude` and `longitude` parameters are used as the origin. |

### Example Request

```bash
# Retrieve EV charge points within 1 kilometer of San Jose Airport.

$ curl "https://api.mapbox.com/ev/v1/locations?access_token=YOUR_MAPBOX_ACCESS_TOKEN&latitude=37.364714&longitude=-121.924238&distance=1"

# Retrieve EV charge points only from ChargePoint and Blink

$ curl "https://api.mapbox.com/ev/v1/locations?access_token=YOUR_MAPBOX_ACCESS_TOKEN&latitude=37.364714&longitude=-121.924238&distance=1&operators=ChargePoint,Blink"

# Retrieve EV charge points from charge point operators other than ChargePoint and Blink

$ curl "https://api.mapbox.com/ev/v1/locations?access_token=YOUR_MAPBOX_ACCESS_TOKEN&latitude=37.364714&longitude=-121.924238&distance=1&exclude_operators=ChargePoint,Blink"

# Retrieve EV charge points in Amsterdam that have a cafe and bus stop nearby.

$ curl "https://api.mapbox.com/ev/v1/locations?access_token=YOUR_MAPBOX_ACCESS_TOKEN&latitude=52.378657&longitude=4.89941&distance=10&amenities=bus_stop,cafe"

# Retrieve EV charge points but ignore locations that are only usable by Tesla drivers

$ curl "https://api.mapbox.com/ev/v1/locations?access_token=YOUR_MAPBOX_ACCESS_TOKEN&latitude=37.364714&longitude=-121.924238&distance=1&exclude=tesla_exclusive"

# Retrieve EV charge points in Amsterdam that accept credit or debit cards.

$ curl "https://api.mapbox.com/ev/v1/locations?access_token=YOUR_MAPBOX_ACCESS_TOKEN&latitude=52.378657&longitude=4.89941&distance=10&payment_methods=ad-hoc"

# Retrieve EV charge points in Amsterdam that are open 24/7.

$ curl "https://api.mapbox.com/ev/v1/locations?access_token=YOUR_MAPBOX_ACCESS_TOKEN&latitude=52.378657&longitude=4.89941&distance=10&opening_times=twentyfourseven"
```

### Response

The response to an EV Charge Finder API request is a [GeoJSON](https://geojson.org) [FeatureCollection](https://www.rfc-editor.org/rfc/rfc7946#section-3.3) object that contains the following properties:

| Property | Type | Description |
| --- | --- | --- |
| `type` | `string` | The GeoJSON object type, this value is always 'FeatureCollection'. |
| `features` | `array` | The GeoJSON Feature objects describe a relevant EV charge point. |
| `features[].type` | `array` | The GeoJSON object type, this value is always 'Feature'. |
| `features[].geometry` | `object` | A [GeoJSON Geometry](https://www.rfc-editor.org/rfc/rfc7946#section-3.1) object that describes where the EV charge point object is located. |
| `features[].properties` | `object` | An object that contains details about an OCPI Location. |
| `features[].properties.location` | `object` | An [OCPI Location](#ocpi-location) object. |
| `features[].properties.proximity` | `object` | A [Proximity](#proximity) object that describes the distance from the proximity location's latitude and longitude to the charge point. |

### Example Response

```json
{
  "type": "FeatureCollection",
  "features": [
    {
      "type": "Feature",
      "geometry": {
        "type": "Point",
        "coordinates": [-121.920559, 37.367285]
      },
      "properties": {
        "location": {
          "country_code": "US",
          "party_id": "MBX",
          "id": "dXJuOm1ieHBvaToxMjM0NQ==",
          "publish": false,
          "name": "Hudson Concourse",
          "address": "1741 Technology Drive",
          "city": "San Jose",
          "postal_code": "95110",
          "state": "CA",
          "country": "USA",
          "coordinates": {
            "latitude": "37.3672850",
            "longitude": "-121.9205590"
          },
          "evses": [
            {
              "uid": "82686",
              "evse_id": "82686",
              "status": "AVAILABLE",
              "connectors": [
                {
                  "id": "5",
                  "standard": "IEC_62196_T1",
                  "format": "CABLE",
                  "power_type": "AC_2_PHASE",
                  "max_voltage": 240,
                  "max_amperage": 27,
                  "max_electric_power": 6.5,
                  "last_updated": "2022-08-11T14:03:44"
                }
              ],
              "coordinates": {
                "latitude": "37.3672850",
                "longitude": "-121.9205590"
              },
              "last_updated": "2022-08-11T14:03:44"
            },
            {
              "uid": "82687",
              "evse_id": "82687",
              "status": "AVAILABLE",
              "connectors": [
                {
                  "id": "5",
                  "standard": "IEC_62196_T1",
                  "format": "CABLE",
                  "power_type": "AC_1_PHASE",
                  "max_voltage": 240,
                  "max_amperage": 27,
                  "max_electric_power": 6.5,
                  "last_updated": "2022-08-11T14:03:44"
                }
              ],
              "coordinates": {
                "latitude": "37.3672850",
                "longitude": "-121.9205590"
              },
              "last_updated": "2022-08-11T14:03:44"
            }
          ],
          "operator": {
            "name": "ChargePoint"
          },
          "owner": {
            "name": "ChargePoint"
          },
          "time_zone": "America/Los_Angeles",
          "opening_times": {
            "twentyfourseven": true
          },
          "last_updated": "2022-08-11T14:03:44"
        },
        "proximity": {
          "latitude": 37.364714,
          "longitude": -121.924238,
          "distance": 0.43294229650723554
        }
      }
    }
  ]
}
```

### Errors

| HTTP Status Code | Response Payload | Description |
| --- | --- | --- |
| `400` | Bad Request | The feature is not enabled for the provided Mapbox access token, or an expected parameter, either latitude, longitude, or distance, was not provided. |
| `401` | Not Authorized | The API is not authorized for use for the provided access token. |

## Get Charge Point details by ID

Get charge point details by ID. First, use the [Search for EV Charge Points](#search-for-ev-charge-points) feature to get a list of charge points. Then, get a specific charge point's ID from the `id` property of the [OCPI Location](#ocpi-location) object and use it to get detailed information about that charge point.

**GET** : `https://api.mapbox.com/ev/v1/locations/{location_id}`

### Required Parameters

| Parameter | Type | Description |
| --- | --- | --- |
| `access_token` | `string` | A valid Mapbox [access token](https://docs.mapbox.com/api/guides/#access-tokens-and-token-scopes). |
| `location_id` | `string` | `id` property of [OCPI Location](#ocpi-location) object |

### Example Request

```bash
# Get details of a charge point near San Jose Airport.

$ curl "https://api.mapbox.com/ev/v1/locations/dXJuOm1ieGV2OmY3Y2E2ZmNlLTZjYTAtMTFlZC1hYjVjLWEwNDIzZjQyYWYwNjtzcmM9NA?access_token=YOUR_MAPBOX_ACCESS_TOKEN"
```

### Response

The response is a [GeoJSON](https://geojson.org) [FeatureCollection](https://www.rfc-editor.org/rfc/rfc7946#section-3.3) object that contains the following properties:

| Property | Type | Description |
| --- | --- | --- |
| `type` | `string` | The GeoJSON object type, this value is always 'Feature'. |
| `geometry` | `object` | A [GeoJSON Geometry](https://www.rfc-editor.org/rfc/rfc7946#section-3.1) object that describes where the EV charge point object is located. |
| `properties` | `object` | An object that contains details about an OCPI Location. |
| `properties.location` | `object` | An [OCPI Location](#ocpi-location) object. |
| `properties.proximity` | `object` | A [Proximity](#proximity) object that describes the distance from the proximity location's latitude and longitude to the charge point. |
| `properties.tariffs` | `array` | An array of [Tariff](#ocpi-tariff) objects that describes the detailed price information for charging. |

### Example Response

```json
{
  "type": "Feature",
  "geometry": {
    "type": "Point",
    "coordinates": [
      -121.928896,
      37.370011
    ]
  },
  "properties": {
    "location": {
      "country_code": "US",
      "party_id": "MBX",
      "id": "dXJuOm1ieGV2OmY3Y2E2ZmNlLTZjYTAtMTFlZC1hYjVjLWEwNDIzZjQyYWYwNjtzcmM9NA",
      "publish": true,
      "address": "1701 Airport Boulevard",
      "city": "San Jose",
      "country": "USA",
      "coordinates": {
        "latitude": "37.370011",
        "longitude": "-121.928896"
      },
      "time_zone": "America/Los_Angeles",
      "last_updated": "2024-11-07T08:09:09Z",
      "evses": [
        {
          "uid": "538824",
          "status": "AVAILABLE",
          "last_updated": "2024-11-07T08:09:09Z",
          "connectors": [
            {
              "id": "5",
              "standard": "IEC_62196_T1",
              "format": "CABLE",
              "power_type": "AC_1_PHASE",
              "max_voltage": 240,
              "max_amperage": 27,
              "last_updated": "2024-11-07T08:09:09Z",
              "tariff_ids": [
                "bd40fa72d66b359796c0d10e9c64389c"
              ],
              "max_electric_power": 6480
            }
          ],
          "evse_id": "538824"
        }
      ],
      "name": "Norman Y. Mineta San Jose International Airport",
      "postal_code": "95110",
      "parking_type": "ALONG_MOTORWAY",
      "operator": {
        "name": "ChargePoint"
      },
      "owner": {
        "name": "ChargePoint",
        "website": "https://www.chargepoint.com/"
      },
      "opening_times": {
        "twentyfourseven": false,
        "regular_hours": [
          {
            "weekday": 1,
            "period_begin": "00:00",
            "period_end": "24:00"
          },
          {
            "weekday": 2,
            "period_begin": "00:00",
            "period_end": "24:00"
          },
          {
            "weekday": 3,
            "period_begin": "00:00",
            "period_end": "24:00"
          },
          {
            "weekday": 4,
            "period_begin": "00:00",
            "period_end": "24:00"
          },
          {
            "weekday": 5,
            "period_begin": "00:00",
            "period_end": "24:00"
          },
          {
            "weekday": 6,
            "period_begin": "00:00",
            "period_end": "24:00"
          },
          {
            "weekday": 7,
            "period_begin": "00:00",
            "period_end": "24:00"
          }
        ],
      },
      "charging_when_closed": false,
    },
    "tariffs": [
      {
        "country_code": "US",
        "party_id": "MBX",
        "id": "bd40fa72d66b359796c0d10e9c64389c",
        "currency": "USD",
        "last_updated": "2024-11-07T03:17:19Z",
        "elements": [
          {
            "price_components": [
              {
                "type": "FLAT",
                "price": 0,
                "step_size": 0
              }
            ]
          }
        ],
        "tariff_alt_text": [],
        "type": "AD_HOC_PAYMENT"
      }
    ]
  }
}
```

### Errors

| HTTP Status Code | Response Payload | Description |
| --- | --- | --- |
| `401` | Not Authorized | The API is not authorized for use for the provided access token. |
| `404` | Not Found | The charge point is not found. |

## Retrieve list of Charge Point Operators

Retrieve a list of charge point operators. The response includes `party_id`, `name`, and `country_code`, helping users to search for charge points from preferred operators using [Search for EV Charge Points](#search-for-ev-charge-points).

**GET** : `https://api.mapbox.com/ev/v1/operators`

### Required Parameters

| Parameter | Type | Description |
| --- | --- | --- |
| `access_token` | `string` | A valid Mapbox [access token](https://docs.mapbox.com/api/guides/#access-tokens-and-token-scopes). |

### Example Request

```bash
# Retrieve the Operators list.

$ curl "https://api.mapbox.com/ev/v1/operators?access_token=YOUR_MAPBOX_ACCESS_TOKEN"
```

### Response

The response is a Array of objects that contains the following properties:

| Property | Type | Cardinality | Description |
| --- | --- | --- | --- |
| `data` | `array` | * | Array of [Operator](#operator) representing EV Charge Point operator. |

### Example Response

```json
{
  "data": [
    {
      "party_id": "BCH",
      "name": "Bc Hydro",
      "country_code": "CA"
    },
    {
      "party_id": "CRK",
      "name": "CircleK/Couche-Tard Recharge",
      "country_code": "CA"
    },
    {
      "party_id": "SIM",
      "name": "ChargeHub",
      "country_code": "CA"
    },
    {
      "party_id": "ECS",
      "name": "EVCS",
      "country_code": "US"
    },
    {
      "party_id": "GYW",
      "name": "evGateway",
      "country_code": "US"
    }
  ]
}
```

### Errors

| HTTP Status Code | Response Payload | Description |
| --- | --- | --- |
| `401` | Not Authorized | The API is not authorized for use for the provided access token. |

## Data Models

The structured representations of data used in the EV Charge Finder API are defined below.

### `Proximity`

proximity in which to conduct a search for charge point POIs.

| Property | Type | Cardinality | Description |
| --- | --- | --- | --- |
| `latitude` | `float` | 1 | Latitude of the proximity in decimal degree. The latitude must be a number between `-90.0` and `90.0`. |
| `longitude` | `float` | 1 | Longitude of the proximity in decimal degree. The longitude must be a number between `-180.0` and `180.0`. |
| `distance` | `float` | 1 | The distance from the origin to the charging station, in kilometers. If `origin_latitude` and `origin_longitude` are provided, `distance` is calculated from this origin point. If not provided, `distance` is calculated from the required `latitude` and `longitude` parameters. When using `eta_type=navigation` this returns the route-based driving distance, otherwise this returns the straight-line distance. |
| `eta` | `float` | ? | The estimated time of arrival from the origin to the charging station, in minutes. If `origin_latitude` and `origin_longitude` are provided, ETA is calculated from this origin point. If not provided, ETA is calculated from the required `latitude` and `longitude` parameters. This field is returned only when the request includes `eta_type=navigation`. The ETA is calculated using driving traffic conditions at the time of the request. |

### `Operator`

Charge point operator that owns EV charge points.

| Property | Type | Cardinality | Description |
| --- | --- | --- | --- |
| `party_id` | `string` | 1 | 3 letter identifier of the operator. |
| `name` | `string` | 1 | Name of the operator. |
| `country_code` | `string` | 1 | [ISO-3166 alpha-2](https://www.iso.org/iso-3166-country-codes.html) country code of the CPO's home country (where the CPO is registered). |
| `country` | `string` | 1 | [ISO-3166-1 alpha-3](https://www.iso.org/iso-3166-country-codes.html) country code of a country where the CPO operates charging locations. |

### OCPI `Location`

The Location object describes the location and its properties where a group of EVSEs that belong together are installed. Refer to this type in the [OCPI GitHub repository](https://github.com/ocpi/ocpi/blob/2.2.1/mod_locations.asciidoc#mod_locations_location_object) for more details.

| Property | Type | Cardinality | Description |
| --- | --- | --- | --- |
| `country_code` | `string` | 1 | [ISO-3166 alpha-2](https://www.iso.org/iso-3166-country-codes.html) country code of the CPO's home country (where the CPO is registered). |
| `party_id` | `string` | 1 | 3 letter identifier of the operator. |
| `id` | `string` | 1 | Uniquely identifies the location among all operators. This field can never be changed, modified or renamed. |
| `publish` | `boolean` | 1 | Defines if a Location may be published on a website or app etc. When set to false, this location is only visible by tokens identified in the field: `publish_allowed_to`. When the same location has EVSEs that may be published and may not be published, two 'Locations' should be created. |
| `publish_allowed_to` | `object` | * | [`PublishTokenType`](#ocpi-publishtokentype) object. This field may only be used when the publish field is `false`. The location is shared only with owners of Tokens that match all the set fields of one `PublishToken` in the list. |
| `name` | `string` | ? | Display name of the location. |
| `address` | `string` | 1 | Street/block name and house number if available. |
| `city` | `string` | 1 | City or town. |
| `postal_code` | `string` | ? | Postal code of the location, may only be omitted when the location has no postal code: in some countries charging locations at highways don't have postal codes. |
| `state` | `string` | ? | State or province of the location, only to be used when relevant. |
| `country` | `string` | 1 | [ISO-3166-1 alpha-3](https://www.iso.org/iso-3166-country-codes.html) country code of the country where the CPO operates this charging location. |
| `coordinates` | `object` | 1 | [`GeoLocation`](#ocpi-geolocation) object representing coordinates of the location. |
| `related_locations` | `array` | * | [`AdditionalGeoLocation`](#ocpi-additionalgeolocation) object representing geographical location of related points relevant to the user. |
| `parking_type` | `string` | ? | [`ParkingType`](#ocpi-parkingtype) value. The general type of parking at the charge point location. |
| `evses` | `array` | * | Array of [`EVSE`](#ocpi-evse) objects that belong to this Location. |
| `directions` | `object` | * | [`DisplayText`](#ocpi-displaytext) object representing human-readable directions on how to reach the location. |
| `operator` | `object` | ? | [`BusinessDetails`](#ocpi-businessdetails) object representing information of the operator. When not specified, the information retrieved from the Credentials module, selected by the country_code and party_id of this Location, should be used instead. |
| `suboperator` | `object` | ? | [`BusinessDetails`](#ocpi-businessdetails) object representing information of the suboperator if available. |
| `owner` | `object` | ? | [`BusinessDetails`](#ocpi-businessdetails) object representing information of the owner if available. |
| `help_phone` | `string` | ? | A telephone number that a driver using the location may call for help. Calling this number will typically connect the caller to the CPO's customer service department. |
| `facilities` | `array` | * | Array of [`Facility`](#ocpi-facility) this charging location directly belongs to. |
| `time_zone` | `string` | 1 | One of [IANA time zone](http://www.iana.org/time-zones) data's TZ-values representing the time zone of the location. Examples: "Europe/Oslo", "Europe/Zurich". |
| `opening_times` | `object` | ? | [`Hours`](#ocpi-hours) object representing the times when the EVSEs at the location can be accessed for charging. |
| `charging_when_closed` | `boolean` | ? | Indicates if the EVSEs are still charging outside the opening hours of the location. for example when the parking garage closes its barriers over night, is it allowed to charge till the next morning? Default: true |
| `images` | `object` | * | [`Image`](#ocpi-image) object representing links to images related to the location such as photos or logos. |
| `energy_mix` | `object` | ? | [`EnergyMix`](#ocpi-energymix) object representing details on the energy supplied at this location. |
| `feedback` | `object` | ? | [`Feedback`](#feedback) object representing quality and reliability for this location based on user feedback and station performance. |
| `last_updated` | `string` | 1 | Timestamp in OCPI [`DateTime`](#ocpi-datetime) format when this Location or one of its EVSEs or Connectors were last updated (or created). |

### OCPI `EVSE`

The EVSE object describes the part that controls the power supply to a single EV in a single session. It always belongs to a Location object. Refer to this type in the [OCPI GitHub repository](https://github.com/ocpi/ocpi/blob/2.2.1/mod_locations.asciidoc#mod_locations_evse_object) for more details.

| Property | Type | Cardinality | Description |
| --- | --- | --- | --- |
| `uid` | `string` | 1 | Uniquely identifies the EVSE within the operators' platform (and suboperator platforms). For example a database ID or the actual "EVSE ID". This field can never be changed, modified or renamed. This is the 'technical' identification of the EVSE, not to be used as 'human readable' identification, use the field `evse_id` for that. |
| `evse_id` | `string` | ? | Compliant with the following specification for EVSE ID from "eMI3 standard version V1.0" ([http://emi3group.com/documents-links/](http://emi3group.com/documents-links/)) "Part 2: business objects." Optional because: If an EVSE ID is to be reused in the real world, you can remove the `evse_id` from an EVSE object if the status after setting its status to REMOVED. |
| `status` | `string` | 1 | [`Status`](#ocpi-status) value that indicates the current status of the EVSE. |
| `status_schedule` | `array` | * | Array of [`StatusSchedule`](#ocpi-statusschedule) representing a planned status update of the EVSE. |
| `capabilities` | `array` | * | Array of [`Capability`](#ocpi-capability) that the EVSE is capable of. |
| `connectors` | `array` | + | Array of available [`Connector`](#ocpi-connector) objects on the EVSE. |
| `floor_level` | `string` | ? | Level on which the Charge Point is located (in garage buildings) in the locally displayed numbering scheme. |
| `coordinates` | `object` | ? | [`GeoLocation`](#ocpi-geolocation) object representing coordinates of the EVSE. |
| `physical_reference` | `string` | ? | A number/string printed on the outside of the EVSE for visual identification. |
| `directions` | `array` | * | [`DisplayText`](#ocpi-displaytext) object representing multi-language human-readable directions when more detailed information on how to reach the EVSE from the Location is required. |
| `parking_restrictions` | `array` | * | Array of [`ParkingRestriction`](#ocpi-parkingrestriction) values that apply to the parking spot. |
| `images` | `array` | * | Array of [`Image`](#ocpi-image) objects representing links to images related to the EVSE such as photos or logos. |
| `last_updated` | `string` | 1 | Timestamp in [`DateTime`](#ocpi-datetime) format when this EVSE or one of its Connectors was last updated (or created). |

### OCPI `Connector`

A Connector is the socket or cable and plug available for the EV to use. A single EVSE may provide multiple Connectors but only one of them can be in use at the same time. A Connector always belongs to an EVSE object. Refer to this type in the [OCPI GitHub repository](https://github.com/ocpi/ocpi/blob/2.2.1/mod_locations.asciidoc#mod_locations_connector_object) for more details.

| Property | Type | Cardinality | Description |
| --- | --- | --- | --- |
| `id` | `string` | 1 | Identifier of the Connector within the EVSE. Two Connectors may have the same id as long as they do not belong to the same EVSE object. |
| `standard` | `string` | 1 | [`ConnectorType`](#ocpi-connectortype) value indicating the standard of the installed connector. |
| `format` | `string` | 1 | [`ConnectorFormat`](#ocpi-connectorformat) value indicating the format (socket/cable) of the installed connector. |
| `power_type` | `string` | 1 | [`PowerType`](#ocpi-powertype) value indicating electrical power configuration. |
| `max_voltage` | `integer` | 1 | Maximum voltage of the connector (line to neutral for AC_3_PHASE), in volt [V]. For example: DC Chargers might vary the voltage during charging when battery almost full. |
| `max_amperage` | `integer` | 1 | Maximum amperage of the connector, in ampere [A]. |
| `max_electric_power` | `integer` | ? | Maximum electric power that can be delivered by this connector, in Watts (W). Used when the maximum electric power is lower than the calculated value from voltage and amperage. For example: A DC Charge Point which can delivers up to 920V and up to 400A can be limited to a maximum of 150kW (max_electric_power = 150000). Depending on the car, it may supply max voltage or current, but not both at the same time. For AC Charge Points, the amount of phases used can also have influence on the maximum power. |
| `tariff_ids` | `string` | * | Identifiers of the valid charging tariffs. Multiple tariffs are possible, but only one of each Tariff.type can be active at the same time. Tariffs with the same type are only allowed if they are not active at the same time: start_date_time and end_date_time period not overlapping. Only included in the response of the [Get Charge Point details by ID API](#get-charge-point-details) |
| `terms_and_conditions` | `string` | ? | URL to the operator's terms and conditions. |
| `last_updated` | `string` | 1 | Timestamp in [`DateTime`](#ocpi-datetime) format when this Connector was last updated (or created). |

### OCPI `AdditionalGeoLocation`

This class defines an additional geographic location that is relevant for the Charge Point. The geodetic system to be used is `WGS 84`. Refer to this type in the [OCPI GitHub repository](https://github.com/ocpi/ocpi/blob/2.2.1/mod_locations.asciidoc#141-additionalgeolocation-class) for more details.

| Property | Type | Cardinality | Description |
| --- | --- | --- | --- |
| `latitude` | `string` | 1 | Latitude of the point in decimal degree. The latitude must be a number between `-90.0` and `90.0`. |
| `longitude` | `string` | 1 | Longitude of the point in decimal degree. The longitude must be a number between `-180.0` and `180.0`. |
| `name` | `object` | ? | [`DisplayText`](#ocpi-displaytext) object representing name of the point in local language or as written at the location. For example the street name of a parking lot entrance or it's number. |

### OCPI `BusinessDetails`

This class is information about a business operator, including its name, an optional website URL, and an optional logo image link. Refer to this type in the [OCPI GitHub repository](https://github.com/ocpi/ocpi/blob/2.2.1/mod_locations.asciidoc#142-businessdetails-class) for more details.

| Property | Type | Cardinality | Description |
| --- | --- | --- | --- |
| `name` | `string` | 1 | Name of the operator. |
| `website` | `string` | ? | Link to the operator's website. |
| `logo` | `object` | ? | [`Image`](#ocpi-image) object representing link to the operator's logo. |

### OCPI `Capability`

The capabilities of an EVSE. Refer to this type in the [OCPI GitHub repository](https://github.com/ocpi/ocpi/blob/2.2.1/mod_locations.asciidoc#143-capability-enum) for more details. The current non-exhaustive list of possible values are:

| Value | Description |
| --- | --- |
| `CHARGING_PROFILE_CAPABLE` | The EVSE supports charging profiles. |
| `CHARGING_PREFERENCES_CAPABLE` | The EVSE supports charging preferences. |
| `CHIP_CARD_SUPPORT` | EVSE has a payment terminal that supports chip cards. |
| `CONTACTLESS_CARD_SUPPORT` | EVSE has a payment terminal that supports contactless cards. |
| `CREDIT_CARD_PAYABLE` | EVSE has a payment terminal that makes it possible to pay for charging using a credit card. |
| `DEBIT_CARD_PAYABLE` | EVSE has a payment terminal that makes it possible to pay for charging using a debit card. |
| `PED_TERMINAL` | EVSE has a payment terminal with a pin-code entry device. |
| `REMOTE_START_STOP_CAPABLE` | The EVSE can remotely be started/stopped. |
| `RESERVABLE` | The EVSE can be reserved. |
| `RFID_READER` | Charging at this EVSE can be authorized with an RFID token. |
| `START_SESSION_CONNECTOR_REQUIRED` | When a `StartSession` is received by this EVSE, the eMSP is required to add the optional connector_id field in the `StartSession` object. |
| `TOKEN_GROUP_CAPABLE` | This EVSE supports token groups, two or more tokens work as one, so that a session can be started with one token and stopped with another (handy when an EV driver has both a card and key-fob). |
| `UNLOCK_CAPABLE` | Connectors have mechanical lock that can be requested by the eMSP to be unlocked. |

### OCPI `ConnectorFormat`

The format of the connector, whether it is a socket or a plug. Refer to this type in the [OCPI GitHub repository](https://github.com/ocpi/ocpi/blob/2.2.1/mod_locations.asciidoc#144-connectorformat-enum) for more details.

| Value | Description |
| --- | --- |
| `SOCKET` | The connector is a socket; the EV user needs to bring a fitting plug. |
| `CABLE` | The connector is an attached cable; the EV users car needs to have a fitting inlet. |

### OCPI `ConnectorType`

The socket or plug standard of the charge point. Refer to this type in the [OCPI GitHub repository](https://github.com/ocpi/ocpi/blob/2.2.1/mod_locations.asciidoc#145-connectortype-enum) for more details. The current non-exhaustive list of possible values are:

| Value | Description |
| --- | --- |
| `CHADEMO` | The connector type is CHAdeMO, DC |
| `CHAOJI` | The ChaoJi connector. The new generation charging connector, harmonized between CHAdeMO and GB/T. DC. |
| `DOMESTIC_A` | Standard/Domestic household, type "A", NEMA 1-15, 2 pins |
| `DOMESTIC_B` | Standard/Domestic household, type "B", NEMA 5-15, 3 pins |
| `DOMESTIC_C` | Standard/Domestic household, type "C", CEE 7/17, 2 pins |
| `DOMESTIC_D` | Standard/Domestic household, type "D", 3 pin |
| `DOMESTIC_E` | Standard/Domestic household, type "E", CEE 7/5 3 pins |
| `DOMESTIC_F` | Standard/Domestic household, type "F", CEE 7/4, Schuko, 3 pins |
| `DOMESTIC_G` | Standard/Domestic household, type "G", BS 1363, Commonwealth, 3 pins |
| `DOMESTIC_H` | Standard/Domestic household, type "H", SI-32, 3 pins |
| `DOMESTIC_I` | Standard/Domestic household, type "I", AS 3112, 3 pins |
| `DOMESTIC_J` | Standard/Domestic household, type "J", SEV 1011, 3 pins |
| `DOMESTIC_K` | Standard/Domestic household, type "K", DS 60884-2-D1, 3 pins |
| `DOMESTIC_L` | Standard/Domestic household, type "L", CEI 23-16-VII, 3 pins |
| `DOMESTIC_M` | Standard/Domestic household, type "M", BS 546, 3 pins |
| `DOMESTIC_N` | Standard/Domestic household, type "N", NBR 14136, 3 pins |
| `DOMESTIC_O` | Standard/Domestic household, type "O", TIS 166-2549, 3 pins |
| `GBT_AC` | Guobiao GB/T 20234.2 AC socket/connector |
| `GBT_DC` | Guobiao GB/T 20234.3 DC connector |
| `IEC_60309_2_single_16` | IEC 60309-2 Industrial Connector single phase 16 amperes (usually blue) |
| `IEC_60309_2_three_16` | IEC 60309-2 Industrial Connector three phases 16 amperes (usually red) |
| `IEC_60309_2_three_32` | IEC 60309-2 Industrial Connector three phases 32 amperes (usually red) |
| `IEC_60309_2_three_64` | IEC 60309-2 Industrial Connector three phases 64 amperes (usually red) |
| `IEC_62196_T1` | IEC 62196 Type 1 "SAE J1772" |
| `IEC_62196_T1_COMBO` | Combo Type 1 based, DC |
| `IEC_62196_T2` | IEC 62196 Type 2 "Mennekes" |
| `IEC_62196_T2_COMBO` | Combo Type 2 based, DC |
| `IEC_62196_T3A` | IEC 62196 Type 3A |
| `IEC_62196_T3C` | IEC 62196 Type 3C "Scame" |
| `MCS` | The MegaWatt Charging System (MCS) connector as developed by CharIN |
| `NEMA_5_20` | NEMA 5-20, 3 pins |
| `NEMA_6_30` | NEMA 6-30, 3 pins |
| `NEMA_6_50` | NEMA 6-50, 3 pins |
| `NEMA_10_30` | NEMA 10-30, 3 pins |
| `NEMA_10_50` | NEMA 10-50, 3 pins |
| `NEMA_14_30` | NEMA 14-30, 3 pins, rating of 30 A |
| `NEMA_14_50` | NEMA 14-50, 3 pins, rating of 50 A |
| `PANTOGRAPH_BOTTOM_UP` | On-board Bottom up Pantograph typically for bus charging |
| `PANTOGRAPH_TOP_DOWN` | Off-board Top down Pantograph typically for bus charging |
| `SAE_J3400` | SAE J3400, also known as North American Charging Standard (NACS), developed by Tesla |
| `TESLA_R` | Tesla Connector "Roadster"-type (round, 4 pin) |
| `TESLA_S` | Tesla Connector "Model-S"-type (oval, 5 pin) |

### OCPI `EnergyMix`

This type is used to specify the energy mix and environmental impact of the supplied energy at a location or in a tariff. Refer to this type in the [OCPI GitHub repository](https://github.com/ocpi/ocpi/blob/2.2.1/mod_locations.asciidoc#146-energymix-class) for more details.

| Property | Type | Cardinality | Description |
| --- | --- | --- | --- |
| `is_green_energy` | `boolean` | 1 | True if 100% from regenerative sources. (CO2 and nuclear waste is zero) |
| `energy_sources` | `array` | * | Array of [`EnergySource`](#ocpi-energysource) objects representing energy sources of this location's tariff in key-value pairs (enum + percentage). |
| `environ_impact` | `array` | * | Array of [`EnvironmentalImpact`](#ocpi-environmentalimpact) objects representing nuclear waste and CO2 exhaust of this location's tariff in Key-value pairs (enum + percentage). |
| `supplier_name` | `string` | ? | Name of the energy supplier, delivering the energy for this location or tariff. |
| `energy_product_name` | `string` | ? | Name of the energy suppliers product/tariff plan used at this location. |

### OCPI `EnergySource`

Key-value pairs (enum + percentage) of energy sources. All given values of all categories should add up to 100 percent. Refer to this type in the [OCPI GitHub repository](https://github.com/ocpi/ocpi/blob/2.2.1/mod_locations.asciidoc#147-energysource-class) for more details.

| Property | Type | Cardinality | Description |
| --- | --- | --- | --- |
| `source` | `string` | 1 | [`EnergySourceCategory`](#ocpi-energysourcecategory) value indicating the type of energy source. |
| `percentage` | `integer` | 1 | Percentage of this source (0-100) in the mix. |

### OCPI `EnergySourceCategory`

Categories of energy sources. Refer to this type in the [OCPI GitHub repository](https://github.com/ocpi/ocpi/blob/2.2.1/mod_locations.asciidoc#148-energysourcecategory-enum) for more details. The current non-exhaustive list of possible values are:

| Value | Description |
| --- | --- |
| `NUCLEAR` | Nuclear power sources. |
| `GENERAL_FOSSIL` | All kinds of fossil power sources. |
| `COAL` | Fossil power from coal. |
| `GAS` | Fossil power from gas. |
| `GENERAL_GREEN` | All kinds of regenerative power sources. |
| `SOLAR` | Regenerative power from photo voltaic. |
| `WIND` | Regenerative power from wind turbines. |
| `WATER` | Regenerative power from water turbines. |

### OCPI `EnvironmentalImpact`

Amount of waste produced/emitted per kWh. Refer to this type in the [OCPI GitHub repository](https://github.com/ocpi/ocpi/blob/2.2.1/mod_locations.asciidoc#149-environmentalimpact-class) for more details.

| Property | Type | Cardinality | Description |
| --- | --- | --- | --- |
| `category` | `string` | 1 | [`EnvironmentalImpactCategory`](#ocpi-environmentalimpactcategory) value indicating the environmental impact category. |
| `amount` | `float` | 1 | Amount of this part in g/kWh. |

### OCPI `EnvironmentalImpactCategory`

Categories of environmental impact values. Refer to this type in the [OCPI GitHub repository](https://github.com/ocpi/ocpi/blob/2.2.1/mod_locations.asciidoc#1410-environmentalimpactcategory-enum) for more details. The current non-exhaustive list of possible values are:

| Value | Description |
| --- | --- |
| `NUCLEAR_WASTE` | Produced nuclear waste in grams per kilowatt hour. |
| `CARBON_DIOXIDE` | Exhausted carbon dioxide in grams per kilowatt hour. |

### `Feedback`

The Feedback object describes user-based feedback for this location.

| Property | Type | Cardinality | Description |
| --- | --- | --- | --- |
| `average_rating` | `float` | ? | The average of all user-provided ratings. Values range from 1 to 5. |
| `rating_count` | `integer` | ? | The total number of user-provided ratings. |

### OCPI `ExceptionalPeriod`

Specifies one exceptional period for opening or access hours. Refer to this type in the [OCPI GitHub repository](https://github.com/ocpi/ocpi/blob/2.2.1/mod_locations.asciidoc#1411-exceptionalperiod-class) for more details.

| Property | Type | Cardinality | Description |
| --- | --- | --- | --- |
| `period_begin` | `string` | 1 | Begin timestamp of the exception in [`DateTime`](#ocpi-datetime) format. In UTC, time_zone field can be used to convert to local time. |
| `period_end` | `string` | 1 | End timestamp of the exception in [`DateTime`](#ocpi-datetime) format. In UTC, time_zone field can be used to convert to local time. |

### OCPI `Facility`

A facility to which a charging location directly belongs. Refer to this type in the [OCPI GitHub repository](https://github.com/ocpi/ocpi/blob/2.2.1/mod_locations.asciidoc#1412-facility-enum) for more details. The current non-exhaustive list of possible values are:

| Value | Description |
| --- | --- |
| `HOTEL` | A hotel. |
| `RESTAURANT` | A restaurant. |
| `CAFE` | A cafe. |
| `MALL` | A mall or shopping center. |
| `SUPERMARKET` | A supermarket. |
| `SPORT` | Sport facilities: gym, field etc. |
| `RECREATION_AREA` | A recreation area. |
| `NATURE` | Located in, or close to, a park, nature reserve etc. |
| `MUSEUM` | A museum. |
| `BIKE_SHARING` | A bike/e-bike/e-scooter sharing location. |
| `BUS_STOP` | A bus stop. |
| `TAXI_STAND` | A taxi stand. |
| `TRAM_STOP` | A tram stop/station. |
| `METRO_STATION` | A metro station. |
| `TRAIN_STATION` | A train station. |
| `AIRPORT` | An airport. |
| `PARKING_LOT` | A parking lot. |
| `CARPOOL_PARKING` | A carpool parking. |
| `FUEL_STATION` | A Fuel station. |
| `WIFI` | Wifi or other type of internet available. |
| `ROOF` | Charging location is protected by a roof. |
| `RESTROOM` | Restrooms are available. |
| `LIGHT` | Charging location is well-lit. |
| `DOG_GROUND` | A dog ground. |
| `TRAILER_FRIENDLY` | Charging location is suitable for trailers. |

### OCPI `GeoLocation`

This class defines the geographic location of the Charge Point. The geodetic system to be used is `WGS 84`. Refer to this type in the [OCPI GitHub repository](https://github.com/ocpi/ocpi/blob/2.2.1/mod_locations.asciidoc#1413-geolocation-class) for more details.

| Property | Type | Cardinality | Description |
| --- | --- | --- | --- |
| `latitude` | `string` | 1 | Latitude of the point in decimal degree. The latitude must be a number between `-90.0` and `90.0`. |
| `longitude` | `string` | 1 | Longitude of the point in decimal degree. The longitude must be a number between `-180.0` and `180.0`. |

### OCPI `Hours`

Opening and access hours of the location. Refer to this type in the [OCPI GitHub repository](https://github.com/ocpi/ocpi/blob/2.2.1/mod_locations.asciidoc#1414-hours-class) for more details.

| Property | Type | Cardinality | Description |
| --- | --- | --- | --- |
| `twentyfourseven` | `boolean` | 1 | True to represent 24 hours a day and 7 days a week, except the given exceptions. |
| `regular_hours` | `array` | * | Array of [`RegularHours`](#ocpi-regularhours) objects representing regular hours, weekday-based. Only to be used if `twentyfourseven=false`, then this field needs to contain at least one `RegularHours` object. |
| `exceptional_openings` | `array` | * | Array of [`ExceptionalPeriod`](#ocpi-exceptionalperiod) objects representing exceptions for specified calendar dates, time-range based. Times the station is operating/accessible. Additional to regular_hours. May overlap regular rules. |
| `exceptional_closings` | `array` | * | Array of [`ExceptionalPeriod`](#ocpi-exceptionalperiod) objects representing exceptions for specified calendar dates, time-range based. Times the station is not operating/accessible. Overwriting regular_hours and exceptional_openings. Should not overlap exceptional_openings. |

### OCPI `Image`

This class references an image related to an EVSE as a file name or URL. Refer to this type in the [OCPI GitHub repository](https://github.com/ocpi/ocpi/blob/2.2.1/mod_locations.asciidoc#1415-image-class) for more details.

| Property | Type | Cardinality | Description |
| --- | --- | --- | --- |
| `url` | `string` | 1 | URL from where the image data can be fetched through a web browser. |
| `thumbnail` | `string` | ? | URL from where a thumbnail of the image can be fetched through a web browser. |
| `category` | `string` | 1 | [`ImageCategory`](#ocpi-imagecategory) value indicating what the image is used for. |
| `type` | `string` | 1 | Image type like: GIF, JPG, PNG, SVG. |
| `width` | `integer` | ? | Width of the full scale image. |
| `height` | `integer` | ? | Height of the full scale image. |

### OCPI `ImageCategory`

The category of an image to get the correct usage in a user presentation. Refer to this type in the [OCPI GitHub repository](https://github.com/ocpi/ocpi/blob/2.2.1/mod_locations.asciidoc#1416-imagecategory-enum) for more details. The current non-exhaustive list of possible values are:

| Value | Description |
| --- | --- |
| `CHARGER` | Photo of the physical device that contains one or more EVSEs. |
| `ENTRANCE` | Location entrance photo. Should show the car entrance to the location from street side. |
| `LOCATION` | Location overview photo. |
| `NETWORK` | Logo of an associated roaming network to be displayed with the EVSE for example in lists, maps and detailed information views. |
| `OPERATOR` | Logo of the charge point operator, for example a municipality, to be displayed in the EVSEs detailed information view or in lists and maps, if no network logo is present. |
| `OTHER` | Other |
| `OWNER` | Logo of the charge point owner, for example a local store, to be displayed in the EVSEs detailed information view. |

### OCPI `ParkingRestriction`

This value, if provided, is the restriction to the parking spot for different purposes. Refer to this type in the [OCPI GitHub repository](https://github.com/ocpi/ocpi/blob/2.2.1/mod_locations.asciidoc#1417-parkingrestriction-enum) for more details. The current non-exhaustive list of possible values are:

| Value | Description |
| --- | --- |
| `EV_ONLY` | Reserved parking spot for electric vehicles. |
| `PLUGGED` | Parking is only allowed while plugged in (charging). |
| `DISABLED` | Reserved parking spot for disabled people with valid ID. |
| `CUSTOMERS` | Parking spot for customers/guests only, for example in case of a hotel or shop. |
| `MOTORCYCLES` | Parking spot only suitable for (electric) motorcycles or scooters. |

### OCPI `ParkingType`

Reflects the general type of the charge point's location. May be used for user information. Refer to this type in the [OCPI GitHub repository](https://github.com/ocpi/ocpi/blob/2.2.1/mod_locations.asciidoc#1418-parkingtype-enum) for more details. The current non-exhaustive list of possible values are:

| Value | Description |
| --- | --- |
| `ALONG_MOTORWAY` | Location on a parking facility/rest area along a motorway, freeway, interstate, highway etc. |
| `PARKING_GARAGE` | Multistorey car park. |
| `PARKING_LOT` | A cleared area that is intended for parking vehicles, as in at super markets, bars, etc. |
| `ON_DRIVEWAY` | Location is on the driveway of a house/building. |
| `ON_STREET` | Parking in public space along a street. |
| `UNDERGROUND_GARAGE` | Multistorey car park, mainly underground. |

### OCPI `PowerType`

This value indicates an electrical power configuration of a connector. Refer to this type in the [OCPI GitHub repository](https://github.com/ocpi/ocpi/blob/2.2.1/mod_locations.asciidoc#1419-powertype-enum) for more details. The current non-exhaustive list of possible values are:

| Value | Description |
| --- | --- |
| `AC_1_PHASE` | AC single phase. |
| `AC_2_PHASE` | AC two phases, only two of the three available phases connected. |
| `AC_2_PHASE_SPLIT` | AC two phases using split phase system. |
| `AC_3_PHASE` | AC three phases. |
| `DC` | Direct Current. |

### OCPI `PublishTokenType`

Defines the set of values that identify a token to which a Location might be published. Refer to this type in the [OCPI GitHub repository](https://github.com/ocpi/ocpi/blob/2.2.1/mod_locations.asciidoc#1420-publishtokentype-class) for more details.

| Property | Type | Cardinality | Description |
| --- | --- | --- | --- |
| `uid` | `string` | ? | Unique ID by which this Token can be identified. |
| `type` | `string` | ? | [`TokenType`](#ocpi-tokentype) value indicating type of the token. |
| `visual_number` | `string` | ? | Visual readable number/identification as printed on the Token (RFID card). |
| `issuer` | `string` | ? | Issuing company, most of the times the name of the company printed on the token (RFID card), not necessarily the eMSP. |
| `group_id` | `string` | ? | This ID groups a couple of tokens. This can be used to make two or more tokens work as one. |

### OCPI `TokenType`

Type of Token. Refer to this type in the [OCPI GitHub repository](Refer to this type in the [OCPI GitHub repository](https://github.com/ocpi/ocpi/blob/2.2.1/mod_tokens.asciidoc#mod_tokens_tokentype_enum) for more details. The current non-exhaustive list of possible values are:

| Value | Description |
| --- | --- |
| `AD_HOC_USER` | One time use Token ID generated by a server (or App.) The eMSP uses this to bind a Session to a customer, probably an app user. |
| `APP_USER` | Token ID generated by a server (or App.) to identify a user of an App. The same user uses the same Token for every Session. |
| `OTHER` | Other type of token |
| `RFID` | RFID Token |

### OCPI `RegularHours`

Regular recurring operation or access hours. Refer to this type in the [OCPI GitHub repository](https://github.com/ocpi/ocpi/blob/2.2.1/mod_locations.asciidoc#1421-regularhours-class) for more details.

| Property | Type | Cardinality | Description |
| --- | --- | --- | --- |
| `weekday` | `integer` | 1 | Number of day in the week, from Monday (1) till Sunday (7) |
| `period_begin` | `string` | 1 | Begin of the regular period, in local time, given in hours and minutes. Must be in 24h format with leading zeros. Example: "18:15". |
| `period_end` | `string` | 1 | End of the regular period, in local time, syntax as for period_begin. Must be later than period_begin. |

### OCPI `Status`

The status of an EVSE. Refer to this type in the [OCPI GitHub repository](https://github.com/ocpi/ocpi/blob/2.2.1/mod_locations.asciidoc#1422-status-enum) for more details. The current non-exhaustive list of possible values are:

| Value | Description |
| --- | --- |
| `AVAILABLE` | The EVSE/Connector is able to start a new charging session. |
| `BLOCKED` | The EVSE/Connector is not accessible because of a physical barrier, as in a car. |
| `CHARGING` | The EVSE/Connector is in use. |
| `INOPERATIVE` | The EVSE/Connector is not yet active, or temporarily not available for use, but not broken or defect. |
| `OUTOFORDER` | The EVSE/Connector is out of order, some part/components may be broken/defective. |
| `PLANNED` | The EVSE/Connector is planned, will be operating soon. |
| `REMOVED` | The EVSE/Connector was discontinued/removed. |
| `RESERVED` | The EVSE/Connector is reserved for a particular EV driver and is unavailable for other drivers. |
| `UNKNOWN` | No status information available (also used when offline). |

### OCPI `StatusSchedule`

This type is used to schedule status period in the future. Refer to this type in the [OCPI GitHub repository](https://github.com/ocpi/ocpi/blob/2.2.1/mod_locations.asciidoc#1423-statusschedule-class) for more details.

| Property | Type | Cardinality | Description |
| --- | --- | --- | --- |
| `period_begin` | `string` | 1 | Begin timestamp in [`DateTime`](#ocpi-datetime) format of the scheduled period. |
| `period_end` | `string` | ? | End timestamp in [`DateTime`](#ocpi-datetime) format of the scheduled period, if known. |
| `status` | `Status` | 1 | Status value during the scheduled period. |

### OCPI `DateTime`

All timestamps are formatted as string following RFC 3339, with some additional limitations. Refer to this type in the [OCPI GitHub repository](https://github.com/ocpi/ocpi/blob/2.2.1/types.asciidoc#12-datetime-type) for more details.

### OCPI `DisplayText`

Refer to this type in the [OCPI GitHub repository](https://github.com/ocpi/ocpi/blob/2.2.1/types.asciidoc#13-displaytext-class) for more details.

| Property | Type | Cardinality | Description |
| --- | --- | --- | --- |
| `language` | `string` | 1 | Language Code ISO 639-1. |
| `text` | `string` | 1 | Text to be displayed to an end user. No markup, HTML etc. allowed. |

### OCPI `Price`

Type of price and cost. Refer to this type in the [OCPI GitHub repository](https://github.com/ocpi/ocpi/blob/2.2.1/types.asciidoc#15-price-class) for more details.

| Property | Type | Cardinality | Description |
| --- | --- | --- | --- |
| `excl_vat` | `float` | 1 | Price/Cost excluding VAT. |
| `incl_vat` | `float` | ? | Price/Cost including VAT. |

### OCPI `Tariff`

A Tariff object consists of a list of one or more Tariff Elements, which in turn consist of Price Components. A Tariff Element is a group of Price Components that apply under the same conditions. A Price Component describes how the usage of a particular dimension (time, energy, etc.) maps to an amount of money owed. Refer to this type in the [OCPI GitHub repository](https://github.com/ocpi/ocpi/blob/2.2.1/mod_tariffs.asciidoc#131-tariff-object) for more details.

| Property | Type | Cardinality | Description |
| --- | --- | --- | --- |
| `country_code` | `string` | 1 | [ISO-3166 alpha-2](https://www.iso.org/iso-3166-country-codes.html) country code of the operator that owns this Tariff. |
| `party_id` | `string` | 1 | 3 letter identifier of the operator. |
| `id` | `string` | 1 | Uniquely identifies the tariff within the CPO's platform (and suboperator platforms). |
| `currency` | `string` | 1 | ISO-4217 code of the currency of this tariff. |
| `type` | `string` | ? | [`TariffType`](#ocpi-tarifftype) of the tariff. This allows for distinction in case of given Charging Preferences. When omitted, this tariff is valid for all sessions. |
| `tariff_alt_text` | `array` | * | [`DisplayText`](#ocpi-displaytext) object representing list of multi-language alternative tariff information texts. |
| `tariff_alt_url` | `string` | ? | URL to a webpage that contains an explanation of the tariff information in human readable form. |
| `min_price` | `object` | ? | [`Price`](#ocpi-price) object. Use this property to declare a Charging Session tariff will base cost of this amount. This is different from a FLAT fee (Start Tariff, Transaction Fee), a FLAT fee is a fixed amount for any Charging Session. A minimum price indicates that when the cost of a Charging Session is lower than this amount, the cost of the Session will be equal to this amount. (Also see note below) |
| `max_price` | `object` | ? | [`Price`](#ocpi-price) object. Set this field and a Charging Session with this tariff will NOT cost more than this amount. (See note below) |
| `elements` | `array` | + | List of [`TariffElement`](#ocpi-tariffelement). |
| `start_date_time` | `string` | ? | The time in OCPI [`DateTime`](#ocpi-datetime) format when this tariff becomes active, in UTC, time_zone field of the Location can be used to convert to local time. Typically used for a new tariff that is already given with the location, before it becomes active. (See note below) |
| `end_date_time` | `string` | ? | The time in OCPI [`DateTime`](#ocpi-datetime) format after which this tariff is no longer valid, in UTC, time_zone field if the Location can be used to convert to local time. Typically used when this tariff is going to be replaced with a different tariff soon. (See note below) |
| `energy_mix` | `object` | ? | Details on the energy supplied with this tariff. |
| `last_updated` | `string` | 1 | Timestamp in OCPI [`DateTime`](#ocpi-datetime) format when this Tariff was last updated (or created). |

### OCPI `TariffType`

The type of the tariff. Refer to this type in the [OCPI GitHub repository](https://github.com/ocpi/ocpi/blob/2.2.1/mod_tariffs.asciidoc#147-tarifftype-enum) for more details. The current non-exhaustive list of possible values are:

| Value | Description |
| --- | --- |
| `AD_HOC_PAYMENT` | Used to describe that a Tariff is valid when ad-hoc payment is used at the Charge Point (for example: Debit or Credit card payment terminal). |
| `PROFILE_CHEAP` | Used to describe that a Tariff is valid when setting Charging Preference: CHEAP for the session. |
| `PROFILE_FAST` | Used to describe that a Tariff is valid when setting Charging Preference: FAST for the session. |
| `PROFILE_GREEN` | Used to describe that a Tariff is valid when setting Charging Preference: GREEN for the session. |
| `REGULAR` | Used to describe that a Tariff is valid when using an RFID, without any Charging Preference, or when setting Charging Preference: REGULAR for the session. |

### OCPI `TariffElement`

The type of tariff element. Refer to this type in the [OCPI GitHub repository](https://github.com/ocpi/ocpi/blob/2.2.1/mod_tariffs.asciidoc#mod_tariffs_tariffelement_class) for more details.

| Property | Type | Cardinality | Description |
| --- | --- | --- | --- |
| `price_components` | `array` | + | List of [`PriceComponent`](#ocpi-pricecomponent) that describe the pricing of a tariff. |
| `restrictions` | `object` | ? | List of [`TariffRestrictions`](#ocpi-tariffrestrictions) Restrictions that describe the applicability of a tariff. |

### OCPI `PriceComponent`

The type of price component for tariff. Refer to this type in the [OCPI GitHub repository](https://github.com/ocpi/ocpi/blob/2.2.1/mod_tariffs.asciidoc#142-pricecomponent-class) for more details.

| Property | Type | Cardinality | Description |
| --- | --- | --- | --- |
| `type` | `string` | 1 | [`TariffDimensionType`](#ocpi-tariffdimensiontype) of tariff dimension. |
| `price` | `float` | 1 | Price per unit (excl. VAT) for this tariff dimension. |
| `vat` | `float` | ? | Applicable VAT percentage for this tariff dimension. If omitted, no VAT is applicable. Not providing a VAT is different from 0% VAT, which would be a value of 0.0 here. |
| `step_size` | `integer` | 1 | Minimum amount to be billed. This unit will be billed in this step_size blocks. Amounts that are less then this step_size are rounded up to the given step_size. For example: if type is TIME and step_size has a value of 300, then time will be billed in blocks of 5 minutes. If 6 minutes were used, 10 minutes (2 blocks of step_size) will be billed. |

### OCPI `TariffDimensionType`

Type of tariff dimension. Refer to this type in the [OCPI GitHub repository](https://github.com/ocpi/ocpi/blob/2.2.1/mod_tariffs.asciidoc#145-tariffdimensiontype-enum) for more details. The current non-exhaustive list of possible values are:

| Value | Description |
| --- | --- |
| `ENERGY` | Defined in kWh, step_size multiplier: 1 Wh |
| `FLAT` | Flat fee without unit for step_size |
| `PARKING_TIME` | Time not charging: defined in hours, step_size multiplier: 1 second |
| `TIME` | Time charging: defined in hours, step_size multiplier: 1 second |

### OCPI `TariffRestrictions`

These restrictions are not for the entire Charging Session. They only describe if a `TariffElement` becomes active or inactive during a Charging Session. Refer to this type in the [OCPI GitHub repository](https://github.com/ocpi/ocpi/blob/2.2.1/mod_tariffs.asciidoc#146-tariffrestrictions-class) for more details.

| Property | Type | Cardinality | Description |
| --- | --- | --- | --- |
| `start_time` | `string` | ? | Start time of day in local time, the time zone is defined in the time_zone field of the Location, for example 13:30, valid from this time of the day. Must be in 24h format with leading zeros. Hour/Minute separator: ":" Regex: ([0-1][0-9] |
| `end_time` | `string` | ? | End time of day in local time, the time zone is defined in the time_zone field of the Location, for example 19:45, valid until this time of the day. Same syntax as start_time. If end_time < start_time then the period wraps around to the next day. To stop at end of the day use: 00:00. |
| `start_date` | `string` | ? | Start date in local time, the time zone is defined in the time_zone field of the Location, for example: 2015-12-24, valid from this day (inclusive). Regex: ([12][0-9]3)-(0[1-9] |
| `end_date` | `string` | ? | End date in local time, the time zone is defined in the time_zone field of the Location, for example: 2015-12-27, valid until this day (exclusive). Same syntax as start_date. |
| `min_kwh` | `float` | ? | Minimum consumed energy in kWh, for example 20, valid from this amount of energy (inclusive) being used. |
| `max_kwh` | `float` | ? | Maximum consumed energy in kWh, for example 50, valid until this amount of energy (exclusive) being used. |
| `min_current` | `float` | ? | Sum of the minimum current (in Amperes) over all phases, for example 5. When the EV is charging with more than, or equal to, the defined amount of current, this `TariffElement` is/becomes active. If the charging current is or becomes lower, this `TariffElement` is not or no longer valid and becomes inactive. This describes NOT the minimum current over the entire Charging Session. This restriction can make a `TariffElement` become active when the charging current is above the defined value, but the `TariffElement` MUST no longer be active when the charging current drops below the defined value. |
| `max_current` | `float` | ? | Sum of the maximum current (in Amperes) over all phases, for example 20. When the EV is charging with less than the defined amount of current, this `TariffElement` becomes/is active. If the charging current is or becomes higher, this `TariffElement` is not or no longer valid and becomes inactive. This describes NOT the maximum current over the entire Charging Session. This restriction can make a `TariffElement` become active when the charging current is below this value, but the `TariffElement` MUST no longer be active when the charging current raises above the defined value. |
| `min_power` | `float` | ? | Minimum power in kW, for example 5. When the EV is charging with more than, or equal to, the defined amount of power, this `TariffElement` is/becomes active. If the charging power is or becomes lower, this `TariffElement` is not or no longer valid and becomes inactive. This describes NOT the minimum power over the entire Charging Session. This restriction can make a `TariffElement` become active when the charging power is above this value, but the `TariffElement` MUST no longer be active when the charging power drops below the defined value. |
| `max_power` | `float` | ? | Maximum power in kW, for example 20. When the EV is charging with less than the defined amount of power, this `TariffElement` becomes/is active. If the charging power is or becomes higher, this `TariffElement` is not or no longer valid and becomes inactive. This describes NOT the maximum power over the entire Charging Session. This restriction can make a `TariffElement` become active when the charging power is below this value, but the `TariffElement` MUST no longer be active when the charging power raises above the defined value. |
| `min_duration` | `integer` | ? | Minimum duration in seconds the Charging Session MUST last (inclusive). When the duration of a Charging Session is longer than the defined value, this `TariffElement` is or becomes active. Before that moment, this `TariffElement` is not yet active. |
| `max_duration` | `integer` | ? | Maximum duration in seconds the Charging Session MUST last (exclusive). When the duration of a Charging Session is shorter than the defined value, this `TariffElement` is or becomes active. After that moment, this `TariffElement` is no longer active. |
| `day_of_week` | `array` | * | Array of [`DayOfWeek`](#ocpi-dayofweek) representing which day(s) of the week this tariff element is active. |
| `reservation` | `string` | ? | [`ReservationRestrictionType`](#ocpi-reservationrestrictiontype). When this field is present, the [`TariffElement`](#ocpi-tariffelement) describes reservation costs. A reservation starts when the reservation is made, and ends when the driver starts charging on the reserved EVSE/Location, or when the reservation expires. A reservation can only have: FLAT and TIME `TariffDimensions`, where TIME is for the duration of the reservation. |

### OCPI `DayOfWeek`

Refer to this type in the [OCPI GitHub repository](https://github.com/ocpi/ocpi/blob/2.2.1/mod_tariffs.asciidoc#mod_tariffs_dayofweek_enum) for more details.

| Value | Description |
| --- | --- |
| `MONDAY` | Monday |
| `TUESDAY` | Tuesday |
| `WEDNESDAY` | Wednesday |
| `THURSDAY` | Thursday |
| `FRIDAY` | Friday |
| `SATURDAY` | Saturday |
| `SUNDAY` | Sunday |

### OCPI `ReservationRestrictionType`

Refer to this type in the [OCPI GitHub repository](https://github.com/ocpi/ocpi/blob/2.2.1/mod_tariffs.asciidoc#mod_tariffs_reservation_restriction_type) for more details. The current non-exhaustive list of possible values are:

| Value | Description |
| --- | --- |
| `RESERVATION` | Used in `TariffElement`s to describe costs for a reservation. |
| `RESERVATION_EXPIRES` | Used in `TariffElement`s to describe costs for a reservation that expires (as in when driver does not start a charging session before expiry_date of the reservation). |

## EV Charge Finder API restrictions and limits

-   Maximum 300 requests per minute.
-   The `limit` parameter up to a maximum of 100.
-   The `distance` parameter up to a maximum of 10 kilometers.

If you require a higher rate limit, [contact us](https://www.mapbox.com/contact/sales).