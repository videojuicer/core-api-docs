Preset Objects
==============

Preset objects are named bundles of transformation settings used to derive one Asset from another. The type and file format of the asset to be derived are key attributes of a preset. Thus if (for example) your application needs to derive a 64Kb/s MP3 from the audio layer of an AVI, a preset will have to have been created with an audio_bit_rate of 64000, a derived_type of 'Audio' and a file_format of 'mp3'.

Attributes
----------

Whilst all of the following attributes may be set at creation time (save for ID, created_at and updated_at), none of them are modifiable via an update except for the preset's name. Where an attribute is unset, the transformation service will do its best to honour the relevant setting from the original asset.

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
		<td>name</td>
		<td>iPhone Small</td>
		<td>A helpful name for the preset. Maximum length 255 characters.</td>
	</tr>
	<tr>
		<td>derived_type</td>
		<td>Audio</td>
		<td>One of the transformable Videojuicer asset types, capitalized. Currently: 'Audio', 'Image' or 'Video'.</td>
	</tr>
	<tr>
		<td>created_at</td>
		<td>2009-01-01T06:01:18+0000</td>
		<td>The timestamp indicating when the preset was originally created. This attribute cannot be set/updated directly.</td>
	</tr>
	<tr>
		<td>updated_at</td>
		<td>2009-01-02T06:10:51+0000</td>
		<td>The timestamp indicating when the preset was last updated (i.e. renamed). This attribute cannot be set/updated directly.</td>
	</tr>
	<tr>
		<td>file_format</td>
		<td>mp3</td>
		<td>The container format for the derived asset. See 'format request' below for details on how to retrieve a list of available formats.</td>
	</tr>
	<tr>
		<td>audio_bit_rate</td>
		<td>64000</td>
		<td>The audio bit rate in bits-per-second.</td>
	</tr>
	<tr>
		<td>audio_channels</td>
		<td>2</td>
		<td>The number of audio channels. Defaults to 2 (implying a stereo stream).</td>
	</tr>
	<tr>
		<td>audio_format</td>
		<td>aiff</td>
		<td>The format for the derived audio stream. See 'format request' below for details on how to retrieve a list of available formats.</td>
	</tr>
	<tr>
		<td>audio_sample_rate</td>
		<td>22050</td>
		<td>The audio sample rate in hertz.</td>
	</tr>
	<tr>
		<td>video_bit_rate</td>
		<td>256000</td>
		<td>The video bit rate in bits-per-second.</td>
	</tr>
	<tr>
		<td>video_format</td>
		<td>libx264</td>
		<td>The format for the derived video stream. See 'format request' below for details on how to retrieve a list of available formats.</td>
	</tr>
	<tr>
		<td>video_frame_rate</td>
		<td>7.5</td>
		<td>The average frame rate in frames-per-second.</td>
	</tr>
	<tr>
		<td>width</td>
		<td>640</td>
		<td>The frame width in pixels.</td>
	</tr>
	<tr>
		<td>height</td>
		<td>480</td>
		<td>The frame height in pixels.</td>
	</tr>
</table>

RESTful Requests
----------------

REST requests / responses work exactly as described in [requests][requests]...

* create: POST name, derived_type and file_format (and optionally any of the asset-apecific attributes above) to...
	http://api.videojuicer.com/presets.json?seed_name=myseed&api_version=1
* read: GET from...
	http://api.videojuicer.com/presets/47.json?seed_name=myseed&api_version=1
* update: PUT name to...
	http://api.videojuicer.com/presets/47.json?seed_name=myseed&api_version=1
* delete: DELETE from...
	http://api.videojuicer.com/presets/47.json?seed_name=myseed&api_version=1

[requests]: requests.html

Format Request
--------------

Since Videojuicer seeks constantly to improve the range of container and stream formats offered by the transformation service, a request endpoint exists to provide the most up-to-date list of these entities. To acquire this list, simply issue a GET request to...

	http://api.videojuicer.com/presets/formats.json?seed_name=myseed&api_version=1

The response is a categorized set of hashes of the form...

	{ file_formats: {Audio:["mp3"], Image:["jpg", "png"], Video:["mp4"]},
	  audio_formats: {pcm_u16be:"PCM unsigned 16-bit big-endian", ...},
	  video_formats: {h261:"H.261"} }
