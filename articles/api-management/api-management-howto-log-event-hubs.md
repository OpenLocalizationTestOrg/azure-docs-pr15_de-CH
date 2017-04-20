<properties 
    pageTitle="Ereignisse Azure Ereignis Hubs in Azure API Management anmelden | Microsoft Azure" 
    description="Informationen Sie zum Protokollieren von Ereignissen zu Azure Ereignis Hubs in Azure API Management." 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="how-to-log-events-to-azure-event-hubs-in-azure-api-management"></a>Ereignisse Azure Ereignis Hubs in Azure API Management anmelden

Azure Event Hubs ist hochskalierbare eindringen, die damit verarbeitet und große Mengen von Daten der angeschlossenen Geräte und Applikationen analysiert Millionen Ereignisse pro Sekunde aufnehmen können. Ereignis-Hubs fungiert als "Vordertür" für ein Ereignispipeline und nachdem ein Ereignis Hub Daten transformiert werden können und mit jedem Anbieter Echtzeitanalysen oder Batchverarbeitung/Speicheradapter gespeichert. Event Hubs entkoppelt die Produktion eines Streams von Ereignissen aus der Ereignisse, sodass Ereignisconsumer Ereignisse nach ihrem eigenen Zeitplan zugreifen können.

Dieser Artikel ist eine Ergänzung zur [Integration von Azure API Management mit Ereignis](https://azure.microsoft.com/documentation/videos/integrate-azure-api-management-with-event-hubs/) -Video und beschreibt die API Management Ereignisse mit Azure Ereignis Hubs.

## <a name="create-an-azure-event-hub"></a>Einen Hub Azure Ereignis erstellen

Erstellen Sie ein neues Ereignis Hub [klassischen Azure-Portal](https://manage.windowsazure.com) anmelden und auf **neu**->**App Services**->**Service Bus**->**Event Hub**->**Schnell erstellen**. Geben Sie einen ein Ereignis Hub Region, wählen Sie ein Abonnement und einen Namespace. Wenn Sie zuvor einen Namespace erstellt haben können Sie ein Geben Sie einen Namen in das Textfeld **Namespace** erstellen. Sobald alle Eigenschaften konfiguriert sind, klicken Sie auf **erstellen neue Ereignis-Hub** Hub Ereignis erstellen.

![Ereignis-Hub erstellen][create-event-hub]

Dann navigieren, die Registerkarte **Konfigurieren** der neue Ereignis-Hub und zwei **freigegebene Zugriffsrichtlinien**erstellen. Benennen Sie die erste **Senden** und ihm Berechtigung **Senden** .

![Senden von Richtlinien][sending-policy]

Name der zweite **empfangen**, gewähren sie **hören** und klicken Sie auf **Speichern**.

![Empfängt eine Richtlinie][receiving-policy]

Jede freigegebene Richtlinie kann Anwendung senden und Empfangen von Ereignissen vom Event Hub und. Um die Verbindungszeichenfolgen für diese Richtlinien zuzugreifen, navigieren Sie zur Registerkarte **Dashboard** des Ereignisses und **Verbindungsinformationen**auf.

![Verbindungszeichenfolge][event-hub-dashboard]

Verbindungszeichenfolge **Senden** Dient beim Protokollieren von Ereignissen und die Verbindungszeichenfolge **empfangen** wird verwendet, wenn Ereignisse vom Event Hub herunterladen.

![Verbindungszeichenfolge][event-hub-connection-string]

## <a name="create-an-api-management-logger"></a>Erstellen einer API Management-Protokollierung

Jetzt haben Sie einen Ereignis-Hub, besteht der nächste Schritt [Protokollierung](https://msdn.microsoft.com/library/azure/mt592020.aspx) in Ihrem API Management-Dienst so konfigurieren, dass sie Ereignisse an den Hub Ereignis anmelden kann.

API Management Protokollierungsmodule sind mit der [API Management REST API](http://aka.ms/smapi)konfiguriert. Überprüfen Sie bevor die REST-API zum ersten Mal verwenden die [erforderlichen Komponenten](https://msdn.microsoft.com/library/azure/dn776326.aspx#Prerequisites) und sicherzustellen Sie, dass Sie [Zugriff auf die REST-API](https://msdn.microsoft.com/library/azure/dn776326.aspx#EnableRESTAPI).

Erstellen Sie eine Protokollierung stellen Sie eine HTTP PUT-Anforderung mit der folgenden URL-Vorlage.

    https://{your service}.management.azure-api.net/loggers/{new logger name}?api-version=2014-02-14-preview

-   Ersetzen Sie `{your service}` mit dem Namen Ihrer API Management Service-Instanz.
-   Ersetzen Sie `{new logger name}` mit dem gewünschten Namen für Ihre neue Protokollierung. Sie werden diesen Namen zu verweisen beim Konfigurieren der Richtlinie für [Protokoll eventhub](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub)

Die folgenden Header der Anforderung hinzufügen.

-   Content-Typ: Application/json
-   Autorisierung: SharedAccessSignature Uid =...
    -   Informationen zum Generieren der `SharedAccessSignature` finden Sie unter [Azure API Management REST API-Authentifizierung](https://msdn.microsoft.com/library/azure/dn798668.aspx).

Geben Sie mit der folgenden Vorlage Anforderungstext.

    {
      "type" : "AzureEventHub",
      "description" : "Sample logger description",
      "credentials" : {
        "name" : "Name of the Event Hub from the Azure Classic Portal",
        "connectionString" : "Endpoint=Event Hub Sender connection string"
        }
    }

-   `type`muss `AzureEventHub`.
-   `description`enthält eine Beschreibung der Protokollierung und kann bei Bedarf eine Zeichenfolge der Länge 0 (null).
-   `credentials`enthält die `name` und `connectionString` von Azure Event Hub.

Wenn Sie die Anforderung vornehmen, wird die Protokollierung Statuscode erstellt `201 Created` zurückgegeben. 

>[AZURE.NOTE] Andere mögliche Rückgabecodes und ihrer Gründe finden Sie unter [Erstellen einer Protokollierung](https://msdn.microsoft.com/library/azure/mt592020.aspx#PUT). Dazu führen Sie andere Vorgänge wie z. B. Liste, Update und Delete, die [Protokollierung](https://msdn.microsoft.com/library/azure/mt592020.aspx) Entität Dokumentation finden Sie unter.

## <a name="configure-log-to-eventhubs-policies"></a>Protokoll Eventhubs Richtlinien konfigurieren

Nachdem die Protokollierung in API Management konfiguriert wurde, können Sie Ihr Protokoll Eventhubs Richtlinien die gewünschten Ereignisse konfigurieren. Protokoll Eventhubs Richtlinie kann Richtlinienabschnitt eingehende oder ausgehende Richtlinienabschnitt verwendet werden.

Konfigurieren von Richtlinien zum [klassischen Azure-Portal](https://manage.windowsazure.com)anmelden navigieren Sie API Management-Dienst und auf **Publisher-Portal** oder **Verwalten** , das Publisher-Portal zugreifen.

![Publisher-portal][publisher-portal]

Klicken Sie auf **Richtlinien** in der API-Menü auf der linken Seite, wählen Sie das gewünschte Produkt und die API und klicken Sie auf **Richtlinie hinzufügen**. In diesem Beispiel fügen wir eine **Echo-API** im Produkt **unbegrenzt** .

![Richtlinie hinzufügen][add-policy]

Positionieren Sie den Cursor in die `inbound` Richtlinie Abschnitt, und klicken Sie auf **Protokoll EventHub** Richtlinie Einfügen der `log-to-eventhub` Anweisung Vorlage.

![Gruppenrichtlinien-editor][event-hub-policy]

    <log-to-eventhub logger-id ='logger-id'>
      @( string.Join(",", DateTime.UtcNow, context.Deployment.ServiceName, context.RequestId, context.Request.IpAddress, context.Operation.Name))
    </log-to-eventhub>

Ersetzen Sie `logger-id` mit dem Namen der im vorherigen Schritt konfigurierten API Management-Protokollierung.

Können Sie einen beliebigen Ausdruck, der eine Zeichenfolge als Wert für die zurückgibt die `log-to-eventhub` Element. In diesem Beispiel wird eine Zeichenfolge mit Datum und Uhrzeit, Namen, Kennung, Anforderung IP-Adresse und Name des Vorgangs protokolliert.

Klicken Sie auf **Speichern** , um die aktuelle Konfiguration speichern. Als speichern die Richtlinie ist aktiv und werden Ereignisse festgelegten Ereignis-Hub.

## <a name="next-steps"></a>Nächste Schritte

-   Erfahren Sie mehr über Azure Ereignis Hubs
    -   [Erste Schritte mit Azure Ereignis Hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)
    -   [Nachrichten mit EventProcessorHost](../event-hubs/event-hubs-csharp-ephcs-getstarted.md#receive-messages-with-eventprocessorhost)
    -   [Ereignis-Hubs Programmierhandbuch](../event-hubs/event-hubs-programming-guide.md)
-   Weitere Informationen zu API-Management und Event Hubs integration
    -   [Entitätsverweis Protokollierung](https://msdn.microsoft.com/library/azure/mt592020.aspx)
    -   [Protokoll Eventhub Richtlinie Bezug](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub)
    -   [Überwachen der APIs Azure API Management Event Hubs und Runscope](api-management-log-to-eventhub-sample.md)    

## <a name="watch-a-video-walkthrough"></a>Sehen Sie sich video-Anleitung

> [AZURE.VIDEO integrate-azure-api-management-with-event-hubs]


[publisher-portal]: ./media/api-management-howto-log-event-hubs/publisher-portal.png
[create-event-hub]: ./media/api-management-howto-log-event-hubs/create-event-hub.png
[event-hub-connection-string]: ./media/api-management-howto-log-event-hubs/event-hub-connection-string.png
[event-hub-dashboard]: ./media/api-management-howto-log-event-hubs/event-hub-dashboard.png
[receiving-policy]: ./media/api-management-howto-log-event-hubs/receiving-policy.png
[sending-policy]: ./media/api-management-howto-log-event-hubs/sending-policy.png
[event-hub-policy]: ./media/api-management-howto-log-event-hubs/event-hub-policy.png
[add-policy]: ./media/api-management-howto-log-event-hubs/add-policy.png






