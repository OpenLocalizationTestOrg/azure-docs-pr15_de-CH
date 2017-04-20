<properties
    pageTitle="Erstellen oder importieren ein Runbook in Azure Automation"
    description="Dieser Artikel beschreibt die neue Runbook in Azure Automation erstellen oder aus einer Datei importieren."
    services="automation"
    documentationCenter=""
    authors="mgoedtel"
    manager="jwhit"
    editor="tysonn" />
<tags
    ms.service="automation"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="09/12/2016"
    ms.author="magoedte;bwren" />

# <a name="creating-or-importing-a-runbook-in-azure-automation"></a>Erstellen oder importieren ein Runbook in Azure Automation

Sie können ein Runbook Azure Automatisierung durch [Erstellen einer neuen](#creating-a-new-runbook) oder vorhandenen Runbook importieren, aus einer Datei oder aus der [Galerie Runbook](automation-runbook-gallery.md)hinzufügen. Dieser Artikel enthält Informationen zum Erstellen und Runbooks aus einer Datei importieren.  Sie können alle Details für den Zugriff auf Community Runbooks und Module in [Galerien Azure Automatisierung Modul Runbook](automation-runbook-gallery.md)abrufen.

## <a name="creating-a-new-runbook"></a>Erstellen eine neue runbook

Eine neue Runbook können in Azure Automation Azure Portals oder Windows PowerShell verwenden. Erstellte Runbooks können Sie mit Informationen in [Learning PowerShell Workflow](automation-powershell-workflow.md) und [grafisch in Azure Automation erstellen](automation-graphical-authoring-intro.md)bearbeiten.

### <a name="to-create-a-new-azure-automation-runbook-with-the-azure-classic-portal"></a>Erstellen ein neues Azure Automatisierung Runbooks mit Azure-Verwaltungsportal

Sie können nur mit [PowerShell Workflow Runbooks](automation-runbook-types.md#powershell-workflow-runbooks) im Azure-Portal arbeiten.

1. In der Azure-Verwaltungsportal klicken, **neue** **App Services**, **Automatisierung**, **Runbook**, **Schnell erstellen**.
2. Geben Sie die erforderlichen Informationen, und klicken Sie auf **Erstellen**. Runbook Name kann muss mit einem Buchstaben beginnen und Buchstaben, Zahlen, Unterstriche und Bindestriche.
3. Wenn Sie Runbooks jetzt bearbeiten möchten, klicken Sie auf **Runbook bearbeiten**. Klicken Sie anderenfalls auf **OK**.
4. Neue Runbook erscheint auf der Registerkarte **Runbooks** .


### <a name="to-create-a-new-azure-automation-runbook-with-the-azure-portal"></a>Erstellen ein neues Azure Automatisierung Runbooks mit Azure-portal

1. Öffnen Sie in Azure-Portal Automation-Konto.
2. Klicken Sie auf die Kachel **Runbooks** Runbooks öffnen.
3. Klicken Sie auf die Schaltfläche **Hinzufügen ein Runbook** und **erstellen eine neue Runbook**.
2. Geben Sie einen **Namen** für das Runbook und wählen Sie seinen [Typ](automation-runbook-types.md). Runbook Name kann muss mit einem Buchstaben beginnen und Buchstaben, Zahlen, Unterstriche und Bindestriche.
3. Klicken Sie auf **Erstellen** , um die Runbook erstellen und öffnen Sie den Editor.


### <a name="to-create-a-new-azure-automation-runbook-with-windows-powershell"></a>Erstellen ein neues Azure Automatisierung Runbooks mit Windows PowerShell

[Neu-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt619376.aspx) -Cmdlet können Sie eine leere [PowerShell Workflow Runbook](automation-runbook-types.md#powershell-workflow-runbooks)erstellen. Sie können entweder **den Namensparameter leere Runbook erstellen, die Sie später bearbeiten können** , oder **Pfadparameter Runbook-Datei importieren** können. **Der Typparameter** sollten auch zum Angeben einer der vier Runbook aufgenommen werden.

Die folgenden Beispielbefehle anzeigen zum Erstellen einer neuen leeren runbook

    New-AzureRmAutomationRunbook -AutomationAccountName MyAccount `
    -Name NewRunbook -ResourceGroupName MyResourceGroup -Type PowerShell

## <a name="importing-a-runbook-from-a-file-into-azure-automation"></a>Ein Runbook importieren in Azure aus einer Datei

Eine neue Runbook können in Azure Automation durch Importieren eines PowerShell-Skripts oder PowerShell Workflow (ps1-Erweiterung) oder eine exportierte Grafik Runbook (.graphrunbook).  Die geben den [Typ Runbooks](automation-runbook-types.md) unter Berücksichtigung der folgenden Aspekte Import erstellt werden.

- Eine .graphrunbook kann nur in eine neue [Grafik Runbook](automation-runbook-types.md#graphical-runbooks)importiert und grafisch Runbooks kann nur von einer .graphrunbook-Datei erstellt.
- Eine. ps1-Datei mit PowerShell Workflow kann nur in [PowerShell Workflow Runbook](automation-runbook-types.md#powershell-workflow-runbooks)importiert werden.  Enthält die Datei mehrere PowerShell Workflows, schlägt der Import fehl. Sie müssen jeder Workflow in einer eigenen Datei speichern und jeweils einzeln importieren.
- Eine. ps1-Datei, die keinen Workflow enthalten kann oder [PowerShell Runbook](automation-runbook-types.md#powershell-runbooks) [PowerShell Workflow Runbook](automation-runbook-types.md#powershell-workflow-runbooks)importiert.  Importieren in ein Runbook PowerShell Workflow dann konvertiert es an einen Workflow und Kommentare im Runbook vorgenommenen Änderungen Angabe enthalten.

### <a name="to-import-a-runbook-from-a-file-with-the-azure-classic-portal"></a>Ein Runbook mit Azure-Verwaltungsportal aus einer Datei importieren
Das folgende Verfahren können Sie eine Skriptdatei in Azure importieren.  Beachten Sie, dass eine. ps1-Datei nur in einem PowerShell Workflow Runbook über dieses Portal importiert werden können.  Verwenden Sie für andere Azure-Portal.

1. Wählen Sie im Azure-Verwaltungsportal **Automatisierung** , und wählen Sie das Konto Automatisierung.
2. Klicken Sie auf **Importieren**.
3. Klicken Sie auf **Datei** , und suchen Sie die Skriptdatei importieren.
4. Wenn Sie Runbooks jetzt bearbeiten möchten, klicken Sie auf **Runbook bearbeiten**. Klicken Sie anderenfalls auf OK.
5. Neue Runbook erscheint auf der Registerkarte **Runbooks** Automation-Konto.
6. Sie müssen [Runbook veröffentlichen](#publishing-a-runbook) , bevor ausgeführt werden kann.


### <a name="to-import-a-runbook-from-a-file-with-the-azure-portal"></a>Ein Runbook mit Azure-Portal aus einer Datei importieren
Das folgende Verfahren können Sie eine Skriptdatei in Azure importieren.  

>[AZURE.NOTE] Beachten Sie, dass eine. ps1-Datei nur in einem PowerShell Workflow Runbook über das Portal importiert werden können.

1. Öffnen Sie in Azure-Portal Automation-Konto.
2. Klicken Sie auf die Kachel **Runbooks** Runbooks öffnen.
3. Klicken Sie auf die Schaltfläche **Hinzufügen ein Runbook** und **Importieren**.
4. Klicken Sie auf **Runbook Datei** wählen Sie die zu importierende Datei
2. Wenn das **Feld** aktiviert ist, müssen Sie es ändern.  Runbook Name kann muss mit einem Buchstaben beginnen und Buchstaben, Zahlen, Unterstriche und Bindestriche.
3. [Runbook Typ](automation-runbook-types.md) automatisch ausgewählt, aber ändern Sie den Typ unter Berücksichtigung der geltenden Einschränkungen berücksichtigt. 
3. Neue Runbook erscheint in der Liste der Runbooks Automation-Konto.
4. Sie müssen [Runbook veröffentlichen](#publishing-a-runbook) , bevor ausgeführt werden kann.

>[AZURE.NOTE] Nach dem Importieren von Grafiken Runbook oder eine Grafik PowerShell Workflow Runbook können in anderen Typ zu konvertieren, wenn. Sie können nicht in Text konvertieren.

### <a name="to-import-a-runbook-from-a-script-file-with-windows-powershell"></a>So importieren Sie ein Runbook aus einer Skriptdatei mit Windows PowerShell

[Import-AzureRMAutomationRunbook](https://msdn.microsoft.com/library/mt603735.aspx) -Cmdlet können Sie eine Skriptdatei als Entwurf PowerShell Workflow Runbook importieren. Runbook bereits vorhanden, wird der Import fehl, es sei denn, Sie verwenden die *-Force* Parameter. 

Die folgenden Beispielbefehle zeigen, wie ein Runbook eine Skriptdatei importieren.

    $automationAccountName =  "AutomationAccount"
    $runbookName = "Sample_TestRunbook"
    $scriptPath = "C:\Runbooks\Sample_TestRunbook.ps1"
    $RGName = "ResourceGroup"

    Import-AzureRMAutomationRunbook -Name $runbookName -Path $scriptPath `
    -ResourceGroupName $RGName -AutomationAccountName $automationAccountName `
    -Type PowerShellWorkflow 


## <a name="publishing-a-runbook"></a>Ein Runbook veröffentlichen

Beim Erstellen oder einer neuen Runbook importieren müssen Sie es veröffentlichen, bevor Sie ausgeführt werden können.  Jedes Runbook Automatisierung hat einen Entwurf und veröffentlichte Version. Nur veröffentlichte Version steht ausgeführt werden, und nur die Entwurfsversion kann bearbeitet werden. Die veröffentlichte Version ist von Änderungen zum Entwurf. Beim Entwurf verfügbar sein soll, veröffentlichen Sie die veröffentlichte Version mit Entwurf überschreibt.

## <a name="to-publish-a-runbook-using-the-azure-classic-portal"></a>Ein Runbook mithilfe der Azure-Verwaltungsportal veröffentlichen

1. Öffnen des Runbooks in Azure-Verwaltungsportal.
1. Klicken Sie am oberen Bildschirmrand auf **Autor**.
1. Klicken Sie am unteren Bildschirmrand auf die Bestätigung auf **Veröffentlichen** und dann auf **Ja** .

## <a name="to-publish-a-runbook-using-the-azure-portal"></a>Ein Runbook mit Azure-Portal veröffentlichen

1. Öffnen des Runbooks in Azure-Portal.
1. Klicken Sie auf die Schaltfläche **Bearbeiten** .
1. Die Bestätigungsnachricht klicken Sie auf die Schaltfläche " **Veröffentlichen** " und dann **Ja** .


## <a name="to-publish-a-runbook-using-windows-powershell"></a>Ein mit Windows PowerShell Runbook veröffentlichen

[Publish-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603705.aspx) -Cmdlet können Runbooks mit Windows PowerShell zu veröffentlichen. Die folgenden Beispielbefehle zeigen ein Beispiel für ein Runbook veröffentlichen.

    $automationAccountName =  AutomationAccount"
    $runbookName = "Sample_TestRunbook"
    $RGName = "ResourceGroup"

    Publish-AzureRmAutomationRunbook -AutomationAccountName $automationAccountName `
    -Name $runbookName -ResourceGroupName $RGName


## <a name="next-steps"></a>Nächste Schritte
- Über die Vorteile von Runbook und PowerShell Modul Katalog finden Sie unter [Runbook und Modul Kataloge für Azure Automation](automation-runbook-gallery.md)
- Erfahren Sie mehr über das Bearbeiten von PowerShell und PowerShell Workflow Runbooks mit einem Text-Editor finden Sie unter [Bearbeiten Text Runbooks in Azure Automation](automation-edit-textual-runbook.md)
- Mehr zum Erstellen von Grafiken für ein Runbook finden Sie unter [Graphical authoring in Azure Automation](automation-graphical-authoring-intro.md)
