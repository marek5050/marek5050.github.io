---
layout: post
title: "Fabric: Working with private collections"
subtitle: "...we'd like to hide information in the blockchain"
date: 2018-02-23
categories: fabric
status: publish
---


## Enabling Private Collections in Fabric 1.1

References:  
https://gerrit.hyperledger.org/r/#/c/14769/  
https://jira.hyperledger.org/browse/FAB-6600  


```
$ git clone https://gerrit.hyperledger.org/r/fabric 
```

Then we grab the appropriate files: 
```
$ git fetch https://gerrit.hyperledger.org/r/fabric refs/changes/69/14769/7 && git cherry-pick FETCH_HEAD
```

Script for testing private data e2e:

Make these changes:

In: 
`/examples/e2e_cli/docker-compose-cli.yaml`  
change command from:
`./scripts/script.sh`
to
`./scripts/script_marbles_private.sh`

In: 
/examples/e2e_cli/base/peer-base.yaml

Add:
``` 
       - CORE_LOGGING_LEVEL=INFO
       - CORE_LOGGING_GOSSIP=DEBUG
       - CORE_PEER_GOSSIP_PVTDATA_TRANSIENTSTOREMAXBLOCKRETENTION=500
       - CORE_LEDGER_PVTDATA_BTLPOLICY_MYCHANNEL_MARBLESP_COLLECTIONMARBLES=1000000
       - CORE_LEDGER_PVTDATA_BTLPOLICY_MYCHANNEL_MARBLESP_COLLECTIONMARBLEPRIVATEDETAILS=3
```
 
In: 
examples/e2e_cli/configtx.yaml
Change:
``` 
    Application: &ApplicationCapabilities
        # V1.1 for Application is a catchall flag for behavior which has been
        # determined to be desired for all peers running v1.0.x, but the
        # modification of which would cause imcompatibilities.  Users should
        # leave this flag set to true.
        V1_1: true
        V1_1_PVTDATA_EXPERIMENTAL: true  
```

```  
make docker-clean docker
 
cd examples/e2e_cli

./network_setup.sh up mychannel

```
 
4 peer network will be up now, and chaincode will be installed.  Do the remaining steps via CLI:

``` 
docker exec -it cli bash
```
 
Marblesp
Instantiate chaincode on org1/peer0
```
peer chaincode instantiate -o orderer.example.com:7050 --tls $CORE_PEER_TLS_ENABLED --cafile /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem -C mychannel -n marblesp -v 1.0 -c '{"Args":["init"]}' -P "OR ('Org0MSP.member','Org1MSP.member')" --collections-config collections.json
```
Output
```
2018-02-22 14:42:13.497 UTC [msp] GetLocalMSP -> DEBU 001 Returning existing local MSP
2018-02-22 14:42:13.497 UTC [msp] GetDefaultSigningIdentity -> DEBU 002 Obtaining default signing identity
2018-02-22 14:42:13.503 UTC [chaincodeCmd] checkChaincodeCmdParams -> INFO 003 Using default escc
2018-02-22 14:42:13.503 UTC [chaincodeCmd] checkChaincodeCmdParams -> INFO 004 Using default vscc
2018-02-22 14:42:13.504 UTC [chaincodeCmd] getChaincodeSpec -> DEBU 005 java chaincode enabled
2018-02-22 14:42:13.505 UTC [msp/identity] Sign -> DEBU 006 Sign: plaintext: 0ACB070A6708031A0C08C5ADBBD40510...0B12090A074F7267314D535018012001 
2018-02-22 14:42:13.505 UTC [msp/identity] Sign -> DEBU 007 Sign: digest: 1E5CE50304B0EB7932F95FC72614AF7DF60F6AAA7487F5ECC170FB1A149E5052 
2018-02-22 14:42:24.874 UTC [msp/identity] Sign -> DEBU 008 Sign: plaintext: 0ACB070A6708031A0C08C5ADBBD40510...F2757C5EB71EF48CC6D508A3163ADFBA 
2018-02-22 14:42:24.874 UTC [msp/identity] Sign -> DEBU 009 Sign: digest: 5D929FE3C6A637E24840BC3F2B6B715467AC023C84C0F38796C544725A7116F6 
2018-02-22 14:42:24.885 UTC [main] main -> INFO 00a Exiting.....
```
 
