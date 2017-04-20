<properties 
    pageTitle="Cloud-Dienst in der Serveremulator Profiling | Microsoft Azure" 
    services="cloud-services"
    description="Untersuchen Sie Leistungsprobleme bei der Cloud-Diensten mit Visual Studio profiler" 
    documentationCenter=""
    authors="TomArcher" 
    manager="douge" 
    editor=""
    tags="" 
    />

<tags 
    ms.service="cloud-services" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="07/30/2016" 
    ms.author="tarcher"/>

# <a name="testing-the-performance-of-a-cloud-service-locally-in-the-azure-compute-emulator-using-the-visual-studio-profiler"></a>Lokal Testen der Leistung eines Cloud-Diensts in der Azure-Serveremulator mit Visual Studio Profiler

Testen der Performance von Clouddiensten stehen eine Vielzahl von Tools und Techniken.
Wenn Sie einen Cloud-Dienst in Azure veröffentlichen, können Sie Visual Studio Profilerstellungsdaten zu sammeln und analysieren, als im [Profil eine Azure-Anwendung][1].
Sie können auch Diagnose verschiedener Leistungsindikatoren verfolgen beschriebenen [Leistungsindikatoren verwenden in Azure][2].
Sie sollten auch die Anwendung vor der Bereitstellung in der Cloud in Serveremulator Profil.

Dieser Artikel behandelt die CPU Samplingmethode Profiling, lokal im Emulator ausgeführt werden. CPU-Sampling ist eine Methode, die Profilierung nicht sehr intrusiv. Am festgelegten Samplingintervall wird der Profiler eine Momentaufnahme der Aufrufliste. Die Daten über einen Zeitraum erfasst und in einem Bericht angezeigt. Diese Methode des profiling ist anzugeben, wo eine rechenintensive Anwendung die meiste CPU Arbeit geschieht.  Dies ermöglicht das "Langsamster Pfad" darauf, in Ihrer Anwendung die meiste Zeit verbringt.



## <a name="1-configure-visual-studio-for-profiling"></a>1: Konfigurieren von Visual Studio für Profilerstellungstools

Zunächst werden einige Visual Studio-Konfigurationsoptionen, die beim Profilfräsen hilfreich sein können. Um die Profilerstellungsdaten Berichte können, benötigen Sie Symbole (PDB-Dateien) für die Anwendung und auch Symbole für Bibliotheken. Sie wollen sicherstellen, dass die verfügbaren Symbolserver verweisen. Klicken Sie dazu im Menü **Extras** in Visual Studio **Optionen Sie**und dann **Debuggen** **Symbole**klicken. Stellen Sie sicher, dass Microsoft-Symbolserver unter **Speicherorte für Symboldateien (.pdb)**aufgeführt ist.  Sie können auch http://referencesource.microsoft.com/symbols, die möglicherweise zusätzliche Dateien verweisen.

![Symboloptionen][4]

Bei Bedarf können Sie die Berichte, die der Profiler generiert, indem nur mein Code vereinfachen. Mit nur mein Code aktiviert ist werden Aufruflisten Funktion vereinfacht, sodass ausschließlich interne Bibliotheken und.NET Framework-Aufrufe aus den Berichten ausgeblendet sind. Wählen Sie im Menü **Extras** auf **Optionen**. Knoten Sie **Leistungstools** , und wählen Sie **Allgemein**. Aktivieren Sie das Kontrollkästchen **Aktivieren nur mein**Code für Profiler-Berichte.

![Nur mein Code-Optionen][17]

Diese Schritte können ein vorhandenes Projekt oder ein neues Projekt.  Beim Erstellen eines neuen Projekts, die unten beschriebenen Verfahren versuchen wählen Sie C# **Azure Cloud Service** Project aus und wählen Sie eine **Webrolle** und eine **Worker-Rolle**.

![Azure-Clouddienst Projektrollen][5]

Zum Beispiel Zwecke Code dem Projekt hinzugefügt, das die viel Zeit und einige offensichtliche Leistungsproblem veranschaulicht. Eine Arbeitskraft Rolle Projekt z. B. den folgenden Code hinzufügen:

    public class Concatenator
    {
        public static string Concatenate(int number)
        {
            int count;
            string s = "";
            for (count = 0; count < number; count++)
            {
                s += "\n" + count.ToString();
            }
            return s;
        }
    }

