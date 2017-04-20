<properties
    pageTitle="Azure App Service und ihre Auswirkung auf vorhandene Azure services"
    description="Erläutert, wie neue Azure App Service und seine Funktionen vorhandene Diensten in Azure auswirken."
    services="app-service"
    documentationCenter=""
    authors="yochay"
    manager="nirma"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/12/2016"
    ms.author="yochayk"/>


# <a name="azure-app-service-and-existing-azure-services"></a>Azure App Service und vorhandene Azure services

Dieser Artikel befasst sich vorhandene Azure Services als Teil der zu kombinieren mehrere Azure Services in [Azure App Service](https://azure.microsoft.com/services/app-service/)ein neues integriertes Angebot.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="overview"></a>Übersicht

[Azure App Service](https://azure.microsoft.com/services/app-service/) ist eine neue und einzigartige Clouddienst, der Entwickler Web- und apps für jede Plattform und Gerät erstellen kann. App Service ist eine integrierte Lösung zur wiederholte Codierung Funktionen optimieren, Enterprise und SaaS-Systeme integrieren und Geschäftsprozesse automatisieren und Ihre Bedürfnisse für Sicherheit, Zuverlässigkeit und Erweiterbarkeit.

App Service vereint die folgenden vorhandenen Azure Services - [Websites](https://azure.microsoft.com/services/websites/), [Mobile Dienste](https://azure.microsoft.com/services/mobile-services/)und [Biztalk-Dienste](https://azure.microsoft.com/services/biztalk-services/) in einem einzelnen kombinierten Dienst beim leistungsfähige neue Funktionen hinzufügen.  App-Dienst können Sie folgende Anwendung hosten:

-   Webapps
-   Mobiler Apps
-   API-Apps
-   Logik-Apps

Die folgende Tabelle erläutert die App Service wie vorhandene Azure Services zuordnen und die app darin verfügbar.

<table>
<thead>
<tr class="header">
<th align="left", style="width:10%">Vorhandene Azure Service</th>
<th align="left", style="width:10%">Azure App Service</th>
<th align="left", style="width:80%">Was sich geändert hat</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Azure Websites</td>
<td align="left">Webapps</td>
<td align="left"><li>Azure Websites ist App Service ausschließlich Namensänderung Websites Web-Apps.
<p><li>Alle vorhandenen Instanzen von Websites sind jetzt Web Apps in App Service.</p>
<p><li>Sie alle vorhandenen Websites unter <em>Web Apps finden</em>können Sie Ihre vorhandenen Websites <a href="http://go.microsoft.com/fwlink/?LinkId=529715">Azure-Portal</a>zugreifen.</p>
<p><li><em>Web-Hosting-Plan</em> ist jetzt <em>App Service-Plan</em>. Eine <em>App Service-Plan</em> bietet keinerlei app App Service wie Web, mobil, Logik oder API-apps.</p>
<p><li>Azure App Service Web Apps wird im allgemeinen Verfügbarkeit.</p>
<p><li><a href="http://azure.microsoft.com/services/app-service/web/">Weitere Informationen zu Web Apps</a>.</p></td>
</tr>
<tr class="even">
<td align="left">Azure Mobile Dienste</td>
<td align="left">Mobiler Apps</td>
<td align="left"><p><li>Mobile Dienste weiterhin als eigenständiger Dienst und vollständig unterstützt.</p>
<p><li>Mobile Apps ist eine app in App-Dienst, der Funktionalität von Mobile Dienste und mehr integriert.</p>
<p><li>Es ist einfach <a href="http://go.microsoft.com/fwlink/?LinkID=724279&clcid=0x409">von Mobile Dienste Mobile Apps migrieren</a>.</p>
<p><li>Als Teil der App Service erhalten Mobile Apps neue Funktionen über Mobile Dienste wie mit lokalen und SaaS staging-Steckplätze, Webaufträge und bessere Skalierungsoptionen.</p>
<p><li><a href="http://azure.microsoft.com/services/app-service/mobile/">Weitere Informationen zu Mobile Apps</a>.</p>
</tr>
<tr class="odd">
<td align="left"></td>
<td align="left">API-Apps</td>
<td align="left">
<p><li>API-Apps ist eine neue Anwendungstyp App-Dienst, mit dem Sie problemlos erstellen und Verwenden von APIs in der Cloud.</p>
<p><li><a href="http://azure.microsoft.com/services/app-service/api/">Weitere Informationen zu API-Apps</a>.</p></td>
</tr>
<tr class="even">
<td align="left"></td>
<td align="left">Logik-Apps</td>
<td align="left">
<p><li>Logik Apps ist eine neue Anwendungstyp App-Dienst mit dem Geschäftsprozess problemlos automatisieren können.</p>
<p><li><a href="http://azure.microsoft.com/services/app-service/logic/">Erfahren Sie mehr über Logik Apps</a>.</p></td>
</tr>
<tr class="odd">
<td align="left">Azure BizTalk-Dienste</td>
<td align="left">BizTalk-API-Apps</td>
<td align="left">
<li><p>BizTalk-Dienste weiterhin als eigenständiger Dienst und vollständig unterstützt.</p>
<li><p>Alle Funktionen der BizTalk-Dienste sind in App Service als API-Apps aktivieren Benutzer Enterprise Application Integration und B2B-Integrationsszenarios mit app-Typen in App Service integriert.</p>
<li><p>Logik Apps können Sie jetzt visual Entwurfserlebnis zum Erstellen von Workflows mit Geschäftsprozesse automatisieren.</p></td>
</tr>
</tbody>
</table>

Besuchen Sie weitere [App Service-Dokumentation](https://azure.microsoft.com/documentation/services/app-service/).
