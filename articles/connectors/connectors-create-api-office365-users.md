<properties
    pageTitle="Office 365 Benutzer Connector Logik Apps hinzufügen | Microsoft Azure"
    description="Überblick über Office 365 Benutzer Verbindung mit REST-API"
    services=""    
    documentationCenter=""     
    authors="msftman"    
    manager="erikre"    
    editor="" 
    tags="connectors" />

<tags
ms.service="multiple"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="integration"
ms.date="08/18/2016"
ms.author="deonhe"/>

# <a name="get-started-with-the-office-365-users-connector"></a>Erste Schritte mit Office 365 Benutzer connector

Verbinden Sie mit Office 365-Benutzern zu Profilen und Suchbenutzer. 

>[AZURE.NOTE] Diese Version des Artikels gilt für Logik apps 2015-08-01-Vorschau-Schemaversion.

Mit Office 365-Benutzer können Sie:

- Erstellen Sie Ihr Geschäftsablauf basierend auf den Daten von Office 365-Benutzern erhalten. 
- Mit Aktionen, die direkten Berichte, bekommen ein Manager Benutzerprofil und mehr. Diese Aktionen eine Antwort erhalten und dann die Ausgabe für andere Aktionen verfügbar machen. Beispielsweise erhalten Sie Mitarbeiter eine Person Informationen Sie diese und aktualisieren Sie SQL Azure-Datenbank. 

Ein Vorgang in Logik-apps finden Sie unter [Erstellen einer app Logik](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Trigger und Aktionen

Der Konnektor Office 365-Benutzer hat die folgenden Aktionen. Es gibt keine Trigger.

| Trigger | Aktionen|
| --- | --- |
|Keine | <ul><li>Manager erhalten</li><li>Mein Profil</li><li>Direkte Berichte</li><li>Profil abrufen</li><li>Nach Benutzern suchen</li></ul>|

Alle Connectors unterstützen in JSON und XML-Daten. 


## <a name="create-a-connection-to-office-365-users"></a>Erstellen einer Verbindung mit Office 365-Benutzer

Beim Hinzufügen dieser Connector zu Logik-apps Sie Ihrem Office 365-Benutzer Konto anmelden und Logik-apps zu Ihrem Konto herstellen können.

>[AZURE.INCLUDE [Steps to create a connection to Office 365 Users](../../includes/connectors-create-api-office365users.md)]

Nachdem Sie eine Verbindung erstellen, geben Sie Office 365 Benutzer Eigenschaften, wie die Benutzer-ID Die **REST-API-Referenz** in diesem Thema werden diese Eigenschaften beschrieben.

>[AZURE.TIP] Office 365 Benutzer dieselbe Verbindung können Sie in anderen apps Logik.


## <a name="office-365-users-rest-api-reference"></a>Office 365 Benutzer REST-API-Referenz
Gilt für Version: 1.0.

### <a name="get-my-profile"></a>Mein Profil 
Ruft das Profil des aktuellen Benutzers ab.  
```GET: /users/me``` 

Es sind keine Parameter für diesen Aufruf.

#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|200|Vorgang war erfolgreich|
|202|Vorgang war erfolgreich|
|400|BadRequest|
|401|Nicht autorisiert|
|403|Verboten|
|500|Interner Serverfehler|
|Standard|Vorgang ist fehlgeschlagen.|


### <a name="get-user-profile"></a>Profil abrufen 
Ruft ein bestimmtes Benutzerprofil an.  
```GET: /users/{userId}``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Benutzer-ID|Zeichenfolge|Ja|Pfad|keine|User principal Name oder e-Mail-id|

#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|200|Vorgang war erfolgreich|
|202|Vorgang war erfolgreich|
|400|BadRequest|
|401|Nicht autorisiert|
|403|Verboten|
|500|Interner Serverfehler|
|Standard|Vorgang ist fehlgeschlagen.|


### <a name="get-manager"></a>Manager erhalten 
Ruft den Benutzerprofil-Manager für den angegebenen Benutzer ab.  
```GET: /users/{userId}/manager``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Benutzer-ID|Zeichenfolge|Ja|Pfad|keine|User principal Name oder e-Mail-id|

#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|200|Vorgang war erfolgreich|
|202|Vorgang war erfolgreich|
|400|BadRequest|
|401|Nicht autorisiert|
|403|Verboten|
|500|Interner Serverfehler|
|Standard|Vorgang ist fehlgeschlagen.|



### <a name="get-direct-reports"></a>Direkte Berichte 
Erhalten Sie Mitarbeiter.  
```GET: /users/{userId}/directReports``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Benutzer-ID|Zeichenfolge|Ja|Pfad|keine|User principal Name oder e-Mail-id|

#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|200|Vorgang war erfolgreich|
|202|Vorgang war erfolgreich|
|400|BadRequest|
|401|Nicht autorisiert|
|403|Verboten|
|500|Interner Serverfehler|
|Standard|Vorgang ist fehlgeschlagen.|



### <a name="search-for-users"></a>Nach Benutzern suchen 
Ruft die Suchergebnisse von Benutzerprofilen.  
```GET: /users``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Suchbegriff|Zeichenfolge|Nein|Abfrage|keine|Suchzeichenfolge (betrifft: Name Vorname Nachname, e-Mail, e-Mail-Spitznamen und Benutzer Benutzerprinzipalnamen anzeigen)|

#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|200|Vorgang war erfolgreich|
|202|Vorgang war erfolgreich|
|400|BadRequest|
|401|Nicht autorisiert|
|403|Verboten|
|500|Interner Serverfehler|
|Standard|Vorgang ist fehlgeschlagen.|



## <a name="object-definitions"></a>Objektdefinitionen

#### <a name="user-user-model-class"></a>Benutzer: Benutzer Modellklasse

|Eigenschaftenname | Datentyp |Erforderlich
|---|---|---|
|DisplayName|Zeichenfolge|Nein|
|Vorname|Zeichenfolge|Nein|
|Nachname|Zeichenfolge|Nein|
|E-Mail-Nachrichten|Zeichenfolge|Nein|
|MailNickname|Zeichenfolge|Nein|
|TelephoneNumber|Zeichenfolge|Nein|
|AccountEnabled|boolescher Wert|Nein|
|ID|Zeichenfolge|Ja
|UserPrincipalName|Zeichenfolge|Nein|
|Abteilung|Zeichenfolge|Nein|
|JobTitle|Zeichenfolge|Nein|
|Kontaktdetails|Zeichenfolge|Nein|


## <a name="next-steps"></a>Nächste Schritte

[Erstellen einer Anwendung Logik](../app-service-logic/app-service-logic-create-a-logic-app.md).

Zurück zur [Liste der APIs](apis-list.md).

<!--References-->
[5]: https://portal.azure.com
[7]: ./media/connectors-create-api-office365-users/aad-tenant-applications.PNG
[8]: ./media/connectors-create-api-office365-users/aad-tenant-applications-add-appinfo.PNG
[9]: ./media/connectors-create-api-office365-users/aad-tenant-applications-add-app-properties.PNG
[10]: ./media/connectors-create-api-office365-users/contoso-aad-app.PNG
[11]: ./media/connectors-create-api-office365-users/contoso-aad-app-configure.PNG
