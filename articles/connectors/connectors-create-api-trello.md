<properties
pageTitle="Trello | Microsoft Azure"
description="Erstellen von Logik apps mit Azure App. Trello gibt Sicht über alle Projekte, am Arbeitsplatz und zu Hause.  Es ist eine einfach, frei, flexibel und visual Möglichkeit Ihre Projekte verwalten und Organisieren alles.  Verbinden Sie mit Trello zum Verwalten von Motherboards, Listen und Karten"
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

# <a name="get-started-with-the-trello-connector"></a>Erste Schritte mit Trello-Anschluss

Trello gibt Sicht über alle Projekte, am Arbeitsplatz und zu Hause.  Es ist eine einfach, frei, flexibel und visual Möglichkeit Ihre Projekte verwalten und Organisieren alles.  Verbinden Sie mit Trello zum Verwalten von Motherboards, Listen und Karten.

>[AZURE.NOTE] Diese Version des Artikels gilt für Logik apps 2015-08-01-Vorschau-Schemaversion. 

Sie können jetzt eine Logik-app erstellen machen, finden Sie unter [Erstellen einer app Logik](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Trigger und Aktionen

Trello-Connector kann als eine Aktion verwendet werden. Es hat Triggern. Alle Connectors unterstützen in JSON und XML-Daten. 

 Trello Konnektor hat folgende Aktionen und Triggern verfügbar:

### <a name="trello-actions"></a>Trello Aktionen
Sie können diese Aktionen durchführen:

|Aktion|Beschreibung|
|--- | ---|
|[ListCards](connectors-create-api-trello.md#listcards)|Liste Karten im board|
|[GetCard](connectors-create-api-trello.md#getcard)|Karte nach Id abrufen|
|[UpdateCard](connectors-create-api-trello.md#updatecard)|Aktualisieren der Karte|
|[DeleteCard](connectors-create-api-trello.md#deletecard)|Karte löschen|
|[CreateCard](connectors-create-api-trello.md#createcard)|Erstellt eine neue Karte in Ihrem Konto trello|
|[ListBoards](connectors-create-api-trello.md#listboards)|Liste-Mainboards|
|[GetBoard](connectors-create-api-trello.md#getboard)|Ruft Motherboard nach Id|
|[ListLists](connectors-create-api-trello.md#listlists)|Listen der Karte im board|
|[GetList](connectors-create-api-trello.md#getlist)|Ruft die Liste von Id|
### <a name="trello-triggers"></a>Trello Trigger
Sie können diese Ereignisse überwachen:

|Trigger | Beschreibung|
|--- | ---|
|Wenn eine neue Karte ein hinzugefügt|Löst einen beim Hinzufügen eine neue Karte ein|
|Wenn eine neue Karte zu einer Liste hinzugefügt|Löst einen Fluss, wenn eine neue Karte zu einer Liste hinzugefügt|


## <a name="create-a-connection-to-trello"></a>Erstellen einer Verbindung mit Trello
Um Logik apps mit Trello erstellen, müssen Sie zunächst eine **Verbindung** anschließend geben Sie die Details für die folgenden Eigenschaften: 

|Eigenschaft| Erforderlich|Beschreibung|
| ---|---|---|
|Token|Ja|Anmeldeinformationen für Trello|
Nach dem Erstellen der Verbindung können sie Aktionen ausführen und warten Sie in diesem Artikel beschriebenen Trigger. 

>[AZURE.INCLUDE [Steps to create a connection to Trello](../../includes/connectors-create-api-trello.md)]

>[AZURE.TIP] Hierzu können Sie in anderen apps Logik.

## <a name="reference-for-trello"></a>Referenz für Trello
Gilt für Version: 1.0

## <a name="onnewcardinboard"></a>OnNewCardInBoard
Wenn eine neue Karte ein hinzugefügt: löst einen beim Hinzufügen eine neue Karte ein 

```GET: /trigger/boards/{board_id}/cards``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|board_id|Zeichenfolge|Ja|Pfad|keine|Eindeutige Id des Disziplinarrates Karten abrufen|

#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|200|Okay|
|400|Ungültige Anforderung|
|401|Nicht autorisiert|
|403|Verboten|
|404|Nicht gefunden|
|500|Interner Serverfehler. Unbekannte Fehler|
|Standard|Vorgang fehlgeschlagen|


## <a name="onnewcardinlist"></a>OnNewCardInList
Wenn eine neue Karte zu einer Liste hinzugefügt: löst einen Fluss, wenn eine neue Karte zu einer Liste hinzugefügt 

```GET: /trigger/lists/{list_id}/cards``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|board_id|Zeichenfolge|Ja|Abfrage|keine|Eindeutige Id des Disziplinarrates Karten abrufen|
|list_id|Zeichenfolge|Ja|Pfad|keine|Eindeutige Id der Liste abrufen von Karten|

#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|200|Okay|
|400|Ungültige Anforderung|
|401|Nicht autorisiert|
|403|Verboten|
|404|Nicht gefunden|
|500|Interner Serverfehler. Unbekannte Fehler|
|Standard|Vorgang fehlgeschlagen|


## <a name="listcards"></a>ListCards
Liste der Karten im Board: Karten im Board auflisten 

```GET: /boards/{board_id}/cards``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|board_id|Zeichenfolge|Ja|Pfad|keine|ID der Platine, alle Karten abzurufen|
|Aktionen|Zeichenfolge|Nein|Abfrage|keine|Liste der Aktionen zurück. Geben Sie 'alle' oder eine durch Kommas getrennte Liste gültiger Werte|
|Anlagen|boolescher Wert|Nein|Abfrage|keine|Anlagen anzeigen|
|attachment_fields|Zeichenfolge|Nein|Abfrage|keine|Listen Sie die Anlagenfelder zurück. Geben Sie 'alle' oder eine durch Kommas getrennte Liste gültiger Werte|
|Aufkleber|boolescher Wert|Nein|Abfrage|keine|Aufkleber anzeigen|
|Mitglieder|boolescher Wert|Nein|Abfrage|keine|Mitglieder anzeigen|
|memeber_fields|Zeichenfolge|Nein|Abfrage|keine|Listen Sie die Memberfelder zurückgegeben. Geben Sie 'alle' oder eine durch Kommas getrennte Liste gültiger Werte|
|CheckItemStates|boolescher Wert|Nein|Abfrage|keine|Die Karte Zustände zurück|
|Prüflisten|Zeichenfolge|Nein|Abfrage|keine|Anzeigen von Prüflisten|
|Grenzwert|ganze Zahl|Nein|Abfrage|keine|Die maximale Anzahl der zurückzugebenden Ergebnisseite, zwischen 1 und 1000|
|seit|Zeichenfolge|Nein|Abfrage|keine|Alle Karten nach diesem Datum abrufen|
|vor|Zeichenfolge|Nein|Abfrage|keine|Alle Karten vor diesem Datum abrufen|
|Filter|Zeichenfolge|Nein|Abfrage|keine|Filtern der Antwort|
|Felder|Zeichenfolge|Nein|Abfrage|keine|Listen Sie die Kartenfelder zurück. Geben Sie 'alle' oder eine durch Kommas getrennte Liste gültiger Werte|

#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|200|Okay|
|400|Ungültige Anforderung|
|401|Nicht autorisiert|
|403|Verboten|
|404|Nicht gefunden|
|500|Interner Serverfehler. Unbekannte Fehler|
|Standard|Vorgang fehlgeschlagen|


## <a name="getcard"></a>GetCard
Karte nach Id abrufen: Karte nach Id 

```GET: /cards/{card_id}``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|board_id|Zeichenfolge|Ja|Abfrage|keine|ID der Platine Karten abrufen|
|card_id|Zeichenfolge|Ja|Pfad|keine|ID der Karte abrufen|
|Aktionen|Zeichenfolge|Nein|Abfrage|keine|Liste der Aktionen zurück. Geben Sie 'alle' oder eine durch Kommas getrennte Liste gültiger Werte|
|actions_entities|boolescher Wert|Nein|Abfrage|keine|Rücklieferungsvorgang Entitäten|
|actions_display|boolescher Wert|Nein|Abfrage|keine|Rücklieferungsvorgang zeigt|
|actions_limit|ganze Zahl|Nein|Abfrage|keine|Maximale Anzahl von zurückzugebenden Aktionen|
|action_fields|Zeichenfolge|Nein|Abfrage|keine|Liste der Aktionsfelder für jede Aktion zurückgegeben. Geben Sie 'alle' oder eine durch Kommas getrennte Liste gültiger Werte|
|action_memberCreator_fields|Zeichenfolge|Nein|Abfrage|keine|Liste der Aktion Memberfelder Ersteller zurückgegeben. Geben Sie 'alle' oder eine durch Kommas getrennte Liste gültiger Werte|
|Anlagen|boolescher Wert|Nein|Abfrage|keine|Zurückgeben von Anlagen|
|attachement_fields|Zeichenfolge|Nein|Abfrage|keine|Liste der Anlagenfelder für jede Anlage zurückgegeben. Geben Sie 'alle' oder eine durch Kommas getrennte Liste gültiger Werte|
|Mitglieder|boolescher Wert|Nein|Abfrage|keine|Member zurückgegeben|
|member_fields|Zeichenfolge|Nein|Abfrage|keine|Liste der Memberfelder für jeden Member zurückgegeben. Geben Sie 'alle' oder eine durch Kommas getrennte Liste gültiger Werte|
|membersVoted|boolescher Wert|Nein|Abfrage|keine|Mitglieder zurück|
|memberVoted_fields|Zeichenfolge|Nein|Abfrage|keine|Liste der stimmen Memberfelder für jedes gewählt zurückgegeben. Geben Sie 'alle' oder eine durch Kommas getrennte Liste gültiger Werte|
|checkItemStates|boolescher Wert|Nein|Abfrage|keine|Return-Karte-Zustände|
|checkItemState_fields|Zeichenfolge|Nein|Abfrage|keine|Liste der Statusfelder für jede Karte Elementzustand zurückgegeben. Geben Sie 'alle' oder eine durch Kommas getrennte Liste gültiger Werte|
|Prüflisten|Zeichenfolge|Nein|Abfrage|keine|Zurückgeben von Prüflisten|
|checklist_fields|Zeichenfolge|Nein|Abfrage|keine|Liste der Prüfliste Felder für jede Prüfliste zurückgegeben. Geben Sie 'alle' oder eine durch Kommas getrennte Liste gültiger Werte|
|Vorstand|boolescher Wert|Nein|Abfrage|keine|Zurückgeben der Platine Karte gehört|
|board_fields|Zeichenfolge|Nein|Abfrage|keine|Listen Sie die Felder Board zurückgegeben. Geben Sie 'alle' oder eine durch Kommas getrennte Liste gültiger Werte|
|Liste|boolescher Wert|Nein|Abfrage|keine|Die Liste die Karte gehört|
|list_fields|Zeichenfolge|Nein|Abfrage|keine|Die Liste der Felder zurück. Geben Sie 'alle' oder eine durch Kommas getrennte Liste gültiger Werte|
|Aufkleber|boolescher Wert|Nein|Abfrage|keine|Aufkleber zurück|
|sticker_fields|Zeichenfolge|Nein|Abfrage|keine|Listen Sie die Aufkleber Felder für jeden Aufkleber zurückgegeben. Geben Sie 'alle' oder eine durch Kommas getrennte Liste gültiger Werte|
|Felder|Zeichenfolge|Nein|Abfrage|keine|Listen Sie die Kartenfelder zurück. Geben Sie 'alle' oder eine durch Kommas getrennte Liste gültiger Werte|

#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|200|Okay|
|400|Ungültige Anforderung|
|401|Nicht autorisiert|
|403|Verboten|
|404|Nicht gefunden|
|500|Interner Serverfehler. Unbekannte Fehler|
|Standard|Vorgang fehlgeschlagen|


## <a name="updatecard"></a>UpdateCard
Aktualisieren: Aktualisieren 

```PUT: /cards/{card_id}``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|board_id|Zeichenfolge|Ja|Abfrage|keine|ID der Platine aus abrufen|
|card_id|Zeichenfolge|Ja|Pfad|keine|ID der Karte aktualisieren|
|updateCard| |Ja|Körper|keine|Aktualisierte Karten|

#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|200|Okay|
|400|Ungültige Anforderung|
|401|Nicht autorisiert|
|403|Verboten|
|404|Nicht gefunden|
|500|Interner Serverfehler. Unbekannte Fehler|
|Standard|Vorgang fehlgeschlagen|


## <a name="deletecard"></a>DeleteCard
Karte löschen: Löschen Karte 

```DELETE: /cards/{card_id}``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|board_id|Zeichenfolge|Ja|Abfrage|keine|ID der Platine aus abrufen|
|card_id|Zeichenfolge|Ja|Pfad|keine|ID der Karte löschen|

#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|200|Okay|
|400|Ungültige Anforderung|
|401|Nicht autorisiert|
|403|Verboten|
|404|Nicht gefunden|
|500|Interner Serverfehler. Unbekannte Fehler|
|Standard|Vorgang fehlgeschlagen|


## <a name="createcard"></a>CreateCard
Karte erstellen: erstellt eine neue Karte in Ihrem Konto Trello 

```POST: /cards``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|board_id|Zeichenfolge|Ja|Abfrage|keine|Eindeutige Id des Disziplinarrates Karten erstellen|
|newCard| |Ja|Körper|keine|Neue Karte hinzufügen gegenüber trello|

#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|200|Okay|
|400|Ungültige Anforderung|
|401|Nicht autorisiert|
|403|Verboten|
|404|Nicht gefunden|
|500|Interner Serverfehler. Unbekannte Fehler|
|Standard|Vorgang fehlgeschlagen|


## <a name="listboards"></a>ListBoards
Liste der Boards: Liste Boards 

```GET: /member/me/boards``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Filter|Zeichenfolge|Nein|Abfrage|keine|Listen Sie Filter für Motherboard Ergebnisse auf. Geben Sie 'alle' oder eine durch Kommas getrennte Liste gültiger Werte|
|Felder|Zeichenfolge|Nein|Abfrage|keine|Listen Sie die Felder Board zurückgegeben. Geben Sie 'alle' oder eine durch Kommas getrennte Liste gültiger Werte|
|Aktionen|Zeichenfolge|Nein|Abfrage|keine|Listen Sie die Aktionsfelder zurück. Geben Sie 'alle' oder eine durch Kommas getrennte Liste gültiger Werte|
|actions_entities|boolescher Wert|Nein|Abfrage|keine|Rücklieferungsvorgang Entitäten|
|actions_limit|ganze Zahl|Nein|Abfrage|keine|Maximale Anzahl von zurückzugebenden Aktionen|
|actions_format|Zeichenfolge|Nein|Abfrage|keine|Geben Sie das Format der Aktionen zurück|
|actions_since|Zeichenfolge|Nein|Abfrage|keine|Aktionen nach dem angegebenen Datum zurück|
|action_fields|Zeichenfolge|Nein|Abfrage|keine|Listen Sie die Felder der Aktion zurück. Geben Sie 'alle' oder eine durch Kommas getrennte Liste gültiger Werte|
|Mitgliedschaften|Zeichenfolge|Nein|Abfrage|keine|Geben Sie Mitgliedschaftsinformationen zurückgegeben. Geben Sie 'alle' oder eine durch Kommas getrennte Liste gültiger Werte|
|Organisation|boolescher Wert|Nein|Abfrage|keine|Gibt an, dass die Organisationsinformationen zurück|
|organization_fields|Zeichenfolge|Nein|Abfrage|keine|Listen Sie die Felder Organisation zurück. Geben Sie 'alle' oder eine durch Kommas getrennte Liste gültiger Werte|
|Listen|Zeichenfolge|Nein|Abfrage|keine|Geben Sie an, ob Listen zurückzugeben, die dem Verwaltungsrat gehören|

#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|200|Okay|
|400|Ungültige Anforderung|
|401|Nicht autorisiert|
|403|Verboten|
|404|Nicht gefunden|
|500|Interner Serverfehler. Unbekannte Fehler|
|Standard|Vorgang fehlgeschlagen|


## <a name="getboard"></a>GetBoard
Ruft Motherboard ID: Ruft Motherboard nach Id 

```GET: /boards/{board_id}``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|board_id|Zeichenfolge|Ja|Pfad|keine|Eindeutige Id des Disziplinarrates zu|
|Aktionen|Zeichenfolge|Nein|Abfrage|keine|Liste der Aktionen zurück. Geben Sie 'alle' oder eine durch Kommas getrennte Liste gültiger Werte|
|action_entities|boolescher Wert|Nein|Abfrage|keine|Gibt an, ob die Aktion Entitäten zurückgegeben|
|actions_display|boolescher Wert|Nein|Abfrage|keine|Geben Sie an, ob Aktivitäten angezeigt werden sollen|
|actions_format|Zeichenfolge|Nein|Abfrage|keine|Geben Sie das Format der Aktionen zurück|
|actions_since|Zeichenfolge|Nein|Abfrage|keine|Nur die Aktionen nach diesem Datum zurück|
|actions_limit|ganze Zahl|Nein|Abfrage|keine|Maximale Anzahl von zurückzugebenden Aktionen|
|action_fields|Zeichenfolge|Nein|Abfrage|keine|Listenfelder mit jedem Feld. Geben Sie 'alle' oder eine durch Kommas getrennte Liste gültiger Werte|
|action_memeber|boolescher Wert|Nein|Abfrage|keine|Gibt an, ob die Aktion Elemente zurückgegeben|
|action_member_fields|Zeichenfolge|Nein|Abfrage|keine|Listen Sie Memberfelder mit jeder Aktionsmember auf Geben Sie 'alle' oder eine durch Kommas getrennte Liste gültiger Werte|
|action_memberCreator|boolescher Wert|Nein|Abfrage|keine|Geben Sie an, ob die Aktion Mitglied Ersteller zurück|
|action_memberCreator_fields|Zeichenfolge|Nein|Abfrage|keine|Listenfelder die Aktion Mitglied Ersteller zurückgegeben. Geben Sie 'alle' oder eine durch Kommas getrennte Liste gültiger Werte|
|Karten|Zeichenfolge|Nein|Abfrage|keine|Geben Sie die Karten wieder|
|card_fields|Zeichenfolge|Nein|Abfrage|keine|Listen Sie die Felder für jede Karte zurückgegeben. Geben Sie 'alle' oder eine durch Kommas getrennte Liste gültiger Werte|
|card_attachments|boolescher Wert|Ja|Abfrage|keine|Gibt an, ob Anlagen für Karten zurück|
|card_attachment_fields|Zeichenfolge|Nein|Abfrage|keine|Listen Sie die Anlagenfelder für jede Anlage zurückgegeben. Geben Sie 'alle' oder eine durch Kommas getrennte Liste gültiger Werte|
|card_checklists|Zeichenfolge|Nein|Abfrage|keine|Geben Sie die Prüflisten für jede Karte zurückgegeben|
|card_stickers|boolescher Wert|Nein|Abfrage|keine|Geben Sie an, ob Karte Aufkleber zurück|
|boardStarts|Zeichenfolge|Nein|Abfrage|keine|Geben Sie Motherboard Sterne zurück|
|Etiketten|Zeichenfolge|Nein|Abfrage|keine|Geben Sie die Daten zurück|
|label_fields|Zeichenfolge|Nein|Abfrage|keine|Listen Sie die Felder Beschriftung für jede Bezeichnung zurückgegeben. Geben Sie 'alle' oder eine durch Kommas getrennte Liste gültiger Werte|
|labels_limits|ganze Zahl|Nein|Abfrage|keine|Maximale Anzahl von zurückzugebenden Etiketten|
|Listen|Zeichenfolge|Nein|Abfrage|keine|Geben Sie die Listen zurück|
|list_fields|Zeichenfolge|Nein|Abfrage|keine|Liste der Listenfelder für jede Liste zurückgegeben. Geben Sie 'alle' oder eine durch Kommas getrennte Liste gültiger Werte|
|Mitgliedschaften|Zeichenfolge|Nein|Abfrage|keine|Liste der Mitgliedschaften zurückgegeben. Geben Sie 'alle' oder eine durch Kommas getrennte Liste gültiger Werte|
|memberships_member|boolescher Wert|Nein|Abfrage|keine|Gibt an, ob die Mitgliedschaft Mitglieder zurück|
|memberships_member_fields|Zeichenfolge|Nein|Abfrage|keine|Listen Sie Memberfelder zurückzugebenden für jedes Mitglied auf. Geben Sie 'alle' oder eine durch Kommas getrennte Liste gültiger Werte|
|Mitglieder|Zeichenfolge|Nein|Abfrage|keine|Liste der zurückzugebenden Elemente|
|member_fields|Zeichenfolge|Nein|Abfrage|keine|Listen Sie die Memberfelder für jeden Member zurückgegeben. Geben Sie 'alle' oder eine durch Kommas getrennte Liste gültiger Werte|
|membersInvited|Zeichenfolge|Nein|Abfrage|keine|Geben Sie die eingeladenen Mitgliedern zurück|
|membersInvited_fields|Zeichenfolge|Nein|Abfrage|keine|Listen Sie die Felder für jede zurückgegeben. Geben Sie 'alle' oder eine durch Kommas getrennte Liste gültiger Werte|
|Prüflisten|Zeichenfolge|Nein|Abfrage|keine|Geben Sie die Prüflisten zurück|
|checklist_fields|Zeichenfolge|Nein|Abfrage|keine|Die Prüfliste Listenfelder für jede Prüfliste zurückgegeben. Geben Sie 'alle' oder eine durch Kommas getrennte Liste gültiger Werte|
|Organisation|boolescher Wert|Nein|Abfrage|keine|Gibt an, ob die Informationen zurück|
|organization_fields|Zeichenfolge|Nein|Abfrage|keine|Liste der Organisation Felder für jede Organisation zurückgegeben. Geben Sie 'alle' oder eine durch Kommas getrennte Liste gültiger Werte|
|organization_memberships|Zeichenfolge|Nein|Abfrage|keine|Liste der Mitgliedschaften zurückgegeben. Geben Sie 'alle' oder eine durch Kommas getrennte Liste gültiger Werte|
|myPerfs|boolescher Wert|Nein|Abfrage|keine|Geben Sie an, ob meine Perfs zurück|
|Felder|Zeichenfolge|Nein|Abfrage|keine|Liste der Felder zurück. Geben Sie 'alle' oder eine durch Kommas getrennte Liste gültiger Werte|

#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|200|Okay|
|400|Ungültige Anforderung|
|401|Nicht autorisiert|
|403|Verboten|
|404|Nicht gefunden|
|500|Interner Serverfehler. Unbekannte Fehler|
|Standard|Vorgang fehlgeschlagen|


## <a name="listlists"></a>ListLists
Listen der Karte im Board: Karte Listen im Board 

```GET: /boards/{board_id}/lists``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|board_id|Zeichenfolge|Ja|Pfad|keine|Eindeutige Id des Disziplinarrates abgerufen werden aufgelistet|
|Karten|Zeichenfolge|Nein|Abfrage|keine|Geben Sie die Karten wieder|
|card_fields|Zeichenfolge|Nein|Abfrage|keine|Listenfelder die Karte wieder aus. Geben Sie 'alle' oder eine durch Kommas getrennte Liste gültiger Werte|
|Filter|Zeichenfolge|Nein|Abfrage|keine|Geben Sie die Filter-Eigenschaft für Listen|
|Felder|Zeichenfolge|Nein|Abfrage|keine|Liste der Felder zurück. Geben Sie 'alle' oder eine durch Kommas getrennte Liste gültiger Werte|

#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|200|Okay|
|400|Ungültige Anforderung|
|401|Nicht autorisiert|
|403|Verboten|
|404|Nicht gefunden|
|500|Interner Serverfehler. Unbekannte Fehler|
|Standard|Vorgang fehlgeschlagen|


## <a name="getlist"></a>GetList
Ruft Liste ID: Ruft Liste nach Id 

```GET: /lists/{list_id}``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|board_id|Zeichenfolge|Ja|Abfrage|keine|Eindeutige Id der Platine aus Listen abrufen|
|list_id|Zeichenfolge|Ja|Pfad|keine|Eindeutige Id der Liste abrufen|
|Karten|Zeichenfolge|Nein|Abfrage|keine|Geben Sie die Karten wieder|
|card_fields|Zeichenfolge|Nein|Abfrage|keine|Listen Sie die Kartenfelder für jede Karte zurückgegeben. Geben Sie 'alle' oder eine durch Kommas getrennte Liste gültiger Werte|
|Vorstand|boolescher Wert|Nein|Abfrage|keine|Geben Sie an, ob Informationen zurück|
|board_fields|Zeichenfolge|Nein|Abfrage|keine|Listen Sie die Felder Board zurückgegeben. Geben Sie 'alle' oder eine durch Kommas getrennte Liste gültiger Werte|
|Felder|Zeichenfolge|Nein|Abfrage|keine|Die Liste der Felder zurück. Geben Sie 'alle' oder eine durch Kommas getrennte Liste gültiger Werte|

#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|200|Okay|
|400|Ungültige Anforderung|
|401|Nicht autorisiert|
|403|Verboten|
|404|Nicht gefunden|
|500|Interner Serverfehler. Unbekannte Fehler|
|Standard|Vorgang fehlgeschlagen|


## <a name="object-definitions"></a>Objektdefinitionen 

### <a name="card"></a>Karte


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|ID|Zeichenfolge|Nein |
|checkItemStates|Array|Nein |
|geschlossen|boolescher Wert|Nein |
|dateLastActivity|Zeichenfolge|Nein |
|DESC|Zeichenfolge|Nein |
|idBoard|Zeichenfolge|Nein |
|Zonen|Zeichenfolge|Nein |
|idMembersVoted|Array|Nein |
|idShort|ganze Zahl|Nein |
|idAttachmentCover|Zeichenfolge|Nein |
|manualCoverAttachment|boolescher Wert|Nein |
|idLabels|Array|Nein |
|Name|Zeichenfolge|Nein |
|POS|ganze Zahl|Nein |
|shortLink|Zeichenfolge|Nein |
|Kennzeichen|nicht definiert|Nein |
|Fälligkeitsdatum|Zeichenfolge|Nein |
|E-Mail|Zeichenfolge|Nein |
|shortUrl|Zeichenfolge|Nein |
|abonniert|boolescher Wert|Nein |
|URL|Zeichenfolge|Nein |



### <a name="badges"></a>Kennzeichen


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|Stimmen|ganze Zahl|Nein |
|ViewingMemberVoted|boolescher Wert|Nein |
|Abonniert|boolescher Wert|Nein |
|Fogbugz|Zeichenfolge|Nein |
|CheckItems|ganze Zahl|Nein |
|CheckItemsChecked|ganze Zahl|Nein |
|Kommentare|ganze Zahl|Nein |
|Anlagen|ganze Zahl|Nein |
|Beschreibung|boolescher Wert|Nein |
|Fälligkeitsdatum|Zeichenfolge|Nein |



### <a name="object"></a>Objekt


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|



### <a name="createcard"></a>CreateCard


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|Zonen|Zeichenfolge|Ja |
|Name|Zeichenfolge|Ja |
|DESC|Zeichenfolge|Nein |
|POS|Zeichenfolge|Nein |
|idMembers|Zeichenfolge|Nein |
|idLabels|Zeichenfolge|Nein |
|urlSource|Zeichenfolge|Nein |
|fileSource|Zeichenfolge|Nein |
|idCardSource|Zeichenfolge|Nein |
|keepFromSource|Zeichenfolge|Nein |



### <a name="updatecard"></a>UpdateCard


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|Name|Zeichenfolge|Nein |
|DESC|Zeichenfolge|Nein |
|geschlossen|boolescher Wert|Nein |
|idMembers|Zeichenfolge|Nein |
|idAttachmentCover|Zeichenfolge|Nein |
|Zonen|Zeichenfolge|Nein |
|idBoard|Zeichenfolge|Nein |
|POS|Zeichenfolge|Nein |
|Fälligkeitsdatum|Zeichenfolge|Nein |
|abonniert|boolescher Wert|Nein |



### <a name="board"></a>Vorstand


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|ID|Zeichenfolge|Nein |
|geschlossen|boolescher Wert|Nein |
|dateLastActivity|Zeichenfolge|Nein |
|dateLastView|Zeichenfolge|Nein |
|DESC|Zeichenfolge|Nein |
|idOrganization|Zeichenfolge|Nein |
|Einladung|Array|Nein |
|eingeladen|boolescher Wert|Nein |
|Markennamen|nicht definiert|Nein |
|Mitgliedschaften|Array|Nein |
|Name|Zeichenfolge|Nein |
|fixiert|boolescher Wert|Nein |
|powerUps|Array|Nein |
|perfs|nicht definiert|Nein |
|shortLink|Zeichenfolge|Nein |
|shortUrl|Zeichenfolge|Nein |
|Favoriten|Zeichenfolge|Nein |
|abonniert|Zeichenfolge|Nein |
|URL|Zeichenfolge|Nein |



### <a name="label"></a>Bezeichnung


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|Grün|Zeichenfolge|Nein |
|Gelb|Zeichenfolge|Nein |
|Orange|Zeichenfolge|Nein |
|Rot|Zeichenfolge|Nein |
|Lila|Zeichenfolge|Nein |
|Blau|Zeichenfolge|Nein |
|Himmel|Zeichenfolge|Nein |
|limone|Zeichenfolge|Nein |
|rosa|Zeichenfolge|Nein |
|Schwarz|Zeichenfolge|Nein |



### <a name="membership"></a>Mitgliedschaft


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|ID|Zeichenfolge|Nein |
|idMember|Zeichenfolge|Nein |
|memberType|Zeichenfolge|Nein |
|nicht bestätigt|boolescher Wert|Nein |



### <a name="perfs"></a>Perfs


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|permissionLevel|Zeichenfolge|Nein |
|Abstimmung|Zeichenfolge|Nein |
|Kommentare|Zeichenfolge|Nein |
|Einladung|Zeichenfolge|Nein |
|selfJoin|boolescher Wert|Nein |
|cardCovers|boolescher Wert|Nein |
|calendarFeedEnabled|boolescher Wert|Nein |
|Hintergrund|Zeichenfolge|Nein |
|Hintergrundfarbe|Zeichenfolge|Nein |
|backgroundImage|Zeichenfolge|Nein |
|backgroundImageScaled|Zeichenfolge|Nein |
|backgroundTile|boolescher Wert|Nein |
|backgroundBrightness|Zeichenfolge|Nein |
|canBePublic|boolescher Wert|Nein |
|canBeOrg|boolescher Wert|Nein |
|canBePrivate|boolescher Wert|Nein |
|canInvite|boolescher Wert|Nein |



### <a name="list"></a>Liste


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|ID|Zeichenfolge|Nein |
|Name|Zeichenfolge|Nein |
|geschlossen|boolescher Wert|Nein |
|idBoard|Zeichenfolge|Nein |
|POS|Anzahl|Nein |
|abonniert|boolescher Wert|Nein |
|Karten|Array|Nein |
|Vorstand|nicht definiert|Nein |


## <a name="next-steps"></a>Nächste Schritte
[Erstellen einer Logik](../app-service-logic/app-service-logic-create-a-logic-app.md)