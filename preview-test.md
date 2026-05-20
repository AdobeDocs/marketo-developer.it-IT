---
title: Prova anteprima EXL
description: Esempi di sintassi markdown Adobe EXL per testare l’anteprima dell’estensione.
source-git-commit: 87d2584ed0ef2c1fa219f2a3ad120c91dc5491e0
workflow-type: tm+mt
source-wordcount: '348'
ht-degree: 11%

---


# Prova anteprima EXL

## Blocchi di avvisi

>[!NOTE]
>
>Questa è una nota. Utilizzare le note per ulteriori informazioni di cui il lettore deve essere a conoscenza.

>[!TIP]
>
>Questo è un suggerimento. Utilizza i suggerimenti per ottenere informazioni facoltative ma utili.

>[!IMPORTANT]
>
>Questo è un avviso importante. Utilizza per informazioni che il lettore non deve ignorare.

>[!WARNING]
>
>Questo è un avviso. Utilizzare per informazioni su potenziali problemi.

>[!CAUTION]
>
>Questa è un&#39;avvertenza. Utilizzare per informazioni sui rischi potenziali.

>[!ADMIN]
>
>Questo è un avviso dell’amministratore. Per contenuti solo amministratori.

>[!AVAILABILITY]
>
>Questa è una nota sulla disponibilità. Per informazioni dettagliate sulla disponibilità delle funzioni.

>[!PREREQUISITES]
>
>Questo è un blocco di prerequisiti. Elencare ciò di cui il lettore ha bisogno prima di iniziare.

## Scatole ombreggiate

>[!BEGINSHADEBOX &quot;Titolo facoltativo&quot;]

Questo contenuto viene visualizzato con uno sfondo grigio. Utilizza le caselle ombra per raggruppare visivamente il contenuto correlato.

Puoi includere gli elenchi:

- Elemento uno
- Elemento due
- Elemento tre

>[!ENDSHADEBOX]

>[!BEGINSHADEBOX]

Casella ombreggiata senza titolo.

>[!ENDSHADEBOX]

## Sezioni comprimibili

+++Fai clic per espandere — esempio di base

Questo contenuto è nascosto finché l’utente non seleziona il titolo.

Puoi includere qui qualsiasi contenuto, compresi i blocchi di codice:

```javascript
const example = 'hello world';
console.log(example);
```

+++

+++Configurazione avanzata

Utilizza sezioni comprimibili per contenuti opzionali o avanzati che altrimenti ingombrerebbero il flusso principale.

| Impostazione | Valore | Descrizione |
| --- | --- | --- |
| timeout | 30 | Secondi prima del timeout della richiesta |
| nuovi tentativi | 3 | Numero di tentativi |

{style="table-layout:auto"}

+++

## Aiuto contestuale

L’anteprima dell’Aiuto contestuale è nascosta. Vedi!
>[!CONTEXTUALHELP]
>id="models_insights_undefinedchannels"
>title="Canali non definiti"
>abstract="I canali non definiti sono inclusi, ma non viene loro attribuita alcuna conversione."

## Video incorporato

>[!VIDEO](https://video.tv.adobe.com/v/3427028/?quality=12&learn=on)

## Macro di localizzazione

Utilizza [!DNL Marketo] per racchiudere i nomi dei prodotti in modo che non siano localizzati.

Utilizza **[!UICONTROL Admin]** > **[!UICONTROL LaunchPoint]** per le etichette degli elementi dell&#39;interfaccia utente.

Esempio combinato: In [!DNL Adobe Analytics], selezionare **[!UICONTROL Workspace]** > **[!UICONTROL Create project]**.

## Distintivi

[!BADGE Beta]{type=Informative}

[!BADGE Disponibile in genere]{type=Positive}

[!BADGE Obsoleto]{type=Negative}

[!BADGE Sperimentale]{type=Caution}

## Schede

>[!BEGINTABS]

>[!TAB Requisiti]

>[!IMPORTANT]
>
>Per completare questa attività è necessario disporre delle autorizzazioni di amministratore.

Campi obbligatori:

| Campo | Tipo | Obbligatorio |
| --- | --- | --- |
| Nome | Stringa | Sì |
| E-mail | Stringa | Sì |
| Ruolo | Enumerazione | No |

>[!TAB Passaggi]

1. Apri Admin Console.
1. Selezionare **[!UICONTROL Users]** > **[!UICONTROL Add user]**.
1. Compila i campi obbligatori.
1. Seleziona **[!UICONTROL Save]**.

>[!NOTE]
>
>Le modifiche hanno effetto immediato.

>[!TAB Risultato]

Il nuovo utente riceve un’e-mail di benvenuto con un collegamento per impostare la password.

- Il collegamento scade dopo 24 ore.
- Gli utenti possono richiedere un nuovo collegamento dalla pagina di accesso.

>[!ENDTABS]

## Blocchi di codice

```json
{
  "name": "example",
  "version": "1.0.0",
  "enabled": true
}
```

```javascript
function greet(name) {
  return `Hello, ${name}!`;
}
```

## Tabelle

| Colonna uno | Colonna due | Colonna tre |
| --- | --- | --- |
| [!UICONTROL Row 1], cella 1 | Riga 1, cella 2 | [!DNL Row 1, cell 3] |
| Riga 2, cella 1 | Riga 2, cella 2 | Riga 2, cella 3 |

