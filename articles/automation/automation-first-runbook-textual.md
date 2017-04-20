<properties
    pageTitle="Meine erste PowerShell Workflow Runbook in Azure Automatisierung | Microsoft Azure"
    description="Lernprogramm, in dem Sie durch das Erstellen, testen und Veröffentlichen eines einfachen Text Runbooks mit PowerShell Workflow geht."
    services="automation"
    documentationCenter=""
    authors="mgoedtel"
    manager="jwhit"
    editor=""
    keywords="PowerShell Workflow, Powershell Beispielworkflows Workflow powershell"/>
<tags
    ms.service="automation"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/19/2016"
    ms.author="magoedte;bwren"/>

# <a name="my-first-powershell-workflow-runbook"></a>Meine erste PowerShell Workflow runbook

> [AZURE.SELECTOR] - [Grafisch](automation-first-runbook-graphical.md) - [PowerShell](automation-first-runbook-textual-PowerShell.md) - [PowerShell Workflow](automation-first-runbook-textual.md)

Dieses Lernprogramm führt Sie durch die Erstellung eines [PowerShell-Workflow Runbook](automation-runbook-types.md#powerShell-workflow-runbooks) in Azure Automation. Wir beginnen mit einem einfachen Runbook, die wir testen und veröffentlichen und erläutert, wie Sie den Status des Auftrags Runbook verfolgen. Dann werden wir Runbook tatsächlich Azure Ressourcen verwalten ändern, in diesem Fall Starten einer Azure Virtual Machine. Wir werden dann Runbooks stabiler machen Runbook Parameter hinzufügen.

## <a name="prerequisites"></a>Erforderliche Komponenten

Um dieses Lernprogramm benötigen Sie Folgendes.

-   Azure-Abonnement. Wenn Sie noch keine haben, können Sie [Ihre MSDN-Abonnementvorteile aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) oder <a href="/pricing/free-account/" target="_blank"> [für ein kostenloses Konto anmelden](https://azure.microsoft.com/free/).
-   [Automation-Konto](automation-security-overview.md) zu Runbooks Azure Ressourcen authentifizieren.  Dieses Konto muss über die Berechtigung zum Starten und beenden Sie den virtuellen Computer.
-   Azure VM. Wir beenden und starten dieses Computers damit Produktion nicht zulässig.

## <a name="step-1---create-new-runbook"></a>Schritt 1 - erstellen Sie neue runbook

Zunächst erstellen eine einfache Runbooks, das den Text *Hello World*ausgibt.

1.  Öffnen Sie in Azure-Verwaltungsportal Automation-Konto.  
    Die Automatisierung Seite bietet einen schnellen Überblick über die Ressourcen auf diesem Konto. Sie haben bereits einige Vermögenswerte. Die meisten sind die Module, die automatisch in ein neues automatisierungskonto enthalten sind. Sie müssen auch Anmeldeinformationen Anlage, die in [Komponenten](#prerequisites)genannt.
2.  Klicken Sie auf die Kachel **Runbooks** Runbooks öffnen.<br> ![Runbooks Steuerelement](media/automation-first-runbook-textual/runbooks-control.png)
3.  Erstellen Sie neue Runbook durch Klicken auf die Schaltfläche **Hinzufügen ein Runbook** und **erstellen eine neue Runbook**.
4.  Geben Sie dem Runbook *MyFirstRunbook Workflow*.
5.  In diesem Fall werden wir ein [Runbook PowerShell Workflow](automation-runbook-types.md#powerShell-workflow-runbooks) erstellen wählen Sie **Powershell Workflow** für **ein Runbook**.<br> ![Neue runbook](media/automation-first-runbook-textual/new-runbook.png)
6.  Klicken Sie auf **Erstellen** , um die Runbook erstellen und den Texten-Editor öffnen.

## <a name="step-2---add-code-to-the-runbook"></a>Schritt 2 - Runbooks Code hinzufügen

Können Sie entweder Code für direkt in die Runbook und Library Control Cmdlets Runbooks und Elemente auswählen und diese Runbooks mit verknüpften Parametern hinzugefügt. In dieser exemplarischen Vorgehensweise werden wir direkt in die Runbook eingeben.

1.  Unsere Runbook ist der entsprechende *Workflow* -Schlüsselwort, den Namen unserer Runbook und die Klammern, die den gesamten Workflow Umhüllen leer. 

    ```
    Workflow MyFirstRunbook-Workflow
    {
    }
    ```

2.  Typ *Write-Output "Hello World."* zwischen den geschweiften Klammern. 
   
    ```
    Workflow MyFirstRunbook-Workflow
    {
      Write-Output "Hello World"
    }
    ```

3.  Speichern des Runbooks **Speichern**.<br> ![Runbook speichern](media/automation-first-runbook-textual/runbook-edit-toolbar-save.png)

## <a name="step-3---test-the-runbook"></a>Schritt 3: Test Runbooks

Vor dem veröffentlichen wir Runbook in der Produktion zur Verfügung stellen wollen wir testen, um sicherzustellen, dass sie ordnungsgemäß funktioniert. Beim Testen ein Runbook **dessen Entwurf** ausgeführt und die Ausgabe interaktiv anzeigen.

1.  Klicken Sie **im Bereich Test** im Bereich Test öffnen.<br> ![Testbereich](media/automation-first-runbook-textual/runbook-edit-toolbar-test-pane.png)
2.  Klicken Sie auf **Start** , um den Test zu starten. Dies sollte die einzige aktivierte Option.
3.  Ein [Runbook Auftrag](automation-runbook-execution.md) erstellt und Status angezeigt.  
    Der Status wird als *Warteschlange* , dass eine Arbeitskraft für ein Runbook in die Cloud sind gewartet wird gestartet. Es wird zu *Starten* nun bei eine Arbeitskraft behauptet das Projekt und dann *Ausführen* , wenn Runbooks tatsächlich gestartet.  
4.  Bei Beendigung des Auftrags Runbook wird die Ausgabe angezeigt. In diesem Fall sollte *Hello World*angezeigt werden.<br> ![Hallo Welt](media/automation-first-runbook-textual/test-output-hello-world.png)
5.  Schließen Sie das Fenster Test der Leinwand zurückzukehren.

## <a name="step-4---publish-and-start-the-runbook"></a>Schritt 4: Veröffentlichen und Runbooks starten

Das soeben erstellte Runbook ist immer noch im Entwurfsmodus. Wir müssen es veröffentlichen, bevor wir in der Produktion ausgeführt werden kann. Beim Veröffentlichen eines Runbooks überschrieben mit Entwurf vorhandene veröffentlichte Version. In unserem Fall haben wir nicht veröffentlichte Version noch da wir nur Runbook erstellt.

1.  Klicken Sie auf **Veröffentlichen** Runbook veröffentlichen und dann auf **Ja** .<br> ![Veröffentlichen](media/automation-first-runbook-textual/runbook-edit-toolbar-publish.png)
2.  Wenn Sie Bildlaufleiste um Runbooks jetzt im **Runbooks** anzeigen, zeigt es ein **Erstellungsstatus** **veröffentlicht**.
3.  Bildlauf nach rechts im Bereich für **MyFirstRunbook-Workflow**anzeigen.  
    Mit den Optionen oben können Runbooks starten, zu einem Zeitpunkt in der Zukunft zu planen oder ein [Webhook](automation-webhooks.md) erstellen, damit er gestartet werden kann über einen HTTP-Aufruf.
4.  Wir wollen nur Runbooks starten klicken Sie auf **Start** und dann auf **Ja** .<br> ![Runbook starten](media/automation-first-runbook-textual/runbook-toolbar-start.png)
5.  Eine im Auftrag wird Runbook Auftrag geöffnet, die wir gerade erstellt haben. Können wir diesen Bereich schließen, aber in diesem Fall wir werden offen, damit wir den Auftrag Fortschritt überwachen können.
6.  Der Status wird in **Jobübersicht** und entspricht der Status, die wir beim Runbooks getestet haben.<br> ![Job-Zusammenfassung](media/automation-first-runbook-textual/job-pane-summary.png)
7.  Sobald der Runbook Status *abgeschlossen*angezeigt wird, klicken Sie auf **Ausgabe**. Ausgabebereich wird geöffnet, und wir sehen unsere *Hello World*.<br> ![Job-Zusammenfassung](media/automation-first-runbook-textual/job-pane-output.png)  
8.  Schließen Sie das Fenster Ausgabe.
9.  Klicken Sie auf **Streams** Bereich des Streams Runbook Projekt öffnen. Sehen wir nur *Hello World* im Ausgabestream, aber dies kann anderen Streams für ein Runbook Projekt ausführlich und Fehler anzeigen, wenn Runbooks schreibt.<br> ![Job-Zusammenfassung](media/automation-first-runbook-textual/job-pane-streams.png)
10. Schließen Sie den Streams und im Auftrag MyFirstRunbook Bereich zurückzukehren.
11. Klicken Sie auf **Aufträge** Aufträge im für dieses Runbook öffnen. Hier werden alle Arbeitsplätze dieser Runbook. Es sollte nur ein Auftrag, da wir den Auftrag nur einmal ausführen angezeigt.<br> ![Aufträge](media/automation-first-runbook-textual/runbook-control-jobs.png)
12. Klicken Sie auf dieses Projekt im gleichen Auftrag öffnen, die wir beim wir Runbook gestartet angezeigt. So können Sie zurück, und zeigen Sie die Details eines Auftrags, der für ein bestimmtes Runbook erstellt wurde.

## <a name="step-5---add-authentication-to-manage-azure-resources"></a>Schritt 5 - Authentifizierung Azure Ressourcen

Wir getestet und unsere Runbook veröffentlicht, aber so weit es nicht sinnvoll. Wir möchten es Azure Ressourcen. Sie werden dazu jedoch ohne Authentifizierung mit den Anmeldeinformationen, die in [Komponenten](#prerequisites)bezeichnet werden, können. Dazu verwenden wir das Cmdlet **AzureRMAccount hinzufügen** .

1.  Öffnen Sie im Texten-Editor im MyFirstRunbook-Workflow auf **Bearbeiten** .<br> ![Runbook bearbeiten](media/automation-first-runbook-textual/runbook-toolbar-edit.png)
2.  Wir benötigen **Schreibzugriff** Ausgabezeile mehr, so weiter und löschen.
3.  Positionieren Sie den Cursor in eine leere Zeile zwischen den geschweiften Klammern.
4.  Geben Sie oder kopieren Sie und fügen Sie folgenden Code, der die Authentifizierung mit Automatisierung ausführen als Konto behandelt:

    ```
    $Conn = Get-AutomationConnection -Name AzureRunAsConnection 
    Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID `
    -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
    ```

5.  Klicken Sie auf **Test Bereich** , sodass Runbooks getestet werden können.
6.  Klicken Sie auf **Start** , um den Test zu starten. Nach Abschluss erhalten Sie eine Ausgabe wie die folgende, mit grundlegenden Informationen von Ihrem Konto. Dadurch wird bestätigt, dass die Anmeldeinformationen gültig ist.<br> ![Authentifizierung](media/automation-first-runbook-textual/runbook-auth-output.png)

## <a name="step-6---add-code-to-start-a-virtual-machine"></a>Schritt 6 - Code zum Starten der virtuellen Computer

Da unsere Runbook Azure-Abonnement authentifiziert, können wir Ressourcen verwalten. Wir fügen einen Befehl zu einem virtuellen Computer. Wählen alle virtuellen Computer in der Azure-Abonnement und jetzt werden wir in das Cmdlet Namen hartzucodieren.

1.  Geben Sie nach dem *Hinzufügen AzureRmAccount* *Start AzureRmVM-Namen 'VMName' - ResourceGroupName 'NameofResourceGroup'* die Namen und Name der Ressourcengruppe des virtuellen Computers zu.  

    ```
    workflow MyFirstRunbook-Workflow
    {
      $Conn = Get-AutomationConnection -Name AzureRunAsConnection
      Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
      Start-AzureRmVM -Name 'VMName' -ResourceGroupName 'ResourceGroupName'
    }
    ``` 

2.  Runbooks und klicken **im Bereich testen** , damit wir sie testen können.
3.  Klicken Sie auf **Start** , um den Test zu starten. Sobald dies abgeschlossen ist, sicher, dass der virtuelle Computer gestartet wurde.

## <a name="step-7---add-an-input-parameter-to-the-runbook"></a>Schritt 7 - Runbooks Eingabeparameter hinzufügen

Unsere Runbook gegenwärtig startet virtual, die hartcodiert im Runbook Computer, aber es wäre besser nutzen wir den virtuellen Computer begann Runbooks bestimmen. Jetzt fügen wir diese Funktionalität Runbook Eingabeparameter hinzufügen.

1.  Runbooks Parameter für *VMName* und *ResourceGroupName* hinzufügen und diese Variablen mit dem **Start-AzureRmVM** -Cmdlet wie im folgenden Beispiel verwenden. 

    ```
    workflow MyFirstRunbook-Workflow
    {
       Param(
        [string]$VMName,
        [string]$ResourceGroupName
       )  
     $Conn = Get-AutomationConnection -Name AzureRunAsConnection 
     Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
     Start-AzureRmVM -Name $VMName -ResourceGroupName $ResourceGroupName
    }
    ```

2.  Speichern Sie Runbook und öffnen Sie das Fenster Test. Beachten Sie, dass Sie nun Werte für zwei Eingabevariablen bereitstellen können, die im Test verwendet werden.
3.  Schließen Sie das Fenster Test.
4.  Klicken Sie auf **Veröffentlichen** , um die neue Version des Runbooks veröffentlichen.
5.  Beenden Sie den virtuellen Computer, den Sie im vorherigen Schritt gestartet.
6.  Klicken Sie auf **Starten** , um Runbooks starten. Geben Sie **VMName** und **ResourceGroupName** für den virtuellen Computer, den Sie starten wollen.<br> ![Runbook starten](media/automation-first-runbook-textual/automation-pass-params.png)

7.  Nach Abschluss das Runbook sicher, dass der virtuelle Computer gestartet wurde.

## <a name="next-steps"></a>Nächste Schritte

-  Zunächst mit grafisch Runbooks sehen Sie [Meine erste grafisch runbook](automation-first-runbook-graphical.md)
-  Zunächst mit PowerShell Runbooks finden Sie [meinen ersten PowerShell runbook](automation-first-runbook-textual-powershell.md)
-  Erfahren Sie mehr über Runbook Typen, deren vor- und Nachteile finden Sie unter [Azure Automation runbook](automation-runbook-types.md)
-  Weitere Informationen zu PowerShell-Skript Feature unterstützt, finden Sie [in Azure Automation unterstützt Native PowerShell-Skript](https://azure.microsoft.com/blog/announcing-powershell-script-support-azure-automation-2/)