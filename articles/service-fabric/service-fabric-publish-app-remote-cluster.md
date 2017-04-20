<properties
    pageTitle="Veröffentlichen eine Anwendung auf einem Remotecluster mit Visual Studio | Microsoft Azure"
    description="So veröffentlichen Sie eine Anwendung auf einem remote Service Fabric-Cluster mithilfe von Visual Studio."
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

# <a name="publish-an-application-to-a-remote-cluster-by-using-visual-studio"></a>Veröffentlichen Sie eine Anwendung auf einem Remotecluster mit Visual Studio

> [AZURE.SELECTOR]
- [PowerShell](service-fabric-deploy-remove-applications.md)
- [Visual Studio](service-fabric-publish-app-remote-cluster.md)

<br/>

Azure Service Fabric-Erweiterung für Visual Studio Weise einfache, wiederholbare und skriptfähige Veröffentlichen einer Anwendung zu einem Cluster Service Fabric.

## <a name="the-artifacts-required-for-publishing"></a>Die Artefakte für die Veröffentlichung erforderlich

### <a name="deploy-fabricapplicationps1"></a>Bereitstellen von FabricApplication.ps1

Dies ist ein Powershellskript, das einen Profilpfad veröffentlichen als Parameter für publishing Service Fabric Applikationen verwendet. Da dieses Teils der Anwendung ist, können Sie ggf. die Anwendung ändern.

### <a name="publish-profiles"></a>Profile veröffentlichen

Ein Ordner im Service Fabric-Anwendungsprojekt namens **PublishProfiles** enthält XML-Dateien, die wichtige Informationen für die Veröffentlichung einer Anwendung wie speichern:

- Service Fabric-Cluster-Verbindungsparameter
- Pfad für eine parameter
- Einstellungen aktualisieren

Standardmäßig wird Ihre Anwendung enthalten zwei Profile veröffentlichen: Local.xml und Cloud.xml. Durch Kopieren und Einfügen eines Standarddateien können Sie weitere Profile hinzufügen.

### <a name="application-parameter-files"></a>Anwendungsdateien parameter

Ein Ordner namens **ApplicationParameters** Service Fabric-Anwendungsprojekt enthält XML-Dateien für benutzerdefinierte Anwendung manifest Parameterwerte. Anwendungsmanifestdateien können parametrisiert werden, damit Sie unterschiedliche Werte für Bereitstellungseigenschaften verwenden können. Informationen zur Parametrisierung der Anwendung finden Sie unter [Umgebung mit mehreren Service-Fabric verwalten](service-fabric-manage-multiple-environment-app-configuration.md).

>[AZURE.NOTE] Akteur-Dienste sollten Sie zuerst bevor Sie die Datei in einem Editor oder über das Dialogfeld Veröffentlichen bearbeiten Projekt erstellen. Dies ist Teil der Manifesten-Dateien während des Buildvorgangs generiert werden.

## <a name="to-publish-an-application-by-using-the-publish-service-fabric-application-dialog-box"></a>Eine Anwendung mit Dialogfeld Service Fabric-Anwendung veröffentlichen veröffentlichen

Die folgenden Schritte demonstrieren das Veröffentlichen Sie eine Anwendung **Veröffentlichen Service Fabric-Anwendung** im Dialogfeld Service Fabric-Tools von Visual Studio bereitgestellt.

1. Wählen Sie im Kontextmenü des Projekts Service Fabric Anwendung **veröffentlichen...** im Dialogfeld **Veröffentlichen Service Fabric-Anwendung** anzeigen.

    ![Die ** Dialogfeld veröffentlichen Service Fabric Anwendung **][0]

    In **Zielprofil** Dropdown-Listenfeld ausgewählte Datei werden alle Einstellungen außer **Manifest Versionen**gespeichert. Sie können ein vorhandenes Profil verwenden oder eine neue erstellen **< Profile verwalten... >** in das **Zielprofil** Dropdown-Listenfeld auswählen. Wenn Sie Veröffentlichungsprofile auswählen, werden dessen Inhalt in die entsprechenden Felder im Dialogfeld angezeigt. Wählen Sie zum Speichern der Änderungen jederzeit Link **Profil speichern** .    

