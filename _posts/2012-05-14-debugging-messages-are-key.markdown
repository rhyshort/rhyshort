---
layout: post
title: Debugging Messages Are Key
date: 2012-05-14 20:09:29.000000000 +01:00
---

Something I learnt today is that debugging messages are key to helping users understand what is going. When I say users I mean advanced users like sys admins. The reason why I think this is because today I was playing with my linode VPS trying to find out why X forwarding kept on failing. Despite googling the error message and searching through the results, I struggled to find the answer however a quick look at the man pages meant that I noticed the -v options for ssh. Using this option and then finding the point where the X forwarding failed. It turned out that my install of ubuntu 12.04 server was lacking a simple program xauth for the connection to work one command later and everything worked. This experience reminded me of something Clive King said while giving a lecture at Aberystwyth University that debugging is a vital skill, and I found that the debugging messages in software is always helpful. So something for future applications I develop and definitely something I’d be including in my dissertation.

 

If you are interested the command was:

`sudo apt-get install xauth`

 


