Presentation Objects
====================

As the fundamental unit of structured delivery, presentation objects essentially wrap the data necessary to create a SMIL document upon request. It is this document that is consumed and rendered by the Videjuicer player.

Attributes
----------

The following attributes encapsulate the non-document metadata about a presentation...

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
		<td>The timestamp indicating when the presentation was originally created. This attribute cannot be set/updated directly.</td>
	</tr>
	<tr>
		<td>updated_at</td>
		<td>2009-01-02T06:10:51+0000</td>
		<td>The timestamp indicating when the presentation was last updated. This attribute cannot be set/updated directly.</td>
	</tr>
	<tr>
		<td>title</td>
		<td>Fly Fishing</td>
		<td>The title of the presentation. Maximum length 255 characters.</td>
	</tr>
	<tr>
		<td>author</td>
		<td>James Frobisher</td>
		<td>An optional attribute containing the presentation's author's full name. Maximum length 255 characters.</td>
	</tr>
	<tr>
		<td>author_url</td>
		<td>http://www.example.org/frobisher</td>
		<td>An optional attribute containing the presentation's author's URL. Maximum length 255 characters.</td>
	</tr>
	<tr>
		<td>abstract</td>
		<td>Lorem ipsum dolor...</td>
		<td>An optional attribute containing a summary of the presentation's content. Maximum length 255 characters.</td>
	</tr>
	<tr>
		<td>callback_url</td>
		<td>DGTODO: EXAMPLE URL</td>
		<td>An optional attribute containing an external service URL used for both visitor redirects and event callbacks. DGTODO: ONE SENTENCE WORKFLOW SUMMARY.</td>
	</tr>
	<tr>
		<td>image_asset_id</td>
		<td>28</td>
		<td>An optional attribute containing the ID of an asset to be used as a thumbnail. This must be set explicitly if a thumbnail is required.</td>
	</tr>
	<tr>
		<td>tags</td>
		<td>foo bar 'multi word'</td>
		<td>The tags to applied to this presentation.</td>
	</tr>
</table>

Additionally, a presentation contains attributes directly involved in rendering the requisite SMIL document. These are...

* document_type (defaulting to 'SMIL 3.0' - the only currently-supported SMIL standard)
* document_layout
* document_content

For detail on these attributes, see 'SMIL Document Workflow' below.

RESTful Requests
----------------

REST requests / responses work exactly as described in [requests][requests]...

* create: POST title (and optionally any of the other directly settable attributes above) to...
	http://api.videojuicer.com/presentations.json?seed_name=myseed&api_version=1
* read: GET from...
	http://api.videojuicer.com/presentations/47.json?seed_name=myseed&api_version=1
* update: PUT any of the directly settable attributes above to...
	http://api.videojuicer.com/presentations/47.json?seed_name=myseed&api_version=1
* delete: DELETE from...
	http://api.videojuicer.com/presentations/47.json?seed_name=myseed&api_version=1

Note that presentations, as embeddable objects, can return HTML or SMIL format responses to a GET request. [Query requests][query_requests] can also respond with SMIL documents (effectively playlists of every returned presentation).

[requests]: requests.html
[query_requests]: query_requests.html

OEmbed Requests
---------------

DGTODO: DOCUMENT WORKFLOW

Related Presentation Requests
-----------------------------

The presentations API offers a special query method for finding all presentations, sorted by how similar their tagging is to a given presentation. To acquire this list, your application should issue a GET request to...

	http://api.videojuicer.com/presentations/47/related_by_tag.json?seed_name=myseed

Don't forget that (as per [the query request documentation][query_requests]), paging criteria can be added onto the query string...

	.../presentation[offset]=0&presentation[limit]=12

SMIL Document Workflow
----------------------

DGTODO: DOCUMENT WORKFLOW