2. Geben Sie im Abschnitt **Verbindungsendpunkt** publishing Endpunkt eine lokale oder remote Service Fabric des Clusters. Zum Hinzufügen oder Ändern des Verbindungsendpunkts auf **Verbindungsendpunkt** Dropdown-Liste. Liste der verfügbaren Service Fabric-Cluster Verbindungsendpunkte, Sie veröffentlichen können, basierend auf der Azure-Abonnements. Hinweis: Wenn Sie sich nicht bereits Visual Studio angemeldet sind, Sie aufgefordert werden möchten.

    Verwenden Sie das Dialogfeld Cluster aus der Menge der verfügbaren Abonnements und Cluster auswählen.

    ![Die ** wählen Sie Service Fabric Cluster ** Dialogfeld][1]

    >[AZURE.NOTE] Wenn Sie mit einem beliebigen Endpunkt (wie eine Partei Cluster) veröffentlichen möchten, finden Sie unter **Veröffentlichen einen beliebigen Cluster Endpunkt** Abschnitt.

    Sobald Sie einen Endpunkt wählen, überprüft Visual Studio die Verbindung zum ausgewählten Service Fabric-Cluster. Wenn Cluster nicht sicher ist, kann Visual Studio sofort damit verbinden. Wenn der Cluster sicher ist, müssen Sie ein Zertifikat auf dem lokalen Computer zunächst installieren. Weitere Informationen finden Sie unter [Verbindung konfigurieren](service-fabric-visualstudio-configure-secure-connections.md) . Wenn Sie fertig sind, wählen Sie **OK** . Der ausgewählte Cluster wird im Dialogfeld **Service Fabric-Anwendung veröffentlichen** .

3. Die **Parameterdatei Anwendung** Dropdown-Listenfeld navigieren Sie zu einer anwendungsparameterdatei. Eine Anwendungsdatei Parameter enthält benutzerdefinierte Werte für Parameter in der Anwendungsmanifestdatei. Um einen Parameter zu ändern oder hinzuzufügen, wählen Sie **Bearbeiten** . Geben Sie ein oder ändern Sie den Wert des Parameters in **das Raster** . Wenn Sie fertig sind, wählen Sie die Schaltfläche **Speichern** .

    ![Die ** Dialogfeld bearbeiten Parameter **][2]

4. Verwendet das **Aktualisieren der Anwendungdes** Kontrollkästchen an, ob diese Aktion veröffentlichen ist eine Aktualisierung. Aktualisierung veröffentlichen Aktionen unterscheiden sich von normalen Aktionen veröffentlichen. Eine Liste der Unterschiede finden Sie unter [Service Fabric-Anwendung aktualisieren](service-fabric-application-upgrade.md) . Konfigurieren Sie Update Standardeinstellungen **Aktualisieren** Einstellungen konfigurieren auswählen Upgrade-Parameter-Editor wird angezeigt. Finden Sie unter [Konfigurieren der Aktualisierung einer Anwendung Fabric Service](service-fabric-visualstudio-configure-upgrade.md) Upgrade-Parameter erfahren.

5. Wählen Sie das **Manifest Versionen...** Schaltfläche, um das Dialogfeld **Bearbeiten Versionen** anzuzeigen. Sie müssen die Anwendung und Service für ein Upgrade zu aktualisieren. Siehe [Anleitung upgrade Service Fabric-Anwendung](service-fabric-application-upgrade-tutorial.md) wie Anwendung und dem Versionen Auswirkung einer Aktualisierung manifest.

    ![Die ** Dialogfeld bearbeiten Versionen **][3]

    Wählen Sie Anwendung und Service-Versionen semantische Versioning 1.0.0 oder numerische Werte in das Format von 1.0.0.0 die Option **Anwendung und Service-Versionen aktualisiert** . Wenn Sie diese Option wählen, den Dienst und Anwendung-Versionsnummern werden automatisch aktualisiert, wenn ein Code Config oder den Paket Version aktualisiert. Die Versionen manuell bearbeiten, deaktivieren Sie das Kontrollkästchen, um diese Funktion zu deaktivieren.

    >[AZURE.NOTE] Erstellen Sie für alle Paket ein Akteur Projekt angezeigt werden zunächst das Projekt, um die Einträge in den Dienst Manifest-Dateien generiert.

