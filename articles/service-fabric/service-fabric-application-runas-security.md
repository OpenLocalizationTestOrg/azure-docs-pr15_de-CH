<properties
   pageTitle="Grundlegendes zu Sicherheitsrichtlinien Service und Service Fabric-Anwendung | Microsoft Azure"
   description="Eine Übersicht zum Ausführen einer Anwendung Service Fabric System und lokale Konten, einschließlich des SetupEntry Punkt eine Anwendung benötigt privilegierte Aktionen ausführen, bevor er beginnt"
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
   ms.date="09/22/2016"
   ms.author="mfussell"/>

# <a name="configure-security-policies-for-your-application"></a>Konfigurieren von Sicherheitsrichtlinien für die Anwendung
Azure Service Fabric ermöglicht das sichere im Cluster unter verschiedenen Benutzerkonten ausgeführt. Fabric Service sichert auch zum Zeitpunkt der Bereitstellung unter dem Benutzerkonto wie Dateien, Verzeichnisse und Zertifikate von der Anwendung verwendeten Ressourcen. Dadurch ausgeführt Applications auch in einer freigegebenen gehosteten Umgebung sicher voneinander. 

Standardmäßig führen Service Fabric-Anwendung unter dem Konto unter Fabric.exe Prozess ausgeführt wird. Service Fabric ermöglicht auch unter einem lokalen Benutzerkonto oder im Anwendungsmanifest angegebene Konto Lokales System ausgeführt. Unterstützt lokales System Kontenarten für sind **LocalUser**, **Netzwerkdienst**, **Lokaler Dienst**und **LocalSystem**.

 Fabric-Dienst unter Windows Server im Rechenzentrum mit dem eigenständigen Installer können Sie Domänenkonten für Active Directory (AD).

Benutzergruppen können definiert und erstellt, sodass jede Gruppe gemeinsam verwaltet werden ein oder mehrere Benutzer hinzugefügt werden können. Dies ist nützlich, wenn mehrere für verschiedene Einstiegspunkte Benutzer und sie müssen bestimmte allgemeine Rechte, die Ebene der Gruppe verfügbar sind.

## <a name="configure-the-policy-for-service-setupentrypoint"></a>Konfigurieren Sie die Richtlinie für den Dienst SetupEntryPoint

Gemäß dem [Anwendungsmodell](service-fabric-application-model.md) ist **SetupEntryPoint** eine privilegierte, die mit den gleichen Anmeldeinformationen vor anderen Einstiegspunkt als Dienst (normalerweise das Konto *Netzwerkdienst* ) ausgeführt wird. Die ausführbare Datei **EntryPoint** angegeben ist normalerweise Diensthost langer so mit einem separates Setup Einstiegspunkt vermeidet ausführbare Diensthost für längere Zeit mit hohen Berechtigungen ausgeführt. Die ausführbare Datei **EntryPoint** angegeben wird ausgeführt, nachdem **SetupEntryPoint** erfolgreich beendet. Der daraus resultierende Prozess überwacht und neu gestartet, angefangen bei **SetupEntryPoint**immer beendet oder stürzt ab.

Nachfolgend eine einfache Service manifest Beispiel der SetupEntryPoint und der wichtigsten Einstiegspunkt für den Dienst.

~~~
<?xml version="1.0" encoding="utf-8" ?>
<ServiceManifest Name="MyServiceManifest" Version="SvcManifestVersion1" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Description>An example service manifest</Description>
  <ServiceTypes>
    <StatelessServiceType ServiceTypeName="MyServiceType" />
  </ServiceTypes>
  <CodePackage Name="Code" Version="1.0.0">
    <SetupEntryPoint>
      <ExeHost>
        <Program>MySetup.bat</Program>
        <WorkingFolder>CodePackage</WorkingFolder>
      </ExeHost>
    </SetupEntryPoint>
    <EntryPoint>
      <ExeHost>
        <Program>MyServiceHost.exe</Program>
      </ExeHost>
    </EntryPoint>
  </CodePackage>
  <ConfigPackage Name="Config" Version="1.0.0" />
