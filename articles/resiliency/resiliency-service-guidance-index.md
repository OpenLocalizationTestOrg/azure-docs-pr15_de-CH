<properties
   pageTitle="Service Resiliency Hinweise | Microsoft Azure"
   description="Links zu Disaster Recovery und proaktive Resiliency und Verfügbarkeit Anleitung für Microsoft Azure Services."
   services=""
   documentationCenter="na"
   authors="adamglick"
   manager="saladki"
   editor=""/>

<tags
   ms.service="resiliency"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/18/2016"
   ms.author="aglick"/>

# <a name="microsoft-azure-service-resiliency-guidance"></a>Microsoft Azure Service Resiliency Anleitung
Microsoft Azure soll Sie mit den Ressourcen, wenn Sie sie benötigen. Als Teil des gutes Design und Betriebsverfahren sollten Sie wissen sowohl Azure Dienste zu hoher Verfügbarkeit entwerfen sowie Wenn eine Unterbrechung des Dienstes die Anwendung beeinflusst. Um Sie dabei zu unterstützen, enthält das Dokument Links zu Disaster Recovery Anleitung Designrichtlinien für verschiedene Azure.

##<a name="disaster-recovery-guidance"></a>Disaster Recovery-Hinweise
Disaster Recovery Anleitung links unten finden Sie die Informationen zu die Anwendung wieder online schnell, wenn Sie eine Azure Service-Unterbrechung beeinflusst. Diese Links wurden entwickelt, um Sie bei der Frage, "Ich bin wird beeinflusst durch eine Unterbrechung Azure Service Was kann ich tun?"

##<a name="design-guidance"></a>Designrichtlinien
Design Anleitung Links unten sind Entwurfs- und Architekturrichtlinien um besser zu verstehen wie jede Azure Service auf eine Weise verwenden, die maximiert die Verfügbarkeit der Anwendung erstellt wurde. Diese Links wurden entwickelt, um Sie bei der Frage "Wie kann ich einen Fehler, Hardwarefehler, serviceunterbrechung oder andere Fehler die allgemeine Verfügbarkeit der Anwendung auswirken wird nicht unbedingt machen?" Ist keine bestimmte Anleitung für Dienst derzeit suchen, möglicherweise [hohe Verfügbarkeit bei Microsoft Azure](./resiliency-high-availability-azure-applications.md) Artikel Weitere Informationen, die Sie in Ihrem Entwurf unterstützen. 

##<a name="service-guidance"></a>Hinweise zu Diensten
| Dienst  | Disaster Recovery-Hinweise | Designrichtlinien |
|:---------|:--------------------------:|:------------------:|
| [Cloud-Dienste] (https://azure.microsoft.com/services/cloud-services/ "Azure Cloud Services")       | [Link] (../cloud-services/cloud-services-disaster-recovery-guidance.md "Azure Cloud Services Disaster Recovery Anleitung")   | Nicht verfügbar |
| [Key Vault] (https://azure.microsoft.com/services/key-vault/ "Azure-Tresor")                      | [Link] (../key-vault/key-vault-disaster-recovery-guidance.md "Azure Key Vault Disaster Recovery Anleitung")        | Nicht verfügbar |
| [Speicher] (https://azure.microsoft.com/services/storage/ "Azure-Speicher")                            | [Link] (../storage/storage-disaster-recovery-guidance.md "Azure Storage Disaster Recovery Anleitung")          | Nicht verfügbar |
| [SQL-Datenbanken] (https://azure.microsoft.com/services/sql-database/ "SQL Azure-Datenbanken")           | [Link] (../sql-database/sql-database-disaster-recovery.md  "Azure SQL-Datenbank Disaster Recovery Anleitung")    | [Link] (../sql-database/sql-database-business-continuity.md "Übersicht über Business Continuity mit Azure SQL-Datenbank") |
| [Virtuelle Computer] (https://azure.microsoft.com/services/virtual-machines/ "Azure Virtual Machines") | [Link] (../virtual-machines/virtual-machines-disaster-recovery-guidance.md "Azure Virtual Machines Disaster Recovery Anleitung") | Nicht verfügbar |
| [Virtuelles Netzwerk] (https://azure.microsoft.com/services/virtual-network/ "Azure Virtual Network")    | [Link] (../virtual-network/virtual-network-disaster-recovery-guidance.md "Azure Virtual Network Disaster Recovery Anleitung")  | Nicht verfügbar |

##<a name="next-steps"></a>Nächste Schritte
Wenn Sie Informationen, die Allgemein auf Systeme und Lösungen suchen, lesen Sie [Disaster Recovery und hohe Verfügbarkeit bei Microsoft Azure](https://aka.ms/drtechguide).
