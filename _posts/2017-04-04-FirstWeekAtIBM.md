---
layout: post
title: "IBM Week 1: Go, vim-go, tunnel go"
subtitle:  "where ever you go."
date:   2016-08-21
categories: go
status: publish
---

I started working with an amazing team here in Austin, surrounded by brilliant people everywhere I turn. We hit the ground
 running and from day one I was rolling in code. I was shocked to find out that IBM internally uses Slack and Github since
 I always imagined IBM would have some crappy internal version control (which I was told it did until a year or so back :D ). It
 definitely seems like I joined at the right time. 
 
So I said we hit the ground running right? So some of the first couple of things I learned were:
 
### SCP isn't scary....
I've always been terrified of **scp** and everytime somebody mentioned it I walked away....
```
$ scp ./localfile root@192.168.1.1:/root/
> localfile
```

### SSH Tunneling is pretty awesome!  
So we're building the next big thing. This requires me to have a number of virtual machines running at the same time and those are usually controlled
by another virtual machine. So imagine <My Machine> -> <Master Controller> -> <Service Node>
SSH Tunneling allows me to open an ssh connection on <Master Controller> that routes all the traffic to <Service Node> 
``` bash
$ ssh -L -N 9022:192.168.1.1:22 &
```
Cool we have a tunnel!

### SCP to a very remote machine! 
So having a tunnel open on port 9022 to the remote machine allows my personal macbook to **scp** files to a machine I'd otherwise not
 have access to. That's because the <Master Controller> routes everything on port 9022 to the <Remove Machine> 
```bash
$ scp -P 9022 ./localfile root@192.168.1.1:/root/
```

### Sure SCP, what about something useful? 
Well another benefit of this tunneling is that it lets you chain remote machines and it actually forwards any ports you might need. 
For example I need port 8181 to remote debug a **Go** binary using **dlv**. I started a headless dlv session on the remote machine
 and was able to connect my local dlv debugger to the remote machine just because of tunneling!
....

## Debugging
...
Considering you followed the previous guide on installing the Swift with Perfect on Digital Ocean. 

## Deploying  



## Additional information
[WWDC Video](https://developer.apple.com/videos/play/wwdc2016/415/)  
Perfect framework and links  
- [Homepage](http://perfect.org)  
- [Git Repository](https://github.com/PerfectlySoft/Perfect)  
