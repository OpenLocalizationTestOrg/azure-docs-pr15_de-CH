<properties 
pageTitle="Cloud Services-Rolle Config XPath Spickzettel | Microsoft Azure" 
description="XPath-Einstellungen in der Cloud-Dienstkonfiguration Rolle können Sie Standardeinstellungen als Umgebungsvariable setzen." 
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
ms.date="08/10/2016" 
ms.author="adegeo"/>

# <a name="expose-role-configuration-settings-as-an-environment-variable-with-xpath"></a>Machen Sie Konfigurationseinstellungen als eine Umgebungsvariable mit XPath verfügbar

In der Cloud Service Arbeitskraft oder Rolle Definition Webdienstdatei können als Umgebungsvariablen Laufzeitwerte verfügbar machen. Die folgende XPath-Werte werden unterstützt (die API-Werte entsprechen).

Diese XPath-Werte stehen in der [Microsoft.WindowsAzure.ServiceRuntime](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.aspx) -Bibliothek. 

## <a name="app-running-in-emulator"></a>Anwendung im emulator

Gibt an, dass die Anwendung im Emulator ausgeführt wird.

| Typ  | Beispiel |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/Deployment/@emulated" |
| Code  | Var X = RoleEnvironment.IsEmulated; |


## <a name="deployment-id"></a>Deployment-ID

Ruft die Deployment-ID für die Instanz.

| Typ  | Beispiel |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/Deployment/@id" |
| Code  | Var DeploymentId = RoleEnvironment.DeploymentId; |


## <a name="role-id"></a>Rollen-ID 

Ruft die aktuelle Rollen-ID für die Instanz.

| Typ  | Beispiel |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/@id" |
| Code  | Var-Id = RoleEnvironment.CurrentRoleInstance.Id; |


## <a name="update-domain"></a>Domäne aktualisieren

Ruft die Domäne aktualisieren der Instanz.

| Typ  | Beispiel |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/@updateDomain" |
| Code  | Var Ud = RoleEnvironment.CurrentRoleInstance.UpdateDomain; |


## <a name="fault-domain"></a>Fehlerdomäne

Ruft die Fehlerdomäne der Instanz.

| Typ  | Beispiel |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/@faultDomain" |
| Code  | Var fd = RoleEnvironment.CurrentRoleInstance.FaultDomain; |


## <a name="role-name"></a>Rollenname

Ruft den Rollennamen Instanzen.

| Typ  | Beispiel |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/@roleName" |
| Code  | Var Rname = RoleEnvironment.CurrentRoleInstance.Role.Name;  |


## <a name="config-setting"></a>Einstellung

Ruft den Wert der angegebenen Einstellung.

| Typ  | Beispiel |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/ConfigurationSettings/ConfigurationSetting[@name='Setting1']/@value" |
| Code  | Var Einstellung = RoleEnvironment.GetConfigurationSettingValue("Setting1"); |
 
## <a name="local-storage-path"></a>Lokaler Pfad

Ruft den lokalen Speicherpfad für die Instanz.

| Typ  | Beispiel |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='LocalStore1']/@path" |
| Code  | Var LocalResourcePath = RoleEnvironment.GetLocalResource("LocalStore1"). RootPath; |


## <a name="local-storage-size"></a>Lokaler Speicher

Ruft die Größe des lokalen Speicher für die Instanz.

| Typ  | Beispiel |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='LocalStore1']/@sizeInMB" |
| Code  | Var LocalResourceSizeInMB = RoleEnvironment.GetLocalResource("LocalStore1"). MaximumSizeInMegabytes; |

## <a name="endpoint-protocol"></a>Endpunktprotokoll 

Ruft das Endpunktprotokoll für die Instanz.

| Typ  | Beispiel |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/Endpoints/Endpoint[@name='Endpoint1']/@protocol" |
| Code  | Prot Var RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1 ="]. Protokoll; |

## <a name="endpoint-ip"></a>Endpunkt IP

Ruft den angegebenen Endpunkt IP-Adresse.

| Typ | Beispiel |
| ----- | ---- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/Endpoints/Endpoint[@name='Endpoint1']/@address" |
| Code  | Adresse von Var RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1 ="]. IPEndpoint.Address |

## <a name="endpoint-port"></a>Endpunkt-Anschluss 

Ruft den Endpunktport für die Instanz.

| Typ  | Beispiel |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/Endpoints/Endpoint[@name='Endpoint1']/@port" |
| Code  | Anschluss Var RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1 ="]. IPEndpoint.Port; |





## <a name="example"></a>Beispiel

Hier ist ein Beispiel für eine Worker-Rolle, die Startaufgabe mit einer Umgebungsvariable `TestIsEmulated` soll die [ @emulated XPath-Wert](#app-running-in-emulator). 

```xml
<WorkerRole name="Role1">
    <ConfigurationSettings>
      <Setting name="Setting1" />
    </ConfigurationSettings>
    <LocalResources>
      <LocalStorage name="LocalStore1" sizeInMB="1024"/>
    </LocalResources>
    <Endpoints>
      <InternalEndpoint name="Endpoint1" protocol="tcp" />
    </Endpoints>
    <Startup>
      <Task commandLine="example.cmd inputParm">
        <Environment>
          <Variable name="TestConstant" value="Constant"/>
          <Variable name="TestEmptyValue" value=""/>
          <Variable name="TestIsEmulated">
            <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated"/>
          </Variable>
          ...
        </Environment>
      </Task>
    </Startup>
    <Runtime>
      <Environment>
        <Variable name="TestConstant" value="Constant"/>
        <Variable name="TestEmptyValue" value=""/>
        <Variable name="TestIsEmulated">
          <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated"/>
        </Variable>
        ...
      </Environment>
    </Runtime>
    ...
</WorkerRole>
```

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über die Datei [ServiceConfiguration.cscfg](cloud-services-model-and-package.md#serviceconfigurationcscfg) .

Erstellt ein [ServicePackage.cspkg](cloud-services-model-and-package.md#servicepackagecspkg) Paket.

Aktivieren Sie [des Remotedesktops](cloud-services-role-enable-remote-desktop.md) für eine Rolle.
