<properties
    pageTitle="Verwalten von Azure Key Vault Azure Automatisierung | Microsoft Azure"
    description="Erfahren Sie, wie Azure Automation Service zur Azure Key Vault verwalten."
    services="Key-Vault, automation"
    documentationCenter=""
    authors="mgoedtel"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/29/2016"
    ms.author="magoedte;csand"/>

#<a name="managing-azure-key-vault-using-azure-automation"></a>Verwalten von Azure Key Vault Azure Automatisierung

Dieses Handbuch wird Ihnen Azure Automation Service und wie sie verwendet werden, zur Vereinfachung des Managements Ihrer Schlüssel und geheime Informationen in Azure Key Vault.

## <a name="what-is-azure-automation"></a>Was ist Automatisierung Azure?

[Azure-Automatisierung](../automation/automation-intro.md) ist eine Azure für vereinfachtes Management durch Automatisierung und Konfiguration des gewünschten Cloud. Azure Automatisierung manueller, können wiederholt, langlebige und fehleranfällige Aufgaben automatisiert werden um Zuverlässigkeit, Effizienz und Wert für Ihr Unternehmen Zeit erhöhen.

Azure Automation bietet eine hochgradig zuverlässige und hochverfügbare Ausführung Workflowmodul, das an Ihre Bedürfnisse. In Azure Automation können Prozesse manuell 3rd Party Systeme oder in bestimmten Zeitabständen gestartet so dass Aufgaben genau bei Bedarf.

Betriebskosten reduzieren und frei IT und DevOps Arbeit konzentrieren, die Unternehmen Mehrwert durch Verschieben der Cloud Verwaltungsaufgaben von Azure Automation automatisch ausgeführt werden.


## <a name="how-can-azure-automation-help-manage-azure-key-vault"></a>Helfen Azure Automatisierung Azure Key Vault verwalten?

Schlüssel Depot kann in Azure Automation mit [AzureRM Schlüssel Depot Cmdlets] verwaltet werden (https://www.powershellgallery.com/packages/AzureRM.KeyVault/1.1.4) und [Azure klassischen Key Vault-Cmdlets](https://msdn.microsoft.com/library/azure/dn868052.aspx). Azure-Modul für die Verwaltung von klassischen Key Vault steht automatisch in Azure Automation und Sie können das [Modul AzureRM Schlüsseltresor](https://www.powershellgallery.com/packages/AzureRM.KeyVault/1.1.4) in Azure Automation viele Schlüssel Depot Verwaltungsaufgaben innerhalb des Dienstes ausführen. Sie können auch diese Cmdlets in Azure Automation Cmdlets für andere Dienste Azure Azure Services und 3. Systeme komplexe Aufgaben automatisieren koppeln.

Azure Key Vault-Cmdlets können Sie diese unter anderem Aufgaben: 

- Erstellen und Konfigurieren von Key vault
- Erstellen oder Importieren eines Schlüssels
- Erstellen oder Aktualisieren eines geheimen Schlüssels
- Aktualisieren von Attributen des Schlüssels
- Ein Schlüssel oder Schlüssel
- Löschen eines Schlüssels oder Schlüssel

Einige Beispiele der Verwendung von PowerShell Key Vault verwalten:  

* [Azure Key Vault - Schritt für Schritt](https://blogs.technet.microsoft.com/kv/2015/06/02/azure-key-vault-step-by-step)
* [Installation und Konfiguration von Azure Key Vault](https://www.simple-talk.com/cloud/platform-as-a-service/setting-up-and-configuring-an-azure-key-vault)


## <a name="next-steps"></a>Nächste Schritte

Nun, Sie beherrschen die Grundlagen der Azure-Automatisierung und Verwendung kann zu Azure Key Vault, folgen Sie diesen Links erfahren Sie mehr über Azure Automation.

* [Erste Schritte abschließen](../automation/automation-first-runbook-graphical.md)Azure Automation anzeigen
* Finden Sie den [Azure Key Vault PowerShell-Skripts](https://gallery.technet.microsoft.com/scriptcenter/site/search?query=azure%20key%20vault&f%5B0%5D.Value=azure%20key%20vault&f%5B0%5D.Type=SearchText&ac=5).
