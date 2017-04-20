<properties 
   pageTitle="Learning PowerShell Workflow"
   description="In diesem Artikel kurz Autoren mit PowerShell soll die spezifischen Unterschiede zwischen PowerShell und PowerShell Workflow kennen."
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
   ms.author="bwren" />

# <a name="learning-windows-powershell-workflow"></a>Learning Windows PowerShell Workflow

Runbooks in Azure Automation werden als Windows PowerShell Workflows implementiert.  Windows PowerShell Workflow ähnelt ein Windows PowerShell-Skript jedoch hat einige bedeutende Unterschiede an einen neuen Benutzer verwirrend sein können.  Dieser Artikel richtet sich an Benutzer bereits mit PowerShell und kurz erläutert Konzepte, die Sie benötigen, wenn Sie ein PowerShell-Skript ein PowerShell-Workflow für die Verwendung in einem Runbook konvertieren.  

Ein Workflow ist eine Schrittsequenz programmiert und die langfristige Aufgaben oder die Koordinierung mehrerer Schritte über mehrere Geräte oder verwalteten Knoten. Der Workflow in einem normalen Skript Vorteile gegen mehrere Geräte gleichzeitig ausführen und die Fähigkeit zu automatisch. Ein Windows PowerShell-Workflow ist ein Windows PowerShell-Skript, die Windows Workflow Foundation nutzt. Während der Workflow mit Windows PowerShell-Syntax geschrieben und von Windows PowerShell gestartet, wird er von Windows Workflow Foundation verarbeitet.

