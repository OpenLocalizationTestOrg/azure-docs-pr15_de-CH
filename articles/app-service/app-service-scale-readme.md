<properties
    pageTitle="Azure App Service: Skalierung App Service Applications"
    description="Lernen Sie die Besonderheiten der Anwendung in App Service Skalierung."
    keywords="App, Azure app Dienst, Skalierung, skalierbare Anwendung Serviceplan app Servicekosten"
    services="app-service"
    documentationCenter=""
    authors="btardif"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/07/2016"
    ms.author="byvinyal"/>

# <a name="azure-app-service-scaling-app-service-applications"></a>Azure App Service: Skalierung App Service Applications

Anwendung in Azure App Service können [massiv erreichen](https://azure.microsoft.com/blog/canadian-broadcasting-corporation-radio-canada-leverage-azure-for-smooth-election-coverage/).
Skalieren einer Anwendung ist jedoch ein komplexes Problem, das keine Lösung "Einheitsgröße". Richtig Skalieren Ihrer Anwendung sind 3 Schlüsselbereiche, in denen Ihre Anwendung Erfolg beitragen:

1. Grundlegendes zu Ihrer Anwendungsarchitektur und Schwächen.
    * Ist Ihre Anwendung Stateful? Statusfrei?
    * Was sind die Komponenten der Anwendung?
        * Wo sind die Engpässe in der Anwendung?
    * Beim Laden der App angewendet wird, was erste unterbricht die?
2. Verständnis der erwarteten Auslastung und Performance Anforderungen.
    * Muss die Anwendung Tausend Benutzer? oder eine Million?
    * Wird von einem geografischen Standort oder Global stammen?
    * Gibt es Jahreszeiten? Traffic-Spitzen?
    * Wie schnell sollte die Anwendung? 1 Sekunde? 1 Millisekunde?
3. Verstehen und richtig nutzen Ihre app hosting Plattform.
    * Nutzen Was sollte mein Maßstab Ziele?

Dieser Abschnitt hilft Ihnen zu verstehen, alle Faktoren und Hilfe eine Strategie entwickeln, die die erforderlichen App Service Features zur Verwirklichung Ihrer Ziele skalierbar nutzt.

[AZURE.INCLUDE [app-service-blueprint-scaling-app-service-applications](../../includes/app-service-blueprint-scaling-app-service-applications.md)]