</ServiceManifest>
~~~

### <a name="configure-the-policy-using-a-local-account"></a>Konfigurieren Sie die Richtlinie mithilfe eines lokalen Kontos

Nach dem Konfigurieren des Dienstes zu Setup Einstiegspunkt können Sie die Sicherheitsberechtigungen ändern, die unter im Anwendungsmanifest ausgeführt wird. Das folgende Beispiel veranschaulicht, wie zur Konfiguration des Dienstes unter Benutzer Konto Administratorrechte ausgeführt.

~~~
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyApplicationType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MyServiceTypePkg" ServiceManifestVersion="1.0.0" />
      <ConfigOverrides />
      <Policies>
         <RunAsPolicy CodePackageRef="Code" UserRef="SetupAdminUser" EntryPointType="Setup" />
      </Policies>
   </ServiceManifestImport>
   <Principals>
      <Users>
         <User Name="SetupAdminUser">
            <MemberOf>
               <SystemGroup Name="Administrators" />
            </MemberOf>
         </User>
      </Users>
   </Principals>
</ApplicationManifest>
~~~

Erstellen Sie zunächst einen **Hauptbenutzer** -Abschnitt mit einem Benutzernamen wie SetupAdminUser. Dies bedeutet, dass der Benutzer Mitglied der Gruppe der Administratoren.

Konfigurieren Sie anschließend im Abschnitt **ServiceManifestImport** eine Richtlinie für diesen Prinzipal auf **SetupEntryPoint**anwenden. Dies weist Fabric Service, dass beim Ausführen der Datei **MySetup.bat** "runas" mit Administratorrechten sein soll. Da Sie *eine Richtlinie für den Haupteinstiegspunkt angewendet* , der Code im **MyServiceHost.exe** System **Netzwerkdienstkonto** ausgeführt wird. Dies ist das Standardkonto alle Einstiegspunkte ausgeführt werden.

Jetzt fügen Sie der Datei MySetup.bat dem Visual Studio-Projekt so testen Sie die Administratorrechte. In Visual Studio mit der rechten Maustaste auf das Service-Projekt und fügen eine neue Datei namens MySetup.bat.

Als Nächstes sicherstellen Sie, dass die Datei MySetup.bat in das Service-Paket enthalten ist. Standardmäßig ist es nicht. Wählen Sie die Datei, rechtsklicken Sie, um das Kontextmenü aufzurufen, und wählen Sie **Eigenschaften**. Klicken Sie im Dialogfeld Eigenschaften sicher, dass **In Ausgabeverzeichnis kopieren** **Kopieren, wenn neuer**festgelegt ist. Siehe dazu folgende Bildschirmdarstellung.

![Visual Studio-CopyToOutput für SetupEntryPoint Batchdatei][image1]

Nun öffnen Sie die Datei MySetup.bat, und fügen Sie die folgenden Befehle:

~~~
REM Set a system environment variable. This requires administrator privilege
setx -m TestVariable "MyValue"
echo System TestVariable set to > out.txt
echo %TestVariable% >> out.txt

REM To delete this system variable us
REM REG delete "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Environment" /v TestVariable /f
~~~

Als Nächstes erstellen und Bereitstellen der Lösung zu einem Cluster lokale Entwicklung.  Nachdem der Dienst gestartet wurde, wie im Service Fabric-Explorer angezeigt, sehen Sie zwei Arten der MySetup.bat war. Öffnen Sie ein PowerShell-Befehlszeile und Typ:

~~~
PS C:\ [Environment]::GetEnvironmentVariable("TestVariable","Machine")
MyValue
~~~

Notieren Sie den Namen des Knotens, in dem der Dienst bereitgestellt wurde und beispielsweise Knoten 2 im Explorer Fabric Service gestartet. Wechseln Sie nun Anwendung Instanz Arbeit Ordner out.txt finden, die dem Wert der **TestVariable**. Für Beispiel, wenn dieser Dienst auf Knoten 2, dann bereitgestellt wurde für diesen Pfad für **MyApplicationType**gehen:

