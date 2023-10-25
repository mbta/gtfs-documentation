# MBTA GTFS Realtime Documentation

### About this document

This document describes the MBTA's implementation of the Realtime extensions
to the General Transit Feed Specification (GTFS). It is intended to supplement,
not replace, [the official GTFS Realtime documentation][rt-docs].

The MBTA provides GTFS Realtime feeds in several formats:

- [Standard](#standard-feeds)
- [JSON](#json-feeds)
- [Enhanced JSON](#enhanced-json-feeds)
- [Standard](#standard-feeds)
- [JSON](#json-feeds)
- [Enhanced JSON](#enhanced-json-feeds)

## Standard feeds

These URLs provide standards-compliant GTFS Realtime feeds in Protobuf format.
The structure is as described in the [official documentation][rt-docs], so that
information is not reproduced here.

- **Service Alerts:** https://cdn.mbta.com/realtime/Alerts.pb
- **Trip Updates:** https://cdn.mbta.com/realtime/TripUpdates.pb
- **Vehicle Positions:** https://cdn.mbta.com/realtime/VehiclePositions.pb

### Experimental fields

The following experimental fields are provided:

| Message                  | Field                  | Notes                                                                          |
| ------------------------ | ---------------------- | ------------------------------------------------------------------------------ |
| [`Alert`][ma]            | `severity_level`       |
| [`EntitySelector`][mes]  | `direction_id`         |
| [`TripDescriptor`][mtd]  | `direction_id`         |
| [`VehiclePosition`][mvp] | `occupancy_percentage` |
| [`VehiclePosition`][mvp] | `occupancy_status`     | The values used are `MANY_SEATS_AVAILABLE`, `FEW_SEATS_AVAILABLE`, and `FULL`. |

Occupancy data is only available for certain routes and vehicles. For details,
see the MBTA's [Crowding Information][crowding] page.

### Uncertainly values

Our [`StopTimeEvent`][mste] messages _may_ have an `uncertainty` value associated
with them. When present their meanings are as follows.

For Bus trips:
`uncertainty` | Meaning
------------- | ------
\< 300 | Valid real-time prediction
300 | Real-time prediction not available. This code is primarily used when a vehicle has not yet been assigned to the trip, (i.e. because the block has not started yet). It is a schedule-based prediction, but we adjust the schedule-based prediction time using observed historical travel times to make predictions more accurate than the schedule.
301 | Valid real-time prediction, though the bus appears to be stalled or significantly delayed and predictions are not as accurate
\> 301 | Likely invalid prediction, recommend not showing anything (and not showing scheduled time), very rare situation.

For Rail trips:
`uncertainty` | Meaning
------------- | ------
60 | The trip has already started
120 | A terminal/reverse trip departure for a trip that has NOT started and a train is awaiting departure at the origin
360 | A terminal/reverse trip for a trip that has NOT started and a train is completing a previous trip

## JSON feeds

These URLs provide [JSON](https://www.json.org/) documents that contain the same
information as the Standard feeds.

- **Service Alerts:** https://cdn.mbta.com/realtime/Alerts.json
- **Trip Updates:** https://cdn.mbta.com/realtime/TripUpdates.json
- **Vehicle Positions:** https://cdn.mbta.com/realtime/VehiclePositions.json

### JSON format

The Protobuf feed is expressed as JSON using the following mappings:

| Protobuf                                     | JSON                                              |
| -------------------------------------------- | ------------------------------------------------- |
| message                                      | object                                            |
| string                                       | string                                            |
| int32, int64, uint32, uint64, float, double  | number                                            |
| enum                                         | string (the name of the value, e.g. `NO_SERVICE`) |
| field with cardinality "Many"                | array of the specified type                       |
| "Optional" or "Conditionally required" field | key omitted from the object if not provided       |

## Enhanced JSON feeds

These URLs use the same format as the JSON feeds (above), but include additional
fields that are not part of the GTFS Realtime specification.

- **Service Alerts:** https://cdn.mbta.com/realtime/Alerts_enhanced.json
- **Trip Updates:** https://cdn.mbta.com/realtime/TripUpdates_enhanced.json
- **Vehicle Positions:** https://cdn.mbta.com/realtime/VehiclePositions_enhanced.json

### Enhanced fields

| Message                    | Field              | Type                        | Cardinality | Required | Description                                                                                                                                                                           |
| -------------------------- | ------------------ | --------------------------- | ----------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [`EntitySelector`][mes]    | `activities`       | [Activity](#enum-activity)  | Many        | Required | Describes the activities impacted by an alert with regard to the selected entity.                                                                                                     |
| [`StopTimeUpdate`][mstu]   | `boarding_status`  | string                      | One         | Optional | Describes the boarding status of the stop time as a short English-language string, for example _"On time"_, _"Now boarding"_, or _"Departed"_. Only provided for Commuter Rail trips. |
| [`TripDescriptor`][mtd]    | `route_pattern_id` | string                      | One         | Optional | Indicates the route pattern the described trip belongs to (from [route_patterns.txt](gtfs.md#route_patternstxt) in GTFS).                                                             |
| [`VehicleDescriptor`][mvd] | `consist`          | [Consist](#message-consist) | Many        | Optional | Information about the individual rail cars that make up the vehicle. Only provided for subway and light rail vehicles.                                                                |

The enhanced feeds may include fields other than those listed here. Such fields
should be treated as **experimental**, subject to change or removal at any time
and without advance notice.

### enum `Activity`

An activity impacted by an alert with regard to a selected entity.

Note the meaning of e.g. `BOARD` is not necessarily that the act of boarding
_itself_ is impacted, but that if a rider's trip involved boarding a vehicle at
the selected entity, their overall trip would be impacted.

| Value              | Description                                        |
| ------------------ | -------------------------------------------------- |
| `BOARD`            | Boarding a vehicle.                                |
| `EXIT`             | Exiting a vehicle (disembarking).                  |
| `RIDE`             | Riding through a stop without boarding or exiting. |
| `USING_ESCALATOR`  | Using an escalator while boarding or exiting.      |
| `USING_WHEELCHAIR` | Using a wheelchair while boarding or exiting.      |
| `BRINGING_BIKE`    | Bringing a bicycle while boarding or exiting.      |
| `STORE_BIKE`       | Storing a bicycle at a station.                    |
| `PARK_CAR`         | Parking a car at a garage or lot in a station.     |

### message `Consist`

An individual rail car within a vehicle that consists of multiple cars.

| Field   | Type   | Cardinality | Required | Description                         |
| ------- | ------ | ----------- | -------- | ----------------------------------- |
| `label` | string | One         | Required | The rider-visible label of the car. |

## Occupancy data 

Real-time crowding information is available for bus, Orange Line, and some Red Line cars. For details, see the [MBTA’s Crowding Information page][crowding].

### Subway 

Orange Line crowding information is available on a per-car basis. Note: new Red Line train cars (19xx series as per the VehicleDescriptor label) also support car-level crowding information.

Crowding estimates for each car are calculated based on live weight information reported by the Orange Line cars every several seconds. Due to the frequency of the weight updates reported by each car, several seconds of data lag is to be expected.

Crowding data is provided within the `multi_carriage_details` element in the `VehiclePosition` message.

Each `multi_carriage_details` element contains a [`CarriageDetails`](https://gtfs.org/realtime/reference/#message-CarriageDetails) message :

| Field                | Type                     | Description                             |
| -------------------- | ------------------------ | --------------------------------------- |
| carriage_sequence    | int32                    | The order of a carriage within a train  |
| id                   | string                   | Not used by the MBTA; expect null.      |
| label                | string                   | Individual car ID                       |
| occupancy_percentage | int32                    | Individual car percentage occupancy     |
| occupancy_status     | [`OccupancyStatus`][mos] | Individual car occupancy status         |

### Non-Revenue trips

Non-revenue vehicle movements, such as traveling to the start point of a trip or movements within a yard,
may be exposed in the enchanced feed as non-revenue trips. In general, these trips should not be shown
to riders as they do not accept passengers. These trips can be identified by the lack of a `trip_id`
within the [`TripDescriptor`][mtd] message combined with all [`StopTimeUpdate`][mstu] messages having
a [`ScheduleRelationship`][msr] of skipped.

Non-revenue trips only appear in the enhanced feed.

At this time, we are continuing to work on identifying non-revenue light rail trips
(Green Line, Mattapan Trolley). In the meantime, these trips may appear as revenue trips in the feeds.

[rt-docs]: https://github.com/google/transit/tree/master/gtfs
[crowding]: https://www.mbta.com/projects/crowding-information-riders
[ma]: https://github.com/google/transit/blob/master/gtfs-realtime/spec/en/reference.md#message-alert
[mes]: https://github.com/google/transit/blob/master/gtfs-realtime/spec/en/reference.md#message-entityselector
[mstu]: https://github.com/google/transit/blob/master/gtfs-realtime/spec/en/reference.md#message-stoptimeupdate
[mtd]: https://github.com/google/transit/blob/master/gtfs-realtime/spec/en/reference.md#message-tripdescriptor
[mvd]: https://github.com/google/transit/blob/master/gtfs-realtime/spec/en/reference.md#message-vehicledescriptor
[mvp]: https://github.com/google/transit/blob/master/gtfs-realtime/spec/en/reference.md#message-vehicleposition
[mos]: https://github.com/google/transit/blob/master/gtfs-realtime/spec/en/reference.md#enum-occupancystatus
[msr]: https://github.com/google/transit/blob/master/gtfs-realtime/spec/en/reference.md#enum-schedulerelationship
[mste]: https://github.com/google/transit/blob/master/gtfs-realtime/spec/en/reference.md#message-stoptimeevent
