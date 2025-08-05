---
title: importToList
feature: SOAP
description: chiamate SOAP importToList
exl-id: 7e4930a9-a78f-44a3-9e8c-eeca908080c8
source-git-commit: 3649db037a95cfd20ff0a2c3d81a3b40d0095c39
workflow-type: tm+mt
source-wordcount: '387'
ht-degree: 3%

---

# importToList

Questa funzione consente di importare un elenco di lead in un elenco statico esistente in Marketo, simile alla funzione di importazione elenco nell’interfaccia utente di Marketo.

**Formato di importazione:** Questi valori sono identici alla struttura di un file CSV utilizzato in un&#39;importazione elenco.

**Esempio:**

| E-mail | Primo | Ultimo |
| --- | --- | --- |
| <joe@company.com> | Joe | Smith |
| <mary@company.com> | Mary | Rodgers |
| <wanda@megacorp.com> | Wanda | Williams |

I valori `displayName` devono essere utilizzati in `importFileHeader` anziché nei valori `name`.

**Contenuto e-mail dinamico:** Facoltativamente, puoi trasmettere valori per lead che fungono da sostituzioni per I miei token in un messaggio e-mail.

| E-mail | Primo | Ultimo | {{my.specialToken}} | {{my.otherToken}} |
| --- | --- | --- | --- | --- |
| <joe@company.com> | Joe | Smith | Pesce | Blu |
| <mary@company.com> | Mary | Rodgers | Pollo | Marrone |
| <wanda@megacorp.com> | Wanda | Williams | Verdura | Hazel |

**Importante:** se aggiungi token per i lead, devi specificare la campagna avanzata che li utilizza. Alla successiva esecuzione della Smart Campaign specificata, verranno utilizzati i valori dell’elenco, anziché i normali valori My Token. Dopo l’esecuzione della singola campagna, i token verranno eliminati.

Il completamento di `importToList` può richiedere del tempo, in particolare per gli elenchi di grandi dimensioni. Se si prevede di utilizzare l&#39;elenco appena importato in altre chiamate API, è necessario utilizzare `importToListStatus` per verificare che l&#39;operazione sia stata completata.

**Nota:** l&#39;importazione di valori NULL per campi numerici in un file CSV può generare un&#39;attività &quot;Modifica valore dati&quot; per tali campi, anche se il campo è già vuoto. Eventuali campagne intelligenti che utilizzano un filtro &quot;Valore modificato dei dati&quot; o un trigger &quot;Modifiche del valore dei dati&quot; potrebbero essere idonee per tali campagne anche se i dati non stanno effettivamente cambiando. Utilizza i vincoli su questi filtri/trigger per garantire che i lead non siano idonei per campagne errate durante l’esecuzione delle importazioni.

## Richiesta

| Nome campo | Obbligatorio/facoltativo | Descrizione |
| --- | --- | --- |
| programName | Obbligatorio | Nome del programma contenente l’elenco statico |
| campaignName | Facoltativo | Se utilizzi le sostituzioni di Il mio token, questo è il nome della campagna in cui verranno utilizzati tali token. La campagna deve rientrare nel programma specificato. |
| listName | Obbligatorio | Nome dell’elenco statico in Marketo a cui vengono aggiunti i lead |
| importFileHeader | Obbligatorio | Intestazioni di colonna per i lead da importare, inclusi gli attributi lead e i nomi dei token personali. |
| importFileRows->stringItem | Obbligatorio | Valori separati da virgole, con una riga per lead |
| importListMode | Obbligatorio | Opzioni valide: `UPSERTLEADS` e `LISTONLY` |
| clearList | Facoltativo | Se true, l&#39;elenco statico viene cancellato prima dell&#39;importazione; se vengono aggiunti dei lead falsi. |

## Richiedi XML

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/" xmlns:ns1="http://www.marketo.com/mktows/">
  <SOAP-ENV:Header>
    <ns1:AuthenticationHeader>
      <mktowsUserId>demo17_1_809934544BFABAE58E5D27</mktowsUserId>
      <requestSignature>17bf0b9e412e58eec836dc557ca9433f666944b6</requestSignature>
      <requestTimestamp>2013-08-05T14:56:58-07:00</requestTimestamp>
    </ns1:AuthenticationHeader>
  </SOAP-ENV:Header>
  <SOAP-ENV:Body>
    <ns1:paramsImportToList>
      <programName>Trav-Demo-Program</programName>
      <importFileHeader>Last Name,First Name,Job Title,Company Name,Email Address</importFileHeader>
      <importFileRows>
        <stringItem>Awesomesauce,Developer,Code Slinger,Marketo,dawesomesauce@marketo.com</stringItem>
        <stringItem>Doe,Jane,VP Marketing,Jane Consulting,jdoe@janeconsulting.com</stringItem>
      </importFileRows>
      <importListMode>UPSERTLEADS</importListMode>
      <listName>Trav-Test-List</listName>
      <clearList>false</clearList>
      <campaignName>Batch Campaign Example</campaignName>
    </ns1:paramsImportToList>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

