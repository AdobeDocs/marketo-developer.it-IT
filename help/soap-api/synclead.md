---
title: "syncLead"
feature: SOAP
description: "chiamate SOAP syncLead"
source-git-commit: d335bdd9f939c3e557a557b43fb3f33934e13fef
workflow-type: tm+mt
source-wordcount: '518'
ht-degree: 1%

---


# syncLean

Questa funzione inserisce o aggiorna un singolo record di lead. Quando si aggiorna un lead esistente, il lead viene identificato con una delle chiavi seguenti:

- ID Marketo
- ID sistema esterno (implementato come `foreignSysPersonId`)
- Cookie Marketo (creato dallo script JS di Munchkin)
- E-mail

Se viene trovata una corrispondenza esistente, la chiamata esegue un aggiornamento. In caso contrario, inserisce e crea un lead. I lead anonimi possono essere aggiornati utilizzando l’ID cookie di Marketo e saranno noti in seguito all’aggiornamento.

Ad eccezione dell’e-mail, tutti questi identificatori sono trattati come chiavi univoche. Il Marketo ID ha la precedenza su tutte le altre chiavi. Se entrambi `foreignSysPersonId` e Marketo ID sono presenti nel record principale, quindi Marketo ID ha la precedenza e il `foreignSysPersonId` verrà aggiornato per tale lead. Se l&#39;unico `foreignSysPersonId` viene fornito, quindi viene utilizzato come identificatore univoco. Se entrambi `foreignSysPersonId` e-mail sono presenti, ma l’ID Marketo non è presente, il `foreignSysPersonId` ha la precedenza e l’E-mail verrà aggiornata per tale lead.

Facoltativamente, è possibile specificare un’intestazione di contesto per denominare l’area di lavoro di destinazione.

Quando le aree di lavoro di Marketo sono abilitate e viene utilizzata l’intestazione, vengono applicate le seguenti regole:

- Se vengono impostate le regole di assegnazione e un nuovo lead è idoneo per una qualsiasi delle regole configurate, i nuovi lead vengono creati nella partizione definita dalla regola di assegnazione. In caso contrario, vengono creati nuovi lead nella partizione primaria dell’area di lavoro denominata.
- I lead corrispondenti all&#39;ID lead di Marketo, un ID di sistema esterno o un cookie di Marketo devono esistere nella partizione primaria dell&#39;area di lavoro denominata. In caso contrario viene restituito un errore
- Se un lead esistente corrisponde tramite e-mail, l’area di lavoro denominata viene ignorata e il lead viene aggiornato nella partizione corrente

Quando le aree di lavoro di Marketo sono abilitate e l’intestazione NON viene utilizzata, vengono applicate le seguenti regole:

- Se vengono impostate le regole di assegnazione e un nuovo lead è idoneo per una qualsiasi delle regole configurate, i nuovi lead vengono creati nella partizione definita dalla regola di assegnazione. In caso contrario, vengono creati nuovi lead nella partizione principale dell’area di lavoro &quot;Predefinita&quot;.
- I lead esistenti vengono aggiornati nella partizione corrente

Se le aree di lavoro di Marketo NON sono abilitate, l’area di lavoro di destinazione DEVE essere l’area di lavoro &quot;predefinita&quot;. Non è necessario trasmettere l’intestazione.

## Richiesta

| Nome campo | Obbligatorio/facoltativo | Descrizione |
| --- | --- | --- |
| leadRecord->Id | Obbligatorio - Solo quando si invia un messaggio e-mail o `foreignSysPersonId` non è presente | ID Marketo del record del lead |
| leadRecord->E-mail | Obbligatorio - Solo quando l’ID o `foreignSysPersonId` non è presente | Indirizzo e-mail associato al record del lead |
| leadRecord->`foreignSysPersonId` | Obbligatorio - Solo quando l’ID o l’e-mail non è presente | ID di sistema esterno associato al record del lead |
| leadRecord->foreignSysType | Facoltativo - Obbligatorio solo quando `foreignSysPersonId` è presente | Il tipo di sistema esterno. Valori possibili: CUSTOM, SFDC, NETSUITE |
| leadRecord->leadAttributeList->attribute->attrName | Obbligatorio | Il nome dell’attributo del lead di cui desideri aggiornare il valore. |
| leadRecord->leadAttributeList->attribute->attrValue | Obbligatorio | Il valore che si desidera impostare sull&#39;attributo lead specificato in attrName. |
| returnLead | Obbligatorio | Se è true, restituisce il record lead aggiornato completo al momento dell&#39;aggiornamento. |
| marketoCookie | Facoltativo | Il [Munchkin JavaScript](../javascript-api/lead-tracking.md) cookie |

## Richiedi XML

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/" xmlns:ns1="http://www.marketo.com/mktows/">
  <SOAP-ENV:Header>
    <ns1:AuthenticationHeader>
      <mktowsUserId>bigcorp1_461839624B16E06BA2D663</mktowsUserId>
      <requestSignature>92f05a7be4838ae1c0e5aafe814891ee72968a08</requestSignature>
      <requestTimestamp>2013-07-31T12:38:47-07:00</requestTimestamp>
    </ns1:AuthenticationHeader>
  </SOAP-ENV:Header>
  <SOAP-ENV:Body>
    <ns1:paramsSyncLead>
      <leadRecord>
        <Email>t@t.com</Email>
        <leadAttributeList>
          <attribute>
            <attrName>FirstName</attrName>
            <attrValue>George</attrValue>
          </attribute>
          <attribute>
            <attrName>LastName</attrName>
            <attrValue>of the Jungle</attrValue>
          </attribute>
        </leadAttributeList>
      </leadRecord>
      <returnLead>false</returnLead>
    </ns1:paramsSyncLead>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

