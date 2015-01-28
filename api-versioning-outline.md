# API Versioning and transition (Outline)

 * The problem
  * Things change, API moves on
  * Many clients
  * Stability. Stable Contracts
 * Common versioning practices
  * URL
  * custom request header
  * accept header
 * The defaults.
 * Accept Header all the things
 * Transition API
  * Requirements
  * API key
  * Deprecated
  * Breaking changes
  * What about the defaults? Well, they change.


## The Problem

The longer the application's and API's lifespan, the greater the commitment to
the users of the application and API. [2]

Things change and API needs to grow and move on. So we need to support API
versioning, and we need to do it for a while. While maintaining multiple
version of a API for a client is user-friendly. Maintaining possibly infinite,
yeah not really infinite, number of API version is not very developer friendly.

We need to support multiple versions of an API for a period of time.
So how do we procede? First of all we need to choose how we do API versioning.
I will start of discussing the a few approaches to API versioning.

Then I will propose a way to transition APIs.

the most important question to ask is “what are you wanting to version?”
The logical extension to this question is,
“what is the contract between your service and client?” [5]


## Common practices

### URL
"URLs suck because they should represent the entity.
I’m retrieving is a breached account, not a version of the breached account." [1]

### Custom headers
"The HTTP spec gives us a means of requesting the nature we’d like the resource
represented in by way of the accept header, why reproduce this?" [1]

### Accept headers
I can no longer just give someone a URL and say “Here, click this”,
rather they have to carefully construct the request and configure
the accept header appropriately. [1]

## The defaults
What happens when no version is provided. Don't break the contract and default
to the first version of the API.

The way to go around this reasonable objection, is to implement the latest API version under versionless API base URI. In this case, API client developers can choose to either develop against the latest one or bind to a specific version of the API (which becomes apparent) but only for a limited time [2]

## Versioning strategy

### Versioning Strategy 1: Adding content to a representation.
In this case, the answer to the versioning question is to just add it.  Now, this assumes that your clients will ignore what they don’t understand. [5]

### Versioning Strategy 2: Breaking changes in a representation.
In the case where you’re either removing or renaming content from an existing representation design, you will be breaking clients. [5]

### Versioning Strategy 3: Breaking the semantic map
In both of the prior strategies, all changes, breaking and non-breaking, have been related to the representations.  However, there may be occasions – hopefully rarely – when you need to break the meaning of a resource and therefore the mapping between the resource and its set of entities. [5]

## Accept-header all the things

using the Accept header as part of content negotiation [4]

RFC4288 section 3.2 outlines how a vendor, i.e. an application,
can make use of customisable MIME types in the Accept header. [4]

## Deprecation

So accessing any of the obsolete URIs should return any of the 30x HTTP
status codes that indicate redirection [2]

But what about changes to the model of the resources? An attribute changing type or
name?

## Transition API
Keeping all versions of an API alive is hard, and I honestly do not believe
they should either. However, when we have an API running in production with
all sort of clients. We do need a way to update and remove older versions of
the API without breaking the existing clients. So how can we delete an old
version of the API without breaking clients dependent upon that API version.
Well, the honest truth is that you cant. But we can mitigate the risk by tracking
API activity and track which client who is currently consuming it.

Tracking API version and consumer activity can be done by introducing API keys.
Every user of that expects the API to be stable must have use an API key.
The keys can be obtained by providing an email or some other contact info.

Now the proud owner of the emails of the client maintainer. You can now track
which clients that are using deprecated versions of your api, and safely
warn the maintainers about them.


"... a particular resources should not change once a resource addressing scheme
becomes public and therefore final" [2].  Does not with in with the transitioning

"Sure, it is possible to embed API version in base URI but only for reasonable
and restricted uses like debugging a API client" [2]. I like this.

If I did a lot of this sort of resource versioning, it is very possible that I could end up with an ugly-looking URL space. But REST was never about pretty URLs and the whole point of the hypermedia constraint is that clients should not need to know how to construct those URLs in the first place – so it really doesn’t matter whether they’re pretty or ugly – to your client, they’re just strings. [5]

API versioning also helps smooth over any major API version transitions as you can continue to offer old API versions for a period of time. [6]

Well documented and announced multi-month deprecation schedules can be an acceptable practice for many APIs. [6]



## Resources
1. http://getpocket.com/a/read/543464569
2. http://stackoverflow.com/questions/389169/best-practices-for-api-versioning
3. http://www.lexicalscope.com/blog/2012/03/12/how-are-rest-apis-versioned/
4. http://pivotallabs.com/api-versioning/
5. http://getpocket.com/a/read/248165345
6. http://www.vinaysahni.com/best-practices-for-a-pragmatic-restful-api
7. http://timelessrepo.com/haters-gonna-hateoas
8. http://barelyenough.org/blog/2008/05/versioning-rest-web-services/

## Note
