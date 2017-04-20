<properties
    pageTitle="Was einen Cloud-Dienst Paket ist | Microsoft Azure"
    description="Beschreibt die Cloud Service-Modell (.csdef, .cscfg) und in Azure-Paket (.cspkg)"
    services="cloud-services"
    documentationCenter=""
    authors="Thraka"
    manager="timlt"
    editor=""/>
<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="adegeo"/>

# <a name="what-is-the-cloud-service-model-and-how-do-i-package-it"></a>Was ist Cloud Service-Modell, und wie Verpacken ich Sie?
Cloud-Dienst wird aus drei Komponenten, die Service-Definition _(.csdef)_Dienst Config _(.cscfg)_und ein Service-Paket _(.cspkg)_erstellt. Die **ServiceDefinition.csdef** und **ServiceConfig.cscfg** basieren auf XML und beschreiben die Struktur des Cloud-Dienst und Konfiguration; das Modell bezeichnet als. **ServicePackage.cspkg** ist eine Zip-Datei, die von **ServiceDefinition.csdef** und generiert wird, enthält alle erforderlichen binär-basiertes Abhängigkeit. Azure erstellt Cloud Service von **ServicePackage.cspkg** und **ServiceConfig.cscfg**.

In Azure Cloud-Dienst läuft, Sie können die Datei **ServiceConfig.cscfg** konfigurieren, aber nicht die Definition ändern.

## <a name="what-would-you-like-to-know-more-about"></a>Was möchten Sie mehr erfahren?

