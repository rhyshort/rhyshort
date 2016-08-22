---
layout: post
title: C# Multithreading
date: 2016-01-16 19:20:43.000000000 +00:00
---
Over the past few weeks, as part of working on a C# library for interacting with Cloudant I got to grips with the multithreading model for C#. 

The multithreading for C# revolves around Tasks, and `async` and `await` keywords. The `async` keyword defines that a method is asynchronous, although it does nothing for the method itself, it certainly makes it more readable. In order for a method to be asynchronous using `async` it needs to make use of a `await` statement. The await statement wraps up execution into a `Task` and returns control to the method caller.

The important thing to remember when using `async` and `await` for multithreading, is that in library code you should always call `configureAwait(continueOnCapturedContext:false)`, this call avoids deadlocks when waiting for a task to complete synchronously. ( I discovered this the hard way). I hope this helps someone else in a similar situation as me.
