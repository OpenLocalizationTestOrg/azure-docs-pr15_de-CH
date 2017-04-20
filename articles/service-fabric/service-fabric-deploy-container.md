<properties
   pageTitle="Service Fabric und Bereitstellen von Containern | Microsoft Azure"
   description="Service Fabric und die Verwendung von Containern Microservice Anwendung bereitstellen. Dieser Artikel beschreibt die Funktionen Fabric Service für Container enthält und zum Bereitstellen eines Windows-Abbilds Container in einem Cluster"
   services="service-fabric"
   documentationCenter=".net"
   authors="msfussell"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/24/2016"
   ms.author="msfussell"/>

# <a name="preview-deploy-a-windows-container-to-service-fabric"></a>Vorschau: Bereitstellen Sie Windows Container für Fabric Service

> [AZURE.SELECTOR]
- [Bereitstellen von Windows-Container](service-fabric-deploy-container.md)
- [Andockfenster Container bereitstellen](service-fabric-deploy-container-linux.md)

Dieser Artikel führt Sie durch Container Dienste in Windows-Container erstellen. 

>[AZURE.NOTE] Dieses Feature ist in der Vorschau für Linux und für Windows Server 2016 derzeit nicht zur Verfügung. Dies wird danach in Preview für Windows Server 2016 in der nächsten Version von Service Fabric verfügbar und in zukünftigen Versionen unterstützt werden.

Fabric Service bietet verschiedene Container, mit denen Sie beim Erstellen von Microservices sind in Containern bestehen Applikationen. Diese sind Container Dienste bezeichnet. 

Die Funktionen umfassen.

- Container Bereitstellung und Aktivierung
- Ressource-governance
- Repository-Authentifizierung
- Container-Port Host Port-Zuordnung
- Containern, Discovery und Kommunikation
- Konfigurieren und Festlegen von Umgebungsvariablen

Schauen aller Funktionen wiederum beim Container Service in Ihre Anwendung aufgenommen werden.

## <a name="packaging-a-windows-container"></a>Verpacken eines Windows-Containers

