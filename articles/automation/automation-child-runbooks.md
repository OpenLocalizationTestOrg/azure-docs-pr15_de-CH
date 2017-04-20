<properties 
   pageTitle="Untergeordnete Runbooks in Azure Automatisierung | Microsoft Azure"
   description="Beschreibt die verschiedenen Methoden für ein anderes Runbook ein Runbook in Azure Automation ab und Informationsaustausch zwischen ihnen."
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
   ms.date="08/17/2016"
   ms.author="magoedte;bwren" />

# <a name="child-runbooks-in-azure-automation"></a>Untergeordnete Runbooks in Azure Automation

Es empfiehlt sich in Azure Automation wieder verwendbare, modulare Runbooks mit einer einzelnen Funktion schreiben, die von anderen Runbooks verwendet werden kann. Übergeordnete Runbook rufen häufig eine oder mehrere untergeordnete Runbooks erforderlichen Funktionen ausführen. Zweierlei untergeordneten Runbook aufrufen und jede hat deutliche Unterschiede, die Sie verstehen sollten, damit Sie ermitteln können, die für verschiedenen Szenarien werden.

##  <a name="invoking-a-child-runbook-using-inline-execution"></a>Aufrufen eines untergeordneten Runbooks mit Inlineausführung

Zum Aufrufen einer Runbook Inline aus anderen Runbook Runbooks verwenden und geben Sie Werte für Parameter genau wie einer Aktivität oder eines Cmdlet.  Alle Runbooks in demselben automatisierungskonto stehen alle anderen auf diese Weise verwendet werden. Übergeordnete Runbook wartet untergeordneten Runbook abgeschlossen in die nächste Zeile und Ausgabe direkt an das übergeordnete Element zurückgegeben.

Eine Runbook Inline aufrufen wird dieselbe Aufgabe wie das übergeordnete Runbook. Es werden keine Auftragsverlauf des untergeordneten Runbooks, die Ausführung. Ausnahmen und Ausgabe von untergeordneten Runbook Stream werden das übergeordnete Element zugeordnet werden. Dies führt zu weniger Aufträge und erleichtert das verfolgen und beheben, da untergeordnete Runbook und seine Ausgabe Stream ausgelöste Ausnahmen der übergeordnete Auftrag zugeordnet sind.

Wenn ein Runbook veröffentlicht wird, müssen alle untergeordneten Runbooks, die ihn aufruft bereits veröffentlicht. Deswegen Azure Automatisierung mit jeder untergeordneten Runbooks erstellt, wenn ein Runbook kompiliert wird. Wenn sie nicht wird das übergeordnete Runbook ordnungsgemäß veröffentlicht, aber generiert eine Ausnahme aus, wenn es gestartet wird. In diesem Fall können Sie übergeordnete Runbook veröffentlichen, um untergeordnete Runbooks ordnungsgemäß verweisen. Sie müssen keine übergeordnete Runbook veröffentlichen, wenn die untergeordneten Runbooks geändert werden, da die Zuordnung wird bereits erstellt wurden.

