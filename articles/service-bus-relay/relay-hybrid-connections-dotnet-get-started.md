<properties
    pageTitle="Einstieg mit Relay Hybrid | Microsoft Azure"
    description="Schreiben Sie ein C#-Konsolenanwendungsprojekt für Hybrid"
    services="service-bus"
    documentationCenter=".net"
    authors="jtaubensee"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="tbd"
    ms.topic="hero-article"
    ms.tgt_pltfrm="dotnet"
    ms.workload="na"
    ms.date="10/28/2016"
    ms.author="jotaub"/>

# <a name="get-started-with-relay-hybrid-connections"></a>Erste Schritte mit Relay Hybrid

[AZURE.INCLUDE [relay-selector-hybrid-connections](../../includes/relay-selector-hybrid-connections.md)]

## <a name="what-will-be-accomplished"></a>Was erfolgt

Da Hybridverbindungen ein Client und ein Server-Komponente erfordert, wird in diesem Lernprogramm gegenseitige Konsole erstellt. Hier sind die Schritte:

1. Erstellen Sie einen Namespace Relay der Azure-Portal.

2. Erstellen einer Verbindung Hybrid der Azure-Portal.

3. Schreiben eines Servers Konsolenanwendungsprojekt Nachrichten empfangen.

4. Schreiben Sie einen Client Konsolenanwendungsprojekt Nachrichten senden.

## <a name="prerequisites"></a>Erforderliche Komponenten

1. [Visual Studio 2013 oder Visual Studio 2015](http://www.visualstudio.com). Die Beispiele in diesem Lernprogramm verwenden Sie Visual Studio 2015.

2. Ein Azure-Abonnement.

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="1-create-a-namespace-using-the-azure-portal"></a>1. erstellen Sie 1. einen Namespace mithilfe des Azure-Portals

Haben Sie bereits einen Relay-Namespace erstellt, ruft im Abschnitt [Erstellen einer Hybrid-Verbindung mithilfe von Azure-Portal](#2-create-a-hybrid-connection-using-the-azure-portal) .

[AZURE.INCLUDE [relay-create-namespace-portal](../../includes/relay-create-namespace-portal.md)]

## <a name="2-create-a-hybrid-connection-using-the-azure-portal"></a>2. erstellen Sie eine Hybridverbindung Azure-portal

Haben Sie bereits ein Hybrid-Verbindung erstellt, ruft im Abschnitt [Erstellen einer Server-Anwendung](#3-create-a-server-application-listener) .

[AZURE.INCLUDE [relay-create-hybrid-connection-portal](../../includes/relay-create-hybrid-connection-portal.md)]

## <a name="3-create-a-server-application-listener"></a>3. erstellen Sie eine Anwendung (Listener)

Überwachen von Relay empfangen und Schreiben wir ein C#-Konsolenanwendungsprojekt mit Visual Studio.

[AZURE.INCLUDE [relay-hybrid-connections-dotnet-get-started-server](../../includes/relay-hybrid-connections-dotnet-get-started-server.md)]

## <a name="4-create-a-client-application-sender"></a>4. Erstellen einer Clientanwendung (Absender)

Zum Senden von Nachrichten an das Relay schreiben wir ein C#-Konsolenanwendungsprojekt mit Visual Studio.

[AZURE.INCLUDE [relay-hybrid-connections-dotnet-get-started-client](../../includes/relay-hybrid-connections-dotnet-get-started-client.md)]

## <a name="5-run-the-applications"></a>5. Führen Sie 5. die Anwendung

1. Die Serveranwendung ausführen.

2. Führen Sie die Clientanwendung und geben Sie Text ein.

3. Sicherstellen Sie, dass die Serverkonsole Anwendung Text ausgegeben, der in der Clientanwendung eingegeben wurde.

![Anwendung ausführen](./media/relay-hybrid-connections-dotnet-get-started/running-applications.png)

Herzlichen Glückwunsch, Sie haben eine End-to-End Hybridverbindungen Anwendung.

## <a name="next-steps"></a>Nächste Schritte:

- [Relay – häufig gestellte Fragen](relay-faq.md)
- [Erstellen eines Namespaces](relay-create-namespace-portal.md)
- [Erste Schritte mit Knoten](relay-hybrid-connections-node-get-started.md)