## XML risposta

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:ns1="http://www.marketo.com/mktows/">
  <SOAP-ENV:Body>
    <ns1:successSyncLead>
      <result>
        <leadId>1089965</leadId>
        <syncStatus>
          <leadId>1089965</leadId>
          <status>UPDATED</status>
          <error xsi:nil="true" />
        </syncStatus>
        <leadRecord xsi:nil="true" />
      </result>
    </ns1:successSyncLead>
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
  $options = array("connection_timeout" =20, "location" =$marketoSoapEndPoint);
  if ($debug) {
    $options["trace"] = true;
  }
 
  // Create Request
  $leadKey = new stdClass();
  $leadKey->Email = "george@jungle.com";
 
  // Lead attributes to update
  $attr1 = new stdClass();
  $attr1->attrName  = "FirstName";
  $attr1->attrValue = "George";
 
  $attr2= new stdClass();
  $attr2->attrName  = "LastName";
  $attr2->attrValue = "of the Jungle";
 
  $attrArray = array($attr1, $attr2);
  $attrList = new stdClass();
  $attrList->attribute = $attrArray;
  $leadKey->leadAttributeList = $attrList;
 
  $leadRecord = new stdClass();
  $leadRecord->leadRecord = $leadKey;
  $leadRecord->returnLead = false;
  $params = array("paramsSyncLead" =$leadRecord);
 
  $soapClient = new SoapClient($marketoSoapEndPoint ."?WSDL", $options);
  try {
    $result = $soapClient->__soapCall('syncLead', $params, $options, $authHdr);
  }
  catch(Exception $ex) {
    var_dump($ex);
  }
 
  if ($debug) {
    print "RAW request:\n" .$soapClient->__getLastRequest() ."\n";
    print "RAW response:\n" .$soapClient->__getLastResponse() ."\n";
  }
  print_r($result);
 
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
import javax.crypto.Mac;
import javax.crypto.spec.SecretKeySpec;
import org.apache.commons.codec.binary.Hex;
import javax.xml.bind.JAXBContext;
import javax.xml.bind.JAXBElement;
import javax.xml.bind.Marshaller;
 
 
public class SyncLead {
 
    public static void main(String[] args) {
        System.out.println("Executing syncLead");
        try {
            URL marketoSoapEndPoint = new URL("https://100-AEK-913.mktoapi.com/soap/mktows/2_1" + "?WSDL");
            String marketoUserId = "demo17_1_809934544BFABAE58E5D27";
            String marketoSecretKey = "27272727aa";
             
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
            ParamsSyncLead request = new ParamsSyncLead();
            LeadRecord key = new LeadRecord();
             
            ObjectFactory objectFactory = new ObjectFactory();
            JAXBElement<Stringemail = objectFactory.createLeadRecordEmail("george@jungle.com");
            key.setEmail(email);
            request.setLeadRecord(key);
             
            Attribute attr1 = new Attribute();
            attr1.setAttrName("FirstName");
            attr1.setAttrValue("George2");
             
            Attribute attr2 = new Attribute();
            attr2.setAttrName("LastName");
            attr2.setAttrValue("of the Jungle");
             
            ArrayOfAttribute aoa = new ArrayOfAttribute();
            aoa.getAttributes().add(attr1);
            aoa.getAttributes().add(attr2);
             
            QName qname = new QName("http://www.marketo.com/mktows/", "leadAttributeList");
            JAXBElement<ArrayOfAttributeattrList = new JAXBElement(qname, ArrayOfAttribute.class, aoa);         
            key.setLeadAttributeList(attrList);
             
            MktowsContextHeader headerContext = new MktowsContextHeader();
            headerContext.setTargetWorkspace("default");
             
            SuccessSyncLead result = port.syncLead(request, header, headerContext);
 
            JAXBContext context = JAXBContext.newInstance(SuccessSyncLead.class);
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
    'ns1:AuthenticationHeader' ={ "mktowsUserId" =mktowsUserId, "requestSignature" =requestSignature, 
    "requestTimestamp"  =requestTimestamp 
    }
}

client = Savon.client(wsdl: 'http://app.marketo.com/soap/mktows/2_3?WSDL', soap_header: headers, endpoint: marketoSoapEndPoint, open_timeout: 90, read_timeout: 90, namespace_identifier: :ns1, env_namespace: 'SOAP-ENV')

#Create Request
request = {
    :lead_record ={
        :Email ="t@t.com",
          :lead_attribute_list ={
              :attribute ={
                :attr_name ="FirstName",
                :attr_value ="George" },
              :attribute! ={
                :attr_name ="LastName",
                :attr_value ="of the Jungle" }
        }
    },
    :return_lead ="false"
}

response = client.call(:sync_lead, message: request)

puts response
```
