---
title: Riferimento endpoint
feature: REST API
description: Riferimenti endpoint API Marketo
exl-id: 27d16b6f-865a-4e40-ab9c-cbabe2927472
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '216'
ht-degree: 0%

---

# Riferimento endpoint

- [Risorsa](https://developer.adobe.com/marketo-apis/api/asset/)
- [Identità](https://developer.adobe.com/marketo-apis/api/identity/)
- [Database lead](https://developer.adobe.com/marketo-apis/api/mapi/)
- [Gestione utente](https://developer.adobe.com/marketo-apis/api/user/)

## Riferimento endpoint

Marketo utilizza Swagger per fornire una definizione formale dell’interfaccia pubblica per le sue API REST. Swagger fornisce un modello ad alta definizione per strutture di URL, modelli di richiesta e modelli di risposta e ha un ecosistema sviluppato di strumenti da utilizzare con l&#39;interazione API, il test e la generazione client.

Il riferimento all&#39;endpoint utilizza il pacchetto JavaScript [Swagger-UI](https://swagger.io/tools/swagger-ui/) per generare le pagine di riferimento sul lato client. Ogni endpoint pubblico viene elencato e fornisce la struttura del modello di risposta, i parametri di richiesta richiesti e il modello di richiesta, se necessario.

## Utilizzo delle definizioni Swagger di Marketo

Lo standard Swagger richiede che sia fornito un host, o che l&#39;host sia generato dall&#39;host che serve il file. È importante correggere l’host nella definizione prima di utilizzare gli strumenti esistenti, in quanto Marketo fornisce un parametro host vuoto con la definizione. L’host REST API per ogni istanza di Marketo è univoco e segue il modello:

`{Munchkin ID}.mktorest.com`

## API di Asset

Le API di Marketo Asset utilizzano parametri di stile `application/x-www-url-formencoded` nelle richieste di endpoint che richiedono un metodo POST. In alcuni casi, tuttavia, ad esempio i parametri di cartella, il valore del parametro può essere un array JSON o un oggetto. Questi parametri sono indicati nel riferimento dell’endpoint.
