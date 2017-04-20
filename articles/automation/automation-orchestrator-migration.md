<properties
   pageTitle="Migrieren von Orchestrator Azure Automatisierung | Microsoft Azure"
   description="Beschreibt, wie System Center Orchestrator Azure Automatisierung Runbooks und Integration Packs migrieren."
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


# <a name="migrating-from-orchestrator-to-azure-automation-beta"></a>Migrieren von Orchestrator Azure Automation (Beta)

In [System Center Orchestrator](http://technet.microsoft.com/library/hh237242.aspx) Runbooks basieren auf Aktivitäten Integration Packs, die speziell für Orchestrator Runbooks in Azure Automation basieren auf Windows PowerShell geschrieben werden.  [Grafisch Runbooks](automation-runbook-types.md#graphical-runbooks) in Azure Automation haben ein ähnliches Erscheinungsbild zu Orchestrator Runbooks mit ihren Aktivitäten zur Darstellung von PowerShell-Cmdlets untergeordnete Runbooks und Anlagen.

[System Center Orchestrator Migration Toolkit](http://www.microsoft.com/download/details.aspx?id=47323&WT.mc_id=rss_alldownloads_all) enthält Tools zur Konvertierung Runbooks von Orchestrator in Azure Automation.  Neben Runbooks selbst konvertieren, müssen Sie die Integration Packs mit Aktivitäten, die Runbooks verwenden, in Integrationsmodule mit Windows PowerShell-Cmdlets konvertieren.  

Es folgt das grundlegende Verfahren zum Konvertieren von Orchestrator Runbooks in Azure Automation.  Jeder dieser Schritte wird in den folgenden Abschnitten ausführlich beschrieben.

1.  [System Center Orchestrator Migration Toolkit](http://www.microsoft.com/download/details.aspx?id=47323&WT.mc_id=rss_alldownloads_all) enthält die Tools und die in diesem Artikel beschriebenen Module herunterladen
2.  Azure Automation importieren Sie [Standardaktivitäten Modul](#standard-activities-module) .  Dies schließt konvertierte Versionen Orchestrator Standardaktivitäten, die konvertierte Runbooks verwendet.
3.  Importieren Sie [System Center Orchestrator Integrationsmodule](#system-center-orchestrator-integration-modules) in Azure für diese Integration Packs anhand der Runbooks, die System Center zugreifen.
4.  Benutzerdefinierte und Drittanbietern Integration Packs mit [Integration Pack Converter](#integration-pack-converter) konvertieren und Importieren in Azure Automation.
5.  Orchestrator Runbooks mit dem [Runbook Konverter](#runbook-converter) konvertiert und in Azure Automation installieren.
6.  Manuell erstellen Sie erforderliche Orchestrator Anlagen in Azure Automation, da Runbook Konverter diese Ressourcen nicht konvertieren.
7.  Konfigurieren einer [Hybrid Runbook Worker](#hybrid-runbook-worker) im lokalen Rechenzentrum konvertierte Runbooks ausgeführt, die auf lokale Ressourcen zugreifen.

## <a name="service-management-automation"></a>Service Management-Automatisierung

[Service Management-Automatisierung](http://technet.microsoft.com/library/dn469260.aspx) (SMA) speichert Runbooks im lokalen Rechenzentrum wie Orchestrator und führt dieselbe Integrationsmodule Azure Automatisierung verwendet. [Runbook Konverter](#runbook-converter) konvertiert Orchestrator Runbooks, grafisch Runbooks jedoch die SMA nicht unterstützt werden.  [Standardmodul Aktivitäten](#standard-activities-module) und [System Center Orchestrator Integrationsmodule](#system-center-orchestrator-integration-modules) können weiterhin in SMA installieren, jedoch müssen Sie manuell [die Runbooks schreiben](http://technet.microsoft.com/library/dn469262.aspx).

## <a name="hybrid-runbook-worker"></a>Hybrid Runbook Worker

Runbooks in Orchestrator auf einem Datenbankserver gespeichert und auf Runbook Servern im lokalen Rechenzentrum ausgeführt.  Runbooks in Azure Automation in der Azure-Cloud gespeichert und kann im lokalen Rechenzentrum über einen [Hybrid Runbook Worker](automation-hybrid-runbook-worker.md)ausgeführt.  Dies ist normalerweise Ausführung wird Runbooks von Orchestrator konvertiert werden, da sie auf lokalen Servern ausgeführt werden soll.

## <a name="integration-pack-converter"></a>Integration Pack Converter

Integration Pack Converter konvertiert Integrationspakete, die erstellt wurden mit [Orchestrator Integration Toolkit (OIT)](http://technet.microsoft.com/library/hh855853.aspx) , Integrationsmodule basierend auf Windows PowerShell, die in Azure Automation oder Service Management-Automatisierung importiert werden können.  

Beim Ausführen von Converter Pack Integration sind ein Assistent angezeigt, mit denen Sie eine Integration Pack (.oip) Datei.  Der Assistent führt die Aktivitäten dieser Integration Packs und können Sie auswählen, die migriert werden.  Beim Ausführen des Assistenten erstellt ein Integrationsmodul, die entsprechende Cmdlet aller Aktivitäten im ursprünglichen Integration Pack enthält.


### <a name="parameters"></a>Parameter

Eigenschaften einer Aktivität in der Integrationspaket werden Parameter des entsprechenden Cmdlets in das Integrationsmodul konvertiert.  Windows PowerShell-Cmdlets haben eine [Allgemeine Parameter](http://technet.microsoft.com/library/hh847884.aspx) für alle Cmdlets verwendet werden können.  Beispielsweise den ausführlichen Parameter ein Cmdlet detaillierte Informationen zu seiner Ausführung ausgegeben wird.  Kein Cmdlet möglicherweise Parameter mit demselben Namen als allgemeine Parameter.  Wenn eine Aktivität eine Eigenschaft mit demselben Namen als allgemeinen Parameter verfügt, fordert der Assistent Sie einen anderen Namen für den Parameter angeben.

### <a name="monitor-activities"></a>Monitor-Aktivitäten

Monitor Runbooks in Orchestrator [Überwachen der Aktivität](http://technet.microsoft.com/library/hh403827.aspx) starten und Ausführen kontinuierlich auf ein bestimmtes Ereignis aufgerufen werden.  Azure Automatisierung unterstützt Runbooks Monitor keine Überwachungsaktivitäten Integration Packs nicht konvertiert werden.  Stattdessen wird ein Platzhalter-Cmdlet in das Integrationsmodul für das Überwachen der Aktivität erstellt.  Dieses Cmdlet hat keine Funktion, jedoch konvertierte Runbook, der verwendet wird, installiert werden können.  Diese Runbooks werden nicht in Azure Automation ausgeführt, aber kann installiert werden, damit Sie es ändern können.

### <a name="integration-packs-that-cannot-be-converted"></a>Integrationspakete konvertiert werden kann

Nicht mit OIT erstellte Integrationspakete können mit Integration Pack Converter konvertiert werden. Es gibt auch einige Integrationspakete Lieferumfang dieses Tool von Microsoft derzeit konvertiert werden kann.  Konvertierte Versionen dieser Integration Packs wurden [zum Download bereitgestellt](#system-center-orchestrator-integration-modules) , damit sie in Azure Automation oder Service Management-Automatisierung installiert werden können.


## <a name="standard-activities-module"></a>Standardaktivitäten Modul

Orchestrator enthält eine Reihe von [standard-Aktivitäten](http://technet.microsoft.com/library/hh403832.aspx) , die nicht in ein Integrationspaket enthalten jedoch von vielen Runbooks verwendet werden.  Die Standardaktivitäten ist eine Integrationsmodul, die eine entsprechende Cmdlet für jede dieser Aktivitäten enthält.  Sie müssen dieses Integrationsmodul in Azure Automation vor dem Importieren alle konvertierten Runbooks, die eine standard-Aktivität verwenden.

Neben der Unterstützung von konvertierten Runbooks, können Cmdlets im Modul Standardaktivitäten von jemand mit Orchestrator zu neuen Runbooks in Azure Automation verwendet werden.  Während die Funktionalität aller der Standardaktivitäten Cmdlets ausgeführt werden kann, können sie unterschiedlich funktionieren.  Die Cmdlets im konvertierten Standardaktivitäten Modul funktionieren genauso wie die entsprechenden Aktivitäten und dieselben Parameter verwenden.  Dadurch können vorhandenen Orchestrator Runbook Autor beim Übergang zur Azure Automatisierung Runbooks.

## <a name="system-center-orchestrator-integration-modules"></a>System Center Orchestrator Integrationsmodule

Microsoft bietet [Integrationspakete](http://technet.microsoft.com/library/hh295851.aspx) für die Erstellung von Runbooks System Center-Komponenten und andere Produkte automatisieren.  Einige dieser Integration Packs aktuell basierend auf OIT jedoch kann nicht derzeit in Integrationsmodule durch bekannte Probleme konvertiert.  [System Center Orchestrator Integrationsmodule](https://www.microsoft.com/download/details.aspx?id=49555) enthält konvertierte Versionen dieser Integration Packs, die in Azure Automatisierung und Service Management-Automatisierung importiert werden kann.  

Durch die RTM-Version dieses Tools werden aktualisierte Versionen der Integration Packs Grundlage mit Integration Pack Converter konvertiert werden kann OIT veröffentlicht.  Anleitung wird auch bei der Konvertierung Runbooks mit Aktivitäten aus Integration Packs nicht basierend auf OIT bereitgestellt.

## <a name="runbook-converter"></a>Runbook Konverter

Runbook Konverter konvertiert Orchestrator Runbooks in [grafisch Runbooks](automation-runbook-types.md#graph-runbooks) in Azure importiert werden kann.  

Runbook Konverter erfolgt als PowerShell-Modul mit einem Cmdlet namens **ConvertFrom-SCORunbook** , der die Konvertierung durchführt.  Wenn Sie das Tool installieren, wird eine Verknüpfung zu einer PowerShell-Sitzung erstellt, die das Cmdlet lädt.   

Der grundlegende Prozess ein Runbook Orchestrator konvertieren und importieren Sie sie in Azure Automation folgt.  Die folgenden Abschnitte enthalten weitere Details zum Werkzeug und Arbeiten mit konvertierten Runbooks.

1. Exportieren Sie ein oder mehrere Runbooks Orchestrator.
2. Integrationsmodule für alle Aktivitäten im Runbook zu erhalten.
3. Orchestrator Runbooks in der exportierten Datei zu konvertieren.
4. Informationen Sie in Protokollen, um die Konvertierung zu überprüfen und alle erforderlichen manuellen Aufgaben.
4. Importieren Sie konvertierte Runbooks in Azure Automation.
5. Erstellen Sie alle erforderlichen Elemente in Azure Automation.
6. Bearbeiten des Runbooks in Azure Automation erforderlichen Aktivitäten ändern.

### <a name="using-runbook-converter"></a>Runbook Konverter verwenden

Die Syntax für **ConvertFrom-SCORunbook** ist wie folgt:

    ConvertFrom-SCORunbook -RunbookPath <string> -Module <string[]> -OutputFolder <string> 

- RunbookPath - Pfad in die Exportdatei mit Runbooks konvertieren.
- Modul - durch Kommas getrennte Liste der Integrationsmodule mit Aktivitäten in der Runbooks.
- Ausgangsordner - Pfad für den Ordner erstellen konvertiert grafisch Runbooks. 


Der Befehl im folgenden Beispiel konvertiert die Runbooks in eine Exportdatei namens **MyRunbooks.ois_export**.  Diese Runbooks verwenden Active Directory und Data Protection Manager Integration Packs.

    ConvertFrom-SCORunbook -RunbookPath "c:\runbooks\MyRunbooks.ois_export" -Module c:\ip\SystemCenter_IntegrationModule_ActiveDirectory.zip,c:\ip\SystemCenter_IntegrationModule_DPM.zip -OutputFolder "c:\runbooks" 


### <a name="log-files"></a>Protokolldateien

Der Konverter Runbook wird die folgenden Protokolldateien in demselben Speicherort wie die konvertierten Runbook erstellen.  Wenn die Dateien bereits vorhanden sind, werden sie mit Daten aus der letzten Konvertierung überschrieben.

| Datei | Inhalt |
|:---|:---|
| Runbook Konverter - Progress.log | Einzelheiten der Konvertierung einschließlich Informationen für jede Aktivität konvertiert und Warnung für jede Aktivität nicht konvertiert. |
| Runbook Konverter - Summary.log  | Zusammenfassung der letzten Konvertierung einschließlich Warnungen und Aufgaben ausführen wie eine Variable für die konvertierte Runbook erforderlich.  |

### <a name="exporting-runbooks-from-orchestrator"></a>Exportieren von Runbooks aus Orchestrator

Runbook Konverter arbeitet mit einer Exportdatei von Orchestrator, die mindestens Runbooks enthält.  Es wird eine entsprechende Azure Automation Runbook für jedes Orchestrator Runbook in der Exportdatei erstellt.  

Um ein Runbook von Orchestrator exportieren Runbook Runbook Designer rechten Maustaste, und wählen Sie **Exportieren**.  Um alle Runbooks in einem Ordner zu exportieren, Namen Sie den des Ordners, und wählen Sie **Exportieren**.


### <a name="runbook-activities"></a>Runbook Aktivitäten

Runbook Konverter konvertiert jede Aktivität in Orchestrator Runbooks mit einer entsprechenden Aktivität in Azure Automation.  Für die Aktivitäten, die konvertiert werden können, wird eine Platzhalter Aktivität im Runbook mit Warnung erstellt.  Nach dem Importieren des konvertierten Runbooks in Azure müssen Sie diese Aktivitäten mit gültigen ersetzen, die die benötigten Funktionen ausführen.

Orchestrator Aktivitäten im [Standardmodul Aktivitäten](#standard-activities-module) werden konvertiert.  Es gibt einige Orchestrator Standardaktivitäten, die jedoch nicht in diesem Modul werden nicht konvertiert.  Beispielsweise seit **Plattformereignis senden** keine Azure Automatisierung entspricht das Event Orchestrator spezifisch ist.

[Monitor-Aktivitäten](https://technet.microsoft.com/library/hh403827.aspx) werden nicht konvertiert, da gibt es keine Entsprechung für diese in Azure Automation.  Die Ausnahme sind Systemmonitor Aktivitäten in [Integration Packs konvertiert](#integration-pack-converter) , die an die Platzhalter-Aktivität konvertiert werden.

Alle Aktivitäten von einem [Integrationspaket konvertiert](#integration-pack-converter) werden **Module** Parameter den Pfad auf das Integrationsmodul bieten konvertiert werden.  System Center Integration Packs können Sie die [System Center Orchestrator Integrationsmodule](#system-center-orchestrator-integration-modules).


### <a name="orchestrator-resources"></a>Orchestrator-Ressourcen

Runbook Konverter konvertiert nur Runbooks, keine anderen Orchestrator Ressourcen wie Leistungsindikatoren, Variablen oder Verbindungen.  Leistungsindikatoren werden in Azure Automation nicht unterstützt.  Variablen und Verbindungen unterstützt, aber Sie müssen sie manuell erstellen.  Die Protokolldateien informieren Sie Runbooks erfordert Ressourcen und geben Sie entsprechende Ressourcen in Azure Automation für konvertierte Runbook ordnungsgemäß erstellt.

Beispielsweise können ein Runbook eine Variable Sie um einen bestimmten Wert in einer Aktivität zu füllen.  Konvertierte Runbook Aktivität konvertieren und Variable Anlage in Azure Automation mit demselben Namen wie die Variable Orchestrator angeben.  Dies wird in der Datei **Runbook Konverter - Summary.log** gekennzeichnet, die nach der Konvertierung erstellt wird.  Sie müssen diese Variable Anlage in Azure Automation manuell erstellen, bevor Sie mit dem Runbook. 

 
### <a name="input-parameters"></a>Eingabeparameter

Runbooks in Orchestrator akzeptieren Eingabeparameter mit den **Daten zu initialisieren** .  Enthält das konvertierte Runbook dieser Aktivität wird ein [Eingabeparameter](automation-graphical-authoring-intro.md#runbook-input-and-output) in Azure Automation Runbook für jeden Parameter in der Aktivität erstellt.  Eine Aktivität [Workflowskript](automation-graphical-authoring-intro.md#activities) ist im konvertierten Runbook erstellt, abgerufen und jeden Parameter zurück.  Alle Aktivitäten im Runbook, die Eingabeparameter verwenden, finden Sie in der Ausgabe aus dieser Aktivität.

Deshalb verwendet diese Strategie ist am besten Funktionen in Orchestrator Runbooks spiegeln.  Aktivitäten in neue grafisch Runbooks sollten direkt um Eingabeparameter ein Runbook Eingabedaten Quelle verweisen.


### <a name="invoke-runbook-activity"></a>Runbook Aktivität aufgerufen

In Orchestrator Runbooks Starten anderer Runbooks mit dem **Runbook aufrufen** . Konvertierte Runbook enthält diese Aktivität und **Warten auf den Abschluss** -Option festgelegt, wird eine Runbook Aktivität im konvertierten Runbook dafür erstellt.  Wenn die **Warten auf den Abschluss** -Option nicht festgelegt ist, wird dann Workflowskript Aktivität erstellt, **Start AzureAutomationRunbook** verwendet Runbooks starten.  Nach dem Importieren des konvertierten Runbooks in Azure müssen Sie diese Aktivität in der Aktivität angegebenen Informationen ändern.




## <a name="related-articles"></a>Verwandte Artikel

- [System Center 2012 - Orchestrator](http://technet.microsoft.com/library/hh237242.aspx)
- [Service Management-Automatisierung](https://technet.microsoft.com/library/dn469260.aspx)
- [Hybrid Runbook Worker](automation-hybrid-runbook-worker.md)
- [Orchestrator Standardaktivitäten](http://technet.microsoft.com/library/hh403832.aspx)
- [Download System Center Orchestrator Migration Toolkit](https://www.microsoft.com/en-us/download/details.aspx?id=47323)
 
