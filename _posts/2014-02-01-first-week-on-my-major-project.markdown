---
layout: post
title: First Week on my Major Project
date: 2014-02-01 01:07:16.000000000 +00:00
---

Okay the first working week on my major project (dissertation in other subjects) is over. I’ve gone from being scared about the amount of work this entails to once more thinking it is achievable. So first some background on my project.

My major project is currently titled “A system to manage co-operating UNIX processes” (working title), essentially  the idea is to create a program which can launch a set of programs which can take any number of inputs and create any number of outputs to and from other UNIX process, creating a long running processes, in a mesh structure working together for a common goal.

The mesh will be  defined using a definition file, this will either be something designed from the scratch or using an existing data format such as XML or JSON, although this has yet to be decided, it is one of the big decisions left to be made.

There are also some other decisions to be made, such as how is the communication going to work? Is there a best order to build the mesh? Is it worth finding the best order?  How can the data flows be monitored for the user of the system to understand how data is moving between the nodes of the mesh?  The biggest decision I’ve made so far is to use C++ as the implementation language of choice.

### Methodology

The methodology I have decided to vaguely follow for the development of the system is SCRUM. I used SCRUM extensively during my year in industry placement at IBM, as a result it is the methodology  I am best placed to modify for it to sort of work for a single person project. The way I am doing this is removing the team focused sections, such as the keeping tasks, as well as stories. The daily scrum has been removed because it is pointless to have a daily scrum with just one person. The daily scrum will be replaced by using physical post-it notes to see how things are progressing and what is blocking me from making progress.

The stakeholder meeting is important, so this will be the meeting with my supervisor in which I hope to have a working demo, to be able to show. So they can keep track of my progress, and having a demo every week will encourage me not to break it and make sure I have it working most of the time.

### What has been achieved this week

This week a series of (untested) dummy nodes have been created, these nodes contain simple programmes which can be used to monitor the data flow through the system, to make sure it is working as expected.

The beginnings of the system which launches the processes has started, although this is little more than a collection of classes. That need to be brought together with other classes that have yet to be created to make the first version of the system.

One last thing to note, look out for #rhysandminion to see my rubber duck programming in action, and the occasional point the blame of it being broken at minion ![:)](http://www.rhys-short.me.uk/wordpress/wp-includes/images/smilies/simple-smile.png)