Die Parameter eines untergeordneten Runbooks Inline aufgerufen können beliebigen Typs einschließlich komplexe Objekte und gibt keine [JSON-Serialisierung](automation-starting-a-runbook.md#runbook-parameters) wird mithilfe der Azure-Verwaltungsportal Runbook starten oder das Cmdlet "Start-AzureRmAutomationRunbook".


### <a name="runbook-types"></a>Runbook Typen

Welche anderen aufrufen können:

- Ein [Runbook PowerShell](automation-runbook-types.md#powershell-runbooks) und [grafisch Runbooks](automation-runbook-types.md#graphical-runbooks) können jedes Inline aufrufen (beide werden PowerShell basiert).
- [PowerShell Workflow Runbook](automation-runbook-types.md#powershell-workflow-runbooks) und grafisch PowerShell Workflow Runbooks kann jedes Inline aufrufen (beide werden PowerShell Workflow basierend)
- PowerShell-Typen PowerShell Workflowtypen nicht gegenseitig Inline aufrufen und müssen Start AzureRmAutomationRunbook verwenden.
    
Wenn Filterreihenfolge wichtig werden veröffentlicht:

- Die Reihenfolge veröffentlichen Runbooks ist nur für PowerShell Workflow und grafisch PowerShell Workflow Runbooks.


Beim Aufrufen einer Graphical oder PowerShell Workflow untergeordneten Runbook Inline Ausführung verwenden Sie einfach den Namen des Runbooks.  Beim Aufrufen einer PowerShell untergeordneten Runbook muss den Namen vorangestellt *.\\ * an das Skript im lokalen Verzeichnis befindet. 

### <a name="example"></a>Beispiel

Im folgenden Beispiel wird ein Test untergeordneten Runbook, die drei Parameter, ein komplexes Objekt, eine ganze Zahl und einen booleschen Wert annimmt. Die Ausgabe des untergeordneten Runbooks wird einer Variablen zugewiesen.  In diesem Fall ist das untergeordnete Runbook PowerShell Workflow runbook

    $vm = Get-AzureRmVM –ResourceGroupName "LabRG" –Name "MyVM"
    $output = PSWF-ChildRunbook –VM $vm –RepeatCount 2 –Restart $true

Dasselbe Beispiel in PowerShell Runbook als untergeordnetes Element folgt.

    $vm = Get-AzureRmVM –ResourceGroupName "LabRG" –Name "MyVM"
    $output = .\PS-ChildRunbook –VM $vm –RepeatCount 2 –Restart $true



##  <a name="starting-a-child-runbook-using-cmdlet"></a>Starten eines untergeordneten Runbooks mit cmdlet

[Start-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603661.aspx) -Cmdlet können Sie ein Runbook Siehe [Runbook mit Windows PowerShell zu](../automation-starting-a-runbook.md#starting-a-runbook-with-windows-powershell)starten. Es gibt zwei Modi für dieses Cmdlet verwenden.  In einem Modus gibt das Cmdlet die Auftrags-Id als untergeordnetes Projekt für untergeordnete Runbook erstellt wird.  Im anderen Modus ermöglicht durch Angabe der **-Warten** Parameter das Cmdlet wartet, bis die untergeordneten Auftrag beendet und kehrt die Ausgabe von untergeordneten Runbook.

Von einem untergeordneten Runbook begann mit einem Cmdlet wird separat von der übergeordneten Runbook ausgeführt. Dies führt zu mehr Arbeitsplätze als Inline Runbook aufrufen und erschwert verfolgen. Das übergeordnete Element kann mehrere untergeordnete Runbooks asynchron starten, ohne warten abgeschlossen. Für dieselbe Art der parallelen Ausführung untergeordnete Runbooks Inline aufrufen müsste das übergeordnete Runbook [parallel-Schlüsselwort](automation-powershell-workflow.md#parallel-processing)verwenden.

Parameter für ein Kind Runbook begann mit einem Cmdlet dienen als Hashtabelle [Runbook Parameter](automation-starting-a-runbook.md#runbook-parameters)beschrieben. Nur einfache Datentypen können verwendet werden. Hat das Runbook Parameter mit einem komplexen Datentyp muss es Inline aufgerufen werden.

### <a name="example"></a>Beispiel

Im folgenden Beispiel startet ein Runbook untergeordnete Parameter und wartet dann, bis er mithilfe der Start-AzureRmAutomationRunbook-Parameter zu warten. Abschluss wird die Ausgabe von untergeordneten Runbook gesammelt.

    $params = @{"VMName"="MyVM";"RepeatCount"=2;"Restart"=$true} 
    $joboutput = Start-AzureRmAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Test-ChildRunbook" -ResouceGroupName "LabRG" –Parameters $params –wait


## <a name="comparison-of-methods-for-calling-a-child-runbook"></a>Vergleich der Methoden für ein Runbook untergeordneten aufrufen

Die folgenden Tabelle werden die Unterschiede zwischen den beiden Methoden für ein Runbook aus anderen Runbook aufrufen.

| | Inline| Cmdlets|
|:---|:---|:---|
|Auftrag|Untergeordnete Runbooks führen in dieselbe Aufgabe wie das übergeordnete Element.|Für untergeordnete Runbook wird separat erstellt.|
|Ausführung|Übergeordnete Runbook wartet untergeordneten Runbook abschließen, bevor Sie fortfahren.|Übergeordnete Runbook weiterhin unmittelbar nach dem Start untergeordneten Runbook untergeordneten Einzelvorgang abgeschlossen *oder* übergeordnete Runbook wartet.|
|Ausgabe|Übergeordnete Runbook erhalten direkt Ausgabe untergeordneten Runbook.|Übergeordnete Runbook muss Ausgabe Abrufen von untergeordneten Runbook Auftrag *oder* übergeordnete Runbook direkt erhalten Ausgabe von untergeordneten Runbook.|
|Parameter|Werte für untergeordnete Runbook Parameter einzeln angegeben und können einen beliebigen Datentyp aufweisen.|Werte für untergeordnete Runbook Parameter in eine Hashtabelle kombiniert müssen und nur einfache zählen array und Objekt-Datentypen, Nutzung der JSON-Serialisierung.|
|Automation-Konto|Übergeordnete Runbook können nur untergeordnete Runbook in demselben automatisierungskonto.|Übergeordnete Runbook können untergeordnete Runbook Automation-Konto aus der gleichen Azure-Abonnements und sogar ein anderes Abonnement haben Sie eine Verbindung.|
|Veröffentlichen|Untergeordnete Runbook muss veröffentlicht werden, bevor übergeordnete Runbook veröffentlicht wird.|Untergeordnete Runbook muss veröffentlicht vor dem übergeordneten Runbook gestartet wird.|

## <a name="next-steps"></a>Nächste Schritte

- [Starten Sie ein Runbook in Azure Automation](automation-starting-a-runbook.md)
- [Runbook Ausgabe und Nachrichten in Azure Automation](automation-runbook-output-and-messages.md)
