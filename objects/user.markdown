User Objects
============

User objects hold the personal details and credentials of system users. Users can own other objects (see the 'Ownership' section of [authenticated requests][authenticated_requests]).

[authenticated_requests]: authenticated_requests.html

Attributes
----------

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
		<td>created_at</td>
		<td>2009-01-01T06:01:18+0000</td>
		<td>The timestamp indicating when the user was originally created. This attribute cannot be set/updated directly.</td>
	</tr>
	<tr>
		<td>updated_at</td>
		<td>2009-01-02T06:10:51+0000</td>
		<td>The timestamp indicating when the user was last updated. This attribute cannot be set/updated directly.</td>
	</tr>
	<tr>
		<td>login</td>
		<td>james</td>
		<td>The user's login. This must be unique for a given seed. Maximum length 255 characters.</td>
	</tr>
	<tr>
		<td>email</td>
		<td>james@example.com</td>
		<td>The user's e-mail address. This must be unique for a given seed. Maximum length 255 characters.</td>
	</tr>
	<tr>
		<td>name</td>
		<td>James Frobisher</td>
		<td>The user's full name. Maximum length 255 characters.</td>
	</tr>
	<tr>
		<td>roles</td>
		<td>[administrator, billing_contact]</td>
		<td>The roles assigned to the user (see 'Roles &amp; Role Requests' below). This attribute cannot be set / updated directly.</td>
	</tr>
</table>

RESTful Requests
----------------

Other than this specific consideration, REST requests / responses work exactly as described in [requests][requests]...

* create: POST login, name and email to...
	http://api.videojuicer.com/users.json?seed_name=myseed&api_version=1
* read: GET from...
	http://api.videojuicer.com/users/47.json?seed_name=myseed&api_version=1
* update: PUT any of login, name and email to...
	http://api.videojuicer.com/users/47.json?seed_name=myseed&api_version=1
* delete: DELETE from...
	http://api.videojuicer.com/users/47.json?seed_name=myseed&api_version=1
	
[requests]: requests.html

Roles & Role Requests
---------------------

Whereas all users can edit content within a seed (assets, presentations and so forth...), two specialised 'roles' may be granted to a user confering additionaly privileges...

* administrator: able to create users as well as inspect and modify their roles and personal data
* billing_contact: able to manipulate billing information for the seed

Note that the first user of a seed has both of the roles and that any user may have any combination of roles deemed appropriate by an administrator. Also note that, for safety, the Videojuicer service will not allow your application to remove a role from the last user who holds that role in a given seed.

Role manipulation is achieved by POSTing the relevant role name (as a 'role' paramater) to...

	http://api.videojuicer.com/users/47/add_role.json?seed_name=myseed&api_version=1 (ADD)
	http://api.videojuicer.com/users/47/remove_role.json?seed_name=myseed&api_version=1 (REMOVE)

The responses from these requests are exactly as for a normal user GET request.
