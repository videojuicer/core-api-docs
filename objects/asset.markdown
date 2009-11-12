Asset Objects
=============

Asset objects encapsulate and describe the raw file data used in [presentations][presentation]. The specific asset types available are...

* audio
* Flash
* image
* text
* video

Assets are created either by uploading some file data or by requesting that a new asset be 'derived' from an existing one. Note that the creation of a non-derived asset is thus the only request with which it is valid to provide file data.

Attributes
----------

The following attributes are common to all asset types...

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
		<td>file</td>
		<td></td>
		<td>The section of the multipart POST containing the asset data. This must be provided at creation time for non-derived assets. It must not be provided otherwise.</td>
	</tr>
	<tr>
		<td>file_name</td>
		<td>song.mp3</td>
		<td>For non-derived assets, this is automatically set to the content disposition filename of the uploaded file. In this case, the extension must be appropriate to the specific asset type. For derived assets, this is set to the original asset's file_name with the relevant extension appended. This attribute cannot be set directly.</td>
	</tr>
	<tr>
		<td>file_size</td>
		<td>1289</td>
		<td>The size of the asset data in bytes. This attribute cannot be set/updated. It's value must exceed zero.</td>
	</tr>
	<tr>
		<td>state</td>
		<td>ready</td>
		<td>The asset's current stage in its lifecyle (see 'Lifecycle' below).</td>
	</tr>
	<tr>
		<td>state_changed_at</td>
		<td>2009-01-02T05:52:12+0000</td>
		<td>The timestamp of the last time the asset's state changed (initially nil). This attribute cannot be set/updated directly.</td>
	</tr>
	<tr>
		<td>state_changed_url</td>
		<td>http://api.example.com/process_asset_change</td>
		<td>If set, this URL will receive a GET request with parameters detailing the changed asset like this: '...change?seed_name=myseed&amp;asset_type=audio&amp;asset_id=12'. Maximum length 255 characters.</td>
	</tr>
	<tr>
		<td>url</td>
		<td>rtmp://vjuicerfvs.cdnetworks.net/vjuicerfvs/flashstream/mp4:assets.videojuicer.com/myseed/fa52f05e-cf81-11de-8413-0026bbfffe51.mp4</td>
		<td>The URL from which the asset data may be streamed once it is in a 'ready' state. Note that this may be an RTMP or HTTP URL for video and audio streams. This attribute cannot be set/updated.</td>
	</tr>
	<tr>
		<td>http_url</td>
		<td>http://assets.videojuicer.com/myseed/7ce083ce-9b84-11de-87f0-001b63992574.jpg</td>
		<td>A HTTP URL from which the asset data may be retrieved once the asset is in a 'ready' state. This may have the same value as the previous url attribute. This attribute cannot be set/updated.</td>
	</tr>
	<tr>
		<td>licensed_at</td>
		<td>2009-01-02</td>
		<td>An optional attribute to track when the asset was licensed under its current terms.</td>
	</tr>
	<tr>
		<td>licensed_by</td>
		<td>John Smith, Alan Potts</td>
		<td>An optional attribute to track the asset's license holders. Maximum length 255 characters.</td>
	</tr>
	<tr>
		<td>licensed_under</td>
		<td>Creative Commons (Non-attributable)</td>
		<td>An optional attribute to track how the asset is licensed. Maximum length 255 characters.</td>
	</tr>
	<tr>
		<td>published_at</td>
		<td>2008-08-08</td>
		<td>An optional attribute to track the asset's original publish date.</td>
	</tr>
	<tr>
		<td>created_at</td>
		<td>2009-01-01T06:01:18+0000</td>
		<td>The timestamp indicating when the asset was originally created. This attribute cannot be set/updated directly.</td>
	</tr>
	<tr>
		<td>updated_at</td>
		<td>2009-01-02T06:10:51+0000</td>
		<td>The timestamp indicating when the asset was last updated. This attribute cannot be set/updated directly.</td>
	</tr>
</table>

In addition to those listed above, there are additional read-only attributes associated with specific asset types. Note that additional attributes are planned but currently unsupported. These will include the number of audio channels, the audio sample rate and the individal audio and video bit rates of a video asset...

<table>
	<tr>
		<th>Attribute</th>
		<th>Example</th>
		<th>Asset Types</th>
		<th>Notes</th>
	</tr>
	<tr>
		<td>audio_format</td>
		<td>QDesign Music 2</td>
		<td>video</td>
		<td>The audio layer's format.</td>
	</tr>
	<tr>
		<td>bit_rate</td>
		<td>128000</td>
		<td>audio, video</td>
		<td>The overall bit rate in bits-per-second.</td>
	</tr>
	<tr>
		<td>duration</td>
		<td>15000</td>
		<td>audio, video</td>
		<td>The total duration in milliseconds.</td>
	</tr>
	<tr>
		<td>format</td>
		<td>QDesign Music 2</td>
		<td>audio</td>
		<td>The audio format.</td>
	</tr>
	<tr>
		<td>height</td>
		<td>480</td>
		<td>image, video</td>
		<td>The frame height in pixels.</td>
	</tr>
	<tr>
		<td>video_format</td>
		<td>Sorenson Video 3</td>
		<td>video</td>
		<td>The audio layer's format.</td>
	</tr>
	<tr>
		<td>video_frame_rate</td>
		<td>7.5</td>
		<td>video</td>
		<td>The average frame rate in frames-per-second.</td>
	</tr>
	<tr>
		<td>width</td>
		<td>640</td>
		<td>image, video</td>
		<td>The frame width in pixels.</td>
	</tr>