Beim Packen Container können Sie entweder ein Visual Studio-Projektvorlage oder [Anwendungspaket manuell erstellen](#manually). Verwendung von Visual Studio werden Paket Anwendungsstruktur und manifest-Dateien durch den Assistenten für neue für Sie erstellt (Dies wird in der nächsten Version kommt).

## <a name="using-visual-studio-to-package-an-existing-container-image"></a>Mithilfe von Visual Studio ein vorhandenes Bild Container Packen

>[AZURE.NOTE] In einer zukünftigen Version von Visual Studio SDK Tools werden Sie einen Container einer Anwendung auf ähnliche Weise hinzufügen ausführbare Gast heute hinzuzufügen. Finden Sie unter [Deploy ausführbare Service Fabric Gast](service-fabric-deploy-existing-app.md) . Zurzeit werden manuelle verpacken möchten, wie unten beschrieben.

<a id="manually"></a>
## <a name="manually-packaging-and-deploying-a-container"></a>Manuelles Verpacken und Bereitstellen von container
Der Prozess des Verpackens manuell Container Service basiert auf die folgenden Schritte aus:

1. Der Container veröffentlicht in Ihrem Repository.
2. Erstellen der Verzeichnisstruktur Paket.
3. Bearbeiten Sie die Manifestdatei Service.
4. Bearbeiten Sie die Anwendungsmanifestdatei.

## <a name="container-image-deployment-and-activation"></a>Container Bereitstellung und Aktivierung.
Im Service Fabric- [Anwendungsmodell](service-fabric-application-model.md)stellt einen Container Anwendungshost, den mehrere Replikate befinden. Bereitstellen Container aktivieren und geben Sie den Namen des Container-Bildes in ein `ContainerHost` Element in das Manifest.

Das Manifest Hinzufügen einer `ContainerHost` für den Eintrag Punkt und die `ImageName` den Namen des Containers Repository und Bild enthalten. Das folgende partielle Manifest zeigt ein Beispiel für Behälter *Myimage:v1* aus einem Repository bezeichnet *Myrepo* bereitstellen

    <CodePackage Name="Code" Version="1.0">
        <EntryPoint>
          <ContainerHost>
            <ImageName>myrepo/myimagename:v1</ImageName>
            <Commands></Commands>
          </ContainerHost>
        </EntryPoint>
    </CodePackage>

Können Sie durch Angabe der optionalen Eingabebefehle Container Bild `Commands` Element durch ein Komma getrennte Gruppe von Befehlen innerhalb des Containers ausgeführt. 

## <a name="resource-governance"></a>Ressource-governance
Ressource ist eine Funktion des Containers und beschränkt die Ressourcen, die der Container auf dem Host verwenden können. Die `ResourceGovernancePolicy`, im Anwendungsmanifest angegeben, können Ressourcengrenzen für ein Paket Service Code deklarieren. Ressourcen können für Grenzwerte;

- Speicher
- MemorySwap
- CpuShares (CPU-Gewichtung)
- MemoryReservationInMB  
- BlkioWeight (BlockIO-Gewichtung). 

>[AZURE.NOTE] Eine zukünftige Version Unterstützung für bestimmte Block IO Hoechstgrenze wie IOPs, Lese-/Schreibzugriff, u.a. BPS möglich.


    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ResourceGovernancePolicy CodePackageRef="FrontendService.Code" CpuShares="500" 
            MemoryInMB="1024" MemorySwapInMB="4084" MemoryReservationInMB="1024" />
        </Policies>
    </ServiceManifestImport>


## <a name="repository-authentication"></a>Repository-Authentifizierung
Einen Container herunterladen müssen Sie Anmeldeinformationen für das Repository Container bereitstellen. Die Anmeldeinformationen im *Anwendungsmanifest* angegeben wird Anmeldeinformationen oder SSH-Schlüssel für den Download von Container-Bild aus dem Abbildrepository angeben.  Das folgende Beispiel zeigt ein Konto namens *"testuser"* und das Kennwort in Klartext. Dies wird **nicht** empfohlen.


    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                <RepositoryCredentials AccountName="TestUser" Password="12345" PasswordEncrypted="false"/>
            </ContainerHostPolicies>
        </Policies>
    </ServiceManifestImport>

Das Kennwort kann und sollte mit einem Zertifikat auf dem Computer bereitgestellt verschlüsselt werden.

Das folgende Beispiel zeigt ein Konto namens *"testuser"* mit dem Kennwort verschlüsselt mit einem Zertifikat *MyCert*aufgerufen. Sie können die `Invoke-ServiceFabricEncryptText` Powershell-Befehl geheimen Geheimtext Kennwort erstellen. Finden Sie diese Artikel [Geheimnisse Service Fabric-Anwendung verwalten](service-fabric-application-secret-management.md) . Der private Schlüssel des Zertifikats zur Entschlüsselung des Kennworts muss auf dem lokalen Computer in einer Out-of-Band-Methode (in Azure über ARM sieht) bereitgestellt werden. Dann wird Fabric Service das Service-Paket auf dem Computer bereitgestellt werden, zusammen mit dem Kontonamen mit dem Container-Repository mithilfe dieser Anmeldeinformationen authentifizieren und den geheimen Schlüssel entschlüsseln kann.


    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                <RepositoryCredentials AccountName="TestUser" Password="[Put encrypted password here using MyCert certificate ]" PasswordEncrypted="true"/>
            </ContainerHostPolicies>
        </Policies>
    </ServiceManifestImport>

## <a name="container-port-to-host-port-mapping"></a>Container-Port Host Port-Zuordnung
Konfigurieren Sie einen Host Port zur Kommunikation mit dem Container durch Angabe einer `PortBinding` im Anwendungsmanifest. Portbindung ordnet den Port, den der Dienst innerhalb des Containers an einen Port auf dem Host überwacht.


    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                <PortBinding ContainerPort="8905"/>
            </ContainerHostPolicies>
        </Policies>
    </ServiceManifestImport>


## <a name="container-to-container-discovery-and-communication"></a>Containern, Discovery und Kommunikation
Mit der `PortBinding` Richtlinie können Sie einen Container Anschluss Zuordnen einer `Endpoint` in das Manifest wie im folgenden Beispiel gezeigt. Der Endpunkt `Endpoint1` einen festen Port angeben, z. B. port 80 oder kein Port, in diesem Fall ein zufälliger Port aus dem Cluster Anwendung Portbereich für Sie ausgewählt.

Gast-Container angeben einer `Endpoint` wie in das Manifest Service Fabric Naming Service automatisch diesen Endpunkt veröffentlichen, sodass andere Dienste im Cluster dieser Container mit auflösen Service REST Abfragen finden kann. 

    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                <PortBinding ContainerPort="8905" EndpointRef="Endpoint1"/>
            </ContainerHostPolicies>
        </Policies>
    </ServiceManifestImport>

Mit dem Naming Service registrieren, möglich leicht Containern, Kommunikation im Code in Ihrem Container mit den [reverse-Proxyserver](service-fabric-reverseproxy.md). Dazu müssen ist reverse-Proxyserver http-Überwachungsport und den Namen der Dienste, die Sie kommunizieren diese Umgebungsvariablen festlegen möchten. Dazu finden Sie im nächsten Abschnitt.  

## <a name="configure-and-set-environment-variables"></a>Konfigurieren und Festlegen von Umgebungsvariablen
Umgebungsvariablen können angegebenen Feind jedes Paket Code im Dienst manifest für beide Dienste in Containern oder als Prozesse-Gast ausführbaren Dateien bereitgestellt werden. Diese Umgebungsvariablenwerte speziell im Anwendungsmanifest überschrieben oder während der Bereitstellung als Anwendungsparameter angegeben werden.

Das folgende Manifest XML-Ausschnitt zeigt ein Beispiel zum Festlegen von Umgebungsvariablen für ein Paket. 

    <ServiceManifest Name="FrontendServicePackage" Version="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <Description>a guest executable service in a container</Description>
        <ServiceTypes>
            <StatelessServiceType ServiceTypeName="StatelessFrontendService"  UseImplicitHost="true"/>
        </ServiceTypes>
        <CodePackage Name="FrontendService.Code" Version="1.0">
            <EntryPoint>
            <ContainerHost>
                <ImageName>myrepo/myimage:v1</ImageName>
                <Commands></Commands>
            </ContainerHost>
            </EntryPoint>
            <EnvironmentVariables>
                <EnvironmentVariable Name="HttpGatewayPort" Value=""/>
                <EnvironmentVariable Name="BackendServiceName" Value=""/>
            </EnvironmentVariables>
        </CodePackage>
    </ServiceManifest>

Diese Variablen können auf manifest überschrieben werden:

    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <EnvironmentOverrides CodePackageRef="FrontendService.Code">
            <EnvironmentVariable Name="BackendServiceName" Value="[BackendSvc]"/>
            <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
        </EnvironmentOverrides>
    </ServiceManifestImport>

Im obigen Beispiel haben wir einen expliziten Wert für angegeben die `HttpGateway` Umgebungsvariable (19000) während der Wert für `BackendServiceName` Parametersatz über die `[BackendSvc]` Anwendungsparameter. Dadurch können Sie den Wert für `BackendServiceName`Wert zum Zeitpunkt der Bereitstellung der Anwendung und einen festen Wert in das Manifest. 

## <a name="complete-examples-for-application-and-service-manifest"></a>Beispiele für die Anwendung und Dienstmanifest abgeschlossen
Nachfolgend ein Beispiel Anwendungsmanifest, das Container-Funktionen anzeigt.


    <ApplicationManifest ApplicationTypeName="SimpleContainerApp" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <Description>A simple service container application</Description>
        <Parameters>
            <Parameter Name="ServiceInstanceCount" DefaultValue="3"></Parameter>
            <Parameter Name="BackEndSvcName" DefaultValue="bkend"></Parameter>
        </Parameters>
        <ServiceManifestImport>
            <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
            <EnvironmentOverrides CodePackageRef="FrontendService.Code">
                <EnvironmentVariable Name="BackendServiceName" Value="[BackendSvcName]"/>
                <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
            </EnvironmentOverrides>
            <Policies>
                <ResourceGovernancePolicy CodePackageRef="Code" CpuShares="500" MemoryInMB="1024" MemorySwapInMB="4084" MemoryReservationInMB="1024" />
                <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                    <RepositoryCredentials AccountName="username" Password="****" PasswordEncrypted="true"/>
                    <PortBinding ContainerPort="8905" EndpointRef="Endpoint1"/>
                </ContainerHostPolicies>
            </Policies>
        </ServiceManifestImport>
    </ApplicationManifest>


Im folgenden werden ein Beispiel Dienstmanifest (angegeben im vorhergehenden Anwendungsmanifest), die Container-Funktionen

    <ServiceManifest Name="FrontendServicePackage" Version="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <Description> A service that implements a stateless frontend in a container</Description>
        <ServiceTypes>
            <StatelessServiceType ServiceTypeName="StatelessFrontendService"  UseImplicitHost="true"/>
        </ServiceTypes>
        <CodePackage Name="FrontendService.Code" Version="1.0">
            <EntryPoint>
            <ContainerHost>
                <ImageName>myrepo/myimage:v1</ImageName>
                <Commands></Commands>
            </ContainerHost>
            </EntryPoint>
            <EnvironmentVariables>
                <EnvironmentVariable Name="HttpGatewayPort" Value=""/>
                <EnvironmentVariable Name="BackendServiceName" Value=""/>
            </EnvironmentVariables>
        </CodePackage>
        <ConfigPackage Name="FrontendService.Config" Version="1.0" />
        <DataPackage Name="FrontendService.Data" Version="1.0" />
        <Resources>
            <Eendpoints>
                <Endpoint Name="Endpoint1" Port="80"  UriScheme="http" />
            </Eendpoints>
        </Resources>
    </ServiceManifest>
