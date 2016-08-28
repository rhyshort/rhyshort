---
layout: post
title: Protocol Orientated Swift.
date: 2016-08-26 20:09:29.000000000 +01:00
---

There are a lot of ways to implement an HTTP layer, with the advent of swift,
and the my latest project as part of Cloudant; [Swift Cloudant][swift-cloudant]. The opportunity
arose to build upon a few principles explored as part of a previous project [Objective Cloudant][objective-cloudant].

[Swift Cloudant][swift-cloudant] is first and foremost a client library for CouchDB and Cloudant.
this provides its own set of problems. The biggest is making an extensible API that
makes adding calls to the [CouchDB][CouchDB] API easy. This would make it quicker
for me to add new APIs to interact with CouchDB and users to create classes
that can be used as first class citizens with the underlying machinery of the client.



[swift-cloudant]: https://github.com/cloudant/swift-cloudant
[cloudant]:  https://cloudant.com
[CouchDB]: http://couchdb.apache.org
[objective-cloudant] https://github.com/cloudant/objective-cloudant
