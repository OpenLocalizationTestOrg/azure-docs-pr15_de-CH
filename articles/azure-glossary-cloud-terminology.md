<properties
    pageTitle="Glossar der Azure - Azure Wörterbuch | Microsoft Azure"
    description="Verwenden des Azure-Glossars, Terminologie Cloud auf der Azure-Plattform. Dieses kurze Azure Wörterbuch enthält Definitionen für Begriffe Wolke Azure."
    keywords="Wörterbuch Azure Cloud Terminologie Azure Glossar, Terminologie Definitionen, Cloud-Begriffe"
    services="na"
    documentationCenter="na"
    authors="MonicaRush"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="monicar"/>


# <a name="microsoft-azure-glossary-a-dictionary-of-cloud-terminology-on-the-azure-platform"></a>Microsoft Azure-Glossar: ein Wörterbuch der Cloud Terminologie der Azure-Plattform

Microsoft Azure-Glossar ist ein kurzes Wörterbuch Cloud Terminologie der Azure-Plattform.

## <a name="find-service-definitions-and-other-cloud-terms"></a>Dienstdefinitionen und andere Begriffe Wolke suchen

* Definitionen Azure und ihre AWS finden Sie in den entsprechenden [Microsoft Azure und Amazon Web Services](https://azure.microsoft.com/campaigns/azure-vs-aws/mapping/).

* Industrie finden Sie Begriffe Wolke [Cloud computing Begriffe](https://azure.microsoft.com/overview/cloud-computing-dictionary/).

Azure Glossar mit den oben genannten zwei bietet eine End-to-End-Taxonomie für Azure und die Cloud.  

## <a name="azure-glossary-list"></a>Liste der Azure-Glossar


### <a name="account"></a>Konto  
Eine Arbeit oder Schule oder persönlichen Microsoft-Konto, das verwaltet und Azure-Abonnement verwendet wird.  
Siehe auch [wie Azure-Abonnements Azure Active Directory zugeordnet sind](./active-directory/active-directory-how-subscriptions-associated-directory.md)


### <a name="availability-set"></a>Festlegen der Verfügbarkeit  
Eine Auflistung von virtuellen Computern, die Anwendungsredundanz und Zuverlässigkeit gemeinsam verwaltet werden. Die Verwendung eines Verfügbarkeit sichergestellt, dass während eines geplanten oder ungeplanten Wartung-Ereignisses mindestens einen virtuellen Computer verfügbar ist.  
Siehe auch [die Verfügbarkeit der virtuellen Windows-Maschinen verwalten](./virtual-machines/virtual-machines-windows-manage-availability.md) oder [die Verfügbarkeit der virtuelle Linux-Computer verwalten](./virtual-machines/virtual-machines-linux-manage-availability.md)


### <a name="classic-model"></a>Azure klassischen Bereitstellungsmodell  
Einer der zwei [Bereitstellungsmodelle](resource-manager-deployment-model.md) Ressourcen in Azure (neue Modell ist Azure-Ressourcen-Manager). Azure Ressourcen können in einem Modell oder anderen bereitgestellt werden, während andere in beiden Modellen bereitgestellt werden können. Anleitung für einzelne Azure Ressourcen Detail eine Ressource Modelle kann mit bereitgestellt werden.


### <a name="cli"></a>Azure Befehlszeilenschnittstelle (CLI)  
Eine [Befehlszeilenschnittstelle](xplat-cli-install.md) , Azure-Dienste von Windows OSX und Linux-Computer verwalten.


### <a name="powershell"></a>Azure PowerShell  
Eine [Befehlszeilenschnittstelle](powershell-install-configure.md) von PCs Windows Azure Services über eine Befehlszeile verwalten. Einige Dienste oder Funktionen können nur über PowerShell oder CLI verwaltet. Anleitung für jede einzelne Azure Ressourcendetails die Ressource Modelle kann mit bereitgestellt werden.   
Auch Informationen Sie [zur Installation und Konfiguration von Azure PowerShell](powershell-install-configure.md)


### <a name="arm-model"></a>Azure Ressourcenmanager Bereitstellungsmodell  
Einer der zwei [Bereitstellungsmodelle](resource-manager-deployment-model.md) zur Bereitstellung von Ressourcen in Microsoft Azure (die andere ist das klassische Bereitstellungsmodell). Azure Ressourcen können in einem Modell oder anderen bereitgestellt werden, während andere in beiden Modellen bereitgestellt werden können. Anleitung für einzelne Azure Ressourcen Detail eine Ressource Modelle kann mit bereitgestellt werden.


### <a name="fault-domain"></a>Fehlerdomäne  
Die Auflistung der virtuellen Computer in eine verfügbarkeitsgruppe, die möglicherweise zur gleichen Zeit ausgeführt werden kann. Ein Beispiel ist eine Gruppe von Computern in einem Rack, die einen gemeinsamen Quelle und Netzwerk Netzschalter gemeinsam nutzen. In Azure werden die virtuellen Computer in einem Verfügbarkeit über mehrere Fehlerdomänen automatisch getrennt.  
Siehe auch [die Verfügbarkeit der virtuellen Windows-Maschinen verwalten](./virtual-machines/virtual-machines-windows-manage-availability.md) oder [die Verfügbarkeit der virtuelle Linux-Computer verwalten](./virtual-machines/virtual-machines-linux-manage-availability.md)  


### <a name="geo"></a>Geo  
Definierte Berandung Residency Daten, die in der Regel zwei oder mehr Bereiche enthält. Die Grenzen möglicherweise innerhalb oder Landesgrenzen und Steuervorschriften beeinflusst. Alle Geo hat mindestens eine Region. Beispiele für Geos sind Asien/Pazifik und Japan. Auch *"Geografie"*genannt.  
Siehe auch [Azure-Regionen](best-practices-availability-paired-regions.md)


### <a name="geo-replication"></a>Geo-Replikation  
Der Prozess der automatische Replikation von Blobs und Tabellen mit Warteschlangen innerhalb eines regionalen Inhalten.  
Siehe auch [Aktive Geo-Replikation für SQL Azure-Datenbank](./sql-database/sql-database-geo-replication-overview.md)


### <a name="image"></a>Bild  
Eine Datei mit dem Betriebssystem und Konfiguration, mit der eine beliebige Anzahl virtueller Computer erstellen. In Azure sind zwei Arten von Bildern: VM und Betriebssystem-Abbild. VM-Image enthält ein Betriebssystem und alle Datenträger, die Erstellung des Abbilds einer virtuellen Maschine zugeordnet. Ein Betriebssystemabbild enthält nur eine allgemeine Betriebssystem mit Konfigurationen keine Daten.  
Siehe auch [Navigieren und wählen Windows virtuellen Computerimages in Azure PowerShell oder CLI](./virtual-machines/virtual-machines-windows-cli-ps-findimage.md)


### <a name="limits"></a>Grenzen  
Die Anzahl der Ressourcen, die erstellt werden können oder Performance-Benchmark, die erreicht werden können. Grenzen sind typischerweise mit Abonnements, Services und Angebote.  
Siehe auch [Azure-Abonnement und Service Grenzen, Kontingente und Integritätsregeln](azure-subscription-service-limits.md)


### <a name="load-balancer"></a>Lastenausgleich  
Eine Ressource, die Datenverkehr zwischen Computern in einem Netzwerk verteilt. Ein Lastenausgleich verteilt in Azure Datenverkehr an virtuelle Computer in einem Lastenausgleich definiert. Ein [System zum Lastenausgleich laden](./load-balancer/load-balancer-overview.md) kann Internetzugriff oder intern sein.  


### <a name="offer"></a>Angebot  
Die Preise, Credits und Azure-Abonnement für ähnliche Begriffe.  
Siehe [Seite bieten Azure](https://azure.microsoft.com/support/legal/offer-details/)


### <a name="portal"></a>Portal  
Zum Bereitstellen und Verwalten von Azure Services sicheren Web-Portal.  Es gibt zwei Portale: [Azure-Portal](http://portal.azure.com/) und [Verwaltungsportal](http://manage.windowsazure.com/). Einige Dienste sind für beide Portale, während andere nur für eine. [Azure Portal Verfügbarkeitsdiagramm](https://azure.microsoft.com/features/azure-portal/availability/) Listet die Dienste in der Portal verfügbar sind.  


### <a name="region"></a>Region  
Ein Bereich innerhalb einer Geo, die Kreuz nationale Rahmen und enthält ein oder mehrere Rechenzentren. Preise, Regionalverkehr und Angebotstypen werden auf der Regionsebene verfügbar gemacht. Ein Bereich ist normalerweise eine andere Region zugeordnet werden bis zu mehreren hundert Meilen entfernt, kann zu einem regionalen Paar. Regionale Paare können als Mechanismus für Disaster Recovery und hohe Verfügbarkeit Szenarien verwendet werden. Auch bezeichnet im Allgemeinen als *Speicherort*.  
Siehe auch [Azure-Regionen](best-practices-availability-paired-regions.md)


### <a name="resource"></a>Ressource  
Ein Element, das Teil der Projektmappe Azure ist. Jede Azure Service können Sie verschiedene Ressourcen wie Datenbanken oder virtuellen Computern bereitstellen.   
Siehe auch [Azure-Ressourcen-Manager (Übersicht)](azure-resource-manager/resource-group-overview.md)


### <a name="resource-group"></a>Ressourcengruppe  
Ein Container in Ressourcen-Manager, der zugehörige Ressourcen für eine Anwendung enthält. Die Ressourcengruppe gehören alle Ressourcen für eine Anwendung oder den Ressourcen, die zusammen gruppiert werden. Sie können die zweisprachigen Zuordnen von Ressourcen zu Ressourcengruppen, die Grundlage für Ihre Organisation am meisten Sinn macht.  
Siehe auch [Azure-Ressourcen-Manager (Übersicht)](azure-resource-manager/resource-group-overview.md)


### <a name="arm-template"></a>Ressourcen-Manager-Vorlage  
Eine JSON-Datei deklarativ definiert eine oder mehrere Azure Ressourcen und Abhängigkeiten bereitgestellten Ressourcen definiert. Die Vorlage kann verwendet werden, die Ressourcen konsistent und wiederholt bereitstellen.  
Siehe auch [Azure Ressourcenmanager erstellen Vorlagen](resource-group-authoring-templates.md)


### <a name="resource-provider"></a>Ressourcenproviders  
Ein Dienst, der die Ressourcen bereitstellt, bereitstellen und Verwalten über Ressourcen-Manager. Jede Ressource Anbieter Operationen für das Arbeiten mit Ressourcen bereitgestellt werden. Ressourcen können Azure-Portal, Azure PowerShell und mehrere Programmiersprachen SDKs zugegriffen.  
Siehe auch [Azure-Ressourcen-Manager (Übersicht)](azure-resource-manager/resource-group-overview.md)


### <a name="role"></a>Rolle  
Eine Möglichkeit zum Steuern des Zugriffs für Benutzer, Gruppen und Dienste zugewiesen werden kann. Rollen können Aktionen erstellen, zum Verwalten und Azure Ressourcen lesen.  
Siehe auch [RBAC: integrierte Rollen](./active-directory/role-based-access-built-in-roles.md)


### <a name="sla"></a>Vereinbarung zum Servicelevel (SLA)  
Die Vereinbarung, die Microsofts Engagement für Verfügbarkeit und Konnektivität beschreibt. Jede Azure Service hat eine bestimmte Vereinbarung zum SERVICELEVEL.  
Siehe auch [Service Level Agreements](https://azure.microsoft.com/support/legal/sla/)


### <a name="storage-account"></a>Konto  
Ein Speicherkonto, das Ihnen bietet Zugang zu den Azure Blob, Warteschlange, Tabelle und Datei in Azure-Speicher. Das Speicherkonto bereit Datenobjekte Azure Storage eindeutigen Namespace.  
Siehe auch [über Azure-Speicherkonten](./storage/storage-create-storage-account.md)


### <a name="subscription"></a>Abonnement  
Zustimmung des Kunden bei Microsoft, das Azure Services erhalten kann. Preise und verwandten Begriffen unterliegen das Angebot für den Dauerauftrag ausgewählt. [Microsoft Online-Abonnement-Vertrag](https://azure.microsoft.com/support/legal/subscription-agreement/)anzeigen  
Siehe auch [wie Azure-Abonnements Azure Active Directory zugeordnet sind](./active-directory/active-directory-how-subscriptions-associated-directory.md)


### <a name="tag"></a>Tag  
Eine Indizierung Begriff, der entsprechend zu verwalten oder Rechnungsadresse kategorisieren kann. Sie können Tags eine komplexe Sammlung von Ressourcengruppen und Ressourcen und müssen die Ressourcen wie anzeigen, die am meisten Sinn macht. Sie könnten z. B. Ressourcen tag, die eine ähnliche Rolle in der Organisation oder Abteilung gehören.  
Siehe auch [mit Tags zu Azure-Ressourcen](resource-group-using-tags.md)


### <a name="update-domain"></a>Domäne aktualisieren  
Die Auflistung der virtuellen Computer in einem Verfügbarkeit, die gleichzeitig aktualisiert werden. Virtuelle Computer in der gleichen Domäne aktualisieren werden während der geplanten Wartung miteinander neu gestartet. Azure startet nie mehr als eine Domäne aktualisieren nacheinander neu. Auch bezeichnet als eine Aktualisierungsdomäne.  
Siehe auch [die Verfügbarkeit der virtuellen Windows-Maschinen verwalten](./virtual-machines/virtual-machines-windows-manage-availability.md) oder [die Verfügbarkeit der virtuelle Linux-Computer verwalten](./virtual-machines/virtual-machines-linux-manage-availability.md)  


### <a name="vm"></a>virtuelle Computer  
Die softwareimplementierung eines physischen Computers, der ein Betriebssystem ausführt. Mehrere virtuelle Maschinen können gleichzeitig auf derselben Hardware ausgeführt werden. In Azure sind virtuelle Maschinen in verschiedenen Größen verfügbar.  
Siehe auch [virtuelle Computer Dokumentation](https://azure.microsoft.com/documentation/services/virtual-machines/)


### <a name="vm-extension"></a>VM-Erweiterung  
Eine Ressource, die Verhalten oder Funktionen entweder andere Programme arbeiten oder bieten die Möglichkeit zur Interaktion mit einem laufenden Computer implementiert. Beispielsweise können Sie die Erweiterung VM RAS Werte Azure VM zu zurücksetzen.  
Siehe auch [über VM Extensions und Funktionen (Windows)](./virtual-machines/virtual-machines-windows-extensions-features.md) oder [virtuellen Extensions und Funktionen (Linux)](./virtual-machines/virtual-machines-linux-extensions-features.md)


### <a name="vnet"></a>virtuelles Netzwerk  
Ein Netzwerk, das Konnektivität zwischen Azure Ressourcen, die von anderen Azure Mieter isoliert ist. Sie können mit anderen Azure virtuellen Netzwerken über ein [VPN-Gateway Azure](./vpn-gateway/vpn-gateway-about-vpngateways.md) und mit dem lokalen Netzwerk [mehrere Optionen](./vpn-gateway/vpn-gateway-cross-premises-options.md)verbunden sein. Sie können die IP-Adressblöcke, DNS-Einstellungen Sicherheitsrichtlinien und Routetabellen im Netzwerk vollständig steuern.  
Siehe auch [Virtual Network (Übersicht)](./virtual-network/virtual-networks-overview.md)  

###<a name="see-also"></a>**Siehe auch**  
- [Erste Schritte mit Azure](https://azure.microsoft.com/get-started/)
- [Cloud-Ressourcen-center](https://azure.microsoft.com/resources/)  
- [Azure für Ihre geschäftliche Anwendung](https://azure.microsoft.com/overview/business-apps-on-azure/)
- [Azure im Rechenzentrum](https://azure.microsoft.com/overview/business-apps-on-azure/) 





