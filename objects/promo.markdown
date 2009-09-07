Promo Objects
=============

Promo objects allow [campaign policies][campaign_policies] to deliver specific assets into [presentations][presentations] based on one or more [criteria][criteria]. 

[campaign_policies]: objects/campaign_policy.html
[criteria]: objects/criterion.html
[presentations]: objects/presentation.html

There are multiple promo types corresponding to the promo-eligible Videojuicer asset types...

* audio
* image
* text
* video

Attributes
----------

Promos have the following attributes...

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
		<td>asset_id</td>
		<td>81</td>
		<td>The ID of the associated asset.</td>
	</tr>
	<tr>
		<td>role</td>
		<td>preroll</td>
		<td>The role of the promo. The standard roles are: preroll, postroll, overlay_top_left, overlay_top_right, overlay_bottom_left and overlay_bottom_right. Maximum length 255 characters.</td>
	</tr>
	<tr>
		<td>href</td>
		<td>http://www.example.org/api/purchase.cgi</td>
		<td>A click-through URL associated with the promo. Maximum length 255 characters.</td>
	</tr>
</table>

RESTful Requests
----------------

The REST endpoints for promos are type-specific. Thus the lead portion of the URLs for each type are of this form...

	http://api.videojuicer.com/campaigns/47/campaign_policies/81/promos/audio
	http://api.videojuicer.com/campaigns/47/campaign_policies/81/promos/image
	http://api.videojuicer.com/campaigns/47/campaign_policies/81/promos/text
	http://api.videojuicer.com/campaigns/47/campaign_policies/81/promos/video

In addition, existing promos are identfied by their attributes rather than their IDs. Thus promos cannot be updated and are deleted using the parameters originally POSTed during their creation. With these considerations in mind, REST requests / responses work as described in [requests][requests]...

* create: POST asset_id, role (and optionally an href) to...
	http://api.videojuicer.com/campaigns/47/campaign_policies/81/promos/audio.json?seed_name=myseed&api_version=1
* read: GET from...
	http://api.videojuicer.com/campaigns/47/campaign_policies/81/promos/audio/1.json?seed_name=myseed&api_version=1
* delete: DELETE with previously POSTed paramaters from..
	http://api.videojuicer.com/campaigns/47/campaign_policies/81/promos/audio.json?seed_name=myseed&api_version=1

[requests]: requests.html
