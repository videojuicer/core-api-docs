Videojuicer Core API
====================

The Videojuicer core API is a RESTful interface to the [various objects][objects] modelled by the Videojuicer service. Through [requests][requests] made to this service, your application can build upon Videojuicer's scalable hosting, manipulation, selection, authorization, monetization and monitoring of on-line video and other digital assets.

[objects]: objects/index.html
[requests]: requests.html

Getting Started
---------------

In order to get underway, you need the names of one or more 'seeds' and (optionally) a 'consumer key'.

**1. The Seed**

All of the assets, campaign rules, users and other mainstays of the Videojuicer service are hosted within a self-contained, secure repository referred to as a 'seed'. When your application makes [calls to the API][requests], a seed name is always one of the required parameters. If you don't already have a seed, you should [sign up for one][signup].

**2. The Consumer Key**

In order to make changes to the [objects][objects] contained within a seed (or to secure access to private content), your application needs to make [authenticated requests][authenticated_requests]. One of the fundamental parameters involved in such requests is a 'consumer key', which you may acquire by [registering your application][consumer_registration].

**3. Start Making Requests**

Armed with an active seed and a consumer key, you are ready to [start interacting with the API][requests].

[signup]: www.videojuicer.com/signup
[authenticated_requests]: authenticated_requests.html
[consumer_registration]: http://api.videojuicer.com/oauth/consumers
