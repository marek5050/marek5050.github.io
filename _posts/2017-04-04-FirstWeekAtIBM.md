---
layout: post
title: "IBM Week 1: Go, tunnels, scp, and eventually sshuttle!"
subtitle:  "where ever you go."
date:   2017-04-04
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

### Tunnels are nice and handy But!! 
We have about 15 virtual machines hiding in a subnet far far away. To access these using SSH tunnels would take
a tremendous amount of work. So instead! There's a very neat Python tool called **[sshuttle](https://github.com/apenwarr/sshuttle)** that allows
a user to route all traffic to another machine.  The way I use is
```
$ sshuttle -r 192.168.100.1 192.168.200.0/24 
```
This routes all the traffic heading to the 192.168.200.0 network through the 192.168.100.1 server which knows
how to route the traffic. 

So in theory this is how the network topology would look for this to work.

![connecting remote networks](/static/sshuttle.png)