~~~
C:\SfDevCluster\Data\_App\Node.2\MyApplicationType_App\work\out.txt
~~~

###  <a name="configure-the-policy-using-local-system-accounts"></a>Konfigurieren Sie die Richtlinie mit lokalen Systemkonten
Häufig empfiehlt es sich, anstelle eines lokalen Systemkontos ein Administratorkonto wie vor Startskripts ausführen. Ausführen der RunAs funktioniert Gruppenrichtlinien Administratoren in der Regel nicht seit Computern der Benutzerkontensteuerung (UAC) aktiviert ist. In solchen Fällen **wird empfohlen, die SetupEntryPoint als LocalSystem ausgeführt anstelle eines lokalen Benutzers Administratorengruppe hinzugefügt**. Das folgende Beispiel veranschaulicht die SetupEntryPoint als LocalSystem ausgeführt.

~~~
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyApplicationType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MyServiceTypePkg" ServiceManifestVersion="1.0.0" />
      <ConfigOverrides />
      <Policies>
         <RunAsPolicy CodePackageRef="Code" UserRef="SetupLocalSystem" EntryPointType="Setup" />
      </Policies>
   </ServiceManifestImport>
   <Principals>
      <Users>
         <User Name="SetupLocalSystem" AccountType="LocalSystem" />
      </Users>
   </Principals>
</ApplicationManifest>
~~~

##  <a name="launch-powershell-commands-from-a-setupentrypoint"></a>PowerShell Befehle aus einem SetupEntryPoint
Führen Sie PowerShell zwischen dem **SetupEntryPoint** können Sie **PowerShell.exe** in einer Batchdatei ausführen, die PowerShell-Datei verweist. Projekt Service wie **MySetup.ps1**, PowerShell-Datei hinzufügen. Denken Sie daran, zu *Kopieren, wenn neuer* -Eigenschaft festlegen, damit die Datei auch in das Service-Paket enthalten ist. Das folgende Beispiel zeigt die Batchdatei zum Starten einer PowerShell-Datei namens MySetup.ps1, setzt eine Systemumgebungsvariable **TestVariable**aufgerufen.


PowerShell-Datei für MySetup.bat.

~~~
powershell.exe -ExecutionPolicy Bypass -Command ".\MySetup.ps1"
~~~

Fügen Sie in der PowerShell so eine Systemumgebungsvariable festgelegt:

~~~
[Environment]::SetEnvironmentVariable("TestVariable", "MyValue", "Machine")
[Environment]::GetEnvironmentVariable("TestVariable","Machine") > out.txt
~~~

**Hinweis:** Standardmäßig sucht führt die Batch-Datei in der Anwendungsordner **arbeiten** für Dateien. In diesem Fall der Ausführung MySetup.bat möchten wir dies die MySetup.ps1 im gleichen Ordner finden **Paket** Anwendungsordner. Den Arbeitsordner festgelegt um diesen Ordner zu ändern, wie im folgenden dargestellt.

~~~
<SetupEntryPoint>
    <ExeHost>
    <Program>MySetup.bat</Program>
    <WorkingFolder>CodePackage</WorkingFolder>
    </ExeHost>
</SetupEntryPoint>
~~~

## <a name="using-console-redirection-for-local-debugging"></a>Umleitung der Konsole verwenden für lokales Debuggen
Gelegentlich empfiehlt es sich, die Konsolenausgabe von Skript für das Debuggen an. Dazu können Sie die Ausgabe in eine Datei schreibt eine Konsole Umleitungsrichtlinie festlegen. Die Ausgabe bezieht sich auf den Anwendungsordner **Protokoll** für den Knoten, in dem die Anwendung bereitgestellt und ausgeführt (Siehe, dies im vorherigen Beispiel finden) aufgerufen.

**Hinweis: nie** Verwendung Umleitungsrichtlinie Konsole in einer Anwendung in der Produktion bereitgestellt werden, da dies das Anwendungsfailover auswirken kann. **Nur** mithilfe dieser für Entwicklung und Debuggen.  

