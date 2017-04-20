<properties 
    pageTitle="Autor Logik App Definitionen | Microsoft Azure" 
    description="Schreiben Sie die JSON-Definition für Logik-apps" 
    authors="jeffhollan" 
    manager="erikre" 
    editor="" 
    services="logic-apps" 
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/25/2016"
    ms.author="jehollan"/>
    
# <a name="author-logic-app-definitions"></a>Autor Logik App Definitionen
In diesem Thema veranschaulicht, wie [Azure Logik Apps](app-service-logic-what-are-logic-apps.md) Definitionen einer einfachen, deklarativen JSON Sprache. Wenn Sie noch nicht getan, zuerst [eine neue Logik-app](app-service-logic-create-a-logic-app.md) erstellen. Sie können auch [vollständige Referenzmaterial Definitionssprache auf MSDN](http://aka.ms/logicappsdocs)lesen.

## <a name="several-steps-that-repeat-over-a-list"></a>Eine Liste wiederholen Sie Schritte

Sie können den [Foreach-Typ](app-service-logic-loops-and-scopes.md) um ein Array von bis zu 10 k Elemente wiederholen und eine Aktion für jedes nutzen.

## <a name="a-failure-handling-step-if-something-goes-wrong"></a>Eine Fehlerbehandlungsroutine Schritt wenn etwas schiefgeht

Allgemein *Wiederherstellung Schritt* schreiben möchten – Logik ausführt, wenn **und nur wenn**eine oder mehrere Anrufe nicht,. In diesem Beispiel erhalten wir Daten aus einer Vielzahl an, wenn der Aufruf fehlschlägt, möchte aber eine Stelle Nachricht damit ich den Fehler später wiederfinden können:  

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
    },
    "triggers": {
        "manual": {
            "type": "manual"
        }
    },
    "actions": {
        "readData": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            }
        },
        "postToErrorMessageQueue": {
            "type": "ApiConnection",
            "inputs": "...",
            "runAfter": {
                "readData": ["Failed"]
            }
        }
    },
    "outputs": {}
}
```

Können Sie verwenden die `runAfter` -Eigenschaft an der `postToErrorMessageQueue` führen nur nach `readData` ist **fehlgeschlagen**.  Dies könnte eine Liste der möglichen Werte so `runAfter` könnte `["Succeeded", "Failed"]`.

Schließlich da nun den Fehler behandelt haben, markieren Sie wir mehr ausführen als **fehlgeschlagen**. Wie Sie hier sehen, ist diese Ausführung **erfolgreich** , obwohl ein Schritt fehlgeschlagen, habe ich den Schritt, um Fehler zu behandeln.

## <a name="two-or-more-steps-that-execute-in-parallel"></a>Zwei (oder mehr) Schritte, die parallel ausgeführt werden.

Ausführung mehrerer Aktionen parallel zu den `runAfter` Eigenschaft zur Laufzeit identisch sein muss. 

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {},
    "triggers": {
        "manual": {
            "type": "manual"
        }
    },
    "actions": {
        "readData": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            }
        },
        "branch1": {
            "type": "Http",
            "inputs": "...",
            "runAfter": {
                "readData": ["Succeeded"]
            }
        },
        "branch2": {
            "type": "Http",
            "inputs": "...",
            "runAfter": {
                "readData": ["Succeeded"]
            }
        }
    },
    "outputs": {}
}
```

Siehe das Beispiel über beide `branch1` und `branch2` nach ausführen sollen `readData`. Daher werden sowohl diese Zweige parallel ausgeführt:

![Parallel](./media/app-service-logic-author-definitions/parallel.png)

Sie sehen, dass der Zeitstempel für beide identisch ist. 

## <a name="join-two-parallel-branches"></a>Verknüpfen von zwei parallelen Teilstrukturen