Create marble1 on org1/peer0
```
peer chaincode invoke -o orderer.example.com:7050 --tls $CORE_PEER_TLS_ENABLED --cafile /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem -C mychannel -n marblesp -c '{"Args":["initMarble","marble1","blue","35","tom","99"]}'
```
Output
```
2018-02-22 14:43:10.315 UTC [msp] GetLocalMSP -> DEBU 001 Returning existing local MSP
2018-02-22 14:43:10.315 UTC [msp] GetDefaultSigningIdentity -> DEBU 002 Obtaining default signing identity
2018-02-22 14:43:10.323 UTC [chaincodeCmd] checkChaincodeCmdParams -> INFO 003 Using default escc
2018-02-22 14:43:10.323 UTC [chaincodeCmd] checkChaincodeCmdParams -> INFO 004 Using default vscc
2018-02-22 14:43:10.323 UTC [chaincodeCmd] getChaincodeSpec -> DEBU 005 java chaincode enabled
2018-02-22 14:43:10.324 UTC [msp/identity] Sign -> DEBU 006 Sign: plaintext: 0ACF070A6B08031A0C08FEADBBD40510...6C75650A0233350A03746F6D0A023939 
2018-02-22 14:43:10.324 UTC [msp/identity] Sign -> DEBU 007 Sign: digest: F33DE4C4AC71ECB8A0202CD832EEF9D6152C1AB1450E79E62D8FEF60D2D7DDE5 
2018-02-22 14:43:10.359 UTC [msp/identity] Sign -> DEBU 008 Sign: plaintext: 0ACF070A6B08031A0C08FEADBBD40510...040B3D2439ECA30C4F76AC70BAC288D7 
2018-02-22 14:43:10.359 UTC [msp/identity] Sign -> DEBU 009 Sign: digest: 865B4FE9049BC8629BCFE338C6C1E8D2B6C767992FD4E94C073CCEB0DE1E42B8 
2018-02-22 14:43:10.369 UTC [chaincodeCmd] chaincodeInvokeOrQuery -> DEBU 00a ESCC invoke result: version:1 response:<status:200 message:"OK" > payload:"\n >\375\357,o\373w\000y\336\006\337i9Nl\n\342@\t\232dFd\226\364{\007z:\370\240\022\270\003\n\237\003\022\030\n\004lscc\022\020\n\016\n\010marblesp\022\002\010\003\022\202\003\n\010marblesp\032\212\001\n\036collectionMarblePrivateDetails\022F\022D\n ^\037\224o\340q]\3406m\t\350H\013\255vr\2425QW\002\233\r\337\274\225\205\017\004W\016\032 4\ny!\365\244\2416\035\205\331%\214\230\251\342\226(S\363:\306i%\312\255\253\022\020\244\365\313\032 ]\366K8n\032\360f\027~\337om\3372\032\031rj\027\222\247J\024\266\202p\300+bH\315\032\350\001\n\021collectionMarbles\022\260\001\n\"\n ^\037\224o\340q]\3406m\t\350H\013\255vr\2425QW\002\233\r\337\274\225\205\017\004W\016\022D\n k\3313[\356\013\001\244W\210.9\2054\036a\201\010\331&@1F\372\321\253\222{o\305\353A\032 n4\013\234\377\263z\230\234\245D\346\273x\n,x\220\035?\26378v\205\021\243\006\027\257\240\035\022D\n ^\037\224o\340q]\3406m\t\350H\013\255vr\2425QW\002\233\r\337\274\225\205\017\004W\016\032 \310;\310M\371\317\256\024\325o\376\004\334N\227\343\035\2415E\244\365\201\337'U\032\331\306\345\201;\032 ll6;\244\202Fq\333\260\367\355p3g\247\306\320G3\273\016{\\Z\324\252`\273\274\254p\032\003\010\310\001\"\017\022\010marblesp\032\0031.0" endorsement:<endorser:"\n\007Org1MSP\022\256\006-----BEGIN CERTIFICATE-----\nMIICLDCCAdSgAwIBAgIQAWpsSiDKqlH1ajwOFeIgWjAKBggqhkjOPQQDAjBzMQsw\nCQYDVQQGEwJVUzETMBEGA1UECBMKQ2FsaWZvcm5pYTEWMBQGA1UEBxMNU2FuIEZy\nYW5jaXNjbzEZMBcGA1UEChMQb3JnMS5leGFtcGxlLmNvbTEcMBoGA1UEAxMTY2Eu\nb3JnMS5leGFtcGxlLmNvbTAeFw0xODAyMjIxNDM2MDRaFw0yODAyMjAxNDM2MDRa\nMHAxCzAJBgNVBAYTAlVTMRMwEQYDVQQIEwpDYWxpZm9ybmlhMRYwFAYDVQQHEw1T\nYW4gRnJhbmNpc2NvMRMwEQYDVQQLEwpGYWJyaWNQZWVyMR8wHQYDVQQDExZwZWVy\nMC5vcmcxLmV4YW1wbGUuY29tMFkwEwYHKoZIzj0CAQYIKoZIzj0DAQcDQgAE0snV\n8tMMO7Lv6MMQTiHZ56Z+PC+e7cTI+pvS68veSrr+5cHTMsrAdyg6HLlg1TEdbLIt\n9BfcKDy9a0p9Bi3XcaNNMEswDgYDVR0PAQH/BAQDAgeAMAwGA1UdEwEB/wQCMAAw\nKwYDVR0jBCQwIoAggd+YRKciyDHfzZvTpNFeepFi5fXpd9McpGQzkR6qOIAwCgYI\nKoZIzj0EAwIDRgAwQwIgI95GgahPexnTMSMjyv2Osn++C34PBE27JelExP1FRWMC\nHy5y/hvAPOqfjqjYOejc4oQdy4Z3r57MKEKfDwVUt1U=\n-----END CERTIFICATE-----\n" signature:"0D\002 '\226\201\321YyN\364\r\310\333\373=zO\242\330\357\207\254I\236\013!\226h\212\270f\332s\023\002 A\306MB\251h\334\034\253\330\377\254\014z\251\033\004\013=$9\354\243\014Ov\254p\272\302\210\327" > 
2018-02-22 14:43:10.370 UTC [chaincodeCmd] chaincodeInvokeOrQuery -> INFO 00b Chaincode invoke successful. result: status:200 
2018-02-22 14:43:10.370 UTC [main] main -> INFO 00c Exiting.....
```
 
Query marble on org1/peer0
```
peer chaincode query -C mychannel -n marblesp -c '{"Args":["readMarble","marble1"]}'
```
Output
```
2018-02-22 14:43:19.676 UTC [msp] GetLocalMSP -> DEBU 001 Returning existing local MSP
2018-02-22 14:43:19.677 UTC [msp] GetDefaultSigningIdentity -> DEBU 002 Obtaining default signing identity
2018-02-22 14:43:19.677 UTC [chaincodeCmd] checkChaincodeCmdParams -> INFO 003 Using default escc
2018-02-22 14:43:19.677 UTC [chaincodeCmd] checkChaincodeCmdParams -> INFO 004 Using default vscc
2018-02-22 14:43:19.677 UTC [chaincodeCmd] getChaincodeSpec -> DEBU 005 java chaincode enabled
2018-02-22 14:43:19.677 UTC [msp/identity] Sign -> DEBU 006 Sign: plaintext: 0ACF070A6B08031A0C0887AEBBD40510...644D6172626C650A076D6172626C6531 
2018-02-22 14:43:19.677 UTC [msp/identity] Sign -> DEBU 007 Sign: digest: 81A206C815B40C073CD400819F63506EF41FA25FD29DEA10E863AC7722AEB0DA 
Query Result: {"docType":"marble","name":"marble1","color":"blue","size":35,"owner":"tom"}
2018-02-22 14:43:19.682 UTC [main] main -> INFO 008 Exiting.....
```
```
peer chaincode query -C mychannel -n marblesp -c '{"Args":["readMarblePrivateDetails","marble1"]}'
```
Output
```
2018-02-22 14:43:31.072 UTC [msp] GetLocalMSP -> DEBU 001 Returning existing local MSP
2018-02-22 14:43:31.072 UTC [msp] GetDefaultSigningIdentity -> DEBU 002 Obtaining default signing identity
2018-02-22 14:43:31.072 UTC [chaincodeCmd] checkChaincodeCmdParams -> INFO 003 Using default escc
2018-02-22 14:43:31.072 UTC [chaincodeCmd] checkChaincodeCmdParams -> INFO 004 Using default vscc
2018-02-22 14:43:31.072 UTC [chaincodeCmd] getChaincodeSpec -> DEBU 005 java chaincode enabled
2018-02-22 14:43:31.072 UTC [msp/identity] Sign -> DEBU 006 Sign: plaintext: 0ACE070A6A08031A0B0893AEBBD40510...44657461696C730A076D6172626C6531 
2018-02-22 14:43:31.075 UTC [msp/identity] Sign -> DEBU 007 Sign: digest: 899D5FE292121400295D4EACD0193EAB3E6FE667B8310D5612D8B3B7775AA83E 
Query Result: {"docType":"marblePrivateDetails","name":"marble1","price":99}
2018-02-22 14:43:31.080 UTC [main] main -> INFO 008 Exiting.....
```

 
Query marble on org1/peer1
```
CORE_PEER_ADDRESS=peer1.org1.example.com:7051  peer chaincode query -C mychannel -n marblesp -c '{"Args":["readMarblePrivateDetails","marble1"]}'
```

Output
```
2018-02-22 14:44:22.512 UTC [msp] GetLocalMSP -> DEBU 001 Returning existing local MSP
2018-02-22 14:44:22.512 UTC [msp] GetDefaultSigningIdentity -> DEBU 002 Obtaining default signing identity
2018-02-22 14:44:22.512 UTC [chaincodeCmd] checkChaincodeCmdParams -> INFO 003 Using default escc
2018-02-22 14:44:22.512 UTC [chaincodeCmd] checkChaincodeCmdParams -> INFO 004 Using default vscc
2018-02-22 14:44:22.512 UTC [chaincodeCmd] getChaincodeSpec -> DEBU 005 java chaincode enabled
2018-02-22 14:44:22.513 UTC [msp/identity] Sign -> DEBU 006 Sign: plaintext: 0ACF070A6B08031A0C08C6AEBBD40510...44657461696C730A076D6172626C6531 
2018-02-22 14:44:22.513 UTC [msp/identity] Sign -> DEBU 007 Sign: digest: AA50BAAD2473A792407B1FFCD191D7CA0D1CAAFBAE7BE80CEC59D918EB1BB0FB 
Query Result: {"docType":"marblePrivateDetails","name":"marble1","price":99}
2018-02-22 14:44:33.789 UTC [main] main -> INFO 008 Exiting.....
```

 
Query marble on org2/peer0
```
CORE_PEER_ADDRESS=peer0.org2.example.com:7051 CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/server.crt peer chaincode query -C mychannel -n marblesp -c '{"Args":["readMarblePrivateDetails","marble1"]}'
```
Output
```
2018-02-22 14:44:55.054 UTC [msp] GetLocalMSP -> DEBU 001 Returning existing local MSP
2018-02-22 14:44:55.054 UTC [msp] GetDefaultSigningIdentity -> DEBU 002 Obtaining default signing identity
2018-02-22 14:44:55.054 UTC [chaincodeCmd] checkChaincodeCmdParams -> INFO 003 Using default escc
2018-02-22 14:44:55.054 UTC [chaincodeCmd] checkChaincodeCmdParams -> INFO 004 Using default vscc
2018-02-22 14:44:55.054 UTC [chaincodeCmd] getChaincodeSpec -> DEBU 005 java chaincode enabled
2018-02-22 14:44:55.054 UTC [msp/identity] Sign -> DEBU 006 Sign: plaintext: 0ACE070A6A08031A0B08E7AEBBD40510...44657461696C730A076D6172626C6531 
2018-02-22 14:44:55.054 UTC [msp/identity] Sign -> DEBU 007 Sign: digest: B31613F67470C7D9166469E460C7E6C5D48EA85231ACEA15C626E205F135728B 
Error: Error endorsing query: rpc error: code = Unknown desc = chaincode error (status: 500, message: {"Error":"Failed to get private details for marble1: Private data matching public hash version is not available. Public hash version = &version.Height{BlockNum:0x4, TxNum:0x0}, Private data version = (*version.Height)(nil)"}) - <nil>
Usage:
  peer chaincode query [flags]

