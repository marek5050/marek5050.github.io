---
layout: post
title: "Introduction to Perfect and Swift on Digital Ocean"
subtitle:  "Perfect, it's all in the name."
date:   2016-08-01
categories: swift,server
---

## [Digital Ocean](https://m.do.co/c/c1c4910901e6)
Login to Digital Ocean or register to create a brand new droplet. Create a droplet with Ubuntu 14.04 64bit. I tried many other combinations with no luck, so 14.04 seems to be the most reliable. 

![Digital Ocean Droplet setup](http://res.cloudinary.com/ngdeploy/image/upload/v1471748807/Screen_Shot_2016-08-20_at_10.06.33_PM_rscmzd.png)

After creating the droplet you'll be taken to main droplet screen. It'll have the ip address of the server. SSH into the box and run the below installation commands.  
![Droplet Access](http://res.cloudinary.com/ngdeploy/image/upload/v1471748545/Screen_Shot_2016-08-20_at_10.02.07_PM_lwwxwp.png)

#### IMPORTANT Only works with Ubuntu 14.04 !!!!!
```
sudo apt-get update
sudo apt-get install make git clang libicu-dev libmysqlclient-dev libpq-dev sqlite3 libsqlite3-dev apache2-dev pkg-config libssl-dev libsasl2-dev libcurl4-openssl-dev uuid-dev wget

wget https://swift.org/builds/development/ubuntu1404/swift-DEVELOPMENT-SNAPSHOT-2016-08-18-a/swift-DEVELOPMENT-SNAPSHOT-2016-08-18-a-ubuntu14.04.tar.gz
tar xvf swift-DEVELOPMENT-SNAPSHOT-2016-08-18-a-ubuntu14.04.tar.gz
export PATH=`pwd`/swift-DEVELOPMENT-SNAPSHOT-2016-08-18-a-ubuntu14.04/usr/bin/:$PATH

git clone https://github.com/PerfectlySoft/PerfectTemplate
cd PerfectTemplate
git checkout d13e8dd8eb4868fea36468758604fe05a48b9aa2
swift build
.build/debug/PerfectTemplate --port 80
```

After everything above is successfully installed we should be able to access the website on the ip address of the server.

![Hello World](http://res.cloudinary.com/ngdeploy/image/upload/v1471748091/Screen_Shot_2016-08-20_at_9.35.20_PM_u8prel.png)


## Additional information
[WWDC Video](https://developer.apple.com/videos/play/wwdc2016/415/)  
Perfect framework and links  
- [Homepage](http://perfect.org)  
- [Git Repository](https://github.com/PerfectlySoft/Perfect)  
