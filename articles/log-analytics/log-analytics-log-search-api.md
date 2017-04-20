<properties
    pageTitle="Protokollanalyse anmelden REST API suchen | Microsoft Azure"
    description="Dieses Handbuch enthält eine Beschreibung wie können die Protokollanalyse Suche REST-API in Operations Management Suite (OMS) und bietet Beispiele demonstrieren, wie die Befehle Lernprogramm."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>


# <a name="log-analytics-log-search-rest-api"></a>Protokollanalyse anmelden Suche REST-API

Dieses Handbuch enthält eine Beschreibung wie können Log Analytics Suche REST API in Operations Management Suite (OMS) und bietet Beispiele demonstrieren, wie die Befehle Lernprogramm. Die Beispiele in diesem Artikel verweisen betriebliche Informationen ist der Name der vorherigen Version der Protokollanalyse.

## <a name="overview-of-the-log-search-rest-api"></a>Übersicht über Protokoll suchen REST-API

Log Analytics Suche REST API ist RESTful und API Ressourcenmanager Azure möglich. In diesem Dokument finden Sie Beispiele, die API durch die [ARMClient](https://github.com/projectkudu/ARMClient), eine open-Source-Befehlszeilentool erfolgt, die Vereinfachung der Azure-Ressourcen-Manager-API aufrufen. Die Verwendung von ARMClient und PowerShell ist eine von vielen Optionen auf das Protokoll Analytics Such-API zugreifen. Eine weitere Option ist mit Azure PowerShell-Modul OperationalInsights Cmdlets auf Suche umfasst. Mit diesen Tools können Sie RESTful Azure Ressourcen-Manager-API Aufrufe OMS Arbeitsbereiche und Suche Befehle darin nutzen. Die API gibt Suche Ergebnisse im JSON-Format können Sie die Suchergebnisse programmgesteuert auf viele verschiedene Arten verwenden.

Der Azure-Ressourcen-Manager kann über eine [Bibliothek für .NET](https://msdn.microsoft.com/library/azure/dn910477.aspx) sowie eine [REST-API](https://msdn.microsoft.com/library/azure/mt163658.aspx)verwendet werden. Überprüfen der verknüpften Webseiten erfahren Sie mehr.

## <a name="basic-log-analytics-search-rest-api-tutorial"></a>Log Analytics Suche REST API-Lernprogramm

### <a name="to-use-the-arm-client"></a>Mit dem ARM

1. Installieren Sie [Chocolatey](https://chocolatey.org/), einer Open Source Paket-Manager für Windows. Öffnen Sie ein Eingabeaufforderungsfenster als Administrator, und führen Sie den folgenden Befehl:

    ```
    @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString('https://chocolatey.org/install.ps1'))" && SET PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin
    ```

2. Installieren Sie die ARMClient durch den folgenden Befehl ausführen:

    ```
    choco install armclient
    ```

### <a name="to-perform-a-simple-search-using-the-armclient"></a>Eine einfache Suche mithilfe der ARMClient

1. Melden Sie Ihr Konto Microsoft oder OrgID:

    ```
    armclient login
    ```

    Eine erfolgreiche Anmeldung Listet alle Abonnements, die mit dem angegebenen Konto verknüpft:

    ```
    PS C:\Users\SampleUserName> armclient login
    Welcome YourEmail@ORG.com (Tenant: zzzzzzzz-zzzz-zzzz-zzzz-zzzzzzzzzzzz)
    User: YourEmail@ORG.com, Tenant: zzzzzzzz-zzzz-zzzz-zzzz-zzzzzzzzzzzz (Example org)
    There are 3 subscriptions
    Subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx (Example Name 1)
    Subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx (Example Name 2)
    Subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx (Example Name 3)
    ```

2. Operations Management Suite Arbeitsbereiche abrufen:

    ```
    armclient get /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces?api-version=2015-03-20
    ```

    Ein erfolgreicher Aufruf der Get Ausgabe alle Arbeitsbereiche mit dem Abonnement verknüpft:

    ```
    {
    "value": [
    {
      "properties": {
    "source": "External",
    "customerId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "portalUrl": "https://eus.login.mms.microsoft.com/SignIn.aspx?returnUrl=https%3a%2f%2feus.mms.microsoft.com%2fMain.aspx%3fcid%xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
      },
      "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/{WORKSPACE NAME}",
      "name": "{WORKSPACE NAME}",
      "type": "Microsoft.OperationalInsights/workspaces",
      "location": "East US"
       ]
    }
    ```
3. Erstellen Sie Suchvariablen:

    ```
    $mySearch = "{ 'top':150, 'query':'Error'}";
    ```
4. Suchen Sie die neue Suche Variable:

    ```
    armclient post /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{WORKSPACE NAME}/search?api-version=2015-03-20 $mySearch
    ```

## <a name="log-analytics-search-rest-api-reference-examples"></a>Protokollieren von Analytics Suche REST API-Beispiele
Die folgenden Beispiele zeigen die Verwendung der Such-API.

### <a name="search---actionread"></a>Suche - Aktion-lesen

**Beispiel-Url:**

```
    /subscriptions/{SubscriptionId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/search?api-version=2015-03-20
```

**Anforderung:**

```
    $savedSearchParametersJson =
    {
      "top":150,
      "highlight":{
        "pre":"{[hl]}",
        "post":"{[/hl]}"
      },
      "query":"*",
      "start":"2015-02-04T21:03:29.231Z",
      "end":"2015-02-11T21:03:29.231Z"
    }
    armclient post /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/search?api-version=2015-03-20 $searchParametersJson
```
Die folgende Tabelle beschreibt die Eigenschaften, die verfügbar sind.

|**Eigenschaft**|**Beschreibung**|
|---|---|
|Nach oben|Die maximale Anzahl der zurückzugebenden Ergebnisseite.|
|Markieren|Enthält Pre- und Post-Parameter zur Regel übereinstimmenden Felder markieren|
|vor|Die angegebene Zeichenfolge in die entsprechenden Felder als Präfix voran.|
|Bereitstellen|Fügt die angegebene Zeichenfolge an die übereinstimmenden Felder.|
|Abfrage|Die Suchabfrage sammeln und Ergebnisse zurückgegeben.|
|Starten|Der Anfang des Zeitfensters Ergebnisse gesucht werden sollen.|
|Ende|Das Ende des Zeitfensters Ergebnisse gesucht werden sollen.|


**Antwort:**

```
    {
      "id" : "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "__metadata" : {
        "resultType" : "raw",
        "total" : 1455,
        "top" : 150,
        "StartTime" : "2015-02-11T21:09:07.0345815Z",
        "Status" : "Successful",
        "LastUpdated" : "2015-02-11T21:09:07.331463Z",
        "CoreResponses" : [],
        "sort" : [{
          "name" : "TimeGenerated",
          "order" : "desc"
        }],
        "requestTime" : 450
      },
      "value": [{
        "SourceSystem" : "OpsManager",
        "TimeGenerated" : "2015-02-07T14:07:33Z",
        "Source" : "SideBySide",
        "EventLog" : "Application",
        "Computer" : "BAMBAM",
        "EventCategory" : 0,
        "EventLevel" : 1,
        "EventLevelName" : "Error",
        "UserName" : "N/A",
        "EventID" : 78,
        "MG" : "00000000-0000-0000-0000-000000000001",
        "TimeCollected" : "2015-02-07T14:10:04.69Z",
        "ManagementGroupName" : "AOI-5bf9a37f-e841-462b-80d2-1d19cd97dc40",
        "id" : "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
        "Type" : "Event",
        "__metadata" : {
          "Type" : "Event",
          "TimeGenerated" : "2015-02-07T14:07:33Z",
          "highlighting" : {
          "EventLevelName" : ["{[hl]}Error{[/hl]}"]
        }
      }]
    ],
            "start" : "2015-02-04T21:03:29.231Z",
            "end" : "2015-02-11T21:03:29.231Z"
          }
        }
      }]
    }
```

### <a name="searchid---actionread"></a>Suchen / {ID} - Aktion-lesen

**Fordern Sie eine gespeicherte Suche:**

```
    armclient post /subscriptions/{SubId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/search/{SearchId}?api-version=2015-03-20
```

>[AZURE.NOTE] Wenn die Suche mit dem Status 'Ausstehend', Abrufen aktualisierter Ergebnisse über diese API möglich. Nach 6 Min. HTTP fehlend zurück und das Ergebnis der Suche aus dem Cache gelöscht. Die ursprünglichen Suchanfrage sofort den Status "Erfolgreich" zurückgegeben wird, wird es nicht im Cache verursacht diese API zurückzugebenden HTTP nicht mehr abgefragt hinzugefügt. Inhalt eine HTTP 200 Ergebnis werden in demselben Format wie die ursprünglichen Suchanfrage mit aktualisierten Werte.

### <a name="saved-searches---rest-only"></a>Gespeicherte Suchen - REST nur

**Liste der gespeicherten Suchen anfordern:**

```
    armclient post /subscriptions/{SubId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/savedSearches?api-version=2015-03-20
```

Unterstützt Methoden: GET PLATZIEREN löschen

Unterstützt Methoden: GET

Die folgende Tabelle beschreibt die Eigenschaften, die verfügbar sind.

|Eigenschaft|Beschreibung|
|---|---|
|ID|Der eindeutige Bezeichner.|
|ETag|**Patch erforderlich**. Bei jeder aktualisiert vom Server. Wert muss gleich dem aktuellen gespeicherten Wert oder ' *' aktualisiert. 409 für alte/ungültige Werte zurückgegeben.|
|Properties.Query|**Erforderlich**. Die Suchabfrage.|
|properties.displayName|**Erforderlich**. Der Benutzername des definierten Anzeigen der Abfrage. Modelliert eine Azure Ressource wäre ein Tag.|
|Properties.Category|**Erforderlich**. Die benutzerdefinierte Kategorie der Abfrage. Wenn als Azure Ressource modelliert ein Tag wäre.|

>[AZURE.NOTE] Protokollanalyse Such-API gibt zurzeit Benutzer erstellte gespeicherte Suchen, wenn für die gespeicherte Suche in einen Arbeitsbereich abgerufen. Die API wird nicht gespeicherte Suchen bereitgestellten Projektmappen zu diesem Zeitpunkt zurück. Diese Funktion wird zu einem späteren Zeitpunkt hinzugefügt werden.

### <a name="create-saved-searches"></a>Erstellen Sie gespeicherte Suchen

**Anforderung:**

```
    $savedSearchParametersJson = "{'properties': { 'Category': 'myCategory', 'DisplayName':'myDisplayName', 'Query':'* | measure Count() by Source', 'Version':'1'  }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/savedSearches/thisIsMyId?api-version=2015-03-20 $savedSearchParametersJson
```

### <a name="delete-saved-searches"></a>Löschen Sie gespeicherte Suchen

**Anforderung:**

```
    armclient delete /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/savedSearches/thisIsMyId?api-version=2015-03-20
```

### <a name="update-saved-searches"></a>Aktualisierung gespeichert Suche

 **Anforderung:**

```
    $savedSearchParametersJson = "{'etag': 'W/`"datetime\'2015-04-16T23%3A35%3A35.3182423Z\'`"', 'properties': { 'Category': 'myCategory', 'DisplayName':'myDisplayName', 'Query':'* | measure Count() by Source', 'Version':'1'  }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/savedSearches/thisIsMyId?api-version=2015-03-20 $savedSearchParametersJson
```

### <a name="metadata---json-only"></a>Metadaten - JSON nur

Hier ist eine Möglichkeit, die Felder für alle Protokolltypen für Daten im Arbeitsbereich angezeigt. Zum Beispiel ist sollen Sie, wenn der Typ ein Feld mit dem Namen Computer hat, dies eine Möglichkeit zum Suchen und überprüfen.

**Anforderung für Felder:**

```
    armclient get /subscriptions/{SubId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/schema?api-version=2015-03-20
```

**Antwort:**

```
    {
      "__metadata" : {
        "schema" : {
          "name" : "Example Name",
          "version" : 2
        },
        "resultType" : "schema",
        "requestTime" : 35
      },
      "value" : [{
          "name" : "MG",
          "displayName" : "MG",
          "type" : "Guid",
          "facetable" : true,
          "display" : false,
          "ownerType" : ["PerfHourly", "ProtectionStatus", "Capacity_SMBUtilizationByHost", "Capacity_ArrayUtilization", "Capacity_SMBShareUtilization", "SQLAssessmentRecommendation", "Event", "ConfigurationChange", "ConfigurationAlert", "ADAssessmentRecommendation", "ConfigurationObject", "ConfigurationObjectProperty"]
        }, {
          "name" : "ManagementGroupName",
          "displayName" : "ManagementGroupName",
          "type" : "String",
          "facetable" : true,
          "display" : true,
          "ownerType" : ["PerfHourly", "ProtectionStatus", "Event", "ConfigurationChange", "ConfigurationAlert", "W3CIISLog", "AlertHistory", "Recommendation", "Alert", "SecurityEvent", "UpdateAgent", "RequiredUpdate", "ConfigurationObject", "ConfigurationObjectProperty"]
        }
      ]
    }
```

Die folgende Tabelle beschreibt die Eigenschaften, die verfügbar sind.

|**Eigenschaft**|**Beschreibung**|
|---|---|
|Name|Name des Feldes.|
|displayName|Der Anzeigename des Felds.|
|Typ|Der Typ des Feldwerts.|
|facetable|Aus aktuellen "Index", "gespeichert" und "Facetten".|
|Anzeigen|Aktuelle Eigenschaft 'anzeigen'. True, wenn in der Suche ist.|
|Besitzertyp|Nur Typen, die von diesem IP reduziert.|


## <a name="optional-parameters"></a>Optionale Parameter
Die folgende Informationen beschreibt optionale Parameter.

### <a name="highlighting"></a>Hervorheben

Parameter "Hervorheben" ist ein optionaler Parameter, Teilsystems suchen anfordern können, eine Reihe von Marken in seiner Antwort.

Diese Marken bedeuten den Beginn und Ende hervorgehobenen Text, Fristen, die in Ihrer Suchabfrage entspricht.
Sie können Start- und Marker angeben, die Suche mit hervorgehobenen Begriff umschließen.

**Beispielabfrage suchen**

```
    $savedSearchParametersJson =
    {
      "top":150,
      "highlight":{
        "pre":"{[hl]}",
        "post":"{[/hl]}"
      },
      "query":"*",
      "start":"2015-02-04T21:03:29.231Z",
      "end":"2015-02-11T21:03:29.231Z"
    }
    armclient post /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/search?api-version=2015-03-20 $searchParametersJson
```

**Beispielergebnis:**

```
    {
        "TimeGenerated":
        "2015-05-18T23:55:59Z", "Source":
        "EventLog": "Application",
        "Computer": "smokedturkey.net",
        "EventCategory": 0,
        "EventLevel":1,
        "EventLevelName":
        "Error"
        "Manager ", "__metadata":
        {
            "Type": "Event",
            "TimeGenerated": "2015-05-18T23:55:59Z",
            "highlighting": {
                "EventLevelName":
                ["{[hl]}Error{[/hl]}"]
            }
        }
    }
```

Beachten Sie, dass oben Ergebnis eine Fehlermeldung enthält, die mit dem Präfix und angefügt.

## <a name="computer-groups"></a>Computergruppen

Computergruppen sind spezielle gespeicherte Suchen, die mehrere Computer zurückgeben.  Eine Computergruppe können in anderen Abfragen Sie die Ergebnisse auf den Computern in der Gruppe.  Eine Gruppe wird als eine gespeicherte Suche mit einer Gruppe mit Computer implementiert.

Es folgt eine Beispielantwort für eine Gruppe.

    "etag": "W/\"datetime'2016-04-01T13%3A38%3A04.7763203Z'\"",
    "properties": {
        "Category": "My Computer Groups",
        "DisplayName": "My Computer Group",
        "Query": "srv* | Distinct Computer",
        "Tags": [{
            "Name": "Group",
            "Value": "Computer"
        }],
    "Version": 1
    }

### <a name="retrieving-computer-groups"></a>Abrufen von Computergruppen

Verwenden Sie die Get-Methode mit Gruppen-ID eine Computergruppe abgerufen.

```
armclient get /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Group ID}`?api-version=2015-03-20
```

### <a name="creating-or-updating-a-computer-group"></a>Erstellen oder Aktualisieren einer Computergruppe

Verwenden der Put-Methode eine eindeutige ID Suche zum Erstellen einer neuen Computergruppe. Verwenden Sie eine vorhandenen Computer Gruppen-ID werden, dass ein geändert. Beim Erstellen einer Computergruppe in OMS-Konsole wird die ID der Gruppe und Namen erstellt.

Zur Definition Abfrage muss eine Gruppe von Computern für die Gruppe ordnungsgemäß zurück.  Es wird empfohlen, am Ende der Abfrage mit *| Verschiedene Computer* sichergestellt, die richtigen Daten zurückgegeben.

Die Definition der gespeicherten Suche umfaßt Gruppe die Marke mit dem Wert des Computers für die Suche einer Computergruppe eingestuft werden.

    $etag=Get-Date -Format yyyy-MM-ddThh:mm:ss.msZ
    $groupName="My Computer Group"
    $groupQuery = "Computer=srv* | Distinct Computer"
    $groupCategory="My Computer Groups"
    $groupID = "My Computer Groups | My Computer Group"

    $groupJson = "{'etag': 'W/`"datetime\'" + $etag + "\'`"', 'properties': { 'Category': '" + $groupCategory + "', 'DisplayName':'"  + $groupName + "', 'Query':'" + $groupQuery + "', 'Tags': [{'Name': 'Group', 'Value': 'Computer'}], 'Version':'1'  }"

    armclient put /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/$groupId`?api-version=2015-03-20 $groupJson

### <a name="deleting-computer-groups"></a>Löschen von Computergruppen

Verwenden Sie die Delete-Methode mit Gruppen-ID eine Computergruppe löschen.

```
armclient delete /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/$groupId`?api-version=2015-03-20
```


## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über [Protokoll suchen](log-analytics-log-searches.md) Abfragen mithilfe benutzerdefinierter Felder Kriterien erstellen.
