![](https://event.livehouse.in/straas.io/admintool/images/logo.png)

**StraaS.io - Streaming as a Service, Your Best OTT Solution**

World-class streaming and advertisement technology to drive profit and provide best multi-screen,
multi-device user experiences.

The Enigma API
====================

Enigma provides transcoding as a service. She converts the source media content to the easily distributed format. 


Making a request
----------------

All URLs start with `https://tx.straas.net/<account_id>/api/v1/`. **SSL only**. The path is prefixed with the account id and the API version.
If we change the API in backward-incompatible ways, we'll bump the version marker and maintain stable support for the old URLs.

That's all!


Authentication
--------------


Handling errors
---------------

If Enigma is having trouble, you might see a 5xx error. `500` means that the app is entirely down, but you might also see `502 Bad Gateway`, `503 Service Unavailable`, or `504 Gateway Timeout`.
It's your responsibility in all of these cases to retry your request later. 


Rate limiting
-------------

You can perform up to 500 requests per 10 second period from the same IP address for the same account. 
If you exceed this limit, you'll get a [429 Too Many Requests](http://tools.ietf.org/html/draft-nottingham-http-new-status-02#section-4) response for subsequent requests.
Check the `Retry-After` header to see how many seconds to wait before retrying the request.


API ready for use
-----------------

