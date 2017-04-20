<properties 
   pageTitle="Zeitpläne in Azure Automatisierung | Microsoft Azure"
   description="Automatisierung Zeitpläne dienen Runbooks in Azure Automation automatisch planen. Beschreibt das Erstellen und Verwalten einer ein Runbook automatisch zu einem bestimmten Zeitpunkt oder auf einem wiederkehrenden Zeitplan starten können."
   services="automation"
   documentationCenter=""
   authors="MGoedtel"
   manager="jwhit"
   editor="tysonn" />
<tags 
   ms.service="automation"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/10/2016"
   ms.author="mgoedtel" />

# <a name="scheduling-a-runbook-in-azure-automation"></a>Planen ein Runbook in Azure Automation

Um ein Runbook in Azure Automation zu einem bestimmten Zeitpunkt planen, verknüpfen Sie es mit einen oder mehrere Zeitpläne. Zeitplan ausführen einmalige oder eine wiederkehrende stündlich oder täglich Zeitplan für Runbooks im klassischen Azure-Portal und Runbooks im Azure-Portal konfiguriert werden, Sie können darüber hinaus planen sie für wöchentlich, monatlich, bestimmte Wochentage oder Tage des Monats oder einen bestimmten Tag des Monats.  Ein Runbook kann mehrere Zeitpläne verknüpft werden kann, und ein Zeitplan mehrere Runbooks verknüpft.

>[AZURE.NOTE]  Zeitpläne unterstützen derzeit keine Azure Automation DSC-Konfigurationen.

## <a name="windows-powershell-cmdlets"></a>Windows PowerShell-Cmdlets

Die Cmdlets in der folgenden Tabelle werden zum Erstellen und Verwalten mit Windows PowerShell in Azure Automation. Sie als Bestandteil des [Azure PowerShell-Modul](../powershell-install-configure.md)geliefert.

