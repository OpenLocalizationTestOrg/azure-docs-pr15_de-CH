<properties
    pageTitle="Webaufträge in Azure App Service"
    description="Informationen Sie zu Webaufträge Hintergrund Tests ausführen, Interaktion mit Storage Service Bus und geplante Tasks erstellen."
    services="app-service"
    documentationCenter=""
    authors="christopheranderson"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/10/2015"
    ms.author="chrande"/>

# <a name="using-webjobs-in-azure-app-service"></a>Webaufträge verwenden in Azure App Service

Dieser Artikel enthält Links zu Dokumentationsressourcen zur Verwendung von Azure Webaufträge und Azure Webaufträge SDK. Azure Webaufträge bieten eine einfache Möglichkeit, Skripts oder Programme als Hintergrundprozesse [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714)ausführen. Laden und Ausführen eine ausführbare Datei z. B. als Cmd, Bat, Exe (.NET), ps1, sh, Php Py, Js und. Diese Programme führen als Webaufträge Zeitplan (Cron) oder permanent.

Das WebJobs SDK vereinfacht die Azure-Speicher verwenden. Das WebJobs SDK enthält ein Bindung und Trigger mit Microsoft Azure Storage Blobs, Warteschlangen und Tabellen sowie Service Bus-Warteschlangen.

Erstellen, bereitstellen und Verwalten von Webaufträge ist nahtlos mit integrierten Tools in Visual Studio. Webaufträge aus Vorlagen erstellen, veröffentlichen und verwalten (Run/Stop/Monitor/Debug) werden.

Webaufträge Dashboard im Azure-Portal bietet leistungsfähige Management-Funktionen, die vollständige Kontrolle über die Ausführung der Webaufträge, einschließlich der Möglichkeit, einzelne Funktionen in Webaufträge aufrufen geben. Das Dashboard zeigt Funktion Laufzeiten und Protokollausgabe.

[AZURE.INCLUDE [app-service-blueprint-webjobs](../../includes/app-service-blueprint-webjobs.md)]