Rufen Sie diesen Code aus der RunAsync-Methode in der Worker-Rolle RoleEntryPoint abgeleiteten Klasse. (Ignorieren Sie die Warnung über die Methode synchron ausgeführt.)

        private async Task RunAsync(CancellationToken cancellationToken)
        {
            // TODO: Replace the following with your own logic.
            while (!cancellationToken.IsCancellationRequested)
            {
                Trace.TraceInformation("Working");
                Concatenator.Concatenate(10000);
            }
        }

Erstellen Sie und führen Sie Ihrem Cloud-Dienst lokal ohne Debuggen (STRG + F5 aus) mit der Konfiguration der Projektmappe auf **Release**festgelegt. Dadurch wird sichergestellt, dass alle Dateien und Ordner für die Anwendung lokal ausgeführt, und stellt sicher, dass alle Emulatoren gestartet werden. Starten Sie Benutzeroberfläche Emulator berechnen aus der Taskleiste zu überprüfen, ob der Worker-Rolle ausgeführt wird.

## <a name="2-attach-to-a-process"></a>2: Anfügen an einen Prozess

Statt das Profil der Anwendung von Visual Studio 2010 IDE ab, müssen Sie den Profiler an einen laufenden Prozess anfügen. 

Anfügen des Profilers an einen Prozess im Menü **Analyse** wählen Sie **Profiler** und **Anfügen/Trennen**.

![Profil anhängen][6]

Eine Worker-Rolle finden Sie in des WaWorkerHost.exe-Prozess.

![WaWorkerHost Prozess][7]

Wenn Projektordner auf einem Netzlaufwerk gespeichert ist, fragt der Profiler einen anderen Speicherort zum Speichern der Profilerstellungsdaten Berichte bereitstellen.

 Sie können eine Webrolle zuordnen, an WaIISHost.exe.
Liegen mehrere Arbeitsprozesse Rolle in Ihrer Anwendung, müssen Sie mit Prozess-ID um zu unterscheiden. Sie können die ProcessID programmgesteuert Prozessobjekt auf Abfragen. Beispielsweise die Run-Methode der RoleEntryPoint abgeleiteten Klasse in einer Rolle dieser Code hinzugefügt wird, können Sie das Protokoll in der Compute-Emulator Benutzeroberfläche wissen, welcher Prozess zum Herstellen betrachten.

    var process = System.Diagnostics.Process.GetCurrentProcess();
    var message = String.Format("Process ID: {0}", process.Id);
    Trace.WriteLine(message, "Information");

Starten Sie zum Anzeigen des Protokolls UI Emulator zu berechnen.

![Serveremulator Benutzeroberfläche starten][8]

Öffnen Sie die Arbeitskraft Rolle Protokoll Konsole UI Emulator berechnen auf der Titelleiste der Konsole. Sie können die Prozess-ID im Protokoll anzeigen.

![Prozess-ID anzeigen][9]

Die Sie angehängt haben, führen Sie die Schritte in der Benutzeroberfläche Ihrer Anwendung (falls erforderlich) um das Szenario zu reproduzieren.

Wenn Sie profiling beenden möchten, wählen Sie den Link **Profiling beenden** .

![Beenden Sie Profiling option][10]

## <a name="3-view-performance-reports"></a>3: Berichte anzeigen

Leistungsbericht für die Anwendung wird angezeigt.

An dieser Stelle der Profiler beendet speichert Daten in einer VSP-Datei und zeigt einen Bericht mit einer Analyse dieser Daten.

![Profilerbericht][11]


String.wstrcpy in den langsamsten Pfad anzuzeigen, klicken Sie auf nur mein Code zum Ändern der Anzeige um Benutzercode nur anzeigen.  Wenn Sie String.Concat, drücken Sie die Schaltfläche Alle Code anzeigen angezeigt.

Verketten Methode und einen großen Teil der Ausführungszeit unter String.Concat sollte angezeigt werden.

![Analyse des Berichts][12]

Wenn Zeichenfolge verketten Code in diesem Artikel hinzugefügt wird, sollte eine Warnung in der Aufgabenliste für diese angezeigt. Eine Warnung kann auch angezeigt werden, gibt laute der Garbagecollection, die die Anzahl der Zeichenfolgen, die erstellt und freigegeben werden.

![Performance-Warnung][14]

