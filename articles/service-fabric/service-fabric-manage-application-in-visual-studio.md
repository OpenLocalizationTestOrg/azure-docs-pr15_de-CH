<properties
   pageTitle="Die Anwendung in Visual Studio verwalten | Microsoft Azure"
   description="Verwenden Sie Visual Studio erstellen, entwickeln, packen, bereitstellen und Debuggen Ihrer Service Fabric Programme und Dienste."
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/09/2016"
   ms.author="seanmck;mikhegn"/>

# <a name="use-visual-studio-to-simplify-writing-and-managing-your-service-fabric-applications"></a>Mit Visual Studio können Sie vereinfachen, schreiben und Verwalten der Service Fabric-Anwendung

Sie können Ihre Azure Service Fabric Programme und Dienste über Visual Studio verwalten. Nachdem Sie die [Umgebung eingerichtet](service-fabric-get-started.md)haben, können Sie Visual Studio Service Fabric erstellen, Register Dienste oder Paket hinzufügen und Applikationen im Cluster lokale Entwicklung bereitstellen.

## <a name="deploy-your-service-fabric-application"></a>Bereitstellen der Anwendung Fabric Service

Durch Kombination Bereitstellung einer Anwendung die folgenden Schritte zu einer einfachen Operation:

1. Anwendungspaket erstellen
2. Das Anwendungspaket hochladen Speicher Bild
3. Registrieren den Anwendungstyp
4. Entfernen alle Anwendungsinstanzen ausgeführt
5. Erstellen einer neuen Anwendungsinstanz

In Visual Studio **F5** drücken auch die Anwendung bereitstellen und Debuggen Sie alle Instanzen. Können Sie **STRG + F5,** um eine Anwendung bereitzustellen, ohne Debuggen oder zu einem lokalen oder remote-Cluster mit Veröffentlichungsprofil veröffentlichen. Weitere Informationen finden Sie unter [Veröffentlichen einer Anwendung auf einem Remotecluster mit Visual Studio](service-fabric-publish-app-remote-cluster.md).

### <a name="application-debug-mode"></a>Anwendung Debug-Modus

Visual Studio entfernt standardmäßig vorhandene Instanzen der Anwendung beim Beenden Debuggen oder (Wenn Sie die Anwendung ohne den Debugger bereitgestellt), wenn Sie die Anwendung erneut bereitstellen. In diesem Fall werden alle für die Anwendung Daten entfernt. Beim lokalen Debuggen Daten, die Sie bereits erstellt haben, eine neue Version der Anwendung testen möchten, die Ausführung der Anwendung beibehalten werden soll oder Sie nachfolgende Debugsitzungen zum upgrade der Anwendung. Visual Studio Service Fabric-Tools stellen eine Eigenschaft **Debugmodus Anwendung**, welche Steuerelemente ob **F5** die Anwendung deinstallieren, halten die Anwendung nach eine Debugsitzung beendet oder die Anwendung auf nachfolgende Debugsitzungen nicht entfernt und erneut bereitgestellt.

#### <a name="to-set-the-application-debug-mode-property"></a>Die Eigenschaft Debugmodus Anwendung festlegen

1. Das Anwendungsprojekt Kontextmenü wählen Sie **Eigenschaften aus** (oder drücken Sie **F4** ).
2. Legen Sie im **Eigenschaftenfenster** die Eigenschaft **Debugmodus Anwendung** .

    ![Festlegen von Anwendungseigenschaften Debug-Modus][debugmodeproperty]

Dies sind die **Anwendung Debugmodus** Optionen verfügbar.

1. **Automatisch aktualisieren**: die Anwendung weiterhin ausgeführt werden, wenn die Debugsitzung endet. Die nächste **F5** die Bereitstellung als Aktualisierung behandelt mit beaufsichtigten Automodus schnell aktualisieren auf eine neuere Version die Anwendung mit Datumszeichenfolge angefügt. Der Aktualisierungsprozess behält alle Daten, die in einer vorherigen Debugsitzung eingegeben.

2. **Anwendung beibehalten**: die Anwendung läuft im Cluster, wenn die Debugsitzung endet. Auf den nächsten **F5** die Anwendung entfernt, und die neu erstellte Anwendung auf dem Cluster bereitgestellt.

3. **Entfernen der Anwendung** wird die Anwendung entfernt, wenn die Debugsitzung endet.

Für **Auto Upgrade** Daten durch Anwenden der Aktualisierung Anwendungsmöglichkeiten Service Fabric erhalten, aber anstatt Sicherheit Leistung optimieren abgestimmt. Weitere Informationen zum Aktualisieren von Applikationen und wie Sie eine Aktualisierung in einer realen Umgebung ausführen können finden Sie unter [Service Fabric-Anwendung aktualisieren](service-fabric-application-upgrade.md).

