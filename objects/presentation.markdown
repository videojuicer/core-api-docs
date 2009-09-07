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
		<td>http://myproduct.com</td>
		<td>The URL to which you'd like viewers to be sent when the presentation stage is clicked.</td>
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
======================

Presentation content is managed using a flexible markup language called <abbr title="Synchronized Multimedia Integration Languag">[SMIL][smil]</abbr>.

SMIL is an XML lingua that allows you to describe complex arrangements of assets in both time and space. The Videojuicer player is designed to consume presentations written using the SMIL language. The Videojuicer API also makes use of the [Liquid templating language][liquid] to render SMIL documents at request time, allowing for on-the-fly customisation of your presentations.

Much like an HTML document, a SMIL document is broken into "head" and "body" sections, containing metadata and content respectively. Here's a very simple example SMIL document that contains a single video with a title and description:

	<smil>
		<head>
			<title>An example document</title>
			<abstract>A longer description</abstract>
		</head>
		<body>
			<video src="http://example.com/foo.flv" />
		</body>
	</smil>
	
The SMIL standard also allows for placing 'regions' on the stage. Regions are independent areas of the player stage that can contain their own assets. Let's extend our simple SMIL document with a _layout_ and some more video files:

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
	
Here's what we added:

* A _layout_ defined inside the &lt;head&gt; tag. It contains a region named 'root', which fills the stage completely, and another region named 'overlay' which has a fixed size of 40x40 pixels, which sits 5 pixels from the bottom edge of the player and 5 pixels from the left. The z-index attribute sets the stacking order of the regions, ensuring that the overlay will always display on top of the root region.
* Some _extra assets_. Within the body we've added another video file, and hyperlink containing an image. As you can see, linking from a SMIL document works along much the same lines as linking in HTML. The assets now have a 'region' attribute set. Note that the two video files are set to appear in the root region, while the button is assigned to the overlay in the bottom-left corner.
* Some _timing tags_. Within the body of the above document, you can see that all the content is included in a 'par' tag, and the two videos are included in a 'seq'. In a SMIL document, these two tags are used to control the flow of time within a presentation. Any content contained within a 'par' tag will play in parallel, starting at the same base time. Any content contained within a 'seq' tag will play sequentially, starting one after the other. The behaviour described in the above document has a sequence of two videos playing while an image is displayed to the user. Both videos will play in sequence, and the image will be overlayed in the bottom-left corner for the entire duration of the presentation. Seq and par tags may be nested within each other to create presentations that are as simple or complicated as you require them to be.

To read more about the tags and features available in the SMIL language, please see the [SMIL 3.0 specification][smil]. The Videojuicer player does not currently support all aspects of the 3.0 specification such as relative timing and the SMIL event model, but all basic timing and layout controls are supported.

Using SMIL in your presentations
--------------------------------

The SMIL document rendered by a presentation is defined by two attributes on the presentation itself:

<table>
	<tr>
		<th>Attribute</th>
		<th>Notes</th>
	</tr>
	<tr>
		<td>document_content</td>
		<td>The content of the SMIL document. Everything intended to go within the &lt;body&gt; tag, not including the &lt;body&gt; itself, should be set as the value of this attribute.</td>
	</tr>
	<tr>
		<td>document_layout</td>
		<td>Optional. The core API defines a default SMIL layout that by default will include presentation metadata and any assets defined in your Campaign settings. Leaving this attribute blank will cause the core API to use the default layout when your SMIL document is rendered. In most cases, you will not use this attribute. However if you wish to create fully customised SMIL documents then you may set it on a per-presentation basis. Be sure to include the {{content}} template variable in your layout, as this will be replaced with the contents of the document_content attribute when the presentation is rendered.</td>
	</tr>
</table>

Getting SMIL output
-------------------

To retrieve a SMIL representation of any presentation, make a request to a URL like so:

	http://api.videojuicer.com/presentations/47.smil?seed_name=myseed
	
Lists of presentations may also be rendered as SMIL documents:

	http://api.videojuicer.com/presentations.smil?seed_name=myseed&presentation[limit]=20&....

Liquid template helpers
-----------------------

When a request for SMIL content is received by the core API, the document layout and content attributes are rendered using the [Liquid][liquid] templating system, allowing presentations to differ from request to request.

Variables can be included in document output by wrapping them in {{two curly braces}}. The following variables are available to presentations when rendered:

