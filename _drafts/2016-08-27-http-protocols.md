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

The easiest way to do this was define a protocol which provides all the information
needed to make a request to CouchDB. The initial CouchDB protocol is defined as:

```swift
public protocol CouchOperation {

    var endpoint: String { get }

    var method: String { get }

    var parameters: [String: String] { get }

    var data: Data? { get }

    var contentType: String { get }

    func callCompletionHandler(response: Any?, httpInfo: HTTPInfo?, error: Swift.Error?)

    func processResponse(json: Any)

    func processResponse(data: Data?, httpInfo: HTTPInfo?, error: Swift.Error?)

    func validate() -> Bool

    func serialise() throws
}
```
The above sets out the information that is required by the machinery of the client
to be able to make a request. However this alone provides a lot of repetitive code,
where by operations need to process the response. However this can be resolved
using the power of protocol extensions it's possible to provide the machinery
to process the response. Since CouchDB also contains binary blobs in the form
of attachments, attachment operations will require different response processing.

So from here two new protocols which specialise the `CouchOperation` is required,
so the response processing can be written for Attachment Operations and document
operations once.

There are two types of response that we could receive from CouchDB, a generic
binary response for attachment data, and a JSON response. So We create two protocols
to match these responses types `JSONOperation` and `DataOperation`. These process the 
response in a similar way, however `JSONOperation` will deserialise the JSON response into
something usable by operation.

`JSONOperation` however has it's own problems, it needs to be able to process responses which
can either be a an Object, or an Array. As a result, `associatedtype` comes into play, with implementing
types defining that the response should be either an Array or a Dictionary, the default implementation can
process the response and provide the response as the correct type, to the implementing type.

As a result you end up with something like:

```swift 
public protocol JSONOperation: CouchOperation {
    
    var completionHandler: ((Json?, HTTPInfo?, Swift.Error?) -> Void)? { get }
}

public extension JSONOperation {
    
    public func callCompletionHandler(response: Any?, httpInfo: HTTPInfo?, error: Swift.Error?) {
        self.completionHandler?(response as? Json, httpInfo, error)
    }
    
    public func processResponse(data: Data?, httpInfo: HTTPInfo?, error: Swift.Error?) {
        guard error == nil, let httpInfo = httpInfo
            else {
                self.callCompletionHandler(error: error!)
                return
        }
        
        do {
            if let data = data {
                let json = try JSONSerialization.jsonObject(with: data)
                if httpInfo.statusCode / 100 == 2 {
                    self.processResponse(json: json)
                    self.callCompletionHandler(response: json, httpInfo: httpInfo, error: error)
                } else {
                    let error = Operation.Error.http(statusCode: httpInfo.statusCode,
                                            response: String(data: data, encoding: .utf8))
                    self.callCompletionHandler(response: json, httpInfo: httpInfo, error: error as Error)
                }
            } else {
                let error = Operation.Error.http(statusCode: httpInfo.statusCode, response: nil)
                self.callCompletionHandler(response: nil, httpInfo: httpInfo, error: error as Error)
            }
        } catch {
            let response: String?
            if let data = data {
                response = String(data: data, encoding: .utf8)
            } else {
                response = nil
            }
            let error = Operation.Error.unexpectedJSONFormat(statusCode: httpInfo.statusCode, response: response)
            self.callCompletionHandler(response: nil, httpInfo: httpInfo, error: error as Error)
        }
    }
}

public protocol DataOperation: CouchOperation {
    
    var completionHandler: ((Data?, HTTPInfo?, Swift.Error?) -> Void)? { get }
}

public extension DataOperation {
    public func callCompletionHandler(response: Any?, httpInfo: HTTPInfo?, error: Swift.Error?) {
        self.completionHandler?(response as? Data, httpInfo, error)
    }
    
    public func processResponse(data: Data?, httpInfo: HTTPInfo?, error: Swift.Error?) {
        guard error == nil, let httpInfo = httpInfo
            else {
                self.callCompletionHandler(error: error!)
                return
        }
        if httpInfo.statusCode / 100 == 2 {
            self.completionHandler?(data, httpInfo, error)
        } else {
            self.completionHandler?(data, httpInfo, Operation.Error.http(statusCode: httpInfo.statusCode, response: String(data: data!, encoding: .utf8)))
        }
    }
}

```

As a result of `JSONOperation` and `DataOperation` it becomes remarkably simple to write operations.
The perfect example of which is `GetAllDatabasesOperation` which is implemented as: 

```swift
public class GetAllDatabasesOperation : CouchOperation, JSONOperation {
    public typealias Json = [Any]

    public init(databaseHandler: ((_ databaseName: String) -> Void)? = nil,
        completionHandler:(([Any]?, HTTPInfo?,Error?) -> Void)? = nil) {
    
        self.databaseHandler = databaseHandler
        self.completionHandler = completionHandler
    }

    public let databaseHandler: ((_ databaseName: String) -> Void)?
    
    public let completionHandler: (([Any]?, HTTPInfo?, Error?) -> Void)?
    
    public var endpoint: String {
        return "/_all_dbs"
    }
    
    public func validate() -> Bool {
        return true
    }
    
    public func processResponse(json: Any) {
        if let json = json as? [String] {
            for dbName in json {
                self.databaseHandler?(dbName)
            }
        }
    }
    
}
```

As you can see with a small amount of work it is possible to make it easy to interact 
with the machinery of the library, with using protocol orientated programming, rather 
than the Object Orientated which can make it difficult to provide the same level 
of reusability.


Note: All Code excerpts are direct from SwiftCloudant are copyrighted to IBM and released
under the apache license, the licence headers are omitted for clarity.



[swift-cloudant]: https://github.com/cloudant/swift-cloudant
[cloudant]:  https://cloudant.com
[CouchDB]: http://couchdb.apache.org
[objective-cloudant]: https://github.com/cloudant/objective-cloudant