|Cmdlets|Beschreibung|
|:---|:---|
|**Azure-Ressourcen-Manager-cmdlets**||
|[AzureRmAutomationSchedule abrufen](https://msdn.microsoft.com/library/mt603733.aspx)|Ruft einen Zeitplan.|
|[Neue AzureRmAutomationSchedule](https://msdn.microsoft.com/library/mt603577.aspx)|Erstellt einen neuen Zeitplan.|
|[AzureRmAutomationSchedule entfernen](https://msdn.microsoft.com/library/mt603691.aspx)|Entfernt einen Zeitplan.|
|[AzureRmAutomationSchedule festlegen](https://msdn.microsoft.com/library/mt603566.aspx)|Legt die Eigenschaften für einen vorhandenen Zeitplan.|
|[AzureRmAutomationScheduledRunbook abrufen](https://msdn.microsoft.com/library/mt619406.aspx)|Geplante Runbooks abgerufen.|
|[AzureRmAutomationScheduledRunbook registrieren](https://msdn.microsoft.com/library/mt603575.aspx)|Ordnet ein Runbook Zeitplan.|
|[Registrierung AzureRmAutomationScheduledRunbook](https://msdn.microsoft.com/library/mt603844.aspx)|Kopiert ein Runbook in einem Ablaufplan.|
|**Azure Service Management-cmdlets**||
|[AzureAutomationSchedule abrufen](http://msdn.microsoft.com/library/dn690274.aspx)|Ruft einen Zeitplan.|
|[Neue AzureAutomationSchedule](http://msdn.microsoft.com/library/dn690271.aspx)|Erstellt einen neuen Zeitplan.|
|[AzureAutomationSchedule entfernen](http://msdn.microsoft.com/library/dn690279.aspx)|Entfernt einen Zeitplan.|
|[AzureAutomationSchedule festlegen](http://msdn.microsoft.com/library/dn690270.aspx)|Legt die Eigenschaften für einen vorhandenen Zeitplan.|
|[AzureAutomationScheduledRunbook abrufen](http://msdn.microsoft.com/library/dn913778.aspx)|Geplante Runbooks abgerufen.|
|[AzureAutomationScheduledRunbook registrieren](http://msdn.microsoft.com/library/dn690265.aspx)|Ordnet ein Runbook Zeitplan.|
|[Registrierung AzureAutomationScheduledRunbook](http://msdn.microsoft.com/library/dn690273.aspx)|Kopiert ein Runbook in einem Ablaufplan.|

## <a name="creating-a-schedule"></a>Erstellen eines Zeitplans

Sie können einen neuen Zeitplan für Runbooks in Azure-Portal im klassischen Portal oder mit Windows PowerShell erstellen. Sie können einen neuen Zeitplan erstellen, wenn Sie einen Zeitplan mit Azure klassischen oder Azure-Portal ein Runbook verknüpfen.

>[AZURE.NOTE] Wenn Sie einen Zeitplan ein Runbook zuordnen, Automatisierung werden die aktuellen Versionen der Module in Ihrem Konto gespeichert und verknüpft diese mit Zeitplans.  Dies bedeutet, dass hätten Sie eine Modul mit Version 1.0 auf Ihrem Konto bei einen Zeitplan erstellt und anschließend das Modul auf Version 2.0 aktualisieren, der Zeitplan weiterhin 1.0 verwenden.  Um die aktualisierte Modulversion verwenden, müssen Sie einen neuen Zeitplan erstellen. 

### <a name="to-create-a-new-schedule-in-the-azure-portal"></a>Erstellen Sie einen neuen Zeitplan im Azure-portal

1. Klicken Sie in Azure-Portal von Ihrem automatisierungskonto **Anlagen** Blade **Anlagen** öffnen.
2. Klicken Sie auf die **Zeitpläne** nebeneinander öffnen **Zeitpläne** Blade.
3. Klicken Sie auf **Zeitplan hinzufügen** oben das Blade.
4. Geben Sie einen **Namen** und optional eine **Beschreibung** für den neuen Zeitplan auf Blade **Terminplan** .
5. Wählen Sie der Zeitplan ausgeführt wird, ob einem wiederkehrenden Zeitplan **einmal** oder **Serie aus**  Wenn Sie **einmal** auswählen geben Sie **Startzeit** und klicken Sie dann auf **Erstellen**.  Wenn Sie **Wiederholung**auswählen, geben Sie **Startzeit** und die Häufigkeit das Runbook wiederholen soll - **Stunde**, **Tag**, **Woche**oder **Monat**.  Bei Auswahl von **Wochen** oder **Monaten** aus der Dropdown-Liste **Serie Option** erscheint in der Blade-Auswahl, Blade **Serie Option** angezeigt und können Wochentag **Woche**ausgewählt.  Wenn Sie **Monat**ausgewählt haben, wählen Sie **Wochentage** oder bestimmte Tage des Monats im Kalender und schließlich soll am letzten Tag des Monats oder nicht und klicken Sie dann auf **OK**.   

### <a name="to-create-a-new-schedule-in-the-azure-classic-portal"></a>Erstellen Sie einen neuen Zeitplan im klassischen Azure-portal

1. Wählen Sie in der Azure-Verwaltungsportal Automatisierung, und wählen Sie dann den Namen eines Kontos Automatisierung.
1. Wählen Sie die Registerkarte **Ressourcen** .
1. Klicken Sie am unteren Rand des Fensters auf **Hinzufügen**.
1. Klicken Sie auf **Zeitplan**.
1. Geben Sie einen **Namen** und optional eine **Beschreibung** für den neuen schedule.your Zeitplan **Einmal** **stündlich**, **täglich**, **wöchentlich**oder **monatlich**ausgeführt werden.
1. Geben Sie eine **Startzeit** und andere Optionen je nach ausgewählten Zeitplan.

### <a name="to-create-a-new-schedule-with-windows-powershell"></a>Erstellen Sie einen neuen Zeitplan mit Windows PowerShell

[Neu AzureAutomationSchedule](http://msdn.microsoft.com/library/azure/dn690271.aspx) -Cmdlet einen neuen Zeitplan in Azure Automation für klassische Runbooks erstellen oder [Neu-AzureRmAutomationSchedule](https://msdn.microsoft.com/library/mt603577.aspx) -Cmdlet können für Runbooks im Azure-Portal. Geben Sie die Startzeit für den Zeitplan und die Häufigkeit, die ausgeführt werden soll.

Die folgenden Beispielbefehle veranschaulicht das Erstellen eines Zeitplans für den 15. und 30. des Monats mit einer Azure-Ressourcen-Manager-Cmdlet.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-MonthlyDaysOfMonthSchedule"
    New-AzureRMAutomationSchedule –AutomationAccountName $automationAccountName –Name `
    $scheduleName -StartTime "7/01/2016 15:30:00" -MonthInterval 1 `
    -DaysOfMonth Fifteenth,Thirtieth -ResourceGroupName "ResourceGroup01"

Die folgenden Beispielbefehle zeigen, wie Sie einen neuen Zeitplan erstellen, der täglich um 3:30 Uhr ab 20. Januar 2015 mit einer Azure Service Management-Cmdlet ausgeführt wird.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-DailySchedule"
    New-AzureAutomationSchedule –AutomationAccountName $automationAccountName –Name `
    $scheduleName –StartTime "1/20/2016 15:30:00" –DayInterval 1

## <a name="linking-a-schedule-to-a-runbook"></a>Einen Zeitplan verknüpfen ein runbook

Ein Runbook kann mehrere Zeitpläne verknüpft werden kann, und ein Zeitplan mehrere Runbooks verknüpft. Wenn ein Runbook Parameter verfügt, können Sie Werte für sie bereitstellen. Sie müssen Werte für alle erforderlichen Parameter und Werte für optionale Parameter können.  Diese Werte werden bei jedem verwendet Runbooks nach diesem Zeitplan gestartet wird.  Können Sie einen anderen Zeitplan derselbe Runbook zuordnen und andere Parameterwerte angeben.

### <a name="to-link-a-schedule-to-a-runbook-with-the-azure-portal"></a>Einen Zeitplan ein Runbook Azure Portal verknüpfen

1. Klicken Sie in Azure-Portal von Ihrem automatisierungskonto **Runbooks** **Runbooks** Blade öffnen.
2. Klicken Sie auf den Namen des Runbooks planen.
3. Wenn Runbook einen Plan aktuell nicht verknüpft ist, dann Sie die Option zum Erstellen eines neuen Zeitplans oder Verknüpfung auf einen vorhandenen Zeitplan erhalten.  
4. Wenn Runbooks Parameter verfügt, wählen Sie die Option **Modify Settings (Standard: Azure) ausgeführt** und **Parameter** Blade wird angezeigt, in dem Sie die Informationen entsprechend eingeben.  

### <a name="to-link-a-schedule-to-a-runbook-with-the-azure-classic-portal"></a>Einen Zeitplan ein Runbook mit klassischen Azure-Portal verknüpfen

1. Aktivieren Sie in der Azure-Verwaltungsportal **Automatisierung** , und klicken Sie auf den Namen eines Kontos Automatisierung.
2. Wählen Sie die Registerkarte **Runbooks** .
3. Klicken Sie auf den Namen des Runbooks planen.
4. Klicken Sie auf die Registerkarte **Zeitplan** .
5. Wenn Runbook einen Plan aktuell nicht verknüpft ist, dann Sie die Option **Verknüpfung auf einen neuen Zeitplan** oder **Verknüpfung auf einen vorhandenen Zeitplan**erhalten.  Wenn Runbooks zurzeit ein Plan verknüpft ist, klicken Sie auf **Verknüpfung** am unteren Fensterrand, um auf diese Optionen zuzugreifen.
6. Wenn Runbooks Parameter verfügt, werden Sie aufgefordert, ihre Werte anzugeben.  

### <a name="to-link-a-schedule-to-a-runbook-with-windows-powershell"></a>Einen Zeitplan ein Runbook mit Windows PowerShell verknüpfen

[Register-AzureAutomationScheduledRunbook](http://msdn.microsoft.com/library/azure/dn690265.aspx) können Sie einen Zeitplan mit einem klassischen Runbook oder [Register-AzureRmAutomationScheduledRunbook](https://msdn.microsoft.com/library/mt603575.aspx) -Cmdlet für Runbooks im Azure-Portal verknüpfen.  Sie können Werte für die Runbook Parameter Parameter Parameter angeben. Weitere Informationen zum Angeben von Parameterwerten finden Sie unter [Starten ein Runbook in Azure Automation](automation-starting-a-runbook.md) .

Die folgenden Beispielbefehle anzeigen wie einen Zeitplan, ein Runbook Parameter mit einer Azure-Ressourcen-Manager-cmdlet

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Test-Runbook"
    $scheduleName = "Sample-DailySchedule"
    $params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
    Register-AzureRmAutomationScheduledRunbook –AutomationAccountName $automationAccountName `
    –Name $runbookName –ScheduleName $scheduleName –Parameters $params `
    -ResourceGroupName "ResourceGroup01"
Die folgenden Beispielbefehle zeigen einen Zeitplan mit einem Cmdlet Azure Service Management mit Parametern verknüpfen.

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Test-Runbook"
    $scheduleName = "Sample-DailySchedule"
    $params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
    Register-AzureAutomationScheduledRunbook –AutomationAccountName $automationAccountName `
    –Name $runbookName –ScheduleName $scheduleName –Parameters $params

## <a name="disabling-a-schedule"></a>Zeitplan deaktivieren

Wenn Sie einen Zeitplan deaktivieren, wird alle verknüpften Runbooks, Zeitplan nicht mehr ausgeführt. Sie können manuell einen Zeitplan deaktivieren oder eine Ablaufzeit für Zeitpläne mit beim Erstellen. Wenn das Ablaufdatum erreicht ist, wird der Zeitplan deaktiviert.

### <a name="to-disable-a-schedule-from-the-azure-portal"></a>So deaktivieren Sie einen Zeitplan von Azure-portal

1. Klicken Sie in Azure-Portal von Ihrem automatisierungskonto **Anlagen** Blade **Anlagen** öffnen.
2. Klicken Sie auf die **Zeitpläne** nebeneinander öffnen **Zeitpläne** Blade.
2. Klicken Sie auf Schedule Details Blade geöffnet.
3. Ändern Sie **aktiviert** auf **Nein**.

### <a name="to-disable-a-schedule-from-the-azure-classic-portal"></a>So deaktivieren Sie einen Zeitplan von klassischen Azure-portal

Sie können einen Zeitplan im klassischen Azure-Portal auf Schedule Details für den Zeitplan deaktivieren.

1. Aktivieren Sie in der Azure-Verwaltungsportal Automatisierung, und klicken Sie auf den Namen eines Kontos Automatisierung.
1. Wählen Sie die Registerkarte Ressourcen.
1. Klicken Sie auf einen Zeitplan für die Detailseite öffnen.
2. Ändern Sie **aktiviert** auf **Nein**.

### <a name="to-disable-a-schedule-with-windows-powershell"></a>So deaktivieren Sie einen Zeitplan mit Windows PowerShell

Das Cmdlet " [Set-AzureAutomationSchedule](http://msdn.microsoft.com/library/azure/dn690270.aspx) " können Sie die Eigenschaften eines vorhandenen Zeitplans für eine klassische Runbook oder [Set-AzureRmAutomationSchedule](https://msdn.microsoft.com/library/mt603566.aspx) -Cmdlet für Runbooks im Azure-Portal ändern. Um den Zeitplan zu deaktivieren, geben Sie **false** **IsEnabled** -Parameter.

Die folgenden Beispielbefehle zeigen einen Zeitplan für ein Runbook mit einem Cmdlet Azure-Ressourcen-Manager deaktivieren.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-MonthlyDaysOfMonthSchedule"
    Set-AzureRmAutomationSchedule –AutomationAccountName $automationAccountName `
    –Name $scheduleName –IsEnabled $false -ResourceGroupName "ResourceGroup01"

Die folgenden Beispielbefehle zeigen einen Zeitplan mit dem Cmdlet Azure Service Management deaktivieren.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-DailySchedule"
    Set-AzureAutomationSchedule –AutomationAccountName $automationAccountName `
    –Name $scheduleName –IsEnabled $false

## <a name="next-steps"></a>Nächste Schritte

- Zunächst mit Runbooks in Azure Automation Siehe [Starten ein Runbook in Azure Automation](automation-starting-a-runbook.md) 