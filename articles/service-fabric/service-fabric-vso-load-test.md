<properties
    pageTitle="Prüfung die Anwendung mit Visual Studio Team Services | Microsoft Azure"
    description="Informationen Sie zum Test betonen Azure Service Fabric-Anwendung mit Visual Studio Team Services."
    services="service-fabric"
    documentationCenter="na"
    authors="cawams"
    manager="timlt"
    editor="" />

<tags
    ms.service="multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="multiple"
    ms.date="07/29/2016"
    ms.author="cawa" />

# <a name="load-test-your-application-by-using-visual-studio-team-services"></a>Prüfung die Anwendung mit Visual Studio Team Services

Dieser Artikel veranschaulicht, wie mit Microsoft Visual Studio Load Test stress Test eine Anwendung. Ein Azure Service Fabric statusbehaftete Dienst Back-End und eine statusfreie Service Web-front-End verwendet. Die Anwendung wird hier ist ein Flugzeug Speicherort Simulator. Sie bieten ein Flugzeug ID Abflug und Ziel. Back-End der Anwendung verarbeitet die Anfragen und front-End zeigt auf einer Karte Flugzeug, das den Kriterien entspricht.

Das folgende Diagramm veranschaulicht die Service Fabric-Anwendung, die Sie testen können.

![Diagramm der Anwendung Beispiel Flugzeug Speicherort][0]

## <a name="prerequisites"></a>Erforderliche Komponenten
Bevor Sie beginnen, müssen Sie Folgendes tun:

