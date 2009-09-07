Campaign Policy Objects
=======================

[Campaigns][campaigns] are composed of campaign policy objects. These policies take a single [presentation][presentation] and apply a set of criteria to it in order to select the appropriate promo(s) (if any) to employ when buidling the its SMIL document.

[campaigns]: objects/campaign.markdown
[presentations]: objects/presentations.markdown

Attributes
----------

As an object joining a presentation with a group of promos and criteria, campaign policy objects have very few attributes...

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
		<td>campaign_id</td>
		<td>47</td>
		<td>The ID of the parent campaign. This attribute cannot be set/updated but is rather set implicitly by the requests listed in 'RESTful Requests' below.</td>
	</tr>
	<tr>
		<td>presentation_id</td>
		<td>62</td>
		<td>The ID of the presentation this policy addresses.</td>
	</tr>
	<tr>
		<td>created_at</td>
		<td>2009-01-01T06:01:18+0000</td>
		<td>The timestamp indicating when the campaign policy was originally created. This attribute cannot be set/updated directly.</td>
	</tr>
	<tr>
		<td>updated_at</td>
		<td>2009-01-02T06:10:51+0000</td>
		<td>The timestamp indicating when the campaign policy was last updated. This attribute cannot be set/updated directly.</td>
	</tr>
</table>

RESTful Requests
----------------

REST requests / responses work exactly as described in [requests][requests] except that campaign policies are 'nested' resources...

* create: POST presentation_id to...
	http://api.videojuicer.com/campaigns/47/campaign_policies.json?seed_name=myseed&api_version=1
* read: GET from...
	http://api.videojuicer.com/campaigns/47/campaign_policies/81.json?seed_name=myseed&api_version=1
* update: PUT presentation_id to...
	http://api.videojuicer.com/campaigns/47/campaign_policies/81.json?seed_name=myseed&api_version=1
* delete: DELETE from...
	http://api.videojuicer.com/campaigns/47/campaign_policies/81.json?seed_name=myseed&api_version=1

[requests]: requests.html