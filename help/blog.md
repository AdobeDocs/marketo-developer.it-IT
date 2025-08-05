---
title: Archivio blog
description: Un archivio del blog Marketo Developers dal 2014-2023
exl-id: d7ae88dd-9938-4957-9798-db43090dab4e
source-git-commit: 3649db037a95cfd20ff0a2c3d81a3b40d0095c39
workflow-type: tm+mt
source-wordcount: '61715'
ht-degree: 0%

---

# Archivio blog

>[!INFO]
>
>Questo è un archivio del blog di Marketo, che si estende dal 2014 al 2023. Viene qui fornito solo come riferimento storico.
>&#x200B;>Alcune informazioni potrebbero non essere aggiornate.  Consulta sempre la documentazione corrente per informazioni sulle funzionalità più recenti.
>

>[!IMPORTANT]
>L’API SOAP diventerà obsoleta e non sarà più disponibile dopo il 31 ottobre 2025. Tutti i nuovi sviluppi devono essere eseguiti con l’API REST di Marketo e i servizi esistenti devono essere migrati entro tale data per evitare interruzioni del servizio. Se si dispone di un servizio che utilizza l&#39;API SOAP, consultare la [Guida alla migrazione dell&#39;API SOAP](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/soap/migration) per informazioni su come eseguire la migrazione.
>

>[!IMPORTANT]
>Il supporto per l&#39;autenticazione tramite il parametro di query `access_token` verrà rimosso il 31 ottobre 2025. Se il progetto utilizza un parametro di query per passare il token di accesso, è necessario aggiornarlo per utilizzare al più presto l&#39;[intestazione autorizzazione](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/rest/authentication#using-an-access-token). I nuovi sviluppi devono utilizzare esclusivamente l’intestazione Autorizzazione.
>

## Blog per sviluppatori di Marketo

Benvenuti Sviluppatori Marketo! Siamo stati molto impegnati qui in Marketo a rilasciare nuove funzioni per aiutare gli addetti al marketing ad avere più successo che mai. Gli addetti al marketing sono oggi supportati da una fenomenale comunità di sviluppatori. Questo blog è dedicato agli sviluppatori web e agli ingegneri software che supportano le esigenze in rapida evoluzione del moderno esperto di marketing. Con l’evolversi della piattaforma Marketo, vengono annunciate nuove opzioni di integrazione e aggiornamenti delle versioni API man mano che queste si sviluppano. Inoltre, presenteremo una nuova serie di articoli dimostrativi in cui condividiamo esempi di codice e best practice sull’integrazione con Marketo Platform. Il primo articolo di questa serie illustra come recuperare in modo efficiente le informazioni sulle persone (clienti/contatti/lead) memorizzate in Marketo utilizzando l’API. Iscrivetevi utilizzando il modulo qui sopra per rimanere aggiornati. Ti inviamo aggiornamenti via e-mail, man mano che vengono pubblicati nuovi articoli.

Pubblicato il _2014-03-06_ da _David_

## Aggiornamenti sulla versione di gennaio 2014

### Forms 2.0

Forms 2.0 consente ai professionisti del marketing di creare moduli web belli, stabili e flessibili senza alcuna conoscenza in materia di programmazione. Oltre all’editor notevolmente migliorato, alla logica condizionale e al design robusto, ora è più facile che mai incorporare Marketo forms in qualsiasi pagina del sito web. Gli sviluppatori possono utilizzare JavaScript per estendere le funzionalità di base; i casi di utilizzo includono:

* Nascondere un modulo dopo l’invio corretto in modo che il visitatore rimanga sulla pagina dopo aver compilato il modulo
* Visualizzazione di un messaggio di errore personalizzato all’invio in base a una logica di business personalizzata
* Visualizzazione del modulo in una finestra di dialogo di stile lightbox

La documentazione per gli sviluppatori è disponibile [qui](/help/javascript-api/forms-api-reference.md).

### È ora disponibile la versione 2_3 dell’API di SOAP

* [getLeadChanges:](/help/soap-api/getleadchanges.md) ha introdotto il campo della richiesta `activityNameFilter`
* [ListOperation:](/help/soap-api/listoperation.md) Campo richiesta rimosso `skipActivityLog`

**Nota:** le revisioni API di SOAP sono compatibili con le versioni precedenti

Pubblicato il _2014-01-26_ da _Travis Kaufman_

## Zapier Part II: Annuncio sull&#39;integrazione di Marketo

In un post precedente, abbiamo discusso su come utilizzare Zapier per integrare fonti di dati esterne con Marketo. Il post ha presentato un approccio pratico per creare il proprio flusso di lavoro Zapier (o &quot;Zap&quot;) da zero per integrare Marketo e altre app. Quindi, ti piace l’idea di utilizzare Zapier con Marketo, ma hai bisogno di aiuto per iniziare? Buone notizie! Zapier ha appena rilasciato un certo numero di Zaps per Marketo che consentono di iniziare rapidamente:

* Acquisire nuovi lead
* Sviluppa i tuoi clienti
* Distribuisci informazioni di contatto
* Reagire a nuovi potenziali clienti

Oltre a questi esempi, puoi sfogliare la pagina [Integrazioni Marketo](https://zapier.com/apps/marketo/integrations) con centinaia di altre app su Zapier e creare in pochi minuti flussi di lavoro automatizzati. Non è richiesta alcuna codifica. Risparmia tempo e lascia che l&#39;automazione faccia il tuo lavoro manuale. Diventa creativo. Il cielo è il limite!

Pubblicato il _2016-06-01_ da _David_

## Recupero di informazioni su clienti e potenziali clienti da Marketo tramite l’API

È possibile recuperare informazioni sui clienti acquisiti e potenziali archiviati in Marketo utilizzando l&#39;API SOAP `getLead` e [`getMultipleLeads`](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads). Spesso è opportuno estrarre queste informazioni su base periodica per mantenere aggiornato un altro sistema, man mano che le informazioni sui clienti e sui potenziali clienti vengono aggiornate o che vengono creati nuovi record in Marketo. Ti mostriamo l’esempio di codice che verrebbe eseguito su base periodica per raccogliere gli aggiornamenti da Marketo. Il diagramma seguente illustra le chiamate API effettuate con un timer periodico impostato. A seconda del caso d’uso, il timer periodico può essere impostato per eseguire il codice seguente ogni 10 minuti.

La prima chiamata a [`getMultipleLeads`](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads) imposterà l&#39;intervallo di tempo, batchSize e i campi da restituire. Tutti i lead all&#39;interno di Marketo che sono stati aggiornati nell&#39;intervallo di tempo specificato verranno restituiti insieme a un streamPosition quando sono disponibili più record rispetto al batchSize specificato.

**Richiesta SOAP per la prima chiamata a getMultipleLeads:**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<ns2:paramsGetMultipleLeads xmlns:ns2="<http://www.marketo.com/mktows/">
  <leadSelector xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="ns2:LastUpdateAtSelector">
    <latestUpdatedAt>2014-02-13T11:51:08.710-08:00</latestUpdatedAt>
    <oldestUpdatedAt>2014-02-12T11:51:08.713-08:00</oldestUpdatedAt>
  </leadSelector>
  <batchSize>2</batchSize>
  <includeAttributes>
    <stringItem>FirstName</stringItem>
    <stringItem>LastName</stringItem>
    <stringItem>Email</stringItem>
  </includeAttributes>
</ns2:paramsGetMultipleLeads>
```

**Risposta SOAP dalla prima chiamata a getMultipleLeads:**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<ns2:successGetMultipleLeads xmlns:ns2="<http://www.marketo.com/mktows/">
  <result>
 <returnCount>2</returnCount><remainingCount>24</remainingCount><newStreamPosition>id:1089965:to:1392234668:tl:1392321068:os:2:rc:24</newStreamPosition><leadRecordList>
      <leadRecord>
        <Id>84105</Id>
        <Email>eimang@marketo.com</Email>
        <ForeignSysPersonId xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:nil="true" />
        <ForeignSysType xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:nil="true" />
        <leadAttributeList xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:nil="true">
          <attribute>
            <attrName>FirstName</attrName>
            <attrType>string</attrType>
            <attrValue>French</attrValue>
          </attribute>
          <attribute>
            <attrName>LastName</attrName>
            <attrType>string</attrType>
            <attrValue>Lead</attrValue>
          </attribute>
        </leadAttributeList>
      </leadRecord>
      <leadRecord>
        <Id>1089965</Id>
        <Email>t@t.com</Email>
        <ForeignSysPersonId xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:nil="true" />
        <ForeignSysType xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:nil="true" />
        <leadAttributeList xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:nil="true">
          <attribute>
            <attrName>FirstName</attrName>
            <attrType>string</attrType>
            <attrValue>George</attrValue>
          </attribute>
          <attribute>
            <attrName>LastName</attrName>
            <attrType>string</attrType>
            <attrValue>of the Jungle</attrValue>
          </attribute>
        </leadAttributeList>
      </leadRecord>
    </leadRecordList>
  </result>
</ns2:successGetMultipleLeads>
```

Se il valore `<remainingCount/>` è maggiore di 0, verranno effettuate chiamate successive a `getMultipleLeads` per impaginare il resto passando il valore `<newStreamPosition/>` restituito nella chiamata precedente al parametro `<streamPosition/>`. **Richiesta SOAP per chiamata successiva a getMultipleLeads:**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<ns2:paramsGetMultipleLeads xmlns:ns2="<http://www.marketo.com/mktows/">
  <leadSelector xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="ns2:LastUpdateAtSelector">
    <latestUpdatedAt>2014-02-13T11:51:08.710-08:00</latestUpdatedAt>
    <oldestUpdatedAt>2014-02-12T11:51:08.713-08:00</oldestUpdatedAt>
  </leadSelector><streamPosition>id:1089965:to:1392234668:tl:1392321068:os:2:rc:24</streamPosition><batchSize>2</batchSize>
  <includeAttributes>
    <stringItem>FirstName</stringItem>
    <stringItem>LastName</stringItem>
    <stringItem>Email</stringItem>
  </includeAttributes>
</ns2:paramsGetMultipleLeads>
```

Questa logica continua finché `<remainingCount/>` è maggiore di zero. **Di seguito è riportato un programma Java di esempio che esegue lo scenario descritto in precedenza.**

```java
import com.marketo.mktows.\*;
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
        QName serviceName = new QName("<http://www.marketo.com/mktows/>",
        "MktMktowsApiService");
        MktMktowsApiService service = new
        MktMktowsApiService(marketoSoapEndPoint, serviceName);
        MktowsPort port = service.getMktowsApiSoapPort();

        // Create Signature
        DateFormat df = new SimpleDateFormat("yyyy-MM-dd'T'HH:mm:ssZ");
        String text = df.format(new Date());
        String requestTimestamp = text.substring(0, 22) + ":" +         text.substring(22);
        String encryptString = requestTimestamp + marketoUserId ;

        SecretKeySpec secretKey = new         SecretKeySpec(marketoSecretKey.getBytes(), "HmacSHA1");

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

        // Build Request Using LastUpdateAtSelector
        LastUpdateAtSelector leadSelector = new LastUpdateAtSelector();
        GregorianCalendar gc = new GregorianCalendar();
        gc.setTimeInMillis(new Date().getTime());
        gc.add( GregorianCalendar.DAY_OF_YEAR, -20);
        DatatypeFactory factory = DatatypeFactory.newInstance();
        ObjectFactory objectFactory = new ObjectFactory();
        JAXBElement<XMLGregorianCalendar> until =         objectFactory.createLastUpdateAtSelectorLatestUpdatedAt(factory.newXMLGregorianCalendar(gc));

        GregorianCalendar since = new GregorianCalendar();
        since.setTimeInMillis(new Date().getTime());
        since.add( GregorianCalendar.DAY_OF_YEAR, -21);
        leadSelector.setOldestUpdatedAt(factory.newXMLGregorianCalendar(since));
        leadSelector.setLatestUpdatedAt(until);
        request.setLeadSelector(leadSelector);

        ArrayOfString attributes = new ArrayOfString();
        attributes.getStringItems().add("FirstName");
        attributes.getStringItems().add("LastName");
        attributes.getStringItems().add("Email");
        request.setIncludeAttributes(attributes);

        JAXBElement<Integer> batchSize = new
        ObjectFactory().createParamsGetMultipleLeadsBatchSize(2);
        request.setBatchSize(batchSize);

        SuccessGetMultipleLeads result = null;

        int count = 0;

        do {
        if (count > 0) {
        // Set the streamPosition on subsequent calls to paginate
        through large result sets
        String pos = result.getResult().getNewStreamPosition();
        JAXBElement<String> streamPos = new
        ObjectFactory().createParamsGetMultipleLeadsStreamPosition(pos);
        request.setStreamPosition(streamPos);
        }

        JAXBContext context =
        JAXBContext.newInstance(ParamsGetMultipleLeads.class);
        Marshaller m = context.createMarshaller();
        m.setProperty(Marshaller.JAXB_FORMATTED_OUTPUT, true);
        System.out.println("REQUEST:");
        m.marshal(request, System.out);
        result = port.getMultipleLeads(request, header);
        System.out.println("RESPONSE:");
        m.marshal(result, System.out);
        count = result.getResult().getReturnCount();
        } while (result.getResult().getRemainingCount() > 0);
        }

        catch(Exception e) {
        // Update to include appropriate retry/error handling
        e.printStackTrace();
        }

  }

}
```

### Suggerimenti

Quando si estraggono grandi volumi di contatti da Marketo, si consiglia di regolare la richiesta API in base ai seguenti parametri:

* `<includeAttributes/>`: si consiglia di richiedere solo i campi che si è interessati a mantenere sincronizzati con il sistema. Questo riduce il payload della risposta e aumenta le prestazioni delle query.
* `<batchSize/>`: l&#39;API supporta fino a 1000 record da restituire in una singola chiamata. Regolando questo valore a 500 record si riduce anche il payload della risposta.
* `<LastUpdatedAtSelector/>`: si consiglia di impostare sia `<oldestUpdatedAt/>` che il parametro `<latestUpdatedAt/>` per limitare l&#39;intervallo di date. Ad esempio, invece di effettuare una singola richiesta per un anno di dati. Interrompi le chiamate API per richiedere intervalli di date più piccoli.

Questo articolo contiene il codice utilizzato per implementare integrazioni personalizzate. A causa della sua natura personalizzata, il team del supporto tecnico Marketo non è in grado di risolvere i problemi relativi al lavoro personalizzato. Non tentare di implementare il seguente esempio di codice senza un’adeguata esperienza tecnica o l’accesso a uno sviluppatore esperto.

Pubblicato il _2014-03-05_ da _Travis Kaufman_

## Aggiornamenti sulla versione di febbraio 2014

### Aggiornamento API SOAP

* [syncMObjects](/help/soap-api/syncmobjects.md): è ora possibile aggiungere e aggiornare tag e canali per i programmi esistenti.

Gli aggiornamenti sono incorporati nel [2_3 WSDL](http://app.marketo.com/soap/mktows/2_3?WSDL).

Pubblicato il _2014-02-26_ da _Travis Kaufman_

## Aggiornamenti sulla versione di marzo 2014

### Aggiornamento API SOAP

* Miglioramenti delle prestazioni per [syncLead](/help/soap-api/synclead.md) e [syncMultipleLeads](/help/soap-api/syncmultipleleads.md)

Gli aggiornamenti sono incorporati nel [2_3 WSDL](http://app.marketo.com/soap/mktows/2_3?WSDL).

Pubblicato il _2014-03-20_ da _Travis Kaufman_

## Unisci attività visitatore anonimo quando il visitatore compila il modulo

Nel post del blog intitolato &quot;Capture Anonymous Visitor Activity Based on Business Logic&quot; (Acquisire attività dei visitatori anonimi in base alla logica di business) abbiamo discusso su come creare record di lead anonimi in Marketo in base a eventi personalizzati. In questo post di blog, ci baseremo su quel post e associeremo un record di lead anonimo a un utente noto dopo che avrai ricevuto le informazioni di contatto dell&#39;utente. Il [codice di tracciamento Munchkin](/help/javascript-api/lead-tracking.md) di Marketo ti consente di monitorare le visite al tuo sito Web. La prima volta che un utente visita una pagina del sito web che presenta il codice di tracciamento di Munchkin, Marketo crea un lead anonimo e utilizza un cookie del browser per tracciarlo. Una volta identificati, diventano lead noti e la cronologia associata al cookie del browser viene unita al record lead di Marketo. **I lead anonimi vengono creati quando un utente:**

1. Visita la pagina di destinazione di Marketo per la prima volta
1. Visita una pagina con codice di tracciamento Munchkin
1. Fare clic sul collegamento Visualizza come pagina Web in un messaggio di posta elettronica di Marketo

**Un lead anonimo viene unito a un lead noto nuovo o esistente quando un utente:**

1. Fai clic su un collegamento a un’e-mail di Marketo
1. Compila un modulo Marketo
1. Utilizza una delle tecniche riportate di seguito per associare un lead anonimo a un record noto.

Per inviare i dati di una persona o associare un cookie di web tracking del browser al record della persona corrispondente in Marketo, utilizza una delle seguenti tecniche: **Invio lato browser** Se il tuo caso d&#39;uso richiede l&#39;invio di dati di una persona dal browser, utilizza l&#39;invio di moduli in background. **Invio lato server** Se non è necessario l&#39;invio lato browser, l&#39;API REST offre molti metodi per l&#39;invio dei dati delle persone e un metodo specifico per associare i cookie ai record delle persone.

Pubblicato il _2014-04-22_ da _Murta_

## Aggiornamenti sulla versione di aprile 2014

### Marketo Forms Security Update

Abbiamo introdotto un limite al numero e alla frequenza di invio dei post modulo da un singolo indirizzo IP. Questo limite viene ora applicato a 30 post al minuto per proteggere i nostri clienti da un uso dannoso di invii di moduli programmatici. L&#39;API [syncLead](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/soap/leads/synclead) è il veicolo di integrazione consigliato per l&#39;invio programmatico di nuovi contatti in Marketo.

Pubblicato il _2014-04-29_ da _Travis Kaufman_

## Sostituisci l’integrazione post modulo con l’API Marketo

È pratica comune che gli addetti al marketing che utilizzano Marketo associino il successo di una determinata campagna di marketing in base al numero di contatti/lead che hanno compilato un modulo web specifico. L’addetto al marketing creerebbe semplicemente un nuovo modulo di Marketo, lo inserirebbe in una pagina di destinazione e quindi configurerebbe una campagna di attivazione in Marketo per associare tutti i contatti che hanno compilato quel modulo specifico come esito positivo per quel programma di marketing. (vedi la schermata seguente). Utilizzare lo stesso approccio durante la registrazione del successo del programma è un&#39;estensione naturale, anche quando il successo della campagna risiede al di fuori di chi compila un modulo. Ad esempio, quando un addetto marketing esegue un’offerta promozionale e la conversione consiste nell’chiamare e pianificare un appuntamento. Il potenziale cliente non sta compilando un modulo in nessuna fase del processo. Nell’articolo seguente viene illustrato come utilizzare l’API syncLead di Marketo per creare un nuovo contatto, se si tratta di nuovi contatti, e quindi l’API requestCampaign per registrare il successo di un determinato programma di marketing.

Pubblicato il _1970-01-01_ da _Travis Kaufman_

## Eliminare un lead con l’API Marketo

Supponiamo che tu voglia eliminare un lead tramite l’API Marketo. A tale scopo, chiama l’API requestCampaign su una campagna con un’azione di flusso predefinita per eliminare un lead. Ti mostriamo innanzitutto come creare una campagna avanzata, poi come impostare un trigger per eseguire una campagna tramite l’API, infine come definire l’eliminazione di un lead come parte di un’azione di flusso e infine come utilizzare un esempio di codice per eseguire questa campagna. **Come creare una nuova campagna avanzata in Marketo** Le campagne avanzate in Marketo eseguono tutte le attività di marketing. In una campagna avanzata, puoi impostare una serie di azioni automatizzate da eseguire su un elenco avanzato di contatti. Se elimini un lead, imposta un trigger nella campagna come mostrato di seguito. Innanzitutto, configuriamo Smart Campaign.

1. In Attività di marketing, scegli un programma, quindi fai clic su Nuova risorsa locale 1 nel menu a discesa Nuovo. Fai clic su Smart Campaign
1. Inserisci il nome della campagna avanzata e fai clic su Crea Aggiungi trigger a una campagna avanzata** L’aggiunta di trigger a una campagna avanzata ti consente di eseguire una campagna avanzata su una persona alla volta in base a un evento live, in questo caso una richiesta tramite l’API requestCampaign.
1. Cerca il trigger &quot;Campaign is Requested&quot; (Campagna è richiesta), quindi trascinalo sull’area di lavoro.
1. Nel trigger, seleziona &quot;è&quot; e &quot;API servizio Web&quot;.

**Come creare un&#39;azione Elimina un flusso di lead in una campagna** Fai clic su Flusso nel menu principale. Dal menu a destra, cerca Elimina lead, quindi trascinalo al centro per aggiungerlo come attivatore alla campagna. Nota: se elimini un lead solo da Marketo e lo lasci nel CRM, quando uno qualsiasi dei dati del lead viene aggiornato, il lead viene ricreato in Marketo.  **Esempio di codice per chiamare l&#39;API requestCampaign** Dopo aver configurato la campagna e i trigger nell&#39;interfaccia di Marketo, ti mostriamo come utilizzare l&#39;API per eseguire questa campagna. Il primo esempio è una richiesta XML, il secondo è una risposta XML e l&#39;ultimo è un esempio di codice Java che può essere utilizzato per generare la richiesta XML. Ti mostriamo anche come trovare l’ID campagna utilizzato quando effettui una chiamata all’API requestCampaign. La chiamata API richiede inoltre di conoscere in anticipo l’ID della campagna Marketo. Puoi determinare l’ID della campagna utilizzando uno dei seguenti metodi:

1. Utilizza l&#39;API [getCampaignsForSource](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getCampaignsUsingGET)
1. Apri la campagna Marketo in un browser e osserva la barra degli indirizzi URL. L’ID della campagna (rappresentato da un numero intero di 4 cifre) si trova immediatamente dopo &quot;SC&quot;. Ad esempio, `https://app-stage.marketo.com/#SC**1025**A1`. La parte in grassetto è l’ID della campagna &quot;1025&quot;. Richiesta SOAP per requestCampaign

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:ns1="<http://www.marketo.com/mktows/">
  <SOAP-ENV:Header>
    <ns1:AuthenticationHeader>
      <mktowsUserId>demo17_1_809939944BFABAE58E5D27</mktowsUserId><requestSignature>48397ad47b71a1439f13a51eea3137df46441979</requestSignature><requestTimestamp>2013-08-01T12:31:14-07:00</requestTimestamp>
    </ns1:AuthenticationHeader>
  </SOAP-ENV:Header>
  <SOAP-ENV:Body>
    <ns1:paramsRequestCampaign>
      <source>MKTOWS</source>
      <campaignId>4496</campaignId>
      <leadList>
        <leadKey>
          <keyType>EMAIL</keyType>
          <keyValue>lead@company.com</keyValue>
        </leadKey>
        <leadKey>
          <keyType>EMAIL</keyType>
          <keyValue>anotherlead@company.com</keyValue>
        </leadKey>
      </leadList>
    </ns1:paramsRequestCampaign>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

Risposta di SOAP per requestCampaign

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:ns1="<http://www.marketo.com/mktows/">
  <SOAP-ENV:Body>
    <ns1:successRequestCampaign>
      <result>
        <success>true</success>
      </result>
    </ns1:successRequestCampaign>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

Di seguito è riportato un programma Java di esempio che esegue lo scenario descritto in precedenza.

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


public class RequestCampaign {

    public static void main(String[] args) {
        System.out.println("Executing Request Campaign");
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
            ParamsRequestCampaign request = new ParamsRequestCampaign();

            request.setSource(ReqCampSourceType.MKTOWS);

            ObjectFactory objectFactory = new ObjectFactory();
            JAXBElement<Integer> campaignId = objectFactory.createParamsRequestCampaignCampaignId(4496);
            request.setCampaignId(campaignId);

            ArrayOfLeadKey leadKeyList = new ArrayOfLeadKey();
            LeadKey key = new LeadKey();
            key.setKeyType(LeadKeyRef.EMAIL);
            key.setKeyValue("lead@company.com");

            LeadKey key2 = new LeadKey();
            key2.setKeyType(LeadKeyRef.EMAIL);
            key2.setKeyValue("anotherlead@company.com");

            leadKeyList.getLeadKeies().add(key);
            leadKeyList.getLeadKeies().add(key2);

            JAXBElement<ArrayOfLeadKey> arrayOfLeadKey = objectFactory.createParamsRequestCampaignLeadList(leadKeyList);
            request.setLeadList(arrayOfLeadKey);

            SuccessRequestCampaign result = port.requestCampaign(request, header);

            JAXBContext context = JAXBContext.newInstance(SuccessRequestCampaign.class);
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

Questo articolo contiene il codice utilizzato per implementare integrazioni personalizzate. A causa della sua natura personalizzata, il team del supporto tecnico Marketo non è in grado di risolvere i problemi relativi al lavoro personalizzato. Non tentare di implementare il seguente esempio di codice senza un’adeguata esperienza tecnica o l’accesso a uno sviluppatore esperto.

Pubblicato il _2014-05-16_ da _Murta_

## Tracciamento personalizzato delle attività con Munchkin AP-

Tieni traccia dell’attività personalizzata in Marketo. Ad esempio, puoi guardare un video su una pagina web e desideri monitorare i visitatori che guardano più del 50% di un video. Puoi farlo utilizzando la funzione di tracciamento attività personalizzato di Munchkin. Questo verrebbe implementato ascoltando un evento sulla pagina, ovvero il video raggiunge il 50%, e chiamando quindi l’API Munchkin. A tal fine, dobbiamo impostare un’attività personalizzata in Marketo da chiamare in base a questo evento sulla pagina. Usiamo YouTube per il lettore video e utilizziamo la loro [API YouTube Iframe](https://developers.google.com/youtube/iframe_api_reference) per chiamare il metodo sull&#39;API Munchkin.

Ti mostriamo innanzitutto come generare il codice di tracciamento di Munchkin in Marketo, poi come modificare il codice di esempio di Munchkin per attivarlo in base agli eventi della pagina, infine come impostare una campagna con un elenco avanzato definito dalle azioni nella pagina con i passaggi dei flussi e infine come verificare che una visita alla pagina da parte di un utente anonimo sia stata registrata in Marketo. ==== Questo post sul blog è un esempio dal vivo del codice che viene spiegato. Compila questo modulo per essere un utente conosciuto in Marketo. In questo modo, quando guardi il 50% del video, ti invierà il resto del video e, se guardi il 100% del video, ti invierà un collegamento a un altro post di blog. === <https://developers.google.com/youtube/iframe_api_reference>

**Come generare codice di tracciamento di Munchkin** Il codice di tracciamento di Munchkin consente di tenere traccia delle visite al sito Web. Esistono tre tipi di codice Munchkin descritti di seguito, ma in questo esempio utilizziamo il codice di tracciamento asincrono di Munchkin. A) Semplice: ha il minor numero di righe di codice, ma non ottimizza per il tempo di caricamento della pagina web. Questo codice carica la libreria jQuery ogni volta che viene caricata una pagina Web. B) Asincrono: riduce il tempo di caricamento della pagina web. Questo codice controlla se la libreria jQuery esiste già, la carica se è mancante e la utilizza per l’esecuzione del codice di tracciamento una volta caricato il resto della pagina web. C) Asynchronous jQuery: riduce il tempo di caricamento delle pagine web e migliora le prestazioni del sistema. Questo codice presuppone che tu disponga già di jQuery e non controlla di caricarlo.

1. Fai clic su Amministratore in alto a destra nell’app.
1. Fai clic su Munchkin nella struttura ad albero a sinistra.
1. Seleziona Asincrono per Tipo di codice di tracciamento.
1. Fai clic su e copia il codice di tracciamento JavaScript da inserire sul sito web. **Codice YouTube** <https://developers.google.com/youtube/js_api_reference#EventHandlers> <https://developers.google.com/youtube/iframe_api_reference> lettore.

`getCurrentTime()` Restituisce il tempo trascorso in secondi dall&#39;avvio della riproduzione del video. `player.getDuration()` Restituisce la durata in secondi del video attualmente riprodotto. `getDuration()` restituirà 0 fino al caricamento dei metadati del video, che di solito avviene subito dopo l&#39;inizio della riproduzione del video. Quando un utente non cookie accede a una pagina con il codice di tracciamento di Munchkin, viene creato un nuovo cookie sul browser dell’utente e un nuovo lead anonimo in Marketo. Se l’utente è già cookie e l’utente è un lead esistente in Marketo, la visita alla pagina verrà registrata nel registro delle attività dell’utente in Marketo. **Esempio di codice per l&#39;utente cookie e evento di tracciamento** Inserisci il codice di tracciamento nelle pagine Web immediatamente prima del tag `</body>`. Le pagine di destinazione create in Marketo contengono automaticamente un codice di tracciamento, pertanto non è necessario inserirlo. Questo esempio di codice chiama l’API Munchkin dopo il caricamento dello script:

```javascript
<script type="text/javascript">
(function() {
  var didInit = false;
  function initMunchkin() {
    if(didInit === false) {
      didInit = true;
      Munchkin.init('XXX-XXX-XXX');
    }
  }
  var s = document.createElement('script');
  s.type = 'text/javascript';
  s.async = true;
  s.src = '//munchkin.marketo.net/munchkin.js';
  s.onreadystatechange = function() {
    if (this.readyState == 'complete' || this.readyState == 'loaded') {
      initMunchkin();
    }
  };
  s.onload = initMunchkin;
  document.getElementsByTagName('head')[0].appendChild(s);
})();
</script>
```

Questo esempio di codice chiama l’API Munchkin dopo che l’utente ha visitato la pagina per 5 secondi ed ha anche fatto scorrere di 500 pixel verso il basso nella pagina:

```javascript
<script src="https://code.jquery.com/jquery-2.1.0.min.js"></script>
<script type="text/javascript">
$(function(){
 setTimeout(function(){
  $(window).scroll(function() {
      var y_scroll_position = window.pageYOffset;
      var scroll_position = 500; //Sets number of pixels user must scroll to be tracked

  if(y_scroll_position > scroll_position) {
  //Munchkin tracking code
   (function() {
     var didInit = false;
     function initMunchkin() {
      if(didInit === false) {
        didInit = true;
        Munchkin.init('XXX-XXX-XXX');
      }
     }

     var s = document.createElement('script');
     s.type = 'text/javascript';
     s.async = true;
    s.src = '//munchkin.marketo.net/munchkin.js';
     s.onreadystatechange = function() {
      if (this.readyState == 'complete' || this.readyState == 'loaded') {
          initMunchkin();
      }
     };
     s.onload = initMunchkin;
     document.getElementsByTagName('head')[0].appendChild(s);
   })();
   }
 },5000); //Sets time delay before tracking user
});
</script>
```

**Come verificare che la visita alla pagina da parte di un utente anonimo sia stata registrata in Marketo**

1. Fai clic su Analytics nel menu principale, quindi fai clic su Nuovo rapporto. Scegliere Attività pagina Web come tipo di report, quindi assegnare un nome al report.
1. Dopo aver creato un report, fare clic su Smart List. Seleziona quindi il filtro Pagina web visitata dalla casella a destra. Inserisci la pagina web in cui inserisci il codice di tracciamento di Munchkin.
1. Fai clic su Configurazione. Seleziona Visitatori anonimi da ISP e cambia l’opzione in Mostrato.
1. Fai clic su Report. Ora potrai vedere l’attività tracciata sulla pagina web selezionata.
1. Fai doppio clic sul record del lead, che mostrerà quindi il registro attività in cui puoi visualizzare la pagina specifica visitata dall’utente anonimo.

Questo articolo contiene il codice utilizzato per implementare integrazioni personalizzate. A causa della sua natura personalizzata, il team del supporto tecnico Marketo non è in grado di risolvere i problemi relativi al lavoro personalizzato. Non tentare di implementare il seguente esempio di codice senza un’adeguata esperienza tecnica o l’accesso a uno sviluppatore esperto.

Per le pagine con contenuti multimediali, ad esempio, può essere utile eseguire il tracciamento personalizzato. Un esempio comune è quello di aggiungere alla pagina il codice di tracciamento Munchkin e utilizzare l’API Munchkin per generare eventi nell’istanza Marketo per attività quali la riproduzione di un video o l’ascolto di una clip audio. È consigliabile inserire il codice di tracciamento di Munchkin nella maggior parte o in tutte le pagine Web. Il codice di tracciamento di Munchkin viene incluso automaticamente nelle pagine di destinazione create con Marketo. Utilizza questa chiamata per registrare che l’utente ha eseguito un’operazione, ad esempio visitare una pagina in Ajax, Flash o un altro ambiente RIA. L’URL non deve contenere &quot;né alcun dominio, ma può puntare a qualsiasi pagina, anche a pagine che non esistono. Se si desidera aggiungere parametri URL, utilizzare l&#39;argomento params.
L’evento viene visualizzato come evento Visita pagina web nel registro delle attività dell’utente sotto il dominio della pagina web chiamante. Nota La prima chiamata a `mktoMunchkin()` crea sempre un evento Visita pagina Web per la pagina corrente. Non è necessario chiamare `visitWebPage` a meno che non si desideri tenere traccia di un&#39;ulteriore visita a una pagina Web. `mktoMunchkinFunction('visitWebPage', { url: '/MyFlashMovie/Story1', params: 'x=y&2=3' });` Nota Assicurati di avere accesso a uno sviluppatore JavaScript esperto. Il supporto tecnico Marketo non è configurato per fornire assistenza nella risoluzione dei problemi relativi al JavaScript personalizzato. L’API JavaScript di Munchkin ti consente di integrare un sistema web di terze parti con il tuo account Marketo. Con alcuni sviluppi web, puoi acquisire nuovi lead o aggiornare i lead correnti con le applicazioni esistenti sul tuo sito web. Supponiamo che tu disponga di un’applicazione web per la registrazione dei clienti che acquisisce nuove informazioni sui clienti. Con solo un po’ di programmazione, puoi anche avere le informazioni sui lead per quegli utenti acquisite in Marketo e un cookie Marketo impostato per il tracciamento web futuro.

Inoltre, un’altra funzione consente agli sviluppatori web di acquisire e tenere traccia delle informazioni sulle attività web da ambienti web avanzati, come Flash o Ajax. Nota: se disponi delle risorse di sviluppo appropriate, dovresti considerare l’utilizzo della nostra API SOAP per l’integrazione anziché di questa API. L’API di SOAP è più solida e offre più funzionalità rispetto all’API di Munchkin. Requisiti API di Marketo SOAP Affinché tutto funzioni, devi includere nella pagina web il codice JavaScript di Munchkin. Puoi trovare i tag script necessari nell’esercitazione di Munchkin. Abilita anche l’API Munchkin, descritta anche nell’esercitazione.
Sotto il Riempimento Dopo aver effettuato una chiamata API Munchkin, l’utente esegue automaticamente i cookie se non dispone di un cookie. In Marketo, registra l’evento (fai clic su un collegamento, visita una pagina web o un nuovo lead) nel registro attività della persona. Se utilizzi il collegamento di clic o visiti una chiamata a una pagina web, l’evento viene aggiunto al registro delle attività del lead (noto o anonimo). Se si tratta di un nuovo lead e si utilizza la chiamata associa lead, tale lead diventa un lead noto e la cronologia delle relative attività verrà mantenuta. Se si tratta di un lead esistente (in base alla corrispondenza dell’indirizzo e-mail), eventuali valori modificati o nuovi verranno aggiornati nel record del lead.

Ecco la forma generale di una chiamata `munchkinFunction`. Includilo come tag nella pagina web ovunque desideri chiamarlo. È possibile richiamarlo come qualsiasi altra funzione di JavaScript. Tuttavia, è necessario chiamare la funzione di tracciamento di Munchkin `mktoMunchkin()` prima di effettuare qualsiasi chiamata `mktoMunchkinFunction()`:

```javascript
<script src="http://munchkin.marketo.net/munchkin.js" type="text/javascript"> mktoMunchkin("###-###-###"); mktoMunchkinFunction('function', { key: 'value', key2: 'value'}, 'hash');
```

Dove: `###-###-###` è l&#39;ID account Munchkin per la funzione account è la chiamata che si desidera impostare come parametri un array di parametri necessari per l&#39;hash della chiamata è necessario solo per `associateLead`

Pubblicato il _1970-01-01_ da _Murta_

## Come inserire dati in Marketo

La presentazione seguente illustra i diversi modi in cui si possono inserire i dati in Marketo. Si concentra su moduli, oggetti personalizzati e integrazioni.

[Come inserire dati in Marketo](https://www.slideshare.net/MurtzaManzur/getting-data-into-marketo-35662408) da [Murtza Manzur](https://www.slideshare.net/MurtzaManzur)

Pubblicato il _2014-06-06_ da _Murta_

## Creazione di lead in un Workspace

Supponiamo che la tua azienda abbia due divisioni: Nord America ed Europa. Desideri segmentare i lead in base alla divisione aziendale in Marketo. Puoi eseguire questa operazione utilizzando le aree di lavoro, una funzione di Marketo che consente di limitare l’accesso ai lead. Per fare questo, si creerebbe uno spazio di lavoro per il Nord America e un altro per l&#39;Europa. Puoi quindi creare un lead in una particolare area di lavoro utilizzando l&#39;[API syncLead](/help/soap-api/synclead.md). È consigliabile utilizzare le aree di lavoro e le partizioni dei lead se l’organizzazione dispone di:

1. Team di marketing separati per più linee di prodotti
1. Team di marketing separati per aree geografiche o paesi diversi
1. Una società madre e società controllate
1. Una società madre e rivenditori

Quando si utilizzano le partizioni lead e le aree di lavoro, è possibile:

1. Limita l’accesso ai lead nella tua organizzazione
1. Limitare l’accesso alle risorse dell’organizzazione
1. Condividere risorse tra i team di marketing

Ti mostriamo innanzitutto come creare un&#39;area di lavoro in Marketo tramite l&#39;interfaccia utente e in secondo luogo come scrivere un lead in tale area di lavoro utilizzando l&#39;[API syncLead](/help/soap-api/synclead.md). **Creazione di un Workspace** Un&#39;area di lavoro è un insieme di lead e risorse Marketo. In un’area di lavoro, puoi visualizzare solo i lead provenienti da tale area di lavoro e le relative risorse (e-mail, campagne, elenchi, ecc.). Le campagne intelligenti in tale area di lavoro influiscono solo sui lead presenti in tale area di lavoro. Per visualizzare le aree di lavoro nel tuo account:

1. Vai alla pagina Aree di lavoro e partizioni lead della sezione Amministratore. Le aree di lavoro vengono visualizzate nella scheda Aree di lavoro. 1. Per creare un nuovo workspace, fare clic sul pulsante Nuovo Workspace nella barra dei menu della scheda Workspace.
1. Nella finestra di dialogo, devi aggiungere alcune informazioni sulla nuova area di lavoro:

* **Nome Workspace**: il nome dell&#39;area di lavoro visualizzato nell&#39;interfaccia
* **Descrizione**: una descrizione testuale facoltativa dell&#39;area di lavoro
* **Partizioni lead**. I lead sono visibili in questa partizione.
* **Tutte le partizioni lead** - vedranno i lead da tutte le partizioni presenti e future
* **Seleziona singole partizioni** - sono visibili solo i lead di tali partizioni (e nessuna partizione futura)
* **Partizione lead primaria** - i lead che visitano le pagine di destinazione vengono aggiunti a questa partizione per impostazione predefinita
* **Lingua** - lingua aziendale dell&#39;area di lavoro

Ti mostriamo come utilizzare i lead di scrittura API in una particolare area di lavoro. Il primo esempio è una richiesta XML, il secondo è una risposta XML e l&#39;ultimo è un esempio di codice Ruby che può essere utilizzato per generare la richiesta XML. 1. Dopo aver creato il lead, la partizione del lead è un campo in Informazioni lead. Richiesta SOAP per `requestCampaign`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:ns1="<http://www.marketo.com/mktows/">
  <SOAP-ENV:Header>
    <ns1:AuthenticationHeader>
      <mktowsUserId>demo17_1_809939944BFABAE58E5D27</mktowsUserId><requestSignature>48397ad47b71a1439f13a51eea3137df46441979</requestSignature><requestTimestamp>2013-08-01T12:31:14-07:00</requestTimestamp>
    </ns1:AuthenticationHeader>
  </SOAP-ENV:Header>
  <SOAP-ENV:Body>
    <ns1:paramsRequestCampaign>
      <source>MKTOWS</source>
      <campaignId>4496</campaignId>
      <leadList>
        <leadKey>
          <keyType>EMAIL</keyType>
          <keyValue>lead@company.com</keyValue>
        </leadKey>
        <leadKey>
          <keyType>EMAIL</keyType>
          <keyValue>anotherlead@company.com</keyValue>
        </leadKey>
      </leadList>
    </ns1:paramsRequestCampaign>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

Risposta di SOAP per requestCampaign

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:ns1="<http://www.marketo.com/mktows/">
  <SOAP-ENV:Body>
    <ns1:successRequestCampaign>
      <result>
        <success>true</success>
      </result>
    </ns1:successRequestCampaign>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

Di seguito è riportato un programma Java di esempio che esegue lo scenario descritto in precedenza.

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


public class RequestCampaign {

    public static void main(String[] args) {
        System.out.println("Executing Request Campaign");
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
            ParamsRequestCampaign request = new ParamsRequestCampaign();

            request.setSource(ReqCampSourceType.MKTOWS);

            ObjectFactory objectFactory = new ObjectFactory();
            JAXBElement<Integer> campaignId = objectFactory.createParamsRequestCampaignCampaignId(4496);
            request.setCampaignId(campaignId);

            ArrayOfLeadKey leadKeyList = new ArrayOfLeadKey();
            LeadKey key = new LeadKey();
            key.setKeyType(LeadKeyRef.EMAIL);
            key.setKeyValue("lead@company.com");

            LeadKey key2 = new LeadKey();
            key2.setKeyType(LeadKeyRef.EMAIL);
            key2.setKeyValue("anotherlead@company.com");

            leadKeyList.getLeadKeies().add(key);
            leadKeyList.getLeadKeies().add(key2);

            JAXBElement<ArrayOfLeadKey> arrayOfLeadKey = objectFactory.createParamsRequestCampaignLeadList(leadKeyList);
            request.setLeadList(arrayOfLeadKey);

            SuccessRequestCampaign result = port.requestCampaign(request, header);

            JAXBContext context = JAXBContext.newInstance(SuccessRequestCampaign.class);
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

**Come posso iscrivermi alle aree di lavoro e alle partizioni lead?** Per i clienti Marketo Enterprise, è possibile accedere gratuitamente alle aree di lavoro e alle partizioni dei lead. Contatta il tuo responsabile dell’abilitazione dei clienti per informazioni dettagliate sull’abilitazione e sull’implementazione di tali funzioni. Per gli altri clienti, contattare il responsabile vendite Marketo o inviare un&#39;e-mail al team di vendita per ulteriori informazioni su questo prodotto.

Questo articolo contiene il codice utilizzato per implementare integrazioni personalizzate. A causa della sua natura personalizzata, il team del supporto tecnico Marketo non è in grado di risolvere i problemi relativi al lavoro personalizzato. Non tentare di implementare il seguente esempio di codice senza un’adeguata esperienza tecnica o l’accesso a uno sviluppatore esperto.

Pubblicato il _1970-01-01_ da _Murta_

## Aggiornamenti sulla versione di giugno 2014

### API Personalization in tempo reale di Marketo

L’API JavaScript di Marketo Real-Time Personalization (RTP) estende la funzionalità di personalizzazione automatizzata della piattaforma. Consente il tracciamento degli eventi e la personalizzazione dinamica di una pagina web. Consulta la documentazione completa [qui](/help/javascript-api/web-personalization.md).

Pubblicato il _2014-06-20_ da _Travis Kaufman_

## Memorizzazione di una chiave esterna in Marketo

Quando si sincronizzano i record dei contatti e dei lead tra sistemi quali un CRM proprietario o un data warehouse, è spesso necessario associare un record dei lead a un identificativo di sistema univoco. In Marketo è possibile creare o aggiornare un record di lead tramite una chiamata API [syncMultipleLeads](/help/soap-api/syncmultipleleads.md) utilizzando l&#39;identificatore di sistema univoco. A tal fine, l’identificatore di sistema univoco (chiave primaria) viene memorizzato come chiave esterna in Marketo. Il nome di questo campo in Marketo per memorizzare una chiave esterna è foreignSysPersonId. Ecco tre aspetti importanti da considerare:

1. foreignSysPersonId non è visibile nell’interfaccia utente di Marketo. Pertanto, è consigliabile compilare anche un campo attributo personalizzato con questo valore.
1. foreignSysPersonId è univoco per un lead, ma un lead può avere più di un foreignSysPersonId.
1. foreignSysPersonId non può essere aggiornato o eliminato, ma può essere riassegnato a un altro record.

Viene illustrato come effettuare una chiamata all&#39;API [syncMultipleLeads](/help/soap-api/syncmultipleleads.md) per scrivere un valore foreignSysPersonId in due record di lead esistenti in Marketo. **Come scrivere foreignSysPersonId utilizzando l&#39;API syncMultipleLeads** È possibile inserire un nuovo record di lead e specificare foreignSysPersonId. Puoi anche aggiungerlo a un lead esistente specificando sia l’ID Marketo che l’ID foreignSysPersonId. Vi guideremo attraverso quest&#39;ultimo caso. **Richiedi XML per la chiamata API SOAP syncMultipleLeads**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:ns1="<http://www.marketo.com/mktows/">
  <SOAP-ENV:Header>
    <ns1:AuthenticationHeader>
      <mktowsUserId>demo17_1_809934544BFABAE58E5D27</mktowsUserId>
      <requestSignature>b5e21953ae9f1b263e644da5eccce9ff33802513</requestSignature>
      <requestTimestamp>2013-08-01T18:22:30-07:00</requestTimestamp>
    </ns1:AuthenticationHeader>
  </SOAP-ENV:Header>
  <SOAP-ENV:Body>
    <ns1:paramsSyncMultipleLeads>
      <leadRecordList>
        <leadRecord>
          <leadId>1090240</leadId>
          <foreignSysPersonId>1224191</foreignSysPersonId>
          <leadAttributeList>
            <attribute>
              <attrName>Company</attrName>
              <attrValue>Marketo1000</attrValue>
            </attribute>
            <attribute>
              <attrName>Phone</attrName>
              <attrValue>650-555-1000</attrValue>
            </attribute>
          </leadAttributeList>
        </leadRecord>
        <leadRecord>
          <leadId>1090239</leadId>
          <foreignSysPersonId>1224192</foreignSysPersonId>
          <leadAttributeList>
            <attribute>
              <attrName>Company</attrName>
              <attrValue>Marketo1001</attrValue>
            </attribute>
            <attribute>
              <attrName>Phone</attrName>
              <attrValue>650-555-1001</attrValue>
            </attribute>
          </leadAttributeList>
        </leadRecord>
      </leadRecordList>
      <dedupEnabled>true</dedupEnabled>
    </ns1:paramsSyncMultipleLeads>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

**XML di risposta per la chiamata API SOAP syncMultipleLeads**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:xsi="<http://www.w3.org/2001/XMLSchema-instance>" xmlns:ns1="<http://www.marketo.com/mktows/">
  <SOAP-ENV:Body>
    <ns1:successSyncMultipleLeads>
      <result>
        <syncStatusList>
          <syncStatus>
            <leadId>1090240</leadId>
            <status>UPDATED</status>
            <error xsi:nil="true" />
          </syncStatus>
          <syncStatus>
            <leadId>1090239</leadId>
            <status>UPDATED</status>
            <error xsi:nil="true" />
          </syncStatus>
        </syncStatusList>
      </result>
    </ns1:successSyncMultipleLeads>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

**Di seguito è riportato un esempio di programma Ruby che restituirà l&#39;XML della richiesta precedente.**

```java
require 'savon' # Use version 2.0 Savon gem
require 'date'

mktowsUserId = "" # CHANGE ME
marketoSecretKey = "" # CHANGE ME
marketoSoapEndPoint = "" # CHANGE ME
marketoNameSpace = "<http://www.marketo.com/mktows/>"

# Create Signature
Timestamp = DateTime.now
requestTimestamp = Timestamp.to_s
encryptString = requestTimestamp + mktowsUserId
digest = OpenSSL::Digest.new('sha1')
hashedsignature = OpenSSL::HMAC.hexdigest(digest, marketoSecretKey, encryptString)
requestSignature = hashedsignature.to_s

# Create SOAP Header
headers = {
 'ns1:AuthenticationHeader' => { "mktowsUserId" => mktowsUserId, "requestSignature" => requestSignature,
 "requestTimestamp"  => requestTimestamp
 }
}

client = Savon.client(wsdl: '<http://app.marketo.com/soap/mktows/2_3?WSDL>', soap_header: headers, endpoint: marketoSoapEndPoint, open_timeout: 90, read_timeout: 90, namespace_identifier: :ns1, env_namespace: 'SOAP-ENV')

# Create Request
request = {
        :lead_record_list => {
                :lead_record => {
                        :lead_id => "1090240",
                        :foreign_sys_person_id => "1224191",
                :lead_attribute_list => {
                        :attribute => {
                                :attr_name => "Company",
                                :attr_value => "Marketo1000" },
                         :attribute! => {
                                :attr_name => "Phone",
                                :attr_value => "650-555-1000" }
                }
        },
        :lead_record! => {
                        :lead_id => "1090239",
                        :foreign_sys_person_id => "1224192",
                :lead_attribute_list => {
                        :attribute => {
                                :attr_name => "Company",
                                :attr_value => "Marketo1001" },
                         :attribute! => {
                                :attr_name => "Phone",
                                :attr_value => "650-555-1001" }
                }
        }
        },
    :dedup_enabled => "True"
}

response = client.call(:sync_multiple_leads, message: request)

puts response
```

Questo articolo contiene il codice utilizzato per implementare integrazioni personalizzate. A causa della sua natura personalizzata, il team del supporto tecnico Marketo non è in grado di risolvere i problemi relativi al lavoro personalizzato. Non tentare di implementare il seguente esempio di codice senza un’adeguata esperienza tecnica o l’accesso a uno sviluppatore esperto.

Pubblicato il _2014-06-27_ da _Murta_

## Aggiornamento dell&#39;indirizzo e-mail di un lead

Supponiamo che un utente compili un modulo di Marketo sul sito. Cosa succede? Marketo esegue il cookie dell’utente e lo associa all’e-mail fornita. Cosa succede se alla prossima visita del sito web l’utente compila di nuovo lo stesso modulo con un’e-mail diversa? Cosa succederà? Marketo creerà un nuovo record di lead e sovrascriverà il primo cookie sul browser dell’utente. L’utente è ora un lead nuovo/diverso in Marketo. Vengono illustrati quattro modi per aggiornare l&#39;indirizzo e-mail di un lead in Marketo, tra cui il metodo API [syncLead](/help/soap-api/synclead.md), il campo personalizzato in un metodo di modulo, l&#39;interfaccia utente di Marketo e l&#39;importazione di un elenco. **Tramite l&#39;API syncLead** è possibile utilizzare l&#39;API [syncLead](/help/soap-api/synclead.md) per aggiornare un record lead utilizzando il relativo ID Marketo e il nuovo indirizzo e-mail. Richiedi XML per la chiamata API SOAP `syncMultipleLeads`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:ns1="<http://www.marketo.com/mktows/">
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
        <leadId>1090240</leadId>
        <Email>t@t.com</Email>
      </leadRecord>
      <returnLead>false</returnLead>
    </ns1:paramsSyncLead>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

XML di risposta per la chiamata API SOAP syncMultipleLeads

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:xsi="<http://www.w3.org/2001/XMLSchema-instance>" xmlns:ns1="<http://www.marketo.com/mktows/">
  <SOAP-ENV:Body>
    <ns1:successSyncLead>
      <result>
        <leadId>1090240</leadId>
        <syncStatus>
          <leadId>1090240</leadId>
          <status>UPDATED</status>
          <error xsi:nil="true" />
        </syncStatus>
        <leadRecord xsi:nil="true" />
      </result>
    </ns1:successSyncLead>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

Di seguito è riportato un esempio di programma Ruby che genererà l’XML della richiesta.

```java
require 'savon' # Use version 2.0 Savon gem
require 'date'

mktowsUserId = "" # CHANGE ME
marketoSecretKey = "" # CHANGE ME
marketoSoapEndPoint = "" # CHANGE ME
marketoNameSpace = "<http://www.marketo.com/mktows/>"

# Create Signature
Timestamp = DateTime.now
requestTimestamp = Timestamp.to_s
encryptString = requestTimestamp + mktowsUserId
digest = OpenSSL::Digest.new('sha1')
hashedsignature = OpenSSL::HMAC.hexdigest(digest, marketoSecretKey, encryptString)
requestSignature = hashedsignature.to_s

# Create SOAP Header
headers = {
 'ns1:AuthenticationHeader' => { "mktowsUserId" => mktowsUserId, "requestSignature" => requestSignature,
 "requestTimestamp"  => requestTimestamp
 }
}

client = Savon.client(wsdl: '<http://app.marketo.com/soap/mktows/2_3?WSDL>', soap_header: headers, endpoint: marketoSoapEndPoint, open_timeout: 90, read_timeout: 90, namespace_identifier: :ns1, env_namespace: 'SOAP-ENV')

# Create Request
request = {
 :lead_record => {
  :Email => "<t@t.com>",
  :lead_id => "1090240",
 :return_lead => "false"
}

response = client.call(:sync_lead, message: request)

puts response
```

**Tramite un campo personalizzato in un modulo** è possibile creare un campo personalizzato per il &quot;Nuovo indirizzo e-mail&quot; in Marketo. Quindi chiedi all’utente di compilare un modulo che includa questo nuovo campo. Quindi crea un programma in Marketo che modificherebbe il valore dei dati del campo dell&#39;indirizzo e-mail di sistema con il token `{{lead.newEmailAddress}}` quando si modifica il nuovo campo personalizzato &quot;Nuovo indirizzo e-mail&quot;. **Tramite l&#39;interfaccia utente di Marketo** Puoi aggiornare manualmente l&#39;indirizzo e-mail di un lead tramite l&#39;interfaccia utente di Marketo. Di seguito è riportato un [articolo della guida](https://nation.marketo.com/) che descrive come eseguire questa operazione (per visualizzare l&#39;articolo è necessario disporre dell&#39;accesso a Marketo). **Importazione di un elenco** Puoi aggiornare l&#39;indirizzo e-mail di un lead utilizzando il metodo import a list in Marketo descritto [qui](https://nation.marketo.com/) (per visualizzare l&#39;articolo è necessario disporre dell&#39;accesso a Marketo).  

Questo articolo contiene il codice utilizzato per implementare integrazioni personalizzate. A causa della sua natura personalizzata, il team del supporto tecnico Marketo non è in grado di risolvere i problemi relativi al lavoro personalizzato. Non tentare di implementare il seguente esempio di codice senza un’adeguata esperienza tecnica o l’accesso a uno sviluppatore esperto.

Pubblicato il _1970-01-01_ da _Murta_

## Memorizza un secondo indirizzo e-mail per un lead

Supponiamo che tu voglia modificare il punteggio di un lead in Marketo utilizzando le API. Questa operazione può essere eseguita con l’API REST utilizzando l’endpoint Create/Update Lead. Se desideri memorizzare più di un’e-mail in un record lead, devi creare un campo personalizzato e memorizzare lì la seconda e-mail. Di seguito è riportato un articolo della guida con ulteriori informazioni: di seguito è riportato un esempio di codice in Ruby che mostra come effettuare questa chiamata.

```java
require 'rest_client'
require 'json'

# Build request URL
# Replace AAA-BBB-CCC with your Marketo instance
marketo_instance = "<https://AAA-BBB-CCC.mktorest.com>"
endpoint = "/rest/v1/leads.json"
# Replace with your access token
auth_token =  "?access_token=" + "ac756f7a-d54d-41ac-8c3c-f2d2a39ee325:ab"
request_url = marketo_instance + endpoint + auth_token

# Build request body
data = { "action" => "updateOnly", "input" => [ { "email" => "<example@email.com>", "leadScore" => "30" } ] }

# Make request
response = RestClient.post request_url, data.to_json, :content_type => :json, :accept => :json

# Returns Marketo API response
puts response
```

Pubblicato il _2015-02-20_ da _Murta_

## Creare un campo personalizzato in Marketo e aggiornare il campo tramite Personalizzazione automatizzata

Supponiamo che tu abbia dati aggiuntivi sui lead che non rientrano nei campi standard di Marketo. Ad esempio, questo campo personalizzato potrebbe essere un punteggio di terze parti. Puoi creare un campo personalizzato in Marketo per il tuo punteggio di terze parti, quindi aggiornare il valore di questo campo tramite le [API REST](https://developer.adobe.com/marketo-apis/) di Marketo o le [API SOAP](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/soap/activity-type-filters). Viene mostrato innanzitutto come creare un campo personalizzato in Marketo e quindi come aggiornare questo campo utilizzando l’API REST.

### Come creare un campo personalizzato in Marketo

1. In Amministratore, fai clic su Gestione campi.
1. Fare clic sul pulsante Nuovo campo personalizzato.
1. Scegliere il tipo di campo. Questo cambia il modo in cui viene riprodotto negli elenchi avanzati e in Forms in Marketo.
1. Immetti il Nome come desideri che appaia in Marketo. Scegli con attenzione il Nome e il Nome API, in quanto rinominare i campi può essere difficile e in alcune situazioni non possibile.
1. Il campo personalizzato creato è ora accessibile tramite le API.

### Come aggiornare il campo personalizzato tramite API REST

Nella sezione precedente è stato creato un campo personalizzato denominato `myCustomField` con la stringa del tipo di dati. Per aggiornare il valore di tale campo, utilizziamo l’endpoint REST API denominato Create/Update Leads. Prima di poter effettuare una richiesta all’API REST, è necessario eseguire l’autenticazione. Questo non rientra nell&#39;ambito di questo articolo, ma informazioni dettagliate [sono disponibili nel sito per sviluppatori di Marketo](/help/rest-api/authentication.md).

**Endpoint**

`/rest/v1/leads.json`

**Corpo richiesta**

```json
{
   "action":"createOrUpdate",
   "input":[
      {
         "email":"<example@example.com>",
         "myCustomField":"examplestring"
      }
   ]
}
```

Questo articolo contiene il codice utilizzato per implementare integrazioni personalizzate. A causa della sua natura personalizzata, il team del supporto tecnico Marketo non è in grado di risolvere i problemi relativi al lavoro personalizzato. Non tentare di implementare il seguente esempio di codice senza un’adeguata esperienza tecnica o l’accesso a uno sviluppatore esperto.

Pubblicato il _2014-08-19_ da _Murta_

## Integrazione di Unbounce e Marketo

**NOTA: questo è un post di Fab Capodicasa. È un consulente certificato Marketo presso [Hoosh Marketing](http://hooshmarketing.com.au/), un partner Marketo LaunchPoint specializzato in B2C. Negli ultimi 13 anni ha lavorato sia in SaaS che nel marketing. Il suo background è una combinazione di IT, marketing diretto e vendite aziendali. Fab è anche un ex dipendente Marketo.**

**Panoramica** In questo articolo viene illustrato come integrare Unbounce, un popolare strumento per pagine di destinazione, con Marketo. Viene mostrato come inserire il tracciamento di Marketo in Unbounce e come modificare i moduli Unbounce per inserire i dati direttamente in Marketo. Il problema dell’integrazione di Unbounce con Marketo è che Unbounce non consente la ridenominazione dei campi predefiniti (ad esempio, first_name non può essere modificato in FirstName). Inoltre, non consente che le etichette dei campi siano diverse dai nomi dei campi. Questa integrazione coinvolge JavaScript che modifica i moduli esistenti per renderli compatibili con Marketo. È consigliabile avere almeno un livello iniziale di JavaScript e un livello intermedio di conoscenza di Marketo per completare le attività descritte in questo articolo.
**Parte 1: aggiungi codice di tracciamento Marketo per annullare la remissione** Per il funzionamento dell&#39;integrazione di Analytics e del modulo è necessario aggiungere lo script di tracciamento Munchkin di Marketo alle pagine non recapitate. Segui questi passaggi: Copia il codice Munchkin da Marketo: passa a Amministratore -> Munchkin e copia la versione &quot;Semplice&quot; di JavaScript. Apri la pagina di destinazione Unbounce e fai clic su JavaScript-> Aggiungi nuovo JavaScript.  Fai clic su Aggiungi, chiama lo script &quot;Munchkin&quot;, seleziona &quot;Prima del tag di fine corpo&quot; e quindi incolla il codice Munchkin. Fare clic sul pulsante Fine. Per le pagine non recapitate future, vai a JavaScript e abilita lo script Munchkin che abbiamo creato. Non è necessario ricrearlo.
**Parte 2: converti il modulo per l&#39;annullamento dell&#39;e-mail non recapitate in un Marketo Form** Ora è necessario modificare il modulo per l&#39;annullamento dell&#39;e-mail aggiungendo alcuni nuovi campi nascosti e JavaScript per consentire alle pagine di destinazione per l&#39;annullamento dell&#39;e-mail di inviare informazioni sul lead direttamente in Marketo. Verrà innanzitutto creato un modulo segnaposto di Marketo. In Marketo, crea un modulo vuoto e approvalo.

Questo è il modulo proxy in Marketo che rappresenta il modulo Unbounce. Aggiungi campi nascosti al modulo per l’annullamento della notifica. Questi campi nascosti sono richiesti da Marketo per determinare a quale istanza di Marketo, a quale modulo e sessione utente verrà applicato l’invio del modulo. In Non recapitare, aprire il modulo facendo doppio clic su. Aggiungere un campo nascosto denominato `_mkt_trk`. Aggiungere un secondo campo nascosto denominato `formid`.  233 deve essere sostituito con l’id del modulo, reperibile nel codice di incorporamento del modulo Marketo in Marketo. In Marketo, Apri il modulo, seleziona Azioni modulo->Incorpora codice. Aggiungere un campo nascosto denominato `returnurl`. `http://hooshmarketing.com.au/thank-you` deve essere sostituito con l&#39;URL di completamento. Si tratta dell&#39;URL a cui devono essere reindirizzati gli utenti dopo l&#39;invio del modulo. Ad esempio, questa potrebbe essere la tua pagina di ringraziamento.
**Parte 3: modulo per l&#39;annullamento del recapito diretto a Marketo** L&#39;URL di follow-up è la pagina a cui verrà reindirizzato il lead dopo l&#39;invio del lead a Marketo. In Unbounce, segui questi passaggi: fai clic sul modulo. Modifica la sezione Conferma modulo. Modifica l’opzione di conferma per pubblicare i dati del modulo in un URL. Impostare l&#39;URL sulla pagina di follow-up desiderata. `fpmarkets` deve essere sostituito con la stringa del tuo account Marketo, che si trova in Marketo in Amministratore->Pagine di destinazione.
**Parte 4: aggiungi JavaScript alla pagina non recapitata** Questo JavaScript convertirà il modulo in modo che sia compatibile con Marketo e lo invierà a Marketo. In Unbounce, segui questi passaggi: apri la pagina di destinazione Unbounce e fai clic su JavaScript-> Aggiungi nuovo JavaScript. Fare clic su Aggiungi, chiamare lo script &#39;Marketo Form Convert&#39;, selezionare &#39;Before Body End Tag&#39;. Incolla il codice JavaScript seguente:

```javascript
var MARKETO_MUNCHKIN_ID='614-CGT-700';
var MARKETO_ACCOUNT_STRING = 'fpmarkets';

var UNBOUNCE_MARKETO_FIELD_MAP = new Object();

//default field mappings
UNBOUNCE_MARKETO_FIELD_MAP['first_name'] = 'FirstName';
UNBOUNCE_MARKETO_FIELD_MAP['email'] = 'Email';
UNBOUNCE_MARKETO_FIELD_MAP['last_name'] = 'LastName';
UNBOUNCE_MARKETO_FIELD_MAP['phone_number'] = 'Phone';

function getMarketoField(k) {
    return UNBOUNCE_MARKETO_FIELD_MAP[k];
}


var formFields = [];
var hiddenClonedFields = [];
var firstForm = document.forms[0];

//Convert Unbounce form names to Marketo field names
for(i=0; i<firstForm.elements.length; i++){
 var formField = firstForm.elements[i];
 var newFieldName = getMarketoField(formField.name);

  if(newFieldName != undefined) {


    //save original field as hidden field
    var hiddenField = document.createElement("input");
    hiddenField.setAttribute("type", "hidden");
    hiddenField.setAttribute("name", formField.name);
    hiddenField.setAttribute("id", formField.id);
    hiddenClonedFields.push(hiddenField);

    //change original field
    console.log ( 'Changed form field name: ' + formField.name + '=>' + newFieldName );
    formField.name = newFieldName;
    formField.id = newFieldName;
    formFields.push(formField);


  } else {
    console.log ( 'Couldn't map:' + formField.name );
  }
}

//Add hidden cloned Unbounce fields to form
//for Unbounce validation
for(i=0; i<hiddenClonedFields.length; i++){
    firstForm.appendChild(hiddenClonedFields[i]);
    formFields[i].onchange = (function(hf) {
        return function(event) {
            hf.value = event.target.value;
          console.log('Changed hidden cloned field:' + hf.name + '=>' + hf.value);
        };
    }(hiddenClonedFields[i]));
    console.log ( 'Added cloned field: ' + hiddenClonedFields[i].name );
}


//Add MunchkinId to form
var input = document.createElement("input");
input.setAttribute("type", "hidden");
input.setAttribute("name", "munchkinId");
input.setAttribute("value", MARKETO_MUNCHKIN_ID);
firstForm.appendChild(input);
console.log('Added hidden field:' + input.name + '=' + input.value);
```

Se disponi di campi personalizzati con nomi API non compatibili con Unbounce, puoi aggiungerli alla mappatura in JavaScript. Se, ad esempio, uno dei campi personalizzati in Marketo era `Comments_c`, ma si desiderava che l&#39;etichetta del campo fosse visualizzata come Commenti, l&#39;opzione Annulla recapito non consentiva di modificare il nome del campo in `Comments_c`.

```
//default field mappings
UNBOUNCE_MARKETO_FIELD_MAP['comments'] = 'Comments_c';
UNBOUNCE_MARKETO_FIELD_MAP['first_name'] = 'FirstName';
UNBOUNCE_MARKETO_FIELD_MAP['email'] = 'Email';
```

_comments è il nome del campo in Unbounce. _Comments_c_ è il nome del campo in Marketo. Per le pagine non recapitate future, accedi semplicemente a JavaScript e abilita lo script Munchkin che abbiamo creato. Non è necessario ricrearlo.
**Parte 5: test** L&#39;ultimo passaggio consiste nel verificare il funzionamento dell&#39;integrazione del modulo. Crea un trigger in Marketo che si attiva durante la compilazione del modulo di Marketo e assicurati che i lead vengano inseriti correttamente in Marketo. Dopo l’invio del modulo, la pagina deve essere reindirizzata all’URL di follow-up.

Postato il _2014-08-04_ da _

## Aggiornamenti della versione di luglio 2014

### Aggiornamenti Munchkin

La nuova versione di Munchkin è la 141. La versione 142 non è supportata e verrà rimossa all&#39;inizio di settembre 2011. Nella versione 142, c&#39;erano funzioni accessibili pubblicamente che non erano documentate sul sito degli sviluppatori. Tali funzioni non documentate non sono più disponibili al pubblico. Le funzioni documentate sul sito degli sviluppatori continueranno a essere supportate a lungo termine.

### Aggiornamenti RTP

L’API RTP dispone di una nuova funzione denominata Ottieni dati visitatore. Questa chiamata API RTP ti consente di ottenere dati sui visitatori in tempo reale, come organizzazione, settore, posizione e corrispondenza del codice segmento.

Pubblicato il _2014-07-30_ da _Murta_

## Utilizzo di più codici di tracciamento Munchkin in una singola pagina

Supponiamo che tu disponga di più istanze di Marketo e che desideri inviare eventi di tracciamento web come visite di pagina o collegamenti selezionati a queste istanze multiple, è possibile farlo con Munchkin. Marketo tiene traccia dei visitatori del sito web per dominio (ad esempio, &quot;marketo.com&quot;). Se ospiti questo script Munchkin su un dominio diverso da quello principale (ad esempio &quot;company.com&quot;), i visitatori verranno visualizzati come lead anonimi fino a quando non compileranno un modulo su tale altro dominio. A questo scopo, aggiungi il parametro `altIds` alla chiamata `Munchkin.init`. Il parametro altIds contiene un array degli ID Munchkin aggiuntivi a cui devono essere inviati gli eventi web. Nell’esempio seguente, sostituisci gli ID Munchkin evidenziati (XXX-XXX-XXX, YYYY-YYY e ZZZ-ZZZ-ZZZ) con gli ID Munchkin di ogni istanza Marketo a cui devono essere inviate le informazioni di tracciamento.

```javascript
<script src="http://munchkin.marketo.net/munchkin.js" type="text/javascript"></script>
<script>Munchkin.init('XXX-XXX-XXX', { altIds:['YYY-YYY-YYY', 'ZZZ-ZZZ-ZZZ'] });</script>
```

Per ulteriori informazioni sui parametri di inizializzazione di Munchkin, vedere [questo documento](/help/javascript-api/configuration.md).

Pubblicato il _2014-08-08_ da _Murta_

## Integrazione di Munchkin con Google Tag Manager

Google Tag Manager consente di aggiungere tag al sito Web. Invece di aggiungere manualmente ogni script di tracciamento come Munchkin al codice sorgente del sito Web, puoi inserire [Google Tag Manager](https://marketingplatform.google.com/about/tag-manager/) nel sito e quindi aggiungere tag come [Munchkin](/help/javascript-api/lead-tracking.md) tramite l&#39;interfaccia utente di Google Tag Manager. In questo post, mostreremo innanzitutto come generare il codice di tracciamento di Munchkin in Marketo, e quindi come aggiungere questo codice di tracciamento di Munchkin a Google Tag Manager.

### Come generare un codice di tracciamento Munchkin

1. Fai clic su **Amministratore** in alto a destra nell&#39;app.
1. Fai clic su **Munchkin** nella struttura a sinistra.
1. Selezionare **Asincrono** per il tipo di codice di tracciamento.
1. Fai clic su e copia il codice di tracciamento di JavaScript.

**Come aggiungere codice di tracciamento Munchkin a Google Tag Manager**

1. Accedi al tuo account Google Tag Manager e **Aggiungi un nuovo tag.**
1. Crea un nuovo **Tag HTML personalizzato**.
1. Copia e incolla il codice Munchkin nel campo **HTML** e fai clic su **Continua**.
1. Seleziona **Attiva su tutte le pagine** e fai clic su **Crea tag.** Nota: se hai un sito Web con traffico molto elevato, puoi escludere sezioni del sito utilizzando **Attiva su alcune pagine**.
1. Fai clic su Salva, quindi verifica che il codice di tracciamento Munchkin sia in fase di caricamento sul sito web.

Questo articolo contiene il codice utilizzato per implementare integrazioni personalizzate. A causa della sua natura personalizzata, il team del supporto tecnico Marketo non è in grado di risolvere i problemi relativi al lavoro personalizzato. Non tentare di implementare il seguente esempio di codice senza un’adeguata esperienza tecnica o l’accesso a uno sviluppatore esperto.

Pubblicato il _2014-08-05_ da _Murta_

## Nascondi Lightbox dopo l’invio del modulo

### Panoramica

Il codice di incorporamento lightbox corrente generato da Marketo non scompare quando il modulo viene inviato. La pagina verrà ricaricata dopo l’invio del modulo, che verrà nuovamente visualizzato. Se desideri creare una lightbox che si nasconde dopo l’invio del modulo, segui i passaggi indicati di seguito.

### Guida

1. Dopo aver creato il modulo in Marketo, genera il codice di incorporamento. A tale scopo, fare clic su Azioni modulo e quindi su Lightbox come tipo di codice. Copia e incolla questo codice in un editor di testo, in modo da poterlo modificare nel passaggio successivo.
1. Sostituisci tutto il codice dopo &quot;(form)&quot; nel codice da incorporare con il codice seguente:

```javascript
{
var lightbox = MktoForms2.lightbox(form).show();
form.onSuccess(function(){
lightbox.hide();
return false;
});
});
</script>
```

Dopo il passaggio 2, la versione completata avrà un aspetto simile al codice riportato di seguito. Il codice è ora pronto per essere utilizzato sul tuo sito web.

```javascript
<script src="http://app-sj04.marketo.com/js/forms2/js/forms2.js"></script>
<form id="mktoForm_0000"></form>
<script>MktoForms2.loadForm("http://app-sj04.marketo.com", "AAA-BBB-CCC", 0000, function (form){
var lightbox = MktoForms2.lightbox(form).show();
form.onSuccess(function(){
lightbox.hide();
return false;
});
});
</script>
```

Questo articolo contiene il codice utilizzato per implementare integrazioni personalizzate. A causa della sua natura personalizzata, il team del supporto tecnico Marketo non è in grado di risolvere i problemi relativi al lavoro personalizzato. Non tentare di implementare il seguente esempio di codice senza un’adeguata esperienza tecnica o l’accesso a uno sviluppatore esperto.

Pubblicato il _2014-08-21_ da _Murta_

## Guida rapida all’API REST di Marketo

Questa guida illustra come effettuare la prima chiamata all’API REST di Marketo in dieci minuti. Viene illustrato come recuperare un singolo lead utilizzando l&#39;endpoint REST API [Get Lead by Id](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadByIdUsingGET). A questo scopo, ti guideremo attraverso il processo di autenticazione per generare un token di accesso, che utilizzi per effettuare una richiesta HTTP GET a [Ottieni lead per ID](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadByIdUsingGET). Quindi ti forniamo il codice per effettuare la richiesta che restituisce informazioni sul lead formattate come JSON.

### Generare un token di autenticazione

>[!NOTE]
> A partire da giugno 2025, il token di autenticazione non è più supportato. Devi utilizzare l’intestazione Autenticazione.
>

Un servizio personalizzato in Marketo consente di descrivere e definire i dati a cui l’applicazione avrà accesso. Per creare un servizio personalizzato e associarlo a un singolo utente solo API, è necessario aver effettuato l’accesso come amministratore Marketo.

1. Passa all’area di amministrazione dell’applicazione Marketo.
1. Fai clic sul nodo Utenti e ruoli nel pannello a sinistra.
1. Crea un nuovo ruolo. Per visualizzare l’elenco delle autorizzazioni del ruolo, fai clic su Access API (API di accesso). Ora scorri verso il basso e seleziona solo le autorizzazioni necessarie. In questo caso, selezioneremo semplicemente l’autorizzazione Lead di sola lettura.
1. Il passaggio successivo consiste nel creare un utente solo API e associarlo al ruolo API creato nel passaggio precedente. Per farlo, seleziona la casella di controllo Utente solo API al momento della creazione dell’utente.
1. È necessario un servizio personalizzato per identificare in modo univoco l’applicazione client. Per creare un’applicazione personalizzata, passa alla schermata Admin > LaunchPoint e crea un nuovo servizio.
1. Specifica il Nome visualizzato, scegli il tipo di servizio &quot;Personalizzato&quot;, fornisci la Descrizione e l&#39;indirizzo e-mail dell&#39;utente creati nel passaggio 1. È consigliabile utilizzare un nome visualizzato descrittivo che rappresenti la società o lo scopo di questo servizio API REST personalizzato.
1. Fai clic sul collegamento &quot;Visualizza dettagli&quot; sulla griglia per ottenere l’ID client e il segreto client. L’applicazione client è in grado di utilizzare l’ID client e il segreto client per generare un token di accesso.
1. Copia e incolla il token di autenticazione in un editor di testo. Il token di autenticazione è simile all’esempio seguente:

`cdf01657-110d-4155-99a7-f986b2ff13a0:int`

### Come determinare l’URL dell’endpoint

Quando effettui una richiesta all’API di Marketo, devi specificare l’istanza di Marketo nell’URL dell’endpoint. Tutte le richieste API non in blocco all’API REST di Marketo seguiranno il formato seguente:

`<REST API Endpoint URL>/rest/`

L’URL dell’endpoint REST API si trova nel pannello Marketo Admin > Web Services. La struttura dell’URL dell’endpoint Marketo deve essere simile a quella riportata di seguito:

`https://100-AEK-913.mktorest.com/rest/v1/lead/{id}.json`

**Nota: gli URL API di massa non hanno il prefisso &quot;/rest/&quot;. Per l&#39;API in blocco, utilizzare solo l&#39;host e aggiungere il percorso appropriato come segue:**

`https://100-AEK-913.mktorest.com/bulk/v1/leads/export/create.json`

### Come utilizzare il token di autenticazione per chiamare il lead per ID AP

Nelle sezioni precedenti, è stato generato un token di autenticazione e è stato trovato l’URL dell’endpoint. Verrà ora effettuata una richiesta a un endpoint REST API denominato [Get Lead by Id](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadByIdUsingGET). Il modo più semplice per effettuare la richiesta all’API REST di Marketo è quello di incollare l’URL nella barra degli indirizzi del browser web. Segui il formato seguente:

`https://<REST API Endpoint URL for your Marketo instance>/rest/v1/<API that you are calling>?access_token=<access_token>`

### Esempio

`https://100-AEK-913.mktorest.com/rest/v1/lead/318581.json?access_token=cdf01657-110d-4155-99a7-f986b2ff13a0:int`

Se la chiamata ha esito positivo, restituisce JSON con il formato seguente:

```json
{
    "requestId": "d82a#14e26755a9c",
    "result": [
        {
            "id": 318581,
            "updatedAt": "2015-06-11T23:15:23Z",
            "lastName": "Doe",
            "email": "<jdoe@marketo.com>",
            "createdAt": "2015-03-17T00:18:40Z",
            "firstName": "John"
        }
    ],
    "success": true
}
```

Se sei interessato a saperne di più sulle API REST di Marketo, questo è un [buon punto di partenza](https://developer.adobe.com/marketo-apis/).

Pubblicato il _2015-09-04_ da _David_

## Eliminare il cookie di tracciamento di Marketo

Domanda: &quot;Abbiamo creato un modulo sul nostro sito web per il team di vendita per annullare l’iscrizione delle persone che hanno chiesto verbalmente di essere rimosse da qualsiasi messaggio e-mail. Quello che stiamo scoprendo è quando entrano nell&#39;e-mail e inviano che sono cotti con quel indirizzo e-mail e iniziano a ricevere avvisi per la loro attività sul nostro sito web. Il modulo che abbiamo è attualmente un modulo incorporato. Qualcuno ha trovato un modo per disabilitare il tracciamento del cuoco su un modulo incorporato, capisco come farlo con una pagina di destinazione di Marketo.&quot; Possiamo fare questo. Per implementarlo, accelera la scadenza del cookie. Una volta creato il cookie dal modulo, puoi forzarne la scadenza immediata. Poiché il cookie è scaduto, il browser lo eliminerà automaticamente. Per avviare questo processo, utilizzeremo la funzionalità del modulo nativo per chiamare nella funzione denominata `deleteCookie`.

Pubblicato il _2014-08-26_ da _Travis Kaufman_

## Integrare una pagina di destinazione di Marketo con Wordpress

Supponiamo che il tuo sito web sia stato creato utilizzando Wordpress e desideri incorporare una pagina di destinazione di Marketo in una delle pagine. Ciò è possibile utilizzando un iframe. Un iframe consente di incorporare una pagina in un’altra pagina. In questo post, mostrerai come incorporare una pagina di destinazione di Marketo in una pagina Wordpress. **Scelta di un plug-in Wordpress** Wordpress Esistono diversi plug-in Wordpress che consentono di: `http://wordpress.org/plugins/iframe/` Ecco alcuni pro e contro che usano questo approccio: `http://www.elixiter.com/marketo-landing-page-and-form-hosting-options/` Ottimo post Murtza! Colby, l’iframe manterrà la parte in cui, dopo l’invio del modulo, verrai reindirizzato a un PDF. Abbiamo utilizzato gli iframe sul nostro sito per molto tempo. Ottimo post Murtza! Colby, l’iframe manterrà la parte in cui, dopo l’invio del modulo, verrai reindirizzato a un PDF. Abbiamo utilizzato gli iframe sul nostro sito per molto tempo. Tieni presente che se desideri passare i parametri URL dall’URL principale all’iframe devi eseguire una certa codifica. Inoltre, assicurati che il Google Analytics sia configurato correttamente. Non vuoi che le visualizzazioni di pagina vengano conteggiate due volte per ogni visita alla pagina con l’iframe.

Pubblicato il _1970-01-01_ da _Murta_

## Integrazione ottimale con una pagina di destinazione Marketo

[In modo ottimale](https://www.optimizely.com/) consente di eseguire test A/B, multipagina e test multivariato sul sito. Puoi utilizzare in modo ottimale con una pagina di destinazione di Marketo. Ecco come eseguire questa operazione:

1. Trova e copia il frammento di codice Optimizely.** Vai alla dashboard in Ottimizzazione e fai clic sul collegamento Codice progetto. Copiare la riga di codice visualizzata nel popup.
1. Accedi a Marketo e seleziona il modello della pagina di destinazione. Quindi fate clic su &quot;Modifica bozza&quot;.**
1. Fai clic su Azioni pagina di destinazione. Quindi fai clic su Modifica metatag pagina**
1. Incolla il frammento di codice Optimizely nella sezione Custom HEAD HTML (Personalizza) e fai clic su Salva.
1. Verifica il funzionamento dello snippet Optimizely nella pagina di destinazione

Pubblicato il _2014-09-18_ da _Murta_

## Ricerca per nome completo tramite API REST di Marketo

**Domanda:** esiste un modo per eseguire query su un lead utilizzando le API di Marketo con solo il nome completo di un lead?
**Risposta:** Non è direttamente possibile. Tuttavia, la soluzione alternativa descritta di seguito consente di eseguire questa operazione.

1. Crea un campo personalizzato denominato &quot;Fullname&quot; in Marketo.
1. Utilizza l&#39;API di SOAP [getMultipleLeads](/help/soap-api/getmultipleleads.md) o [Ottieni più lead per tipo di filtro](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadByIdUsingGET) per eseguire una query sul database dei lead. Includi nome e cognome come attributi nella richiesta alle API REST o SOAP.
1. Dopo aver eseguito una query sul database dei lead, concatenare &quot;Nome&quot; e &quot;Cognome&quot; per ciascun lead e memorizzare i dati in una colonna &quot;Nome completo&quot;. 1. Utilizza l&#39;API SOAP [syncMultipleLeads](/help/soap-api/syncmultipleleads.md) per inviare questi dati al campo personalizzato &quot;Fullname&quot;. In alternativa, puoi utilizzare l&#39;API [Importa lead](/help/rest-api/leads.md) o importare un file CSV o XLS tramite l&#39;interfaccia utente di Marketo.
1. È ora possibile eseguire una query per nome completo utilizzando l&#39;API [Ottieni più lead per tipo di filtro](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET) per cercare questo campo personalizzato. Specificare &quot;Fullname&quot; come &quot;filterType&quot; e &quot;filterValue&quot; come &quot;Joe Johnson&quot; con una chiamata API REST Get Multiple Leads by Filter Type.

Pubblicato il _2014-09-09_ da _Murta_

## Elimina cookie di tracciamento Marketo

Utilizza questo metodo solo se l’effetto desiderato è la rimozione completa del cookie.

Questo esempio di codice può essere utilizzato per eliminare un cookie di tracciamento di Marketo dal browser di un utente dopo l’invio del modulo di Marketo. Funziona chiamando il metodo `deleteMarketoCookie` dopo che un utente ha inviato un modulo. Questo metodo fa scadere il cookie impostando la data di scadenza su una data nel passato. Il comportamento predefinito del browser consiste nell’eliminare questo cookie perché è scaduto.

Pubblicato il _2014-09-09_ da _Murta_

## Limita i domini e-mail gratuiti durante la compilazione del modulo

Si supponga che si desideri impedire ai visitatori del sito di inviare un modulo con un dominio e-mail gratuito, ad esempio Gmail o Yahoo. A questo scopo, includi lo script seguente nel codice sorgente della pagina con il modulo Marketo. Controlla se un utente ha inserito un’e-mail non aziendale (Gmail, Hotmail, ecc.) e, se un utente ne ha inserita una, impedisce l’invio di un modulo Marketo. Puoi estendere l’elenco dei domini e-mail bloccati modificando la riga 9 per includere i domini che desideri limitare.

Pubblicato il _2014-09-09_ da _Murta_

## Debug di un campo non accessibile tramite l’API

Se si arriva a questo campo <https://nation.marketo.com/> Quando si tenta di aggiornare il campo AnnualRevenue utilizzando l&#39;API SOAP, la risposta indica che il record è aggiornato, tuttavia il campo AnnualRevenue non persiste la modifica. Se si tenta di aggiornare il campo manualmente utilizzando il database dei lead, le modifiche apportate a questo campo non vengono mantenute. Controlla se il campo è bloccato in Amministrazione. Un’altra cosa che talvolta può accadere è che potrebbe trattarsi di un campo account sincronizzato da SFDC. Per impostazione predefinita, le modifiche a tali campi vengono bloccate perché in molti casi SFDC è il sistema di registrazione per tali campi. 1) Creato Il 2) Marketo SFDC ID 3) Codice Univoco Marketo 4) SFDC Tipo 5) Aggiornato Il 6) Urgenza 7) Priorità 8) Data Di Creazione Vendite

Pubblicato il _1970-01-01_ da _Murta_

## Aggiungere codice personalizzato a una pagina di destinazione di Marketo

Supponiamo che tu voglia aggiungere alla pagina di destinazione di Marketo uno script di tracciamento di terze parti, ad esempio Google Analytics. Ciò è possibile tramite l’interfaccia utente di Marketo. Più in generale, puoi aggiungere qualsiasi codice personalizzato (HTML, CSS, JavaScript) alla pagina di destinazione del Marketo seguendo le istruzioni di seguito.

1. Seleziona la pagina di destinazione e fai clic su Modifica bozza.
1. Trascina nell’elemento HTML.
1. Immetti il codice personalizzato e fai clic su Salva.

Pubblicato il _2014-09-10_ da _Murta_

## Post modulo lato server

`https://community.marketo.com/MarketoResource?id=kA650000000GsXXCA0`

Pubblicato il _2014-09-11_ da _Murta_

## Cancellazione del cookie di tracciamento di Marketo dagli invii di Forms 2.0

### Panoramica

Forms 1.0 conteneva il valore per il cookie di tracciamento Munchkin come campo nel DOM. Tale richiesta è stata presentata insieme a tutti gli altri fattori produttivi. [Forms 2.0](/help/javascript-api/forms-api-reference.md) omette questo campo e popola dinamicamente il valore al momento dell&#39;invio anziché al caricamento del modulo. Anche se questo è generalmente accettabile, crea una classe di casi d’uso che richiedono l’eliminazione del cookie di tracciamento per evitare il tracciamento e la precompilazione involontari. Ad esempio, ciò può verificarsi in una fiera in cui un cliente Marketo utilizza lo stesso modulo sullo stesso dispositivo e riceve informazioni di contatto da più persone. Lo snippet di codice seguente consente di cancellare il valore del cookie all’invio del modulo, senza dover eliminare il cookie stesso dal browser dell’utente.

### Esempio di codice

Questo snippet prevede il caricamento di un singolo modulo sulla pagina. Se sono presenti più moduli, è necessario utilizzare i metodi [loadForm o getForm](/help/javascript-api/forms-api-reference.md) per implementare i callback.

```javascript
<script>
//add a callback to the first ready form on the page
MktoForms2.whenReady( function(form){
 //add the tracking field to be submitted
        form.addHiddenFields({"_mkt_trk":""});
        //clear the value during the onSubmit event to prevent tracking association
 form.onSubmit( function(form){
  form.vals({"_mkt_trk":""});
 })
})
</script>
```

Pubblicato il _2014-09-11_ da _Kenny_

## Aggiornamenti della versione di settembre 2014

### Aggiornamenti all’API REST

È stato aggiunto un nuovo valore per i campi facoltativi all&#39;API [Get Multiple Leads](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET) che restituirà i valori dei cookie Munchkin associati a un record di lead. È sufficiente aggiungere &quot;?fields=cookies&quot; alla richiesta.

Pubblicato il _2014-09-16_ da _Murta_

## Aggiunta di dati sulla posizione dall’API RTP a Marketo Form Compilazione

**Per implementare il caso d&#39;uso descritto in questo post di blog, sono necessari abbonamenti RTP e MLM attivi.**
Utilizzando le [API JavaScript RTP](/help/javascript-api/web-personalization.md) e [Marketo Forms 2.0](/help/javascript-api/forms-api-reference.md), puoi estrarre i dati di posizione dedotti da RTP e inviarli a Marketo tramite una compilazione di modulo. Questo consente di visualizzare la posizione dedotta dall’utente (in base all’indirizzo IP) durante l’attività del modulo più recente. Per iniziare, devi creare tre campi stringa personalizzati in Marketo. Puoi eseguire questa operazione tramite il CRM se dispone di un’integrazione nativa con Marketo oppure dal menu Field Management nella sezione admin di Marketo. Consiglio di denominare questi campi &quot;Paese più recente&quot;, &quot;Stato più recente&quot; e &quot;Città più recente&quot;. Continuiamo questo blog utilizzando questa convenzione di denominazione. I nomi API per questi campi sono &quot;mostRecentCountry&quot;, &quot;mostRecentState&quot; e &quot;mostRecentCity&quot;. Per recuperare i dati relativi alla posizione, viene utilizzato il metodo [RTP per ottenere i dati relativi alla posizione del visitatore](/help/javascript-api/web-personalization.md), quindi passarli nel modulo utilizzando i metodi [addHiddenFields e vals](/help/javascript-api/forms-api-reference.md) di Marketo Forms 2.0. Nella pagina, aggiungi il tag JS RTP e un modulo Marketo. Quindi, includi lo script seguente. Se utilizzi una convenzione di denominazione diversa da quella descritta in precedenza, devi modificare i nomi dei campi del modulo di destinazione nel codice di esempio.

```javascript
<script>
//modify the form and grab the user
MktoForms2.whenReady( function(form) {
        //add the hidden fields to the form
 form.addHiddenFields({
  "mostRecentCountry":"",
  "mostRecentState":"",
  "mostRecentCity":""});
 //Grab the visitor data, a JS object with it is passed in the callback function of the third argument
 rtp('get','visitor',function(visitor){
  //add the desired info from the visitor object to the form fields
  form.vals({
   "mostRecentCountry":visitor.results.location.country,
   "mostRecentCity":visitor.results.location.city,
   "mostRecentState":visitor.results.location.state});
  }
  );
 });
</script>
```

Pubblicato il _2014-09-17_ da _Kenny_

## Blocca ricerca per indicizzazione e indicizzazione di ricerca di una pagina di destinazione di Marketo

Supponiamo che tu voglia impedire che una pagina di destinazione di Marketo venga sottoposta a ricerca per indicizzazione e mostrata nei risultati da motori di ricerca come Google. Ecco come eseguire questa operazione:

1. Accedi a Marketo e seleziona la pagina di destinazione. Quindi fate clic su &quot;Modifica bozza&quot;.
1. Fai clic su Azioni pagina di destinazione. Quindi fai clic su Modifica metadati pagina
1. Cambia il campo Robots in No Index, No Follow. Quindi fai clic su Salva.

Pubblicato il _2014-09-19_ da _Murta_

## Domande frequenti su REST Marketo e API SOAP

**Aggiornato: marzo 2016** Di seguito sono riportate le risposte alle domande più frequenti sulle API Marketo [REST](/help/rest-api/rest-api.md) e [SOAP](/help/soap-api/soap-api.md). **D: quali sono le principali differenze tra le API REST di Marketo e le API di SOAP?** A: Sebbene la capacità di inviare/richiamare dati specifici tramite API REST e SOAP si sovrapponga, alcune funzionalità esistono solo nelle API REST o SOAP. In termini di prestazioni, l&#39;API REST ha un [throughput](https://en.wikipedia.org/wiki/Throughput) migliore dell&#39;API SOAP. In termini di modello di autenticazione, l’API REST dispone di un modello di autenticazione che utilizza un token in scadenza. La nostra API REST fornisce anche l&#39;accesso alle [risorse](https://developer.adobe.com/marketo-apis/api/asset/) di Marketo.   **D: quali funzioni sono disponibili nell&#39;API REST e non nell&#39;API SOAP?** A: [Elenco di elenchi API](/help/rest-api/list-of-standard-fields.md), [rimozione di un lead da un elenco API](/help/rest-api/lead-database.md), [API di utilizzo](/help/rest-api/rest-api.md) e [API di errore](/help/rest-api/rest-api.md) sono disponibili solo con l&#39;API REST. **D: è previsto l&#39;aumento del numero di API disponibili per l&#39;API SOAP?** A: No **D: è previsto l&#39;aumento del numero di API disponibili per l&#39;API REST?** A: Sì. REST è l’obiettivo principale dello sviluppo API di Marketo in questo momento.

Pubblicato il _2014-09-20_ da _Murta_

## Post modulo lato server

**Nota: questa API non è pubblica e non è supportata, non è supportata e il suo comportamento può cambiare in qualsiasi momento** Se utilizzi i moduli sul tuo sito Web, puoi comunque inviare questi dati a Marketo utilizzando un post lato server. Il vantaggio di questo approccio è che puoi mantenere la logica del modulo e dell’applicazione esistente, ma puoi comunque utilizzare un post modulo effettivo in Marketo. Questo offre agli utenti di Marketo un evento &quot;Completa modulo&quot;, che può essere utilizzato per attivare processi automatizzati o per la segmentazione. **NOTA: esiste un limite di 30 post di moduli lato server al minuto da un singolo indirizzo IP.** In ogni linguaggio di script o di programmazione sono disponibili opzioni diverse per l&#39;invio di un modulo lato server e possono essere disponibili oggetti o metodi diversi che possono essere utilizzati per effettuare la chiamata post. Ad esempio, in PHP molte persone utilizzano la libreria cURL. In tutti i casi, si pubblicano coppie nome-valore in un URL specificato. I nomi devono essere identici ai nomi API dei campi Marketo. Inoltre, è necessario includere un paio di campi di sistema per acquisire correttamente l’invio del modulo.

1. Creare un modulo. Il primo passaggio consiste nella creazione di un modulo in Marketo o nell&#39;utilizzo di un modulo esistente che si desidera inviare. Il nome del modulo deve essere descrittivo, ma in realtà non richiede alcun campo modulo. Se crei un nuovo modulo, inserisci semplicemente un nome, deseleziona la casella &quot;Apri editor modulo&quot; e fai clic su Fine.
1. Trova un ID modulo. Nell&#39;interfaccia utente di Marketo, selezionare il modulo e controllare l&#39;URL: dovrebbe essere nel formato `https://app-x.marketo.com/#FO8B2ZN12`. Dietro il simbolo #, controllare il numero immediatamente successivo a &quot;FO&quot; per trovare l&#39;ID modulo. In questo caso, l’ID modulo è 8. In alcuni casi, il primo modulo può essere numerato 1001 e contare da lì. L’ID modulo è una variabile che consente di attivare l’invio di diversi moduli.
1. Ottieni l’ID del tuo account Marketo. Vai a Amministratore > Munchkin e copia l’ID account Munchkin, che ha il formato 000-AAA-000. È necessario questo in modo che il modulo venga inviato nell’istanza Marketo corretta.
1. Determina l’URL POST. Nell&#39;interfaccia utente di Marketo, annotare il dominio nella barra della posizione, in genere nel formato `<http://app-x.marketo.com/>`. Elimina qualcosa dopo la barra, quindi aggiungi &quot;index.php/leadCapture/save&quot; per ottenere il modulo completo dell’URL POST. Nota 1: questo fa distinzione tra maiuscole e minuscole. Nota 2: le sandbox di Marketo possono avere un dominio diverso rispetto al sistema Marketo di produzione. Un URL di esempio sarebbe: `http://app-x.marketo.com/index.php/leadCapture/save` Puoi anche utilizzare HTTPS invece di HTTP (non utilizzare il tuo CNAME in quanto fornisce un&#39;eccezione di sicurezza).
1. Trova i nomi dei campi modulo** Vai a Amministratore > Gestione campi e fai clic sul pulsante &quot;Esporta nomi dei campi&quot; per scaricare un foglio di calcolo con i nomi dei campi API. Utilizza il nome API come nome nelle coppie nome-valore.
1. Decidi quali campi pubblicare. Puoi includere qualsiasi campo Lead Marketo nell’invio del modulo. I nomi dei campi fanno distinzione tra maiuscole e minuscole. Oltre ai campi che desideri inviare, esistono due campi obbligatori e due campi consigliati: Campi obbligatori nel modulo: (1) `munchkinId` - Questo campo viene utilizzato per l’ID account Munchkin (2) `formid` - Questo campo indica quale modulo in Marketo è stato inviato Campi consigliati nel modulo: (1) E-mail - questo campo viene utilizzato come chiave primaria per la deduplicazione. Se Marketo trova un indirizzo e-mail corrispondente nel database di Marketo, aggiorna il record esistente, altrimenti crea un nuovo record. Se sono presenti più corrispondenze, aggiorna il record aggiornato più di recente (2) `_mkt_trk` - questo campo contiene le informazioni sui cookie, in modo da poter tenere traccia delle visite alle pagine Web del singolo utente. Se nella pagina del modulo è presente Munchkin, Munchkin immetterà automaticamente un valore in questo campo del modulo nascosto. In caso contrario, leggilo dal cookie con lo stesso nome e trasmettilo a Marketo in questo campo. Nota: il corpo del POST in un modulo Marketo deve essere codificato in URL.
1. Vedi la Risposta** La risposta al post del modulo sarà un codice di reindirizzamento HTTP 302. In alcuni sistemi, verrà visualizzato come un errore. Tuttavia, in questo caso significa che il lead è stato creato o aggiornato correttamente. In caso di errore, viene visualizzato un codice di errore 4xx o 5xx.

Ecco un [post](https://nation.marketo.com:443/t5/product-blogs/how-to-build-an-external-subscription-center/ba-p/242185) sull&#39;utilizzo di questa tecnica per gli scenari di annullamento dell&#39;abbonamento [di Justin Cooperman, Sr. Product Manager]

Pubblicato il _2014-11-07_ da _Murta_

## Trova lead aggiornati in un intervallo di date specifico

Supponiamo che tu voglia trovare lead aggiornati in date specifiche tramite l&#39;[API Marketo](/help/soap-api/soap-api.md). Ciò è possibile con l&#39;API SOAP [getMultipleLeads](/help/soap-api/getmultipleleads.md). Questo metodo restituisce tutti i lead con una modifica del valore dei dati o una nuova attività in Marketo per l’intervallo di date richiesto. Per `leadSelector`, specificare `LastUpdateAtSelector`. Quindi, puoi definire gli intervalli di date con `oldestUpdatedAt` e `latestUpdatedAt` limiti di tempo. Di seguito è riportato un esempio di codice XML di richiesta che mostra come trovare i lead aggiornati tra le ore 12:00 PST del 6 giugno 2014 e le ore 12:00 PST del 7 giugno 2011. Nota: l’intervallo di date non deve superare i 30 giorni.

**Esempio di XML della richiesta per trovare lead aggiornato entro la data**

```xml
<soapenv:Envelope xmlns:xsi="<http://www.w3.org/2001/XMLSchema-instance>" xmlns:soapenv="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:mkt="<http://www.marketo.com/mktows/">
<soapenv:Header>
<mkt:AuthenticationHeader>
<mktowsUserId>REPLACE</mktowsUserId>
<requestSignature>REPLACE</requestSignature>
<requestTimestamp>2014-10-23T12:19:51-07:00</requestTimestamp>
</mkt:AuthenticationHeader>
</soapenv:Header>
<soapenv:Body>
<mkt:paramsGetMultipleLeads>
<leadSelector xsi:type="mkt:LastUpdateAtSelector">
<oldestUpdatedAt>2014-06-06T00:00:00-08:00</oldestUpdatedAt>
<latestUpdatedAt>2014-06-07T00:00:00-08:00</latestUpdatedAt>
</leadSelector>
</mkt:paramsGetMultipleLeads>
</soapenv:Body>
</soapenv:Envelope>
```

Pubblicato il _2014-09-24_ da _Murta_

## Aggiungere contenuto dinamico a un’e-mail

Supponiamo che tu invii un’e-mail giornaliera e desideri includere automaticamente la data di quel giorno nel modello e-mail. A questo scopo, puoi utilizzare i token e gli script e-mail in Marketo.

1. Crea un token.** Passa al programma in cui desideri utilizzare il token. Fai clic su I miei token.
1. Fai doppio clic su Script e-mail. Quindi denomina questo token. Quindi fai clic su Modifica.
1. Incolla lo script e-mail di seguito in questa finestra. Quindi fai clic su Salva.

## Accedere all’oggetto calendario di Velocity

`set($x = $date.calendar)`

## Formato data

`set($current_date = $date.format('dd-MM-yyyy', $x.getTime()))`

## Restituisce la data odierna

`$current_date`

1. Fai riferimento al token nel modello e-mail.** Prendi nota del nome del token. Passa alla bozza e-mail. Includi il token.  Quando l’e-mail viene inviata, il valore del token viene popolato. Per ulteriori informazioni, vedere la [documentazione per gli sviluppatori di script di posta elettronica](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/email-scripting).

Pubblicato il _2014-11-22_ da _Murta_

## Bash Security Advisory

Marketo ha condotto un&#39;indagine approfondita sulla vulnerabilità Bash, nota anche come [Shellshock (CVE-2014-6271)](https://nvd.nist.gov/view/vuln/detail?vulnId=CVE-2014-6271), e ha concluso che non siamo suscettibili a questi attacchi. Inoltre, abbiamo intrapreso azioni preventive aggiornando il software alla versione più recente per garantire la conformità con la [raccomandazione CERT](https://www.cisa.gov/news-events/alerts/2014/09/25/gnu-bourne-again-shell-bash-shellshock-vulnerability-cve-2014-6271).

Pubblicato il _2014-09-26_ da _Murta_

## Come aggiornare le credenziali API di SOAP

È consigliabile aggiornare regolarmente le credenziali dell&#39;[API SOAP](/help/soap-api/soap-api.md). Attualmente, non è possibile eseguire questa operazione a livello di programmazione tramite l’API Marketo. Le istruzioni seguenti mostrano come aggiornare le credenziali API di SOAP tramite l’interfaccia utente di Marketo.

1. Vai alla sezione Amministratore e fai clic su Servizi web.
1. Impostare una chiave di crittografia di almeno 10 caratteri e fare clic su Salva modifiche.

Pubblicato il _2014-09-29_ da _Murta_

## Come pulire il database Marketo

**NOTA: questo è un post di blog per gli ospiti. Josh Hill è Marketo Practice Lead presso Perkuto, un&#39;agenzia di marketing automation. Josh lavora all&#39;intersezione tra marketing, vendite e tecnologia per generare nuovi profitti. In [MarketingRockstarGuides.com](https://www.marketingrockstarguides.com/) scrive informazioni sull&#39;automazione del marketing e sulla generazione della domanda.** Mantenere pulito il database di Marketo è una parte importante dell&#39;amministrazione di questo potente sistema. Se i tuoi dati Marketo sono sporchi o i tuoi dati CRM sono sporchi, allora il tuo lavoro come responsabile marketing diventa molto più difficile, in quanto spieghi perché le campagne sono andate alle persone sbagliate o i tuoi rapporti sono &quot;direzionali&quot;. [Uno studio di Gartner del 2011 ha rilevato](https://www.data.com/export/sites/data/common/assets/pdf/DS_Gartner.pdf) che la scarsa qualità dei dati ha ridotto la produttività della manodopera del 20%. Si tratta del 20% del tempo (8 ore/settimana o 1 giorno/settimana) che viene sprecato perché è stato necessario correggere i dati. Ma questa perdita impallidisce in confronto ai ricavi persi perché il targeting era sbagliato. Ecco le cinque ragioni principali per investire nel mantenimento dei dati puliti: - È possibile migliorare la segmentazione di lead e clienti, consentendo di focalizzare il messaggio sulle persone giuste al momento giusto. Quel 20% di sconto dovrebbe andare solo ai nuovi clienti potenziali, non ai migliori clienti, giusto? - Evitare l’invio di e-mail duplicate. Nonostante tutte le precauzioni, è possibile inviare più e-mail allo stesso lead, in genere perché i record duplicati non vengono uniti. - Segnalazione accurata al tuo capo. Poiché la CMO ha un posto al tavolo dei ricavi, è necessario disporre di rapporti accurati, coerenti e ripetibili...oppure non è più possibile essere un esperto di marketing dei ricavi. - Ferire le offerte in sospeso se si invia il materiale di marketing errato ai potenziali clienti durante le trattative di vendita.

Se il database non fornisce tali dettagli nella fase del ciclo di vita, potrebbe affondare un&#39;offerta o due. - Rimuovere i lead vecchi o cattivi per rimanere al di sotto delle soglie di prezzo. Molto è stato scritto sul costo dei dati sbagliati. Il problema più immediato è che troppi lead danneggiati, duplicati o vecchi ti spingono oltre il livello di prezzo di Marketo o Salesforce, che può comportare migliaia di tariffe all&#39;anno. Quindi, come si tiene pulito il database? Puoi seguire questi principi di pulizia dei dati, ma come ci arrivi all&#39;interno di Marketo? Faccio alcune ipotesi sul vostro sistema: - Avete un sistema Marketo standard. - È disponibile una configurazione Salesforce tipica per account di contatto lead. : stai sincronizzando tutti i record tra sistemi. - Non utilizzi duplicati mirati.

Trova i lead giusti da correggere** Individuiamo innanzitutto i lead che rappresentano potenziali problemi. Con ogni client, eseguo una serie di elenchi avanzati per determinare lo stato del database. Ti suggerisco di fare lo stesso su base mensile. Una volta che gli elenchi avanzati funzionano, non ti occorrono più di 15 minuti al mese. È un ottimo modo per dimostrare il ritorno sull&#39;investimento del tuo lavoro e di Marketo. Crea una tabella per comprendere l’impatto totale sul database. L’esempio seguente include i criteri che cerco, quindi assicurati di includere altri campi importanti per la tua attività. Raggruppare ogni gruppo in base a Solo Marketo, Lead SFDC e Contatti SFDC.

Utilizzare l’automazione per correggere i valori dei dati: l’utilizzo delle regole di automazione per correggere errori ortografici comuni o dati mancanti risale a diversi decenni fa. Negli anni &#39;60, la scarsa qualità dei dati durante l&#39;invio di direct mailing poteva causare uno spreco di migliaia di dollari. Oggi, gli errori di qualità dei dati rovinano la reputazione più rapidamente di un messaggio di posta mal diretto. La reputazione delle e-mail, la scelta della lingua e l’esperienza del cliente sono importanti e contano di più perché gli errori possono diventare virali pubblicamente in pochi minuti. Risparmia la reputazione della tua azienda con la pulizia automatizzata dei dati. Questi flussi di gestione dei dati sono una delle prime cose che ho configurato in Marketo. La maggior parte delle aziende ha impostato flussi simili: - Correttore paese (anche se avresti dovuto seguire il Principio 1 per non averne bisogno) - Correttore di stato o Mapper - spesso utile se hai Paese e Stato dedotto. - Numero di dipendenti nell&#39;intervallo dipendenti. - Lead Source non valido per un buon lead Source - E-mail non valida per l’e-mail è buona se l’e-mail è stata modificata. : trasforma i vecchi valori di campo in un nuovo campo (da Completo a Vuoto). Questo flusso, ad esempio, adegua l&#39;intervallo dipendente in base al numero dipendente.

Aggiunta di dati Strumenti La compilazione di campi vuoti è fondamentale per segmentare meglio il database. I lead non sempre compilano il corretto Settore, Funzione o Titolo in un modulo. A volte sono presenti dati legacy provenienti da un sistema precedente. La mancanza di dati comporta la necessità di inviare l&#39;e-mail a un numero inferiore di persone o, probabilmente, a persone sbagliate. Per risolvere questo problema, è necessario disporre di strumenti di aggiunta dati.

Opzione 1: compilalo da solo. Puoi interpolare i dati per compilare nuovamente i campi vuoti. Forse hai un codice SIC invece del nome del settore o del Ricavo annuale rispetto all’intervallo di ricavi annuale. Marketo può automatizzare facilmente queste correzioni.

Opzione 2: Trovare un fornitore di aggiunta/arricchimento dati tramite LaunchPoint Sono disponibili [diversi fornitori in Launchpoint](https://exchange.adobe.com/apps/browse/ec?product=MRKTO), ad esempio NetProspex e ReachForce, che possono aiutarti ad arricchire i tuoi dati dei lead. Alcuni ti chiedono un foglio dei tuoi dati, che poi puliscono e inviano indietro. L’opzione migliore è uno strumento automatizzato in Marketo o Salesforce che controlla i campi desiderati e quindi restituisce i dati corretti. La maggior parte dei fornitori utilizza l&#39;API di [Marketo o i webhook](/help/home.md) per eseguire questa operazione.

Opzione 3: Utilizzare le API di Marketo per aggiornare i lead È possibile utilizzare le API di Marketo per identificare i lead da pulire, quindi aggiornarli tramite l’API. [Ottieni più lead per tipo di filtro REST API](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET) è un buon punto di partenza per estrarre dati da Marketo che corrispondono a determinati criteri. Per aggiornare i lead, vedere l&#39;[API REST per creare/aggiornare i lead](/help/rest-api/leads.md).

In alternativa, è possibile impostare un [webhook di Marketo](/help/webhooks/webhooks.md) per notificare a un sistema esterno che si è verificato un determinato evento, ad esempio la compilazione di un modulo. È quindi possibile rispondere con i valori per aggiornare il lead con.

Cosa automatizzare? Ho fornito alcuni suggerimenti nel passaggio 1. Ma se vogliamo ripulire ulteriormente il database, dobbiamo andare oltre la correzione dei valori dei dati. - Inserimento in blacklist dei concorrenti: assicurati di automatizzare la raccolta e la blacklist dei tuoi concorrenti. Se hai dei partner concorrenti, è comunque bene contrassegnarli correttamente e inserirli in un elenco da eliminare.  - Più mancati recapiti permanenti: automatizza sicuramente questo flusso. Se un lead non recapita più di due volte in un periodo di 30 giorni, impostalo su Sospeso o Non valido. Poi vai una volta al mese per verificare se il problema è stato un errore di battitura o altro.  - Più mancati recapiti non permanenti in 30 giorni: impostarli su marketing SUSPENDED=TRUE per 30 giorni.  - Sospensione/eliminazione trappola spam: fai attenzione se il tuo prodotto significa che le persone usano possibili indirizzi trappola spam. Consulta un elenco delle trappole spam. L’elenco avanzato.

**Che cosa non automatizzare** Non automatizzare mai l&#39;eliminazione dei lead, perché vi è troppo rischio di eliminare accidentalmente in massa i record errati. Al contrario, rivedi una volta al mese i lead contrassegnati come Cestino. Ma se desideri eliminare i lead eliminati, esegui un flusso come questo con un’attesa di 30 giorni.

Avvertenze: l’utilizzo dell’automazione è un ottimo modo per risparmiare tempo e mantenere pulito il database. L&#39;automazione è una spada a doppio taglio perché se la si imposta in modo errato, può causare il caos in pochi minuti. Stai attento mentre segui questi processi e tieni tutti informati. In genere si consiglia di non eliminare i contatti di SFDC perché hanno più probabilità di essere opportunità attive, client o ex client. L&#39;Ufficio finanziario o l&#39;Ufficio legale possono richiedere di conservare alcuni documenti e di allegare i Contatti a tali documenti. Concentrati invece sui contatti che subiscono mancati recapiti, che non sono più validi o che non sono più presenti. Ora si conoscono alcune tecniche per mantenere pulito il database Marketo. Dicci se hai trovato altri modi per automatizzare questi processi utilizzando l’API o i webhook.

Pubblicato il _2014-10-08_ da _Josh_

## Aggiornamenti della versione di ottobre 2014

### Precompilazione pagina esterna

I moduli di Marketo non forniscono funzionalità di precompilazione native quando vengono caricati all’esterno di una pagina di destinazione di Marketo. Tuttavia, è ancora possibile implementarlo utilizzando le [API Marketo](/help/rest-api/rest-api.md) e le [API Forms 2.0 JavaScript](/help/javascript-api/forms-api-reference.md/). Il primo passaggio consiste nel recuperare i dati dei lead da Marketo tramite una chiamata REST dal server. Supponendo di non disporre di un modo immediato per fare riferimento incrociato agli ID lead o a un altro identificatore univoco dal server, è necessario utilizzare il cookie Munchkin &#39;_mkto_trk&#39; per recuperare dati dal server Marketo, utilizzando il metodo [Get Leads By Filter Type](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET).
Per effettuare questa chiamata, sono necessari gli endpoint di autenticazione e REST della tua istanza. Dopo aver eseguito l&#39;autenticazione con l&#39;istanza Marketo, è necessario effettuare una chiamata all&#39;API lead all&#39;indirizzo `https://<host>/rest/v1/leads.json`. È quindi necessario creare una querystring per filtrare in base al cookie Marketo come `?filterType=cookie&filterValues=`. È necessario recuperare il valore specifico dalla chiave &quot;_mkto_trk&quot; inviata al server dal client. NOTA: il valore del cookie _mkto_trk include una e commerciale e deve essere un URL codificato in `%26` per essere accettato correttamente dall&#39;endpoint Marketo. Per impostazione predefinita, l&#39;API lead restituisce quattro campi: `id`, `email`, `firstName` e `updatedAt`. Per impostare un set di campi specifico, è necessario includere un parametro di query `fields`, con i nomi dei campi separati da virgole come: `&fields=email,firstName,lastName,company`. Alla fine la nostra chiamata sarà simile alla seguente:

`https://<host>/rest/v1/leads.json?filterType=cookie&filterValues=<cookie>&fields=email,firstName,lastName,company&access_token=<token>`

Quando effettuiamo questa chiamata, questo restituisce un oggetto JSON che si presenta così:

```json
{
    "requestId":"e42b#14272d07d78",
    "success":true,
    "result":[
        {
        "id":50,
        "firstName":"Kenny",
 "lastName":"Elkington",
        "email":"<mkto@example.com>",
 "company":"Marketo Inc."
        }]
};
```

Ora che disponiamo dei dettagli del lead, possiamo procedere con la mappatura di questi in un modulo Marketo, utilizzando i metodi whenReady e vals in JavaScript. Innanzitutto, è necessario impostare i risultati del lead come variabile sulla pagina:

```javascript
<script>
//print your JSON object dynamically as the mktoLead variable
var mktoLead = {
    "requestId":"e42b#14272d07d78",
    "success":true,
    "result":[
        {
        "id":50,
        "firstName":"Kenny",
  "lastName":"Elkington",
        "email":"mkto@example.com",
  "company":"Marketo Inc."
        }]
}
</script>
```

Ora che abbiamo i risultati sulla pagina, possiamo mapparli nei nostri campi modulo:

```javascript
<script>
MktoForms2.whenReady( function(form) {
 //set the first result as local variable
 var mktoLeadFields = mktoLead.result[0];
    //map your results from REST call to the corresponding field name on the form
 var prefillFields = {
   "Email" : mktoLeadFields.email,
   "FirstName" : mktoLeadFields.firstName,
   "LastName" : mktoLeadFields.lastName,
   "Company" : mktoLeadFields.company
   };
 //pass our prefillFields objects into the form.vals method to fill our fields
 form.vals(prefillFields);
 }
 );
</script>
```

Pubblicato il _2014-10-24_ da _Kenny_

## Sostituisci e-mail HTML

Questo post illustra come rimuovere il HTML generato da Marketo per un messaggio e-mail. Potrai quindi utilizzare il tuo HTML senza che Marketo lo riformi.

1. Passa all’e-mail e fai clic su Modifica bozza.
1. Fare clic su Azioni e-mail, Strumenti di HTML e quindi su Sostituisci HTML.
1. Sostituisci il codice in questa casella con il tuo HTML. Quindi fai clic su Salva.

Pubblicato il _2014-10-28_ da _Murta_

## Ottieni l’ID cookie di un visitatore, quindi effettua una query sui dati del lead associato

Utilizzando l&#39;endpoint REST [Get Multiple Leads by Filter Type](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET), è possibile eseguire query sui dati dei lead in base all&#39;ID cookie di un utente. È possibile, ad esempio, utilizzare questo approccio per precompilare un modulo su una pagina di destinazione non Marketo. Questo post mostra come acquisire il valore del cookie dell&#39;utente durante una visita a una pagina Web, eseguire la query [Get Multiple Leads REST API](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET) con tale ID cookie e quindi restituire i dati del lead dell&#39;utente. Innanzitutto, è necessario il valore del cookie Munchkin dell’utente, &quot;_mkto_trk&quot;. Di seguito è riportato un esempio di funzione JavaScript che puoi utilizzare per ottenere il valore del cookie. Per ulteriori informazioni su questo approccio, vedere [questa pagina StackOverflow](https://stackoverflow.com/questions/10730362/get-cookie-by-name). Prima di richiamare questa funzione, è consigliabile impostare un ritardo di 500 ms dopo l’evento di caricamento della pagina. Questo offre a Munchkin il tempo di caricare e cookie per l’utente.

```javascript
//Function to read value of a cookie
function readCookie(name) {
    var cookiename = name + "=";
    var ca = document.cookie.split(';');
    for(var i=0;i < ca.length;i++) {
        var c = ca[i];
        while (c.charAt(0)==' ') c = c.substring(1,c.length);
        if (c.indexOf(cookiename) == 0) return c.substring(cookiename.length,c.length);
    }
    return null;
}

//Call readCookie function to get value of user's Marketo cookie
var value = readCookie('_mkto_trk');
```

Quindi, passa il valore del cookie &quot;_mkto_trk&quot; al server. Per recuperare i dati dei lead, dal server effettuare una chiamata all&#39;API REST [Get Multiple Leads](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET) con questo valore cookie. Sono necessari gli endpoint di autenticazione e REST dell’istanza. La chiamata deve essere strutturata nel modo seguente:

NOTA: il valore del cookie `_mkto_trk` include una e commerciale e deve essere un URL codificato in `%26` per essere accettato correttamente dall&#39;endpoint Marketo.

```java
require 'rest_client'
require 'json'

# Build request URL
# Replace AAA-BBB-CCC with your Marketo instance
marketo_instance = "<https://AAA-BBB-CCC.mktorest.com>"
endpoint = "/rest/v1/leads.json"
# Replace with your access token
auth_token =  "?access_token=" + "cde42eff-aca0-48cf-a1ac-576ffec65a84:ab"
# Replace with filter type and values
filter_type_and_values = "&filterType=cookies&filterValues=id:AAA-BBB-CCC%26token:_mch-marketo.com-1418418733122-51548&fields=cookies,email"
request_url = marketo_instance + endpoint + auth_token + filter_type_and_values

# Make request
response = RestClient.get request_url

# Returns Marketo API response
puts response
```

L’esempio precedente restituirà l’e-mail e tutti i cookie associati all’utente. Puoi quindi utilizzare questi dati per personalizzare la pagina successiva visitata dall’utente.

`{"requestId":"aa00#14a405aa786","result":[{"id":583,"email":"<testaccount@gmail.com>","cookies":"_mch-marketo.com-1418418733122-51548"}],"success":true}`

Pubblicato il _2014-10-30_ da _Murta_

## Quando hai bisogno di uno sviluppatore per supportare l’automazione del marketing?

NOTA: questo è un post di un blog ospite. Josh Hill è Marketo Practice Lead presso Perkuto, un&#39;agenzia di marketing automation. Josh lavora all&#39;intersezione tra marketing, vendite e tecnologia per generare nuovi profitti. In [MarketingRockstarGuides.com](https://www.marketingrockstarguides.com/) scrive informazioni sull&#39;automazione del marketing e sulla generazione della domanda.

Le piattaforme di automazione del marketing sono estremamente potenti, pronte all’uso e nelle mani di un operatore esperto. Le piattaforme, per definizione, consentono l’utilizzo di applicazioni di estensione per fare al sistema cose ancora più sorprendenti per il tuo team. Potresti pensare che il motore logico di Marketo sia in grado di fare così tanto (e lo è), ma ci sono limitazioni. Marketo non può e non deve fare tutto per te.

Ci sono altri strumenti là fuori che svolgono la loro funzione meglio di quanto Marketo possa costruirla. La piattaforma di Marketo è molto aperta e consente l&#39;esistenza dell&#39;[ecosistema LaunchPoint delle applicazioni](https://exchange.adobe.com/apps/browse/ec?product=MRKTO). Puoi anche utilizzare questa apertura per espandere le funzionalità del sito e di Marketo in base alle tue esigenze aziendali. La cosa bella di una piattaforma come Marketo è che consente all’addetto al marketing tipico di creare pagine, e-mail e logica di indirizzamento senza essere un programmatore a tutti gli effetti.
Un addetto al marketing di questi tempi deve capire la logica, ma è meglio lasciare la programmazione effettiva agli esperti. Quindi, come fai a sapere quando devi chiamare uno sviluppatore? Ho alcune regole di base, o euristica, per decidere quando un programmatore deve essere coinvolto: - Quando Marketo non ha un filtro ovvio, trigger o funzionalità per la necessità, spesso può essere fatto con alcuni JavaScript o jQuery. - La questione sarà troppo complessa per Marketo da sola? - Marketo è in grado di farlo? - Questa personalizzazione del sito web non è facilmente supportata? - Marketo deve contattare un sito Web o un altro database? - Può sembrare qualcosa che un computer può fare, ma Marketo non ha una funzione per farlo? Marketo potrebbe non offrire una funzione preconfigurata, ma si connette a molte integrazioni di terze parti e a connessioni personalizzate.

Dai un&#39;occhiata ad alcune di queste categorie nel [Marketplace LaunchPoint](https://exchange.adobe.com/apps/browse/ec?product=MRKTO): - [Strumenti di Analytics](https://exchange.adobe.com/apps/browse/ec?product=MRKTO) - [Aggiunta dati](https://exchange.adobe.com/apps/browse/ec?product=MRKTO) - [Sistemi di gestione dei contenuti](https://exchange.adobe.com/apps/browse/ec?product=MRKTO) Alcune applicazioni di terze parti forniscono pannelli di controllo intuitivi e strumenti di configurazione direttamente nella piattaforma (Webinar GoTo). Si tratta di integrazioni &quot;native&quot; in cui il lavoro più necessario è configurare l’accesso e quindi utilizzarlo in Marketo. Altre estensioni, tuttavia, richiedono l’utilizzo di API più complesse che devono essere programmate in modo più diretto.

**Opzioni di integrazione di Marketo** - Integrazione di LaunchPoint - in genere accesso o impostazioni semplici. - Integrazione API - Richiede l&#39;installazione dell&#39;API e la programmazione: (1) [REST API](/help/rest-api/rest-api.md) (2) [SOAP API](/help/soap-api/soap-api.md) (3) [Integrazione webhook](/help/webhooks/webhooks.md) - richiede l&#39;installazione di un codice speciale, ma abbastanza semplice. (4) [Script e-mail](./email-scripting.md) (Velocity) - JavaScript e jQuery: (1) [Forms 2.0](/help/javascript-api/forms-api-reference.md) (2) [Tracciamento lead (Munchkin)](/help/javascript-api/lead-tracking.md) (3) [Social JS](/help/javascript-api/social.md) (4) [RTP JS](/help/javascript-api/web-personalization.md) Di seguito sono riportati alcuni casi d&#39;uso per l&#39;utilizzo di uno sviluppatore per estendere le funzionalità della piattaforma Marketo. Hai uno di questi casi d’uso? Se è così, potrebbe essere il momento di parlare con uno sviluppatore. [Visitare la sezione dei partner di servizi in LaunchPoint](https://exchange.adobe.com/apps/browse/ec?product=MRKTO).

Pubblicato il _2014-11-06_ da _Josh_

## Integrazione di Slack con Marketo

[Slack è una piattaforma di collaborazione aziendale](https://slack.com/). Se il tuo team utilizza Slack, puoi inserire facilmente le notifiche Marketo nel flusso di lavoro. Questo post mostra come aggiungere una notifica al registro chat quando si verifica una specifica attività del lead in Marketo. I potenziali casi d’uso includono la notifica all’intero team di una compilazione di un modulo, una visita a una pagina di determinazione prezzi o un lead che non è stato contattato in 30 giorni. La schermata seguente mostra l’aspetto della notifica Marketo in Slack dopo aver seguito i passaggi descritti in questo articolo della guida.

1. Accedi a Slack. Fai clic su Integrazioni in Slack
1. Fare clic sul pulsante Aggiungi per i webhook in ingresso
1. Scegli il canale per la notifica Marketo. Quindi Fai Clic Su Aggiungi Integrazione Webhook In Ingresso.
1. Copia e incolla l’URL del webhook (necessario per il passaggio otto)
1. Scegli un nome per la notifica
1. Accedi a Marketo. Vai a Amministratore. Fai clic su Webhook.
1. Fai clic su Nuovo webhook
1. Immettere un nome per il webhook. Immetti l’URL del webhook dal passaggio quattro. Inserire Contabilizza come tipo di richiesta. Inserisci un modello di payload.

Ecco il modello di payload dalla schermata. Utilizza token di nome, società e indirizzo e-mail a livello di lead.

`payload={"text": "DEVELOPER SITE ALERT: {{lead.First Name:default=edit me}} {{lead.Company=edit me}}, {{lead.Email Address:default=no email address}}" }`

1. Configurare Trigger Campaign in Marketo. Il passaggio del flusso consiste nel chiamare il webhook a Slack. Smart List è una visita a una pagina Web.
1. Verifica Che Funzioni.

Per ulteriori informazioni sui webhook in Marketo, consulta la [documentazione per sviluppatori](./webhooks/webhooks.md).

Pubblicato il _2014-11-10_ da _Murta_

## Integrazione di Litmus con Marketo

[Litmus è un servizio](https://www.litmus.com/) per il test delle e-mail tra browser e client di posta elettronica. Litmus fornisce anche analisi sulle e-mail, inclusi clic, aperture ed eliminazioni. Questo post illustra come integrare Marketo con Litmus.

1. Durante la configurazione del programma e-mail in Marketo, fai clic su &quot;I miei token&quot; nel dashboard del programma
1. Trascina il token &quot;Email Script&quot; nel pannello centrale per aggiungerlo.
1. Denomina il token e fai clic su &quot;Fai clic per modificare&quot;
1. A destra, sotto &quot;Oggetti standard&quot;, espandere la categoria &quot;Lead&quot;. Trova il campo &quot;Email Address&quot; (Indirizzo e-mail) e seleziona la casella. Nello spazio di script vuoto a sinistra della stessa pagina, incolla il codice di tracciamento fornito da Litmus. Nel codice Litmus, sostituire ogni istanza di `{{lead.Email Address}}` con `${lead.Email}`. Fare clic su Salva per chiudere la finestra lightbox e fare di nuovo clic su Salva nella pagina dei token.
1. Prendere nota del nome del token `{{my.LitmusToken}}`. Apri l’e-mail di cui desideri tenere traccia. In fondo all’e-mail, inserisci il nuovo token di script. È inoltre possibile aggiungere informazioni predefinite per la versione di Litmus `{{my.LitmusToken:default=editme}}`.

Quando l’e-mail viene inviata, lo script verrà inserito nell’e-mail da Marketo.

Pubblicato il _2014-11-18_ da _Murta_

## Specifica un’immagine quando una pagina di destinazione di Marketo viene condivisa su Facebook

Supponiamo che tu voglia che un’immagine venga visualizzata automaticamente quando condividi una pagina di destinazione di Marketo su Facebook. Forse vorrai anche che questa immagine non sia sulla pagina di destinazione di Marketo, ma solo sulla condivisione di Facebook. Per farlo, aggiungi un tag meta open-graph alla pagina di destinazione di Marketo. Di seguito sono riportati i passaggi necessari per eseguire questa operazione.

1. Seleziona la pagina di destinazione. Quindi fate clic su Modifica bozza (Edit Draft).
1. Fai clic su Modifica metatag pagina.
1. Aggiungi metadati open-graph alla sezione Facebook OG Tags. Quindi fai clic su Salva. Formato: `<meta property="og:image" content="http://example.com/example.jpg"/>`

[Per ulteriori informazioni, consulta la documentazione per gli sviluppatori di Facebook](https://developers.facebook.com/docs/sharing/best-practices) sui metadati open-graph.

Pubblicato il _2014-11-17_ da _Murta_

## Reindirizza pagina in base al referente

Supponiamo che tu voglia impedire il traffico diretto verso una pagina di destinazione di Marketo. Immagina che questa pagina contenga contenuto scaricabile come un PDF che un utente dovrebbe compilare un modulo prima di riceverlo. Per risolvere questo problema, controlla se l’utente proviene da una determinata pagina. In questo caso, si tratta della pagina in cui l’utente deve compilare un modulo. Se l&#39;utente non proviene da tale pagina, è possibile reindirizzarlo alla pagina di compilazione del modulo. A questo scopo, devi verificare se la pagina di provenienza della pagina di destinazione con il contenuto è la pagina di compilazione del modulo.
Sostituire entrambe le istanze di `http://example.com/PageWithForm` nel frammento seguente con un collegamento alla pagina da cui si desidera che l&#39;utente provenga. Potrebbe trattarsi della pagina di compilazione del modulo.**

```javascript
<script>
window.onload = function() {
  if (document.referrer !== "http://example.com/PageWithForm") {
    document.location.href = "http://example.com/PageWithForm";
  };
 };
</script>
```

Includi con il contenuto lo snippet JavaScript personalizzato prima del tag di chiusura nella pagina di destinazione di Marketo.** Se l’utente non viene inviato alla pagina di destinazione con il contenuto della pagina di compilazione del modulo, verrà reindirizzato alla pagina di compilazione del modulo.

Pubblicato il _2014-11-18_ da _Murta_

## Integrazione di Trello con Marketo

Trello è una [applicazione di gestione dei progetti basata sul Web](https://trello.com/). Se il tuo team utilizza Trello, puoi inserire facilmente le notifiche Marketo nel flusso di lavoro. Questo post illustra come aggiungere una scheda con una notifica Marketo alla bacheca Trello. Questa scheda verrà aggiunta quando si verifica un’attività lead specifica in Marketo. I potenziali casi d’uso includono la notifica all’intero team di una compilazione di un modulo, una visita a una pagina di determinazione prezzi o un lead che non è stato contattato in 30 giorni.

1. Accedi a Trello. Passa alla bacheca Trello che aggiungerà le notifiche Marketo in. Fare clic su Aggiungi elenco e quindi denominarlo.
1. Fare clic su Mostra barra laterale. Fai clic su Impostazioni da e-mail a scheda. Annota l’e-mail nella casella &quot;Il tuo indirizzo e-mail per questa bacheca&quot;. Questa e-mail verrà utilizzata nel passaggio sei. Scegli quale elenco aggiungere la notifica Marketo.
1. Accedi a Marketo. Fai clic sulla nuova Smart Campaign. Immettere un nome e quindi fare clic su Salva.
1. Passa a Elenco avanzato. Scegli un trigger per questa campagna avanzata. In questo esempio, utilizziamo il trigger Riempimento modulo. Trascina il trigger Modulo di compilazione nel pannello centrale. Seleziona il modulo per attivare questa notifica.
1. Creare un messaggio e-mail. Fai clic su Nuovo. Fai clic su Risorsa locale. Fai clic su Nuova e-mail. Denomina l’e-mail. Quindi fai clic su Crea.
1. Fai clic su &quot;Modifica bozza&quot; per l’e-mail creata nel passaggio cinque. Trascina i token da mostrare nella scheda Trello. L’oggetto dell’e-mail Marketo viene visualizzato nel titolo della scheda Trello e il corpo dell’e-mail Marketo viene visualizzato nella descrizione della scheda Trello. È ad esempio possibile utilizzare &quot;AVVISO LEAD: `{{lead.First Name:default=edit me}}` `{{lead.Last Name:default=edit me}}` se si desidera che il nome e il cognome del lead siano nel titolo della scheda Trello. Quindi approva l’e-mail.
1. Passa a Smart Campaign. Fare clic su Flusso. Trascinare Invia avviso nel pannello centrale. Seleziona l’e-mail appena creata. Selezionare &quot;Invia a&quot; come Nessuno. Selezionare &quot;Ad altre e-mail&quot; come e-mail Trello dal passaggio due.
1. Fai clic su Pianifica nel menu principale. Fai clic su Attiva. Fai clic su Conferma.
1. Verifica l’integrazione. Una scheda con il nome e il cognome del lead nel titolo verrà visualizzata nella bacheca Trello. Per ulteriori informazioni, vedere la documentazione di [Trello](https://support.atlassian.com/trello/).

Pubblicato il _2014-11-18_ da _Murta_

## Trova lead per valore campo personalizzato

Supponiamo che tu voglia ottenere lead dall’API di Marketo che corrispondono a determinati criteri di attività o inattività. Ad esempio, potresti voler trovare un lead il cui punteggio non è cambiato negli ultimi 30 giorni. Seguendo i passaggi descritti in questo post, puoi ottenere questo elenco di lead. A tal fine, creiamo una campagna intelligente in Marketo che identifica i lead il cui punteggio non è cambiato negli ultimi 30 giorni, quindi memorizziamo un valore su questi lead per identificarli. Quindi eseguiremo una query sull’API con questo valore.

1. Crea un nuovo campo personalizzato denominato customLeadStatus** Accedi a Marketo e passa al pannello di amministrazione. Fai clic su Gestione campi. Fare clic su &quot;Nuovo campo personalizzato&quot;.  Denomina il campo. Quindi fai clic su Crea.
1. Crea una campagna avanzata con un elenco avanzato che cerca i lead che non sono stati aggiornati in 30 giorni.** Fai Clic Su Nuova Campagna Intelligente. Assegna un nome alla nuova campagna avanzata. Trascina senza punteggio è stato cambiato dal pannello di destra al pannello centrale.
1. Aggiungi un passaggio di flusso alla campagna avanzata dal passaggio 3 per aggiornare il campo customLeadStatus con un nuovo valore.** Trascina il valore dei dati di modifica dal pannello di destra al pannello centrale.
1. Aggiorna Smart Campaign per consentire l’esecuzione di lead più volte.** Fare Clic Su Pianifica. Quindi fai clic su Modifica.  Seleziona ogni volta. Quindi fai clic su Salva. La campagna ora inizierà a essere in esecuzione.
1. Eseguire una query su [Ottieni più lead per tipo di filtro API REST](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET). Fornire i parametri filterType=customLeadStatus &amp; filterValue=needEnrichment.**

Questa è una richiesta di esempio che restituisce questi dati.

`<https://AAA-BBB-CCC.mktorest.com/rest/v1/leads.json?access_token=><yourAccessToken>&filterType=customLeadStatus&filterValues=needsEnrichment`

Una chiamata API corretta restituisce dati JSON con lead il cui campo customLeadStatus corrisponde al valore di needEnrichment. Per ulteriori informazioni, vedere [Ottieni più lead per tipo di filtro API REST](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET).

Pubblicato il _2014-11-22_ da _Murta_

## Sincronizzazione delle opportunità tramite API SOAP

Questo post descrive come inserire opportunità in Marketo tramite l’API SOAP e associarle a società e lead. Inizia con una spiegazione di come funziona questo processo e fornisce esempi di codice per ciascuno degli scenari.

**Diagramma struttura tabella** Il diagramma seguente descrive innanzitutto la struttura della tabella. L’ID viene generato automaticamente al momento della creazione di un record (numero sequenziale). Il campo in grassetto è obbligatorio: solo il campo Ruolo persona opportunità è obbligatorio. I campi tra parentesi sono facoltativi, così come le connessioni con una linea tratteggiata.

**Inserimento opportunità di base** Prima si inserisce l&#39;opportunità, quindi il ruolo della persona dell&#39;opportunità, che collega l&#39;opportunità ai lead. Per questo esempio, utilizza correttamente l’ID lead e l’ID opportunità per specificare il ruolo di persona dell’opportunità. L’ID dell’opportunità viene visualizzato nella risposta di SOAP al momento della creazione dell’opportunità. L’ID lead è visibile su ogni lead nel database dei lead di Marketo.

**Collegamento società** Nella maggior parte dei casi, si desidera collegare un&#39;opportunità a un&#39;azienda, oltre a un singolo utente. In Marketo non puoi accedere ai record aziendali tramite l’API SOAP, ma solo ai record Lead (i record Lead contengono i campi società). È ancora possibile collegare le opportunità a un’azienda aggiungendo un identificatore aziendale univoco a ciascun lead e utilizzando tale ID nell’opportunità. Il passaggio 1 consiste nel creare un campo &quot;ID azienda&quot; sul record lead e compilarlo con un identificatore univoco, di solito da un sistema back-end. Il passaggio 2 consiste nel creare un campo &quot;ID azienda&quot; sull’opportunità: è necessario chiedere al supporto o alla consulenza Marketo di creare tale campo. Quindi compila questo campo durante la creazione di opportunità, che collega l’opportunità all’azienda. Questo è particolarmente importante se utilizzi Marketo Revenue Cycle Analytics e vuoi usare Opportunity Influence Analyzer (Analisi dell’influenza dell’opportunità), che dipende dalle informazioni aziendali.

**Utilizzo di identificatori esterni** In molti casi, è possibile disporre di identificatori univoci durante l&#39;integrazione con un sistema back-end. È possibile utilizzare questi identificatori univoci in Marketo tramite chiavi esterne. Per i lead, in genere si utilizza l’ID persona del sistema esterno (FSPID), che viene utilizzato al posto dell’ID Marketo o dell’indirizzo e-mail come identificatore univoco. Il FSPID è un campo di sistema nascosto, non visibile all’interno di Marketo. Se non lo stai già facendo, è necessario che la sincronizzazione delle opportunità salvi anche l’FSPID in un campo personalizzato, ad esempio &quot;ID esterno&quot; (puoi assegnare un nome qualsiasi al campo). Puoi creare questo campo autonomamente come amministratore Marketo. Per Opportunità, si dispone del supporto Marketo per creare un altro campo personalizzato sull’opportunità, ad esempio denominato &quot;ID esterno&quot; (è possibile denominarlo qualsiasi cosa). Quindi compila questo campo quando inserisci opportunità. Infine, quando crei il ruolo Persona opportunità, utilizzi entrambe le chiavi esterne per specificare il collegamento tra lead e opportunità, invece di utilizzare gli ID Marketo. È inoltre possibile utilizzare le chiavi esterne per aggiornare i lead di opportunità. Al momento, non è possibile aggiungere una chiave esterna ai record Ruolo persona dell’opportunità, pertanto per questa operazione dovrai utilizzare l’ID Marketo generato automaticamente (ottenuto nella risposta di SOAP dopo la creazione del ruolo Persona dell’opportunità).

**Esempio Di Codice** Passaggi: 1. Inserisci/aggiorna lead con chiave esterna e ID società 1. Inserisci opportunità con chiave esterna 1. Inserisci il ruolo della persona dell’opportunità con chiavi esterne 1. Richiesta SOAP: aggiornamento del lead esistente con chiave esterna e ID società Questo aggiorna un lead esistente (ID Marketo &quot;6&quot;) con la chiave esterna &quot;12346&quot; e l’ID account/società &quot;C123&quot;. Stiamo anche salvando la Chiave esterna in un campo personalizzato, perché ne abbiamo bisogno per il Ruolo Persona opportunità. L’utilizzo di una chiave esterna è facoltativo: puoi anche utilizzare il Marketo Id per collegare questo lead all’opportunità. Anche l’utilizzo dell’ID azienda è facoltativo, ma è necessario se desideri utilizzare Opportunity Influence Analyzer in RCA. Richiesta:

```xml
<soapenv:Envelope xmlns:soapenv="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:mkt="<http://www.marketo.com/mktows/">
   <soapenv:Header>
      <mkt:AuthenticationHeader>
         <mktowsUserId>\*\*\*</mktowsUserId>
         <requestSignature>\*\*\*</requestSignature>
         <requestTimestamp>2014-11-20T15:18:30-07:00</requestTimestamp>
      </mkt:AuthenticationHeader>
   </soapenv:Header>
   <soapenv:Body>
      <mkt:paramsSyncLead>
         <leadRecord>
            <Id>6</Id>
            <ForeignSysPersonId>12346</ForeignSysPersonId>
            <leadAttributeList>
               <attribute>
                  <attrName>FSPID</attrName>
                  <attrValue>12346</attrValue>
               </attribute>
               <attribute>
                  <attrName>cAccountFSID</attrName>
                  <attrValue>C123</attrValue>
               </attribute>
            </leadAttributeList>
         </leadRecord>
         <returnLead>false</returnLead>
      </mkt:paramsSyncLead>
   </soapenv:Body>
</soapenv:Envelope>
```

Risposta:

```xml
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:xsi="<http://www.w3.org/2001/XMLSchema-instance>" xmlns:ns1="<http://www.marketo.com/mktows/">
   <SOAP-ENV:Body>
      <ns1:successSyncLead>
         <result>
            <leadId>6</leadId>
            <syncStatus>
               <leadId>6</leadId>
               <status>UPDATED</status>
               <error xsi:nil="true"/>
            </syncStatus>
            <leadRecord xsi:nil="true"/>
         </result>
      </ns1:successSyncLead>
   </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

Richiesta SOAP - Creazione opportunità In questo caso, sono stati creati 2 campi personalizzati nella tabella Opportunità: - `opportunityId` → contiene l&#39;ID univoco dell&#39;opportunità - `cAccountFSID` → contiene il riferimento della società Anziché specificare un proprio ID opportunità. Puoi anche utilizzare l’ID opportunità generato da Marketo. In tal caso, si omette il nodo Chiave esterna. Anche l’associazione con l’azienda è facoltativa, ma è necessaria se desideri utilizzare Opportunity Influence Analyzer (Analisi influenza opportunità) in RCA. Richiesta:

```xml
<soapenv:Envelope xmlns:soapenv="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:mkt="<http://www.marketo.com/mktows/">
   <soapenv:Header>
      <mkt:AuthenticationHeader>
         <mktowsUserId>\*\*\*</mktowsUserId>
         <requestSignature>\*\*\*</requestSignature>
         <requestTimestamp>2014-11-20T15:03:28-07:00</requestTimestamp>
      </mkt:AuthenticationHeader>
   </soapenv:Header>
   <soapenv:Body>
      <mkt:paramsSyncMObjects>
         <mObjectList>
            <!--Zero or more repetitions:-->
            <mObject>
               <type>Opportunity</type>
               <externalKey>
                  <name>opportunityId</name>
                  <value>Opportunity_4</value>
               </externalKey>
               <attribList>
                  <attrib>
                     <name>opportunityId</name>
                     <value>Opportunity_4</value>
                  </attrib>
                  <attrib>
                     <name>Name</name>
                     <value>Opportunity 4 for ACME</value>
                  </attrib>
                  <attrib>
                     <name>IsClosed</name>
                     <value>1</value>
                  </attrib>
                  <attrib>
                     <name>IsWon</name>
                     <value>1</value>
                  </attrib>
                  <attrib>
                     <name>Amount</name>
                     <value>501.00</value>
                  </attrib>
                  <attrib>
                     <name>CloseDate</name>
                     <value>2014-10-24</value>
                  </attrib>
                  <attrib>
                     <name>ExpectedRevenue</name>
                     <value>501</value>
                  </attrib>
                  <attrib>
                     <name>Probability</name>
                     <value>100</value>
                  </attrib>
               </attribList>
               <associationList>
                  <mObjAssociation>
                     <mObjType>Company</mObjType>
                     <externalKey>
                        <name>cAccountFSID</name>
                        <value>C123</value>
                     </externalKey>
                  </mObjAssociation>
               </associationList>
            </mObject>
         </mObjectList>
         <operation>UPSERT</operation>
      </mkt:paramsSyncMObjects>
   </soapenv:Body>
</soapenv:Envelope>
```

Risposta:

```xml
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:ns1="<http://www.marketo.com/mktows/">
   <SOAP-ENV:Body>
      <ns1:successSyncMObjects>
         <result>
            <mObjStatusList>
               <mObjStatus>
                  <id>40</id>
                  <externalKey>
                     <name>opportunityId</name>
                     <value>Opportunity_4</value>
                  </externalKey>
                  <status>CREATED</status>
               </mObjStatus>
            </mObjStatusList>
         </result>
      </ns1:successSyncMObjects>
   </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

Richiesta SOAP: ruolo della persona dell’opportunità Questa richiesta collega il lead all’opportunità. È possibile specificare più collegamenti in una singola richiesta SOAP (in questo esempio l’opportunità viene collegata a un solo lead). In questo modo vengono utilizzate le chiavi esterne per specificare il collegamento, ma nei commenti viene anche mostrato come utilizzare gli ID effettivi (in questo caso: 6 per l’ID lead e 40 per l’ID opportunità). I campi &quot;IsPrimary&quot; e &quot;Role&quot; sono facoltativi. Richiesta:

```xml
<soapenv:Envelope xmlns:soapenv="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:mkt="<http://www.marketo.com/mktows/">
   <soapenv:Header>
      <mkt:AuthenticationHeader>
         <mktowsUserId>\*\*\*</mktowsUserId>
         <requestSignature>\*\*\*</requestSignature>
         <requestTimestamp>2014-11-20T15:18:30-07:00</requestTimestamp>
      </mkt:AuthenticationHeader>
   </soapenv:Header>
   <soapenv:Body>
      <mkt:paramsSyncMObjects>
         <mObjectList>
            <!--Zero or more repetitions:-->
            <mObject>
               <type>OpportunityPersonRole</type>
               <attribList>
                  <attrib>
                     <name>IsPrimary</name>
                     <value>1</value>
                  </attrib>
                  <attrib>
                     <name>Role</name>
                     <value>Marketing Manager</value>
                  </attrib>
               </attribList>
               <associationList>
                  <mObjAssociation>
                     <mObjType>Lead</mObjType>
                     <!--id>6</id-->
                     <externalKey>
                      <name>FSPID</name>
                      <value>12346</value>
                     </externalKey>
                  </mObjAssociation>
                  <mObjAssociation>
                     <mObjType>Opportunity</mObjType>
                     <!--id>40</id-->
                     <externalKey>
                      <name>opportunityId</name>
                      <value>Opportunity_4</value>
                    </externalKey>
                  </mObjAssociation>
               </associationList>
            </mObject>
         </mObjectList>
         <operation>UPSERT</operation>
      </mkt:paramsSyncMObjects>
   </soapenv:Body>
</soapenv:Envelope>
```

Risposta:

```xml
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:ns1="<http://www.marketo.com/mktows/">
   <SOAP-ENV:Body>
      <ns1:successSyncMObjects>
         <result>
            <mObjStatusList>
               <mObjStatus>
                  <id>11</id>
                  <status>CREATED</status>
               </mObjStatus>
            </mObjStatusList>
         </result>
      </ns1:successSyncMObjects>
   </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

**Approccio alternativo (eseguire i passaggi 2 e 3 in una chiamata)** Anche se è possibile inserire prima l&#39;opportunità e poi il ruolo della persona dell&#39;opportunità, è possibile eseguire questa operazione in una chiamata SOAP. Tuttavia, devi utilizzare la chiave esterna per l’opportunità (non puoi utilizzare l’ID opportunità generato automaticamente nel ruolo di persona dell’opportunità, perché l’opportunità non è ancora stata generata). Naturalmente, puoi anche collegare più lead a questa opportunità nella stessa chiamata API (questo esempio collega l’opportunità a solo 1 lead). Richiesta:

```xml
<soapenv:Envelope xmlns:soapenv="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:mkt="<http://www.marketo.com/mktows/">
   <soapenv:Header>
      <mkt:AuthenticationHeader>
         <mktowsUserId>\*\*\*</mktowsUserId>
         <requestSignature>\*\*\*</requestSignature>
         <requestTimestamp>2014-11-20T15:44:08-07:00</requestTimestamp>
      </mkt:AuthenticationHeader>
   </soapenv:Header>
   <soapenv:Body>
      <mkt:paramsSyncMObjects>
         <mObjectList>
            <!--Zero or more repetitions:-->
            <mObject>
               <type>Opportunity</type>
               <externalKey>
                  <name>opportunityId</name>
                  <value>Opportunity_5</value>
               </externalKey>
               <attribList>
                  <attrib>
                     <name>opportunityId</name>
                     <value>Opportunity_5</value>
                  </attrib>
                  <attrib>
                     <name>Name</name>
                     <value>Opportunity 5 for ACME</value>
                  </attrib>
                  <attrib>
                     <name>IsClosed</name>
                     <value>1</value>
                  </attrib>
                  <attrib>
                     <name>IsWon</name>
                     <value>1</value>
                  </attrib>
                  <attrib>
                     <name>Amount</name>
                     <value>1500</value>
                  </attrib>
                  <attrib>
                     <name>CloseDate</name>
                     <value>2014-10-24</value>
                  </attrib>
                  <attrib>
                     <name>ExpectedRevenue</name>
                     <value>1500</value>
                  </attrib>
                  <attrib>
                     <name>Probability</name>
                     <value>100</value>
                  </attrib>
               </attribList>
               <associationList>
                  <mObjAssociation>
                     <mObjType>Company</mObjType>
                     <externalKey>
                        <name>cAccountFSID</name>
                        <value>C123</value>
                     </externalKey>
                  </mObjAssociation>
               </associationList>
            </mObject>
             <mObject>
               <type>OpportunityPersonRole</type>
               <attribList>
                  <attrib>
                     <name>IsPrimary</name>
                     <value>1</value>
                  </attrib>
                  <attrib>
                     <name>Role</name>
                     <value>Marketing Manager</value>
                  </attrib>
               </attribList>
               <associationList>
                  <mObjAssociation>
                     <mObjType>Lead</mObjType>
                     <!--id>6</id-->
                     <externalKey>
                      <name>FSPID</name>
                      <value>12346</value>
                     </externalKey>
                  </mObjAssociation>
                  <mObjAssociation>
                     <mObjType>Opportunity</mObjType>
                     <externalKey>
                      <name>opportunityId</name>
                      <value>Opportunity_5</value>
                    </externalKey>
                  </mObjAssociation>
               </associationList>
            </mObject>
         </mObjectList>
         <operation>UPSERT</operation>
      </mkt:paramsSyncMObjects>
   </soapenv:Body>
</soapenv:Envelope>
```

Risposta:

```xml
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:ns1="<http://www.marketo.com/mktows/">
   <SOAP-ENV:Body>
      <ns1:successSyncMObjects>
         <result>
            <mObjStatusList>
               <mObjStatus>
                  <id>41</id>
                  <externalKey>
                     <name>opportunityId</name>
                     <value>Opportunity_5</value>
                  </externalKey>
                  <status>CREATED</status>
               </mObjStatus>
               <mObjStatus>
                  <id>12</id>
                  <status>CREATED</status>
               </mObjStatus>
            </mObjStatusList>
         </result>
      </ns1:successSyncMObjects>
   </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

Pubblicato il _2014-11-26_ da _Jep_

## Richieste REST API multithread

Se desideri migliorare le prestazioni quando chiami l’API Marketo, puoi effettuare richieste simultanee. Questo approccio consente di ottenere più dati in un periodo di tempo più breve. Quando si effettua una richiesta API, parte del tempo di andata e ritorno tra il client e il server è il tempo di trasferimento sul cavo. Quindi, se possiamo ridurre il tempo di trasferimento su rete cablata per le richieste in aggregato, miglioriamo le prestazioni. Il codice di esempio seguente mostra come eseguire questa operazione in Ruby. Utilizza EventMachine, una libreria di elaborazione eventi [utilizzata per eseguire richieste multithread](https://github.com/igrigorik/em-http-request/wiki/Parallel-Requests). L&#39;esempio seguente chiama l&#39;API [Lead Activities](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadActivitiesUsingGET) ed effettua due richieste simultanee. Questo approccio elimina il tempo di trasferimento dal client al server per la seconda richiesta. A tal fine, include la seconda richiesta contemporaneamente alla prima. Le risposte API vengono scritte in un file di testo.

```java
require 'em-http-request'
require 'json'

# Build request URL
# Replace AAA-BBB-CCC with your Marketo instance
marketo_instance = "<https://AAA-BBB-CCC.mktorest.com>"
endpoint = "/rest/v1/activities.json"
# Replace with your access token
auth_token =  "?access_token=" + "ac756f7a-d54d-41ac-8c3c-f2d2a39ee325:ab"
# Specify datetime needed as nextPageToken
since_date_time = ["&nextPageToken=A5YMOYZQBOGD2OSYYBYDAQGEMGLBDGDANAABQGRAQWAAKKID", "&nextPageToken=GIYDAOBNGEYS2MBWKQYDAORQGA5DAMBOGAYDAKZQGAYDALBQ"]
# Specify activities needed
activity_type_ids = "&activityTypeIds=1&activityTypeIds=12"
requesturl_a = marketo_instance + endpoint + auth_token + since_date_time.at(0) + activity_type_ids
requesturl_b = marketo_instance + endpoint + auth_token + since_date_time.at(1) + activity_type_ids

# Make request
EventMachine.run do
  http1 = EventMachine::HttpRequest.new(requesturl_a).get
  http2 = EventMachine::HttpRequest.new(requesturl_b).get

# When API response is received, write response to a text file
  http1.callback {
  File.open('response1.txt', 'w') do |t|
  t.puts http1.response
  end }

  http2.callback {
  File.open('response2.txt', 'w') do |t|
  t.puts http2.response
  end }
end
```

Pubblicato il _2014-12-03_ da _Murta_

## Richieste API di ottimizzazione delle prestazioni

Questo post illustra strategie per migliorare le prestazioni quando si richiedono dati dall’API di Marketo. Tuttavia, è necessario valutare i vantaggi di queste strategie rispetto al vincolo operativo dei limiti giornalieri delle API di Marketo.
**Strategia 1 - Richiedi meno dati in ogni chiamata API** In genere, quando richiedi più dati in una chiamata API, aumenta il tempo necessario per cercare i dati nel database dal server Marketo. Se effettui una chiamata API con intervalli di date, ad esempio l&#39;API SOAP [getMultipleLeads](/help/soap-api/getmultipleleads.md), riduci l&#39;intervallo di tempo per chiamata e compensa con altre chiamate. Ad esempio, invece di richiedere i dati dal 1° giugno al 1° luglio, richiedi un singolo giorno alla volta, ad esempio una chiamata per i giorni dal 1° al 2 giugno e un’altra per i giorni dal 2 al 1 giugno. Se effettui una chiamata API che restituisce dati dai campi lead di Marketo, richiedi solo i campi necessari. Ogni campo lead aggiuntivo aumenta in modo incrementale la quantità di tempo che una chiamata API richiede. Un altro approccio consiste nel ridurre la dimensione del batch o il numero di lead richiesti per chiamata.
**Strategia 2 - Richieste simultanee** Per migliorare le prestazioni ed estrarre più dati contemporaneamente. Puoi effettuare richieste simultanee all’API. Questo approccio riduce il tempo impiegato dalle richieste API wire per aggregare i dati. Ad esempio, supponiamo che tu stia effettuando richieste al metodo Get Multiple Leads by Filter Type (Ottieni più lead per tipo di filtro). Puoi effettuare richieste simultanee per una richiesta eseguendo una query sui lead da 1 a 300 e per un’altra richiesta eseguendo una query sui lead da 301 a 600.
**Strategia 3 - Memorizza dati nella cache** Alcuni dati in Marketo vengono modificati meno frequentemente, ad esempio l&#39;elenco dei campi lead, rispetto ad altri dati, ad esempio i dati relativi all&#39;attività lead. Se memorizzi in cache dati che vengono aggiornati meno frequentemente, riduci il numero di chiamate API da effettuare. Inoltre, si otterranno prestazioni migliori perché la ricerca dei dati in locale è generalmente più veloce rispetto all’accesso da un servizio web remoto.

Pubblicato il _2014-12-05_ da _Murta_

## Invia dati Marketo Form a Google Analytics

In Google Analytics, puoi inviare eventi di dati personalizzati, quindi utilizzare i dati per segmentare e analizzare le prestazioni del sito web. Lo snippet di codice JavaScript riportato di seguito consente di inviare automaticamente i dati del modulo Marketo 2.0 a Google Analytics dopo che un visitatore ha inviato un modulo web. Ecco come configurarlo.

**Passaggio uno** Inserisci il tag JavaScript in qualsiasi pagina che includa Marketo Forms nella parte inferiore del codice (prima del tag). JavaScript invia solo campi non nascosti (sendHiddenFields : false). È possibile regolare questo valore modificando sendHiddenFields da false a true. Puoi anche selezionare i campi da escludere aggiungendo ID di campo aggiuntivi nell’array &quot;fieldsToExclude&quot;.

```javascript
function pushFormDataToGa(a){
setTimeout(function () {
document.getElementsByTagName('form')[0].getElementsByClassName(a.submitButton)[0].addEventListener('click', function() {
  allFields = document.getElementsByTagName('form')[0].getElementsByTagName('input');
  for(i=0;i<allFields.length;i++){
   if( (allFields[i].type !="hidden" && allFields[i].type !="submit" && allFields[i].value !="" && a.fieldsToExclude.indexOf(allFields[i].id) === -1  ) || (allFields[i].type === "hidden" && a.sendHiddenFields) ){
    console.log( allFields[i].name + ": "  + allFields[i].value);
    if(typeof(_gaq) != "undefined"){
    //Classic
    _trackEvent("Marketo Form Submission", allFields[i].value , allFields[i].name
{'nonInteraction': 1});
    }else if(typeof(ga) !="undefined"){
    //Universal
    ga('send', 'event',"Marketo Form Submission", allFields[i].value , allFields[i].name, {'nonInteraction': 1});
}}}}, false);
}, 3000);}
pushFormDataToGa({
 submitButton: "mktoButton",
 fieldsToExclude: ["Email","LastName", "FirstName"],
 sendHiddenFields : false
});
```

**Passaggio due** I dati in GA vengono visualizzati nella sezione Report. Vai a Comportamento > Eventi > Eventi principali. **Limitazioni script:** - Questo esempio di codice è compatibile solo con [Marketo Forms 2.0](/help/javascript-api/forms-api-reference.md). - In base all&#39;informativa sulla privacy di Google, non è consentito inviare informazioni personali (e-mail o nome). A parte potenziali problemi di privacy, queste sono informazioni personali identificabili e quindi violano i [Termini di servizio di Google Analytics](https://marketingplatform.google.com/about/analytics/terms/us/):

&quot;Non puoi (e non consentirai a terze parti di) utilizzare il Servizio per monitorare, raccogliere o caricare dati che identificano personalmente un individuo (come un nome, un indirizzo e-mail o informazioni di fatturazione), o altri dati che possono essere ragionevolmente collegati a tali informazioni da Google.&quot;

Pubblicato il _2014-12-16_ da _Yanir_

## Aggiungere un campo Nome completo a un Marketo Form

Sappiamo che i moduli web più brevi migliorano i tassi di conversione. L&#39;esempio di codice JavaScript seguente consente di rendere i moduli ancora più brevi unendo i campi Nome e Cognome in un unico campo Nome completo. Quando i visitatori digitano il nome completo, lo script suddivide automaticamente il testo in campi di nome e cognome. Per i visitatori noti, lo script unisce il nome e il cognome, quindi li copia nel nuovo campo in modo che non debbano riempire nuovamente il campo. Ecco come configurarlo.

**Passaggio uno** Crea un nuovo campo personalizzato in Marketo denominato Nome completo. Non è necessario crearlo nella piattaforma CRM, in quanto lo script utilizzerà questo campo solo per visualizzare il nome completo.
**Passaggio due** Aggiungi questo campo a tutti i tuoi moduli web. Impostare i campi Nome e Cognome come nascosti. In JavaScript, modifica la configurazione &quot;splitFullName&quot; in modo che contenga i 3 nomi di campo. Nota: accertati che questi nomi non vengano visualizzati altrove nella pagina.
**Passaggio tre** Inserisci il JavaScript in tutte le pagine di destinazione nella parte inferiore del codice, prima del tag.

```javascript
<script>
MktoForms2.whenReady(function (form){
        function splitFullName(a,b,c){
                String.prototype.capitalize = function(){
                        return this.replace( /(^|s)([a-z])/g , function(m,p1,p2){ return p1+p2.toUpperCase(); } );
                };
                document.getElementsByName[c](0).oninput=function(){
                        var fullName = document.getElementsByName[c](0).value;
                        if((fullName.match(/ /g) || []).length ===0 || fullName.substring(fullName.indexOf(" ")+1,fullName.length) === ""){
                                var first = fullName.capitalize();;
                                var last = "null";
                        }else if(fullName.substring(0,fullName.indexOf(" ")).indexOf(".")>-1){
                                var first = fullName.substring(0,fullName.indexOf(" ")).capitalize() + " " + fullName.substring(fullName.indexOf(" ")+1,fullName.length).substring(0,fullName.substring(fullName.indexOf(" ")+1,fullName.length).indexOf(" ")).capitalize();
                                var last = fullName.substring(first.length +1,fullName.length).capitalize();
                        }else{
                                var first = fullName.substring(0,fullName.indexOf(" ")).capitalize();
                                var last = fullName.substring(fullName.indexOf(" ")+1,fullName.length).capitalize();
                        }
                        document.getElementsByName[a](0).value = first;
                        document.getElementsByName[b](0).value = last;
                };
                //Initial Values
                if(document.getElementsByName[c](0).value.length < 2 && document.getElementsByName[b](0).value.length.length >2 && document.getElementsByName[a](0).value.length.length >2 ){
                        var first = document.getElementsByName[a](0).value.capitalize();
                        var last = document.getElementsByName[b](0).value.capitalize();
                        var fullName =  first + " " + last ;
                        console.log(fullName);
                        document.getElementsByName[c](0).value = fullName;
                }
        }
        splitFullName("FirstName","LastName","leadFullName");
});
</script>
```

Nota: questo codice funziona solo con Marketo Forms 2.0.

Pubblicato il _2014-12-16_ da _Yanir_

## Utilizzare cURL per importare lead tramite l’API REST

Vuoi importare lead da un file CSV tramite l’API REST, ma hai notato che è difficile farlo utilizzando l’estensione Postman Chrome. In questo post, esaminiamo come eseguire questa operazione con cURL.

1. [Scarica e installa cURL](https://curl.se/download.html), uno strumento per riga di comando che utilizziamo per inviare dati all&#39;API REST di Marketo.
1. Apri la riga di comando e passa alla posizione in cui si trova il file CSV. Le intestazioni di colonna nel file CSV devono corrispondere ai nomi dei campi API, non ai nomi dei campi Marketo.
1. È necessario un token di accesso. Accedi a Marketo, vai ad Admin, quindi a LaunchPoint. Trova l’utente REST API e fai clic su &quot;Visualizza dettagli&quot;. Fai clic sul pulsante &quot;Ottieni token&quot;.
1. Sarà inoltre necessario l’endpoint REST specifico per la tua istanza di Marketo. Accedere a Marketo, quindi ad Admin e infine a Web Services. Nella sezione contrassegnata &quot;REST API&quot; trovi l’URL dell’endpoint.
1. Nella riga di comando, segui questo formato per la chiamata cURL. Sostituisci `<accesstoken>` con il tuo token di accesso dal passaggio tre e sostituisci `<REST API Endpoint URL>` con l&#39;URL dell&#39;endpoint API REST dal passaggio quattro. Ulteriori informazioni sono [disponibili qui](https://developer.adobe.com/marketo-apis/api/mapi/#operation/importLeadUsingPOST). Il &quot;/bulk&quot; qui sostituirà il &quot;/rest&quot; alla fine dell’URL dell’endpoint. Se l’endpoint è impostato per /rest/bulk, viene restituito un errore.

`curl -i -F format=csv -F file=@leaddata.csv -F access_token=<accesstoken> <REST API Endpoint URL>/bulk/v1/leads.json`

Pubblicato il _2014-12-16_ da _Giordania_

## Aggiungere un avviso di conferma a un Marketo per

Supponiamo che, quando un utente fa clic sul pulsante &quot;Invia&quot; in un modulo di Marketo, desideri visualizzare una notifica che chieda all’utente se &quot;È davvero opportuno inviare?&quot;. Ciò è possibile implementando alcune righe di JavaScript, che mostreranno una casella di conferma quando qualcuno fa clic sul pulsante Invia. Ecco un esempio di come farlo. Aggiungere la funzione onSubmit al modulo Marketo come illustrato di seguito. Per ulteriori informazioni sull&#39;API Forms di Marketo, [consulta la documentazione per gli sviluppatori](/help/javascript-api/forms-api-reference.md).

```javascript
<script src="//app-e.marketo.com/js/forms2/js/forms2.js"></script>
<form id="mktoForm_19"></form>
<script>
MktoForms2.loadForm("//app-e.marketo.com", "212-RBI-463", 19,function(form){

//Add this function to your Marketo form script
form.onSubmit(function(){
alert("Do you really want to submit the form?");
});
});
</script>
```

Pubblicato il _2014-12-17_ da _David_

## Mostra il messaggio di ringraziamento senza una pagina di destinazione di completamento

In genere, quando si utilizzano i moduli di Marketo, si creano due pagine di destinazione, una in cui inserire il modulo e una in cui reindirizzarlo dopo il completamento del modulo. Tuttavia, in alcuni casi potrebbe non essere utile avere due pagine di destinazione separate ma molto simili da mantenere. Puoi utilizzare la stessa pagina di destinazione per il modulo e per il messaggio di ringraziamento, utilizzando l’API JavaScript di Forms 2.0. A questo scopo, crea innanzitutto la pagina di destinazione e il modulo di registrazione e inserisci il modulo nella pagina di destinazione come faresti normalmente. Quindi, aggiungi un elemento HTML alla pagina. In questo elemento viene aggiunto del codice che si attiva nel momento in cui il modulo viene inviato. Il modulo verrà quindi nascosto e verrà visualizzato un <div> che contiene il messaggio di ringraziamento. Il JavaScript dovrebbe essere simile al seguente:

```javascript
//Edit host with your Marketo instance info
<script src="//<host>/js/forms2/js/forms2.js"></script>
<script>
MktoForms2.whenReady(function (form){
 //Add an onSuccess handler
  form.onSuccess(function(values, followUpUrl){
   //get the form's jQuery element and hide it
   form.getFormElem().hide();
   document.getElementById('confirmform').style.visibility = 'visible';
   //return false to prevent the submission handler from taking the lead to the follow up url.
   return false;
 });
});
</script>
```

Modifica il testo del messaggio di ringraziamento.

`<div id="confirmform" style="visibility:hidden;"><p><strong>Thank you. Check your email for details on your request.</strong></p></div>`

Puoi modificare il nome host e il messaggio di ringraziamento nell’esempio di codice. Il primo deve fare riferimento all&#39;istanza di Marketo (ad esempio &quot;//app-sj06.marketo.com/js/forms2/js/forms2.js&quot;) e il secondo deve contenere il testo di ringraziamento che si desidera visualizzare una volta completato il modulo. Il testo viene visualizzato nella pagina di destinazione nella posizione esatta in cui si posiziona l&#39;elemento HTML, pertanto è necessario modificarlo nella finestra delle proprietà. È inoltre necessario assicurarsi che il livello dell&#39;elemento HTML sia più piccolo del livello del modulo. Per impostazione predefinita, entrambi saranno posizionati al livello 15, quindi sei sicuro se realizzi l’elemento HTML Livello 11. Se non si esegue questa operazione, non sarà possibile digitare nelle caselle dei campi modulo sovrapposte al messaggio di ringraziamento. Non è necessario modificare il tipo di follow-up nel modulo o nella pagina di destinazione, in quanto JavaScript sovrascriverà tali impostazioni. Per ulteriori informazioni sull&#39;API Forms di Marketo, consulta la [documentazione per gli sviluppatori](/help/javascript-api/forms-api-reference.md).

Pubblicato il _2014-12-19_ da _Kristin_

## Evidenziazione dei progetti Open Source realizzati sulla piattaforma Marketo

Questo è il primo post di una serie in corso che evidenzia progetti open-source creati intorno alla piattaforma Marketo dalla community di sviluppatori. Abbiamo [un elenco sull&#39;account GitHub di Marketo](https://github.com/Marketo/Community-Supported-Client-Libraries) in cui teniamo traccia delle librerie client e dei progetti creati dalla community di sviluppatori Marketo. Di seguito sono riportati tre progetti sviluppati intorno alle API REST e SOAP di Marketo. Daniel Chesterton ha creato [una libreria client in PHP per l&#39;API REST di Marketo](https://github.com/dchesterton/marketo-rest-api). Al momento la libreria client ha una copertura per 12 endpoint API REST.** Kyle Halstvedt di Elixiter ha creato un progetto per [richiamare i lead dagli elenchi statici di Marketo in un foglio di calcolo di Google](https://github.com/Elixiter/mkto_google-spreadsheet). Il progetto di Kyle utilizza l&#39;API REST di Marketo.  David Santoso ha creato un [gioiello rubino per l&#39;API SOAP di Marketo.](https://github.com/davidsantoso/markety) Questo progetto può aiutarti a integrare più rapidamente l&#39;API SOAP di Marketo con un&#39;app Ruby on Rails.  Siamo entusiasti di vedere altri progetti creati dalla community di sviluppatori sulla piattaforma Marketo. Se stai lavorando a un progetto open source per la piattaforma Marketo, [invialo a questo archivio GitHub tramite una richiesta pull](https://github.com/Marketo/Community-Supported-Client-Libraries).

Pubblicato il _2015-01-02_ da _Murta_

## Modifica dinamica del contenuto della pagina in base alla posizione di un utente

Supponiamo che tu voglia modificare dinamicamente il numero di telefono in una pagina di destinazione a seconda di dove si trova un utente. Ad esempio, se la persona si trova in California, puoi mostrare il numero di telefono dell’ufficio in California nella pagina di destinazione; se invece si trova in Giappone, puoi mostrare il numero di telefono dell’ufficio in Giappone.  Un modo per implementarlo è utilizzare JavaScript e l&#39;API di geolocalizzazione [HTML5](https://developer.mozilla.org/en-US/docs/Web/API/Geolocation_API). Il vantaggio di questo approccio è che è possibile creare una pagina di destinazione e modificarla in modo dinamico in base alla posizione di un utente, anziché più pagine di destinazione statiche. Passiamo ai dettagli tecnici dell’implementazione riportati di seguito. Creiamo un oggetto per le sedi di ufficio con coordinate di latitudine e longitudine e un secondo oggetto con numeri di telefono di ufficio. Nella produzione, sarebbe preferibile combinare questi due oggetti in un unico oggetto.

```json
//Coordinates for Marketo offices
var officeLocations = {
    "San Mateo": {latitude: 37.5596465, longitude: -122.2870142},
    "Atlanta": {latitude: 33.8547013, longitude: -84.35552349999999},
    "Tokyo": {latitude: 35.6895, longitude: 139.6917},
    "Dublin": {latitude: 53.3478, longitude: -6.2603097},
    "Sydney": {latitude: -33.873651, longitude: 151.2068896},
    "Portland": {latitude: 45.512089, longitude: -122.6763367},
    "Tel Aviv": {latitude: 32.0852999, longitude: 34.78176759999999}
}

//Phone numbers for Marketo offices
var officePhoneNumbers = {
    "San Mateo": "+1-650-376-2300",
    "Atlanta": "+1-877-260-6586",
    "Tokyo": "+81-03-6759-8280",
    "Dublin": "+353-1-242-3000",
    "Sydney": "+61-2-9045-2711",
    "Portland": "+1-877-260-6586",
    "Tel Aviv": "+1-877-260-6586"
}
```

Creiamo un metodo per richiedere la posizione di un utente. Per gestire gli errori, se la posizione dell’utente non è accessibile, per impostazione predefinita viene utilizzata la sede centrale di Marketo e il relativo numero di telefono.

```javascript
//Method to get user's current location. Returns a position object with user's geo coordinates
function getLocation() {
    if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(findNearestOffice);
    } else {
        x.innerHTML = "Marketo Location: San Mateo
Marketo Phone Number: +1-877-260-6586";
    }
}
```

Infine, viene creato un metodo per trovare l&#39;ufficio più vicino alla posizione dell&#39;utente, quindi viene restituito il numero di telefono dell&#39;ufficio più vicino sulla pagina. Questo metodo utilizza il metodo findNearest di [Geolib, una libreria JavaScript che fornisce operazioni geospaziali](https://github.com/manuelbieh/Geolib).

```javascript
//Find nearest Marketo office to user's location
function findNearestOffice(position) {
        var nearestOffice = geolib.findNearest({latitude: position.coords.latitude, longitude: position.coords.longitude}, officeLocations);
        x.innerHTML = "Marketo Location: " + nearestOffice.key + "
Marketo Phone Number: " +  officePhoneNumbers[nearestOffice.key];
}
```

Ecco la completa implementazione. Il metodo getLocation viene attivato quando l’utente fa clic sul pulsante nella pagina. L&#39;archivio [GitHub](https://github.com/MurtzaM/Find-Nearest-Marketo-Office) contiene i file necessari per configurare questa demo.

Pubblicato il _2014-12-20_ da _Murta_

## Tracciamento dei lead e più domini

Il codice di tracciamento Munchkin di Marketo consente di monitorare le visite al sito web. È probabile che tu voglia utilizzare il codice di tracciamento di Munchkin per cookie di lead anonimi per la maggior parte o tutte le pagine del tuo sito web. Passiamo ora al funzionamento di Munchkin. Le visite alla pagina vengono registrate per i lead esistenti; se un visitatore non cookie visita la pagina, verrà creato e memorizzato un nuovo cookie e verrà creato un nuovo lead anonimo nel database di Marketo. Il tracker di Munchkin inserirà automaticamente il cookie di un visitatore se non dispone già di un cookie esistente per il dominio corrente. In Marketo, registra l’evento (fai clic su un collegamento, visita una pagina web o un nuovo lead) nel registro delle attività del lead. Il valore memorizzato nel cookie è univoco per un visitatore specifico. Il valore è una combinazione dell’ID di tracciamento dell’account Munchkin univoco, del nome di dominio, della marca temporale e di un numero intero casuale.
**Cosa succede se ho più domini?** Si supponga di disporre di due siti di cui si desidera tenere traccia: `<www.apples.com>` e `<www.bananas.com>`. Puoi inserire il codice di tracciamento in entrambi i siti, tuttavia è necessario considerare quanto segue. I cookie di Marketo sono &quot;cookie di prime parti&quot; e sono quindi specifici del dominio. Ciò significa che un visitatore del sito 1 verrà creato come lead anonimo in Marketo; se lo stesso lead passa al sito 2, verrà creato un secondo lead anonimo separato in Marketo. Se il lead compila un modulo sul sito 1 e poi questo record diventa noto, il record anonimo per il sito 2 rimarrà e continuerà ad accumulare visite successive a quel sito. Se il lead compila un modulo sul sito 2 con lo stesso indirizzo e-mail utilizzato sul sito 1, entrambi i lead noti verranno uniti automaticamente e tutti i comportamenti passati e futuri verranno tracciati su un singolo record in Marketo. Entrambi gli ID cookie sono associati allo stesso lead e tutte le attività Web (da entrambi i domini) si troveranno su tale lead.
**E per quanto riguarda più sottodomini?** I sottodomini non sono un problema. Usiamo Marketo.com come esempio. Dispone di più sottodomini per diverse lingue, ad esempio fr.marketo.com e de.marketo.com. Con i sottodomini tutte le attività vengono registrate in base allo stesso record/cookie di lead.

Pubblicato il _2015-01-13_ da _David_

## Modificare il colore del testo dei suggerimenti in un Marketo Form

Si supponga che si desideri modificare il colore del testo del suggerimento (detto anche testo segnaposto) in Forms 2.0. Ciò è possibile tramite CSS personalizzato. Ad esempio, nella schermata seguente ho fatto il testo del suggerimento in questo Marketo Form blu. Sono disponibili tre opzioni per eseguire questa operazione, a seconda di come si utilizza Marketo Forms.

**Opzione 1: se incorpori un modulo Marketo, aggiungi il file CSS seguente direttamente al file CSS principale.**

```css
::-webkit-input-placeholder {
  color: blue;
}
::-moz-placeholder {
  color: blue;
}
:-ms-input-placeholder {
  color: blue;
}
:-moz-placeholder {
  color: blue;
}
```

**Opzione 2: quando si incorpora un modulo di Marketo, è possibile aggiungere il CSS direttamente nella pagina tra `<style></style>` tag nella sezione `<head>`.**

```css
<style>
::-webkit-input-placeholder {
  color: blue;
}
::-moz-placeholder {
  color: blue;
}
:-ms-input-placeholder {
  color: blue;
}
:-moz-placeholder {
  color: blue;
}
</style>
```

**Opzione 3: se utilizzi un modulo Marketo in una pagina di destinazione di Marketo, puoi aggiungere questo file CSS personalizzato tramite l’interfaccia utente di Marketo.** Trovare la pagina di destinazione nella struttura di navigazione di Marketo. Quindi fate clic su Modifica bozza (Edit Draft). Fai clic su Modifica metatag pagina. Aggiungi il CSS seguente alla sezione HTML HEAD personalizzato. I tag `<style></style>` devono essere inclusi.

```css
<style>
::-webkit-input-placeholder {
  color: blue;
}
::-moz-placeholder {
  color: blue;
}
:-ms-input-placeholder {
  color: blue;
}
:-moz-placeholder {
  color: blue;
}
</style>
```

Fate clic su Approva bozza (Approve Draft). Quando visiti la pagina di destinazione di Marketo, il testo del suggerimento è il colore definito nel CSS. Per ulteriori informazioni su Marketo Forms, [consulta la documentazione](/help/javascript-api/forms-api-reference.md).

Pubblicato il _2015-01-14_ da _Murta_

## Ottenere i dati di attività tramite l’API REST

Supponiamo che tu voglia ottenere tutti i lead che sono stati aggiunti a un elenco questo mese. Utilizzando l&#39;API REST [Get Lead Activities](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadActivitiesUsingGET), puoi ottenere questi dati. Prima di chiamare l&#39;API Get Lead Activities, è necessario ottenere un token di accesso dall&#39;API di autenticazione e un token di data di inizio dall&#39;[Get Paging Token API](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getActivitiesPagingTokenUsingGET). Di seguito è riportato un codice di esempio in Ruby che illustra i singoli endpoint API che dovresti chiamare per restituire tutti i lead aggiunti a un elenco questo mese. 1. Ottieni token di accesso**

```ruby
require 'rest_client'
require 'json'

# Build request URL
# Replace AAA-BBB-CCC with your Marketo instance
marketo_instance = "<https://AAA-BBB-CCC.mktorest.com/identity/oauth/token?grant_type=client_credentials>"
# Relace with your client id
client_id = "99985d09-22a9-3jl2-84av-f5baae7c3a45"
# Replace with your your  client secret
client_secret = "tZPVrKiEmUDezE18yZfeaPlTJ2vKn2fw"
request_url = marketo_instance + "&client_id=" + client_id + "&client_secret=" + client_secret

# Make request
response = RestClient.get request_url

# Parse reponse and return only access token
results = JSON.parse(response.body)
access_token = results["access_token"]
puts access_token
```

1. Ottieni token di paging

```ruby
require 'rest_client'
require 'json'

# Build request URL
# Replace AAA-BBB-CCC with your Marketo instance
marketo_instance = "<https://AAA-BBB-CCC.mktorest.com>"
endpoint = "/rest/v1/activities/pagingtoken.json"
# Replace with your access token
auth_token =  "?access_token=" + "ac756f7a-d54d-41ac-8c3c-f2d2a39ee325:ab"
# Specify date
since_date_time = "&sinceDatetime=2015-01-01T00:00:00-08:00"
request_url = marketo_instance + endpoint + auth_token + since_date_time

# Make request
response = RestClient.get request_url

# Returns Marketo API response
puts response
```

1. Ottieni dati attività** per determinare l&#39;ID del tipo di attività necessario per questa chiamata, eseguire una query sull&#39;API [Gotten Activity Types](/help/rest-api/activities.md). L’API Get Activity Types restituisce uno schema con tutti i tipi di attività e gli ID associati. Ad esempio, restituisce l’ID 12 per i nuovi lead creati e l’ID 1 per la visita della pagina web.

```java
require 'rest_client'
require 'json'

# Build request URL
# Replace AAA-BBB-CCC with your Marketo instance
marketo_instance = "<https://AAA-BBB-CCC.mktorest.com>"
endpoint = "/rest/v1/activities.json"
# Replace with your access token
auth_token =  "?access_token=" + "ac756f7a-d54d-41ac-8c3c-f2d2a39ee325:ab"
# Specify datetime needed as nextPageToken
since_date_time = "&nextPageToken=GIYDAOBNGEYS2MBWKQYDAORQGA5DAMBOGAYDAKZQGAYDALBQ"
# Specify activities needed
activity_type_ids = "&activityTypeIds=24"
request_url = marketo_instance + endpoint + auth_token + since_date_time + activity_type_ids

# Make request
response = RestClient.get request_url

# Returns Marketo API response
puts response
```

1. L’API Get Lead Activities restituisce un token di paging con ogni risposta che è possibile utilizzare per impaginare attraverso il set di risultati.** Per ulteriori informazioni, consulta la [documentazione REST API](/help/rest-api/rest-api.md).

Pubblicato il _2015-01-20_ da _Murta_

## Evidenziazione di progetti Source aperti basati sulla piattaforma Marketo: parte due

Questo è il secondo post di una serie in corso che evidenzia progetti open-source creati intorno alla piattaforma Marketo dalla community di sviluppatori. Abbiamo [un elenco sull&#39;account GitHub di Marketo](https://github.com/Marketo/Community-Supported-Client-Libraries) in cui teniamo traccia delle librerie client e dei progetti creati dalla community di sviluppatori Marketo. Di seguito sono riportati tre progetti sviluppati intorno alle API Marketo SOAP e Munchkin. [PunchTab](https://www.punchtab.com/) ha creato [una libreria client in Python per l&#39;API SOAP di Marketo](https://github.com/PunchTab/suds-marketo). [Flickerbox](https://www.flickerbox.com/) ha creato [una libreria client in PHP per l&#39;API SOAP di Marketo](https://github.com/flickerbox/marketo).* [Richard Morrison](https://x.com/mozz100) ha creato [uno script PHP per ottenere i dati dei lead dall&#39;API SOAP di Marketo e quindi trasmetterli al client tramite JavaScript.](https://github.com/mozz100/marketo-whodat) Questo progetto può aiutarti a modificare una pagina basata sui dati di un utente in Marketo.  Siamo entusiasti di vedere altri progetti creati dalla community di sviluppatori sulla piattaforma Marketo. Se stai lavorando a un progetto open source per la piattaforma Marketo, [invialo a questo archivio GitHub tramite una richiesta pull](https://github.com/Marketo/Community-Supported-Client-Libraries).

Pubblicato il _2015-01-20_ da _Murta_

## Invia clic del motore di consigli RTP a Google Analytics

Questa è una soluzione che consente agli utenti di Marketo Real-Time Personalization (RTP) di visualizzare i clic dal motore di consigli dei contenuti in Google Analytics. Quando un visitatore fa clic sulla barra dei consigli del contenuto, viene inviato un evento a Google Analytics nella categoria &quot;Consigli RTP&quot;. In Analytics, il Testo consigliato (visualizzato nella barra) verrà aggiunto all’Etichetta evento e l’URL della risorsa consigliata verrà aggiunto all’Azione evento. Lo script funziona sia per Classic Google Analytics che per Google Universal Analytics. Questo tag deve essere incollato alla fine del codice della pagina HTML, quindi è l&#39;ultimo tag prima del tag `</body>`.

```javascript
$( document ).ready(function() {
if(document.getElementsByClassName("insightera-bar-content").length
 >0){
document.getElementsByClassName("insightera-bar-content")[0].getElementsByTagName('a')[0].addEventListener("click",
 function(){
assetName
 = document.getElementsByClassName("insightera-bar-content")[0].getElementsByTagName('a')[0].innerText;
assetURL
 = document.getElementsByClassName("insightera-bar-content")[0].getElementsByTagName('a')[0].href;
assetURL=
 assetURL.substring(assetURL.lastIndexOf("/"),assetURL.indexOf("?iesrc"));
console.log(assetName

 * " | " + assetURL);
if(typeof(_gaq)
 != "undefined"){
//Classic
_trackEvent("RTP-Recommendations",
 assetName , assetURL , {'nonInteraction': 1});
}else
 if(typeof(ga) !="undefined"){
//Universal
ga('send',
 'event',"RTP-Recommendations", assetName , assetURL, {'nonInteraction': 1});
}
});
}
});
```

Pubblicato il _2015-01-22_ da _Yanir_

## Utilizzo dell’API REST di Marketo con Boomi: recupero e archiviazione di un token di autenticazione REST

Impostare un’esportazione automatica di lead che soddisfano determinati criteri è un caso d’uso molto comune con Marketo. Anche se al momento questo non può essere fatto nell’interfaccia di Marketo, è piuttosto semplice da eseguire utilizzando lo strumento di terze parti come Dell Boomi, un elenco statico con alcune campagne di gestione dati e l’API REST di Marketo. L&#39;API REST? Pensavo che Boomi non avesse un connettore API REST di Marketo. Al momento non è così, ma è possibile eseguire la stessa operazione utilizzando il connettore HTTP e definendo manualmente le forme di risposta jSON. Il primo passaggio consiste nel configurare l&#39;istanza di Marketo per l&#39;utilizzo dell&#39;API REST, come descritto nella [pagina per sviluppatori Marketo per API REST](/help/rest-api/rest-api.md). Suppongo inoltre che abbiate accesso a un account Dell Boomi e possiate le competenze necessarie per creare questi tipi di processi di integrazione. Il processo finale si presenta come segue e includerà le chiamate alle seguenti operazioni API REST di Marketo, ognuna delle quali ha una forma di risposta jSON associata che si trova sul sito per sviluppatori. Per risparmiare tempo, li ho elencati di seguito Esempio JSON per [Autenticazione](/help/rest-api/authentication.md)

```json
{
  "access_token": "",
  "token_type": "",
  "expires_in": 0,
  "scope": ""
}
```

Esempio di JSON per [ottenere più lead per ID elenco](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists)

```json
{
  "requestId": "",
  "success": true,
  "nextPageToken": "",
  "result": [
    {
      "id": 0,
      "email": "",
      "firstName": "",
      "lastName": ""
    },
    {
      "id": 0,
      "email": "",
      "firstName": "",
      "lastName": ""
    },
    {
      "id": 0,
      "email": "",
      "firstName": "",
      "lastName": ""
    }
  ]
}
```

Esempio di JSON per [rimuovere lead dall&#39;elenco](https://developer.adobe.com/marketo-apis/api/mapi/#operation/removeLeadsFromListUsingDELETE)

```json
{
  "requestId": "",
  "success": true,
  "result": [
    {
      "id": 1,
      "status": ""
    },
    {
      "id": 2,
      "status": "",
      "reasons": [
        {
          "code": "",
          "message": ""
        }
      ]
    }
  ]
}
```

Definisci proprietà: prima di iniziare a chiamare REST, è importante esternalizzare e incapsulare le variabili che stai utilizzando. Ho definito quanto segue.

* ClientID: ottieni questo dato dal servizio REST Launchpoint
* Segreto client: ottieni questo dato dal servizio REST Launchpoint
* AccessToken: questo risultato è stato ottenuto da una chiamata REST
* Static ListID: l’ID ELENCO dell’elenco statico sul quale opereremo. Ottieni questo dall’URL in Marketo
* Campi: un elenco separato da virgole di campi che il servizio rimanente ottiene da Marketo per ogni lead. Il mio è &quot;id, email,firstName,lastName&quot; * IDStringToDelete: conterrà l’ID di tutti i lead nell’elenco statico da utilizzare per la loro rimozione dall’elenco
* Tipi di attività: verranno utilizzati nella Parte 2 di questo blog, dove approfondisco questo argomento!
* SinceDateTime: verrà usato nella parte 2 di questo blog, dove approfondisco questo argomento!
* PagingToken: verrà usato nella Parte 2 di questo blog, dove approfondisco questo argomento!
* Cartella - In uscita: percorso della cartella in uscita sul server SFTP. In questo esempio utilizzo &quot;/data/outgoing&quot;. Ci consente di parametrizzare l’operazione SFTP per renderla generica.

Token di autenticazione: come ho detto, posizioneremo un connettore sull’area di lavoro dopo aver creato il processo con una forma di avvio &quot;Nessun dato&quot; (questa è solo una scelta personale, mi piacciono tutti i miei connettori che sembrano tappi britannici).
Il connettore deve essere configurato come segue: - Il connettore è un client HTTP GET - La connessione utilizza l’URL: `https://123-ABC-456.mktorest.com` (nota n. /rest alla fine in modo che sia possibile utilizzarlo per le chiamate REST e per ottenere il token di accesso alle identità. e modifica 123-ABC-456 a quella giusta per l’istanza di Marketo) - L’operazione è &quot;Get oAuth Token&quot; (new!) - Profilo richiesta = Nessuno - Profilo risposta = JSON - Nuovo profilo denominato &quot;Authentication Token Response&quot; - Tipo di contenuto: semplice - Metodo HTTP: GET - Percorso risorsa (aggiungere 4 senza virgolette): &quot;identity/oAuth/token?grant_type=client_credentials&amp;client_id=&quot;; &quot;ClientID (variabile di sostituzione)&quot;; &quot;&amp;client_secret=&quot;; &quot;ClientSecret (variabile di sostituzione)&quot; - Impostare i parametri in Configure —> Parameters —>(+): Set ClientID = Process Property Client ID; Set ClientSecret = Process Property Client Secret Dopo questa operazione, memorizzare il token di successo nella variabile &quot;AccessToken&quot; delle proprietà del processo come mostrato, estraendolo dalla risposta jSON.
Il modello per questo passaggio verrà ripetuto per i passaggi successivi, ma utilizzando nuove operazioni con profili di ritorno jSON diversi. Infatti, molte chiamate REST saranno gestite nello stesso modo con modifiche minori! Nella prossima puntata, approfondiremo questo argomento e otterremo un elenco di lead da un elenco statico utilizzando REST! Per il momento, esegui il processo, ma inserisci una forma di arresto dopo &quot;Imposta proprietà&quot; ed esegui debug per assicurarti di visualizzare lo stesso token visualizzato in Marketo. Dovrebbero combaciare perfettamente!

Pubblicato il _2015-01-26_ da _John_

## Utilizzare un’API Font di Google per aggiungere un font personalizzato a una pagina di destinazione di Marketo

**Nota: questo è un post di [Murtza Manzur](https://www.linkedin.com/in/murtzam). Murtza è un Marketo Developer Evangelist con sede nella baia di San Francisco.**
Supponiamo che tu stia creando una pagina di destinazione in Marketo e desideri utilizzare un font personalizzato. Questo è possibile utilizzando l’API Font di Google.  Aggiungi un metodo di importazione al file CSS con riferimento a Google Fonts:

`@import url(http://fonts.googleapis.com/css?family=Open+Sans:400,300,600);`

Pubblicato il _2015-01-26_ da _Murta_

## Usa lo script e-mail per rendere maiuscolo il nome di un lead

Supponiamo che un lead entri nel suo nome in minuscolo, come ad esempio &quot;John doe&quot;. Quando si invia una campagna e-mail, tuttavia, si desidera utilizzare l&#39;iniziale del nome del lead, ad esempio John Doe. È possibile rendere maiuscolo il nome di un lead utilizzando gli script e-mail. Ecco come farlo.

1. Nel programma di posta elettronica, fai clic sulla scheda &quot;I miei token&quot;.
1. Crea un nuovo token di script e-mail trascinando &quot;Script e-mail&quot; dal pannello di destra al pannello centrale. Denomina il token.
1. Nella casella di testo Modifica token script, incolla il codice seguente. Nel pannello a destra, in Oggetto lead, selezionare la casella di controllo Nome. Quindi fai clic su Salva.

```javascript
# set($name = ${lead.FirstName})
# set($formattedFirstName = $name.substring(0).toUpperCase())
$formattedFirstName
```

1. Fai riferimento al token nella risorsa e-mail. Viene restituito il nome del lead con la prima lettera maiuscola. Per ulteriori informazioni sugli script di posta elettronica, visitare la [documentazione sugli script di posta elettronica](/help/email-scripting.md).

Pubblicato il _2015-01-26_ da _Murta_

## Ottieni tutti i lead dall’API REST di Marketo

[domanda su StackOverflow che chiede come ottenere un elenco di tutti i lead da Marketo tramite l&#39;API REST](https://stackoverflow.com/questions/28184900/how-do-i-get-the-list-of-all-the-leads-in-marketo). È possibile eseguire query su questi dati utilizzando [Ottieni più lead per tipo di filtro endpoint API REST](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET). Ai lead in Marketo vengono assegnati ID lead in ordine sequenziale a partire da 1. Utilizzando l&#39;endpoint REST API [ di tipo ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET)Ottieni più lead per filtro, è possibile eseguire query su 300 lead per ID lead per ogni chiamata. È necessario specificare id come filterType e gli ID lead come filterValues per ogni chiamata a questo endpoint. Per ottenere tutti i lead, è necessario scorrere il numero totale di lead 300 alla volta. Y
Puoi ottenere il conteggio totale dei lead in un’istanza Marketo tramite l’interfaccia utente di Marketo. Nell’interfaccia utente di Marketo, vai alla scheda Database lead, fai clic su Elenchi smart di sistema, poi su Elenco smart lead e infine sulla scheda Lead. Quindi fai clic sulla colonna ID e ordina in ordine decrescente. Una volta ordinati i lead, l&#39;ID del primo lead sarà il limite superiore per l&#39;ID lead quando si esegue una query su tutti i lead. Se non hai accesso all&#39;interfaccia utente di Marketo per ottenere il conteggio totale dei lead, esiste un approccio [alternativo per ottenere questo valore utilizzando l&#39;API REST Get Lead Activities](https://stackoverflow.com/questions/28419967/get-all-leads-programmatically-in-marketo-v1).

1. Prima chiamata API: sostituisci ... con tutti i valori compresi tra:

`/rest/v1/leads.json?filterType=Id&filterValues=1,2,3,...,298,299,300`

Di seguito è riportato un codice di esempio in Ruby per la prima chiamata.

```java
require 'rest_client'
require 'json'

# Build request URL
# Replace AAA-BBB-CCC with your Marketo instance
marketo_instance = "<https://AAA-BBB-CCC.mktorest.com>"
endpoint = "/rest/v1/leads.json"
# Replace with your access token
auth_token =  "?access_token=" + "ac756f7a-d54d-41ac-8c3c-f2d2a39ee325:ab"
# Replace with filter type and values
ids_needed = (1..300).to_a.join(",")
filter_type_and_values = "&filterType=Id&filterValues=" + ids_needed
request_url = marketo_instance + endpoint + auth_token + filter_type_and_values

# Make request
response = RestClient.get request_url

# Returns Marketo API response
puts response
```

1. La seconda chiamata API e ogni chiamata API successiva seguirebbero lo stesso pattern fino al raggiungimento del conteggio totale di lead:

```
//replace ... with all the values in between
/rest/v1/leads.json?filterType=Id&filterValues=301,302,303,...,598,599,600
```

Per ulteriori informazioni, consulta la [documentazione REST API](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET).

Pubblicato il _2015-01-28_ da _Murta_

## Esegui azioni di invio modulo da Iframe a pagina padre

Abbiamo visto alcuni casi in cui gli utenti utilizzano i moduli iframe e desiderano indirizzare i visitatori che hanno compilato il modulo a una pagina di ringraziamento o a PDF, video, ecc. Il problema è che, poiché il modulo è incorporato in una pagina di destinazione diversa da quella padre, l’azione viene eseguita solo nella pagina interna in cui si trova il modulo. Per risolvere questo problema, di seguito sono riportati 2 tag JavaScript creati. Inserisci come elemento HTML nelle pagine iframe o direttamente nel modello della pagina di destinazione utilizzato per gli iframe. Posizionalo prima dell&#39;ultimo tag `</body>`. Il primo tag esegue l’azione sulla pagina padre e il secondo tag la apre in una nuova scheda.

**Azione modulo su una pagina padre**

```javascript
<script>
MktoForms2.whenReady(function (form){
form.onSuccess(function (values, url){
window.parent.location.assign(url);
return false;
           });
});
</script>
```

**Azione modulo in una nuova scheda**

```javascript
<script>
MktoForms2.whenReady(function (form){
var newWin;
form.onSubmit(function (){
newWin = window.open('about:blank', 'myWindow');
});
form.onSuccess(function (values, url){
newWin.location.replace(url);
return
 false;
});
});
</script>
```

Pubblicato il _2015-02-02_ da _Yanir_

## Inviare dati di visualizzazione da un video YouTube al mercato

Supponiamo che tu voglia segmentare i lead in Marketo in base al fatto che abbiano iniziato o terminato un video specifico. È possibile farlo utilizzando Munchkin, l’API Iframe di YouTube ed elenchi avanzati in Marketo. Il codice di esempio riportato in questo post ti consente di inviare video avviati e video completati agli eventi in Marketo tramite Munchkin. Affinché questo funzioni, è necessario caricare Munchkin anche sulla pagina prima di poter iniziare a inviare eventi di visualizzazione video in Marketo. Il video avviato e completato verrà visualizzato nel registro attività del lead. Una volta che i dati sono in Marketo, puoi creare un elenco avanzato e segmentare i lead che hanno iniziato o terminato un video.

1. Ottieni l’ID del video YouTube da incorporare.** Dall&#39;URL del video di YouTube che desideri utilizzare, annota l&#39;ID, che è la serie di caratteri casuali dopo `v=`.
1. Posiziona l’ID video di YouTube dal passaggio 1 nell’ottava riga di questo esempio di codice. Quindi inserisci il codice prima di `</body>` nel HTML della pagina.

```javascript
<div id="player"></div>
<script>
var tag = document.createElement('script');
tag.src = "https://www.youtube.com/iframe_api";
document.getElementsByTagName('head')[0].appendChild(tag);

//Change 'iiqxcjxJ5Us' to video needed
var player, videoId = 'iiqxcjxJ5Us';
function onYouTubeIframeAPIReady() {
player = new YT.Player('player', {
height: '390',
width: '640',
videoId: videoId,
events: {
'onStateChange': onPlayerStateChange
}
});
}

function onPlayerStateChange(event) {
switch( event.data ) {
//Send video started event to Marketo
case YT.PlayerState.PLAYING: Munchkin.munchkinFunction('visitWebPage', {
url: '/video/'+videoId
, params: 'video=started'
}
);
break;
//Send video finished event to Marketo
case YT.PlayerState.ENDED: Munchkin.munchkinFunction('visitWebPage', {
url: '/video/'+videoId
, params: 'video=finished'
}
);
break;
}

}
</script>
```

1. Crea un elenco avanzato in Marketo con l’URL del video e l’evento di visualizzazione che stai cercando come valore di &quot;Querystring contiene&quot;. Per ulteriori informazioni sull&#39;API Iframe di YouTube, [consulta la documentazione API di YouTube](https://developers.google.com/youtube/iframe_api_reference). Per ulteriori informazioni su Munchkin, [consulta la documentazione per gli sviluppatori di Marketo](/help/javascript-api/lead-tracking.md).

Pubblicato il _2015-02-02_ da _Murta_

## Suggerimenti e trucchi per l’API di Marketo SOAP

NOTA: questo è un post di un blog ospite. [Ed Blachman è Senior Architect](https://www.linkedin.com/uas/login?session_redirect=https%3A%2F%2Fwww.linkedin.com%2Fprofile%2Fview%3Fid%3D2777965) presso [TIBCO Software, un noto fornitore di software aziendale](https://exchange.adobe.com/apps/browse/ec?product=MRKTO). Ed sta lavorando a prodotti che permettono a quelli che Gartner chiama &quot;sviluppatori cittadini&quot; di integrare i servizi cloud che utilizzano senza dover fare alcuna programmazione da soli. [L&#39;API SOAP di Marketo](/help/soap-api/soap-api.md) è uno strumento potente con cui gli sviluppatori possono sfruttare la potenza di Marketo e integrarla con le nostre applicazioni. Tra [la documentazione formale](./getting-started.md) e [le risorse della community](https://nation.marketo.com/), sono disponibili molte informazioni su come utilizzarle. Quando ho iniziato, ho attinto molto a quelle informazioni e le ho trovate inestimabili. Tuttavia, in questo processo, ho messo su alcuni suggerimenti che non avevo visto in nessun luogo. Ecco un po&#39; di quello che ho capito.

**Sandbox per sviluppatori** La Sandbox è ovviamente una risorsa straordinaria per gli sviluppatori API: un luogo sicuro in cui sperimentare le funzionalità di Marketo, aggiungere e rimuovere oggetti senza interferire con le attività di marketing reali svolte dagli utenti effettivi di Marketo della tua organizzazione. Tuttavia, la Sandbox non è una panacea.
Ad esempio, dovevo condividere la nostra Sandbox con un altro gruppo di sviluppo, e ci è voluto un po’ di tempo perché si erano abituati all’idea di essere proprietari della Sandbox. Alla fine abbiamo scoperto un paio di best practice per la condivisione: - Non scrivere test che dipendono dalla conoscenza completa del contenuto della sandbox. In quanto risorsa condivisa, gli schemi possono essere soggetti a modifiche senza preavviso, nonché a intere voci nel database dei lead, nei programmi o in altre entità. Se i test presuppongono una conoscenza completa della Sandbox, il ciclo di sviluppo crea periodi di sospensione attività per i gruppi con cui lo condividi. Poiché in genere il loro ciclo di sviluppo non coincide con il tuo, ciò equivale ad hogging della risorsa-not cool. E non è necessario, se ci pensate. : utilizza una convenzione per etichettare tutte le tue cose: i lead, i campi dello schema del lead, i programmi, ecc. Se ognuno di voi è in grado di identificare i propri oggetti e se concordate con i vostri co-tenant che ognuno di voi lascerà gli oggetti degli altri, dovreste essere su una solida base per la condivisione. Per i lead, puoi creare un campo personalizzato e una convenzione utilizzando questo campo personalizzato per identificare questi lead come lead di test. Per gli elenchi o i programmi, è possibile iniziare i nomi degli oggetti con una stringa che identifichi tali oggetti come appartenenti all&#39;utente. - Valuta la possibilità di scrivere test che ripuliranno i propri contenuti, che creeranno innanzitutto gli oggetti desiderati, quindi li aggiorneranno o li elimineranno in modo selettivo, infine li rimuoveranno. Tieni presente che non è possibile ottenere questo risultato al 100% nell’API di SOAP, perché non tutto ciò che si trova nella Sandbox o, per inciso, in un’istanza reale può essere gestito tramite l’API di SOAP. Anche in questo caso, vale comunque la pena fare il più possibile.)

**Istanze reali** Il problema con la Sandbox è che non viene utilizzata in produzione, quindi è difficile capire a cosa assomiglia l&#39;utilizzo reale in un&#39;istanza Marketo. Ora, se sei abbastanza fortunato da avere un utente avanzato di Marketo nel tuo team, o se stai facendo sviluppo su misura per utenti interni di Marketo, non è un tale problema. Ma nel caso della mia squadra, si è trattato di un grosso affare. Nessuno di noi era esperto di Marketo, e dato che ci è stato chiesto di capire un gran numero di servizi cloud, semplicemente non avevamo il personale per diventare esperti in nulla. Ecco alcune informazioni acquisite dall’accesso a un’istanza reale: - Schemi lead di grandi dimensioni. Lo schema lead nell’istanza di produzione a cui abbiamo effettuato l’accesso contiene oltre 200 campi. Questo ha reso chiaro ai nostri designer dell’interfaccia utente che l’interfaccia utente che stavano progettando doveva contenere schemi di quelle dimensioni (o più grandi). - Utilizzo bursty. Abbiamo notato due ordini di grandezza di differenza tra i tempi di utilizzo più elevati e quelli di utilizzo più ridotto (in termini di numero di lead creati o aggiornati). Questo ha interessato sia il volume di dati che avremmo ottenuto dalle chiamate API (ovvio) e il tempo necessario affinché una chiamata API risponda (probabilmente meno ovvio).

**Tempo di risposta della chiamata API** A seconda dell&#39;ora del giorno, dei dettagli della chiamata API e del contenuto dell&#39;istanza, è possibile che il tempo di risposta dell&#39;API SOAP richieda più tempo della media. A volte, le chiamate API impiegavano un minuto e mezzo per rispondere. Devi essere consapevole della possibilità di affrontarlo: - Test. Forse questo non è un problema per il tuo utilizzo. Ma non pensate solo a questo, fate qualche test. - Modifica l’utilizzo. Nel nostro caso, il problema più grande è stato quello di impostare la dimensione della pagina per le chiamate a [getMultipleLeads](/help/soap-api/getmultipleleads.md) in modo che sia la dimensione consentita dall&#39;API. Nel nostro contesto ha un certo senso perché il nostro obiettivo è di essere il più efficiente possibile con la quota API del nostro cliente. Ma nel tuo contesto, potresti non doverti preoccupare così intensamente delle quote di chiamate API degli utenti, nel qual caso otterrai sicuramente un tempo di risposta migliore chiedendo pagine di dati più piccole.

**Partizionamento lead** Marketo fornisce potenti strumenti, partizioni e aree di lavoro, che consentono a più gruppi di marketing di condividere una singola istanza Marketo. Tuttavia, questi strumenti non si riflettono direttamente nell’API di SOAP. Ad esempio, quando si utilizza getMultipleLeads per ottenere tutti i lead che sono stati aggiornati o creati da una data o un&#39;ora, vengono restituiti tutti i lead nell&#39;istanza per la quale è il caso, senza considerare (e con nulla da indicare) quale partizione o area di lavoro contiene un determinato lead. La creazione e l’aggiunta di lead agli elenchi sono altri contesti in cui il partizionamento dei lead può influire sulle operazioni effettive delle chiamate API. Tieni presente che le partizioni e le aree di lavoro potrebbero non essere la soluzione necessaria al problema della condivisione delle sandbox descritto in precedenza. Quindi, come si fa a capire se questo è un problema per voi? Ho trovato tutte queste cose utili: gli Evangelisti Sviluppatori si impegnano per il nostro successo nell&#39;uso delle API, e dove ci sono domande, sono straordinariamente bravi a lavorare per trovare risposte. - [Documentazione API](./getting-started.md). Gli evangelisti hanno già inserito questo tema in parte della documentazione, e come parte del loro impegno per il nostro successo, sono davvero bravi ad aggiornare il documento. - I Suoi Casi Di Prova. Anche se l’utilizzo di partizioni e aree di lavoro per la condivisione della sandbox potrebbe non essere una buona idea, la sandbox è un ottimo punto di gioco per le partizioni e le aree di lavoro e per capire se rappresentano una sfida per l’utilizzo previsto. (è anche un buon modo per restringere le vostre domande agli evangelisti, che sono sempre una buona idea.)

**TIMTOWTDI e test** &quot;Esiste più di un modo per farlo&quot; - il motto di programmazione Perl si applica effettivamente in alcuni contesti all&#39;API SOAP di Marketo. Ad esempio, volevo combinare l’aggiornamento di un set di lead con l’aggiunta di tali lead a un certo elenco. L’API di SOAP offre due modi per farlo: 1. [importToList](/help/soap-api/importtolist.md) + [getImportToListStatus](/help/soap-api/getimporttoliststatus.md). Leggendo la documentazione, questo è ovviamente il modo &quot;normale&quot; per farlo. Tuttavia, il fatto che si debba eseguire il polling per lo stato dell&#39;operazione di importazione ha sollevato un flag giallo per me. È stato questo il modo in cui volevo implementare la mia importazione? 1. [syncMultipleLeads](/help/soap-api/syncmultipleleads.md) + [listOperation](/help/soap-api/listoperation.md). Questo sembra molto meno elegante di una chiamata importToList unitaria, ma non si basa sul polling. Era un&#39;opzione praticabile? Casi come questi sono difficili da affrontare per gli Evangelisti, perché dipendono davvero dalla natura delle istanze che si stanno affrontando ed esattamente da ciò che si sta cercando di fare. Fortunatamente, se hai impostato un ambiente di testing di unità affidabile, dovresti essere in grado di utilizzarlo anche per esplorare domande come queste. In questo caso particolare, si è scoperto che l&#39;opzione 2 era migliore per il mio caso d&#39;uso rispetto all&#39;opzione 1-non a causa del polling, ma piuttosto perché ho incontrato limitazioni orientate ai campi su importToList, e anche perché stavo cercando di scrivere codice che poteva essere utilizzato in contesti e istanze su cui non avevo controllo. Ma il tuo caso d’uso potrebbe essere diverso e il test è l’unico modo per scoprirlo.

**Conclusione** Non credo che nessuno di questi sia un segreto enorme. D&#39;altra parte, sarei stato in vantaggio se avessi saputo tutto questo prima di iniziare. Spero che lo trovi utile.

Pubblicato il _2015-02-05_ da _David_

## Utilizzo dell’API REST di Marketo con Boomi: recupero ed eliminazione di lead da un elenco statico

Nella parte 1 di questa serie, ho discusso come era possibile iniziare a utilizzare l’API REST tramite Boomi con il connettore HTTP Boomi, in particolare ottenere il token di autenticazione necessario per accedere all’API REST e memorizzarlo in una variabile di processo. Successivamente, inizieremo a effettuare chiamate in Marketo e in questa puntata ti mostrerò come [ottenere più lead per ID elenco](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists) e [rimuovere i lead dall&#39;elenco](/help/rest-api/lead-database.md). Presta particolare attenzione alla rimozione dei lead da una lista perché c&#39;è un aspetto molto &quot;leggermente documentato&quot; e sottile di Boomi al lavoro che approfondisco quando ci arriviamo.

Nel prossimo capitolo approfondiamo questa funzionalità per iniziare a fare cose interessanti come ottenere l&#39;attività Lead, ma questo è un blog per un altro giorno. Per questa puntata parleremo della seconda e della terza area in evidenza. Come recensione, ho incluso le risposte JSON che abbiamo bisogno di seguito. Ricorda che per creare un profilo JSON in Boomi, tutto ciò che devi fare è creare un componente di profilo di tipo JSON, quindi fai clic su &quot;importa&quot; e seleziona il file. Boomi fa il resto, estrapolando cose come se dovessero esserci più ID consentiti. Esempio di JSON per [ottenere più lead per ID elenco](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists)

```json
{
  "requestId": "",
  "success": true,
  "nextPageToken": "",
  "result": [
    {
      "id": 0,
      "email": "",
      "firstName": "",
      "lastName": ""
    },
    {
      "id": 0,
      "email": "",
      "firstName": "",
      "lastName": ""
    },
    {
      "id": 0,
      "email": "",
      "firstName": "",
      "lastName": ""
    }
  ]
}
```

Esempio di JSON per [rimuovere lead dalla richiesta elenco](https://developer.adobe.com/marketo-apis/api/mapi/#operation/removeLeadsFromListUsingDELETE)

```json
{
   "input":[
      {
         "id": ""
      },
      {
         "id": ""
      }
   ]
}
```

Esempio di JSON per [Rimuovi lead dalla risposta elenco](https://developer.adobe.com/marketo-apis/api/mapi/#operation/removeLeadsFromListUsingDELETE)

```json
{
  "requestId": "",
  "success": true,
  "result": [
    {
      "id": 1,
      "status": ""
    },
    {
      "id": 2,
      "status": "",
      "reasons": [
        {
          "code": "",
          "message": ""
        }
      ]
    }
  ]
}
```

La funzione Ottieni più lead per ID elenco** rilascia un altro connettore (Get) nel processo, utilizzando la stessa connessione definita nell’articolo precedente. Crea una nuova operazione chiamata &quot;Ottieni più lead per ID elenco&quot; (per coerenza sono un adesivo) I suoi attributi sono i seguenti: - Profilo richiesta: Nessuno (questo utilizza l’URL della richiesta) - Tipo di profilo risposta: jSON - Profilo risposta: Crea un nuovo profilo basato sulla precedente risposta Ottieni più lead per ID elenco. Si noti che è possibile modificarlo in modo che restituisca i campi desiderati, non solo quelli elencati. È importante ricordare che il profilo di risposta JSON deve corrispondere esattamente all’elenco di campi che si richiedono dall’API REST e si devono richiedere solo i campi necessari. Nell&#39;oggetto Process Properties è stata definita una proprietà denominata &quot;fields&quot;, che è un elenco separato da virgole dei campi che si desidera vengano restituiti da REST. e questo è l&#39;elenco che deve corrispondere al profilo. Tipo di contenuto: text/plain (si tratta solo di una richiesta URL) Metodo HTTP: GET (cerchi questo nei documenti REST API, è sempre elencato) Percorso risorsa (aggiungi 5) rest/v1/list/ listID (variabile di sostituzione) /leads.json?access_token= access_token (variabile di sostituzione) &amp;fields= fields (variabile di sostituzione). Quindi, nella scheda dei parametri sul connettore, puoi immettere i valori delle variabili, che abbiamo precedentemente popolato nelle proprietà del processo. Nella sezione successiva parlerò di come evitare di compilarli manualmente. Salterò la parte del processo in cui mappo la risposta per Ottieni più lead per ID elenco in un profilo di file flat e la incollerò su un server FTP perché si tratta di una funzionalità semplice di Boomi.

Eliminare i lead da un elenco Quindi questo è interessante, uno dei miei colleghi, [Ken Niwa](https://www.linkedin.com/uas/login?session_redirect=https%3A%2F%2Fwww.linkedin.com%2Fprofile%2Fview%3Fid%3D7429494) mi ha insegnato questa tecnica successiva ed è piuttosto interessante e si basa su un articolo di Boomi intitolato &quot;Come creare una richiesta POST per un&#39;applicazione RESTful&quot;, mostrato di seguito.  ...ma prima le cose importanti. Nel processo, che deriva dalla forma &quot;Ottieni più lead per ID elenco&quot;, abbiamo la forma Ottieni più lead per risposta ID elenco, e dobbiamo mapparla nella &quot;Rimuovi lead dalla richiesta elenco&quot; che la mappatura è abbastanza semplice, semplicemente mappando l’ID che abbiamo ottenuto dai lead nell’elenco originale all’elenco ID che stiamo passando al jSON di eliminazione. Quindi, rilascia un altro connettore con l’azione &quot;Invia&quot;, utilizzando la stessa Connessione Crea una nuova operazione denominata &quot;Rimuovi lead dalla richiesta elenco&quot;. i cui attributi sono Profilo richiesta: jSON Tipo di contenuto: application/json Profilo richiesta: [Profilo JSON] Rimuovi lead dalla richiesta elenco (creata dal file indicato sopra) Tipo di profilo risposta: jSON Profilo risposta: [Profilo JSON] Rimuovi lead dalla risposta elenco (creata dal file indicato sopra) Tipo di contenuto: application/json Metodo HTTP: Percorso risorsa DELETE (add4) rest/v1/LISTS/ listID (variabile di sostituzione) /leads.json?access_token= access_token (variabile di sostituzione) Ecco l&#39;aspetto interessante di questo connettore. NON aggiungeremo esplicitamente i parametri nella scheda del connettore. Al contrario, come si legge nell’articolo, vengono create proprietà di documento dinamiche con gli stessi nomi delle variabili di sostituzione. In questo caso, le variabili listID e access_token. In questo caso, la forma jSON scorre nella chiamata REST e i parametri vengono visualizzati nella posizione corretta sull’URL. Non è possibile eseguire questa operazione con la chiamata precedente perché si tratta di un GET e non di un POST. Quindi, a questo punto hai visto una chiamata API GET e POST REST e puoi iniziare a vedere il pattern per effettuare queste chiamate REST. Nella prossima sezione inizieremo a esaminare l’esportazione delle attività lead tramite l’API REST, che è un po’ più complessa.

Pubblicato il _2015-02-06_ da _John_

## Incorporare un video YouTube con tracciamento dei lead in una pagina di destinazione Marketo

In un post precedente sul blog, ho descritto come segmentare i lead in Marketo in base al fatto che abbiano iniziato o terminato un video YouTube specifico. In questo post di blog, esaminiamo come estrarre l’implementazione da quel post e utilizzarla in una pagina di destinazione di Marketo.

1. Passa al Programma in Marketo, dove desideri creare la nuova pagina di destinazione. Fai clic su Nuova risorsa locale, quindi fai clic su Pagina di destinazione.
1. Assegna un nome alla pagina di destinazione. Assegna un URL di pagina. Seleziona un modello. Quindi fai clic su Crea.
1. Dopo aver creato la pagina di destinazione, fai clic su Modifica bozza.
1. Dal pannello di destra, trascina il pulsante HTML nell’area di lavoro principale a sinistra.
1. Nella casella Editor HTML personalizzato visualizzata. Quindi fai clic su Salva.
1. Regola le dimensioni di un elemento HTML trascinando il contorno della casella. Quindi fai clic su Approva e chiudi.
1. Verifica la versione live della pagina di destinazione facendo clic su Visualizza pagina approvata. In una nuova finestra viene visualizzata una pagina di destinazione con YouTube. Il video iniziato e finito verrà visualizzato nel registro delle attività del lead come mostrato nella prima e nella seconda schermata di seguito. Una volta che i dati sono in Marketo, puoi creare un elenco avanzato e segmentare i lead che hanno iniziato o terminato un video, come mostrato nella schermata seguente. Per ulteriori informazioni sull&#39;API Iframe di YouTube, [consulta la documentazione API di YouTube](https://developers.google.com/youtube/iframe_api_reference). Per ulteriori informazioni su Munchkin, [consulta la documentazione per gli sviluppatori di Marketo](/help/javascript-api/lead-tracking.md).

Pubblicato il _2015-02-09_ da _Murta_

## Tracciamento web di applicazioni a pagina singola con Munchkin

Un’applicazione a pagina singola è un sito web che carica tutte le risorse necessarie per navigare nel sito al primo caricamento di pagina. Quando un utente fa clic su un collegamento, il contenuto viene caricato dai dati di caricamento della prima pagina. Per l’utente, il sito web si comporta come previsto, perché l’URL nella barra degli indirizzi è simile alla navigazione tradizionale della pagina. Munchkin funziona bene con i siti web tradizionali perché Munchkin viene eseguito ogni volta che gli utenti caricano una nuova pagina. Tuttavia, con un’applicazione a pagina singola se non carichi una nuova pagina, Munchkin verrà eseguito una sola volta. L&#39;approccio che seguo in questo post è di tenere traccia di quando un utente fa clic su un collegamento, e quindi inviare queste informazioni a Munchkin. Questo viene implementato utilizzando la funzione Munchkin `clickLink`. Di seguito è riportato un esempio di implementazione in jQuery che associa gli eventi di clic al metodo Munchkin `clickLink`. Quando si chiama il metodo Munchkin `clickLink`, viene passato il parametro per l&#39;URL su cui è stato fatto clic.

```javascript
<script>
$("a").on('click', function(event) {
    var urlThatWasClicked = $(this).attr('href');
    Munchkin.munchkinFunction('clickLink', { href: urlThatWasClicked});
});
</script>
```

Pubblicato il _2015-02-11_ da _Murta_

## Modificare il punteggio di un lead tramite l’API REST

Supponiamo che tu voglia modificare il punteggio di un lead in Marketo utilizzando le API. Questa operazione può essere eseguita con l’API REST utilizzando l’endpoint Create/Update Lead. Di seguito è riportato un esempio di codice in Ruby che mostra come effettuare questa chiamata.

```ruby
require 'rest_client'
require 'json'

# Build request URL
# Replace AAA-BBB-CCC with your Marketo instance
marketo_instance = "https://AAA-BBB-CCC.mktorest.com"
endpoint = "/rest/v1/leads.json"
# Replace with your access token
auth_token =  "?access_token=" + "ac756f7a-d54d-41ac-8c3c-f2d2a39ee325:ab"
request_url = marketo_instance + endpoint + auth_token

# Build request body
data = { "action" => "updateOnly", "input" => [ { "email" => "<example@email.com>", "leadScore" => "30" } ] }

# Make request
response = RestClient.post request_url, data.to_json, :content_type => :json, :accept => :json

# Returns Marketo API response
puts response
```

Nel corpo JSON della richiesta, specifichiamo `updateOnly` come azione. Ciò significa che la richiesta funzionerà solo se il lead esiste, altrimenti non riuscirà. Se si desidera creare un lead se non esiste, specificare `createOrUpdate` come azione. Utilizziamo l’e-mail del lead come identificatore principale per trovare il record del lead in Marketo. Infine, viene specificato il valore per il punteggio del lead utilizzando la chiave `leadScore`. Con questo metodo è possibile aggiornare 300 lead alla volta.

Pubblicato il _2015-02-19_ da _Murta_

## Evidenziazione di progetti Source aperti basati sulla piattaforma Marketo: terza parte

Questo è il terzo post di una serie in corso che evidenzia progetti open-source creati intorno alla piattaforma Marketo dalla community di sviluppatori. Abbiamo [un elenco sull&#39;account GitHub di Marketo](https://github.com/Marketo/Community-Supported-Client-Libraries) in cui teniamo traccia delle librerie client e dei progetti creati dalla community di sviluppatori Marketo. Di seguito sono riportati tre progetti sviluppati intorno alle API REST di Marketo. **[Usermind](http://www.usermind.com/) ha creato [una libreria client Node.js per l&#39;API REST di Marketo](https://github.com/MadKudu/node-marketo).** **[Arunim Samat](https://github.com/asamat) ha creato [una libreria client in Python per l&#39;API REST di Marketo](https://github.com/asamat/python_marketo).** **[Jacques Lemieux di Marketo](https://www.linkedin.com/in/jalemieux) ha creato [una libreria client in Ruby per l&#39;API REST di Marketo.](https://github.com/jalemieux/mkto_rest)** Siamo entusiasti di vedere altri progetti creati dalla community di sviluppatori sulla piattaforma Marketo. Se stai lavorando a un progetto open source per la piattaforma Marketo, [invialo a questo archivio GitHub tramite una richiesta pull](https://github.com/Marketo/Community-Supported-Client-Libraries).

Pubblicato il _2015-02-20_ da _Murta_

## Inserire un Marketo Form in una campagna RTP

Molti esperti di marketing sono interessati a inserire un Marketo Form in una campagna Marketo Real-Time Personalization (RTP). Che si tratti di una finestra di dialogo, di un tipo di campagna RTP zona o widget, puoi copiare il codice HTML del modulo e incollarlo nell’editor campagne RTP. Ho visto questi esempi di utilizzo: - Iscrizione dei visitatori alla newsletter dopo un secondo o un terzo clic sul sito - Modulo di iscrizione rapido ed efficace per i webinar - Download di un caso di successo - Offerta di lead che hanno annullato l’iscrizione in passato per iscriversi nuovamente Compila un modulo nella campagna e ricevi il ringraziamento o il contenuto richiesto, con conseguente clic in meno per raggiungere gli obiettivi. Ecco la spiegazione di come eseguire questa operazione e incorporare un Marketo Form 2.0 in una campagna RTP Marketo. Di seguito è riportato un ottimo esempio tratto da eMarketing: gli utenti RTP che lo hanno portato un passo avanti e, invece di indirizzare i visitatori a una pagina di ringraziamento, hanno deciso di mostrare un messaggio di ringraziamento all’interno della campagna RTP. Di seguito è riportato anche il codice per questa opzione. Godetevi e sono felice di sentire la vostra esperienza con esso!

1. Fai clic con il pulsante destro del mouse su un modulo approvato. Seleziona **Codice di incorporamento.**
1. Copia il **codice.**
1. In Marketo RTP, passa a **Campagne**.
1. Fai clic su **CREA NUOVA CAMPAGNA**.
1. Nell&#39;Editor Rich Text, fare clic sull&#39;icona **HTML**.
1. Incolla il codice da incorporare del modulo nell’editor di HTML Source. Fai clic su **Aggiorna**.
1. Il modulo non verrà visualizzato nella vista dell’editor, ma puoi visualizzarlo in anteprima per vedere come verrà riprodotto in una campagna.
1. Fai clic su **Avvia** per avviare la campagna.

### Nota

Eventuali modifiche apportate al modulo devono essere effettuate nell’ambito delle attività di marketing di Marketo in Modifica bozza del modulo.

### Articoli correlati

* [Forms 2.0](/help/javascript-api/forms-api-reference.md)

Pubblicato il _2015-12-20_ da _Yanir_

## Aggiungere un pulsante di ripristino a un Marketo Form

```javascript
<script src="//app-sj01.marketo.com/js/forms2/js/forms2.min.js"></script>
<form id="mktoForm_116"></form>
<script>MktoForms2.loadForm("//app-sj01.marketo.com", "410-XOR-673", 116,
function(form) { form.getFormElem()[0].querySelector('button[type="submit"]').insertAdjacentHTML('afterend','<button type="reset" class="mktoButton">Reset</button>') });
</script>
```

Pubblicato il _2015-03-18_ da _Murta_

## Aggiornamenti sulla versione di marzo 2015

L&#39;API [Marketo REST Asset è stata rilasciata nella versione di marzo 2015](https://developer.adobe.com/marketo-apis/api/asset/). Questa API consente di accedere agli oggetti file, cartella, token, e-mail e modello e-mail di Marketo. Sono state aggiunte due autorizzazioni di ruolo per fornire accesso agli endpoint API di Asset: Assets di sola lettura e Assets di lettura e scrittura. Se il tuo ruolo utente API è precedente al rilascio delle API di Asset, devi creare un nuovo ruolo utente API con queste autorizzazioni per abilitare l’accesso. In caso contrario, viene visualizzata una risposta di errore 603 &quot;Accesso negato&quot;. Oltre al rilascio dell’API REST Asset, erano presenti aggiornamenti agli endpoint REST API esistenti. L&#39;endpoint REST API [Merge Lead](https://developer.adobe.com/marketo-apis/api/mapi/#operation/mergeLeadsUsingPOST) è stato aggiornato per consentire l&#39;unione di più lead. L&#39;endpoint API REST di [Schedule Campaign](https://developer.adobe.com/marketo-apis/api/mapi/#operation/scheduleCampaignUsingPOST) è stato aggiornato per consentire la clonazione di una campagna durante la pianificazione di una campagna.

Pubblicato il _2015-03-23_ da _Murta_

## Attivazione di campagne RTP con un ritardo

Questo JavaScript personalizzato consente agli utenti RTP di mostrare le campagne pochi secondi dopo il caricamento della pagina web. Questa funzione è consigliata per le campagne di finestre di dialogo e widget. Può essere utilizzato per mostrare una campagna dopo un ritardo, una volta che il visitatore ha visualizzato il contenuto normale sulla pagina. Si consiglia di implementare questo codice solo su pagine specifiche in cui visualizzare le campagne. L’utilizzo di questo codice su tutte le pagine non è consigliato, in quanto potrebbe influire sulle prestazioni. **Istruzioni di installazione** Il codice personalizzato invia un evento dati personalizzato RTP (t=timeOnPage, ovvero t=60) e quindi carica la campagna che corrisponde a questo evento. Per impostazione predefinita, viene attivato dopo 60 secondi. È possibile personalizzarlo modificando il parametro sendCustomRTPEvent in qualsiasi altro numero. Inserisci il codice subito dopo il codice RTP standard:

```javascript
<script>
function sendCustomRTPEvent(a){
 var eventValue="t="+a;
 setTimeout(function(){
  rtp('send', 'event', {value: eventValue});
  rtp('get', 'campaign',true);
 }, 1000 \* a);
}
sendCustomRTPEvent(60); //Seconds
</script>
```

Per organizzare una campagna RTP che reagisca dopo un certo lasso di tempo: 1. Accedi al tuo account RTP 1. Crea un nuovo segmento 1. Nella sezione Eventi segmento aggiungi: `t=60`. Un visitatore può far corrispondere ogni segmento RTP solo una volta per sessione, visualizzando quindi ogni campagna una sola volta (a meno che non sia impostato su sticky).

Pubblicato il _2015-03-24_ da _Yanir_

## Aggiornamenti sulla versione di aprile 2015

### Marketo Mobile Engagement SDK v0.3.2

Marketo ora include l’automazione del marketing e il coinvolgimento degli utenti per le app mobili. L&#39;installazione di [Marketo Mobile SDK](/help/mobile/mobile.md) nell&#39;app iOS o Android consente agli addetti al marketing di rilevare gli eventi in-app e di inviare notifiche push pertinenti.

### Miglioramenti API REST

* Oggetti personalizzati

Sono stati introdotti nuovi [endpoint per oggetti personalizzati](/help/rest-api/custom-objects.md) che consentono di elencare, descrivere e sottoporre a CRUD a livello di programmazione i dati che risiedono con un oggetto personalizzato di Marketo.

Sono state aggiunte le autorizzazioni del ruolo per fornire accesso agli endpoint API per oggetti personalizzati: Oggetto personalizzato di sola lettura, Oggetto personalizzato di lettura-scrittura. Se il tuo ruolo utente API è precedente al rilascio delle API per oggetti personalizzati, devi creare un nuovo ruolo utente API con queste autorizzazioni per abilitare l’accesso. In caso contrario, viene visualizzata una risposta di errore 603 &quot;Accesso negato&quot;.

* Pianifica campagna - Clona programma

È stato introdotto un nuovo parametro opzionale &quot;cloneToProgramName&quot; nell&#39;API [pianificazione campagna](/help/rest-api/data-ingestion.md). Se questo parametro è presente, il programma principale della campagna verrà clonato e la campagna appena creata verrà pianificata. Il parametro specifica il nome desiderato per il programma risultante.

Pubblicato il _2015-04-28_ da _Travis Kaufman_

## Sincronizzazione degli annullamenti dell’iscrizione alle e-mail tra le istanze

Gestite più istanze di Marketo? Mantenere le informazioni sui lead sincronizzate tra le istanze può essere difficile. Di seguito è riportato un modo per sincronizzare gli annullamenti dell’iscrizione alle e-mail tra le istanze utilizzando un webhook che chiama un servizio web esterno. Il servizio web esterno esegue un ciclo in ogni istanza alla ricerca del lead noto che ha attivato l’evento di annullamento dell’abbonamento. Quando viene individuato un lead corrispondente, viene aggiornato il campo &quot;unsubscribed&quot; (annullato) nel record del lead corrispondente. Ecco un diagramma che illustra l&#39;idea.  L’implementazione del servizio web dipende da te, ma il codice riportato di seguito dovrebbe aiutarti ad avviare rapidamente il processo.

### Servizio Web esterno

Il servizio Web esterno esegue i seguenti passaggi per ogni istanza di Marketo che deve essere sincronizzata:

1. Compone l&#39;API REST [URL endpoint](/help/rest-api/endpoint-reference.md) specifico per l&#39;istanza
1. Ottiene il token di accesso utilizzando [Identità](/help/rest-api/authentication.md)
1. Ottiene l&#39;elenco dei record dei lead che corrispondono all&#39;indirizzo e-mail utilizzando [Ottieni più lead per tipo di filtro](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET)
1. Aggiorna il campo &quot;disiscrizione&quot; di ciascun record di lead tramite Crea/Aggiorna lead

Ecco un altro diagramma che mostra in dettaglio la chiamata al servizio web esterno e le chiamate API REST di Marketo.  Il codice di esempio seguente non è un servizio web fornito con il prodotto. Si tratta piuttosto di un programma in modalità console a cui è possibile trasmettere argomenti tramite la riga di comando. L’obiettivo è quello di mostrare come chiamare le API Marketo appropriate per aggiornare i record dei lead tra le istanze. L’implementazione del servizio web viene lasciata al lettore come un esercizio.
**Codice di esempio** Per rendere il codice di esempio operativo, è necessario creare un progetto Java nell&#39;IDE preferito. In seguito, dovrai apportare le seguenti modifiche: 1. Il codice di esempio utilizza [json-simple](https://code.google.com/archive/p/json-simple) per analizzare le stringhe JSON. Aggiungi il file jar json-simple al progetto Java. 1. Il codice di esempio ha una struttura che contiene i metadati per ogni istanza di Marketo. Posiziona i valori effettivi dalle istanze nella struttura come segue:

```java
public static String instanceInfo[][] = {
{ "AccountId1", "ClientId1","ClientSecret1" },    // Instance 1 metadata
{ "AccountId2", "ClientId2","ClientSecret2" },    // Instance 2 metadata
{ "AccountId3", "ClientId3","ClientSecret3" }     // Instance 3 metadata
};
```

Puoi trovare i metadati per l’istanza nel pannello di amministrazione di Marketo:

* ID account Amministratore > Integrazione > Munchkin > Account Munchkin
* Amministratore ID client e segreto client > Integrazione > LaunchPoint > Sincronizzazione annullamento iscrizione e-mail > Visualizza dettagli

Il codice di esempio accetta due argomenti della riga di comando che simulano i parametri di query &quot;id&quot; e &quot;email&quot; per il servizio web esterno descritto sopra.

1. args[0] = ID account
1. args[1] = Indirizzo e-mail

Passa i valori effettivi dall’istanza come argomenti al programma. Ecco una schermata di configurazione del progetto da Intellij IDEA.

**SyncEmailUnsubscribe.java**

```java
package com.marketo;

import org.json.simple.JSONArray;
import org.json.simple.JSONObject;
import org.json.simple.parser.JSONParser;
import org.json.simple.parser.ParseException;

import javax.net.ssl.HttpsURLConnection;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.net.MalformedURLException;
import java.net.URL;
import java.util.Iterator;
import java.util.NoSuchElementException;
import java.util.Scanner;

public class SyncEmailUnsubscribe {
    // Define Marketo instance meta data here.
    // Each row contains three elements: Account Id, Client Id, Client Secret.
    // For example:
    //  public static String instanceData[][] = {
    //    {"111-AAA-222", "2f4a4435-f6fa-4bd9-3248-098754982345", "asdf6IVE9h4Jjcl59cOMAKFSk78ut12W"},
    //    {"222-BBB-333", "5f4a6657-f6fa-4cd9-4356-123083238821", "gfjgfIVE9h4Jjcl59cOMAKFSk78ut12W"},
    //    {"444-CCC-444", "9f4a4678-f6fa-4dd9-7735-908713247721", "xzcxvbVE9h4Jjcl59cOMAKFSk78ut12W"}
    //  };
    //
    public static String instanceData[][] = {
            // ADD YOUR INSTANCE META DATA HERE
    };

    public static void main(String[] args) {
        String accountId = args[0];     // Account id that processed the unsubscribe
        String emailAddress = args[1];  // Email address of lead that unsubscribed

        SyncEmailUnsubscribe seu = new SyncEmailUnsubscribe();

        // Loop through each Marketo instance
        for (int i = 0; i < instanceData.length; i++) {

            // Make sure we skip instance that triggered the webhook
            if (!accountId.equals(instanceData[i][0])) {
                String endpointUrl = String.format("https://%s.mktorest.com", instanceData[i][0]);

                // Generate access token
                String identityUrl = String.format("%s/identity/oauth/token?grant_type=client_credentials&client_id=%s&client_secret=%s", endpointUrl, instanceData[i][1], instanceData[i][2]);
                String token = seu.getToken(identityUrl);

                // Get lead records for given email address (may be duplicates)
                String getLeadsUrl = String.format("%s/rest/v1/leads.json?access_token=%s&filterType=email&filterValues=%s", endpointUrl, token, emailAddress);
                String leads = seu.getLeads(getLeadsUrl);

                // Update unsubscribed field in lead record
                String updateLeadsUrl = String.format("%s/rest/v1/leads.json?access_token=%s", endpointUrl, token);
                seu.updateLeads(updateLeadsUrl, leads, accountId);
            }
        }

        System.exit(0);
    }

    // Call Identity Service to generate access token
    public String getToken(String url) {
        // Call Identity Service
        String tokenData = getData(url);

        // Convert response into JSONObject
        JSONParser parser = new JSONParser();
        Object obj = null;
        try {
            obj = parser.parse(tokenData);
        } catch (ParseException pe) {
            System.out.println("position: " + pe.getPosition());
            System.out.println(pe);
        }

        // Retrieve access_token
        JSONObject jsonObject = (JSONObject)obj;
        return jsonObject.get("access_token").toString();
    }

    // Call Get Multiple Leads by Filter Type Service to get lead records
    public String getLeads(String url) {
        return getData(url);
    }

    // Call Create/Update Lead Service to update "unsubscribed" flag in lead record
    public void updateLeads(String url, String leads, String account) {
        JSONObject body = composeBody(leads, account);
        if (body != null) {
            postData(url, body);
        }
    }

    // Compose JSON body for Create/Update Leads Service
    private JSONObject composeBody(String leads, String account) {
        JSONObject body = new JSONObject();

        // Convert leads into JSONObject
        JSONParser parser = new JSONParser();
        Object obj = null;
        try {
            obj = parser.parse(leads);
        } catch (ParseException pe) {
            System.out.println("position: " + pe.getPosition());
            System.out.println(pe);
        }
        JSONObject leadsObj = (JSONObject)obj;

        Object success = leadsObj.get("success");
        if (success.equals(true)) {
            body.put("action", "updateOnly");
            body.put("lookupField", "id");
            body.put("asyncProcessing", "true");

            // Build array of lead objects
            JSONArray input = new JSONArray();
            JSONArray result = (JSONArray) leadsObj.get("result");
            Iterator<JSONObject> iterator = result.iterator();
            while (iterator.hasNext()) {
                JSONObject leadIn = (JSONObject)iterator.next();
                JSONObject lead = new JSONObject();
                lead.put("id", leadIn.get("id"));
                lead.put("unsubscribed", "true");
                lead.put("unsubscribedReason", "Cross instance synch triggered by webhook from: " + account);
                input.add(lead);
            }

            body.put("input", input);
        }
        return body;
    }

    // HTTP POST request
    private String postData(String endpoint, JSONObject body) {
        String data = "";
        try {
            // Make request
            URL url = new URL(endpoint);
            HttpsURLConnection urlConn = (HttpsURLConnection) url.openConnection();
            urlConn.setRequestMethod("POST");
            urlConn.setAllowUserInteraction(false);
            urlConn.setDoOutput(true);
            urlConn.setRequestProperty("Content-type", "application/json");
            urlConn.setRequestProperty("accept", "application/json");
            urlConn.connect();
            OutputStream os = urlConn.getOutputStream();
            os.write(body.toJSONString().getBytes());
            os.close();
            int responseCode = urlConn.getResponseCode();
            if (responseCode == 200) {
                System.out.println("Status: 200");
                InputStream inStream = urlConn.getInputStream();
                data = convertStreamToString(inStream);
                System.out.println(data);
            } else {
                System.out.println(responseCode);
                data = "Status:" + responseCode;
            }
        } catch (MalformedURLException e) {
            System.out.println("URL not valid.");
        } catch (IOException e) {
            System.out.println("IOException: " + e.getMessage());
            e.printStackTrace();
        }
        return data;
    }

    // HTTP GET request
    private String getData(String endpoint) {
        String data = "";
        try {
            URL url = new URL(endpoint);
            HttpsURLConnection urlConn = (HttpsURLConnection) url.openConnection();
            urlConn.setRequestMethod("GET");
            urlConn.setAllowUserInteraction(false);
            urlConn.setDoOutput(true);
            int responseCode = urlConn.getResponseCode();
            if (responseCode == 200) {
                System.out.println("Status: 200");
                InputStream inStream = urlConn.getInputStream();
                data = convertStreamToString(inStream);
                System.out.println(data);
            } else {
                System.out.println(responseCode);
                data = "Status:" + responseCode;
            }
        } catch (MalformedURLException e) {
            System.out.println("URL not valid.");
        } catch (IOException e) {
            System.out.println("IOException: " + e.getMessage());
            e.printStackTrace();
        }
        return data;
    }

    private String convertStreamToString(InputStream inputStream) {
        try {
            return new Scanner(inputStream).useDelimiter("A").next();
        } catch (NoSuchElementException e) {
            return "";
        }
    }
}
```

### Installazione di Marketo

Per ogni istanza di Marketo da sincronizzare, effettua le seguenti operazioni.

1. Creare un servizio personalizzato con l’autorizzazione del ruolo: lead di lettura-scrittura. Se non conosci la creazione di un servizio personalizzato, fai clic [qui](/help/rest-api/custom-services.md).
1. Crea un webhook che chiama il servizio web esterno. Se non hai familiarità con la creazione dei webhook, fai clic [qui](/help/webhooks/webhooks.md).
1. Aggiungi Webhook come passaggio di flusso in Smart Campaign.

La schermata seguente mostra come creare un webhook per richiamare il servizio specificato in precedenza utilizzando i token per popolare automaticamente i parametri di query. Ora che abbiamo creato il nostro webhook, possiamo aggiungerlo a una campagna avanzata come azione di flusso.  L’elenco avanzato deve contenere un trigger &quot;Unsubscriptions from Email&quot; (Annullamenti abbonamenti da e-mail).

### Convalida

Per eseguire il test, crea un lead con lo stesso indirizzo e-mail in diverse istanze di Marketo. Assicurati di essere il proprietario dell’indirizzo e-mail. In un’istanza, attiva un’azione di invio del flusso e-mail, apri l’e-mail risultante e fai clic su annulla iscrizione. Per convalidare i risultati, accedi a ciascuna delle altre istanze e controlla i record dei lead associati all’indirizzo e-mail. La casella di controllo &quot;Unsubscscriptions&quot; (Annullamento iscrizione) deve essere selezionata e il campo &quot;Unsubscscribe Reason&quot; (Motivo dell’annullamento dell’iscrizione) deve contenere una nota con l’ID dell’account di origine che ha avviato la sincronizzazione.

Pubblicato il _2015-05-11_ da _David_

## Sincronizzazione delle modifiche ai dati dei lead tramite API REST

Il post presentava un esempio di codice che poteva essere eseguito su base periodica per verificare la disponibilità di aggiornamenti in Marketo. L’idea era di utilizzare le API di Marketo per identificare le modifiche ai dati dei lead ed estrarre i dati dei lead che erano stati modificati. Questi dati potrebbero quindi essere inviati a un sistema esterno a scopo di sincronizzazione. L’esempio di codice presentato ha utilizzato la nostra API SOAP. Bene, abbiamo un [nuovo modo di camminare](https://www.youtube.com/watch?v=G-7ZJjLy5D8&feature=youtu.be), e in questo modo si utilizza l&#39;[API REST di Marketo](/help/rest-api/rest-api.md). Questo post mostra come raggiungere lo stesso obiettivo utilizzando due endpoint REST: [Get Lead Changes](/help/rest-api/rest-api.md), [Get Lead by Id](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadByIdUsingGET). Il programma prevede due fasi principali:

1. Richiama Ottieni modifiche lead per generare un elenco di tutti gli ID lead per i quali sono stati modificati campi lead specifici o che sono stati aggiunti durante un determinato periodo di tempo.
1. Richiama Ottieni ID lead per ogni ID lead nell’elenco per recuperare i dati del campo dal record lead.

Prenderemo i dati recuperati nel passaggio 2 e li formatteremo per l&#39;utilizzo da parte di un sistema esterno.  **Input programma** Per impostazione predefinita il programma &quot;torna indietro&quot; di un giorno dalla data corrente per cercare le modifiche. Ad esempio, è possibile eseguire questo programma ogni giorno alla stessa ora. Per tornare più indietro nel tempo, è possibile specificare il numero di giorni come argomento della riga di comando, aumentando di fatto l&#39;intervallo di tempo. Il programma contiene diverse variabili modificabili: CUSTOM_SERVICE_DATA - Contiene i dati del [Servizio personalizzato](/help/rest-api/custom-services.md) di Marketo (ID account, ID client, segreto client). LEAD_CHANGE_FIELD_FILTER - Contiene un elenco separato da virgole di campi lead che verranno esaminati per le modifiche. READ_BATCH_SIZE: numero di record da recuperare alla volta. Utilizzare questa opzione per regolare la risposta in base alle dimensioni del corpo. **Output programma** Il programma raccoglie tutti i record lead modificati e li formatta in JSON come array di oggetti lead come segue:

```json
{
    "result": [
        {
            "leadId": "318592",
            "updatedAt": "2015-07-22T19:19:07Z",
            "firstName": "David",
            "lastName": "Everly",
            "email": "<deverly@marketo.com>"
        },

        ...more lead objects here...
    ]
}
```

L’idea è di poter passare questo JSON come payload della richiesta a un servizio web esterno per sincronizzare i dati. **Logica del programma** Innanzitutto stabiliamo l&#39;intervallo di tempo, componiamo gli URL dell&#39;endpoint REST e otteniamo il token di accesso per l&#39;autenticazione. Successivamente viene attivato un loop Get Paging Token/Get Lead Changes in cui viene eseguita fino a esaurimento della fornitura di modifiche di lead. Lo scopo di questo ciclo è quello di accumulare un elenco di ID lead univoci in modo da poterli passare a Ottieni lead per ID più avanti nel programma. In questo esempio, viene indicato a Recupera modifiche lead di cercare le modifiche nei campi seguenti: firstName, lastName, e-mail. È possibile selezionare qualsiasi combinazione di campi per le proprie esigenze. Ottieni modifiche lead restituisce oggetti &quot;result&quot; contenenti un ID tipo di attività, che può essere utilizzato per filtrare i risultati. Nota: è possibile ottenere un elenco di tipi di attività chiamando l&#39;endpoint REST [Get Activity Types](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getAllActivityTypesUsingGET). Siamo interessati a 2 tipi di attività restituiti: 1. Nuovo lead (12)

```json
{
    "id": 12024682,
    "leadId": 318581,
    "activityDate": "2015-03-17T00:18:41Z",
    "activityTypeId": 12,
    "primaryAttributeValueId": 318581,
    "primaryAttributeValue": "David Everly",
    "attributes": [
        {
            "name": "Created Date",
            "value": "2015-03-16"
        },
        {
            "name": "Source Type",
            "value": "New lead"
        }
    ]
}
```

1. Change Data Value (13) È possibile individuare il campo modificato esaminando la proprietà &quot;name&quot; nella risposta Change Data Value.

```json
{
    "id": 12024689,
    "leadId": 318581,
    "activityDate": "2015-03-17T22:58:18Z",
    "activityTypeId": 13,
    "fields": [
        {
            "id": 31,
            "name": "lastName",
            "newValue": "Evely",
            "oldValue": "Everly"
        }
    ],
    "attributes": [
        {
            "name": "Source",
            "value": "Web form fillout"
        }
    ]
}
```

Quando viene restituito uno di questi due tipi di attività, l&#39;ID lead associato viene memorizzato in un elenco. Una volta che abbiamo il nostro elenco, possiamo scorrerlo chiamando Get Lead by Id per ogni elemento. In questo modo verranno recuperati i dati più recenti per ogni lead nell’elenco. In questo esempio vengono recuperati i seguenti campi lead: `leadId`, `updatedAt`, `firstName`, `lastName` e `email`. È possibile selezionare qualsiasi combinazione di campi per le proprie esigenze. A tal fine, specifica il parametro dei campi per ottenere lead per ID. Infine, JSONify i risultati come un array di oggetti lead come descritto sopra.

**Codice programma**

```java
package com.marketo;

// minimal-json library (<https://github.com/ralfstx/minimal-json>)
import com.eclipsesource.json.JsonArray;
import com.eclipsesource.json.JsonObject;
import com.eclipsesource.json.JsonValue;

import java.io.\*;
import java.net.MalformedURLException;
import java.net.URL;
import java.text.DateFormat;
import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.\*;

import javax.net.ssl.HttpsURLConnection;

public class LeadChanges {
    //
    // Define Marketo REST API access credentials: Account Id, Client Id, Client Secret.  For example:
    //    public static String CUSTOM_SERVICE_DATA[] =
    //      {"111-AAA-222", "2f4a4435-f6fa-4bd9-3248-098754982345", "asdf6IVE9h4Jjcl59cOMAKFSk78ut12W"};
    //
    private static final String CUSTOM_SERVICE_DATA[] =
            {INSERT YOUR CUSTOM SERVICE DATA HERE};

    // Lead fields that we are interested in
    private static final String LEAD_CHANGE_FIELD_FILTER = "firstName,lastName,email";

    // Number of lead records to read at a time
    private static final String READ_BATCH_SIZE = "200";

    // Activity type ids that we are interested in
    private static final int ACTIVITY_TYPE_ID_NEW_LEAD = 12;
    private static final int ACTIVITY_TYPE_ID_CHANGE_DATA_VALE = 13;

    public static void main(String[] args) {
        // Command line argument to set how far back to look for lead changes (number of days)
        int lookBackNumDays = 1;
        if (args.length == 1) {
            lookBackNumDays = Integer.parseInt(args[0]);
        }

        // Establish "since date" using current timestamp minus some number of days (default is 1 day)
        Calendar cal = Calendar.getInstance();
        cal.add(Calendar.DAY_OF_MONTH, -lookBackNumDays);
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd'T'HH:mm:ssZ");
        String sinceDateTime = sdf.format(cal.getTime());

        // Compose base URL
        String baseUrl = String.format("https://%s.mktorest.com",
                CUSTOM_SERVICE_DATA[0]);

        // Compose Identity URL
        String identityUrl = String.format("%s/identity/oauth/token?grant_type=%s&client_id=%s&client_secret=%s",
                baseUrl, "client_credentials", CUSTOM_SERVICE_DATA[1], CUSTOM_SERVICE_DATA[2]);

        // Call Identity API
        JsonObject identityObj = JsonObject.readFrom(getData(identityUrl));
        String accessToken = identityObj.get("access_token").asString();

        // Compose URLs for Get Lead Changes, and Get Paging Token
        String leadChangesUrl = String.format("%s/rest/v1/activities/leadchanges.json?access_token=%s&fields=%s&batchSize=%s",
                baseUrl, accessToken, LEAD_CHANGE_FIELD_FILTER, READ_BATCH_SIZE);
        String pagingTokenUrl = String.format("%s/rest/v1/activities/pagingtoken.json?access_token=%s&sinceDatetime=%s",
                baseUrl, accessToken, sinceDateTime);

        HashSet leadIdList = new HashSet();

        // Call Get Paging Token API
        JsonObject pagingTokenObj = JsonObject.readFrom(getData(pagingTokenUrl));
        if (pagingTokenObj.get("success").asBoolean()) {

            String nextPageToken = pagingTokenObj.get("nextPageToken").asString();
            boolean moreResult;

            do {
                moreResult = false;

                // Call Get Lead Changes API
                JsonObject leadChangesObj = JsonObject.readFrom(getData(String.format("%s&nextPageToken=%s",
                        leadChangesUrl, nextPageToken)));
                if (leadChangesObj.get("success").asBoolean()) {
                    moreResult = leadChangesObj.get("moreResult").asBoolean();
                    nextPageToken = leadChangesObj.get("nextPageToken").asString();

                    if (leadChangesObj.get("result") != null) {
                        JsonArray resultAry = leadChangesObj.get("result").asArray();
                        for (JsonValue resultObj : resultAry) {
                            int activityTypeId = resultObj.asObject().get("activityTypeId").asInt();


                            // Store lead ids for later use
                            boolean storeThisId = false;
                            if (activityTypeId == ACTIVITY_TYPE_ID_NEW_LEAD) {
                                storeThisId = true;
                            } else if (activityTypeId == ACTIVITY_TYPE_ID_CHANGE_DATA_VALE) {
                                // See if any of the changed fields are of interest to us
                                JsonArray fieldsAry = resultObj.asObject().get("fields").asArray();
                                for (JsonValue fieldsObj : fieldsAry) {
                                    String name = fieldsObj.asObject().get("name").asString();
                                    if (LEAD_CHANGE_FIELD_FILTER.contains(name)) {
                                        storeThisId = true;
                                    }
                                }

                            }

                            if (storeThisId) {
                                leadIdList.add(resultObj.asObject().get("leadId").toString());
                            }
                        }
                    }
                }

            } while (moreResult);
        }

        JsonObject result = new JsonObject();
        JsonArray leads = new JsonArray();

        for (Object o : leadIdList) {
            String leadId = o.toString();

            // Compose Get Lead by Id URL
            String getLeadUrl = String.format("%s/rest/v1/lead/%s.json?access_token=%s",
                    baseUrl, leadId, accessToken);

            // Call Get Lead by Id API
            JsonObject leadObj = JsonObject.readFrom(getData(getLeadUrl));
            if (leadObj.get("success").asBoolean()) {
                if (leadObj.get("result") != null) {
                    JsonArray resultAry = leadObj.get("result").asArray();
                    for (JsonValue resultObj : resultAry) {

                        // Create lead object
                        JsonObject lead = new JsonObject();
                        lead.add("leadId", leadId);
                        lead.add("updatedAt", resultObj.asObject().get("updatedAt").asString());
                        lead.add("firstName", resultObj.asObject().get("firstName").asString());
                        lead.add("lastName", resultObj.asObject().get("lastName").asString());
                        lead.add("email", resultObj.asObject().get("email").asString());

                        // Add lead object to leads array
                        leads.add(lead);
                    }
                }
            }
        }

        // Add leads array to result object
        result.add("result", leads);

        // Print out result object
        System.out.println(result);

        System.exit(0);
    }

    // Perform HTTP GET request
    private static String getData(String endpoint) {
        String data = "";
        try {
            URL url = new URL(endpoint);
            HttpsURLConnection urlConn = (HttpsURLConnection) url.openConnection();
            urlConn.setRequestMethod("GET");
            urlConn.setAllowUserInteraction(false);
            urlConn.setDoOutput(true);
            int responseCode = urlConn.getResponseCode();
            if (responseCode == 200) {
                InputStream inStream = urlConn.getInputStream();
                data = convertStreamToString(inStream);
            } else {
                System.out.println(responseCode);
                data = "Status:" + responseCode;
            }
        } catch (MalformedURLException e) {
            System.out.println("URL not valid.");
        } catch (IOException e) {
            System.out.println("IOException: " + e.getMessage());
            e.printStackTrace();
        }
        return data;
    }

    private static String convertStreamToString(InputStream inputStream) {
        try {
            return new Scanner(inputStream).useDelimiter("A").next();
        } catch (NoSuchElementException e) {
            return "";
        }
    }
}
```

Ecco qua, [la vita è una canzone felice](https://www.youtube.com/watch?v=zFaBwZDywLk). Buon lavoro!

Pubblicato il _2015-07-31_ da _David_

## Aggiornamenti sulla versione di maggio 2015

### REST API

* API opportunità. Sono stati introdotti nuovi [endpoint opportunità](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Opportunities) che consentono di elencare, descrivere e convertire in formato CRUD i dati che risiedono in un oggetto opportunità di Marketo a livello di programmazione.

Nota: sono state aggiunte le autorizzazioni del ruolo per fornire accesso agli endpoint Opportunità: Opportunità di sola lettura, Opportunità di lettura/scrittura. Se il tuo ruolo utente API è precedente al rilascio delle API Opportunity, devi creare un nuovo ruolo utente API con queste autorizzazioni per abilitare l’accesso. In caso contrario, viene visualizzata una risposta di errore 603 &quot;Accesso negato&quot;.

* API Asset - Snippet. Sono stati introdotti nuovi [endpoint di risorse per snippet](https://developer.adobe.com/marketo-apis/api/asset/#snippet_endpoints) per consentire di manipolare gli oggetti snippet a livello di programmazione. I frammenti possono essere utilizzati come blocchi di contenuto dinamici nelle e-mail e nelle pagine di destinazione.
* API lead - Aggiorna partizione lead. È stato aggiunto un nuovo endpoint [lead per le partizioni](https://developer.adobe.com/marketo-apis/api/mapi/#operation/updatePartitionsUsingPOST) per consentire l&#39;aggiornamento della partizione per uno o più lead.
* È stato risolto un problema a causa del quale le API relative al lead non disponevano della compensazione del fuso orario negli attributi &quot;createdAt&quot; e &quot;updatedAt&quot;.
* È stato risolto un problema a causa del quale Schedule Campaign non restituiva il codice di errore corretto quando era stato superato il numero massimo giornaliero di chiamate.
* È stato risolto un problema a causa del quale Ottieni cartella per ID talvolta restituiva un valore null per gli attributi &quot;parent&quot; e &quot;description&quot;.
* È stato risolto un problema a causa del quale l’approvazione dell’e-mail per ID generava, in alcuni casi, un errore di sistema.
* È stato risolto un problema a causa del quale la funzione Crea token per ID cartella generava un token inutilizzabile in alcuni casi.

### Real-Time Personalization (RTP)

* API di consigli per contenuti multimediali avanzati. Nuove funzionalità di [consigli per rich media](/help/javascript-api/web-personalization.md) sono state aggiunte all&#39;API JavaScript RTP. Il consiglio sui contenuti rich media coinvolge i visitatori web con i contenuti più rilevanti basati sull’apprendimento automatico e sull’analisi predittiva. Migliora le risorse di contenuto con descrizioni di testo e immagini e incorpora più consigli di contenuto nel sito web.

### SDK per il coinvolgimento mobile

iOS v0.3.4/Android v0.3.3

* Azioni personalizzate. È stata aggiunta la possibilità di tenere traccia dell’interazione dell’utente inviando azioni personalizzate. Per informazioni dettagliate, consulta &quot;Invio di azioni personalizzate&quot; [qui](/help/mobile/mobile.md).
* Il metodo trackPushNotification è stato dichiarato obsoleto.

Pubblicato il _2015-05-26_ da _David_

## Aggiornamenti sulla versione di giugno 2015

### REST API

* API società

Sono stati introdotti nuovi [endpoint aziendali](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Companies) che ti consentono di elencare, descrivere e sottoporre a CRUD a livello di programmazione i dati residenti all&#39;interno di un oggetto aziendale Marketo.

Nota: sono state aggiunte le autorizzazioni del ruolo per fornire accesso agli endpoint del programma: Società di sola lettura, Società di lettura/scrittura. Se il tuo ruolo utente API è precedente al rilascio delle API aziendali, devi aggiornare il tuo ruolo utente API con queste autorizzazioni per abilitare l’accesso. In caso contrario, riceverai una risposta di errore 603 &quot;Accesso negato&quot;

* API Asset - Programmi

API di Asset aggiornata per supportare sia le cartelle Applicazione che le cartelle Programma. Diverse API di Asset hanno accettato un ID cartella nella richiesta sotto forma di numero intero. L’ID cartella può essere passato come parte dell’URI, o come parte del corpo della richiesta, a seconda dell’API.

Ora devi specificare il tipo di cartella accanto all’ID della cartella. Il tipo di cartella è &quot;Program&quot; o &quot;Folder&quot; (Programma). &quot;Folder&quot; specifica una cartella Application, mentre &quot;Program&quot; specifica una cartella Program. Il tipo di cartella viene specificato in due modi diversi, a seconda dell’API che stai chiamando:

1. Utilizza la struttura dati &quot;FolderIdType&quot;. FolderIdType è un semplice blocco JSON contenente l’ID e la coppia di tipi come segue:

`{ "id" : **id**, "type" : "**type**" }`

Dove **id** è l&#39;ID cartella e &quot;**type**&quot; è il tipo di cartella. I valori consentiti per &quot;**type**&quot; sono &quot;Folder&quot; o &quot;Program&quot;.

Esempio: creare una cartella

`POST /asset/v1/folders.json`

`parent={"id":416,"type":"Folder"}&name=Test Folder&description=This is a test folder`

1. Utilizza il parametro id esistente e specificane il tipo utilizzando un parametro di query &quot;type&quot;.

Esempio - Ottieni cartella per ID

`GET /rest/asset/v1/folder/1016.json?type=Program`

Tutte le risposte API che contenevano un ID cartella nell’oggetto Result ora conterranno anche un attributo folderId il cui valore è FolderIdType. Può essere utilizzato per determinare il tipo di cartella per un determinato ID cartella.

Esempio - Ottieni cartella per nome

`GET /rest/asset/v1/folder/byName.json?name=Social Media`

```json
"result": [
{
...

"folderId": {"id":341, "type": "Program"},
...

"id":"341"
}
]
```

Per determinare il tipo di cartella per un determinato ID, è possibile utilizzare l&#39;API [Sfoglia cartelle](https://developer.adobe.com/marketo-apis/api/asset/browse-folders/).

L’attributo &quot;type&quot; nelle risposte API è stato rinominato &quot;folderType&quot;. In questo modo si evita confusione con l&#39;elemento &quot;type&quot; contenuto in FolderIdType.

Ad esempio:

&quot;**type**&quot;:&quot;Cartella di marketing&quot;

in questo:

&quot;**folderType**&quot;: &quot;Cartella di marketing&quot;

### SDK per il coinvolgimento mobile

iOS 0.3.5

* È stato risolto un problema che causava l’esecuzione della finestra di dialogo per il set test device sul thread principale. [MOB-638]
* È stata aggiunta la gestione degli errori in caso di mancata registrazione del dispositivo di prova. [MOB-639]

Android 0.3.3

* Attributo android:configChanges aggiunto all&#39;elemento `<activity>` AndroidManifest.xml per evitare che la finestra di dialogo relativa all&#39;avanzamento venga chiusa quando si aggiunge un dispositivo di test e si modifica l&#39;orientamento. [MOB-687]

Pubblicato il _2015-06-30_ da _David_

## Web Personalization in-app (Beta) tramite l’API RTP

Molti dei nostri clienti forniscono soluzioni di app web per i loro utenti e riceviamo richieste se possono utilizzare Marketo Real-Time Personalization (RTP) all’interno del loro ambiente di app web protetto. La risposta è sì! Abbiamo rilasciato un’API per la messaggistica in-app per personalizzare i contenuti e promuovere attività di marketing come webinar, nuove versioni di funzioni, up-sell e coinvolgere gli utenti in base ai dati dell’app web. Ad esempio, personalizzare il contenuto in-app per:

* Offerte di prova basate sull’attività dell’utente
* Diversi tipi di abbonamento (up-sell, cross-selling o formazione tramite webinar)
* Nuove funzioni relative all’attività dell’utente

**Esempio di caso d&#39;uso** Il team Customer Success di Marketo utilizza In-App Web Personalization per comunicare con tipi di abbonamento specifici (Spark, Standard, Select o Enterprise) con contenuti personalizzati, assicurandosi che visualizzino campagne progressive e stimolino gli utenti in-app in base al loro coinvolgimento. Vediamo come eseguire questa operazione per un utente con un tipo di abbonamento Enterprise. **Prerequisito** Comprendere l&#39;API [Contesto utente RTP](/help/javascript-api/web-personalization.md). **Abilitare la richiesta dell&#39;API per il contesto utente** dal supporto Marketo per abilitare l&#39;API per il contesto utente per l&#39;account RTP. **Imposta la variabile personalizzata** Sono disponibili 5 slot di variabili personalizzate in RTP a cui inviare i dati. In questo esempio, invieremo un abbonamento utente di tipo Enterprise alla variabile personalizzata 1.

`rtp('set', 'customVar1', 'Enterprise');`

**Crea un nuovo segmento RTP** Vai a **Segmenti**, fai clic su **CREA NUOVO**.

1. Trascina il filtro **API Contesto utente** nel generatore di segmenti.
1. Selezionare la **Variabile personalizzata 1** (tipo di sottoscrizione) che **valore** è **Enterprise**.

**Mostra campagna basata sulla cronologia visite precedenti** Per mostrare al visitatore un&#39;altra campagna se ha già fatto clic su una campagna in una visita precedente.

1. Nell&#39;API **Contesto utente**, fare clic su **(+)** per aggiungere un altro attributo API Contesto utente
1. Aggiungi l&#39;operatore **AND o OR.**
1. Seleziona **Campagne - Fai clic su.** Imposta **ID campagna** sull&#39;ID della campagna. Consulta la nota seguente su come trovare l’ID campagna.
1. Fai clic su **SALVA E DEFINISCI CAMPAGNA** per creare la creatività della campagna.

Nel complesso, questo segmento corrisponderà se un visitatore è associato alla variabile personalizzata (tipo di abbonamento) uguale a Enterprise e se ha fatto clic sulla campagna (ID: 5390) in una visita precedente. Il passaggio successivo consiste nel definire una campagna personalizzata per questo segmento. La schermata seguente mostra una campagna di dialogo RTP (in basso a sinistra) che appare sulla pagina My Marketo per promuovere un webinar per gli utenti Enterprise.  **NOTA:** **Individuazione dell&#39;ID campagna** Vai a **Campagne**, passa il puntatore del mouse su **Nome campagna** per individuare l&#39;ID campagna.
Pubblicato il _2015-06-17_ da _David_

## Invio di e-mail transazionali con l’API REST di Marketo: parte 1

In Marketo sono presenti alcuni requisiti di configurazione per eseguire la chiamata richiesta con l’API REST di Marketo.

* Il destinatario deve avere un record in Marketo
* È necessario creare e approvare un’e-mail transazionale nella tua istanza di Marketo.
* È necessaria una campagna trigger attiva con la campagna richiesta, Source: API del servizio web, configurata per inviare l’e-mail

Crea e approva il tuo indirizzo e-mail[. ](https://experienceleague.adobe.com/it/docs/marketo/using/home) Se l’e-mail è effettivamente transazionale, probabilmente dovrai impostarla su operativa, ma assicurati che sia giuridicamente qualificata come operativa. Questa è configurata da con la schermata Modifica in Azioni e-mail > Impostazioni e-mail. Approvalo e siamo pronti a creare la nostra campagna. Se non hai ancora creato le campagne, consulta l&#39;articolo [Creare una nuova campagna avanzata](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/core-marketo-concepts/smart-campaigns/creating-a-smart-campaign/create-a-new-smart-campaign) su docs.marketo.com. Dopo aver creato la campagna, dobbiamo seguire questi passaggi. Configurare l’elenco avanzato con il trigger Richiesta di Campaign: ora è necessario configurare il flusso in modo che indirizzi un passaggio Invia e-mail alla nostra e-mail. Prima dell’attivazione, è necessario decidere alcune impostazioni nella scheda Pianificazione. Se questa e-mail in particolare deve essere inviata una sola volta a un determinato record, lascia invariate le impostazioni di qualifica. Tuttavia, se è necessario che ricevano l’e-mail più volte, è necessario modificarla ogni volta o in base a una delle cadenze disponibili. Ora è possibile attivare.

### Invio delle chiamate API

**Nota:** negli esempi Java seguenti, utilizziamo il pacchetto minimal-json per gestire le rappresentazioni JSON nel nostro codice. Puoi trovare ulteriori informazioni su questo progetto qui: [https://github.com/ralfstx/minimal-json](https://github.com/ralfstx/minimal-json) La prima parte dell&#39;invio di un&#39;e-mail transazionale tramite l&#39;API consiste nel verificare che nell&#39;istanza Marketo esista un record con l&#39;indirizzo e-mail corrispondente e che sia possibile accedere al relativo ID lead. Ai fini del presente post, supponiamo che gli indirizzi e-mail siano già presenti in Marketo e che sia sufficiente recuperare l’ID del record. Per questo, stiamo utilizzando la chiamata [Ottieni più lead per tipo di filtro](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET) e stiamo riutilizzando parte del codice Java del post precedente su. Diamo un’occhiata al nostro metodo principale per richiedere la campagna:

```java
package dev.marketo.blog_request_campaign;

import com.eclipsesource.json.JsonArray;

public class App
{
    public static void main( String[] args )
    {
     //Create an instance of Auth so that we can authenticate with our Marketo instance
        Leads leadsRequest = new Leads(auth).setFilterType("email").addFilterValue("<requestCampaign.test@marketo.com>");

        //Create and parameterize an instance of Leads
        //Set your email filterValue appropriately
        Leads leadsRequest = new Leads(auth).setFilterType("email").addFilterValue("test.requestCamapign@example.com");

        //Get the inner results array of the response
        JsonArray leadsResult = leadsRequest.getData().get("result").asArray();

        //Get the id of the record indexed at 0
        int lead = leadsResult.get(0).asObject().get("id").asInt();

        //Set the ID of your campaign from Marketo
        int campaignId = 0;
        RequestCampaign rc = new RequestCampaign(auth, campaignId).addLead(lead);

        //Send the request to Marketo
        rc.postData();
    }
}
```

Per ottenere questi risultati dalla risposta JsonObject di leadRequest, è necessario scrivere del codice. Per recuperare il primo risultato nell’array, è necessario estrarre l’array dall’oggetto JsonObject e ottenere l’oggetto indicizzato su 0:

`JsonArray leadsResult = leadsRequest.getData().get("result").asArray();`
`int leadId = leadsResult.get(0).asObject().get("id").asInt();`

Da qui ora tutto ciò che dobbiamo fare è la chiamata Request Campaign. A questo scopo, i parametri richiesti sono ID nell’URL della richiesta e una matrice di oggetti JSON contenente un membro, &quot;id&quot;. Diamo un’occhiata al codice per questo:

```java
package dev.marketo.blog_request_campaign;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.io.Reader;
import java.net.MalformedURLException;
import java.net.URL;
import java.util.ArrayList;
import javax.net.ssl.HttpsURLConnection;
import com.eclipsesource.json.JsonArray;
import com.eclipsesource.json.JsonObject;

public class RequestCampaign {
 private String endpoint;
 private Auth auth;
 public ArrayList leads = new ArrayList();
 public ArrayList tokens = new ArrayList();

 public RequestCampaign(Auth auth, int campaignId) {
  this.auth = auth;
  this.endpoint = this.auth.marketoInstance + "/rest/v1/campaigns/" + campaignId + "/trigger.json";
 }
 public RequestCampaign setLeads(ArrayList leads) {
  this.leads = leads;
  return this;
 }
 public RequestCampaign addLead(int lead){
  leads.add(lead);
  return this;
 }
 public RequestCampaign setTokens(ArrayList tokens) {
  this.tokens = tokens;
  return this;
 }
 public RequestCampaign addToken(String tokenKey, String val){
  JsonObject jo = new JsonObject().add("name", tokenKey);
  jo.add("value", val);
  tokens.add(jo);
  return this;
 }
 public JsonObject postData(){
  JsonObject result = null;
  try {
   JsonObject requestBody = buildRequest(); //builds the Json Request Body
   String s = endpoint + "?access_token=" + auth.getToken(); //takes the endpoint URL and appends the access_token parameter to authenticate
   System.out.println("Executing RequestCampaign calln" + "Endpoint: " + s + "nRequest Body:n"  + requestBody);
   URL url = new URL(s);
   HttpsURLConnection urlConn = (HttpsURLConnection) url.openConnection(); //Return a URL connection and cast to HttpsURLConnection
   urlConn.setRequestMethod("POST");
   urlConn.setRequestProperty("Content-type", "application/json");
            urlConn.setRequestProperty("accept", "text/json");
            urlConn.setDoOutput(true);
   OutputStreamWriter wr = new OutputStreamWriter(urlConn.getOutputStream());
   wr.write(requestBody.toString());
   wr.flush();
   InputStream inStream = urlConn.getInputStream(); //get the inputStream from the URL connection
   Reader reader = new InputStreamReader(inStream);
   result = JsonObject.readFrom(reader); //Read from the stream into a JsonObject
   System.out.println("Result:n" + result);
  } catch (MalformedURLException e) {
   e.printStackTrace();
  } catch (IOException e) {
   e.printStackTrace();
  }
  return result;
 }

 private JsonObject buildRequest(){
  JsonObject requestBody = new JsonObject(); //Create a new JsonObject for the Request Body
  JsonObject input = new JsonObject();
  JsonArray leadsArray = new JsonArray();
  for (int lead : leads) {
   JsonObject jo = new JsonObject().add("id", lead);
   leadsArray.add(jo);
  }
  input.add("leads", leadsArray);
  JsonArray tokensArray = new JsonArray();
  for (JsonObject jo : tokens) {
   tokensArray.add(jo);
  }
  input.add("tokens", tokensArray);
  requestBody.add("input", input);
  return requestBody;
 }

}
```

Questa classe ha un costruttore che esegue un’autenticazione e l’ID della campagna. I lead vengono aggiunti all&#39;oggetto passando un oggetto ArrayList `<Integer>` contenente gli ID dei record da impostareLeads oppure utilizzando addLead, che prende un numero intero e lo aggiunge all&#39;oggetto ArrayList esistente nella proprietà lead. Per attivare la chiamata API per passare i record dei lead alla campagna, è necessario chiamare postData, che restituisce un JsonObject contenente i dati di risposta della richiesta. Quando viene chiamata una campagna di richiesta, ogni lead passato alla chiamata viene elaborato dalla campagna del trigger di destinazione in Marketo e a be viene inviata l’e-mail creata in precedenza. Congratulazioni, hai attivato un’e-mail tramite l’API REST di Marketo. Tieni d’occhio la Parte 2, in cui esamineremo la possibilità di personalizzare dinamicamente il contenuto di un’e-mail tramite Request Campaign.

Pubblicato il _2015-07-17_ da _Kenny_

## Autenticazione e recupero di dati di lead da Marketo con l’API REST

**Nota:** negli esempi Java seguenti, utilizziamo il pacchetto minimal-json per gestire le rappresentazioni JSON nel nostro codice. Ulteriori informazioni sul progetto sono disponibili qui: [https://github.com/ralfstx/minimal-json](https://github.com/ralfstx/minimal-json) Uno dei requisiti più comuni durante l&#39;integrazione con Marketo è il recupero dei dati del lead. La maggior parte delle integrazioni, se non tutte, richiederà il recupero o l&#39;invio di dati sui lead da una sottoscrizione di Marketo. Pertanto, oggi esamineremo un&#39;attività di recupero di base delle informazioni sui lead [autenticando](/help/rest-api/authentication.md) con una sottoscrizione e recuperando i dati sui lead da essa. Per recuperare i lead, devi prima eseguire l’autenticazione con l’istanza Marketo di destinazione utilizzando OAuth 2.0. È necessario autenticare tre informazioni con Marketo, l’ID client, il segreto client e l’host dell’istanza Marketo. Di seguito è riportata la classe che utilizziamo per l’autenticazione:

```java
package dev.marketo.blog_leads;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.io.Reader;
import java.net.MalformedURLException;
import java.net.URL;
import javax.net.ssl.HttpsURLConnection;
import com.eclipsesource.json.JsonObject;

public class Auth {
 protected String marketoInstance; //Instance Host obtained from Admin -> Web Service
 private String clientId; //clientId obtained from Admin -> Launchpoint -> View Details for selected service
 private String clientSecret; //clientSecret obtained from Admin -> Launchpoint -> View Details for selected service
 private String idEndpoint; //idEndpoint constructed to authenticate with service when constructor is used
 private String token; //token is stored for reuse until expiration
 private Long expiry; //used to store time of expiration

 //Creates an instance of Auth which is used to Authenticate with a particular service on a particular instance
 public Auth(String id, String secret, String instanceUrl) {
  this.clientId = id;
  this.clientSecret = secret;
  this.marketoInstance = instanceUrl;
  this.idEndpoint = marketoInstance + "/identity/oauth/token?grant_type=client_credentials&client_id=" + clientId + "&client_secret=" + clientSecret;
 }
 //The only public method of Auth, used to check if the current value of Token is valid, and then to retrieve a new one if it is not
 public String getToken(){
  long now  = System.currentTimeMillis();
  if (expiry == null || expiry < now){
   System.out.println("Token is empty or expired. Trying new authentication");
   JsonObject jo = getData();
   System.out.println("Got Authentication Response: " + jo);
   this.token = jo.get("access_token").asString();
                        //expires_in is reported as seconds, set expiry to system time in ms + expires \* 1000
   this.expiry = System.currentTimeMillis() + (jo.get("expires_in").asLong() \* 1000);
  }
  return this.token;
 }
 //Executes the authentication request
 private JsonObject getData(){
  JsonObject jsonObject = null;
  try {
   URL url = new URL(idEndpoint);
   HttpsURLConnection urlConn = (HttpsURLConnection) url.openConnection();
   urlConn.setRequestMethod("GET");
            urlConn.setRequestProperty("accept", "application/json");
            System.out.println("Trying to authenticate with " + idEndpoint);
            int responseCode = urlConn.getResponseCode();
            if (responseCode == 200) {
                InputStream inStream = urlConn.getInputStream();
                Reader reader = new InputStreamReader(inStream);
                jsonObject = JsonObject.readFrom(reader);
            }else {
                throw new IOException("Status: " + responseCode);
            }
  } catch (MalformedURLException e) {
   e.printStackTrace();
  }catch (IOException e) {
            e.printStackTrace();
        }
  return jsonObject;
 }
}
```

Questo codice consente di creare un’istanza di Auth con il nostro ID client, segreto client e host (come marketoInstance) da Admin -> Launchpoint (ID e segreto) e Admin -> Web Services (host). Espone il metodo getToken che verifica se il token attualmente memorizzato è nullo o scaduto e quindi restituisce il token esistente oppure effettua una nuova richiesta di autenticazione e quindi restituisce il nuovo token dal membro &quot;access_token&quot; della risposta JSON. Ora che puoi autenticarti con la tua istanza di Marketo, il passaggio successivo è recuperare i lead. Questa classe è in uso:

```java
package dev.marketo.blog_leads;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.io.Reader;
import java.net.MalformedURLException;
import java.net.URL;
import java.util.ArrayList;
import javax.net.ssl.HttpsURLConnection;
import com.eclipsesource.json.JsonObject;

public class Leads {
 private StringBuilder endpoint;
 private Auth auth;
 public String filterType;
 public ArrayList filterValues = new ArrayList();
 public Integer batchSize;
 public String nextPageToken;
 public ArrayList fields = new ArrayList();

 public Leads(Auth a) {
  this.auth = a;
  this.endpoint = new StringBuilder(this.auth.marketoInstance + "/rest/v1/leads.json");
 }
 public Leads setFilterType(String filterType) {
  this.filterType = filterType;
  return this;
 }
 public Leads setFilterValues(ArrayList filterValues) {
  this.filterValues = filterValues;
  return this;
 }
 public Leads addFilterValue(String filterVal) {
  this.filterValues.add(filterVal);
  return this;
 }
 public Leads setBatchSize(Integer batchSize) {
  this.batchSize = batchSize;
  return this;
 }
 public Leads setNextPageToken(String nextPageToken) {
  this.nextPageToken = nextPageToken;
  return this;
 }
 public Leads setFields(ArrayList fields) {
  this.fields = fields;
  return this;
 }

 public JsonObject getData() {
        JsonObject result = null;
        try {
         endpoint.append("?access_token=" + auth.getToken() + "&filterType=" + filterType + "&filterValues=" + csvString(filterValues));
         if (batchSize != null && batchSize > 0 && batchSize <= 300){
             endpoint.append("&batchSize=" + batchSize);
            }
            if (nextPageToken != null){
             endpoint.append("&nextPageToken=" + nextPageToken);
            }
            if (fields != null){
             endpoint.append("&fields=" + csvString(fields));
            }
            URL url = new URL(endpoint.toString());
            HttpsURLConnection urlConn = (HttpsURLConnection) url.openConnection();
            urlConn.setRequestMethod("GET");
            urlConn.setRequestProperty("accept", "text/json");
            InputStream inStream = urlConn.getInputStream();
            Reader reader = new InputStreamReader(inStream);
            result = JsonObject.readFrom(reader);
        } catch (MalformedURLException e) {
            System.out.println("URL not valid.");
        } catch (IOException e) {
            System.out.println("IOException: " + e.getMessage());
            e.printStackTrace();
        }
        return result;
    }
 private String csvString(ArrayList fields) {
  StringBuilder fieldCsv = new StringBuilder();
     for (int i = 0; i < fields.size(); i++){
      fieldCsv.append(fields.get(i));
      if (i + 1 != fields.size()){
       fieldCsv.append(",");
      }
     }
  return fieldCsv.toString();
 }
}
```

Questa classe ha un singolo costruttore che accetta un oggetto Auth e quindi espone diversi setter sia per i parametri facoltativi che per quelli obbligatori. In questo caso, dobbiamo preoccuparci solo di impostare filterType e filterValues per ottenere i lead per indirizzo e-mail. Pertanto, utilizzeremo setFilterType per &quot;e-mail&quot; e addFilterValue per l’indirizzo e-mail per il quale abbiamo bisogno di recuperare un ID. Dopo aver impostato i parametri, è possibile utilizzare il metodo getData per recuperare un oggetto JsonObject dall&#39;endpoint lead, contenente una matrice di risultati con una rappresentazione JSON dei record lead recuperati.

### Mettere insieme

Dopo aver analizzato il codice di esempio in uso, è possibile esaminare un semplice esempio per recuperare i lead corrispondenti a un indirizzo di posta elettronica di prova, <testlead@marketo.com>. A questo scopo, è necessario utilizzare setFilterType per &quot;email&quot; e addFilterValue per l’indirizzo e-mail per il quale è necessario recuperare informazioni. Dopo aver impostato i parametri, è possibile utilizzare il metodo getData per recuperare un oggetto JsonObject dall&#39;endpoint lead, contenente una matrice di risultati con una rappresentazione JSON dei record lead recuperati.

```java
package dev.marketo.blog_leads;

import com.eclipsesource.json.JsonObject;

public class App
{
    public static void main( String[] args )
    {
     //Create an instance of Auth so that we can authenticate with our Marketo instance
        //Change the credentials here to your own
        Auth auth = new Auth("CHANGE ME", "CHANGE ME", "CHANGE ME");
        //Create and parameterize an instance of Leads
        Leads getLeads = new Leads(auth)
              .setFilterType("email")
              .addFilterValue("<testlead@marketo.com>");
        //get the inner results array of the response
        JsonObject leadsResult = getLeads.getData();
        System.out.println(leadsResult);
    }
}
```

In questo esempio di metodo principale viene creata un&#39;istanza di Auth e quindi viene passata a un nuovo costruttore di lead. Utilizzando setFilterType e addFilterValue, l&#39;istanza di Lead viene configurata per recuperare solo i lead corrispondenti all&#39;indirizzo e-mail &quot;<testlead@marketo.com>&quot;.&quot; Questo esempio stampa il file nella console:

Il token è vuoto o scaduto. Tentativo di nuova autenticazione
Tentativo di autenticazione con <https://299-BYM-827.mktorest.com/identity/oauth/token?grant_type=client_credentials&client_id=b417d98f-9289-47d1-a61f-db141bf0267f&client_secret=0DipOvz4h2wP1ANeVjlfwMvECJpo0ZYc>
Risposta di autenticazione ottenuta: {&quot;access_token&quot;:&quot;ec0f02c0-28ac-4d6c-b7d7-00e47ae85ff1:st&quot;,&quot;token_type&quot;:&quot;bearer&quot;,&quot;expires_in&quot;:538,&quot;scope&quot;:&quot;<apiuser@mktosupport.com>&quot;}
{&quot;requestId&quot;:&quot;14fb6#14e6a7a9ad6&quot;,&quot;result&quot;:[{&quot;id&quot;:1026322,&quot;updatedAt&quot;:&quot;2015-07-07T21:43:25Z&quot;,&quot;lastName&quot;:&quot;Lead&quot;,&quot;email&quot;:&quot;<testlead@marketo.com>&quot;,&quot;createdAt&quot;:&quot;2015-07-07T21:43:25Z&quot;,&quot;firstName&quot;:&quot;Test&quot;},{ &quot;:1026323,&quot;updatedAt&quot;:&quot;2015-07-07T21:43:43Z&quot;,&quot;lastName&quot;:&quot;Lead2&quot;,&quot;email&quot;:&quot;<testlead@marketo.com>&quot;,&quot;createdAt&quot;:&quot;2015-07-07T21:43:43Z&quot;,&quot;firstName&quot;:&quot;Test&quot;}],&quot;success&quot;:true}

Ora abbiamo dati di riferimento che possiamo elaborare in qualsiasi modo ci serva. Grazie per la lettura. Lasciate nei commenti il vostro feedback.

Pubblicato il _2015-07-10_ da _Kenny_

## Aggiornamenti della versione di luglio 2015

REST API

* API venditore

Sono stati introdotti nuovi [endpoint per i venditori](/help/rest-api/sales-persons.md) che ti consentono di elencare, descrivere e convertire in formato CRUD i dati che risiedono in un oggetto per i venditori Marketo. Inoltre, un venditore può essere assegnato a un lead, a un&#39;opportunità o a una società. Questa operazione viene eseguita specificando un attributo &quot;externalSalesPersonId&quot; quando si chiama l’endpoint Create/Update/Upsert per lead, opportunità o società.

Nota: sono state aggiunte le autorizzazioni per il ruolo per fornire accesso agli endpoint del programma: Venditore di sola lettura, Venditore di lettura e scrittura. Se il tuo ruolo utente API è precedente al rilascio delle API Venditore, devi aggiornare il tuo ruolo utente API con queste autorizzazioni per abilitare l’accesso. In caso contrario, viene visualizzata una risposta di errore 603 &quot;Accesso negato&quot;.

* API risorse - Modello per pagina di destinazione

Sono stati introdotti nuovi [endpoint per modelli di pagina di destinazione](https://developer.adobe.com/marketo-apis/api/asset/#landing_page_templates_endpoints) che ti consentono di elencare, creare e aggiornare in modo programmatico i dati associati a un modello di pagina di destinazione.

* API Asset - Segmenti

Sono stati introdotti due endpoint correlati ai segmenti:

[Ottieni segmenti](https://developer.adobe.com/marketo-apis/api/asset/get-segments)

[Ottieni segmentazione per ID](https://developer.adobe.com/marketo-apis/api/asset/get-segmentation-by-id)

* È stato risolto un problema che impediva a Get Folder by Name di rispettare il parametro &quot;workSpace&quot;. [LM-61059]
* Sono stati apportati diversi miglioramenti alle prestazioni delle API per oggetti personalizzati.

Pubblicato il _2015-07-17_ da _David_

## Creare e associare lead, società e opportunità con l’API REST di Marketo

Per sfruttare appieno le funzionalità di analisi di Marketo, è fondamentale creare associazioni corrette e solide tra i record relativi a lead, azienda e opportunità. Quando non si sfrutta una sincronizzazione CRM nativa, la creazione di queste relazioni può porre alcune difficoltà, quindi oggi passiamo attraverso la loro creazione.

### Relazioni tra oggetti

In Marketo, esistono alcune relazioni vitali per stabilire completamente la generazione di rapporti sulle opportunità:

* Lead e opportunità hanno una relazione molti-a-molti con l&#39;oggetto OpportunityRole.
* OpportunityRole dispone di un campo leadId e di un campo externalopportunityid per creare la relazione da lead a opportunità.
* Per essere idoneo per un filtro con elenco avanzato Opportunità, un lead deve avere un OpportunityRole correlato a un’opportunità.
* Le opportunità hanno una relazione molti-a-uno con l’oggetto Company tramite il campo externalCompanyId.
* I lead hanno una relazione uno-a-molti con le aziende tramite il campo externalCompanyId.
* Le opportunità vengono attribuite a un programma basato sul programma di acquisizione di un lead o sulla sua appartenenza e successo in un programma (vedere [Informazioni sull&#39;attribuzione](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/reporting/revenue-cycle-analytics/revenue-tools/attribution/understanding-attribution)).

La creazione di queste relazioni nel database dei lead ti consentirà di sfruttare appieno le analisi di Marketo e di comprendere l’influenza dei tuoi programmi sulla creazione di opportunità e sui tassi di successo.

### Aziende

Il modo più semplice per costruire queste relazioni è partire dalla creazione di un’azienda. In questo modo possiamo passare externalCompanyId alle nostre opportunità durante la creazione, invece di dover eseguire chiamate API aggiuntive per aggiornare le opportunità dopo che sono state create. A seconda della configurazione esistente, questo può essere o meno un passaggio necessario, ma per creare le relazioni è necessario aggiungere questi record all’istanza di Marketo per i nuovi lead e contatti netti con le aziende associate. Vediamo quindi un po’ di codice per creare i record aziendali.

```java
package dev.marketo.opportunities;

import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.io.Reader;
import java.net.MalformedURLException;
import java.net.URL;
import java.util.ArrayList;
import java.util.List;

import javax.net.ssl.HttpsURLConnection;

import com.eclipsesource.json.JsonArray;
import com.eclipsesource.json.JsonObject;

public class UpsertCompanies {
 public List<JsonObject> input; //a list of Companies to use for input.  Each must have a member "externalCompanyId".
 public String action; //specify the action to be undertaken, createOnly, updateOnly, createOrUpdate
 public String dedupeBy; //select mode of Deduplication, dedupeFields for all dedupe parameters(externalCompanyId), idField for marketoId
 private String endpoint; //endpoint URL created with Constructor
 private Auth auth; //Marketo Auth Object


 //Constructs an UpsertOpportunities with Auth, but with no input set
 public UpsertCompanies(Auth auth){
  this.auth = auth;
  this.endpoint = this.auth.marketoInstance + "/rest/v1/companies.json";
 }
 //Constructs and UpsertOpportunities with Auth and input set
 public UpsertCompanies(Auth auth, List<JsonObject> input) {
  this(auth);
  this.input = input;
 }
 //adds input to existing list, creates arraylist if it was built without a list
 public UpsertCompanies addCompanies(JsonObject... companies){
  if (this.input == null){
   this.input = new ArrayList();
  }
  for (JsonObject jo : companies) {
   System.out.println(jo);
   this.input.add(jo);
  }
  return this;
 }

 public JsonObject postData(){
  JsonObject result = null;
  try {
   JsonObject requestBody = buildRequest(); //builds the Json Request Body
   String s = endpoint + "?access_token=" + auth.getToken(); //takes the endpoint URL and appends the access_token parameter to authenticate
   URL url = new URL(s);
   HttpsURLConnection urlConn = (HttpsURLConnection) url.openConnection(); //Return a URL connection and cast to HttpsURLConnection
   urlConn.setRequestMethod("POST");
   urlConn.setRequestProperty("Content-type", "application/json");
            urlConn.setRequestProperty("accept", "text/json");
            urlConn.setDoOutput(true);
   OutputStreamWriter wr = new OutputStreamWriter(urlConn.getOutputStream());
   wr.write(requestBody.toString());
   wr.flush();
   InputStream inStream = urlConn.getInputStream(); //get the inputStream from the URL connection
   Reader reader = new InputStreamReader(inStream);
   result = JsonObject.readFrom(reader); //Read from the stream into a JsonObject
   urlConn.disconnect();
  } catch (MalformedURLException e) {
   e.printStackTrace();
  } catch (IOException e) {
   e.printStackTrace();
  }
  return result;
 }

 private JsonObject buildRequest(){
  JsonObject requestBody = new JsonObject(); //Create a new JsonObject for the Request Body
  JsonArray in = new JsonArray(); //Create a JsonArray for the "input" member to hold Opp records
  for (JsonObject jo : input) {
   in.add(jo); //add our company records to the input array
  }
  requestBody.add("input", in);
  if (this.action != null){
   requestBody.add("action", action); //add the action member if available
  }
  if (this.dedupeBy != null){
   requestBody.add("dedupeBy", dedupeBy); //add the dedupeBy member if available
  }
  return requestBody;
 }
 //Getters and Setters
 //Setters return the UpsertCompanies instance to allow simple formatting:
 public List<JsonObject> getInput() {
  return input;
 }
 //sets or replaces existing input with list
 public UpsertCompanies setInput(List input) {
  this.input = input;
  return this;
 }
 public String getAction() {
  return action;
 }
 public UpsertCompanies setAction(String action) {
  this.action = action;
  return this;
 }
 public String getDedupeBy() {
  return dedupeBy;
 }
 public UpsertCompanies setDedupeBy(String dedupeBy) {
  this.dedupeBy = dedupeBy;
  return this;
 }

}
```

L’API companies fornisce due opzioni di deduplicazione, impostate dal parametro dedupeBy nella richiesta, &quot;dedupeFields&quot; e &quot;idField&quot;. Questi possono essere recuperati esplicitamente chiamando Descrivi società. Se dedupeBy non è impostato, per impostazione predefinita viene impostata la proprietà dedupeFields. Nel caso dei record aziendali, dedupeFields corrisponde sempre a &quot;externalCompanyId&quot;, che è una stringa arbitraria impostata da un&#39;origine esterna, e idField, corrispondente al campo &quot;marketoId&quot;, che è un numero intero generato e restituito da Marketo dopo la creazione. A seconda della selezione di dedupeBy, uno di externalCompanyId o marketoId deve essere incluso in qualsiasi chiamata upsert per un record aziendale. Questi stessi requisiti si applicano alle API oggetto Ruolo opportunità e Opportunità. Il nostro codice espone due costruttori: uno che accetta un singolo argomento di un oggetto Auth e un altro che accetta Auth e un elenco di [record aziendali JsonObject](https://github.com/ralfstx/minimal-json). Se vengono costruiti senza un elenco di input, è necessario aggiungere i record aziendali tramite il metodo addCompanies, che controllerà se l&#39;input è Null la creazione di un nuovo ArrayList e quindi aggiungerà tutti gli argomenti JsonObject all&#39;elenco di input. Di seguito è riportato un esempio di utilizzo:

```java
//Create a new company to associate to
JsonObject myCompany = new JsonObject().add("externalCompanyId", "myCompany");
UpsertCompanies upsertCompanies = new UpsertCompanies(auth).addCompanies(myCompany);
JsonObject companiesResult = upsertCompanies.postData();
System.out.println(companiesResult);
```

È in corso la creazione di un oggetto JsonObject per una singola società con un solo campo, `externalCompanyId`, quindi la costruzione di un&#39;istanza di UpsertCompanies e l&#39;aggiunta della società all&#39;elenco di input con `addCompanies`.

### Opportunità

Analogamente agli oggetti aziendali, l’API dell’opportunità dispone di un parametro `dedupeBy` che accetta &quot;dedupeFields&quot; o &quot;idField&quot;, corrispondente rispettivamente a &quot;externalopportunityid&quot; e &quot;marketoGUID&quot;. Ecco il nostro codice, che è molto simile alla classe UpsertCompanies:

```java
package dev.marketo.opportunities;

import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.io.Reader;
import java.net.MalformedURLException;
import java.net.URL;
import java.util.ArrayList;
import java.util.List;

import javax.net.ssl.HttpsURLConnection;

import com.eclipsesource.json.JsonArray;
import com.eclipsesource.json.JsonObject;

public class UpsertOpportunities {
 public List<JsonObject> input; //a list of Opportunities to use for input.  Each must have a member "externalopportunityid".  Each can optionally include "externalCompanyId" for company association
 public String action; //specify the action to be undertaken, createOnly, updateOnly, createOrUpdate
 public String dedupeBy; //select mode of Deduplication, dedupeFields for all dedupe parameters, idField for marketoId
 private String endpoint; //endpoint URL created with Constructor
 private Auth auth; //Marketo Auth Object


 //Constructs an UpsertOpportunities with Auth, but with no input set
 public UpsertOpportunities(Auth auth){
  this.auth = auth;
  this.endpoint = this.auth.marketoInstance + "/rest/v1/opportunities.json";
 }
 //Constructs and UpsertOpportunities with Auth and input set
 public UpsertOpportunities(Auth auth, List<JsonObject> input) {
  this(auth);
  this.input = input;
 }
 public UpsertOpportunities addOpportunities(JsonObject... opp){
  if (this.input == null){
   this.input = new ArrayList();
  }
  for (JsonObject jo : opp) {
   System.out.println(jo);
   this.input.add(jo);
  }
  return this;
 }

 public JsonObject postData(){
  JsonObject result = null;
  try {
   JsonObject requestBody = buildRequest(); //builds the Json Request Body
   String s = endpoint + "?access_token=" + auth.getToken(); //takes the endpoint URL and appends the access_token parameter to authenticate
   URL url = new URL(s);
   HttpsURLConnection urlConn = (HttpsURLConnection) url.openConnection(); //Return a URL connection and cast to HttpsURLConnection
   urlConn.setRequestMethod("POST");
   urlConn.setRequestProperty("Content-type", "application/json");
            urlConn.setRequestProperty("accept", "text/json");
            urlConn.setDoOutput(true);
   OutputStreamWriter wr = new OutputStreamWriter(urlConn.getOutputStream());
   wr.write(requestBody.toString());
   wr.flush();
   InputStream inStream = urlConn.getInputStream(); //get the inputStream from the URL connection
   Reader reader = new InputStreamReader(inStream);
   result = JsonObject.readFrom(reader); //Read from the stream into a JsonObject
   urlConn.disconnect();
  } catch (MalformedURLException e) {
   e.printStackTrace();
  } catch (IOException e) {
   e.printStackTrace();
  }
  return result;
 }

 private JsonObject buildRequest(){
  JsonObject requestBody = new JsonObject(); //Create a new JsonObject for the Request Body
  JsonArray in = new JsonArray(); //Create a JsonArray for the "input" member to hold Opp records
  for (JsonObject jo : input) {
   in.add(jo); //add our Opportunity records to the input array
  }
  requestBody.add("input", in);
  if (this.action != null){
   requestBody.add("action", action); //add the action member if available
  }
  if (this.dedupeBy != null){
   requestBody.add("dedupeBy", dedupeBy); //add the dedupeBy member if available
  }
  return requestBody;
 }
 //Getters and Setters
 //Setters return the UpsertOpportunites instance to allow simple formatting:
 public List<JsonObject> getInput() {
  return input;
 }
 public UpsertOpportunities setInput(List<JsonObject> input) {
  this.input = input;
  return this;
 }
 public String getAction() {
  return action;
 }
 public UpsertOpportunities setAction(String action) {
  this.action = action;
  return this;
 }
 public String getDedupeBy() {
  return dedupeBy;
 }
 public UpsertOpportunities setDedupeBy(String dedupeBy) {
  this.dedupeBy = dedupeBy;
  return this;
 }

}
```

Vengono fornite le stesse opzioni del costruttore, utilizzando un metodo Auth o `Auth+List<JsonObject>` e un metodo `addOpportunities` per immettere opportunità JsonObject. Di seguito è riportato un esempio di utilizzo:

```java
//Create some JsonObjects for Opportunity Data
JsonObject opp1 = new JsonObject().add("name", "opportunity1")
    .add("externalopportunityid", "Opportunity1Test")
    .add("externalCompanyId", "myCompany")
    .add("externalCreatedDate", "2015-01-01T00:00:00z");
JsonObject opp2 = new JsonObject().add("name", "opportunity2")
    .add("externalopportunityid", "Opportunity2Test")
    .add("externalCompanyId", "myCompany")
    .add("externalCreatedDate", "2015-01-01T00:00:00z");

//Create an Instance of UpsertOpportunities and POST it
UpsertOpportunities upsertOpps = new UpsertOpportunities(auth)
                        .setAction("createOnly")
                        .addOpportunities(opp1, opp2);
JsonObject oppsResult = upsertOpps.postData();
System.out.println(oppsResult);
```

In questo caso, stiamo creando due opportunità di esempio e fornendo loro i valori per i campi name, externalopportunityid, externalCompanyId e externalCreatedDate. Non abbiamo ancora discusso externalCreatedDate, ma è importante utilizzarlo poiché viene trattato come campo principale in RCE per quando è stata creata un’opportunità, rendendola importante per la corretta attribuzione. Puoi utilizzare la logica di business della tua organizzazione per determinare cosa hai inserito in questo campo, in base al fatto che stai eseguendo la retrocompilazione dei dati di opportunità esistenti o creandone di nuovi al volo. Creiamo l’istanza di UpsertOpportunities e quindi aggiungiamo i nostri JsonObjects tramite addOpportunities. Ora che l’istanza è configurata, puoi inviarla a Marketo con postData e stampare il risultato

### Ruoli

I ruoli sono molto simili ai due oggetti precedenti, con la differenza che hanno requisiti leggermente diversi quando si imposta dedupeBy su dedupeFields. I ruoli richiedono l&#39;inclusione di tre campi durante la creazione o l&#39;aggiornamento di un record tramite questo metodo, &quot;leadId&quot;, &quot;role&quot; e &quot;externalopportunityid&quot;. &quot;role&quot; può essere un qualsiasi valore stringa, ma gli altri due devono fare riferimento rispettivamente a un ID valido di un lead e a un ID valido di un’opportunità.

```java
package dev.marketo.opportunities;

import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.io.Reader;
import java.net.MalformedURLException;
import java.net.URL;
import java.util.ArrayList;
import java.util.List;

import javax.net.ssl.HttpsURLConnection;

import com.eclipsesource.json.JsonArray;
import com.eclipsesource.json.JsonObject;

public class UpsertOpportunityRoles {
 public List<JsonObject> input; //Array of Opportunity Roles as JsonObjects, must have "leadId", "role" and "externalopprtunityid"
 public String action; //specify the action to be undertaken, createOnly, updateOnly, createOrUpdate, defaults to createOrUpdate if unset
 public String dedupeBy;//select mode of Deduplication, dedupeFields for all dedupe parameters, idField for marketoId
 private String endpoint; //endpoint URL created with Constructor
 private Auth auth; //Marketo Auth Object

 //Constructs an UpsertOpportunityRoles with Auth, but with no input set
 public UpsertOpportunityRoles(Auth auth) {
  this.auth = auth;
  this.endpoint = this.auth.marketoInstance + "/rest/v1/opportunities/roles.json";
 }
 //Constructs and UpsertOpportunities with Auth and input set
 public UpsertOpportunityRoles(Auth auth, List<JsonObject> input) {
  this(auth);
  this.input = input;
 }
 public UpsertOpportunityRoles addRoles(JsonObject... role){
  if (this.input == null){
   this.input = new ArrayList();
  }
  for (JsonObject jo : role) {
   System.out.println(jo);
   this.input.add(jo);
  }
  return this;
 }
 //executes the request to Marketo, body will be empty if input is not set
 public JsonObject postData(){
  JsonObject result = null;
  try {
   JsonObject requestBody = buildRequest(); //builds the Json Request Body
   String s = endpoint + "?access_token=" + auth.getToken(); //takes the endpoint URL and appends the access_token parameter to authenticate
   URL url = new URL(s);
   HttpsURLConnection urlConn = (HttpsURLConnection) url.openConnection(); //Return a URL connection and cast to HttpsURLConnection
   urlConn.setRequestMethod("POST");
   urlConn.setRequestProperty("Content-type", "application/json");//"application/json" content-type is required.
            urlConn.setRequestProperty("accept", "text/json");
            urlConn.setDoOutput(true);
   OutputStreamWriter wr = new OutputStreamWriter(urlConn.getOutputStream());
   wr.write(requestBody.toString());
   wr.flush();
   InputStream inStream = urlConn.getInputStream();  //get the inputStream from the URL connection
   Reader reader = new InputStreamReader(inStream);
   result = JsonObject.readFrom(reader); //Read from the stream into a JsonObject
   urlConn.disconnect();
  } catch (MalformedURLException e) {
   e.printStackTrace();
  } catch (IOException e) {
   e.printStackTrace();
  }
  return result;
 }

 public JsonObject buildRequest(){
  JsonObject requestBody = new JsonObject();
  JsonArray in = new JsonArray();
  for (JsonObject jo : input) {
   in.add(jo);
  }
  requestBody.add("input", in);
  if (this.action != null){
   requestBody.add("action", action);
  }
  if (this.dedupeBy != null){
   requestBody.add("dedupeBy", dedupeBy);
  }
  return requestBody;
 }
 //Getters and Setters
 //Setters return the UpsertOpportunites instance to allow simple formatting:
 public List<JsonObject> getInput() {
  return input;
 }
 public UpsertOpportunityRoles setInput(List<JsonObject> input) {
  this.input = input;
  return this;
 }
 public String getAction() {
  return action;
 }
 public UpsertOpportunityRoles setAction(String action) {
  this.action = action;
  return this;
 }
 public String getDedupeBy() {
  return dedupeBy;
 }
 public UpsertOpportunityRoles setDedupeBy(String dedupeBy) {
  this.dedupeBy = dedupeBy;
  return this;
 }

}
```

Stiamo seguendo un pattern simile a quello degli esempi precedenti per i metodi costruttors e addRoles. Ecco un esempio.

```java
//Create Some opp roles now
JsonObject opp1Role = new JsonObject()
                        .add("role", "Captain")
                        .add("externalopportunityid", opp1.get("externalopportunityid").asString())
                        .add("leadId", 318794);
JsonObject opp2Role = new JsonObject()
                        .add("role", "Commander")
                        .add("externalopportunityid", opp2.get("externalopportunityid").asString())
                        .add("leadId", 318795);

//Create an Instance of UpsertOpportunityRoles and POST it
UpsertOpportunityRoles upsertRoles = new UpsertOpportunityRoles(auth)
                        .setAction("createOnly")
                        .addRoles(opp1Role, opp2Role);
JsonObject rolesResult = upsertRoles.postData();
System.out.println(rolesResult);
```

Stiamo creando i nuovi JsonObjects per i nostri 2 ruoli di esempio e aggiungendo i loro dedupeFields richiesti, estraendo l’esternalopportunityid dalle opportunità già create, quindi inviandoli a Marketo.

### Tutti gli elementi insieme

Ecco l’esempio completo del nostro metodo principale:

```java
package dev.marketo.opportunities;

import com.eclipsesource.json.JsonObject;

public class App
{
    public static void main( String[] args )
    {
     //create an Instance of Auth
        Auth auth = new Auth("CLIENT_ID_CHANGE_ME", "CLIENT_SECRET_CHANGE_ME", "MARKETO_HOST_CHANGE_ME");

        //Create a new company to associate to
        JsonObject myCompany = new JsonObject().add("externalCompanyId", "myCompany");
        UpsertCompanies upsertCompanies = new UpsertCompanies(auth).addCompanies(myCompany);
        JsonObject companiesResult = upsertCompanies.postData();
        System.out.println(companiesResult);

        //Create some JsonObjects for Opportunity Data
        JsonObject opp1 = new JsonObject().add("name", "opportunity1")
           .add("externalopportunityid", "Opportunity1Test")
           .add("externalCompanyId", "myCompany")
           .add("externalCreatedDate", "2015-01-01T00:00:00z");
        JsonObject opp2 = new JsonObject().add("name", "opportunity2")
           .add("externalopportunityid", "Opportunity2Test")
           .add("externalCompanyId", "myCompany")
           .add("externalCreatedDate", "2015-01-01T00:00:00z");

        //Create an Instance of UpsertOpportunities and POST it
        UpsertOpportunities upsertOpps = new UpsertOpportunities(auth)
                                .setAction("createOnly")
                                .addOpportunities(opp1, opp2);
        JsonObject oppsResult = upsertOpps.postData();
        System.out.println(oppsResult);

        //Create Some opp roles now
        JsonObject opp1Role = new JsonObject()
           .add("role", "Captain")
           .add("externalopportunityid", opp1.get("externalopportunityid").asString())
           .add("leadId", 318794);
        JsonObject opp2Role = new JsonObject()
           .add("role", "Commander")
           .add("externalopportunityid", opp2.get("externalopportunityid").asString())
           .add("leadId", 318795);

        //Create an Instance of UpsertOpportunityRoles and POST it
        UpsertOpportunityRoles upsertRoles = new UpsertOpportunityRoles(auth)
           .setAction("createOnly")
           .addRoles(opp1Role, opp2Role);
        JsonObject rolesResult = upsertRoles.postData();
        System.out.println(rolesResult);

    }
}
```

Puoi vedere la sequenza di creazione di società, opportunità e ruoli. Ora è possibile sincronizzare i dati aziendali e di opportunità con Marketo.

Pubblicato il _2015-08-07_ da _Kenny_

## Invio di e-mail transazionali con l’API REST di Marketo: parte 2, contenuto personalizzato

Questa settimana stiamo esaminando come trasmettere contenuto dinamico alle e-mail tramite la chiamata API Request Campaign. Richiedi campagna non solo consente l&#39;attivazione delle e-mail esternamente, ma puoi anche sostituire il contenuto di [I miei token](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/core-marketo-concepts/programs/tokens/understanding-my-tokens-in-a-program) all&#39;interno di un&#39;e-mail. I token sono contenuti riutilizzabili che possono essere personalizzati a livello di programma o di cartella di marketing. Possono anche esistere come segnaposto da sostituire tramite la chiamata della campagna di richiesta.

### Creazione dell’e-mail

Per personalizzare il contenuto, è innanzitutto necessario configurare un [programma](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/core-marketo-concepts/programs/creating-programs/create-a-program) e un [messaggio e-mail](https://experienceleague.adobe.com/it/docs/marketo/using/home) in Marketo. Per generare il contenuto personalizzato, è necessario creare token all’interno del programma, quindi inserirli nell’e-mail che stiamo per inviare. Per semplicità, in questo esempio viene utilizzato un solo token, ma è possibile sostituire qualsiasi numero di token in un’e-mail, in Da e-mail, Da nome, Risposta o qualsiasi parte di contenuto nell’e-mail. Quindi creiamo un token Rich Text per la sostituzione e chiamiamolo &quot;bodyReplacement&quot;. Il formato Rich Text consente di sostituire qualsiasi contenuto nel token con HTML arbitrari che si desidera inserire. Non è possibile salvare i token mentre sono vuoti, quindi inserisci qui del testo segnaposto. Ora è necessario inserire il token nell’e-mail: questo token sarà ora accessibile per la sostituzione tramite una chiamata Request Campaign. Questo token può essere semplice come una singola riga di testo che deve essere sostituita per e-mail o può includere quasi l’intero layout dell’e-mail.

### Il codice

Stiamo espandendo il codice dalla settimana scorsa per passare token personalizzati alla chiamata della nostra campagna di richieste.

```java
package dev.marketo.blog_request_campaign;

import com.eclipsesource.json.JsonArray;

public class App
{
    public static void main( String[] args )
    {
     //Create an instance of Auth so that we can authenticate with our Marketo instance
        Auth auth = new Auth("Client ID - CHANGE ME", "Client Secret - CHANGE ME", "Host - CHANGE ME");

        //Create and parameterize an instance of Leads
        Leads leadsRequest = new Leads(auth).setFilterType("email").addFilterValue("requestCampaign.test@marketo.com");

        //get the inner results array of the response
        JsonArray leadsResult = leadsRequest.getData().get("result").asArray();

        //get the id of the record indexed at 0
        int lead = leadsResult.get(0).asObject().get("id").asInt();

        //Set the ID of our campaign from Marketo
        int campaignId = 1578;
        RequestCampaign rc = new RequestCampaign(auth, campaignId).addLead(lead);

        //Create the content of the token here, and add it to the request
        String bodyReplacement = "<div class="replacedContent"><p>This content has been replaced</p></div>";
        rc.addToken("{{my.bodyReplacement}}", bodyReplacement);
        rc.postData();
    }
}
```

Questa volta stiamo creando il contenuto del token nella variabile bodyReplacement e quindi utilizzando il metodo addToken per aggiungerlo alla richiesta. addToken prende una chiave e un valore, quindi crea una rappresentazione JsonObject e la aggiunge all’array dei token interni. Viene quindi serializzato durante il metodo postData e viene creato un corpo simile al seguente:

`{"input":{"leads":[{"id":1}],"tokens":[{"name":"{{my.bodyReplacement}}","value":"<div class="replacedContent"><p>This content has been replaced</p></div>"}]}}`

Combinato, l’output della console si presenta così:

```bash
Token is empty or expired. Trying new authentication
Trying to authenticate with&nbsp;...
Got Authentication Response: {"access_token":"19d51b9a-ff60-4222-bbd5-be8b206f1d40:st","token_type":"bearer","expires_in":3565,"scope":"<apiuser@mktosupport.com>"}
Executing RequestCampaign call
Endpoint: .../rest/v1/campaigns/1578/trigger.json?access_token=19d51b9a-ff60-4222-bbd5-be8b206f1d40:st
Request Body:
{"input":{"leads":[{"id":1}],"tokens":[{"name":"{{my.bodyReplacement}}","value":"<div class="replacedContent"><p>This content has been replaced</p></div>"}]}}
Result:
{"requestId":"1e8d#14eadc5143d","result":[{"id":1578}],"success":true}
```

### Ritorno a capo

Questo metodo è estensibile in diversi modi, modificando il contenuto nelle e-mail all’interno di singole sezioni di layout o all’esterno delle e-mail, consentendo di trasmettere valori personalizzati in attività o momenti interessanti. Con questo metodo è possibile personalizzare qualsiasi punto in cui un token può essere utilizzato all’interno di un programma. Funzionalità simili sono disponibili anche con la chiamata [Pianifica campagna](https://developer.adobe.com/marketo-apis/api/mapi/#operation/scheduleCampaignUsingPOST) che consente di elaborare i token in un&#39;intera campagna batch. Non possono essere personalizzate in base al singolo lead, ma sono molto utili per personalizzare i contenuti in un ampio insieme di lead.

Pubblicato il _2015-07-24_ da _Kenny_

## Aggiornamento dei dati dei lead in Marketo tramite l’API

Più di un anno fa abbiamo pubblicato Aggiornamento delle informazioni su clienti e potenziali clienti in Marketo utilizzando l’API. Il post presentava un esempio di codice che poteva essere eseguito su base periodica per raccogliere gli aggiornamenti da un sistema esterno. L&#39;idea era quella di recuperare i dati esterni e inviarli in Marketo per aggiornare le informazioni del lead. L’esempio di codice presentato ha utilizzato la nostra API SOAP. Endpoint REST per raggiungere lo stesso obiettivo. **Input programma** È probabile che sia necessario trasformare i dati dal sistema esterno in un formato utilizzabile dalle API REST di Marketo. Poiché utilizziamo l’API Create/Update Leads, i dati devono essere formattati come JSON e vengono inviati nel corpo della richiesta. Questo è meglio spiegato da un esempio. Nel codice di esempio Java riportato di seguito, sono stati inseriti dati lead fittizi in un array di stringhe. Ogni stringa che segue i dati per ogni lead: nome, cognome, indirizzo e-mail, titolo del processo.

```java
private static String externalLeadData[] = {
        "Henry, Adams, <henry@superstar.com>, Director of Demand Generation",
        "Suzie, Smith, <ssmith@gmail.com>, VP Marketing"
};
```

Il codice di esempio trasforma l’array nel blocco JSON seguente.

```json
{
"input":
    [
        {"firstName":"Henry", "lastName":"Adams", "email":"<henry@superstar.com>", "title":"Director of Demand Generation"},
        {"firstName":"Suzie", "lastName":"Smith", "email":"<ssmith@gmail.com>", "title":"VP Marketing"}
    ]
}
```

Ogni elemento di array &quot;input&quot; corrisponde a un singolo lead in Marketo. Gli elementi array sono oggetti JSON che contengono uno o più nomi di campi lead di Marketo e i rispettivi valori. I nomi dei campi che decidi di specificare (in questo caso nome, cognome, e-mail e titolo) devono corrispondere al nome API REST definito per la sottoscrizione di Marketo. Puoi trovare i nomi REST API nella sezione gestione campi nel pannello di amministrazione di Marketo esportando i nomi dei campi. I nomi dei campi verranno esportati in un file Excel, come illustrato di seguito. In alternativa, è possibile trovare i nomi dei campi a livello di programmazione chiamando l&#39;API [Descrivi lead](https://developer.adobe.com/marketo-apis/api/mapi/#operation/describeUsingGET_2). Ad esempio, di seguito è riportato uno snippet di risposta Describe che contiene il nome API REST per il campo Nome.

```json
{
    "id": 29,
    "displayName": "First Name",
    "dataType": "string",
    "length": 255,
    "soap": {
        "name": "FirstName",
        "readOnly": false
    },
    "rest": {
        "name": "firstName",
        "readOnly": false
    }
},
```

**Logica del programma** Una volta che il payload è nel formato corretto, siamo pronti a chiamare Crea/Aggiorna lead. In questo esempio non viene specificato alcun parametro facoltativo. Il comportamento predefinito consiste nel creare o aggiornare i record dei lead (upsert), utilizzare l’e-mail a scopo di deduplicazione e utilizzare l’elaborazione sincrona. Se la chiamata a Crea/Aggiorna lead ha esito positivo, il corpo della risposta contiene un array &quot;risultato&quot; contenente l’ID lead di Marketo e lo stato dell’operazione Crea/Aggiorna. A seconda del parametro &quot;action&quot; trasmesso nella richiesta, lo stato può essere &quot;updated&quot; (aggiornato), &quot;created&quot; (creato) o &quot;skipped&quot; (ignorato). Continuando con il nostro esempio, supponiamo che il primo lead non esista (Henry Adams), e il secondo lead esista (Suzie Smith). Una risposta simile alla seguente indicherebbe che il primo lead è stato creato e il secondo lead è stato aggiornato.

```json
{
    "success":true,
    "requestId":"118f8#14f1a0b82fc",
    "result":[
        {
            "id":318798,
            "status":"created"
        },
        {
            "id":318797,
            "status":"updated"
        }
    ]
}
```

È tutto per ora. Buon codice! **Codice programma**

```java
package com.marketo;

// minimal-json library (<https://github.com/ralfstx/minimal-json>)
import com.eclipsesource.json.JsonArray;
import com.eclipsesource.json.JsonObject;

import javax.net.ssl.HttpsURLConnection;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStreamWriter;
import java.net.MalformedURLException;
import java.net.URL;
import java.util.\*;

public class CreateUpdateLeads {
    //
    // Define Marketo REST API access credentials: Account Id, Client Id, Client Secret.  For example:
    //    public static String CUSTOM_SERVICE_DATA[] =
    //      {"111-AAA-222", "2f4a4435-f6fa-4bd9-3248-098754982345", "asdf6IVE9h4Jjcl59cOMAKFSk78ut12W"};
    //
    private static final String CUSTOM_SERVICE_DATA[] =
            { INSERT YOUR CUSTOM SERVICE DATA HERE };

    //
    // Mock up data that could be read from an external data source.
    // An array of CSV strings the form:
    //   "firstName, lastName, email, title"
    private static String externalLeadData[] = {
        "Henry, Adams, henry@superstar.com, Director of Demand Generation",
        "Suzie, Smith, ssmith@gmail.com, VP Marketing"
    };

    public static void main(String[] args) {
        // Compose base URL
        String baseUrl = String.format("https://%s.mktorest.com",
                CUSTOM_SERVICE_DATA[0]);

        // Compose Identity URL
        String identityUrl = String.format("%s/identity/oauth/token?grant_type=%s&client_id=%s&client_secret=%s",
                baseUrl, "client_credentials", CUSTOM_SERVICE_DATA[1], CUSTOM_SERVICE_DATA[2]);

        // Call Identity API
        JsonObject identityObj = JsonObject.readFrom(httpRequest("GET", identityUrl, null));
        String accessToken = identityObj.get("access_token").asString();

        // Compose URL for Create/Update Leads
        String createUpdateLeadsUrl = String.format("%s/rest/v1/leads.json?access_token=%s", baseUrl, accessToken);

        // Convert String array into JsonArray to pass as part of request body
        JsonArray input = convertExternalLeadDataToJson(externalLeadData);

        // Build request body JSON
        JsonObject requestObj = new JsonObject();
        requestObj.add("input", input);

        // Call Create/Update Lead API
        JsonArray result = new JsonArray();
        JsonObject leadObj = JsonObject.readFrom(httpRequest("POST", createUpdateLeadsUrl, requestObj.toString()));
        if (leadObj.get("success").asBoolean()) {
            if (leadObj.get("result") != null) {
                result = leadObj.get("result").asArray();
            }
        }

        // Print out results object
        System.out.println(result);
        System.exit(0);
    }

    // Convert array of CSV formatted Strings into an array of JSON objects
    private static JsonArray convertExternalLeadDataToJson(String input[]) {
        JsonArray output = new JsonArray();

        // Loop through each CSV row in array
        for (int i = 0; i < input.length; i++) {
            // Split CSV row into separate fields
            List items = Arrays.asList(input[i].split(","));

            // Add fields to JSON object
            JsonObject lead = new JsonObject();
            lead.add("firstName", items.get(0));
            lead.add("lastName", items.get(1));
            lead.add("email", items.get(2));
            lead.add("title", items.get(3));
            output.add(lead);
        }
        return output;
    }

    // Perform HTTP request
    private static String httpRequest(String method, String endpoint, String body) {
        String data = "";
        try {
            URL url = new URL(endpoint);
            HttpsURLConnection urlConn = (HttpsURLConnection) url.openConnection();
            urlConn.setDoOutput(true);
            urlConn.setRequestMethod(method);
            switch (method) {
                case "GET":
                    break;
                case "POST":
                    urlConn.setRequestProperty("Content-type", "application/json");
                    urlConn.setRequestProperty("accept", "text/json");
                    OutputStreamWriter wr = new OutputStreamWriter(urlConn.getOutputStream());
                    wr.write(body);
                    wr.flush();
                    break;
                default:
                    System.out.println("Error: Invalid method.");
                    return data;
            }
            int responseCode = urlConn.getResponseCode();
            if (responseCode == 200) {
                InputStream inStream = urlConn.getInputStream();
                data = convertStreamToString(inStream);
            } else {
                System.out.println(responseCode);
                data = "Status:" + responseCode;
            }
        } catch (MalformedURLException e) {
            System.out.println("URL not valid.");
        } catch (IOException e) {
            System.out.println("IOException: " + e.getMessage());
            e.printStackTrace();
        }
        return data;
    }

    private static String convertStreamToString(InputStream inputStream) {
        try {
            return new Scanner(inputStream).useDelimiter("A").next();
        } catch (NoSuchElementException e) {
            return "";
        }
    }
}
```

Pubblicato il _2015-08-14_ da _David_

## Aggiungi dati SalesPerson a Marketo

Con le nuove API SalesPerson, puoi associare liberamente i lead Marketo ai record SalesPerson in istanze senza un’integrazione CRM nativa. Ciò consente l&#39;utilizzo di {{lead.Lead Owner Email Address}} e dei relativi campi e token all&#39;interno di Marketo.

### Creazione di record SalesPerson

Per associare i lead ai record SalesPerson, è necessario inserire i record SalesPerson in Marketo. Ecco un esempio di classe in PHP:

```php
<?php

class SalesPerson{
 private $auth;//auth object
 private $action;// string designating request action, createOnly, updateOnly, createOrUpdate
 private $dedupeBy;//dedupeFields or idField
 private $input;//array of salesperson objects for input

 //takes an Auth object as the first argument
 public function _construct($auth, $input){
  $this->auth = $auth;
  $this->input = $input;
 }

 //constructs the json request body
 private function bodyBuilder(){
  $body = new stdClass();
  $body->input = $this->input;
  if (isset($this->action)){
   $body->action = $this->action;
  }
  if (isset($this->dedupeBy)){
   $body->dedupeBy = $this->dedupeBy;
  }
  return json_encode($body);
 }
 //submits the request to Marketo and returns the response as a string
 public function postData(){
  $url = $this->auth->getHost() . "/rest/v1/salespersons.json?access_token=" . $this->auth->getToken();
  $ch = curl_init($url);
  $requestBody = $this->bodyBuilder();
  curl_setopt($ch,  CURLOPT_RETURNTRANSFER, 1);
  curl_setopt($ch, CURLOPT_HTTPHEADER, array('accept: application/json','Content-Type: application/json'));
  curl_setopt($ch, CURLOPT_POST, 1);
  curl_setopt($ch, CURLOPT_POSTFIELDS, $requestBody);
  curl_getinfo($ch);
  $response = curl_exec($ch);
  curl_close($ch);
  return $response;
 }
 //getters and setters
 public function setAction($action){
  $this->action = $action;
 }
 public function getAction($action){
  return $this->action;
 }
 public function setDedupeBy($dedupeBy){
  $this->dedupeBy = $dedupeBy;
 }
 public function getDedupeBy($dedupeBy){
  return $this->dedupeBy;
 }
 public function addSalesPersons($input){
  if($this->input != null){
   if (is_array($input)){
    foreach($input as $item){
     array_push($this->input, $item);
    }
   }else{
    array_push($this->input, $input);
   }
  }else{
   $this->input = $input;
  }
 }
 public function getInput(){
  return $this->input;
 }

}
```

Nella classe precedente, la variabile $input è un array di oggetti stdClass con possibili membri:

* externalSalesPersonId(valido solo al momento della creazione)
* id(solo come chiave in modalità updateOnly)
* e-mail
* fax
* firstName
* lastName
* mobilePhone
* telefono
* titolo

Informazioni più dettagliate sui tipi e sulle lunghezze dei campi possono essere recuperate tramite la chiamata Describe SalesPerson.

### Sincronizzazione lead

Di seguito è riportata una breve classe di esempio per la sincronizzazione dei lead necessari:

```php
<?php

class Leads{
 private $auth;//auth object
 private $action;// string designating request action, createOnly, updateOnly, createOrUpdate
 private $lookupField;//dedupeFields or idField
 private $input;//array of salesperson objects for input
 private $asyncProcessing;//specify whether input should be processed asynchronously
 private $partitionName;//partition name for update if requires

 //takes an Auth object as the first argument
 public function _construct($auth, $input){
  $this->auth = $auth;
  $this->input = $input;
 }

 //constructs the json request body
 private function bodyBuilder(){
  $body = new stdClass();
  $body->input = $this->input;
  if (isset($this->action)){
   $body->action = $this->action;
  }
  if (isset($this->lookupField)){
   $body->lookupField = $this->lookupField;
  }
  return json_encode($body);
 }
 //submits the request to Marketo and returns the response as a string
 public function postData(){
  $url = $this->auth->getHost() . "/rest/v1/leads.json?access_token=" . $this->auth->getToken();
  $ch = curl_init($url);
  $requestBody = $this->bodyBuilder();
  curl_setopt($ch,  CURLOPT_RETURNTRANSFER, 1);
  curl_setopt($ch, CURLOPT_HTTPHEADER, array('accept: application/json','Content-Type: application/json'));
  curl_setopt($ch, CURLOPT_POST, 1);
  curl_setopt($ch, CURLOPT_POSTFIELDS, $requestBody);
  curl_getinfo($ch);
  $response = curl_exec($ch);
  curl_close($ch);
  return $response;
 }
 //getters and setters
 public function setAction($action){
  $this->action = $action;
 }
 public function getAction(){
  return $this->action;
 }
 public function setDedupeBy($lookupField){
  $this->dedupeBy = $lookupField;
 }
 public function getDedupeBy(){
  return $this->dedupeBy;
 }
 public function addLeads($input){
  if($this->input != null){
   if (is_array($input)){
    foreach($input as $item){
     array_push($this->input, $item);
    }
   }else{
    array_push($this->input, $input);
   }
  }else if (is_array($input)){
   $this->input = $input;
  }else{
   $this->input = [$input];
  }
 }
 public function getInput(){
  return $this->input;
 }
 public function setAsyncProcessing($asyncProcessing){
  $this->asyncProcessing = $asyncProcessing;
 }
 public function getAsyncProcessing() {
  return $this->asyncProcessing;
 }
 public function setPartitionName($partitionName){
  $this->partitionName = $partitionName;
 }
 public function getPartitionName() {
  return $this->partitionName;
 }

}
```

### Processo

Di seguito è riportato un esempio di creazione di due record di venditore e di associazione a due record di lead:

```php
<?php

require 'Auth.php';
require 'SalesPerson.php';
require 'Leads.php';

//Create an Auth object for authentication
$auth = new Auth("https://284-RPR-133.mktorest.com", "7f99bd49-0e0e-4715-a72a-0c9319f75552", "O5lZHhrQDfDKRhulY89IU42PfdhRSe6m");

//Compose new SalesPerson Records
$sales1 = new stdClass();
$sales1->externalSalesPersonId = "SalesPerson 1";
$sales1->email = "sales1@example.com";
$sales1->firstName = "Jane";
$sales1->lastName = "Doe";

$sales2 = new stdClass();
$sales2->externalSalesPersonId = "SalesPerson 2";
$sales2->email = "sales2@example.com";
$sales2->firstName = "John";
$sales2->lastName = "Doe";

//Compose lead records
$lead1 = new stdClass();
$lead1->email = "marketoDev@example.com";
//Add the external id of the desired salesperson
$lead1->externalSalesPersonId = $sales1->externalSalesPersonId;

$lead2 = new stdClass();
$lead2->email = "devBlog@example.com";
$lead2->externalSalesPersonId = $sales2->externalSalesPersonId;

//Create a new instance of SalesPerson to submit records
$salesPerson = new SalesPerson($auth, [$sales1, $sales2]);
$spResponse = $salesPerson->postData();
print_r($spResponse . "rn");
$json = json_decode($spResponse);

//Check for success on synching SalesPersons
if ($json->success){
 $leads = new Leads($auth, [$lead1, $lead2]);
 $leadsResponse = $leads->postData();
 print_r($leadsResponse . "rn");
}else{
 print_r("SalesPerson request failed with errors:rn");
 foreach ($json->errors as $error){
  print_r("code: " . $error->code . ", message: " . $error->message . "rn");
 }
}
```

Per le classi di esempio, stiamo solo creando oggetti stdClass per rappresentare i nostri record SalesPerson e Lead che devono essere sincronizzati, con ogni campo desiderato aggiunto come membro. Dopo l&#39;esecuzione di questo codice, i lead <marketoDev@example.com> e <devBlog@example.com> avranno entrambi i campi E-mail proprietario lead, Nome proprietario lead e Cognome proprietario lead compilati, consentendo loro di utilizzare i token pertinenti per tali campi e di filtrarli in base ai filtri elenco smart pertinenti.

### Classe di autenticazione

```php
<?php

class Auth{
 private $host;//host of the target Marketo instance
 private $clientId;//client Id
 private $clientSecret;//client secret
 private $token;//access_token
 private $expiry;

 function _construct($host, $clientId, $clientSecret){
  $this->host = $host;
  $this->clientId = $clientId;
  $this->clientSecret = $clientSecret;
 }
 public function getToken(){
  if (!isset($this->token) || $this->expiry > time()){
   $data = $this->getData();
   $this->expiry = time() + $data->expires_in;
   $this->token = $data->access_token;
   return $this->token;
  }else{
   return $this->token;
  }
 }
 private function getData(){
  $url = $this->host . "/identity/oauth/token?grant_type=client_credentials&client_id=" . $this->clientId . "&client_secret="
     . $this->clientSecret;
  $ch = curl_init($url);
  curl_setopt($ch,  CURLOPT_RETURNTRANSFER, 1);
  curl_setopt($ch, CURLOPT_HTTPHEADER, array('accept: application/json','Content-Type: application/json'));
  $response = curl_exec($ch);
  $json = json_decode($response);
  curl_close($ch);
  return $json;
 }
 public function getHost(){
  return $this->host;
 }
}
```

Pubblicato il _2015-08-21_ da _Kenny_

## Best practice per gli utenti API e i servizi personalizzati

Le API REST di Marketo utilizzano servizi personalizzati per l’autenticazione e ciascuno di questi servizi è di proprietà di un utente Marketo solo API. Le funzionalità di ciascun servizio personalizzato sono determinate dalle autorizzazioni di ciascun ruolo assegnato a tale utente. L’allocazione di singoli utenti e servizi personalizzati alle integrazioni offre diversi vantaggi:

* Puoi ottimizzare le [autorizzazioni](/help/rest-api/custom-services.md) assegnate a ogni singolo servizio tramite il ruolo assegnato all&#39;utente.
* Puoi disabilitare singoli servizi web dall’effettuare chiamate all’istanza eliminando il servizio personalizzato corrispondente, senza disabilitarne altri.
* I rapporti sull’utilizzo delle chiamate API saranno suddivisi per utente, consentendo di identificare l’utilizzo elevato e anormale
* È più facile determinare a quali dati ha accesso ogni servizio web
* Le istanze abilitate per Workspace possono limitare l’accesso a specifiche unità aziendali, assegnando ruoli solo alle aree di lavoro accessibili

### Utilizzo API

Ciascuno degli utenti API viene segnalato singolarmente nel rapporto sull’utilizzo delle API, pertanto la suddivisione dei servizi web per utente consente di tenere facilmente conto dell’utilizzo di ciascuna integrazione. Se il numero di chiamate API all’istanza supera il limite e le chiamate successive non riescono, l’utilizzo di questa procedura ti consente di tenere conto del volume di ciascuno dei servizi e di valutare come risolvere il problema. Controlla l’utilizzo andando in Amministratore -> Servizi web e facendo clic sul numero di chiamate negli ultimi 7 giorni.

### Disattivare un servizio

Se un’integrazione ha effetti indesiderati, può essere noioso e difficile da disabilitare se non hai assegnato a ciascuna un singolo servizio personalizzato. Suddividerli uno per uno rende facile quanto eliminare il servizio offensivo nel tuo Admin -> Launchpoint.

### Gestione Workspace

Per le sottoscrizioni di Marketo Enterprise, in genere un servizio richiede l&#39;accesso a una sola area di lavoro e questo può essere [imposto dall&#39;assegnazione di un ruolo all&#39;utente API](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/workspaces-and-person-partitions/allow-user-access-to-a-workspace). Ogni ruolo utente può essere assegnato a livello globale o per area di lavoro, pertanto l’accesso alle aree di lavoro può essere limitato, se necessario, fornendo il set di autorizzazioni più ridotto possibile.

Pubblicato il _2015-08-28_ da _Kenny_

## Come specificare le partizioni dei lead utilizzando l’API REST

**Partizionamento lead** Le partizioni lead di Marketo offrono un modo pratico per isolare i lead. Le partizioni possono consentire a diversi gruppi di marketing all’interno della tua organizzazione di condividere una singola istanza di Marketo. Per ulteriori informazioni, vedere [Informazioni sulle aree di lavoro e sulle partizioni lead](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/workspaces-and-person-partitions/understanding-workspaces-and-person-partitions). Supponiamo di utilizzare le partizioni lead e di creare lead a livello di programmazione utilizzando l’API REST di Marketo. Come puoi garantire che i lead creati finiscano nella partizione corretta? Questo post ti mostra come! Per il bene di questo esempio, utilizzeremo le aree di lavoro e le partizioni per isolare i lead in base alla geografia.

Innanzitutto definiremo un’area di lavoro denominata &quot;Country&quot;. Successivamente, vengono create due partizioni nell&#39;area di lavoro denominate &quot;Messico&quot; e &quot;Canada&quot;.  **Crea lead nella partizione** Si supponga ora di voler creare due lead nella partizione &quot;Messico&quot;. Per creare lead, si chiama. Per specificare la partizione, è necessario includere l’attributo &quot;partitionName&quot; nel corpo della richiesta. Come è possibile sapere cosa utilizzare per il valore partitionName? È possibile recuperare un elenco di valori di nomi di partizione validi per l&#39;istanza chiamando l&#39;API [Get Lead Partitions](https://developer.adobe.com/marketo-apis/api/mapi/#operation/describeProgramMemberUsingGET) nel modo seguente:

`GET /rest/v1/leads/partitions.json`

```json
{
    "requestId": "20ae#14f9a1a5a30",
    "result": [
        {
            "id": 1,
            "name": "Default",
            "description": "Initial system lead partition"
        },
        {
            "id": 2,
            "name": "Mexico",
            "description": "Leads that live in Mexico"
        },
        {
            "id": 3,
            "name": "Canada",
            "description": "Leads that live in Canada"
        }
    ],
    "success": true
}
```

In questo caso, il valore corretto è &quot;Messico&quot;, quindi lo trasmettiamo a Crea/Aggiorna lead come segue:

`POST /rest/v1/leads.json`

```json
{
    "input": [
        {"email":"enrique.pena-nieto@gob.mx"},
        {"email":"angelica.rivera@gob.mx"}
    ],
    "action":"createOrUpdate",
    "partitionName":"Mexico"
}
```

Ecco i nuovi lead creati in Marketo.  **Aggiorna lead nella partizione** Per aggiornare i lead esistenti in una partizione, è sufficiente chiamare Crea/Aggiorna lead e specificare partitionName come in precedenza. Per questo aggiornamento, assegneremo nome, cognome e titolo ai lead creati in precedenza.

`POST /rest/v1/leads.json`

```json
{
"input": [
        {"email":"enrique.pena-nieto@gob.mx", "firstName":"Enrique", "lastName":"Pena Neito", "title": "El Presidente"},
        {"email":"angelica.rivera@gob.mx", "firstName":"Angelica", "lastName": "Rivera", "title": "Primera Dama"}
    ],
    "action":"createOrUpdate",
    "partitionName":"Mexico"
}
```

Ecco i lead appena aggiornati in Marketo.
**Identificare la partizione per un lead** Come è possibile sapere in quale partizione si trova un lead? Per questo utilizziamo l&#39;API [Get Lead by Id](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadByIdUsingGET) e specifichiamo &quot;leadPartitionId&quot; nel parametro di query &quot;fields&quot;. In questo caso, recupereremo le informazioni per il 318816 ID lead creato in precedenza.

`GET /rest/v1/lead/318816.json?fields=leadPartitionId,email,firstName,lastName,title`

```json
{
    "requestId": "5c57#14f9a495b1f",
    "result": [
        {
            "id": 318816,
            "lastName": "Pena Neito",
            "title": "El Presidente",
            "email": "enrique.pena-nieto@gob.mx",
            "firstName": "Enrique",
            "leadPartitionId": 2
        }
    ],
    "success": true
}
```

Viene restituito leadPartitionId anziché partitionName. Per ottenere il partitionName è necessario rivedere l’output dall’API Get Lead Partitions dall’alto.

```json
{
    "id": 2,
    "name": "Mexico",
    "description": "Leads that live in Mexico"
},
```

In questo caso, un valore leadPartitionId pari a 2 corrisponde al partitionName di &quot;Mexico&quot;. È tutto per ora. Buon partizionamento!

Pubblicato il _2015-09-04_ da _David_

## Confronto dei campi punteggio nello script e-mail di Marketo

**Nota:** Questo è un post di Cathal Moran. Cathal è un Solutions Consultant che lavora presso l&#39;ufficio EMEA di Marketo a Dublino, Irlanda.

### Confronto dei campi Punteggio

Molti clienti Marketo, in particolare quelli che si concentrano sulle vendite incrociate, hanno più campi di punteggio e questo viene spesso utilizzato per misurare l’interesse di un lead in un particolare prodotto/area. Immaginate se vendessi mele e banane, se un piombo avesse un punteggio di 50 per le mele e 10 per le banane, sarebbe chiaro dove si trova la preferenza, non sarebbe bello se il mio contenuto riflettesse questa preferenza. Gli script e-mail possono essere utilizzati per confrontare i punteggi e personalizzare il contenuto di un’e-mail in base al punteggio più alto (o più basso) per quel particolare lead che riceve l’e-mail.

### Per confrontare 2 numeri è necessario convertire i valori dei campi in numeri interi

```
#set ($score1 = $math.toInteger(${lead.Apple_Score}))
#set ($score2 = $math.toInteger(${lead.Banana_Score}))
##check if the lead score is greater than feature score
#if($score1 >= $score2)
                ##if Apple score is greater
                #set($Interest = "Special offer on Apples")
##check is the feature score is higher
#elseif($score2 >= $score1)
                ##if Feature score is greater
                #set($Interest = "Special offer on Bananas")
#else
                ##otherwise
                #set($Interest = "Special offer on Fruit")
#end
##display the Interest as content
${Interest}
```

Nell’esempio precedente sto semplicemente personalizzando il testo, ma potrei altrettanto facilmente, ad esempio, visualizzare un’immagine diversa in base al punteggio più alto.

Pubblicato il _2015-09-14_ da _Kenny_

## Invio di un Marketo Form in background

Quando la tua organizzazione dispone di diverse piattaforme per l’hosting di contenuti web e dati dei clienti, diventa abbastanza comune richiedere invii paralleli da un modulo, in modo che i dati risultanti possano essere raccolti in piattaforme separate. Esistono diverse strategie per farlo, ma quella migliore è spesso la più semplice: utilizzare l’API Forms 2 per inviare un modulo Marketo nascosto. Questa opzione funziona con qualsiasi nuovo Marketo Form, ma è consigliabile creare un modulo vuoto, che non contenga campi. In questo modo il modulo non verrà caricato più dati del necessario, poiché non è necessario eseguire il rendering. Ora è sufficiente acquisire il [codice di incorporamento](https://experienceleague.adobe.com/it/docs/marketo/using/home) dal modulo e aggiungerlo al corpo della pagina desiderata, apportando una piccola modifica. Il codice di incorporamento include un elemento modulo come questo:

`<form id="mktoForm_1068"></form>`

Aggiungere &#39;style=&quot;display:none&quot;&#39; all&#39;elemento in modo che non sia visibile, come segue:

`<form id="mktoForm_1068" style="display:none"></form>`

Una volta che il modulo è incorporato e nascosto, il codice per inviarlo è davvero abbastanza semplice:

```javascript
var myForm = MktoForms2.allForms()[0];
myForm.addHiddenFields({
 //These are the values which will be submitted to Marketo
 "Email":"test@example.com",
 "FirstName":"John",
 "LastName":"Doe"
 });
myForm.submit();
```

Forms ha inviato questo metodo di comportamento esattamente come se il lead avesse compilato e inviato un modulo visibile. L’attivazione dell’invio varierà da un’implementazione all’altra poiché ogni implementazione ha un’azione diversa per richiedere l’invio, ma puoi eseguirlo praticamente su qualsiasi azione. La parte importante è impostare correttamente campi e valori. Assicurati di utilizzare il nome API SOAP dei campi che puoi trovare con [Esporta nomi campi](/help/rest-api/list-of-standard-fields.md) per garantire la corretta invio dei valori.

Migrazione dal lead associato Munchkin

L’invio di moduli in background è uno dei metodi di sostituzione consigliati per Munchkin Associate Lead. L’esempio di pagina HTML riportato di seguito illustra la migrazione ad alto livello, riutilizzando gli stessi valori inviati ad Associate Lead (Associa lead).

```html
<html>
<head>
    <!--
      Munchkin Code
      Replace with your own instance code
    -->
    <script type="text/javascript">
        (function() {
          var didInit = false;
          function initMunchkin() {
            if(didInit === false) {
              didInit = true;
              Munchkin.init('CHANGE ME');
            }
          }
          var s = document.createElement('script');
          s.type = 'text/javascript';
          s.async = true;
          s.src = '//munchkin.marketo.net/munchkin-beta.js';
          s.onreadystatechange = function() {
            if (this.readyState == 'complete' || this.readyState == 'loaded') {
              initMunchkin();
            }
          };
          s.onload = initMunchkin;
          document.getElementsByTagName('head')[0].appendChild(s);
        })();
        </script>
</head>

<body>
  <!--
    Start Embed code.
    Pasted from Form Actions -> Embed Code except for addition of 'style="display:none"' to the form tag in order to hide it, and instance-specific codes redacted
    Replace with your own code for testing
  -->
  <script src="CHANGE ME"></script>
  <form id="CHANGE ME" style="display:none"></form>
  <script>MktoForms2.loadForm("CHANGE ME", "CHANGE ME", "CHANGE ME TO AN INTEGER ID");</script>
  <!--End Embed Code-->

  <!--Demo code-->
    <script>
        //The same map which is assembled for the Munchkin submission can be reused for the form submission
        let values = {
            "Email": "email@example.com",
            "FirstName": "Test",
            "LastName": "Record"
        }
        //whenReady lets us apply a callback to all mkto forms on the page
        //the callback function fires whenever a form is completely loaded
        //for most use cases this will just be used to capture a reference to the form for later usage
        //submission is done in line here for demonstration only
        MktoForms2.whenReady(function(form){

            //the addHiddenFields methods lets us add arbitrary fields to the form as well as their values
            form.addHiddenFields(values);

            //submit the form
            form.submit();


        })
    </script>
</body>
</html>
```

Pubblicato il _2015-09-25_ da _Kenny_

## Eccezioni API REST di Marketo e gestione degli errori: Parte 1

L’API REST di Marketo può restituire un’eccezione o un errore che, per comodità, chiameremo semplicemente gli errori da qui in poi. Gli errori possono verificarsi a tre livelli diversi:

* HTTP, questi errori sono indicati da un codice 4xx
* Livello di risposta, questi errori sono inclusi nell’array &quot;errors&quot; della risposta JSON
* A livello di record, questi errori sono inclusi nell’array &quot;result&quot; della risposta JSON e sono indicati su base di record individuale con il campo &quot;status&quot; e l’array &quot;reason&quot;

### Errori HTTP

In circostanze operative normali, Marketo dovrebbe restituire solo due errori di stato HTTP, 413 Entità richiesta troppo grande e 414 URI richiesta troppo lungo. Entrambi possono essere recuperati recuperando l’errore, modificando la richiesta e riprovando, ma con le pratiche di codifica intelligente non dovresti mai incontrarli in modo selvaggio. Marketo restituirà 413 se il payload della richiesta supera 1 MB, o 10 MB nel caso di [lead di importazione](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Leads). Nella maggior parte degli scenari è improbabile che questi limiti vengano raggiunti, ma aggiungendo un controllo alle dimensioni della richiesta e spostando eventuali record che causano il superamento del limite in una nuova richiesta si dovrebbe evitare che qualsiasi circostanza che porti a questo errore venga restituita da qualsiasi endpoint. Verrà restituito 414 quando l’URI di una richiesta GET supera 8 KiB. Per evitarlo, confrontalo con la lunghezza della stringa di query per vedere se supera questo limite. Se la richiesta viene modificata in un metodo POST, inserisci la stringa di query come corpo della richiesta con il parametro aggiuntivo &#39;_method=GET&#39;. In questo modo viene superata la limitazione sugli URI. È raro che questo limite venga raggiunto nella maggior parte dei casi, ma è piuttosto comune quando si recuperano grandi batch di record con valori di filtro singoli lunghi, ad esempio un GUID.

### Errori a livello di risposta

Gli errori a livello di risposta sono presenti quando il parametro &quot;success&quot; della risposta è impostato su false. Ogni voce in &quot;errors&quot; ha due membri, &quot;code&quot; un numero, 6xx o 7xx, e un &quot;message&quot; che fornisce il motivo plaintext dell&#39;errore. I codici 6xx indicano sempre che una richiesta non è riuscita completamente e non è stata eseguita. Un esempio è un token di accesso 601 &quot;Access token invalid&quot;, che può essere recuperato autenticando nuovamente e passando il nuovo token di accesso con la richiesta. Gli errori 7xx indicano che la richiesta non è riuscita perché non sono stati restituiti dati o perché la richiesta non era parametrizzata correttamente, ad esempio includendo una data non valida o mancando un parametro obbligatorio.

### Errori a livello di record

Gli errori a livello di record indicano che non è stato possibile completare un&#39;operazione per un singolo record, ma la richiesta stessa era valida. Questi si verificano all’interno di un singolo record nell’array &quot;result&quot; di una risposta. Il campo &quot;status&quot; di questi record sarà &quot;skipped&quot; e sarà presente un array &quot;reason&quot;. Ogni motivo contiene un membro &quot;code&quot; e un membro &quot;message&quot;. Il codice sarà sempre 1xxx e il messaggio indicherà il motivo per cui il record è stato ignorato. Un esempio potrebbe essere un caso in cui una richiesta Create/Update Leads ha &quot;action&quot; impostato su &quot;createOnly&quot;, ma esiste già un lead per una delle chiavi nei record inviati. Questo caso restituisce il codice 1005 e un messaggio di tipo &quot;Lead esiste già&quot;. Nei prossimi post, vedremo alcuni errori recuperabili e alcuni esempi su come gestirli all’interno del codice.

Pubblicato il _2015-10-09_ da _Kenny_

## Post guest: connettori SQL diretti al database di Marketo

**Questo è un post di Sumit Sarkar di Progress, Inc.**

**I connettori SQL al database Marketo** Marketo dispongono di API ben documentate per l&#39;accesso ai dati. Tuttavia, a volte le organizzazioni hanno bisogno di accesso diretto a SQL. Nei negozi Marketo, le linee di confine tra il marketing e l&#39;IT sono sempre più confuse, il che fa aumentare la domanda di connettività SQL basata su standard. Connettori SQL diretti a Marketo disponibili tramite [Progress DataDirect Cloud](https://www.progress.com/connectors/cloud-data-integration). DataDirect Cloud è un servizio di connettività dati che espone i dati Marketo attraverso standard industriali aperti per l&#39;accesso SQL in ODBC (&quot;Open Database Connectivity&quot;) o JDBC (&quot;Java Database Connectivity&quot;) e REST con OData (&quot;Open Data Protocol&quot;). Di seguito sono riportati alcuni casi d’uso comuni per la connessione di dati Marketo predefiniti tramite DataDirect Cloud:

* Individuazione e visualizzazione dei dati (Qlik, Tableau, SAP Lumira)
* Reporting aziendale (SAP Business Objects, Microstrategy, Cognos)
* Integrazione dei dati (SQL Server Integration Services - SSIS, Oracle Data Integrator - ODI, Informatica)
* Federazione dei dati (server collegato SQL Server, SDA SAP Hana, gateway di database Oracle)
* Query ad hoc (Microsoft Office, DB Visualizer, Aqua Data Studio)
* Preparazione dei dati (Alteryx, Trifecta, Paxata)

**Come funziona l&#39;accesso SQL a Marketo?**

* DataDirect Cloud crea uno schema logico per i dati esposti dalle API di integrazione di Marketo.
* DataDirect Cloud elabora le richieste SQL da client ODBC o JDBC leggeri utilizzando un motore di query elastico in tempo reale sui dati recuperati dalle API di Marketo.
* La connettività di DataDirect Cloud è diretta e in tempo reale, senza alcuna duplicazione dei dati.
* DataDirect Cloud Service astrae l&#39;API Marketo in modo che gli utenti finali non debbano preoccuparsi delle modifiche della versione API o dell&#39;utilizzo di SOAP rispetto a REST.

DataDirect ha creato questo stile di connettività alle origini dati SaaS dal 2006 a partire dal primo driver ODBC Salesforce basato sulle API dei servizi Web. **Guida introduttiva alla connessione a Marketo:**

1. [Registrati per accedere a DataDirect Cloud](https://pacific.progress.com/console/register?productName=d2c&ignoreCookie=true)
1. Fai clic su &quot;Origini dati&quot; e quindi sul pulsante &quot;+New Data Source&quot;
1. Selezionare &quot;Marketo&quot; e immettere le informazioni di connessione. Contatta l&#39;amministratore di Marketo o accedi per trovare [informazioni sulla connessione per l&#39;integrazione di SOAP](/help/soap-api/soap-api.md).
1. Fare clic sul pulsante &quot;Prova connessione&quot;. Nota che è disponibile una scheda OData per produrre OData da Marketo e ne parleremo in un post di blog futuro.
1. Fare clic su &quot;SQL Testing&quot; se si desidera esaminare lo schema Marketo esposto o eseguire query SQL di base dall&#39;interfaccia utente.
1. Fai clic su &quot;Download&quot; a sinistra e seleziona il driver DataDirect Cloud ODBC o JDBC da installare per l’applicazione e la piattaforma.
1. Una volta installato il driver ODBC o JDBC di DataDirect Cloud, è possibile collegare a Marketo qualsiasi applicazione basata su standard.

Ecco un esempio video di [connessione tramite il client ODBC DataDirect Cloud](https://www.youtube.com/watch?v=H6PHra56Iig). Di seguito sono riportati altri tutorial di DataDirect Cloud applicabili a Marketo:

* [Analisi dati SAP](http://scn.sap.com/community/lumira/blog/2015/08/05/connect-sap-lumira-to-eloqua-marketo-google-analytics)
* [Generazione di rapporti aziendali per microstrategia](https://community.microstrategy.com/t5/Tech-Corner/What-MSTR-developers-should-know-about-Cloud-Data-Sources/ba-p/253083)
* [Integrazione dei dati di Oracle](https://www.ateam-oracle.com/post/a-universal-cloud-applications-adapter-for-odi/)

**La creazione della connettività SQL tra le origini cloud, ad esempio Marketo**, è problematica per R&amp;S

Non tutte le API SaaS espongono un linguaggio di query standard. In questi casi, il team tecnico esamina ogni oggetto singolarmente. Ogni oggetto può essere esposto con un’API diversa con regole univoche per richiamare, cercare filtri, ecc. È stato necessario uno sforzo significativo per fornire un’esperienza standard con cui eseguire query sull’intero modello di dati.

Gestione delle funzionalità di join completo. Nei casi in cui le API SaaS non supportano un linguaggio di query con funzionalità JOIN, il team di progettazione deve eseguire tale operazione. Questo richiede una traduzione da SQL per chiamare in modo efficiente le API Marketo per restituire la quantità minima di dati prima di eseguire l’unione. Quando si uniscono due oggetti di grandi dimensioni, il livello di accesso ai dati può utilizzare risorse considerevoli sul server applicazioni o sul desktop. Pertanto, la distribuzione del livello di accesso ai dati a un servizio cloud elastico come DataDirect Cloud ha molto senso per due motivi:

1. Prestazioni più veloci e utilizzo di meno risorse di memoria/CPU sul server applicazioni client o sul desktop
1. Sfrutta la larghezza di banda superiore tra DataDirect Cloud e Marketo, dove vengono scambiati set di dati pre-uniti.

Come si gestiscono i modelli di dati? È statico o dinamico? Come vengono rilevate e comunicate le modifiche al client? Ogni origine dati SaaS è diversa e nel caso di Marketo, alcuni oggetti vengono interrogati meglio tramite viste e altri tramite tabelle. La gestione di questa matrice di modelli di dati e oggetti tra tutte le origini SaaS è stata certamente una sfida.

**Riferimento Marketo e DataDirect Cloud per sviluppatori:**

* Proprietà connessione Marketo ([collegamento a documenti](https://www.progress.com/output/DataDirect/DataDirectCloud/index.html))
* SQL ed estensioni supportate con Marketo ([collegamento alla documentazione](https://www.progress.com/output/DataDirect/DataDirectCloud/index.html,-hubspot,-and-marketo.html))
* Tabelle e visualizzazioni di Marketo esposte ([collegamento alla documentazione](https://www.progress.com/output/DataDirect/DataDirectCloud/index.html))
* Messaggi di errore comuni restituiti da Marketo ([collegamento alla documentazione](https://www.progress.com/output/DataDirect/DataDirectCloud/index.html))

**Biografia:** Sumit Sarkar è Chief Data Evangelist di Progress, con oltre 10 anni di esperienza nel campo della connettività dati. Il consulente leader a livello mondiale sulla connettività degli standard open data con i dati cloud, gli interessi di Sumit includono il tuning delle prestazioni del livello di accesso ai dati per il quale ha sviluppato una tecnologia in attesa di brevetto per la sua analisi; business intelligence e data warehousing per le piattaforme SaaS; e la connettività dei dati per gli ambienti PaaS, con un focus su standard come ODBC, JDBC, ADO.NET e ODATA. È un consulente certificato IBM per IBM Cognos Business Intelligence e un membro di TDWI. Ha presentato sessioni sulla connettività dei dati in varie conferenze tra cui Dreamforce, Oracle OpenWorld, Strata Hadoop, MongoDB World e SAP Analytics and Business Objects Conference, tra molti altri. Twitter: @SAsInSumit LinkedIn: [www.linkedin.com/in/meetsumit](http://www.linkedin.com/in/meetsumit)

Pubblicato il _2015-12-07_ da _Kenny_

## Creare un dashboard per l’utilizzo delle API e i conteggi di errori

In qualità di consumatore di API Marketo, queste sono informazioni utili che devi tenere d’occhio. E se potessi ottenere dati storici sull’utilizzo per rilevare le tendenze nel tempo? E se fosse possibile ottenere un riepilogo dei codici di errore API per misurare lo stato dell’integrazione? In qualità di partner tecnologico di Marketo, cosa succede se è possibile ottenere dati di utilizzo ed errore in tutti gli account cliente in un&#39;unica dashboard? Questo post ti offrirà un approccio per rispondere alle domande precedenti. Allacciati, ci siamo!
**Processo pianificato per recupero statistiche** Creiamo un&#39;app che recuperi i i dati di utilizzo ed errore utilizzando gli endpoint [Get Daily Usage](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLast7DaysErrorsUsingGET) e [Get Daily Errors](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getDailyErrorsUsingGET). L’app è progettata per essere pianificata per essere eseguita una volta al giorno. Ogni volta che l’app viene eseguita, aggiunge i dati di utilizzo relativi a un giorno a un file e i dati di errore relativi a un giorno a un altro file. All&#39;inizio di ogni mese viene creata una nuova coppia di file. Questi file fungeranno da record cronologico a cui possiamo accedere in qualsiasi momento. Logica dell’app ...

* Leggi le informazioni sull’account Marketo (Munchkin ID e credenziali client) da un’origine esterna. Nota: questa origine deve essere protetta per impedire ad altri utenti di accedere ai dati dell&#39;account.
* Effettua iterazioni tramite ogni account e...
   * Chiamata per ottenere l&#39;utilizzo giornaliero per recuperare i dati di utilizzo per un giorno
   * Aggiungere i dati di utilizzo giornalieri a un file di utilizzo mensile
   * Richiama Recupera errori giornalieri per recuperare i dati di errore per un giorno
   * Aggiungi dati di errori giornalieri a un file di &quot;errori&quot; mensile

Formato del file di output Il formato dei file di output è JSON che corrisponde all’array &quot;result&quot; restituito dalle rispettive chiamate API (Utilizzo ed Errore). Ogni elemento dell’array &quot;result&quot; è un oggetto JSON che contiene i dati per un giorno. Denominazione file di output I file di output sono denominati come segue:

`<type>_<yyyy>_<mm>_<account>.json`

Dove:

`<type>` - Tipo di dati (&quot;utilizzo&quot; o &quot;errori&quot;) `<yyyy>` - Anno (4 cifre) `<mm>` - Mese (2 cifre) `<account>` - ID account (Munchkin id)

Esempi di file di output usage_2015_10_111-AAA-222.json

```json
[
    { "date": "2015-10-15", "total": 0, "users" : [] },
    { "date": "2015-10-16", "total": 9, "users": [ { "userId": "some.body@yahoo.com", "count": 9 } ] },
    { "date": "2015-10-17", "total": 1120, "users": [ { "userId": "some.body@yahoo.com", "count": 200 }, { "userId": "some.body@marketo.com", "count": 200 }, { "userId": "some.body@gmail.com", "count": 720 } ] },
]
```

`errors_2015_10_111-AAA-222.json:`

```json
[
    { "date": "2015-10-15", "total":80, "errors": [ { "errorCode":"1003", "count":80 } ] },
    { "date": "2015-10-16", "total":148, "errors": [ { "errorCode":"612", "count":40 }, { "errorCode":"609", "count":70 }, { "errorCode":"1008", "count":38 } ] },
    { "date": "2015-10-17", "total":73, "errors": [ { "errorCode":"604", "count":1 }, { "errorCode":"609", "count":56 }, { "errorCode":"610", "count":16 } ] },
]
```

Il codice per questa app è presentato alla fine di questo post (Stats.java). **Servizio Web Stats** Ora abbiamo bisogno di un modo per ottenere questi dati nel nostro browser. La proposta è quella di creare un servizio web per distribuire i dati. Il servizio web utilizzerà i file di output dell’app e quindi restituirà i dati al browser in un formato che possa essere facilmente presentato. Per semplicità e per aggirare le restrizioni dei criteri per la stessa origine, sfrutteremo il pattern [JSONP](https://en.wikipedia.org/wiki/JSONP). Specifiche dell&#39;endpoint REST proposte per il servizio Web esterno: **URI**: /stats **Metodo**: GET

**Parametro**

**Descrizione**

**Esempio**

mese

Recupera i dati per questo mese. Elenco di mesi separati da virgole da includere (rappresentazione a 2 cifre). Impostazione predefinita per tutti i mesi.

10,11

anno

Recupera i dati per quest’anno. Elenco degli anni da includere, separati da virgole (rappresentazione a 4 cifre). Impostazione predefinita per tutti gli anni.

2015

account

Recupera i dati per questo account (Munchkin id).

111-AAA-222

callback

Nome della funzione con cui racchiudere il contenuto JSON.

processStats

Esempio di richiesta

`GET //localhost:8080/stats?month=10&year=2015&account=111-AAA-222&callback=processStats`

Il servizio web legge i file di &quot;utilizzo&quot; ed &quot;errore&quot;, li combina e li restituisce in questo formato:

```html
**<Name of Callback here>**

<Contents of Usage file here>,

<Contents of Error file here>
```

Questo è un callback JSONP con 2 argomenti. Il primo argomento è la matrice &quot;result&quot; dell’utilizzo. Il secondo argomento è la matrice &quot;result&quot; degli errori. Esempio di risposta

```json
processStats(
    [
        { "date": "2015-10-15", "total": 0, "users" : [] },
        { "date": "2015-10-16", "total": 9, "users": [ { "userId": "some.body@yahoo.com", "count": 9 } ] },
        { "date": "2015-10-17", "total": 1120, "users": [ { "userId": "some.body@yahoo.com", "count": 200 }, { "userId": "some.body@marketo.com", "count": 200 }, { "userId": "some.body@gmail.com", "count": 720 } ] }
    ],
    [
        { "date": "2015-10-15", "total":80, "errors": [ { "errorCode":"1003", "count":80 } ] },
        { "date": "2015-10-16", "total":148, "errors": [ { "errorCode":"612", "count":40 }, { "errorCode":"609", "count":70 }, { "errorCode":"1008", "count":38 } ] },
        { "date": "2015-10-17", "total":73, "errors": [ { "errorCode":"604", "count":1 }, { "errorCode":"609", "count":56 }, { "errorCode":"610", "count":16 } ] },
    ]
);
```

Come puoi vedere, il servizio web ha semplicemente racchiuso i contenuti dei due file di output dalla nostra app. È stata creata una risposta fittizia del servizio Web utilizzando [Mocky](https://designer.mocky.io). Un esempio del servizio Web il cui modello è [.](http://www.mocky.io/v2/5627b2f9270000f2226eec63?month=10&year=2015&account=111-AAA-222&callback=processStats) La creazione di questo servizio Web viene lasciata come esercizio per il lettore: **Pagina Web del dashboard** Ora è sufficiente una pagina Web che chiami il servizio Web e formatta i dati. Per utilizzare il pattern JSONP è sufficiente aggiungere un tag `<script>` che richiami il servizio Web:

`<script src="http: //<hostname>/stats?month=10&year=2015&account=284-RPR-133&callback=processStats"></script>`

In questo modo il corpo della risposta del servizio Web verrà inserito direttamente nella pagina HTML. Viene quindi aggiunta la funzione di callback JSONP:

```javascript
function processStats(usage, errors) {
    var cfg = { maxDepth: 5};
    document.write("<h2>Usage</h2>");
    document.body.appendChild(prettyPrint(usage, cfg));
    document.write("<h2>Errors</h2>");
    document.body.appendChild(prettyPrint(errors, cfg));
;
```

Questa funzione viene chiamata automaticamente dopo la chiamata del servizio web. In questo esempio viene chiamato un semplice &quot;dumper variabile&quot; di JavaScript denominato [prettyPrint.js](https://github.com/padolsey-archive/prettyprint.js/tree/master) su ogni array. La funzione prettyPrint genera semplicemente una tabella HTML utilizzando il contenuto dell&#39;array. Ecco una schermata delle tabelle di HTML. Voilà, questo è il nostro cruscotto! Concesso questo non è molto elegante, ma dovrebbe dare un&#39;idea di ciò che è possibile. Non c’è nulla che vi impedisca di trasformare i dati in qualsiasi modo vogliate creare visualizzazioni accattivanti. La pagina HTML è la seguente (Index.html) Puoi visualizzare le tabelle di cui sopra in tempo reale nel browser seguendo la procedura riportata di seguito:

1. Salvare una copia locale di Index.html
1. Salva una copia locale di prettyPrint.js
1. Apri Index.html nel browser

Tutto qui. Speriamo che questo post ti abbia dato qualche idea sul monitoraggio delle statistiche delle API di Marketo. Buon codice! **Statistiche.java**

```java
package com.marketo;

// minimal-json library (https://github.com/ralfstx/minimal-json)
import com.eclipsesource.json.JsonArray;
import com.eclipsesource.json.JsonObject;

import java.io.\*;
import java.lang.reflect.Array;
import java.net.MalformedURLException;
import java.net.URL;
import java.util.\*;

import java.nio.file.Files;
import java.nio.file.Paths;

import javax.net.ssl.HttpsURLConnection;

public class Stats {
    //
    // Define Marketo instance meta data here. Each row contains three elements: Account Id, Client Id, Client Secret.
    //
    // Note: that this information would typically be stored read from an external source (i.e. database)
    //
    // For example:
    //
    // private static String final CUSTOM_SERVICE_DATA[][] = {
    // {"111-AAA-222", "2f4a4435-f6fa-4bd9-3248-098754982345", "asdf6IVE9h4Jjcl59cOMAKFSk78ut12W"},
    // {"222-BBB-333", "5f4a6657-f6fa-4cd9-4356-123083238821", "gfjgfIVE9h4Jjcl59cOMAKFSk78ut12W"},
    // {"444-CCC-444", "9f4a4678-f6fa-4dd9-7735-908713247721", "xzcxvbVE9h4Jjcl59cOMAKFSk78ut12W"}
    // };
    //
    private static final String CUSTOM_SERVICE_DATA[][] = {
    };

    // Output directory for stats files
    private static final String OUTPUT_DIR = "C:stats";

public static void main(String[] args) {

    // Loop through each Marketo instance
    for (int i = 0; i < CUSTOM_SERVICE_DATA.length; i++) {

        // Compose base URL
        String baseUrl = String.format("https://%s.mktorest.com",
            CUSTOM_SERVICE_DATA[i][0]);

        // Compose Identity URL
        String identityUrl = String.format("%s/identity/oauth/token?grant_type=%s&client_id=%s&client_secret=%s",
            baseUrl, "client_credentials", CUSTOM_SERVICE_DATA[i][1], CUSTOM_SERVICE_DATA[i][2]);

        // Call Identity API
        JsonObject identityObj = JsonObject.readFrom(httpRequest("GET", identityUrl, null));
        String accessToken = identityObj.get("access_token").asString();

        // Compose Get Last 7 Days Usage URL
        String usageUrl = String.format("%s/rest/v1/stats/usage/last7days.json?access_token=%s",
            baseUrl, accessToken);

        // Compose Get Last 7 Days Errors URL
        String errorsUrl = String.format("%s/rest/v1/stats/errors/last7days.json?access_token=%s",
            baseUrl, accessToken);

        // Process usage data
        JsonObject usageObj = JsonObject.readFrom(httpRequest("GET", usageUrl, null));
        if (usageObj.get("success").asBoolean()) {
            if (usageObj.get("result") != null) {
                updateFile(usageObj, "usage", CUSTOM_SERVICE_DATA[i][0]);
            }
        }

        // Process errors data
        JsonObject errorsObj = JsonObject.readFrom(httpRequest("GET", errorsUrl, null));
        if (usageObj.get("success").asBoolean()) {
            if (errorsObj.get("result") != null) {
                updateFile(errorsObj, "errors", CUSTOM_SERVICE_DATA[i][0]);
            }
        }
    }
    System.exit(0);
}

// Write yesterday's data to output file
private static void updateFile(JsonObject usageObj, String statsType, String account){

    JsonArray usageResultAry = usageObj.get("result").asArray();
    JsonObject yesterdayUsageResultObj = usageResultAry.get(1).asObject();

    String yesterdayDate = yesterdayUsageResultObj.get("date").asString();
    String[] yesterdayDateAry = yesterdayDate.split("[-]+");

    // "C:statsstats_yyyy_mm_account.json"
    String statsFile = String.format("%s%s_%s_%s_%s.json",
        OUTPUT_DIR, statsType, yesterdayDateAry[0], yesterdayDateAry[1], account);

    // Create file
    File file = new File(statsFile);
    try {
        if (file.createNewFile()) {
            // created new file, seed with empty array
            FileWriter fw = new FileWriter(file.getAbsoluteFile());
            BufferedWriter bw = new BufferedWriter(fw);
            bw.write("[n]");
            bw.close();
        }
    } catch (IOException e) {
        e.printStackTrace();
    }

    // Read file
    String content = null;
    try {
        content = new String(Files.readAllBytes(Paths.get(statsFile)));
    } catch (IOException e) {
        e.printStackTrace();
    }

    // Remove trailing "]", append new record, append trailing "]"
    content = content.substring(0, content.length() - 1);
    content += yesterdayUsageResultObj.toString();
    content += "n]";

    // Write file
    FileWriter fw = null;
    try {
        fw = new FileWriter(file.getAbsoluteFile());
        BufferedWriter bw = new BufferedWriter(fw);
        bw.write(content);
        bw.close();
    } catch (IOException e) {
        e.printStackTrace();
    }
}

// Perform HTTP request
private static String httpRequest(String method, String endpoint, String body) {
    String data = "";
    try {
        URL url = new URL(endpoint);
        HttpsURLConnection urlConn = (HttpsURLConnection) url.openConnection();
        urlConn.setDoOutput(true);
        urlConn.setRequestMethod(method);
        switch (method) {
            case "GET":
                break;
            case "POST":
                urlConn.setRequestProperty("Content-type", "application/json");
                urlConn.setRequestProperty("accept", "text/json");
                OutputStreamWriter wr = new OutputStreamWriter(urlConn.getOutputStream());
                wr.write(body);
                wr.flush();
                break;
            default:
                System.out.println("Error: Invalid method.");
                return data;
        }
        int responseCode = urlConn.getResponseCode();
        if (responseCode == 200) {
            InputStream inStream = urlConn.getInputStream();
            data = convertStreamToString(inStream);
        } else {
            System.out.println(responseCode);
            data = "Status:" + responseCode;
        }
    } catch (MalformedURLException e) {
        System.out.println("URL not valid.");
    } catch (IOException e) {
        System.out.println("IOException: " + e.getMessage());
        e.printStackTrace();
    }
    return data;
}

private static String convertStreamToString(InputStream inputStream) {
    try {
        return new Scanner(inputStream).useDelimiter("A").next();
    } catch (NoSuchElementException e) {
        return "";
    }
}
}
```

**Indice.html**

```html
<html>
<head>
<title>Marketo API Stats</title>
<!-- Browser JavaScript variable dumper                  -->
<!-- https://github.com/padolsey-archive/prettyprint.js  -->
<script src="prettyPrint.js"></script>
</head>
<body>

<h1>Marketo API Stats</h1>

<script>
// JSONP callback that uses prettyPrint to format API stats
function processStats(usage, errors) {
    var cfg = { maxDepth: 5};
    document.write("<h2>Usage</h2>");
    document.body.appendChild(prettyPrint(usage, cfg));
    document.write("<h2>Errors</h2>");
    document.body.appendChild(prettyPrint(errors, cfg));
};
</script>

<!-- Web service for you to implement as an exercise                                                        -->
<!-- <script src="http://localhost:8080/stats?month=10&account=111-AAA-222&callback=processStats"></script> -->

<!-- Mock web service that returns sample payload -->
<!-- http://www.mocky.io/                         -->
<script src="http://www.mocky.io/v2/5627b2f9270000f2226eec63?month=10&year=2015&account=111-AAA-222&callback=processStats"></script>"
</body>
</html>
```

Pubblicato il _2015-10-22_ da _David_

## Eccezioni API REST di Marketo e gestione degli errori: Parte 3

Nella maggior parte dei casi, gli errori ricevuti dall’API REST di Marketo non saranno recuperabili automaticamente. Tuttavia, in alcuni casi è possibile ripristinare automaticamente o assicurarsi di non visualizzare mai un determinato tipo di errore.

### Errori di dimensione richiesta

Come abbiamo visto nell&#39;ultimo post di questa serie, Marketo emetterà il codice di stato HTTP 414 se l&#39;URI supera gli 8 KiB di lunghezza, o 413 se il corpo della richiesta supera 1 MB, o 10 MB per l&#39;importazione di lead. Sebbene i 414 siano rari, è possibile che vengano visualizzati se si utilizza il tipo di filtro Recupera lead per per richiedere record basati su 300 GUID separati o criteri simili. Si supponga di disporre della seguente richiesta: `<https://AAA-BBB-CCC.mktorest.com/rest/v1/leads.json?filterType=customGUID&fields=email`, company...firstName, lastName> Quando si invia la richiesta, Marketo restituisce lo stato 414 perché l&#39;URI supera gli 8 KiB.

Per gestire questo problema, è necessario modificare il pattern di questa richiesta e inviare un POST invece di un GET, aggiungere &quot;_method=GET&quot; all’URI e passare la stringa di query nel corpo della richiesta come richiesta x-www-form-urlencoded: URI: `<https://AAA-BBB-CCC.mktorest.com/rest/v1/leads.json?_method=GET>` Corpo richiesta: filterType=customGUID&amp;fields=email,company..firstName, lastName Invece di intercettare questa eccezione dalla risposta HTTP, è possibile controllare la lunghezza totale della richiesta in fase di esecuzione e distribuire questo pattern alternativo se l’URI supera gli 8 KB. In alternativa, è possibile utilizzare il metodo POST in tutti i casi per il recupero batch di record. Per i modelli 413, è possibile seguire un pattern simile, verificando la lunghezza del corpo della richiesta quando si aggiungono record durante il passaggio di serializzazione e suddividendo la richiesta in più parti in caso di superamento di questo limite.

### Errori di autenticazione

La nostra prossima classe di errori recuperabili è relativa all&#39;autenticazione. Quando si utilizza un token di accesso precedentemente valido dopo la scadenza del relativo periodo expires_in, il primo utilizzo restituirà un codice di errore 602, &quot;Token di accesso scaduto&quot;. In seguito, l’utilizzo dello stesso token restituirà il messaggio 601 &quot;Token di accesso non valido&quot;. Qualsiasi altro utilizzo di una stringa che non è un token di accesso valido per la sottoscrizione di destinazione darà come risultato un 601. In entrambi i casi, è possibile recuperare l’errore effettuando di nuovo l’autenticazione e passando il nuovo token di accesso con un nuovo tentativo di richiesta non riuscita.

### Timeout

In circostanze molto rare, una chiamata può restituire un 604, &quot;Richiesta scaduta&quot;, dopo la scadenza del periodo di timeout di 30 secondi. Per le richieste in batch, ad esempio Crea/aggiorna lead, la richiesta può essere divisa in batch più piccoli e ritentata fino alla restituzione del risultato positivo (se il batch viene suddiviso in meno di 100 record e la richiesta è ancora scaduta, è probabilmente necessario archiviare un caso di supporto). L&#39;altro caso più comune riguarda le chiamate di approvazione delle risorse, in cui un blocco può essere mantenuto sul record approvato corrente da un altro utente o servizio, ad esempio il caso di un [E-mail](https://developer.adobe.com/marketo-apis/api/asset/approve-email-by-id/) o di un [Modello e-mail](https://developer.adobe.com/marketo-apis/api/asset/approve-email-template-by-id/). In questi casi, [backoff esponenziale](https://en.wikipedia.org/wiki/Exponential_backoff) deve essere utilizzato per i nuovi tentativi per consentire la risoluzione di eventuali blocchi esistenti. Controlla di nuovo nelle prossime settimane la parte finale della serie dove vedremo alcuni errori specifici e non recuperabili.

Pubblicato il _2015-10-30_ da _Kenny_

## Miglioramenti alla sicurezza di Marketo

Marketo prende sul serio la sicurezza. Nell&#39;ambito di una **[iniziativa a livello di settore](https://security.googleblog.com/2014/09/gradually-sunsetting-sha-1.html)**, Marketo sta aggiornando l&#39;autenticazione e la crittografia Web per migliorare le protezioni di sicurezza. L’implementazione è pianificata per il 12 dicembre 2011. **Chi sarà interessato?** Il problema riguarda solo un numero limitato di utenti, solo quelli che hanno un&#39;integrazione con Marketo da un sistema che ha più di dieci anni e non è stato aggiornato di recente. Consulta **[questo elenco](https://www.digicert.com/faq/sha2/sha-2-compatibility)** per ulteriori informazioni sui sistemi e sulle versioni supportati. **I seguenti utenti non saranno interessati:**

* Utenti finali che accedono a Marketo.com tramite browser moderni ([vedi elenco](https://www.digicert.com/faq/sha2/sha-2-compatibility))
* Clienti che utilizzano partner di integrazione come Informatica, Dell Boomi e Scribe.
* Clienti che utilizzano i partner Launchpoint.

Tutti gli altri domini Marketo utilizzano già certificati SHA2.

Pubblicato il _2015-11-18_ da _Kenny_

## Polling per attività tramite API REST

Le attività sono un oggetto principale nella piattaforma Marketo. Le attività sono i dati comportamentali memorizzati per ogni visita a una pagina web, e-mail aperta, partecipazione a un webinar o partecipazione a una fiera. Un caso d’uso comune è la combinazione di dati di attività con dati provenienti da altre parti di un’organizzazione. Questo programma di esempio contiene 6 passaggi:

1. Chiama [Ottieni attività lead](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadActivitiesUsingGET) per generare un elenco di tutti i record di attività creati in una data/ora specificata. Utilizziamo un filtro per limitare il tipo di record di attività restituiti.
1. Estrai i campi di interesse da ciascun record di attività.
1. Chiamare [Get Multiple Leads by Filter Type](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET) per generare un elenco di record di lead corrispondenti alle attività del passaggio 1. Come filtro, utilizziamo il campo leadId estratto dal record di attività nel passaggio 2 per specificare quali lead vengono restituiti.
1. Estrarre i campi di interesse da ogni record di lead.
1. Unisci i dati dell’attività del passaggio 2 con i dati del lead del passaggio 4.
1. Trasforma i dati del passaggio 5 in un formato utilizzabile da un sistema esterno.

Come mostra il diagramma seguente, per questo esempio abbiamo scelto di acquisire le attività relative alle e-mail.

**Input programma** Per impostazione predefinita il programma torna indietro nel tempo di un giorno dalla data corrente per cercare le modifiche. Ad esempio, è possibile eseguire questo programma ogni giorno alla stessa ora. Per tornare più indietro nel tempo, è possibile specificare il numero di giorni come argomento della riga di comando, aumentando di fatto la finestra di tempo. Il programma contiene diverse variabili che è possibile modificare: CUSTOM_SERVICE_DATA - Questo contiene i dati del servizio personalizzato Marketo (ID account, ID client, segreto client). READ_BATCH_SIZE: numero di record da recuperare alla volta. Utilizzare questa opzione per regolare la risposta in base alle dimensioni del corpo. LEAD_FIELDS: contiene un elenco di campi lead che si desidera raccogliere. ACTIVITY_TYPES: contiene un elenco dei tipi di attività che si desidera raccogliere.

**Logica del programma** Innanzitutto stabiliamo l&#39;intervallo di tempo, componiamo gli URL dell&#39;endpoint REST e otteniamo il token di accesso per l&#39;autenticazione. Successivamente viene attivato un ciclo Get Paging Token/Get Lead Activities che viene eseguito fino a esaurimento della fornitura di attività. Lo scopo di questo loop è quello di recuperare i record di attività ed estrarre i campi di interesse da tali record. Diciamo a Get Lead Activities di cercare solo i seguenti tipi di attività:

* E-mail consegnata
* Annulla iscrizione e-mail
* Apri e-mail
* Fai clic su E-mail.

Estraggiamo i seguenti campi di interesse da ciascun record di attività:

* leadId
* activityType
* activityDate
* primaryAttributeValue

Puoi selezionare qualsiasi combinazione di tipi di attività e campi di attività in base allo scopo. Successivamente viene attivato un loop Get Multiple Leads by Filter Type che viene eseguito fino a quando non esauriamo la fornitura di lead. Tieni presente che utilizziamo il parametro &quot;filterType=id&quot; in combinazione con una serie di parametri &quot;filterValues&quot; per limitare i record dei lead recuperati solo ai lead associati alle attività recuperate in precedenza. Estraggiamo i seguenti campi di interesse da ciascun record di lead:

* firstName
* lastName
* e-mail

Anche in questo caso, si consiglia di selezionare qualsiasi campo di lead desiderato. Ora uniamo i campi lead con i campi attività utilizzando l’ID lead per collegarli. Infine, eseguiamo un ciclo tra tutti i dati, li trasformiamo in formato JSON e li stampiamo nella console. **Output programma** Ecco un esempio di output del programma di esempio. Mostra i campi attività e i campi lead combinati come oggetti &quot;risultato&quot; JSON. L’idea qui è di poter trasmettere questo JSON come payload di richiesta a un servizio web esterno.

```json
{
    "result": [
        {
            "leadId": 318581,
            "activityType": "Email Delivered",
            "activityDate": "2015-03-17T20:00:06Z",
            "primaryAttributeValue": "My Email Program",
            "firstName": "David",
            "lastName": "Everly",
            "email": "everlyd@marketo.com"
        },
        {
            "leadId":318581,
            "activityType":"Open Email",
            "activityDate":"2015-03-17T23:23:12Z",
            "primaryAttributeValue":"My Email Program - Auto Response",
            "firstName":"David",
            "lastName":"Everly",
            "email":"everlyd@marketo.com"
       },
        ... more result objects here...
    ]
}
```

**Codice programma**

```java
package com.marketo;

// minimal-json library (https://github.com/ralfstx/minimal-json)
import com.eclipsesource.json.JsonArray;
import com.eclipsesource.json.JsonObject;
import com.eclipsesource.json.JsonValue;

import java.io.\*;
import java.net.MalformedURLException;
import java.net.URL;
import java.text.DateFormat;
import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.\*;

import javax.net.ssl.HttpsURLConnection;

public class LeadActivities {
//
// Define Marketo REST API access credentials: Account Id, Client Id, Client Secret.  For example:
    private static Map<String, String> CUSTOM_SERVICE_DATA;
    static {
        CUSTOM_SERVICE_DATA = new HashMap<String, String>();
//        CUSTOM_SERVICE_DATA.put("accountId", "111-AAA-222");
//        CUSTOM_SERVICE_DATA.put("clientId", "2f4a4435-f6fa-4bd9-3248-098754982345");
//        CUSTOM_SERVICE_DATA.put("clientSecret", "asdf6IVE9h4Jjcl59cOMAKFSk78ut12W");
    }

    // Number of lead records to read at a time
    private static final String READ_BATCH_SIZE = "200";

    // Lookup lead records using lead id as primary key
    private static final String LEAD_FILTER_TYPE = "id";

    // Lead record lookup returns these fields
    private static final String LEAD_FIELDS = "firstName,lastName,email";

    // Lookup activity records for these activity types
    private static Map<Integer, String> ACTIVITY_TYPES;
    static {
        ACTIVITY_TYPES = new HashMap<Integer, String>();
        ACTIVITY_TYPES.put(7, "Email Delivered");
        ACTIVITY_TYPES.put(9, "Unsubscribe Email");
        ACTIVITY_TYPES.put(10, "Open Email");
        ACTIVITY_TYPES.put(11, "Click Email");
    }

    public static void main(String[] args) {
        // Command line argument to set how far back to look for lead changes (number of days)
        int lookBackNumDays = 1;
        if (args.length == 1) {
            lookBackNumDays = Integer.parseInt(args[0]);
        }

        // Establish "since date" using current timestamp minus some number of days (default is 1 day)
        Calendar cal = Calendar.getInstance();
        cal.add(Calendar.DAY_OF_MONTH, -lookBackNumDays);
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd'T'HH:mm:ssZ");
        String sinceDateTime = sdf.format(cal.getTime());

        // Compose base URL
        String baseUrl = String.format("https://%s.mktorest.com",
            CUSTOM_SERVICE_DATA.get("accountId"));

        // Compose Identity URL
        String identityUrl = String.format("%s/identity/oauth/token?grant_type=%s&client_id=%s&client_secret=%s",
                baseUrl, "client_credentials", CUSTOM_SERVICE_DATA.get("clientId"), CUSTOM_SERVICE_DATA.get("clientSecret"));

        // Call Identity API
        JsonObject identityObj = JsonObject.readFrom(getData(identityUrl));
        String accessToken = identityObj.get("access_token").asString();

        // Compose URLs for Get Lead Changes, and Get Paging Token
        String activityTypesUrl = String.format("%s/rest/v1/activities/types.json?access_token=%s",
                baseUrl, accessToken);
        String pagingTokenUrl = String.format("%s/rest/v1/activities/pagingtoken.json?access_token=%s&sinceDatetime=%s",
                baseUrl, accessToken, sinceDateTime);

        // Use activity ids to create filter parameter
        String activityTypeIds = "";
        for (Integer id : ACTIVITY_TYPES.keySet()) {
            activityTypeIds += "&activityTypeIds=" + id.toString();
        }

        // Compose URL for Get Lead Activities
        // Only retrieve activities that match our defined activity types
        String leadActivitiesUrl = String.format("%s/rest/v1/activities.json?access_token=%s%s&batchSize=%s",
                baseUrl, accessToken, activityTypeIds, READ_BATCH_SIZE);

        Map<Integer, List> activitiesMap = new HashMap<Integer, List>();
        Set leadsSet = new HashSet();

        // Call Get Paging Token API
        JsonObject pagingTokenObj = JsonObject.readFrom(getData(pagingTokenUrl));
        if (pagingTokenObj.get("success").asBoolean()) {
            String nextPageToken = pagingTokenObj.get("nextPageToken").asString();
            boolean moreResult;
            do {
                moreResult = false;

                // Call Get Lead Activities API to retrieve activity data
                JsonObject leadActivitiesObj = JsonObject.readFrom(getData(String.format("%s&nextPageToken=%s",
                        leadActivitiesUrl, nextPageToken)));
                if (leadActivitiesObj.get("success").asBoolean()) {
                    moreResult = leadActivitiesObj.get("moreResult").asBoolean();
                    nextPageToken = leadActivitiesObj.get("nextPageToken").asString();

                    if (leadActivitiesObj.get("result") != null) {
                        JsonArray activitiesResultAry = leadActivitiesObj.get("result").asArray();
                        for (JsonValue activitiesResultObj : activitiesResultAry) {

                            // Extract activity fields of interest (leadID, activityType, activityDate, primaryAttributeValue)
                            JsonObject leadObj = new JsonObject();
                            int leadId = activitiesResultObj.asObject().get("leadId").asInt();
                            leadObj.add("activityType", ACTIVITY_TYPES.get(activitiesResultObj.asObject().get("activityTypeId").asInt()));
                            leadObj.add("activityDate", activitiesResultObj.asObject().get("activityDate").asString());
                            leadObj.add("primaryAttributeValue", activitiesResultObj.asObject().get("primaryAttributeValue").asString());

                            // Store JSON containing activity fields in a map using lead id as key
                            List leadLst = activitiesMap.get(leadId);
                            if (leadLst == null) {
                                activitiesMap.put(leadId, new ArrayList());
                                leadLst = activitiesMap.get(leadId);
                            }
                            leadLst.add(leadObj);

                            // Store unique lead ids for use as lead filter value below
                            leadsSet.add(leadId);
                        }
                    }
                }
            } while (moreResult);
        }

        // Use unique lead id values to create filter parameter
        String filterValues = "";
        for (Object object : leadsSet) {
            if (filterValues.length() > 0) {
                filterValues += ",";
            }
            filterValues += String.format("%s", object);
        }

        // Compose Get Multiple Leads by Filter Type URL
        // Only retrieve leads that match the list of lead ids that was accumulated during activity query
        String getMultipleLeadsUrl = String.format("%s/rest/v1/leads.json?access_token=%s&filterType=%s&fields=%s&filterValues=%s&batchSize=%s",
                baseUrl, accessToken, LEAD_FILTER_TYPE, LEAD_FIELDS, filterValues, READ_BATCH_SIZE);
        String nextPageToken = "";

        do {
            String gmlUrl = getMultipleLeadsUrl;

            // Append paging token to Get Multiple Leads by Filter Type URL
            if (nextPageToken.length() > 0) {
                gmlUrl = String.format("%s&nextPageToken=%s", getMultipleLeadsUrl, nextPageToken);
            }

            // Call Get Multiple Leads by Filter Type API to retrieve lead data
            JsonObject multipleLeadsObj = JsonObject.readFrom(getData(gmlUrl));
            if (multipleLeadsObj.get("success").asBoolean()) {
                if (multipleLeadsObj.get("result") != null) {
                    JsonArray multipleLeadsResultAry = multipleLeadsObj.get("result").asArray();

                    // Iterate through lead data
                    for (JsonValue leadResultObj : multipleLeadsResultAry) {
                        int leadId = leadResultObj.asObject().get("id").asInt();

                        // Join activity data with lead fields of interest (firstName, lastName, email)
                        List leadLst = activitiesMap.get(leadId);
                        for (JsonObject leadObj : leadLst) {
                            leadObj.add("firstName", leadResultObj.asObject().get("firstName").asString());
                            leadObj.add("lastName", leadResultObj.asObject().get("lastName").asString());
                            leadObj.add("email", leadResultObj.asObject().get("email").asString());
                        }
                    }
                }
            }

            nextPageToken = "";
            if (multipleLeadsObj.asObject().get("nextPageToken") != null) {
                nextPageToken = multipleLeadsObj.get("nextPageToken").asString();
            }
        } while (nextPageToken.length() > 0);

        // Now place activity data into an array of JSON objects
        JsonArray activitiesAry = new JsonArray();
        for (Map.Entry<Integer, List> activity : activitiesMap.entrySet()) {
            int leadId = activity.getKey();
            for (JsonObject leadObj : activity.getValue()) {
                // do something with leadId and each leadObj
                leadObj.add("leadId", leadId);
                activitiesAry.add(leadObj);
            }
        }

        // Print out result objects
        JsonObject result = new JsonObject();
        result.add("result", activitiesAry);
        System.out.println(result);

        System.exit(0);
    }

    // Perform HTTP GET request
    private static String getData(String endpoint) {
        String data = "";
        try {
            URL url = new URL(endpoint);
            HttpsURLConnection urlConn = (HttpsURLConnection) url.openConnection();
            urlConn.setRequestMethod("GET");
            urlConn.setAllowUserInteraction(false);
            urlConn.setDoOutput(true);
            int responseCode = urlConn.getResponseCode();
            if (responseCode == 200) {
                InputStream inStream = urlConn.getInputStream();
                data = convertStreamToString(inStream);
            } else {
                System.out.println(responseCode);
                data = "Status:" + responseCode;
            }
        } catch (MalformedURLException e) {
            System.out.println("URL not valid.");
        } catch (IOException e) {
            System.out.println("IOException: " + e.getMessage());
            e.printStackTrace();
        }
        return data;
    }

    private static String convertStreamToString(InputStream inputStream) {
        try {
            return new Scanner(inputStream).useDelimiter("A").next();
        } catch (NoSuchElementException e) {
            return "";
        }
    }
}
```

Tutto qui. Buon codice!

Pubblicato il _2015-11-20_ da _David_

## Integrazione del lettore SoundCloud con l&#39;API Munchkin

SoundCloud offre un&#39;incredibile piattaforma di hosting audio, con analisi e funzionalità avanzate per qualsiasi tipo di applicazione, dagli aspiranti artisti indie rock agli artisti EDM all&#39;avanguardia nel settore musicale, fino ai podcast che raccontano storie. Oltre all’incredibile funzionalità nativa della piattaforma, viene fornito un programma API di altissimo livello per spostare i dati e tenere traccia del comportamento di ascolto. Questa funzione è particolarmente utile per i podcast e può consentire di correlare specifiche azioni di ascolto, come riproduzioni, pause e condivisioni, a specifici contenuti dello script e dell’audio. Oggi vedremo come sfruttare l&#39;API widget di [SoundCloud](https://developers.soundcloud.com/docs/api/html5-widget) per inviare e tenere traccia di queste attività in Marketo. Vediamo innanzitutto come generare un’attività Munchkin che verrà registrata nel Marketo di accesso dell’attività di un lead. Alla base, effettuiamo una chiamata a Munchkin.munchkinFunction e trasmettiamo &quot;visitWebPage&quot; come primo argomento. In questo modo viene registrata un’attività Pagina web visite con Marketo e vengono registrati tutti gli URL arbitrari e i dati della stringa di query passati al metodo. Il secondo argomento accetta un oggetto JavaScript con i nostri dati, che ha due membri, &quot;url&quot; e &quot;params&quot;, entrambe le stringhe. Il membro URL corrisponde alla pagina web dell’attività in Marketo, mentre i parametri corrispondono alla stringa di query. A questo scopo, utilizzeremo l&#39;URL come identificatore per le azioni relative a SoundCloud, &quot;soundCloudInteraction&quot;, mentre i parametri conterranno dati aggiuntivi su una particolare attività. Di seguito è illustrata la funzione che utilizziamo per tenere traccia di ogni azione:

```javascript
var trackActivity = function(action){

    //set action param to be the string passed to the function
    var qs = "action=" + action;

    //use getCurrentSound callback to get the name of the current track
    soundCloudMunchkin.widget.getCurrentSound(function(currentSound){

        //add it to our querystring
        qs = qs + "&sound=" + currentSound.title;

        //use the getPosition callback to get the position of the track in ms
        soundCloudMunchkin.widget.getPosition(function(position){

            //add it to the querystring
            qs = qs + "&position=" + position;

            //assemble our data object for the munchkin activity
            var dataObject = {
                "url": "soundCloudInteraction",
                "params": qs
            }

            //call the munchkinFunction to submit the activity
            Munchkin.munchkinFunction("visitWebPage", dataObject);
        });
    });
}
```

Poiché il widget SoundCloud standard è incorporato in un iframe, il widget utilizza i messaggi post per comunicare e i callback devono essere utilizzati per ottenere i dati, come è possibile vedere con i metodi currentSound e getPosition. L&#39;API widget di SoundCloud fornisce un set di callback JavaScript che possiamo utilizzare per rispondere a singoli eventi nel lettore e inviarli a Marketo. Gli eventi a cui siamo interessati sono ciò che l’utente ascolta, per quanto tempo l’utente ascolta e le interazioni che l’utente compie con il lettore, quindi stiamo esaminando i seguenti eventi:

* PLAY
* PAUSA
* FINE
* RICERCA
* CLICK_DOWNLOAD
* CLICK_BUY
* OPEN_SHARE_PANEL

Per aggiungere callback a ciascuno di questi eventi, è inoltre necessario utilizzare il metodo bind() del widget. Vediamo un esempio:

```javascript
widget.bind(SC.Widget.Events.PLAY, function(){
    soundCloudMunchkin.trackPlay();
});
```

In questo modo, ogni volta che viene riprodotto un brano, verrà attivato il metodo trackPlay per inviare un evento a Marketo con i dati del brano corrente. Lo script completo [è disponibile qui](https://gist.github.com/kelkingtron/6750bb07c1397d93d9c7#file-soundcloudmunchkin-js). L&#39;oggetto soundCloudMunchkin dispone di un metodo init che accetta come unico argomento un oggetto widget SoundCloud, che associa i metodi di tracciamento ai callback rilevanti e imposta il widget per tenere traccia dell&#39;attività in Marketo. Sarà necessario caricare il [codice Munchkin](/help/javascript-api/lead-tracking.md) e la [libreria API SoundCloud](https://w.soundcloud.com/player/api.js) nella pagina. È inoltre necessario inizializzare tutto, oltre a incorporare il widget SoundCloud effettivo:

```javascript
window.onload=function(){
var iframe = document.getElementById(iframeId);
    if(iframe) {
        widget = SC.Widget(iframe);
        soundCloudMunchkin.init(widget);
    };
};
```

Pubblicato il _2015-12-21_ da _Kenny_

## RTP e direttiva UE sulla privacy elettronica

Questo post spiega come utilizzare l&#39;RTP per notificare ai visitatori del sito web che vengono tracciati o disabilitare automaticamente il tracciamento per i visitatori europei. Dal 2012, qualsiasi sito web disponibile ai visitatori europei deve essere conforme alla direttiva UE sulla privacy elettronica. Nel 2011 sono entrate in vigore nuove leggi che impediscono la memorizzazione di informazioni di identificazione sul computer di un utente senza la sua conoscenza e il suo consenso. Se utilizzi cookie o altre tecnologie per il tracciamento non essenziale, devi:

1. Informare gli utenti che vengono utilizzate tecnologie di tracciamento.
1. Spiegare i motivi dell’utilizzo di tali tecnologie.
1. Ottieni il consenso dell’utente prima di utilizzare quella tecnologia e consenti loro di revocare l’autorizzazione in qualsiasi momento.

Pubblicato il _1970-01-01_ da _Yanir_

## Aggiornamento delle informazioni su clienti e potenziali clienti in Marketo tramite Personalizzazione automatizzata

Esistono scenari in cui i sistemi proprietari vengono utilizzati per aggiornare le informazioni sui clienti attuali e potenziali. Il team Marketing vorrebbe che tali aggiornamenti venissero rispecchiati in Marketo in modo da disporre del sistema di registrazione più accurato da utilizzare nelle campagne di marketing. Utilizzando l’approccio seguente è possibile impostare caricamenti periodici in Marketo per mantenere aggiornate le informazioni di contatto di Marketo con i dati modificati nel sistema proprietario. Il diagramma seguente mostra le chiamate API effettuate con un timer periodico impostato. Quando viene attivato il timer periodico, la logica client recupera prima i contatti aggiornati dal sistema proprietario. Questa operazione varia da sistema a sistema utilizzando API o esportazioni di dati dal sistema proprietario. Vengono descritte in dettaglio le API di Marketo che vengono eseguite dopo il recupero delle informazioni di contatto aggiornate. Richiesta SOAP per syncMultipleLeads:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<ns2:paramsSyncMultipleLeads xmlns:ns2="<http://www.marketo.com/mktows/">
  <leadRecordList>
    <leadRecord>
      <Email>henry@superstar.com</Email>
      <ns2:leadAttributeList>
        <attribute>
          <attrName>FirstName</attrName>
          <attrValue>Henry</attrValue>
        </attribute>
        <attribute>
          <attrName>LastName</attrName>
          <attrValue>Adams</attrValue>
        </attribute>
        <attribute>
          <attrName>Title</attrName>
          <attrValue>Director of Demand Generation</attrValue>
        </attribute>
      </ns2:leadAttributeList>
    </leadRecord>
    <leadRecord>
      <Email>ssmith@gmail.com</Email>
      <ns2:leadAttributeList>
        <attribute>
          <attrName>FirstName</attrName>
          <attrValue>Suzie</attrValue>
        </attribute>
        <attribute>
          <attrName>LastName</attrName>
          <attrValue>Smith</attrValue>
        </attribute>
        <attribute>
          <attrName>Title</attrName>
          <attrValue>VP Marketing</attrValue>
        </attribute>
      </ns2:leadAttributeList>
    </leadRecord>
  </leadRecordList>
  <dedupEnabled>true</dedupEnabled>
</ns2:paramsSyncMultipleLeads>
```

Risposta di SOAP da syncMultilpeLeads:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<ns2:successSyncMultipleLeads xmlns:ns2="<http://www.marketo.com/mktows/">
  <result>
    <syncStatusList>
      <syncStatus>
        <leadId>1094593</leadId>
        <status>UPDATED</status>
        <error xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:nil="true" />
      </syncStatus>
      <syncStatus>
        <leadId>1094594</leadId>
        <status>UPDATED</status>
        <error xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:nil="true" />
      </syncStatus>
    </syncStatusList>
  </result>
</ns2:successSyncMultipleLeads>
```

`syncMultipleLeads` esegue un&#39;operazione UPSERT. Se esiste già un contatto in Marketo in base all’indirizzo e-mail inviato, gli attributi verranno aggiornati. Se un contatto non esiste, verrà creato. La risposta di `syncMultipleLeads` restituisce lo stato per ogni contatto inviato. I valori `<attrName/>` all&#39;interno di `<leadAttributeList/>` devono corrispondere al nome API di SOAP definito per la sottoscrizione di Marketo. Per scoprire i nomi API di SOAP nella sezione gestione campi del pannello di amministrazione di Marketo, esporta i nomi dei campi.

Vedi il seguente programma Java di esempio che esegue lo scenario descritto sopra:

```java
 import com.marketo.mktows.\*;
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
import java.util.\*;

public class SyncMultipleLeadsExample {

  public static void main(String[] args) {

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
      ParamsSyncMultipleLeads request = new ParamsSyncMultipleLeads();

      ObjectFactory objectFactory = new ObjectFactory();

      JAXBElement dedup = objectFactory.createParamsSyncMultipleLeadsDedupEnabled(true);
      request.setDedupEnabled(dedup);

      // The list of contacts defined here would be retrieved from the proprietary system
      Contact contact = new Contact("Henry","Adams","henry@superstar.com", "Director of Demand Generation");
      Contact contact2 = new Contact("Suzie","Smith","ssmith@gmail.com", "VP Marketing");

      ArrayList updatedContacts = new ArrayList();
      updatedContacts.add(contact);
      updatedContacts.add(contact2);

      ArrayOfLeadRecord arrayOfLeadRecords = new ArrayOfLeadRecord();

      Iterator it = updatedContacts.iterator();
      while(it.hasNext())
      {
        Contact c = it.next();

        LeadRecord leadRec = new LeadRecord();

        JAXBElement email = objectFactory.createLeadRecordEmail(c.email);
        leadRec.setEmail(email);

        Attribute attr1 = new Attribute();
        attr1.setAttrName("FirstName");
        attr1.setAttrValue(c.fname);

        Attribute attr2 = new Attribute();
        attr2.setAttrName("LastName");
        attr2.setAttrValue(c.lname);

        Attribute attr3 = new Attribute();
        attr3.setAttrName("Title");
        attr3.setAttrValue(c.title);

        ArrayOfAttribute aoa = new ArrayOfAttribute();
        aoa.getAttributes().add(attr1);
        aoa.getAttributes().add(attr2);
        aoa.getAttributes().add(attr3);

        QName qname = new QName("http://www.marketo.com/mktows/", "leadAttributeList");
        JAXBElement attrList = new JAXBElement(qname, ArrayOfAttribute.class, aoa);

        leadRec.setLeadAttributeList(attrList);
        arrayOfLeadRecords.getLeadRecords().add(leadRec);

      }

      request.setLeadRecordList(arrayOfLeadRecords);

      JAXBContext context = JAXBContext.newInstance(SuccessSyncMultipleLeads.class);
      Marshaller m = context.createMarshaller();
      m.setProperty(Marshaller.JAXB_FORMATTED_OUTPUT, true);
      m.marshal(request, System.out);

      SuccessSyncMultipleLeads result = port.syncMultipleLeads(request, header);

      m.marshal(result, System.out);
    }

    catch(Exception e) {
      e.printStackTrace();
    }
  }

  public static class Contact {
    public String fname, lname, email, title;

      public Contact(String fname, String lname, String email, String title) {
          this.fname = fname;
          this.lname = lname;
          this.email = email;
          this.title = title;
      }
  }
}
```

Questo articolo contiene il codice utilizzato per implementare integrazioni personalizzate. A causa della sua natura personalizzata, il team del supporto tecnico Marketo non è in grado di risolvere i problemi relativi al lavoro personalizzato. Non tentare di implementare il seguente esempio di codice senza un’adeguata esperienza tecnica o l’accesso a uno sviluppatore esperto.

Pubblicato il _2014-03-24_ da _Travis Kaufman_

## Invio di un’e-mail transazionale da Marketo tramite l’API

Richiede che sia creata una campagna avanzata esistente utilizzando l’interfaccia utente di Marketo. Inoltre, richiede che il destinatario dell’e-mail esista in Marketo. Prima di chiamare l&#39;API requestCampaign, utilizza l&#39;API [getLead]&#x200B;(/help/soap-api/getlead.md) per verificare se l&#39;e-mail esiste in Marketo. Dopo aver effettuato una chiamata tramite l’API requestCampaign, puoi confermarla controllando per verificare se Smart Campaign è stato eseguito in Marketo. Ti mostriamo innanzitutto come creare una campagna avanzata, poi come impostare un trigger per inviare una campagna tramite l’API, infine come definire un’e-mail come parte di un’azione di flusso e infine un esempio di codice che verrebbe utilizzato per eseguire questa campagna.
**Come creare una nuova campagna avanzata in Marketo** Le campagne avanzate in Marketo eseguono tutte le attività di marketing. Puoi impostare una serie di azioni automatizzate da eseguire su un elenco avanzato di contatti. Nel caso di invio di e-mail transazionali, imposta un trigger nella campagna, come mostrato di seguito, per inviare e-mail utilizzando l’API. Innanzitutto, configuriamo Smart Campaign. 1. In Attività di marketing, scegli un programma, quindi fai clic su Nuova risorsa locale nel menu a discesa Nuovo.

1. Fai clic su Smart Campaign
1. Immetti un nome per la campagna avanzata e fai clic su Crea

**Aggiungi trigger a una campagna avanzata** L&#39;aggiunta di trigger a una campagna avanzata consente di eseguire una campagna avanzata su una persona alla volta in base a un evento live, che in questo caso è una richiesta tramite l&#39;[API requestCampaign](https://developer.adobe.com/marketo-apis/api/mapi/#operation/triggerCampaignUsingPOST). 1. Cerca il trigger &quot;Campaign is Requested&quot; (Campagna è richiesta), quindi trascinalo sull’area di lavoro.

1. Nel trigger, seleziona &quot;è&quot; e &quot;API servizio Web&quot;.

**Come creare un&#39;azione di flusso e-mail in una campagna** L&#39;associazione di un&#39;e-mail con una campagna avanzata consente agli addetti al marketing di gestire l&#39;aspetto che deve avere un&#39;e-mail e permette all&#39;applicazione di terze parti di determinare chi la riceve e quando. Dopo aver creato un messaggio e-mail come nuova risorsa locale, puoi impostarlo come azione di flusso in una campagna.  Trova e seleziona l’e-mail che desideri inviare.

**Esempio di codice per chiamare l&#39;API requestCampaign** Dopo aver configurato la campagna e i trigger nell&#39;interfaccia di Marketo, ti mostriamo come utilizzare l&#39;API per inviare un&#39;e-mail. Il primo esempio è una richiesta XML, il secondo è una risposta XML e l&#39;ultimo è un esempio di codice Java che può essere utilizzato per generare la richiesta XML. Viene inoltre illustrato come trovare l&#39;ID campagna utilizzato quando si effettua una chiamata all&#39;API `requestCampaign`.
La chiamata API richiede inoltre di conoscere in anticipo l’ID della campagna Marketo. Puoi determinare l’ID della campagna utilizzando uno dei seguenti metodi: 1. Utilizza l&#39;API 1 [getCampaignsForSource](/help/soap-api/getcampaignsforsource.md). Apri la campagna Marketo in un browser e osserva la barra degli indirizzi URL. L’ID della campagna (rappresentato da un numero intero di 4 cifre) si trova immediatamente dopo &quot;SC&quot;. Ad esempio, `<https://app-stage.marketo.com/#SC**1025**A1>`. La parte in grassetto è l’ID della campagna &quot;1025&quot;. Richiesta SOAP per `requestCampaign`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:ns1="<http://www.marketo.com/mktows/">
  <SOAP-ENV:Header>
    <ns1:AuthenticationHeader>
      <mktowsUserId>demo17_1_809939944BFABAE58E5D27</mktowsUserId><requestSignature>48397ad47b71a1439f13a51eea3137df46441979</requestSignature><requestTimestamp>2013-08-01T12:31:14-07:00</requestTimestamp>
    </ns1:AuthenticationHeader>
  </SOAP-ENV:Header>
  <SOAP-ENV:Body>
    <ns1:paramsRequestCampaign>
      <source>MKTOWS</source>
      <campaignId>4496</campaignId>
      <leadList>
        <leadKey>
          <keyType>EMAIL</keyType>
          <keyValue>lead@company.com</keyValue>
        </leadKey>
        <leadKey>
          <keyType>EMAIL</keyType>
          <keyValue>anotherlead@company.com</keyValue>
        </leadKey>
      </leadList>
    </ns1:paramsRequestCampaign>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

Risposta di SOAP per requestCampaign

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:ns1="<http://www.marketo.com/mktows/">
  <SOAP-ENV:Body>
    <ns1:successRequestCampaign>
      <result>
        <success>true</success>
      </result>
    </ns1:successRequestCampaign>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

Di seguito è riportato un programma Java di esempio che esegue lo scenario descritto in precedenza.

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


public class RequestCampaign {

    public static void main(String[] args) {
        System.out.println("Executing Request Campaign");
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
            ParamsRequestCampaign request = new ParamsRequestCampaign();

            request.setSource(ReqCampSourceType.MKTOWS);

            ObjectFactory objectFactory = new ObjectFactory();
            JAXBElement<Integer> campaignId = objectFactory.createParamsRequestCampaignCampaignId(4496);
            request.setCampaignId(campaignId);

            ArrayOfLeadKey leadKeyList = new ArrayOfLeadKey();
            LeadKey key = new LeadKey();
            key.setKeyType(LeadKeyRef.EMAIL);
            key.setKeyValue("lead@company.com");

            LeadKey key2 = new LeadKey();
            key2.setKeyType(LeadKeyRef.EMAIL);
            key2.setKeyValue("anotherlead@company.com");

            leadKeyList.getLeadKeies().add(key);
            leadKeyList.getLeadKeies().add(key2);

            JAXBElement<ArrayOfLeadKey> arrayOfLeadKey = objectFactory.createParamsRequestCampaignLeadList(leadKeyList);
            request.setLeadList(arrayOfLeadKey);

            SuccessRequestCampaign result = port.requestCampaign(request, header);

            JAXBContext context = JAXBContext.newInstance(SuccessRequestCampaign.class);
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

Questo articolo contiene il codice utilizzato per implementare integrazioni personalizzate. A causa della sua natura personalizzata, il team del supporto tecnico Marketo non è in grado di risolvere i problemi relativi al lavoro personalizzato. Non tentare di implementare il seguente esempio di codice senza un’adeguata esperienza tecnica o l’accesso a uno sviluppatore esperto.

Pubblicato il _2014-03-27_ da _Murta_

## Invio di un’e-mail con contenuti dinamici da Marketo tramite AP-

Immagina di voler automatizzare le e-mail di follow-up del call center. Dopo che il rappresentante di supporto parla con un cliente, desideri inviare automaticamente un’e-mail di ringraziamento per aver contattato la tua azienda. Facciamo un passo avanti e diciamo che vuoi includere l&#39;argomento di conversazione specifico discusso con il cliente che tieni traccia nel tuo CRM. Puoi eseguire questa operazione da Marketo utilizzando l’API SOAP requestCampaign per inviare un’e-mail con contenuto dinamico. L’API requestCampaign ti consente di trasmettere uno o più lead. Consente inoltre di trasmettere token di programma che possono essere utilizzati con una campagna esistente per inviare contenuto dinamico. L’API SOAP requestCampaign richiede che il destinatario dell’e-mail esista in Marketo. Pertanto, prima di chiamare l&#39;API requestCampaign, utilizza l&#39;[API getLead](/help/soap-api/getlead.md) per verificare se l&#39;e-mail esiste in Marketo. Ti mostriamo innanzitutto come creare una campagna avanzata, poi come impostare un trigger per inviare una campagna tramite l’API, infine come creare un’e-mail che accetta contenuti dinamici tramite token di programma, infine come definire un’e-mail come parte di un’azione di flusso e infine come quinto esempio di codice che verrebbe utilizzato per eseguire questa campagna. **Come creare una nuova campagna avanzata in Marketo** Le campagne avanzate in Marketo eseguono tutte le attività di marketing. Puoi impostare una serie di azioni automatizzate da eseguire su un elenco avanzato di contatti. Nel caso di invio di e-mail transazionali, imposta un trigger nella campagna, come mostrato di seguito, per inviare e-mail utilizzando l’API. Innanzitutto, configuriamo Smart Campaign. 1. In Attività di marketing, scegli un programma, quindi fai clic su Nuova risorsa locale nel menu a discesa Nuovo

1. Fai clic su Smart Campaign
1. Immetti il nome della campagna avanzata e fai clic su Crea **Aggiungi trigger a una campagna avanzata** L&#39;aggiunta di trigger a una campagna avanzata ti consente di eseguire una campagna avanzata su una persona alla volta in base a un evento live, che in questo caso è una richiesta tramite l&#39;API [requestCampaign](https://developer.adobe.com/marketo-apis/api/mapi/#operation/triggerCampaignUsingPOST).
1. Cerca il trigger &quot;Campaign is Requested&quot; (Campagna è richiesta), quindi trascinalo sull’area di lavoro.
1. Nel trigger, seleziona &quot;è&quot; e &quot;API servizio Web&quot;.

**Come passare contenuti dinamici utilizzando l&#39;API** In Marketo, i miei token sono variabili che puoi utilizzare nel programma. I miei token consentono di immettere le informazioni relative al programma in un&#39;unica posizione, sostituirle con un valore specificato e recuperarle in altre parti dell&#39;applicazione, ad esempio in un modello di e-mail. Utilizzando l’API SOAP requestCampaign, puoi passare un array di token di programma, che sostituirà i token esistenti. Dopo l’esecuzione della campagna, i token vengono scartati. Puoi creare I miei token a livello di cartella Campaign o a livello di Programma. I miei token a livello di cartella Campaign erediteranno tutti i programmi contenuti nella cartella Campaign. Se crei I miei token a livello di cartella Campaign, puoi sovrascrivere il valore ereditato a livello di programma. Ad esempio, se definisci i token per la Data programma e la Descrizione del programma a livello di cartella Campaign, puoi sovrascrivere questi valori a livello di singolo programma.

Ecco come farlo. 1. Dalla struttura Attività di marketing, seleziona la cartella Campaign o il programma in cui desideri creare i token. Dalla barra dei menu superiore, seleziona I miei token. Viene quindi visualizzata l’area di lavoro I miei token. Dalla struttura a destra, trascina un Tipo di token nell’area di lavoro, in questo caso &quot;Testo&quot;. Nel campo Nome token, evidenzia Mio token e immetti un Nome token univoco, che in questo caso è &quot;my.conversationtopic&quot;. Nel campo Valore, immetti un valore rilevante per il token, che in questo caso è &quot;Grazie per averci chiamato oggi&quot;. Tieni presente che utilizzando l’API sostituiremo il valore My Token predefinito. Fai clic su Salva per salvare il token personalizzato.  1. Crea un nuovo messaggio e-mail facendo clic su Nuovo. Fai clic su New Local Assets (Nuovo locale) e seleziona Email (E-mail). Compila quindi i campi pertinenti per assegnare un nome all’e-mail. Quando redigi l’e-mail, fai clic sull’icona Token per includere i token nell’e-mail. Dopo aver creato l’e-mail modello con i token, nel passaggio successivo aggiungeremo l’e-mail come azione di flusso per la campagna. Quindi, quando chiami la campagna tramite l’API, l’e-mail viene inviata.
**Come creare un&#39;azione di flusso e-mail in una campagna** L&#39;associazione di un&#39;e-mail con una campagna avanzata consente agli addetti al marketing di gestire l&#39;aspetto che deve avere un&#39;e-mail e permette all&#39;applicazione di terze parti di determinare chi la riceve e quando. Dopo aver creato un messaggio e-mail come nuova risorsa locale, puoi impostarlo come azione di flusso in una campagna. Trova e seleziona l’e-mail che desideri inviare.
**Esempio di codice per chiamare l&#39;API requestCampaign** Dopo aver configurato la campagna e i trigger nell&#39;interfaccia di Marketo, ti mostriamo come utilizzare l&#39;API per inviare un&#39;e-mail. Il primo esempio è una richiesta XML, il secondo è una risposta XML e l&#39;ultimo è un esempio di codice Java che può essere utilizzato per generare la richiesta XML. Ti mostriamo anche come trovare l’ID campagna utilizzato quando effettui una chiamata all’API requestCampaign. La chiamata API richiede inoltre di conoscere in anticipo l’ID della campagna Marketo. Puoi determinare l’ID della campagna utilizzando uno dei seguenti metodi: 1. Utilizza l&#39;API 1 [getCampaignsForSource](/help/soap-api/getcampaignsforsource.md). Apri la campagna Marketo in un browser e osserva la barra degli indirizzi URL. L’ID della campagna (rappresentato da un numero intero di 4 cifre) si trova immediatamente dopo &quot;SC&quot;. Ad esempio, `<https://app-stage.marketo.com/#SC&#x200B;**1025**&#x200B;A1>`. La parte in grassetto è l’ID della campagna &quot;1025&quot;. Richiesta SOAP per requestCampaign

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:ns1="<http://www.marketo.com/mktows/">
  <SOAP-ENV:Header>
    <ns1:AuthenticationHeader>
      <mktowsUserId>demo17_1_809939944BFABAE58E5D27</mktowsUserId><requestSignature>48397ad47b71a1439f13a51eea3137df46441979</requestSignature><requestTimestamp>2013-08-01T12:31:14-07:00</requestTimestamp>
    </ns1:AuthenticationHeader>
  </SOAP-ENV:Header>
  <SOAP-ENV:Body>
    <ns1:paramsRequestCampaign>
      <source>MKTOWS</source>
      <campaignId>4496</campaignId>
      <leadList>
        <leadKey>
          <keyType>EMAIL</keyType>
          <keyValue>lead@company.com</keyValue>
        </leadKey>
      </leadList>
      <programTokenList>
        <attrib>
          <name>{{my.conversationtopic}}</name>
          <value>Thank you for calling about adding a line of service to your current plan.</value>
        </attrib>
      </programTokenList>
    </ns1:paramsRequestCampaign>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

Risposta di SOAP per requestCampaign

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:ns1="<http://www.marketo.com/mktows/">
  <SOAP-ENV:Body>
    <ns1:successRequestCampaign>
      <result>
        <success>true</success>
      </result>
    </ns1:successRequestCampaign>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

Di seguito è riportato un programma Java di esempio che esegue lo scenario descritto in precedenza.

```java
import com.marketo.mktows.\*;
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


public class RequestCampaign {

    public static void main(String[] args) {
        System.out.println("Executing Request Campaign");
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
            ParamsRequestCampaign request = new ParamsRequestCampaign();

            request.setSource(ReqCampSourceType.MKTOWS);

            ObjectFactory objectFactory = new ObjectFactory();
            JAXBElement<Integer> campaignId = objectFactory.createParamsRequestCampaignCampaignId(4496);
            request.setCampaignId(campaignId);

            ArrayOfLeadKey leadKeyList = new ArrayOfLeadKey();
            LeadKey key = new LeadKey();
            key.setKeyType(LeadKeyRef.EMAIL);
            key.setKeyValue("lead@company.com");

            leadKeyList.getLeadKeies().add(key);

            JAXBElement<ArrayOfLeadKey> arrayOfLeadKey = objectFactory.createParamsRequestCampaignLeadList(leadKeyList);
            request.setLeadList(arrayOfLeadKey);

            ArrayOfAttrib aoa = new ArrayOfAttrib();

            Attrib attrib = new Attrib();
            attrib.setName("{{my.conversationtopic}}");
            attrib.setValue("Thank you for calling about adding a line of service to your current plan.");

            aoa.getAttribs().add(attrib);

            JAXBElement<ArrayOfAttrib> arrayOfAttrib = objectFactory.createParamsRequestCampaignProgramTokenList(aoa);
            request.setProgramTokenList(arrayOfAttrib);

            SuccessRequestCampaign result = port.requestCampaign(request, header);

            JAXBContext context = JAXBContext.newInstance(SuccessRequestCampaign.class);
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

Dopo aver effettuato una chiamata tramite l’API requestCampaign, puoi confermarla controllando per verificare se Smart Campaign è stato eseguito in Marketo.

Questo articolo contiene il codice utilizzato per implementare integrazioni personalizzate. A causa della sua natura personalizzata, il team del supporto tecnico Marketo non è in grado di risolvere i problemi relativi al lavoro personalizzato. Non tentare di implementare il seguente esempio di codice senza un’adeguata esperienza tecnica o l’accesso a uno sviluppatore esperto.

Pubblicato il _2014-04-03_ da _Murta_

## Acquisire le attività dei visitatori anonimi in base alla logica di business

Immagina di voler tracciare gli utenti che visitano un post specifico sul blog aziendale. Diciamo che, sul numero totale di utenti che visitano un post, vorresti solo tenere traccia degli utenti che segnalano il loro interesse spendendo almeno 5 secondi e scorrendo la pagina verso il basso. Per gli utenti anonimi si desidera creare un nuovo lead in Marketo con questo evento e per gli utenti noti si desidera aggiornare la loro attività lead con questo evento. Puoi eseguire questa operazione utilizzando il [codice di tracciamento di Munchkin](/help/javascript-api/lead-tracking.md) sul tuo sito Web. Quando un utente non cookie accede a una pagina con il codice di tracciamento di Munchkin, viene creato un nuovo cookie sul browser dell’utente e un nuovo lead anonimo in Marketo. Se l’utente è già cookie e l’utente è un lead esistente in Marketo, la visita alla pagina verrà registrata nel registro delle attività dell’utente in Marketo. Ti mostriamo innanzitutto come generare il codice di tracciamento di Munchkin in Marketo, poi come modificare il codice di esempio di Munchkin per attivare solo se determinate condizioni sono soddisfatte e infine come verificare che una visita di pagina da un utente anonimo sia stata registrata in Marketo.

**Come generare codice di tracciamento di Munchkin** Il codice di tracciamento di Munchkin consente di tenere traccia delle visite al sito Web. Esistono tre tipi di codice Munchkin descritti di seguito, ma in questo esempio utilizziamo il codice di tracciamento asincrono di Munchkin. A) Semplice: ha il minor numero di righe di codice, ma non ottimizza per il tempo di caricamento della pagina web. Questo codice carica la libreria jQuery ogni volta che viene caricata una pagina Web. B) Asincrono: riduce il tempo di caricamento della pagina web. Questo codice controlla se la libreria jQuery esiste già, la carica se è mancante e la utilizza per l’esecuzione del codice di tracciamento una volta caricato il resto della pagina web. C) Asynchronous jQuery: riduce il tempo di caricamento delle pagine web e migliora le prestazioni del sistema. Questo codice presuppone che tu disponga già di jQuery e non controlla di caricarlo. 1. Fai clic su Amministratore in alto a destra nell’app.  1. Fai clic su Munchkin nell’albero a sinistra.  1. Seleziona Asincrono per Tipo di codice di tracciamento. 1. Fai clic su e copia il codice di tracciamento JavaScript da inserire sul sito web.
**Esempio di codice per l&#39;utente cookie e evento di tracciamento** Inserisci il codice di tracciamento nelle pagine Web immediatamente prima del tag `</body>`. Le pagine di destinazione create in Marketo contengono automaticamente un codice di tracciamento, pertanto non è necessario inserirlo. Questo esempio di codice chiama l’API Munchkin dopo il caricamento dello script:

```javascript
<script type="text/javascript">
(function() {
  var didInit = false;
  function initMunchkin() {
    if(didInit === false) {
      didInit = true;
      Munchkin.init('XXX-XXX-XXX');
    }
  }
  var s = document.createElement('script');
  s.type = 'text/javascript';
  s.async = true;
  s.src = '//munchkin.marketo.net/munchkin.js';
  s.onreadystatechange = function() {
    if (this.readyState == 'complete' || this.readyState == 'loaded') {
      initMunchkin();
    }
  };
  s.onload = initMunchkin;
  document.getElementsByTagName('head')[0].appendChild(s);
})();
</script>
```

Questo esempio di codice chiama l’API Munchkin dopo che l’utente ha visitato la pagina per 5 secondi ed ha anche fatto scorrere di 500 pixel verso il basso nella pagina:

```javascript
<script src="https://code.jquery.com/jquery-2.1.0.min.js"></script>
<script type="text/javascript">
$(function(){
 setTimeout(function(){
  $(window).scroll(function() {
      var y_scroll_position = window.pageYOffset;
      var scroll_position = 500; //Sets number of pixels user must scroll to be tracked

  if(y_scroll_position > scroll_position) {
  //Munchkin tracking code
   (function() {
     var didInit = false;
     function initMunchkin() {
      if(didInit === false) {
        didInit = true;
        Munchkin.init('XXX-XXX-XXX');
      }
     }

     var s = document.createElement('script');
     s.type = 'text/javascript';
     s.async = true;
    s.src = '//munchkin.marketo.net/munchkin.js';
     s.onreadystatechange = function() {
      if (this.readyState == 'complete' || this.readyState == 'loaded') {
          initMunchkin();
      }
     };
     s.onload = initMunchkin;
     document.getElementsByTagName('head')[0].appendChild(s);
   })();
   }
 },5000); //Sets time delay before tracking user
});
</script>
```

**Come verificare che la visita alla pagina da parte di un utente anonimo sia stata registrata in Marketo**

1. Fai clic su Analytics nel menu principale, quindi fai clic su Nuovo rapporto. Scegliere Attività pagina Web come tipo di report, quindi assegnare un nome al report.
1. Dopo aver creato un report, fare clic su Smart List. Seleziona quindi il filtro Pagina web visitata dalla casella a destra. Inserisci la pagina web in cui inserisci il codice di tracciamento di Munchkin.
1. Fai clic su Configurazione. Seleziona Visitatori anonimi da ISP e cambia l’opzione in Mostrato.
1. Fai clic su Report. Ora potrai vedere l’attività tracciata sulla pagina web selezionata.
1. Fai doppio clic sul record del lead, che mostrerà quindi il registro attività in cui puoi visualizzare la pagina specifica visitata dall’utente anonimo.

Questo articolo contiene il codice utilizzato per implementare integrazioni personalizzate. A causa della sua natura personalizzata, il team del supporto tecnico Marketo non è in grado di risolvere i problemi relativi al lavoro personalizzato. Non tentare di implementare il seguente esempio di codice senza un’adeguata esperienza tecnica o l’accesso a uno sviluppatore esperto.

Pubblicato il _2014-04-17_ da _Murta_

## Modifica dinamica del numero di telefono locale tramite RTP

Personalization è tutto. Lo abbiamo capito molto tempo fa. Detto questo, mi sorprende che ogni volta che ho bisogno di assistenza immediata, sia così difficile trovare i numeri di telefono locali rilevanti su un sito web. È positivo che in [ sia installato ](https://business.adobe.com/products/marketo/content-personalization.html)Marketo Real-Time Personalization<https://business.adobe.com/products/marketo/adobe-marketo.html> (RTP). È possibile sfruttare l&#39;[API visitatore RTP](/help/javascript-api/web-personalization.md) per modificare dinamicamente il numero di telefono visualizzato da un visitatore Web in diverse sezioni del sito Web. Wow! Riesce a credere a questo? Come funziona questa magia? Innanzitutto devi installare RTP sul tuo sito Web come descritto [qui](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript). Quindi, segui le istruzioni riportate di seguito e implementa il codice JavaScript sul tuo sito web:

1. Inserisci il tuo numero di telefono internazionale nella configurazione di **defaultPhone**
1. Inserisci gli ID elemento HTML nella configurazione **divIds**
1. Se desideri impostare il numero di telefono come collegamento click-to-call per i browser mobili, imposta la configurazione di **mobileLink** su **true**.
1. Mappa le diverse posizioni con i relativi numeri di telefono nelle configurazioni **cityPhone**, **statePhone** e **countryPhone**

Ad esempio, di seguito sono riportati alcuni valori di esempio per le impostazioni di configurazione:

```json
  defaultPhone:"+1.503.608.4679", // Optional
  divIds:["phoneId1","phoneId2"],
  mobileLink: true,
  cityPhone: {
    "<a href="#">yanir</a>": ["San Mateo", "San Francisco"],
    "+353.1.242.3000": ["tel-aviv"]
  },
  statePhone: {
    "+1.650.376.2300": ["CA"],
    "+1.650.376.2302": ["OR"]
  },
  countryPhone: {
    "+1.650.376.2300": ["United States"],
    "+353.1.242.3000": ["Israel"]
  }
```

Infine, inserisci un tag di ancoraggio HTML contenente un ID corrispondente a uno degli ID in **divIds** (dal passaggio 2 precedente). Ad esempio, se hai specificato &quot;phoneId1&quot; in **divIds**, il tag di ancoraggio HTML sarà simile al seguente:

`<a href="tel:+1800229933" id="phoneId1">+1800229933</a>`

Lo script controlla se c’è una corrispondenza in questo ordine: cityPhone > statePhone > countryPhone > defaultPhone Puoi anche sostituire i numeri di telefono con del testo (Esempio: &quot;Unisciti al nostro gruppo di utenti di San Francisco!&quot;) o codice HTML e modificare dinamicamente il contenuto in base alla geolocalizzazione. Buon lavoro!

```html
<a href="tel:+1800229933" id="phoneId1">+1800229933</a>
<script>
(function(a){
    rtp('get','visitor',function(yc){
        var location = yc.results.location;
        var loop = true;
        var phoneChanged = false;
        console.log(yc.results);
        function checkObj(obj){
            return Object.getOwnPropertyNames(obj).length >0;
        }
        function changePhone(p){
            d=a.divIds;
            for(i=0;i<d.length;i++){
                if(document.getElementById(d[i]) !== null){
                    document.getElementById(d[i]).innerHTML = p;
                    if(a.mobileLink){
                        document.getElementById(d[i]).href= "tel:" + p;
                    }
                    console.log(p);
                }
            }
            loop = false;
            phoneChanged = true;
        }
        function matchLocation(loc,objc){
            for (var key in objc) {
                for(i=0;i<objc[key].length && loop;i++){
                    if (!loop) { return true;};
                    val = objc[key][i];
                    //console.log(loc + location[loc] + " ? " + val);
                    if(location[loc].toLowerCase() === val.toLowerCase()){
                        changePhone(key);
                    }
                }
            }
        }
        if(checkObj(a.cityPhone)){
            matchLocation("city",a.cityPhone);
        }else if(checkObj(a.statePhone)){
            matchLocation("state",a.statePhone);
        }else if(checkObj(a.countryPhone)){
            matchLocation("country", a.countryPhone);
        }else if(!phoneChanged && a.defaultPhone.length > 0 ){
            changePhone(a.divId,a.defaultPhone);
        }
    });
})({
    defaultPhone:"",  //  [Optional] the number to show if visitor does not match the mapping below
    divIds:["phoneId1","Floater"],  //the phone HTML element ID, can be <div>, <a>, <span>, <p> etc.
    mobileLink: true,  //if you use click to call link (with href="tel:") you can also change its number

    cityPhone: {
        "<a href='#'>yanir</a>": ["San Mateo", "San Francisco"],
        "+353.1.242.3000": ["tel-aviv"]
    },
    statePhone: {
        "+1.650.376.2300": ["CA"],
        "+1.650.376.2302": ["OR"]
    },
    countryPhone: {
        "+1.650.376.2300": ["United States"],
        "+353.1.242.3000": ["Israel"]
    }
});
</script>
```

Pubblicato il _2016-02-02_ da _Yanir_

## Aggiornamenti inverno 2016

### Oggetti personalizzati

* [Sono ora supportati gli oggetti personalizzati N:N relazioni](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-custom-objects/create-marketo-custom-objects)
   * I record Lead o Account possono ora avere relazioni molti-a-molti attraverso oggetti personalizzati tramite la definizione di oggetti intermedi. Dopo aver creato un tipo di oggetto personalizzato autonomo, è possibile creare un tipo di oggetto intermedio con campi di collegamento sia all&#39;oggetto autonomo che ai lead o agli account.
   * Non vi sono nuove chiamate API per questa funzionalità, ma le definizioni degli oggetti devono essere configurate correttamente per sfruttare queste relazioni tramite l’API.
* `getLeadActivities` e `getLeadChanges` non restituiranno più attività di lead anonimi. Per ulteriori informazioni, consulta le [domande frequenti sul tracciamento dei Munchkin di nuova generazione](https://experienceleague.adobe.com/it/docs/marketo/using/home)

Pubblicato il _2016-02-05_ da _Kenny_

## Recuperare le attività per un singolo lead tramite API REST

Ecco una domanda che ci viene ripetutamente fatta dalla nostra community di sviluppatori:

&quot;Come posso ottenere un elenco delle attività passate per un singolo lead?&quot;

Fino a poco tempo fa, non esisteva un modo semplice per eseguire questa operazione utilizzando l’API REST. Ma ora c&#39;è! La versione invernale 2016 della nostra API REST contiene un piccolo miglioramento. [Attività lead](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadActivitiesUsingGET) ora accetta il parametro **leadIds** che può essere utilizzato per specificare un ID lead. Quando si specifica il parametro **leadIds**, verranno restituite solo le attività per tale ID lead. Questo può essere considerato come un filtro per l’ID lead. Tieni presente che il parametro **leadIds** può accettare un elenco separato da virgole di ID lead nel caso in cui desideri filtrare i risultati per più di un lead (fino a 30). Ciò potrebbe rivelarsi utile, ad esempio, quando si limitano le attività ai lead per una particolare azienda. **Esempio** Di seguito è riportata una richiesta di esempio per [ottenere attività lead](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadActivitiesUsingGET) contenente il parametro **leadIds**. Ho specificato un valore di &quot;50&quot; per il parametro **leadIds**, che corrisponde a un lead arbitrario nella mia istanza di Marketo. Ho specificato il valore &quot;129&quot; per il parametro activityTypeIds, che corrisponde all&#39;attività &quot;Sessione app mobile&quot; nella mia istanza di Marketo.

`<https://123-abc-456.mktorest.com/rest/v1/activities.json?leadIds=50&activityTypeIds=129&nextPageToken=WQV2VQVPPCKHC6AQYVK7JDSA3J4SMAZRQO4RKIXCEMLFCM2APRSQ====>`

Di seguito è riportato un estratto della risposta a tale richiesta. Come puoi vedere, contiene solo oggetti risultato con &quot;leadId&quot;: 50 e &quot;activityTypeId&quot;: 129.

```json
{
    "id": 846,
    "leadId": 50,
    "activityDate": "2015-04-06T21:58:59Z",
    "activityTypeId": 129,
    "primaryAttributeValueId": 13,
    "primaryAttributeValue": "Sample App",
    "attributes": [
        {
            "name": "Device Type",
            "value": "iPhone"
        },
        {
            "name": "Mobile App Session Length",
            "value": 7
        },
        {
            "name": "Platform",
            "value": "ios"
        }
    ]
}
{
    "id": 879,
    "leadId": 50,
    "activityDate": "2015-04-07T00:45:11Z",
    "activityTypeId": 129,
    "primaryAttributeValueId": 13,
    "primaryAttributeValue": "Sample App",
    "attributes": [
        {
            "name": "Device Type",
            "value": "iPhone"
        },
        {
            "name": "Mobile App Session Length",
            "value": 5
        },
        {
            "name": "Platform",
            "value": "ios"
        }
    ]
}
{
    "id": 1114,
    "leadId": 50,
    "activityDate": "2015-04-08T00:02:41Z",
    "activityTypeId": 129,
    "primaryAttributeValueId": 13,
    "primaryAttributeValue": "Sample App",
    "attributes": [
        {
            "name": "Device Type",
            "value": "iPhone"
        },
        {
            "name": "Mobile App Session Length",
            "value": 241
        },
        {
            "name": "Platform",
            "value": "ios"
        }
    ]
}
{
    "id": 1551,
    "leadId": 50,
    "activityDate": "2015-04-09T23:31:56Z",
    "activityTypeId": 129,
    "primaryAttributeValueId": 13,
    "primaryAttributeValue": "Sample App",
    "attributes": [
        {
            "name": "Device Type",
            "value": "iPhone"
        },
        {
            "name": "Mobile App Session Length",
            "value": 223
        },
        {
            "name": "Platform",
            "value": "ios"
        }
    ]
}
{
    "id": 1716,
    "leadId": 50,
    "activityDate": "2015-04-15T22:44:19Z",
    "activityTypeId": 129,
    "primaryAttributeValueId": 13,
    "primaryAttributeValue": "Sample App",
    "attributes": [
        {
            "name": "Device Type",
            "value": "iPhone"
        },
        {
            "name": "Mobile App Session Length",
            "value": 223
        },
        {
            "name": "Platform",
            "value": "ios"
        }
    ]
}
```

## Integrazione perfetta con Marketo e oltre 500 applicazioni utilizzando Zapier

Questo è il post di Philippe Delle Case, Principal Solutions Consultant di Marketo,

### Obiettivi

Questo articolo spiega in dettaglio come integrare Marketo con potenzialmente oltre 500 app cloud, grazie a Zapier. Per questo, costruiremo da zero un connettore Zapier per Marketo e implementeremo due casi d’uso pratici di integrazione: Caso d’uso 1: integrazione unidirezionale di lead da FullContact Card Reader a Marketo

* Scansiona il biglietto da visita di qualsiasi contatto con l’app Reader per schede mobili FullContact e crea automaticamente un lead in Marketo.
* Aggiungere un lead esistente a un elenco statico in Marketo e trovare il lead aggiunto automaticamente al foglio Google.
* Modificare un lead nel foglio di Google e trovare la modifica ripetuta in Marketo.

### Prerequisiti

**Iscriviti gratuitamente a Zapier** [Zapier](https://zapier.com/) è un servizio di automazione delle app Web che consente di automatizzare facilmente le attività tra altre app online senza la necessità di programmatori o risorse IT. Una breve introduzione è disponibile [qui](https://zapier.com/api/v4/helpdocs/category/redirect/what-is-zapier). Oggi Zapier supporta più di 500 app in molti domini diversi come Marketing, CRM, CMS, Assistenza clienti, Firma elettronica, Forms, ecc. Una singola integrazione tra un’app e un’altra si chiama Zap. Controlla il [zapbook](https://zapier.com/apps) di Zapier per un elenco completo delle app web supportate.  Registrati per ottenere un account gratuito [qui](https://zapier.com/sign-up/). Puoi accedere a un massimo di 100 attività/mese, 5 zap e zap in esecuzione ogni 15 minuti. Naturalmente puoi ottenere molto di più sottoscrivendo i piani a pagamento di Zapier (base, business, business plus, ecc.)

**Accesso a un&#39;istanza di Marketo come amministratore o con un account utente API fornito** Il connettore Zapier utilizzerà l&#39;API REST di Marketo per inviare dati lead a Marketo. Per utilizzare questa API, è necessario disporre di un utente API e di un servizio personalizzato che è possibile creare autonomamente se si è amministratore dell’istanza di Marketo. In caso contrario, un amministratore dovrà fornirteli. È inoltre disponibile un webhook da creare, accessibile solo a un amministratore Marketo. Una spiegazione dettagliata su come creare l&#39;utente API di Marketo e il servizio personalizzato è disponibile [qui](http://rest-api/custom-services/). Al termine, è necessario disporre delle seguenti credenziali per richiamare l’API REST di Marketo: ID client, segreto client, ID account Munchkin, ID account Munchkin

Puoi ottenere l’ID account Munchkin dalle schermate Munchkin o Amministrazione servizi Web. Il modello è simile al seguente: `000-XXX-000`.  Non è necessario ottenere un token di accesso, in quanto sarebbe valido solo per una singola ora. Il connettore genera automaticamente i token.
**Iscriviti gratuitamente a un account con Google Docs, Sheets e Slides sono app di produttività che consentono di creare diversi tipi di documenti online, lavorarci in tempo reale con altre persone e archiviarli in Google Drive online. Il nostro caso d’uso richiede un foglio Google. Diverse funzioni di Google Docs e la creazione di un account con Google sono disponibili [qui](https://workspace.google.com/products/docs/).
**Registrati per un account gratuito con FullContact** FullContact ti mantiene completamente connesso alle persone più importanti, richiamando tutti i tuoi contatti e sincronizzandoli continuamente con modifiche a profili social, foto, firme e-mail, informazioni aziendali e altro ancora. Offrono un lettore di biglietti da visita per dispositivi mobili in grado di scansionare i biglietti in oltre 250 app web, tra cui Zapier. Puoi registrarti per un account gratuito [qui](https://app.fullcontact.com/login). Puoi anche abbonarti a un account premium a pagamento con più funzionalità e capacità. L&#39;app mobile può essere scaricata da [Apple AppStore](https://apps.apple.com/us/app/fullcontact-business-card/id576780807) o da [Google Play](https://play.google.com/store/apps/details?id=com.fullcontact.cardreader). Gli Zap FullContact sono documentati [qui](https://zapier.com/apps/contacts-plus/integrations).

### Implementazione del connettore Marketo per Zapier

**Crea l&#39;app Marketo** Dall&#39;interfaccia Web di Zapier, vai al portale per sviluppatori. Fai clic su **Aggiungi nuova app** e compila almeno il Titolo (ad esempio &#39;Marketo&#39;) e la Descrizione. Il logo è facoltativo, ma piacevole da avere.\ **Autenticazione** In questa sezione vengono dichiarati i diversi campi utilizzati per l&#39;autenticazione API REST di Marketo e le impostazioni di autenticazione. Crea prima i campi seguenti:

Modificare le impostazioni di autenticazione:

* Tipo di autenticazione: Autenticazione sessione
* Mappatura autenticazione:

  `{"access_token":"{{access_token}}"}`

* Posizionamento token di accesso&#x200B;**:** Token in Querystring

Una volta creato un servizio personalizzato Marketo, diventano disponibili l’ID client e il segreto client. Utilizziamo l&#39;ID client e il segreto client per generare un token di accesso tramite l&#39;endpoint REST API [Authentication](/help/rest-api/authentication.md). Possiamo quindi utilizzare questo token di accesso per effettuare richieste successive all’API REST. Il token scade dopo un’ora e deve essere generato di nuovo per continuare a chiamare l’API REST. Abbiamo scelto Tipo di autenticazione = &#39;Autenticazione sessione&#39; perché ci consente di eseguire uno script di autenticazione personalizzato ogni volta che il token di sessione è scaduto. Nella sezione &#39;Scripting API&#39; vedremo come implementare questo meccanismo che può funzionare solo con questo tipo di autenticazione.
**Triggers** Trigger Zapier per inserire dati in Zapier. Non ne abbiamo bisogno per i nostri casi d’uso in quanto utilizzeremo invece un webhook Marketo. Tuttavia, è ancora necessario scrivere un trigger fittizio come test obbligatorio per il connettore Marketo. Stiamo per creare un trigger di test che chiama l&#39;endpoint API REST di Marketo [Get Daily Usage](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getDailyUsageUsingGET). Fai clic su **Aggiungi nuovo trigger** per avviare la procedura guidata e compilare i campi seguenti (i campi non menzionati possono essere lasciati vuoti): Nome e descrizione

* Nome: Trigger di test
* Chiave: test_trigger
* Descrizione: Trigger di test dell’app Marketo
* Importante? Non selezionato
* Nascondere? Selezionato

Campi trigger

* Nessuna

Da dove provengono i dati

* Data Source: polling
* URL di polling: `https://{{munchkin_account_id}}.mktorest.com/rest/v1/stats/usage.json`

Risultato di esempio

* Lascia vuoto

Fai clic su **Gestisci impostazioni trigger** e imposta il trigger di test su quello che utilizzeremo per verificare le credenziali di un utente. **Azioni** Azioni Zapier disponibili per inviare dati da Zapier. Stiamo per implementare l’azione lead Create_Update che chiama l’API REST di Marketo. Questa azione consente di creare un nuovo lead all’interno di Marketo oppure, se il lead esiste già, lo aggiorna con i valori inviati. Per la deduplicazione verrà utilizzato il campo e-mail. Fai clic su **Aggiungi nuova azione** per avviare la procedura guidata e compilare i campi seguenti (i campi non menzionati possono essere lasciati vuoti): Nome e Descrizione

* Nome: Create_Update Lead
* Nome: lead
* Chiave: create-update-lead
* Descrizione: crea un nuovo lead in Marketo oppure, se il lead esiste già, aggiornalo con i valori inviati
* Importante? Selezionato
* Nascondere? Non selezionato

Campi azione Campi azione sono i campi in cui gli utenti mappano i dati. Sceglieteli con attenzione in base alle vostre esigenze, in quanto rappresenteranno tutti i dati che siete in grado di aggiornare in Marketo. In Zapier è disponibile un’opzione che offre all’utente finale tutti i campi disponibili in Marketo, ma che produrrebbe più codice e complessità, non necessari per un connettore usa e getta. Ad esempio, abbiamo selezionato i campi seguenti.

Il nome della partizione è obbligatorio nel nostro caso perché la nostra istanza Marketo ha Partizioni lead in servizio. In caso contrario, potrebbe essere omesso. L’abbiamo separato dal gruppo &quot;input&quot; in modo che l’utente finale capisca che non si tratta di un campo da sincronizzare. Il campo &quot;Note&quot; proviene da una sincronizzazione tra Marketo e Salesforce; non utilizzarlo se non lo si dispone nell’istanza Marketo. Il campo &quot;Chiamata&quot; è stato creato nell’istanza Marketo. Non utilizzarlo se non lo si dispone nell’istanza Marketo. Naturalmente, l&#39;obiettivo è quello di consentire di scegliere i campi necessari da Marketo. Si consiglia di iniziare in piccolo e aggiungere i campi aggiuntivi in un secondo momento. Dove inviare i dati

* URL endpoint azione: `https://{{munchkin_account_id}}.mktorest.com/rest/v1/leads.json`

Risultato di esempio

* Lascia vuoto

### API di scripting Zapier

La funzione di scripting di Zapier consente di manipolare le richieste e le risposte scambiate tra l’API dell’app e Zapier. Puoi modificare le richieste HTTP immediatamente prima che vengano inviate e analizzare le risposte prima che Zapier esegua qualsiasi operazione con esse. È necessario per completare l’autenticazione &quot;Autenticazione sessione&quot; personalizzata in modo che funzioni con Marketo. Ulteriori informazioni sono [qui](https://zapier.com/developer/documentation/scripting/). Copia il seguente codice e in seguito forniremo alcune spiegazioni:

```javascript
var Zap = {

    get_session_info: function(bundle) {

       console.log('Entering get_session_info method ...');

         var access_token,
            access_token_request_payload,
            access_token_response;


        // Assemble the meta data for our Access Token swap request
         console.log('building Request with client_id=' + bundle.auth_fields.client_id + ', and client_secret=' + bundle.auth_fields.client_secret);
        access_token_request_payload = {
            method: 'POST',
            url: 'https://' + bundle.auth_fields.munchkin_account_id + '.mktorest.com/identity/oauth/token',
            params: {
                'grant_type' : 'client_credentials',
                'client_id' : bundle.auth_fields.client_id,
                'client_secret' : bundle.auth_fields.client_secret
            },
            headers: {
                'Content-Type': 'application/json',  // Could be anything.
                Accept: 'application/json'
            }
        };

        // Fire off the Access Token request.
        access_token_response = z.request(access_token_request_payload);

        // Extract the Access Token from returned JSON.
        access_token = JSON.parse(access_token_response.content).access_token;
        console.log('New Access_Token=' + access_token);

        // This will be mixed into bundle.auth_fields in future calls.
        //bundle.auth_fields.access_token=access_token;
        return {'access_token': access_token};
    },


    test_trigger_pre_poll: function(bundle) {

         console.log('Entering test_trigger_pre_poll method ...');

         bundle.request.params = {
         'access_token':bundle.auth_fields.access_token
         };

         return bundle.request;

    },


    test_trigger_post_poll: function(bundle) {

        console.log('Entering test_trigger_post_poll method ...');

        var data = JSON.parse(bundle.response.content);
        if ((!data.success)&&((data.errors[0].code=="601")||(data.errors[0].code=="600"))){
            console.log('Access Token expired or invalid, requesting new one - data.success=' + data.success + ', data.errors[0].code=' + data.errors[0].code);

           throw new InvalidSessionException(); // Calling get_session_info() to regenerate Access Token
        }

        return JSON.parse(bundle.response.content);
    },

    create_update_lead_pre_write: function(bundle) {

       bundle.request.params = {'access_token':bundle.auth_fields.access_token};
       return bundle.request;
    },

    create_update_lead_post_write: function(bundle) {

         var data = JSON.parse(bundle.response.content);
         if ((!data.success)&&((data.errors[0].code=="601")||(data.errors[0].code=="600"))){
            console.log('Access Token expired or invalid, requesting new one - data.success=' + data.success + ', data.errors[0].code=' + data.errors[0].code);
            throw new InvalidSessionException(); // Calling get_session_info() to regenerate Access Token
        }
        return JSON.parse(bundle.response.content);
    }
};
```

**Metodi** **get_session_info**

* Questo metodo è responsabile della generazione o della rigenerazione di un token di accesso che chiama l&#39;endpoint API REST di Marketo [Authentication](/help/rest-api/authentication.md).
* Viene chiamato ogni volta che un metodo &quot;post_poll&quot; rileva un errore &quot;Token di accesso scaduto&quot;. La scadenza pianificata di un token di accesso è 1 ora, quindi è prevista.
* URL endpoint azione: https://{{munchkin_account_id}}.mktorest.com/identity/oauth/token

**pre_polling, pre_write**

* È necessario creare un metodo di &quot;pre-polling&quot; su qualsiasi trigger creato, per modificare la richiesta HTTP immediatamente prima dell’invio, in modo da poter aggiungere il token di accesso Marketo nei relativi parametri.
* Per lo stesso motivo, è necessario creare un metodo &quot;pre-write&quot; su qualsiasi azione creata.

**post_poll, post_write**

* Dobbiamo creare un metodo di post-polling su qualsiasi trigger creato, per analizzare le risposte prima che Zapier faccia qualcosa con loro e alla fine intercettare l’errore &quot;Token di accesso scaduto&quot;.
* Per lo stesso motivo, è necessario creare un metodo &quot;post-scrittura&quot; su qualsiasi azione creata.
* Se si verifica un errore di questo tipo, viene generata un’eccezione InvalidSessionException che indica a Zapier di ripetere l’autenticazione ed eseguire nuovamente il metodo get_session_info.

Puoi accedere ai registri del bundle dall’API Scripting dal menu &quot;Collegamenti rapidi&quot;, nell’angolo in alto a destra dello schermo. Questa funzione è molto utile per eseguire il debug degli script.

Caso d&#39;uso 1: integrazione di Marketo con FullContact Card Reader

Per questa integrazione viene creato un unico Zap da FullContact a Marketo. Con questo Zap, è possibile scansionare i biglietti da visita con il Reader FullContact Mobile Card e inviare i lead a Marketo.   **Zap FullContact -> Marketo** Dal dashboard di Zapier, fare clic sul pulsante &#39;Crea un nuovo Zap&#39;.

**Trigger in Zapier**

* Scegli l&#39;app FullContact
* Scegli trigger FullContact &#39;Nuovo biglietto da visita&#39;
* Connessione all&#39;account FullContact
* Test dell&#39;app FullContact

**Azione in Zapier**

* Scegli l’app Marketo appena creata, che dovrebbe essere visualizzata in Beta
* Scegliere l&#39;azione Marketo &#39;Crea_aggiorna lead&#39;
* Connettiti al tuo account Marketo, compilando i parametri di autenticazione (ID account Munchkin, ID client, segreto client)
* Mappare i campi da FullContact a Marketo
* Compilare un Nome partizione nel punto in cui i nuovi lead dovrebbero andare (solo se le partizioni esistono nell’istanza Marketo)
* Test dell’app Marketo
* Attiva Zap

Nota: assicurati di scaricare i biglietti da visita Reader da FullContact e attivare l’integrazione Zapier direttamente dal tuo dispositivo mobile.

Caso d&#39;uso 2: integrazione di Marketo con i fogli Google

Per questa integrazione creiamo due Zap. Una da Marketo a Google Sheets e un&#39;altra da Google Sheets a Marketo. Con questo Zap, puoi sincronizzare alcuni dei tuoi lead o contatti tra Marketo e un foglio di Google.   **Webhook Marketo Zap -> Fogli Google**
Per il primo Zap, non ci affidiamo a un connettore personalizzato per Marketo, ma sfruttiamo i webhook di Marketo e il trigger &quot;Webhook by Zapier&quot;. Dal dashboard di Zapier, fai clic sul pulsante &quot;Crea un nuovo Zap&quot;. **Attivazione parte 1 in Zapier**

* Scegli l&#39;app trigger &#39;Webhook by Zapier&#39;
* Seleziona &quot;Catch Hook&quot; (Acquisisci hook) che consente di attendere che un POST o un GET raggiunga un URL Zapier
* Non è necessario selezionare una chiave figlio
* Zapier ha generato un **URL webhook** personalizzato a cui inviare richieste e copiarlo negli Appunti

**Webhook in Marketo (operazioni che devono essere eseguite da un amministratore)**

* Vai a Amministratore -> Webhook
* Crea un nuovo webhook denominato &quot;Push Lead to Zapier&quot; e modifica il modulo Webhook.

Nel campo del modello, dichiara tutti i campi del lead che desideri trasferire a Zapier e sfruttare i token di Marketo. Per i nostri Casi d’uso, utilizziamo gli stessi campi definiti per il connettore Zapier personalizzato che invia i lead a Marketo:

```json
{
"firstName":"{{lead.First Name}}",
"lastName":"{{lead.Last Name}}",
"email":"{{lead.Email Address}}",
"phone":"{{lead.Phone Number}}",
"leadOwner":"{{lead.Lead Owner First Name}} {{lead.Lead Owner Last Name}}",
"leadOwnerEmail":"{{lead.Lead Owner Email Address}}",
"leadNotes":"{{lead.Lead Notes:default=edit me}}",
"called":"{{lead.Called}}"
}
```

* Salvare il modulo
* Non è necessario utilizzare una mappatura della risposta, quindi il webhook è pronto

**Prova campagna in Marketo (procedura che deve essere eseguita da un addetto al marketing o da un amministratore)**

* Dalle attività di marketing, crea una nuova campagna avanzata

A scopo di test, creeremo una campagna che attiva il nostro webhook ogni volta che lo stato di un lead cambia in MQL. Naturalmente puoi utilizzare il webhook per qualsiasi altro scopo aziendale.

* Modificare l’elenco avanzato
* Chiamare il webhook nel flusso
* Pianificare la campagna
* Assicurati che ogni lead possa attraversare il flusso ogni volta
* Attivare la campagna avanzata

**Attivazione della parte 2 in Zapier**

* Per completare l’app del trigger &quot;Webhook by Zapier&quot;, è necessario attivare la campagna intelligente Marketo una volta e acquisire il webhook in Zapier
* Nel nostro caso di test, dobbiamo semplicemente andare a Marketo Lead Database, aprire un lead e cambiarne lo stato in &quot;MQL&quot;

**Creare il foglio di calcolo in Google Sheets**

* Crea un nuovo foglio di calcolo
* Creare un foglio di lavoro o utilizzare quello predefinito
* Aggiungi una colonna per ogni campo che desideri sincronizzare da Marketo (quelli dichiarati nel webhook di Marketo)

  **Azione in Zapier**

* Scegli i fogli di App Google
* Selezionare l&#39;opzione &#39;Crea riga foglio di calcolo&#39;
* Connetti al tuo account Google Sheets
* Selezionare il foglio di calcolo Google Sheets
* Selezionare il foglio di lavoro
* Mappa tutti i campi tra l’app trigger &quot;Webhooks by Zapier&quot; e i fogli di Google.

* Test dell’app dei fogli di Google
* Attiva Zap

**Zap Google Sheets -> Marketo**

Dal dashboard di Zapier, fai clic sul pulsante &quot;Crea un nuovo Zap&quot;.

**Trigger in Zapier**

* Scegli l&#39;app di attivazione &quot;Fogli Google&quot;
* Selezionare la &#39;Riga foglio di calcolo aggiornato&#39; che viene attivata quando viene aggiunta o modificata una nuova riga in un foglio di calcolo
* Connetti al tuo account Google
* Selezionare il foglio di calcolo da cui si desidera attivare (deve essere lo stesso utilizzato nella Zap precedente) e il foglio di lavoro
* Imposta colonna trigger su &#39;any_column&#39;
* Test dell’app dei fogli di Google

**Azione in Zapier**

* Scegli l’app Marketo appena creata, che dovrebbe essere visualizzata in Beta
* Scegliere l&#39;azione Marketo &#39;Crea_aggiorna lead&#39;
* Connettiti al tuo account Marketo, compilando i parametri di autenticazione (ID account Munchkin, ID client, segreto client)
* Mappare i campi da Google Sheets a Marketo
* Compilare un Nome partizione nel punto in cui i nuovi lead dovrebbero andare (solo se le partizioni esistono nell’istanza Marketo)
* Test dell’app Marketo
* Attiva Zap

### Conclusione

Ecco alcune idee per migliorare il connettore Marketo per Zapier:

* Aggiunta di altri trigger e azioni correlati a diversi oggetti di Marketo (elenchi, oggetti personalizzati, ecc.)
* Invece di codificare i campi da Marketo, è possibile estrarre dinamicamente i campi da Marketo, ma ciò richiederebbe un lavoro di traduzione tecnica tra Marketo e Zapier.
* Condividere il connettore con il team di sviluppo e infine renderlo generalmente disponibile.

È possibile che Zapier distribuisca una scheda Premium Marketo, il che renderebbe molto più semplice implementare i nostri casi d’uso. In ogni caso, questo articolo potrebbe sempre essere utilizzato per integrare Marketo con Zapier con un piano Zapier gratuito, e anche per creare casi d’uso personalizzati che potrebbero non essere supportati da un adattatore premium. Ci auguriamo che questo articolo ti sia stato utile per ottenere risultati ancora migliori con Marketo e Zapier. Grazie!

Pubblicato il _2016-04-17_ da _David_

## Aggiornamenti della primavera 2016

**API REST**

* API Asset - Pagine web
   * Le **pagine di destinazione** sono ora esposte tramite quindici nuovi endpoint che consentiranno la creazione, l&#39;aggiornamento, l&#39;eliminazione, la clonazione e la gestione delle bozze per le pagine di destinazione. Ora i modelli di pagina di destinazione presentano anche endpoint di gestione delle bozze esposti
      * Ottieni pagine di destinazione
      * Ottieni pagina di destinazione per ID
      * Ottieni pagina di destinazione per nome
      * Crea pagina di destinazione
      * Aggiorna metadati pagina di destinazione
      * Ottieni contenuto pagina di destinazione
      * Aggiungi sezione contenuto pagina di destinazione
      * Sezione Aggiorna contenuto pagina di destinazione
      * Elimina sezione contenuto pagina di destinazione
      * Ottieni sezione contenuti dinamici
      * Aggiorna sezione contenuti dinamici
      * Elimina bozza pagina di destinazione
      * Approva pagina di destinazione
      * Annulla la bozza della pagina di destinazione
      * Elimina pagina di destinazione
   * **Modelli di pagina di destinazione**
      * Elimina bozza modello pagina di destinazione
      * Approva modello pagina di destinazione
      * Annullare la validità del modello della pagina di destinazione
      * Elimina modello pagina di destinazione
   * **Forms** ha 21 nuovi endpoint rilasciati che forniscono funzionalità complete di creazione, modifica e gestione tramite l&#39;API. Le API non supporteranno le modifiche ai moduli di Forms 1.0.
      * Ottieni Forms
      * Ottieni modulo per ID
      * Ottieni modulo per nome
      * Ottieni elenco campi modulo
      * Aggiorna elenco campi modulo
      * Crea modulo
      * Ottieni pagina di ringraziamento modulo
      * Aggiorna pagina di ringraziamento modulo
      * Aggiorna modulo
      * Elimina bozza modulo
      * Approva modulo
      * Annulla approvazione modulo
      * Clona modulo
      * Elimina modulo
      * Aggiorna campo modulo
      * Rimuovi campo modulo
      * Aggiorna regole di visibilità campo modulo
      * Aggiungi campo modulo Rich Text
      * Aggiungi set di campi
      * Rimuovi campo dal set di campi
      * Ottieni campi modulo disponibili
      * Cambia posizioni campo modulo
      * Pulsante Aggiorna invio
   * Quando si utilizza **Ottieni o sfoglia programmi**, verrà restituito l&#39;ID SFDC Campaign per i programmi collegati a una campagna SFDC

Gli oggetti personalizzati **Oggetti personalizzati** ora supportano i tipi di dati Area di testo, consentendo la memorizzazione di campi stringa contenenti un massimo di 2.000 caratteri nei campi oggetto personalizzati di questo tipo. **Inserimento di indirizzi IP nella whitelist** Gli utenti amministratori potranno ora gestire una whitelist di indirizzi IP per impedire l&#39;accesso non autorizzato tramite le API. [Ulteriori informazioni su questa funzionalità sono disponibili qui](https://experienceleague.adobe.com/it/docs/marketo/using/home). **Interfaccia attività personalizzata** Gli utenti amministratori potranno ora definire tipi di attività personalizzati nel menu di amministrazione e aggiungere record ai lead tramite l&#39;API [Aggiungi attività personalizzate](https://developer.adobe.com/marketo-apis/api/mapi/#operation/addCustomActivityUsingPOST). [Informazioni sulla definizione dei tipi di attività personalizzati sono disponibili qui](https://experienceleague.adobe.com/it/docs/marketo/using/home).

Pubblicato il _2016-06-01_ da _Kenny_

## Aggiornamenti dell’estate 2016

Per la versione dell’estate 2016 del 23 settembre, verranno rilasciate tre funzioni orientate agli sviluppatori.

### Supporto di Email 2.0 nell’API REST

Tutte le [API di Asset preesistenti](/help/rest-api/assets.md) compatibili solo con e-mail e modelli v1.0 ora sono abilitate per l&#39;utilizzo con le risorse e-mail v2.0.

### Invia lead a Marketo

[Il lead push](/help/rest-api/leads.md) è un metodo alternativo di sincronizzazione dei lead progettato per semplificare l&#39;attivazione nelle campagne avanzate. È possibile creare un singolo elemento di registro attività, associare un lead e aggiornare il record del lead in una chiamata. Questa funzione funziona in modo simile a un singolo modulo compilato da un lead e può essere utilizzata più facilmente come metodo proxy per l’invio di moduli, anziché utilizzare il metodo Lead di sincronizzazione esistente.

### Compressione HTTP

L’API REST può ora comprimere le risposte utilizzando lo standard definito dalla specifica HTTP 1.1. Questo può aiutare a ridurre le dimensioni della risposta, aumentando la velocità di trasferimento e riducendo al minimo l&#39;utilizzo della larghezza di banda.  

Pubblicato il _2016-09-23_ da _Kenny_

## Utilizzo di swagger-codegen con Marketo

[Swagger-codegen](https://github.com/swagger-api/swagger-codegen) è una potente libreria Java in grado di generare sia stub server che client API da definizioni Swagger. Questo può alleviare notevolmente le difficoltà e i costi di generazione dei client per qualsiasi lingua specifica. Per iniziare e generare il primo client, devi prima acquisire una copia specifica dell’istanza di una delle definizioni Swagger di Marketo. Inserisci l’ID Munchkin dall’istanza con cui desideri eseguire il test. Inizia con la definizione dell’identità. Ora che disponi di una definizione specifica per la tua istanza, devi scaricare e installare swagger-codegen. Il processo è specifico del sistema operativo in uso ed è possibile ottenere le istruzioni [qui](https://github.com/swagger-api/swagger-codegen#prerequisites) Con le impostazioni predefinite, il codegen restituirà un client che copre tutti gli endpoint e i modelli forniti. Questi vengono in genere gestiti tramite una classe denominata DefaultApi, contenente metodi per chiamare gli endpoint disponibili con esempi forniti in una cartella &quot;docs&quot; (per impostazione predefinita, non tutte le lingue includono modelli per questo elemento). Ora creiamo il primo client. Crea una cartella in cui desideri che il codice esista, quindi vai nella sessione terminale e utilizza il comando genera per generare un client nella lingua desiderata (supponiamo che tu abbia utilizzato il metodo di installazione homebrew):

swagger-codegen generate -i $definitionLocation -l $yourLanguage -o $yourLocation

Il codice client verrà inviato alla posizione desiderata. Ora vediamo come utilizzare questo per chiamare l’endpoint di identità e ottenere un token di accesso:

```java
 public static void main(String[] args){
  String client_id = args[0];
  String client_secret = args[1];
  ApiClient client = new ApiClient();
  DefaultApi id = new DefaultApi();
  try {
   String token = id.identityOauthTokenGet(client_id, client_secret, "client_credentials").getAccessToken();
   System.out.println(token);
  } catch (ApiException e) {
   e.printStackTrace();
  }
 }
```

```php
<?php
require_once(_DIR_ . '/vendor/autoload.php');

$api_instance = new Swagger\\Client\\Api\\DefaultApi();
$client_id = "client_id_example";
$client_secret = "client_secret_example";
$grant_type = "grant_type_example";

try {
    $result = $api_instance->identityOauthTokenGet($client_id, $client_secret, $grant_type);
    print_r($result->getAccessToken);
} catch (Exception $e) {
    echo 'Exception when calling DefaultApi->identityOauthTokenGet: ', $e->getMessage(), "\\n";
}
?>
```

```c#
  public static void Main(string[] args)
  {
      string clientId = "CHANGE ME";
      string clientSecret = "CHANGE ME";

      IdentityApi instance = new IdentityApi();
      ResponseOfIdentity response = instance.IdentityUsingGET(clientId, clientSecret, "client_credentials");

      string message = string.Format("Access Token: {0}, Expires In: {1}, Scope: {2}, Token Type: {3}",
          response.AccessToken, response.ExpiresIn, response.Scope, response.TokenType);
      Console.WriteLine(message);
  }
```

Ora che sappiamo come ottenere l’autorizzazione, esamineremo casi d’uso più avanzati di clienti generati automaticamente nelle prossime settimane.

Pubblicato il _2016-10-10_ da _Kenny_

## Integrazione di Excel Parte 1: Estrarre e modellare i dati di Marketo utilizzando Power Query

Questo è il primo di una serie di articoli che spiegano come sfruttare la tecnologia Power BI incorporata in Microsoft Excel per creare una vera esperienza di analisi aziendale self-service con Marketo. Con i concetti trattati in questi articoli, puoi:

* Importare dati da Marketo in Excel
* Importare e combinare dati provenienti da altre origini (applicazioni SaaS, database, file sequenziali, ecc.)
* Forma dati per esigenze aziendali e scopo di analisi
* Aggiorna su richiesta i dati da Excel
* Creare colonne e misure calcolate utilizzando le formule
* Creare relazioni tra dati eterogenei
* Analizzare i dati e creare rapporti avanzati con tabelle pivot e grafici pivot
* Produrre visualizzazioni dati straordinarie

### Power Query per Excel

Questo primo articolo tratta il processo di importazione e di creazione di forme dei dati mediante la tecnologia Power Query. Power Query è una tecnologia di connessione dati che consente di individuare, connettere, combinare e perfezionare le origini dati per soddisfare le esigenze di analisi. Le funzionalità di Power Query sono disponibili in Excel e Power BI Desktop. Power Query può connettersi a molte origini dati come database, Facebook, Salesforce, MS Dynamics CRM, ecc. Marketo non è supportato come funzionalità integrata, ma fortunatamente è possibile utilizzare le API REST di Marketo per l’esecuzione remota di molte delle funzionalità del sistema e Power Query viene fornito con un set completo di formule (informalmente note come &quot;M&quot;) che consentono di scrivere un’origine dati personalizzata.

### Connettore personalizzato

Con Power Query è banale creare script per una singola chiamata API REST, ma diventa più difficile gestire i seguenti requisiti:

* Gestione dei token di accesso, compreso il meccanismo di autenticazione e l’aggiornamento periodico dei token
* Meccanismo di impaginazione per un ampio insieme di dati
* Gestione degli errori

Questo articolo spiega come creare un solido connettore personalizzato che possa utilizzare le API REST di Marketo per richiamare tutti i tipi di dati (lead, attività, oggetti personalizzati, programmi, ecc.). L’unica restrizione è limitata al limite giornaliero di richieste dell’API Marketo. I concetti qui descritti si concentrano su Marketo, ma potrebbero anche essere utilizzati per integrare altre soluzioni SaaS che forniscono un’API REST.

### Prerequisiti

#### Power Query

Prima del rilascio di Excel 2016, Microsoft Power Query per Excel funzionava come componente aggiuntivo di Excel scaricato e installato in Excel 2010 o Excel 2011. Da Excel 2016, questa tecnologia è una funzionalità nativa integrata nella barra multifunzione &#39;Dati&#39; nella sezione &#39;Get &amp; Transform&#39;. Tutti gli script prodotti per questo articolo sono stati testati su Excel 2016 per Windows. I concetti devono essere gli stessi per Excel 2013 o Excel 2010, ma potrebbero essere necessari alcuni adattamenti.  Power Query è attualmente disponibile solo nella versione di Excel per Microsoft Windows; la versione per Mac non è supportata.

#### Marketo

Power Query utilizzerà le API REST di Marketo per accedere ai dati da Marketo. Per utilizzare queste API, è necessario disporre di un utente API e di un servizio personalizzato che possano essere creati autonomamente, se si è amministratori dell’istanza di Marketo. In caso contrario, un amministratore dovrà fornirteli. Una spiegazione dettagliata su come creare l&#39;utente API di Marketo e il servizio personalizzato è disponibile [qui](/help/rest-api/custom-services.md). Al termine, è necessario disporre delle credenziali seguenti per richiamare le API REST di Marketo: **ID client** e **Segreto client**. L&#39;**endpoint REST API** si trova nella sezione REST API dell&#39;amministratore dei servizi Web in Marketo e deve avere il seguente pattern:

`<https://XXX-XXX-XXX.mktorest.com/rest>`

Marketo dispone di un limite di richieste giornaliero per le API; tale limite è disponibile nell’amministratore dei servizi web insieme a un rapporto sul consumo. **Assicurati di non superare mai il limite giornaliero durante la progettazione delle query, in quanto potresti perdere alcuni dati nei report**.

### Creazione cartella di lavoro Power Query

Iniziamo con una nuova cartella di lavoro di Excel. Viene creato un foglio di lavoro di configurazione specifico per dichiarare tutte le impostazioni API REST di Marketo. In questo foglio di lavoro vengono create tre tabelle:

Tabella &#39;**REST_API_Authentication**&#39; con le colonne: **URL**: endpoint API REST di Marketo. **ID client**: dalle credenziali OAuth2.0 dell&#39;API REST di Marketo. **Segreto client**: dalle credenziali OAuth2.0 dell&#39;API REST di Marketo.
Tabella &#39;**Scoping**&#39; con le colonne: **Token di paging SinceDatetime**: una data successiva alla notazione di data standard ISO 8601 (ad esempio &quot;2016-10-06T13:22:17-08:00&quot;, &quot;2016-10-06&quot; sono data/ora valide) utilizzata per recuperare le attività Marketo da un determinato periodo, grazie a un token di paging iniziale &quot;basato su data&quot;. Questa data viene utilizzata principalmente per limitare la quantità di dati da importare nella cartella di lavoro. **ID elenco**: l&#39;ID di un elenco statico in Marketo che fa riferimento a tutti i lead/contatti con cui abbiamo a che fare. Questo elenco statico può essere gestito liberamente in Marketo (ad esempio una campagna intelligente può alimentarlo periodicamente o in tempo reale con lead e contatti).
Per ottenere l’ID di un elenco statico, aprilo in Marketo e ottieni il relativo ID numerico dall’URL, ad esempio `<https://myorg.marketo.com/#ST3517A1LA1>`, ID elenco=3511. **Max. pagine record**: viene utilizzato per i nostri algoritmi pseudo-ricorsivi che eseguono iterazioni nei dati di output di Marketo, utilizzando token di paging &quot;basati sulla posizione&quot;, con una capacità massima di 300 record per pagina. Poiché è nostro interesse ottenere il maggior numero possibile di record per pagina, ci atterremo a 300. Pertanto, in genere un massimo di pagine di record impostato su 33,333 significa una capacità di 33,333 X 300 = 9,9999 milioni di record, ma significa anche 33,333 K sul limite di richieste giornaliere API di Marketo. Gli algoritmi si interrompono comunque non appena vengono ottenuti tutti i dati dalle query, quindi questo parametro è solo un limite di sicurezza per un ciclo.

Tabella `Leads` con colonna **Campi lead**: campi lead separati da virgole da raccogliere da Marketo durante la query dei lead e dei contatti. La dichiarazione di una tabella in Excel è semplice. Immettete due righe nel foglio di calcolo con i nomi e i valori delle colonne, evidenziate con il mouse il perimetro della tabella, selezionate l&#39;icona Tabella nel menu &#39;Inserisci&#39;, quindi assegnate un nome. I nomi assegnati alle tabelle e alle relative colonne sono importanti in quanto verranno chiamati direttamente dai nostri script.

## Token di autenticazione e accesso

### Informazioni sull’autenticazione API REST di Marketo

Le API REST di Marketo sono autenticate con OAuth 2.0 a 2 gambe. Gli ID client e i segreti client vengono forniti dai servizi personalizzati definiti dall’utente. Ogni servizio personalizzato è di proprietà di un utente solo API con un set di ruoli e autorizzazioni che autorizzano il servizio a eseguire azioni specifiche. Un token di accesso è associato a un singolo servizio personalizzato.
Il meccanismo di autenticazione completo è documentato [qui](/help/rest-api/authentication.md) sul sito Marketo Developer. Quando viene creato originariamente un token di accesso, la sua durata è di 3600 secondi o 1 ora. Ogni chiamata di autenticazione consecutiva per lo stesso servizio personalizzato restituisce il token di accesso corrente con la durata rimanente. Una volta scaduto il token, l’autenticazione restituisce un token di accesso nuovo. La gestione della scadenza dei token di accesso è importante per garantire il corretto funzionamento dell’integrazione e impedire il verificarsi di errori di autenticazione imprevisti durante il normale funzionamento.

#### Crea query

Creare una nuova query facendo clic sull&#39;icona Nuova query nella sezione Get&amp;Transform del menu Dati. Seleziona una query vuota per iniziare con e assegnale un nome come &quot;MktoAccessToken&quot;. Avviare l&#39;Editor avanzato dall&#39;Editor query, in modo da poter eseguire manualmente lo script di alcune formule di Power Query. Immetti il seguente codice nell’editor avanzato:

```
let
    // Get url and credentials from config worksheet - Table REST_API_Authentication
    mktoUrlStr = Excel.CurrentWorkbook(){[Name="REST_API_Authentication"]}[Content]{0}[URL],
    clientIdStr = Excel.CurrentWorkbook(){[Name="REST_API_Authentication"]}[Content]{0}[Client ID],
    clientSecretStr = Excel.CurrentWorkbook(){[Name="REST_API_Authentication"]}[Content]{0}[Client Secret],

    // Calling Marketo API Get Access Token
    getAccessTokenUrl = mktoUrlStr & "/identity/oauth/token?grant_type=client_credentials&client_id=" & clientIdStr & "&client_secret=" & clientSecretStr,
    TokenJson = try Json.Document(Web.Contents(getAccessTokenUrl)) otherwise "Marketo REST API Authentication failed, please check your credentials",

    // Parsing access token
    accessTokenStr = TokenJson [access_token]

in
    accessTokenStr
```

I commenti incorporati nel codice sorgente, preceduti da &quot;//&quot; rendono il codice auto-esplicativo. Se hai bisogno di un riferimento di funzione, consulta i collegamenti forniti nella sezione Riferimento di questo articolo. Fare clic sul pulsante &quot;Fine&quot;. Verifica che il token di accesso sia visualizzato correttamente nell’output per il passaggio finale applicato &quot;accessTokenStr&quot;.  Un rapido commento sulla sicurezza in Excel; a volte ti potrebbe essere chiesto di abilitare Connessioni dati esterne dal banner giallo. Questo è necessario per consentire il corretto funzionamento delle query.

#### Conversione di una query in una funzione

Torna all’Editor avanzato e racchiudi il codice con la seguente dichiarazione di funzione:

```
let
    FnMktoGetAccessToken =()=>

        let
            // Get url and credentials from config worksheet - Table REST_API_Authentication
            mktoUrlStr = Excel.CurrentWorkbook(){[Name="REST_API_Authentication"]}[Content]{0}[URL],
            clientIdStr = Excel.CurrentWorkbook(){[Name="REST_API_Authentication"]}[Content]{0}[Client ID],
            clientSecretStr = Excel.CurrentWorkbook(){[Name="REST_API_Authentication"]}[Content]{0}[Client Secret],

            // Calling Marketo API Get Access Token
           getAccessTokenUrl = mktoUrlStr & "/identity/oauth/token?grant_type=client_credentials&client_id=" & clientIdStr & "&client_secret=" & clientSecretStr,
           TokenJson = try Json.Document(Web.Contents(getAccessTokenUrl)) otherwise "Marketo REST API Authentication failed, please check your credentials",

            // Parsing access token from Json
           accessTokenStr = TokenJson [access_token]

        in
            accessTokenStr

in FnMktoGetAccessToken
```

La funzione non accetta parametri nell’input, ma li ottiene dal foglio di lavoro di configurazione. Genera il token di accesso come output. Rinominare la query `FnMktoGetAccessToken` e salvarla. Puoi visualizzare tutte le query in qualsiasi momento in Excel facendo clic sul pulsante &quot;Mostra query&quot; nella sezione &quot;Ottieni e trasforma&quot; del menu Dati. A questo punto la funzione deve essere contrassegnata con l&#39;icona funzione &#39;Fx&#39;.

### Carica membri dell&#39;elenco statico

#### Ottieni lead

L’API lead di Marketo offre semplici operazioni CRUD rispetto ai record dei lead, la possibilità di modificare l’appartenenza di un lead a elenchi e programmi statici e di avviare l’elaborazione di campagne intelligenti per i lead. Tutte queste funzionalità sono documentate [qui](/help/rest-api/leads.md). È possibile recuperare un set elevato di record di lead in base all&#39;appartenenza a un elenco statico o a un programma. Utilizzando l&#39;ID di un elenco statico, è possibile recuperare tutti i record dei lead che sono membri di tale elenco statico. L’ID dell’elenco è un parametro di percorso nella chiamata di. Per informazioni dettagliate, consulta il capitolo &quot;Elenco e appartenenza al programma&quot; nella documentazione per gli sviluppatori di Marketo. Il numero massimo di record di lead che possiamo ottenere per chiamata API è 300, quindi dobbiamo sfruttare i token di paging per raccogliere i record per pagina di 300 record. Il token di paging viene visualizzato nella risposta Json dopo la prima chiamata e sappiamo che l’operazione è completata quando il token di paging non è più nell’output.

### Query di base

Iniziamo con una query completamente funzionante volta a scaricare tutti i lead da un elenco statico. Crea una nuova query vuota denominata &quot;MktoLeads&quot; e immetti il seguente codice nell’editor avanzato:

```
let
    // Get Url from config worksheet - Table REST_API_Authentication
    mktoUrlStr = Excel.CurrentWorkbook(){[Name="REST_API_Authentication"]}[Content]{0}[URL],
    // Get the number of iterations (pages of 300 records) - Table Scoping
    iterationsNum = Excel.CurrentWorkbook(){[Name="Scoping"]}[Content]{0}[Max Records Pages],
    // Get the List id - Table Scoping
    listIdStr = Number.ToText(Excel.CurrentWorkbook(){[Name="Scoping"]}[Content]{0}[List ID], "D", ""),
    // Get the Lead fields to extract - Table Leads
    LeadFieldsStr = Excel.CurrentWorkbook(){[Name="Leads"]}[Content]{0}[Lead Fields],


    // Build Multiple Leads by List Id URL
    getMultipleLeadsByListIdUrl = mktoUrlStr & "/rest/v1/list/" & listIdStr & "/leads.json?fields=" & LeadFieldsStr,

    // Build Marketo Access Token URL parameter
    accessTokenParamStr = "&access_token=" & FnMktoGetAccessToken(),

    pagingTokenParamStr = "",

    // Function iterating though the pages
    FnProcessOnePage =
    (accessTokenParamStr, pagingTokenParamStr) as record =>
        let

            // Send REST API Request
            content = Web.Contents(getMultipleLeadsByListIdUrl & accessTokenParamStr & pagingTokenParamStr),

            // Recover Json output and watch if token is expired, in that case, regenerate access token
            newAccessTokenParamStr = if Json.Document(content)[success]=true then accessTokenParamStr else "?access_token=" & FnMktoGetAccessToken(),
            getMultipleLeadsByListIdJson = if Json.Document(content)[success]=true then Json.Document(content) else Json.Document(Web.Contents(getMultipleLeadsByListIdUrl & newAccessTokenParamStr & pagingTokenParamStr)),

            // Parse Json outputs: data and next page token
            data = try getMultipleLeadsByListIdJson[result] otherwise null,
            next  = try  "&nextPageToken=" & getMultipleLeadsByListIdJson[nextPageToken] otherwise null,
            res = [Data=data, Next=next, Access=newAccessTokenParamStr]
        in
            res,

    // Generates a list of values given four functions that generate the initial value initial, test against a condition, and if successful select the result and generate the next value next. An optional parameter, selector, may also be specified
    GeneratedList =
        List.Generate(
            ()=>[i=0, res = FnProcessOnePage(accessTokenParamStr, pagingTokenParamStr)],
            each [i]<iterationsNum and [res][Data]<>null,
            each [i=[i]+1, res = FnProcessOnePage([res][Access],[res][Next])],
            each [res][Data])
in
    GeneratedList
```

Power Query non offre funzioni di loop tradizionali (ad es. For-loop, While-loop) e non supporta la ricorsione. Una buona soluzione consiste nell’implementare un ciclo For utilizzando List.Generate. Questa funzione è documentata [qui](http://msdn.microsoft.com/query-bi/m/list-generate). Con elenco. Genera è possibile eseguire iterazioni sulle pagine. In ogni passaggio dell’iterazione estraiamo una pagina di dati, mantenendo l’URL che include il token di paging per la pagina successiva, e memorizziamo i risultati nell’elemento successivo dell’elenco generato. Il blog di [Datachant](https://datachant.com/2016/06/27/cursor-based-pagination-power-query/) è un&#39;ottima risorsa per risolvere il problema. Il nostro parametro &#39;Max Records Pages&#39; è qui per limitare il numero di pagine e limitarlo a un intervallo realistico evitando un loop infinito. Un’altra sfida consiste nel garantire che il token di accesso non sia mai scaduto. Tenere traccia della durata rimanente sarebbe troppo complesso con Power Query. Pertanto, per tutte le chiamate all’API REST viene eseguito un backup con un controllo degli errori. Se si verifica un errore, supponiamo che il token sia scaduto e lo rinnoviamo prima di ripetere la chiamata. Se la seconda chiamata non riesce, il secondo errore verrà notificato a Excel (nel peggiore dei casi, non si ottengono dati di conseguenza). Avvia la query, dopo averla salvata o facendo clic sul pulsante Aggiorna in qualsiasi momento.  Nel nostro caso, sono stati estratti 1364 record di lead, adattandoli in 5 pagine di dati all’interno di 5 elenchi.

### Modellazione dei dati

È necessario modellare i dati in modo che tutti questi record siano inclusi in un unico elenco di record. Esistono due modi per farlo:

* Utilizzo di altro codice
* Sfruttare l’interfaccia utente di Power Query

Fai clic con il pulsante destro del mouse sulla griglia di output e scegli &quot;A tabella&quot; nel menu contestuale per convertirla in una tabella di elenchi.  Nella finestra a comparsa &#39;A tabella&#39;, lasciare i valori predefiniti nelle 2 liste di selezione.  Espandere la tabella di elenchi risultante. Ora abbiamo tutti i record in un&#39;unica lista. I record codificati in formato Json contengono i campi e i relativi valori associati. Espandi di nuovo. Seleziona nel pop-up tutti i campi che vuoi mantenere, deseleziona la casella di spunta &quot;Usa il nome della colonna originale come prefisso&quot;.  Et voilà! Tutti i record vengono visualizzati correttamente nella tabella. Se riapriamo l’editor avanzato, noteremo che sono state aggiunte 3 righe di codice per modellare i nostri dati:

```
# "Converted to Table" = Table.FromList(GeneratedList, Splitter.SplitByNothing(), null, null, ExtraValues.Error),
# "Expanded Column1" = Table.ExpandListColumn(#"Converted to Table", "Column1"),
# "Expanded Column2" = Table.ExpandRecordColumn(#"Expanded Column1", "Column1", {"id", "updatedAt", "lastName", "email", "createdAt", "firstName"}, {"id", "updatedAt", "lastName", "email", "createdAt", "firstName"})
```

In Power Query è possibile fare molto di più, ad esempio creare colonne aggiuntive con valori calcolati. In seguito verranno visualizzate ulteriori possibilità. Salvare e chiudere questa query. Ora può essere aggiornato manualmente in qualsiasi momento o automaticamente tramite aggiornamenti in background.

### Reindirizzamento dei risultati

Ora la domanda è, dove inviare i dati dei risultati? Passa il puntatore del mouse sulla query e seleziona il menu &quot;Carica in...&quot; nel menu contestuale. Nella finestra a comparsa, puoi selezionare:

* &#39;Tabella&#39; se si desidera inviare tutti i dati con forma a un foglio di lavoro (nuovo o esistente),
* &#39;Crea connessione&#39; se l&#39;obiettivo è quello di eseguire ulteriori analisi in Power Pivot.

La casella di controllo &#39;Aggiungi al modello dati&#39; consente di sfruttare i dati in Power Pivot; questo è ciò che si desidera per la seconda parte di questo articolo.

### Gestione dell’impaginazione

Poiché lo scopo del nostro progetto è quello di creare molte più query, eseguiamo un po’ di refactoring ed estraiamo una funzione riutilizzabile che gestirebbe l’impaginazione. Crea una nuova query vuota denominata **FnMktoGetPagedData** e immetti il seguente codice nell&#39;editor avanzato:

```
let
    FnMktoGetPagedData =(url, accessTokenParamStr, pagingTokenParamStr)=>

    let

        // Get the number of iterations (pages of 300 records) - Table Scoping
        iterationsNum = Excel.CurrentWorkbook(){[Name="Scoping"]}[Content]{0}[Max Records Pages],

        // Sub-function iterating though the REST API service result pages
        FnProcessOnePage =
        (accessTokenParamStr, pagingTokenParamStr) as record =>
            let

                // Send REST API Request
                content = Web.Contents(url& accessTokenParamStr & pagingTokenParamStr),

                // Recover Json output and watch if token is expired, in that case, regenerate access token
                newAccessTokenParamStr = if Json.Document(content)[success]=true then accessTokenParamStr else "?access_token=" & FnMktoGetAccessToken(),
                contentJson = if Json.Document(content)[success]=true then Json.Document(content) else Json.Document(Web.Contents(url & newAccessTokenParamStr & pagingTokenParamStr)),

                // Parse Json outputs: data and next page token
                data = try contentJson[result] otherwise null,
                next  = try  "&nextPageToken=" & contentJson[nextPageToken] otherwise null,
                res = [Data=data, Next=next, Access=newAccessTokenParamStr]
            in
                res,

        // Generates a list of values given four functions that generate the initial value initial, test against a condition, and if successful select the result and generate the next value next. An optional parameter, selector, may also be specified
        GeneratedList =
            List.Generate(
                ()=>[i=0, res = FnProcessOnePage(accessTokenParamStr, pagingTokenParamStr)],
                each [i]<iterationsNum and [res][Data]<>null,
                each [i=[i]+1, res = FnProcessOnePage([res][Access],[res][Next])],
                each [res][Data])
    in
        GeneratedList

in FnMktoGetPagedData
```

Salva la query. Lo useremo dopo.

### Query semplificata

Riscriviamo la query &#39;MktoLeads&#39; che chiamerà la funzione **FnMktoGetPagedData**.

```
let
    // Get Url from config worksheet - Table REST_API_Authentication
    mktoUrlStr = Excel.CurrentWorkbook(){[Name="REST_API_Authentication"]}[Content]{0}[URL],
    // Get the List id - Table Scoping
    listIdStr = Number.ToText(Excel.CurrentWorkbook(){[Name="Scoping"]}[Content]{0}[List ID], "D", ""),
    // Get the Lead fields to extract - Table Leads
    LeadFieldsStr = Excel.CurrentWorkbook(){[Name="Leads"]}[Content]{0}[Lead Fields],


    // Build Multiple Leads by List Id URL
    getMultipleLeadsByListIdUrl = mktoUrlStr & "/rest/v1/list/" & listIdStr & "/leads.json?fields=" & LeadFieldsStr,

    // Build Marketo Access Token URL parameter
    accessTokenParamStr = "&access_token=" & FnMktoGetAccessToken(),

    // No initial paging token required for this call
    pagingTokenParamStr = "",

    // Invoke the multiple REST API calls through the FnMktoGetPagedData function
    result = FnMktoGetPagedData (getMultipleLeadsByListIdUrl , accessTokenParamStr, pagingTokenParamStr)

in
    result
```

Come puoi vedere, la nostra query è ora molto semplice da leggere e gestire. Stiamo per sfruttare nuovamente la funzione **FnMktoGetPagedData** per le altre query.

### Caricare attività specifiche da un periodo di tempo definito

#### Ottieni attività con impaginazione

Marketo consente un&#39;ampia varietà di tipi di attività relativi ai record dei lead. Quasi ogni modifica, azione o passaggio di flusso viene registrato rispetto al registro attività di un lead e può essere recuperato tramite l’API o utilizzato nei filtri e nei trigger di Smart Campaign e Smart List. Le attività sono sempre correlate al record lead tramite il leadId, corrispondente all’ID del record, e dispongono anche di un proprio ID intero univoco. La documentazione API REST completa [è disponibile qui](/help/rest-api/activities.md).

Esistono molti tipi di attività potenziali, che possono variare da un abbonamento all’altro e che hanno definizioni univoche per ciascuno di essi. Anche se ogni attività avrà un proprio ID univoco, leadId e activityDate, primaryAttributeValueId e primaryAttributeValue avranno un significato diverso. Ci concentreremo sui Momenti Interessanti, un tipo di attività tracciate da Marketo con l&#39;ID 41. Le nuove sfide che risolveremo sono:

* È necessario avviare un token di paginazione &quot;basato su data&quot; per definire il periodo di tempo in cui si sono verificate le attività,
* La creazione di forme per i dati è un po’ più difficile, in quanto a seconda dei tipi di attività, in Json viene fornito un elenco di attributi specifici per l’attività che devono essere analizzati e appiattiti per facilitare l’analisi.

#### Token di paging basato sulla data

Innanzitutto, devi generare questa funzione per generare il token di paging iniziale &quot;basato sulla data&quot;, necessario per definire il periodo di tempo per le query dell’attività. La documentazione sul token di paging [è disponibile qui](/help/rest-api/paging-tokens.md). Crea una nuova query vuota denominata **FnMktoGetPagingToken** e immetti il seguente codice nell&#39;editor avanzato:

```
let
    FnMktoGetPagingToken =(accessTokenStr)=>

        let
            // Get url from config worksheet - Table REST_API_Authentication
            mktoUrlStr = Excel.CurrentWorkbook(){[Name="REST_API_Authentication"]}[Content]{0}[URL],

            // Get Paging Token SinceDatetime from config worksheet - Table Scoping
            mktoPTSinceDatetimeStr = DateTime.ToText(Excel.CurrentWorkbook(){[Name="Scoping"]}[Content]{0}[Paging Token SinceDatetime], "yyyy-MM-ddThh:mm:ss"),

            // Building URL for API Call
            getPagingTokenUrl = mktoUrlStr & "/rest/v1/activities/pagingtoken.json?access_token=" & accessTokenStr & "&sinceDatetime=" & mktoPTSinceDatetimeStr,

            // Calling Marketo API Get Paging Token
            content = Web.Contents(getPagingTokenUrl),

            // Recover Json output and watch if access token is expired, in that case, regenerate it
            newAccessTokenStr = if Json.Document(content)[success]=true then accessTokenStr else "?access_token=" & FnMktoGetAccessToken(),
            pagingTokenJson = if Json.Document(content)[success]=true then Json.Document(content) else Json.Document(Web.Contents(mktoUrlStr & "/rest/v1/activities/pagingtoken.json?access_token=" & newAccessTokenStr & "&sinceDatetime=" & mktoPTSinceDatetimeStr)),

            // Parsing Paging Token
            pagingTokenStr = pagingTokenJson[nextPageToken]

        in
            pagingTokenStr

in FnMktoGetPagingToken
```

Salva la funzione. Lo useremo dopo.

#### Attività per i momenti di interesse

Ora scriviamo la query &#39;MktoInterestingMomentsActivities&#39; che chiamerà le funzioni **FnMktoGetPagedData** e **FnMktoGetPagingToken**.

```
let

    // Get Url from config worksheet - Table REST_API_Authentication
    mktoUrlStr = Excel.CurrentWorkbook(){[Name="REST_API_Authentication"]}[Content]{0}[URL],
    // Get the List id - Table Scoping
    listIdStr = Number.ToText(Excel.CurrentWorkbook(){[Name="Scoping"]}[Content]{0}[List ID], "D", ""),

    // Build Get Activities URL
    getActivitiesUrl = mktoUrlStr & "/rest/v1/activities.json?ListId=" & listIdStr & "&activityTypeIds=46",

    // Build Marketo Access Token URL parameter
    accessTokenStr = FnMktoGetAccessToken(),
    accessTokenParamStr = "&access_token=" & accessTokenStr,

    // Obtain date-based paging token used to scope in time the activities
    pagingTokenParamStr = "&nextPageToken=" & FnMktoGetPagingToken(accessTokenStr),

    // Invoke the multiple REST API calls through the FnMktoGetPagedData function
    result = FnMktoGetPagedData (getActivitiesUrl , accessTokenParamStr, pagingTokenParamStr)

in
    result
```

Il risultato di questa query è ancora una volta un elenco di elenchi, pertanto richiede un’ulteriore elaborazione dei dati per essere utilizzabile per l’analisi.

### Modellazione dei dati

Facciamo le stesse operazioni di modellazione che abbiamo fatto per i lead:

* Fai clic con il pulsante destro del mouse sulla griglia di output e scegli &quot;A tabella&quot; nel menu contestuale per convertirla in una tabella di elenchi.
* Espandere la tabella di elenchi risultante,
* Espandi ancora una volta, selezionando nel pop-up tutti i campi che desideri mantenere (deseleziona la casella di spunta &quot;Usa il nome della colonna originale come prefisso&quot;).

Puoi visualizzare le colonne con i relativi valori, ad eccezione della colonna &quot;attributi&quot; che contiene ancora un elenco di attributi specifici associati ai momenti di interesse. Espandiamo questi attributi. Ora che l’elenco si è espanso in record, si espande di nuovo, selezionando i campi desiderati (nome e valore di ciascun attributo) e si deseleziona la casella di spunta &quot;usa il nome della colonna originale come prefisso&quot;.  Di conseguenza, tutti i nostri dati sono visibili, inclusi gli attributi, ma ogni attività del momento di interesse si estende su 3 righe. Questo sarà difficile da usare per la nostra analisi.  Idealmente vogliamo una sola riga per attività, con tutti gli attributi visualizzati come colonne aggiuntive. Possiamo farlo semplicemente ruotando i 3 attributi dalla nostra tabella. Seleziona le 2 colonne &quot;Nome&quot; e &quot;Valore&quot; dagli attributi dell’attività e fai clic su &quot;Colonna pivot&quot; nel menu &quot;Trasforma&quot;. Richiedi le opzioni avanzate nel pop-up e seleziona &#39;Colonna valori&#39; = valore e &#39;Non aggregare&#39; funzione di valore.  Fare clic su &#39;OK&#39; per generare una singola riga di dati per attività.  Le seguenti righe di codice di &quot;data shaping&quot; devono essere state aggiunte automaticamente allo script della query:

```
  #"Converted to Table" = Table.FromList(result, Splitter.SplitByNothing(), null, null, ExtraValues.Error),
    #"Expanded Column1" = Table.ExpandListColumn(#"Converted to Table", "Column1"),
    #"Expanded Column2" = Table.ExpandRecordColumn(#"Expanded Column1", "Column1", {"id", "leadId", "activityDate", "activityTypeId", "campaignId", "primaryAttributeValue", "attributes"}, {"id", "leadId", "activityDate", "activityTypeId", "campaignId", "primaryAttributeValue", "attributes"}),
    #"Expanded attributes" = Table.ExpandListColumn(#"Expanded Column2", "attributes"),
    #"Expanded attributes1" = Table.ExpandRecordColumn(#"Expanded attributes", "attributes", {"name", "value"}, {"name", "value"}),
    #"Pivoted Column" = Table.Pivot(#"Expanded attributes1", List.Distinct(#"Expanded attributes1"[name]), "name", "value")
in
    #"Pivoted Column"
```

### Passaggi successivi

Ora dovresti essere in grado di progettare tutte le query necessarie per accedere a qualsiasi dato Marketo specifico disponibile tramite le API REST. Ci auguriamo che questo articolo ti sia piaciuto e che ti abbia aiutato a sfruttare i grandi vantaggi combinati di Excel e Marketo. Nel secondo articolo viene inoltre fornita una cartella di lavoro di esempio con tutte le query.

### Riferimenti

#### Power Query

* [Power Query - Panoramica e apprendimento](https://support.microsoft.com/en-us/article/Power-Query-Overview-and-Learning-ed614c81-4b00-4291-bd3a-55d80767f81d)
* [Riferimento formula Power Query](http://msdn.microsoft.com/query-bi/m/power-query-m-reference)
* [Matt Masson Blog](http://www.mattmasson.com/tag/power-query/) fornisce alcune buone risorse su Power Query
* [Blog DataChant](https://datachant.com/2016/06/27/cursor-based-pagination-power-query/) è molto utile per l&#39;implementazione del meccanismo di impaginazione

Pubblicato il _2016-10-18_ da _Philippe_

## Aggiornamenti di autunno 2016

Nella versione di autunno 2016, verrà aggiunto il supporto CRUD per le variabili e i moduli E-mail v2 e il supporto CRUD per gli account denominati. Ora potrai leggere e modificare localmente i contenuti utilizzando le API REST di Marketo, nonché spostarli, riordinarli ed eliminarli. Di seguito è riportato l&#39;elenco completo degli aggiornamenti:

### API del database lead

* [**Account denominati**](/help/rest-api/named-accounts.md)
   * Nuovi endpoint per la lettura, l&#39;aggiornamento e l&#39;eliminazione di account denominati
   * Problemi noti:
      * A partire dalla versione di autunno 2016, i lead non possono essere associati ad account denominati tramite l’API

### API di Asset

* [**E-mail**](https://developer.adobe.com/marketo-apis/api/asset/#operation/describeUsingGET_5)
   * Nuovi endpoint per la manipolazione delle variabili e-mail v2
   * Nuovi endpoint per la manipolazione dei moduli E-mail v2
   * Problemi noti:
      * Le query e gli aggiornamenti per le sezioni che contengono token predittivi restituiranno un errore
      * Le e-mail con sezioni di contenuto che contengono token predittivi potrebbero non essere approvate utilizzando l’API

Pubblicato il _2016-12-07_ da _Kenny_

## Perché ci siamo ritirati @MarketoDev Twitter Handl3

Abbiamo deciso di ritirare l&#39;handle di @MarketoDev su Twitter. L’account verrà disattivato il 9 dicembre 2011. Curioso perché? **Riavvolgi all&#39;inizio del 2014...** Avevamo appena avviato il sito per sviluppatori Marketo per aiutare gli sviluppatori a creare rispetto alle nostre API. Volevamo aumentare la consapevolezza della piattaforma Marketo e ridurre le barriere all&#39;ingresso per gli sviluppatori. Insieme a questo impegno, abbiamo creato @MarketoDev per interagire con i nostri partner tecnologici e con i clienti che erano propensi a creare soluzioni integrate con Marketo. Come ogni nuova impresa coraggiosa, non eravamo sicuri di come avrebbe funzionato. Inizialmente abbiamo pubblicato su Twitter nuovi post di blog e nuove versioni API. Le cose andavano bene; il traffico verso il sito per sviluppatori è aumentato! Abbiamo anche iniziato a ricevere una serie di domande alle quali abbiamo dovuto rispondere. **Avanza rapidamente alla fine del 2016...** Con la crescita dell&#39;ecosistema [Partner LaunchPoint](https://exchange.adobe.com/apps/browse/ec?product=MRKTO), è aumentata la quantità di attività tweet. Rispondere al volume di domande inviate a @MarketoDev era diventato un lavoro importante per il nostro piccolo team. Abbiamo ricevuto una quantità crescente di domande sul prodotto in generale e sulle vendite, che abbiamo reindirizzato a @Marketo o @MarketoCares. Avevamo anche scoperto che 140 caratteri non erano semplicemente sufficienti per le domande relative allo sviluppo. Rispondere a queste domande ha spesso portato a lunghi e coinvolti thread di conversazione, un approccio che non ha avuto proporzioni. Infine, abbiamo analizzato le fonti di traffico per i visitatori del sito per sviluppatori e abbiamo scoperto che la maggior parte proviene da ricerche organiche e dalla funzionalità &quot;Subscribe Now&quot; sul nostro blog. Per questi motivi, abbiamo deciso di @MarketoDev. la spina **Da qui in avanti...** Se sei un fan di Twitter (e chi non lo è), non temere! Il nostro account Twitter aziendale @Marketo continua a vivere, così come il nostro servizio di assistenza clienti gestisce @MarketoCares.

Pubblicato il _2016-12-02_ da _David_

## Integrazione di Excel Parte 2: creazione di report e visualizzazioni dati avanzati di Marketo mediante Power Pivot e Power View

Questo articolo è il secondo di una serie di due articoli che spiegano come sfruttare la tecnologia Power BI incorporata in Microsoft Excel per creare una vera esperienza di analisi aziendale self-service con Marketo. Con i concetti trattati in questi articoli, puoi:

* Importare dati da Marketo in Excel
* Importare e combinare dati provenienti da altre origini (applicazioni SaaS, database, file sequenziali, ecc.)
* Forma i dati per esigenze aziendali e scopi di analisi
* Aggiorna dati su richiesta in Excel
* Creare colonne e misure calcolate utilizzando le formule
* Creare relazioni tra dati eterogenei
* Analizzare i dati e creare rapporti avanzati con tabelle pivot e grafici pivot
* Produrre visualizzazioni dati straordinarie

### Power Pivot e Power View per Excel

In questo articolo, forniamo esempi su come creare quanto segue:

* Rapporti Marketo avanzati che sfruttano le relazioni tra diverse raccolte di dati Marketo utilizzando Power Pivot
* Visualizzazioni statiche e animate interessanti con Power View

**Power Pivot** è un componente aggiuntivo di Excel, già incluso in Excel 2016, che è possibile utilizzare per eseguire potenti analisi dei dati e creare modelli di dati sofisticati. Con Power Pivot è possibile eseguire il mash-up di grandi volumi di dati da varie origini, eseguire rapidamente l&#39;analisi delle informazioni e condividere facilmente le informazioni. I dati estratti da diverse origini dati con Power Query possono essere inviati al modello dati, al foglio di calcolo Excel o a entrambi. Nel primo articolo, abbiamo importato e modellato i dati da Marketo e li abbiamo inviati al modello di dati per eseguire analisi più sofisticate prima di renderli disponibili sul foglio di calcolo. **Power View** è un&#39;alternativa al livello di visualizzazione di Excel. Si tratta di un’esperienza interattiva di esplorazione, visualizzazione e presentazione dei dati che incoraggia l’uso di reporting e dashboard intuitivi e ad hoc.

Tutti i passaggi illustrati in questo articolo sono stati testati su Excel 2016 per Windows. I concetti devono essere gli stessi per Excel 2013 o Excel 2010 (senza Power View), ma potrebbero essere necessari alcuni adattamenti. Power Pivot e Power View sono attualmente disponibili solo nella versione Microsoft Windows di Excel 2016; la versione Office 365 di Excel 2016 non supporta completamente Power Pivot e Power View.

### Cartella di lavoro Marketo Power

#### Scarica cartella di lavoro

Nel primo articolo, abbiamo trattato il processo di importazione e modellazione dei dati utilizzando la tecnologia Power Query. Abbiamo imparato a implementare alcune query avanzate di alimentazione per estrarre lead e attività da Marketo. Perché alcuni di voi vorrebbero passare direttamente al punto in cui generano rapporti e visualizzazioni, senza codificare. Questa cartella di lavoro contiene tutte le query descritte nel primo articolo e alcune di più. Abbiamo migliorato la gestione degli errori e aggiunto alcuni parametri aggiuntivi nel foglio di lavoro di configurazione. Se hai esaminato il primo articolo, ti consigliamo comunque di scaricare la cartella di lavoro di Marketo Power e di controllare ciò che è stato aggiunto.

Dichiarazione di non responsabilità: la cartella di lavoro per il risparmio di energia di Marketo non è un prodotto Marketo ufficiale e pertanto non è supportata da Marketo. Puoi utilizzarlo ed espanderlo per le tue esigenze aziendali, ma puoi farlo a tuo rischio e pericolo.

#### Configura cartella di lavoro

Compila tutte le informazioni richieste dal foglio di lavoro di configurazione di Marketo:

* **Autenticazione API REST Marketo:** richiesta
* **Ambito:** impostare il token di paging SinceDatetime e l&#39;ID dell&#39;elenco statico di Marketo contenente tutti i lead che si desidera analizzare
* **Lead:** per i report futuri, è necessario specificare almeno i seguenti campi Lead: `id`, `firstName`, `lastName`, `email`, create`edAt`, `updatedAt`, `title`, `company`, `industry`, `inferredCountry`, `inferredCity`
   * Se le informazioni sulla città sono più precise in uno dei campi personalizzati, puoi utilizzare un campo personalizzato
* **Attività:** I tipi di attività da recuperare dal database di Marketo sono specificati qui per ogni set di attività, non è necessario modificarli ora.
   * Nella cartella di lavoro è stata fornita una query di utilità che elenca, direttamente nella cartella di lavoro di Excel, tutti i tipi di attività esistenti se si desidera modificare queste informazioni in seguito

Potresti visualizzare alcuni pop-up relativi alla sicurezza. Considera attendibili le connessioni esterne e impostale su &#39;Pubblico&#39;. Se visualizzi il pop-up seguente, utilizza il contenuto di accesso web &quot;Anonimo&quot;. L’autenticazione per Marketo è gestita direttamente dalle query personalizzate, quindi non è necessario abilitare altri tipi di accesso.

#### Scarica dati Marketo

Assicurati innanzitutto che il parametro definito nell’area di ambito del foglio di lavoro di configurazione Marketo non comporti il download di troppi dati, superando il limite giornaliero di richieste API Marketo. Quando è pronto, fare clic sul pulsante &#39;Aggiorna tutto&#39; nel menu &#39;Dati&#39; e attendere che tutti i dati vengano scaricati nella cartella di lavoro. Se durante il download dei dati vengono visualizzati messaggi di errore di formattazione, simili a &quot;column1 not found&quot;, significa che una o più query non riescono a ottenere i dati e che la formattazione non riesce. Riprova più tardi, se l’errore persiste, verifica la versione di Excel (non utilizzare Excel 2016 da Office 365). È inoltre importante rispettare la latenza dalla piattaforma Marketo. Se si apportano modifiche in un elenco statico o nei dati del lead, è preferibile attendere prima di avviare le query di alimentazione.

### Modellazione dati in Power Pivot

Aprire Power Pivot facendo clic sul pulsante &#39;Gestisci&#39; nel menu Power Pivot, disponibile nella barra dei menu superiore (se non disponibile controllare la versione di Excel in uso, Power Pivot può essere installato come componente aggiuntivo in alcune versioni di Excel). Tutti i dati scaricati da Marketo e inviati al modello dati devono essere accessibili dalle diverse schede nella parte inferiore della finestra di Power Pivot.

### Espressioni di analisi dei dati (DAX)

È necessario arricchire o riformattare i dati per alcuni rapporti. Utilizzare le espressioni di analisi dei dati di Power Pivot (DAX) per definire alcuni calcoli personalizzati come colonne e misure calcolate (note anche come campi calcolati). Per ulteriori informazioni su DAX, vedere il collegamento &#39;DAX in Power Pivot&#39; nella sezione Riferimenti. Accertarsi che l&#39;area di calcolo sia visualizzata nella finestra di Power Pivot; in caso contrario, attivarla dal menu Home di Power Pivot.  Selezionare la scheda **MktoLeads** e aggiungere la misura **Conteggio lead** in qualsiasi punto dell&#39;area di calcolo lead: **Conteggio lead:=**&#x200B;**DISTINCTCOUNT**&#x200B;**([id])**. Questa misura sta conteggiando i lead distinti disponibili nell’elenco, in base al loro ID. Essa terrà conto anche degli eventuali filtri in essere nel contesto di una relazione. Questa misura non è realmente necessaria in quanto i rapporti sono in grado di sommare il numero di lead, ma abbiamo fatto in modo che il conteggio dei lead avesse un nome più appropriato rispetto alla &quot;somma di MktoLeads&quot;. È anche un esempio semplice che consente di immaginare facilmente alcune misure più complesse che fanno medie, min, max per un tipo specifico di immissione di dati (ad esempio, tutti i lead con un punteggio superiore a 50, un punteggio medio, ecc.). ...).  Selezionare la scheda **MktoWebActivities** e creare tre colonne calcolate. Per inserire le colonne calcolate seguenti, scorrere fino all&#39;estrema destra della tabella e fare clic sulla colonna &#39;Aggiungi colonna&#39;. **Attività:** Per ottenere l&#39;etichetta di attività intuitiva, cercare l&#39;ID attività nella tabella MktoActivityTypes. **\=**&#x200B;**LOOKUPVALUE**&#x200B;**(MktoActivityTypes[name],MktoActivityTypes[id],[activityTypeId])** **Anno-Mese:** riformattare la data attività con un modello &#39;YYYYmm&#39; più adatto per alcuni rapporti. **\=**&#x200B;**LEFT**&#x200B;**([activityDate],4)&amp;**&#x200B;**MID**&#x200B;**([activityDate],6,2)** **Date:** Activity Date è solo una stringa della query originale, trasformala in una data corretta. **\=**&#x200B;**DATE**&#x200B;**(**&#x200B;**LEFT**&#x200B;**([activityDate],4),**&#x200B;**MID**&#x200B;**([activityDate],6,2),**&#x200B;**MID**&#x200B;**([activityDate],9,2))** Ora creiamo le tre stesse misure per la scheda **MktoEmailActivities** e altre 2: **Campaign:** Ottieni l&#39;utente Nome della campagna ricercando l’ID campagna nella tabella MktoCampaigns. **\=**&#x200B;**LOOKUPVALUE**&#x200B;**(MktoCampaigns[name],MktoCampaigns[id],[campaignId])** **Programma:** Per ottenere il nome intuitivo del programma, cercare l&#39;ID della campagna nella tabella MktoCampaigns. La tabella MktoPrograms può fornire ulteriori dettagli sul Programma come la cartella, l&#39;area di lavoro, ecc. **\=**&#x200B;**LOOKUPVALUE**&#x200B;**(MktoCampaigns[programName],MktoCampaigns[id],[campaignId])**

### Entity-Relationships

In precedenza è stato possibile cercare informazioni da un&#39;altra tabella all&#39;interno del modello per completare alcune informazioni mancanti. Power Pivot offre un’opzione più potente per definire le relazioni tra alcune tabelle del modello dati, consentendoci di sfruttare tali relazioni direttamente dai rapporti. Lasciati definire le relazioni chiave per i nostri rapporti. Selezionare la Vista diagramma dalla finestra di Power Pivot. Traccia le seguenti relazioni all’interno del diagramma del modello dati:

* **MktoInterestingMomentActivities:leadId →** **MktoLeads:id**
* **MktoScoringActivities:leadId →** **MktoLeads:id**
* **MktoRevenueStageActivities:leadId →** **MktoLeads:id**
* **MktoWebActivities:leadId →** **MktoLeads:id**
* **MktoEmailActivities:leadId →** **MktoLeads: id**

Non utilizzeremo tutte queste relazioni e questi oggetti nei nostri rapporti, ma solo le attività Lead, Web e E-mail. Ora è il momento di creare alcuni rapporti.

### Grafico pivot delle prestazioni delle e-mail

Questo primo rapporto mostra i KPI relativi alle prestazioni delle e-mail basati su un grafico pivot standard di Excel. Ci consente di filtrare i dati per Settore e/o Campagna. È possibile creare un grafico pivot direttamente dal menu di Power Pivot selezionando Grafico pivot dal selettore Tabella pivot.  In alternativa, è possibile creare un grafico pivot direttamente dal foglio di calcolo di Excel, selezionando l&#39;opzione &#39;Utilizza il modello dati della cartella di lavoro&#39;.  Trascina e rilascia i campi dalle tabelle **MktoEmailActivities** e **MktoLeads**, come illustrato nella figura seguente: **MktoEmailActivities.Activity →** **Legend** (questa utilizza la colonna calcolata DAX implementata in **MktoEmailActivities** precedenti) **MktoEmailActivities.Date →** **Axis** (questa utilizza la colonna calcolata DAX implementata in **MktoEmailActivities** precedenti) **MktoEmailActivities.Id →** **∑ Valori** **MktoEmailActivities.Campaign →** **Filter** **MktoLeads.industry →** **Filter**

Puoi creare un nome personalizzato selezionando &quot;Impostazioni campo valore&quot; su ciascun campo rilasciato. In questo caso, abbiamo rilasciato il campo ID attività e-mail nella sezione &quot;Valori ∑&quot; e ne abbiamo modificato il nome personalizzato come &quot;Numero di attività&quot;. Ora configuriamo il grafico pivot. Fare clic con il pulsante destro del mouse direttamente sul grafico e selezionare l&#39;opzione &#39;Cambia tipo di grafico&#39; nel menu di scelta rapida. Ed è così che abbiamo selezionato il diverso tipo di grafico per tutte le serie di dati.

### Mappa dei lead con Power View

Il secondo rapporto visualizza i lead e i contatti per area geografica su una mappa del mondo e per settore. Per questo report è necessario Power View. Segui il link di riferimento qui sotto per attivare il menu in Excel. In alternativa, è possibile digitare &#39;visualizzazione risparmio energia&#39; nella casella di ricerca di Excel. Selezionare &#39;Inserisci report Power View&#39;.  Nel report Power View vuoto, selezionare la tabella **MktoLeads** nel pannello di destra e trascinare il campo della posizione del lead (ad esempio **inferredCity**). Ora nel menu principale viene visualizzato il menu Progettazione.

Passare alla visualizzazione Mappa selezionando Mappa nel menu Progettazione di Power View. Trascina e rilascia i campi dalla tabella **MktoLeads**, come illustrato nella figura seguente: **MktoLeads.industry →** **Color** **MktoLeads.inferredCity →** **Locations** **MktoLeads.Leads Count →** **∑ Size** (questa utilizza la misura DAX implementata in **MktoLeads** precedenti) E la mappa dei lead è pronta. È sufficiente modificare le dimensioni della mappa, personalizzare il titolo e le legende. Power View consente di creare dashboard avanzate con più grafici su un singolo foglio di calcolo. Consulta l&#39;esercitazione a cui si fa riferimento di seguito &#39;[Creare report di Amazing Power View](https://support.microsoft.com/en-us/article/Tutorial-Create-Amazing-Power-View-Reports-Part-1-e2842c8f-585f-4a07-bcbd-5bf8ff2243a7)&#39; per scoprire come procedere con altri componenti del dashboard con Power View.

### Attività web animate su una mappa 3D

Questo terzo rapporto mostra le attività web dei lead, per settore, su una mappa del mondo 3D. È necessaria una mappa 3D per questo rapporto. Digita semplicemente &#39;3D&#39; nella casella di ricerca di Excel e seleziona &#39;Mappa 3D&#39;. Creare una nuova presentazione dalla finestra popup.  Seleziona il Grafico a bolle nel pannello di destra. Trascina e rilascia i campi dalle tabelle **MktoLeads** e **MktoWebActivities**, come illustrato di seguito: **MktoLeads.industry →** **Category** **MktoLeads.inferredCity →** **Location** **MktoWebActivities.Activity →** **Time** (questa colonna viene utilizzata per il calcolo DAX implementato in **MktoWebActivities** in precedenza. Il campo ID può essere utilizzato anche per il conteggio delle attività.) **MktoWebActivities.Date →** **Time** (questa colonna calcolata DAX è stata implementata in **MktoWebActivities** precedenti) **MktoWebActivities.Activity** può essere utilizzata anche come filtro per filtrare i diversi tipi di attività Web.

Utilizzare il pulsante Temi per modificare la combinazione di colori della mappa 3D. Aprite la finestra Opzioni scena per personalizzare le animazioni.
E avete finito con la 3D World Map, ora potete divertirvi animando il mondo e creando video da esso.

### Passaggi successivi

Abbiamo appena grattato la superficie di ciò che è possibile fare con gli strumenti di Excel Power BI. Ti consigliamo di cercare sul web altri articoli e tutorial utili per espandere le tue competenze Excel e progettare i rapporti necessari per raggiungere gli obiettivi aziendali. Ci auguriamo che questi articoli ti siano piaciuti e che ti abbiano aiutato a sfruttare i grandi vantaggi combinati di Excel e Marketo.

### Riferimenti

#### Power Pivot

* [Power Pivot: analisi dati e modellazione dati potenti in Excel](https://support.microsoft.com/en-us/article/Power-Pivot-Powerful-data-analysis-and-data-modeling-in-Excel-d7b119ed-1b3b-4f23-b634-445ab141b59b)
* [Espressioni analisi dati (DAX) in Power Pivot](https://support.microsoft.com/en-us/article/Data-Analysis-Expressions-DAX-in-Power-Pivot-bab3fbe3-2385-485a-980b-5f64d3b0f730)

#### Power View

* [Attiva Power View in Excel 2016](https://support.microsoft.com/en-us/article/Turn-on-Power-View-in-Excel-2016-for-Windows-f8fc21a6-08fc-407a-8a91-643fa848729a)
* [Esercitazione: Creazione di report di Power View straordinari](https://support.microsoft.com/en-us/article/Tutorial-Create-Amazing-Power-View-Reports-Part-1-e2842c8f-585f-4a07-bcbd-5bf8ff2243a7)

Pubblicato il _2017-02-02_ da _Philippe_

## Modifica importante ai record di attività nell’API di Marketo

**Nota: questo post verrà aggiornato per riflettere le modifiche apportate ai record di attività restituiti dall&#39;API a causa della migrazione alla nuova infrastruttura.** **Ultimo aggiornamento: 13 settembre 2018** Con il rollout del servizio di attività di nuova generazione di Marketo a partire da settembre 2017, non sarà possibile applicare l&#39;univocità o la presenza del campo &quot;id&quot; intero nelle attività, nelle modifiche al valore dei dati o nei record di eliminazione dei lead restituiti dalle API di Marketo. Per evitare interruzioni del servizio per le integrazioni che recuperano i record di attività, il campo ID deve essere trattato come facoltativo. Il passaggio di questa modifica inizierà a influenzare gli abbonamenti e la prossima versione. Questa modifica interessa i seguenti endpoint: REST API

I tipi di SOAP interessati sono `ActivityRecord` e `LeadChangeRecord`.

### Esempi

Gli esempi seguenti mostrano i tipi di record che saranno interessati. In entrambi gli esempi, il campo interessato è denominato &quot;id&quot;.

**Esempio di campo REST: id**

```json
  {
      "id" : 102988,
      "leadId" : 1,
      "activityDate" : "2015-01-16T23:32:19Z",
      "activityTypeId" : 1,
      "primaryAttributeValueId" : 71,
      "primaryAttributeValue" : "localhost/munchkintest2.html",
      "attributes" : [
          {
              "name" : "Client IP Address",
              "value" : "10.0.19.252"
          },
          {
              "name" : "Query Parameters",
              "value" : ""
          },
          {
              "name" : "Referrer URL",
              "value" : ""
          },
          {
              "name" : "User Agent",
              "value" : "Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/39.0.2171.95 Safari/537.36"
          },
          {
              "name" : "Webpage URL",
              "value" : "/munchkintest2.html"
          }
      ]
  }
```

**Esempio di campo SOAP: id**

```xml
  <activityRecord>
    <id>1030680</id>
    <activityDateTime>2013-07-09T16:44:28-05:00</activityDateTime>
    <activityType>Visit Webpage</activityType>
    <mktgAssetName>ClickDemo</mktgAssetName>
    <activityAttributes>
      <attribute>
        <attrName>Webpage ID</attrName>
        <attrType xsi:nil="true" />
        <attrValue>2547</attrValue>
      </attribute>
      <attribute>
        <attrName>Webpage URL</attrName>
        <attrType xsi:nil="true" />
        <attrValue>/ClickDemo.html</attrValue>
      </attribute>
    </activityAttributes>
    <campaign />
    <personName xsi:nil="true" />
    <mktPersonId>1089965</mktPersonId>
    <foreignSysId xsi:nil="true" />
    <orgName xsi:nil="true" />
    <foreignSysOrgId xsi:nil="true" />
  </activityRecord>
```

Pubblicato il _2017-03-01_ da _Kenny_

## Aggiornamenti inverno 2017

Nella versione di inverno 2017, è stata aggiunta la possibilità di importare in blocco i dati di oggetti personalizzati in modo asincrono e di manipolare le variabili nelle pagine di destinazione guidate. Di seguito è riportato l&#39;elenco completo degli aggiornamenti.

### API del database lead

#### Importazione in blocco di oggetti personalizzati

Nuovi endpoint per supportare l&#39;importazione in blocco di oggetti personalizzati. I dettagli sono disponibili [qui](/help/rest-api/custom-objects.md).

#### Avviso delle modifiche imminenti alle attività

Tieni presente che si verificherà un cambiamento significativo quando Marketo rilascerà il servizio Activity Service di nuova generazione. Questa modifica influirà sui seguenti endpoint:

SOAP

[getLeadActivity](/help/soap-api/getleadactivity.md), [getLeadChanges](/help/soap-api/getleadchanges.md)

Il campo &quot;id&quot; intero contenuto nei record restituiti da questi endpoint non avrà più la garanzia di essere univoco. Questo influirà sui tipi di record di attività, modifica del valore dei dati e eliminazione dei lead. Per evitare interruzioni del servizio per le integrazioni che recuperano questi tipi di record, il campo ID deve essere trattato come facoltativo.

#### Rimozione token push

È stata aggiunta la possibilità di rimuovere i token push tramite l’API SDK. I dettagli sono disponibili qui: [iOS](/help/mobile/push-notifications.md), [Android](/help/mobile/push-notifications.md).

Pubblicato il _2017-03-01_ da _David_

## Aggiornamenti della primavera 2017

Nella versione di primavera 2017, è stata aggiunta la possibilità di estrarre in blocco i dati di lead e oggetti attività in modo asincrono e di manipolare gli elenchi di account denominati. Di seguito è riportato l&#39;elenco completo degli aggiornamenti.

### Estrazione in blocco di lead

Nuovi endpoint per supportare l’estrazione in blocco di lead. Specificare i criteri di selezione dei record utilizzando diverse opzioni. I dettagli sono disponibili [qui](/help/rest-api/bulk-lead-extract.md).

### Estrazione in blocco delle attività

Nuovi endpoint per supportare l’estrazione di attività in blocco. Specifica i criteri di selezione dei record utilizzando l’intervallo di date e l’elenco delle attività. I dettagli sono disponibili [qui](/help/rest-api/bulk-extract.md).

### Elenchi di account denominati

Nuovi endpoint per il supporto di operazioni CRUD negli elenchi di account denominati. I dettagli sono disponibili [qui](/help/rest-api/named-account-lists.md).

### Altri miglioramenti

* L&#39;endpoint [Ricevi campagne](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getCampaignByIdUsingGET) ora consente di filtrare le campagne &quot;attivabili&quot;. Ciò si ottiene passando &quot;isTriggerable=true&quot; come parametro di query.
* L&#39;endpoint [Clone Program](https://developer.adobe.com/marketo-apis/api/asset/#operation/cloneProgramUsingPOST) ora supporta programmi che contengono tutti i tipi di risorse ad eccezione di SMS Message.

### Ionico

È ora possibile utilizzare Marketo Mobile MME e con il framework dell&#39;applicazione [Ionic](https://ionicframework.com/). I dettagli sono disponibili [qui](/help/mobile/ionic.md).

### Annulla registrazione token di notifica push

È stato aggiunto un nuovo metodo a SDK che consente di annullare la registrazione del token di notifica push con Marketo. Questa funzione è utile, ad esempio, quando un utente esce dall’app. Per informazioni sull&#39;utilizzo, vedere `unregisterPushDeviceToken` (iOS) o il metodo `uninitializeMarketoPush`qui[&#128279;](/help/mobile/push-notifications.md) (Android).

Pubblicato il _2017-06-16_ da _David_

## Internet delle cose per gli addetti al marketing con IFTTT e Zapier

L&#39;Internet delle cose (IoT) è l&#39;interconnessione di dispositivi, apparecchi, dispositivi indossabili, veicoli, ecc. con elettronica, software, sensori e connettività di rete incorporati che consentono a questi oggetti di raccogliere e scambiare dati con i sistemi informativi cloud. Queste tecnologie stanno crescendo e si stanno evolvendo così rapidamente che avranno un impatto immediato su come viviamo, come lavoriamo e come lavoriamo. Marketo, la principale piattaforma di marketing Engagement, è pronta per l’IoT con le sue funzionalità di scalabilità e interazione con qualsiasi tipo di canale di comunicazione. Marketo è in grado di tenere traccia di oltre 70 tipi di attività correlate a e-mail, web, dispositivi mobili, gestione delle relazioni con i clienti e così via. Inoltre supporta [attività personalizzate](https://experienceleague.adobe.com/docs/marketo/using/product-docs/administration/marketo-custom-activities/create-a-custom-activity.html) che possono essere alimentate da qualsiasi sistema di terze parti. Gli [oggetti personalizzati](https://experienceleague.adobe.com/docs/marketo/using/product-docs/administration/marketo-custom-objects/understanding-marketo-custom-objects.html) di Marketo consentono di tenere traccia di tutti i tipi di metriche di terze parti correlate alla tua azienda e consentono agli addetti al marketing di sfruttare tali metriche direttamente dai filtri e dai trigger di campagne intelligenti di Marketo. L’implementazione di IoT per i consumatori richiederebbe un server centralizzato per interagire con i dispositivi dei consumatori e tale server scambierebbe dati con la piattaforma aperta Marketo, con funzionalità come API REST, oggetti personalizzati, attività personalizzate, ecc. - ha documentato [qui](http://eto.com/). Non è facile dimostrarlo tramite un post di blog. Invece di questo, integreremo il servizio IFTTT con Marketo per implementare alcuni interessanti casi d’uso IoT per gli addetti al marketing, come:

* Migliorare il team di marketing ogni volta che un lead viene registrato per un road show lampeggiando una luce colorata in ufficio
* Migliorare il team di vendita ogni volta che si vince un accordo attivando automaticamente una campana collegata a una presa di corrente collegata
* Pubblica automaticamente le tappe fondamentali del successo di Marketing sui social network come LinkedIn, Facebook, Slack, ecc. ...
* Avvia automaticamente una campagna di marketing in base a:
   * quando si verifica un allarme meteorologico (vento, temperatura, pioggia, ecc.)
   * quando un nuovo articolo viene pubblicato da un giornale come il New York Times, che soddisfa alcuni criteri specifici
   * quando il Senato o la Camera dei Rappresentanti degli Stati Uniti vota
   * quando la Stazione Spaziale Internazionale passa sopra una certa posizione
   * ecc. ...

Potresti trovare questi scenari divertenti ma inutili, ma sono qui per dimostrare un nuovo modo concettuale di fare Marketing non solo con le persone, ma anche con le cose nel nostro mondo connesso. Un altro punto interessante trattato in questo articolo è come sfruttare una piattaforma di integrazione web aperta come Zapier come &quot;tratteggio di servizio&quot; tra un sistema di terze parti e Marketo, ad esempio per gestire l’autenticazione.

### Servizio IFTTT

IFTTT è l&#39;acronimo di &quot;IF This Then That&quot;. Si tratta di un servizio gratuito basato sul web che le persone utilizzano per creare catene di semplici istruzioni condizionali, chiamate applet. Un&#39;applet viene attivata dalle modifiche che si verificano all&#39;interno di alcuni servizi Web partner e, di conseguenza, le azioni vengono inviate ad altri servizi Web partner. L&#39;IFTTT è stato lanciato nel 2011 da Linden Tibbets, Jesse Tane, Scott Tong e Alexander Tibbets a San Francisco. A prima vista, IFTTT sembra simile a un servizio come [Zapier](https://zapier.com/) per esempio, ha un focus molto più forte sui consumatori e dispositivi IoT (telecomandi, allarmi, luci, termostati, automobili, stampanti, telefoni cellulari e molto altro).  Innanzitutto, è necessario creare un account per IFTTT dal [sito Web IFTTT](https://ifttt.com/explore). Sentiti libero di scoprire tutte le fantastiche applet già disponibili in quanto questo ti darà alcune idee per altri scenari di sicuro!

### Il canale del produttore

Un’applicazione web che non dispone di un canale, ovvero una partnership con IFTTT, deve utilizzare il canale Maker. Con Maker Channel, puoi creare applet che funzionano con qualsiasi dispositivo o app in grado di effettuare o ricevere una richiesta web. Offre le seguenti integrazioni:

* Trigger in entrata per ricevere una richiesta web da un sistema di terze parti per attivare un’azione
* Azioni in uscita per rendere pubblicamente accessibile su Internet una richiesta web a un sistema di terze parti

Dall’IFTTT, cerca il servizio &quot;Maker&quot; e fai clic su di esso.  La prima volta, è necessario attivarlo facendo clic sul pulsante &quot;Connect&quot;.  Ora il canale del produttore è attivo. Puoi ottenere la tua chiave segreta facendo clic sul pulsante Impostazioni Maker: Copia e incolla l’URL fornito nel browser per ulteriori dettagli.

### Attivazione diretta di un&#39;azione IFTTT da parte del mercato

Innanzitutto, ci concentreremo sull’attivazione di tutti i tipi di azioni di servizi web di terze parti da parte di Marketo. Per questo utilizzeremo un [webhook di Marketo](https://experienceleague.adobe.com/docs/marketo/using/product-docs/administration/additional-integrations/create-a-webhook.html). Inizieremo con un messaggio push sul tuo cellulare o tablet tramite l’app mobile IFTTT, e poi implementeremo uno scenario IoT lampeggiando una luce Philips Hue.

### Webhook Marketo

Attivare un evento da Marketo, che funge da &quot;se&quot; dell’IFTTT, è semplice. Tutto ciò che devi fare è inviare una richiesta web POST a IFTTT con un nome evento e la chiave segreta, seguendo questo URL di modello:

`<https://maker.ifttt.com/trigger/{event_name}/with/key/{secret_key}>`

Il Maker consente anche di comunicare fino a 3 parametri tramite la richiesta web. Questa operazione può essere eseguita utilizzando i parametri di query,

`<https://maker.ifttt.com/trigger/{event_name}/with/key/{secret_key}?value1={value1}&value2={value2}&value3={value3}>`

o utilizzando un corpo JSON composto da un massimo di tre valori:

`{ "value1" : "{value1}", "value2" : "{value2}", "value3" : "{value3}" }`

In Marketo, crea un nuovo webhook dall’interfaccia di amministrazione.  Fornisci le seguenti informazioni per il nuovo webhook:

**Nome webhook:** programma IFTTT completato

**Descrizione:** attiva un evento su IFTTT da una campagna avanzata per il successo di un programma

**URL:** `<https://maker.ifttt.com/trigger/{event_name}/with/key/{secret_key}?value1={{program.name}}&value2={{lead.Email> Address}}&value3={{lead.Full Name}}`

event_name, utilizza MarketoProgramSuccess, ad esempio

secret_key, utilizza la chiave segreta del servizio di creazione IFTTT

Utilizza token di testo statico o Marketo per i tre valori disponibili. Puoi inviare messaggi più interattivi definendo i tuoi token a livello di programma e trasmetterli attraverso questi valori.

**Tipo richiesta:** POST

**Modello:** Lascia vuoto

**Codifica Token Di Richiesta:** Modulo/Url

**Tipo di risposta:** Nessuno

Non è necessario definire una mappatura di risposta.

### Applicazione IFTTT

Nel portale web IFTTT, seleziona &quot;My Applets&quot; nel menu principale.  Fare clic sul pulsante &quot;New Applet&quot; e fare clic sulla sezione **+this**.  Cerca il servizio Maker.  Crea il trigger che verrà attivato ogni volta che il servizio Maker riceve una richiesta Web per la notifica di un evento. Utilizza lo stesso Nome evento specificato nell’URL del webhook Marketo, ad esempio &quot;MarketoProgramSuccess&quot; e fai clic sul pulsante &quot;Crea trigger&quot;.  È ora di specificare il servizio Action facendo clic sulla sezione **+che**.  Inizieremo con un semplice servizio di azione che chiunque sarebbe in grado di testare senza dover investire in alcun dispositivo IoT, il servizio di notifiche. Cerca e seleziona il servizio Notifiche.
Scegli l’azione &quot;Send a notification&quot; (Invia una notifica) che invia una notifica ai dispositivi.  Puoi sfruttare i 3 valori inviati da Marketo aggiungendoli come Ingredienti per inviare una notifica significativa all’utente, proprio come nell’esempio seguente ... E quindi fai clic sul pulsante &quot;Crea azione&quot;. Rivedere e terminare l&#39;applet IFTTT. Assicurati che sia abilitato.

### Verifica dell&#39;applet IFTTT

Se desideri ricevere una notifica sul tuo dispositivo mobile, devi prima scaricare l’app IFTTT per il tuo dispositivo.  Puoi attivare un evento di successo del programma Marketo utilizzando il webhook in un flusso di Marketo Smart Campaign. Ricorda che i webhook di Marketo funzionano esclusivamente con campagne intelligenti attivate (ad esempio, attivano una volta che un modulo di contatto compilato, è stato aggiunto a un elenco, ecc.).  Ecco un esempio di una notifica IFTTT sul cellulare.

### Creative con IFTTT

IFTTT offre Applet Actions con oltre 300 partner, quindi il tuo portfolio di app e appliance insieme alla tua immaginazione sono i limiti ... Facciamo un esempio con le [luci Hue di Philips](https://www.philips-hue.com/en-us) che puoi acquistare ovunque nei negozi di elettronica o online. Gli applet seguenti lampeggiano una delle luci con il colore attualmente assegnato quando Marketo attiva un programma di successo, che potrebbe stimolare il tuo team di marketing in ufficio. Creiamo un nuovo Applet, seguendo gli stessi passaggi di prima, in cui Marketo viene attivato con un webhook, ma questa volta scegliamo l’azione dal servizio Philips Hue.

Selezionare l&#39;azione &quot;Lampi intermittenti&quot;. L&#39;app richiederà a Philips Hue tutte le luci disponibili, in modo da poter scegliere quella da lampeggiare. Dovresti prima impostare un account con Philips Hue, il ponte Hue e naturalmente almeno una lampadina Hue, una striscia luminosa, un proiettore o una lampada.  Abbiamo appena aggiunto un nuovo Applet che lampeggerà con una luce colorata ogni volta che un lead viene registrato in un roadshow o webinar. Il tuo team di marketing sarà lieto di offrirti quotidianamente una configurazione del genere in ufficio.

### Esecuzione di un&#39;azione Marketo da IFTTT tramite Zapie

Ora attiveremo una campagna Marketo Smart Campaign dalla piattaforma IFTTT. Per questo utilizzeremo [API REST di Marketo](/help/rest-api/rest-api.md). Poiché questa API è protetta e richiede un’autenticazione OAuth2 prima di richiamare qualsiasi cosa, è necessario gestire tale autenticazione tramite un’altra piattaforma come Zapier, perché IFTTT non consente di concatenare due chiamate consecutive su un’API con il canale Maker. Abbiamo scelto [Zapier](https://zapier.com/) Web app Automation Service da quando abbiamo pubblicato già l&#39;introduzione di Zapier e spiegato passo passo come implementare un connettore Marketo personalizzato per Zapier. Anche altre piattaforme come [Workato](https://www.workato.com/integrations/marketo) potrebbero essere una soluzione.

### Marketo Campaign

Crea il tuo programma Marketo con una campagna avanzata pianificata. A scopo di test, è possibile creare la seguente Smart Campaign come esempio: **Smart List** Utilizza solo filtri, non trigger. Assicurati almeno di essere idoneo. **Flusso** ti invia un&#39;e-mail o qualsiasi altro tipo di notifica. **Pianifica** Assicurati di poter eseguire il flusso ogni volta per gestire i test multipli. Puoi ottenere l’ID della campagna avanzata dall’URL. Esempio: _https://{{marketo_url}}/#SC4289A1_ - l&#39;ID della campagna avanzata sarà 4289. Puoi attivare questa campagna tramite l’API REST di Marketo. Puoi ad esempio utilizzare il plug-in [Postman](https://www.postman.com/) per Chrome e inviare le due seguenti chiamate HTTPS consecutive: **Passaggio di autenticazione:**

`https://{{Your Munchkin_Account_id}}.mktorest.com/identity/oauth/token?grant_type=client_credentials&client_id={{Your_Client_Id}}&client_secret={{Your_Client_Secret}}`

Recupera il token di accesso dalla risposta JSON. **Passaggio di avvio della campagna:**

`https://{{Your_Munchkin_Account_id}}.mktorest.com/rest/v1/campaigns/{{Campaign_Id}}/schedule.json?access_token={{access_token}}`

### Connettore personalizzato intermedio Zapier per avviare la campagna Marketo

Dobbiamo creare un connettore Zapier personalizzato che si autentichi con l’API REST di Marketo e dia il via alla nostra Smart Campaign.

* Prerequisiti
* Implementazione del connettore Marketo per Zapier
* Utilizza un titolo diverso, ad esempio &quot;Marketo Campaign&quot;
   * Eseguire il passaggio &quot;Autenticazione&quot;
   * Eseguire il passaggio &quot;Triggers&quot; (necessario per lo scopo del test Zapier)
   * Effettua il seguente passaggio specifico, responsabile del lancio di una campagna Marketo, descritto di seguito:

Dove inviare l’URL dell’endpoint dell’azione sui dati:

`https://{{munchkin_account_id}}.mktorest.com/rest/v1/campaigns/{{CampaignId}}/schedule.json`

Lascia vuoti gli altri campi facoltativi.

#### API di scripting

La funzione di scripting di Zapier consente di manipolare le richieste e le risposte scambiate tra l’API dell’app e Zapier. Puoi modificare le richieste HTTP immediatamente prima che vengano inviate e analizzare le risposte prima che Zapier esegua qualsiasi operazione con esse. È necessario per completare l’autenticazione personalizzata &quot;Autenticazione sessione&quot;. Ulteriori informazioni sono disponibili nell’articolo originale. Copiare il seguente codice molto simile all&#39;originale, abbiamo appena modificato i metodi di azione:

```javascript
var Zap = {

 get_session_info: function(bundle) {

 console.log('Entering get_session_info method ...');

 var access_token,
 access_token_request_payload,
 access_token_response;


 // Assemble the meta data for our Access Token swap request
 console.log('building Request with client_id=' + bundle.auth_fields.client_id + ', and client_secret=' + bundle.auth_fields.client_secret);
 access_token_request_payload = {
 method: 'POST',
 url: 'https://' + bundle.auth_fields.munchkin_account_id + '.mktorest.com/identity/oauth/token',
 params: {
 'grant_type' : 'client_credentials',
 'client_id' : bundle.auth_fields.client_id,
 'client_secret' : bundle.auth_fields.client_secret
 },
 headers: {
 'Content-Type': 'application/json', // Could be anything.
 Accept: 'application/json'
 }
 };

 // Fire off the Access Token request.
 access_token_response = z.request(access_token_request_payload);

 // Extract the Access Token from returned JSON.
 access_token = JSON.parse(access_token_response.content).access_token;
 console.log('New Access_Token=' + access_token);

 // This will be mixed into bundle.auth_fields in future calls.
 //bundle.auth_fields.access_token=access_token;
 return {'access_token': access_token};
 },

 test_trigger_pre_poll: function(bundle) {

 console.log('Entering test_trigger_pre_poll method ...');

 bundle.request.params = {
 'access_token':bundle.auth_fields.access_token
 };

 return bundle.request;

 },

 test_trigger_post_poll: function(bundle) {

 console.log('Entering test_trigger_post_poll method ...');

 var data = JSON.parse(bundle.response.content);
 if ((!data.success)&&((data.errors[0].code=="601")||(data.errors[0].code=="600"))){
 console.log('Access Token expired or invalid, requesting new one - data.success=' + data.success + ', data.errors[0].code=' + data.errors[0].code);

 throw new InvalidSessionException(); // Calling get_session_info() to regenerate Access Token
 }

 return JSON.parse(bundle.response.content);
 },

 launch_campaign_pre_write: function(bundle) {

 bundle.request.params = {'access_token':bundle.auth_fields.access_token};
 return bundle.request;
 },

 launch_campaign_post_write: function(bundle) {

 var data = JSON.parse(bundle.response.content);
 if ((!data.success)&&((data.errors[0].code=="601")||(data.errors[0].code=="600"))){
 console.log('Access Token expired or invalid, requesting new one - data.success=' + data.success + ', data.errors[0].code=' + data.errors[0].code);
 throw new InvalidSessionException(); // Calling get_session_info() to regenerate Access Token
 }
 return JSON.parse(bundle.response.content);
 }

};
```

##### Nuovo Zap

Dalla dashboard di Zapier, fai clic sul pulsante &quot;Crea un nuovo Zap&quot;. **Attivatore**

* Scegli l’app trigger &quot;Webhook by Zapier&quot;
* Seleziona &quot;Catch Hook&quot; (Aggancio)
* Non è necessario selezionare una chiave figlio
* Zapier ha generato un URL del webhook personalizzato con cui puoi inviare richieste e tenerlo al sicuro da qualche parte
* Verifica l’URL del webhook avviando lo scenario &quot;Applet IFTTT che chiama il webhook Zapier&quot; di seguito. Questo consente a Zapier di imparare il payload del webhook e di assegnare l’ID della campagna all’azione

**Azione**

* Seleziona il connettore Marketo Campaign creato in precedenza
* Scegli l&#39;unica azione disponibile: **Avvia campagna**
* Connettiti al tuo account Marketo, compilando i parametri di autenticazione (ID account Munchkin, ID client, segreto client)
* Modifica il modello e associa l’ID campagna dal trigger al parametro ID campagna &quot;Launch Campaign&quot;
* Verifica il passaggio e controlla che venga avviata la campagna Marketo

### Applet IFTTT che chiama il webhook Zapier

Cominciamo con uno scenario semplice, facile da testare. Scegli in IFTTT un trigger di data e ora che darà il via alla campagna Marketo ogni ora. L’azione è una richiesta web che viene inviata all’URL del webhook Zapier e che trasmette l’ID della campagna avanzata. Assicurati che Zapier Zap e l’applet IFTTT siano entrambi attivi e verifica che tutto funzioni come previsto.

### Creative con IFTTT

IFTTT offre Applet Triggers con oltre 300 partner, quindi anche in questo caso il tuo portfolio di app e appliance insieme alla tua immaginazione sono i limiti ... Prendiamo un esempio con il servizio [Meteo sotterraneo](https://www.wunderground.com/) che useremo per lanciare la nostra campagna Marketo sull&#39;allerta meteo. Il trigger seguente prenderebbe il via quando viene annunciata una condizione di pioggia. Quindi associa il trigger all’azione Maker Webhook, e come in precedenza compila i parametri del webhook Zapier.  Et voilà, ora serve solo una buona pioggia per verificare che questo stia funzionando.

Ci auguriamo che tu ti diverta molto applicando i concetti forniti in questo articolo. Ma soprattutto, pensiamo che aiuterà chiunque voglia integrare Marketo con altri sistemi di terze parti, grazie ai concetti chiave di questo articolo:

* API REST di Marketo
* Webhook Marketo
* Come sfruttare una piattaforma di integrazione web aperta come Zapier come &quot;tratteggio di servizio&quot; tra un sistema di terze parti e Marketo, per gestire ad esempio l’autenticazione

Pubblicato il _2017-06-20_ da _Philippe_

## Aggiornamenti dell’estate 2017

Nella versione dell’estate 2017, verranno rilasciati alcuni miglioramenti minori alle API del programma.

### Sfoglia programmi

Stiamo aggiungendo la possibilità di ottenere programmi per intervallo di date all&#39;endpoint [Get Programs](https://developer.adobe.com/marketo-apis/api/asset/#operation/browseProgramsUsingGET) tramite l&#39;aggiunta dei parametri opzionali earlyUpdatedAt e latestUpdatedAt. È possibile impostare uno o entrambi i parametri con il datetime scelto per restituire solo i programmi creati o aggiornati tra le due date. Questo è utile per recuperare set nuovi e aggiornati di materiale promozionale di marketing, soprattutto per casi di utilizzo di traduzione e business intelligence.

Pubblicato il _1970-01-01_ da _Kenny_

## Estensione della logica di business di Marketo con la funzione cloud di Google

Questo articolo propone una soluzione per estendere Marketo con alcune funzionalità di logica di business con Google Cloud Platform (GCP), basata sul seguente semplice esempio: 3 campi personalizzati nel record Lead di Marketo:

* **OnLinePreference**: un punteggio incrementale che indica una maggiore propensione dei potenziali clienti a comunicare in linea.
* **OfflinePreference**: un punteggio incrementale che indica una maggiore propensione dei clienti e dei potenziali clienti a comunicare in modalità offline.
* **Preferenza**: campo calcolato da GCP che visualizza &quot;offline&quot; se il punteggio offline è superiore a quello online e &quot;online&quot; viceversa

Questa tecnologia apre la strada a una logica di business più avanzata e alla fine alla chiamata di servizi web esterni, trasformando e consolidando i risultati in Marketo.  

### Informazioni sulla piattaforma e sulle funzioni di Google Cloud

[Google Cloud Platform](https://cloud.google.com/) (GCP) è una suite di servizi di cloud computing che viene eseguita sulla stessa infrastruttura utilizzata internamente da Google per i prodotti degli utenti finali, ad esempio Google Search e YouTube. Oltre a un set di strumenti di gestione, fornisce una serie di servizi cloud modulari tra cui elaborazione, archiviazione dei dati, analisi dei dati, apprendimento automatico, big data e molto altro. Avremmo potuto utilizzare molti servizi GCP diversi per le nostre esigenze, come Compute Engine, App Engine o Kubernetes Engine, ma abbiamo scelto le [funzioni cloud](https://cloud.google.com/functions) (ancora in Beta) per i seguenti vantaggi principali:

* Il cloud computing senza server è il luogo in cui la logica può essere attivata on-demand in risposta a eventi come le chiamate HTTP.
* Allevia la maggior parte dei problemi causati dalla manutenzione e dall&#39;installazione dei server.
* Conveniente, poiché paghi GCP solo per ogni chiamata di funzione e non per mantenere un server operativo.
* Implementazione semplice e veloce, in quanto l&#39;attenzione è rivolta solo alla logica dell&#39;applicazione.
* Scalabilità automatica, pronta per carichi di lavoro molto elevati.

Per ulteriori informazioni su questa tecnologia e sui relativi prezzi, visitare il sito Web [GCP](https://cloud.google.com/). In genere, questo tutorial non dovrebbe indurre alcun costo importante e si adatta perfettamente all’interno del credito gratuito di una prova GCP.  

### Preparazione dell’ambiente Google Cloud

È necessario un account Google Cloud. Puoi provare GCP gratuitamente con un credito più che sufficiente per eseguire questa esercitazione, fai clic sul pulsante &quot;Prova gratis&quot; sul [sito Web GCP](https://cloud.google.com/). Segui tutti i passaggi della sezione &quot;Prima di iniziare&quot; nella [esercitazione HTTP](https://cloud.google.com/functions/docs/calling) da Google:

1. Creare un progetto Cloud Platform: VAI ALLA PAGINA GESTIONE RISORSE
1. Abilita fatturazione per il progetto: [ABILITA FATTURAZIONE](https://cloud.google.com/billing/docs/how-to/modify-project?visit_id=638816637273392093-1926929734&rd=1)
1. Abilita l&#39;API delle funzioni cloud: [ABILITA L&#39;API](https://accounts.google.com/InteractiveLogin?continue=https://console.cloud.google.com/flows/enableapi?apiid%3Dcloudfunctions%26redirect%3Dhttps://cloud.google.com/functions/docs/tutorials/http&followup=https://console.cloud.google.com/flows/enableapi?apiid%3Dcloudfunctions%26redirect%3Dhttps://cloud.google.com/functions/docs/tutorials/http&osid=1&passive=1209600&service=cloudconsole&ifkv=ASKV5Mh81NGNsqcJqhx7hst0KFnyA0MJ-2zay8ovyluBfpvDoM820nF9Wq_SKbC1m_sjQvvRNoKSuA)
1. [Installa e inizializza Cloud SDK](https://cloud.google.com/sdk/docs/)
1. Aggiornare e installare i componenti cloud

   **aggiornamento componenti cloud&amp;** **installazione beta componenti cloud**

1. Prepara l&#39;ambiente per lo sviluppo di Node.js: [VAI ALLA GUIDA ALLA CONFIGURAZIONE](https://cloud.google.com/nodejs/docs/setup)

### Implementazione della funzione scoreCompare Cloud

1. Crea un bucket di archiviazione cloud per posizionare nell’area intermedia i file delle funzioni cloud. Puoi eseguire questa operazione con la riga di comando:

   `gsutil mb gs://[YOUR_STAGING_BUCKET_NAME]`

   oppure dall’interfaccia web di Google Cloud, selezionando il progetto e facendo clic sul menu Archiviazione:
   * Assegna un nome univoco al bucket di archiviazione
   * Selezionare la classe di archiviazione predefinita
   * Seleziona la località geografica più adatta

1. Crea una directory sul sistema locale per il codice dell’applicazione.
1. Crea un file &quot;index.js&quot; in questa directory con il seguente codice JavaScript: il codice è molto semplice da capire. Analizza i due parametri di input dal corpo della richiesta HTTP in JSON, esegue l’elaborazione e codifica in JSON la risposta HTTP.

```javascript
 /\*\*
     \* HTTP scoreCompare Cloud Function.
     \*
     \* @param {Object} req Cloud Function request context.
     \* @param {Object} res Cloud Function response context.
     \*/
    exports.scoreCompare = function scoreCompare (req, res) {
     var onlineScore=parseInt(req.body.onlineScore);
     var offlineScore=parseInt(req.body.offlineScore);
     console.log('/scoreCompare: got values onlineScore =' + onlineScore + ', offlineScore =' + offlineScore);
     var result;
     if (onlineScore>offlineScore) {result = 'online';} else {result = 'offline';}
     console.log('/scoreCompare: and result is ' + result);
     res.status(200).json({output: result}).end();
    };
```

Distribuisci il punteggio della funzioneConfronta con un trigger HTTP. Esegui il comando seguente dalla directory:

**le funzioni beta cloud distribuiscono [FUNCTION] —stage-bucket [YOUR_STAGING_BUCKET_NAME] —trigger-http**

Dove [YOUR_STAGING_BUCKET_NAME] è il nome del bucket dell&#39;archiviazione cloud di staging. Nel nostro esempio:

**gcloud beta functions deploy scoreCompare —stage-bucket mktostorage —trigger-http**

1. Osserva l&#39;URL della funzione cloud (httpsTrigger URL) dall&#39;output della console, che si presenta così: `https://[YOUR_REGION]-[YOUR_PROJECT_ID].cloudfunctions.net/[FUNCTION]` dove

   * [AREA_PERSONALE] è l&#39;area in cui è distribuita la funzione. Questo è visibile nel terminale al termine della distribuzione della funzione.
   * [IL_TUO_ID_PROGETTO] è l&#39;ID del tuo progetto cloud. Questo è visibile nel terminale al termine della distribuzione della funzione.
   * [FUNCTION] è il nome della funzione.

   Nel nostro esempio: [**https://us-central1-marketo-cloud-logic.cloudfunctions.net/scoreCompare**](https://us-central1-marketo-cloud-logic.cloudfunctions.net/scoreCompare)
1. Prova la tua funzione con uno strumento come [Postman](https://www.postman.com/):
   * Verbo HTTP: POST
   * URL: [https://us-central1-marketo-cloud-logic.cloudfunctions.net/scoreCompare](https://us-central1-marketo-cloud-logic.cloudfunctions.net/scoreCompare)
   * Intestazioni: content-type = application/json
   * Corpo: {&quot;onlineScore&quot;:110, &quot;offlineScore&quot;:200}L&#39;output deve fornire: {&quot;output&quot;: &quot;offline&quot;}.

### Chiamare la funzione cloud dal webhook di un Marketo

È necessario creare i tre campi personalizzati seguenti nel record Lead in Marketo:

* **OnlinePreference**: numero intero
* **OfflinePreference**: numero intero
* **Preferenza**: stringa

Crea il seguente webhook dall’interfaccia di amministrazione di Marketo utilizzando l’URL della funzione cloud &quot;scoreCompare&quot; e i token del campo personalizzato. Prova il webhook con una campagna intelligente attivata da Marketo.

* **I webhook di Marketo possono essere richiamati solo da campagne intelligenti attivate, non da campagne intelligenti batch.**
* **Se non utilizzi la funzione cloud, eliminala o elimina l&#39;intero progetto, per evitare di dover sostenere spese per l&#39;account Google Cloud Platform.**

Ci auguriamo che questo tutorial sia stato utile e che possa farti pensare a scenari più avanzati che coinvolgono elaborazioni complesse e servizi di terze parti. Un buon esempio potrebbe essere quello di sfruttare Google Cloud AI, i servizi di apprendimento automatico di Google. Ad esempio, potresti analizzare del testo libero da un modulo di Marketo e chiedere all’API di Google Natural Language di rivelare la struttura e il significato del testo per poi salvare questa analisi in Marketo; aprendo semplicemente le porte alle idee.

Pubblicato il _2017-11-21_ da _Philippe_

## Aggiornamenti di autunno 2017

Nella versione di autunno 2017 di, sono stati rilasciati diversi miglioramenti alle API di Asset. Di seguito è riportato l&#39;elenco completo degli aggiornamenti.

### Sfoglia programmi per intervallo di date

È stata aggiunta la possibilità di ottenere programmi per intervallo di date all&#39;endpoint [Get Programs](https://developer.adobe.com/marketo-apis/api/asset/#operation/cloneEmailUsingPOST). Questa operazione viene eseguita utilizzando i parametri `earliestUpdatedAt` e `latestUpdatedAt`. È possibile impostare uno o entrambi i parametri con il datetime scelto per restituire solo i programmi creati o aggiornati tra le due date.

### Anteprima e-mail

Molti utenti ora visualizzano in anteprima un messaggio e-mail utilizzando l&#39;endpoint [Ottieni contenuto e-mail completo](https://developer.adobe.com/marketo-apis/api/asset/#operation/getEmailFullContentUsingGET), che restituisce la versione serializzata di un messaggio e-mail in HTML. Viene eseguito il rendering completo di tutti i token, i frammenti, il contenuto dinamico e i componenti incorporati. È possibile passare un parametro facoltativo **leadId** per rappresentare un lead specificato.

### Sostituisci HTML di E-mail 2.0

È stato aggiunto l&#39;endpoint [Aggiorna contenuto e-mail completo](https://developer.adobe.com/marketo-apis/api/asset/#operation/createEmailFullContentUsingPOST) per consentire la sostituzione di blocchi di contenuto e-mail di HTML. Se modifichi il codice HTML di un&#39;e-mail di Marketo utilizzando l&#39;Editor e-mail 2.0 di Marketo, la relazione tra l&#39;e-mail e il relativo modello non funzionerà più, ulteriori informazioni su [qui](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/email-marketing/general/functions-in-the-editor/edit-an-emails-html). Utilizzando questo endpoint, puoi aggiornare in modo programmatico il contenuto HTML di un messaggio e-mail la cui relazione è stata interrotta. Inoltre, abbiamo modificato tutti gli altri endpoint relativi al ciclo di vita delle e-mail in modo che siano compatibili con le e-mail in cui la relazione è stata interrotta:

* Approva bozza e-mail
* Annulla approvazione e-mail
* Elimina e-mail
* Elimina bozza e-mail
* Clona e-mail
* Aggiorna metadati e-mail

### Altri miglioramenti

* L’intervallo di tempo massimo per i filtri dell’intervallo di date è stato aumentato a 31 giorni. Si tratta di [filtri di estrazione lead bulk](/help/rest-api/bulk-lead-extract.md) (createdAd o updatedAt) e [filtro di estrazione attività bulk](/help/rest-api/bulk-activity-extract.md) (createdAt).

Pubblicato il _2017-12-15_ da _David_

## TEST: collegamento video esterno nella community

[Video tutorial sul Power Pack per il recapito messaggi e-mail](https://nation.marketo.com:443/t5/product-space-archive-videos/email-deliverability-power-pack-tutorial-video/m-p/283550)  

Pubblicato il _1970-01-01_ da _David_

## Aggiornamenti inverno 2018

Nella versione di inverno 2018, sono stati rilasciati alcuni miglioramenti alle API. Di seguito è riportato l&#39;elenco completo degli aggiornamenti.

### Attiva/Disattiva campagne trigger

È stata aggiunta la possibilità di attivare e disattivare le campagne trigger, che possono semplificare il processo di automazione dei modelli di programma. Ciò si ottiene chiamando due endpoint appena aggiunti: [Attiva Smart Campaign](https://developer.adobe.com/marketo-apis/api/asset/#operation/activateSmartCampaignUsingPOST), [Disattiva Smart Campaign](https://developer.adobe.com/marketo-apis/api/asset/#operation/deactivateSmartCampaignUsingPOST). Per informazioni dettagliate, consulta la sezione Trigger nella documentazione di [Campagne](/help/rest-api/assets.md).

### Ottieni programma per nome

Sono stati aggiunti due parametri all&#39;endpoint [Ottieni programma per nome](https://developer.adobe.com/marketo-apis/api/asset/#operation/getProgramByNameUsingGET) per facilitare il recupero dei costi e dei tag del programma. Per informazioni dettagliate, consulta i parametri **includeCosts** e **includeTags** nella documentazione di [Programmi](/help/rest-api/assets.md).

### Altri miglioramenti

L’API di estrazione in blocco è ora in grado di riconoscere l’area di lavoro. Quando si crea un utente con accesso solo API per un [servizio personalizzato](/help/rest-api/custom-services.md), è necessario selezionare un ruolo utente con accesso API per una o più aree di lavoro. In precedenza, al servizio personalizzato era consentito l’accesso a tutte le aree di lavoro. Ora il servizio personalizzato può accedere solo alle aree di lavoro selezionate durante la creazione utente di sola API.

Pubblicato il _2018-03-02_ da _David_

## Aggiornamenti della primavera 2018

Nella versione di primavera 2018 vengono rilasciate nuove API REST e miglioramenti alla privacy per il tracciamento web. Di seguito è riportato l&#39;elenco completo degli aggiornamenti.

REST API

### CRUD elenco statico

Consente agli utenti di creare, leggere, aggiornare ed eliminare in modalità remota record di elenchi statici. Consente la gestione dell’intero ciclo di vita di un elenco statico tramite API REST, incluso il popolamento e il mantenimento dell’iscrizione. I dettagli sono disponibili [qui](/help/rest-api/static-lists.md).

### Metadati attività personalizzati

Consente agli utenti di creare in remoto record di attività personalizzati. Consente la gestione dell’intero ciclo di vita delle attività personalizzate tramite API REST, inclusa la manipolazione dei tipi e degli attributi dei tipi. I dettagli sono disponibili [qui](/help/rest-api/activities.md).

### Metadati elenco avanzato

Consente agli utenti di leggere, clonare ed eliminare in modalità remota i record degli elenchi avanzati. Consente la gestione degli elenchi avanzati tramite API REST. I dettagli sono disponibili [qui](/help/rest-api/smart-lists.md).

### Privacy tracciamento web

Il codice di tracciamento web di Munchkin JavaScript è stato migliorato per includere i seguenti miglioramenti relativi alla privacy:

* Rinuncia: possibilità di consentire ai visitatori di rinunciare definitivamente al tracciamento web. I dettagli sono disponibili [qui](/help/javascript-api/lead-tracking.md).
* Anonimizzazione degli indirizzi IP: rispetta le normative locali e internazionali sulla privacy fornendo la possibilità di rendere anonimi gli indirizzi IP dei visitatori web. Vedi **anonymizeIP** parametro di configurazione [qui](/help/javascript-api/configuration.md).

### Altri miglioramenti

* L&#39;endpoint [Get Folders](https://developer.adobe.com/marketo-apis/api/asset/#operation/getFolderUsingGET) ora restituisce tutte le cartelle quando sono specificati un elemento padre non radice e maxDepth=1.
* L&#39;endpoint [Ottieni pagina di destinazione per ID](https://developer.adobe.com/marketo-apis/api/asset/#operation/getLandingPageByIdUsingGET) ora restituisce gli attributi URL con il protocollo in tutti i casi (http:// o https://).

Pubblicato il _2018-06-29_ da _David_

## Aggiornamenti dell’estate 2018

La versione dell’estate 2018 è principalmente una versione di manutenzione composta da miglioramenti minori e risoluzioni dei difetti. Di seguito è riportato l&#39;elenco completo degli aggiornamenti.

### REST API

* È stato aggiunto il supporto per i campi di disposizione e-mail omessi inutilmente in origine. Questi campi saranno ora disponibili per la lettura e la scrittura su REST, a seconda delle necessità.
   * Inserito in lista bloccati
   * Marketing sospeso
   * E-mail sospesa
   * Urgenza relativa
* L&#39;endpoint [Get Leads by Filter Type](https://developer.adobe.com/marketo-apis/api/lead-database-endpoint-reference/#/Leads/getLeadsByFilterUsingGET) ora supporta leadPartionId come filterType.

### Risoluzioni dei difetti

**Endpoint REST**

**Descrizione**

[Approva programma](https://developer.adobe.com/marketo-apis/api/asset/#operation/approveProgramUsingPOST)

Se durante la creazione del programma hai impostato Blocca e-mail non operative su false, una chiamata ad Approve Program (Approva programma) viene reimpostata su true.

[Estrai in blocco](/help/rest-api/bulk-extract.md)

Alcuni caratteri Unicode sono danneggiati nel file di output di estrazione.

[Clona programma](https://developer.adobe.com/marketo-apis/api/asset/#operation/cloneProgramUsingPOST)

* Se hai clonato Email Program (Programma e-mail), la logica del filtro SmartList è stata reimpostata su &quot;All&quot; (Tutti) nel programma risultante, indipendentemente dall’impostazione iniziale.
* Se si è tentato di clonare un programma che conteneva un elenco statico (che era stato eliminato), è stato visualizzato il messaggio di errore 709 &quot;Le risorse seguenti non sono supportate:List&quot;.
* Se hai provato a clonare un programma in più aree di lavoro, hai ricevuto un errore 611, &quot;Impossibile clonare il programma&quot;.

[Ottieni elenco statico per ID](https://developer.adobe.com/marketo-apis/api/asset/#operation/getStaticListByIdUsingGET)

Se il servizio personalizzato disponesse dell’autorizzazione per il ruolo Risorsa di sola lettura, riceveresti un errore 603, &quot;Accesso negato&quot;.

[Invia lead a Marketo](https://developer.adobe.com/marketo-apis/api/mapi/#operation/pushToMarketoUsingPOST)

Se l&#39;attributo **cookies** è stato specificato in un oggetto lead dell&#39;array **input**, l&#39;attività anonima precedente non è stata associata al lead appena creato.

[Pianifica campagna](https://developer.adobe.com/marketo-apis/api/mapi/#operation/scheduleCampaignUsingPOST)

Se hai specificato una data **runAt** lontana nel futuro, la campagna non è mai stata eseguita e è stato restituito **success=true**. Ora, se la data **runAt** è successiva ai 2 anni, viene restituito un errore [1042](/help/rest-api/error-codes.md).

[Lead di sincronizzazione](https://developer.adobe.com/marketo-apis/api/mapi/#operation/syncLeadUsingPOST)

Se sono stati specificati `action=createDuplicate` e un parametro `externalCompanyId` (per associare il nuovo lead a una società esistente), il lead è stato associato a una società vuota (anziché alla società specificata).

#### Varie

* È stato rimosso il seguente valore di modifica dati da tutte le risposte endpoint: mktoClientReqId. Solo per uso interno.
* È stata aggiunta la funzionalità di ricerca errori. Inserisci un codice di errore REST API nella casella di ricerca e seleziona dall’elenco di completamento automatico sottostante per passare alla descrizione dell’errore.
* Aggiunta [Pagina Riferimento endpoint](/help/rest-api/endpoint-reference.md). Questo è un elenco ordinabile di tutti gli endpoint REST API in un’unica posizione. Puoi anche utilizzare questa pagina per generare il set minimo di autorizzazioni necessarie per l’applicazione. Questa funzione si rivela utile durante la creazione di un servizio personalizzato.

Pubblicato il _2018-10-12_ da _David_

## Aggiornamenti di autunno 2018

La versione di autunno 2019 è principalmente una versione di manutenzione composta da miglioramenti minori e risoluzioni dei difetti. Di seguito è riportato l&#39;elenco completo degli aggiornamenti.

### Miglioramenti

* È stato aggiunto il supporto per [Campi CC e-mail](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/email-marketing/general/email-cc) per [API risorsa](/help/rest-api/assets.md). Le impostazioni del campo CC vengono propagate come previsto durante le operazioni di approvazione/clonazione (approvazione bozza modello e-mail o e-mail, clone e-mail o programma). Tutti gli endpoint relativi alle e-mail ora restituiscono i valori dei campi CC nella proprietà **ccFields**. Scorri verso il basso nella risposta seguente per visualizzare un esempio. Questa modifica interessa i seguenti endpoint: [Ricevi e-mail per ID](https://developer.adobe.com/marketo-apis/api/asset/#operation/getEmailByIdUsingGET), [Ricevi e-mail per nome](https://developer.adobe.com/marketo-apis/api/asset/#operation/getEmailByNameUsingGET), [Ricevi e-mail](https://developer.adobe.com/marketo-apis/api/asset/#operation/getEmailUsingGET), [Approva bozza e-mail](https://developer.adobe.com/marketo-apis/api/asset/#operation/approveDraftUsingPOST), [Approva bozza modello e-mail](https://developer.adobe.com/marketo-apis/api/asset/#operation/approveDraftUsingPOST_1), [Clona e-mail](https://developer.adobe.com/marketo-apis/api/asset/#operation/cloneEmailUsingPOST), [Clona programma.](https://developer.adobe.com/marketo-apis/api/asset/#operation/cloneProgramUsingPOST)

```json
{
    "success": true,
    "errors": [],
    "requestId": "cc96#166e836348b",
    "warnings": [],
    "result": [
        {
            "id": 2061,
            "name": "Test Email",
            "description": "This is a test",
            "createdAt": "2018-11-06T05:27:10Z+0000",
            "updatedAt": "2018-11-06T08:33:16Z+0000",
            "url": "<https://app-sjqe.marketo.com/#EM2061A1LA1>",
            "subject": {
                "type": "Text",
                "value": "This is a test with CC Fields"
            },
            "fromName": {
                "type": "Text",
                "value": "Tommy Tester"
            },
            "fromEmail": {
                "type": "Text",
                "value": "<tommy.tester@marketo.com>"
            },
            "replyEmail": {
                "type": "Text",
                "value": "<tommy.tester@marketo.com>"
            },
            "folder": {
                "type": "Program",
                "value": 7494,
                "folderName": "Initiative"
            },
            "operational": false,
            "textOnly": false,
            "publishToMSI": false,
            "webView": false,
            "status": "approved",
            "template": 1218,
            "workspace": "Default",
            "version": 2,
            "autoCopyToText": true,
            "ccFields": [
                {
                    "attributeId": "855",
                    "objectName": "lead",
                    "displayName": "emailcc",
                    "apiName": "emailcc"
                },
                {
                    "attributeId": "857",
                    "objectName": "lead",
                    "displayName": "leadDetails",
                    "apiName": "leadDetails"
                },
                {
                    "attributeId": "859",
                    "objectName": "company",
                    "displayName": "headquarters",
                    "apiName": "headquarters"
                }
            ]
        }
    ]
}
```

### Risoluzioni dei difetti

* È stato modificato il supporto di [Più domini di branding](https://experienceleague.adobe.com/it/docs/marketo/using/home) per [Asset API](/help/rest-api/assets.md). In precedenza, le impostazioni Più domini di branding non venivano propagate durante l’approvazione di una bozza e-mail, la clonazione di un’e-mail o la clonazione di un programma. Questo problema è stato corretto. Questa modifica interessa i seguenti endpoint: [Approva bozza e-mail](https://developer.adobe.com/marketo-apis/api/asset/#operation/approveDraftUsingPOST), [Clona e-mail](https://developer.adobe.com/marketo-apis/api/asset/#operation/cloneEmailUsingPOST), [Clona programma.](https://developer.adobe.com/marketo-apis/api/asset/#operation/cloneProgramUsingPOST)
* È stata aggiunta l&#39;impostazione di configurazione [apiOnly](/help/javascript-api/configuration.md). Per impostazione predefinita, le pagine web che contengono il tag Munchkin attivano un evento &quot;Visite pagina web&quot; quando la pagina web viene caricata nel browser. In alcuni casi, ciò non è auspicabile. Ad esempio, le applicazioni web a pagina singola che richiedono il controllo completo di quando viene attivato questo evento. Per supportare questo caso d&#39;uso, è stata aggiunta una nuova impostazione di configurazione **apiOnly**. Se è impostato su true, il tag Munchkin non genera un&#39;attività &quot;Visite pagina web&quot; durante il caricamento della pagina.
* Aggiunta impostazione di configurazione [domainSelectorV2](/help/javascript-api/configuration.md) completata. Per impostazione predefinita, il tag Munchkin non gestisce correttamente le pagine Web ospitate su siti con [domini di primo livello con codice paese a due lettere](https://en.wikipedia.org/wiki/Country_code_top-level_domain) (esempi: .io, .co, .ly). Questo causa l’impostazione errata dell’attributo del dominio del cookie Munchkin. Per ottenere una migliore esperienza preconfigurata, è stata aggiunta una nuova impostazione di configurazione **domainSelectorV2**. Se è impostato su true, viene utilizzato un algoritmo migliorato per impostare automaticamente l’attributo del dominio dei cookie di Munchkin.
* Dominio cookie [Opt-Out](/help/javascript-api/lead-tracking.md) modificato. In alcuni casi, l’attributo di dominio del cookie di rinuncia di Munchkin (mkto_opt_out) non veniva impostato correttamente. Il cookie di rinuncia di Munchkin ora utilizza la stessa logica del cookie di Munchkin (_mkto_trk) per determinare l&#39;attributo del cookie di dominio, incluso il rispetto dell&#39;impostazione di configurazione **domainLevel**.
* Gli sviluppatori di applicazioni Android ora possono utilizzare direttamente [Firebase Cloud Messaging](https://firebase.google.com/docs/cloud-messaging/) (FCM) di Google con questo SDK. I dettagli sono disponibili [qui](/help/mobile/installation.md).

Pubblicato il _2018-12-07_ da _David_

## Aggiornamenti della primavera 2019

La versione di primavera 2019 è principalmente una versione di manutenzione composta da miglioramenti minori e risoluzioni dei difetti. Di seguito è riportato l&#39;elenco completo degli aggiornamenti.

Pubblicato il _2019-03-19_ da _David_

## Aggiornamenti di giugno 2019

La versione di giugno 2019 è principalmente una versione di manutenzione composta da miglioramenti minori e risoluzioni dei difetti. Di seguito è riportato l&#39;elenco completo degli aggiornamenti.

### REST API

1. È stato aggiunto un checksum agli endpoint di stato per l’esportazione in blocco. Puoi confrontare il checksum con un hash del file recuperato per verificare l’integrità del file recuperato. Il checksum è un hash SHA-256 del file esportato e viene memorizzato nell&#39;attributo fileCheckSum quando viene completato un processo di esportazione.

I seguenti endpoint restituiscono il checksum: [Ottieni stato processo lead esportazione](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getExportLeadsStatusUsingGET), [Ottieni processi lead esportazione](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getExportLeadsFileUsingGET), [Ottieni stato processo attività esportazione](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getExportLeadsStatusUsingGET), [Ottieni processi attività esportazione](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getExportActivitiesUsingGET)

#### Risoluzioni dei difetti

1. È stato risolto un problema relativo all&#39;importazione di [oggetti personalizzati in blocco](/help/rest-api/bulk-custom-object-import.md) durante l&#39;importazione di numeri decimali in campi integer. Prima della correzione, il numero decimale veniva convertito in un numero intero assegnando la porzione del numero intero e scartando la porzione frazionaria (ad esempio, 5,432 veniva convertito in 5). Ora per ogni riga contenente dati non corrispondenti viene generato l’errore &quot;Tipo di dati non valido nel campo Source ID&quot;.
1. È stato risolto un problema a causa del quale un programma e-mail creato utilizzando l&#39;endpoint [Clone Program](https://developer.adobe.com/marketo-apis/api/asset/#operation/cloneProgramUsingPOST) non rispettava le impostazioni dei limiti di comunicazione in alcuni casi.
1. È stato risolto un problema con l&#39;endpoint [Approve Landing Page Draft](https://developer.adobe.com/marketo-apis/api/asset/#operation/approveLandingPageUsingPOST) che restituiva 611. &quot;Errore di sistema&quot; quando la pagina di destinazione conteneva il modulo per l’annullamento dell’iscrizione e-mail.
1. È stato risolto un problema con l&#39;endpoint [Approve Landing Page Draft](https://developer.adobe.com/marketo-apis/api/asset/#operation/approveLandingPageUsingPOST) che restituiva 611. &quot;Errore di sistema&quot; quando la pagina di destinazione è stata clonata utilizzando l&#39;endpoint [Clone Landing Page](https://developer.adobe.com/marketo-apis/api/asset/#operation/cloneLandingPageUsingPOST).

#### Funzionalità deprecate

1. Come parte di [Deprecazione editor e-mail 1.0](https://nation.marketo.com:443/t5/knowledgebase/email-editor-1-0-is-being-deprecated-june-18th/ta-p/250666), 1.0 Email Assets diventerà di sola lettura alla fine del 2019. Tutte le e-mail Assets 1.0 devono essere convertite in 2.0 come bozza e-mail descriDiscard, letto [qui](https://nation.marketo.com:443/t5/knowledgebase/email-editor-1-0-is-being-deprecated-june-18th/ta-p/250666). Per aiutare gli sviluppatori a prepararsi per tale evento, sono stati aggiunti degli avvisi a tutti gli endpoint relativi all’e-mail che tentano di modificare la versione 1.0 di E-mail Assets. Di seguito è riportato un esempio di risposta che contiene l’avviso:

`{` `"success": true,` `"errors": [],` `"requestId": "15c57#16b338d6e75",` `"warnings": [` `"This is a v1 email asset. API support for modifying v1 emails is being dropped, and this operation will not work on v1 emails in the future. To avoid service interruptions, upgrade this and related assets by editing them in the User Interface."` `],` `"result": [` `"{\"service\":\"sendTestEmail\",\"result\":true}"` `]` `}`

I seguenti endpoint relativi alle e-mail restituiscono l’avviso:

* Aggiorna contenuto completo e-mail
* Aggiorna contenuto e-mail
* Invia e-mail di esempio
* Annulla approvazione e-mail
* Clona programma
* Clona e-mail
* Approva bozza e-mail
* Sezione Aggiornamento contenuto dinamico e-mail
* Aggiorna metadati e-mail
* Approva programma

Script E-Mail (Apache Velocity)

#### Funzionalità deprecate

1. Un sottoinsieme di funzionalità Script Velocity è stato disabilitato a scopo di sicurezza. Sono inclusi i metodi e le variabili seguenti: getClass(), $class, $context, $text. Ulteriori informazioni sono disponibili [qui](https://nation.marketo.com:443/t5/knowledgebase/unsupported-velocity-tools-disabled-in-june-2019-release/ta-p/251177).

Pubblicato il _2019-06-14_ da _David_

## Aggiornamenti di agosto 2019

Ad agosto 2019 verranno rilasciate nuove API REST, verranno migliorate le API esistenti e verranno risolti i problemi. Di seguito è riportato l&#39;elenco completo degli aggiornamenti.

### Miglioramenti

1. Funzionalità avanzate del ciclo di vita di Smart Campaign. Sono stati aggiunti nuovi endpoint per consentire l’esecuzione delle seguenti operazioni sulle campagne avanzate: ottieni per nome, crea, aggiorna, clona ed elimina. Le informazioni complete sono disponibili [qui](/help/rest-api/smart-campaigns.md).
1. Endpoint di elenchi avanzati migliorati per migliorare le funzionalità di query.
   1. L&#39;endpoint [Get Smart List by Id](https://developer.adobe.com/marketo-apis/api/asset/#operation/getSmartListByIdUsingGET) ora restituisce le descrizioni delle regole di elenchi avanzati (trigger e filtri) quando si passa il parametro booleano **includeRules**.
   1. L&#39;endpoint [Get Smart Lists](https://developer.adobe.com/marketo-apis/api/asset/#operation/getSmartListsUsingGET) ora consente di filtrare i risultati per intervallo di date quando si trasmettono i parametri datetime **earlyUpdatedAt** e **latestUpdatedAt**. Inoltre, questo endpoint ora restituisce elenchi avanzati che sono membri di campagne e programmi e-mail.
1. Sono stati aggiunti endpoint per l’estrazione delle definizioni degli elenchi avanzati.
   1. L&#39;endpoint [Ottieni elenco avanzato per ID campagna avanzata](https://developer.adobe.com/marketo-apis/api/asset/#operation/getSmartListBySmartCampaignIdUsingGET) restituisce il record dell&#39;elenco avanzato per un determinato ID campagna avanzata.
   1. L&#39;endpoint [Ottieni elenco avanzato per ID programma](https://developer.adobe.com/marketo-apis/api/asset/#operation/getSmartListByProgramIdUsingGET) restituisce il record elenco avanzato per un determinato ID programma.
1. È stato migliorato l&#39;endpoint [Aggiorna contenuto e-mail](https://developer.adobe.com/marketo-apis/api/asset/#operation/updateEmailContentUsingPOST) per consentire gli aggiornamenti ai campi dell&#39;intestazione e-mail per le e-mail che sono state interrotte dal modello (oggetto, nome, indirizzo e-mail, risposta a). Interrotto dal modello è descritto [qui](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/email-marketing/general/functions-in-the-editor/edit-an-emails-html).

### Risoluzioni dei difetti

1. È stato risolto un problema che causava l&#39;eliminazione della pagina di destinazione con la chiamata [Elimina pagina di destinazione](https://developer.adobe.com/marketo-apis/api/asset/#operation/deleteLandingPageByIdUsingPOST) in una pagina di destinazione approvata. Ora restituisce correttamente l’errore &quot;709, La pagina di destinazione approvata non può essere eliminata&quot;. [LM-127271]
1. È stato risolto un problema con l&#39;endpoint [Invia e-mail di esempio](https://developer.adobe.com/marketo-apis/api/asset/#operation/sendSampleEmailUsingPOST) a causa del quale restituiva 611. &quot;Errore di sistema&quot; quando l’e-mail era stata interrotta dal relativo modello. [LM-127288]
1. È stato risolto un problema con l&#39;endpoint [Elimina programma](https://developer.adobe.com/marketo-apis/api/asset/#operation/cloneProgramUsingPOST) a causa del quale, in alcuni casi, veniva eliminato un programma in uso invece di essere emesso il messaggio &quot;709, Impossibile eliminare il programma. Le attività sono utilizzate altrove o non possono essere cancellate per errore. [LM-125431]

### Funzionalità deprecate

1. Il supporto API per Email Editor 1.0 diventerà obsoleto a gennaio 2020. Ricordati di convertire le risorse in versione 2.0 prima di tale data. I tentativi di scrivere o clonare risorse e-mail 1.0 dopo gennaio genereranno errori anziché avvisi. Ulteriori informazioni sulle API Email [qui](https://nation.marketo.com:443/t5/knowledgebase/email-2-0-and-email-api-faq-s/ta-p/251423).
1. Per allinearsi allo standard Adobe di livello mondiale per la sicurezza, a partire dal 13 dicembre 2019 verrà dichiarato obsoleto il supporto per TLS (Transport Layer Security) 1.0 e 1.1. I sistemi che si integrano con Marketo non conformi al protocollo 1.2 potrebbero perdere l’accesso ai servizi Marketo Engage. Per mantenere l’accesso a Marketo Engage, assicurati che tutti i sistemi client siano conformi a TLS 1.2 prima del 13 dicembre 2019. Ulteriori informazioni sono disponibili [qui](https://nation.marketo.com:443/t5/knowledgebase/tls-1-0-1-1-deprecation-faq/ta-p/249085).

1. Tutto il contenuto correlato a Smart Campaign ora risiede nella voce di menu [Smart Campaigns](/help/rest-api/smart-campaigns.md) (sotto REST API > Assets).

Pubblicato il _2019-08-16_ da _David_

## Aggiornamenti di gennaio 2020

A gennaio 2020 verranno rilasciate nuove API REST, verranno migliorate le API esistenti e verranno risolti i problemi. Di seguito è riportato l&#39;elenco completo degli aggiornamenti.

* È stata aggiunta la possibilità di creare in modo programmatico le definizioni dello schema di oggetti personalizzati. Questo consente di definire un oggetto personalizzato una volta ed eseguirne il provisioning per tutte le istanze necessarie. Ciò consente agli utenti di sfruttare in modo efficace i modelli sandbox e del centro di eccellenza. Consente inoltre agli ISV di semplificare il processo di onboarding dei clienti. Per accedere all’API dei metadati dell’oggetto personalizzato è necessario un tipo di abbonamento appropriato.
* È stata aggiunta la possibilità di importare ed esportare in blocco i membri del programma. Questo nuovo set di endpoint segue il pattern API REST di Marketo esistente per la creazione di processi di elaborazione asincroni in blocco. I record Membro del programma possono contenere campi personalizzati Membro del programma e/o Campi lead.
* È stato aggiunto l’endpoint &quot;Ottieni campi membri modulo disponibili&quot; per supportare l’utilizzo dei campi personalizzati dei membri del programma come campi modulo. Restituisce un elenco di tutti i campi personalizzati dei membri del programma che possono essere utilizzati in un Marketo Form.
* Aggiunto [Ottieni modello e-mail utilizzato da un endpoint](/help/rest-api/email-templates.md) che restituisce un elenco di risorse e-mail che dipendono da un determinato modello e-mail. Questo consente di comprendere rapidamente l’impatto di una potenziale modifica del modello e-mail e di affrontare più facilmente queste dipendenze.

Pubblicato il _2020-01-17_ da _David_

## Recuperare ogni oggetto personalizzato

Spesso ci viene chiesto come utilizzare l&#39;API di Marketo per ottenere un elenco di tutti i [oggetti personalizzati](https://experienceleague.adobe.com/it/docs/marketo/using/home) (CO). La ricerca di CO richiede più del nome: è necessario conoscere _a priori_ di ogni CO. I metodi per ottenere tale conoscenza potrebbero non essere evidenti, in quanto l’API non fornisce alcun metodo per eseguire direttamente una query. Come per molti obiettivi in Marketo Engage, gli elenchi avanzati forniscono una risposta per i CO collegati a Persone (lead). Gli elenchi avanzati funzionano in modo diverso con le società e si finirà con un elenco di tutte le persone le cui società sono collegate al tipo di oggetto per il filtro, in modo da poter trovare necessario deduplicare le società a seconda dei tuoi obiettivi. Ogni volta che viene approvato un nuovo oggetto personalizzato, viene creato un filtro associato. Verrà denominato nel formato &quot;**Has CO NAME**&quot;. Nell&#39;esempio seguente, il nome dell&#39;oggetto personalizzato è &quot;**Sottoscrizione traccia conferenze&quot;** e il relativo filtro è denominato &quot;**Ha sottoscrizione traccia conferenze**&quot;. Dopo aver creato l&#39;elenco avanzato, è possibile recuperare le informazioni necessarie per eseguire una query per i CO associati utilizzando l&#39;[endpoint per oggetti personalizzati](/help/rest-api/custom-objects.md). Esporta l’elenco in modo da includere il campo collegato (ID o indirizzo e-mail). Puoi esportare utilizzando il filtro [Bulk Lead Extract API](/help/rest-api/bulk-lead-extract.md) per **smartListName** o **smartListId** oppure [esporta dall&#39;interfaccia utente](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/core-marketo-concepts/smart-lists-and-static-lists/managing-people-in-smart-lists/export-people-to-excel-from-a-list-or-smart-list). Nel passaggio successivo, utilizzerai ogni valore di campo collegato per eseguire una query individuale sugli oggetti personalizzati associati. In questo esempio il nome dell&#39;oggetto personalizzato è **&quot;Conference Track Subscription&quot;** e il nome API è **conferenceTrackSubscription_c**. Il nome API è presente sia nell&#39;interfaccia utente come &quot;**Nome API**&quot; che nell&#39;API come &quot;**nome**&quot;.  Amministratore | Oggetti personalizzati Marketo[/caption]. Frammento restituito dall&#39;endpoint [List Custom Objects API](https://developer.adobe.com/marketo-apis/api/mapi/#operation/listCustomObjectsUsingGET):

```json
{
    "name": "conferenceTrackSubscription_c",
    "displayName": "Conference Track Subscription",
    "description": "Track subscription for conference attendees",
    "createdAt": "2020-01-13T19:50:06Z",
    "updatedAt": "2020-01-13T19:50:06Z",
    "idField": "marketoGUID",
    "dedupeFields": [
        "subscriptionID"
    ],
    "searchableFields": [
        [
            "subscriptionID"
        ],
        [
            "marketoGUID"
        ],
        [
            "leadID"
        ]
    ],
    "relationships": [
        {
            "field": "leadID",
            "type": "child",
            "relatedTo": {
                "name": "Lead",
                "field": "Id"
            }
        }
    ]
}
```

Per recuperare gli oggetti personalizzati associati uno a uno (1:1) o uno a molti (1:N) con le persone nell&#39;elenco avanzato, effettua una richiesta come questa:

`GET /rest/v1/customobjects/conferenceTrackSubscription_c.json?filterType=leadID&filterValues=1000302,1000303,1000304,1000306,1000307`

In questo esempio, l&#39;oggetto personalizzato è collegato a Persons dal campo **leadID**, pertanto il tipo di filtro è &quot;**leadID**&quot;. Il parametro dei valori del filtro è un elenco separato da virgole degli ID estratti dall’esportazione di elenchi avanzati. La richiesta può includere tutti i valori di filtro che è possibile adattare in un singolo URI di richiesta: fino a 8.000 caratteri. Le richieste che superano questa lunghezza restituiscono un codice di errore a livello HTTP 414. La risposta può essere restituita in più blocchi. In tal caso, **moreResult** sarà **true** e verrà incluso **nextPageToken**. Dovrai quindi [passare da ](/help/rest-api/paging-tokens.md) ai risultati fino a **moreResult**, che sarà **false**. Ecco parte del risultato per la richiesta API precedente:

```json
"result": [
    {
        "seq": 0,
        "marketoGUID": "d6b3ed3d-4eb8-4066-a9cd-184c8d385cfe",
        "leadID": "1000302",
        "subscriptionID": "4ad59184-6bf1-4eeb-a583-d82aeee68210",
        "createdAt": "2020-01-13T21:24:04Z",
        "updatedAt": "2020-01-13T21:24:04Z"
    },
    {
        "seq": 1,
        "marketoGUID": "e5e0aba4-f27f-494d-93ed-9cb580989bf3",
        "leadID": "1000303",
        "subscriptionID": "fc5596d5-6fa2-4848-b4a2-89d96e245c59",
        "createdAt": "2020-01-13T21:24:04Z",
        "updatedAt": "2020-01-13T21:24:04Z"
    },
    {
        "seq": 2,
        "marketoGUID": "e65007cd-86b1-4c17-8d55-057c96e1788a",
        "leadID": "1000304",
        "subscriptionID": "7e54b8a0-2170-4d81-a809-4eac349508d0",
        "createdAt": "2020-01-13T21:24:04Z",
        "updatedAt": "2020-01-13T21:24:04Z"
    },
    {
        "seq": 3,
        "marketoGUID": "39d956b2-85e2-4c24-94e7-e9fa5a09d3d0",
        "leadID": "1000306",
        "subscriptionID": "644c8e5b-fc0c-4d4a-80f8-358a65ce0a68",
        "createdAt": "2020-01-13T21:24:04Z",
        "updatedAt": "2020-01-13T21:24:04Z"
    },
    {
        "seq": 4,
        "marketoGUID": "a2151350-50c8-437f-bc71-7a054bb601f0",
        "leadID": "1000307",
        "subscriptionID": "bf14218c-ae6a-42b3-a14e-f7182903cbcd",
        "createdAt": "2020-01-13T21:24:04Z",
        "updatedAt": "2020-01-13T21:24:04Z"
    }
```

Ora disponi dei valori per ogni oggetto personalizzato direttamente associato alle persone nell&#39;elenco avanzato e oltre a recuperare i valori, puoi utilizzare **marketoGUID** per [Aggiornare](/help/rest-api/custom-objects.md) o [Eliminare](/help/rest-api/custom-objects.md) questi oggetti. Per gli oggetti personalizzati associati a Persone in una relazione molti-a-molti (N:N), la tecnica precedente restituisce l&#39;oggetto di primo livello, che è l&#39;oggetto intermedio che collega ciascuna persona a più CO di secondo livello.

Per recuperare questi CO di secondo livello, avviare un nuovo set di query per il tipo di CO di secondo livello filtrando il campo di collegamento e i valori estratti dall&#39;oggetto intermedio di primo livello. Ad esempio, l&#39;oggetto &quot;**Conference Track Subscription&quot;** di cui sopra potrebbe avere un altro livello di oggetti che rappresentano le sessioni denominate **&quot;Session&quot;**, che probabilmente sarebbero collegate dal **subscriptionID**. La richiesta di recuperare le sessioni collegate alle iscrizioni alla traccia conferenze di cui sopra sarà simile alla seguente:

`GET /rest/v1/customobjects/session_c.json?filterType=subscriptionID&filterValues=4ad59184-6bf1-4eeb-a583-d82aeee68210,e5e0aba4-f27f-494d-93ed-9cb580989bf3,e65007cd-86b1-4c17-8d55-057c96e1788a,39d956b2-85e2-4c24-94e7-e9fa5a09d3d0,bf14218c-ae6a-42b3-a14e-f7182903cbcd`

_Nota a piè di pagina_ _1)**smartListName**&#x200B;e **smartListId**&#x200B;tipi di filtro non disponibili per alcune sottoscrizioni. Se non disponibile per la sottoscrizione, si riceverà un errore durante la chiamata dell&#39;endpoint del processo di creazione del lead di esportazione (**&quot;1035, tipo di filtro non supportato per la sottoscrizione di destinazione&quot;**). I clienti possono contattare il supporto tecnico Marketo per richiedere che questa funzionalità sia abilitata nella sottoscrizione._

Pubblicato il _2020-01-14_ da _Tony_

## Come recuperare ogni persona (lead)

Riceviamo molte richieste sui processi necessari per recuperare ogni persona (lead) da un’istanza di Marketo Engage. Abbiamo fornito molte risposte utili, ma nessuna è stata così completa come questa. Ho identificato alcuni concetti chiave necessari per estrarre ogni lead utilizzando API Bulk Extract di Marketo. Tutti gli altri dettagli possono essere appresi dal codice dimostrativo che ho creato per accompagnarlo. Dopo aver letto questo post ed esplorato il codice demo, avrai a disposizione tutte le informazioni necessarie per recuperare ogni lead da un’istanza di Marketo Engage.

### Panoramica

La tecnica di base utilizza l&#39;[API Bulk Lead Extract](/help/rest-api/bulk-lead-extract.md). Potresti aspettarti di poter creare un processo di esportazione lead in blocco senza filtro, ma non puoi: **l&#39;API richiede un filtro**. I filtri disponibili sono **createdAt**, **staticListName**, **staticListId,** **updatedAt**, **smartListName** e **smartListId**. Anche filtrare in base a un elenco avanzato senza filtri sembra interessante. Provalo e scopri che il sistema è abbastanza intelligente da trattare un elenco avanzato senza filtri allo stesso modo: l’API richiede un filtro anche qui. Poiché è necessario un filtro, il filtro canonico e affidabile da utilizzare è **createdAt**. Questo tipo di filtro consente intervalli di date/ora fino a 31 giorni, pertanto è necessario eseguire più processi e combinare i risultati. Per prima cosa, nell’istanza di destinazione viene trovata la data di creazione più vecchia possibile per un lead. A partire da quella data più lontana possibile, vengono creati processi che si estendono su 31 giorni meno un secondo (di cui ulteriori in seguito). Dopo aver creato ogni processo, lo accoderemo e lo attenderemo per il completamento. Quindi scaricheremo il file risultante e ne verificheremo l’integrità utilizzando un checksum. Infine, deduplica i lead per ID e quindi scrivi lead univoci in un file CSV di output.

### Trova il lead meno recente

Sto usando un piccolo &quot;trucco&quot; per ottenere la data più antica possibile per un lead nell’istanza target. Non c&#39;è un endpoint API dedicato a questa attività, quindi abbiamo bisogno di un po&#39; di creatività. Eseguire una query su tutte le cartelle con **maxDepth = 1** per ottenere un elenco di tutte le cartelle di primo livello nell&#39;istanza. Poi raccolgo le date **createdAt**, le analizzo e trovo la data meno recente. Questo metodo funziona perché alcune cartelle predefinite di livello superiore vengono create con l’istanza e non è stato possibile creare lead prima di tale momento.

### Seleziona campi obbligatori

È necessario decidere quali campi estrarre. Trova i campi disponibili per l&#39;istanza di destinazione utilizzando l&#39;endpoint [Descrivi lead 2](https://developer.adobe.com/marketo-apis/api/mapi/#operation/describeUsingGET_2). La risposta a tale richiesta includerà un elenco denominato &quot;fields&quot;. Ecco un estratto di una risposta di esempio:

```json
  "fields": [
      {
          "name": "AccountSource",
          "displayName": "Account Source",
          "dataType": "string",
          "length": 40,
          "updateable": true,
          "crmManaged": false
      },
      {
          "name": "acquisitionProgramId",
          "displayName": "Acquisition Program",
          "dataType": "reference",
          "updateable": true,
          "crmManaged": false
      },
      {
          "name": "Active_c",
          "displayName": "Active",
          "dataType": "string",
          "length": 255,
          "updateable": true,
          "crmManaged": false
      },
      {
          "name": "address",
          "displayName": "Address",
          "dataType": "text",
          "updateable": true,
          "crmManaged": false
      },
      {
          "name": "Address_lead",
          "displayName": "Address (L)",
          "dataType": "string",
          "length": 255,
          "updateable": true,
          "crmManaged": false
      },
      {
          "name": "annualRevenue",
          "displayName": "Annual Revenue",
          "dataType": "currency",
          "updateable": true,
          "crmManaged": false
      },
      {
          "name": "anonymousIP",
          "displayName": "Anonymous IP",
          "dataType": "string",
          "length": 255,
          "updateable": true,
          "crmManaged": false
      }, ...
```

Questo endpoint restituisce un elenco completo che include campi standard e personalizzati. Maggiore è il numero di campi richiesti, più lungo sarà il completamento del processo di esportazione e maggiore sarà il file risultante. In genere è necessario scegliere solo i campi necessari. Nulla ti impedisce di richiedere ogni campo disponibile, quindi questo è ciò che sto dimostrando. L&#39;identificatore di campo richiesto per la creazione di un processo di esportazione è il valore **name**. Estrarrò i valori dei nomi in un elenco di tutti i nomi di campo. Poi lo userò per richiedere ogni campo disponibile quando creo ogni processo di esportazione.

### Intervalli date processo esportazione: 31 giorni ciascuno

Ogni processo di esportazione può estendersi fino a 31 giorni. L&#39;istanza demo che sto utilizzando è stata creata ad agosto 2016, quindi ho bisogno di creare poco più di 40 lavori oggi. Questo è il numero di giorni dalla prima data di creazione diviso per 31 arrotondato per eccesso. L’API consente l’elaborazione contemporanea di due processi di esportazione, in modo da poterli estrarre con due processi in esecuzione in parallelo. I processi di estrazione in blocco sono una risorsa condivisa con ogni altra integrazione, quindi sarò gentile. Lascio l’altro processo disponibile per altre integrazioni e dimostrerò che i singoli processi sono in esecuzione uno dopo l’altro. Le date utilizzate per il filtro **createdAt** sono formattate in base alle [specifiche ISO 8601](https://www.w3.org/TR/NOTE-datetime). Sono sempre in GMT (Z+0000) quindi il fuso orario sarà semplicemente rappresentato come &quot;Z&quot; o &quot;+00:00&quot;. Il 1° agosto 2016 è **2016-08-01T00:00:00+00:00** e 31 giorni dopo è il 1° settembre 2016, ovvero **2016-09-01T00:00:00+00:00.** Sia l&#39;ora di inizio che l&#39;ora di fine sono inclusive, quindi sottrarrò 1 secondo dall&#39;ora di fine: **2016-09-01T00:00:00+00:00** diventa **2016-08-31T23:59:59+00:00**. La sottrazione di un secondo evita la sovrapposizione dei tempi. Poiché GMT è il valore predefinito, puoi anche lasciare inattivo **Z** o **+00:00**.

### Deduplica

Anche se ho evitato la sovrapposizione dei tempi, ho implementato anche la deduplicazione. L&#39;ho fatto perché ci sono alcuni casi limite in cui i tempi cambiano ([Ora legale](https://en.wikipedia.org/wiki/Daylight_saving_time)) causando valori ambigui e, di conseguenza, l&#39;API di estrazione in blocco di Marketo può restituire lead duplicati altrimenti imprevisti. È raro che questo accada, ma deve essere tenuto in considerazione in qualsiasi integrazione utilizzando intervalli di filtri di data e ora. Ho rimosso un secondo per chiarire che i tempi sono inclusivi. Non voglio che tu pensi che la creazione di un processo con **createdAt** e **endAt** volte di **2016-08-01T00:00:00Z** e **2016-09-01T00:00:00Z** rispettivamente non includa i lead creati il **2016-09-01T00:00:00Z**; lo farà.

### Crea un processo

Il primo passaggio consiste nel creare un processo utilizzando l&#39;endpoint [Crea processo lead esportazione](https://developer.adobe.com/marketo-apis/api/mapi/#operation/createExportLeadsUsingPOST). In questa demo, la richiesta di creazione del primo processo di esportazione è simile alla seguente:

`POST /bulk/v1/leads/export/create.json`

```json
{ "filter": {"createdAt": {"startAt": "2016-08-01T00:00:00",
                           "endAt": "2016-09-01T00:00:00"}},
"fields":["AccountSource","acquisitionProgramId","Active_c","address","Address_lead","annualRevenue","anonymousIP","BillingAddress","billingCity","billingCountry","BillingGeocodeAccuracy","BillingLatitude","BillingLongitude","billingPostalCode","billingState","billingStreet","blackListed","blackListedCause","city","CleanStatus","CleanStatus_account","company","CompanyDunsNumber","contactCompany","cookies","country","createdAt","customfield","DandbCompanyId","DandbCompanyId_account","dateOfBirth","dddS","department","doNotCall","doNotCallReason","dS","dueDate","DunsNumber","email","EmailBouncedDate","EmailBouncedReason","emailInvalid","emailInvalidCause","emailSuspended","emailSuspendedAt","emailSuspendedCause","externalCompanyId","externalSalesPersonId","facebookDisplayName","facebookId","facebookPhotoURL","facebookProfileURL","facebookReach","facebookReferredEnrollments","facebookReferredVisits","fax","firstName","gender","GeocodeAccuracy","holll","id","industry","inferredCity","inferredCompany","inferredCountry","inferredMetropolitanArea","inferredPhoneAreaCode","inferredPostalCode","inferredStateRegion","interested","interestedIn","isAnonymous","IsEmailBounced","isLead","iSTRUE","Jigsaw","JigsawCompanyId_account","JigsawContactId_lead","Jigsaw_account","Languages_c","lastName","LastReferencedDate","LastReferencedDate_account","lastReferredEnrollment","lastReferredVisit","LastViewedDate","LastViewedDate_account","Latitude","leadPartitionId","leadPerson","leadRevenueCycleModelId","leadRevenueStageId","leadRole","leadScore","leadSource","leadStatus","linkedInDisplayName","linkedInId","linkedInPhotoURL","linkedInProfileURL","linkedInReach","linkedInReferredEnrollments","linkedInReferredVisits","links","Longitude","MailingAddress","MailingGeocodeAccuracy","MailingLatitude","MailingLongitude","mainPhone","marketingSuspended","marketingSuspendedCause","middleName","mktoAcquisitionDate","mktocomment1","mktocomments2","mktoCompanyNotes","mktoDoNotCallCause","mktoIsCustomer","mktoIsPartner","mktoName","mktoPersonNotes","mktosync","mktotest1","mobile","mobilePhone","NaicsCode","NaicsDesc","newcustom","numberOfEmployees","originalReferrer","originalSearchEngine","originalSearchPhrase","originalSourceInfo","originalSourceType","OtherAddress","OtherGeocodeAccuracy","OtherLatitude","OtherLongitude","personPrimaryLeadInterest","personTimeZone","personType","phone","PhotoUrl","PhotoUrl_account","postalCode","priority","ProductInterest_c","rating","referal","registrationSourceInfo","registrationSourceType","relativeScore","relativeUrgency","requiredNumberofCylinder","salutation","sfdcAccountId","sfdcContactId","sfdcId","sfdcLeadId","sfdcLeadOwnerId","sfdcType","ShippingAddress","ShippingGeocodeAccuracy","ShippingLatitude","ShippingLongitude","sicCode","SicDesc","site","state","surveyAnswers","syndicationId","testBooleanField","testscore","title","totalReferredEnrollments","totalReferredVisits","Tradestyle","twitterDisplayName","twitterId","twitterPhotoURL","twitterProfileURL","twitterReach","twitterReferredEnrollments","twitterReferredVisits","uNSUBSCIBE","unsubscribed","unsubscribedReason","updatedAt","urgency","url","website","YearStarted"]}
```

La risposta si presenta così:

```json
{
  "requestId": "6902#16fb52118bf",
  "result": [
      {
          "exportId": "4f2b9115-c3f2-4e40-a87c-bf803bbfed99",
          "format": "CSV",
          "status": "Created",
          "createdAt": "2020-01-17T20:10:43Z"
      }
  ],
  "success": true
}
```

### Accoda il processo

Il lavoro è stato creato, ma non è stato fatto nulla. Per eseguire il processo, è necessario chiamare l&#39;endpoint [enqueue](https://developer.adobe.com/marketo-apis/api/mapi/#operation/enqueueExportLeadsUsingPOST) utilizzando il valore **exportId** per generare l&#39;URI per la richiesta. Ecco un esempio:

`POST /bulk/v1/leads/export/4f2b9115-c3f2-4e40-a87c-bf803bbfed99/enqueue.json`

Non c&#39;è alcun corpo per questo POST, stiamo semplicemente utilizzando il verbo HTTP POST qui. La richiesta genererà una risposta come questa:

```json
{
  "requestId": "1836a#16fb5238a48",
  "result": [
      {
          "exportId": "4f2b9115-c3f2-4e40-a87c-bf803bbfed99",
          "format": "CSV",
          "status": "Queued",
          "createdAt": "2020-01-17T20:10:43Z",
          "queuedAt": "2020-01-17T20:13:23Z"
      }
  ],
  "success": true
}
```

Come ho detto prima, il numero di processi che è possibile eseguire alla volta è limitato. È inoltre previsto un limite al numero di processi in coda contemporaneamente: 10. Abbiamo bisogno di più di 40, quindi questo limite ci impedisce di creare tutti i posti di lavoro contemporaneamente. Anche altre integrazioni possono eseguire processi, quindi dobbiamo tenere conto della possibilità che tutti gli slot siano pieni. Se si tenta di accodare un nuovo processo quando sono già presenti 10 processi in coda, verrà generato un errore [1029](/help/rest-api/error-codes.md). Quando ottieni un **1029**, utilizza un backoff esponenziale fino a quando il processo non può essere accodato. Attendo 1 minuto e raddoppio di tale valore ogni volta che ricevo un codice di errore **1029** fino a 4 minuti tra le richieste, ma mai più lungo di questo. Questa tecnica è nota come [Backoff esponenziale binario troncato](https://devopedia.org/binary-exponential-backoff) ed è una best practice per gli errori recuperabili e i controlli dello stato.

### Attendere il completamento del processo

L&#39;esecuzione di ogni processo richiede un po&#39; di tempo, pertanto chiameremo l&#39;endpoint [status](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getExportLeadsStatusUsingGET) per monitorarne l&#39;avanzamento. Anche in questo caso, includeremo **exportId** nell&#39;URI della richiesta come segue:

`GET /bulk/v1/leads/export/4f2b9115-c3f2-4e40-a87c-bf803bbfed99/status.json`

Prima del completamento del processo, si ottiene una risposta simile alla seguente:

```json
{
    "requestId": "153cb#16fb525435d",
    "result": [
        {
            "exportId": "4f2b9115-c3f2-4e40-a87c-bf803bbfed99",
            "format": "CSV",
            "status": "Processing",
            "createdAt": "2020-01-17T20:10:43Z",
            "queuedAt": "2020-01-17T20:13:23Z",
            "startedAt": "2020-01-17T20:13:49Z"
        }
    ],
    "success": true
}
```

Eseguo lo stesso backoff esponenziale (da 1 minuto a 4 minuti) mentre lo stato del processo è &quot;In coda&quot;. Lo stato non è in tempo reale, viene aggiornato una volta al minuto e non c’è molto vantaggio nel polling più veloce. Reimposta il backoff quando lo stato del processo cambia in &quot;Elaborazione&quot;. Stiamo aspettando lo stato &quot;Completato&quot;, che si presenta così:

```json
{
  "requestId": "10ad9#16fb5268f9b",
  "result": [
      {
          "exportId": "4f2b9115-c3f2-4e40-a87c-bf803bbfed99",
          "format": "CSV",
          "status": "Completed",
          "createdAt": "2020-01-17T20:10:43Z",
          "queuedAt": "2020-01-17T20:13:23Z",
          "startedAt": "2020-01-17T20:13:49Z",
          "finishedAt": "2020-01-17T20:15:28Z",
          "numberOfRecords": 59,
          "fileSize": 74436,
          "fileChecksum": "sha256:de553362e7ffad6556ae9ea749655c35010c7f0e944fc5a85782183130dca18d"
      }
  ],
  "success": true
}
```

Il valore **numberOfRecords** è zero quando la richiesta non restituisce lead. Controllo questo valore e salto i passaggi successivi quando ha senso. Quando vengono restituiti i lead, estraggo il valore **fileChecksum**. Lo utilizziamo per verificare l’integrità del file quando viene scaricato.

### Ottieni i tuoi lead

Se **numberOfRecords** è maggiore di zero, scaricare il file esportato utilizzando [Ottieni file lead di esportazione](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getExportLeadsFileUsingGET) con una richiesta simile alla seguente:

`GET /bulk/v1/leads/export/4f2b9115-c3f2-4e40-a87c-bf803bbfed99/file.json`

Verifica che il file sia stato trasferito senza errori: calcola il checksum del file e confronta con **fileChecksum** salvato in precedenza. Calcola il checksum utilizzando [SHA-2](https://en.wikipedia.org/wiki/SHA-2) e in particolare la funzione hash SHA256. Se i checksum calcolati non corrispondono, si è verificato un errore nel trasferimento del file e si può riprovare il trasferimento oppure interrompere e recuperare manualmente.

### Aggregare i dati

Dopo aver effettuato un ciclo ogni 31 giorni, dal primo lead a oggi, avrai a disposizione un set completo. Avrai anche un file per ogni intervallo. Il modo più semplice per creare un singolo file aggregato con ogni lead consiste nel concatenare questi file dopo aver rimosso la riga di intestazione per tutti i file tranne il primo. In tal caso, non dimenticare di prevedere e pianificare eventuali duplicazioni in un secondo momento nella pipeline di elaborazione dei dati. Nella mia dimostrazione, elaboro i file mentre li scarico. Prima di aggiungere ogni riga di dati al file di output, deduco verificando se l’ID lead della riga è già stato scritto.

Ho sviluppato un codice dimostrativo ospitato [qui](https://github.com/Marketo/REST-Sample-Code/tree/master/python/LeadDatabase/Leads) che, si spera, riempie i dettagli di questo processo e può fungere da modello per il tuo sviluppo. Il codice demo è destinato a essere uno strumento di apprendimento, in modo da apportare miglioramenti alla robustezza che sarebbero necessari per un sistema di produzione. **Il codice viene fornito AS-IS** con licenza MIT, ma è probabilmente sufficiente per un utilizzo una tantum con supervisione umana. Non c&#39;è niente che ti stia fermando ora! Seguendo questa procedura, estrarrai ogni lead utilizzando l’API di estrazione in blocco di Marketo e potenzialmente ogni campo per l’istanza Marketo Engage di destinazione. Per estendere ulteriormente i dati, ottieni le attività di ogni lead utilizzando le tecniche.

Pubblicato il _2020-03-05_ da _Tony_

## Aggiornamenti di febbraio 2020

A febbraio 2020 verranno rilasciate nuove API REST. Di seguito è riportato l&#39;elenco completo degli aggiornamenti.

### Annunci

* Dopo settembre 2020, gli endpoint [Asset API](/help/rest-api/assets.md) non accetteranno più il parametro di query **_method**. Questo veniva utilizzato per trasmettere i parametri di query nel corpo di un POST al fine di aggirare le limitazioni relative alla lunghezza degli URI. Per soddisfare le richieste che richiedevano questo parametro, il limite URI per le API Asset verrà aumentato da 6KiB a 65KiB.
* Per quanto riguarda la nostra posizione su ITP, consulta questo post della community Marketo: [Aggiornamenti dei cookie del browser: impatto su Marketo/Munchkin](https://nation.marketo.com:443/t5/knowledgebase/browser-cookie-updates-how-marketo-munchkin-is-affected/ta-p/251524)
* È stata apportata una modifica all’attività &quot;Modifica stato in progressione&quot;. L’attributo &quot;ID membro del programma&quot; è stato aggiunto a a supporto della prossima funzione &quot;Campi personalizzati membro del programma&quot;.

Pubblicato il _2020-02-26_ da _David_

## Obsolescenza del metodo lead associato Munchkin

Con la prossima versione del client JavaScript di Munchkin, versione 159, verrà dichiarata obsoleta la versione [Associate Lead method](/help/javascript-api/api-reference.md) di Munchkin. A partire da questa versione, quando il metodo viene richiamato, nella console del browser viene visualizzato un avviso che indica che il metodo verrà rimosso in una versione futura. Una volta rimosso il metodo, i tentativi di utilizzarlo risulteranno in un errore. I clienti Marketo che abbiamo identificato come utenti di questo metodo di recente riceveranno una notifica individuale del loro utilizzo. Per ulteriori informazioni sulla versione di Munchkin 159, [consulta questo articolo sulla nazione di marketing](https://nation.marketo.com:443/t5/product-documents/munchkin-javascript-version-159-amp-associate-lead-deprecation/ta-p/299687).

### Domande frequenti

Come posso sapere se sono interessato?

Adobe comunicherà ai clienti dove ha osservato l’utilizzo di questo metodo al momento dell’abbonamento e lo farà più volte durante il periodo di deprecazione.

Quando verrà rimosso il metodo?

Questo metodo verrà rimosso in Munchkin JS v161, pianificato per la disponibilità generale insieme alla versione di Marketo di ottobre 2021.

Perché questo metodo viene rimosso?

Dall’introduzione di questo metodo sono stati implementati e pubblicati metodi più efficaci per soddisfare gli stessi casi d’uso. Al fine di migliorare le prestazioni e lo stato dei nostri servizi, a volte è necessario rimuovere le funzioni che non funzionano secondo standard accettabili.

Cosa dovrei usare al posto di questo metodo?

Munchkin Associate Lead ha due casi d’uso principali: invio di dati personali e associazione di un cookie di tracciamento web del browser al record persona corrispondente in Marketo. Dall’introduzione di Munchkin Associate Lead, sono stati implementati modi più solidi per l’invio dei dati e l’associazione dei cookie.

#### Invio lato server

Se non è necessario l&#39;invio lato browser, l&#39;API REST offre molti metodi per l&#39;invio di [dati personali](/help/rest-api/leads.md) e un [metodo specifico per associare i cookie ai record delle persone](/help/rest-api/leads.md).

Quando viene eseguito il rollout della versione 159 di Munchkin?

La versione 159 è disponibile in General Availability dalla versione di Marketo di ottobre 2020.

Pubblicato il _2020-05-06_ da _Kenny_

## Invio di un Marketo Form in background

Quando la tua organizzazione dispone di diverse piattaforme per l’hosting di contenuti web e dati dei clienti, diventa abbastanza comune richiedere invii paralleli da un modulo, in modo che i dati risultanti possano essere raccolti in piattaforme separate. Esistono diverse strategie per farlo, ma quella migliore è spesso la più semplice: utilizzare l’API Forms 2 per inviare un modulo Marketo nascosto. Questa opzione funziona con qualsiasi nuovo Marketo Form, ma è consigliabile creare un modulo vuoto, che non contenga campi. In questo modo il modulo non verrà caricato più dati del necessario, poiché non è necessario eseguire il rendering. Ora è sufficiente acquisire il [codice di incorporamento](https://experienceleague.adobe.com/it/docs/marketo/using/home) dal modulo e aggiungerlo al corpo della pagina desiderata, apportando una piccola modifica. Il codice di incorporamento include un elemento modulo come questo:

`<form id="mktoForm_1068"></form>`

Aggiungere &#39;style=&quot;display:none&quot;&#39; all&#39;elemento in modo che non sia visibile, come segue:

`<form id="mktoForm_1068" style="display:none"></form>`

Una volta che il modulo è incorporato e nascosto, il codice per inviarlo è davvero abbastanza semplice:

```javascript
var myForm = MktoForms2.allForms()[0];
myForm.addHiddenFields({
 //These are the values which will be submitted to Marketo
 "Email":"<test@example.com>",
 "FirstName":"John",
 "LastName":"Doe"
 });
myForm.submit();
```

Forms ha inviato questo metodo di comportamento esattamente come se il lead avesse compilato e inviato un modulo visibile. L’attivazione dell’invio varierà da un’implementazione all’altra poiché ogni implementazione ha un’azione diversa per richiedere l’invio, ma puoi eseguirlo praticamente su qualsiasi azione. La parte importante è impostare correttamente campi e valori. Assicurati di utilizzare il nome API SOAP dei campi che puoi trovare con Esporta nomi dei campi per garantire la corretta invio dei valori.

### Migrazione dal lead associato Munchkin

L’invio di moduli in background è uno dei metodi di sostituzione consigliati per Munchkin Associate Lead. L’esempio di pagina HTML riportato di seguito illustra la migrazione ad alto livello, riutilizzando gli stessi valori inviati ad Associate Lead (Associa lead).

```html
<html>
<head>
    <!--
      Munchkin Code
      Replace with your own instance code
    -->
    <script type="text/javascript">
        (function() {
          var didInit = false;
          function initMunchkin() {
            if(didInit === false) {
              didInit = true;
              Munchkin.init('CHANGE ME');
            }
          }
          var s = document.createElement('script');
          s.type = 'text/javascript';
          s.async = true;
          s.src = '//munchkin.marketo.net/munchkin-beta.js';
          s.onreadystatechange = function() {
            if (this.readyState == 'complete' || this.readyState == 'loaded') {
              initMunchkin();
            }
          };
          s.onload = initMunchkin;
          document.getElementsByTagName['head'](0).appendChild(s);
        })();
        </script>
</head>

<body>
  <!--
    Start Embed code.
    Pasted from Form Actions -> Embed Code except for addition of 'style="display:none"' to the form tag in order to hide it, and instance-specific codes redacted
    Replace with your own code for testing
  -->
  <script src="CHANGE ME"></script>
  <form id="CHANGE ME" style="display:none"></form>
  <script>MktoForms2.loadForm("CHANGE ME", "CHANGE ME", "CHANGE ME TO AN INTEGER ID");</script>
  <!--End Embed Code-->

  <!--Demo code-->
    <script>
        //The same map which is assembled for the Munchkin submission can be reused for the form submission
        let values = {
            "Email": "email@example.com",
            "FirstName": "Test",
            "LastName": "Record"
        }
        //whenReady lets us apply a callback to all mkto forms on the page
        //the callback function fires whenever a form is completely loaded
        //for most use cases this will just be used to capture a reference to the form for later usage
        //submission is done in line here for demonstration only
        MktoForms2.whenReady(function(form){

            //the addHiddenFields methods lets us add arbitrary fields to the form as well as their values
            form.addHiddenFields(values);

            //pass the same set of values to associateLead
            //hashString: secret + email
            Munchkin.munchkinFunction('associateLead', values, "CHANGE ME");

            //submit the form
            form.submit();


        })
    </script>
</body>

</html>
```

Pubblicato il _2020-05-26_ da _Kenny_

## Aggiornamenti di luglio 2020

A luglio 2020 verranno rilasciate nuove API REST, verranno migliorate le API esistenti e verranno risolti i problemi. Di seguito è riportato l&#39;elenco completo degli aggiornamenti.

* Sono stati aggiunti due endpoint che consentono di eseguire query ed eliminare utenti che non hanno ancora accettato l’invito (ovvero utenti &quot;in sospeso&quot;). L&#39;endpoint [Get Invited User by Id](/help/rest-api/user-management.md) consente di eseguire una query su un utente in sospeso. L&#39;endpoint [Elimina utente invitato](/help/rest-api/user-management.md) consente di eliminare un utente in sospeso.
* L&#39;endpoint [Invita utente](/help/rest-api/user-management.md) è stato aggiornato per accettare stringhe datetime conformi allo standard ISO 8601 per il parametro **expiresAt**.
* Gli endpoint [Ottieni utente per ID](/help/rest-api/user-management.md) e [Aggiorna attributo utente](/help/rest-api/user-management.md) sono stati aggiornati per restituire l&#39;ora dell&#39;ultimo accesso utente nell&#39;attributo **lastLoginAt**.
* È stato risolto un problema a causa del quale l&#39;endpoint [Crea elenco statico](https://developer.adobe.com/marketo-apis/api/asset/#operation/createStaticListUsingPOST) restituiva l&#39;errore &quot;611, errore di sistema&quot; quando si tentava di creare un elenco statico già esistente. È stato modificato per restituire l’errore &quot;709, Esiste già un elenco statico con lo stesso nome&quot;. [LM-135934]
* È stato risolto un problema a causa del quale l&#39;endpoint [Crea e-mail](https://developer.adobe.com/marketo-apis/api/asset/#operation/createEmailUsingPOST) restituiva l&#39;errore &quot;611, errore di sistema&quot; quando si tentava di creare un messaggio e-mail già esistente. È stato modificato per restituire l’errore &quot;709, Esiste già un’e-mail con lo stesso nome&quot;. [LM-138648]
* È stato risolto un problema a causa del quale gli endpoint di query della pagina di destinazione restituivano valori **createdAt** errati. Gli endpoint restituivano l’ora dell’ultima approvazione di una pagina di destinazione. Ora tornano all’ora di creazione di una pagina di destinazione. [LM-138648]
* È stato risolto un problema a causa del quale l&#39;endpoint [Unisci lead](https://developer.adobe.com/marketo-apis/api/mapi/#operation/mergeLeadsUsingPOST) restituiva l&#39;errore &quot;611, errore di sistema&quot; per un&#39;operazione di unione non valida. Si è verificato quando l&#39;unione ha prodotto un lead duplicato e **mergeinCRM** è stato impostato su true. È stato modificato per restituire l’errore &quot;712, stai creando un record duplicato. Consigliamo invece di utilizzare un record esistente&quot;. [LM-137463]

Pubblicato il _2020-08-01_ da _David_

## Post ospite - approfondimento: oggetto personalizzato vs. attività personalizzate vs. campo personalizzato

**Questo è un post per gli ospiti di Amit Jain, Marketo Champion 2020, MarTech IT Specialist** In qualità di piattaforma di automazione del marketing a livello aziendale, Marketo gestisce le informazioni di utenti/clienti potenziali acquisite da diverse origini e le gestisce in Marketo per migliorare la personalizzazione, la segmentazione e il reporting. Queste origini possono variare dai moduli del sito web alla build elenco, ai dati del sistema di gestione delle relazioni con i clienti fino ai dati di e-commerce. Marketo offre oggetti standard come lead, società, opportunità e attività, ecc. Questi oggetti standard talvolta non sono sufficienti per soddisfare le esigenze di marketing emergenti dei set di dati estesi disponibili in Marketo.

Ad esempio, articoli aggiunti al carrello, studenti richiesti per diversi corsi, prodotti di proprietà di una persona specifica e consensi per diversi abbonamenti, ecc. Queste informazioni possono essere fondamentali per consentire agli addetti al marketing di mantenere coinvolti clienti potenziali e clienti, fornendo contenuti più personalizzati e un’esperienza utente unificata su più piattaforme. Per soddisfare queste esigenze aziendali, Marketo offre modalità personalizzate per memorizzare questo tipo di dati in campi personalizzati, attività personalizzate e oggetti personalizzati. Ho notato persone che avevano problemi nel capire quando utilizzare quale delle opzioni. In questo post di blog, **comprenderemo nel dettaglio cosa sono effettivamente queste tre cose e quando usarne una con alcuni esempi.**

**Campi personalizzati**

L’oggetto &quot;Lead&quot; in Marketo è l’oggetto principale e tutto il resto è collegato a questo, direttamente o indirettamente. Marketo consente di creare campi personalizzati sull’oggetto &quot;Lead&quot;, oggetto &quot;Company&quot;, e di recente ha annunciato il supporto dei campi personalizzati &quot;Membro del programma&quot;. Questi campi personalizzati soddisfano le tue esigenze di archiviazione di determinati tipi di dati. Ad esempio, potrebbe essere necessario memorizzare la Funzione processo o diversi consensi dell’utente. In Marketo sono disponibili due tipi di campi personalizzati:

1. **Sincronizzato dai campi di gestione delle relazioni con i clienti:** Nell&#39;ambito della sincronizzazione automatica di Marketo ↔︎ SFDC, Marketo sincronizza tutti i campi personalizzati visibili all&#39;utente di Marketo in SFDC in lead, contatti, account e oggetti opportunità.
1. **Campi solo Marketo:** Puoi creare direttamente un campo in Marketo. I dati di questi campi non verranno sincronizzati con SFDC.

**Suggerimenti rapidi**

* Disponi di un campo Solo Marketo e ora desideri sincronizzare i dati in SFDC? Consulta [questo blog](https://themarketingautomationblog.com/2019/12/06/field-management-merging-remapping-hiding-marketo-fields/) per scoprire come farlo.
* Ulteriori informazioni sui campi personalizzati [qui](https://themarketingautomationblog.com/2019/11/15/knowing-your-marketo-fields/).
* Le API di Marketo non supportano al momento l’aggiornamento o la creazione di campi personalizzati.

**Oggetti personalizzati**

Oltre agli oggetti standard, Marketo consente di creare oggetti personalizzati. _È possibile creare oggetti personalizzati e riferirsi all&#39;oggetto Company o Lead o a un altro oggetto personalizzato._ Un oggetto personalizzato è un insieme di record personalizzati che integrano i record standard del lead e dell&#39;azienda. Gli oggetti personalizzati ti consentono di memorizzare dati aggiuntivi in modo scalabile e di collegarli a un record Lead o Azienda. È possibile creare un oggetto personalizzato con qualsiasi combinazione di campi standard (Collega campi) e personalizzati, compilare tali campi per creare l&#39;oggetto personalizzato _record_ e quindi collegare tali record a un record Lead o Company. I record collegati potenti e flessibili arricchiscono segmenti, elenchi avanzati e campagne consentendo di creare tali risorse con informazioni non presenti nei campi Lead e nei campi Società. Un oggetto personalizzato può avere uno dei seguenti tipi di relazioni:

* **Relazione uno-a-uno**: in cui ogni oggetto personalizzato ha un singolo record oggetto lead/azienda.
* **Relazione uno-a-molti**: in cui ogni oggetto personalizzato contiene più record di oggetti personalizzati correlati a un lead/società.
* **Relazione molti-a-molti:** in cui più record di oggetti personalizzati possono essere collegati a più oggetti lead/società. Ad esempio, più studenti sono iscritti a più corsi da un catalogo di corsi.

**Suggerimenti rapidi**

* Ulteriori informazioni sulla configurazione degli oggetti personalizzati [qui](https://experienceleague.adobe.com/it/docs/marketo/using/home).
* È possibile utilizzare l&#39;oggetto personalizzato di Marketo come oggetto intermedio e come oggetto personalizzato di oggetto personalizzato.

### Perché scegliere oggetti personalizzati?

Gli oggetti personalizzati ti consentono di compilare e utilizzare dati univoci rilevanti per un’azienda o un lead, ma che non sono necessariamente informazioni statiche sull’azienda o sul lead stesso. Mentre i campi lead si riferiscono alle informazioni sul lead (_Indirizzo e-mail_, _Codice postale_ e così via), alle informazioni aziendali (_Titolo processo_, _Settore_ e così via) o alle informazioni guidate dal sistema (_Marketo ID_, un SFDC ID o una data di creazione ecc.), i campi oggetto personalizzati sono completamente personalizzabili. Ad esempio, utilizzare oggetti personalizzati per memorizzare informazioni quali:

* Cronologia acquisti di un utente.
* Informazioni sul carrello.
* Che un cliente abbia o meno utilizzato uno dei tuoi codici di sconto promozionali a tempo limitato.
* Informazioni supplementari ottenute dai risultati dei sondaggi, dalle interviste e dalla partecipazione a eventi.
* Informazioni sulla versione di prova di un utente, inclusi l’URL dell’istanza di prova, la data di inizio, la data di fine, il numero di utenti e così via.

### Limitazioni per gli oggetti personalizzati di Marketo

1. Per impostazione predefinita, Marketo consente di creare 10 oggetti personalizzati. Puoi aumentare il limite con una tariffa di abbonamento aggiuntiva.
1. Il numero predefinito di campi per oggetto è 50, ma se necessario puoi richiedere il supporto di Marketo per campi aggiuntivi. Grazie [Michael Florin](https://www.linkedin.com/in/michaelflorin/) per l&#39;ulteriore contributo.
1. Esiste un limite al numero di record che è possibile avere in tutti gli oggetti personalizzati. Dipende dalla sottoscrizione a Marketo. Questo limite può essere aumentato con una tariffa di abbonamento aggiuntiva.
1. Se è stato creato un oggetto personalizzato utilizzando l’API, Marketo non consente di modificare lo schema CO dall’interfaccia utente di Marketo.

**Suggerimenti rapidi**

* L’API di Marketo supporta l’operazione CRUD (Create, Read, Update, Delete) su oggetti personalizzati.
* Puoi utilizzare questi dati oggetto personalizzati per la personalizzazione delle e-mail utilizzando lo script Velocity.
* Tutti gli endpoint relativi a oggetti personalizzati [sono disponibili qui](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getExportProgramMembersStatusUsingGET).

### Terminologia oggetto personalizzato

**Oggetto personalizzato**: contenitore che contiene un raggruppamento di tutti i record oggetto personalizzati. Noto formalmente come set di schede di dati o tabella personalizzata. **Record oggetto personalizzato**: record di dati contenente informazioni di campo aggiuntive che possono essere collegate a un lead o a un&#39;azienda. Un record può essere costituito da campi standard del lead o dell&#39;azienda e da campi di record oggetto personalizzati. Noto formalmente come scheda dati o riga di tabella dati. **Campo record oggetto personalizzato**: campi completamente personalizzabili per raccogliere informazioni univoche o temporanee. Questi campi vengono creati e inseriti all’interno dell’oggetto personalizzato stesso. Formalmente noto come campo scheda dati o campo/colonna della tabella del database. **Campo collegamento**: tipo speciale di campo record oggetto personalizzato per definire la relazione tra il record oggetto personalizzato e il record oggetto lead/società collegato. Quando si creano oggetti personalizzati, è necessario fornire campi di collegamento per collegare il record oggetto personalizzato al record padre corretto.

* Per una struttura personalizzata uno-a-molti o uno-a-uno, utilizzare il campo collegamento nell&#39;oggetto personalizzato per collegarlo a una persona o a un&#39;azienda.
* Per una struttura molti-a-molti, si utilizzano due campi di collegamento, connessi da un oggetto intermedio creato separatamente (che è anche un tipo di oggetto personalizzato). Un collegamento si connette a persone o società nel database e l&#39;altro all&#39;oggetto personalizzato. In questo caso, il campo del collegamento non si trova nell’oggetto personalizzato stesso.

### Attività personalizzate

**È possibile interagire in diversi modi con la nostra organizzazione. Possono visitare il sito web della tua azienda, partecipare a una fiera o fare clic su un link in un&#39;e-mail che hai inviato. Queste azioni sono attività** e qualsiasi azione intraprendano, Marketo le acquisisce in modo che i team di marketing e vendita possano comprendere meglio il comportamento dell&#39;utente per un coinvolgimento personalizzato e unificato. **_Attività personalizzate_** _consente di tenere traccia di un&#39;attività non correlata a un modulo di Marketo, un&#39;e-mail o una pagina di destinazione_. Ad esempio, se desideri monitorare quando qualcuno ha visualizzato un video su un sito web o ha svolto un sondaggio, utilizza un’attività personalizzata. Le attività personalizzate sono diverse dagli oggetti personalizzati. Utilizza oggetti personalizzati quando il valore può cambiare (ad esempio, il &quot;colore dell’auto&quot; cambia da blu a rosso). Utilizza le attività personalizzate per tenere traccia dei momenti che si sono verificati e i cui dettagli non possono cambiare (ad esempio, &quot;auto acquistata&quot;). Per impostazione predefinita, il limite del numero massimo di attività personalizzate che possono essere definite è 10. Questa cifra può essere aumentata con una quota di abbonamento aggiuntiva. In base ai [criteri di conservazione dei dati di Marketo](https://nation.marketo.com/t5/knowledgebase/tkb-p/support_solutions-documents), le attività personalizzate verranno eliminate automaticamente dopo 25 mesi.

**Attività personalizzata:** eventi non Marketo di cui tenere traccia in Marketo. **ID attività personalizzato:** Marketo assegna un ID numerico all&#39;attività personalizzata che può essere utilizzata durante il tentativo di inviare/estrarre i dati dell&#39;attività tramite l&#39;API Marketo. **Campi attività personalizzati:** I metadati dell&#39;attività possono essere memorizzati in un campo attività. Ad esempio, se tieni traccia delle visualizzazioni sul video, i campi potrebbero essere URL della pagina, Titolo video e così via. **Campo primario attività personalizzato:** campi di attività personalizzati che possono essere utilizzati come criteri di filtro dell&#39;elenco smart.

**Suggerimenti rapidi**

* Gli endpoint API per le attività personalizzate sono disponibili [qui.](https://developer.adobe.com/marketo-apis/api/mapi/#operation/addCustomActivityUsingPOST)

## Oggetto personalizzato e attività personalizzata

**Oggetto personalizzato**

**Attività personalizzata**

1

Max. 10 oggetti personalizzati per impostazione predefinita per istanza.

Per impostazione predefinita, fino a 10 attività personalizzate.

2

Massimo 50 campi oggetto personalizzato per oggetto personalizzato.

Ogni tipo di attività personalizzata può avere fino a 20 attributi secondari.

3

Max record oggetto personalizzati da 1 millimetro; potrebbe essere molto basato sul tuo abbonamento.

Dopo 25 mesi le attività personalizzate verranno eliminate in base ai criteri di conservazione dei dati di Marketo.

4

Può essere utilizzato come filtro e attivatore negli elenchi avanzati e nelle campagne avanzate.

Può essere utilizzato come filtro e attivatore negli elenchi avanzati e nelle campagne avanzate.

5

Può essere utilizzato per personalizzare il contenuto dell’e-mail.

Impossibile utilizzare per personalizzare il contenuto dell’e-mail.

6

Può eseguire un&#39;operazione CRUD su un record oggetto personalizzato.

Nelle attività personalizzate è consentito solo Creare e Leggere.

7

Utilizza oggetti personalizzati quando il valore può cambiare (ad esempio, il &quot;colore dell’auto&quot; cambia da blu a rosso).

Utilizza le attività personalizzate per tenere traccia dei momenti che si sono verificati e i cui dettagli non possono cambiare (ad esempio, &quot;auto acquistata&quot;).

8

Gli oggetti personalizzati vi dicono il fatto. ovvero il valore attuale.

Le attività personalizzate ti raccontano gli eventi che si sono verificati in passato.

Pubblicato il _2020-10-18_ da _Amit_

## Aggiornamenti di gennaio 2021

A gennaio 2021 verranno rilasciate nuove API REST e verranno risolti diversi difetti. Di seguito è riportato l&#39;elenco completo degli aggiornamenti.

* È stato aggiunto l&#39;endpoint [Invia modulo](/help/rest-api/leads.md) che consente di inviare moduli a livello di programmazione. I moduli di terze parti possono ora integrarsi con i moduli di Marketo per sfruttare i flussi di lavoro di marketing esistenti.
* Aggiunto l&#39;endpoint [Ottieni contenuto pagina di destinazione completo](/help/rest-api/landing-pages.md) che restituisce la versione serializzata HTML di una pagina di destinazione. Consente di eseguire il rendering di anteprime completamente personalizzate delle pagine di destinazione senza dover accedere a Marketo Engage. Questo consente di semplificare i flussi di lavoro di modifica e traduzione nelle applicazioni integrate.
* Ora puoi configurare il numero di oggetti personalizzati disponibili per l’accesso tramite lo script Velocity. Le istruzioni di configurazione sono disponibili [qui](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/email-setup/change-custom-object-retrieval-limits-in-velocity-scripting).

### Risoluzioni dei difetti

* È stato risolto un problema a causa del quale l&#39;endpoint [Elimina utente](/help/rest-api/user-management.md) consentiva di eliminare un utente di sola API in uso da un servizio personalizzato. Ora restituisce un errore &quot;611, Non è possibile eliminare un utente API che è utilizzato in servizio API&quot;. [LM-141893]
* È stato risolto un problema a causa del quale l&#39;endpoint [Get Users](/help/rest-api/user-management.md) restituiva, in alcuni casi, gli utenti eliminati. [LM-141542]
* È stato risolto un problema in cui l&#39;endpoint [Clone Program](https://developer.adobe.com/marketo-apis/api/asset/#operation/cloneProgramUsingPOST) era impostato su. Se hai specificato un nome di programma che supera i 255 caratteri, verrà restituito &quot;611, Errore Impossibile clonare il programma&quot;. Ora restituisce &quot;701, il nome non può superare i 255 caratteri&quot;. [LM-143436]
* È stato risolto un problema con l&#39;endpoint [Approve Landing Page Draft](https://developer.adobe.com/marketo-apis/api/asset/#operation/approveLandingPageUsingPOST). Quando approvi una pagina di destinazione con la versione mobile attivata, in alcuni casi visualizzi il contenuto della versione mobile nella versione desktop. [LM-146867]
* È stato risolto un problema con l&#39;endpoint [Unapprove Landing Page](https://developer.adobe.com/marketo-apis/api/asset/#operation/unapproveLandingPageByIdUsingPOST) (Annulla approvazione pagina di destinazione) che consentiva di annullare l&#39;approvazione di una pagina di destinazione utilizzata come pagina di follow-up da uno o più moduli. Ora viene restituito l’errore &quot;709, Pagina di destinazione di annullamento approvazione non riuscita. Pagina di destinazione utilizzata da uno o più moduli come pagina di follow-up con ID modulo:[_formId1,formId2,..._]&quot;. [LM-143326]

Pubblicato il _2021-01-15_ da _David_

## API Beta e Beacon di Munchkin 160

**27 gennaio 2021:** alcuni utenti di Marketo che saranno interessati dalla rimozione di Associate Lead hanno ricevuto una notifica e-mail per errore che indica che l&#39;impostazione Munchkin Beta è abilitata in una o più istanze. Questa versione viene mantenuta fino a quando il pubblico corretto non riceve una notifica. A partire dalla versione 160 di Munchkin JavaScript, l&#39;API [Beacon](https://developer.mozilla.org/en-US/docs/Web/API/Beacon_API) diventa il modo predefinito con cui Munchkin comunica con il backend di Marketo. Questa opzione è diventata disponibile nell&#39;estate 2020 con il rilascio della versione 159 tramite il parametro di configurazione **useBeaconAPI**. L’API Beacon offre diversi vantaggi rispetto all’utilizzo del vecchio metodo XMLHttpRequest, ma il miglioramento principale è rappresentato dal fatto che si tratta di un’API asincrona non bloccante per la comunicazione HTTP, disponibile per l’utilizzo in tutti i moderni browser Internet. Anche se la maggior parte degli utenti di Munchkin non noterà una modifica nel comportamento del sito web, questo aggiornamento impedirà a Munchkin di bloccare la navigazione in attesa di inviare un evento di clic al backend, o più semplicemente, questo elimina la possibilità che Munchkin causi il blocco di un browser dopo aver fatto clic su un collegamento a una nuova pagina. Si è trattato di un evento raro ma frustrante per alcuni clienti di Marketo.

A partire dal 27 gennaio 2021, il rollout di questa versione è in attesa della riprogrammazione. Anche se non sono previsti problemi correlati a questa modifica e non ne è stato identificato alcuno durante il test, è impossibile per Marketo testare tutte le possibili configurazioni di distribuzione di Munchkin e può essere opportuno testare queste modifiche in anticipo o rinunciare a questa modifica fino alla disponibilità generale di questa versione. Di seguito sono riportate le istruzioni per i vari scenari.

### Test API beacon

Se desideri testare l&#39;API Beacon aggiornata in previsione della prossima versione, puoi farlo aggiungendo il parametro **useBeaconAPI** alla configurazione Munchkin in una pagina di test esterna. Questo test funziona con la versione generalmente disponibile o beta di Munchkin. Il parametro di configurazione è mostrato di seguito nel secondo argomento della chiamata del metodo `Munchkin.init()` alla riga 7: `{ 'useBeaconAPI': true}`

```javascript
<script type="text/javascript">
(function() {
  var didInit = false;
  function initMunchkin() {
    if(didInit === false) {
      didInit = true;
      Munchkin.init('299-BYM-827', {"useBeaconAPI":true});
    }
  }
  var s = document.createElement('script');
  s.type = 'text/javascript';
  s.async = true;
  s.src = '//munchkin.marketo.net/munchkin.js';
  s.onreadystatechange = function() {
    if (this.readyState == 'complete' || this.readyState == 'loaded') {
      initMunchkin();
    }
  };
  s.onload = initMunchkin;
  document.getElementsByTagName['head'](0).appendChild(s);
})();
</script>
```

### Disattiva Munchkin Beta sulle pagine di destinazione di Marketo

Per disabilitare Munchkin Beta nelle pagine di destinazione di Marketo, devi accedere al menu [Treasure Chest](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/settings/enable-or-disable-treasure-chest-features) nella sezione Admin del tuo abbonamento e modificare l&#39;impostazione di Munchkin Beta sulle pagine di destinazione in disabilitato.

### Disattiva Munchkin Beta su pagine esterne

Se hai implementato la versione Beta di Munchkin JavaScript su pagine web esterne e desideri rinunciare a questa modifica finché non sarà generalmente disponibile, devi modificare il frammento JS di Munchkin per eseguire il targeting di **munchkin.**&#x200B;**js** file invece di **munchkin-beta.**&#x200B;**js** file. Nell&#39;esempio seguente, questo è il valore della variabile **s.src** alla riga 11. Il frammento potrebbe non essere molto simile all’esempio o potrebbe essere distribuito da un gestore di tag sulle pagine esterne e potresti dover contattare le risorse IT o chiunque gestisca i siti web con il tracciamento di Munchkin abilitato.

```javascript
<script type="text/javascript">
(function() {
  var didInit = false;
  function initMunchkin() {
    if(didInit === false) {
      didInit = true;
      Munchkin.init('299-BYM-827');
    }
  }
  var s = document.createElement('script');
  s.type = 'text/javascript';
  s.async = true;
  s.src = '//munchkin.marketo.net/munchkin.js';//This line should have the munchkin.js file, not munchkin-beta.js
  s.onreadystatechange = function() {
    if (this.readyState == 'complete' || this.readyState == 'loaded') {
      initMunchkin();
    }
  };
  s.onload = initMunchkin;
  document.getElementsByTagName['head'](0).appendChild(s);
})();
</script>
```

Pubblicato il _2021-01-08_ da _Kenny_

## API finale obsoleta di E-mail V1

[La deprecazione dell&#39;e-mail V1 è iniziata quasi due anni fa](https://nation.marketo.com:443/t5/knowledgebase/email-editor-1-0-is-being-deprecated-june-18th/ta-p/250666) e a partire dalla versione di manutenzione di marzo per gli abbonamenti a Londra e Paesi Bassi il 17 marzo 2021 e tutti gli altri abbonamenti il 19 marzo 2021, tutto il supporto API per le e-mail V1 verrà terminato. Dopo questa versione, eventuali tentativi di interagire con le e-mail V1 tramite le API Asset genereranno errori e non verranno intraprese azioni. Sono state inviate notifiche a tutti gli utenti rimanenti noti a partire dal 24 febbraio 2021, ma è possibile che esistano ancora integrazioni che potrebbero tentare di interagire con queste risorse. I tipi più comuni di integrazioni interessate sono i servizi che offrono gestione delle risorse digitali, traduzione e localizzazione. Se a seguito di questa modifica si verificano errori di integrazione, [sarà comunque possibile aggiornare le risorse problematiche modificandole e approvandole](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/email-marketing/general/email-editor-2/transitioning-to-email-editor-2-0). Una volta aggiornata una risorsa e-mail alla versione 2, dovresti poter riprendere a utilizzarla con i servizi integrati.

Pubblicato il _2021-03-17_ da _Kenny_

## Aggiornamenti di maggio 2021

A maggio 2021 verranno rilasciate nuove API REST, verranno migliorate le API REST esistenti e verranno risolti diversi problemi. Di seguito è riportato l&#39;elenco completo degli aggiornamenti.

* Sono state aggiunte API per i membri del programma che consentono di recuperare, aggiornare ed eliminare i record di iscrizione al programma. Per ulteriori informazioni, vedere [API REST > Database lead > Membri del programma](/help/rest-api/program-members.md).
* Sono state aggiunte API Bulk Custom Object Extract che consentono di esportare record di oggetti personalizzati di primo livello di Marketo associati a lead in una relazione uno-a-molti. Per ulteriori informazioni, vedere [API REST > Estrazione in blocco > Estrazione oggetto personalizzata in blocco](/help/rest-api/bulk-custom-object-extract.md).
* Sono state migliorate sia l&#39;API [Lead](/help/rest-api/leads.md) che l&#39;API [Bulk Lead Extract](/help/rest-api/bulk-lead-extract.md) per consentire agli utenti di recuperare l&#39;ID Adobe Experience Cloud (ECID). Questo consente agli utenti che [sincronizzano i tipi di pubblico da Adobe Experience Cloud](https://experienceleague.adobe.com/docs/marketo/using/product-docs/core-marketo-concepts/miscellaneous/set-up-adobe-experience-cloud-audience-sharing.html) di identificare i lead a cui sono associati ECID. In questo modo si aprono [possibilità di integrazione](https://adobeexchangeec.zendesk.com/hc/en-us/articles/360024277392-Adobe-Experience-Cloud-Using-the-ECID-for-integration) con altri prodotti Adobe Experience Cloud.
* L&#39;API [Importazione lead in blocco](/help/rest-api/bulk-lead-import.md) è stata migliorata per supportare l&#39;aggiunta di lead ai record aziendali durante il processo di importazione. A tale scopo, includere il campo **externalCompanyId** nel file di importazione.
* Sono stati migliorati diversi endpoint del programma per fornire parità con le funzionalità presenti nell’interfaccia utente di Marketo Engage. Sono stati migliorati gli endpoint [Crea programmi](/help/rest-api/assets.md) e [Clona programmi](https://developer.adobe.com/marketo-apis/api/asset/) per consentire la creazione, la clonazione o lo spostamento di operazioni sui programmi evento. Questo è per gli utenti che organizzano i programmi evento &quot;nidificandoli&quot; sotto altri tipi di programmi. Abbiamo anche migliorato l&#39;endpoint [Elimina programma](https://developer.adobe.com/marketo-apis/api/asset/) per consentire l&#39;eliminazione dei programmi che contengono le seguenti risorse: notifiche push, messaggi in-app, report, pagine di destinazione con Assets social incorporato.
* In qualità di amministratore di Marketo, puoi [contrassegnare un campo specifico come &quot;sensibile&quot;](https://experienceleague.adobe.com/it/docs/marketo/using/home), in modo che i relativi valori [non vengano mai precompilati nei moduli](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/demand-generation/forms/form-fields/disable-pre-fill-for-a-form-field), proteggendo in tal modo i dati sensibili degli utenti. Sono stati migliorati diversi endpoint per campi modulo per fornire parità con questa funzionalità presente nell’interfaccia utente di Marketo Engage.

### Risoluzioni dei difetti

* È stato risolto un problema relativo all’endpoint Elimina programma. Se si tenta di eliminare un programma in una cartella condivisa, viene restituito &quot;611, Errore di sistema&quot;. Ora restituisce &quot;Il programma di destinazione si trova in una cartella condivisa e non può essere eliminato. Prima di tentare l’eliminazione, è necessario annullare la condivisione della cartella.&quot;
* È stato risolto un problema relativo all’endpoint del programma Clone. Se si tenta di clonare un programma che contiene un valore DateTime in un passaggio di flusso, verrà restituito &quot;611, Errore di sistema&quot;. Ora il programma viene clonato correttamente.
* È stato risolto un problema con l’endpoint Create Programs (Crea programmi) che consentiva inavvertitamente di creare un programma sotto un programma e-mail (operazione non consentita).
* È stato risolto un problema relativo all’endpoint del programma Clone. Se hai clonato un programma che conteneva una pagina di destinazione, nel nome della pagina di destinazione nel programma di destinazione mancava un trattino basso tra il nome del programma e il nome della pagina di destinazione. Esempio: `http://<_pod_\>.marketo.com/lp/<_munchkin_\>/<_program name_\>**_**<_LP name_\>.html`

Pubblicato il _2021-05-07_ da _David_

## Offerta limitata di tempo per partecipare ad Adobe Exchange

Il supporto alla community dei partner Marketo Engage è uno dei pilastri del successo dei nostri clienti. Vogliamo garantire che l’ecosistema di integrazione di Marketo Engage sia ben rappresentato nel Marketplace di Exchange e abbiamo un’offerta speciale per i partner LaunchPoint. Per un periodo di tempo molto limitato che non verrà prorogato, offriamo ai nostri partner LaunchPoint una partnership gratuita Innovate nel programma Exchange fino alla fine del 2022 (valore di circa $ 15.000). Questa offerta è stata progettata per incoraggiare i partner LaunchPoint a creare elenchi di integrazioni nel portale Exchange Partner, che sarà quindi possibile cercare pubblicamente nel Marketplace Adobe Exchange. Per visualizzare un elenco completo dei vantaggi della partnership Innovate che riceverai gratuitamente fino a dicembre 2022.

1. Vai al [Centro assistenza partner Adobe Exchange](https://adobeexchangeec.zendesk.com/hc/en-us?mkt_tok=NjA4LURIVi05MTUAAAF-P5lIeVWOuBmKMS_uE_NpgFKtC0ukt7z_ksnq_Sbzb6mzXUuXpqpqQeujtPdZ24WcjMdptygQSR9XrYt_Cw)
1. Fai clic su &quot;Invia una richiesta&quot; in alto a destra
1. In **Scegliere il problema seguente** scegliere &quot;Supporto Adobe Exchange&quot;
1. In **Indirizzo e-mail** immetti il tuo indirizzo e-mail
1. Nella casella **Oggetto** immettere &quot;Offerta LaunchPoint&quot;
1. Nella casella **Descrizione** immettere &quot;Offerta LaunchPoint&quot;.
1. Nel menu a discesa **Tipo di supporto**, seleziona &quot;Supporto del programma&quot;
1. Nel menu a discesa **Prodotto Adobe Exchange**, seleziona &quot;Programma Adobe Exchange&quot;
1. Inviare il modulo. Il nostro team ti contatterà a breve.

Pubblicato il _2021-07-22_ da _David_

## Aggiornamenti di agosto 2021

Ad agosto 2021 stiamo migliorando le API REST esistenti e risolvendo diversi difetti. Di seguito è riportato l&#39;elenco completo degli aggiornamenti.

* Abbiamo migliorato l’API di estrazione dell’attività in blocco per consentire agli utenti di filtrare utilizzando gli attributi primari per 6 tipi di attività diversi. Per ulteriori informazioni, vedi [Estrazione attività in blocco](/help/rest-api/bulk-activity-extract.md).
* Per consentire agli utenti di Marketo Sales Connect di accedere più facilmente ai dati delle loro attività di vendita, abbiamo abilitato attributi aggiuntivi per le attività di vendita. Abbiamo aggiunto i seguenti attributi alle attività Invia e-mail vendite, Apri e-mail vendite e Fai clic su E-mail vendite:

* ID persona vendita Marketo - ID univoco per il record persona in Sales Connect
* Nome campagna di vendita: nome della campagna di vendita, se l’e-mail faceva parte di una campagna di vendita
* URL della campagna di vendita - URL di Sales Connect per la campagna di vendita se l’e-mail faceva parte di una campagna di vendita
* Nome modello di vendita: nome del modello di e-mail in Sales Connect, se è stato utilizzato un modello
* URL modello vendite - URL Sales Connect per modello e-mail, se è stato utilizzato un modello

### E-mail

* È stato migliorato l&#39;endpoint Get Emails aggiungendo il filtro `earliestUpdatedAt`/`latestUpdatedAt`. Questo consente di utilizzare il campo `updatedAt` per cercare solo un sottoinsieme di e-mail, consentendo la sincronizzazione incrementale.
* Abbiamo migliorato gli endpoint Get Email, Get Email by Name, Get Email by Id per supportare il recupero dei record e-mail di tipo [Champion e Challenger](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/email-marketing/general/functions-in-the-editor/email-tests-champion-challenger/add-an-email-champion-challenger).

### Risoluzioni dei difetti

* È stato risolto un problema relativo all’endpoint Get Users. Gli utenti a cui era stata rilasciata una licenza di [Marketing Calendar](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/core-marketo-concepts/marketing-calendar/understanding-the-calendar/issue-revoke-a-marketing-calendar-license) non venivano restituiti. Gli utenti del calendario di marketing ora vengono restituiti correttamente.
* È stato risolto un problema relativo all’endpoint &quot;Invia modulo&quot;. In caso di record di lead duplicati, il modulo di invio viene utilizzato per emettere un errore &quot;1007, Multiple lead match criteria&quot; (criteri di ricerca di più lead). Invia modulo ora aggiorna il record aggiornato più di recente in modo analogo all&#39;API [Forms 2.0](/help/javascript-api/forms-api-reference.md).
* Sono stati migliorati diversi messaggi di errore fuorvianti restituiti dagli endpoint Aggiorna campo lead e Crea campi lead. [LM-151890, LM-151888, LM-151889]
* È stato risolto un problema relativo agli endpoint Get Lead Field by Name e Get Lead Fields. Entrambi gli endpoint potrebbero potenzialmente restituire informazioni leggermente non aggiornate. Ora restituiscono sempre le informazioni correnti.
* È stato risolto un problema relativo all&#39;API [Bulk Extract API](/help/rest-api/bulk-extract.md) quando si utilizza l&#39;intestazione HTTP &quot;range&quot; per il recupero parziale, in cui l&#39;ultimo byte dell&#39;intervallo non è stato restituito.
* È stato risolto un problema relativo all’endpoint Update Snippet Metadata. Quando si aggiornava il nome o la descrizione del frammento, lo stato del frammento veniva modificato in &quot;approvato con bozza&quot;. Ora lo stato del frammento rimane invariato dopo l&#39;aggiornamento del nome o della descrizione del frammento.
* È stato risolto un problema relativo all’endpoint Aggiungi modulo e-mail. Quando si aggiunge un modulo che contiene un frammento, viene restituito un errore di sistema &quot;611&quot;. Ora aggiunge correttamente il modulo all’e-mail.
* È stato risolto un problema relativo all’endpoint Elimina programma. Quando si elimina un programma che conteneva una risorsa locale per i messaggi in-app, viene restituito un errore di sistema &quot;611&quot;. Ora elimina correttamente il programma.

Pubblicato il _2021-08-22_ da _David_

## Rollout di Munchkin versione 161

Il 7 settembre 2021, la versione 161 di Munchkin inizierà a distribuire il 10% degli abbonamenti con Munchkin Beta abilitato, seguita dal 50% il 16 settembre e dal 100% il 30 settembre. Questa modifica interessa le pagine di destinazione di Marketo e la versione del file munchkin-beta.js trasmessa alle pagine di destinazione esterne che vengono caricate dagli abbonamenti a cui è stato distribuito la nuova versione. Questa versione depreca completamente il metodo Munchkin Associate Lead, una funzione che consente l’invio di dati personali a un abbonamento Marketo e la cronologia di esplorazione Web associata a un record persona noto. È in corso la rimozione di Associa lead a favore di alternative più moderne e sicure, come [Forms JS API](/help/javascript-api/forms-api-reference.md), Form Submit API e [Associate Lead REST API](/help/rest-api/leads.md). Se tu o la tua organizzazione utilizzi questo metodo, devi effettuare la migrazione dall’utilizzo entro il 12 ottobre 2021, quando è pianificato l’inizio del rollout della versione di ottobre. Se non desideri più partecipare alla versione beta di Munchkin, puoi disabilitare l&#39;utilizzo nelle pagine di destinazione di Marketo attivando la funzione &quot;Munchkin Beta sulle pagine di destinazione&quot; su `disabled` nel [menu Sfondo tesoro](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/settings/enable-or-disable-treasure-chest-features). Se hai implementato Munchkin Beta JavaScript in pagine web esterne e desideri passare al canale di rilascio predefinito di Munchkin, devi aggiornare il frammento di codice per caricare Munchkin JavaScript da munchkin.js invece di munchkin-beta.js.

Pubblicato il _2021-08-24_ da _Kenny_

## Terminazione di TLS 1.0 e TLS 1.1 per il servizio Munchkin

A partire dal 21 ottobre 2021, munchkin.marketo.net, utilizzato per distribuire Munchkin JavaScript ai visitatori, non accetterà più connessioni che utilizzano [TLS 1.0 o TLS 1.1.](https://en.wikipedia.org/wiki/Transport_Layer_Security) Questi standard di crittografia non sono più accettati come parte delle best practice per la sicurezza web e non sono più supportati dalle versioni moderne del browser. Non si prevedono effetti negativi a seguito di questo cambiamento.

Pubblicato il _2021-10-04_ da _Kenny_

## Aggiornamenti di ottobre 2021

A ottobre 2021 stiamo migliorando le API REST esistenti e risolvendo diversi difetti. Di seguito è riportato l&#39;elenco completo degli aggiornamenti.

* L&#39;endpoint [Invia modulo](https://developer.adobe.com/marketo-apis/api/mapi/#operation/SubmitFormUsingPOST) è stato migliorato per supportare i campi personalizzati dei membri del programma come parte dell&#39;invio del modulo. Facoltativamente, è possibile specificare un programma come programma a cui aggiungere il modulo e/o il programma a cui aggiungere i campi personalizzati del membro del programma come descritto [qui](/help/rest-api/leads.md).
L&#39;endpoint [Ottieni membri del programma](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getProgramMembersUsingGET) è stato migliorato per supportare query basate su intervalli di date basate sull&#39;attributo updatedAt. Questa operazione viene eseguita passando i parametri datetime iniziale e finale come descritto [qui](/help/rest-api/program-members.md).
* Le API [Leads Fields](/help/rest-api/leads.md) sono state migliorate per supportare [Sensitive Fields](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/field-management/mark-a-field-as-sensitive). L&#39;attributo isSensitive è ora supportato dagli endpoint [Ottieni campo lead per nome](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadFieldByNameUsingGET), [Ottieni campi lead](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadFieldsUsingGET), [Crea campi lead](https://developer.adobe.com/marketo-apis/api/mapi/#operation/createLeadFieldUsingPOST) e [Aggiorna campo lead](https://developer.adobe.com/marketo-apis/api/mapi/#operation/updateLeadFieldUsingPOST).

### Risoluzioni dei difetti

* È stato risolto un problema relativo all&#39;API [User Management](/help/rest-api/user-management.md). Riguarda gli utenti Marketo configurati per l&#39;utilizzo con [Sales Insight](https://business.adobe.com/products/marketo/sales-insight.html). Questi utenti vengono ora restituiti dall&#39;endpoint [Get Users](https://developer.adobe.com/marketo-apis/api/user/#operation/getUsersUsingGET) e possono essere eliminati utilizzando l&#39;endpoint [Delete User](https://developer.adobe.com/marketo-apis/api/user/#operation/deleteUserUsingPOST). [LM-155864]
* È stato risolto un problema con l&#39;endpoint Add [Rich Text Field](https://developer.adobe.com/marketo-apis/api/asset/#tag/Form-Fields/addRichTextFieldUsingPOST). Quando si aggiunge un campo in formato Rich Text con più di 65.000 caratteri a un’e-mail, a una pagina di destinazione, a uno snippet o a un modulo, viene restituito un errore di sistema di 611 caratteri. Ora restituisce l’errore &quot;701, Operazione non può essere completata. &#39;content&#39; supera una lunghezza massima di 65.535 byte&quot;.

Pubblicato il _2021-10-25_ da _David_

## Aggiornamenti di gennaio 2022

A gennaio 2022 stiamo migliorando le API REST esistenti e risolvendo diversi difetti. Di seguito è riportato l&#39;elenco completo degli aggiornamenti.

* È stata migliorata l&#39;API [Bulk Custom Object Extract](/help/rest-api/bulk-custom-object-extract.md) per consentire agli utenti di filtrare utilizzando un intervallo di date **updatedAt**.
* Sono state aggiunte API di metadati per i campi dei membri del programma, che consentono di creare, aggiornare e recuperare metadati per i campi dei membri del programma. Per ulteriori informazioni, vedere [Membri del programma > Campi](/help/rest-api/program-members.md).
* Sono state aggiunte API di metadati per campi aziendali, che consentono di recuperare i metadati per i campi aziendali. Per ulteriori informazioni vedere [Aziende > Campi](/help/rest-api/companies.md).
* Sono state aggiunte API di metadati per i campi Opportunità, che consentono di recuperare i metadati per i campi Opportunità. Per ulteriori informazioni, vedere [Opportunità > Campi](/help/rest-api/opportunities.md).
* Sono state aggiunte API per i metadati dei campi Account denominato, che consentono di recuperare i metadati per i campi Account denominato. Per ulteriori informazioni, vedere [Account denominati > Campi](/help/rest-api/named-accounts.md).
* Sono stati aggiornati gli endpoint per i metadati dei campi per restituire una nuova proprietà booleana **isApiCreated** per indicare se un campo è stato creato o meno dall&#39;API REST.

### Risoluzioni dei difetti

* È stato risolto un problema di latenza tra l&#39;ora della chiamata all&#39;endpoint [Crea campi lead](https://developer.adobe.com/marketo-apis/api/mapi/#operation/createLeadFieldUsingPOST) e l&#39;ora in cui il campo lead appena creato era disponibile nell&#39;elenco avanzato. [LM-152838]
* È stato risolto un problema con l&#39;endpoint [Crea campi lead](https://developer.adobe.com/marketo-apis/api/mapi/#operation/createLeadFieldUsingPOST) a causa del quale i campi creati non erano disponibili nell&#39;elenco a discesa dei campi modulo utilizzato per [aggiungere campi al modulo](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/demand-generation/forms/creating-a-form/add-a-field-to-a-form) nell&#39;interfaccia utente di Marketo Engage. [LM-158243]
* È stato risolto un problema con l&#39;endpoint [Get Campaigns](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getCampaignsUsingGET) a causa del quale non venivano restituite campagne attivabili quando era specificato il parametro isTriggerable=true. [LM-158283]
* È stato risolto un problema a causa del quale l&#39;endpoint [Get Leads by List Id](https://developer.adobe.com/marketo-apis/api/mapi/#operation/deleteTokenByNameUsingPOST) restituiva l&#39;errore &quot;611, System error&quot; in alcuni casi. [LM-157214]
* Sono stati eliminati diversi messaggi di errore restituiti dall&#39;endpoint [Aggiorna campo lead](/help/rest-api/leads.md). [LM-151886, LM-151888, LM-151889]

Pubblicato il _2022-01-27_ da _David_

## Aggiornamenti di marzo 2022

A marzo 2022 stiamo migliorando le API REST esistenti e risolvendo diversi difetti. Di seguito è riportato l&#39;elenco completo degli aggiornamenti.

* Il campo **actionResult** è stato aggiunto al file di esportazione prodotto dall&#39;API Bulk Activity Extract. Questo campo può essere utilizzato per distinguere tra attività riuscite, saltate e non riuscite.
* Il campo **isOpenTrackingDisabled** è stato aggiunto alle risposte dell&#39;[API Email](/help/rest-api/emails.md). Questo campo può essere utilizzato per determinare se la funzionalità [Disabilita tracciamento apertura](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/email-marketing/general/email-editor-2/email-editor-v2-0-overview) è abilitata.
* Sono stati aggiunti due endpoint che consentono di gestire selettivamente i tag del programma. L&#39;endpoint [Aggiorna tag programma](/help/rest-api/programs.md) consente di aggiornare in modo selettivo un tag programma. L&#39;endpoint [Elimina tag di programma](/help/rest-api/programs.md) consente di eliminare in modo selettivo un tag di programma.
* Il parametro **isExecutable** è stato aggiunto all&#39;endpoint [Clone Smart Campaign](/help/rest-api/smart-campaigns.md). Questo parametro consente di clonare un programma come programma eseguibile.
* Il campo **headStart** è stato aggiunto all&#39;API [Programmi](/help/rest-api/programs.md). Questo consente di creare, aggiornare e recuperare l&#39;impostazione [Inizio intestazione](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/email-marketing/email-programs/email-program-actions/head-start-for-email-programs) per i programmi e-mail.

### Risoluzioni dei difetti

* È stato risolto un problema con l&#39;endpoint [Get Email Dynamic Content](https://developer.adobe.com/marketo-apis/api/asset/#operation/getEmailDynamicContentUsingGET). Quando si tentava di recuperare righe dell’oggetto con contenuto dinamico da e-mail con relazioni di modello non funzionanti, veniva restituito un errore, 709, &quot;L’API consente solo operazioni su e-mail con un modello&quot;. L’endpoint ora restituisce il contenuto dinamico. [LM-152331]
* È stato risolto un problema con l&#39;endpoint [Sincronizza lead](https://developer.adobe.com/marketo-apis/api/mapi/#operation/syncLeadUsingPOST). Quando si utilizza externalSalesPersonId per associare la persona di vendita a un lead utilizzando externalSalesPersonId e si utilizza action = createDuplicate, l&#39;associazione della persona di vendita non viene eseguita. [LM-158990]

### Integrazione Adobe IMS

* Gli utenti che hanno effettuato l&#39;onboarding in [Adobe IMS](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-with-adobe-identity/adobe-identity-management-overview) non possono utilizzare tutte le [API di gestione utenti di Marketo](/help/rest-api/user-management.md). I seguenti endpoint restituiranno un errore in caso di chiamata su istanze di Marketo che sono state integrate con Adobe IMS: [Invita utente](https://developer.adobe.com/marketo-apis/api/user/#operation/inviteUserUsingPOST), [Ottieni utente invitato da ID](https://developer.adobe.com/marketo-apis/api/user/#operation/getInvitedUserUsingGET), [Aggiorna attributi utente](https://developer.adobe.com/marketo-apis/api/user/#operation/updateUserAttributeUsingPOST), [Elimina utente](https://developer.adobe.com/marketo-apis/api/user/#operation/deleteUserUsingPOST) e [Elimina utente invitato](https://developer.adobe.com/marketo-apis/api/user/#operation/deleteInvitedUserUsingPOST). In sostituzione, è necessario utilizzare le [API di gestione utenti di Adobe](https://developer.adobe.com/umapi/).

Pubblicato il _2022-03-14_ da _David_

## Aggiornamenti di maggio 2022 -

A maggio 2022 stiamo migliorando le API REST esistenti e risolvendo diversi difetti. Di seguito è riportato l&#39;elenco completo degli aggiornamenti.

* È stata aggiunta la possibilità di recuperare i record [Società](/help/rest-api/companies.md), [Opportunità](/help/rest-api/opportunities.md) e [Persone di vendita](/help/rest-api/sales-persons.md) quando [Sincronizzazione SFDC](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/crm-sync/salesforce-sync/sfdc-sync-details/sfdc-sync-field-sync) o [Sincronizzazione Microsoft Dynamics](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/crm-sync/microsoft-dynamics/microsoft-dynamics-sync-details/microsoft-dynamics-sync-user-sync) sono abilitati nell&#39;istanza Marketo Engage.
* L&#39;endpoint [Get Email Dynamic Content](https://developer.adobe.com/marketo-apis/api/asset/#operation/getEmailDynamicContentUsingGET) è stato aggiornato per consentire il recupero di [Dynamic Content](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/email-marketing/general/functions-in-the-editor/using-dynamic-content-in-an-email) da una riga dell&#39;oggetto dell&#39;e-mail. Questo funziona indipendentemente dal fatto che l’e-mail specificata sia collegata a un modello e-mail.

`POST /rest/asset/v1/form/{id}/field/State.json?values=[{"label":"Alaska"},{"value":"AK"},{"label":"West Virginia","value":"WV"},{"label":"Wyoming","value":"WY"}]`

* L&#39;endpoint [Aggiungi regole di visibilità campo modulo](https://developer.adobe.com/marketo-apis/api/asset/#operation/getAllProgramMemberFieldsUsingGET) è stato aggiornato per consentire l&#39;aggiunta di più valori di confronto per **isNot** tipo [regole di invisibilità](/help/rest-api/forms.md). Ecco un esempio:

`POST /rest/asset/v1/form/{id}/field/LastName/visibility.json?visibilityRule={"ruleType":"show","rules":[{"subjectField":"LastName","operator":"isNot","values":["A","B","C"]}`

### Risoluzioni dei difetti

* È stato risolto un problema con l&#39;endpoint [Submit Form](/help/rest-api/leads.md) che si verificava quando si trasmetteva &quot;null&quot; per un attributo nel parametro [leadFormFields](/help/rest-api/leads.md). È stato restituito un errore, &quot;611, Errore di sistema&quot;. Ora restituisce correttamente l’errore &quot;1003, Convalida modulo non riuscita&quot;. [LM-162213]

Pubblicato il _2022-05-09_ da _David_

## Aggiornamenti di agosto 2022

Ad agosto 2022 stiamo migliorando le API REST esistenti. Di seguito è riportato l&#39;elenco completo degli aggiornamenti.

Sono stati aggiunti diversi nuovi filtri che possono essere utilizzati per richiamare l’endpoint del processo Crea membro del programma di esportazione. Tieni presente che molti filtri possono essere utilizzati in combinazione tra loro per perfezionare il set di dati estratto.

* Il filtro **programIds** può essere utilizzato per specificare fino a 10 identificatori di programma che possono contribuire a migliorare la velocità effettiva.
* Il filtro **isExhausted** può essere utilizzato per filtrare i record per [persone con contenuto esaurito](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/email-marketing/drip-nurturing/using-engagement-programs/people-who-have-exhausted-content).
* Il filtro **nurtureCadence** può essere utilizzato per filtrare i record in base alla [cadenza del programma di coinvolgimento](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/core-marketo-concepts/smart-campaigns/program-flow-actions/change-engagement-program-cadence).
* Il filtro **statusNames** può essere utilizzato per filtrare i record per uno o più [stati di programma](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/core-marketo-concepts/programs/creating-programs/understanding-program-membership).
* Il filtro **updatedAt** può essere utilizzato per filtrare i record in base a un intervallo di date.

### Annunci

* Il comportamento dell&#39;endpoint [Identity](https://developer.adobe.com/marketo-apis/api/identity/#operation/identityUsingGET) è stato modificato. Quando si chiama l&#39;endpoint e non si include il parametro **access_token**, viene restituito l&#39;errore &quot;603, Accesso negato&quot;. In precedenza, veniva restituito l’errore &quot;600, Empty access token&quot;. L’errore &quot;600, Empty access token&quot; è diventato obsoleto.

Pubblicato il _2022-09-03_ da _David_

## Aggiornamenti di ottobre 2022

A ottobre 2022 stiamo migliorando le API REST esistenti. Di seguito è riportato l&#39;elenco completo degli aggiornamenti.

* L&#39;API [Importazione lead in blocco](/help/rest-api/bulk-lead-import.md) è stata migliorata per supportare l&#39;aggiunta di lead ai record dei venditori durante il processo di importazione. A tale scopo, includere il campo **externalSalesPersonId** nel file di importazione.
* È stato risolto un problema con l&#39;endpoint [Crea campi lead](/help/rest-api/leads.md) che si verificava durante la creazione dei campi di tipo Punteggio. Questi campi non sono disponibili per l&#39;utilizzo nell&#39;azione di flusso [Change Score](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/core-marketo-concepts/smart-campaigns/flow-actions/change-score) nell&#39;interfaccia utente di Marketo Engage. [LM-166815]

### Annunci

* L&#39;attributo di appartenenza al programma `acquiredBy` è stato modificato per essere aggiornabile.

Pubblicato il _2022-10-18_ da _David_

## Prossima modifica all’app REST di Marketo Forms

A partire dalla versione 2022.R2, attualmente pianificata per la settimana del 24 marzo 2023, le API di Adobe Marketo Engage Forms Asset restituiranno in modo coerente solo il nome del modulo senza un nome di programma prefisso, indipendentemente dal fatto che il modulo sia o meno figlio di un programma. Con questa modifica, i comportamenti dell’API Forms per quanto riguarda i nomi delle risorse diventeranno coerenti con gli altri API delle risorse Adobe Marketo Engage. Per evitare interruzioni del servizio, controlla tutte le integrazioni che utilizzano le API Forms di Marketo Engage e collabora con gli integratori per verificare se sono necessarie modifiche per adattarsi a questo requisito. Prima di questa modifica imminente, ai nomi restituiti dalle API Forms veniva aggiunto in modo incoerente il prefisso del nome del programma per i moduli figli di programmi nelle attività di marketing. Ad esempio, un modulo denominato &quot;Modulo 1&quot; figlio di &quot;Programma 1&quot; potrebbe avere il nome restituito dall’API come: Programma 1.Modulo 1 O Modulo 1 A partire dalla versione 2022.R2, il nome di un modulo verrà sempre restituito senza un nome di programma prefisso. Utilizzando lo stesso esempio, il nome sarà sempre: Modulo 1

Pubblicato il _2022-11-04_ da _Kenny_

## Aggiornamenti di gennaio 2023

A gennaio 2023 è stato introdotto un miglioramento relativo all’API per l’interfaccia utente di amministrazione, a cui ora si aggiungono due annunci. Di seguito è riportato l&#39;elenco completo degli aggiornamenti.

Interfaccia utente amministratore

### Estrazione lead in blocco

* Abbiamo migliorato l’interfaccia utente di amministrazione di Marketo Engage per consentirti di visualizzare l’allocazione della capacità giornaliera dell’API di estrazione in blocco per il tuo abbonamento. Inoltre, puoi visualizzare la capacità utilizzata dall’utente API negli ultimi 7 giorni. Ulteriori informazioni sono disponibili [qui](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/settings/bulk-export-api-information).

### Risoluzioni dei difetti

* È stato risolto un problema con l&#39;endpoint [Elimina opportunità](https://developer.adobe.com/marketo-apis/api/mapi/#operation/deleteOpportunitiesUsingPOST). In alcuni casi non veniva generata un’attività &quot;Rimuovi dall’opportunità&quot;. [LM-172208]

### Annunci

* Consulta [questo articolo](https://nation.marketo.com/t5/product-documents/upcoming-change-to-marketo-rest-api/ta-p/331698) sulla community Marketo relativo all&#39;API REST e una modifica al messaggio di risposta HTTP [frase motivo](https://www.rfc-editor.org/rfc/rfc7230#section-3.1.2).
* L&#39;attributo di appartenenza al programma **statusReason** è stato modificato per essere aggiornabile.

Pubblicato il _2023-01-21_ da _David_
