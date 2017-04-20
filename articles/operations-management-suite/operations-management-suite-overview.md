<properties
   pageTitle="Operations Management Suite (OMS) Übersicht | Microsoft Azure"
   description="Microsoft Operations Management Suite (OMS) ist Microsoft Cloud-basierten IT Management Lösung, verwalten und schützen Ihre lokalen und cloud-Infrastruktur.  In diesem Artikel identifiziert OMS andere Dienste und enthält Links zu ausführlichen Inhalt."
   services="operations-management-suite"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="bwren" />

# <a name="what-is-operations-management-suite-oms"></a>Was ist Operations Management Suite (OMS)?

Microsoft Operations Management Suite (OMS) ist Microsoft Cloud-basierten IT Management Lösung, verwalten und schützen Ihre lokalen und cloud-Infrastruktur.  Da OMS als Cloud-basierten Dienst implementiert ist, haben es läuft schnell bei minimaler Investition in Infrastruktur-Services Sie.  Neue Features werden automatisch speichern Wartung und Kosten aktualisieren.

Neben wertvolle Dienste selbst integrieren OMS mit System Center-Komponenten wie System Center Operations Manager Ihre Management-Investitionen in die Cloud erweitern.  System Center und OMS können gemeinsam eine vollständige Hybrid Management bereitzustellen.

Die folgenden Abschnitte enthalten umfassende Beschreibung der anderen Wert OMS und den Diensten, die sie implementieren.  OMS-Architektur, eine Übersicht über die verschiedenen OMS-Komponenten finden Sie vor die detaillierte Dokumentation zu überprüfen.


## <a name="insight-and-analyticsmediaoperations-management-suite-overviewicon-insight-analyticspng-insight-and-analytics"></a>![Einblicke und Analysen](media/operations-management-suite-overview/icon-insight-analytics.png) Einblicke und Analysen

[Protokollanalyse](http://azure.microsoft.com/documentation/services/log-analytics) können Sie sammeln, Korrelieren, suchen und bearbeiten protokollieren und Leistung von Betriebssystemen und Anwendungsprogrammen generiert. Es gibt in Echtzeit betriebliche Informationen mit Such- und benutzerdefinierte Dashboards Millionen von Datensätzen für alle Arbeitslasten und Server unabhängig von deren Standort leicht analysieren.

Sie können problemlos hinzufügen Solutions Protokollanalyse, die zu sammelnden Daten festlegen und die Logik für die Analyse.  Solutions können weitere Funktionen enthalten, die automatisch mit minimalem oder keine Konfiguration bereitgestellt.  Neben einzelnen Lösungen Analysetools, können Sie benutzerdefinierte Suchvorgänge über das gesamte Dataset ausführen, um Daten zwischen Systemen und Applikationen zu korrelieren.  


## <a name="automation--controlmediaoperations-management-suite-overviewicon-automation-controlpng-automation--control"></a>![Automatisierung und Kontrolle](media/operations-management-suite-overview/icon-automation-control.png) Automatisierung und Kontrolle

Azure Automatisierung automatisiert administrative mit [Runbooks](../automation/automation-runbook-types.md) , die PowerShell basiert und in der Azure-Cloud ausgeführt.  Runbooks möglich, Produkte oder Dienstleistungen mit PowerShell einschließlich Ressourcen in anderen Wolken wie Amazon Web Services (AWS) verwaltet werden kann.  Runbooks kann auch auf einem Server im lokalen Rechenzentrum verwalten lokale Ressourcen ausgeführt werden.

Azure Automation bietet Konfiguration mit [PowerShell DSC](../automation/automation-dsc-overview.md).  Erstellen und verwalten DSC in Azure gehostet, und Cloud und lokale Systeme definieren und erzwingen automatisch die Konfiguration zuweisen.


## <a name="protection-and-recoverymediaoperations-management-suite-overviewicon-protection-recoverypng-protection-and-disaster-recovery"></a>![Schutz und Recovery](media/operations-management-suite-overview/icon-protection-recovery.png) Schutz und Disaster Recovery

[Azure Backup](http://azure.microsoft.com/documentation/services/backup) schützt Ihre Anwendungsdaten und behält Jahren ohne große Investitionen und geringen Betriebskosten.  Sie können Daten aus physischen und virtuellen Windows Server neben Anwendungsworkloads wie SQL Server und SharePoint sichern.  Sie können auch von System Center Data Protection Manager (DPM) verwendet werden geschützte Daten in Azure für Redundanz und langfristigen Speicherung replizieren.

[Azure Site Recovery](http://azure.microsoft.com/documentation/services/site-recovery) trägt durch Replikation, Failover und Recovery von lokalen Hyper-V virtuelle Computer, virtuelle VMware-Maschinen und Windows-Linux-Servern orchestriert die Geschäftskontinuität und einer Wiederherstellungsstrategie (BCDR). Maschinen auf einem sekundären Rechenzentrum replizieren oder Rechenzentrum replizieren auf Azure erweitern können. Site Recovery bietet auch einfache Failover und Recovery für Arbeitslasten. Disaster Recovery-Mechanismen wie SQL Server AlwaysOn integriert und einfachen Failover Arbeitslasten auf mehreren Computern tiered Recovery-Pläne bereit.


## <a name="oms-security-and-compliancemediaoperations-management-suite-overviewicon-security-compliancepng-security-and-compliance"></a>![OMS-Sicherheit und Compliance](media/operations-management-suite-overview/icon-security-compliance.png) Sicherheit und Compliance
Sicherheit und Kompatibilität können Sie identifizieren, bewerten und entschärfen Sie Sicherheitsrisiken für Ihre Infrastruktur.  Diese Funktionen von OMS werden über mehrere Projektmappen Protokollanalyse implementiert, die Daten und Systeme bei der laufenden Sicherung Ihrer Umgebung Konfiguration analysieren.

- Die [Sicherheits- und Lösung](oms-security-getting-started.md ) sammelt und analysiert Sicherheitsereignisse auf verwalteten Systemen, verdächtigen Aktivitäten zu erkennen.
- [Antimalware-Lösung](log-analytics-malware.md ) zeigt den Status der Malware-Schutz auf verwalteten Systemen.  
- Systemupdates Lösung führt eine Analyse der Sicherheitsupdates und andere auf Ihren verwalteten Systemen aktualisiert, sodass problemlos Systeme identifizieren benötigen Patches.


## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie [Protokollanalyse](http://azure.microsoft.com/documentation/services/log-analytics).
- Erfahren Sie mehr über [Azure Automation](../automation/automation-intro.md).
- Erfahren Sie mehr über [Azure Backup](http://azure.microsoft.com/documentation/services/backup).
- Erfahren Sie mehr über [Azure Site Recovery](http://azure.microsoft.com/documentation/services/site-recovery).
