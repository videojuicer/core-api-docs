Videojuicer Core Objects
========================

Each request made by your application will address one main type of object from the following set...

* [Asset][assets]: An audio, Flash, image, text or video file with associated lifecyle, ownership and licensing information. One asset may be derived from another by the Videojuicer transformation service through the use of a Preset.

* [Campaign][campaigns]: A named collection of Campaign Policies.

* [Campaign Policy][campaign_policies]: A rule allowing the Videojuicer service to choose which Promo(s) (if any) to apply to a given set of Presentations (based on criteria such as the embedding site, the time/date and the end-user's location).

* [Criterion][criteria]: Collections of these allow a Campaign Policy to select the appropriate Promo for a given Presentation.

* [Presentation][presentations]: Various chunks of a SMIL document in their to-be-compiled state with some associated lifecycle, ownership and authoring information. The SMIL document will typically carry references to Videojuicer-hosted Assets. Code (and thumbnails) used in embedding Presentations can be accessed via the Videojuicer OEmbed endpoint.

* [Preset][presets]: A bundle of transformation settings used to derive one Asset from another.

* [Promo][promos]: A particular Asset in the context of a given Campaign Policy along with it's role name (preroll, overlay_top_left, etc...) and an optional click-through URL.

* [User][users]: The personal details and credentials of a user of the system. Users can own other objects.

[assets]: objects/asset.html
[campaigns]: objects/campaign.html
[campaign_policies]: objects/campaign_policy.html
[criteria]: objects/criterion.html
[presentations]: objects/presentation.html
[presets]: objects/preset.html
[promos]: objects/promo.html
[users]: objects/user.html