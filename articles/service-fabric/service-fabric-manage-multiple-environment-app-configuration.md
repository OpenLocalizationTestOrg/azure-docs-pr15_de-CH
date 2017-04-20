<properties
   pageTitle="Umgebung mit mehreren Service-Fabric verwalten | Microsoft Azure"
   description="Service Fabric Applikationen können auf Cluster ausgeführt werden, die Größe von einem Computer Tausende von Computern liegen. In einigen Fällen möchten Sie eine Anwendung für verschiedenen Umgebungen unterschiedlich konfigurieren. Dieser Artikel beschreibt unterschiedliche Parameter pro Umgebung definieren."
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="07/19/2016"
   ms.author="seanmck"/>

# <a name="manage-application-parameters-for-multiple-environments"></a>Parameter der Anwendung für mehrere Unternehmen verwalten

Azure Service Fabric-Cluster können mithilfe von überall aus zu Tausenden von Computern. Während Anwendungsbinärdateien ohne Änderung in diesem breiten Spektrum der Umgebung ausgeführt werden können, sollten Sie häufig unterschiedlich je nach Computer konfigurieren Sie die Anwendung bereitstellen möchten.

Ein einfaches Beispiel `InstanceCount` für statusfreie Service. Bei Anwendung in Azure ausführen, sollten Sie im Allgemeinen dieser Parameter auf den speziellen Wert "-1" festgelegt. Dadurch, dass der Dienst auf jedem Knoten im Cluster ausgeführt wird. Allerdings eignet sich diese Konfiguration nicht für einen Cluster mit einem einzelnen Computer seit mehreren Prozessen auf dieselbe IP-Adresse auf einem einzigen Computer haben kann. Stattdessen festlegen `InstanceCount` auf "1".

## <a name="specifying-environment-specific-parameters"></a>Umgebung-Parameter angeben

Die Lösung für dieses Problem ist eine Reihe von parametrisierten Standarddienste und Parameter-Anwendungsdateien, die die Parameterwerte für eine bestimmte Umgebung füllen. Anwendung und Service Manifeste sind Standarddienste und Anwendungsparameter konfiguriert. Die Schemadefinition für die ServiceManifest.xml und ApplicationManifest.xml wird mit Service Fabric-SDK Tools *C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*installiert.

### <a name="default-services"></a>Standard-services

Service Fabric Applikationen bestehen aus einer Auflistung von Instanzen. Es ist möglich, eine leere Anwendung erstellen und dann alle Instanzen dynamisch erstellen, haben die meisten eine Basisdienste immer erstellt werden soll, wenn die Anwendung instanziiert wird. Diese werden als "Standarddienste" bezeichnet. Sie werden im Anwendungsmanifest mit Platzhaltern für pro-Umgebungskonfiguration eingeschlossen in Klammern angegeben:

    <DefaultServices>
        <Service Name="Stateful1">
            <StatefulService
                ServiceTypeName="Stateful1Type"
                TargetReplicaSetSize="[Stateful1_TargetReplicaSetSize]"
                MinReplicaSetSize="[Stateful1_MinReplicaSetSize]">

                <UniformInt64Partition
                    PartitionCount="[Stateful1_PartitionCount]"
                    LowKey="-9223372036854775808"
                    HighKey="9223372036854775807"
                />
        </StatefulService>
    </Service>
  </DefaultServices>

Benannte Parameter muss im Parameter-Element des Anwendungsmanifests definiert werden:

    <Parameters>
        <Parameter Name="Stateful1_MinReplicaSetSize" DefaultValue="2" />
        <Parameter Name="Stateful1_PartitionCount" DefaultValue="1" />
        <Parameter Name="Stateful1_TargetReplicaSetSize" DefaultValue="3" />
    </Parameters>

Das DefaultValue-Attribut gibt den Wert für eine bestimmte Umgebung keine spezifischere Parameter verwendet werden.

>[AZURE.NOTE] Nicht alle Instanzparameter eignen sich für die Konfiguration pro Umgebung. Im obigen Beispiel werden die Werte für den Dienst Partitionierungsschema LowKey und Bildkontrast explizit für alle Instanzen des Dienstes definiert, da Bereich Partition Bereich Daten nicht der Umgebung ist.


### <a name="per-environment-service-configuration-settings"></a>Pro Umgebung Service-Konfigurationen

Das [Anwendungsmodell Service Fabric](service-fabric-application-model.md) können Dienste Konfigurationspakete einzubeziehen, die benutzerdefinierte Schlüssel-Wert-Paare enthalten, die zur Laufzeit gelesen werden. Die Werte dieser Einstellungen können auch differenziert Umgebung durch Angabe einer `ConfigOverride` im Anwendungsmanifest.

