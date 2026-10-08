---
title: URL di base
feature: REST API
description: Scopri come generare richieste API REST di Marketo, comprendere la risorsa e i parametri del percorso dell’URL di base e trovare l’URL di base univoco.
exl-id: 6c3f122c-3ace-4ed3-bed0-a6b89cedc99a
TQID: 'https://experienceleague.adobe.com/NZisV6V-FMPi0RHpdaFrc1kZc3nb15YomwRgohaQmEE'
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
    internal-label: Marketo Engage
feature_v2:
  - id: dca84292-69e9-4116-a575-667d31fa060d
    internal-label: APIs
subfeature_v2:
  - id: cf1396d8-ab85-4e93-b35d-d9b573024abf
    internal-label: REST APIs
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
    internal-label: Admin
source-git-commit: 5620f050ba834be3f6648650b5cc7d781ea394bf
workflow-type: tm+mt
source-wordcount: '142'
ht-degree: 3%
---
# URL di base

Ogni chiamata API in [Riferimento endpoint](endpoint-reference.md) specifica il metodo, il percorso, la risorsa e i parametri REST. Aggiungi questi componenti all’URL di base per creare una richiesta.

Di seguito è riportato un esempio di URL REST ben formato:

`https://284-RPR-133.mktorest.com/rest/v1/lead/318581.json?fields=email,firstName,lastName`

L’esempio contiene i seguenti componenti:

- **URL di base:** `https://284-RPR-133.mktorest.com/rest`
- **Percorso:** `/v1/lead/`
- **Risorsa:** `318582.json`
- **Parametro query:** `fields=email,firstName,lastName`

L’URL di base contiene l’ID account, noto anche come Munchkin ID, ed è univoco per ogni abbonamento a Marketo.

Per trovare l&#39;URL di base, accedere a Marketo e passare a **[!UICONTROL Admin]** > **[!UICONTROL Integration]** > **[!UICONTROL Web Services]**. L’URL di base è etichettato come &quot;Endpoint:&quot; nella sezione &quot;REST API&quot;, come mostrato nell’immagine seguente.

![Endpoint URL di base servizi Web](assets/rest-api-base-url-web-services.png)

Copia l’URL di base e includilo nell’URL per ogni chiamata API REST.