* Ich möchte mehr über die Dateien [ServiceDefinition.csdef](#csdef) und [ServiceConfig.cscfg](#cscfg) .
* Ich weiß bereits, mir [einige Beispiele](#next-steps) was ich konfigurieren können.
* [ServicePackage.cspkg](#cspkg)erstellt werden soll.
* Ich verwende Visual Studio und möchte...
    * [Neue Clouddienst erstellen][vs_create]
    * [Konfigurieren Sie einen vorhandenen Cloud-Dienst][vs_reconfigure]
    * [Bereitstellen eines Cloud-Dienst][vs_deploy]
    * [Remote Desktop in einer Cloud-Dienstinstanz][remotedesktop]

<a name="csdef"></a>
## <a name="servicedefinitioncsdef"></a>ServiceDefinition.csdef
Die Datei **ServiceDefinition.csdef** gibt die Einstellungen von Azure verwendet werden, um einen Clouddienst zu konfigurieren. [Azure Service Definition Schema (.csdef Datei)](https://msdn.microsoft.com/library/azure/ee758711.aspx) stellt das zulässige Format für eine Service-Definitionsdatei. Das folgende Beispiel zeigt die Einstellung für die Web- und Workerrollen Rollen definiert werden können:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceDefinition name="MyServiceName" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <WebRole name="WebRole1" vmsize="Medium">
    <Sites>
      <Site name="Web">
        <Bindings>
          <Binding name="HttpIn" endpointName="HttpIn" />
        </Bindings>
      </Site>
    </Sites>
    <Endpoints>
      <InputEndpoint name="HttpIn" protocol="http" port="80" />
      <InternalEndpoint name="InternalHttpIn" protocol="http" />
    </Endpoints>
    <Certificates>
      <Certificate name="Certificate1" storeLocation="LocalMachine" storeName="My" />
    </Certificates>
    <Imports>
      <Import moduleName="Connect" />
      <Import moduleName="Diagnostics" />
      <Import moduleName="RemoteAccess" />
      <Import moduleName="RemoteForwarder" />
    </Imports>
    <LocalResources>
      <LocalStorage name="localStoreOne" sizeInMB="10" />
      <LocalStorage name="localStoreTwo" sizeInMB="10" cleanOnRoleRecycle="false" />
    </LocalResources>
    <Startup>
      <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple" />
    </Startup>
  </WebRole>

  <WorkerRole name="WorkerRole1">
    <ConfigurationSettings>
      <Setting name="DiagnosticsConnectionString" />
    </ConfigurationSettings>
    <Imports>
      <Import moduleName="RemoteAccess" />
      <Import moduleName="RemoteForwarder" />
    </Imports>
    <Endpoints>
      <InputEndpoint name="Endpoint1" protocol="tcp" port="10000" />
      <InternalEndpoint name="Endpoint2" protocol="tcp" />
    </Endpoints>
  </WorkerRole>
</ServiceDefinition>
```

Finden Sie das [Service Definition Schema] [] für ein besseres Verständnis der hier verwendete XML-Schema, hier jedoch eine kurze Erläuterung der Elemente:

**Standorte**  
Enthält die Definitionen für Websites oder Web Applications, die in IIS7 gehostet werden.

**InputEndpoints**  
Enthält die Definitionen für Endpunkte mit Cloud-Dienst verwendet werden.

**InternalEndpoints**  
Enthält die Definitionen für Endpunkte von Instanzen verwendet werden, um miteinander kommunizieren.

**ConfigurationSettings**  
Enthält Einstellungsdefinitionen für Funktionen einer bestimmten Rolle.

**Zertifikate**  
Enthält die Definitionen für Zertifikate, die für eine Rolle erforderlich sind. Im vorherigen Codebeispiel zeigt ein Zertifikat, das für die Konfiguration von Azure Connect verwendet wird.

**LocalResources**  
Enthält die Definitionen für lokale Speicherressourcen. Lokale Speicherressourcen ist ein reservierter Verzeichnis im Dateisystem des virtuellen Computers in der Instanz einer Rolle ausgeführt wird.

**Einfuhren**  
Enthält die Definitionen für importierte Module. Im vorherigen Codebeispiel zeigt die Module zum Remotedesktopverbindung und Azure Connect.

**Start**  
Enthält Aufgaben, die ausgeführt werden, wenn die Rolle gestartet wird. Die Aufgaben werden in einem cmd oder ausführbaren Datei definiert.



<a name="cscfg"></a>
## <a name="serviceconfigurationcscfg"></a>ServiceConfiguration.cscfg
Die Konfiguration der Einstellungen für den Clouddienst bestimmt die Werte in der Datei **ServiceConfiguration.cscfg** . Sie geben die Anzahl der Instanzen für jede Rolle in dieser Datei bereitstellen möchten. Die Werte für die Konfigurationsdateien, die in der Service-Definitionsdatei definiert werden Dienstkonfigurationsdatei hinzugefügt. Fingerabdrücke für Management Zertifikate, die mit Cloud-Dienst werden auch die Datei hinzugefügt. [Azure Service Configuration Schema (.cscfg Datei)](https://msdn.microsoft.com/library/azure/ee758710.aspx) stellt das zulässige Format für eine Dienstkonfigurationsdatei.

Die Dienstkonfigurationsdatei nicht mit der Anwendung verpackt aber in Azure als separate Datei hochgeladen und Cloud-Dienst konfiguriert. Sie können eine neue Service-Konfigurationsdatei hochladen, ohne erneute Bereitstellung Cloud-Dienst. Die Konfigurationswerte für Cloud-Dienst können geändert werden, während der Clouddienst ausgeführt wird. Das folgende Beispiel zeigt die Konfigurationsdateien für die Web- und Workerrollen Rollen definiert werden können:

```xml
<?xml version="1.0"?>
<ServiceConfiguration serviceName="MyServiceName" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration">
  <Role name="WebRole1">
    <Instances count="2" />
    <ConfigurationSettings>
      <Setting name="SettingName" value="SettingValue" />
    </ConfigurationSettings>

    <Certificates>
      <Certificate name="CertificateName" thumbprint="CertThumbprint" thumbprintAlgorithm="sha1" />
      <Certificate name="Microsoft.WindowsAzure.Plugins.RemoteAccess.PasswordEncryption"
         thumbprint="CertThumbprint" thumbprintAlgorithm="sha1" />
    </Certificates>
  </Role>
</ServiceConfiguration>
```

[Konfigurationsschema Service](https://msdn.microsoft.com/library/azure/ee758710.aspx) für ein besseres Verständnis des hier verwendeten XML-Schemas finden hier jedoch eine kurze Erläuterung der Elemente:

**Instanzen**  
Die Anzahl der ausgeführten Instanzen für die Rolle konfigurieren. Um Ihre Cloud-Dienst verhindern möglicherweise nicht verfügbar bei Upgrades, wird empfohlen, dass mehr als eine Instanz Ihrer Web verbundenen Rollen bereitstellen. Damit halten Sie Richtlinien in [Azure Compute Service Level Agreement (SLA)](http://azure.microsoft.com/support/legal/sla/)garantiert 99,95 % externe Konnektivität für Internetzugriff Rollen bei zwei oder mehr Instanzen für einen Dienst bereitgestellt werden.

**ConfigurationSettings**  
Konfiguriert die Einstellungen für die ausgeführten Instanzen für eine Rolle. Der Name der `<Setting>` Elemente muss die Einstellungsdefinitionen in der Service-Definitionsdatei.

**Zertifikate**  
Das vom Dienst verwendete Zertifikate konfiguriert. Im vorherigen Codebeispiel zeigt, wie das Zertifikat für das Modul RAS. Der Wert des Attributs *Fingerabdruck* muss in den Fingerabdruck des Zertifikats festgelegt werden, verwendet.

<p/>

 >[AZURE.NOTE] Der Fingerabdruck des Zertifikats kann in der Konfigurationsdatei mit einem Texteditor hinzugefügt werden, oder der Wert auf die Registerkarte **Zertifikate** der **Eigenschaftenseite der Visual Studio-Rolle** hinzugefügt werden.



## <a name="defining-ports-for-role-instances"></a>Definieren von Ports für Instanzen
Azure ermöglicht nur einen Einstiegspunkt in einer Webrolle. Dies bedeutet, dass alle Datenverkehr über eine IP-Adresse auftritt. Sie können Ihre Websites einen Port freigeben, indem Sie den Hostheader die Anforderung an den richtigen Speicherort Direct konfigurieren. Sie können auch Ihre Anwendung bekannter Ports auf die IP-Adresse abhören konfigurieren.

Das folgende Beispiel zeigt die Konfiguration für eine Webrolle mit einer Website und Anwendung. Die Website als Eintrag Standardspeicherort auf Port 80 konfiguriert und Web-Applikationen konfiguriert aus einem alternativen Hostheader Anfragen, die "mail.mysite.cloudapp.net" aufgerufen wird.

```xml
<WebRole>
  <ConfigurationSettings>
    <Setting name="DiagnosticsConnectionString" />
  </ConfigurationSettings>
  <Endpoints>
    <InputEndpoint name="HttpIn" protocol="http" <mark>port="80"</mark> />
    <InputEndpoint name="Https" protocol="https" port="443" certificate="SSL"/>
    <InputEndpoint name="NetTcp" protocol="tcp" port="808" certificate="SSL"/>
  </Endpoints>
  <LocalResources>
    <LocalStorage name="Sites" cleanOnRoleRecycle="true" sizeInMB="100" />
  </LocalResources>
  <Site name="Mysite" packageDir="Sites\Mysite">
    <Bindings>
      <Binding name="http" endpointName="HttpIn" />
      <Binding name="https" endpointName="Https" />
      <Binding name="tcp" endpointName="NetTcp" />
    </Bindings>
  </Site>
  <Site name="MailSite" packageDir="MailSite">
    <Bindings>
      <Binding name="mail" endpointName="HttpIn" <mark>hostheader="mail.mysite.cloudapp.net"</mark> />
    </Bindings>
    <VirtualDirectory name="artifacts" />
    <VirtualApplication name="storageproxy">
      <VirtualDirectory name="packages" packageDir="Sites\storageProxy\packages"/>
    </VirtualApplication>
  </Site>
</WebRole>
```


## <a name="changing-the-configuration-of-a-role"></a>Ändern einer Rolle
Sie können die Konfiguration dem Cloud-Dienst aktualisieren, während es in Azure ausgeführt wird, ohne den Dienst offline. Ändern Sie Konfigurationsinformationen können Sie hochladen eine neuen Konfigurationsdatei oder Sie bearbeiten die Konfigurationsdatei vorhanden und gelten für den ausgeführten Dienst. Die folgenden kann die Konfiguration eines Dienstes geändert werden:

- **Ändern der Werte von Konfigurationen**  
Bei einer Konfiguration festlegen ändert eine Rolleninstanz können Änderung übernehmen, während die Instanz online, oder die Instanz ordnungsgemäß wiederverwenden und die Änderung während der Instanz offline ist.

- **Service-Topologie von Instanzen ändern**  
Topologie beeinflussen nicht ausgeführte Instanzen außer dem eine Instanz entfernt wird. Im Allgemeinen müssen alle verbleibende Instanzen nicht wiederverwendet werden. Sie können Instanzen auf Topologie wiederverwenden.

- **Ändern den Fingerabdruck des Zertifikats**  
Sie können ein Zertifikat nur aktualisieren, wenn eine Rolleninstanz offline ist. Ein Zertifikat wird hinzugefügt, gelöscht oder geändert werden, während eine Rolleninstanz online ist, nimmt Azure ordnungsgemäß die Instanz offline Zertifikat aktualisieren und wieder online zu bringen, nachdem die Änderung abgeschlossen ist.

### <a name="handling-configuration-changes-with-service-runtime-events"></a>Neukonfiguration Service Runtime Ereignisse behandeln
[Azure-Laufzeitbibliothek](https://msdn.microsoft.com/library/azure/mt419365.aspx) enthält den [Microsoft.WindowsAzure.ServiceRuntime](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.aspx) -Namespace bietet Klassen für die Interaktion mit der Azure-Umgebung von Code in einer Instanz einer Rolle. [RoleEnvironment](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.aspx) -Klasse definiert die folgenden Ereignisse, die vor und nach der konfigurationsänderung einer ausgelöst werden:

- **[Changing](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.changing.aspx) -Ereignis**  
Dies tritt vor der Änderung der Konfiguration angegebene Instanz einer Rolle, wodurch Sie die Möglichkeit, Instanzen der Rolle übernehmen.
- **[Changed](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.changed.aspx) -Ereignis**  
Tritt nach der Änderung der Konfiguration angegebene Instanz einer Rolle zugewiesen ist.

> [AZURE.NOTE] Da Zertifikat ändert Instanzen einer Rolle offline nehmen, lösen sie keine RoleEnvironment.Changing oder RoleEnvironment.Changed-Ereignisse.

<a name="cspkg"></a>
## <a name="servicepackagecspkg"></a>ServicePackage.cspkg
Zur Bereitstellung einer Anwendung als Cloud-Dienst in Azure müssen Sie zuerst die Anwendung im entsprechenden Format packen. Das Befehlszeilentool **CSPack** ( [Azure SDK](https://azure.microsoft.com/downloads/)installiert) können Sie die Datei als Alternative zu Visual Studio zu erstellen.

**CSPack** verwendet den Inhalt der Datei und Dienst definiert den Inhalt des Pakets. **CSPack** erzeugt eine Anwendung Paketdatei (.cspkg), die Sie in Azure mithilfe von [Azure-Portal](cloud-services-how-to-create-deploy-portal.md#create-and-deploy)hochladen können. Standardmäßig heißt das Paket `[ServiceDefinitionFileName].cspkg`, aber Sie können einen anderen Namen mit der `/out` Option **CSPack**.

**CSPack** wird in der Regel am  
`C:\Program Files\Microsoft SDKs\Azure\.NET SDK\[sdk-version]\bin\`

>[AZURE.NOTE]
CSPack.exe (unter Windows) ist die **Microsoft Azure Command Prompt** -Verknüpfung, die mit dem SDK installiert ist.  
>  
>Führen Sie CSPack.exe aus, um die Dokumentation zu allen möglichen Optionen und Befehle.

<p />

>[AZURE.TIP]
Cloud-Dienst in **Microsoft Azure-Serveremulator**lokal ausführen, die Option **/copyonly** diese Option die Binärdateien der Anwendung eine Verzeichnisstruktur kopiert aus dem sie im Serveremulator ausgeführt werden können.

### <a name="example-command-to-package-a-cloud-service"></a>Beispielbefehl Cloud Service-Paket
Das folgende Beispiel erstellt ein Anwendungspaket mit den Informationen für eine Webrolle. Der Befehl gibt Service-Definitionsdatei Verzeichnis verwenden binäre Dateien finden, und der Name der Paketdatei.

    cspack [DirectoryName]\[ServiceDefinition]
           /role:[RoleName];[RoleBinariesDirectory]
           /sites:[RoleName];[VirtualPath];[PhysicalPath]
           /out:[OutputFileName]

Wenn die Anwendung eine Webrolle und eine Worker-Rolle enthält, ist der folgende Befehl verwendet:

    cspack [DirectoryName]\[ServiceDefinition]
           /out:[OutputFileName]
           /role:[RoleName];[RoleBinariesDirectory]
           /sites:[RoleName];[VirtualPath];[PhysicalPath]
           /role:[RoleName];[RoleBinariesDirectory];[RoleAssemblyName]

Wo sind die Variablen wie folgt definiert:

| Variable                  | Wert |
| ------------------------- | ----- |
| \[Verzeichnisname\]         | Das Unterverzeichnis unter dem Stammverzeichnis des Projekts mit der .csdef Datei des Projekts Azure.|
| \[ServiceDefinition\]     | Der Name der Definitionsdatei Service. Standardmäßig heißt diese Datei ServiceDefinition.csdef.  |
| \[OutputFileName\]        | Der Name für die generierte Datei. Dies ist normalerweise der Name der Anwendung fest. Wenn kein Dateiname angegeben ist, wird als Anwendungspaket erstellt \[ApplicationName\].cspkg.|
| \[Rollenname\]              | Der Name der Rolle in der Service-Definitionsdatei definiert.|
| \[RoleBinariesDirectory] | Der Speicherort der Binärdateien für die Rolle.|
| \[VirtualPath\]           | Die physischen Verzeichnisse für jeden virtuellen Pfad gemäß Abschnitt Sites die Dienstdefinition.|
| \[PhysicalPath\]          | Die physischen Verzeichnisse des Inhalts jedes virtuellen Pfad in der Dienstdefinition-Websiteknoten definiert.|
| \[RoleAssemblyName\]      | Der Name der Binärdatei für die Rolle.| 


## <a name="next-steps"></a>Nächste Schritte

Ich erstelle ein Cloud Service-Paket, und ich möchte...

* [Remotedesktop für eine Cloud Service-Instanz einrichten][remotedesktop]
* [Bereitstellen eines Cloud-Dienst][deploy]

Ich verwende Visual Studio und möchte...

* [Neue Clouddienst erstellen][vs_create]
* [Konfigurieren Sie einen vorhandenen Cloud-Dienst][vs_reconfigure]
* [Bereitstellen eines Cloud-Dienst][vs_deploy]
* [Remotedesktop für eine Cloud Service-Instanz einrichten][vs_remote]

[deploy]: cloud-services-how-to-create-deploy-portal.md
[remotedesktop]: cloud-services-role-enable-remote-desktop.md
[vs_remote]: ../vs-azure-tools-remote-desktop-roles.md
[vs_deploy]: ../vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md
[vs_reconfigure]: ../vs-azure-tools-configure-roles-for-cloud-service.md
[vs_create]: ../vs-azure-tools-azure-project-create.md
