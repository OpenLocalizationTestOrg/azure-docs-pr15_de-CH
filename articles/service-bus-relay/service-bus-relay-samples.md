<properties 
    pageTitle="Service Bus Relay Proben Übersicht | Microsoft Azure"
    description="Kategorisiert und Service Bus Relay Samples Links zu jedem beschrieben."
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

# <a name="service-bus-relay-samples"></a>Service Bus Relay-Beispiele

Service Bus Relay-Beispiele veranschaulichen Schlüsselfunktionen in [Service Bus Relay](https://azure.microsoft.com/services/service-bus/). In diesem Artikel kategorisiert und mit Links zu Beispielen beschrieben.

>[AZURE.NOTE] Service Bus Beispiele sind nicht mit dem SDK installiert. Beispiele finden Sie auf der [Seite Azure SDK-Beispielen](https://code.msdn.microsoft.com/site/search?query=service%20bus&f%5B0%5D.Value=service%20bus&f%5B0%5D.Type=SearchText&ac=5).
>
>Zusätzlich gibt es ein aktualisierter Satz von Service Bus Relay-Beispiele [hier](https://github.com/Azure-Samples/azure-servicebus-relay-samples) (dieses Schreibens, sie sind nicht in diesem Artikel beschriebenen).  

Messaging-Beispiele finden Sie unter [Service Bus messaging Samples](../service-bus-messaging/service-bus-samples.md).

## <a name="service-bus-relay"></a>Service Bus relay

Die folgenden Beispiele veranschaulichen die Anwendung schreiben, die den Service Bus-Dienst verwenden.

Beachten Sie, dass die Relay-Beispiele eine Verbindungszeichenfolge für den Service Bus-Namespace zugreifen.

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

## <a name="service-bus-relay"></a>Service Bus relay

Beispiele für die Relay Service Bus.

### <a name="getting-started"></a>Erste Schritte

|Name des Beispiels|Beschreibung|Minimale SDK-Version|Verfügbarkeit|
|---|---|---|---|
|[Messaging weitergeleitet: Azure](http://code.msdn.microsoft.com/Relayed-Messaging-Windows-0d2cede3)|Veranschaulicht, wie auf Azure Service Bus Client und Dienst ausgeführt. In diesem Beispiel wird programmgesteuert Service Bus konfiguriert. Nur Umgebung und Informationen werden in der Konfigurationsdatei gespeichert.|1.8|Microsoft Azure Servicebus|
|[Weitergeleitete Messaging-Authentifizierung: Gemeinsamer geheimer Schlüssel](http://code.msdn.microsoft.com/Relayed-Messaging-92b04c02)|Veranschaulicht, wie Ausstellername und Aussteller Secret Service Bus authentifizieren.|1.8|Microsoft Azure Servicebus|
|[Messaging-Authentifizierung weitergeleitet: WebNoAuth](http://code.msdn.microsoft.com/Relayed-Messaging-a4f0b831)|Veranschaulicht, wie einen HTTP-Dienst verfügbar machen, der keine Clientauthentifizierung Benutzer erfordert.|1.8|Microsoft Azure Servicebus|
|[Messaging-Bindings weitergeleitet: WebHttpBehavior](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-a6477ba0)|Veranschaulicht, wie die Bindung **WebHttpRelayBinding** im Web programming Model Binärdaten zurückgegeben.|1.8|Microsoft Azure Servicebus|
|[Weitergeleitete Nachrichten Bindungen: NetTcp weitergeleitet](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-2dec7692)|Veranschaulicht, wie die **NetTcpRelayBinding** -Bindung.|1.8|Microsoft Azure Servicebus|

### <a name="exploring-features"></a>Entdecken Sie features

Beispiele für die verschiedenen Service Bus Relay-Funktionen.

|Name des Beispiels|Beschreibung|Minimale SDK-Version|Verfügbarkeit|
|---|---|---|---|
|[Weitergeleitete Messaging-Authentifizierung: Einfache WebToken](http://code.msdn.microsoft.com/Relayed-Messaging-32c74392)|Veranschaulicht, wie eine simple Web token-Anmeldeinformation zur Authentifizierung mit Service Bus. Im Beispiel ähnelt dem Beispiel Echo mit einigen Änderungen. In diesem Beispiel fügt insbesondere Verhalten in der ServiceHost (Dienst) und ChannelFactory (Client).|1.8|Microsoft Azure Servicebus|
|[Weitergeleitete Nachrichten: Lastenausgleich](http://code.msdn.microsoft.com/Relayed-Messaging-Load-bd76a9f8)|Veranschaulicht, wie Microsoft Azure Service Bus zum Weiterleiten von Nachrichten an mehrere Empfänger. Zeigt mehrere Instanzen des einfachen Dienst Kommunikation mit einem Client über die **NetTcpRelayBinding** -Bindung|1.8|Microsoft Azure Servicebus|
|[Weitergeleitete Nachrichten Bindungen: Net-Ereignis](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-c0176977)|Veranschaulicht die Verwendung der **NetEventRelayBinding** Bindung in Microsoft Azure Service Bus.|1.8|Microsoft Azure Servicebus|
|[Weitergeleitete Nachrichten Bindungen: WS2007Http Sitzung](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-ef1f1fcb)|Veranschaulicht die Verwendung von **WS2007HttpRelayBinding** Bindung mit zuverlässigen Sitzung aktiviert. Es veranschaulicht auch Service Bus in der Konfigurationsdatei statt programmgesteuert angeben.|1.8|Microsoft Azure Servicebus|
|[Weitergeleitete Nachrichten Bindungen: WS2007Http MsgSecCertificate](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-f29c9da5)|Veranschaulicht, wie die Bindung **WS2007HttpRelayBinding** mit Nachricht zum Sichern von End-to-End-Nachrichten weiterhin Kunden Service Bus Authentifizierung erfordert.|1.8|Microsoft Azure Servicebus|
|[Weitergeleitete Nachrichten: Metadatenaustausch](http://code.msdn.microsoft.com/Relayed-Messaging-Metadata-f122312e)|Veranschaulicht, wie Metadatenendpunkt verfügbar machen, der die Relay-Bindung verwendet. **MetadataExchange** wird in den folgenden Relay-Bindungen unterstützt: **NetTcpRelayBinding**, **NetOnewayRelayBinding**, **BasicHttpRelayBinding**und **WS2007HttpRelayBinding**.|1.8|Microsoft Azure Servicebus|
|[Weitergeleitete Nachrichten Bindungen: NetTcp Direct](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-ca039161)|Veranschaulicht die Bindung **NetTcpRelayBinding** **hybride** Verbindungsmodus unterstützen, wird zuerst eine weitergeleitete Verbindung, wenn möglich automatisch auf eine direkte Verbindung zwischen einem Client und einem Dienst konfigurieren.|1.8|Microsoft Azure Servicebus|
|[Weitergeleitete Nachrichten Bindungen: NetTcp MsgSec Benutzername](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-30542392)|Veranschaulicht, wie die **NetTcpRelayBinding** -Bindung mit Nachricht.|1.8|Microsoft Azure Servicebus|
|[Weitergeleitete Messaging-Bindung: Net Oneway](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-bb5b813a)|Veranschaulicht das Verfügbarmachen und Nutzen von einem Dienstendpunkt, der **NetOnewayRelayBinding** binden.|1.8|Microsoft Azure Servicebus|
|[Weitergeleitete Nachrichten Bindungen: WS2007Http einfach](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-aa4b793a)|Veranschaulicht die Verwendung der **WS2007HttpRelayBinding** Bindung. Veranschaulicht einen einfachen Dienst, der kein Sicherheitsoptionen verwendet und benötigt keine Clients authentifiziert werden.|1.8|Microsoft Azure Servicebus|

## <a name="next-steps"></a>Nächste Schritte

Finden Sie unter den folgenden Themen Übersichten über Service Bus.

- [Service Bus Relay-Übersicht](service-bus-relay-overview.md)
- [Service-Architektur](../service-bus-messaging/service-bus-architecture.md)
- [Service Bus-Grundlagen](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md)