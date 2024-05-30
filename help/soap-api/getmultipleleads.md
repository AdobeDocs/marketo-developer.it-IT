---
title: "getMultipleLeads"
feature: SOAP
description: "chiamate SOAP getMultipleLeads"
source-git-commit: d335bdd9f939c3e557a557b43fb3f33934e13fef
workflow-type: tm+mt
source-wordcount: '384'
ht-degree: 2%

---


# getMultipleLeads

Mi piace `getLead`, `getMultipleLeads` recupera i record dei lead da Marketo. Invece dei dati per un singolo lead, questa chiamata restituisce i dati per un batch di lead che corrispondono ai criteri passati nel parametro leadSelector. I criteri possono essere un intervallo di date, ad esempio l’ultima data aggiornata, una matrice di chiavi principali o un elenco statico.

Nota: se si utilizza una matrice di chiavi lead, il limite massimo è 100 per batch; le chiavi aggiuntive verranno ignorate.

Se è necessario solo un sottoinsieme dei campi del lead, il `includeAttributes` Il parametro deve essere utilizzato per specificare i campi desiderati.

Ogni `getMultipleLeads` la chiamata di funzione restituisce fino a 1000 lead. Se devi recuperare più di 1000 lead, il risultato sarà un [posizione flusso](stream-position.md), che può essere utilizzato nelle chiamate successive per recuperare il batch successivo di 1000 lead. Il conteggio rimanente nel risultato indica esattamente quanti lead rimangono. Durante il recupero da un elenco statico, la condizione di terminazione è remainingCount == 0.

Un caso d’uso comune per questo endpoint è la ricerca di lead aggiornati in date specifiche. Il `LastUpdateAtSelector` consente di eseguire questa operazione.

## Richiesta

| Nome campo | Obbligatorio/facoltativo | Descrizione |
| --- | --- | --- |
| leadSelector | Obbligatorio | Può essere uno dei seguenti 3 tipi:`LeadKeySelector`, `StaticListSelector`,`LastUpdateAtSelector` |
| keyType | Obbligatorio | Il tipo di ID su cui desideri eseguire la query. I valori includono IDNUM, COOKIE, EMAIL, LEADOWNEREMAIL, SFDCACCOUNTID, SFDCCONTACTID, SFDCLEADID, SFDCLEADOWNERID, SFDCOPPTYID. |
| keyValues->stringItem | Obbligatorio | Elenco dei valori chiave. Ovvero &quot;lead@email.com&quot; |
| LastUpdateAtSelector: leadSelector->olderUpdatedAt | Obbligatorio | La marca temporale per specificare i criteri &quot;da&quot;. In altre parole, restituisce tutti i lead aggiornati dall&#39;ora specificata. (Formato data/ora WSDL W3C) |
| LastUpdateAtSelector: leadSelector->latestUpdatedAt | Facoltativo | Il timestamp per specificare i criteri &quot;fino a&quot;. In altre parole, restituire tutti i lead aggiornati fino all&#39;ora specificata. (Formato data/ora WSDL W3C) |
| StaticListSelector: leadSelector->staticListName | Facoltativo quando `leadSelector->staticListId` è presente | Nome dell&#39;elenco statico |
| StaticListSelector: leadSelector->staticListId | Facoltativo quando `leadSelector->staticListName` è presente | ID dell’elenco statico |
| lastUpdatedAt | **Obsoleto** | Utilizzare `LastUpdateAtSelector` invece |
| includeAttributes | Facoltativo | Elenco di attributi che desideri recuperare. Limitare i campi lead restituiti può migliorare il tempo di risposta dell’API. |
| batchSize | Facoltativo | Numero massimo di record da restituire. Limite di sistema a 100 o `batchSize`, se inferiore |
| streamPosition | Facoltativo | Utilizzato per impaginare attraverso un numero elevato di risposte lead. Il `streamPosition` il valore viene restituito dal campo risposta chiamate precedente `newStreamPosition` |

## Richiedi XML

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/" xmlns:ns1="http://www.marketo.com/mktows/" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <SOAP-ENV:Header>
    <ns1:AuthenticationHeader>
      <mktowsUserId>demo17_1_809934544BFABAE58E5D27</mktowsUserId>
      <requestSignature>146ec93f4f1e2a45f7a545f7e42e2d053c0457e7</requestSignature>
      <requestTimestamp>2013-08-02T15:47:06-07:00</requestTimestamp>
    </ns1:AuthenticationHeader>
  </SOAP-ENV:Header>
  <SOAP-ENV:Body>
    <ns1:paramsGetMultipleLeads>
      <leadSelector xsi:type="ns1:LeadKeySelector">
        <keyType>EMAIL</keyType>
        <keyValues>
          <stringItem>formtest1@marketo.com</stringItem>
          <stringItem>joe@marketo.com</stringItem>
        </keyValues>
      </leadSelector>
      <batchSize>100</batchSize>
    </ns1:paramsGetMultipleLeads>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

