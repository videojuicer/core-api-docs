Authenticated Requests
======================

Requests that modify objects or seek to access protected information must be authenticated. The Videojuicer core API makes use of the [OAuth][oauth] protocol in allowing your application (the 'consumer') to make requests on behalf of an authorized user. In conjunction with this approach, the Videojuicer service employs a simple privelege model that supports applications that need to control the entire sign-in process for a user (see 'Proxy Authentication' below).

[oauth]: http://oauth.net

Why Videojuicer Chose OAuth
---------------------------

Whereas some APIs require that you [hand over your username and password][antipattern] when using third-party services, OAuth allows such services ('providers') to issue consumers with a 'token'. This token allows the consumers to authenticate on behalf of a user, with a subset of the user's privileges.

This leaves users free to change their passwords without breaking any of the consumers they've authorized to act on their behalf. Futhermore, should a user decide that they no longer trust an application, they're free to downgrade its privileges or even revoke its access entirely without affecting any other applications.

[antipattern]: http://adactio.com/journal/1357

Getting Authorized
------------------

Before your application can make authenticated requests, it needs to obtain an 'authorized access token'. This token confers the set of priveleges associated with a particular Videojuicer user. The process for acquiring this token is laid out below. Note that a quick example of how to build a signed OAuth URL can be found in the following section ('Making An Authenticated Request').

**1. [Request an unauthorized request token][request_token_spec]**
Send a signed request to the Videojuicer 'request token endpoint' in order to receive an 'unauthorized request token'. This request, unlike the requests discussed in the next section, should not include a token or token secret.

	Request token endpoint: http://api.videojuicer.com/oauth/tokens

**2. [Obtain user authorization][user_authorisation_spec]**
Use the token received to derive a signed URL for the Videojuicer 'authorisation endpoint'. Redirect the user to this URL so that they can provide their credentials independent of your application. If your application is Web-based, remember to include an 'oauth_callback' parameter so that the authorisation endpoint knows which URL to redirect the user back to.

	Authorisation endpoint: http://api.videojuicer.com/oauth/tokens/new

**3. [Request the authorized access token][access_token_spec]**
Once the user has returned to your application, send a request signed (once again) with the unauthorized request token to the Videojuicer 'access token endpoint'. An authorized access token is returned. This access token may be used to sign all further requests, conferring the privileges available to the authorizing user.

	Access token endpoint: http://api.videojuicer.com/oauth/tokens

[request_token_spec]: http://oauth.net/core/1.0a#auth_step1
[user_authorisation_spec]: http://oauth.net/core/1.0a#auth_step2
[access_token_spec]: http://oauth.net/core/1.0a#auth_step3

Making An Authenticated Request
-------------------------------

Now that your application is in possession of an authorized access token, the process for making an authenticated request works as follows. Note that if your application intends to make authenticated requests over SSL, it is important to ensure that the 'https://' protocol prefix is used throughout this process.

1. Establish the HTTP verb, URL and parameters for the request...
		POST
		http://api.videojuicer.com/presentations.json
		seed_name=myseed
		api_version=1
		presentation[author]=John Mitchell
		presentation[title]=Harmony

2. Generate a signed request in accordance with [the most recent OAuth specification][oauth_spec]. Note that the current Videojuicer OAuth implementation supports 'HMAC-SHA1' and 'HMAC-MD5' signature methods (the former being preferred). As a quick example, here is some Ruby code for a non-multipart request...
		normalized_params = [
			"api_version=1",
			"oauth_consumer_key=myconsumerkey",
			"oauth_nonce=2431",
			"oauth_singature_method=HMAC-SHA1",
			"oauth_timestamp=1251757434",
			"oauth_token=ha644gf5qbv1",
			"presentation%5Bauthor%5D=John%20Mitchell",
			"presentation%5Btitle%5D=Harmony",
			"seed_name=myseed"
		]
		signature_base_string = [
			URI.escape("POST"),
			URI.escape("http://api.videojuicer.com/presentations.json"),
			URI.escape(normalized_params.sort.join("&"))
		].join("&")
		signature_secret = "my_consumer_secret&authorized_token_secret"
		signature_octet = HMAC::SHA1.digest(signature_secret, signature_base_string)
		encoded_signature = signature_octet.pack("m").delete("\n")
		normalized_params << "oauth_signature=#{encoded_signature}"
		query_string = normalized_params.sort.join("&")
		final_request = {
			:body => query_string,
			:content_length => query_string.length,
			:content_type => "application/x-www-form-urlencoded",
			:url => "http://api.videojuicer.com/presentations.json"
		}

3. Send the request ensuring that the relevant HTTP components are in place (see 'final_request' in the previous step).

[oauth_spec]: http://oauth.net/core/1.0a

Permissions
-----------

When first [registering your application][consumer_registration], you will be prompted to select the level of privilege that your consumer requires in order to function properly. There are four permission levels to choose from...

**read-user** applications can read objects with the same permissions as the authorizing user.
**write-user** applications can read and manipulate content with the same permissions as the authorizing user.
**read-master** applications can read objects with the same permission as any user within the authorizing user's seed.
**write-master** applications can read or write objects with the same permission as any user within the authorizing user's seed.

It should be noted that only users with the 'administrator' role can authorize consumers that require read-master or write-master permissions. The first user in a seed always starts out with this role.

Read-master and write-master permissions are primarily intended for applications such as Videojuicer's own Management Panel application. These sorts of applications provide pass-through authorization of individual users once they're up and running. Hence they need to be initially authorized once, for an entire seed, by a single administrator. Most applications such as upload, export or production workflow tools should only require read-user or write-user permissions.

[consumer_registration]: http://api.videojuicer.com/oauth/consumers

Proxy Authentication
--------------------

Under the traditional OAuth authentication model, signing a request with an authorized access token would authenticate your application as that token's authorizing user for the duration of the request. The multi-user, 'master' style of application discussed in the previous section needs to be able to indicate which user it is acting as during any given request. This is achieved by including a 'user_id' attribute in the parameter set (its value being the Videojuicer-supplied numeric ID for the user in question).

When making requests using proxy authentication (i.e. requests with a user ID), the Videojuicer service will respond as though your application were authenticated directly as the specified user. Attempting to perform actions or access content for which the this user does not have permission will result in an 401 Unauthenticated response.

Ownership
---------

Objects created via the Videojuicer core API are given ownership automatically by having their user_id attribute set to that of the request's authenticated user. In the event that your application is using proxy authentication (see above), the specified user_id will be honoured.

If your application is authorized with write-master access, it will also be able to set the user_id of any ownable object when creating or updating that object. This attribute cannot be set by a request with a permission level other than write-master.
