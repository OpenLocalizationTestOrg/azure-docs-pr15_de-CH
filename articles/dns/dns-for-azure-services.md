<properties
  pageTitle="Mithilfe von Azure DNS mit anderen Azure | Microsoft Azure"
  description="Grundlegendes zum Azure DNS zum Auflösen von Namen für andere Azure-Dienste verwenden"
  services="dns"
  documentationCenter="na"
  authors="sdwheeler"
  manager="carmonm"
  editor=""
  tags="azure dns"
/>
<tags
  ms.service="dns"
  ms.devlang="na"
  ms.topic="article"
  ms.tgt_pltfrm="na"
  ms.workload="infrastructure-services"
  ms.date="09/21/2016"
  ms.author="sewhee"
/>

# <a name="using-azure-dns-with-other-azure-services"></a>Verwendung von Azure DNS mit anderen Azure services

Azure DNS wird eine gehostete DNS-Verwaltung und Namen Auflösung. Ermöglicht das Erstellen von öffentlicher DNS-Namen für die auch die von Ihnen bereitgestellte in Azure. Erstellen einen Namen für eine Azure Service in Ihre benutzerdefinierte Domain ist so einfach wie das Hinzufügen eines Datensatzes vom richtigen Typ für Ihren Dienst.

* Dynamisch zugewiesene IP-Adressen müssen Sie DNS CNAME-Eintrag erstellen, der den DNS-Namen zuordnet, die Azure für Ihren Dienst erstellt. DNS-Standards verhindern Sie einen CNAME-Eintrag für Zone Apex.
* Statisch zugewiesene IP-Adressen können Sie mit einem beliebigen Namen, einschließlich _naked_ Domänenname in der Zone Spitze einen DNS A-Datensatz erstellen.

Der folgenden Tabelle werden die unterstützten Datensatztypen, die für verschiedene Azure verwendet werden können. Diese Tabelle entnehmen können, unterstützt Azure DNS nur DNS-Einträge für Internetzugriff Netzwerkressourcen. Azure DNS kann für Auflösung internal, private Adressen verwendet werden.

| Azure Service | Netzwerkschnittstelle | Beschreibung |
|---------------|-------------------|-------------|
| Application Gateway | Front-End-öffentliche IP-Adresse | Sie können einen DNS A oder CNAME-Datensatz erstellen. |
| Lastenausgleich | Front-End-öffentliche IP-Adresse | Sie können einen DNS A oder CNAME-Datensatz erstellen. Lastenausgleich können IPv6-öffentliche IP-Adresse, die dynamisch zugewiesen wird. Daher müssen Sie einen CNAME-Eintrag für eine IPv6-Adresse erstellen. |
| Traffic Manager | Öffentlicher name | Sie können nur einen CNAME erstellen, die den Namen trafficmanager.net Traffic Manager-Profil zugeordnet ist. Weitere Informationen finden Sie unter [Wie Traffic Manager](../traffic-manager/traffic-manager-how-traffic-manager-works.md#traffic-manager-example). |
| Cloud-Dienst | Öffentliche IP-Adresse | Statisch zugewiesene IP-Adressen können Sie einen DNS A-Datensatz erstellen. Dynamisch zugewiesene IP-Adressen müssen Sie einen CNAME-Eintrag erstellen, der den _cloudapp.net_ Namen zugeordnet ist. Diese Regel gilt für VMs im klassischen Portal erstellt werden, da sie als Cloud-Dienst bereitgestellt werden. Weitere Informationen finden Sie unter [Configure Domainnamen in Cloud-Dienste](../cloud-services/cloud-services-custom-domain-name-portal.md). |
| App Service | Externe IP-Adresse | Externe IP-Adressen können Sie einen DNS A-Datensatz erstellen. Andernfalls müssen Sie einen CNAME-Eintrag erstellen, der den *.azurewebsites.NET Namen zugeordnet ist. Weitere Informationen finden Sie in der [Karte eines benutzerdefinierten Domänennamen auf eine Azure-Anwendung](../app-service-web/web-sites-custom-domain-name.md) |
| Ressourcenmanager VMs | Öffentliche IP-Adresse | Ressourcenmanager VMs können öffentliche IP-Adressen. Ein virtueller Computer mit einer öffentlichen IP-Adresse möglicherweise hinter einem Lastenausgleich. Sie können einen DNS A oder CNAME-Datensatz für die öffentliche Adresse erstellen. Dieser benutzerdefinierte Name kann verwendet werden, die VIP Lastenausgleich umgehen. |
| Klassische VMs | Öffentliche IP-Adresse | Klassische VMs mit PowerShell erstellt oder CLI kann eine dynamische oder statische (reserviert) virtuellen Adresse konfiguriert werden. Sie können DNS CNAME oder einen Datensatz erstellen. |