Das folgende Beispiel veranschaulicht konsolenumleitung mit einem FileRetentionCount-Wert.

~~~
<SetupEntryPoint>
    <ExeHost>
    <Program>MySetup.bat</Program>
    <WorkingFolder>CodePackage</WorkingFolder>
    <ConsoleRedirection FileRetentionCount="10"/>
    </ExeHost>
</SetupEntryPoint>
~~~

Wenn Sie nun die Datei MySetup.ps1, um ein **Echo** Befehl ändern, wird diese in die Ausgabedatei für debugging-Zwecke schreiben.

~~~
Echo "Test console redirection which writes to the application log folder on the node that the application is deployed to"
~~~

**Debuggen von Skript umgehend entfernen Sie diese Konsole Umleitungsrichtlinie**

## <a name="configure-policy-for-service-code-packages"></a>Konfigurieren von Richtlinien für Servicepakete code 
In den vorherigen Schritten wurde gezeigt, wie RunAs auf SetupEntryPoint anwenden. Sehen wir etwas genauer wie andere Sicherheitsprinzipale erstellt werden, die als Richtlinien angewendet werden können.

### <a name="create-local-user-groups"></a>Lokale Benutzergruppen erstellen
Benutzergruppen können definiert und erstellt ein oder mehrere Benutzer zu einer Gruppe hinzugefügt werden. Dies ist besonders nützlich, wenn mehrere für verschiedene Einstiegspunkte Benutzer und sie müssen bestimmte allgemeine Rechte, die Ebene der Gruppe verfügbar sind. Das folgende Beispiel zeigt eine lokale Gruppe namens **LocalAdminGroup** mit Administratorrechten. Zwei Benutzer Customer1 und Customer2, erfolgen Mitglied dieser lokalen Gruppe.

~~~
<Principals>
 <Groups>
   <Group Name="LocalAdminGroup">
     <Membership>
       <SystemGroup Name="Administrators"/>
     </Membership>
   </Group>
 </Groups>
  <Users>
     <User Name="Customer1">
        <MemberOf>
           <Group NameRef="LocalAdminGroup" />
        </MemberOf>
     </User>
    <User Name="Customer2">
      <MemberOf>
        <Group NameRef="LocalAdminGroup" />
      </MemberOf>
    </User>
  </Users>
</Principals>
~~~

### <a name="create-local-users"></a>Erstellen von lokalen Benutzern
Sie können einen lokalen Benutzer erstellen, mit der einen Dienst innerhalb der Anwendung zu sichern. Wenn ein Konto **LocalUser** Prinzipale Abschnitt des Anwendungsmanifests angegeben, erstellt Service Fabric lokale Benutzerkonten auf, in dem die Anwendung bereitgestellt wird. Standardmäßig haben diese Konten nicht denselben Namen wie die im Anwendungsmanifest (z. B. Customer3 in dem folgenden Beispiel). Stattdessen werden dynamisch erstellt und zufällige Kennwörter.

~~~
<Principals>
  <Users>
     <User Name="Customer3" AccountType="LocalUser" />
  </Users>
</Principals>
~~~

<!-- If an application requires that the user account and password be same on all machines (for example, to enable NTLM authentication), the cluster manifest must set NTLMAuthenticationEnabled to true. The cluster manifest must also specify an NTLMAuthenticationPasswordSecret that will be used to generate the same password across all machines.

<Section Name="Hosting">
      <Parameter Name="EndpointProviderEnabled" Value="true"/>
      <Parameter Name="NTLMAuthenticationEnabled" Value="true"/>
      <Parameter Name="NTLMAuthenticationPassworkSecret" Value="******" IsEncrypted="true"/>
 </Section>
-->