Flags:
  -C, --channelID string   The channel on which this command should be executed
  -c, --ctor string        Constructor message for the chaincode in JSON format (default "{}")
  -x, --hex                If true, output the query value byte array in hexadecimal. Incompatible with --raw
  -n, --name string        Name of the chaincode
  -r, --raw                If true, output the query value as raw bytes, otherwise format as a printable string
  -t, --tid string         Name of a custom ID generation algorithm (hashing and decoding) e.g. sha256base64

Global Flags:
      --cafile string                       Path to file containing PEM-encoded trusted certificate(s) for the ordering endpoint
      --certfile string                     Path to file containing PEM-encoded X509 public key to use for mutual TLS communication with the orderer endpoint
      --clientauth                          Use mutual TLS when communicating with the orderer endpoint
      --keyfile string                      Path to file containing PEM-encoded private key to use for mutual TLS communication with the orderer endpoint
      --logging-level string                Default logging level and overrides, see core.yaml for full syntax
  -o, --orderer string                      Ordering service endpoint
      --ordererTLSHostnameOverride string   The hostname override to use when validating the TLS connection to the orderer.
      --tls                                 Use TLS when communicating with the orderer endpoint
      --transient string                    Transient map of arguments in JSON encoding
  -v, --version                             Display current version of fabric peer server
