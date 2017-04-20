<properties
pageTitle="SMTP | Microsoft Azure"
description="Erstellen von Logik apps mit Azure App. Verbinden Sie mit SMTP e-Mail."
services="logic-apps"   
documentationCenter=".net,nodejs,java"  
authors="msftman"   
manager="erikre"    
editor=""
tags="connectors" />

<tags
ms.service="app-service-logic"
ms.devlang="multiple"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="integration"
ms.date="07/15/2016"
ms.author="deonhe"/>

# <a name="get-started-with-the-smtp-connector"></a>Erste Schritte mit der SMTP-connector

Verbinden Sie mit SMTP e-Mail.

Um [Alle Connector](./apis-list.md)verwenden, müssen Sie eine Logik-app erstellen. Sie erstellen [jetzt eine Anwendung Logik](../app-service-logic/app-service-logic-create-a-logic-app.md)beginnen.

## <a name="connect-to-smtp"></a>SMTP-Verbindung

Bevor Ihre Anwendung Logik einen Dienst zugreifen kann, müssen Sie zuerst eine *Verbindung* mit dem Dienst. Eine [Verbindung](./connectors-overview.md) stellt eine Verbindung zwischen einer Anwendung Logik und ein weiterer. Beispielsweise benötigen SMTP herstellen Sie zuerst eine SMTP- *Verbindung*. Zum Erstellen einer Verbindung müssen Sie die Anmeldeinformationen, die Sie normalerweise verwenden, den Zugriff auf Dienste, die Sie herstellen möchten. So würde im Beispiel SMTP Anmeldeinformationen Verbindungsname, SMTP-Serveradresse und Anmeldung benötigen Sie um SMTP-Verbindung zu erstellen. [Weitere Informationen zu Verbindungen]()  

### <a name="create-a-connection-to-smtp"></a>Erstellen Sie eine SMTP-Verbindung

>[AZURE.INCLUDE [Steps to create a connection to SMTP](../../includes/connectors-create-api-smtp.md)]

## <a name="use-an-smtp-trigger"></a>Verwenden Sie einen SMTP-trigger

Ein Trigger ist ein Ereignis, das zum Starten des Workflows definiert eine Logik-App verwendet werden kann. [Weitere Informationen zu Triggern](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).

In diesem Beispiel da SMTP keinen Trigger der verwenden Trigger **Salesforce - beim Erstellen eines Objekts** wir. Dieser Trigger wird aktiviert, wenn ein neues Objekt in Salesforce erstellt wird. In unserem Beispiel es eingerichtet, jedes Mal ein neuer Lead in Salesforce erstellt wird, eine *e-Mail* über den SMTP-Connector mit einem neuen Lead erstellt geschieht.

1. *Salesforce* Designer apps Logik in das Suchfeld eingeben und wählen Sie dann den Trigger **Salesforce - beim Erstellen eines Objekts** .  
 ![](../../includes/media/connectors-create-api-salesforce/trigger-1.png)  

2. Das Steuerelement **beim Erstellen eines Objekts** wird angezeigt.
 ![](../../includes/media/connectors-create-api-salesforce/trigger-2.png)  

3. Wählen Sie den **Objekttyp** , und wählen Sie aus der Liste der Objekte *führen* . In diesem Schritt bestätigen Sie, dass Sie einen Trigger erstellen, der Logik-app benachrichtigt, wenn neu in Salesforce erstellt wird.  
 ![](../../includes/media/connectors-create-api-salesforce/trigger3.png)  

4. Der Trigger wurde erstellt.  
 ![](../../includes/media/connectors-create-api-salesforce/trigger-4.png)  

## <a name="use-an-smtp-action"></a>Verwenden Sie eine SMTP-Aktion

Eine Aktion ist eine Operation in eine Logik-app definierten Workflow durchgeführt. [Weitere Informationen zu Aktionen](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).

Der Trigger hinzugefügt wurde, folgendermaßen Sie vor, um eine SMTP-Aktion hinzufügen, die beim Erstellen ein neues Leads in Salesforce stattfindet.

