---
layout: post
title: "SAP: API HTTP requests to IBM Cloud"
subtitle: "... and dealing with self-signed certificates..."
date: 2018-02-16
categories: sap
status: publish
---


*In the last few weeks I've come across a couple of interesting SAP ABAP challenges. For example sending a simple HTTP Post request to a IBM Cloud service.
 This was more difficult than expected, because of the self-signed certificate bluemix uses.*


Reference:   
* [Hands on Call Watson from SAP Powerpoint](https://www.ibm.com/developerworks/community/files/form/anonymous/api/library/25ecde0d-ebfb-47d4-a379-a048a1ccea57/document/d7c30a1b-da62-4066-b4b5-aedc92dfb139/media/Hands-on_Call_Watson_from_SAP_20171009.pdf)
* [Arbitrary IBM Cloud subdomain where we can get a *.bluemix.net token](https://drink.bluemix.net/)


```
* xstring will be the XML we generated in the previous guide. 
* token will be a oauth access token we want to use for authentication

FORM SENDXML USING value(xstr) TYPE xstring
                   value(token) TYPE string .
DATA:
    l_xml TYPE string ,
    l_xml = cl_proxy_service=>xstring2cstring( xstr ) ,
    host        TYPE STRING VALUE 'https://api.mybluemix.net' ,
    url         TYPE STRING VALUE  'https://api.mybluemix.net/v1/assets' ,
    apikey          TYPE string ,
    http_client     TYPE REF TO if_http_client ,
    response        TYPE STRING .
    
concatenate 'Bearer ' token into apikey RESPECTING BLANKS.
* Token should be in the Bearer .....key... format 

cl_http_client=>create_by_url( EXPORTING url = url IMPORTING client = http_client ).
IF SY-SUBRC = 0.
    http_client->request->set_method( if_http_request=>co_request_method_post ).
ENDIF.
IF SY-SUBRC = 0.
    http_client->request->set_content_type( 'application/xml' ).
ENDIF.
IF SY-SUBRC = 0 .
    http_client->request->set_header_field( EXPORTING name = 'Authorization' value = apikey ).
ENDIF.
http_client->request->set_cdata( l_xml ).
http_client->propertytype_logon_popup = if_http_client=>co_disabled.
http_client->send( ).
TRY.
  http_client->receive( ).
CATCH cx_root.
  WRITE 'error' .
ENDTRY.
response = http_client->response->get_cdata( ).
WRITE response.
ENDFORM.
```

Okay, this will most likely result in a failing API call if we are hitting a server on the IBM Cloud. This is because most
of the service that run on https fall under an umbrella self-signed certificate. Since it's an IBM Cloud certificate that encompasses
all domains we can go to for example https://wow.bluemix.net to download it. 

There will be 2 types of errors depending how deep we look. On the top level it'll be a
`ABAP programming error` `HTTP_COMMUNICATION_FAILURE` as can be seen below.      

![api_error](/static/sap/api_error.png){:width='400px'}

If we do some serious troubleshooting we'll find the below error. 
`SSL handshake with` `failed: SSSLERR_PEER_CERT_UNTRUSTED(-102)` `#The peer's X.509 Certificate (chain) is untrusted ## SapSSLSessionStartNB()....`
  
![api_error](/static/sap/api_dig_deeper.png){:width='400px'}

The way to bypass this is to add the *.bluemix.net certificate into `STRUST`.


To download the `*.bluemix.net` certificate we can navigate Google Chrome, Firefox, or any other browser to 
any Bluemix page. For example the above https://wow.bluemix.net will work. If in Chrome
we can click on the `Secure` button next to the URL. A box will pop up, then click on Valid, finally we can drag and drop the certificate
onto our Desktop.

For reference see the following gif or the [Presentation](https://www.ibm.com/developerworks/community/files/form/anonymous/api/library/25ecde0d-ebfb-47d4-a379-a048a1ccea57/document/d7c30a1b-da62-4066-b4b5-aedc92dfb139/media/Hands-on_Call_Watson_from_SAP_20171009.pdf)   

![Saving the * wildcard bluemix certificate](/static/sap/save_bluemix_cert.gif){:width='600px'}

We can follow the linked presentation for loading the certificate into `STRUST`. 
 
### Install Certificate in the SAP System
Proceed as follows to install the exported SSL certificate in your SAP system.
1. In SAP, call transaction STRUST.
2. Switch to edit mode (press according tool bar icon).
3. If a local PSE file does not exist already, create it by right-clicking on SSL client SSL Client (Standard) and selecting Create from context menu. Keep all default settings in next popup dialog.  
![strust ssl client](/static/sap/strust_1.png){:width='300px'}
4. In Certificate section, click Import (alternatively select menu item Certificate → Import). Choose file watsonplatformBase64X509.cer and import the certificate.
5. Click Add to Certificate List.  
![strust add certificate](/static/sap/strust_2.png){:width='500px'}
6. Click Save (F3).


### Done
We should be able to send the data without issues. Let's go to `SE80`
![strust add certificate](/static/sap/api_success.png)
