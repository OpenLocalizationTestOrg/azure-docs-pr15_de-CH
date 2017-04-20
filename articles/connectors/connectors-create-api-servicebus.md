<properties
pageTitle="Lernen Azure Service Bus-Verbindung in Logik-apps | Microsoft Azure"
description="Erstellen von Logik apps mit Azure App. Verbinden Sie mit Azure Service Bus senden und Empfangen von Nachrichten. Sie können Aktionen wie an Warteschlange Thema senden, in Warteschlange empfangen und Abonnement erhalten."
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
ms.date="08/02/2016"
ms.author="deonhe"/>

# <a name="get-started-with-the-azure-service-bus-connector"></a>Erste Schritte mit Azure Service Bus-Anschluss

Verbinden Sie mit Azure Service Bus senden und Empfangen von Nachrichten. Sie können Aktionen wie an Warteschlange Thema senden, in Warteschlange empfangen und Abonnement erhalten.

Um [Alle Connector](./apis-list.md)verwenden, müssen Sie eine Logik-app erstellen. Sie erstellen [jetzt eine Anwendung Logik](../app-service-logic/app-service-logic-create-a-logic-app.md)beginnen.

## <a name="connect-to-service-bus"></a>Verbinden mit Servicebus

Ihre app Logik einen Dienst zugreifen zu kann müssen Sie zuerst eine Verbindung mit dem Dienst erstellen. Eine [Verbindung](./connectors-overview.md) stellt eine Verbindung zwischen einer Anwendung Logik und ein weiterer.  

>[AZURE.INCLUDE [Steps to create a connection to Azure Service Bus](../../includes/connectors-create-api-servicebus.md)]

## <a name="use-a-service-bus-trigger"></a>Verwenden eines Triggers Service Bus

Ein Trigger ist ein Ereignis, das zum Starten des Workflows definiert eine Logik-App verwendet werden kann. [Weitere Informationen zu Triggern](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).  

>[AZURE.INCLUDE [Steps to create a Service Bus trigger](../../includes/connectors-create-api-servicebus-trigger.md)]  

## <a name="use-a-service-bus-action"></a>Service Bus Aktion

Eine Aktion ist eine Operation in eine Logik-app definierten Workflow durchgeführt. [Weitere Informationen zu Aktionen](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).

[AZURE.INCLUDE [Steps to create a Service Bus action](../../includes/connectors-create-api-servicebus-action.md)]  

## <a name="technical-details"></a>Technische details

Hier werden Einzelheiten über Trigger, Aktionen und Reaktionen, die diese Verbindung unterstützt.

### <a name="service-bus-triggers"></a>Service Bus-Trigger

Service Bus hat folgende Trigger:  

