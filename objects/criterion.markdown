Criterion Objects
=================

An individual [campaign policy][campaign_policies] uses one or more criterion objects in selecting the appropriate [promo(s)][promos] (if any) to associate with a given [presentation][presentations].

[campaign_policies]: objects/campaign_policy.html
[presentations]: objects/presentation.html
[promos]: objects/promo.html

Multiple types of criteria exist to support different matching scenarios...

* date_range: a time window between two timestamps
* geolocation: a progressively tighter match based on country, region and city as derived from the end-user's IP address
* request: a successive match against the host name and path segments of the embedding site
* time: a time-of-day string-matching pattern
* week_day: a simple boolean match against any / all of the days of the week

Attributes
----------

The following attributes are common to all criteria...

<table>
	<tr>
		<th>Attribute</th>
		<th>Example</th>
		<th>Notes</th>
	</tr>
	<tr>
		<td>id</td>
		<td>12</td>
		<td>System-assigned auto-ID. This attribute cannot be set/updated.</td>
	</tr>
	<tr>
		<td>campaign_policy_id</td>
		<td>47</td>
		<td>The ID of the parent campaign policy. This attribute cannot be set/updated but is rather set implicitly by the requests listed in 'RESTful Requests' below.</td>
	</tr>
	<tr>
		<td>created_at</td>
		<td>2009-01-01T06:01:18+0000</td>
		<td>The timestamp indicating when the criterion was originally created. This attribute cannot be set/updated directly.</td>
	</tr>
	<tr>
		<td>updated_at</td>
		<td>2009-01-02T06:10:51+0000</td>
		<td>The timestamp indicating when the criterion was last updated. This attribute cannot be set/updated directly.</td>
	</tr>
</table>

The attributes for date_range criteria are as follows. Note that at least one of 'from' / 'until' must be set and that, if they are both set, 'until' must be greater than 'from'...

<table>
	<tr>
		<th>Attribute</th>
		<th>Example</th>
		<th>Notes</th>
	</tr>
	<tr>
		<td>from</td>
		<td>2009-01-01T06:01:18+0000</td>
		<td>The timestamp indicating the beginning of the time window.</td>
	</tr>
	<tr>
		<td>until</td>
		<td>2009-01-02T06:10:51+0000</td>
		<td>The timestamp indicating when the end of the time window.</td>
	</tr>
</table>

The attributes for geolocation criteria are as follows...

<table>
	<tr>
		<th>Attribute</th>
		<th>Example</th>
		<th>Notes</th>
	</tr>
	<tr>
		<td>from</td>
		<td>2009-01-01T06:01:18+0000</td>
		<td>The timestamp indicating the beginning of the time window.</td>
	</tr>
	<tr>
		<td>until</td>
		<td>2010-01-02T06:10:51+0000</td>
		<td>The timestamp indicating when the end of the time window.</td>
	</tr>
</table>

The attributes for geolocation criteria are as follows...

<table>
	<tr>
		<th>Attribute</th>
		<th>Example</th>
		<th>Notes</th>
	</tr>
	<tr>
		<td>country</td>
		<td>United Kingdom</td>
		<td>The end-user's country. Maximum length 50 characters.</td>
	</tr>
	<tr>
		<td>region</td>
		<td>Texas</td>
		<td>An optional attribute specifying the end-user's region. Maximum length 50 characters.</td>
	</tr>
	<tr>
		<td>city</td>
		<td>Paris</td>
		<td>An optional attribute specifying the end-user's city. This may only be specified if a region has also been specified. Maximum length 255 characters.</td>
	</tr>
	<tr>
		<td>exclude</td>
		<td>true</td>
		<td>An optional attribute specifying that the criterion should only fire if it's settings are _not_ matched. Defaults to false.</td>
	</tr>
</table>

The attributes for request criteria are as follows...

<table>
	<tr>
		<th>Attribute</th>
		<th>Example</th>
		<th>Notes</th>
	</tr>
	<tr>
		<td>host</td>
		<td>http://www.example.org</td>
		<td>The embedding site's domain (note that sub-domains are matched progressively so that 'example.org' would match 'www.example.org', 'ftp.myaccount.example.org' and so on). Maximum length 255 characters.</td>
	</tr>
	<tr>
		<td>path</td>
		<td>/api/enable/5.xml</td>
		<td>An optional attribute specifying a progressively matched URL path (so that '/api/' would match '/api/example', '/api/example/5.xml' and so on). Maximum length 255 characters.</td>
	</tr>
	<tr>
		<td>exclude</td>
		<td>true</td>
		<td>An optional attribute specifying that the criterion should only fire if it's settings are _not_ matched. Defaults to false.</td>
	</tr>
</table>

The attributes for time criteria are as follows. Note that at least one of 'from' / 'until' must be set and that, if they are both set, 'until' must be greater than 'from'...

<table>
	<tr>
		<th>Attribute</th>
		<th>Example</th>
		<th>Notes</th>
	</tr>
	<tr>
		<td>from</td>
		<td>03:38</td>
		<td>A simple 'HH:MM' string indicating the starting time of day (inclusively).</td>
	</tr>
	<tr>
		<td>until</td>
		<td>15:04</td>
		<td>A simple 'HH:MM' string indicating the finishing time of day (inclusively).</td>
	</tr>
</table>

The attributes for week_day criteria are as follows...

<table>
	<tr>
		<th>Attribute</th>
		<th>Example</th>
		<th>Notes</th>
	</tr>
	<tr>
		<td>monday, tuesday, wednesday, thursday, friday, saturday, sunday</td>
		<td>true</td>
		<td>Whether or not to match on the specified day. At least one of the days of the week mst be set to true.</td>
	</tr>
	<tr>
		<td>exclude</td>
		<td>true</td>
		<td>An optional attribute specifying that the criterion should only fire if it's settings are _not_ matched. Defaults to false.</td>
	</tr>
</table>

RESTful Requests
----------------

The REST endpoints for criteria are type-specific. Thus the lead portion of the URLs for each type are of this form...

	http://api.videojuicer.com/campaigns/47/campaign_policies/81/criteria/date_range
	http://api.videojuicer.com/campaigns/47/campaign_policies/81/criteria/geolocation
	http://api.videojuicer.com/campaigns/47/campaign_policies/81/criteria/request
	http://api.videojuicer.com/campaigns/47/campaign_policies/81/criteria/time
	http://api.videojuicer.com/campaigns/47/campaign_policies/81/criteria/week_day

In addition, criteria may only be created and destroyed. Existing criteria are identfied by their attributes rather than their IDs. With these considerations in mind, REST requests / responses work as described in [requests][requests]...

* create: POST one or both of 'from' and 'until' to...
	http://api.videojuicer.com/campaigns/47/campaign_policies/81/criteria/date_range.json?seed_name=myseed&api_version=1
* delete: DELETE with previously POSTed paramaters from..
	http://api.videojuicer.com/campaigns/47/campaign_policies/81/criteria/date_range.json?seed_name=myseed&api_version=1

[requests]: requests.html
