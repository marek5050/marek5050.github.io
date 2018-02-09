---
layout: post
title: Fabric 1.1 Deploying a Fabric Composer Network
subtitle: they say it should make your life easier
date: 2018-02-09
categories: go
status: draft
---


## 1. Deploy a Fabric 1.1 network 

For this step we can just follow instructions from the official README.md 
1. In a directory of your choice (these instructions will assume `~/fabric-tools`), download the archive file that contains these tools. There are both .zip and .tar.gz formats - select one of these options:

```
$ mkdir ~/fabric-tools && cd ~/fabric-tools
$ curl -O https://raw.githubusercontent.com/hyperledger/composer-tools/master/packages/fabric-dev-servers/fabric-dev-servers.zip
$ unzip fabric-dev-servers.zip
```

2. Select the version of Hyperledger Fabric you wish to use. If for example you are using Hyperledger Composer v0.17
or higher then you should select Hyperledger Fabric V1.1 usually by exporting the environment variable as follows

```
export FABRIC_VERSION=hlfv11
```

3. If this is the first time that you have run these scripts, you'll need to download Hyperledger Fabric first. If you have already downloaded Hyperledger Fabric, then first start Hyperledger Fabric, and then create a Hyperledger Composer PeerAdmin card. After that you can then choose to stop Hyperledger Fabric, and start it again later. Alternatively, to completely clean up, you can teardown Hyperledger Fabric.

All the scripts will be available in the directory `~/fabric-tools`. A typical sequence of commands for using these scripts with Hyperledger Composer would be:

```
$ cd ~/fabric-tools
$ ./downloadFabric.sh
$ ./startFabric.sh
$ ./createPeerAdminCard.sh
```

Then at the end of your development session:

```
$ cd ~/fabric-tools
$ ./stopFabric.sh
$ ./teardownFabric.sh
```



## 2. Install Fabric Composer 


Install the most recent Fabric Composer libraries: 
```
$ npm install -g composer-cli@next composer-rest-server@next generator-hyperledger-composer@next composer-playground@next
```
This might take some time. 

## 3. Configure Fabric Composer 

