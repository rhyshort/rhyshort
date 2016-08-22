---
layout: post
title: JUnit 4 on Android
date: 2015-06-03 19:55:27.000000000 +01:00
---

One of the hardest things when developing a library (or app) for android is using JUnit 4 for testing. There are many advantages to using JUnit 4, but from my point of view developing [Cloudant sync for android](https://github.com/cloudant/java-cloudant/) is being able to reuse test code that was written with Java SE in mind. Originally We created a app which acted as a test runner for our unit tests. This worked by lacked the ability to run single tests on the android platform. This is where [android test kit](https://code.google.com/p/android-test-kit/) comes in. It is a JUnit 4 test runner for android, it is available as part of the android support repository from the android SDK.

Adding to your project

Its easy to add to your project.

In your gradle file:
```java
 .... 
android { 
    .... 
    testInstrumentationRunner "android.support.test.runner.AndroidJUnitRunner"
    .... 
} 
.... 
dependencies {
    ... 
    androidTestCompile 'com.android.support.test:runner:0.2' 
    ... 
}
```

And that’s it, you’ll be able to run tests on android using JUnit 4. To see a more complex example, where you need to run a full suite of tests on Java SE and Android see [Cloudant Sync – Android](https://github.com/cloudant/java-cloudant/)‘s gradle files (even the ones in the AnroidTest sub directory)


