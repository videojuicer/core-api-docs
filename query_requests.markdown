Query Requests
==============

The Videojuicer core API provides a unified mechanism for requesting (optionally filtered) lists of objects and conventions for determining their collation and pagination.

Requests
--------

Query settings are passed in as parameters to a normal GET request without an ID...

	http://api.videojuicer.com/presentations.json?seed_name=demo&api_version=1&query_setting_1&query_setting_2&...
	
Each query setting is a key-value pair specifying an object/collation/pagination attribute and its associated value. All specified settings acumulate (so that the sense of the query is 'match all' rather than 'match any'). The exact syntax of a setting is...

	object_type[attribute]=value
	
As a concrete example, the end of the query URL above could look like this...

	...&presentation[title]=Harmony&presentation[order]=created_at.desc&presentation[offset]=14&presentation[limit]=7

Multiple values for filtering and ordering settings are specified using comma-seperated notation.
	
The filtering operations available are...

* equals...
	presentation[title]=Harmony
* does not equal...
	presentation[title.not]=Melody,Rhapsody
* like (supporting the use of escaped '%' characters as wildcards)...
	presentation[title.like]=Harm%25
* greater than (or equal to)...
	presentation[created_at.gt]=2005-05-10
	presentation[created_at.gte]=2005-05-09
* less than (or equal to)...
	presentation[created_at.lt]=2005-05-07
	presentation[created_at.lte]=2005-05-08
	
The collation/pagination operations available are...

* limit: the number of results to return (required if 'offset' or 'page' are specified)
	presentation[limit] = 10
* offset: the number of results to skip
	presentation[offset] = 22
* page: this is a convenience method equivalent to specifying an offset of (limit * (page - 1))
	presentation[page]=5
* order: the sort order of the results (sort direction 'asc'/'desc' is required)...
	presentation[order]=title.asc,author.desc
	
Responses
---------

Responses are in the form of an attribute hash (here presented in JSON notation but, as with regular requests, available in HTML, XML and YAML as required)...

	{ "items": [object_1, object_2, ...], "count": 4, "limit": 7, "offset": 0 }
	
Note that each object in this example is an attribute hash exactly as described in the [single-object request API][requests]. Additionally, the 'count' attribute indicates the total number of found objects (and so can and will often be exceed the value of 'limit').

[requests]: requests.html
