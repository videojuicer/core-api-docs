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
		<td>The ID of the creating user. This attribute may be set/updated directly only by applications with 'write-master' privileges.</td>
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
		<td>http://myproduct.com</td>
		<td>The URL to which end-users will be sent when the presentation stage is clicked.</td>
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

HTML snippets for embedding individual presentations may be obtained by using the Videojuicer [OEmbed][oembed] endpoint. To obtain one of these 'embed codes', first take the resource URL of the presentation to be embedded...

	http://api.videojuicer.com/presentations/47.json?seed_name=myseed&api_version=1
	
Armed with this URL, build a request to the OEmbed endpoint...

	http://api.videojuicer.com/oembed
	
...ensuring that the following parameters are included...

* url: a URI-escaped version of the presentation URL (above)
* maxwidth: the maximum desired width for the embedded presentation's thumbnail (if available)
* maxheight: the maximum desired height for the embedded presentation's thumbnail (if available)
* format: the required response format (either 'json' or 'xml)
* api_version: the Videojuicer API version (required, as always)
* seed_name: the seed name (required, as always)

Thus a valid OEmbed URL constructed along these lines will resemble the following...

	http://api.videojuicer.com/oembed?format=json&seed_name=myseed&api_version=1&maxwidth=650&maxheight=400&url=http%3A%2F%2Fapi.videojuicer.com%2Fpresentations%2F47.json%3Fseed_name%3Dmyseed%26api_version%3D1
	
All responses from the OEmbed endpoint conform to the OEmbed 1.0 specification for the 'video' media type. The output for a call similar to the one outlined above will look like the following sample. Note the 'html' attribute containing the actal embed snippet.

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
	
[oembed]: http://oembed.com/

Related Presentation Requests
-----------------------------

The presentations API offers a special query method for finding all presentations, sorted by how similar their tagging is to a given presentation. To acquire this list, your application should issue a GET request to...

	http://api.videojuicer.com/presentations/47/related_by_tag.json?seed_name=myseed

Don't forget that (as per [the query request documentation][query_requests]), paging criteria can be added onto the query string...

	.../presentation[offset]=0&presentation[limit]=12

SMIL Document Workflow
======================

Presentation content is managed using the flexible markup language <abbr title="Synchronized Multimedia Integration Language">[SMIL][smil]</abbr>.

SMIL is an XML lingua that allows for the description of complex arrangements of assets and their placement in both time and space. Making use of the [Liquid templating language][liquid], the Videojucier service renders presentations as SMIL documents at request time, allowing for on-the-fly customisation to take place. In turn, the Videojuicer player consumes and renders these documents, delivering primary and promotional content to end-users.

A SMIL document is broken into 'head' and 'body' sections, containing metadata and content respectively. The following is a simple SMIL document for presenting a single video...

	<smil>
		<head>
			<title>An example document</title>
			<abstract>A longer description</abstract>
		</head>
		<body>
			<video src="http://example.com/foo.flv" />
		</body>
	</smil>
	
The SMIL standard supports the concept of 'regions'. Regions are independent areas of the player stage that can contain their own assets. Extending the previous document to include two regions (and some other constructs) yields...

	<smil>
		<head>
			<title>An example document</title>
			<abstract>A longer description</abstract>
			<layout>
				<region xml:id="root" width="100%" height="100%" z-index="0" />
				<region xml:id="overlay" width="40" height="40" bottom="5" left="5" z-index="1" />
			</layout>
		</head>
		<body>
			<par>
				<seq>
					<video src="http://example.com/foo.flv" region="root" />
					<video src="http://example.com/bar.flv" region="root" />
				</seq>
				<a href="http://purchasemystuff.com">
					<img src="http://example.com/buy_now.jpg" region="overlay" />
				</a>
			</par>
		</body>
	</smil>
	
The additional constructs comprise...

* A _layout_ defined inside the 'head' tag. This layout contains a region named 'root', which fills the stage completely, and another region named 'overlay' which has a fixed size of 40x40 pixels and sits 5 pixels from the bottom and left-hand edges of the player. The 'z-index' attribute determines the stacking order of the regions, ensuring that the overlay will always display on top of the root region.
* An additional video file and a hyperlink containing an image (which is notably similar in form to its HTML equivalent). All of the assets have their 'region' attributes set. As a result, the two video files will appear in the root region whilst the button is assigned to the overlay in the bottom-left corner.
* Some _timing tags_. The body of the document now contains a 'par' tag, and a child 'seq' tag holding the two videos. These two tags are used to control the flow of time within a presentation. Any assets within the 'par' tag will play in parallel, starting at the same base time. Any content contained within the 'seq' tag will play sequentially. Thus the document now describes a sequence of two videos playing sequentially with an overlay image displayed throughout.

To learn more about the SMIL language, please see the [SMIL 3.0 specification][smil]. The Videojuicer player does not currently implement all aspects of the 3.0 specification (such as relative timing and the SMIL event model). However, all basic timing and layout controls are supported.

[smil]: http://www.w3.org/TR/SMIL/

Using SMIL in Presentations
---------------------------

As implied in 'Attributes' above, the SMIL document delivered for a presentation is constructed using two key attributes on the presentation itself...

* document_content: all of the code destined for the 'body' section of the document
* document_layout: an optional attribute allowing for the definition of completely bespoke SMIL documents

If no document_layout is specified, the Videojuicer service will employ a default one that includes the presentation's metadata and the assets / promos defined by the relevant [campaign][campaigns] settings.

When creating bespoke SMIL documents, remember to include the '{{content}}' template variable in the layout to allow the document_content to be inserted when the presentation is rendered.

[campaigns]: objects/campaings.html

Liquid Template Helpers
-----------------------

As discussed, SMIL document rendering is based on the [Liquid][liquid] templating system, allowing presentations to differ from request to request.

Variables can be included in document output by wrapping them in {{two curly braces}}. The following variables are available to presentations when rendered...

* title, abstract, author and author_url: the values of these presentation attributes
* promos: an hash eligible [promos][promos], keyed by promo role
* content: the _rendered_ value of the document_content attribute (only availble when rendering the document_layout)

Each promo value is an array of eligible promos. Thus {{promos.preroll}} will return the set of promos intended to display before the presentation begins. A random promo can be picked by invoking the 'pick_random' method on the promo array: {{promos.preroll.pick_random}}.

Additionally, all asset and promo objects gain an additional 'smil_fragment' property. This renders a SMIL reference to the particular object so that {{promos.preroll.pick_random.smil_fragment}} will return a SMIL asset tag referencing the picked preroll asset (if such an asset exists).

The Videojuicer service provides a series of template helpers for rendering assets into presentations. The following snippet renders the video asset with ID 10...

	{% video %}{% id 10 %}{% endvideo %}
	
SMIL attributes on the asset tag may be overridden by adding them to the template call...

	{% video %}{% id 10 %}{% region "overlay" %}{% endvideo %}

Naturally, this approach works with all of the Videojuicer asset types (audio, Flash, image, text and video).

[liquid]: http://www.liquidmarkup.org/
[promos]: objects/promos.html

Publishing Presentations To Social Networks
-------------------------------------------

By default, the Videojuicer player allows users to share presentations through a number of social networks such as [Facebook][facebook]. The following best-practice guide addresses those applications with specialized requirements in this area.

In most cases, all that a social network requires in order to share any item with another user is a URL to which the user's friends will be linked. Furthermore, many social networks such as [Facebook][facebook], [Digg][digg], and [Reddit][reddit] perform inspections of shared pages, attempting to extract useful data such as thumbnails and embed codes for video.

For this reason, the Vdeojuicer service offers a _discovery URL_ for each presentation...

	http://api.videojuicer.com/presentations/10.html?seed_name=myseed
	
Whenever the in-player sharing features are used, it is this URL that is shared with the appropriate social network. Accordingly, it is highly recommended that your application distribute this discovery URL rather than the URL of a page where the presentation has previously been embedded.

The discovery endpoint behaves as follows...

* If the presentation has a callback_url set...
	* Users will be redirected to the callback_url.
	* Search engines will be permanently redirected to the callback_url, ensuring that shared links still contribute to page rankings.
	* Social networks searching for metadata will be presented with a specially-crafted page containing everything required for that network to embed your presentation.
* If the presentation has no callback_url set...
	* Users and search engines visiting the endpoint directly will be shown a very simple white page containing the embedded presentation.
	
[facebook]: http://www.facebook.com/
[digg]: http://www.digg.com/
[reddit]: http://www.reddit.com/

Appendix A: Default SMIL Document Layout
----------------------------------------

<p>If the document_layout attribute is left blank, the following template will be used. Note the use of the {{content}} variable which is replaced with the value of the document_content attribute at render time.</p>

	<?xml version="1.1" encoding="UTF-8"?>
	<!DOCTYPE smil PUBLIC "-//W3C//DTD SMIL 3.0 Language//EN" "http://www.w3.org/2008/SMIL30/SMIL30Language.dtd">
	<!-- Default SMIL document layout rendered at {{now | date: "%H:%M:%S %Y:%m:%d" }}-->
	<smil xmlns="http://www.w3.org/2008/SMIL30/" xml:lang="en">
    <head>
      <title>{{title}}</title>
      <author>{{author}}</author>
      <abstract>{{abstract}}</abstract>
      <copyright>{{author}}</copyright>
      <layout>
        <region xml:id="root" top="0" left="0" width="100%" height="100%" z-index="0" />
        
        {% if promos.overlay_top_left.size > 0 %}
          {% assign overlay_top_left = promos.overlay_top_left.pick_random %}
          <region xml:id="overlay_top_left" width="{{overlay_top_left.asset.width}}" height="{{overlay_top_left.asset.height}}" top="0" left="0" z-index="1" />
        {% endif %}

        {% if promos.overlay_top_right.size > 0 %}
          {% assign overlay_top_right = promos.overlay_top_right.pick_random %}
          <region xml:id="overlay_top_right" width="{{overlay_top_right.asset.width}}" height="{{overlay_top_right.asset.height}}" top="0" right="0" z-index="1" />
        {% endif %}
        
        {% if promos.overlay_bottom_left.size > 0 %}
          {% assign overlay_bottom_left = promos.overlay_bottom_left.pick_random %}
          <region xml:id="overlay_bottom_left" width="{{overlay_bottom_left.asset.width}}" height="{{overlay_bottom_left.asset.height}}" bottom="0" left="0" z-index="1" />
        {% endif %}
        
        {% if promos.overlay_bottom_right.size > 0 %}
          {% assign overlay_bottom_right = promos.overlay_bottom_right.pick_random %}
          <region xml:id="overlay_bottom_right" width="{{overlay_bottom_right.asset.width}}" height="{{overlay_bottom_right.asset.height}}" bottom="0" right="0" z-index="1" />
        {% endif %}
        
      </layout>
    </head>
    <body>
      <par>
        <seq>
          {{ promos.preroll.pick_random.smil_fragment }}
          {{ content }}
          {{ promos.postroll.pick_random.smil_fragment }}
        </seq>
        <par>

              {% if overlay_top_left %}
                {{ overlay_top_left.smil_fragment }} 
              {% endif %}

              {% if overlay_top_right %}
                {{ overlay_top_right.smil_fragment }} 
              {% endif %}
              
              {% if overlay_bottom_left %}
                {{ overlay_bottom_left.smil_fragment }} 
              {% endif %}
              
              {% if overlay_bottom_right %}
                {{ overlay_bottom_right.smil_fragment }} 
              {% endif %}

        </par>
      </par>
    </body>
	</smil>
