<properties
    pageTitle="Problembehandlung bei Azure-Diagnose"
    description="Problembehandlung bei Azure Diagnostics in Azure Cloud Services, virtuellen Maschinen und "
    services="multiple"
    documentationCenter=".net"
    authors="rboucher"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="robb"/>


# <a name="azure-diagnostics-troubleshooting"></a>Problembehandlung bei Azure-Diagnose
Informationen zur Verwendung von Azure Diagnostics Problembehandlung. Weitere Informationen zu Azure Diagnostics finden Sie in [Azure Diagnostics Overview](azure-diagnostics.md#cloud-services).

## <a name="azure-diagnostics-is-not-starting"></a>Azure-Diagnose nicht gestartet

Diagnose besteht aus zwei Komponenten: Gast Agent Plug-in und den Überwachungsagenten. Überprüfen Sie die Protokolldateien **DiagnosticsPluginLauncher.log** und **DiagnosticsPlugin.log** Informationen über Warum Diagnose nicht gestartet werden.  
  
Eine Rolle Cloud-Dienst befinden sich die Protokolldateien für Gast-Agenten-Plugin: 

``` 
C:\Logs\Plugins\Microsoft.Azure.Diagnostics.PaaSDiagnostics\1.6.3.0\ 
``` 

Ein Azure Virtual Machine befinden sich Protokolldateien für Gast-Agenten-Plugin: 

``` 
C:\Packages\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\1.6.3.0\Logs\ 
``` 
 
Die letzte Zeile der Protokolldateien enthält den Exitcode.  

``` 
DiagnosticsPluginLauncher.exe Information: 0 : [4/16/2016 6:24:15 AM] DiagnosticPlugin exited with code 0 
``` 

Das Plug-In zurückgegeben folgende Exitcodes:

Exit-Code|Beschreibung
---|---
0|Erfolg.
-1|Allgemeiner Fehler.
-2|Fehler beim Laden der Rcf-Datei.<p>Dies ist ein interner Fehler nur geschehen soll, wenn Gast Agent Plugin Launcher manuell, falsch auf dem virtuellen Computer aufgerufen wird.
-3|Diagnose-Konfigurationsdatei kann nicht geladen werden.<p><p>Lösung: Dies ist das Ergebnis einer Konfigurationsdatei Schema-Validierung nicht übergeben. Die Lösung ist mit dem Schema zu einer Konfigurationsdatei entspricht.
-4|Eine andere Instanz von Überwachungsagenten Diagnose wird bereits das lokalen Ressourcenverzeichnis verwendet.<p><p>Lösung: Geben Sie einen anderen Wert für **LocalResourceDirectory**.
-6|Gast-Agent Plugin Launcher versucht Diagnose mit einer ungültigen Befehlszeilenoption gestartet.<p><p>Dies ist ein interner Fehler nur geschehen soll, wenn Gast Agent Plugin Launcher manuell, falsch auf dem virtuellen Computer aufgerufen wird.
-10|Diagnose-Plugin wurde mit einer nicht behandelten Ausnahme beendet.
-11|Gast-Agent konnte den Prozess zu starten Überwachung überwachen den Agent erstellen.<p><p>Lösung: Überprüfen Sie die Systemressourcen vorhanden, um neue Prozesse starten.<p>
-101|Ungültige Argumente beim Aufrufen des Diagnose-Plugins.<p><p>Dies ist ein interner Fehler nur geschehen soll, wenn Gast Agent Plugin Launcher manuell, falsch auf dem virtuellen Computer aufgerufen wird.
-102|Plugin-Prozess kann nicht initialisiert werden.<p><p>Lösung: Überprüfen Sie die Systemressourcen vorhanden, um neue Prozesse starten.
-103|Plugin-Prozess kann nicht initialisiert werden. Insbesondere kann er das Protokollierungsobjekt erstellen.<p><p>Lösung: Überprüfen Sie die Systemressourcen vorhanden, um neue Prozesse starten.
-104|Fehler beim Laden von Gast-Agent bereitgestellten Rcf-Datei.<p><p>Dies ist ein interner Fehler nur geschehen soll, wenn Gast Agent Plugin Launcher manuell, falsch auf dem virtuellen Computer aufgerufen wird.
-105|Diagnose-Plugin kann nicht die Diagnose-Konfigurationsdatei geöffnet werden.<p><p>Dies ist ein interner Fehler nur geschehen soll, wenn das Plug-in Diagnose manuell, falsch auf dem virtuellen Computer aufgerufen wird.
-106|Diagnose-Konfigurationsdatei kann nicht gelesen werden.<p><p>Lösung: Dies ist das Ergebnis einer Konfigurationsdatei Schema-Validierung nicht übergeben. So ist die Lösung zu einer Konfigurationsdatei entspricht dem Schema. Finden Sie die XML, die die diagnoseerweiterung in den Ordner *%SystemDrive%\WindowsAzure\Config* auf dem virtuellen Computer an. Öffnen Sie die entsprechende XML-Datei und suchen Sie für **Microsoft.Azure.Diagnostics**und für das **XmlCfg** -Feld. Die Daten sind base64-codierte so zu [Entschlüsseln](http://www.bing.com/search?q=base64+decoder) müssen Sie die XML-Daten anzeigen, die Diagnose geladen wurde.<p>
-107|Resource Directory übergeben, überwacht der Agent ist ungültig.<p><p>Dies ist ein interner Fehler nur geschehen soll, wenn überwacht der Agent manuell, falsch auf dem virtuellen Computer aufgerufen wird.</p>
-108    |Konnte Konfigurationsdatei Diagnose in monitoring Agent-Konfigurationsdatei konvertiert.<p><p>Dies ist ein interner Fehler nur geschehen soll, wenn das Diagnose-Plugin mit eine ungültige Konfigurationsdatei manuell aufgerufen wird.
-110|Allgemeine Diagnose Konfigurationsfehler.<p><p>Dies ist ein interner Fehler nur geschehen soll, wenn das Diagnose-Plugin mit eine ungültige Konfigurationsdatei manuell aufgerufen wird.
-111|Überwachung starten.<p><p>Lösung: Stellen Sie sicher, dass Systemressourcen verfügbar sind.
-112|Allgemeiner Fehler


## <a name="diagnostics-data-is-not-logged-to-azure-storage"></a>Diagnosedaten werden Azure-Speicher nicht angemeldet
Azure-Diagnose speichert alle Daten in Azure-Speicher.

Die häufigste Ursache für fehlende Daten ist falsch definierte Speicher-Kontoinformationen.

Lösung: Korrigieren Sie Konfigurationsdatei Diagnose und installieren Sie Diagnose.
Wenn weiterhin nach diagnoseerweiterung erneut installieren, müssen Sie möglicherweise zum Debuggen weiter die Überwachung Agent Fehler erkennen. Bevor Daten an das Speicherkonto hochgeladen wird in der LocalResourceDirectory gespeichert.

Cloud-Rolle ist der LocalResourceDirectory:

    C:\Resources\Directory\<CloudServiceDeploymentID>.<RoleName>.DiagnosticStore\WAD<DiagnosticsMajorandMinorVersion>\Tables

Für virtuelle Computer ist der LocalResourceDirectory:

    C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<DiagnosticsVersion>\WAD<DiagnosticsMajorandMinorVersion>\Tables

Es sind keine Dateien im Ordner "LocalResourceDirectory", ist überwacht der Agent konnte nicht gestartet werden. Dies wird normalerweise durch eine ungültige Konfigurationsdatei ein Ereignis verursacht, die die CommandExecution.log gemeldet werden sollen.

Wenn das Monitoring Agent erfolgreich Daten sammeln sehen Sie TSF-Dateien für jedes Ereignis in der Konfigurationsdatei definiert. Monitoring Agent protokolliert die Fehler in der Datei MaEventTable.tsf. Tabel2csv-Anwendung können Sie zum Überprüfen der Inhalt dieser Datei TSF-Datei in eine Datei mit durch Kommas getrennten values(.csv) konvertieren:

Eine Cloud Service-Rolle:

    %SystemDrive%\Packages\Plugins\Microsoft.Azure.Diagnostics.PaaSDiagnostics\<DiagnosticsVersion>\Monitor\x64\table2csv maeventtable.tsf

*%SystemDrive%* Cloud Service Rolle ist in der Regel D:

Auf einem virtuellen Computer:

    C:\Packages\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<DiagnosticsVersion>\Monitor\x64\table2csv maeventtable.tsf

Die oben aufgeführten Befehle generiert die Log-Datei *maeventtable.csv*, öffnen und prüfen Sie auf Fehlermeldungen.    


## <a name="diagnostics-data-tables-not-found"></a>Diagnosedaten Tabellen nicht gefunden
Die Tabellen in Azure Speicher Azure Diagnosedaten heißen folgenden Code:

        if (String.IsNullOrEmpty(eventDestination)) {
            if (e == "DefaultEvents")
                tableName = "WADDefault" + MD5(provider);
            else
                tableName = "WADEvent" + MD5(provider) + eventId;
        }
        else
            tableName = "WAD" + eventDestination;

Hier ist ein Beispiel:

        <EtwEventSourceProviderConfiguration provider=”prov1”>
          <Event id=”1” />
          <Event id=”2” eventDestination=”dest1” />
          <DefaultEvents />
        </EtwEventSourceProviderConfiguration>
        <EtwEventSourceProviderConfiguration provider=”prov2”>
          <DefaultEvents eventDestination=”dest2” />
        </EtwEventSourceProviderConfiguration>

4 Tabellen generiert:

Ereignis|Tabellenname
---|---
Provider = "prov1" &lt;Ereignis-Id = "1" /&gt;|WADEvent + MD5("prov1") + "1"
Provider = "prov1" &lt;Ereignis-Id = "2" EventDestination = "dest1" /&gt;|WADdest1
Provider = "prov1" &lt;DefaultEvents /&gt;|WADDefault+MD5("prov1")
Provider = "prov2" &lt;DefaultEvents EventDestination = "dest2" /&gt;|WADdest2
