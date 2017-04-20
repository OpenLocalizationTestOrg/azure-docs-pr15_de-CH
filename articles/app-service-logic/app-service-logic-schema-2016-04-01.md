<properties 
    pageTitle="Neue Schemaversion 2016-06-01 | Microsoft Azure" 
    description="Schreiben Sie die JSON-Definition für die neueste Version von Logik-apps" 
    authors="jeffhollan" 
    manager="dwrede" 
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
    
# <a name="new-schema-version-2016-06-01"></a>Neue Schemaversion 2016-06-01

Das neue Schema und API-Version für Logik-apps hat eine Anzahl Verbesserung der Zuverlässigkeit und Einfachheit der Verwendung von Logik-apps. Es gibt 3 wichtige Unterschiede:

1. Hinzufügen der Bereiche handeln, die eine Auflistung von Aktionen enthalten.
1. Schleifen sind erstklassige Aktionen
1. Ausführung der Bestellung ausführlicher über `runAfter` -Eigenschaft (welche ersetzt `dependsOn`)

Informationen zum Aktualisieren von Logik-apps aus dem Schema 2015-08-01-Vorschau Schema 2016-06-01 [Upgrade Abschnitt Auschecken.](#upgrading-to-2016-06-01-schema)


## <a name="1-scopes"></a>1. Bereiche

Eines der größten Änderungen in diesem Schema ist die Ergänzung Bereiche und Aktionen ineinander schachteln.  Dies ist hilfreich, wenn eine Reihe von Aktionen zusammen gruppieren oder müssen Aktionen ineinander schachteln (z. B. kann eine Bedingung eine weitere Bedingung enthalten).  Weitere Informationen zur Syntax Bereich kann finden [hier](app-service-logic-loops-and-scopes.md)jedoch einen einfachen Bereich Beispiel finden Sie unter:


```
{
    "actions": {
        "My_Scope": {
            "type": "scope",
            "actions": {                
                "Http": {
                    "inputs": {
                        "method": "GET",
                        "uri": "http://www.bing.com"
                    },
                    "runAfter": {},
                    "type": "Http"
                }
            }
        }
    }
}
```

## <a name="2-conditions-and-loops-changes"></a>2. Vorschriften und Schleifen ändern

In früheren Versionen des Schemas wurden Konditionen und Schleifen Parameter einer Aktion.  Diese Einschränkung wurde in diesem Schema aufgehoben und jetzt Konditionen und Schleifen als den Typ einer Aktion.  Weitere Informationen finden Sie [in diesem Artikel](app-service-logic-loops-and-scopes.md)und ein einfaches Beispiel für eine bedingungsaktion unten:

```
{
    "If_trigger_is_foo": {
        "type": "If",
        "expression": "@equals(triggerBody(), 'foo')",
        "runAfter": { },
        "actions": {
            "Http_2": {
                "inputs": {
                    "method": "GET",
                    "uri": "http://www.bing.com"
                },
                "runAfter": {},
                "type": "Http"
            }
        },
        "else": 
        {
            "if_trigger_is_bar": "..."
        }      
    }
}
```

## <a name="3-runafter-property"></a>3. RunAfter-Eigenschaft

Die neue `runAfter` -Eigenschaft ersetzt `dependsOn` zu mehr Präzision bei ausführen können.  `dependsOn`war gleichbedeutend mit "Aktion ausgeführt und war erfolgreich," jedoch oft müssen eine Aktion ausführen, wenn die vorherige Aktion erfolgreich ist, Fehler oder übersprungen.  `runAfter`Diese Flexibilität ermöglicht.  Es ist ein Objekt, das alle Aktionsnamen es läuft gibt nach und definiert ein Array von Status, die vom Trigger zulässig sind.  Für Beispiel, wenn Sie nach Schritt A wurde und B ausführen erfolgreich oder Fehler, würden Sie Folgendes erstellen `runAfter` Eigenschaft:

```
{
    "...",
    "runAfter": {
        "A": ["Succeeded"],
        "B": ["Succeeded", "Failed"]
    }
}
```

## <a name="upgrading-to-2016-06-01-schema"></a>2016-06-01-Schema aktualisieren

Aktualisierung auf das neue Schema 2016-06-01 dauert nur wenige Schritte.  Die Unterschiede zwischen dem Schema [in diesem Artikel](app-service-logic-schema-2016-04-01.md)finden.  Der Aktualisierungsprozess enthält Ausführen des Aktualisierungsskripts speichern als neue Logik app und alten Logik Anwendung ggf. überschreiben.

1. Öffnen Sie Ihre aktuelle Logik app.
1. Klicken Sie auf der Symbolleiste auf die Schaltfläche **Schema aktualisieren**
   
    ![][1]
   
    Aktualisierte Definition zurückgegeben werden.  Sie können kopieren und fügen diese Ressourcendefinition benötigen Sie jedoch **empfehlen** Sie mithilfe der Schaltfläche **Speichern** alle sichergestellt Verweise sind in der aktualisierten Logik app zulässig.
1. Klicken Sie auf Schaltfläche **Speichern** auf der Upgrade-Blade.
1. Füllen Sie den Namen und Logik app-Status und klicken Sie auf **Erstellen** , um Ihre app Upgrade Logik bereitstellen.
1. Überprüfen Sie, ob Ihre app aktualisierten Logik wie erwartet funktioniert.

    >[AZURE.NOTE] Bei einem Trigger manuell oder Anforderung wird Callback-URL im neuen Logik Anwendung geändert haben.  Verwenden Sie die neue URL überprüfen End-to-End funktioniert und Sie können über die vorhandene Logik app vorherigen URLs zu klonen.

1. *Optionale* Die Schaltfläche **Klon** (neben dem Symbol **Aktualisierungsschema** in der obigen Abbildung) auf vorherige Logik-app mit neuen Schemaversion überschreiben.  Dies ist nur erforderlich, wenn die gleichen Ressourcen-ID oder Trigger-URL der Anwendung Logik anfordern möchten.

### <a name="upgrade-tool-notes"></a>Aktualisierungstool Notizen

#### <a name="condition-mapping"></a>Bedingung-Zuordnung

Das Tool machen bemüht, true und false Zweigaktionen in einen Bereich in der aktualisierten Definition gruppieren.  Speziell die Designer Muster `@equals(actions('a').status, 'Skipped')` wird angezeigt als ein `else` Aktion.  Jedoch erkennt das Tool Muster wird nicht erkannt, möglicherweise separate Vorschriften für den True und false Verzweigung erstellt wird.  Aktionen können umgeleitet werden bei Bedarf nach einer Aktualisierung.

#### <a name="foreach-with-condition"></a>Mit ForEach
  
Das vorherige Muster eine Foreach-Schleife mit einer Bedingung pro Artikel kann im neuen Schema mit der Filteraktion repliziert werden.  Dies sollte automatisch auf Upgrade auftreten.  Die Bedingung wird eine Filteraktion, bevor die Foreach-Schleife (ein Array von Elementen, die die Bedingung erfüllen zurückgeben) und Arrays in der Foreach-Aktion übergeben.  Sie können ein Beispiel für das [in diesem Artikel](app-service-logic-loops-and-scopes.md) anzeigen

#### <a name="resource-tags"></a>Resource-tags

Resource-Tags auf Aktualisierung entfernt und müssen erneut für aktualisierte Workflow festgelegt.

## <a name="other-changes"></a>Sonstige

### <a name="manual-trigger-renamed-to-request-trigger"></a>Manuelle Auslösung Anforderung Trigger umbenannt

Der Typ `manual` veraltet und umbenannt wurde, um `request` mit der `http`.  Ist mit der Art des Musters, der Trigger verwendet wird

### <a name="new-filter-action"></a>Neue 'Filter'-Aktion

Wenn Sie ein großes Array und Filtern auf eine kleinere Gruppe von Elementen arbeiten, können Sie den neuen Typ 'Filter'.  Sie wird akzeptiert ein Array und eine Bedingung und für jedes Element die Bedingung auszuwerten und ein Array von Elementen, die die Bedingung erfüllen.

### <a name="foreach-and-until-action-restrictions"></a>ForEach bis Aktion einschränken

Foreach und until-Schleife beschränken sich auf eine einzelne Aktion.

### <a name="trackedproperties-on-actions"></a>TrackedProperties Aktionen

Aktionen können nun eine zusätzliche Eigenschaft (Geschwister, `runAfter` und `type`) namens `trackedProperties`.  Es ist ein Objekt, das angibt, bestimmte Aktion Eingaben oder Ausgaben in der Azure-Diagnose Telemetrie berücksichtigt werden, die als Teil eines Workflows ausgegeben wird.  Zum Beispiel:

```
{                
    "Http": {
        "inputs": {
            "method": "GET",
            "uri": "http://www.bing.com"
        },
        "runAfter": {},
        "type": "Http",
        "trackedProperties": {
            "responseCode": "@action().outputs.statusCode",
            "uri": "@action().inputs.uri"
        }
    }
}
```

## <a name="next-steps"></a>Nächste Schritte
- [Die Workflowdefinition Logik app verwenden](app-service-logic-author-definitions.md)
- [Erstellen Sie eine Logik app Bereitstellungsvorlage](app-service-logic-create-deploy-template.md)


<!-- Image references -->
[1]: ./media/app-service-logic-schema-2016-04-01/upgradeButton.png
