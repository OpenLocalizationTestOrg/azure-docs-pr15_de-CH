<properties 
    pageTitle="Überblick über Enterprise Integration | Microsoft Azure App Service | Microsoft Azure" 
    description="Mit den Features der Unternehmensintegration Business-Prozess und Integration Szenarien mit Logik apps" 
    services="logic-apps" 
    documentationCenter=".net,nodejs,java"
    authors="msftman" 
    manager="erikre" 
    editor="cgronlun"/>

<tags 
    ms.service="logic-apps" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/08/2016" 
    ms.author="deonhe"/>

# <a name="overview-of-the-enterprise-integration-pack"></a>Überblick über Enterprise-Integrationspaket

## <a name="what-is-the-enterprise-integration-pack"></a>Was ist das Enterprise Integration Pack?
Enterprise-Integration Pack ist Microsoft Cloud-basierte Lösung für Business to Business (B2B) Kommunikation nahtlos aktivieren. Das Pack verwendet Standardprotokolle [AS2](./app-service-logic-enterprise-integration-as2.md), [X12](./app-service-logic-enterprise-integration-x12.md)und [EDIFACT](./app-service-logic-enterprise-integration-edifact.md) Nachrichten zwischen Geschäftspartnern austauschen. Nachrichten können optional mithilfe von Verschlüsselung und digitalen Signaturen gesichert werden. 

Das Pack kann Organisationen, die unterschiedliche Protokolle und Formate um elektronische Nachrichten in ein Format, das beide Organisationen Systeme interpretieren und ergreifen die verschiedenen Formate transformiert. 

Wenn Sie mit BizTalk Server oder Microsoft Azure BizTalk Services vertraut sind, finden Sie es benutzerfreundliche Enterprise-Integration bietet die Begriffe ähnlich sind. Ein Hauptunterschied darin, dass Enterprise Integration Integration Konten vereinfacht die Speicherung und Verwaltung von Artefakten zur B2B-Kommunikation verwendet. 

Architektonisch Integrationspaket Enterprise basiert **Integration Konten** , die alle Elemente speichern, die zum Entwerfen, bereitstellen und Verwalten Ihrer B2B-apps verwendet werden können. Eine Firma Integration ist im Grunde cloudbasierten Container Sie Artefakte Schemas Partner, Zertifikate, Karten und Abkommen speichern. Diese Artefakte können dann Logik Apps zu B2B-Workflows verwendet werden. Bevor Sie Artefakte in einer Logik-app verwenden können, müssen Sie Ihr Konto Integration für Ihre Anwendung Logik zu verknüpfen. Nach verknüpfen, wird Ihre Anwendung Logik auf Integration Konto Artefakte zugreifen.  

## <a name="why-should-you-use-enterprise-integration"></a>Warum sollten Sie Unternehmensintegration verwenden?
- Enterprise-Integration können Sie alle Elemente an einem Ort speichern, ist Ihr Konto Integration. 
- Sie können Logik apps Engine und alle Connectors B2B-Workflows erstellen und integrieren 3rd Party SaaS-Applikationen lokalen apps sowie andere Programme nutzen
- Sie können auch Azure-Funktionen nutzen.

## <a name="how-to-get-started-with-enterprise-integration"></a>Wie Enterprise-Integration?
Sie können erstellen und B2B-apps mithilfe des Enterprise Integration Packs über Logik apps Designer **Azure-Portal**verwalten.  

[PowerShell](https://msdn.microsoft.com/library/azure/mt652195.aspx "Logik apps PowerShell Themen") können Sie um Logik-apps zu verwalten. 

Übersicht der Schritte müssen Sie vor dem Erstellen von apps in Azure-Portal: ![Übersicht Übersichtsbild](./media/app-service-logic-enterprise-integration-overview/overview-0.png)  

## <a name="what-are-some-common-scenarios"></a>Was sind einige häufige Szenarien?

Enterprise-Integration unterstützt diese Industriestandards:   

- EDI - Datenaustausch  
- EAI - Enterprise Application Integration  

## <a name="heres-what-you-need-to-get-started"></a>Sie benötigen für den Einstieg
- Azure-Abonnement mit einem Konto integration
- Visual Studio 2015 Karten und Schemas erstellen
- [Microsoft Azure Logik Apps Unternehmensintegration Tools für Visual Studio 2015 2.0](https://aka.ms/vsmapsandschemas)  

## <a name="try-it"></a>Versuch es
[Probieren Sie es jetzt](https://github.com/Azure/azure-quickstart-templates/tree/master/201-logic-app-as2-send-receive) ein betriebsbereites Beispiel AS2 bereitgestellt senden und Empfangen von Logik-app, die B2B-Features von Logik-Apps verwendet.

## <a name="learn-more-about"></a>Erfahren Sie mehr über:
- [Verträge] (./app-service-logic-enterprise-integration-agreements.md "Erfahren Sie mehr über Enterprise Integration Abkommen")
- [Business to Business (B2B) Szenarien] (./app-service-logic-enterprise-integration-b2b.md "Informationen zum Erstellen von Logik apps mit B2B")  
- [Zertifikate] (./app-service-logic-enterprise-integration-certificates.md "Erfahren Sie mehr über Enterprise Integration Zertifikate")
- [Codierung/Decodierung Flatfile] (./app-service-logic-enterprise-integration-flatfile.md "Informationen zum Kodieren und Dekodieren von Flatfile-Inhalt")  
- [Integration Konten] (./app-service-logic-enterprise-integration-accounts.md "Erfahren Sie mehr über Integration Konten")
- [Karten] (./app-service-logic-enterprise-integration-maps.md "Erfahren Sie mehr über Unternehmensintegration Karten")
- [Partner] (./app-service-logic-enterprise-integration-partners.md "Informationen zu Enterprise-Integrationspartner")
- [Schemas] (./app-service-logic-enterprise-integration-schemas.md "Erfahren Sie mehr über Enterprise Integration schemas")
- [XML-Überprüfung] (./app-service-logic-enterprise-integration-xml.md "Erfahren Sie, wie XML-Nachrichten mit Logik apps überprüfen")
- [XML transformieren] (./app-service-logic-enterprise-integration-transform.md "Erfahren Sie mehr über Unternehmensintegration Karten")
- [Enterprise-Integration Connectors] (../connectors/apis-list.md "Erfahren Sie mehr über Enterprise Integration Packs connectors")