## XML risposta

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/" xmlns:ns1="http://www.marketo.com/mktows/">
  <SOAP-ENV:Body>
    <ns1:successImportToList>
      <result>
        <!-- Possible Values: COMPLETE/PROCESSING/FAILED/CANCELED -->
        <importStatus>PROCESSING</importStatus>
      </result>
    </ns1:successImportToList>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

## Codice di esempio - PHP

```php
 <?php

  $debug = true;

  $marketoSoapEndPoint     = "";  // CHANGE ME
  $marketoUserId           = "";  // CHANGE ME
  $marketoSecretKey        = "";  // CHANGE ME
  $marketoNameSpace        = "http://www.marketo.com/mktows/";

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
  $options = array("connection_timeout" => 20, "location" => $marketoSoapEndPoint);
  if ($debug) {
    $options["trace"] = true;
  }

  // Create Request
  $request = new stdClass();
  $request->programName = "Trav-Demo-Program";
  $request->campaignName = "Batch Campaign Example";
  $request->importFileHeader = "Last Name,First Name,Job Title,Company Name,Email Address";


  $request->importFileRows = array("Awesomesauce,Developer,Code Slinger,Marketo,dawesomesauce@marketo.com","Doe,Jane,VP Marketing,Jane Consulting,jdoe@janeconsulting.com");
  $request->importListMode = "UPSERTLEADS"; // UPSERTLEADS or LISTONLY
  $request->listName = "Trav-Test-List";
  $request->clearList = false;
  $params = array("paramsImportToList" => $request);

  $soapClient = new SoapClient($marketoSoapEndPoint ."?WSDL", $options);
  try {
    $response = $soapClient->__soapCall('importToList', $params, $options, $authHdr);
  }
  catch(Exception $ex) {
    var_dump($ex);
  }
  if ($debug) {
    print "RAW request:\n" .$soapClient->__getLastRequest() ."\n";
    print "RAW response:\n" .$soapClient->__getLastResponse() ."\n";
  }

  print_r($response);
?>
```

## Codice di esempio - Java

```java
import com.marketo.mktows.*;
import java.net.URL;
import javax.xml.namespace.QName;
import java.text.DateFormat;
import java.text.SimpleDateFormat;
import java.util.ArrayList;
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

public class ImportToList {

    public static void main(String[] args) {
        System.out.println("Executing Import To List");
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
            ParamsImportToList request = new ParamsImportToList();

            request.setProgramName("Trav-Demo-Program");
            request.setCampaignName("Batch Campaign Example");
            request.setImportFileHeader("Last Name,First Name,Job Title,Company Name,Email Address");

            ArrayOfString rows = new ArrayOfString();
            rows.getStringItems().add("Awesomesauce,Developer,Code Slinger,Marketo,dawesomesauce@marketo.com");
            rows.getStringItems().add("Doe,Jane,VP Marketing,Jane Consulting,jdoe@janeconsulting.com");
            request.setImportFileRows(rows);

            request.setImportListMode(ImportToListModeEnum.UPSERTLEADS);
            request.setListName("Trav-Test-List");
            request.setClearList(false);

            SuccessImportToList result = port.importToList(request, header);


            JAXBContext context = JAXBContext.newInstance(SuccessImportToList.class);
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
    :program_name => "Trav-Demo-Program",
    :import_file_header => "Last Name,First Name,Job Title,Company Name,Email Address",
    :import_file_rows => {
        :string_item => ["Awesomesauce,Developer,Code Slinger,Marketo,dawesomesauce@marketo.com", "Doe,Jane,VP Marketing,Jane Consulting,jdoe@janeconsulting.com"] },
    :import_list_mode => "UPSERTLEADS",
    :list_name => "Trav-Test-List",
    :clear_list => "false",
    :campaign_name => "Batch Campaign Example"
}

response = client.call(:import_to_list, message: request)

puts response
```
