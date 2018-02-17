---
layout: post
title: "SAP: Generating GS1 XML documents"
subtitle: ".... they still use XML?"
date: 2018-02-15
categories: sap
status: publish
---

*Generating XMLs to communicate with external systems is critical in the enterprise. I'd love to share a method I discovered in the last few days.* 

Generating XML files in SAP ABAP is fairly straight forward. Then further the `xstring -> string` conversion 
isn't too bad either. Once we are done with this process we will be able to send an http request with the XML in the request's body. This will be in the next SAP post.    

Reference:   
[SAP: Create XML File](https://wiki.scn.sap.com/wiki/display/ABAP/iXML+-+Create+XML+file)


```
**********************************************************************
***  Build Master Data Item GS1 XML **********************************
**********************************************************************
* Sample XML 
*<?xml version="1.0" encoding="UTF-8"?>
*<item_data_notification:itemDataNotificationMessage 
*    xmlns:item_data_notification="urn:gs1:ecom:item_data_notification:xsd:3" 
*    xmlns:sh="http://www.unece.org/cefact/namespaces/StandardBusinessDocumentHeader" 
*    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
*    xsi:schemaLocation="urn:gs1:ecom:item_data_notification:xsd:3 ../Schemas/gs1/ecom/ItemDataNotification.xsd">
*    <sh:StandardBusinessDocumentHeader>
*        <sh:HeaderVersion>1.0</sh:HeaderVersion>
*        <sh:Sender>
*            <sh:Identifier Authority="GS1">4098765000010</sh:Identifier>
*            <sh:ContactInformation>
*                <sh:Contact>John Doe</sh:Contact>
*            </sh:ContactInformation>
*        </sh:Sender>
*        <sh:Receiver>
*            <sh:Identifier Authority="GS1">5412345000013</sh:Identifier>
*            <sh:ContactInformation>
*                <sh:Contact>Mary Smith</sh:Contact>
*            </sh:ContactInformation>
*        </sh:Receiver>
*        <sh:DocumentIdentification>
*            <sh:Standard>GS1</sh:Standard>
*            <sh:TypeVersion>3.2</sh:TypeVersion>
*            <sh:InstanceIdentifier>ID37788</sh:InstanceIdentifier>
*            <sh:Type>Item Data Notification</sh:Type>
*            <sh:MultipleType>false</sh:MultipleType>
*            <sh:CreationDateAndTime>2004-05-24T18:12:00.000-05:00</sh:CreationDateAndTime>
*        </sh:DocumentIdentification>
*    </sh:StandardBusinessDocumentHeader>
*    <tradeItemData>
*        <tradeItemDescription languageCode="en">Ingredient ABC</tradeItemDescription>
*        <gtin>10614141073464</gtin>
*        <sku>5512123221</sku>
*        <dataSource>
*            <gln>4098765000010</gln>
*        </dataSource>
*        <dataRecipient>
*            <gln>5412345000013</gln>
*        </dataRecipient>
*    </tradeItemData>
*</item_data_notification:itemDataNotificationMessage>


DATA:    
         lo_document TYPE REF TO if_ixml_document, 
         lo_item_root  TYPE REF TO if_ixml_element,
         lo_employee TYPE REF TO if_ixml_element,
         lo_tradeItem TYPE REF TO if_ixml_element,
         lo_trade_item_gtin TYPE REF TO if_ixml_element,
         lo_trade_item_sku TYPE REF TO if_ixml_element,
         lo_trade_item_data_source TYPE REF TO if_ixml_element,
         lo_trade_item_desc TYPE REF TO if_ixml_element,
         lo_trade_sender TYPE REF TO if_ixml_element,
         lo_trade_receiver TYPE REF TO if_ixml_element,
         lo_header TYPE REF TO if_ixml_element,
         temp TYPE REF TO if_ixml_element.
         
lo_document = lo_ixml->create_document( ).
lo_item_root  = lo_document->create_simple_element_ns(
                                    name    = 'item_data_notification:itemDataNotificationMessage'
                                    parent  = lo_document ).

lo_item_root->set_attribute_ns( name = 'xmlns:item_data_notification' value = 'urn:gs1:ecom:item_data_notification:xsd:3' ).
lo_item_root->set_attribute_ns( name = 'xmlns:sh' value = 'http://www.unece.org/cefact/namespaces/StandardBusinessDocumentHeader' ).
lo_item_root->set_attribute_ns( name = 'xmlns:xsi' value = 'http://www.w3.org/2001/XMLSchema-instance' ).
lo_item_root->set_attribute_ns( name = 'xsi:schemaLocation' value = 'urn:gs1:ecom:item_data_notification:xsd:3 ../Schemas/gs1/ecom/ItemDataNotification.xsd' ).

lo_header = lo_document->create_simple_element_ns(
                                    name    = 'sh:StandardBusinessDocumentHeader'
                                    parent  = lo_item_root ).

lo_document->create_simple_element_ns( name    = 'sh:HeaderVersion'
                                       parent  = lo_header
                                       value = '1.0' ).

lo_trade_sender = lo_document->create_simple_element_ns(
                             name    = 'sh:Sender'
                             parent  = lo_header ).

temp = lo_document->create_simple_element_ns(
                       name    = 'sh:Identifier'
                       parent  = lo_trade_sender
                       value = '0030000000359' ).
temp->set_attribute_ns( name = 'Authority' value = 'GS1' ).

lo_trade_sender = lo_document->create_simple_element_ns(
                       name    = 'sh:ContactInformation'
                       parent  = lo_trade_sender ).

lo_document->create_simple_element_ns(
                       name    = 'sh:Contact'
                       parent  = lo_trade_sender
                       value = 'Joe Smith' ).

lo_trade_receiver = lo_document->create_simple_element_ns(
                       name    = 'sh:Receiver'
                       parent  = lo_header ).
temp = lo_document->create_simple_element_ns(
                       name    = 'sh:Identifier'
                       parent  = lo_trade_receiver
                       value = '0030000000359' ).
temp->set_attribute_ns( name = 'Authority' value = 'GS1' ).
lo_trade_receiver = lo_document->create_simple_element_ns(
                       name    = 'sh:ContactInformation'
                       parent  = lo_trade_receiver ).
lo_document->create_simple_element_ns(
                       name    = 'sh:Contact'
                       parent  = lo_trade_receiver
                       value = 'Mary Smith' ).
                       
temp = lo_document->create_simple_element_ns( name = 'sh:DocumentIdentification'  parent = lo_header ).
lo_document->create_simple_element_ns( name = 'sh:Standard'  parent = temp  value = 'GS1' ).
lo_document->create_simple_element_ns( name = 'sh:TypeVersion' parent = temp value = '3.2' ).
lo_document->create_simple_element_ns( name = 'sh:InstanceIdentifier' parent = temp  value = 'ID37788' ).
lo_document->create_simple_element_ns( name = 'sh:Type' parent = temp  value = 'Item Data Notification' ).
lo_document->create_simple_element_ns( name = 'sh:MultipleType' parent = temp  value = 'false' ).
lo_document->create_simple_element_ns( name = 'sh:CreationDateAndTime' parent = temp  value = '2004-05-24T18:12:00.000-05:00' ).

lo_tradeItem = lo_document->create_simple_element_ns(
                                    name    = 'tradeItemData'
                                    parent  = lo_item_root ).
lo_trade_item_desc = lo_document->create_simple_element_ns(
                                    name    = 'tradeItemDescription'
                                    parent  = lo_tradeItem
                                    value = item-descr ).
lo_trade_item_desc->set_attribute_ns( name = 'languageCode' value = 'en' ).
lo_trade_item_gtin = lo_document->create_simple_element_ns(
                                    name    = 'gtin'
                                    parent  = lo_tradeItem
                                    value = item-gtin ).
lo_trade_item_sku = lo_document->create_simple_element_ns(
                                    name    = 'sku'
                                    parent  = lo_tradeItem
                                    value = item-sku ).
lo_trade_item_data_source = lo_document->create_simple_element_ns(
                                    name    = 'dataSource'
                                    parent  = lo_tradeItem ).
lo_document->create_simple_element_ns(
                                    name    = 'gln'
                                    parent  = lo_trade_item_data_source
                                    value   = item-gln_datasource ).
temp = lo_document->create_simple_element_ns(
                                    name    = 'dataRecipient'
                                    parent  = lo_tradeItem ).
lo_document->create_simple_element_ns(
                                    name    = 'gln'
                                    parent  = temp
                                    value   = item-gln_recipient ).
```

Now once we have the `lo_document` reference object we can use
it to render an XML file or `xstring`. 


Using the following: 

```
DATA:
      lo_document TYPE REF TO  if_ixml_document,
      lo_streamfactory  TYPE REF TO if_ixml_stream_factory,
      lo_ostream        TYPE REF TO if_ixml_ostream,
      lo_renderer       TYPE REF TO if_ixml_renderer,
      lv_rc             TYPE i,
      lv_xml_size       TYPE i,
      lo_ixml TYPE REF TO if_ixml.

* Create iXML object
lo_ixml = cl_ixml=>create( ).

* Create Stream Factory
lo_streamfactory = lo_ixml->create_stream_factory( ).
* Crate Output Stream
lo_ostream  = lo_streamfactory->create_ostream_uri( system_id = p_fname ).
* Craete renderer
lo_renderer = lo_ixml->create_renderer( ostream  = lo_ostream
                                        document = lo_document ).
* Set Pretty Print
lo_ostream->set_pretty_print( 'X' ).
* Render
lv_rc = lo_renderer->render( ).
* Get XML file size
lv_xml_size = lo_ostream->get_num_written_raw( ).
* Display Output
WRITE : 'XML File: ', p_fname, lv_xml_size,  'Bytes'.
```

But maybe we are trying to send the XML document over REST and we'd love to store it in an `xstring`

```
DATA:
      xstr TYPE string, 
      lo_document       TYPE REF TO if_ixml_document, 
      lo_streamfactory  TYPE REF TO if_ixml_stream_factory,
      lo_ostream        TYPE REF TO if_ixml_ostream,
      lo_renderer       TYPE REF TO if_ixml_renderer,
      lv_rc             TYPE i,
      lv_xml_size       TYPE i.
      
* Create Stream Factory
lo_streamfactory = lo_ixml->create_stream_factory( ).
* Crate Output Stream
lo_ostream  = lo_streamfactory->create_ostream_xstring( string = xstr ).
* Craete renderer
lo_renderer = lo_ixml->create_renderer( ostream  = lo_ostream
                                        document = lo_document ).
* Set Pretty Print
lo_ostream->set_pretty_print( 'X' ).
* Render
lv_strrc = lo_renderer->render( ).
l_xml = cl_proxy_service=>xstring2cstring( xstr ).
WRITE l_xml.
```

Run the function
![run_xml_gen](/static/sap/run_xml_gen.png)
String output displayed
![run_xml_gen](/static/sap/lxml_write.png)

Then the generated XML file can be found using AL11
![run_xml_gen](/static/sap/xml_saved.png) 
