<properties
    pageTitle="Web App Ressourcen in eine andere Ressourcengruppe verschieben"
    description="Beschreibt die Szenarien, in denen Sie Web-Apps und Anwendungsdienste aus einer Ressourcengruppe verschieben können."
    services="app-service"
    documentationCenter=""
    authors="ZainRizvi"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="01/04/2016"
    ms.author="zarizvi"/>
    
# <a name="supported-move-configurations"></a>Verschiebungstypen unterstützt Konfigurationen

Sie können mithilfe der [ARM bewegen Ressourcen Api](../resource-group-move-resources.md)Azure Web App Ressourcen verschieben.

Azure Web Apps unterstützt derzeit die folgenden Szenarien verschieben:

* Der gesamte Inhalt einer Ressourcengruppe (webapps app Servicepläne und Zertifikate) verschieben in eine andere Ressourcengruppe 
    * Hinweis: Die Ziel-Ressourcengruppe kann Microsoft.Web Ressourcen in diesem Szenario nicht enthalten
* Verschieben einzelner webapps unterschiedlichen Ressourcengruppen, während noch in ihrer aktuellen app Service-Plan (Serviceplan app bleibt Ressource Gruppe) hosten
