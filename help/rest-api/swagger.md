---
title: Scarica definizioni Swagger
feature: REST API, Programs
description: Scarica i file di definizione Swagger per l'utilizzo locale.
source-git-commit: 85062243d57a3fc6d15251163e926495858edf2a
workflow-type: tm+mt
source-wordcount: '170'
ht-degree: 0%

---

# Scarica definizioni Swagger

[Swagger](https://swagger.io/) o [OpenAPI](https://www.openapis.org/) Le definizioni sono file di dati che descrivono tutti i metodi e i parametri di un’API REST. Puoi scaricare e utilizzare questi file di dati localmente come riferimento API personale.

Per utilizzare le definizioni di Marketo Engage Swagger/OpenAPI, `host` deve essere aggiornato per corrispondere al nome host REST API dalla tua istanza di Marketo Engage.

Scarica innanzitutto la definizione OpenAPI che desideri utilizzare:

* [Risorsa](assets/swagger-asset.json)
* [Lead](assets/swagger-mapi.json)
* [Identità   ](assets/swagger-identity.json)
* [Gestione utente](assets/swagger-user.json)

Quindi, ottieni il nome host REST API dall’amministratore di Marketo. Vai a _Amministratore_-> _Servizi Web_ nel Marketo Engage e copia il nome host dal campo Endpoint. Il `hostname` è la stringa tra il protocollo, `https://`, e `/rest`, che si presenta come `AAA-999-AAA.mktorest.com`

Apri il file OpenAPI in un editor di testo e individua `host` nel JSON e modificane il valore nel nome host dell’API REST: `"host":"localhost:8080"` a `"host":"AAA-999-AAA.mktorest.com"`.

Una volta salvato, puoi eseguire il file di definizione nell’istanza dell’interfaccia utente Swagger o in qualsiasi altro software OpenAPI.
