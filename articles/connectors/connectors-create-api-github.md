<properties
pageTitle="GitHub | Microsoft Azure"
description="Erstellen von Logik apps mit Azure App. GitHub ist eine Web-basierte Git hosting-Dienst. Es bietet die verteilte Version Steuerelement und Datenquelle Management (SCM) Funktionalität Git sowie eigene Funktionen hinzufügen."
services="logic-apps"   
documentationCenter=".net,nodejs,java"  
authors="msftman"   
manager="erikre"    
editor=""
tags="connectors" />

<tags
ms.service="logic-apps"
ms.devlang="multiple"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="integration"
ms.date="08/18/2016"
ms.author="deonhe"/>

# <a name="get-started-with-the-github-connector"></a>Erste Schritte mit der GitHub-connector

GitHub ist eine Web-basierte Git hosting-Dienst. Es bietet die verteilte Version Steuerelement und Datenquelle Management (SCM) Funktionalität Git sowie eigene Funktionen hinzufügen.

>[AZURE.NOTE] Diese Version des Artikels gilt für Logik apps 2015-08-01-Vorschau-Schemaversion. 

Sie können jetzt eine Logik-app erstellen machen, finden Sie unter [Erstellen einer app Logik](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Trigger und Aktionen

GitHub-Connector kann als eine Aktion verwendet werden. Es hat Triggern. Alle Connectors unterstützen in JSON und XML-Daten. 

 GitHub Konnektor hat folgende Aktionen und Triggern verfügbar:

### <a name="github-actions"></a>GitHub Aktionen
Sie können diese Aktionen durchführen:

|Aktion|Beschreibung|
|--- | ---|
|[CreateIssue](connectors-create-api-github.md#createissue)|Erstellt ein Problem|
### <a name="github-triggers"></a>GitHub Trigger
Sie können diese Ereignisse überwachen:

|Trigger | Beschreibung|
|--- | ---|
|Wenn ein Problem geöffnet|Ein Problem wird geöffnet.|
|Wenn ein Problem geschlossen|Ein Problem wird geschlossen|
|Wenn ein Problem zugewiesen|Ein Problem zugewiesen|


## <a name="create-a-connection-to-github"></a>Erstellen einer Verbindung mit GitHub
Um Logik apps mit GitHub erstellen, müssen Sie zunächst eine **Verbindung** anschließend geben Sie die Details für die folgenden Eigenschaften: 

|Eigenschaft| Erforderlich|Beschreibung|
| ---|---|---|
|Token|Ja|Anmeldeinformationen für GitHub|
Nach dem Erstellen der Verbindung können sie Aktionen ausführen und warten Sie in diesem Artikel beschriebenen Trigger. 

>[AZURE.INCLUDE [Steps to create a connection to GitHub](../../includes/connectors-create-api-github.md)]

>[AZURE.TIP] Hierzu können Sie in anderen apps Logik.

## <a name="reference-for-github"></a>Referenz für GitHub
Gilt für Version: 1.0

## <a name="createissue"></a>CreateIssue
Ein Problem erstellen: erstellt ein Problem 

```POST: /repos/{repositoryOwner}/{repositoryName}/issues``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|repositoryOwner|Zeichenfolge|Ja|Pfad|keine|Repository-Eigentümer|
|: repositoryName|Zeichenfolge|Ja|Pfad|keine|Repository-name|
|issueBasicDetails| |Ja|Körper|keine|Problemdetails|

#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|200|Okay|
|400|Ungültige Anforderung|
|401|Nicht autorisiert|
|403|Verboten|
|404|Nicht gefunden|
|500|Interner Serverfehler. Unbekannte Fehler|
|Standard|Vorgang ist fehlgeschlagen.|


## <a name="issueopened"></a>IssueOpened
Wenn ein Problem geöffnet: ein Problem geöffnet 

```GET: /trigger/issueOpened``` 

Es gibt keine Parameter für diesen Aufruf
#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|200|Okay|
|400|Ungültige Anforderung|
|401|Nicht autorisiert|
|403|Verboten|
|404|Nicht gefunden|
|500|Interner Serverfehler. Unbekannte Fehler|
|Standard|Vorgang ist fehlgeschlagen.|


## <a name="issueclosed"></a>IssueClosed
Wenn ein Problem geschlossen: Problem geschlossen 

```GET: /trigger/issueClosed``` 

Es gibt keine Parameter für diesen Aufruf
#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|200|Okay|
|400|Ungültige Anforderung|
|401|Nicht autorisiert|
|403|Verboten|
|404|Nicht gefunden|
|500|Interner Serverfehler. Unbekannte Fehler|
|Standard|Vorgang ist fehlgeschlagen.|


## <a name="issueassigned"></a>IssueAssigned
Wenn ein Problem zugewiesen: ein Problem zugewiesen 

```GET: /trigger/issueAssigned``` 

Es gibt keine Parameter für diesen Aufruf
#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|200|Okay|
|400|Ungültige Anforderung|
|401|Nicht autorisiert|
|403|Verboten|
|404|Nicht gefunden|
|500|Interner Serverfehler. Unbekannte Fehler|
|Standard|Vorgang ist fehlgeschlagen.|


## <a name="object-definitions"></a>Objektdefinitionen 

### <a name="issuebasicdetailsmodel"></a>IssueBasicDetailsModel


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|Titel|Zeichenfolge|Ja |
|Körper|Zeichenfolge|Ja |
|Beauftragten|Zeichenfolge|Ja |



### <a name="issuedetailsmodel"></a>IssueDetailsModel


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|Titel|Zeichenfolge|Ja |
|Körper|Zeichenfolge|Ja |
|Beauftragten|Zeichenfolge|Ja |
|Anzahl|Zeichenfolge|Nein |
|Zustand|Zeichenfolge|Nein |
|created_at|Zeichenfolge|Nein |
|repository_url|Zeichenfolge|Nein |


## <a name="next-steps"></a>Nächste Schritte
[Erstellen einer Logik](../app-service-logic/app-service-logic-create-a-logic-app.md)