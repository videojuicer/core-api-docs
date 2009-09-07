Videojuicer Core Objects
========================

Each request made by your application will address one main type of object from the following set.

Asset
-----

An audio, Flash, image, text or video file with associated lifecyle, ownership and licensing information. One asset may be derived from another by the Videojuicer transformation service through the use of a Preset.

Campaign
--------

A named, re-usable collection of Campaign Policies associating a set of Presentations with a series of Promos.

Campaign Policy
---------------

A rule allowing the Videojuicer service to choose which Promo(s) (if any) to apply to the Presentations in a given Campaign (based on criteria such as the embedding site, the time/date and the end-user's location).

Criterion
---------

Collections of these allow a Campaign Policy to be selected.

Presentation
------------

Various chunks of a SMIL document in their to-be-compiled state with some associated lifecycle, ownership and authoring information. The SMIL document will typically carry references to Videojuicer-hosted Assets. Code (and thumbnails) used in embedding Presentations can be accessed via the Videojuicer OEmbed endpoint.

Preset
------

A bundle of transformation settings used to derive one Asset from another.

Promo
-----

A particular Asset in the context of a given Campaign Policy along with it's role name (preroll, overlay_top_left, etc...) and an optional click-through URL.

Seed
----

The self-contained, secure repository in which all of the other objects listed on this page are stored.

User
----

The personal details of a user of the system. Users can own other objects.