- Visual Studio Team Services-Konto zu erhalten. Sie erhalten eine kostenlos auf [Visual Studio Team Services](https://www.visualstudio.com).
- Abrufen und Installieren von Visual Studio 2013 oder Visual Studio 2015. In diesem Artikel verwendet Visual Studio 2015 Enterprise Edition sollte, aber Visual Studio 2013 und anderen Editionen entsprechend.
- Bereitstellen Sie die Anwendung in einer Stagingumgebung. Informationen finden Sie unter [Bereitstellen von Clientanwendungen auf einem Remotecluster mit Visual Studio](service-fabric-publish-app-remote-cluster.md) .
- Kennen Sie die Anwendung Verwendungsmuster. Diese Informationen zum Auslastungsmuster simulieren.
- Verstehen Sie das Ziel für die Auslastungstests. Dadurch interpretiert und Analysieren der Auslastungstestergebnisse.

## <a name="create-and-run-the-web-performance-and-load-test-project"></a>Erstellen Sie und führen Sie des Projekts Webleistungstests und Auslastungstests aus

### <a name="create-a-web-performance-and-load-test-project"></a>Erstellen eines Webleistungstests und Auslastungstests

1. Öffnen Sie Visual Studio 2015. Wählen Sie **Datei** > **neu** > **Projekt** auf der Menüleiste im Dialogfeld **Neues Projekt** öffnen.

2. Erweitern Sie den Knoten **Visual C#** , und wählen Sie **Test** > **Webleistungstests und Auslastungstests Projekt**. Benennen Sie dem Projekt, und wählen Sie dann auf **OK** .

    ![Bildschirmabbild des Dialogfelds Neues Projekt][1]

    Ein neues Webleistungstests und Auslastungstests Projekt im Projektmappen-Explorer sollte angezeigt werden.

    ![Screenshot der Projektmappen-Explorer mit dem neuen Projekt][2]

### <a name="record-a-web-performance-test"></a>Aufzeichnen eines Webleistungstests

1. Öffnen Sie das Webtest-Projekt.

2. Wählen Sie das Symbol **Hinzufügen aufzeichnen** Aufzeichnung im Browser starten.

    ![Screenshot der Aufzeichnung hinzufügen-Symbol in einem browser][3]

    ![Screenshot der Schaltfläche Datensatz in einem browser][4]

3. Wechseln Sie zu der Service Fabric-Anwendung. Bedienfeld "Aufnahme" sollte Webanfragen angezeigt werden.

    ![Screenshot der Webanfragen im Bedienfeld "Aufnahme"][5]

4. Führen Sie eine Reihe von Aktionen, die der Benutzer ausführen. Diese Aktionen werden als Muster verwendet, die Auslastung generieren.

5. Wenn Sie fertig sind, wählen Sie **Beenden** auf Aufzeichnung beenden.

    ![Screenshot der Schaltfläche zum Beenden][6]

    Das Webtest-Projekt in Visual Studio sollte eine Reihe von Anfragen erfasst haben. Dynamische Parameter automatisch ersetzt. An diesem Punkt können Sie Anfragen zusätzliche und Abhängigkeit löschen, die nicht von Ihr Testszenario.

6. Speichern Sie das Projekt, und wählen Sie dann den Befehl **Test ausführen** des Webleistungstests lokal und sicherzustellen, dass alles ordnungsgemäß funktioniert.

    ![Screenshot des Befehls testen][7]

### <a name="parameterize-the-web-performance-test"></a>Parametrisieren des Webleistungstests

Sie können den Webleistungstest in einen codierten Webleistungstest konvertieren und bearbeiten den Code parametrisieren. Als Alternative können Sie eine Liste den Webleistungstest binden, damit die Daten der Test durchläuft. Weitere Informationen zum Konvertieren des Webleistungstests in einen codierten Test [generieren und Ausführen eines codierten Webleistungstests](https://msdn.microsoft.com/library/ms182552.aspx) zu. Informationen zum Binden von Daten an einen Webleistungstest finden Sie unter [Hinzufügen einer Datenquelle für einen Webleistungstest testen](https://msdn.microsoft.com/library/ms243142.aspx) .

Beispielsweise konvertieren wir des Webleistungstests einen codierten Test Flugzeug-ID mit einer generierten GUID ersetzen und fügen Sie weitere Anfragen Flüge an verschiedenen Orten.

### <a name="create-a-load-test-project"></a>Erstellen eines Testprojekts laden

Ein Projekt laden besteht aus ein oder mehrere Szenarien, die Webleistungstests und Komponententests sowie zusätzliche angegebenen Load Test Settings beschrieben. Die folgenden Schritte zeigen, wie Load erstellen:

1. Wählen Sie im Kontextmenü des Projekts Webleistungstests und Auslastungstests **Hinzufügen** > **Auslastungstest**. Wählen Sie im **Auslastungstest** -Assistenten **Weiter** , um die Tests zu konfigurieren.

2. Wählen Sie im Abschnitt **Auslastungsmuster** ob Konstante Benutzerlast oder ein Schritt Laden beginnt mit wenigen Benutzern und Benutzer steigt.

    Wenn Sie eine gute Schätzung der Benutzerlast und Leistung des aktuellen Systems anzeigen möchten, wählen Sie **Konstante laden**. Wenn Ihr Ziel ist, festzustellen, ob das System ständig unter verschiedenen durchführt, wählen Sie **Schrittweise Auslastung**.

3. Wählen Sie im Abschnitt **Test Mix** die Schaltfläche **Hinzufügen** , und wählen Sie den Test im Auslastungstest enthalten sein sollen. Spalte **Verteilung** können Sie den Prozentsatz der Summe für jeden Testlauf ausgeführten Tests angeben.

4. Geben Sie im Abschnitt **Run Settings** die Dauer des Auslastungstests.

    >[AZURE.NOTE] Die **Testiterationen** -Option steht nur beim Ausführen eines Auslastungstests lokal mit Visual Studio.

5. Im Bereich **Speicherort** des **Run Settings**Adresse Load Test Anfragen entstehen. Der Assistent kann Ihr Team Services-Konto anmelden aufgefordert. Melden Sie an und dann einen Standort. Wenn Sie fertig sind, wählen Sie **Beenden** .

6. Nach dem Erstellen des Auslastungstests LoadTest-Projekt öffnen, und wählen Sie den aktuellen Testlauf wie **Run Settings** > **Settings1 ausführen [aktiv]**. Zur Einstellung im Fenster **Eigenschaften** wird geöffnet.

7. **Im Abschnitt **Ergebnisse** des Fensters Eigenschaften **Run Settings** soll **Speicher für Details der zeitlichen Steuerung** Einstellung den Standardwert keine.** Ändern Sie diesen Wert auf **Alle einzelnen Details** auf die Ergebnisse informieren. Weitere Informationen zu Visual Studio Team Services Ausführen eines Auslastungstests finden Sie unter [Testen geladen](https://www.visualstudio.com/load-testing.aspx) .

### <a name="run-the-load-test-by-using-visual-studio-team-services"></a>Ausführen des Auslastungstests mithilfe von Visual Studio Team Services

Wählen Sie den Befehl **Auslastungstest ausführen** den Testlauf gestartet.

![Screenshot des Befehls Load Test ausführen][8]

## <a name="view-and-analyze-the-load-test-results"></a>Zeigen Sie an und analysieren Sie der Auslastungstestergebnisse

Die Last testen verläuft, die Leistungsdaten grafisch dargestellt. Ähnlich dem folgenden Diagramm sollte angezeigt werden.

![Screenshot der Leistungsdiagramm für Auslastungstests][9]

1. Wählen Sie den **Bericht** Link im oberen Bereich der Seite. Nach dem Download des Berichts wählen Sie **Bericht anzeigen** .

    Auf der Registerkarte **Grafik** Diagramme für verschiedene Leistungsindikatoren angezeigt. Auf der Registerkarte **Zusammenfassung** werden die Tests. Register " **Tabellen** " zeigt die Gesamtzahl der übergebenen und fehlgeschlagene Load Tests.

2. Wählen Sie die Zahl Links **Test** > **Fehler** und **Fehler** > **Anzahl** Spalten Fehler angezeigt.

    Auf die Registerkarte **Details** zeigt virtuelle Benutzer und Test Szenario für fehlgeschlagene Anfragen. Daten können hilfreich sein, wenn der Auslastungstest mehrere Szenarien enthält.

Weitere Informationen zum Anzeigen von Auslastungstestergebnissen finden Sie unter [Analysieren der Auslastungstestergebnisse in der Diagrammansicht des Auslastungstest-Analyzers](https://www.visualstudio.com/load-testing.aspx) .

## <a name="automate-your-load-test"></a>Automatisieren von Auslastungstests

Visual Studio Team Services Auslastungstest stellt APIs Auslastungstests verwalten und Analysieren von Ergebnissen in einem Team Services-Konto. Weitere Informationen finden Sie in der [Cloud Load Tests Rest-APIs](http://blogs.msdn.com/b/visualstudioalm/archive/2014/11/03/cloud-load-testing-rest-apis-are-here.aspx) .

## <a name="next-steps"></a>Nächste Schritte
- [Überwachung und Diagnose-Services in einer lokalen Entwicklung Einrichtung](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[0]: ./media/service-fabric-vso-load-test/OverviewDiagram.png
[1]: ./media/service-fabric-vso-load-test/NewProjectDialog.png
[2]: ./media/service-fabric-vso-load-test/Project.png
[3]: ./media/service-fabric-vso-load-test/AddRecording.png
[4]: ./media/service-fabric-vso-load-test/AddRecording2.png
[5]: ./media/service-fabric-vso-load-test/ActionSequence.png
[6]: ./media/service-fabric-vso-load-test/StopRecording.png
[7]: ./media/service-fabric-vso-load-test/RunTest.png
[8]: ./media/service-fabric-vso-load-test/RunTest2.png
[9]: ./media/service-fabric-vso-load-test/Graph.png
