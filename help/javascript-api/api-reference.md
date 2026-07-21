---
title: Riferimento API di Munchkin
description: Utilizza l’API JavaScript di Munchkin per tenere traccia di visite di pagina, clic di collegamento ed eventi personalizzati con i metodi init, createTrackingCookie e munchkinFunction.
feature: Munchkin Tracking Code, Javascript
exl-id: e9727691-5501-4223-bc98-2b4bacc33513
TQID: https://experienceleague.adobe.com/s97x6wVZijnnxZwS7HMIkQAKlxXkcfPXuSZG4KjXGoc
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: b3b8a63f-51fc-40f6-a7d2-a31c5d49fb45
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 414
ht-degree: 7%

---

# Riferimento API di Munchkin

Munchkin fornisce funzioni JavaScript per il tracciamento personalizzato degli eventi del browser. Ad esempio, puoi tenere traccia delle riproduzioni video o dei clic su elementi che non sono collegamenti.

## Funzioni

L’API Munchkin include le seguenti funzioni:

- `init`
- `createTrackingCookie`
- `munchkinFunction`

<a name="munchkin_init"></a>

### Munchkin.init()

`Munchkin.init()` deve essere chiamato prima di qualsiasi altra funzione. Imposta Munchkin sulla pagina corrente per inviare attività a un’istanza specifica e genera un’attività &quot;Pagina web visite&quot; per la pagina corrente.

| Nome parametro | Facoltativo/Obbligatorio | Tipo | Descrizione |
| --- | --- | --- | --- |
| ID Munchkin | Obbligatorio | Stringa | L’ID account Munchkin si trova nel menu Amministrazione > Integrazione > Munchkin. Imposta l&#39;istanza di destinazione a cui inviare le attività. |
| [Impostazioni di configurazione](configuration.md) | Facoltativo | Oggetto | Abilita impostazioni di comportamento alternative per Munchkin. |

```javascript
Munchkin.init('299-BYM-827');
```

### Munchkin.createTrackingCookie()

`Munchkin.createTrackingCookie()` controlla se nel browser esiste un cookie `_mkto_trk`. Se il cookie non esiste, la funzione ne crea uno.

Quando `cookieAnon` è impostato su false, utilizzare questa funzione per tenere traccia degli utenti durante azioni specifiche, ad esempio la registrazione o il download di una risorsa.

| Nome parametro | Facoltativo/Obbligatorio | Tipo | Descrizione |
| --- | --- | --- | --- |
| forceCreate | Obbligatorio | Booleano | Crea cookie anche se `cookieAnon` è impostato su false. |

```javascript
Munchkin.createTrackingCookie(true);
```

### Munchkin.munchkinFunction()

Utilizza `Munchkin.munchkinFunction()` per creare comportamenti di tracciamento personalizzati. Ad esempio, tieni traccia dell’attività del lettore video o delle visite alle pagine da una navigazione non standard, come le modifiche hash.

| Nome parametro | Facoltativo/Obbligatorio | Tipo | Descrizione |
| --- | --- | --- | --- |
| Tipo di funzione | Obbligatorio | Stringa | Determina l’attività da registrare. Valori consentiti: `visitWebPage`, `clickLink`, `associateLead` |
| Dati | Obbligatorio | Oggetto | Contiene i dati per l’attività da registrare. |

#### visitWebPage

La chiamata a `munchkinFunction()` con `visitWebPage` invia un&#39;attività &#39;visita&#39; per l&#39;utente corrente a Marketo. Utilizzare l&#39;oggetto dati nel secondo argomento per personalizzare l&#39;URL e `querystring`.

| Nome proprietà dati | Facoltativo/Obbligatorio | Tipo | Descrizione |
| --- | --- | --- | --- |
| url | Obbligatorio | Stringa | Percorso del file URL utilizzato per registrare una visita di pagina.  Questo valore viene aggiunto al nome di dominio corrente per creare il nome di pagina completo. Ad esempio, se l&#39;URL è `/index.html` e il nome di dominio è `www.example.com`, la pagina visitata viene registrata come `www.example.com/index.html`. |
| params | Facoltativo | Stringa | Stringa di query dei parametri desiderati da registrare. |

Ad esempio, `foo=bar&biz=baz`.

```javascript
Munchkin.munchkinFunction('visitWebPage', {
        'url': '/Football/Team/Seahawks',
        'params': 'defense=legion_of_boom&qb=wilson'
    }
);
```

#### clickLink

La chiamata a `munchkinFunction()` con `clickLink` invia un&#39;attività di clic per l&#39;utente corrente a Marketo. Utilizzare la proprietà `href` nell&#39;oggetto dati per personalizzare l&#39;URL di clic.

| Nome proprietà dati | Facoltativo/Obbligatorio | Tipo | Descrizione |
| --- | --- | --- | --- |
| href | Obbligatorio | Stringa | Percorso del file URL utilizzato per registrare il clic su un collegamento. Questo valore viene aggiunto al nome di dominio corrente per creare un collegamento completo. |

Ad esempio, se href è `/index.html` e il nome di dominio è `www.example.com`, il clic sul collegamento verrà registrato come `www.example.com/index.html`.

```javascript
Munchkin.munchkinFunction('clickLink', {
        'href': '/Football/Team/Seahawks'
    }
);
```

#### associateLead

Questo metodo è obsoleto e non è più disponibile.