Zwei Aktionen zur parallelen Ausführung durch Hinzufügen von Elementen festgelegt wurden Verknüpfen der `runAfter` -Eigenschaft ähnlich wie oben.

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-04-01-preview/workflowdefinition.json#",
    "actions": {
        "readData": {
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            },
            "runAfter": {},
            "type": "Http"
        },
        "branch1": {
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            },
            "runAfter": {
                "readData": [
                    "Succeeded"
                ]
            },
            "type": "Http"
        },
        "branch2": {
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            },
            "runAfter": {
                "readData": [
                    "Succeeded"
                ]
            },
            "type": "Http"
        },
        "join": {
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            },
            "runAfter": {
                "branch1": [
                    "Succeeded"
                ],
                "branch2": [
                    "Succeeded"
                ]
            },
            "type": "Http"
        }
    },
    "contentVersion": "1.0.0.0",
    "outputs": {},
    "parameters": {},
    "triggers": {
        "manual": {
            "inputs": {
                "schema": {}
            },
            "kind": "Http",
            "type": "Request"
        }
    }
}
```

![Parallel](./media/app-service-logic-author-definitions/join.png)

## <a name="mapping-items-in-a-list-to-some-different-configuration"></a>Einige andere Konfiguration zuordnen Elemente in einer Liste

Weiter angenommen, wir ganz unterschiedliche Inhalte abhängig von einem Wert einer Eigenschaft möchten. Wir erstellen eine Zuordnung der Werte, Ziele als Parameter:  

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "specialCategories": {
            "defaultValue": ["science", "google", "microsoft", "robots", "NSA"],
            "type": "Array"
        },
        "destinationMap": {
            "defaultValue": {
                "science": "http://www.nasa.gov",
                "microsoft": "https://www.microsoft.com/en-us/default.aspx",
                "google": "https://www.google.com",
                "robots": "https://en.wikipedia.org/wiki/Robot",
                "NSA": "https://www.nsa.gov/"
            },
            "type": "Object"
        }
    },
    "triggers": {
        "manual": {
            "type": "manual"
        }
    },
    "actions": {
        "getArticles": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "https://ajax.googleapis.com/ajax/services/feed/load?v=1.0&q=http://feeds.wired.com/wired/index"
            },
            "conditions": []
        },
        "getSpecialPage": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "@parameters('destinationMap')[first(intersection(item().categories, parameters('specialCategories')))]"
            },
            "conditions": [{
                "expression": "@greater(length(intersection(item().categories, parameters('specialCategories'))), 0)"
            }],
            "forEach": "@body('getArticles').responseData.feed.entries"
        }
    }
}
```

In diesem Fall erhalten wir zunächst eine Liste mit Artikeln und dann im zweite Schritt sucht in einer Zuordnung basierend auf der Kategorie, die als Parameter der Inhalte von URL definiert wurde. 