## XML risposta

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:ns1="http://www.marketo.com/mktows/">
  <SOAP-ENV:Body>
    <ns1:successGetMultipleLeads>
      <result>
        <returnCount>2</returnCount>
        <remainingCount>0</remainingCount>
        <newStreamPosition />
        <leadRecordList>
          <leadRecord>
            <Id>65835</Id>
            <Email>formtest1@marketo.com</Email>
            <ForeignSysPersonId xsi:nil="true" />
            <ForeignSysType xsi:nil="true" />
            <leadAttributeList>
              <attribute>
                <attrName>Company</attrName>
                <attrType>string</attrType>
                <attrValue>Marketo Test</attrValue>
              </attribute>
              <attribute>
                <attrName>FirstName</attrName>
                <attrType>string</attrType>
                <attrValue>FnameForm1</attrValue>
              </attribute>
              <attribute>
                <attrName>LastName</attrName>
                <attrType>string</attrType>
                <attrValue>LnameForm1</attrValue>
              </attribute>
              <attribute>
                <attrName>LeadScore</attrName>
                <attrType>integer</attrType>
                <attrValue>16</attrValue>
              </attribute>
              <attribute>
                <attrName>Website</attrName>
                <attrType>url</attrType>
                <attrValue>marketo.com</attrValue>
              </attribute>
            </leadAttributeList>
          </leadRecord>
          <leadRecord>
            <Id>67508</Id>
            <Email>joe@marketo.com</Email>
            <ForeignSysPersonId xsi:nil="true" />
            <ForeignSysType xsi:nil="true" />
            <leadAttributeList>
              <attribute>
                <attrName>Company</attrName>
                <attrType>string</attrType>
                <attrValue>Marketo</attrValue>
              </attribute>
              <attribute>
                <attrName>Date_of_purchase</attrName>
                <attrType>date</attrType>
                <attrValue>2012-04-04</attrValue>
              </attribute>
              <attribute>
                <attrName>FirstName</attrName>
                <attrType>string</attrType>
                <attrValue>Joe</attrValue>
              </attribute>
              <attribute>
                <attrName>LastName</attrName>
                <attrType>string</attrType>
                <attrValue>Orante</attrValue>
              </attribute>
              <attribute>
                <attrName>LeadScore</attrName>
                <attrType>integer</attrType>
                <attrValue>11</attrValue>
              </attribute>
            </leadAttributeList>
          </leadRecord>
        </leadRecordList>
      </result>
    </ns1:successGetMultipleLeads>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

## Codice di esempio - PHP

