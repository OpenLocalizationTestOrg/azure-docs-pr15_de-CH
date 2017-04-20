
<properties 
    pageTitle="Optionen für aus Azure RemoteApp | Microsoft Azure" 
    description="Erfahren Sie mehr über die Optionen für aus Azure RemoteApp." 
    services="remoteapp" 
    documentationCenter="" 
    authors="ericorman" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/06/2016" 
    ms.author="elizapo" />

# <a name="options-for-migrating-out-of-azure-remoteapp"></a>Optionen für aus Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp wird eingestellt. Lesen Sie die [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) für Details.

Wenn Sie aufgehört haben Azure RemoteApp wegen [Pensionierung Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) oder Evaluierung abgeschlossen haben, müssen Sie zu einer anderen app Service von Azure RemoteApp migrieren. Es gibt zwei verschiedene Ansätze für die Migration: einen selbstverwalteten (oft als Infrastruktur als Dienst [IaaS]) Bereitstellung oder eine vollständig verwaltete (häufig als Plattform als Dienst) oder Software als Service [PaaS/SaaS] bietet. 

IaaS Self-service ist eine Yourself Bereitstellung, die verwaltet, betrieben und Besitz, direkt auf virtuellen Maschinen (VMs) oder physischen Systemen bereitgestellt. Am anderen Ende eine vollständig verwaltete PaaS/SaaS-Lösung ist eher Azure RemoteApp - Partner bietet einen Service auf einer Remotinglösung Betrieb behandelt und Wartung, während Sie als Kunde sind Bild und Anwendung verwaltet.

Weitere Informationen und Beispiele für die verschiedenen Hostingoptionen lesen.    

## <a name="self-managed-iaas-solutions"></a>Selbstmanagement (IaaS) solutions

### <a name="rds-on-iaas"></a>**RDS auf IaaS** 
Bereitstellen eine einheitlichen sitzungsbasierten Remote Desktop Services (in Windows Server) Umgebung mit RemoteApp und Desktop-Systeme lokal oder in einer gehosteten Umgebung (wie in Azure VMs). RDS IaaS-Bereitstellung für Kunden, die bereits mit vertraut sind und vorhandenes Fachwissen mit RDS-Bereitstellungen. 

> [AZURE.NOTE]
> Sie benötigen Volumenlizenzierung mit Software Assurance (SA) für RDS-Clientzugriffslizenzen diese Bereitstellungsmethode verwenden.