<table>
	<tr>
		<th>Variable</th>
		<th>Notes</th>
	</tr>
	<tr>
		<td>title</td>
		<td>The value of the 'title' attribute on the presentation.</td>
	</tr>
	<tr>
		<td>abstract</td>
		<td>The value of the 'abstract' attribute on the presentation.</td>
	</tr>
	<tr>
		<td>author</td>
		<td>The value of the 'author' attribute on the presentation.</td>
	</tr>
	<tr>
		<td>author_url</td>
		<td>The value of the 'author_url' attribute on the presentation.</td>
	</tr>
	<tr>
		<td>promos</td>
		<td>An object containing the promos that are eligible for display based on the current request, keyed by promo role. For instance, {{promos.preroll}} will return an array of promos intended to display before the presentation begins. For a complete list of roles available to promos, see the Promo documentation. You may also call {{promos.preroll.pick_random}} to randomly select a single promo from the list.</td>
	</tr>
	<tr>
		<td>content</td>
		<td>Only available to the document_layout. Contains the rendered value of the document_content attribute.</td>
	</tr>
</table>

Additionally, all asset and promo objects available to your template gain an additional property:

<table>
	<tr>
		<th>Property</th>
		<th>Notes</th>
	</tr>
	<tr>
		<td>smil_fragment</td>
		<td>Can be called against any asset or promo object. Renders a valid SMIL reference to the given object. For instance, {{promos.preroll.pick_random.smil_fragment}} will return a SMIL asset tag referencing the chosen preroll asset, if one exists See Appendix A for more usage examples.</td>
	</tr>
</table>

Rendering assets using Liquid
-----------------------------

The core API also provides a series of template helpers for rendering assets into your presentations. To render the video asset with ID 10, we'd simply enter:

	{% video %}{% id 10 %}{% endvideo %}
	
This would output the _smil fragment_ for the asset:

	<video src="http://path/to/video/asset/10" dur="120s" region="root" />
	
You may also override attributes on the asset tag by adding them to your template call:

	{% video %}{% id 10 %}{% region "overlay" %}{% endvideo %}

The same premise applies to assets of all other types:

	{% audio %}{% id 120 %}{% endaudio %}
	{% text %}{% id 11 %}{% endtext %}
	{% flash %}{% id 3 %}{% endflash %}
	{% image %}{% id 107 %}{% endimage %}

Therefore the simplest possible playable presentation can be created by following this simple workflow:

1. Create a Video asset and note the ID of the returned object
2. Create a presentation, setting the document_content attribute to:

	{% video %}{% id id_of_your_asset %}{% endvideo %}

3. There is no step three.

[smil]: http://www.w3.org/TR/SMIL/
[liquid]: http://www.liquidmarkup.org/
[promo_roles]: http://TODO

Sharing presentations to social networks
----------------------------------------

By default, the Videojuicer player allows users to share presentations through a number of social networks including Facebook. If your custom application has a special workflow for sharing presentations, then you should read the following best practice passages for taking advantage of Videojuicer's social integration features. 

In most cases, all that a social network requires in order to share any item with another user is a URL to which the user's friends will be linked. Further to this, many social networks such as Facebook, Digg, and Reddit actually inspect pages when they are shared, attempting to grab useful data such as thumbnails and, in our case, embed codes for video.

For this reason, the core API offers a _discovery URL_ for each presentation:

	http://api.videojuicer.com/presentations/10.html?seed_name=myseed
	
Whenever the in-player sharing features are used, it is actually this discovery URL that is shared with the social networks. It is recommended that whenever you wish to link to a presentation in a sharable fashion, you give out the discovery URL rather than the URL of a page where you have previously embedded the presentation.

The discovery URL behaves as follows:

* If the presentation has a callback_url set:
	* Users will be redirected to the callback_url
	* Search engines will be permanently redirected to the callback_url, ensuring that shared links still contribute to your page rankings.
	* Social networks searching for metadata will be presented with a specially-crafted page containing everything the social network needs to embed your presentation.
* If the presentation has *no* callback_url set:
	* The discovery URL will behave the same as above, but users visiting the page directly will be shown a very simple white page containing the embedded presentation.

Appendix A: Default SMIL document layout
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

TODO: How to mock request properties for purposes of promo rendering
TODO: Fix link to promo role values
