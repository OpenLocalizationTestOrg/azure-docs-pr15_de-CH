<properties
   pageTitle="Azure Security Center und Azure Virtual Machines | Microsoft Azure"
   description="Dieses Dokument hilft Ihnen zu verstehen, wie Azure Security Center Sie Azure Virtual Machines sichern können."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/07/2016"
   ms.author="yurid"/>

# <a name="azure-security-center-and-azure-virtual-machines"></a>Azure Security Center und Azure Virtual Machines

[Azure Security Center](https://azure.microsoft.com/services/security-center/) hilft zu verhindern, erkennen und reagieren auf Gefahren. Bietet integrierte Überwachung und Policy-Management über Abonnements Azure, Gefahren, die andernfalls vielleicht übersehen würden und arbeitet mit einem ausgedehnten System Security Solutions hilft bei der Ermittlung.

In diesem Artikel wird das Sicherheitscenter Azure-virtuellen Maschinen (VM) sichern helfen.

## <a name="why-use-security-center"></a>Wozu dient das Sicherheitscenter?

Security Center können Sie die Daten der virtuellen Maschine in Azure mit Transparenz der virtuellen Maschine Sicherheitsrichtlinien schützen. Beim Sicherheitscenter Ihre VMs sichert werden folgenden Funktionen zur Verfügung:

- Betriebssystem (OS) Einstellungen Vorschriften empfohlen
- Sicherheit und Updates, die fehlen
- EndPoint Protection recommendations
- Disk Encryption Validierung
- Bewertung der Sicherheitslücken und Behebung
- Erkennung

Neben der Azure-VMs zu schützen, bietet Security Center auch sicherheitsüberwachung und Management für Cloud-Services, Anwendungsdienste und virtuelle Netzwerke. 

>[AZURE.NOTE] Finden Sie unter [Einführung in Azure Security Center](security-center-intro.md) Azure Security Center erfahren.

## <a name="prerequisites"></a>Erforderliche Komponenten

Zunächst mit Azure Security Center müssen Sie wissen, und beachten Folgendes:

- Sie müssen ein Abonnement für Microsoft Azure. Weitere Informationen zum Security Center freien und standard-Ebenen finden Sie unter [Security Center Preise](https://azure.microsoft.com/pricing/details/security-center/) .
- Planen Sie Ihre Annahme Sicherheitscenter, finden Sie unter Planung und Betrieb Aspekte kennen [Azure Security Center Planung und Betrieb führen](security-center-planning-and-operations-guide.md) .
- Finden Sie Informationen zum Betriebssystem Unterstützungsfunktionen [Azure Security Center häufig gestellte Fragen (FAQS)](security-center-faq.md). 

## <a name="set-security-policy"></a>Festgelegte Sicherheitsrichtlinie

Datensammlung muss aktiviert werden, sammeln, Azure Security Center die Informationen zur empfohlenen und generierten Warnungen basierend auf der Sicherheitsrichtlinie konfigurieren. In der folgenden Abbildung können Sie sehen, dass **Datensammlung** wurde **aktiviert**.

Eine Sicherheitsrichtlinie definiert die Steuerelemente für das angegebene Abonnement oder Ressourcengruppe Ressourcen empfohlen. Vor der Aktivierung der Sicherheitsrichtlinie, Sie Datensammlung aktiviert, Security Center sammelt Daten von der virtuellen Maschinen um ihren Sicherheitsstatus bewerten, Sicherheitsaspekte, und Sie auf Gefahren hinweisen. Im Sicherheitscenter definieren Sie Richtlinien für Ihre Azure-Abonnements oder Ressourcengruppen Sicherheitsbedarfs Ihres Unternehmens und der Anwendung oder die Daten in jedem Abonnement. 

![Sicherheitsrichtlinien](./media/security-center-virtual-machine/security-center-virtual-machine-fig1.png)

>[AZURE.NOTE] Über jede **Intrusionspräventions-Richtlinie** verfügbar ist, finden Sie unter [Festlegen von Sicherheitsrichtlinien](security-center-policies.md) Artikel mehr.

## <a name="manage-security-recommendations"></a>Sicherheitsaspekte verwalten

Sicherheitscenter analysiert den Sicherheitsstatus Ihrer Azure-Ressourcen. Beim Sicherheitscenter Sicherheitslücken identifiziert, erstellt Recommendations. Empfehlung führen Sie durch den Prozess des Konfigurierens benötigten Steuerelemente.

Nach dem Festlegen einer Sicherheitsrichtlinie, analysiert Sicherheitscenter den Sicherheitsstatus Ihrer Ressourcen zur Identifizierung möglicher Sicherheitslücken. Die Empfehlung werden im Tabellenformat angezeigt, wobei jede Zeile eine Empfehlung darstellt. Die Tabelle enthält Beispiele empfohlenen für Azure VMs und was jede bei Anwendung. Beim Auswählen einer Empfehlung werden Informationen bereitgestellt, die zum Implementieren der Empfehlung im Sicherheitscenter zeigt.

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
| [Bewertung der Sicherheitslücken nicht installiert](security-center-vulnerability-assessment-recommendations.md) | Empfiehlt eine Schwachstelle Lösung auf Ihrem virtuellen Computer installieren. |
| [Beheben von Sicherheitslücken](security-center-vulnerability-assessment-recommendations.md#review-recommendation) | Können Sie auf System- und Sicherheitslücken erkannt durch die Sicherheitslücke Lösung auf die VM installiert. |

>[AZURE.NOTE] Weitere Informationen zu empfohlenen [Verwalten Sicherheitsaspekte](security-center-recommendations.md) Artikel.

## <a name="monitor-security-health"></a>Sicherheitsstatus überwachen

Nach dem aktivieren [von Sicherheitsrichtlinien](security-center-policies.md) für ein Abonnement Ressourcen analysieren Sicherheitscenter der Sicherheits Ihrer Ressourcen zur Identifizierung möglicher Sicherheitslücken.  Sie können den Sicherheitsstatus Ihrer Ressourcen und Probleme in der **Ressource Sicherheitsstatus** Blade-anzeigen. Wenn Sie **virtuelle Computer** **Sicherheit** Gesundheit Kachel klicken, öffnet Blade **virtuelle Computer** mit Ihrer virtuellen Computer. 

![Sicherheitsstatus](./media/security-center-virtual-machine/security-center-virtual-machine-fig2.png)

## <a name="manage-and-respond-to-security-alerts"></a>Verwalten und Beantworten von Sicherheitshinweisen

Sicherheitscenter automatisch erfasst, analysiert und Protokolldaten Azure Ressourcen, Netzwerk und verbundenen Partner Solutions (wie Firewall und Endpoint Protection Solutions), echte Gefahren erkennen und reduziert Fehlmeldungen integriert. Sicherheitscenter kann mithilfe verschiedene Aggregation [Erkennungsfunktionen](security-center-detection-capabilities.md)generieren priorisierte Sicherheitshinweise schnell das Problem zu untersuchen und zu beheben von möglichen Angriffen bieten.

![Sicherheitshinweise](./media/security-center-virtual-machine/security-center-virtual-machine-fig3.png)

Wählen Sie eine Warnung erfahren Ereignisse, die ausgelöst, Warnung und was, wenn alle Schritte Sie um einen Angriff zu beseitigen müssen. Sicherheitshinweise werden nach [Typ](security-center-alerts-type.md) und Datum gruppiert.


## <a name="see-also"></a>Siehe auch

Erfahren Sie mehr über das Sicherheitscenter finden Sie hier:

- [Festlegen von Sicherheitsrichtlinien in Azure Security Center](security-center-policies.md) – Informationen zum Konfigurieren von Sicherheitsrichtlinien für Ihren Azure-Abonnements und Ressourcengruppen.
- [Verwalten von und reagieren auf Sicherheitshinweise in Azure Security Center](security-center-managing-and-responding-alerts.md) - Informationen verwalten und Sicherheitshinweise auf.
- [Azure Security Center FAQ](security-center-faq.md) - finden Sie häufig gestellte Fragen zur Verwendung des Dienstes.