Weitere Informationen zu den Themen in diesem Artikel finden Sie unter [Erste Schritte mit Windows PowerShell Workflow](http://technet.microsoft.com/library/jj134242.aspx).

## <a name="types-of-runbook"></a>Typen von runbook

Es gibt drei Arten von Runbook in Azure Automation, *PowerShell Workflow* *PowerShell* und *grafisch*.  Sie definieren die Runbook Runbooks erstellen und ein Runbook kann nicht in den Typ konvertiert werden, erstellt werden.

PowerShell Workflow Runbooks und PowerShell Runbooks verwenden für Benutzer, die lieber direkt mit PowerShell-Code entweder in Azure Automation den Text-Editor oder eine offline-Editor wie PowerShell ISE. Wenn ein Runbook PowerShell Workflow erstellen, sollten Sie die Informationen in diesem Artikel verstehen. 

Grafisch Runbooks können Sie ein Runbook dieselben Aktivitäten und Cmdlets aber mit einer Benutzeroberfläche, die verbirgt die Komplexität der zugrunde liegenden PowerShell Workflow erstellen.  Konzepte in diesem Artikel Prüfpunkte und parallele Ausführung gelten weiterhin für grafisch Runbooks, jedoch müssen die ausführliche Syntax kümmern. 

## <a name="basic-structure-of-a-workflow"></a>Grundlegende Struktur eines Workflows

Der erste Schritt zum Konvertieren eines PowerShell-Skripts zu PowerShell-Workflow ist es mit dem **Workflow** -Schlüsselwort einschließen.  Ein Workflow beginnt mit dem **Workflow** -Schlüsselwort, gefolgt von den Hauptteil des Skripts in geschweiften Klammern. Der Name des Workflows folgt **Workflow** -Schlüsselwort in der folgenden Syntax dargestellt. 

    Workflow Test-Workflow
    {
       <Commands>
    }

Der Name des Workflows muss dem Namen der Automatisierung Runbooks entsprechen. Runbooks importiert wird, kann der Dateiname muss der Name des Workflows und ps1 enden muss.

Workflow Parameter hinzufügen, verwenden Sie das **Param** -Schlüsselwort wie ein Skript. 

## <a name="code-changes"></a>Ändern von Code

PowerShell Workflowcode sieht fast identisch mit PowerShell Skriptcode außer einige bedeutende Unterschiede.  In den folgenden Abschnitten beschrieben, wie sich ein PowerShell-Skript dafür in einem Workflow ausführen zu müssen.

### <a name="activities"></a>Aktivitäten

Eine Aktivität ist eine bestimmte Aufgabe in einem Workflow. Wie ein Skript aus einem oder mehreren Befehlen besteht, ein Workflow einer oder mehreren Aktivitäten besteht, die nacheinander ausgeführt werden. Windows PowerShell Workflow konvertiert automatisch viele der Windows PowerShell-Cmdlets Aktivitäten bei der Ausführung eines Workflows. Bei Angabe eines dieser Cmdlets in Ihrem Runbook wird die entsprechende Aktivität tatsächlich von Windows Workflow Foundation ausgeführt. Diese Cmdlets ohne entsprechende Aktivität wird Windows PowerShell Workflow automatisch Cmdlet in [InlineScript](#inlinescript) Aktivität ausgeführt. Es gibt eine Reihe von Cmdlets, die ausgeschlossen werden und nicht in einem Workflow verwendet werden, wenn Sie explizit in einem Block InlineScript einschließen. Weitere Informationen zu diesen Konzepten finden Sie unter [Aktivitäten in Skript Workflows verwenden](http://technet.microsoft.com/library/jj574194.aspx).

Workflowaktivitäten Teilen gemeinsame Parameter zur Konfiguration ihrer Ausführung. Ausführliche Informationen über die allgemeinen Parameter finden Sie unter [About_WorkflowCommonParameters](http://technet.microsoft.com/library/jj129719.aspx).

### <a name="positional-parameters"></a>Positionelle Parameter

Sie können keine Positionsparameter und Cmdlets in einem Workflow verwenden.  Dies bedeutet lediglich, Parameternamen zu verwenden.

Betrachten Sie beispielsweise den folgenden Code, der alle Dienste abruft.

     Get-Service | Where-Object {$_.Status -eq "Running"}

Wenn Sie versuchen, denselben Code in einem Workflow ausführen, erhalten Sie eine Meldung wie "Parametersatz aufgelöst werden kann mit den angegebenen benannten Parameter."  Um dies zu korrigieren, geben Sie den Parameternamen wie im folgenden.

    Workflow Get-RunningServices
    {
        Get-Service | Where-Object -FilterScript {$_.Status -eq "Running"}
    }

### <a name="deserialized-objects"></a>Deserialisierte Objekte

Objekte in Workflows werden deserialisiert.  Dies bedeutet, dass ihre Eigenschaften verfügbar sind, jedoch nicht deren Methoden.  Betrachten Sie beispielsweise den folgenden PowerShell-Code, der einen Dienst mithilfe der Stop-Methode des Objekts beendet.

    $Service = Get-Service -Name MyService
    $Service.Stop()

Wenn Sie versuchen, diese in einem Workflow auszuführen, erhalten Sie eine Fehlermeldung "Methodenaufruf nicht in einem Windows PowerShell-Workflow unterstützt".  

Eine Option ist diese beiden Codezeilen ein [InlineScript](#InlineScript) Block in diesem Fall $Service umschließen ein Dienstobjekt innerhalb des Blocks. 

    Workflow Stop-Service
    {
        InlineScript {
            $Service = Get-Service -Name MyService
            $Service.Stop()
        }
    } 

Eine weitere Option ist ein anderes Cmdlet, das dieselbe Funktionen wie die Methode führt verwenden, sofern verfügbar.  In unserem Beispiel das Cmdlet "Stop-Service" bietet die gleiche Funktionalität wie die Stop-Methode und können Sie Folgendes für einen Workflow.

    Workflow Stop-MyService
    {
        $Service = Get-Service -Name MyService
        Stop-Service -Name $Service.Name
    }


## <a name="inlinescript"></a>InlineScript

**InlineScript** Aktivität ist nützlich, wenn Sie einen oder mehrere Befehle als herkömmliche PowerShell-Skript statt PowerShell Workflow ausführen müssen.  Während Befehle in einem Workflow an Windows Workflow Foundation zur Verarbeitung gesendet werden, werden von Windows PowerShell-Befehle in einem Block InlineScript verarbeitet. 

InlineScript verwendet die unten aufgeführte Syntax.

    InlineScript
    {
      <Script Block>
    } <Common Parameters>

Ausgabe kehren von einem InlineScript durch die Ausgabe einer Variablen zuweisen. Im folgenden Beispiel beendet einen Dienst und gibt dann den Namen.

    Workflow Stop-MyService
    {
        $Output = InlineScript {
            $Service = Get-Service -Name MyService
            $Service.Stop()
            $Service
        }

        $Output.Name
    }


Werte können in einem Block InlineScript übergeben, aber **$Using** Bereich Modifizierer verwenden.  Im folgende Beispiel entspricht dem vorherigen Beispiel, der Namen einer Variable bereitgestellt wird. 

    Workflow Stop-MyService
    {
        $ServiceName = "MyService"
    
        $Output = InlineScript {
            $Service = Get-Service -Name $Using:ServiceName
            $Service.Stop()
            $Service
        }

        $Output.Name
    }


Zwar InlineScript Aktivitäten in bestimmten Workflows von entscheidender Bedeutung, sie Workflow Konstrukte nicht unterstützt und sollte nur verwendet werden, wenn aus den folgenden Gründen erforderlich:

- Sie können [Prüfpunkte](#Checkpoints) in einem Block InlineScript nicht verwenden. Tritt ein Fehler im Block muss vom Anfang des Blocks fortgesetzt werden.
- Sie können keine [parallele Ausführung](#parallel-execution) innerhalb einer InlineScriptBlock.
- InlineScript betrifft Skalierbarkeit des Workflows, da Windows PowerShell-Sitzung für die gesamte Länge des Blocks InlineScript enthält.

Weitere Informationen zur Verwendung von InlineScript finden Sie unter [Windows PowerShell-Befehle in einem Workflow ausgeführt](http://technet.microsoft.com/library/jj574197.aspx) und [About_InlineScript](http://technet.microsoft.com/library/jj649082.aspx).


## <a name="parallel-processing"></a>Parallele Verarbeitung

Ein Vorteil von Windows PowerShell Workflows ist die Möglichkeit der Befehle parallel statt sequenziell mit einem normalen Skript ausführen. 

**Parallele** -Schlüsselwort können Sie um einen Skriptblock mit mehreren Befehlen zu erstellen, die gleichzeitig ausgeführt werden. Die unten dargestellte Syntax wird verwendet. In diesem Fall werden Activity1 und Activity2 zur gleichen Zeit gestartet. Activity3 beginnt erst, wenn Activity1 und Activity2 abgeschlossen haben.

    Parallel
    {
      <Activity1>
      <Activity2>
    }
    <Activity3>


Betrachten Sie beispielsweise die folgenden PowerShell-Befehle, die mehrere Dateien in einem Netzwerkziel kopieren.  Diese Befehle werden nacheinander ausgeführt, so, dass eine Datei kopieren beendet, bevor die nächste beginnt.     

    $Copy-Item -Path C:\LocalPath\File1.txt -Destination \\NetworkPath\File1.txt
    $Copy-Item -Path C:\LocalPath\File2.txt -Destination \\NetworkPath\File2.txt
    $Copy-Item -Path C:\LocalPath\File3.txt -Destination \\NetworkPath\File3.txt

Der folgende Workflow führt Befehle parallel, sodass sie alle gleichzeitig kopieren.  Erst alle wird vollständig kopiert die Vollzugsmeldung angezeigt.

    Workflow Copy-Files
    {
        Parallel 
        {
            $Copy-Item -Path "C:\LocalPath\File1.txt" -Destination "\\NetworkPath"
            $Copy-Item -Path "C:\LocalPath\File2.txt" -Destination "\\NetworkPath"
            $Copy-Item -Path "C:\LocalPath\File3.txt" -Destination "\\NetworkPath"
        }

        Write-Output "Files copied."
    }


Können die **ForEach-parallele** Prozess Befehle für jedes Element in einer Auflistung gleichzeitig erstellen. Die Elemente in der Auflistung werden parallel verarbeitet, während die Befehle im Skriptblock sequenziell ausgeführt. Die unten dargestellte Syntax wird verwendet. In diesem Fall startet Activity1 gleichzeitig für alle Elemente in der Auflistung. Für jedes Element wird nach Abschluss der Activity1 Activity2 gestartet. Activity3 beginnt erst, wenn Activity1 und Activity2 für alle Elemente abgeschlossen haben.

    ForEach -Parallel ($<item> in $<collection>)
    {
      <Activity1>
      <Activity2>
    }
    <Activity3>

Im folgende Beispiel ähnelt dem vorherigen Beispiel Dateien parallel.  In diesem Fall wird eine Meldung für jede Datei angezeigt, nachdem sie kopiert.  Erst alle ist vollständig kopiert die Fertigstellung Meldung.

    Workflow Copy-Files
    {
        $files = @("C:\LocalPath\File1.txt","C:\LocalPath\File2.txt","C:\LocalPath\File3.txt")

        ForEach -Parallel ($File in $Files) 
        {
            $Copy-Item -Path $File -Destination \\NetworkPath
            Write-Output "$File copied."
        }
        
        Write-Output "All files copied."
    }

> [AZURE.NOTE]  Wir empfehlen nicht untergeordneten Runbooks parallel ausgeführt werden, da dies zu unzuverlässigen Ergebnissen angezeigt wurde.  Die Ausgabe der untergeordneten Runbook manchmal angezeigt und in einem untergeordneten Runbook kann Einstellungen anderen parallelen untergeordnete runbooks 


## <a name="checkpoints"></a>Prüfpunkte

Ein *Prüfpunkt* ist ein Snapshot des aktuellen Zustands des Workflows, der den aktuellen Wert für Variablen und darauf generierte Ausgabe enthält. Wenn ein Workflow Fehler oder angehalten wird, wird das nächste Mal ausgeführt wird es vom letzten Prüfpunkt statt Anfang der starten.  Sie können einen Checkpoint in einem Workflow mit **Checkpoint** Workflowaktivität festlegen.

Im folgenden Beispielcode eine Ausnahme auftritt, nachdem Activity2 verursacht des Workflows enden. Wenn der Workflow erneut ausgeführt wird, wird mit Activity2 direkt nach dem letzten Checkpoint festgelegt war.

    <Activity1>
    Checkpoint-Workflow
    <Activity2>
    <Exception>
    <Activity3>

Sie sollten Prüfpunkte in einem Workflow festlegen, Aktivitäten, die Ausnahme anfällig und sollte wiederholt werden, wenn der Workflow fortgesetzt wird. Der Workflow kann z. B. einen virtuellen Computer erstellen. Sie können einen Checkpoint vor und nach den Befehlen zum Erstellen des virtuellen Computers festlegen. Wenn die Erstellung fehlschlägt würde die Befehle wiederholt werden, wenn der Workflow erneut gestartet wird. Wenn das der schlägt nach die Erstellung erfolgreich ist, wird der virtuelle Computer nicht erneut erstellt wird, wenn der Workflow fortgesetzt wird.

Im folgenden Beispiel mehrere Dateien an einen anderen Speicherort kopiert und setzt einen Checkpoint nach jeder Datei.  Wenn der Speicherort im Netzwerk verloren geht, wird der Workflow Fehler beendet.  Beim erneuten Starten wird fortgesetzt des letzten Checkpoints zurückgesetzt, d. h. die bereits kopierten Dateien übersprungen werden.

    Workflow Copy-Files
    {
        $files = @("C:\LocalPath\File1.txt","C:\LocalPath\File2.txt","C:\LocalPath\File3.txt")

        ForEach ($File in $Files) 
        {
            $Copy-Item -Path $File -Destination \\NetworkPath
            Write-Output "$File copied."
            Checkpoint-Workflow
        }
        
        Write-Output "All files copied."
    }

Da [Suspend](https://technet.microsoft.com/library/jj733586.aspx) Workflowaktivität aufrufen oder nach dem letzten Checkpoint Username-Anmeldeinformationen nicht beibehalten werden, müssen Sie die Anmeldeinformationen festgelegt auf null festlegen und anschließend Abrufen wieder aus den Asset Store **Workflow unterbrechen** oder Checkpoint aufgerufen wurde.  Andernfalls erhalten Sie folgende Fehlermeldung: *Workflow-Aufgabe kann nicht fortgesetzt werden, entweder weil Dauerhaftigkeitsdaten konnte nicht vollständig gespeichert oder gespeicherte Dauerhaftigkeitsdaten wurde beschädigt. Neustart den Workflow.*

Denselben Code veranschaulicht, wie in der PowerShell Workflow Runbooks dafür.

       
    workflow CreateTestVms
    {
       $Cred = Get-AzureAutomationCredential -Name "MyCredential"
       $null = Add-AzureRmAccount -Credential $Cred

       $VmsToCreate = Get-AzureAutomationVariable -Name "VmsToCreate"

       foreach ($VmName in $VmsToCreate)
         {
          # Do work first to create the VM (code not shown)
        
          # Now add the VM
          New-AzureRmVm -VM $Vm -Location "WestUs" -ResourceGroupName "ResourceGroup01"

          # Checkpoint so that VM creation is not repeated if workflow suspends
          $Cred = $null
          Checkpoint-Workflow
          $Cred = Get-AzureAutomationCredential -Name "MyCredential"
          $null = Add-AzureRmAccount -Credential $Cred
         }
     } 


Dies ist nicht erforderlich, wenn die Authentifizierung mit einem Konto ausführen als mit einem Service principal konfiguriert.  

Weitere Informationen zu Prüfpunkten finden Sie unter [Prüfpunkte Skript Workflow hinzufügen](http://technet.microsoft.com/library/jj574114.aspx).


## <a name="next-steps"></a>Nächste Schritte

- Zunächst mit PowerShell Workflow Runbooks finden Sie [meinen ersten PowerShell Workflow runbook](automation-first-runbook-textual.md) 
