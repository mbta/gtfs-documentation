# MBTA GTFS Documentation

#### About this document

This document describes the MBTA's implementation of the General Transit Feed Specification (GTFS). It is intended to supplement, not replace, [documentation on GTFS itself](https://github.com/google/transit/tree/master/gtfs).

The MBTA GTFS feed can be downloaded [here](https://cdn.mbta.com/MBTA_GTFS.zip).

#### About the fields in this document

* **Status** field
	* **Included** 
	* **Included (some records)** 
	* **Included (empty)**: Included, but not populated
	* **N/A**: Part of GTFS specifications, but not included in MBTA GTFS
	* **Coming soon**: Will be included in upcoming GTFS version
	* **Deprecated**: No longer supported
	* **Persistent**: Value for record will not change between one **feed_version** and the next

* **GTFS spec** field
	* **Required** 
	* **Optional**
	* **Experimental**: Not part of GTFS specifications, but included in MBTA GTFS

## GTFS Files

Table Name | GTFS spec | Status | Notes
---------- | ---------| ------- | ---------
[agency.txt](#agencytxt) | Required | Included |
[calendar.txt](#calendartxt) | Required | Included | Generated programatically. May not be easy for humans to read.
[calendar_dates.txt](#calendar_datestxt) | Optional | Included | Generated programatically. May not be easy for humans to read.
[checkpoints.txt](#checkpointstxt) | Experimental | Included | Similar in part to [stops.txt](#stopstxt), this table provides human-readable names to checkpoints from [stop_times.txt](#stop_timestxt).
[facilities.txt](#facilitiestxt) | Experimental | Included | Station amenities such as elevators, escalators, parking lots, and bike storage.
[facilities_properties.txt](#facilities_propertiestxt) | Experimental | Included | Properties of station amenities in [facilities.txt](#facilitiestxt).
[facilities_properties_definitions.txt](#facilities_properties_definitionstxt) | Experimental | Included | Definitions of the property_ids and values used in [facilities_properties.txt](#facilities_propertiestxt).
[fare_attributes.txt](#fare_attributestxt) | Optional | N/A |
[fare_rules.txt](#fare_rulestxt) | Optional | N/A |
[feed_info.txt](#feed_infotxt) | Optional | Included |
[frequencies.txt](#frequenciestxt) | Optional | Included | Only used for Massport airport shuttles. If these move to a different feed, then [frequencies.txt](#frequenciestxt) will be removed.
[levels.txt](#levelstxt) | Experimental | Included | Provides relative elevation information for stop_ids (including boarding platforms and station entrances) within a parent station.
[multi_route_trips.txt](#multi_route_tripstxt) | Experimental | Included | For trips that travel on more than one route, this file identifies additional routes with which the trip should be associated.
[pathways.txt](#pathwaystxt) | Experimental | Included | Information and travel times about paths within and out of parent stations, including paths between platforms and to/from station entrances.
[routes.txt](#routestxt) | Required | Included |
[shapes.txt](#shapestxt) | Optional | Included |
[stops.txt](#stopstxt) | Required | Included |
[stop_times.txt](#stop_timestxt) | Required | Included |
[transfers.txt](#transferstxt) | Optional | Included |
[trips.txt](#tripstxt) | Required | Included |

## agency.txt

Agencies in the MBTA GTFS feed include the MBTA itself, [CapeFlyer](http://capeflyer.com/), and [Massport](http://www.massport.com/logan-airport/to-from-logan/transportation-options/on-airport-shuttle/) (for airport shuttles). CapeFlyer and Massport may move to separate GTFS feeds in the future.

Field Name | GTFS spec| Status | Notes
---------- | -------- | ------ | -------
agency_id | Optional | Included | MBTA `agency_id` = `1`.
agency_name | Required | Included |
agency_url | Required | Included | 
agency_timezone | Required | Included | 
agency_lang | Optional | Included | 
agency_phone | Optional | Included | 
agency_fare_url | Optional | N/A | 
agency_email | Optional | N/A | 

## calendar.txt

Field Name | GTFS spec | Status | Notes
---------- | -------- | ------ | ------- 
service_id | Required | Included | 
monday | Required | Included | 
tuesday | Required | Included | 
wednesday | Required | Included | 
thursday | Required | Included | 
friday | Required | Included | 
saturday | Required | Included | 
sunday | Required | Included | 
start_date | Required | Included | 
end_date | Required | Included |

## calendar_dates.txt

Field Name | GTFS spec | Status | Notes
---------- | -------- | ------ | --------
service_id | Required | Included |  
date | Required | Included | 
exception_type | Required | Included | 

## checkpoints.txt

This table provides human-readable names to checkpoints from [stop_times.txt](#stop_timestxt). The tables are joined on the experimental `checkpoint_id` field.

Field Name | GTFS spec | Status | Notes
---------- | -------- | ------ | --------
checkpoint_id | Experimental | Included | Values in this field must be unique.
checkpoint_name | Experimental | Included | A general location can have multiple checkpoints; values in this field are not necessarily unique.

## facilities.txt

Field Name | GTFS spec | Status | Notes
---------- | -------- | ------ | --------
facility_id | Experimental | Included | Uniquely identifies each facility. Must be unique within this table.
facility_id | Experimental | Included (some records) | Number or code to identify facility to customers. If the facility is labeled with a number in practice, that is used for a code.
facility_class | Experimental | Included | Specifies overarching category of a facility. Current valid values are:<ul><li>`0`: Unidentified or other</li><li>`1`: Pedestrian access (Examples include: walkway, elevator, stairway)</li><li>`2`: Wheelchair access (Exclusively to facilitate wheelchair movement, with examples such as: wheelchair lift, bridge plate, ramp)</li><li>`3`: Private mode access (Examples include: parking garage, parking space, bike storage, pick-up/drop-off, taxi stand)</li><li>`4`: Fare collection (Examples include: fare vending machine, turnstile array, turnstile, ticket window, fare collection store, retail sales location)</li><li>`5`: Information and assistance (Examples include: customer service desk, customer assistance phone, emergency phone, schedule card rack, countdown sign)</li><li>`6`: Amenities (Examples include: wi-fi, payphone, bench, artwork, waiting area)</li></ul>
facility_type | Experimental | Included | Specifies the direct type of a facility, answering the question, “What is this facility?” Current valid values are:<ul><li>`elevator`</li><li>`escalator`</li><li>`ramp`</li><li>`elevated-subplatform` (also called a "mini-high")</li><li>`fully-elevated-platform`</li><li>`portable-boarding-lift`</li><li>`bridge-plate`</li><li>`parking-area`</li><li>`pickup-dropoff`</li><li>`taxi-stand`</li><li>`bike-storage`</li><li>`electric-car-chargers`</li><li>`fare-vending-machine`</li><li>`fare-vending-retailer`</li><li>`fare-media-assistant`</li><li>`fare-media-assistance-facility`</li><li>`ticket-window`</li><li>`other`</li></ul>
stop_id | Experimental | Included | Specifies one `stop_id` with which the facility is associated. Common field with [stops.txt](#stopstxt). If a facility is associated with more than one `stop_id` within a parent station, the `stop_id` of the parent station should fill this field. In this case, if the facility does not serve every child stop within the parent stop, each excluded child stop served must be specified as a separate entry in [facilities_properties.txt](#facilities_propertiestxt) with the property `excludes-stop`.
facility_short_name | Experimental | Included (some records) | Contains a short name of the facility. Note that this name might not include the station name or the type of facility. At least one of `facility_short_name` or `facility_long_name` must be specified, or potentially both if appropriate.
facility_long_name | Experimental | Included (some records) | Contains a descriptive name of the facility which can be used without additional context concerning the station or type of facility. At least one of `facility_short_name` or `facility_long_name` must be specified, or potentially both if appropriate.
facility_desc | Experimental | Included (some records) | 
facility_lat | Experimental | Included (some records) | 
facility_lon | Experimental | Included (some records) | 
wheelchair_facility | Experimental | Included | Identifies whether a facility is accessible to customers using a wheelchair. The field can have the following values:<ul><li>`0` (or empty): indicates that there is no accessibility information for the facility</li><li>`1`: indicates that the facility is wheelchair accessible</li><li>`2`: facility not accessible to persons in wheelchairs</li></ul>

## facilities_properties.txt

Field Name | GTFS spec | Status | Notes
---------- | -------- | ------ | --------
facility_id | Experimental | Included | Uniquely identifies each facility. Does not need to be unique within this table.
property_id | Experimental | Included | Property or function for which details are provided in the value field. This may describe a particular function not described by `facility_class` in [facilities.txt](#facilitiestxt), or another property describing a detail about the facility.
value | Experimental | Included | Value corresponding to a particular property for a `facility_id`.

## facilities_properties_definitions.txt

Field Name | GTFS spec | Status | Notes
---------- | -------- | ------ | --------
property_id | Experimental | Included | Property ID which is referenced from the property field in [facilities_properties.txt](#facilities_propertiestxt).
definition | Experimental | Included | Text describing the associated `property_id`, its intended usage.
possible_values | Experimental | Included | Text describing the allowed values and format of the `property_id`’s respective value field.

## feed_info.txt

Field Name | GTFS spec | Status | Notes
---------- | -------- | ------ | ------
feed_publisher_name | Required | Included | 
feed_publisher_url | Required | Included | 
feed_contact_url | Experimental | Included | 
feed_contact_email | Experimental | Included | 
feed_lang | Required | Included | 
feed_start_date | Optional | Included | When comparing two GTFS feeds, the newer one will always have a newer `feed_start_date`.
feed_end_date | Optional | Included |
feed_version | Optional | Included | 

## frequencies.txt

Used only for [Massport airport shuttles](http://www.massport.com/logan-airport/to-from-logan/transportation-options/on-airport-shuttle/). If these move to a different feed then [frequencies.txt](#frequenciestxt) will be removed.

Field Name | GTFS spec | Status | Notes
---------- | -------- | ------ | --------
trip_id | Required | Included (persistent) | 
start_time | Required | Included | 
end_time | Required | Included | 
headway_secs | Required | Included | 
exact_times | Optional | N/A | 

## levels.txt

Experimental file used to describe the vertical levels within a station. level_ids can be applied to stops in **stops.txt**. [Learn more about the levels extension to GTFS.](https://github.com/google/transit/pull/86)

Field Name | GTFS spec | Status | Notes
---------- | -------- | ------ | --------
level_id | Experimental | Included | The `level_id` field contains an ID that uniquely identifies the level. The `level_id` is dataset unique.
level_index | Experimental | Included | Relative position of a level. In general, a value of `0` indicates the ground (or street) level, position values indicate levels above ground, and negative values indicate levels below ground. For some stations on slopes, it may be necessary to have multiple levels to represent adjacent streets, if the streets are at different elevations.
level_name | Experimental | Included | 
level_elevation | Experimental (empty) | Included | 

## multi_route_trips.txt

Some transit trips serve more than one route. For example, Commuter Rail trips that travel partly on the Lowell Line and partly on the Haverhill Line, or bus trips labeled "Route 62/76" that should be shown on the respective schedules for both Route 62 and Route 76. This applies when a trip serves one route and then the other, but also when the trip serves a unique combination of stops from each route.

This experimental file indicates routes that a trip is associated with, in addition to the `route_id` identified with this trip in [trips.txt](#tripstxt). If showing a schedule of all service on a route, that display should include trips associated with the route in [multi_route_trips.txt](#multi_route_tripstxt). If showing general information about a trip in a different context, the `route_id` identified with the in [trips.txt](#tripstxt) is still the best route to label the trip with.

If a every trip of a `route_id` appears here, that route does not have any trips of "its own" and should probably not be shown to customers in lists of routes. Examples include Route 62/76, which to a customer is an indication of Route 62 service and Route 76 service, not its own route.

Field Name | GTFS spec | Status | Notes
---------- | ------- | ------- | --------
added_route_id | Experimental | Included | An additional `route_id` associated with the given `trip_id`. Can appear repeatedly if more than one trip is added to it. 
trip_id | Experimental | Included | The `trip_id` of the trip which has the additional route associated with it. Can appear repeatedly if it is being added to more than one route.

## pathways.txt

Experimental file used to describe the various pedestrian paths of travel within and around a station. [Learn more about the pathways extension to GTFS.](https://github.com/google/transit/pull/86)

At this time, the MBTA implementation pathways does not include all stations. For those stations for which we show pathways, we show a single `pathway_id` between a pair of platforms, or between a platform and station entrance, with the `traversal_time` and `wheelchair_traversal_time` incorporating time estimates of distance traveled, as well as escalator and elevator travel times.

Field Name | GTFS spec | Status | Notes
---------- | ------- | ------- | --------
pathway_id | Experimental | Included | The `pathway_id` field contains an ID that uniquely identifies the pathway. The `pathway_id` is dataset unique.
from_stop_id | Experimental | Included | Stop IDs are referenced from the [stops.txt](#stopstxt) file.
to_stop_id | Experimental | Included | Stop IDs are referenced from the [stops.txt](#stopstxt) file.
facility_id | Experimental | Included (empty) | This additional field, while not in the official proposal, will be used by the MBTA in the near future to link pathways to facilities (in [facilities.txt](#facilitiestxt)) which may be required to be transversed, such as elevators.
pathway_mode | Experimental | Included | The value `0` is currently used for all pathways in the MBTA GTFS file
pathway_type | Experimental | Included | The `pathway_type` field specifies the type of connection. Valid values for this field are:<ul><li>`1`: Connects station locations to an entrance</li><li>`2`: Connects stops/stations for transfers off the streets or within stations</li><li>`3`: Connects stops/stations/entrances for transfers on the streets</li></ul>
traversal_time | Experimental | Included | 
wheelchair_traversal_time | Experimental | Included | 
ramp_slope | Experimental | Included (empty) | 
stair_count | Experimental | Included (empty) | 
pathway_name | Experimental | Included | Contains description of the path origin and destination.
pathway_code | Experimental (empty) | Included | 
signposted_as | Experimental | Included | Contains indication of the path destination as it is signed at the particular station.
instructions | Experimental (empty) | Included | 

## routes.txt

Field Name | GTFS spec | Status |  Notes
---------- | ------- | ------- | --------
route_id | Required | Included (persistent) |
agency_id | Optional | Included (persistent) | 
route_short_name | Required | Included (some records) | Populated for service with branches (such as Green Line or Silver Line), all MBTA bus services, and Massport.<br><br>If `route_short_name` is populated for a route, regardless of the presence of `route_long_name`, then "Route `route_short_name`" is an appropriate way to describe the route.
route_long_name | Required | Included (some records) | Populated for all modes. For rapid transit services with branches (Green Line), both `route_long_name` and `route_short_name` are populated: `route_short_name` containts the branch designation only, and `route_long_name` identifies both the route and the branch.<br><br>For rail-based and ferry services, `route_long_name` identifies a lengthier, commonly-used route identifier, such as "Red Line" or "Providence/Stoughton Line".<br>For bus services only, `route_long_name` provides the typical end points for the route, and should **not** be used as a standalone route identifier.
route_desc | Optional | Included (some records) | Categorizes type of service. For example, identifies whether bus service is express bus or local bus, identifies if service is rapid transit. Note that "Rapid Transit" `route_desc` values are associated with multiple `route_type` values.<br><br>**Possible Values:**<ul><li>`Airport Shuttle`</li><li>`Commuter Rail`</li><li>`Rapid Transit`</li><li>`Local Bus`</li><li>`Key Bus Route (Frequent Service)`</li><li>`Limited Service`</li><li>`Express Bus`</li><li>`Ferry`</li><li>`Rail Replacement Bus`</li></ul>
route_type | Required | Included | Indicates the type of vehicle that operates the route. It is not recommended to use this field's values to categorize MBTA service. 
route_url | Optional | Included (some records) | 
route_color | Optional | Included (some records) | 
route_text_color | Optional | Included (some records) | 
route_sort_order | Experimental | Included | Integer value that can be used for ordering routes in a way that is ideal for presentation to customers.

## shapes.txt

Field Name | GTFS spec | Status |  Notes
---------- | -------- | ------ | -------
shape_id | Required | Included | 
shape_pt_lat | Required | Included | 
shape_pt_lon | Required | Included | 
shape_pt_sequence | Required | Included | 
shape_dist_traveled | Optional | Included (empty) | 

## stops.txt

Field Name | GTFS spec | Status | Notes
---------- | -------- | ------ | --------
stop_id | Required | Included (persistent) | Expect field length to not exceed 45 characters.
stop_code | Optional | Included (some records) (persistent) | Field is populated only for subway and bus stops, where it has the same value as `stop_id`. Not prominently displayed or widely used, but visible on some bus stop signs.
stop_name | Required | Included | For most bus stops, indicates geographic location of stop, with first street listed as the street the bus is traveling on, and second street listed (after "@" or "opp") as the cross street. For bus and subway stop with a `parent_station` value, gives the name of the parent station. If more specific information about the stop is desired, place see `platform_name` (to use in conjunction with stop_name) or `stop_desc`.<br><br>Bus stops do not contain "stop" with a few exceptions (namely "Terminal B stop 1" and "Terminal B stop 2"). Subway and commuter rail stations do not contain "station" with a few exceptions (namely "North Station" and "South Station").<br><br>For example, the platform toward Ashmont and Braintree on the Red Line at Park Street will have a `stop_name` of `Park Street`.
stop_desc | Optional | Included (some records) | For stops with a `parent_station` value, indicates the name of the station and that of the specific boarding location.<br><br>For example, the platform toward Ashmont and Braintree on the Red Line at Park Street will have a `stop_desc` of `Park Street - Red Line - Ashmont/Braintree`.
platform_code | Experimental | Included (some records) | Indicates the platform identifier for a platform stop (e.g. `G` or `3`), if one is signed at a station. Many stations will not have `platform_code`s. Words like "platform" or "track" are not included.<br><br>For example, the platform toward Ashmont and Braintree on the Red Line at Park Street will have a `platform_code` of `5`.
platform_name | Experimental | Included (some records) | Indicates the platform name as labeled at a station, often indicating the direction or destination of services passing through a platform. Words like "platform" or "track" (or the feed's language-specific equivalent) should not be included.<br><br>For example, the platform toward Ashmont and Braintree on the Red Line at Park Street will have a `platform_name` of `Ashmont/Braintree`.
stop_lat | Required | Included | Records with a `parent_station` value currently have the same `stop_lat` value as the `parent_station`. However, the `parent_station` and child `stop_lat` values will differ in future versions of GTFS. 
stop_lon | Required | Included | Records with a `parent_station` value currently have the same `stop_lon` value as the `parent_station`. However, the `parent_station` and child `stop_lon` values will differ in future versions of GTFS.
zone_id | Optional | Included (empty) | 
stop_address | Experimental | Included (some records) | Optional field which allows a stop to have included a human-readable address.
stop_url | Optional | Included (empty) | 
level_id | Experimental | Included (some records) | Reference to vertical station level from [levels.txt](#levelstxt).
location_type | Optional | Included | A value of `2` designates a station entrance/exit, whereas `0` indicates a distinct boarding location, and `1` indicates a parent station complex.
parent_station | Optional | Included (some records) | For subway stations, the `parent_station`'s `stop_id` represents the whole facility and the child stop represents a specific platform. Use for identifying a station and indicating where specifically to enter a vehicle, respectively.<br><br>All subway stops have a parent station. If a commuter rail stop also serves the subway, then it will have a parent station. Future updates will add parent stations for all commuter rail stops.
stop_timezone | Optional | N/A | 
wheelchair_boarding | Optional | Included | Additional guidance for bus stops only:<ul><li>`0`: Minor to moderate accessibility barriers exist at the stop. Bus operator may need to relocate bus for safe boarding and exiting.</li><li>`2`: Significant accessibility barriers exist at the stop. Customers using wheeled mobility devices may need to board at street level.</li></ul>

## stop_times.txt

Field Name | GTFS spec | Status | Notes
---------- | -------- | ------ | --------
trip_id | Required | Included |  
arrival_time | Required | Included | 
departure_time | Required | Included | 
stop_id | Required | Included (persistent) | 
stop_sequence | Required | Included | 
stop_headsign | Optional | Included (empty) | While not currently used, this field is expected to be used in the future and should be handled by feed users. 
pickup_type | Optional | Included | A `pickup_type` of 3 represents a commuter rail "flag" stop marked with "F" on the schedule. Customer must visible to the approaching train to board.  
drop_off_type | Optional | Included | A `drop_off_type` of 3 represents a commuter rail "flag" stop marked with "F" on the schedule. Customer must notify the conductor to exit. 
shape_dist_traveled | Optional | N/A | 
timepoint| Optional | Included | On Commuter Rail, stops that are not timepoints and are not the last stop are labeled "L" on printed schedules to indicate that the train may leave early. MBTA bus and subway operations do not have many scheduled hold points so very few of their stop times have a `timepoint` value of 1.  

## transfers.txt

Field Name | GTFS spec | Status | Notes
---------- | -------- | ------ | ----------
from_stop_id | Required | Included | 
to_stop_id | Required | Included | 
transfer_type | Required | Included | 
min_transfer_time | Optional | Included (some records) | As specified in the GTFS standard, this field is the sum of `min_walk_time` and `suggested_buffer_time`.
min_walk_time | Experimental | Included (some records) | Minimum time required to travel by foot from `from_stop_id` to `to_stop_id`.
min_wheelchair_time | Experimental | Included (some records) | Minimum time required to travel by wheelchair `from_stop_id` to `to_stop_id`. If the transfer is not wheelchair accessible, this field will be blank.
suggested_buffer_time | Experimental | Included (some records) | Recommended buffer time to allow to make a successful transfer between two services. This is also partly based on the significance of missing the transfer (due to service frequency).
accessible_transfer | Experimental | Included (some records) | `1` if a transfer is wheelchair accessible, `2` if it is not.

## trips.txt

Field Name | GTFS spec | Status | Notes
---------- | ------- | ------- | --------
route_id | Required | Included (persistent)| 
service_id | Required | Included | 
trip_id | Required | Included (persistent)|
trip_headsign | Optional | Included | The average `trip_headsign` length is 13 characters. Expect field length to not exceed 80 characters.<br><br>Formats:<ul><li>**Destination** (Example: `Park Street`)</li><li>**Destination via Street or Landmark** (Example: `Andrew via South Bay Center`)</li><li>**Destination via Street or Landmark & Second Street or Landmark** (Example: `Wood Island via Market Basket & Admiral's Hill`)</li><li>**Destination via Street or Landmark and Second Street or Landmark** (Example: `Downtown Crossing via Airport and Paradise Road`)</li><li>**Destination (Express)** (Example: `Haymarket (Express)`)</li><li>**Destination (Limited Stops)** (Example: `Boston Medical Center (Limited Stops)`)</li><li>**Destination via Street or Landmark (Express)** (Example: `Woburn via Medford Square (Express)`)</li><li>**Destination via Street or Landmark & Second Street or Landmark (Express)** (Example: `Marblehead via Central Sq & Humphrey St (Express)`)</li></ul>
trip_short_name | Optional | Included (some records) (persistent) | `trip_short_name` is only a persistent identifier for commuter rail and CapeFlyer, field is blank for other modes. Even though it is not explicitly an identification field, it can be used to keep track of trips spanning across regular schedule changes. Do not assume that every commuter rail trip will have a `trip_short_name`.
direction_id | Optional | Included (persistent) | Refer to `trip_headsign` value to determine which destination indicates inbound or outbound direction for route.
block_id | Optional | Included (some records) | `block_id` here does **not** imply that a customer can remain on a vehicle through to the next trip, although there are some routes where this is permitted. These routes will be identified in a future GTFS change.<br><br>MBTA routes which allow ride-through to next trip: `26`, `43`, `741`, `742`, `749`, `67`, `76`
shape_id | Optional | Included | Generally included, but not guaranteed.
wheelchair_accessible | Optional | Included | For MBTA vehicles that are wheelchair accessible, the value is always `1`.
bikes_allowed | Optional | N/A | 
trip_route_type | Experimental | Included (some records) | Indicates the type of vehicle that operates the particular trip if the type of vehicle is not the same as specified in the route's `route_type` (i.e. if part of Blue Line subway service is replaced with a shuttle bus for one weekend). Definitions match those of `route_type`. For most trips, this is empty.
