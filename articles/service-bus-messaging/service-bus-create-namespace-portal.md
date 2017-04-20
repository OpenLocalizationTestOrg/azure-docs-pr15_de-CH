<properties
    pageTitle="Erstellen Sie einen Service Bus-Namespace mit Azure-Portal | Microsoft Azure"
    description="Service Bus Einstieg benötigen Sie einen Namespace. Hier ist mit der Azure-Portal erstellen."
    services="service-bus"
    documentationCenter=".net"
    authors="jtaubensee"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="tbd"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="dotnet"
    ms.workload="na"
    ms.date="08/22/2016"
    ms.author="jotaub"/>

# <a name="create-a-service-bus-namespace-using-the-azure-portal"></a>Erstellen Sie einen Service Bus-Namespace mit Azure-portal

Ein Namespace ist eine allgemeine Container für alle messaging-Komponenten. Mehrere Warteschlangen und Themen können in einem einzelnen Namespace befinden und Namespaces dienen häufig als Anwendungscontainer. Derzeit sind zwei Arten Service Bus-Namespace erstellen.

1.  Azure-Portal (dieser Artikel)

2.  [Ressourcen-Manager-Vorlagen][create-namespace-using-arm]

## <a name="create-a-namespace-in-the-azure-portal"></a>Erstellen Sie einen Namespace in Azure-portal

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

Herzlichen Glückwunsch! Sie haben jetzt einen Service Bus Messaging-Namespace erstellt.

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich unsere [GitHub Proben] [ github-samples] einige der erweiterten Features von Azure Service Bus Messaging anzuzeigen.

[create-namespace-using-arm]: service-bus-resource-manager-overview.md
[github-samples]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples