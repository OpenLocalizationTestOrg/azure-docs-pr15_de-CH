<properties 
   pageTitle="Testen der Leistung eines Cloud-Diensts | Microsoft Azure"
   description="Testen Sie die Leistung eines Cloud-Diensts mit dem Visual Studio-profiler"
   services="visual-studio-online"
   documentationCenter="n/a"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="visual-studio-online"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />


# <a name="testing-the-performance-of-a-cloud-service"></a>Testen der Leistung eines Cloud-Diensts 

##<a name="overview"></a>Übersicht

Sie können die Leistung eines Cloud-Diensts folgt testen:

- Verwenden Sie Azure-Diagnose, Informationen über Anfragen und Verbindungen und Websitestatistiken überprüfen, die Leistung des Dienstes aus Kundensicht anzeigen. Zunächst mit finden Sie unter [Konfigurieren von Diagnose für Azure Cloud Services und virtuelle Maschinen]( http://go.microsoft.com/fwlink/p/?LinkId=623009).

- Mithilfe von Visual Studio Profiler, eine detaillierte Analyse der rechnerische Aspekte wie der Dienst ausgeführt wird. In diesem Thema beschrieben, können den Profiler Sie messen, wie ein Dienst in Azure ausgeführt wird. Informationen mit der Profiler messen, wie ein Dienst in einem Serveremulator lokal ausgeführt wird finden Sie unter [Testen der Leistung einer Azure Cloud Service lokal in der Compute Emulator mit Visual Studio Profiler](http://go.microsoft.com/fwlink/p/?LinkId=262845).



## <a name="choosing-a-performance-testing-method"></a>Wählen einen Leistungstest-Methode

###<a name="use-azure-diagnostics-to-collect"></a>Verwenden Sie Azure Diagnostics sammeln:###

- Statistik auf Webseiten oder Dienste wie Anfragen und Verbindungen.

- Statistiken für Aufgaben wie oft eine Rolle neu gestartet wird.

- Insgesamt setzen Informationen zur Speicherverwendung wie den Prozentsatz der Zeit, die der Garbage Collector oder der Speicher einer ausgeführten Rolle

###<a name="use-the-visual-studio-profiler-to"></a>Verwenden des Visual Studio-Profilers:###

- Bestimmen Sie, welche Funktionen die dauern.

- Wie viel Zeit jeder Teil eine rechenintensive Anwendung zu messen.

- Vergleichen Sie zwei Versionen eines Dienstes detaillierte Leistungsberichte.

- Analysieren Sie Speicher reservieren ausführlicher als die Ebene der einzelnen Speicherbereiche.

- Probleme bei Multithreadingcode zu analysieren.

Bei Verwendung den Profiler können Daten gesammelt werden, wenn ein Cloud-Dienst lokal oder in Azure ausgeführt wird.

###<a name="collect-profiling-data-locally-to"></a>Lokal Profilerstellungsdaten zu sammeln:###

- Testen der Leistung eines Teils eines Cloud-Diensts, wie die Ausführung von bestimmten Arbeitskraft Rolle, eine realistische simulierte Auslastung nicht.

- Testen der Leistung eines Cloud-Diensts isoliert kontrolliert.

- Testen Sie die Leistung der Cloud-Dienst vor der Bereitstellung in Azure.

- Testen der Leistung eines Cloud-Diensts, ohne vorhandenen Installationen.

- Testen Sie die Leistung des Dienstes ohne Aufpreis in Azure ausgeführt.

###<a name="collect-profiling-data-in-azure-to"></a>Sammeln von Profilerstellungsdaten in Azure:###

- Testen der Leistung eines Cloud-Diensts simulierten oder tatsächlichen Belastung.

- Mithilfe der Instrumentationsmethode sammeln von Profilerstellungsdaten, wie später beschrieben.

- Testen Sie die Leistung des Dienstes in derselben Umgebung wie wenn der Dienst in der Produktion ausgeführt wird.

Simulieren Sie normalerweise zu Test Cloud unter normalen oder Belastung.

## <a name="profiling-a-cloud-service-in-azure"></a>Einen Azure-Clouddienst Profiling

Beim Veröffentlichen der Cloud-Dienst von Visual Studio können profile den Dienst und die Profilerstellungsdaten Einstellung, die Ihnen die Informationen, die Sie möchten. Für jede Instanz einer Rolle wird profiling-Sitzung gestartet. Für Weitere Informationen an den Dienst von Visual Studio finden Sie unter [Veröffentlichen einer Azure-Cloud-Dienst von Visual Studio](https://msdn.microsoft.com/library/azure/ee460772.aspx).

Über Performance profiling in Visual Studio finden Sie unter [Handbuch für Einsteiger, Performance Profiling](https://msdn.microsoft.com/library/azure/ms182372.aspx) und [Analyzing Application Performance by Using Profiling Tools](https://msdn.microsoft.com/library/azure/z9z62c29.aspx).

>[AZURE.NOTE] Sie können IntelliTrace oder profiling Cloud-Dienst veröffentlichen. Sie können nicht beide aktivieren.

###<a name="profiler-collection-methods"></a>Profiler Methoden

Unterschiedliche Methoden können für ein Profil erstellen, Sie anhand von Performance-Problemen:

- **CPU-Sampling** - diese Methode sammelt Anwendungsstatistiken, die für ursprüngliche Analyse von CPU-Auslastungsproblemen nützlich sind. CPU-Sampling ist die empfohlene Vorgehensweise für die meisten Leistung Ermittlungen ab. Gibt es eine geringe Auswirkung auf die Anwendung, die beim Sammeln von Samplingdaten CPU profiling sind.

- **Instrumentation** -diese Methode sammelt Daten, die für fokussierte Analysen und zum Analysieren von Leistungsproblemen Eingabe/Ausgabe nützlich ist. Die Instrumentationsmethode zeichnet jeden Eintrag beenden und Funktionsaufruf Funktionen in einem Modul in einem Profil ausführen. Diese Methode ist hilfreich für ausführliche Zeitsteuerungsdaten zu einem Abschnitt des Codes erfasst und den Einfluss von Eingabe- und Operationen zur Leistung der Anwendung. Diese Methode ist für einen Computer mit einer 32-Bit-Betriebssystem deaktiviert. Diese Option steht nur beim Cloud-Dienst in Azure nicht lokal im Serveremulator ausführen.

- **.NET Memory Allocation** - diese Methode sammelt.NET Framework Daten mithilfe das Sampling-Profilerstellungsmethode. Die gesammelten Daten enthalten die Anzahl und Größe von zugeordneten Objekten

- **Parallelität** - diese Methode sammelt Ressourcenkonfliktdaten und Prozess- und Daten, die mit mehreren Threads und mehreren Prozessen Anwendung analysieren. Die Parallelitätsmethode erfasst Daten für jedes Ereignis blockiert die Ausführung des Codes, wie wenn ein Thread wartet auf eine Anwendungsressource freigegeben werden gesperrt. Diese Methode ist nützlich für das Analysieren von Multithreadanwendungen.

- Sie können auch **Tier Interaktion Profiling**aktivieren bietet weitere Informationen über die Ausführungszeiten der synchronen ADO.NET Funktionen Multi-Tier-Anwendung aufrufen, die mit einer oder mehreren Datenbanken kommunizieren. Sie können Ebeneninteraktionsdaten mit Profilerstellungsmethoden sammeln. Weitere Informationen zum Tier Interaktion profiling anzeigen Sie [Ebeneninteraktions-Ansicht](https://msdn.microsoft.com/library/azure/dd557764.aspx)

## <a name="configuring-profiling-settings"></a>Konfigurieren von Profilerstellungsdaten

Die folgende Abbildung veranschaulicht die Profilerstellungsdaten Einstellungen im Dialogfeld Azure-Anwendung veröffentlichen.

![Konfigurieren Sie Profiling Einstellungen](./media/vs-azure-tools-performance-profiling-cloud-services/IC526984.png)

>[AZURE.NOTE] Aktivieren Sie das Kontrollkästchen **Aktivieren profiling** muss den Profiler installiert auf dem lokalen Computer, mit dem Sie den Clouddienst veröffentlichen. Standardmäßig wird der Profiler zusammen mit Visual Studio installiert.

### <a name="to-configure-profiling-settings"></a>Profil konfigurieren

1. Öffnen Sie im Projektmappen-Explorer im Kontextmenü für Azure-Projekts, und wählen Sie dann **Veröffentlichen**. Ausführliche Informationen zum Cloud-Dienst veröffentlichen finden Sie unter [Veröffentlichen einer Cloud service mit Azure Tools](http://go.microsoft.com/fwlink/p?LinkId=623012).

1. Wählen im Dialogfeld **Azure-Anwendung veröffentlichen** **Erweiterte** Einstellungen.

1. Um die Profiler-Lauf ermöglichen das Kontrollkästchen Sie **Profiler-Lauf ermöglichen** .

1. Um Ihr Profil konfigurieren wählen Sie **Einstellungen** Hyperlink aus Profiling dieses Dialogfeld angezeigt wird.

1. Wählen Sie aus Optionsfeldern **Welche Methode Profilerstellungsmethode möchten Sie verwenden** der Profilerstellungstools müssen Sie.

1. Sammeln Sie die Profilerstellungsdaten Ebeneninteraktionsdaten Kontrollkästchen Sie **Aktivieren Tier Interaktion Profiling** .

1. Wählen Sie die Schaltfläche **OK** , um die Einstellung zu speichern.

    Beim Veröffentlichen dieser Anwendung dienen dazu profiling-Sitzung für jede Rolle erstellen.

## <a name="viewing-profiling-reports"></a>Profiling Berichte anzeigen

Profiling-Sitzung wird für jede Instanz einer Rolle im Cloud-Dienst erstellt. Zum Anzeigen der Profilerstellungsdaten Berichte jeder Sitzung von Visual Studio können Sie Fenster Server-Explorer anzeigen und Azure Compute-Knoten auf eine Instanz einer Rolle wählen. Sie können dann profiling Bericht anzeigen, wie in der folgenden Abbildung dargestellt.

![Anzeigen von Azure Profilerstellungsbericht](./media/vs-azure-tools-performance-profiling-cloud-services/IC748914.png)

### <a name="to-view-profiling-reports"></a>Profilerstellungsdaten Berichte anzeigen

1. Um das Fenster Server-Explorer in Visual Studio anzeigen in der Menüleiste wählen Sie Ansicht, Server-Explorer.

1. Wählen Sie Azure Compute-Knoten aus und dann den Knoten Azure-Bereitstellung Cloud-Dienst, den gewählte Profil, wenn von Visual Studio veröffentlicht.

1. Profilerstellungsdaten Berichte für eine Instanz, wählen die Rolle im Dienst, öffnen Sie das Kontextmenü für eine bestimmte Instanz und **Profiling Bericht anzeigen**wählen.

    Bericht VSP-Datei wird jetzt heruntergeladen Azure und der Status des Downloads im Azure-Aktivitätsprotokoll angezeigt. Wenn der Download abgeschlossen ist, erscheint profiling Bericht in einer Registerkarte im Editor für Visual Studio mit dem Namen <Role name> _<Instance Number>_ <identifier>VSP. Zusammenfassende Daten für den Bericht angezeigt.

1. Um verschiedene Ansichten des Berichts in der Liste Aktuelle Ansicht anzuzeigen, geben Sie die gewünschte Ansicht. Weitere Informationen finden Sie unter [Profiling Tools Report Views](https://msdn.microsoft.com/library/azure/bb385755.aspx).

## <a name="next-steps"></a>Nächste Schritte

[Debuggen von Clouddiensten](https://msdn.microsoft.com/library/azure/ee405479.aspx)

[Veröffentlichen in Azure Cloud-Dienst von Visual Studio](https://msdn.microsoft.com/library/azure/ee460772.aspx)