### <a name="assign-policies-to-the-service-code-packages"></a>Zuweisen von Richtlinien zu Code Servicepakete
Abschnitt **RunAsPolicy** **ServiceManifestImport** gibt das Konto vom Abschnitt Principals Ausführen eines codepakets verwendet werden soll. Außerdem werden Benutzerkonten im Abschnitt Prinzipale Pakete in das Manifest zugeordnet. Können Sie angeben, für die Installation oder wichtigsten Einstiegspunkte, oder Sie können `All` für beide übernehmen. Das folgende Beispiel zeigt unterschiedliche Richtlinien angewendet werden:

~~~
<Policies>
<RunAsPolicy CodePackageRef="Code" UserRef="LocalAdmin" EntryPointType="Setup"/>
<RunAsPolicy CodePackageRef="Code" UserRef="Customer3" EntryPointType="Main"/>
</Policies>
~~~

Wenn **EntryPointType** nicht angegeben wird, die Standardeinstellung von EntryPointType = "Main". Angeben **SetupEntryPoint** ist besonders nützlich, wenn bestimmte hoher Berechtigung Installationsvorgang unter einem Systemkonto ausgeführt werden soll. Der eigentlichen Code kann unter einem Konto mit niedrigeren Berechtigungen ausgeführt.

### <a name="apply-a-default-policy-to-all-service-code-packages"></a>Alle Servicepakete Code eine Standardrichtlinie zuweisen
**DefaultRunAsPolicy** Abschnitt dient ein Standardbenutzerkonto für alle Pakete an, die einen bestimmten **RunAsPolicy** definiert haben. Benötigen gemäß Dienstmanifest von einer Anwendung verwendete Code-Pakete unter demselben Benutzer ausgeführt werden, kann die Anwendung nur eine RunAs-Standardrichtlinie mit diesem Benutzerkonto definieren. Das folgende Beispiel gibt an, dass ein Paket kein **RunAsPolicy** angegeben, das Paket unter der **MyDefaultAccount** im Abschnitt "Hauptbenutzer" ausgeführt werden soll.

~~~
<Policies>
  <DefaultRunAsPolicy UserRef="MyDefaultAccount"/>
</Policies>
~~~
### <a name="using-an-active-directory-domain-group-or-user"></a>Verwenden eine Active Directory-Domäne-Gruppe oder einen Benutzer
Für Dienst auf Windows Server mit dem eigenständigen Installationsprogramm installiert läuft, den Dienst die Anmeldeinformationen für eine Active Directory (AD) Benutzer- oder Gruppenkonto. Hinweis: Dies ist Active Directory lokal in der Domäne und nicht mit Azure Active Directory (AAD). Mithilfe von einen Domänenbenutzer oder eine Gruppe können Sie dann andere Ressourcen in der Domäne (z. B. Dateifreigaben) zugreifen, die Berechtigungen erteilt wurden.

Das folgende Beispiel zeigt einen AD-Benutzer *TestUser* mit ihr Domänenkennwort verschlüsselt mit einem Zertifikat namens *MyCert*aufgerufen. Sie können die `Invoke-ServiceFabricEncryptText` Powershell-Befehl geheimen Geheimtext erstellen. Finden Sie diese Artikel [Geheimnisse Service Fabric-Anwendung verwalten](service-fabric-application-secret-management.md) . Der private Schlüssel des Zertifikats zur Entschlüsselung des Kennworts muss auf dem lokalen Computer in einer Out-of-Band-Methode (in Azure über Ressourcen-Manager sieht) bereitgestellt werden. Dann wird Fabric Service das Service-Paket auf dem Computer bereitgestellt werden, zusammen mit den Benutzernamen authentifizieren mit diesen Anmeldeinformationen ausführen und den geheimen Schlüssel entschlüsseln kann.

~~~
<Principals>
  <Users>
    <User Name="TestUser" AccountType="DomainUser" AccountName="Domain\User" Password="[Put encrypted password here using MyCert certificate]" PasswordEncrypted="true" />
  </Users>
</Principals>
<Policies>
  <DefaultRunAsPolicy UserRef="TestUser" />
  <SecurityAccessPolicies>
    <SecurityAccessPolicy ResourceRef="MyCert" PrincipalRef="TestUser" GrantRights="Full" ResourceType="Certificate" />
  </SecurityAccessPolicies>