6. Sie alle erforderlichen Einstellungen festlegen wählen **Veröffentlichen** , veröffentlichen Sie die Anwendung mit dem ausgewählten Service Fabric-Cluster. Die angegebene Einstellung gelten für den Veröffentlichungsvorgang.

## <a name="publish-to-an-arbitrary-cluster-endpoint-including-party-clusters"></a>Veröffentlichen Sie mit einem beliebigen Cluster-Endpunkt (einschließlich Partei Clustern)

Veröffentlichen auf remote-Cluster eine Azure-Abonnements zugeordnet ist Visual Studio veröffentlichen Erfahrung optimiert. Jedoch kann in beliebigen Endpunkten (z. B. Service Fabric Partei Cluster) veröffentlichen Veröffentlichungsprofil XML direkt bearbeiten. Wie oben beschrieben, zwei veröffentlichen Profile stammen von Standard -**Local.xml** und **Cloud.xml**– aber gerne weitere Profile für verschiedene Unternehmen erstellen. Sie möchten beispielsweise ein Profil für die Veröffentlichung Partei Clustern vielleicht namens **Party.xml**erstellen.

Wenn Sie mit einem nicht gesicherten Cluster verbinden, ist erforderlich Verbindungsendpunkt Cluster wie `partycluster1.eastus.cloudapp.azure.com:19000`. In diesem Fall würde der Verbindungsendpunkt im Veröffentlichungsprofil wie folgt aussehen:

```XML
<ClusterConnectionParameters ConnectionEndpoint="partycluster1.eastus.cloudapp.azure.com:19000" />
```

  Wenn Sie mit einem gesicherten Cluster verbinden, müssen Sie die Details des Clientzertifikats aus dem lokalen Speicher für die Authentifizierung verwendet werden. Weitere Informationen finden Sie in [Verbindung mit einem Service Fabric-Cluster konfigurieren](service-fabric-visualstudio-configure-secure-connections.md).

  Nachdem Sie Ihr Veröffentlichungsprofil eingerichtet haben, können Sie es wie folgt im Dialogfeld veröffentlichen verweisen.

  ![Neues Profil veröffentlichen im Dialogfeld veröffentlichen][4]

  Beachten Sie, dass in diesem Fall das neue Veröffentlichungsprofil zu Anwendungsdateien Parameter Standardmäßig zeigt. Dies ist dieselbe Anwendungskonfiguration auf Umgebung veröffentlichen möchten. Dagegen in Fällen, wo sollen verschiedene Konfigurationen für jede Umgebung, die Sie veröffentlichen möchten, sinnvoll erstellen Sie eine entsprechende anwendungsparameterdatei.

## <a name="next-steps"></a>Nächste Schritte

Automatisieren den Veröffentlichungsprozess in einer Umgebung mit kontinuierlicher Integration finden Sie unter [Service Fabric fortlaufende Integration eingerichtet](service-fabric-set-up-continuous-integration.md).


[0]: ./media/service-fabric-publish-app-remote-cluster/PublishDialog.png
[1]: ./media/service-fabric-publish-app-remote-cluster/SelectCluster.png
[2]: ./media/service-fabric-publish-app-remote-cluster/EditParams.png
[3]: ./media/service-fabric-publish-app-remote-cluster/EditVersions.png
[4]: ./media/service-fabric-publish-app-remote-cluster/publish-to-party-cluster.png
