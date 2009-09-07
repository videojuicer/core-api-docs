Campaign Objects
================

A campaign object is a named, re-usable collection of [campaign policies][campaign_policies].

[campaign_policies]: objects/campaign_policy.html

Attributes
----------

As a simple, coordinating entity a campaign object has only a handful of attributes...

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
		<td>user_id</td>
		<td>47</td>
		<td>The ID of the creating user. This may be set/updated directly only by applications with 'write-master' privileges.</td>
	</tr>
	<tr>
		<td>created_at</td>
		<td>2009-01-01T06:01:18+0000</td>
		<td>The timestamp indicating when the campaign was originally created. This attribute cannot be set/updated directly.</td>
	</tr>
	<tr>
		<td>updated_at</td>
		<td>2009-01-02T06:10:51+0000</td>
		<td>The timestamp indicating when the campaign was last updated. This attribute cannot be set/updated directly.</td>
	</tr>
	<tr>
		<td>name</td>
		<td>Winter 2009 Snowboarding Upsell</td>
		<td>The name of the campaign. Maximum length 255 characters. Note that the format of this name is restricted to alphanumeric characters, underscores and spaces.</td>
	</tr>
</table>

RESTful Requests
----------------

REST requests / responses work exactly as described in [requests][requests]...

* create: POST name to...
	http://api.videojuicer.com/campaigns.json?seed_name=myseed&api_version=1
* read: GET from...
	http://api.videojuicer.com/campaigns/47.json?seed_name=myseed&api_version=1
* update: PUT name to...
	http://api.videojuicer.com/campaigns/47.json?seed_name=myseed&api_version=1
* delete: DELETE from...
	http://api.videojuicer.com/campaigns/47.json?seed_name=myseed&api_version=1

[requests]: requests.html
