<properties 
   pageTitle="Veröffentlichte Clouddienst Visual Studio mit IntelliTrace Debuggen | Microsoft Azure"
   description="Veröffentlichte Clouddienst Visual Studio mit IntelliTrace Debuggen"
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

# <a name="debugging-a-published-cloud-service-with-intellitrace-and-visual-studio"></a>Veröffentlichte Clouddienst Visual Studio mit IntelliTrace Debuggen

##<a name="overview"></a>Übersicht

Mit IntelliTrace können Sie ausführliche Debuginformationen für eine Rolleninstanz protokollieren, wenn es in Azure ausgeführt wird. Benötigen Sie die Ursache für ein Problem finden, können Sie die IntelliTrace-Protokolle, Code von Visual Studio durchlaufen in Azure ausgeführt werden. IntelliTrace Datensätze Schlüssel, Ausführung von Code und Daten als Ihre Azure-Anwendung als Cloud-Dienst in Azure ausgeführt wird, können Sie die Wiedergabe der aufgezeichneten Daten aus Visual Studio. Als Alternative können Sie Remotedebuggen direkt einen Cloud-Dienst an, die in Azure ausgeführt wird. Siehe [Debuggen von Cloud-Diensten](http://go.microsoft.com/fwlink/p/?LinkId=623041).

>[AZURE.IMPORTANT] IntelliTrace für Debug-Szenarien vorgesehen und sollte nicht für eine Bereitstellung in der Produktion verwendet werden.

>[AZURE.NOTE] Sie können IntelliTrace Visual Studio Enterprise installiert haben und das Azure-Anwendung auf.NET Framework 4 oder höher. IntelliTrace sammelt Informationen für Ihre Azure-Rollen. Die virtuellen Computer für Rollen führen immer 64-Bit-Betriebssysteme.

## <a name="to-configure-an-azure-application-for-intellitrace"></a>Eine Azure-Anwendung für IntelliTrace konfigurieren

Zum Aktivieren von IntelliTrace für eine Azure-Anwendung müssen Sie erstellen und veröffentlichen Sie die Anwendung aus Visual Studio Azure-Projekt. Bevor Sie in Azure veröffentlichen, müssen Sie IntelliTrace für Ihre Azure-Anwendung konfigurieren. Veröffentlichen Sie die Anwendung ohne IntelliTrace aber dann entscheiden, dass Sie das wollen müssen Sie die Anwendung aus Visual Studio erneut veröffentlichen. Weitere Informationen finden Sie unter [Veröffentlichen einer Cloud-Dienst mithilfe der Azure-Tools](http://go.microsoft.com/fwlink/p/?LinkId=623012).

1. Wenn Sie Ihre Azure-Anwendung bereitstellen möchten, überprüfen Sie, ob Ihr Projekt Build **Debuggen**sind.

1. Öffnen Sie das Kontextmenü für den Azure-Projekt im Projektmappen-Explorer, und wählen Sie **Veröffentlichen**.
 
    Azure-Anwendung veröffentlichen-Assistent wird angezeigt.

1. Zum Sammeln von IntelliTrace-Protokolle für die Anwendung bei der Veröffentlichung in der Cloud das Kontrollkästchen Sie **IntelliTrace aktivieren** .

    >[AZURE.NOTE] Sie können IntelliTrace oder profiling veröffentlichen Ihre Azure-Anwendung. Sie können nicht beide aktivieren.

1. Anpassen die Basiskonfiguration IntelliTrace wählen Sie **Einstellungen** Hyperlink aus

    IntelliTrace-Optionen-Dialogfeld wird angezeigt, wie in der folgenden Abbildung dargestellt. Sie können Ereignisse Protokoll, ob Aufrufinformationen sammeln, meldet, welche Module und Prozesse sammeln und wie viel Speicherplatz die Aufzeichnung zuordnen. Weitere Informationen zu IntelliTrace finden Sie unter [Debuggen mit IntelliTrace](http://go.microsoft.com/fwlink/?LinkId=214468).

    ![VST_IntelliTraceSettings](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/IC519063.png)

Das IntelliTrace-Protokoll ist eine zirkuläre Protokolldatei die maximale Größe in die IntelliTrace-Standardeinstellungen (die Standardgröße ist 250 MB) angegeben. IntelliTrace-Protokolle werden in eine Datei im Dateisystem des virtuellen Computers erfasst. Fordern Sie die Protokolle an, ein Snapshot zu diesem Zeitpunkt übernommen und auf Ihren lokalen Computer heruntergeladen.

Nach Veröffentlichung der Azure-Anwendung in Azure bestimmen Sie IntelliTrace Azure Compute-Knoten im Server-Explorer aktiviert wurde, wie in der folgenden Abbildung dargestellt:

![VST_DeployComputeNode](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/IC744134.png)

## <a name="downloading-intellitrace-logs-for-a-role-instance"></a>Eine Rolleninstanz IntelliTrace Logs Download

Sie können IntelliTrace Logs für eine Instanz der Rolle von **Cloud-Services** -Knoten im **Server-Explorer**. Knoten Sie **Clouddienste** bis der Instanz suchen, die, der Sie interessieren, öffnen Sie das Kontextmenü für diese Instanz, und wählen Sie **IntelliTrace-Protokolle anzeigen**. Die IntelliTrace-Protokolle werden in einer Datei in einem Verzeichnis auf dem lokalen Computer gedownloadet. Jedes Mal, wenn Sie IntelliTrace anfordern anmeldet, ein neuer Snapshot erstellt.

Wenn Protokolle heruntergeladen werden, zeigt Visual Studio den Fortschritt des Vorgangs im Aktivitätsprotokoll Azure an. Wie in der folgenden Abbildung dargestellt, können Sie den Artikel für den Arbeitsgang mehr Details erweitern.

![VST_IntelliTraceDownloadProgress](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/IC745551.png)

Sie können weiterhin in Visual Studio arbeiten, während die IntelliTrace-Protokolle herunterladen. Wenn Protokoll heruntergeladen wurde, wird es automatisch in Visual Studio geöffnet.

>[AZURE.NOTE] Die IntelliTrace-Protokolle können Ausnahmen enthalten, die generiert und anschließend verarbeitet. Internen Code generiert diese Ausnahmen bei normalen Start eine Rolle, damit sie ignoriert werden können.

## <a name="see-also"></a>Siehe auch

[Debuggen von Clouddiensten](https://msdn.microsoft.com/library/ee405479.aspx)

