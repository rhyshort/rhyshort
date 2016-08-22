---
layout: post
title: Progress so far
date: 2014-02-28 22:27:42.000000000 +00:00
---

I’m several weeks into my dissertation, and it is currently progressing well. There have been ups and downs, and silly mistakes, but thats what happens when you are learning. One of the things achieved was that I picked a name cromlech. It’s meaning can be found [here.](http://en.wikipedia.org/wiki/Cromlech)

### Implemented Features

Currently there are a number of features implement that appear to be fully working (It is difficult to fully test a program such as this). The main feature of creating processes from a definition file, is completed. The file uses JSON (JavaScript Object Notation) as the definition language. A definition file follows the below specification.

 { nodes:[ { name:"ProcessName", inputFD:[3,4], outputFD:[5,6], search:true filePath:"path to executable" } ], searchPath: ["path to search"] }

Thanks to the [Jansson C JSON library](http://www.digip.org/jansson/) most of the interoperation of this file is straight forward. The search path option defines a set of paths that should be searched for the executables as defined in the nodes. So a node in the mesh can either have its executable searched for or can have the path directly to the executable set. inputFD and outputFD are arrays of integers which define the links between nodes, so if inputFD on one node is 3, on a second node the outputFD should be 3, this defines the connection between the nodes in the mesh.

Executable searching uses a simple POSIX function to determine if a file exists, that function is stat. stat was brought to my attention as the quickest way to see if a file existed by [this stack overflow question](http://stackoverflow.com/questions/12774207/fastest-way-to-check-if-a-file-exist-using-standard-c-c11-c). Since POSIX functions are found in UNIX and UNIX like operating systems, it was the logical soloution to the problem.

The next feature implemented is the ability to persist PIDs so there wasn’t the constant need to have cromlech running at all times in order to manage the lifecycle of the mesh. The persistent pid file, uses JSON as the data format just like the definition file. Since this is an internal file, the definition might change, however currently it looks like this:

 { pids:[1453] }

This is a very simple file only holding the PIDS for the programs launched by cromlech.

Communicating between processes is achieved using a UNIX socket pair, so all the communications is handled just like writing and reading from a file. This approach was chosen as it appeared to be the best option for communicating between processes efficiently.

 

Next on the list of things to do is insert monitoring and create a UI to display the information collected by the mounting nodes. So the general layout of connections between nodes on the mesh will now look like this A -> Montoring Node -> B instead of A->B. Once the monitoring nodes have been inserted, and the UI created to view them, monitoring the input and output of processes will be a lot simpler as it will be possible to view the number of bytes following between the nodes.

 