Next download the Busines Network Archive file from here: 
```
$ curl https://github.com/marek5050/static/bna/carauction-network.bna
``` 
or feel free to compile your own Business Network Archive from the [fabric composer samples page](https://github.com/hyperledger/composer-sample-networks).

Then deploy the network using:

```
$ composer network deploy -a ./carauction-network.bna -A admin -S adminpw -c PeerAdmin@hlfv1 -f "./PeerAdmin@hlfv1.card"

Deploying business network from archive: ./carauction-network.bna
Business network definition:
        Identifier: carauction-network
        Description: Car Auction Business Network

✔ Installing runtime for business network undefined. This may take a minute...

Starting business network from archive: ./carauction-network.bna
Business network definition:
        Identifier: carauction-network@0.1.14
        Description: Car Auction Business Network

Processing these Network Admins: 
        userName: admin

✔ Starting business network definition. This may take a minute...
Successfully created business network card:
        Filename: ./PeerAdmin@hlfv1.card

Command succeeded
```

## 4. Import the Composer Network Card
 
 ```
 $composer card import -f ./fabric-scripts/hlfv1/PeerAdmin@hlfv1.card
 
 Successfully imported business network card
         Card file: ./fabric-scripts/hlfv1/PeerAdmin@hlfv1.card
         Card name: admin@carauction-network
 
 Command succeeded
```

List the available cards.  
```
composer card list
The following Business Network Cards are available:

Connection Profile: hlfv1
┌──────────────────────────┬───────────┬────────────────────┐
│ Card Name                │ UserId    │ Business Network   │
├──────────────────────────┼───────────┼────────────────────┤
│ admin@carauction-network │ admin     │ carauction-network │
├──────────────────────────┼───────────┼────────────────────┤
│ PeerAdmin@hlfv1          │ PeerAdmin │                    │
└──────────────────────────┴───────────┴────────────────────┘


Issue composer card list --name <Card Name> to get details a specific card

Command succeeded

```


## Run the Composer Playground
After running the Composer you should see the admin@carauction-network card and click Connect. 
Here go to the Model file and make changes to the model. 
Let's make a minor change and just add a year field to the car model and add a new coffee model. 


It'll look like this:
```
asset Coffee identifier by name {
    o String name
}

asset Vehicle identified by vin {
  o String vin
  o Integer year
  --> Member owner
}
```

Export the new BNA using the Export button on the bottom left. 


## Verify the existing network 
Before we update the network let's verify the existing information using. Notice it doesn't have the Coffee model yet. 

```
$ composer network list -c admin@carauction-network
✔ List business network from card admin@carauction-network
models: 
  - org.hyperledger.composer.system
  - org.acme.vehicle.auction
scripts: 
  - lib/logic.js
registries: 
  org.acme.vehicle.auction.Vehicle: 
    id:           org.acme.vehicle.auction.Vehicle
    name:         Asset registry for org.acme.vehicle.auction.Vehicle
    registryType: Asset
  org.acme.vehicle.auction.VehicleListing: 
    id:           org.acme.vehicle.auction.VehicleListing
    name:         Asset registry for org.acme.vehicle.auction.VehicleListing
    registryType: Asset
  org.acme.vehicle.auction.Auctioneer: 
    id:           org.acme.vehicle.auction.Auctioneer
    name:         Participant registry for org.acme.vehicle.auction.Auctioneer
    registryType: Participant
  org.acme.vehicle.auction.Member: 
    id:           org.acme.vehicle.auction.Member
    name:         Participant registry for org.acme.vehicle.auction.Member
    registryType: Participant

Command succeeded
```

## Update the existing network
Once we have the new BNA file we can update the network using the command:


```
$ composer network update -a  ./carauction-network_new.bna -c admin@carauction-network
Updating business network from archive: ./carauction-network_new.bna
Business network definition:
        Identifier: carauction-network@0.1.14
        Description: Car Auction Business Network

✔ Updating business network definition. This may take a few seconds...
Successfully updated business network

Command succeeded
```

## Verify changes

```
$ composer network list -c admin@carauction-network✔ List business network from card admin@carauction-network
models: 
  - org.hyperledger.composer.system
  - org.acme.vehicle.auction
scripts: 
  - lib/logic.js
registries: 
  org.acme.vehicle.auction.Coffee: 
    id:           org.acme.vehicle.auction.Coffee
    name:         Asset registry for org.acme.vehicle.auction.Coffee
    registryType: Asset
  org.acme.vehicle.auction.Vehicle: 
    id:           org.acme.vehicle.auction.Vehicle
    name:         Asset registry for org.acme.vehicle.auction.Vehicle
    registryType: Asset
  org.acme.vehicle.auction.VehicleListing: 
    id:           org.acme.vehicle.auction.VehicleListing
    name:         Asset registry for org.acme.vehicle.auction.VehicleListing
    registryType: Asset
  org.acme.vehicle.auction.Auctioneer: 
    id:           org.acme.vehicle.auction.Auctioneer
    name:         Participant registry for org.acme.vehicle.auction.Auctioneer
    registryType: Participant
  org.acme.vehicle.auction.Member: 
    id:           org.acme.vehicle.auction.Member
    name:         Participant registry for org.acme.vehicle.auction.Member
    registryType: Participant

Command succeeded
```


## Build the REST Api
To Deploy the REST Api we use the composer-rest-server command. 
```
$ composer-rest-server -c admin@carauction-network -n required -w true
Discovering types from business network definition ...
Discovered types from business network definition
Generating schemas for all types in business network definition ...
Generated schemas for all types in business network definition
Adding schemas for all types to Loopback ...
Added schemas for all types to Loopback
Web server listening at: http://localhost:3000
Browse your REST API at http://localhost:3000/explorer
```


Now we can visit http://localhost:3000/explorer to see the API. 
![Swagger](/static/bna/swagger.png)


and into the Matrix we run! 

<img src="https://nerdist.com/wp-content/uploads/2017/10/The-Matrix-code.jpg" width="300" />