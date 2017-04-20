<properties 
    pageTitle="Bearbeiten von Text Runbooks in Azure Automation"
    description="Dieser Artikel enthält verschiedene Verfahren zum Arbeiten mit PowerShell und PowerShell Workflow Runbooks in Azure Automation im Text-Editor."
    services="automation"
    documentationCenter=""
    authors="mgoedtel"
    manager="stevenka"
    editor="tysonn" />
<tags 
    ms.service="automation"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="02/23/2016"
    ms.author="magoedte;bwren" />

# <a name="editing-textual-runbooks-in-azure-automation"></a>Bearbeiten von Text Runbooks in Azure Automation

Der Texte-Editor in Azure Automation dienen [PowerShell Runbooks](automation-runbook-types.md#powershell-runbooks) und [PowerShell Workflow Runbooks](automation-runbook-types.md#powershell-workflow-runbooks)bearbeiten. Dies weist normalerweise andere Code-Editoren wie Intellisense Farbkennzeichnung zusätzliche spezielle Features unterstützen Sie beim Zugriff auf Ressourcen in Runbooks.  Dieser Artikel enthält detaillierte Schritte für unterschiedliche Funktionen mit diesem Editor.

Der Texte-Editor enthält eine Funktion zum Einfügen von Code für die Aktivitäten, Ressourcen und untergeordnete Runbooks in ein Runbook. Anstatt im Code selbst, können Sie aus einer Liste der verfügbaren Ressourcen auswählen und den entsprechenden Code der Runbook eingefügt.

Jedes Runbook in Azure Automation verfügt über zwei Versionen, Entwurf und veröffentlicht. Sie bearbeiten die Entwurfsversion der Runbooks und dann veröffentlichen, damit es ausgeführt werden kann. Die veröffentlichte Version kann nicht bearbeitet werden. Weitere Informationen finden Sie unter [ein Runbook veröffentlichen](automation-creating-importing-runbook.md#publishing-a-runbook) .

Arbeiten mit [Grafiken Runbooks](automation-runbook-types.md#graphical-runbooks)finden Sie unter [Graphical authoring in Azure Automation](automation-graphical-authoring-intro.md).

## <a name="to-edit-a-runbook-with-the-azure-portal"></a>So bearbeiten Sie ein Runbook mit Azure-portal

Gehen Sie ein Runbook zur Bearbeitung im Text-Editor öffnen.

1. Wählen Sie in Azure-Portal Automation-Konto.
2. Klicken Sie auf die Kachel **Runbooks** zum Öffnen der Liste Runbooks.
3. Runbook zu bearbeiten und anschließend auf die Schaltfläche **Bearbeiten** klicken.
6. Führen Sie die erforderlichen Bearbeitung.
7. Klicken Sie auf **Speichern** , wenn die Bearbeitung abgeschlossen ist.
8. Klicken Sie auf **Veröffentlichen** der aktuellsten Entwurfsversion Runbooks veröffentlicht werden.

### <a name="to-insert-a-cmdlet-into-a-runbook"></a>So fügen Sie ein Runbook ein cmdlet

2. Positionieren Sie den Cursor im Bereich des Text-Editors soll das Cmdlet platzieren.
3. Erweitern Sie die **Cmdlets** in Library Control. 
3. Erweitern Sie das Modul mit dem Cmdlet zu verwenden.
4. Klicken Sie das Cmdlet zu einfügen hinzufügen **zu**.  Das Cmdlet mehrere Parameter festgelegt ist, wird der standardmäßige Satz hinzugefügt.  Sie können auch das Cmdlet wählen Sie einen anderen Parametersatz erweitern.
4. Der Code für das Cmdlet mit die gesamte Liste der Parameter eingefügt.
5. Geben Sie einen entsprechenden Wert Datentyp <> für alle erforderlichen Parameter Klammern umgeben.  Entfernen Sie alle nicht benötigten Parameter.

### <a name="to-insert-code-for-a-child-runbook-into-a-runbook"></a>So fügen Sie ein Runbook Code für ein Runbook Kind

2. Positionieren Sie den Cursor im Bereich des Text-Editors, den Code für die [untergeordneten Runbook](automation-child-runbooks.md)soll.
3. Erweitern Sie den Knoten **Runbooks** Library Control. 
3. Klicken Sie Runbook einfügen und **Hinzufügen zu**wählen.
4. Der Code für die untergeordneten Runbook wird mit Platzhalter für Parameter Runbook eingefügt.
5. Ersetzen Sie die Platzhalter mit den entsprechenden Werten für jeden Parameter.

### <a name="to-insert-an-asset-into-a-runbook"></a>Ein Runbook Anlage einfügen

2. Positionieren Sie den Cursor im Bereich des Text-Editors, den Code für die untergeordneten Runbook soll.
3. Erweitern Sie in der Library Control **Anlagen** . 
4. Erweitern Sie den Knoten für den Typ der gewünschten Anlage.
3. Klicken Sie die Anlage einfügen und hinzufügen **zu**.  Wählen Sie für [Variable Elemente](automation-variables.md) **Hinzufügen "Get Variable" Leinwand** oder **"Variable festlegen" auf die Leinwand hinzufügen** je nachdem ob Sie abrufen oder Festlegen der Variablen.
4. Der Code für die Anlage wird Runbook eingefügt.



## <a name="to-edit-a-runbook-with-the-azure-portal"></a>So bearbeiten Sie ein Runbook mit Azure-portal

Gehen Sie ein Runbook zur Bearbeitung im Text-Editor öffnen.

1. Aktivieren Sie in Azure-Portal **Automatisierung** , und klicken Sie auf den Namen eines Kontos Automatisierung.
2. Wählen Sie die Registerkarte **Runbooks** .
3. Klicken Sie zu bearbeiten, und wählen die Registerkarte **Autor** Runbook.
5. Klicken Sie am unteren Bildschirmrand auf die Schaltfläche **Bearbeiten** .
6. Führen Sie die erforderlichen Bearbeitung.
7. Klicken Sie auf **Speichern** , wenn die Bearbeitung abgeschlossen ist.
8. Klicken Sie auf **Veröffentlichen** der aktuellsten Entwurfsversion Runbooks veröffentlicht werden.

### <a name="to-insert-an-activity-into-a-runbook"></a>Eine Aktivität in ein Runbook einfügen

1. Positionieren Sie den Cursor im Bereich des Text-Editors soll die Aktivität an.
1. Klicken Sie am unteren Bildschirmrand auf **Einfügen** und **Aktivität**.
1. Wählen Sie in der Spalte **Integrationsmodul** das Modul mit der Aktivität.
1. Wählen Sie im Bereich **Aktivitäten** einer Aktivität.
1. Beachten Sie in der Spalte **Beschreibung** die Beschreibung der Aktivität ein. Sie können auch auf anzeigen klicken detaillierte Hilfe beim Starten der Hilfe für die Aktivität im Browser.
1. Klicken Sie auf den Pfeil nach rechts.  Wenn die Aktivität über Parameter verfügt, werden sie zu Ihrer Information aufgeführt.
1. Klicken Sie auf die Schaltfläche überprüfen.  Code zum Ausführen der Aktivität in Runbook eingefügt.
1. Wenn die Aktivität Parameter erfordert, geben Sie einen entsprechenden Wert Datentyp <> Klammern umgeben.

### <a name="to-insert-code-for-a-child-runbook-into-a-runbook"></a>So fügen Sie ein Runbook Code für ein Runbook Kind

1. Positionieren Sie den Cursor im Bereich Text-Editor, in dem Sie [untergeordnete Runbook](automation-child-runbooks.md)platzieren möchten.
2. Klicken Sie am unteren Bildschirmrand auf **Einfügen** und **Runbook**.
3. Wählen Sie für ein Runbook in der mittleren Spalte und auf den Pfeil nach rechts.
4. Wenn Runbooks Parameter verfügt, werden sie zu Ihrer Information aufgeführt.
5. Klicken Sie auf die Schaltfläche überprüfen.  Ausgewählte Runbook auszuführenden Code in den aktuellen Runbook eingefügt.
7. Wenn Runbooks Parameter erfordert, geben Sie einen entsprechenden Wert Datentyp <> Klammern umgeben.

### <a name="to-insert-an-asset-into-a-runbook"></a>Ein Runbook Anlage einfügen

1. Positionieren Sie den Cursor im Bereich des Text-Editors, die Tätigkeit die Anlage abrufen soll.
1. Klicken Sie am unteren Bildschirmrand auf **Einfügen** und **festlegen**.
1. Wählen Sie die gewünschte Aktion in der Spalte **Aktion festlegen** .
1. Wählen Sie aus den verfügbaren Ressourcen in der mittleren Spalte.
1. Klicken Sie auf die Schaltfläche überprüfen.  Code zum Abrufen oder Festlegen der Anlage in der Runbook eingefügt.



## <a name="to-edit-an-azure-automation-runbook-using-windows-powershell"></a>So bearbeiten Sie eine Azure Automation Runbook mit Windows PowerShell

Bearbeiten Sie ein Runbook mit Windows PowerShell verwenden Sie den Editor Ihrer Wahl und in einer PS1-Datei speichern. Das Cmdlet " [Get-AzureAutomationRunbookDefinition](http://aka.ms/runbookauthor/cmdlet/getazurerunbookdefinition) " können Sie den Inhalt der Runbook [Set-AzureAutomationRunbookDefinition](http://aka.ms/runbookauthor/cmdlet/setazurerunbookdefinition) -Cmdlet vorhandenen Entwurf Runbook durch die geänderte ersetzen abrufen.

### <a name="to-retrieve-the-contents-of-a-runbook-using-windows-powershell"></a>Zum Abrufen des Inhalts eines Runbooks mit Windows PowerShell

Die folgenden Beispielbefehle zeigen, wie das Skript für ein Runbook abgerufen und in einer Datei speichern. In diesem Beispiel wird die Entwurfsversion abgerufen. Es kann auch die veröffentlichte Version des Runbooks abrufen, diese Version kann nicht geändert werden.

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Sample-TestRunbook"
    $scriptPath = "c:\runbooks\Sample-TestRunbook.ps1"
    
    $runbookDefinition = Get-AzureAutomationRunbookDefinition -AutomationAccountName $automationAccountName -Name $runbookName -Slot Draft
    $runbookContent = $runbookDefinition.Content

    Out-File -InputObject $runbookContent -FilePath $scriptPath

### <a name="to-change-the-contents-of-a-runbook-using-windows-powershell"></a>So ändern Sie den Inhalt eines Runbooks mit Windows PowerShell

Die folgenden Beispielbefehle zeigen, wie ein Runbook vorhandener Inhalt mit dem Inhalt einer Datei ersetzen. Beachten Sie, dass dieses Beispiel dasselbe wie [ein Runbook von einer Skriptdatei mit Windows PowerShell zu importieren](../automation-creating-or-importing-a-runbook#ImportRunbookScriptPS).

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Sample-TestRunbook"
    $scriptPath = "c:\runbooks\Sample-TestRunbook.ps1"

    Set-AzureAutomationRunbookDefinition -AutomationAccountName $automationAccountName -Name $runbookName -Path $scriptPath -Overwrite
    Publish-AzureAutomationRunbook –AutomationAccountName $automationAccountName –Name $runbookName

## <a name="related-articles"></a>Verwandte Artikel

- [Erstellen oder importieren ein Runbook in Azure Automation](automation-creating-importing-runbook.md)
- [Learning PowerShell workflow](automation-powershell-workflow.md)
- [Grafisch in Azure Automation erstellen](automation-graphical-authoring-intro.md)
- [Zertifikate](automation-certificates.md)
- [Anschlüsse](automation-connections.md)
- [Anmeldeinformationen](automation-credentials.md)
- [Zeitpläne](automation-schedules.md)
- [Variablen](automation-variables.md)