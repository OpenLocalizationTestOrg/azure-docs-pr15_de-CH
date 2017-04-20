<properties 
    pageTitle="Übersicht über messaging Service Bus Beispiele | Microsoft Azure"
    description="Kategorisiert und Service Bus mit Links zu messaging beschreibt."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/07/2016"
    ms.author="sethm" />

# <a name="service-bus-messaging-samples"></a>Messaging Service Bus-Beispiele

Service Bus messaging Beispiele veranschaulichen Schlüsselfunktionen [messaging Service Bus](https://azure.microsoft.com/services/service-bus/) (Cloud-Dienst) und [Service Bus für Windows Server](https://msdn.microsoft.com/library/dn282144.aspx). In diesem Artikel kategorisiert und mit Links zu Beispielen beschrieben.

>[AZURE.NOTE] Service Bus Beispiele sind nicht mit dem SDK installiert. Beispiele finden Sie auf der [Seite Azure SDK-Beispielen](https://code.msdn.microsoft.com/site/search?query=service%20bus&f%5B0%5D.Value=service%20bus&f%5B0%5D.Type=SearchText&ac=5).
>
>Zusätzlich gibt es ein aktualisierter Satz von Service Bus messaging Samples [hier](https://github.com/Azure-Samples/azure-servicebus-messaging-samples) (dieses Schreibens, sie sind nicht in diesem Artikel beschriebenen).  

Relay-Beispiele finden Sie unter [Service Bus Relay-Beispiele](../service-bus-relay/service-bus-relay-samples.md).

## <a name="service-bus-messaging"></a>Service Bus messaging

Die folgenden Beispiele veranschaulichen, wie schreiben, mit dem messaging Service Bus.

Beachten Sie, dass das messaging Samples eine Verbindungszeichenfolge für den Service Bus-Namespace zugreifen.

### <a name="to-obtain-a-connection-string-for-azure-service-bus"></a>Azure Service Bus zu einer Verbindungszeichenfolge

1. Melden Sie sich bei [Azure-Portal](http://portal.azure.com).

1. Klicken Sie in der linken Spalte auf **Service Bus**.

1. Klicken Sie auf den Namen des Namespaces in der Liste.

3. Klicken Sie auf **freigegebene Zugriffsrichtlinien**Blatt Namespace.

4. **Freigegebene Richtlinien** Blatt klicken Sie auf **RootManageSharedAccessKey**.

6. Kopieren Sie die Verbindungszeichenfolge in die Zwischenablage.

### <a name="to-obtain-a-connection-string-for-service-bus-for-windows-server"></a>Eine Verbindungszeichenfolge für Service Bus für Windows Server erhalten

1. Führen Sie das folgende PowerShell-Cmdlet ein:

    ```
    get-sbClientConfiguration
    ```

2. Fügen Sie die Verbindungszeichenfolge in der Datei App.config für das Beispiel.

### <a name="getting-started-samples"></a>Erste Schritte Beispiele

Diese Beispiele beschreiben grundlegende Messagingfunktionen.

|Name des Beispiels|Beschreibung|Minimale SDK-Version|Verfügbarkeit|
|---|---|---|---|
|[Erste Schritte: Messaging mit Warteschlangen](http://code.msdn.microsoft.com/Getting-Started-Brokered-aa7a0ac3)|Veranschaulicht, wie mit Microsoft Azure Service Bus senden und Empfangen von Nachrichten aus einer Warteschlange.|1.8|Microsoft Azure Servicebus; Servicebus für WindowsServer|
|[Erste Schritte: Messaging mit Themen](http://code.msdn.microsoft.com/Getting-Started-Brokered-614d42e5)|Veranschaulicht, wie mit Microsoft Azure Service Bus senden und Empfangen von Nachrichten über ein Thema mit mehreren Abonnements.|1.8|Microsoft Azure Servicebus; Servicebus für WindowsServer|

### <a name="exploring-features"></a>Entdecken Sie features

Die folgenden Beispiele veranschaulichen verschiedene Features von Service Bus.

|Name des Beispiels|Beschreibung|Minimale SDK-Version|Verfügbarkeit|
|---|---|---|---|
|[HTTP-Token-Anbieter](http://code.msdn.microsoft.com/Service-Bus-HTTP-Token-38f2cfc5)|Beschreibt verschiedene Verfahren für die Authentifizierung von eines HTTP-REST-Clients mit Service Bus.|2.1|Microsoft Azure Servicebus; Servicebus für WindowsServer|
|[Service Bus HTTP-Client](http://code.msdn.microsoft.com/Service-Bus-HTTP-client-fe7da74a)|Veranschaulicht das Senden und Empfangen von Nachrichten von Service Bus über HTTP-REST.|2.3|Microsoft Azure Servicebus; Servicebus für WindowsServer|
|[Automatische Weiterleitung Service Bus](http://code.msdn.microsoft.com/Service-Bus-Autoforwarding-b9df470b)|Veranschaulicht, wie Nachrichten aus einer Warteschlange, Abonnement oder Warteschlange für unzustellbare Nachrichten automatisch in eine andere Warteschlange oder Thema weiterzuleiten. Es wird veranschaulicht, wie eine Nachricht in eine Warteschlange oder ein Thema über eine Übertragungswarteschlange.|2.3|Microsoft Azure Servicebus; Servicebus für WindowsServer|
|[Vermittelten Messaging: Beispiel für einen WCF-Kanal Sitzung](http://code.msdn.microsoft.com/Brokered-Messaging-WCF-0a526451)|Veranschaulicht, wie Microsoft Azure Service Bus mit Windows Communication Foundation (WCF)-Kanäle. Das Beispiel zeigt die Verwendung von WCF Kanäle senden und Empfangen von Nachrichten über ein Service Bus-Warteschlange. Das Beispiel zeigt die Sitzung und nicht Sitzung Kommunikation über Service Bus.|1.8|Microsoft Azure Servicebus; Servicebus für WindowsServer|
|[Vermittelte Messaging: Transaktionen](http://code.msdn.microsoft.com/Brokered-Messaging-8cd41d1e)|Veranschaulicht, wie Microsoft Azure Service Bus messaging-Funktionen innerhalb eines Transaktionsbereichs um sicherzustellen, dass Chargen-Nachrichtenvorgänge sind atomar.|1.8|Microsoft Azure Servicebus; Servicebus für WindowsServer|
|[Vermittelte Messaging: Verwaltungsvorgänge Verwendung von REST](http://code.msdn.microsoft.com/Brokered-Messaging-569cff88)|Veranschaulicht das Management Service Bus mit anderen Operationen.|1.8|Microsoft Azure Servicebus; Servicebus für WindowsServer|
|[Ressourcenanbieter REST-APIs](http://code.msdn.microsoft.com/Service-Bus-Resource-5d887203)|Veranschaulicht, wie die neuen Service Bus RDFE REST APIs Namespaces und messaging Entitäten verwalten.|1.8|Microsoft Azure Servicebus; Servicebus für WindowsServer|
|[Vermittelte Messaging: WCF Sitzung Beispiel](http://code.msdn.microsoft.com/Brokered-Messaging-WCF-db4262c2)|Veranschaulicht, wie Microsoft Azure Service Bus mit WCF Service-Modell. Im Beispiel wird die Verwendung der WCF Service-Modell zu Sitzung Kommunikation über Service Bus-Warteschlange.|1.8|Microsoft Azure Servicebus; Servicebus für WindowsServer|
|[Vermittelten Messaging: Anforderungsantwort](http://code.msdn.microsoft.com/Brokered-Messaging-Request-2b4ff5d8)|Veranschaulicht, wie Microsoft Azure Service Bus und die Anforderung/Antwort. Das Beispiel veranschaulicht einfache Clients und Server kommunizieren über Service Bus-Warteschlange.|1.8|Microsoft Azure Servicebus; Servicebus für WindowsServer|
|[Vermittelten Messaging: Warteschlange für unzustellbare](http://code.msdn.microsoft.com/Brokered-Messaging-Dead-22536dd8)|Veranschaulicht, wie Microsoft Azure Service Bus und "Dead Letter-Warteschlange" Messagingfunktionen. Das Beispiel zeigt eine einfache Absender und Empfänger über Service Bus-Warteschlange.|1.8|Microsoft Azure Servicebus; Servicebus für WindowsServer|
|[Vermittelten Messaging: Zurückgestellte Nachrichten](http://code.msdn.microsoft.com/Brokered-Messaging-ccc4f879)|Veranschaulicht, wie die Nachricht Rückstellung-Funktion von Microsoft Azure Service Bus. Das Beispiel zeigt eine einfache Absender und Empfänger über Service Bus-Warteschlange.|1.8|Microsoft Azure Servicebus; Servicebus für WindowsServer|
|[Vermittelten Messaging: Sitzung Nachrichten](http://code.msdn.microsoft.com/Brokered-Messaging-Session-41c43fb4)|Veranschaulicht, wie Microsoft Azure Service Bus und die Messaging-Sitzung. Das Beispiel veranschaulicht einfache Absender und Empfänger über Service Bus-Warteschlange.|1.8|Microsoft Azure Servicebus; Servicebus für WindowsServer|
|[Vermittelte Messaging: Anforderung Antwort Thema](http://code.msdn.microsoft.com/Brokered-Messaging-Request-6759a36e)|Veranschaulicht, wie mit Microsoft Azure Service Bus Topics und Abonnements Anforderung/Antwort-Muster. Das Beispiel veranschaulicht einfache Clients und Server über ein Service Bus-Thema.|1.8|Microsoft Azure Servicebus; Servicebus für WindowsServer|
|[Vermittelten Messaging: Anforderung Antwortwarteschlange](http://code.msdn.microsoft.com/Brokered-Messaging-Request-0ce8fcaf)|Veranschaulicht, wie Microsoft Azure Service Bus und die Anforderung/Antwort. Das Beispiel veranschaulicht einfache Clients und Servern über zwei Servicebuswarteschlangen.|1.8|Microsoft Azure Servicebus; Servicebus für WindowsServer|
|[Vermittelten Messaging: Duplikaterkennungsregeln](http://code.msdn.microsoft.com/Brokered-Messaging-c0acea25)|Veranschaulicht, wie Erkennung doppelter Nachrichten Microsoft Azure Service Bus mit Warteschlangen. Es erstellt zwei Warteschlangen mit doppelten aktiviert und andere ohne Duplikaterkennungsregeln.|1.8|Microsoft Azure Servicebus; Servicebus für WindowsServer|
|[Vermittelten Messaging: Asynchronen Messaging](http://code.msdn.microsoft.com/Brokered-Messaging-Async-211c1e74)|Veranschaulicht, wie mit Microsoft Azure Service Bus senden und Empfangen von Nachrichten aus einer Warteschlange asynchron. Die Warteschlange enthält entkoppelte, asynchrone Kommunikation zwischen einem Absender und eine beliebige Anzahl von Empfängern (hier einzelne Empfänger).|1.8|Microsoft Azure Servicebus; Servicebus für WindowsServer|
|[Vermittelte Messaging: Erweiterte Filter](http://code.msdn.microsoft.com/Brokered-Messaging-6b0d2749)|Veranschaulicht, wie Microsoft Azure Service Bus Veröffentlichen/Abonnieren erweiterte Filter. Es erstellt ein Thema und 3 Abonnements mit verschiedenen Filterdefinitionen sendet Nachrichten zum Thema und Abonnements alle Nachrichten empfängt.|1.8|Microsoft Azure Servicebus; Servicebus für WindowsServer|
|[Vermittelten Messaging: Nachrichten Prefetch](http://code.msdn.microsoft.com/Brokered-Messaging-be2dac1d)|Veranschaulicht, wie das Microsoft Azure Service Bus Nachrichten Prefetch-Feature. Es veranschaulicht, wie Nachrichten Prefetch-Feature auf empfangen.|1.8|Microsoft Azure Servicebus; Servicebus für WindowsServer|

## <a name="service-bus-reference-tools"></a>Service Bus Referenztools

Die folgenden Beispiele veranschaulichen verschiedene Features des Diensts.

|Name des Beispiels|Beschreibung|Minimale SDK-Version|Verfügbarkeit|
|---|---|---|---|
|[Service Bus-Explorer](http://code.msdn.microsoft.com/Service-Bus-Explorer-f2abca5a)|Service Bus-Explorer ermöglicht einen Service Bus Service Namespace und messaging Entitäten auf einfache Weise verwalten. Das Tool bietet erweiterte Funktionen wie Import-/Exportfunktion und Messaging-Entitäten und -Relay-Dienste testen.|1.8|Microsoft Azure Servicebus; Servicebus für WindowsServer|
|[Autorisierung: SBAzTool](http://code.msdn.microsoft.com/Authorization-SBAzTool-6fd76d93)|Dieses Beispiel veranschaulicht, wie erstellen und Verwalten von Dienstidentitäten Microsoft Azure Active Directory Access Control (auch bekannt als Access Control Service oder ACS) mit Service Bus.|N/A|Microsoft Azure Servicebus|

## <a name="next-steps"></a>Nächste Schritte

Finden Sie unter den folgenden Themen Übersichten über Service Bus.

- [Übersicht über Service Bus](service-bus-messaging-overview.md)
- [Service-Architektur](service-bus-architecture.md)
- [Service Bus-Grundlagen](service-bus-fundamentals-hybrid-solutions.md)
