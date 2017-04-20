<properties
   pageTitle="Übersicht über die Sicherheit von Azure Virtual Machines | Microsoft Azure"
   description=" Azure Virtual Machines flexibel Virtualisierung ohne zu kaufen die physische Hardware, die den virtuellen Computer ausgeführt wird.  Dieser Artikel enthält einen Überblick über die wichtigsten Azure Sicherheitsfunktionen mit Azure virtuelle Computer verwendet werden können. "
   services="security"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/16/2016"
   ms.author="terrylan"/>

# <a name="azure-virtual-machines-security-overview"></a>Übersicht über Sicherheit von Azure virtuelle Computer

Azure Virtual Machines können Sie eine Vielzahl von computing-Lösungen agile-Methode bereitstellen. Mit Unterstützung für Microsoft Windows, Linux, Microsoft SQL Server, Oracle, IBM, SAP und Azure BizTalk-Dienste können Sie jede Arbeitslast und jede Sprache auf nahezu jedem Betriebssystem bereitstellen.

Ein Azure Virtual Machine Flexibilität der Virtualisierung ohne zu kaufen die physische Hardware, die den virtuellen Computer ausgeführt wird.  Sie können erstellen und Bereitstellen einer Anwendung die Gewissheit, dass Ihre Daten geschützt und sicher in unserem hochsicheren Rechenzentren.

Mit Azure können Sie sicheren, kompatiblen Lösungen, die erstellen:

- Schützen Sie Ihre virtuellen Computer vor Viren und malware
- Verschlüsseln Sie vertrauliche Daten
- Sichern des Netzwerkdatenverkehrs
- Identifizieren und Erkennen von Risiken
- Erfüllung behördlicher Auflagen

Das Ziel dieses Artikels ist eine Übersicht über die wichtigsten Azure Sicherheitsfunktionen bereitstellen, die mit virtuellen Computern verwendet werden kann. Wir bieten auch Links zu Artikeln, die alle Details der Features bieten, so informieren.  

Die Core Azure Virtual Machine-Sicherheitsfunktionen, die in diesem Artikel behandelt:

- Antimalware
- Hardwaresicherheitsmoduls
- Verschlüsselung von virtuellen Festplatten
- Backups virtueller Maschinen
- Azure Site Recovery
- Virtuelle Netzwerke
- Verwaltung von Sicherheitsrichtlinien und reporting
- Compliance

## <a name="antimalware"></a>Antimalware

Mit Azure können Antischadsoftware von Sicherheitsanbietern wie Microsoft, Symantec, Trend Micro, McAfee und Kaspersky die virtuellen Computer schädliche Dateien, Adware und andere Gefahren schützen. Im Abschnitt Weitere Informationen zu Artikeln Partner Solutions.

Microsoft Antimalware Azure Cloud Services und virtuellen Computern ist eine Echtzeitschutz-Funktion, die erkennen und Entfernen von Viren, Spyware und andere bösartige Software.  Microsoft Antimalware bietet konfigurierbare Alarm meldet, wenn bekannte schädliche oder unerwünschte Software versucht, installiert oder auf den Azure-Systemen ausgeführt.

Microsoft Antimalware ist eine Single-Agent-Lösung für Applikationen und Tenant-Umgebung im Hintergrund ohne Benutzereingriffe ausgeführt. Sie können Schutz Bedarf der Anwendungslasten mit beiden grundlegenden secure standardmäßig oder erweiterte Konfiguration einschließlich Antimalware Überwachung bereitstellen.

Beim Bereitstellen und Microsoft Antimalware aktivieren stehen die folgenden wichtigsten Funktionen:

- Echtzeitschutz - Monitore Aktivität im Cloud-Dienste und virtuelle Computer erkennen und Blockieren von Malware Ausführung.
- Regelmäßig führt geplante Scan - gezielte Überprüfung zum Erkennen von Malware, einschließlich Programme aktiv.
- Malware Remediation - Ausführung automatische von Aktionen auf gefundene Malware löschen bösartige Dateien isolieren oder Entfernen von bösartigen Registrierungseinträgen.
- Signaturupdates – automatisch installiert die neuesten Schutzsignaturen (Virendefinitionen) Schutz ist aktuell festgelegten Häufigkeit.
- Antimalware-Modul – automatisch aktualisiert Microsoft Antimalware Engine aktualisiert.
- Antimalware-Plattform – automatisch aktualisiert Microsoft Antimalware-Plattform aktualisiert.
- Aktiver Schutz - meldet Azure Telemetrie Metadaten zu erkannten Gefahren und verdächtige Ressourcen schnelle Reaktion und ermöglicht in Echtzeit synchrone Signatur durch Microsoft Active Protection System (MAPS).
- Beispiele für reporting - bietet und meldet Beispiele Microsoft Antimalware-Dienst verfeinern des Dienstes und Problembehandlung zu aktivieren.
- Ausschlüsse – Anwendung und Administratoren Konfigurieren bestimmter Dateien, Prozesse und Laufwerke für Schutz und Leistung und anderem Scannen ausgeschlossen.
- Ereignissammlung Antimalware - Antimalware-Dienststatus, verdächtige Aktivitäten und Remediation-Maßnahmen im Betriebssystem-Ereignisprotokoll aufgezeichnet und in Azure Storage Debitorenkonto erfasst.

