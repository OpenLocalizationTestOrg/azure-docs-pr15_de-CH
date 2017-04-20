<properties
   pageTitle="Schützen Ihre virtuellen Computer in Azure Security Center | Microsoft Azure"
   description="Dieses Dokument Adressen Recommendations in Azure Security Center, mit denen Sie Ihre virtuellen Computer schützen und unter Einhaltung von Sicherheitsrichtlinien bleiben."
   services="security-center"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/25/2016"
   ms.author="terrylan"/>

# <a name="protecting-your-virtual-machines-in-azure-security-center"></a>Schützen Ihre virtuellen Computer in Azure Security Center

Azure Security Center analysiert den Sicherheitsstatus Ihrer Azure-Ressourcen. Beim Sicherheitscenter Sicherheitslücken identifiziert, erstellt Vorschläge, die Sie schrittweise durch den Prozess des Konfigurierens benötigten Steuerelemente.  Vorschläge gelten für Azure Ressourcentypen: virtuelle Maschinen (VMs) Netzwerken, SQL und Applikationen.

Dieser Artikel behandelt die empfohlenen VMs zuweisen.  VM Recommendations Mittelpunkt Datensammlung System aktualisieren, Antimalware, Bereitstellung verschlüsseln VM Datenträgern und mehr.  Verwenden Sie die Tabelle als Referenz erfahren Sie verfügbaren VM Recommendations und was jede bei Anwendung.

## <a name="available-vm-recommendations"></a>Verfügbare VM-Empfehlung

|Empfehlung|Beschreibung|
|-----|-----|
|[Datensammlung für Abonnements aktivieren](security-center-enable-data-collection.md)|Empfiehlt, dass Sie Datensammlung in der Sicherheitsrichtlinie für Ihre Abonnements sowie alle virtuellen Maschinen (VMs) in Ihre Abonnements aktivieren.|
|[Betriebssystem-Sicherheitslücken beseitigen](security-center-remediate-os-vulnerabilities.md)|Ausrichten die OS-Konfigurationen mit Regeln empfohlene Konfiguration empfiehlt z. B. erlauben nicht Kennwörter gespeichert werden.|
|[System-Updates anwenden](security-center-apply-system-updates.md)|Empfiehlt, dass Sie fehlende Sicherheit und wichtige Updates auf VMs bereitstellen.|
|[Neustart nach System-updates](security-center-apply-system-updates.md#reboot-after-system-updates)|Empfiehlt, Neustart virtueller Computer zum Abschluss der System-Updates anwenden.|
|[Endpoint Protection installieren](security-center-install-endpoint-protection.md)|Empfiehlt Antimalware Programme VMs (nur Windows-VMs) bereitstellen.|
|[Endpoint Protection Health Alarme](security-center-resolve-endpoint-protection-health-alerts.md)|Empfiehlt, dass Sie Endpoint Protection Fehler beheben.|
|[VM-Agent aktivieren](security-center-enable-vm-agent.md)|Können Sie die VMs benötigen den VM-Agent. VM-Agent muss auf virtuellen Computern installiert werden, um Patchsuche, geplante Scans und Antimalware-Programme bereitstellen. VM-Agent ist für virtuelle Computer standardmäßig, die aus dem Markt Azure bereitgestellt werden. Artikel [VM und Extensions – Teil2](http://azure.microsoft.com/blog/2014/04/15/vm-agent-and-extensions-part-2/) enthält Informationen wie den VM-Agent installieren.|
| [Disk Encryption anwenden](security-center-apply-disk-encryption.md) |Empfiehlt die VM Datenträger Verschlüsselung Azure Datenträger (Windows und Linux VMs) verschlüsseln. Verschlüsselung wird für das Betriebssystem und die Daten-Volumes auf Ihrem virtuellen Computer empfohlen.|
| [Aktualisieren Betriebssystem](security-center-update-os-version.md) | Empfiehlt, Version des Betriebssystems (OS) für den Cloud-Dienst auf die neueste verfügbare Version für Ihr Betriebssystem-Familie zu aktualisieren.  Über Cloud-Dienste finden Sie unter [Übersicht über Cloud-Services](../cloud-services/cloud-services-choose-me.md). |
| [Bewertung der Sicherheitslücken nicht installiert](security-center-vulnerability-assessment-recommendations.md) | Empfiehlt eine Schwachstelle Lösung auf Ihrem virtuellen Computer installieren. |
| [Beheben von Sicherheitslücken](security-center-vulnerability-assessment-recommendations.md#review-recommendation) | Können Sie auf System- und Sicherheitslücken erkannt durch die Sicherheitslücke Lösung auf die VM installiert. |

## <a name="see-also"></a>Siehe auch

Erfahren Sie mehr über Vorschläge, die für andere Typen von Azure gelten finden Sie hier:

- [Schützen Ihre Anwendung in Azure Security Center](security-center-application-recommendations.md)
- [Schützen Sie Ihr Netzwerk in Azure Security Center](security-center-network-recommendations.md)
- [Schützen Ihren SQL Azure-Dienst in Azure Security Center](security-center-sql-service-recommendations.md)

Erfahren Sie mehr über das Sicherheitscenter finden Sie hier:

- [Festlegen von Sicherheitsrichtlinien in Azure Security Center](security-center-policies.md) – Informationen zum Konfigurieren von Sicherheitsrichtlinien für Ihren Azure-Abonnements und Ressourcengruppen.
- [Verwalten von und reagieren auf Sicherheitshinweise in Azure Security Center](security-center-managing-and-responding-alerts.md) - Informationen verwalten und Sicherheitshinweise auf.
- [Azure Security Center FAQ](security-center-faq.md) - finden Sie häufig gestellte Fragen zur Verwendung des Dienstes.
