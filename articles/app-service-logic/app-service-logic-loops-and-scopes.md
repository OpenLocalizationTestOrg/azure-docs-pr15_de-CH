<properties
   pageTitle="Logik-Apps Schleifen, Bereiche und vorgelagerte | Microsoft Azure"
   description="Logik App Schleife, Umfang und vorgelagerte Konzepte"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="dwrede"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="05/14/2016"
   ms.author="jehollan"/>
   
# <a name="logic-apps-loops-scopes-and-debatching"></a>Logik-Apps Schleifen, Bereiche und vorgelagerte
  
>[AZURE.NOTE] Diese Version des Artikels gilt für Logik Apps 2016-04-01-Vorschau Schema und höher.  Konzepte für ältere Schemas sind jedoch Bereiche sind nur verfügbar für dieses Schema.
  
## <a name="foreach-loop-and-arrays"></a>ForEach-Schleife und Arrays
  
Logik Apps kann Daten durchlaufen und für jedes Element eine Aktion ausführen.  Dies erfolgt über die `foreach` Aktion.  Im Designer können zum Hinzufügen einer for each-Schleife.  Nachdem Sie auf das Array durchlaufen möchten, können Sie beginnen, Hinzufügen von Aktionen.  Derzeit sind nur eine Aktion pro Foreach-Schleife auf, aber in den kommenden Wochen wird diese Einschränkung aufgehoben.  Innerhalb der Schleife können Sie sobald an, was auf jeden Wert des Arrays erfolgen soll.

Wenn Code anzeigen, können Sie eine for each-Schleife wie unten.  Dies ist ein Beispiel für eine for each-Schleife, die eine e-Mail für jede e-Mail-Adresse, die gesendet "microsoft.com" enthält:

```
{
    "email_filter": {
        "type": "query",
        "inputs": {
            "from": "@triggerBody()['emails']",
            "where": "@contains(item()['email'], 'microsoft.com')
        }
    },
    "forEach_email": {
        "type": "foreach",
        "foreach": "@body('email_filter')",
        "actions": {
            "send_email": {
                "type": "ApiConnection",
                "inputs": {
                "body": {
                    "to": "@item()",
                    "from": "me@contoso.com",
                    "message": "Hello, thank you for ordering"
                }
                "host": {
                    "connection": {
                        "id": "@parameters('$connections')['office365']['connection']['id']"
                    }
                }
                }
            }
        },
        "runAfter":{
            "email_filter": [ "Succeeded" ]
        }
    }
}
```
  
  Ein `foreach` Aktion über arrays bis zu 5.000 Zeilen durchlaufen kann.  Jede Iteration kann es möglicherweise notwendig, Nachrichten an eine Warteschlange ggf. Flusskontrolle parallel ausführen.
  
## <a name="until-loop"></a>Until-Schleife
  
  Sie können eine Aktion oder eine Reihe von Aktionen ausführen, bis eine Bedingung erfüllt ist.  Das häufigste Szenario für dieses Ruft einen Endpunkt, bis die Antwort erhalten, die Sie suchen.  Im Designer können zum Hinzufügen einer until-Schleife.  Nach dem Hinzufügen von Aktionen in der Schleife, Festlegen der Bedingung sowie die Schleife Grenzen.  Gibt es eine 1 Minute Verzögerung zwischen Schleife.
  
  Wenn Code anzeigen, Sie können eine bis Schleife wie unten.  Dies ist ein Beispiel für einen HTTP-Endpunkt aufrufen, bis der Antworttext den Wert 'Abgeschlossen' hat.  Wird abgeschlossen, wenn entweder 
  
  * HTTP-Antwort hat Status 'Abgeschlossen'
  * Es wurde 1 Stunde versucht.
  * Es ist 100 Mal durchlaufen.
  
  ```
  {
      "until_successful":{
        "type": "until",
        "expression": "@equals(actions('http')['status'], 'Completed'),
        "limit": {
            "count": 100,
            "timeout": "PT1H"
        },
        "actions": {
            "create_resource": {
                "type": "http",
                "inputs": {
                    "url": "http://provisionRseource.com",
                    "body": {
                        "resourceId": "@triggerBody()"
                    }
                }
            }
        }
      }
  }
  ```
  
## <a name="spliton-and-debatching"></a>SplitOn und vorgelagerte

Manchmal erhalten ein Trigger ein Array von Elementen, die zu debatch pro Artikel Workflow starten.  Dies kann über die `spliton` Befehl.  Wenn Ihr Trigger stolz Nutzlast ein Array ist standardmäßig ein `spliton` wird hinzugefügt, und starten Sie einen pro Artikel.  SplitOn kann nur ein Trigger hinzugefügt werden.  Dies kann manuell konfiguriert oder in der Codeansicht Definition überschrieben.  Derzeit SplitOn debatch kann bis zu 5.000 Elemente arrays.  Keine `spliton` und auch das Muster Syncronous Antwort.  Jeder Workflow aufgerufen hat einen `response` Aktion an `spliton` Asyncronously ausgeführt wird und sofort senden `202 Accepted` Antwort.  

SplitOn kann in der Code-Ansicht wie im folgenden Beispiel angegeben werden.  Diese empfängt ein Array von Elementen und debatches für jede Zeile.

```
{
    "myDebatchTrigger": {
        "type": "Http",
        "inputs": {
            "url": "http://getNewCustomers",
        },
        "recurrence": {
            "frequency": "Second",
            "interval": 15
        },
        "spliton": "@triggerBody()['rows']"
    }
}
```

## <a name="scopes"></a>Bereiche

Es ist möglich, eine Reihe von Aktionen mit einem Bereich Gruppieren.  Dies ist besonders hilfreich für die Implementierung Ausnahmebehandlung.  Im Designer können Sie einen neuen Bereich hinzufügen und hinzufügen Aktionen verreisen.  Sie können Bereiche in der Codeansicht wie folgt definieren:


```
{
    "myScope": {
        "type": "scope",
        "actions": {
            "call_bing": {
                "type": "http",
                "inputs": {
                    "url": "http://www.bing.com"
                }
            }
        }
    }
}
```