</Policies>
<Certificates>
~~~


## <a name="assign-securityaccesspolicy-for-http-and-https-endpoints"></a>Zuweisen von SecurityAccessPolicy für HTTP- und HTTPS-Endpunkte
Wenn ein Dienst RunAs-Richtlinie zuweisen Dienstmanifest Ressourcen für HTTP deklariert, geben Sie eine **SecurityAccessPolicy** um sicherzustellen, dass Endpunkte zugewiesenen Ports richtig Zugriffskontrolle für RunAs-Benutzerkonto, dem den Dienst aufgeführt. Andernfalls **http.sys** keinen Zugriff auf den Dienst und Fehler durch Aufrufe vom Client erhalten. Das folgende Beispiel gilt Customer3 Konto für einen Endpunkt aufgerufen **ServiceEndpointName**, volle Zugriffsrechte erteilen.

~~~
<Policies>
   <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
   <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and the Endpoint is http -->
   <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
</Policies>
~~~

Für die HTTPS-Endpunkt können Sie den Namen des Zertifikats an den Client zurückgegeben. Sie können dazu das Zertifikat in einem Abschnitt Zertifikate im Anwendungsmanifest mit **EndpointBindingPolicy**.

~~~
<Policies>
   <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
  <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and the Endpoint is http -->
   <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
  <!--EndpointBindingPolicy is needed if the EndpointName is secured with https -->
  <EndpointBindingPolicy EndpointRef="EndpointName" CertificateRef="Cert1" />
</Policies
~~~


## <a name="a-complete-application-manifest-example"></a>Eine vollständige manifest wird
Das folgenden Anwendungsmanifest zeigt viele der anderen:

~~~
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="Application3Type" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Parameters>
      <Parameter Name="Stateless1_InstanceCount" DefaultValue="-1" />
   </Parameters>
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="Stateless1Pkg" ServiceManifestVersion="1.0.0" />
      <ConfigOverrides />
      <Policies>
         <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
         <RunAsPolicy CodePackageRef="Code" UserRef="LocalAdmin" EntryPointType="Setup" />
        <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and the Endpoint is http -->
         <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
        <!--EndpointBindingPolicy is needed the EndpointName is secured with https -->
        <EndpointBindingPolicy EndpointRef="EndpointName" CertificateRef="Cert1" />
     </Policies>
   </ServiceManifestImport>
   <DefaultServices>
      <Service Name="Stateless1">
         <StatelessService ServiceTypeName="Stateless1Type" InstanceCount="[Stateless1_InstanceCount]">
            <SingletonPartition />
         </StatelessService>
      </Service>
   </DefaultServices>
   <Principals>
      <Groups>
         <Group Name="LocalAdminGroup">
            <Membership>
               <SystemGroup Name="Administrators" />
            </Membership>
         </Group>
      </Groups>
      <Users>
         <User Name="LocalAdmin">
            <MemberOf>
               <Group NameRef="LocalAdminGroup" />
            </MemberOf>
         </User>
         <!--Customer1 below create a local account that this service runs under -->
         <User Name="Customer1" />
         <User Name="MyDefaultAccount" AccountType="NetworkService" />
      </Users>
   </Principals>
   <Policies>
      <DefaultRunAsPolicy UserRef="LocalAdmin" />
   </Policies>
   <Certificates>
     <EndpointCertificate Name="Cert1" X509FindValue="FF EE E0 TT JJ DD JJ EE EE XX 23 4T 66 "/>
  </Certificates>
</ApplicationManifest>
~~~


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Nächste Schritte

* [Verstehen der Anwendung](service-fabric-application-model.md)
* [Geben Sie Ressourcen in einem Servicemanifest](service-fabric-service-manifest-resources.md)
* [Bereitstellen einer Anwendung](service-fabric-deploy-remove-applications.md)

[image1]: ./media/service-fabric-application-runas-security/copy-to-output.png
