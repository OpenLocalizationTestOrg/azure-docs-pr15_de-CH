<properties
   pageTitle="Verschlüsselung in Azure Security Center Laufwerke anwenden | Microsoft Azure"
   description="Dieses Dokument veranschaulicht die Empfehlung Azure Security Center **Übernehmen datenträgerverschlüsselung**implementieren."
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
   ms.date="07/29/2016"
   ms.author="terrylan"/>

# <a name="apply-disk-encryption-in-azure-security-center"></a>Verschlüsselung in Azure Security Center Laufwerke anwenden

Azure Security Center wird empfehlen Sie datenträgerverschlüsselung anwenden, wenn Windows oder Linux VM Festplatten, die nicht mit Azure Disk Encryption verschlüsselt sind. Datenträgerverschlüsselung können Sie Ihre Windows- und Linux-Neuerung Datenträger verschlüsseln.  Verschlüsselung wird für das Betriebssystem und die Daten-Volumes auf Ihrem virtuellen Computer empfohlen.


Disk Encryption nutzt die Branche [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) Standardfunktion von Windows und die Funktion [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) Linux Betriebssystem und Daten schützen und Ihre Daten und Ihre Organisation Sicherheits- und Compliance Verpflichtungen Verschlüsselung bereitstellen. Disk Encryption ist [Azure Key Vault](https://azure.microsoft.com/documentation/services/key-vault/) zu steuern und die Datenträger Schlüssel und geheime Informationen in Ihrem Abonnement Key Vault dabei sicherzustellen, dass alle Daten in der VM-Festplatten ruhende im [Azure-Speicher](https://azure.microsoft.com/documentation/services/storage/)verschlüsselt werden integriert.

> [AZURE.NOTE] Azure Datenträgerverschlüsselung wird auf folgenden Windows Server-Betriebssysteme - Windows Server 2008 R2, Windows Server 2012 und Windows Server 2012 R2 unterstützt. Datenträgerverschlüsselung wird für die folgenden Linux Serverbetriebssysteme – Ubuntu CentOS, SUSE und SUSE Linux Enterprise Server (SLES) unterstützt.

## <a name="implement-the-recommendation"></a>Implementieren der Empfehlung

1. Wählen Sie Blade **Recommendations** **Verschlüsselung Festplatten übernehmen**.
2. Blatt **Verschlüsselung Festplatten übernehmen** sehen eine Liste der VMs Sie für die Verschlüsselung der Festplatte empfohlen.
3. Führen Sie die Schritte auf die VMs Verschlüsselung angewendet.

![][1]

Um Azure Virtual Machines verschlüsseln, die als Verschlüsselung vom Security Center identifiziert wurden, empfehlen wir die folgenden Schritte aus:

- Installieren und Konfigurieren von Azure PowerShell. Dadurch können Sie zur Einrichtung von Azure Virtual Machines verschlüsseln erforderliche PowerShell-Befehle ausführen.
- Beziehen und Azure Datenträger Verschlüsselung erforderliche Azure PowerShell-Skript ausführen.
- Verschlüsseln Sie Ihre virtuellen Computer.

[Verschlüsseln einer Azure Virtual Machine](security-center-disk-encryption.md) gehen Sie diese Schritte.  In diesem Thema wird davon ausgegangen, Sie verwenden Windows 10 als Clientcomputer aus dem datenträgerverschlüsselung konfiguriert wird.

Es gibt viele Methoden, mit die die Komponenten einrichten und Konfigurieren der Verschlüsselung für Azure Virtual Machines verwendet werden können. Wenn Sie bereits in Azure PowerShell oder Azure CLI versiert sind, bevorzugen Sie alternative Ansätze verwenden. Diese finden Sie andere Ansätze [Azure datenträgerverschlüsselung](../security/azure-security-disk-encryption.md).



## <a name="see-also"></a>Siehe auch

Dieses Dokument wurde gezeigt, wie das Sicherheitscenter Empfehlung "Übernehmen Disk Encryption". Weitere Informationen zur datenträgerverschlüsselung finden Sie hier:

- [Verschlüsselung und Schlüsselmanagement mit Azure Schlüssel](https://azure.microsoft.com/documentation/videos/azurecon-2015-encryption-and-key-management-with-azure-key-vault/) (video, 36 39 Minuten) – lernen Datenträgerverwaltungs IaaS VMs und Azure Key Vault-Verschlüsselung zu schützen und Ihre Daten zu schützen.
- [Verschlüsseln von Azure Virtual Machine](security-center-disk-encryption.md) (Dokument) Informationen zum Verschlüsseln von Azure Virtual Machines.
- [Verschlüsselung Azure Festplatten](../security/azure-security-disk-encryption.md) (Dokument) - Windows und Linux VMs Verschlüsselung aktivieren.

Erfahren Sie mehr über das Sicherheitscenter finden Sie hier:

- [Festlegen von Sicherheitsrichtlinien in Azure Security Center](security-center-policies.md) – Informationen zum Konfigurieren von Sicherheitsrichtlinien.
- [Security Health monitoring in Azure Security Center](security-center-monitoring.md) – erfahren Sie, wie Ihre Azure Ressourcen überwachen.
- [Verwalten von und reagieren auf Sicherheitshinweise in Azure Security Center](security-center-managing-and-responding-alerts.md) - Informationen verwalten und Sicherheitshinweise auf.
- [Verwalten von Sicherheitsaspekte in Azure Security Center](security-center-recommendations.md) – erfahren Sie, wie Recommendations Azure Ressourcen schützen.
- [Azure Security Center FAQ](security-center-faq.md) - finden Sie häufig gestellte Fragen zur Verwendung des Dienstes.
- [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) – Blog suchen Beiträge über Azure Sicherheit und Compliance.



<!--Image references-->
[1]: ./media/security-center-apply-disk-encryption/apply-disk-encryption.png