![Beispiel für neue Anwendungsversion mit Datum][preservedata]

>[AZURE.NOTE] Diese Eigenschaft ist vor Version 1.1 Service Fabric-Tools für Visual Studio vorhanden. Verwenden Sie vor 1.1 die Eigenschaft **Beibehalten Daten auf Start** um dasselbe Verhalten zu erreichen. Die Option "Anwendung beibehalten" wurde in Version 1.2 der Service Fabric-Tools für Visual Studio eingeführt.

## <a name="add-a-service-to-your-service-fabric-application"></a>Der Service Fabric-Anwendung einen Dienst hinzufügen

Sie können neue Services erweitern die Funktionalität der Anwendung hinzufügen.  Um sicherzustellen, dass der Dienst in das Anwendungspaket enthalten ist, fügen Sie den Dienst über das Menüelement **Fabric... Service hinzu** .

![Fügen Sie einen neuen Fabric Service zur Anwendung hinzu][newservice]

Wählen Sie Service Fabric Projekttyp Hinzufügen der Anwendung aus, und geben Sie einen Namen für den Dienst.  Finden Sie unter [Auswählen eines Frameworks für den Dienst](service-fabric-choose-framework.md) entscheiden, welcher Diensttyp verwendet.

![Wählen Sie Hinzufügen der Anwendung Fabric Service Projekttyp aus][addserviceproject]

Der neue Dienst wird Projektmappen und vorhandenen Anwendungspaket hinzugefügt. Die Verweise und eine Dienstinstanz standardmäßig das Anwendungsmanifest hinzugefügt. Der Dienst erstellt und der Anwendung bereitstellen wieder gestartet.

![Der neue Dienst wird das Anwendungsmanifest hinzugefügt][newserviceapplicationmanifest]

## <a name="package-your-service-fabric-application"></a>Packen der Anwendung Fabric Service

Zum Bereitstellen der Anwendung und die Dienste zu einem Cluster müssen Sie ein Anwendungspaket erstellen.  Das Paket organisiert das Anwendungsmanifest/die Anwendungsmanifeste Service und anderen erforderlichen Dateien in einem bestimmten Layout.  Visual Studio eingerichtet und verwaltet das Paket in das Anwendungsprojekt Ordner im Verzeichnis "Pkg".  **Paket** klicken, im Kontextmenü **Anwendung** erstellt oder aktualisiert das Anwendungspaket.  Sie sollten, wenn Sie die Anwendung mithilfe von PowerShell Scripts bereitstellen.

## <a name="remove-applications-and-application-types-using-cloud-explorer"></a>Entfernen Sie und Anwendung mit Cloud Explorer

Sie können grundlegende Cluster Management Operationen von Visual Studio mit Cloud-Explorer aus dem Menü **Ansicht starten** ausführen. Beispielsweise können Sie Applikationen löschen und aufheben Anwendungstypen auf lokalen oder remote-Cluster.

![Entfernen einer Anwendung](./media/service-fabric-manage-application-in-visual-studio/removeapplication.png)

>[AZURE.TIP] Cluster-Management den Funktionsumfang finden Sie unter [Cluster mit Service Fabric-Explorer visualisieren](service-fabric-visualizing-your-cluster.md).


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Nächste Schritte

- [Service Fabric-Anwendungsmodell](service-fabric-application-model.md)
- [Bereitstellung von Service Fabric-Anwendung](service-fabric-deploy-remove-applications.md)
- [Anwendung Parameter für mehrere Unternehmen](service-fabric-manage-multiple-environment-app-configuration.md)
- [Debuggen der Anwendung Fabric Service](service-fabric-debugging-your-application.md)
- [Visualisierung des Clusters mit Service Fabric-Explorer](service-fabric-visualizing-your-cluster.md)

<!--Image references-->
[addserviceproject]:./media/service-fabric-manage-application-in-visual-studio/addserviceproject.png
[manageservicefabric]: ./media/service-fabric-manage-application-in-visual-studio/manageservicefabric.png
[newservice]:./media/service-fabric-manage-application-in-visual-studio/newservice.png
[newserviceapplicationmanifest]:./media/service-fabric-manage-application-in-visual-studio/newserviceapplicationmanifest.png
[preservedata]:./media/service-fabric-manage-application-in-visual-studio/preservedata.png
[debugmodeproperty]:./media/service-fabric-manage-application-in-visual-studio/debugmodeproperty.png
