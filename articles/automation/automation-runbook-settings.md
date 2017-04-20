<properties 
   pageTitle="Runbook settings"
   description="Beschreibt die Konfigurationsdateien für ein Runbook in Azure Automation und zum Verwenden der Azure-Verwaltungsportal und Windows PowerShell ändern."
   services="automation"
   documentationCenter=""
   authors="bwren"
   manager="stevenka"
   editor="tysonn" />
<tags 
   ms.service="automation"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/09/2016"
   ms.author="bwren" />

# <a name="runbook-settings"></a>Runbook settings

Jedes Runbook in Azure Automation hat mehrere Einstellungen, mit die Hilfe identifiziert werden und das Protokollierungsverhalten ändern. Diese Einstellungen wird nachfolgend beschrieben, gefolgt von Prozeduren zum Ändern.

## <a name="settings"></a>Einstellungen

### <a name="name-and-description"></a>Name und Beschreibung

Sie können den Namen eines Runbooks nach erstellten ändern. Die Beschreibung ist optional und kann bis zu 512 Zeichen.

### <a name="tags"></a>Tags

Tags können Sie unterschiedliche Wörter und Ausdrücke identifizieren ein Runbook zuweisen. Wenn ein Runbook [Runbook Katalog](https://msdn.microsoft.com/library/dn781422.aspx)senden, geben Sie bestimmte Tags identifizieren die Kategorien Runbook in aufgelistet werden soll. Sie können mehrere Tags für ein Runbook durch Kommas getrennt angeben.

### <a name="logging"></a>Protokollierung

Standardmäßig werden ausführlich und Fortschritt Datensätze nicht in Auftragsverlauf geschrieben. Sie können die für ein bestimmtes Runbook protokollieren diese Datensätze ändern. Weitere Informationen zu dieser Datensätze finden Sie unter [Runbook und Nachrichten](https://msdn.microsoft.com/library/dn879148.aspx).

## <a name="changing-runbook-settings"></a>Runbook Einstellungen

### <a name="changing-runbook-settings-with-the-azure-management-portal"></a>Einstellungen Runbook mit Azure-Verwaltungsportal

Sie können für ein Runbook in Azure-Verwaltungsportal auf **Konfigurieren** für das Runbook ändern.

1. Aktivieren Sie in Azure-Verwaltungsportal **Automatisierung** , und klicken Sie auf den Namen eines Kontos Automatisierung.
1. Wählen Sie die Registerkarte **Runbooks** .
1. Klicken Sie auf den Namen eines Runbooks.
1. Wählen Sie die Registerkarte **Konfigurieren** .

### <a name="changing-runbook-settings-with-windows-powershell"></a>Einstellungen Runbook mit Windows PowerShell

[Set-AzureAutomationRunbook](https://msdn.microsoft.com/library/dn690275.aspx) -Cmdlet können Sie für ein Runbook ändern. Wenn Sie mehrere Tags angeben möchten, kann entweder ein Array oder eine einzelne Zeichenfolge kommagetrennte Werte Parameters Tags bieten. Sie erhalten die aktuellen Tags mit [Get-AzureAutomationRunbook](https://msdn.microsoft.com/library/dn690278.aspx).

Die folgenden Beispielbefehle zeigen, wie die Eigenschaften für ein Runbook festlegen. Dieses Beispiel fügt drei Tags auf vorhandene Tags und gibt an, dass ausführliche Datensätze protokolliert werden sollen.

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Sample-TestRunbook"
    $tags = (Get-AzureAutomationRunbook –AutomationAccountName $automationAccountName –Name $runbookName).Tags
    $tags += "Tag1,Tag2,Tag3"
    Set-AzureAutomationRunbook –AutomationAccountName $automationAccountName –Name $runbookName –LogVerbose $true –Tags $tags

## <a name="related-articles"></a>Verwandte Artikel
- [Runbook Ausgabe und Nachrichten](../automation-runbook-output-and-messages) 
- [Erstellen oder importieren ein Runbook](https://msdn.microsoft.com/library/dn643637.aspx) 