---
layout: post
title: Extending SwiftCloudant
date: 2016-10-15 15:09:29.000000000 +01:00
---

[SwiftCloudant][sc] is an asynchronous client library for Cloudant. It is based
on Foundation's OperationQueue and Operation classes. As a result each API endpoint
that [SwiftCloudant][sc] supports is it's own discrete operation. It is still
early in the development cycle for [SwiftCloudant][sc], which means that the API
isn't quite settled on and some APIs are not supported. Hopefully this post will
guide you to understanding how add support for APIs that currently aren't
in the release.

Let's take a look at adding support for `_active_tasks` API endpoint. To get
started, a class outline needs to be created:

```swift
class GetActiveTasks {

}
```

For this class to be accepted by client for processing, it needs to implement
the protocol `CouchDBOperation`. This protocol signifies that the class
defines an operation that should be performed on the server. It has a number
of properties and functions. There is only one property that is required, the
`endpoint` property. This should return the API which the operation is for. In
our case that is `_active_tasks`.

```swift
class GetActiveTasks {
    var endpoint: String { return "/_active_tasks" }
}
```

> Note: there will be a compile error at this point but don't worry.

This isn't the end of the story, there is another protocol that`GetActiveTasks`
needs to implement so it can get a lot of function such as response processing
for free. That protocol is `JSONOperation`. This protocol defines another property
`completionHandler` this is used to provide the user code with the response.

```swift
class GetActiveTasks {
    typealias Json = [[String : Any]]
    var endpoint: String { return "/_active_tasks" }

    let completionHandler: (([String : Any]?, HTTPInfo?, Error?) -> Void)?
}
```
The `Json` typealias informs the operation what kind of JSON structure is expected
to be returned from the server. For the case of `_active_tasks` that is an array,
of JSON objects. Now all is left is to create in the `init` method.


```swift
class GetActiveTasks {
    typealias Json = [[String : Any]]
    var endpoint: String { return "/_active_tasks" }

    let completionHandler: (([String : Any]?, HTTPInfo?, Error?) -> Void)?

    init(completionHandler: (([String : Any]?, HTTPInfo?, Error?) -> Void)? = nil) {
      self.completionHandler = completionHandler
    }
}
```

Now the `GetActiveTasks` class can be used directly with `CouchDBClient.submit(operation:)`
to get the active tasks for the server.

There are a few more hooks which make it possible to perform additional processing
like the `GetAllDocsOperation` and `QueryViewOperation` make use of to call a
handler for reach row in the response. But this is left as an exercise to the reader.

[sc]: https://github.com/cloudant/swift-cloudant
