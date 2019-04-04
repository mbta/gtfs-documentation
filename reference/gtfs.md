# MBTA GTFS Documentation

#### About this document

This document describes the MBTA's implementation of the General Transit Feed Specification (GTFS). It is intended to supplement, not replace, [documentation on GTFS itself](https://github.com/google/transit/tree/master/gtfs).

The MBTA GTFS feed is available from the following URL: [https://cdn.mbta.com/MBTA_GTFS.zip](https://cdn.mbta.com/MBTA_GTFS.zip).

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
[calendar_attributes.txt](#calendar_attributestxt) | Experimental | Included | Adds human-readable names and further context to calendar `service_id`s.
[calendar_dates.txt](#calendar_datestxt) | Optional | Included | Generated programatically. May not be easy for humans to read.
[checkpoints.txt](#checkpointstxt) | Experimental | Included | Similar in part to [stops.txt](#stopstxt), this table provides human-readable names to checkpoints from [stop_times.txt](#stop_timestxt).
[directions.txt](#directionstxt) | Experimental | Included | Provides for passenger-facing names to be documented for `direction_id` on a route-by-route basis.
[facilities.txt](#facilitiestxt) | Experimental | Included | Station amenities such as elevators, escalators, parking lots, and bike storage.
[facilities_properties.txt](#facilities_propertiestxt) | Experimental | Included | Properties of station amenities in [facilities.txt](#facilitiestxt).
[facilities_properties_definitions.txt](#facilities_properties_definitionstxt) | Experimental | Included | Definitions of the `property_id`s and `value`s used in [facilities_properties.txt](#facilities_propertiestxt).
[fare_attributes.txt](#fare_attributestxt) | Optional | N/A |
[fare_rules.txt](#fare_rulestxt) | Optional | N/A |
[feed_info.txt](#feed_infotxt) | Optional | Included |
[frequencies.txt](#frequenciestxt) | Optional | N/A | 
[levels.txt](#levelstxt) | Optional | Included | Provides relative elevation information for `stop_id`s (including boarding platforms, station entrances, and generic nodes) within a parent station.
[lines.txt](#linestxt) | Experimental | Included | Groups similar routes (such as those which serve the same trunk corridor or bus terminal) for the purpose of customer display.
[linked_datasets.txt](#linked_datasetstxt) | Experimental | Included | URLs to linked GTFS-realtime datasets: Trip Updates, Vehicle Positions and Service Alerts.
[multi_route_trips.txt](#multi_route_tripstxt) | Experimental | Included | For trips that travel on more than one route, this file identifies additional routes with which the trip should be associated.
[pathways.txt](#pathwaystxt) | Experimental | Included | Information and travel times about paths within and out of parent stations, including paths between platforms and to/from station entrances.
[routes.txt](#routestxt) | Required | Included |
[route_patterns.txt](#route_patternstxt) | Experimental | Included | Information about the different variations of service that may be run within a single `route_id`, including when and how often they are operated.
[shapes.txt](#shapestxt) | Optional | Included |
[stops.txt](#stopstxt) | Required | Included |
[stop_times.txt](#stop_timestxt) | Required | Included |
[transfers.txt](#transferstxt) | Optional | Included |
[trips.txt](#tripstxt) | Required | Included |

## agency.txt

Agencies in the MBTA GTFS feed include the MBTA itself and [Cape Cod RTA](http://capecodrta.org/) (for the CapeFLYER). The CapeFLYER may move to a separate GTFS feed in the future, in which case CCRTA would be removed from this file.

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

## calendar_attributes.txt

Adds human-readable names to calendar `service_id`s and further information about when they operate and how closely the service aligns to service on a typical day. This specificiation adds onto the [GTFS+ calendar_attributes.txt extension](https://www.transitwiki.org/TransitWiki/images/e/e7/GTFS+_Additional_Files_Format_Ver_1.7.pdf).

Field Name | GTFS spec | Status | Notes
---------- | -------- | ------ | --------
service_id | Experimental | Included | Linked to values in [calendar.txt](#calendartxt) and [calendar_dates.txt](#calendar_datestxt).
service_description | Experimental | Included | Human-readable description of the service, as it should appear on public-facing websites and applications. Some of the possible values include, but are not limited to, "Weekday schedule", "Weekday schedule (no school)", "Saturday schedule", "Saturday schedule (modified)", "Holiday (Sunday schedule)", and "Storm schedule (reduced service)".
service_schedule_name | Experimental | Included | Description of when the `service_id` is in effect. Typically will look like "Weekday", "Weekday (no school)", "Holiday", "Saturday", "Sunday", and "Storm (reduced schedule)".
service_schedule_type | Experimental | Included | Description of the schedule type the `service_id` can be applied. For example, on a holiday, the `service_schedule_type` value may be "Saturday" or "Sunday". Current valid values are:<ul><li>Weekday</li><li>Saturday</li><li>Sunday</li><li>Other</li></ul>
service_schedule_typicality | Experimental | Included | Describes how well this schedule represents typical service for the listed `service_schedule_type`. Current valid values are:<ul><li>`0` (or empty): Not defined.</li><li>`1`: Typical service with perhaps minor modifications</li><li>`2`: Extra service supplements typical schedules</li><li>`3`: Reduced holiday service is provided by typical Saturday or Sunday schedule</li><li>`4`: Major changes in service due to a planned disruption, such as construction</li><li>`5`: Major reductions in service for weather events or other atypical situations</li></ul>

## calendar_dates.txt

Field Name | GTFS spec | Status | Notes
---------- | -------- | ------ | --------
service_id | Required | Included |  
date | Required | Included | 
exception_type | Required | Included | 
holiday_name | Experimental | Included (some records) | Names a particular holiday for which the service exception is in effect (e.g. "Thanksgiving Day").

## checkpoints.txt

This table provides human-readable names to checkpoints from [stop_times.txt](#stop_timestxt). The tables are joined on the experimental `checkpoint_id` field.

Field Name | GTFS spec | Status | Notes
---------- | -------- | ------ | --------
checkpoint_id | Experimental | Included | Values in this field must be unique.
checkpoint_name | Experimental | Included | A general location can have multiple checkpoints; values in this field are not necessarily unique.

## directions.txt

Provides for passenger-facing names to be documented for each direction_id value on a route-by-route basis. This specificiation adds onto the [GTFS+ directions.txt extension](https://www.transitwiki.org/TransitWiki/images/e/e7/GTFS+_Additional_Files_Format_Ver_1.7.pdf).

Field Name | GTFS spec | Status | Notes
---------- | -------- | ------ | --------
route_id | Experimental | Included | Linked to [routes.txt](#routestxt) file.
direction_id | Experimental | Included | Binary value linked to [trips.txt](#tripstxt) file. Combining `route_id` and `direction_id` forms the unique primary key for this file.
direction | Experimental | Included | Human-readable text corresponding to a direction name in a given list. Current valid values are: <ul><li>North</li><li>South</li><li>East</li><li>West</li><li>Northeast</li><li>Northwest</li><li>Southeast</li><li>Southwest</li><li>Clockwise</li><li>Counterclockwise</li><li>Inbound</li><li>Outbound</li><li>Loop A</li><li>Loop B</li><li>Loop</li></ul>
direction_destination | Experimental | Included | MBTA extension which provides the customer-facing direction name used on in-station wayfinding. It can, but need not, include all possible destinations; it could instead list a cardinal direction or city/town. For example, `direction_id` `0` on `route_id` `Red` may have a direction value of `South`, and a direction_destination of `Ashmont/Braintree`.

## facilities.txt

Field Name | GTFS spec | Status | Notes
---------- | -------- | ------ | --------
facility_id | Experimental | Included | Uniquely identifies each facility. Must be unique within this table.
facility_code | Experimental | Included (some records) | Number or code to identify facility to customers. If the facility is labeled with a number in practice, that is used for a code.
facility_class | Experimental | Included | Specifies overarching category of a facility. Current valid values are:<ul><li>`0`: Unidentified or other</li><li>`1`: Pedestrian access (Examples include: walkway, elevator, stairway)</li><li>`2`: Wheelchair access (Exclusively to facilitate wheelchair movement, with examples such as: wheelchair lift, bridge plate, ramp)</li><li>`3`: Private mode access (Examples include: parking garage, parking space, bike storage, pick-up/drop-off, taxi stand)</li><li>`4`: Fare collection (Examples include: fare vending machine, turnstile array, turnstile, ticket window, fare collection store, retail sales location)</li><li>`5`: Information and assistance (Examples include: customer service desk, customer assistance phone, emergency phone, schedule card rack, countdown sign)</li><li>`6`: Amenities (Examples include: wi-fi, payphone, bench, artwork, waiting area)</li></ul>
facility_type | Experimental | Included | Specifies the direct type of a facility, answering the question, "What is this facility?" Current valid values are:<ul><li>`elevator`</li><li>`escalator`</li><li>`ramp`</li><li>`elevated-subplatform` (also called a "mini-high")</li><li>`fully-elevated-platform`</li><li>`portable-boarding-lift`</li><li>`bridge-plate`</li><li>`parking-area`</li><li>`pickup-dropoff`</li><li>`taxi-stand`</li><li>`bike-storage`</li><li>`electric-car-chargers`</li><li>`fare-vending-machine`</li><li>`fare-vending-retailer`</li><li>`fare-media-assistant`</li><li>`fare-media-assistance-facility`</li><li>`ticket-window`</li><li>`other`</li></ul>
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
possible_values | Experimental | Included | Text describing the allowed values and format of the `property_id`'s respective value field.

## feed_info.txt

Field Name | GTFS spec | Status | Notes
---------- | -------- | ------ | ------
feed_publisher_name | Required | Included | 
feed_publisher_url | Required | Included | 
feed_lang | Required | Included | 
feed_start_date | Optional | Included | When comparing two GTFS feeds, the newer one will always have a newer `feed_start_date`.
feed_end_date | Optional | Included |
feed_version | Optional | Included | 
feed_contact_email | Optional | Included | An email address for communication regarding the GTFS dataset and data publishing practices. It is a technical contact for GTFS-consuming applications. Provide customer service contact information through [agency.txt](agencytxt).

## levels.txt

Used to describe the vertical levels within a station. `level_id`s can be applied to stops in [stops.txt](#stopstxt). [Learn more about the recent introduction of levels to GTFS.](https://github.com/google/transit/pull/143)

Field Name | GTFS spec | Status | Notes
---------- | -------- | ------ | --------
level_id | Required | Included | The `level_id` field contains an ID that uniquely identifies the level. The `level_id` is dataset unique.
level_index | Required | Included | Relative position of a level. In general, a value of `0` indicates the ground (or street) level, position values indicate levels above ground, and negative values indicate levels below ground. For some stations on slopes, it may be necessary to have multiple levels to represent adjacent streets, if the streets are at different elevations.
level_name | Optional | Included |

## lines.txt

In this file, a line is a combination of existing routes in [routes.txt](#routestxt). This concept can be used to group similar routes when displaying them to customers, such as for routes which serve the same trunk corridor or bus terminal. Lines have similar attributes as routes, such as short and long names, descriptions, URLs, colors, and sort ordering.

Field Name | GTFS spec | Status | Notes
---------- | -------- | ------ | --------
line_id | Experimental | Included | Feed-unique identifier for lines, or groups of routes. This ID will be referenced in [routes.txt](#routestxt) for routes which are contained in this line.
line_short_name | Experimental | Included | Short, public-facing name for the group of routes represented in this line. Usage between this field and `line_long_name` is the same as the relationship between `route_short_name` and `route_long_name`.
line_long_name | Experimental | Included | Lengthier, public-facing name for the group of routes represented in this line. Usage between this field and `line_short_name` is the same as the relationship between `route_short_name` and `route_long_name`.
line_desc | Experimental (empty) | Included | Contains a human-readable description of the line.
line_url | Experimental | Included (empty) | Contains the URL of a web page about the particular line. This should be different from the agency_url or any route_url values. The value must be a fully qualified URL that includes `http://` or `https://`, and any special characters in the URL must be correctly escaped.
line_color | Experimental | Included | In systems that have colors assigned to lines, the route_color field defines a color that corresponds to a line. The color must be provided as a six-character hexadecimal number, for example, `00FFFF`. If no color is specified, the default route color is white (`FFFFFF`).
line_text_color | Experimental | Included | This field can be used to specify a legible color to use for text drawn against a background of line_color. The color must be provided as a six-character hexadecimal number, for example, `FFD700`. If no color is specified, the default text color is black (`000000`).
line_sort_order | Experimental | Included | The `line_sort_order` field can be used to order the lines in a way which is ideal for presentation to customers. It must be a non-negative integer. Lines with smaller `line_sort_order` values should be displayed before lines with larger `line_sort_order` values.

## linked_datasets.txt

Experimental file containing URLs and authentication information for linked GTFS-realtime datasets: Trip Updates, Vehicle Positions and Service Alerts. [Learn more about the proposed linked_datasets extension to GTFS.](https://github.com/google/transit/pull/93)

Field Name | GTFS spec | Status | Notes
---------- | -------- | ------ | --------
url | Experimental | Included | Contains the URL to the linked dataset.
trip_updates | Experimental | Included | Indicates whether the dataset at the specified URL may contain [a `TripUpdate` entity](https://github.com/google/transit/blob/master/gtfs-realtime/spec/en/reference.md#message-tripupdate). Valid values are `0` if the dataset does not contain TripUpdate entities, or `1` if the dataset may contain TripUpdate entities.
vehicle_positions | Experimental | Included | Indicates whether the dataset at the specified URL may contain [a `VehiclePosition` entity](https://github.com/google/transit/blob/master/gtfs-realtime/spec/en/reference.md#message-vehicleposition). Valid values are `0` if the dataset does not contain VehiclePosition entities, or `1` if the dataset may contain VehiclePosition entities.
service_alerts | Experimental | Included | Indicates whether the dataset at the specified URL may contain [an `Alert` entity](https://github.com/google/transit/blob/master/gtfs-realtime/spec/en/reference.md#message-alert). Valid values are `0` if the dataset does not contain Alert entities, or `1` if the dataset may contain Alert entities.
authentication_type | Experimental | Included | Defines the type of authentication required to access the URL. Presently, all included URLs have a value of `0` indicating that no authentication is required.

## multi_route_trips.txt

Some transit trips serve more than one route. For example, Commuter Rail trips that travel partly on the Lowell Line and partly on the Haverhill Line, or bus trips labeled "Route 62/76" that should be shown on the respective schedules for both Route 62 and Route 76. This applies when a trip serves one route and then the other, but also when the trip serves a unique combination of stops from each route.

This experimental file indicates routes that a trip is associated with, in addition to the `route_id` identified with this trip in [trips.txt](#tripstxt). If showing a schedule of all service on a route, that display should include trips associated with the route in [multi_route_trips.txt](#multi_route_tripstxt). If showing general information about a trip in a different context, the `route_id` identified with the in [trips.txt](#tripstxt) is still the best route to label the trip with.

If a every trip of a `route_id` appears here, that route does not have any trips of "its own" and should probably not be shown to customers in lists of routes. Examples include Route 62/76, which to a customer is an indication of Route 62 service and Route 76 service, not its own route.

Field Name | GTFS spec | Status | Notes
---------- | ------- | ------- | --------
added_route_id | Experimental | Included | An additional `route_id` associated with the given `trip_id`. Can appear repeatedly if more than one trip is added to it. 
trip_id | Experimental | Included | The `trip_id` of the trip which has the additional route associated with it. Can appear repeatedly if it is being added to more than one route.

## pathways.txt

Experimental file used to describe the various pedestrian paths of travel within and around a station. [Learn more about the recent introduction of pathways to GTFS.](https://github.com/google/transit/pull/143)

At this time, the MBTA implementation of pathways does not include all stations, with additional stations being added to the file continually.

Field Name | GTFS spec | Status | Notes
---------- | ------- | ------- | --------
pathway_id | Required | Included | The `pathway_id` field contains an ID that uniquely identifies the pathway. The `pathway_id` is dataset unique.
from_stop_id | Required | Included | Stop IDs are referenced from the [stops.txt](#stopstxt) file, and include service stops, station entrances, and generic nodes.
to_stop_id | Required | Included | Stop IDs are referenced from the [stops.txt](#stopstxt) file, and include service stops, station entrances, and generic nodes.
facility_id | Required | Included (empty) | Links pathways to facilities (in [facilities.txt](#facilitiestxt)) which may be required to be transversed, such as escalators and elevators. Can be coupled with real-time alerts feeds/API calls to determine if the pathway is available at a given time.
pathway_mode | Required | Included | Type of pathway. Values used by the MBTA for this field are:<br><ul><li>`1`: Walkways, including ramps</li><li>`2`: Staircases</li><li>`4`: Escalators</li><li>`5`: Elevators</li><li>`6`: Entering fare control (by passing through fare gates)</li><li>`7`: Exiting fare control (by passing through fare gates or exit-only gates)</li></ul>
is_bidirectional | Required | Included | For the MBTA implementation, this field will always be `0`. All pathways will be unidirectional.
length | Optional | Included (some records) | Length is provided in meters.
wheelchair_length | Experimental | Included (some records) | This field is temporarily added for backward compatibility alongside `wheelchair_traversal_time` and will be deprecated and removed in the future. Wheelchair length is provided in meters.
traversal_time | Optional | Included (some records) | Traversal time is provided in seconds.
wheelchair_traversal_time | Experimental | Included (some records) | This field is included for backward compatibility and will be deprecated and removed in the future. Wheelchair traversal time is provided in seconds.
stair_count | Optional | Included (some records) |
pathway_name | Experimental | Included | Contains description of the path origin and destination.
pathway_code | Experimental | Included (empty) |
signposted_as | Optional | Included | Contains indication of the path destination as it is signed at the particular station.
instructions | Experimental | Included (empty) |

## routes.txt

Field Name | GTFS spec | Status |  Notes
---------- | ------- | ------- | --------
route_id | Required | Included (persistent) |
agency_id | Optional | Included (persistent) | 
route_short_name | Required | Included (some records) | Populated for service with branches on separate route IDs (such as Green Line or Silver Line) and all MBTA bus services.<br><br>If `route_short_name` is populated for a route, regardless of the presence of `route_long_name`, then "Route `route_short_name`" is an appropriate way to describe the route.
route_long_name | Required | Included (some records) | Populated for all modes. For rapid transit services with branches (Green Line), both `route_long_name` and `route_short_name` are populated: `route_short_name` containts the branch designation only, and `route_long_name` identifies both the route and the branch.<br><br>For rail-based and ferry services, `route_long_name` identifies a lengthier, commonly-used route identifier, such as "Red Line" or "Providence/Stoughton Line".<br>For bus services only, `route_long_name` provides the typical end points for the route, and should **not** be used as a standalone route identifier.
route_desc | Optional | Included | Categorizes a route's level of service. For example, identifies whether bus service runs frequently all day, is aimed at weekday commuters, or supplements service on other routes.<br><br>**Possible Values:**<ul><li>`Commuter Rail`</li><li>`Rapid Transit`</li><li>`Local Bus`</li><li>`Key Bus`</li><li>`Supplemental Bus`</li><li>`Community Bus`</li><li>`Commuter Bus`</li><li>`Ferry`</li><li>`Rail Replacement Bus`</li></ul>
route_fare_class | Experimental | Included | Specifies the fare type of the route, which can differ from the service category. This proposal uses this field instead of using [fare_rules.txt](#fare_rulestxt) and [fare_attributes.txt](#fare_attributestxt) as those files currently do not support the entirety of the MBTA's fare and transfer policies For public-facing applications, we recommend that use and display of route_fare_class be equally or more prominent than `route_desc`, as passengers often identify routes by their fares..<br><br>**Possible values for the MBTA implementation:**<ul><li>`Local Bus`</li><li>`Inner Express`</li><li>`Outer Express`</li><li>`Rapid Transit`</li><li>`Commuter Rail`</li><li>`Ferry`</li><li>`Free`</li><li>`Special`</li></ul>
route_type | Required | Included | Indicates the type of vehicle that operates the route. It is not recommended to use this field's values to categorize MBTA service. 
route_url | Optional | Included (some records) | 
route_color | Optional | Included (some records) | 
route_text_color | Optional | Included (some records) | 
route_sort_order | Experimental | Included | Integer value that can be used for ordering routes in a way that is ideal for presentation to customers.
line_id | Experimental | Included (some records) | References `line_id` values from [lines.txt](#linestxt). Indicates in which grouping of routes this route belongs, if any. For example, `route_id` `62` may have a `line_id` value of `line-6276`. Note that groupings are subject to change without notice.
listed_route | Experimental | Included (some records) | Indicates whether route should be included in a public-facing list of all routes. This is useful for determining which routes which should not be shown by themselves, but rather part of another route. Most uses of this field should incorporate data from [multi_route_trips.txt](#multi_route_tripstxt). The following values are valid:<ul><li>`0` (or empty): Route should be included in a list of routes.</li><li>`1`: Route should not be included in a public-facing list of routes.</li></ul>For example, in the future, service for route "450W" trips may appear under a `route_id` of `450W`, but with a `listed_route` value of `1`, and all of the route's trips included in [multi_route_trips.txt](#multi_route_tripstxt) to be displayed together with `route_id` `450`.

## route_patterns.txt

Experimental file used to describe the subsets of a route, representing different possible patterns of where trips may serve. For example, a bus route may have multiple branches, and each branch may be modeled as a separate route pattern per direction. Hieratically, the route pattern level may be considered to be larger than the trip level and smaller than the route level.

For most MBTA modes, a route pattern will typically represent a unique set of stops that may be served on a route-trip combination. Seasonal schedule changes may result in trips within a route pattern having different routings. In simple changes, such a single bus stop removed or added between one schedule rating and the next (for example, between the Summer and Fall schedules), trips will be maintained on the same `route_pattern_id`. If the changes are significant, a new `route_pattern_id` may be introduced.

For Commuter Rail, express or skip-stop trips use the same route pattern as local trips. Some branches do have multiple route patterns when the train takes a different path. For example, `CR-Providence` has two route patterns per direction, one for the Wickford Junction branch and the other for the Stoughton branch.

Field Name | GTFS spec | Status |  Notes
---------- | -------- | ------ | -------
route_pattern_id | Experimental | Included | Database-unique identifier for the route pattern. For the MBTA, this will generally be a concatenation including the `route_id` and `direction_id`. Values from this field are referenced in [trips.txt](#tripstxt).
route_id | Experimental | Included | Route IDs for which route patterns are valid are referenced in [routes.txt](#routestxt).
direction_id | Experimental | Included | Direction IDs for which route patterns are valid are referenced in [trips.txt](#tripstxt).
route_pattern_name | Experimental | Included | User-facing description of where trips on the route pattern serve. These names are published in the form **Destination**, **Destination via Street or Landmark**, **Origin - Destination**, or **Origin - Destination via Street or Landmark**. Note that names for bus and subway route patterns currently do not include the origin location, but will in the future.
route_pattern_time_desc | Experimental | Included (some records) | User-facing description of when the route pattern operate. Not all route patterns will include a time description. Several examples of possible values include `Early mornings only`, `Weekday evenings only`, `Saturdays only`, and `School days only`.
route_pattern_typicality | Experimental | Included | Explains how common the route pattern is. For the MBTA, this is within the context of the entire route. Current valid values are:<ul><li>`0` (or empty): Not defined.</li><li>`1`: Typical. Pattern is common for the route. Most routes will have only one such pattern per direction. A few routes may have more than 1, such as the Red Line (with one branch to Ashmont and another to Braintree); routes with more than 2 are rare.</li><li>`2`: Pattern is a deviation from the regular route.</li><li>`3`: Pattern represents a highly atypical pattern for the route, such as a special routing which only runs a handful of times per day.</li><li>`4`: Diversions from normal service, such as planned detours, bus shuttles, or snow routes.</li></ul>
route_pattern_sort_order | Experimental | Included | The `route_pattern_sort_order` field can be used to order the route patterns in a way which is ideal for presentation to customers. Values are non-negative integers. Route patterns with smaller `route_pattern_sort_order` values should be displayed before those with larger `route_pattern_sort_order` values.
representative_trip_id | Experimental | Included | A trip ID that can be considered a canonical trip for the route pattern. Referring to this value in [trips.txt](#tripstxt) and [stoptimes.txt](#stoptimestxt), a pattern's canonical set of stops and shape ID and be deduced. Note that in this implementation, if a GTFS feed includes trips from multiple schedule ratings, which may include changes in the pattern's routing, the `representative_trip_id` will come from the schedule rating in effect at the time of the feed's release.

## shapes.txt

Field Name | GTFS spec | Status |  Notes
---------- | -------- | ------ | -------
shape_id | Required | Included | 
shape_pt_lat | Required | Included | 
shape_pt_lon | Required | Included | 
shape_pt_sequence | Required | Included | 
shape_dist_traveled | Optional | Included | Distances reported in meters.

## stops.txt

Field Name | GTFS spec | Status | Notes
---------- | -------- | ------ | --------
stop_id | Required | Included (persistent) | Expect field length to not exceed 45 characters.
stop_code | Optional | Included (some records) (persistent) | Field is populated only for subway and bus stops, where it has the same value as `stop_id`. Not prominently displayed or widely used, but visible on some bus stop signs.
stop_name | Required | Included | For most bus stops, indicates geographic location of stop, with first street listed as the street the bus is traveling on, and second street listed (after "@" or "opp") as the cross street. For bus and subway stop with a `parent_station` value, gives the name of the parent station. If more specific information about the stop is desired, place see `platform_name` (to use in conjunction with stop_name) or `stop_desc`.<br><br>Bus stops do not contain "stop" with a few exceptions (namely "Terminal B stop 1" and "Terminal B stop 2"). Subway and commuter rail stations do not contain "station" with a few exceptions (namely "North Station" and "South Station").<br><br>For example, the platform toward Ashmont and Braintree on the Red Line at Park Street will have a `stop_name` of `Park Street`.
stop_desc | Optional | Included (some records) | For stops with a `parent_station` value, indicates the name of the station and that of the specific boarding location.<br><br>For example, the platform toward Ashmont and Braintree on the Red Line at Park Street will have a `stop_desc` of `Park Street - Red Line - Ashmont/Braintree`.
platform_code | Experimental | Included (some records) | Indicates the platform identifier for a platform stop (e.g. `G` or `3`), if one is signed at a station. Many stations will not have `platform_code`s. Words like "platform" or "track" are not included.<br><br>For example, the platform toward Ashmont and Braintree on the Red Line at Park Street will have a `platform_code` of `5`.
platform_name | Experimental | Included (some records) | Indicates the platform name as labeled at a station, often indicating the direction or destination of services passing through a platform. Words like "platform" or "track" (or the feed's language-specific equivalent) should not be included.<br><br>For example, the platform toward Ashmont and Braintree on the Red Line at Park Street will have a `platform_name` of `Ashmont/Braintree`.
stop_lat | Required | Included (some records) | Children stops with a `parent_station` may have different `stop_lat` value as that of the parent. Generic nodes (having a `location_type` of `3`) may not have a `stop_lat`.
stop_lon | Required | Included (some records) | Children stops with a `parent_station` may have different `stop_lon` value as that of the parent. Generic nodes (having a `location_type` of `3`) may not have a `stop_lon`.
zone_id | Optional | Included (some records) | Populated with Commuter Rail fare zone information at Commuter Rail stations and stops. Special values may be applied at stations at which multiple fare zones or where special event fares are in effect.
stop_address | Experimental | Included (some records) | Optional field which allows a stop to have included a human-readable address.
stop_url | Optional | Included (some records) | Populated for stops which are parent stations (`location_type` of `1`) or scheduled boarding locations (`location_type` of `0`).
level_id | Experimental | Included (some records) | Reference to vertical station level from [levels.txt](#levelstxt).
location_type | Optional | Included | A value of `1` indicates a parent station complex, whereas `2` designates a station entrance/exit, `0` indicates a distinct boarding location, and `3` indicates a generic node within a station, such as the end of a staircase, elevator, or escalator.
parent_station | Optional | Included (some records) | For subway stations, the `parent_station`'s `stop_id` represents the whole facility and the child stop represents a specific platform, entrance, or a generic node (such as the end of a staircase or elevator). Use for identifying a station and indicating where specifically to access a vehicle or station.<br><br>All subway stops have a parent station. If a commuter rail stop also serves the subway, then it will have a parent station. Future updates will add parent stations for all commuter rail stops.
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
checkpoint_id | Experimental | Included (some records) | Identifier for locations at which on-time performance is measured. This includes all MBTA subway stops and major bus stops. Primarily for internal use. Refer also to [checkpoints.txt](#checkpointstxt). A `checkpoint_id` corresponds to a general geographic location (such as a public square or rail station) and can be shared by several routes at different `stop_id`s in close proximity.

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
wheelchair_transfer | Experimental | Included (some records) | `1` if a transfer is wheelchair accessible, `2` if it is not.

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
trip_route_type | Experimental | Included (some records) | Indicates the type of vehicle that operates the particular trip if the type of vehicle is not the same as specified in the route's `route_type` (i.e. if part of Blue Line subway service is replaced with a shuttle bus for one weekend). Definitions match those of `route_type`. For most trips, this is empty.
route_pattern_id | Experimental | Included | Indicates the pattern of a route that a particuar trip will operate. Values linked to those in [route_patterns.txt](#routepatternstxt).
bikes_allowed | Optional | Included | `1` if bikes are allowed on the trip, `2` if they are not. Empty or `0` values make no claim to whether bikes are allowed.
