<properties
   pageTitle="Die Anwendung in Visual Studio debuggen | Microsoft Azure"
   description="Verbessern Sie die Zuverlässigkeit und Leistung der Dienste entwickeln und Debuggen sie in Visual Studio auf einem Cluster lokale Entwicklung."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="06/21/2016"
   ms.author="vturecek;mikhegn"/>

# <a name="debug-your-service-fabric-application-by-using-visual-studio"></a>Debuggen Sie Service Fabric-Anwendung mithilfe von Visual Studio

## <a name="debug-a-local-service-fabric-application"></a>Debuggen einer lokalen Service Fabric-Anwendung

Sie können Zeit und Geld sparen, bereitstellen und Debuggen der Azure Service Fabric-Anwendung in einem lokalen Computer Entwicklung Cluster. Visual Studio kann in dem lokalen Cluster bereitzustellen und für alle Instanzen der Anwendung den Debugger automatisch verbinden.

1. Starten eines Clusters Entwicklung mithilfe der Schritte in [Service Fabric Umgebung einrichten](service-fabric-get-started.md).

2. Drücken Sie **F5,** oder klicken Sie auf **Debuggen** > **Debuggen starten**.

    ![Debuggen einer Anwendung][startdebugging]

3. Legen Sie Haltepunkte im Code durchgehen der Anwendung durch Klicken auf Befehle im Menü **Debuggen** .

    > [AZURE.NOTE] Visual Studio fügt alle Instanzen der Anwendung. Während Sie schrittweise Code ausführen kann durch mehrere Prozesse gleichzeitig Sitzungen Haltepunkte abzurufen. Deaktivieren Sie Haltepunkte, nachdem sie ein Haltepunkt die Thread-ID abhängig machen oder Ereignisse getroffen werden.

4. **Diagnoseereignisse** Fenster wird automatisch geöffnet, damit Sie Ereignisse in Echtzeit anzeigen.

    ![Ereignisse in Echtzeit anzeigen][diagnosticevents]

5. Sie können auch Fenster **Ereignisse** in Cloud Explorer öffnen.  Unter **Service Fabric**Maustaste auf einen beliebigen Knoten und **Streaming Spuren anzeigen**.

    ![Öffnen Sie das Fenster Ereignisse][viewdiagnosticevents]

    Aktivieren Sie Ihre Spuren zu einem bestimmten Dienst oder Anwendung gefiltert werden soll, einfach streaming Spuren auf den bestimmten Dienst oder Anwendung.

6. Die Ereignisse sehen die automatisch generierte **ServiceEventSource.cs** -Datei und aus dem Anwendungscode aufgerufen.

    ```csharp
    ServiceEventSource.Current.ServiceMessage(this, "My ServiceMessage with a parameter {0}", result.Value.ToString());
    ```

7. Das Fenster **Ereignisse** unterstützt filtern, Anhalten und Ereignisse in Echtzeit überprüfen.  Der Filter ist eine einfache Zeichenfolgensuche Ereignisnachricht einschließlich seines Inhalts.

    ![Filtern, Anhalten und fortsetzen oder Ereignisse in Echtzeit überprüfen][diagnosticeventsactions]

8. Debugdienste wird wie jede andere Anwendung debuggen. Sie werden normalerweise durch Visual Studio einfach Debuggen Haltepunkte. Obwohl zuverlässige Sammlungen über mehrere Knoten replizieren implementieren sie noch IEnumerable. Dies bedeutet, dass Sie die in Visual Studio verwenden können während des Debuggens um zu sehen, was Sie in gespeichert haben. Einfach legen Sie einen Haltepunkt an einer beliebigen Stelle im Code.

    ![Debuggen einer Anwendung][breakpoint]

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->

## <a name="debug-a-remote-service-fabric-application"></a>Debuggen Sie remote Service Fabric-Anwendung

Wenn der Service Fabric-Anwendung in einem Cluster Service Fabric in Azure ausführen, können Sie diese direkt von Visual Studio Remote Debuggen.

