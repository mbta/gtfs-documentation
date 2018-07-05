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

* **GTFS Spec** field
	* **Required** 
	* **Optional**
	* **Experimental**: Not part of GTFS specifications, but included in MBTA GTFS

#### GTFS Files

Table Name | GTFS Spec | Status | Notes
---------- | ---------| ------- | ---------
agency.txt | Required | Included |
calendar.txt | Required | Included | Generated programatically. May not be easily human readable.	
calendar_dates.txt | Optional | Included | Generated programatically. May not be easily human readable.	
fare_attributes.txt | Optional | N/A |
fare_rules.txt | Optional | N/A |
feed_info.txt | Optional | Included |
frequencies.txt | Optional | Included | **frequencies.txt** is only used for Massport airport shuttles. If these move to a different feed then **frequencies.txt** will be removed.
multi_route_trips.txt | Experimental | Included | For trips that travel on more than one route, this file identifies additional routes the trip should be associated with. 
routes.txt | Required | Included |
shapes.txt | Optional | Included |
stops.txt | Required | Included |
stop_times.txt | Required | Included |
transfers.txt | Optional | Included |
trips.txt | Required | Included |

### agency.txt

Agencies include the MBTA, CapeFlyer, and Massport (for airport shuttles). CapeFlyer and Massport may move to separate GTFS feeds in the future.

Field Name | GTFS Spec| Status | Notes
---------- | -------- | ------ | -------
agency_id | Optional | Included | Yes | 
agency_name | Required | Included |
agency_url | Required | Included | 
agency_timezone | Required | Included | 
agency_lang | Optional | Included | 
agency_phone | Optional | Included | 
agency_fare_url | Optional | N/A | 
agency_email | Optional | N/A | 

#### calendar.txt:

Field Name | GTFS Spec | Status | Notes
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

#### calendar_dates.txt:

Field Name | GTFS Spec | Status | Notes
---------- | -------- | ------ | --------
service_id | Required | Included |  
date | Required | Included | 
exception_type | Required | Included | 

#### feed_info.txt:

Field Name | GTFS Spec | Status | Notes
---------- | -------- | ------ | ------
feed_publisher_name | Required | Included | 
feed_publisher_url | Required | Included | 
feed_lang | Required | Included | 
feed_start_date | Optional | Included | When comparing two GTFS feeds the newer one will always have a newer **feed_start_date**.
feed_end_date | Optional | Included |
feed_version | Optional | Included | 

#### frequencies.txt:

**Frequencies.txt** is only used for Massport airport shuttles. If these move to a different feed then **frequencies.txt** will be removed.

Field Name | GTFS Spec | Status | Notes
---------- | -------- | ------ | --------
trip_id | Required | Included (persistent) | 
start_time | Required | Included | 
end_time | Required | Included | 
headway_secs | Required | Included | 
exact_times | Optional | N/A | 


#### multi_route_trips.txt:

This is an experimental file. Some transit trips serve more than one route. Examples include commuter rail train trips that travel partly on the Lowell Line and partly on the Haverhill Line, and bus trips with labels like "Route 62/76" that should be shown on both Route 62's schedule and Route 76's schedule. This can apply to cases in which a trip serves one route and then the other, but also to cases in which the trip serves a unique combination of stops from each route. 

This file indicates routes on whose schedules such routes should be shown, besides the **route_id** identified with the trip in **trips.txt**. If showing one route for the trip, use the route identified with it in **trips.txt**. 

Field Name | GTFS Spec | Status |Notes
---------- | ------- | ------- | --------
added_route_id | Experimental | Included | An additional **route_id** associated with the given **trip_id**. Can appear repeatedly if more than one trip is added to it. 
trip_id | Experimental | Included | The **trip_id** of the trip which has the additional route associated with it. Can appear repeatedly if it is being added to more than one route. 

#### routes.txt:

Field Name | GTFS Spec | Status |  Notes
---------- | ------- | ------- | --------
route_id | Required | Included (persistent) |
agency_id | Optional | Included (persistent) | 
route_short_name | Required | Included (some records) | Populated for service with branches (such as Green Line or Silver Line), local bus, express bus, limited service bus, key bus routes, and Massport.<br>If **route_short_name** is populated for a record and **route_long_name** is not, then "Route **route_short_name**" is an appropriate way to describe the route.
route_long_name | Required | Included (some records) | Populated for commuter rail, CapeFlyer, rapid transit, ferry, and Massport.<br>For services with branches, both **route_long_name** and **route_short_name** are populated. **route_short_name** containts the branch designation only, and **route_long_name** identifies both the route and the branch.
route_desc | Optional | Included (some records) | Categorizes type of service. For example, identifies whether bus service is express bus or local bus, identifies if service is rapid transit.<br>Note that "Rapid Transit" **route_desc** values are associated with multiple **route_type** values.<br>**Possible Values:**<br>Airport Shuttle<br>Commuter Rail<br>Rapid Transit<br>Local Bus<br>Key Bus Route (Frequent Service)<br>Limited Service<br>Express Bus<br>Ferry
route_type | Required | Included | Indicates the type of vehicle that operates the route. It is not recommended to use this field's values to categorize MBTA service. 
route_url | Optional | Included (some records) | 
route_color | Optional | Included (some records) | 
route_text_color | Optional | Included (some records) | 
route_sort_order | Experimental | Included | Integer value that can be used for ordering routes in a way that is ideal for presentation to customers.

#### shapes.txt:

Field Name | GTFS Spec | Status |  Notes
---------- | -------- | ------ | -------
shape_id | Required | Included | 
shape_pt_lat | Required | Included | 
shape_pt_lon | Required | Included | 
shape_pt_sequence | Required | Included | 
shape_dist_traveled | Optional | Included (empty) | 