Angenommen, die folgende Einstellung in der Datei Config\Settings.xml für die `Stateful1` Service:


    <Section Name="MyConfigSection">
      <Parameter Name="MaxQueueSize" Value="25" />
    </Section>

Zum Überschreiben dieser Wert für ein Arbeitsumfeld Anwendung erstellen Sie eine `ConfigOverride` Wenn das Manifest im Anwendungsmanifest importieren.

    <ConfigOverrides>
     <ConfigOverride Name="Config">
        <Settings>
           <Section Name="MyConfigSection">
              <Parameter Name="MaxQueueSize" Value="[Stateful1_MaxQueueSize]" />
           </Section>
        </Settings>
     </ConfigOverride>
  </ConfigOverrides>

Dieser Parameter kann dann Umgebung konfiguriert werden, wie oben dargestellt. Hierzu können Sie im Parameterabschnitt des Anwendungsmanifests deklarieren und umgebungsspezifischen Werte im Parameter-Anwendungsdateien angeben.

>[AZURE.NOTE] Bei Service-Konfigurationen sind drei Stellen, kann der Wert eines Schlüssels festlegen: Service Konfigurationspaket, das Anwendungsmanifest und anwendungsparameterdatei. Service Fabric wählt immer von der anwendungsparameterdatei erste (falls angegeben), dann das Anwendungsmanifest und schließlich das Konfigurationspaket.


### <a name="application-parameter-files"></a>Anwendungsdateien parameter

Service Fabric Anwendungsprojekt kann ein oder mehrere Parameter Anwendungsdateien enthalten. Jeder von ihnen definiert bestimmte Werte für die Parameter, die im Anwendungsmanifest definiert sind:

    <!-- ApplicationParameters\Local.xml -->

    <Application Name="fabric:/Application1" xmlns="http://schemas.microsoft.com/2011/01/fabric">
        <Parameters>
            <Parameter Name ="Stateful1_MinReplicaSetSize" Value="2" />
            <Parameter Name="Stateful1_PartitionCount" Value="1" />
            <Parameter Name="Stateful1_TargetReplicaSetSize" Value="3" />
        </Parameters>
    </Application>

Eine neue Anwendung enthält zwei Parameter Anwendungsdateien namens Local.xml und Cloud.xml:

![Parameter der Anwendungsdateien im Projektmappen-Explorer][app-parameters-solution-explorer]

Erstellen Sie eine neue Parameterdatei einfach kopieren Sie und fügen Sie eine vorhandene und geben Sie einen neuen Namen.

## <a name="identifying-environment-specific-parameters-during-deployment"></a>Identifizieren der Umgebung Parameter während der Bereitstellung

Zum Zeitpunkt der Bereitstellung müssen Sie die entsprechenden Parameter Datei mit der Anwendung. Sie können über das Dialogfeld Veröffentlichen in Visual Studio oder PowerShell dazu.

### <a name="deploy-from-visual-studio"></a>Bereitstellen von Visual Studio

Sie können aus der Liste der verfügbaren Parameterdateien beim Veröffentlichen der Anwendung in Visual Studio.

![Wählen Sie eine Datei im Dialogfeld "Veröffentlichen"][publishdialog]

### <a name="deploy-from-powershell"></a>Bereitstellen von PowerShell

Die `Deploy-FabricApplication.ps1` PowerShell-Skript die Projektvorlage Anwendung ein Veröffentlichungsprofil als Parameter akzeptiert und das PublishProfile enthält einen Verweis auf die Datei der Anwendung.

  ```PowerShell
    ./Deploy-FabricApplication -ApplicationPackagePath <app_package_path> -PublishProfileFile <publishprofile_path>
  ```

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über die wichtigsten Konzepte, die in diesem Thema beschriebenen [technischen Service Fabric-Übersicht](service-fabric-technical-overview.md)anzeigen Informationen zu anderen app-Management-Funktionen, die in Visual Studio finden Sie unter [Service Fabric-Anwendung in Visual Studio verwalten](service-fabric-manage-application-in-visual-studio.md).

<!-- Image references -->

[publishdialog]: ./media/service-fabric-manage-multiple-environment-app-configuration/publish-dialog-choose-app-config.png
[app-parameters-solution-explorer]:./media/service-fabric-manage-multiple-environment-app-configuration/app-parameters-in-solution-explorer.png
