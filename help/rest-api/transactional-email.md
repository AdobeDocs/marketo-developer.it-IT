---
title: E-mail transazionale
feature: REST API
description: Gestisce le e-mail transazionali per le campagne di richiesta.
exl-id: 057bc342-53f3-4624-a3c0-ae619e0c81a5
source-git-commit: 981ed9b254f277d647a844803d05a1a2549cbaed
workflow-type: tm+mt
source-wordcount: '971'
ht-degree: 0%

---

# E-mail transazionale

Un caso d&#39;uso comune per l&#39;API Marketo è quello di attivare l&#39;invio di e-mail transazionali a record specifici tramite la chiamata API [Richiedi campagna](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Campaigns/operation/triggerCampaignUsingPOST). In Marketo sono presenti alcuni requisiti di configurazione per eseguire la chiamata richiesta con l’API REST di Marketo.

- Il destinatario deve avere un record in Marketo
- Nell’istanza di Marketo deve essere stato creato e approvato un messaggio e-mail transazionale.
- Deve essere presente una campagna trigger attiva con &quot;Campaign is Requested, 1. Source: Web Service API&quot;, configurato per inviare l’e-mail

Crea e approva il tuo indirizzo e-mail[. ](https://experienceleague.adobe.com/docs/marketo/using/home.html?lang=it) Se l’e-mail è effettivamente transazionale, probabilmente dovrai impostarla su operativa, ma assicurati che sia giuridicamente qualificata come operativa. Questa è configurata da con la schermata Edit (Modifica) in Email Actions (Azioni e-mail) > Email Settings (Impostazioni e-mail):

![Richieste-Campagne-Impostazioni-E-Mail](assets/request-campaign-email-settings.png)

![Richiesta-Campagna-Operativa](assets/request-campaign-operational.png)

Approvalo e siamo pronti a creare la nostra campagna:

![RequestCampaign-Approve-Draft](assets/request-campaign-approve-draft.png)

Se non hai ancora creato le campagne, consulta l&#39;articolo [Creare una nuova campagna avanzata](https://experienceleague.adobe.com/docs/marketo/using/product-docs/core-marketo-concepts/smart-campaigns/creating-a-smart-campaign/create-a-new-smart-campaign.html). Dopo aver creato la campagna, dobbiamo seguire questi passaggi. Configura l’elenco avanzato con il trigger Campaign is Requested:

![Richiesta-Campagna-Elenco Smart](assets/request-campaign-smart-list.png)

Ora è necessario configurare il flusso in modo che indirizzi un passaggio Invia e-mail alla nostra e-mail:

![Richiesta-Campagna-Flusso](assets/request-campaign-flow.png)

Prima dell’attivazione, è necessario decidere alcune impostazioni nella scheda Pianificazione. Se questa e-mail in particolare deve essere inviata una sola volta a un determinato record, lascia invariate le impostazioni di qualifica. Tuttavia, se è necessario che ricevano l’e-mail più volte, è necessario modificarla ogni volta o in base a una delle cadenze disponibili:

Ora è possibile attivare:

![Richiesta-Campagna-Pianificazione](assets/request-campaign-schedule.png)

## Invio delle chiamate API

**Nota:** negli esempi Java seguenti, utilizziamo il [pacchetto minimal-json](https://github.com/ralfstx/minimal-json) per gestire le rappresentazioni JSON nel nostro codice.

La prima parte dell’invio di un’e-mail transazionale tramite l’API consiste nel garantire che nell’istanza Marketo esista un record con l’indirizzo e-mail corrispondente e che sia possibile accedere al relativo ID lead. Ai fini di questo post, supponiamo che gli indirizzi e-mail siano già in Marketo e che si debba recuperare solo l’ID del record. Per questo, stiamo utilizzando la chiamata [Get Leads by Filter Type](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/getLeadsByFilterUsingGET). Vediamo il nostro metodo principale per richiedere la campagna:

```java
package dev.marketo.blog_request_campaign;

import com.eclipsesource.json.JsonArray;

public class App
{
    public static void main( String[] args )
    {
        //Create an instance of Auth so that we can authenticate with our Marketo instance
        Leads leadsRequest = new Leads(auth).setFilterType("email").addFilterValue("requestCampaign.test@marketo.com");

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

```java
JsonArray leadsResult = leadsRequest.getData().get("result").asArray();
int leadId = leadsResult.get(0).asObject().get("id").asInt();
```

Da qui ora tutto quello che dobbiamo fare è la chiamata Request Campaign. A questo scopo, i parametri richiesti sono ID nell’URL della richiesta e una matrice di oggetti JSON contenente un membro, &quot;id&quot;. Vediamo il codice per questo:

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
            System.out.println("Executing RequestCampaign call\n" + "Endpoint: " + endpoint + "\nRequest Body:\n"  + requestBody);
            URL url = new URL(endpoint);
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
            System.out.println("Result:\n" + result);
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

Questa classe ha un costruttore che esegue un’autenticazione e l’ID della campagna. I lead vengono aggiunti all&#39;oggetto passando un `ArrayList<Integer>` contenente gli ID dei record a setLeads oppure utilizzando addLead, che prende un numero intero e lo aggiunge all&#39;oggetto ArrayList esistente nella proprietà lead. Per attivare la chiamata API per passare i record dei lead alla campagna, è necessario chiamare postData, che restituisce un JsonObject contenente i dati di risposta della richiesta. Quando viene chiamata la campagna di richiesta, ogni lead passato alla chiamata verrà elaborato dalla campagna del trigger di destinazione in Marketo e gli verrà inviata l’e-mail creata in precedenza. Congratulazioni, hai attivato un’e-mail tramite l’API REST di Marketo. Tieni d’occhio la Parte 2, in cui esaminiamo la personalizzazione dinamica del contenuto di un’e-mail tramite Request Campaign.

### Creazione dell’e-mail

Per personalizzare il contenuto, è innanzitutto necessario configurare un [programma](https://experienceleague.adobe.com/docs/marketo/using/product-docs/core-marketo-concepts/programs/creating-programs/create-a-program.html) e un [messaggio e-mail](https://experienceleague.adobe.com/docs/marketo/using/home.html?lang=it) in Marketo. Per generare il contenuto personalizzato, è necessario creare token all’interno del programma, quindi inserirli nell’e-mail che stiamo per inviare. Per semplicità, in questo esempio viene utilizzato un solo token, ma è possibile sostituire qualsiasi numero di token in un’e-mail, in Da e-mail, Da nome, Risposta o qualsiasi parte di contenuto nell’e-mail. Quindi creiamo un token Rich Text per la sostituzione e chiamiamolo &quot;bodyReplacement&quot;. Il formato Rich Text consente di sostituire qualsiasi contenuto nel token con HTML arbitrari che si desidera inserire.

![Nuovo token](assets/New-Token.png)

Non è possibile salvare i token mentre sono vuoti, quindi inserisci qui del testo segnaposto. Ora dobbiamo inserire il token nell’e-mail:

![Aggiungi token](assets/Add-Token.png)

Questo token sarà ora accessibile per la sostituzione tramite una chiamata Request Campaign. Questo token può essere semplice come una singola riga di testo che deve essere sostituita per e-mail o può includere quasi l’intero layout dell’e-mail.

### Il codice

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
        String bodyReplacement = "<div class=\"replacedContent\"><p>This content has been replaced</p></div>";
        rc.addToken("{{my.bodyReplacement}}", bodyReplacement);
        rc.postData();
    }
}
```

Se il codice ha un aspetto familiare, è perché ha solo due righe aggiuntive dal metodo principale precedente. Questa volta stiamo creando il contenuto del token nella variabile bodyReplacement e quindi utilizzando il metodo addToken per aggiungerlo alla richiesta. addToken prende una chiave e un valore, quindi crea una rappresentazione JsonObject e la aggiunge all’array dei token interni. Viene quindi serializzato durante il metodo postData e viene creato un corpo simile al seguente:

```json
{
    "input":
    {
        "leads": [
            {
                "id": 1
            }
        ],
        "tokens": [
            {
                "name": "{{my.bodyReplacement}}",
                "value": "<div class=\"replacedContent\"><p>This content has been replaced</p></div>"
            }
        ]
    }
}
```

Combinato, l’output della console si presenta così:

```bash
Token is empty or expired. Trying new authentication
Trying to authenticate with ...
Got Authentication Response: {"access_token":"19d51b9a-ff60-4222-bbd5-be8b206f1d40:st","token_type":"bearer","expires_in":3565,"scope":"apiuser@mktosupport.com"}
Executing RequestCampaign call
Endpoint: .../rest/v1/campaigns/1578/trigger.json
Request Body:
{"input":{"leads":[{"id":1}],"tokens":[{"name":"{{my.bodyReplacement}}","value":"<div class=\"replacedContent\"><p>This content has been replaced</p></div>"}]}}
Result:
{"requestId":"1e8d#14eadc5143d","result":[{"id":1578}],"success":true}
```

## Ritorno a capo

Questo metodo è estensibile in diversi modi, modificando il contenuto nelle e-mail all’interno di singole sezioni di layout o all’esterno delle e-mail, consentendo di trasmettere valori personalizzati in attività o momenti interessanti. Con questo metodo è possibile personalizzare qualsiasi punto in cui un token può essere utilizzato all’interno di un programma. Funzionalità simili sono disponibili anche con la chiamata [Pianifica campagna](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Campaigns/operation/scheduleCampaignUsingPOST) che consente di elaborare i token in un&#39;intera campagna batch. Non possono essere personalizzate in base al singolo lead, ma sono utili per personalizzare il contenuto in un ampio insieme di lead.