## <a name="4-make-changes-and-compare-performance"></a>4: ändern und Leistungsvergleich

Sie können auch die Leistung vor und nach einem Code vergleichen.  Beenden Sie den laufenden Prozess und bearbeiten Sie den Code mit StringBuilder Verkettungsvorgang Zeichenfolge ersetzen:

    public static string Concatenate(int number)
    {
        int count;
        System.Text.StringBuilder builder = new System.Text.StringBuilder("");
        for (count = 0; count < number; count++)
        {
             builder.Append("\n" + count.ToString());
        }
        return builder.ToString();
    }

Führen Sie eine andere Leistung ausführen, und vergleichen Sie die Leistung. Im Explorer Leistung Wenn Testläufe in derselben Sitzung können Sie nur wählen beide Berichte öffnen Sie das Kontextmenü und wählen Sie **Leistungsberichte vergleichen**. Wenn Sie mit einem in einer anderen Sitzung Performance vergleichen möchten, öffnen Sie im Menü **Analyse** und wählen Sie **Leistungsberichte vergleichen**. Geben Sie beide Dateien im Dialogfeld.

![Performance-Berichte Option vergleichen][15]

Markieren Sie Unterschiede zwischen den beiden Berichte.

![Vergleichsbericht][16]

Herzlichen Glückwunsch! Sie haben mit dem Profiler begonnen.

## <a name="troubleshooting"></a>Problembehandlung

- Überprüfen Sie einen Releasebuild profiling und Starten ohne Debuggen.

- Wenn die Option Anfügen/Trennen im Menü Profiler nicht aktiviert ist, führen Sie Leistung.

- Verwenden Sie UI-Emulator berechnen den Status der Anwendung anzeigen. 

- Wenn Sie Probleme beim Starten der Anwendung im Emulator oder Anfügen des Profilers beendet Serveremulator und starten Sie ihn neu. Wenn das Problem nicht, starten Sie neu. Dieses Problem kann auftreten, wenn der Serveremulator und entfernen ausgeführte Installationen verwenden.

- Wenn die Profilerstellungstools Befehle von der Befehlszeile aus, insbesondere die globalen Einstellungen verwenden sicher, dass VSPerfClrEnv/GlobalOff aufgerufen wurde und VsPerfMon.exe beendet wurde.

- Wenn beim Abtasten, die Meldung "PRF0025: Es wurden keine Daten erfasst," Überprüfen Sie, ob der Prozess angefügt, CPU-Aktivität. Programme, die nicht berechnete Arbeit möglicherweise keine Samplingdaten erzeugen.  Es ist auch möglich, dass der Prozess beendet, bevor alle Stichproben durchgeführt wurde. Überprüfen Sie die Run-Methode für eine Rolle, die Sie ein Profil erstellen nicht beendet wird.

## <a name="next-steps"></a>Nächste Schritte

Instrumentation Azure Binärdateien im Emulator wird in Visual Studio Profiler nicht unterstützt, aber wenn Sie Speicherzuordnung testen möchten, Sie können diese Option beim Profilfräsen. Sie können auch profiling Parallelität können Sie bestimmen, ob Threads sperren oder Tier Interaktion profiling, um Zeitverschwendung die hilft sind, Probleme aufspüren Interaktion zwischen Schichten einer Anwendung häufig zwischen der Datenebene und eine Worker-Rolle.  Sie können Datenbankabfragen, die Ihre Anwendung generiert anzeigen und Verwenden von Profilerstellungsdaten, die Nutzung der Datenbank. Informationen zum Tier Interaktion profiling finden Sie im Blogbeitrag [Walkthrough: Tier Interaktion Profiler in Visual Studio Team System 2010 mit][3].



[1]: http://msdn.microsoft.com/library/azure/hh369930.aspx
[2]: http://msdn.microsoft.com/library/azure/hh411542.aspx
[3]: http://blogs.msdn.com/b/habibh/archive/2009/06/30/walkthrough-using-the-tier-interaction-profiler-in-visual-studio-team-system-2010.aspx
[4]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally09.png
[5]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally10.png
[6]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally02.png
[7]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally05.png
[8]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally010.png
[9]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally07.png
[10]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally06.png
[11]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally03.png
[12]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally011.png
[14]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally04.png 
[15]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally013.png
[16]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally012.png
[17]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally08.png
 