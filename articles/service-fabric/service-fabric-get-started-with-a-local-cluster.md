<properties
   pageTitle="Erste Schritte mit bereitstellen und Aktualisieren von apps im lokalen Cluster | Microsoft Azure"
   description="Lokale Service Fabric-Cluster richten Sie ein, Bereitstellen Sie eine vorhandene Anwendung und aktualisieren Sie die Anwendung."
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/09/2016"
   ms.author="ryanwi;mikhegn"/>

# <a name="get-started-with-deploying-and-upgrading-applications-on-your-local-cluster"></a>Erste Schritte mit bereitstellen und Aktualisieren einer Anwendung auf dem lokalen cluster
Azure Service Fabric-SDK umfasst eine vollständige lokale Entwicklungsumgebung, mit denen Sie zu schnell bereitstellen und Verwalten von Applikationen auf einem lokalen Cluster. In diesem Artikel einen lokalen Cluster erstellen, eine vorhandene Anwendung bereitstellen und aktualisieren Sie die Anwendung auf eine neue Version von Windows PowerShell.

> [AZURE.NOTE] In diesem Artikel wird vorausgesetzt, dass Sie bereits [Umgebung einrichten](service-fabric-get-started.md).

## <a name="create-a-local-cluster"></a>Erstellen Sie einen lokalen cluster
Service Fabric-Cluster stellt einen Satz von Hardwareressourcen Anwendung bereitstellen können. In der Regel ein Cluster überall von fünf auf Tausende von Computern besteht aus. Service Fabric-SDK enthält jedoch eine Clusterkonfiguration, die auf einem einzigen Computer ausführen können.

Es ist wichtig zu verstehen, dass der lokale Cluster Service Fabric keinem Emulator oder Simulator. Der gleichen Plattformcode, der auf mehreren Computern Clustern ausgeführt. Der einzige Unterschied ist, dass die Plattform-Prozesse ausgeführt werden, die normalerweise über fünf Computer auf einem Computer verteilt sind.

Das SDK enthält zwei Arten lokalen Cluster einrichten: ein Windows PowerShell-Skript und lokalen Cluster Manager System Tray app. In diesem Tutorial verwenden wir das PowerShell-Skript.

> [AZURE.NOTE] Wenn Sie einen lokalen Cluster bereits durch die Bereitstellung einer Anwendung von Visual Studio erstellt haben, können Sie diesen Abschnitt überspringen.


1. Starten Sie ein PowerShell-Fenster als Administrator.

2. Führen Sie das Setupskript Cluster SDK Ordner aus:

    ```powershell
    & "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\ClusterSetup\DevClusterSetup.ps1"
    ```

    Die Clustereinrichtung dauert einen Moment. Nach Abschluss der Installation erhalten Sie eine Ausgabe ähnlich:

    ![Cluster Setup Ausgabe][cluster-setup-success]

    Sie können nun versuchen, eine Anwendung zum Cluster bereitstellen.

## <a name="deploy-an-application"></a>Bereitstellen einer Anwendung
Service Fabric-SDK enthält einen umfangreichen Satz von Frameworks und Entwickler für Applikationen erstellen. Wenn Sie Anwendung in Visual Studio erstellen interessiert sind, finden Sie unter [erstellen Ihre erste Service Fabric-Anwendung in Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md).

In diesem Lernprogramm werden vorhandene Beispiel (WordCount genannt) verwenden, damit wir die Aspekte der Plattform – einschließlich Bereitstellung, Überwachung und Aktualisierung konzentrieren können.


1. Starten Sie ein PowerShell-Fenster als Administrator.

2. Importieren Sie Service Fabric-SDK PowerShell-Modul.

    ```powershell
    Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
    ```

3. Erstellen Sie ein Verzeichnis zum Speichern der Anwendungdes, die Sie herunterladen und bereitstellen, wie C:\ServiceFabric.

    ```powershell
    mkdir c:\ServiceFabric\
    cd c:\ServiceFabric\
    ```