1. Wählen Sie **+ Neuer Schritt** Aktion hinzufügen, die beim Erstellen ein neues Leads nutzen möchten.  
 ![](../../includes/media/connectors-create-api-salesforce/trigger4.png)  

2. Wählen Sie **eine Aktion hinzufügen**. Dieses Öffnet das Suchfeld, wobei Sie für jede Aktion suchen können, möchten.  
 ![](../../includes/media/connectors-create-api-smtp/using-smtp-action-2.png)  

3. Geben Sie *smtp* für Aktionen im Zusammenhang mit SMTP suchen.  

4. Wählen Sie die durchzuführende Aktion beim Erstellen der neue Lead **SMTP - E-Mail senden** . Aktion-Kontrollblock wird geöffnet. Sie müssen die SMTP-Verbindung im Designer Block einrichten, wenn Sie nicht bereits getan haben.  
 ![](../../includes/media/connectors-create-api-smtp/smtp-2.png)    

5. Geben Sie die gewünschte e-Mail-Informationen im Block **SMTP - E-Mail senden** .  
 ![](../../includes/media/connectors-create-api-smtp/using-smtp-action-4.PNG)  

6. Speichern Sie Ihre Arbeit, um den Workflow zu aktivieren.  

## <a name="technical-details"></a>Technische details

Hier werden Einzelheiten über Trigger, Aktionen und Reaktionen, die diese Verbindung unterstützt:

## <a name="smtp-triggers"></a>SMTP-Trigger

SMTP hat keine Trigger. 

## <a name="smtp-actions"></a>SMTP-Aktionen

SMTP hat folgende Maßnahmen:


|Aktion|Beschreibung|
|--- | ---|
|[E-Mail senden](connectors-create-api-smtp.md#send-email)|Dieser Vorgang sendet eine e-Mail an einen oder mehrere Empfänger.|

### <a name="action-details"></a>Aktionsdetails

Hier werden Details zur Aktivität des Konnektors mit Antworten:


### <a name="send-email"></a>E-Mail senden
Dieser Vorgang sendet eine e-Mail an einen oder mehrere Empfänger. 


|Eigenschaftenname| Angezeigter Name|Beschreibung|
| ---|---|---|
|An|An|Adressen Sie e-Mail-getrennt durch Semikolons wierecipient1@domain.com;recipient2@domain.com|
|CC|cc|Adressen Sie e-Mail-getrennt durch Semikolons wierecipient1@domain.com;recipient2@domain.com|
|Betreff|Betreff|Betreff|
|Körper|Körper|E-Mail-Text|
|Von|Von|E-Mail-Adresse des Absenders wiesender@domain.com|
|IsHtml|HTML|Die e-Mail wird als HTML (True/False)|
|Bcc|Bcc|Adressen Sie e-Mail-getrennt durch Semikolons wierecipient1@domain.com;recipient2@domain.com|
|Bedeutung|Bedeutung|Bedeutung von e-Mails (hoch, Normal oder niedrig)|
|ContentData|Daten anhängen|Content-Daten (für Streams und als base64-codierte-Zeichenfolge ist)|
|ContentType|Anlagen-Inhaltstyp|Inhaltstyp|
|ContentTransferEncoding|Anlagen Content Transfer-Codierung|Content-Transfer-Encoding (base64 oder keine)|
|Dateiname|Dateinamen von Anlagen|Dateiname|
|ContentId|Anlagen Inhalts-ID|Inhalts-id|

Ein * zeigt an, dass eine Eigenschaft


## <a name="http-responses"></a>HTTP-Antworten

Aktionen und Auslöser oben können eine oder mehrere der folgenden HTTP-Statuscodes zurück: 

|Name|Beschreibung|
|---|---|
|200|Okay|
|202|Akzeptiert|
|400|Ungültige Anforderung|
|401|Nicht autorisiert|
|403|Verboten|
|404|Nicht gefunden|
|500|Interner Serverfehler. Es ist ein Fehler aufgetreten.|
|Standard|Vorgang ist fehlgeschlagen.|

## <a name="next-steps"></a>Nächste Schritte
[Erstellen einer Logik](../app-service-logic/app-service-logic-create-a-logic-app.md)