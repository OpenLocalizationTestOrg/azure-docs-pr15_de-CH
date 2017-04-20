<properties
pageTitle="ProjectOnline | Microsoft Azure"
description="Erstellen von Logik apps mit Azure App. Projekt Online ist eine flexible Lösung für Project-Portfoliomanagement (PPM) und die tägliche Arbeit von Microsoft online. Übermittelt über Office 365 Project Online können Unternehmen Einstieg mit leistungsstarken Projekt planen, priorisieren und verwalten und Projekt-portfolioinvestitionen – von überall aus auf fast jedem Gerät."
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

# <a name="get-started-with-the-projectonline-connector"></a>Erste Schritte mit der ProjectOnline-connector

Projekt Online ist eine flexible Lösung für Project-Portfoliomanagement (PPM) und die tägliche Arbeit von Microsoft online. Übermittelt über Office 365 Project Online können Unternehmen Einstieg mit leistungsstarken Projekt planen, priorisieren und verwalten und Projekt-portfolioinvestitionen – von überall aus auf fast jedem Gerät.

>[AZURE.NOTE] Diese Version des Artikels gilt für Logik apps 2015-08-01-Vorschau-Schemaversion. 

Sie können jetzt eine Logik-app erstellen machen, finden Sie unter [Erstellen einer app Logik](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Trigger und Aktionen

ProjectOnline-Connector kann als eine Aktion verwendet werden. Es hat Triggern. Alle Connectors unterstützen in JSON und XML-Daten. 

 ProjectOnline Konnektor hat folgende Aktionen und Triggern verfügbar:

### <a name="projectonline-actions"></a>ProjectOnline Aktionen
Sie können diese Aktionen durchführen:

|Aktion|Beschreibung|
|--- | ---|
|[ListProjects](connectors-create-api-projectonline.md#listprojects)|Listet die Projekte in der Projektsite online|
|[CreateProject](connectors-create-api-projectonline.md#createproject)|Erstellt ein neues Projekt in Ihr Projekt online-Website|
|[CreateTask](connectors-create-api-projectonline.md#createtask)|Erstellt eine neue Aufgabe im Projekt|
|[CreateResource](connectors-create-api-projectonline.md#createresource)|Erstellt ein Enterprise-Ressourcen in Ihrem Projekt online-Website|
|[ListTasks](connectors-create-api-projectonline.md#listtasks)|Die veröffentlichten Aufgaben in einem Projekt|
|[CheckoutProject](connectors-create-api-projectonline.md#checkoutproject)|Checkt ein Projekt in Ihrer Website|
|[PublishProject](connectors-create-api-projectonline.md#publishproject)|Einchecken, veröffentlichen und vorhandene Projekt in Ihrer Website|
### <a name="projectonline-triggers"></a>ProjectOnline Trigger
Sie können diese Ereignisse überwachen:

|Trigger | Beschreibung|
|--- | ---|
|Wenn ein neues Projekt erstellt|Löst einen beim Erstellen ein neues Projekts|
|Wenn eine neue Ressource erstellt wird|Löst einen neuen Datenfluss beim Erstellen eine neue Ressource|
|Wenn ein neuer Vorgang erstellt|Löst einen beim Erstellen eines neuen Vorgangs|


## <a name="create-a-connection-to-projectonline"></a>Erstellen Sie eine Verbindung zu ProjectOnline
Um Logik apps mit ProjectOnline erstellen, müssen Sie zunächst eine **Verbindung** anschließend geben Sie die Details für die folgenden Eigenschaften: 

|Eigenschaft| Erforderlich|Beschreibung|
| ---|---|---|
|Token|Ja|Anmeldeinformationen für ProjectOnline|

>[AZURE.INCLUDE [Steps to create a connection to ProjectOnline](../../includes/connectors-create-api-projectonline.md)]

>[AZURE.TIP] Hierzu können Sie in anderen apps Logik.

## <a name="reference-for-projectonline"></a>Referenz für ProjectOnline
Gilt für Version: 1.0

## <a name="onnewproject"></a>OnNewProject
Erstellung ein neues Projekts: einen auslöst, wenn ein neues Projekt erstellt wird 

```GET: /trigger/_api/ProjectData/Projects``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|siteUrl|Zeichenfolge|Ja|Abfrage|keine|Root-Website-Url für die Projektwebsite (Beispiel: https://sampletenant.sharepoint.com/teams/sampleteam)|

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


## <a name="onnewresource"></a>OnNewResource
Wenn eine neue Ressource erstellt: löst keinen neuen Datenfluss beim Erstellen eine neue Ressource 

```GET: /trigger/_api/ProjectData/Resources``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|siteUrl|Zeichenfolge|Ja|Abfrage|keine|Root-Website-Url für die Projektwebsite (Beispiel: https://sampletenant.sharepoint.com/teams/sampleteam)|

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


## <a name="onnewtask"></a>OnNewTask
Wenn ein neuer Vorgang erstellt: löst einen beim Erstellen eines neuen Vorgangs 

```GET: /trigger/_api/ProjectData/Tasks``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|siteUrl|Zeichenfolge|Ja|Abfrage|keine|Root-Website-Url für die Projektwebsite (Beispiel: https://sampletenant.sharepoint.com/teams/sampleteam)|

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


## <a name="listprojects"></a>ListProjects
Liste der Projekte: führt die Projekte in der Projektsite online 

```GET: /_api/ProjectServer/Projects``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|siteUrl|Zeichenfolge|Ja|Abfrage|keine|Root-Website-Url für die Projektwebsite (Beispiel: https://sampletenant.sharepoint.com/teams/sampleteam)|

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


## <a name="createproject"></a>CreateProject
Erstellt neues Projekt: erstellt ein neues Projekt in Ihr Projekt online-Website 

```POST: /_api/ProjectServer/Projects``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|siteUrl|Zeichenfolge|Ja|Abfrage|keine|Root-Website-Url für die Projektwebsite (Beispiel: https://sampletenant.sharepoint.com/teams/sampleteam)|
|proj| |Ja|Körper|keine|Neues Projekt erstellen|

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


## <a name="createtask"></a>CreateTask
Erstellt neue Aufgabe: erstellt eine neue Aufgabe im Projekt 

```POST: /_api/ProjectServer/Projects('{project_id}')/Draft/Tasks/Add``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|siteUrl|Zeichenfolge|Ja|Abfrage|keine|Root-Website-Url für die Projektwebsite (Beispiel: https://sampletenant.sharepoint.com/teams/sampleteam)|
|PROJECT_ID|Zeichenfolge|Ja|Pfad|keine|Eindeutige Kennung des Projekts, den Vorgang hinzufügen|
|Aufgabe| |Ja|Körper|keine|Neue Aufgabe zum Projekt hinzufügen|

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


## <a name="createresource"></a>CreateResource
Neue Ressource erstellen: erstellt ein Enterprise-Ressourcen in Ihrem Projekt online-Website 

```POST: /_api/ProjectServer/EnterpriseResources``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|siteUrl|Zeichenfolge|Ja|Abfrage|keine|Root-Website-Url für die Projektwebsite (Beispiel: https://sampletenant.sharepoint.com/teams/sampleteam)|
|Ressource| |Ja|Körper|keine|Neue Enterprise-Ressource zum Projekt hinzufügen|

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


## <a name="listtasks"></a>ListTasks
Führt Aufgaben: die veröffentlichte Aufgaben in einem Projekt 

```GET: /_api/ProjectServer/Projects('{project_id}')/Tasks``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|siteUrl|Zeichenfolge|Ja|Abfrage|keine|Root-Website-Url für die Projektwebsite (Beispiel: https://sampletenant.sharepoint.com/teams/sampleteam)|
|PROJECT_ID|Zeichenfolge|Ja|Pfad|keine|Eindeutige ID des Projekts Aufgaben abrufen|

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


## <a name="checkoutproject"></a>CheckoutProject
Ein Projekt auschecken: Auschecken von Projekten in Ihrer Website 

```POST: /_api/ProjectServer/Projects('{project_id}')/checkOut``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|siteUrl|Zeichenfolge|Ja|Abfrage|keine|Root-Website-Url für die Projektwebsite (Beispiel: https://sampletenant.sharepoint.com/teams/sampleteam)|
|PROJECT_ID|Zeichenfolge|Ja|Pfad|keine|Eindeutige Kennung des Projekts, den Vorgang hinzufügen|

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


## <a name="publishproject"></a>PublishProject
Einchecken und Projekt veröffentlichen: Einchecken veröffentlichen und vorhandene Projekt in Ihrer Website 

```POST: /_api/ProjectServer/Projects('{project_id}')/Draft/Publish(true)``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|siteUrl|Zeichenfolge|Ja|Abfrage|keine|Root-Website-Url für die Projektwebsite (Beispiel: https://sampletenant.sharepoint.com/teams/sampleteam)|
|PROJECT_ID|Zeichenfolge|Ja|Pfad|keine|Eindeutige ID des Projekts Einchecken|

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

### <a name="triggerprojectswrapper"></a>TriggerProjectsWrapper


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|Wert|Array|Nein |



### <a name="triggerproject"></a>TriggerProject


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|ProjectStartDate|Zeichenfolge|Nein |
|ProjectFinishDate|Zeichenfolge|Nein |
|ProjectCreatedDate|Zeichenfolge|Nein |
|ProjectId|Zeichenfolge|Nein |
|ProjectModifiedDate|Zeichenfolge|Nein |
|ProjectType|ganze Zahl|Nein |
|Projektname|Zeichenfolge|Nein |



### <a name="triggerresourceswrapper"></a>TriggerResourcesWrapper


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|Wert|Array|Nein |



### <a name="triggerresource"></a>TriggerResource


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|ResourceId|Zeichenfolge|Nein |
|ResourceBaseCalendar|Zeichenfolge|Nein |
|ResourceBookingType|ganze Zahl|Nein |
|ResourceCanLevel|boolescher Wert|Nein |
|ResourceCostPerUse|Anzahl|Nein |
|ResourceCreatedDate|Zeichenfolge|Nein |
|ResourceEarliestAvailableFrom|Zeichenfolge|Nein |
|ResourceEmail|Zeichenfolge|Nein |
|ResourceInitials|Zeichenfolge|Nein |
|ResourceIsActive|boolescher Wert|Nein |
|ResourceIsGeneric|boolescher Wert|Nein |
|ResourceLatestAvailableTo|Zeichenfolge|Nein |
|ResourceModifiedDate|Zeichenfolge|Nein |
|ResourceName|Zeichenfolge|Nein |
|ResourceStatsuName|Zeichenfolge|Nein |
|ResourceType|ganze Zahl|Nein |
|TypeDescription|Zeichenfolge|Nein |
|TypeName|Zeichenfolge|Nein |



### <a name="triggertaskswrapper"></a>TriggerTasksWrapper


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|Wert|Array|Nein |



### <a name="triggertask"></a>TriggerTask


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|ProjectId|Zeichenfolge|Nein |
|TaskId|Zeichenfolge|Nein |
|Projektname|Zeichenfolge|Nein |
|TaskName|Zeichenfolge|Nein |
|TaskCreatedDate|Zeichenfolge|Nein |
|TaskModifieddate|Zeichenfolge|Nein |
|TaskStartDate|Zeichenfolge|Nein |
|TaskFinishDate|Zeichenfolge|Nein |
|TaskPriority|ganze Zahl|Nein |
|TaskIsActive|boolescher Wert|Nein |



### <a name="newproject"></a>Neues Projekt


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|Name|Zeichenfolge|Ja |
|Beschreibung|Zeichenfolge|Nein |
|Starten|Zeichenfolge|Nein |



### <a name="newreource"></a>NewReource


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|Name|Zeichenfolge|Ja |
|IsBudget|boolescher Wert|Nein |
|IsGeneric|boolescher Wert|Nein |
|IsInactive|boolescher Wert|Nein |



### <a name="project"></a>Projekt


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|ApprovedStart|Zeichenfolge|Nein |
|ApprovedEnd|Zeichenfolge|Nein |
|CheckedOutDate|Zeichenfolge|Nein |
|CheckOutDescription|Zeichenfolge|Nein |
|CheckOutId|Zeichenfolge|Nein |
|CreatedDate|Zeichenfolge|Nein |
|ID|Zeichenfolge|Nein |
|IsCheckedOut|boolescher Wert|Nein |
|LastPublishedDate|Zeichenfolge|Nein |
|LastSavedDate|Zeichenfolge|Nein |
|OptimizerDecision|ganze Zahl|Nein |
|PlannerDecision|ganze Zahl|Nein |
|ProjectType|ganze Zahl|Nein |
|Name|Zeichenfolge|Nein |
|WinprojVersion|Zeichenfolge|Nein |



### <a name="projectswrapper"></a>ProjectsWrapper


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|Wert|Array|Nein |



### <a name="newtask"></a>NewTask


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|Parameter|nicht definiert|Ja |



### <a name="taskparameters"></a>TaskParameters


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|Name|Zeichenfolge|Ja |
|Notizen|Zeichenfolge|Nein |
|Starten|Zeichenfolge|Nein |
|Dauer|Zeichenfolge|Nein |



### <a name="enterpriseresource"></a>EnterpriseResource


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|CanLevel|boolescher Wert|Nein |
|Code|Zeichenfolge|Nein |
|CostAccrual|ganze Zahl|Nein |
|CostCenter|Zeichenfolge|Nein |
|Erstellt|Zeichenfolge|Nein |
|DefaultBookingType|ganze Zahl|Nein |
|E-Mail|Zeichenfolge|Nein |
|ExternalId|Zeichenfolge|Nein |
|Gruppe|Zeichenfolge|Nein |
|HireDate|Zeichenfolge|Nein |
|ID|Zeichenfolge|Nein |
|Initialen|Zeichenfolge|Nein |
|IsActive|boolescher Wert|Nein |
|IsBudget|boolescher Wert|Nein |
|IsCheckedOut|boolescher Wert|Nein |
|IsGeneric|boolescher Wert|Nein |
|IsTeam|boolescher Wert|Nein |
|MaterialLabel|Zeichenfolge|Nein |
|Geändert|Zeichenfolge|Nein |
|Name|Zeichenfolge|Nein |
|Lautschrift|Zeichenfolge|Nein |
|ResourceType|ganze Zahl|Nein |
|TerminationDate|Zeichenfolge|Nein |



### <a name="taskswrapper"></a>TasksWrapper


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|Wert|Array|Nein |



### <a name="task"></a>Aufgabe


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|Erstellt|Zeichenfolge|Nein |
|Geändert|Zeichenfolge|Nein |
|Starten|Zeichenfolge|Nein |
|Fertig stellen|Zeichenfolge|Nein |
|Name|Zeichenfolge|Nein |
|ID|Zeichenfolge|Nein |
|Priorität|ganze Zahl|Nein |
|PercentComplete|ganze Zahl|Nein |
|Notizen|Zeichenfolge|Nein |
|Kontakt|Zeichenfolge|Nein |


## <a name="next-steps"></a>Nächste Schritte
[Erstellen einer Logik](../app-service-logic/app-service-logic-create-a-logic-app.md)