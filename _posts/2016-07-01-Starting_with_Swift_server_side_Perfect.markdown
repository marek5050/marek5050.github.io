---
layout: post
title: "Introduction to Perfect and Swift on Digital Ocean"
subtitle:  "Perfect, it's all in the name."
date:   2016-08-01
categories: swift,server
---

## Additional information
[WWDC Video](https://developer.apple.com/videos/play/wwdc2016/415/)
Perfect framework and links
[Homepage](http://perfect.org)
[Git Repository](https://github.com/PerfectlySoft/Perfect)


## [Digital Ocean](https://m.do.co/c/c1c4910901e6)

#### IMPORTANT Ubuntu 14.04!!!!!
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

![Digital Ocean Droplet setup](http://res.cloudinary.com/ngdeploy/image/upload/v1471748100/Screen_Shot_2016-08-20_at_9.16.25_PM_ocbqhj.png)
![Hello World](http://res.cloudinary.com/ngdeploy/image/upload/v1471748091/Screen_Shot_2016-08-20_at_9.35.20_PM_u8prel.png)