|Trigger | Beschreibung|
|--- | ---|
|[Beim Empfang einer Nachricht in einer Warteschlange](connectors-create-api-servicebus.md#when-a-message-is-received-in-a-queue)|Dieser Vorgang löst einen Fluss, wenn eine in einer Warteschlange Nachricht.|
|[Beim Empfang einer Nachricht in einem Thema Abonnement](connectors-create-api-servicebus.md#when-a-message-is-received-in-a-topic-subscription)|Dieser Vorgang löst einen Fluss, wenn eine in einem Thema Abonnement Nachricht.|


### <a name="service-bus-actions"></a>Service Bus-Aktionen

Service Bus enthält Folgendes:


|Aktion|Beschreibung|
|--- | ---|
|[Nachricht senden](connectors-create-api-servicebus.md#send-message)|Dieser Vorgang sendet eine Nachricht an eine Warteschlange oder ein Thema.|
### <a name="action-and-trigger-details"></a>Aktion und die trigger

Hier werden die Details für die Aktionen und Trigger für diesen Connector mit ihrer Antworten.



#### <a name="send-message"></a>Nachricht senden

|Eigenschaftenname| Angezeigter name|Beschreibung|
| ---|---|---|
|ContentData *|Inhalt|Inhalt der Nachricht.|
|ContentType|Inhaltstyp|Inhaltstyp des Nachrichteninhalts.|
|Eigenschaften|Eigenschaften|Schlüssel-Wert-Paare für jeden vermittelte Eigenschaft.|
|Name *|Warteschlange-Thema|Name der Warteschlange oder Thema.|

Diese erweiterten Parameter sind verfügbar:

|Eigenschaftenname| Angezeigter name|Beschreibung|
| ---|---|---|
|MessageId|Nachrichten-Id|Ein benutzerdefinierter Wert, mit dem Service Bus kann zu doppelten Nachrichten aktiviert.|
|An|An|Adresse zu senden.|
|ReplyTo|Antworten|Adresse der Warteschlange beantworten.|
|ReplyToSessionId|Antwort auf Sitzungsbezeichner|Bezeichner der Sitzung beantworten.|
|Bezeichnung|Bezeichnung|Anwendungsspezifische Bezeichnung.|
|ScheduledEnqueueTimeUtc|ScheduledEnqueueTimeUtc|Datum und Uhrzeit in UTC, wenn die Meldung die Warteschlange hinzugefügt werden.|
|SessionId|Sitzung-Id|Bezeichner der Sitzung.|
|CorrelationId|Korrelations-Id|Der Korrelations-ID.|
|TimeToLive|Gültigkeitsdauer|Die Dauer in Teilstrichen, dass eine Nachricht gültig ist. Die Dauer wird von die Meldung Service Bus gesendet wird.|



Ein * zeigt an, dass eine Eigenschaft erforderlich ist.


#### <a name="when-a-message-is-received-in-a-queue"></a>Beim Empfang einer Nachricht in einer Warteschlange

|Eigenschaftenname| Angezeigter name|Beschreibung|
| ---|---|---|
|QueueName *|Name der Warteschlange|Name der Warteschlange.|


Ein * zeigt an, dass eine Eigenschaft erforderlich ist.


##### <a name="output-details"></a>Ausgabedetails

ServiceBusMessage: Dieses Objekt verfügt über den Inhalt und die Eigenschaften einer Nachricht Service Bus.


| Eigenschaftenname | Datentyp | Beschreibung |
|---|---|---|
|ContentData|Zeichenfolge|Inhalt der Nachricht.|
|ContentType|Zeichenfolge|Inhaltstyp des Nachrichteninhalts.|
|Eigenschaften|Objekt|Schlüssel-Wert-Paare für jeden vermittelte Eigenschaft.|
|MessageId|Zeichenfolge|Ein benutzerdefinierter Wert, mit dem Service Bus kann zu doppelten Nachrichten aktiviert.|
|An|Zeichenfolge|An Adresse senden.|
|ReplyTo|Zeichenfolge|Adresse der Warteschlange beantworten.|
|ReplyToSessionId|Zeichenfolge|Bezeichner der Sitzung beantworten.|
|Bezeichnung|Zeichenfolge|Anwendungsspezifische Bezeichnung.|
|ScheduledEnqueueTimeUtc|Zeichenfolge|Datum und Uhrzeit in UTC, wenn die Meldung die Warteschlange hinzugefügt werden.|
|SessionId|Zeichenfolge|Bezeichner der Sitzung.|
|CorrelationId|Zeichenfolge|Der Korrelations-ID.|
|TimeToLive|Zeichenfolge|Die Dauer in Teilstrichen, dass eine Nachricht gültig ist. Die Dauer wird von die Meldung Service Bus gesendet wird.|




#### <a name="when-a-message-is-received-in-a-topic-subscription"></a>Beim Empfang einer Nachricht in einem Thema Abonnement

|Eigenschaftenname| Angezeigter name|Beschreibung|
| ---|---|---|
|Themenname *|Thema|Name des Themas.|
|SubscriptionName *|Themenname Subskription|Name des Abonnements Thema.|


Ein * zeigt an, dass eine Eigenschaft erforderlich ist.


##### <a name="output-details"></a>Ausgabedetails

ServiceBusMessage: Dieses Objekt verfügt über den Inhalt und die Eigenschaften einer Nachricht Service Bus.


| Eigenschaftenname | Datentyp | Beschreibung |
|---|---|---|
|ContentData|Zeichenfolge|Inhalt der Nachricht.|
|ContentType|Zeichenfolge|Inhaltstyp des Nachrichteninhalts.|
|Eigenschaften|Objekt|Schlüssel-Wert-Paare für jeden vermittelte Eigenschaft.|
|MessageId|Zeichenfolge|Ein benutzerdefinierter Wert, mit dem Service Bus kann zu doppelten Nachrichten aktiviert.|
|An|Zeichenfolge|An Adresse senden.|
|ReplyTo|Zeichenfolge|Adresse der Warteschlange beantworten.|
|ReplyToSessionId|Zeichenfolge|Bezeichner der Sitzung beantworten.|
|Bezeichnung|Zeichenfolge|Anwendungsspezifische Bezeichnung.|
|ScheduledEnqueueTimeUtc|Zeichenfolge|Datum und Uhrzeit in UTC, wenn die Meldung die Warteschlange hinzugefügt werden.|
|SessionId|Zeichenfolge|Bezeichner der Sitzung.|
|CorrelationId|Zeichenfolge|Der Korrelations-ID.|
|TimeToLive|Zeichenfolge|Die Dauer in Teilstrichen, dass eine Nachricht gültig ist. Die Dauer wird von die Meldung Service Bus gesendet wird.|



### <a name="http-responses"></a>HTTP-Antworten

Die vorangehenden Aktionen und Trigger können eine oder mehrere der folgenden HTTP-Statuscodes zurück:

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
[Erstellen einer Anwendung Logik](../app-service-logic/app-service-logic-create-a-logic-app.md).