Zwei Elemente hier beachten: der [`intersection()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#intersection) Funktion überprüfen, ob die Kategorie eines bekannten Kategorien entspricht. Zweitens angekommen Kategorie, wir können ziehen das Element der Zuordnung mit Klammern: `parameters[...]`. 

## <a name="working-with-strings"></a>Arbeiten mit Zeichenfolgen

Es gibt verschiedene Funktionen Zeichenfolge. Nehmen wir ein Beispiel, wo wir eine Zeichenfolge, die an ein System übertragen werden soll, aber werden nicht Zeichen ordnungsgemäß behandelt wird. Können base64 codiert diese Zeichenfolge. Allerdings zu Escapezeichen in einem URL gehen wir einige Zeichen ersetzt. 

Möchten wir auch ein Teil der des Auftrags benennen, da die ersten 5 Zeichen nicht verwendet werden.

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "order": {
            "defaultValue": {
                "quantity": 10,
                "id": "myorder1",
                "orderer": "NAME=Stèphén__Šīçiłianö"
            },
            "type": "Object"
        }
    },
    "triggers": {
        "manual": {
            "type": "manual"
        }
    },
    "actions": {
        "order": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "http://www.example.com/?id=@{replace(replace(base64(substring(parameters('order').orderer,5,sub(length(parameters('order').orderer), 5) )),'+','-') ,'/' ,'_' )}"
            }
        }
    },
    "outputs": {}
}
```

Arbeiten von innen heraus:

1. Abrufen der [`length()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#length) Name des Auftraggebers zurückgegeben wieder die Gesamtanzahl der Zeichen

2. Subtrahieren Sie 5 (da wir eine kürzere Zeichenfolge möchten)

3. Tatsächlich die [`substring()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#substring) . Wir beginnen mit Index `5` und den Rest der Zeichenfolge.

4. Diese Teilzeichenfolge Konvertieren einer [`base64()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#base64) Zeichenfolge

5. [`replace()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#replace)alle von der `+` mit Zeichen`-`

6. [`replace()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#replace)alle von der `/` mit Zeichen`_`

## <a name="working-with-date-times"></a>Arbeiten mit Zeitangaben

Zeitangaben können nützlich sein, insbesondere beim um Daten aus einer Datenquelle, die natürlich **Trigger**nicht unterstützt.  Zeitangaben können Sie ermitteln, wie lange die verschiedenen Schritte. 

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "order": {
            "defaultValue": {
                "quantity": 10,
                "id": "myorder1"
            },
            "type": "Object"
        }
    },
    "triggers": {
        "manual": {
            "type": "manual"
        }
    },
    "actions": {
        "order": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "http://www.example.com/?id=@{parameters('order').id}"
            }
        },
        "timingWarning": {
            "actions" {
                "type": "Http",
                "inputs": {
                    "method": "GET",
                    "uri": "http://www.example.com/?recordLongOrderTime=@{parameters('order').id}&currentTime=@{utcNow('r')}"
                },
                "runAfter": {}
            }
            "expression": "@less(actions('order').startTime,addseconds(utcNow(),-1))"
        }
    },
    "outputs": {}
}
```

In diesem Beispiel werden extrahiert die `startTime` des vorherigen Schrittes. Und wir sind immer aktuelle subtrahieren einer Sekunde:[`addseconds(..., -1)`](https://msdn.microsoft.com/library/azure/mt643789.aspx#addseconds) (Sie können andere Zeiteinheiten wie `minutes` oder `hours`). Schließlich können wir diese zwei Werte vergleichen. Ob die erste kleiner als die zweite, dann bedeutet dies, dass mehr als eine Sekunde vergangen ist, seit die Reihenfolge platziert. 

Außerdem Beachten Sie, dass wir Zeichenfolge Formatierungsprogramme Datum formatieren: in der Abfragezeichenfolge verwende [`utcnow('r')`](https://msdn.microsoft.com/library/azure/mt643789.aspx#utcnow) der RFC1123 zu. Alle Datumsangaben Formatierung [auf MSDN dokumentiert](https://msdn.microsoft.com/library/azure/mt643789.aspx#utcnow). 

## <a name="using-deployment-time-parameters-for-different-environments"></a>Deployment-Parameter verwenden für verschiedene umge bungen

Es ist üblich, einen Lebenszyklus Bereitstellung haben Sie eine, eine und Produktion. In all diesen möglicherweise soll die gleiche Definition jedoch z. B. verschiedene Datenbanken verwenden. Ebenso sollten Sie die gleiche Definition für hohe Verfügbarkeit in vielen verschiedenen Regionen verwenden, aber jeweils Anwendung Logik des Bereichs Datenbank mit. 

Ist dies nicht mit anderen Parametern zur *Laufzeit*für sollten die `trigger()` wie oben genannt. 

Sie können eine sehr einfache Definition folgendermaßen starten:

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "uri": {
            "type": "string"
        }
    },
    "triggers": {
        "manual": {
            "type": "manual"
        }
    },
    "actions": {
        "readData": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "@parameters('uri')"
            }
        }
    },
    "outputs": {}
}
```

Klicken Sie dann in der `PUT` Anforderung für die Anwendung Logik können Sie den Parameter `uri`. Beachten Sie, wie mehr Standardwert dieser Parameter in der app-Nutzlast Logik erforderlich ist:

```
{
    "properties": {},
        "definition": {
          // Use the definition from above here
        },
        "parameters": {
            "connection": {
                "value": "https://my.connection.that.is.per.enviornment"
            }
        }
    },
    "location": "westus"
}
``` 

In jeder Umgebung dann können Sie einen anderen Wert für die `connection` Parameter. 

Finden Sie die [REST-API-Dokumentation](https://msdn.microsoft.com/library/azure/mt643787.aspx) für alle Optionen zum Erstellen und Verwalten von Logik-apps. 