> [AZURE.NOTE] Die Funktion erfordert [Service Fabric SDK 2.0](http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015) und [Azure SDK für .NET 2.9](https://azure.microsoft.com/downloads/).    

<!-- -->
> [AZURE.WARNING] Remotedebuggen für Test-/Szenarios, nicht die Auswirkung auf die Programme in produktionsumgebungen verwendet werden soll.

1. Navigieren Sie zum Cluster in **Cloud Explorer**mit der rechten Maustaste und wählen Sie **Debuggen aktivieren**

    ![Remotedebugging aktivieren][enableremotedebugging]

    Dies wird remote debugging Erweiterung der Clusterknoten sowie erforderliche Netzwerkkonfigurationen aktivieren eingeflogen.

2. Maustaste auf Clusterknoten in **Cloud Explorer**und wählen **Debugger anhängen**

    ![Debugger Anfügen][attachdebugger]

3. Wählen Sie im Dialogfeld **an den Prozess anhängen** den Prozess, den Sie debuggen möchten, und klicken Sie auf **Anfügen**

    ![Prozess auswählen][chooseprocess]

    Der Name des Prozesses, anfügen möchten gleich Name der Service Project Assemblynamen.

    Der Debugger wird auf alle Knoten des Prozesses.
    - Bei dem statusfreien Dienst Debuggen gehören alle Instanzen des Dienstes auf allen Knoten die Debug-Sitzung.
    - Debuggen von statusbehaftete Dienst werden primäre Replikat der Partition aktiv und daher vom Debugger gefangen. Bewegt das primäre Replikat während der Debugsitzung, werden die Verarbeitung dieses Replikat Teil der Debugsitzung.
    - Um nur relevante Partitionen oder Instanzen eines bestimmten Dienstes abfangen, können Sie bedingte Haltepunkte nur Partition oder Instanz unterbrochen.

    ![Bedingten Haltepunkt][conditionalbreakpoint]

    > [AZURE.NOTE] Derzeit unterstützen wir nicht Debuggen Service Fabric-Cluster mit mehreren Instanzen des gleichen ausführbaren Dienstnamens.

4. Nachdem Sie das Debuggen der Anwendung abgeschlossen haben, können Sie remote debugging Erweiterung durch Rechtsklicken auf den Cluster **Cloud Explorer** deaktivieren und wählen Sie **Debuggen deaktivieren**

    ![Remote-debugging deaktivieren][disableremotedebugging]

## <a name="streaming-traces-from-a-remote-cluster-node"></a>Spuren von einem Remoteclusterknoten Streaming

Sie können auch zu Stream direkt von einem Remoteclusterknoten zu Visual Studio. Dieses Feature ermöglicht Stream ETW-Ablaufverfolgungsereignisse auf einem Clusterknoten Fabric Service in Visual Studio erstellt.

> [AZURE.NOTE] Die Funktion erfordert [Service Fabric SDK 2.0](http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015) und [Azure SDK für .NET 2.9](https://azure.microsoft.com/downloads/).

<!-- -->
> [AZURE.WARNING]Streaming-Spuren für Test-/Szenarios, nicht die Auswirkung auf die Programme in produktionsumgebungen verwendet werden soll.
> In einem Produktionsszenario darf Sie Weiterleiten von Ereignissen mithilfe von Azure Diagnostics abhängig.

1. Zum Cluster in **Cloud Explorer**navigieren, mit der rechten Maustaste und wählen Sie **Streaming Spuren aktivieren**

    ![Remote streaming Spuren aktivieren][enablestreamingtraces]

    Dies wird-streaming Spuren Erweiterung der Clusterknoten sowie erforderliche Netzwerkkonfigurationen aktivieren starten.

2. Erweitern Sie das Element **-Knoten** in **Cloud Explorer**der rechten Maustaste auf den Knoten stream Spuren und **Streaming Spuren anzeigen**

    ![Remote streaming Spuren anzeigen][viewremotestreamingtraces]

    Wiederholen Sie Schritt 2 für die gewünschten Spuren von Knoten. Jeder Knoten-Stream wird in einem Fenster angezeigt.

    Sie können jetzt die Spuren von Service Fabric und Dienstleistungen ausgegeben. Wenn Sie Ereignisse, um nur eine bestimmte Anwendung filtern möchten, geben Sie einfach der Anwendung des Filters.

    ![Streaming-Spuren anzeigen][viewingstreamingtraces]

4. Wenn Sie streaming Spuren vom Cluster erfolgt, können Sie remote streaming Spuren durch Rechtsklicken auf den Cluster **Cloud Explorer** deaktivieren und **Streaming Spuren deaktivieren** auswählen

    ![Remote streaming deaktivieren][disablestreamingtraces]

## <a name="next-steps"></a>Nächste Schritte

- [Test Service Fabric Service](service-fabric-testability-overview.md).
- [Die Service Fabric-Anwendung in Visual Studio verwalten](service-fabric-manage-application-in-visual-studio.md).

<!--Image references-->
[startdebugging]: ./media/service-fabric-debugging-your-application/startdebugging.png
[diagnosticevents]: ./media/service-fabric-debugging-your-application/diagnosticevents.png
[viewdiagnosticevents]: ./media/service-fabric-debugging-your-application/viewdiagnosticevents.png
[diagnosticeventsactions]: ./media/service-fabric-debugging-your-application/diagnosticeventsactions.png
[breakpoint]: ./media/service-fabric-debugging-your-application/breakpoint.png
[enableremotedebugging]: ./media/service-fabric-debugging-your-application/enableremotedebugging.png
[attachdebugger]: ./media/service-fabric-debugging-your-application/attachdebugger.png
[chooseprocess]: ./media/service-fabric-debugging-your-application/chooseprocess.png
[conditionalbreakpoint]: ./media/service-fabric-debugging-your-application/conditionalbreakpoint.png
[disableremotedebugging]: ./media/service-fabric-debugging-your-application/disableremotedebugging.png
[enablestreamingtraces]: ./media/service-fabric-debugging-your-application/enablestreamingtraces.png
[viewingstreamingtraces]: ./media/service-fabric-debugging-your-application/viewingstreamingtraces.png
[viewremotestreamingtraces]: ./media/service-fabric-debugging-your-application/viewremotestreamingtraces.png
[disablestreamingtraces]: ./media/service-fabric-debugging-your-application/disablestreamingtraces.png
