# MBTA GTFS Archive

#### About this document

This document describes the MBTA's historical archive of previously-published GTFS files.

## MBTA GTFS archive

For those interested in historical schedule data, an archive of GTFS files previously published by the MBTA are available. They are organized by the dates for which they were available and valid. 

Compared to the versions originally published, the GTFS files in the archive include several adjustments made to improve accuracy:
* Added [feed_info.txt](gtfs.md#feed_infotxt) to older files that did not contain it
* Adjusted the `feed_start_date` and `feed_end_date` to reflect the best date range to use for each file
* When a feed was published to correct an error in another, we replaced the erroneous feed, rather than adding a new feed

Earlier feeds have not been gone through to correct errors not identified at the time, or to add fields introduced in later feeds. We do not plan to do this in the future. Descriptions of each file, including their URLs, are available in CSV format in the archived_feeds.txt file at the following URL: [https://cdn.mbta.com/archive/archived_feeds.txt](https://cdn.mbta.com/archive/archived_feeds.txt). The fields contained in this file are described in the next section.

## archived_feeds.txt

Field Name | Description
---------- | --------
feed_start_date	| First date for which this feed should be used (matches same field in feed_info.txt)
feed_end_date	| Last date for which this feed should be used (matches same field in feed_info.txt)
feed_version	| A human-readable name for the feed (matches same field in feed_info.txt)
archive_url	| A URL where the feed is available for download
archive_note	| Notes on the content of the feed