```

 
```
docker logs peer0.org1.example.com 2>&1 | grep -i -a -E 'private'
```

Output:
```
2018-02-22 14:41:27.731 UTC [gossip/privdata] listMissingPrivateData -> DEBU 193 Retrieving private write sets for 0 transactions from transient store
2018-02-22 14:41:27.731 UTC [gossip/privdata] StoreBlock -> DEBU 194 No missing collection private write sets to fetch from remote peers
2018-02-22 14:41:31.300 UTC [gossip/privdata] listMissingPrivateData -> DEBU 299 Retrieving private write sets for 0 transactions from transient store
2018-02-22 14:41:31.306 UTC [gossip/privdata] StoreBlock -> DEBU 2a2 No missing collection private write sets to fetch from remote peers
2018-02-22 14:42:26.934 UTC [gossip/privdata] listMissingPrivateData -> DEBU 12ab Retrieving private write sets for 0 transactions from transient store
2018-02-22 14:42:26.934 UTC [gossip/privdata] StoreBlock -> DEBU 12ad No missing collection private write sets to fetch from remote peers
2018-02-22 14:43:10.335 UTC [gossip/comm] func1 -> DEBU 1ef9 Got message: GossipMessage: nonce:15240159861497212547 channel:"mychannel" tag:CHAN_ONLY private_data:<payload:<collection_name:"collectionMarbles" namespace:"marblesp" tx_id:"c3a385d75bf1f170761376bf63df8b5b29fd237ca75c323ccb93468b24ed25a6" private_rwset:"\032\036\n\031\000color~name\000blue\000marble1\000\032\001\000\032W\n\007marble1\032L{\"docType\":\"marble\",\"name\":\"marble1\",\"color\":\"blue\",\"size\":35,\"owner\":\"tom\"}" > > , Envelope: 249 bytes, Signature: 0 bytes
2018-02-22 14:43:10.336 UTC [gossip/state] listen -> DEBU 1efa Dispatching a message GossipMessage: nonce:15240159861497212547 channel:"mychannel" tag:CHAN_ONLY private_data:<payload:<collection_name:"collectionMarbles" namespace:"marblesp" tx_id:"c3a385d75bf1f170761376bf63df8b5b29fd237ca75c323ccb93468b24ed25a6" private_rwset:"\032\036\n\031\000color~name\000blue\000marble1\000\032\001\000\032W\n\007marble1\032L{\"docType\":\"marble\",\"name\":\"marble1\",\"color\":\"blue\",\"size\":35,\"owner\":\"tom\"}" > > , Envelope: 249 bytes, Signature: 0 bytes
2018-02-22 14:43:10.336 UTC [gossip/state] dispatch -> DEBU 1efb Handling private data collection message
2018-02-22 14:43:10.350 UTC [gossip/state] privateDataMessage -> DEBU 1efc Private data for collection collectionMarbles has been stored
2018-02-22 14:43:12.413 UTC [gossip/privdata] isEligible -> DEBU 1f76 Skipping namespace marblesp collection collectionMarblePrivateDetails because we're not eligible for the private data
2018-02-22 14:43:12.414 UTC [gossip/privdata] listMissingPrivateData -> DEBU 1f77 Retrieving private write sets for 1 transactions from transient store
2018-02-22 14:43:12.415 UTC [gossip/privdata] StoreBlock -> DEBU 1f7b No missing collection private write sets to fetch from remote peers
2018-02-22 14:43:12.415 UTC [gossip/privdata] StoreBlock -> DEBU 1f7c Added 1 namespace private write sets for block [4], tran [0]
2018-02-22 14:45:06.673 UTC [chaincode] func1 -> ERRO 3f37 [c4cb2aff]Failed to get chaincode state(Private data matching public hash version is not available. Public hash version = &version.Height{BlockNum:0x4, TxNum:0x0}, Private data version = (*version.Height)(nil)). Sending ERROR
```