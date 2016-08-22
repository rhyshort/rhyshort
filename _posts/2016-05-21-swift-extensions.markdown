---
layout: post
title: Swift Extensions
date: 2016-05-21 13:28:23.000000000 +01:00
---
Extensions in swift are an easy way to provide extend the functionality of built in classes and protocols, for example:

```swift
public extension Array where Element : NSQueryItem {
    // Add methods which only apply to [NSQueryItem]
}
```

However there is a limitation. You cannot limit an extension to only apply to a specific type when it is on an extension. 

```swift
public extension Array where Element == SomeStruct {}
```
Limiting the an extension on the Generic type is not supported in swift (even in the swift trunk snapshots). Fortunately there is a way to get around it. You can define a protocol and then apply the extension to that.

```swift
protocol SomeProtocol {}
struct SomeStruct: SomeProtocol {}

public extension Array where Element : SomeProtocol {}
```