Bereitstellung von RDS unter Azure VMs wird einfacher Bereitstellung und Patches Vorlagen (eine [Übersicht](https://blogs.technet.microsoft.com/enterprisemobility/2015/07/13/azure-resource-manager-template-for-rds-deployment/) und dann [Gehen sie](https://aka.ms/rdautomation)lesen) verwenden. Die gleichen elastischen Skalierung Funktionen mit klassischen Azure-Bereitstellung Modellressourcen (nicht Azure Ressourcenmodell) in Azure RemoteApp erhalten mithilfe der [automatischen Skalierung Skript](https://gallery.technet.microsoft.com/scriptcenter/Automatic-Scaling-of-9b4f5e76), weitere Anpassungen und Konfigurationen. Beim Bereitstellen von RDS unter Azure VMs ist durch [Azure unterstützt](https://azure.microsoft.com/support/plans/)dieselbe Supportmitarbeiter unterstützt, die Azure RemoteApp unterstützt. Sie können erhalten Kosten basierend auf der vorhandenen Verwendung von [Azure](https://azure.microsoft.com/support/plans/)Kundendienst oder Berechnungen selbst ausführen mit einer demnächst Kostenrechner.  Fügen Sie auch mit N-Serie VMs (derzeit in privaten Vorschau) vGPU - vGPU hinzufügen und die Informationen in unseren entzünden Sitzung zu [RDS Verbesserung Windows Server 2016](https://myignite.microsoft.com/videos/2794) hören.   

Wir haben Schritt Bereitstellungshandbücher für [Windows Server 2012 R2](http://aka.ms/rdsonazure) und [Windows Server 2016](http://aka.ms/rdsonazure2016) bei der Bereitstellung. [Remotedesktop Blog](https://blogs.technet.microsoft.com/enterprisemobility/?product=windows-server-remote-desktop-services) Neuigkeiten anzeigen
 
### <a name="citrix-on-iaas"></a>**Citrix auf IaaS** 
Eine systemeigene Citrix XenApp sitzungsbasierte oder XenDesktop Bereitstellung kann lokal bereitgestellt oder in einer gehosteten Umgebung (z. B. Azure VMs). 

Die schrittweise Bereitstellungshandbuch [Citrix XA 7.6 Azure](http://www.citrixandmicrosoft.com/Documents/Citrix-Azure Deployment Guide-v.1.0.docx)Weitere Informationen anzeigen Weitere Informationen zu [Citrix in Azure](http://www.citrixandmicrosoft.com/Solutions/AzureCloud.aspx), einschließlich Taschenrechners Preis. Sie auch finden [wenden Sie sich an Citrix](http://citrix.com/English/contact/index.asp) Ihre Optionen zu.

## <a name="fully-managed-paassaas-offerings"></a>Vollständig verwaltete (PaaS/SaaS) Angebote

### <a name="citrix-cloud"></a>**Citrix Cloud** 
[Citrix vorhandenen Cloud-Lösung](https://www.citrix.com/products/citrix-cloud/)mit architektonisch Citrix XenApp Express. Citrix bietet [50 % Rabatt Promotion](https://www.citrix.com/blogs/2016/10/03/special-promotion-for-microsoft-azure-remoteapp-customers/) Azure RemoteApp-Kunden. 

### <a name="citrix-xenapp-express-in-tech-preview"></a>**Citrix XenApp Express (Tech Preview)**
[Registrieren Sie ihre technischen Vorschau](http://now.citrix.com/remoteapp), woraufhin die [Sitzung entzünden](https://myignite.microsoft.com/videos/2792) (ab 20:30 Minuten). XenApp Express entspricht architektonisch Citrix Cloud außer es enthält vereinfachtes Management UI und andere Features und Funktionen Azure RemoteApp ähnelt. 

Erfahren Sie mehr über [Citrix XenApp Express](http://now.citrix.com/remoteapp).   

### <a name="citrix-service-provider-program"></a>**Citrix Service Provider Program** 
Citrix Service Provider Program erleichtert Dienstanbieter übermitteln die Einfachheit der virtuellen Cloud SMBs, sie einfach, Pay Modell gewünschten Dienste anbieten. Citrix-Dienstanbieter Microsoft SPLA Wachstum und Investitionen Plattform RDS Geräte erweitern, Zugriff, größte Anwendungssupport, eine Erfahrung, Sicherheit und erhöhter Skalierbarkeits-. Wiederum Citrix-Dienstanbieter gewinnen mehr Kundenzufriedenheit und ihre Betriebskosten senken. [Weitere](http://www.citrix.com/products/service-providers.html) oder [Partner](https://www.citrix.com/buy/partnerlocator.html).

### <a name="microsoft-hosted-service-provider"></a>**Microsoft Hosted Service Provider** 
Hosting bieten in der Regel eine vollständig verwaltete Windows Desktop gehostet und Anwendungsdienst, z. B. Verwalten von Azure-Ressourcen, Betriebssysteme, Applikationen und Helpdesk mit dem Partner die Lizenzverträge mit Microsoft und anderen Softwareanbietern zusammen mit einem Service Provider License Agreement zu vertreibt Abonnenten Access License (SAL). Informationen und Kontaktdaten für einige Hoster, die Kunden bei der Migration Azure RemoteApp spezialisiert. Überprüfen Sie [die aktuelle Liste der Dienstanbieter gehostet](http://aka.ms/rdsonazurecertified) , die die RDS IaaS learning Path und Bewertung abgeschlossen haben.  

#### <a name="aspex"></a>**ASPEX**
[ASPEX](http://www.aspex.be/en) Übergang in die Cloud ISVs und ISV spezialisiert "auf um ihre aktuellen Cloud-Setups zu optimieren. ASPEX bietet ein breites Spektrum an Dienstleistungen, Devops und Beratung.  

Primärer Standort: Antwerpen, Belgien

Operationsbereichs: Westeuropa

Partner-Status: [Silber](https://partnercenter.microsoft.com/pcv/solution-providers/aspex_9397f5dd-ebdd-405b-b926-19a5bda61f7a/cfe00bac-ea36-4591-a60b-ec001c4c3dff)

Microsoft-Cloud Service Provider: Ja

Sitzungsbasierte RemoteApp und Desktop Lösungen: Ja, beide

Azure RemoteApp Migration Solutions: Ja, [Weitere Informationen](https://www.aspex.be/en/azure-remote-apps)

**Kontakt:**

- Telefon: +3232202198
- E-Mail:[info@aspex.be](mailto:info@aspex.be)
- Web: [http://cloud.aspex.be/contact-ara-0](http://cloud.aspex.be/contact-ara-0)

#### <a name="conexlink-platform-name-mycloudit"></a>**Conexlink (Platform Name: MyCloudIT)**
[MyCloudIT](http://www.mycloudit.com) ist eine Automatisierungsplattform für IT-Unternehmen vereinfachen, Optimierung und Skalierung der Migration und Bereitstellung von Remotedesktops Remoteanwendungen und Microsoft Azure-Cloud-Infrastruktur. 

MyCloudIT-Plattform reduziert die Bereitstellungszeit Azure Kosten um 30 % und 95 % und gesamten IT-Infrastruktur des Kunden in die Cloud innerhalb von wenigen Tastenkombinationen verschoben. Partner können Kunden jetzt von einem globalen Dashboard verwalten, Service auf der ganzen Welt wie nie zuvor, und Steigerung des Umsatzes ohne zusätzlichen Aufwand Azure Schulung.  

Primärer Standort: Dallas, Texas, USA

Operationsbereichs: weltweit

Partner-Status: [Gold](https://partnercenter.microsoft.com/pcv/solution-providers/conexlink_4298787366/843036_1?k=Conexlink)

Microsoft-Cloud Service Provider: Ja

Sitzungsbasierte RemoteApp und Desktop Lösungen: Ja, beide

Azure RemoteApp Migration Solutions: Ja, [Weitere Informationen](https://mycloudit.com/remote-app-microsoft/)

**Kontakt:**
- Brian Garoutte, VP of Business Development

   Telefon: 972-218-0741

   E-Mail:[brian.garoutte@conexlink.com](mailto:brian.garoutte@conexlink.com)

#### <a name="acuutech"></a>**Acuutech**
[Acuutech](http://www.acuutech.com) spezialisiert gehosteten desktop Solutions mit vollständigen Desktop und ISV-Anwendung basiert auf Microsoft Technologie einer globalen Kunden von Azure und ihre Datencenter Erfahrungen.

Primärer Standort: London, Großbritannien; Singapur Houston, TX

Operationsbereichs: weltweit

Partner-Status: Gold

Microsoft-Cloud Service Provider: Ja

Sitzungsbasierte RemoteApp und Desktop Lösungen: Ja, beide

Azure RemoteApp Migration Solutions: Ja, [Weitere Informationen](http://www.acuutech.com/ara-migration/)

**Kontakt:**

- Vereinigtes Königreich:

  Langston Straße 5-6 York House,

  Loughton, Essex IG10 3TQ
  
  Telefon: + 44 (0) 20 8502 2155
 
- Singapur:

  100 Cecil Street, #09-02, 
  
  Globus, Singapur 069532
  
  Telefon: + 65 6709 4933
 
- Nordamerika: 

  3601 S. Sandman str.
  
  Suite 200, Houston, TX 77098
  
  Telefon: +1 713 691 0800

## <a name="need-more-help"></a>Benötigen Sie weitere Hilfe?
Weiterhin benötigen Hilfe bei der Auswahl oder weitere Fragen? Verwenden Sie eine der folgenden Methoden, um Hilfe zu erhalten. 

1.  E-Mail an [arainfo@microsoft.com](mailto:arainfo@microsoft.com).
2.  Kontaktieren Sie [Azure-support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). Öffnen einer [Supportanfrage Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)starten.
3.  Rufen Sie uns an. [Lokale Auftragsnummer suchen](https://azure.microsoft.com/overview/sales-number/).
