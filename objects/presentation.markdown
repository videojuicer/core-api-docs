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

HTML Embed codes for individual presentations may be obtained by using the Videojuicer [OEmbed][oembed] endpoint. To obtain an embed code, first take the resource URL for the presentation you wish to embed:

	http://api.videojuicer.com/presentations/47.json?seed_name=myseed&api_version=1
	
Once you have the presentation URL, build a request to the OEmbed endpoint:

	http://api.videojuicer.com/oembed
	
Make sure to include the following parameters:

<table>
	<tr>
		<th>Parameter</th>
		<th>Example</th>
		<th>Notes</th>
	</tr>
	<tr>
		<td>url</td>
		<td>http%3A%2F%2Fapi.videojuicer.com%2Fpresentations%2F47.json%3Fseed_name%3Dmyseed%26api_version%3D1</td>
		<td>The CGI-escaped presentation URL</td>
	</tr>
	<tr>
		<td>seed_name</td>
		<td>myseed</td>
		<td>As with all calls to the API, the seed_name is a required parameter.</td>
	</tr>
	<tr>
		<td>api_version</td>
		<td>1</td>
		<td>As with all calls to the API, the api_version is a required parameter.</td>
	</tr>
	<tr>
		<td>maxwidth</td>
		<td>700</td>
		<td>The maximum desired width for the returned embed code.</td>
	</tr>
	<tr>
		<td>maxheight</td>
		<td>700</td>
		<td>The maximum desired height for the returned embed code.</td>
	</tr>
	<tr>
		<td>format</td>
		<td>json</td>
		<td>Either "json" or "xml". The OEmbed output will be returned in the specified format.</td>
	</tr>
</table>

A properly-created OEmbed URL will look something like:

	http://api.videojuicer.com/oembed?format=json&seed_name=myseed&api_version=1&maxwidth=650&maxheight=400&url=http%3A%2F%2Fapi.videojuicer.com%2Fpresentations%2F47.json%3Fseed_name%3Dmyseed%26api_version%3D1
	
All responses from the OEmbed endpoint conform to the OEmbed 1.0 specification for the 'video' media type. The output for a call similar to the one outlined above will look like this:

	{
		"title": "Example presentation",
		"abstract": "A longer description goes here.",
		"type":"video",
		"width": 650,
		"height": 366,
		"thumbnail_url":"http: / /thumbnails.videojuicer.com /demo /presentations /10.jpg?maxwidth=650&maxheight=400",
		"thumbnail_width": 650,
		"thumbnail_height": 366,
		"provider_name": "Videojuicer",
		"provider_url": "http://videojuicer.com",
		"author_url":"http://videojuicer.com",
		"author_name": "Dan Glegg",
		"version": "1.0",
		"cache_age": 43200,
		"html": "<object width= "650 " height= "366 "> <param name= "movie " value= "http: / /player.videojuicer.com /bootstrap.swf "  /> <param name= "allowFullScreen " value= "true "  /> <param name= "allowscriptaccess " value= "always "  /> <param name= "FlashVars " value= "seed_name=demo&presentation_id=10 "  /> <embed src= "http: / /player.videojuicer.com /bootstrap.swf " type= "application /x-shockwave-flash " allowscriptaccess= "always " allowfullscreen= "true " FlashVars= "seed_name=demo&presentation_id=10 " width= "650 " height= "366 "  /> < /object> <a href= "http: / /api.videojuicer.com /presentations /10.html?seed_name=demo ">Example presentation on Videojuicer< /a>"
	}
	
The "html" value is the embed code that may be distributed.

[oembed]: http://oembed.com/

Related Presentation Requests
-----------------------------

The presentations API offers a special query method for finding all presentations, sorted by how similar their tagging is to a given presentation. To acquire this list, your application should issue a GET request to...

	http://api.videojuicer.com/presentations/47/related_by_tag.json?seed_name=myseed

Don't forget that (as per [the query request documentation][query_requests]), paging criteria can be added onto the query string...

	.../presentation[offset]=0&presentation[limit]=12

SMIL Document Workflow
----------------------

DGTODO: DOCUMENT WORKFLOW