4. [WordCount-Anwendung herunterladen](http://aka.ms/servicefabric-wordcountapp) , an der Position erstellt.  Hinweis: der Microsoft Edge-Browser speichert die Datei mit der Erweiterung *.zip* .  Ändern Sie die Erweiterung in *.sfpkg*.

5. Verbindung mit dem lokalen Cluster:

    ```powershell
    Connect-ServiceFabricCluster localhost:19000
    ```

6. Erstellen Sie eine neue Anwendung mit dem SDK Bereitstellung Befehl einen Namen und einen Pfad für das Anwendungspaket.

    ```powershell  
  Publish-NewServiceFabricApplication -ApplicationPackagePath c:\ServiceFabric\WordCountV1.sfpkg -ApplicationName "fabric:/WordCount"
    ```

    Wenn alles gut geht, sollte folgende Ausgabe angezeigt werden:

    ![Bereitstellen einer Anwendung auf dem lokalen cluster][deploy-app-to-local-cluster]

7. Um die Anwendung in Aktion zu sehen, starten Sie den Browser, und navigieren Sie zu [Http://localhost:8081/wordcount/index.html](http://localhost:8081/wordcount/index.html). Sollte angezeigt werden:

    ![Bereitgestellte Benutzeroberfläche der Anwendung][deployed-app-ui]

    WordCount-Anwendung ist sehr einfach. Sie enthält clientseitige JavaScript-Code zum Generieren von zufälligen fünfstelligen "Wörter" werden dann an die Anwendung über ASP.NET Web API weitergeleitet werden. Statusbehafteter Dienst verfolgt die Anzahl der Wörter gezählt. Sie werden anhand der ersten Zeichen des Worts partitioniert. Den Quellcode finden Sie WordCount-App [Beispiele Einstieg](https://azure.microsoft.com/documentation/samples/service-fabric-dotnet-getting-started/).

    Wir bereitgestellte Anwendung enthält vier Partitionen. Wörter mit A bis G in der ersten Partition gespeichert sind, mit H bis N werden in die zweite Partition, usw.

## <a name="view-application-details-and-status"></a>Bewerbungsdetails anzeigen und status
Da wir die Anwendung bereitgestellt haben, betrachten einige Details app in PowerShell.

1. Alle bereitgestellten Anwendung im Cluster Abfragen:

    ```powershell
    Get-ServiceFabricApplication
    ```

    Vorausgesetzt, dass Sie nur die WordCount-Anwendung bereitgestellt haben, sehen Sie ungefähr:

    ![Alle bereitgestellte Applikationen in PowerShell Abfragen][ps-getsfapp]

2. Wechseln Sie auf die nächste Ebene durch Abfragen der festgelegten Dienstleistungen WordCount-Anwendung.

    ```powershell
    Get-ServiceFabricService -ApplicationName 'fabric:/WordCount'
    ```

    ![Listet Dienste für die Anwendung in PowerShell][ps-getsfsvc]

    Die Anwendung besteht zwei Dienste – Web-front-End und der statusbehaftete Dienst, der Wörter verwaltet.

3. Abschließend sehen Sie sich die Liste der Partitionen für WordCountService:

    ```powershell
    Get-ServiceFabricPartition 'fabric:/WordCount/WordCountService'
    ```

    ![Zeigen Sie die Servicepartitionen in PowerShell][ps-getsfpartitions]

    Die Befehle, die wie alle Service Fabric PowerShell Befehle verwendet, zur Verfügung für alle Cluster, lokalen oder Remotecomputer herstellen können.

    Visuelle Weise Cluster interagieren können Sie den Service Fabric-Explorer Webtool [Http://localhost:19080-Explorer](http://localhost:19080/Explorer) im Browser navigieren.

    ![Bewerbungsdetails in Service Fabric-Explorer anzeigen][sfx-service-overview]

    > [AZURE.NOTE] Weitere Informationen zu Service Fabric-Explorer finden Sie unter [Visualisierung Cluster mit Service Fabric-Explorer](service-fabric-visualizing-your-cluster.md).

## <a name="upgrade-an-application"></a>Aktualisieren einer Anwendung
Service Fabric bietet keine Ausfallzeiten Upgrades durch die Überwachung der Anwendung als im Cluster rollt. Führen wir ein einfaches Upgrade WordCount-Anwendung.

Die neue Version der Anwendung jetzt zählt nur Wörter, die mit einem Vokal beginnen. Die Aktualisierung rollt, sehen wir zwei ändert das Verhalten der Anwendung. Zunächst sollte die Anzahl wächst Rate verlangsamen, da weniger Wörter gezählt werden. Zweitens sollten die erste Partition hat zwei Vokale (A und E) und alle anderen Partitionen nur jeweils die Anzahl irgendwann andere übersteigen.

1. Dort, wo Sie das v1 Paket, [WordCount v2 Paket](http://aka.ms/servicefabric-wordcountappv2) .

2. PowerShell-Fenster zurückzukehren und Befehl das SDK Upgrade auf die neue Version des Clusters registrieren. Dann aktualisieren Fabric: / WordCount-Anwendung.

    ```powershell
    Publish-UpgradedServiceFabricApplication -ApplicationPackagePath C:\ServiceFabric\WordCountV2.sfpkg -ApplicationName "fabric:/WordCount" -UpgradeParameters @{"FailureAction"="Rollback"; "UpgradeReplicaSetCheckTimeout"=1; "Monitored"=$true; "Force"=$true}
    ```

    Finden Sie unter Ausgabe in PowerShell ähnelt der folgenden als Aktualisierung beginnt.

    ![Bei PowerShell aktualisieren][ps-appupgradeprogress]

3. Während die Aktualisierung läuft, kann den Status von Service Fabric-Explorer überwachen einfacher. Starten Sie ein Browser-Fenster und navigieren Sie zum [Http://localhost:19080-Explorer](http://localhost:19080/Explorer). Erweitern Sie **Applikationen** in der Struktur auf der linken Seite, und wählen Sie **WordCount**und **Fabric: / WordCount**. Auf der Registerkarte Essentials sehen Sie den Status der Aktualisierung wie durch den Cluster aktualisieren Domänen verläuft.

    ![Bei Service Fabric-Explorer aktualisieren][sfx-upgradeprogress]

    Wie das Upgrade über jede Domäne erfolgt erfolgen Gesundheitskontrolle um sicherzustellen, dass die Anwendung richtig funktioniert.

4. Wenn Sie früher für die Gruppe von Diensten im Fabric Pfeil: / WordCount-Anwendung, beachten, die die Version des WordCountService geändert aber die Version WordCountWebService nicht:

    ```powershell
    Get-ServiceFabricService -ApplicationName 'fabric:/WordCount'
    ```

    ![Abfrage-Anwendungsdienste nach Aktualisierung][ps-getsfsvc-postupgrade]

    Dies zeigt, wie Service Fabric Upgrades der Anwendung verwaltet. Es betrifft nur die Gruppe von Services (oder Code-Konfiguration Pakete innerhalb dieser Dienste), die geändert wurden, wodurch der Aktualisierung schneller und zuverlässiger.

5. Schließlich zurück an den Browser beobachten Sie das Verhalten der neuen Anwendungsversion. Wie erwartet verläuft langsam die Anzahl und die erste Partition endet mit etwas des Volumes.

    ![Die neue Version der Anwendung im Browser anzeigen][deployed-app-ui-v2]

## <a name="cleaning-up"></a>Bereinigen

Vor abschließen, ist es wichtig zu bedenken, dass der lokale Cluster real. Anwendung weiterhin im Hintergrund ausgeführt, bis Sie sie entfernen.  Je nach Ihrer apps kann erhebliche Ressourcen auf Ihrem Computer ausgeführte app aufnehmen. Sie haben mehrere Optionen zur Anwendung und den Cluster verwalten:

1. Um eine einzelne Anwendung und ihre Daten zu entfernen, führen Sie folgenden Befehl:

    ```powershell
    Unpublish-ServiceFabricApplication -ApplicationName "fabric:/WordCount"
    ```

    Oder löschen Sie die Anwendung im Menü **Aktionen** des Service Fabric-Explorer oder im Kontextmenü der Anwendung Listenansicht im linken Bereich.

    ![Löschen einer Anwendung ist Service Fabric-Explorer][sfe-delete-application]

2. Nach dem Löschen der Anwendungdes aus dem Cluster können Sie Version 1.0.0 und 2.0.0 Anwendungstyp WordCount abmelden. Löschen entfernt Anwendungspakete, einschließlich des Codes und der Konfiguration des Clusters Image Store.

    ```powershell
    Remove-ServiceFabricApplicationType -ApplicationTypeName WordCount -ApplicationTypeVersion 2.0.0
    Remove-ServiceFabricApplicationType -ApplicationTypeName WordCount -ApplicationTypeVersion 1.0.0
    ```

    Oder im Service Fabric-Explorer wählen Sie **Das Aufheben der Bereitstellung** der Anwendung.

3. Klicken Sie zum Cluster Herunterfahren unter Beibehaltung der Anwendungsdaten und Spuren, **Lokalen Cluster beenden** in System Tray app.

4. Klicken Sie Cluster vollständig **Lokalen Cluster entfernen** in System Tray app. Diese Option führt eine andere langsam Bereitstellung beim nächsten drücken Sie F5 in Visual Studio. Entfernen Sie den lokalen Cluster nur, wenn Sie nicht für einige Zeit verwenden möchten oder benötigen Sie Ressourcen

## <a name="1-node-and-5-node-cluster-mode"></a>Knoten 1 und 5 Node Cluster-Modus

Beim Arbeiten mit dem lokalen Cluster Anwendungsentwicklung häufig finden Sie sich schnelle Iterationen programmieren, Debuggen, Ändern von Code, Debuggen usw.. Um diesen Prozess zu optimieren, der lokale Cluster kann in zwei Modi ausgeführt: 1 oder 5 Knoten. Beide Modi Cluster haben ihre Vorteile.
5 Knoten Cluster-Modus können Sie mit einem echten Cluster arbeiten. Sie Failover-Szenarien testen, Instanzen und Replikate Ihrer Dienste arbeiten.
1 Node Cluster-Modus ist die schnelle Bereitstellung und Registrierung des Services können Sie schnell überprüfen Sie Code mit Service Fabric-Laufzeit optimiert.

Modus 1 Clusterknoten sowohl 5 Node Cluster-Modus ist kein Emulator oder Simulator. Der gleichen Plattformcode, der auf mehreren Computern Clustern ausgeführt.

> [AZURE.NOTE] Dieses Feature steht in SDK Version 5.2 und höher.

Um einen Cluster mit Knoten 1 Clustermodus ändern, entweder Service Fabric lokalen Cluster Manager oder PowerShell wie folgt:

1. Starten Sie ein PowerShell-Fenster als Administrator.

2. Führen Sie das Setupskript Cluster SDK Ordner aus:

    ```powershell
    & "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\ClusterSetup\DevClusterSetup.ps1" -CreateOneNodeCluster
    ```

    Die Clustereinrichtung dauert einen Moment. Nach Abschluss der Installation erhalten Sie eine Ausgabe ähnlich:
    
    ![Cluster Setup Ausgabe][cluster-setup-success-1-node]

Wenn Sie den Service Fabric lokalen Cluster-Manager verwenden:

![Switch-Cluster-Modus][switch-cluster-mode]

> [AZURE.WARNING] Clustermodus Ändern des aktuellen Clusters wird entfernt von Ihrem System und ein neuer Cluster erstellt wird. Die Daten, die im Cluster gespeichert sind müssen gelöscht werden Wenn Sie Clustermodus ändern.

## <a name="next-steps"></a>Nächste Schritte
- Jetzt bereitgestellt und aktualisiert einige vorgefertigte Programme können Sie [versuchen, Ihre eigenen in Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md).
- Ein [Azure-Cluster](service-fabric-cluster-creation-via-portal.md) sowie alle Aktionen auf dem lokalen Cluster in diesem Artikel möglich.
- Das Upgrade, das wir in diesem Artikel ausgeführt war. Anzeigen Sie das [Aktualisieren der Dokumentation](service-fabric-application-upgrade.md) erfahren Sie mehr über die Leistungsfähigkeit und Flexibilität von Service Fabric upgrades

<!-- Images -->

[cluster-setup-success]: ./media/service-fabric-get-started-with-a-local-cluster/LocalClusterSetup.png
[extracted-app-package]: ./media/service-fabric-get-started-with-a-local-cluster/ExtractedAppPackage.png
[deploy-app-to-local-cluster]: ./media/service-fabric-get-started-with-a-local-cluster/DeployAppToLocalCluster.png
[deployed-app-ui]: ./media/service-fabric-get-started-with-a-local-cluster/DeployedAppUI-v1.png
[deployed-app-ui-v2]: ./media/service-fabric-get-started-with-a-local-cluster/DeployedAppUI-PostUpgrade.png
[sfx-app-instance]: ./media/service-fabric-get-started-with-a-local-cluster/SfxAppInstance.png
[sfx-two-app-instances-different-partitions]: ./media/service-fabric-get-started-with-a-local-cluster/SfxTwoAppInstances-DifferentPartitionCount.png
[ps-getsfapp]: ./media/service-fabric-get-started-with-a-local-cluster/PS-GetSFApp.png
[ps-getsfsvc]: ./media/service-fabric-get-started-with-a-local-cluster/PS-GetSFSvc.png
[ps-getsfpartitions]: ./media/service-fabric-get-started-with-a-local-cluster/PS-GetSFPartitions.png
[ps-appupgradeprogress]: ./media/service-fabric-get-started-with-a-local-cluster/PS-AppUpgradeProgress.png
[ps-getsfsvc-postupgrade]: ./media/service-fabric-get-started-with-a-local-cluster/PS-GetSFSvc-PostUpgrade.png
[sfx-upgradeprogress]: ./media/service-fabric-get-started-with-a-local-cluster/SfxUpgradeOverview.png
[sfx-service-overview]: ./media/service-fabric-get-started-with-a-local-cluster/sfx-service-overview.png
[sfe-delete-application]: ./media/service-fabric-get-started-with-a-local-cluster/sfe-delete-application.png
[cluster-setup-success-1-node]: ./media/service-fabric-get-started-with-a-local-cluster/cluster-setup-success-1-node.png
[switch-cluster-mode]: ./media/service-fabric-get-started-with-a-local-cluster/switch-cluster-mode.png
