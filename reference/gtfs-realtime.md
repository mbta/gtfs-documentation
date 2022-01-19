# MBTA GTFS Realtime Documentation

### About this document

This document describes the MBTA's implementation of the Realtime extensions
to the General Transit Feed Specification (GTFS). It is intended to supplement,
not replace, [the official GTFS Realtime documentation][rt-docs].

The MBTA provides GTFS Realtime feeds in several formats:

* [Standard](#standard-feeds)
* [JSON](#json-feeds)
* [Enhanced JSON](#enhanced-json-feeds)


## Standard feeds

These URLs provide standards-compliant GTFS Realtime feeds in Protobuf format.
The structure is as described in the [official documentation][rt-docs], so that
information is not reproduced here.

* **Service Alerts:** https://cdn.mbta.com/realtime/Alerts.pb
* **Trip Updates:** https://cdn.mbta.com/realtime/TripUpdates.pb
* **Vehicle Positions:** https://cdn.mbta.com/realtime/VehiclePositions.pb

### Experimental features

* [`Alert`][ma]: The experimental `severity_level` field is supported.
* [`EntitySelector`][mes]: The experimental `direction_id` field is supported.
* [`TripDescriptor`][mtd]: The experimental `direction_id` field is supported.
* [`VehiclePosition`][mvp]: The experimental `occupancy_status` field is
  supported. The values used are `MANY_SEATS_AVAILABLE`, `FEW_SEATS_AVAILABLE`,
  and `FULL`.


## JSON feeds

These URLs provide [JSON](https://www.json.org/) documents that contain the same
information as the Standard feeds.

* **Service Alerts:** https://cdn.mbta.com/realtime/Alerts.json
* **Trip Updates:** https://cdn.mbta.com/realtime/TripUpdates.json
* **Vehicle Positions:** https://cdn.mbta.com/realtime/VehiclePositions.json

### JSON format

The Protobuf feed is expressed as JSON using the following mappings:

Protobuf | JSON
-------- | ----
message | object
string | string
int32, int64, uint32, uint64, float, double | number
enum | string (the name of the value, e.g. `NO_SERVICE`)
field with cardinality "Many" | array of the specified type
"Optional" or "Conditionally required" field | key omitted from the object if not provided


## Enhanced JSON feeds

These URLs use the same format as the JSON feeds (above), but include additional
fields that are not part of the GTFS Realtime specification.

* **Service Alerts:** https://cdn.mbta.com/realtime/Alerts_enhanced.json
* **Trip Updates:** https://cdn.mbta.com/realtime/TripUpdates_enhanced.json
* **Vehicle Positions:** https://cdn.mbta.com/realtime/VehiclePositions_enhanced.json

### Enhanced fields

Message | Field | Type | Cardinality | Required | Description
------- | ----- | ---- | ----------- | -------- | -----------
[`EntitySelector`][mes] | `activities` | [Activity](#enum-activity) | Many | Required | Describes the activities impacted by an alert with regard to the selected entity.
[`StopTimeUpdate`][mstu] | `boarding_status` | string | One | Optional | Short English-language string that describes the boarding status of the stop time, for example _"On time"_, _"Now boarding"_, or _"Departed"_. Only provided for Commuter Rail trips.
[`VehicleDescriptor`][mvd] | `consist` | [Consist](#message-consist) | Many | Optional | Information about the individual rail cars that make up the vehicle. Only provided for subway and light rail vehicles.

The enhanced feeds may include fields other than those listed here. Such fields
should be treated as **experimental**, subject to change or removal at any time
and without advance notice.

### enum `Activity`

An activity impacted by an alert with regard to a selected entity.

Note the meaning of e.g. `BOARD` is not necessarily that the act of boarding
_itself_ is impacted, but that if a rider's trip involved boarding a vehicle at
the selected entity, their overall trip would be impacted.

Value | Description
----- | -----------
`BOARD` | Boarding a vehicle.
`EXIT` | Exiting a vehicle (disembarking).
`RIDE` | Riding through a stop without boarding or exiting.
`USING_ESCALATOR` | Using an escalator while boarding or exiting.
`USING_WHEELCHAIR` | Using a wheelchair while boarding or exiting.
`BRINGING_BIKE` | Bringing a bicycle while boarding or exiting.
`STORE_BIKE` | Storing a bicycle at a station.
`PARK_CAR` | Parking a car at a garage or lot in a station.

### message `Consist`

An individual rail car within a vehicle that consists of multiple cars.

Field | Type | Cardinality | Required | Description
----- | ---- | ----------- | -------- | -----------
`label` | string | One | Required | The rider-visible label of the car.


[rt-docs]: https://github.com/google/transit/tree/master/gtfs
[ma]: https://github.com/google/transit/blob/master/gtfs-realtime/spec/en/reference.md#message-alert
[mes]: https://github.com/google/transit/blob/master/gtfs-realtime/spec/en/reference.md#message-entityselector
[mstu]: https://github.com/google/transit/blob/master/gtfs-realtime/spec/en/reference.md#message-stoptimeupdate
[mtd]: https://github.com/google/transit/blob/master/gtfs-realtime/spec/en/reference.md#message-tripdescriptor
[mvd]: https://github.com/google/transit/blob/master/gtfs-realtime/spec/en/reference.md#message-vehicledescriptor
[mvp]: https://github.com/google/transit/blob/master/gtfs-realtime/spec/en/reference.md#message-vehicleposition
