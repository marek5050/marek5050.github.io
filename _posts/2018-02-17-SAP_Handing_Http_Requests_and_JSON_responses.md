---
layout: post
title: "SAP: Handling HTTP requests and JSON responses"
subtitle: "...it's so easy in other languages..."
date: 2018-02-17
categories: sap
status: publish
---


*The last few weeks I've been involved with GS1 standards and SAP. I've put together a list of interesting problems and experiences.*


Reference:   
* [Hands on Call Watson from SAP Powerpoint](https://www.ibm.com/developerworks/community/files/form/anonymous/api/library/25ecde0d-ebfb-47d4-a379-a048a1ccea57/document/d7c30a1b-da62-4066-b4b5-aedc92dfb139/media/Hands-on_Call_Watson_from_SAP_20171009.pdf)
* [One more JSON Serializer/Deserializer](https://wiki.scn.sap.com/wiki/display/Snippets/One+more+ABAP+to+JSON+Serializer+and+Deserializer)
* [Three different ways to serialize and deserialize complex ABAP data](https://blogs.sap.com/2013/02/21/three-different-ways-to-serialize-and-deserialize-complex-abap-data/)

When calling the IBM Cloud identity service we receive a JSON in this form: 
```
{
    "success": 200,
    "id_token": "eyJhbGciwPzsA"
}
```

There are a few options in SAP ABAP to parse this json document. 
Using `cl_trex_json_deserializer` or `/ui2/cl_json`.

One thing I noticed is that `cl_trex_json_deserializer` only deserializes Javascript objects not necessarily JSON. 
What is the difference you may ask :D - In JSON keys have to be strings and that means
the keys will be surrounded with quotes `"success":`. Javascript objects on the other
hand can have almost anything as a key and don't have to be surrounded by quotes.
There's a few sources for this difference [Stackoverflow for example](https://stackoverflow.com/questions/949449/json-spec-does-the-key-have-to-be-surrounded-with-quotes). 

```
From 2.2. Objects

    An object structure is represented as a pair of curly brackets surrounding zero or more name/value pairs (or members). A name is a string.

and from 2.5. Strings

    A string begins and ends with quotation marks.

So I would say that according to the standard: yes, you should always quote the key (although some parsers may be more forgiving)
```  

So this means `cl_trex_json_deserializer` will when used on the above JSON return.


So we'll need to use `/ui2/cl_json`. 


For this we'll also need to create a `type` that has a similar structure. IT'll contain
`success` and `id_token`. 

```
TYPES:
        begin of ty_stru,
              success  TYPE i,
              id_token  TYPE string,
          end of ty_stru.
DATA:
   out TYPE ty_stru 
```

Now we are able to use `/ui2/cl_json` to deserialize the json into the structure. 

```
DATA: result TYPE string VALUE '{"success": 200, "id_token": "eyJhbGciwPzsA"}' . 
  
/ui2/cl_json=>deserialize( EXPORTING json = response 
                           pretty_name = /ui2/cl_json=>pretty_mode-camel_case 
                           CHANGING data = out ).
WRITE out-id_token .
```

For reference here's the full working code:

```
FORM GETTOKEN .
TYPES:
        begin of ty_stru,
              success  TYPE i,
              id_token TYPE string,
          end of ty_stru.
DATA:
   out TYPE ty_stru ,
   url TYPE STRING VALUE 'https://identity_service.mybluemix.net/identity/v0/token' ,
   userid TYPE STRING VALUE '$userid' ,
   clientid TYPE STRING VALUE '$clientid' ,
   secret TYPE STRING VALUE '$client_secret' ,
   token_payload TYPE string ,
   http_client TYPE REF TO if_http_client ,
   response TYPE STRING .

concatenate '{"grant_type": "password", "username": "' userid '","password": "' secret '", "client_id":"' clientid '"}' into token_payload .

cl_http_client=>create_by_url( EXPORTING url = url IMPORTING client = http_client ).

IF SY-SUBRC = 0.
    http_client->request->set_method( if_http_request=>co_request_method_post ).
ENDIF.
IF SY-SUBRC = 0.
    http_client->request->set_content_type( 'application/json' ).
ENDIF.

http_client->request->set_cdata( token_payload ).
http_client->propertytype_logon_popup = if_http_client=>co_disabled.
http_client->send( ).

TRY.
  http_client->receive( ).
CATCH cx_root.
  WRITE 'error' .
ENDTRY.
CREATE OBJECT lo_json_deserializer.
response = http_client->response->get_cdata( ).
WRITE response.

/ui2/cl_json=>deserialize( EXPORTING json = response pretty_name = /ui2/cl_json=>pretty_mode-camel_case CHANGING data = out ).
WRITE l_xml .
WRITE out-id_token .

ENDFORM.

```