</table>

Finally, there are a few attributes that are only employed by derived assets. Again, these are read-only and serve only as an audit record of the derivation process...

<table>
	<tr>
		<th>Attribute</th>
		<th>Example</th>
		<th>Notes</th>
	</tr>
	<tr>
		<td>original_asset_id</td>
		<td>15</td>
		<td>The ID of the asset from which this one is derived.</td>
	</tr>
	<tr>
		<td>original_asset_type</td>
		<td>Audio</td>
		<td>The type of the asset from which this one is derived.</td>
	</tr>
	<tr>
		<td>preset_id</td>
		<td>84</td>
		<td>The ID of the preset that was used to derive this asset.</td>
	</tr>
	<tr>
		<td>source_space_window</td>
		<td>{left: "L+0", right: "R-0", bottom: "B+0", top: "T-0"}</td>
		<td>The space cropping used in deriving the asset (if the original asset was of type image/video).</td>
	</tr>
	<tr>
		<td>source_time_window</td>
		<td>{start: "S+0", end: "E-0"}</td>
		<td>The time cropping used in deriving the asset (if the original asset was of type audio/video).</td>
	</tr>
</table>

Lifecycle
---------

All assets debut in a 'pending' state. They then follow a linear progression through the following sequence...

1. to_acquire
2. acquiring
3. to_transform
4. transforming
5. to_stow
6. stowing
7. ready

Note that steps 1-4 are visited only for derived assets (see the 'Derive' request below). An asset can be used only once it is in its 'ready' state. Any attempt to access the location indicated by its 'url' attribute before it is 'ready' will return a 404 NotFound error.

RESTful Requests
----------------

The REST endpoints for assets are type-specific. This is becase the unique identifier for an asset within a seed is the combination of its type _and_ its ID. Thus the lead portion of the URLs for each type are of this form...

	http://api.videojuicer.com/assets/audio
	http://api.videojuicer.com/assets/flash
	http://api.videojuicer.com/assets/image
	http://api.videojuicer.com/assets/text
	http://api.videojuicer.com/assets/video

Other than this specific consideration, REST requests / responses work exactly as described in [requests][requests]...

* create: POST file (and optionally the file_name, the various licensed_* attributes and a state_changed_url) to...
	http://api.videojuicer.com/assets/audio.json?seed_name=myseed&api_version=1
* read: GET from...
	http://api.videojuicer.com/assets/audio/47.json?seed_name=myseed&api_version=1
* update: PUT any of the various licensed_* attributes and/or a new state_changed_url to...
	http://api.videojuicer.com/assets/audio/47.json?seed_name=myseed&api_version=1
* delete: DELETE from...
	http://api.videojuicer.com/assets/audio/47.json?seed_name=myseed&api_version=1
	
Note that the assets interface supports the validation methods described in [requests][requests] with the additional feature that no file need be presented (so that validation can be achieved without a bandwidth-consuming upload).

[requests]: requests.html

Derivation Request
------------------

The Videojuicer service offers the ability to transform assets on your behalf, transcoding a video file or extracting the audio track from a video and so on. Before your application can request a derived asset, it must first have knowledge of two objects...

* a valid original asset
* a valid [preset][preset]

To request a derived asset, simply POST the preset_id to the original asset's end point...

	http://api.videojuicer.com/assets/audio/47.json?seed_name=myseed&api_version=1
	
Assuming that the original asset and the specified preset both exist _and_ do not (in combination) imply an unsupported transformation, a new asset will be created and returned (with the usual 'pending' state). The backend transformation pipeline will then activate and deliver the derived asset's data as quickly as possible, updating the asset's state as it goes (see 'Lifecycle' above).

Note that future support is planned for the ability to crop the source in height, width and time (as appropriate) but that this feature is currently unavailable.

Externally Derived Assets
-------------------------

In order to support the scenario whereby derived versions of an asset are created externally and then imported (without recourse to the Videojuicer transformation service), it is possible to mark an asset as being derived from another explicitly. In order to achieve this, your application must have knowledge of three objects...

* two valid assets (to act as 'original' and 'derived')
* a valid [preset][preset] (i.e. that which would have been used had this been a standard derivation request - see above)

To mark an asset as derived, POST the preset_id, original_asset_type ('audio', 'flash', ...) and original_asset_id to the to-be-marked asset's set_derived URL...

	http://api.videojuicer.com/assets/audio/set_derived/47.json?seed_name=myseed&api_version=1

As with standard derivation requests (see above) this request does require that both the assets and the preset exist. What it will not do is check whether the combination of these three objects implies an otherwise unsupported transformation - this is for your application to decide.