#### stops.txt:

Field Name | GTFS Spec | Status | Notes
---------- | -------- | ------ | --------
stop_id | Required | Included (persistent) | Expect field length to not exceed 45 characters.
stop_code | Optional | Included (some records)(persistent) | Field is populated only for subway and bus stops, where it has the same value as **stop_id**. Not prominently displayed or widely used, but visible on some bus stop signs.
stop_name | Required | Included | For bus stop, indicates geographic location of stop, with first street listed as the street the bus is traveling on, and second street listed (after "@" or "opp") as the cross street. For subway stop, indicates inbound or outbound direction.<br>For records with a **parent_station** value, use the child **stop_name** to indicate specific boarding location, but use the parent **stop_name** to identify the stop generally.<br>Average **stop_name** length is 24 characters. Expect field length to not exceed 60 characters.<br>Bus stops do not contain "stop" with a few exceptions ("Terminal B stop 1" "Terminal B stop 2".) Subway and commuter rail stations do not contain "station" with a few exceptions ("North Station" "South Station.")
stop_desc | Optional | Included (empty) | 
stop_lat | Required | Included | Records with a **parent_station** value currently have the same **stop_lat** value as the **parent_station**. However, the **parent_station** and child **stop_lat** values will differ in future versions of GTFS. 
stop_lon | Required | Included | Records with a **parent_station** value currently have the same **stop_lon** value as the parent_station. However, the **parent_station** and child **stop_lon** values will differ in future versions of GTFS.
zone_id | Optional | Included (empty) | 
stop_url | Optional | Included (empty) | 
location_type | Optional | Included | 
parent_station | Optional | Included (some records) | For subway stations, the **parent_station** stop represents the whole facility and the child stop represents a specific platform. Use for identifying station and indicating where specifically to enter a vehicle, respectively.<br/><br>Currently, only commuter rail stops that are located in a station that also serves subway have **parent_station** values, but in an upcoming GTFS version, all commuter rail stops will have **parent_station** values. 
stop_timezone | Optional | N/A | 
wheelchair_boarding | Optional | Included | 

#### stop_times.txt:

Field Name | GTFS Spec | Status | Notes
---------- | -------- | ------ | --------
trip_id | Required | Included (persistent) |  
arrival_time | Required | Included | 
departure_time | Required | Included | 
stop_id | Required | Included (persistent) | 
stop_sequence | Required | Included | 
stop_headsign | Optional | Included (empty) | May be used in the future. 
pickup_type | Optional | Included | A **pickup_type** of 3 represents a commuter rail "flag" stop marked with "F" on the schedule. Customer must visible to the approaching train to board.  
drop_off_type | Optional | Included | A **drop_off_type** of 3 represents a commuter rail "flag" stop marked with "F" on the schedule. Customer must notify the conductor to exit. 
shape_dist_traveled | Optional | N/A | 
timepoint| Optional | Included | On commuter rail, stops that are not timepoints and are not the last stop are labeled "L" on printed schedules to indicate that the train may leave early. MBTA bus and subway operation does not have many scheduled hold points so very few of their stop_times have a **timepoint** value of 1.  

#### transfers.txt:

There are few transfers currently, however more are planned for the future. 

Field Name | GTFS Spec | Status | Notes
---------- | -------- | ------ | ----------
from_stop_id | Required | Included | 
to_stop_id | Required | Included | 
transfer_type | Required | Included | 
min_transfer_time | Optional | Included (some records) | 

#### trips.txt:

Field Name | GTFS Spec | Status | Notes
---------- | ------- | ------- | --------
route_id | Required | Included (persistent)| 
service_id | Required | Included | 
trip_id | Required | Included (persistent)|
trip_headsign | Optional | Included | The average **trip_headsign** length is 13 characters. Expect field length to not exceed 80 characters.<br> Formats:<br><u>**Destination**</u><br> Example: Park Street<br>**<u>Destination</u> via <u>Street or Landmark</u>**<br>Example: Andrew via South Bay Center<br>**<u>Destination</u> via <u>Street or Landmark</u> & <u>Second Street or Landmark</u>**<br>Example: Wood Island via Market Basket & Admiral's Hill<br>**<u>Destination</u> via <u>Street or Landmark</u> and <u>Second Street or Landmark**</u><br>Example: Downtown Express via Airport and Paradise Road<br>**<u>Destination (Express)</u>**<br>Example: Haymarket (Express)<br>**<u>Destination (Limited Stops)</u>**<br>Example: Boston Medical Center (Limited Stops)<br>**<u>Destination</u> via <u>Street or Landmark (Express)</u>**<br>Example: Woburn via Medford Square (Express)<br>**<u>Destination</u> via <u>Street or Landmark</u> & <u>Second Street or Landmark (Express)</u>**<br>Example: Marblehead via Central Sq & Humphrey St (Express)
trip_short_name | Optional | Included (some records)(persistent) | **trip_short_name** is only a persistent identifier for commuter rail and CapeFlyer, field is blank for other modes. Even though it is not an "ID" field it can be used to keep track of trips through schedule changes. 
direction_id | Optional | Included (persistent) | Refer to **trip_headsign** value to determine which destination indicates inbound or outbound direction for route.
block_id | Optional | Included (some records) | **block_id** does not imply that a customer can remain on a vehicle through to the next trip, although there are some routes where this is permitted. These routes will be identified in a future GTFS change. <br>**Routes that allow ride-through to next trip:**<br>26, 43, 741, 742, 749, 67, 76
shape_id | Optional | Included | Generally included, but not guaranteed.
wheelchair_accessible | Optional | Included |  All MBTA vehicles are wheelchair accessible, value is always **1**.
bikes_allowed | Optional | N/A | 