Weitere Informationen: über Antischadsoftware zu virtuellen Computern finden Sie unter:

- [Microsoft Antimalware Azure Cloud Services und virtuellen Maschinen](../security/azure-security-antimalware.md)
- [Antimalware Lösungen auf Azure Virtual Machines](https://azure.microsoft.com/blog/deploying-antimalware-solutions-on-azure-virtual-machines/)
- [Zum Installieren und Konfigurieren von Trend Micro Deep Security als Dienst auf einem Windows-VM](../virtual-machines/virtual-machines-windows-classic-install-trend.md)
- [Zum Installieren und Konfigurieren von Symantec Endpoint Protection auf einer Windows-VM](../virtual-machines/virtual-machines-windows-classic-install-symantec.md)
- [Neue Antimalware-Optionen zum Schutz von Azure Virtual Machines – McAfee Endpoint Protection](https://azure.microsoft.com/blog/new-antimalware-options-for-protecting-azure-virtual-machines/)
- [Sicherheitsfunktionen in Azure Marketplace](https://azure.microsoft.com/marketplace/?term=security)

## <a name="hardware-security-module"></a>Hardware Security Module

Verschlüsselung und Authentifizierung verbessern nicht Sicherheit, wenn die Tasten geschützt sind. Sie können die Verwaltung und Sicherheit Ihrer kritischen Kennwörter und Schlüssel speichern in Azure Key Vault vereinfachen. Key Vault bietet die Möglichkeit, Ihre Schlüssel im Hardwaresicherheitsmodule (HSMs) zertifiziert FIPS 140-2 Level 2-Standards. Verschlüsselungsschlüssel SQL Server-Sicherung oder [transparente Verschlüsselung](https://msdn.microsoft.com/library/bb934049.aspx) können alle im Schlüsseltresor Schlüssel oder geheime Informationen von der Anwendung gespeichert werden. Berechtigungen und Zugriff auf die geschützten Elemente werden durch [Active Directory Azure](https://azure.microsoft.com/documentation/services/active-directory/)verwaltet.

Weitere Informationen:

- [Was ist Azure Key Vault?](../key-vault/key-vault-whatis.md)
- [Erste Schritte mit Azure Schlüssel](../key-vault/key-vault-get-started.md)
- [Azure Key Vault-blog](https://blogs.technet.microsoft.com/kv/)

## <a name="virtual-machine-disk-encryption"></a>Verschlüsselung von virtuellen Festplatten

Azure Disk Encryption ist eine neue Funktion, mit der Sie Ihre Datenträger Windows und Linux Azure Virtual Machine verschlüsseln kann. Azure verschlüsselt Datenträger die Branche [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) Standardfunktion von Windows und die Funktion [dm-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) Linux Volume-Verschlüsselung für das Betriebssystem und die Datenträger.

Die Lösung ist integriert mit Azure Schlüssel zum Steuern und Verwalten der Datenträger Verschlüsselungsschlüssel und Geheimnisse in Ihrem Abonnement Key Vault dabei sicherzustellen, dass alle virtuellen Laufwerke ruhende in Azure-Speicher verschlüsselt werden.

Weitere Informationen:

- [Azure Verschlüsselung Windows und Linux IaaS VMs](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0)
- [Azure Verschlüsselung Linux- und Windows-VMs](https://blogs.msdn.microsoft.com/azuresecurity/2015/11/16/azure-disk-encryption-for-linux-and-windows-virtual-machines-public-preview-now-available/)
- [Verschlüsseln von einem virtuellen Computer](../security-center/security-center-disk-encryption.md)

## <a name="virtual-machine-backup"></a>Backups virtueller Maschinen

Azure Backup ist eine skalierbare Lösung, die Ihre Anwendungsdaten Investitionen und geringen Betriebskosten schützt. Anwendungsfehler können Ihre Daten beschädigen und menschliche Fehler können Fehler in der Anwendung führen. Azure-Sicherung werden die virtuellen Computer unter Windows und Linux geschützt.

Weitere Informationen:

- [Was ist Azure Backup?](../backup/backup-introduction-to-azure-backup.md)
- [Azure Backup Learning Path](https://azure.microsoft.com/documentation/learning-paths/backup/)
- [Azure Backup Service - FAQ](../backup/backup-azure-backup-faq.md)

## <a name="azure-site-recovery"></a>Azure Site Recovery

Ein wichtiger Bestandteil Ihrer Organisation BCDR Strategie ist herauszufinden, wie zu Unternehmen Arbeitslasten und apps und bei geplante und ungeplante Ausfällen ausgeführt. Azure Site Recovery unterstützt Replikation, Failover und Recovery von Arbeitslasten und apps koordinieren, damit sie stehen einem sekundären Standort, wenn der primäre Standort ausfällt.

Site Recovery:

- **Vereinfacht die BCDR Strategie** -Site Recovery vereinfacht die Replikation, Failover und Recovery mehrerer Arbeitslasten in Ihrem Unternehmen und apps von einem einzigen Standort behandelt. Site Recovery koordiniert, Replikation und Failover jedoch nicht Ihre Anwendung Daten oder Informationen darüber.
- **Bietet flexible Replikation** – mithilfe von Site Recovery können Arbeitslasten auf Hyper-V virtuelle Computer, virtuelle VMware-Maschinen und Windows-Linux-Servern repliziert.
- **Unterstützt Failover und Recovery** -Site Recovery bietet Test-Failover Unterstützung Disaster Recovery Bohrer ohne Produktion. Sie können auch geplanten Failover mit Null Datenverlust für erwartete Ausfälle oder ungeplante Failovers mit minimalem Datenverlust (je nach Häufigkeit der Replikation) für unerwarteten Notfällen ausführen. Können Sie nach einem Failover Failback primäre Standorte. Site Recovery bietet Recovery-Pläne, die Skripts und Azure Automation Arbeitsmappen können Failover- und Multi-Tier-Anwendung anpassen können.
- **Sekundäre Datencenter eliminiert** , lokalen sekundären Standort und Azure replizieren. Mithilfe von Azure als Ziel für die Wiederherstellung eliminiert Kosten und Komplexität der Verwaltung eines sekundären Standorts. Replizierte Daten werden in Azure-Speicher gespeichert.
- **Integriert mit vorhandenen BCDR** -Site Recovery arbeitet mit anderen BCDR Funktionen. Site Recovery können Sie um SQL Server-Back-End Unternehmens Arbeitslasten zu schützen. Dies umfasst systemeigene Unterstützung für SQL Server AlwaysOn Failover Verfügbarkeitsgruppen verwalten.

Weitere Informationen:

- [Was ist Azure Site Recovery?](../site-recovery/site-recovery-overview.md)
- [Funktionsweise von Azure Site Recovery](../site-recovery/site-recovery-components.md)
- [Was Arbeitslasten von Azure Site Recovery geschützt werden?](../site-recovery/site-recovery-workload.md)

## <a name="virtual-networking"></a>Virtuelle Netzwerke

Virtuelle Computer benötigen Netzwerkkonnektivität. Um diese Anforderung zu unterstützen, muss Azure virtuelle Computer ein virtuelles Azure-Netzwerk verbunden werden. Ein Azure Virtual Network ist ein logischer Konstrukt auf die physische Netzwerkstruktur Azure erstellt. Jede logische Azure Virtual Network ist von allen anderen virtuellen Azure-Netzwerken isoliert. Diese Isolierung hilft sicherzustellen, dass Netzwerkverkehr in der Bereitstellung nicht anderen Kunden Microsoft Azure.

Weitere Informationen:

- [Übersicht über die Sicherheit von Azure-Netzwerk](security-network-overview.md)
- [Virtuelles Netzwerk (Übersicht)](../virtual-network/virtual-networks-overview.md)
- [Netzwerkfunktionen und Partnerschaften Unternehmensszenarien](https://azure.microsoft.com/blog/networking-enterprise/)

## <a name="security-policy-management-and-reporting"></a>Verwaltung von Sicherheitsrichtlinien und reporting

Azure Security Center können Sie verhindern, erkennen und reagieren auf Gefahren und erhöhte Transparenz, und Kontrolle über die Sicherheit von Azure Ressourcen bietet. Bietet integrierte Überwachung und Policy-Management über Abonnements Azure, Gefahren, die andernfalls vielleicht übersehen würden und arbeitet mit einem ausgedehnten System Security Solutions hilft bei der Ermittlung.

Azure Security Center können Sie optimieren und Sicherheit von virtuellen Computer überwachen:

- Mit virtuellen [Sicherheitsaspekte](../security-center/security-center-recommendations.md) wie System installieren, ACLs Endpunkte konfigurieren Antimalware aktivieren, können Netzwerk-Sicherheitsgruppen und datenträgerverschlüsselung angewendet.
- Überwachen des Status virtueller Maschinen

Weitere Informationen:

- [Einführung in Azure Security Center](../security-center/security-center-intro.md)
- [Häufig gestellte Fragen zum Azure Security Center](../security-center/security-center-faq.md)
- [Azure Security Center Planung und Betrieb](../security-center/security-center-planning-and-operations-guide.md)

## <a name="compliance"></a>Compliance

Azure Virtual Machines ist zertifiziert für FedRAMP, FISMA, HIPAA, PCI DSS Level 1 und andere wichtige Compliance-Programme. Die Zertifizierung vereinfacht Azure Anwendung Auflagen zu erfüllen und Ihr Unternehmen eine Vielzahl von nationalen und internationalen Vorschriften.

Weitere Informationen:

- [Microsoft-Sicherheitscenter: Compliance](https://www.microsoft.com/TrustCenter/Compliance/default.aspx)
- [Vertrauenswürdige Cloud: Microsoft Azure-Sicherheit, Datenschutz und Compliance](http://download.microsoft.com/download/1/6/0/160216AA-8445-480B-B60F-5C8EC8067FCA/WindowsAzure-SecurityPrivacyCompliance.pdf)
