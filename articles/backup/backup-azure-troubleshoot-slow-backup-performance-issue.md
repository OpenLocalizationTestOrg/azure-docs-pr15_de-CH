<properties
   pageTitle="Problembehebung bei der Sicherung von Dateien und Ordnern in Azure Backup langsam | Microsoft Azure"
   description="Bietet Hinweise zur Problembehandlung, um Ihnen die Ursache von Leistungsproblemen Azure Backup diagnostizieren"
   services="backup"
   documentationCenter=""
   authors="genlin"
   manager="jimpark"
   editor=""/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/13/2016"
    ms.author="genli"/>

# <a name="troubleshoot-slow-backup-of-files-and-folders-in-azure-backup"></a>Problembehebung bei der Sicherung von Dateien und Ordnern in Azure Backup langsam

Dieser Artikel enthält Hinweise zur Problembehandlung, um Ihnen bei der diagnose der Ursache von backup langsam für Dateien und Ordner, wenn Sie Azure Backup verwenden. Wenn Sie Azure Backup-Agent zum Sichern von Dateien verwenden, kann der Sicherungsvorgang länger dauern. Diese Verzögerung kann eine oder mehrere der folgenden Ursachen haben:

-   [Gibt es Performance-Engpässe auf dem Computer gesichert wird.](#cause1)
-   [Einen anderen Prozess oder Antivirensoftware stört Azure Backup-Prozess.](#cause2)
-   [Backup-Agent läuft auf einer Azure Virtual Machine (VM).](#cause3)  
-   [Sie sind eine große Anzahl (Millionen) Dateien sichern.](#cause4)

Vor der Problembehandlung empfiehlt Microsoft herunterladen und Installieren der [neuesten Azure Backup-Agent](http://aka.ms/azurebackup_agent). Wir stellen häufige Updates Backup Agent verschiedene beheben, Funktionen und Leistung.

Wir empfehlen auch lesen Sie [häufig gestellte Fragen zur Azure Backup Service](backup-azure-backup-faq.md) um sicherzustellen, dass nicht die allgemeine Konfiguration Probleme auftreten.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

<a id="cause1"></a>
## <a name="cause-performance-bottlenecks-on-the-computer"></a>Ursache: Performance-Engpässe auf dem computer

Engpässe auf dem Computer gesichert werden können verlangsamen. Beispielsweise kann die Funktion eines Computers zum Lesen oder Schreiben auf Festplatte oder Bandbreite zum Senden von Daten über das Netzwerk zu Engpässen führen.

Windows bietet ein integriertes Tool, das [Systemmonitor](https://technet.microsoft.com/magazine/2008.08.pulse.aspx) (Perfmon) um diese Engpässe erkennen aufgerufen hat.

Hier sind einige Leistungsindikatoren und Bereiche, die Diagnose von Engpässen für optimale Backups hilfreich sein können.

| Zähler  | Status  |
|---|---|
|Logischer Datenträger (Festplatte)--% im Leerlauf   | • 100 % auf 50 % im Leerlauf idle = fehlerfrei</br>• 49 % 20 % im Leerlauf idle = Warnung oder Monitor</br>• 19 % 0 % im Leerlauf idle = kritisch oder von Spezifikationen|
|  Logischer Datenträger (Festplatte)--% durchschnittliche S Lese- oder Schreibvorgang Datenträger |  • 0,001 ms 0,015 ms = fehlerfrei</br>• 0,015 ms 0,025 MS = Warnung oder Monitor</br>• 0.026 ms oder mehr = kritisch oder von Spezifikationen|
|  Logischer Datenträger (Festplatte) – Aktuelle Warteschlangenlänge (für alle Instanzen) | 80 Anfragen mehr als 6 Minuten |
| Arbeitsspeicher - ausgelagerter Pool nicht Bytes|• Weniger als 60 % der Pool verbraucht = fehlerfrei<br>• 61 bis 80 % der Pool verbraucht = Warnung oder Monitor</br>• Mehr als 80 % Pool verbraucht = kritisch oder von Spezifikationen|
| Arbeitsspeicher - ausgelagerter Pool Bytes |• Weniger als 60 % der Pool verbraucht = fehlerfrei</br>• 61 bis 80 % der Pool verbraucht = Warnung oder Monitor</br>• Mehr als 80 % Pool verbraucht = kritisch oder von Spezifikationen|
| Speicher - Verfügbare MB| • 50 % freier Speicherplatz oder mehr = fehlerfrei</br>• 25 % freier Speicherplatz = Monitor</br>• 10 % freien Speicherplatz = Warnung</br>• Weniger als 100 MB und 5 % freien Speicherplatz = kritisch oder von Spezifikationen|
|Prozessor -\%Prozessorzeit (alle Instanzen)|• 60 % verbraucht = fehlerfrei</br>• 61 bis 90 % verbraucht = Monitor oder Vorsicht</br>• 91 bis 100 % verbraucht = kritisch|


> [AZURE.NOTE] Wenn Sie feststellen, dass die Infrastruktur der Täter, empfiehlt sich das Defragmentieren der Festplatten regelmäßig für eine bessere Leistung.

<a id="cause2"></a>
## <a name="cause-another-process-or-antivirus-software-interfering-with-azure-backup"></a>Ursache: Einen anderen Prozess oder Antivirensoftware stören Azure Backup

Wir haben gesehen, mehrere Instanzen, in denen andere Prozesse im System Windows Leistung der Azure Backup Agent negativ beeinflusst haben. Beispielsweise können Sie Azure Backup-Agent und einem anderen Programm verwenden, um Daten sichern oder wenn antivirus-Software ausgeführt wird und eine Sperre auf Dateien gesichert werden, die mehrere Sperren auf Dateien Konflikte verursachen. In diesem Fall Fehlschlagen der Sicherung und der Auftrag kann länger dauern als erwartet.

Empfohlene in diesem Szenario ist, deaktivieren Sie die anderen Sicherungsprogramm darauf, ob backup für Azure Backup Agent geändert wird. In der Regel reicht sicherstellen, dass nicht mehrere Sicherungsaufträge gleichzeitig ausführen verhindern gegenseitig beeinflussen.

Antivirus-Programme wird empfohlen, die folgenden Dateien und Verzeichnisse ausschließen:

- C:\Program Files\Microsoft Azure Recovery Services Agent\bin\cbengine.exe als Prozess
- C:\Program Files\Microsoft Azure Recovery Services Agent\ Ordner
- Kratzer Speicherort (sofern Sie nicht den Standardspeicherort verwenden)

<a id="cause3"></a>
## <a name="cause-backup-agent-running-on-an-azure-virtual-machine"></a>Ursache: Backup Agent auf ein Azure virtuellen Computer

Wenn Sie Backup-Agent auf einem virtuellen Computer ausgeführt, werden langsamer als auf einem physischen Computer ausführen. Dies dürfte IOPS bedingt.  Jedoch können Sie die Leistung optimieren, der Azure-Premium-Speicher gesichert werden Datenlaufwerke wechseln. Wir arbeiten auf dieses Problem und das Update wird in einer zukünftigen Version.

<a id="cause4"></a>
## <a name="cause-backing-up-a-large-number-millions-of-files"></a>Ursache: Eine große Anzahl (Millionen) Dateien sichern

Verschieben einer großen Datenmenge dauert länger als eine kleinere Datenmenge verschieben. In einigen Fällen ist nicht nur die Größe der Daten, sondern auch die Anzahl der Dateien oder Ordner backup-Zeit verwandt. Dies gilt besonders bei Millionen von kleinen Dateien (einige Bytes auf wenige Kilobyte) gesichert werden.

Dieses Verhalten tritt auf, weil während Sie die Daten sichern und verschieben in Azure, Azure gleichzeitig Dateien katalogisiert ist. In einigen seltenen Fällen kann der Vorgang länger dauern.

Die folgenden Indikatoren können Sie verstehen des Engpass und entsprechend zu den nächsten Schritten:

- **Benutzeroberfläche zeigt Fortschritt für die Datenübertragung**. Die Daten werden immer noch übertragen. Die Netzwerkbandbreite oder Daten möglicherweise verzögert verursacht.

- Die **Benutzeroberfläche zeigt Fortschritt für die Datenübertragung**. Am C:\Microsoft Azure Recovery Services Agent\Temp Protokolle öffnen Sie, und suchen Sie den Eintrag FileProvider::EndData in den Protokollen. Dieser Eintrag gibt an, dass die Datenübertragung abgeschlossen und der Vorgang. Kein Abbrechen der Sicherungsaufträge. Stattdessen warten Sie länger für den Vorgang zu beenden. Wenn das Problem weiterhin besteht, wenden Sie sich an [Azure unterstützt](https://portal.azure.com/#create/Microsoft.Support).
