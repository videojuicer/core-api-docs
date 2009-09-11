Single-object Requests
======================

As described previously, the Videojuicer core API is a RESTful interface to the various objects modelled by the Videojuicer service. In practice, this means that the vast majority of the requests from your application will be in the form of a create, read, update or delete of a uniquely named object.

Where extra methods are available for particular objects, such methods are defined in the [documentation specific to those objects][objects].

For details on how to retrieve lists of objects, refer to the [querying API][query_requests].

[objects]: objects/index.html
[query_requests]: query_requests.html

Requests
--------

The URLs for all RESTful requests follow a common convention (and always specify both a seed name and the required [API version][api_versions])...

<table>
	<tr><th>HTTP</th> <th>URL</th> <th>Action</th></tr>
	<tr>
		<td>POST</td>
		<td>http://api.videojuicer.com/presentations.xml?seed_name=myseed&amp;api_version=1</td>
		<td>Creates a new presentation from the posted form data.</td>
	</tr>
	<tr>
		<td>GET</td>
		<td>http://api.videojuicer.com/presentations/52.xml?seed_name=myseed&amp;api_version=1</td>
		<td>Reads an existing presentation (ID 52).</td>
	</tr>
	<tr>
		<td>PUT</td>
		<td>http://api.videojuicer.com/presentations/52.xml?seed_name=myseed&amp;api_version=1</td>
		<td>Updates an existing presentation (ID 52).</td>
	</tr>
	<tr>
		<td>DELETE</td>
		<td>http://api.videojuicer.com/presentations/52.xml?seed_name=myseed&amp;api_version=1</td>
		<td>Deletes an existing presentation (ID 52).</td>
	</tr>
</table>

Many browsers and gateways have limited HTTP verb support (particularly where PUT and DELETE are concerned). In order to overcome these limitations, requests may include an '_method' parameter (either in the form or as part of the URL) to simulate the necessary verb.

Since they modify an object, create, update and delete actions will need to made as [authenticated requests][authenticated_requests]. This also applies to read requests that access objects with some degree of privacy or those requests that need access to attributes not normally vended to the public at large (such as the publishing dates on a presentation).

Note also that all core objects support validation without creation / modification. This is useful in testing whether a POST / PUT would succeed if it were to be attempted. The API for validating creates and updates operates exactly as above except that the endpoints look like this...

	http://api.videojuicer.com/presentations/validate.json (POST)
	http://api.videojuicer.com/presentations/12/validate.json (PUT)

[api_versions]: api_versions.html
[authenticated_requests]: authenticated_requests.html

Responses
---------

The standard response to a RESTful request is an attribute hash of the object in question (whether it is newly created, already exists or has just been deleted). The individual documentation for each object type lists the attributes your application can expect to encounter in these responses and what they mean.

Overarching exceptions like requests for non-existent objects, violation of business rules and authentication issues all set the appropriate HTTP status code on the response _and_ provide an 'error' attribute resembling the following (in JSON notation)...

	{..., "error": {"http-equiv": "404 NotFound", "message" => "No such presentation."}, ...}

Validation problems for individual objects (almost always as a result of create/update requests) cause an additional 'errors' sub-hash to be returned with all the other marshalled attributes like so...

	{..., "errors": {"title": ["Title must not be blank"]}, ...}

The above examples request [XML][xml] formatted responses. The other formats that are available to your application are...

* [JSON:][json] '.json'
* [YAML:][yaml] '.yaml'

[json]: http://www.json.org
[xml]: http://www.w3.org/XML
[yaml]: http://www.yaml.org
