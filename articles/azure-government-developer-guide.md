<properties 
    pageTitle="Azure Government Developers Guide" 
    description="Dies bietet einen Vergleich der Features und Hinweise auf die Anwendungsentwicklung für Azure" 
    services="" 
    cloud="gov"
    documentationCenter="" 
    authors="Joharve2" 
    manager="Chrisnie" 
    editor=""/>

<tags 
    ms.service="multiple" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="azure-government" 
    ms.date="10/29/2015" 
    ms.author="jharve"/>


#  <a name="microsoft-azure-government-developer-guide"></a>Microsoft Azure Regierung Developer Guide 

<p> Microsoft Azure ist eine physisch isolierten Netzwerk- Instanz der Microsoft Azure.  Dabei Entwickler bietet Details zu den unterschieden, Entwickler und Administratoren zu interagieren diese separaten Bereiche von Azure muss.

<!--Table of contents for topic, the words in brackets must match the heading wording exactly-->


## <a name="in-this-topic"></a>In diesem Thema


+ [Übersicht](#Overview)
+ [Anleitung für Entwickler](#Guidance)
+ [Funktionen, die derzeit in Microsoft Azure Government](#Features)
+ [Endpunkt-Mapping](#Endpoint)
+ [Nächste Schritte](#next)


## <a name="Overview"></a>Übersicht

Microsoft Azure Government ist eine separate Instanz von Microsoft Azure Service Bedürfnisse Sicherheit und Kompatibilität von US-Bundesbehörden, Zustand und lokalen Gebietskörperschaften und ihre Lösungsanbieter. Azure Government bietet physische und Netzwerkisolation US-Regierung Bereitstellungen und überwachten US-Mitarbeiter. 

Microsoft bietet eine Reihe von Tools zum Erstellen und Bereitstellen von Cloudanwendungen zu Microsoft global Microsoft Azure-Dienst "Global" Microsoft Azure Government Services.

Entwickler müssen beim Erstellen und Bereitstellen von Clientanwendungen Regierungsstellen Azure gegenüber globalen Dienst die Hauptunterschiede der beiden Dienste kennen.  Speziell auf einrichten und konfigurieren ihre Programmierumgebung, Konfigurieren von Endpunkten Applikationen schreiben und als Azure Government Services bereitstellen.

Die Informationen in diesem Dokument werden die Unterschiede zusammengefasst und ergänzen die Informationen [Azure Government](http://www.azure.com/gov "Azure Regierung") und die [Technische Bibliothek für Microsoft Azure](http://msdn.microsoft.com/cloud-app-development-msdn "MSDN") auf MSDN. Offizielle auch möglicherweise an vielen anderen Orten wie die [Microsoft Azure Trust Center] (https://azure.microsoft.com/support/trust-center/ "Microsoft Azure Trust Center" /), [Azure Dokumentation Center](https://azure.microsoft.com/documentation/) und [Azure Blogs] (https://azure.microsoft.com/blog/ "Azure Blogs" /). 

Dieser Inhalt soll für Partner und Entwickler, die Microsoft Azure Government bereitstellen.



## <a name="Guidance"></a>Anleitung für Entwickler
Da technische Inhalte, die derzeit verfügbar ist annimmt, dass Applikationen für den globalen Dienst nicht für Microsoft Azure entwickelt werden, ist es wichtig sicherzustellen, dass Entwickler der Unterschiede für Applikationen entwickelt, um in Azure Government gehostet werden.

- Zunächst gibt Services und Funktionsunterschiede, dies bedeutet, dass bestimmte Features, die in bestimmten Regionen Global Service nicht in Azure Government vorliegen.

- Zweitens für Azure Regierung angebotenen Funktionen unterscheiden Konfiguration von globalen Dienst.  Daher sollten die Beispielcode, Konfigurationen und Maßnahmen, die Sie erstellen und in Azure Government Cloud Services-Umgebung ausführen.


## <a name="Features"></a>Funktionen, die derzeit in Microsoft Azure Government
Azure Regierung hat uns GOV IOWA und uns GOV VIRGINIA derzeit die folgenden Dienste:

- Virtuelle Computer
- Cloud-Dienste
- Speicher
- Active Directory
- Planer
- Virtuelle Netzwerke
- SQL-Datenbank
- Azure-Dateien
- Media Services
- Traffic Manager
- Servicebus
- StorSimple
- Redis Cache
- Azure Backup
- Automatisierung
- ExpressRoute
- usw.

Andere Dienste stehen und weitere Dienste kontinuierlich hinzugefügt.  Aktuelle Liste der Dienste finden Sie unter die [Bereiche Seite](https://azure.microsoft.com/regions/#services) verfügbaren Regionen und ihre Dienste hervorgehoben wird.  

Derzeit sind uns GOV Iowa und uns GOV Virginia Azure Regierung Rechenzentren.  Siehe Seite Regionen über aktuelle Rechenzentren und Dienstleistungen.

## <a name="Endpoint"></a>Endpunkt-Mapping

Verwenden Sie in der folgenden Tabelle führen bei öffentlichen Microsoft Azure und SQL Datenbank Endpunkte zu Azure Government Endpunkte.


Diensttyp|Azure Public|Azure Government
---|---|---
Verwaltungsportal|Manage.windowsazure.com|Manage.windowsazure.US
Allgemein|*. Windows|*. usgovcloudapi.net
Core|*. von Core.Windows.NET befinden.|*. core.usgovcloudapi.net
Berechnen|*. cloudapp.net|*. usgovcloudapp.net
BLOB-Speicher|*. blob.core.windows.net|   *. blob.core.usgovcloudapi.net
Warteschlangenspeicher|*. queue.core.windows.net|*. queue.core.usgovcloudapi.net
Tabelle speichern|*. table.core.windows.net|*. table.core.usgovcloudapi.net
Service-Management|Management.Core.Windows.NET|Management.Core.usgovcloudapi.NET
SQL-Datenbank|*. database.windows.net|*. database.usgovcloudapi.net
ARM-Lastenausgleich Endpunkt|https://Management.Windows.NET|https://Management.usgovcloudapi.NET  

* ARM-Authentifizierung über Azure AD finden Sie [Authentifizieren Azure Ressourcenmanager Anfragen](https://msdn.microsoft.com/library/azure/dn790557.aspx)

## <a name="next"></a>Nächste Schritte

Wenn Sie weitere und Azure Regierung interessieren nutzen Sie einige der folgenden Links.

- **[Melden Sie sich für eine Testversion](https://azuregov.microsoft.com/trial/azuregovtrial)**

- **[Erfassen und Zugriff auf Azure Government](http://azure.com/gov)**

- **[Azure Government (Übersicht)](/azure-government-overview)**

- **[Azure Government Blog](http://blogs.msdn.com/b/azuregov/)**

- **[Azure Compliance](https://azure.microsoft.com/support/trust-center/compliance/)**

<!--Anchors-->



<!-- Images. -->

[1]: ./media/azure-government-developer-guide/publisherguide.png


<!--Link references-->
[Link 1 to another azure.microsoft.com documentation topic]: virtual-machines-windows-hero-tutorial.md
[Link 2 to another azure.microsoft.com documentation topic]: web-sites-custom-domain-name.md
[Link 3 to another azure.microsoft.com documentation topic]: storage-whatis-account.md