```php
<?php
$debug = true;
$marketoSoapEndPoint    = "";  // CHANGE ME
$marketoUserId      = "";  // CHANGE ME
$marketoSecretKey   = "";  // CHANGE ME
$marketoNameSpace   = "http://www.marketo.com/mktows/";
 
// Create Signature
$dtzObj = new DateTimeZone("America/Los_Angeles");
$dtObj  = new DateTime('now', $dtzObj);
$timeStamp = $dtObj->format(DATE_W3C);
$encryptString = $timeStamp . $marketoUserId;
$signature = hash_hmac('sha1', $encryptString, $marketoSecretKey);
 
// Create SOAP Header
$attrs = new stdClass();
$attrs->mktowsUserId = $marketoUserId;
$attrs->requestSignature = $signature;
$attrs->requestTimestamp = $timeStamp;
$authHdr = new SoapHeader($marketoNameSpace, 'AuthenticationHeader', $attrs);
$options = array("connection_timeout" => 15, "location" => $marketoSoapEndPoint);
if ($debug) {
  $options["trace"] = 1;
}
 
// Create Request
/*  
//Query by Email
$leadSel = new stdClass();
$leadSel->keyType = 'EMAIL';
$keyValues = array("formtest1@marketo.com", "joe@marketo.com");
$leadKeys = new stdClass();
$leadKeys->stringItem = $keyValues;
$leadSel->keyValues = $leadKeys;
$leadSelSoap = new stdClass();
$leadSelSoap = array("leadSelector" => $leadSel);
// $leadSelParams = array("leadSelector" => $leadSelSoap, "batchSize" => 10, "streamPosition" => $startPosition);
// $params = array("paramsGetMultipleLeads" => $leadSelParams);
$leadSelSoap = new SoapVar($leadSel, SOAP_ENC_OBJECT, "LeadKeySelector", "http://www.marketo.com/mktows/");
*/
 
/*
// Query by Update time
$leadSel = new stdClass();
$leadSel->latestUpdatedAt = "2013-08-06T15:45:00-07:00";
$leadSel->oldestUpdatedAt = "2013-08-06T12:00:00-07:00";
$leadSelSoap = new stdClass();
$leadSelSoap = array("leadSelector" => $leadSel);
$leadSelSoap = new SoapVar($leadSel, SOAP_ENC_OBJECT, "LastUpdateAtSelector", "http://www.marketo.com/mktows/");
*/
 
// Query from a Static List
$leadSel = new stdClass();
//ProgramName.ListName
$leadSel->staticListName = "SMSProgram.listForTesting";
$leadSelSoap = new stdClass();
$leadSelSoap = array("leadSelector" => $leadSel);
$leadSelSoap = new SoapVar($leadSel, SOAP_ENC_OBJECT, "StaticListSelector", "http://www.marketo.com/mktows/");
 
$params->leadSelector = $leadSelSoap;
$params->streamPosition = $startPosition;
$params->batchSize = 100;
$soapClient = new SoapClient($marketoSoapEndPoint ."?WSDL", $options);
try {
  $leads = $soapClient->__soapCall('getMultipleLeads', array($params), $options, $authHdr);
}
catch(Exception $ex) {
  var_dump($ex);
}
if ($debug) {
  print "RAW request:\n" .$soapClient->__getLastRequest() ."\n";
  print "RAW response:\n" .$soapClient->__getLastResponse() ."\n";
}
?>
```

## Codice di esempio - Java

