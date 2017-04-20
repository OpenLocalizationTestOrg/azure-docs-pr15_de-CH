<properties 
   pageTitle="Azure Runbook Automatisierungstypen"
   description="Beschreibt die verschiedenen Typen von Runbooks, mit denen Sie in Azure Automation und Aspekte, die Sie berücksichtigen sollten beim Bestimmen des um zu verwendenden Typs. "
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

# <a name="azure-automation-runbook-types"></a>Azure Automatisierungstypen runbook

Azure Automation unterstützt vier Typen von Runbooks kurz beschrieben werden in der folgenden Tabelle.  Die folgenden Abschnitte enthalten weitere Informationen zu den einzelnen Aspekte bei einzelnen einschließlich.


| Typ |  Beschreibung |
|:---|:---|
| [Grafik](#graphical-runbooks) | Basiert auf Windows PowerShell und erstellt und vollständig bearbeitete Grafikeditor Azure-Portal. | 
| [Grafisch PowerShell-Workflow](#graphical-runbooks) | Basierend auf Windows PowerShell Workflow erstellt und im Grafik-Editor von Azure-Portal vollständig bearbeitet. 
| [PowerShell](#powershell-runbooks) | Text für ein Runbook basierend auf Windows PowerShell-Skript.
| [PowerShell-Workflow](#powershell-workflow-runbooks) | Text für ein Runbook basiert auf Windows PowerShell. |


## <a name="graphical-runbooks"></a>Grafisch runbooks

[Grafik-](automation-runbook-types.md#graphical-runbooks) und grafisch PowerShell Workflow Runbooks werden erstellt und mit dem Grafik-Editor im Azure-Portal bearbeitet.  In eine Datei exportieren und dann in eine andere automatisierungskonto importieren, aber nicht erstellen oder mit einem anderen Programm bearbeiten.  Grafisch Runbooks PowerShell-Code generieren, aber nicht direkt anzeigen oder ändern Sie den Code. Grafisch Runbooks in [Textformat](automation-runbook-types.md)konvertiert werden kann und ein Runbook Text umgewandelt werden kann Grafikformat. Grafisch Runbooks können Runbooks grafisch PowerShell Workflow importieren und umgekehrt konvertiert.

### <a name="advantages"></a>Vorteile

- Erstellen Sie mit minimalem [PowerShell](automation-powershell-workflow.md)Runbooks.
- Darzustellen Sie Prozesse visuell.
- Schließen Sie anderer Runbooks als untergeordnete Runbooks auf hoher Ebene Workflows erstellen ein.


### <a name="limitations"></a>Grenzen

- Runbook außerhalb Azure-Portal bearbeiten
- Erfordern eine Codeaktivität mit PowerShell-Code so komplexen Logik.
- Nicht anzeigen oder den PowerShell-Code, der vom Workflow grafisch erstellt direkt bearbeiten. Beachten Sie, dass den Code anzeigen können, Code Aktivitäten erstellen.


## <a name="powershell-runbooks"></a>PowerShell runbooks

PowerShell Runbooks basiert auf Windows PowerShell.  Sie bearbeiten direkt den Code Runbooks Texteditor in Azure-Portal.  Sie können auch alle offline Texteditor und [importieren das Runbook](http://msdn.microsoft.com/library/azure/dn643637.aspx) in Azure.

### <a name="advantages"></a>Vorteile

- Implementieren Sie alle komplexen Logik mit PowerShell-Code ohne die zusätzliche Komplexität der PowerShell Workflow. 
- Runbook startet schneller Graphical oder PowerShell Workflow Runbooks da vor Ausführung kompiliert werden muss.

### <a name="limitations"></a>Grenzen

- Muss mit PowerShell vertraut sein.
- Können [parallele Verarbeitung](automation-powershell-workflow.md#parallel-processing) mehrerer Aktionen parallel ausführen.
- Können [Prüfpunkte](automation-powershell-workflow.md#checkpoints) Runbook bei Fehler fortsetzen.
- PowerShell Workflow Runbooks und grafisch Runbooks können nur werden als untergeordnete Runbooks mit dem Start-AzureAutomationRunbook-Cmdlet schafft einen neuen Auftrag.

### <a name="known-issues"></a>Bekannte Probleme
Aktuelle bekannte Probleme mit PowerShell Runbooks folgen.

- PowerShell Runbooks kann keiner unverschlüsselten [Variable Anlage](automation-variables.md) mit dem Wert null abrufen kann.
- PowerShell Runbooks kann nicht abgerufen werden eine [Variable Anlage](automation-variables.md) mit *~* im Namen.
- Get-Process in einer Schleife in einem PowerShell Absturz Runbook 80 Iterationsschritten. 
- PowerShell Runbook kann fehlschlagen, wenn es versucht, gleichzeitig eine große Datenmenge in den Ausgabestream geschrieben.   Sie können in der Regel umgehen dieses Problem mit der Ausgabe beim Arbeiten mit großen Objekten benötigten Informationen.  Ausgabe Sie beispielsweise statt etwas wie *Get-Process*ausgeben, die erforderlichen Felder mit Get-Process *| Wählen Sie ProcessName CPU*.

## <a name="powershell-workflow-runbooks"></a>PowerShell Workflow runbooks

PowerShell Workflow Runbooks sind Text Runbooks basiert auf [Windows PowerShell](automation-powershell-workflow.md).  Sie bearbeiten direkt den Code Runbooks Texteditor in Azure-Portal.  Sie können auch alle offline Texteditor und [importieren das Runbook](http://msdn.microsoft.com/library/azure/dn643637.aspx) in Azure.

### <a name="advantages"></a>Vorteile

- Implementieren Sie alle komplexen Logik mit PowerShell Workflowcode.
- Verwenden Sie [Prüfpunkte](automation-powershell-workflow.md#checkpoints) Runbook bei Fehler wieder.
- Verwenden Sie [parallele Verarbeitung](automation-powershell-workflow.md#parallel-processing) mehrerer Aktionen parallel ausgeführt.
- Andere Grafik Runbooks und PowerShell Workflow Runbooks als untergeordnete Runbooks auf hoher Ebene Workflows erstellen gehören.


### <a name="limitations"></a>Grenzen

- Autor muss mit PowerShell Workflow vertraut sein.
- Die zusätzliche Komplexität der PowerShell Workflow wie [deserialisiert Objekte](automation-powershell-workflow.md#code-changes)muss Runbook behandelt.
- Runbook länger als PowerShell Runbooks gestartet werden, da er vor der Ausführung kompiliert werden muss.
- PowerShell Runbooks beschränkt als untergeordnete Runbooks mit dem Start-AzureAutomationRunbook-Cmdlet schafft einen neuen Auftrag.


## <a name="considerations"></a>Hinweise

Sie sollten folgende zusätzliche Aspekte berücksichtigen bestimmt, welchen Typ Sie für ein bestimmtes Runbook verwenden.

- Sie können keine Runbooks grafisch auf Text oder umgekehrt konvertieren.
- Gibt eine untergeordnete Runbook Runbooks verschiedener unter Einschränkungen.  Weitere Informationen finden Sie im [untergeordneten Runbooks in Azure Automation](automation-child-runbooks.md) .

  
## <a name="next-steps"></a>Nächste Schritte

- Mehr zum Erstellen von Grafiken für ein Runbook finden Sie unter [Graphical authoring in Azure Automation](automation-graphical-authoring-intro.md)
- Die Unterschiede zwischen PowerShell und PowerShell kennen Workflows für Runbooks, siehe [Learning Windows PowerShell Workflow](automation-powershell-workflow.md)
- Weitere Informationen zum Erstellen oder importieren ein Runbook finden Sie unter [Erstellen oder importieren Sie ein Runbook](automation-creating-importing-runbook.md)



