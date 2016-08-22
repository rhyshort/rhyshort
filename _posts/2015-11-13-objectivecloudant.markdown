---
layout: post
title: ObjectiveCloudant
date: 2015-11-13 11:25:21.000000000 +00:00
---
[ObjectiveCloudant](https://cocoapods.org/pods/ObjectiveCloudant) is a library for Objective-C and Swift for interacting with Remote CouchDB or Cloudant instances. It is the replacement for the remote function of [CloudantToolKit](https://cocoapods.org/pods/CloudantToolKit). [ObjectiveCloudant](https://cocoapods.org/pods/ObjectiveCloudant) is written in Objective-C, but makes full use of the latest language features, such as generics and nullability annotations, to make the library work seamlessly with swift, (it can also build as a framework).

[ObjectiveCloudant](https://cocoapods.org/pods/ObjectiveCloudant) has a CloudKit inspired API. It is completely Asynchronous and encourages users to use completion handlers rather than wait for the operation to complete.

[ObjectiveCloudant](https://cocoapods.org/pods/ObjectiveCloudant) is still very much in development and lacks several key features, which I hope we will be able to bring in the near future. Features such as Search, being able to use custom authentication schemes via the HTTPinterceptor API. For the HTTPInterceptor API the ground work has already been laid, and the authentication method used by [ObjectiveCloudant](https://cocoapods.org/pods/ObjectiveCloudant), session cookies, uses an HTTP Interceptor under the hood to authenticate requests. 