```java
import com.marketo.mktows.*;
 
import java.net.URL;
import javax.xml.namespace.QName;
import java.text.DateFormat;
import java.text.SimpleDateFormat;
import java.util.Date;
import java.util.GregorianCalendar;
 
import javax.crypto.Mac;
import javax.crypto.spec.SecretKeySpec;
import org.apache.commons.codec.binary.Hex;
import javax.xml.bind.JAXBContext;
import javax.xml.bind.JAXBElement;
import javax.xml.bind.Marshaller;
import javax.xml.datatype.DatatypeFactory;
import javax.xml.datatype.XMLGregorianCalendar;
 
 
public class GetMultipleLeads {
 
    public static void main(String[] args) {
        System.out.println("Executing GetMultipleLeads");
        try {
            URL marketoSoapEndPoint = new URL("CHANGE ME" + "?WSDL");
            String marketoUserId = "CHANGE ME";
            String marketoSecretKey = "CHANGE ME";
             
            QName serviceName = new QName("http://www.marketo.com/mktows/", "MktMktowsApiService");
            MktMktowsApiService service = new MktMktowsApiService(marketoSoapEndPoint, serviceName);
            MktowsPort port = service.getMktowsApiSoapPort();
             
            // Create Signature
            DateFormat df = new SimpleDateFormat("yyyy-MM-dd'T'HH:mm:ssZ");
            String text = df.format(new Date());
            String requestTimestamp = text.substring(0, 22) + ":" + text.substring(22);           
            String encryptString = requestTimestamp + marketoUserId ;
             
            SecretKeySpec secretKey = new SecretKeySpec(marketoSecretKey.getBytes(), "HmacSHA1");
            Mac mac = Mac.getInstance("HmacSHA1");
            mac.init(secretKey);
            byte[] rawHmac = mac.doFinal(encryptString.getBytes());
            char[] hexChars = Hex.encodeHex(rawHmac);
            String signature = new String(hexChars); 
             
            // Set Authentication Header
            AuthenticationHeader header = new AuthenticationHeader();
            header.setMktowsUserId(marketoUserId);
            header.setRequestTimestamp(requestTimestamp);
            header.setRequestSignature(signature);
             
            // Create Request
            ParamsGetMultipleLeads request = new ParamsGetMultipleLeads();
             
            // Request Using LeadKey Selector
            ////////////////////////////////////////////////////////
            LeadKeySelector keySelector = new LeadKeySelector();
            keySelector.setKeyType(LeadKeyRef.EMAIL);
             
            ArrayOfString aos = new ArrayOfString();
            aos.getStringItems().add("formtest1@marketo.com");
            aos.getStringItems().add("joe@marketo.com");
            keySelector.setKeyValues(aos);
            request.setLeadSelector(keySelector);
 
            /*
            // Request Using LastUpdateAtSelector
            ////////////////////////////////////////////////////////
            LastUpdateAtSelector leadSelector = new LastUpdateAtSelector();
             
            GregorianCalendar gc = new GregorianCalendar();
            gc.setTimeInMillis(new Date().getTime());
            gc.add( GregorianCalendar.DAY_OF_YEAR, -2);
             
            DatatypeFactory factory = DatatypeFactory.newInstance();
 
            ObjectFactory objectFactory = new ObjectFactory();
            JAXBElement<XMLGregorianCalendar> until =objectFactory.createLastUpdateAtSelectorLatestUpdatedAt(factory.newXMLGregorianCalendar(gc));            
             
            GregorianCalendar since = new GregorianCalendar();
            since.setTimeInMillis(new Date().getTime());
            since.add( GregorianCalendar.DAY_OF_YEAR, -5);
             
            leadSelector.setOldestUpdatedAt(factory.newXMLGregorianCalendar(since));
            leadSelector.setLatestUpdatedAt(until);     
             
            request.setLeadSelector(leadSelector);
            */
 
            /*
            // Request Using StaticList Selector
            ////////////////////////////////////////////////////////
            StaticListSelector staticListSelector = new StaticListSelector();
 
            //staticListSelector.setStaticListId(value)
            ObjectFactory objectFactory = new ObjectFactory();
            JAXBElement<String> listName = objectFactory.createStaticListSelectorStaticListName("SMSProgram.listForTesting");
            staticListSelector.setStaticListName(listName);
             
            // JAXBElement<Integer> listId = objectFactory.createStaticListSelectorStaticListId(6926);
            // staticListSelector.setStaticListId(listId);
             
            request.setLeadSelector(staticListSelector);
            */
 
 
            ArrayOfString attributes = new ArrayOfString();
            attributes.getStringItems().add("FirstName");
            attributes.getStringItems().add("AnonymousIP");
            attributes.getStringItems().add("Company");
             
            request.setIncludeAttributes(attributes);
             
            JAXBElement<Integer> batchSize = new ObjectFactory().createParamsGetMultipleLeadsBatchSize(10);
            request.setBatchSize(batchSize);
             
            SuccessGetMultipleLeads result = port.getMultipleLeads(request, header);
             
            JAXBContext context = JAXBContext.newInstance(SuccessGetLead.class);
            Marshaller m = context.createMarshaller();
            m.setProperty(Marshaller.JAXB_FORMATTED_OUTPUT, true);
            m.marshal(result, System.out);
             
        }
        catch(Exception e) {
            e.printStackTrace();
        }
    }
}
```

## Codice di esempio - Rubino

```ruby
require 'savon' # Use version 2.0 Savon gem
require 'date'

mktowsUserId = "" # CHANGE ME
marketoSecretKey = "" # CHANGE ME
marketoSoapEndPoint = "" # CHANGE ME
marketoNameSpace = "http://www.marketo.com/mktows/"

#Create Signature
Timestamp = DateTime.now
requestTimestamp = Timestamp.to_s
encryptString = requestTimestamp + mktowsUserId
digest = OpenSSL::Digest.new('sha1')
hashedsignature = OpenSSL::HMAC.hexdigest(digest, marketoSecretKey, encryptString)
requestSignature = hashedsignature.to_s

#Create SOAP Header
headers = { 
    'ns1:AuthenticationHeader' => { "mktowsUserId" => mktowsUserId, "requestSignature" => requestSignature,                     
    "requestTimestamp"  => requestTimestamp 
    }
}

client = Savon.client(wsdl: 'http://app.marketo.com/soap/mktows/2_3?WSDL', soap_header: headers, endpoint: marketoSoapEndPoint, open_timeout: 90, read_timeout: 90, namespace_identifier: :ns1, env_namespace: 'SOAP-ENV')

#Create Request
request = {
    :lead_selector => {
        :key_type => "EMAIL",
        :key_values => {
            :string_item => ["formtest1@marketo.com", "joe@marketo.com"]
        }
        
    },
    :batch_size => "100"
}

response = client.call(:get_multiple_leads, message: request)

puts response
```
