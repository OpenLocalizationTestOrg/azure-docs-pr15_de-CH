<properties
   pageTitle="Veröffentlichung WebApplicationVM | Microsoft Azure"
   description="Weitere Informationen zum Bereitstellen einer Web-Anwendung auf einem virtuellen Computer. Dieses Skript erstellt die erforderlichen Ressourcen in Azure-Abonnement ggf. nicht."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="publish-webapplicationvm-windows-powershell-script"></a>(Windows PowerShell-Skript) veröffentlichen-WebApplicationVM

Stellt eine Web-Anwendung auf einem virtuellen Computer. Das Skript erstellt die erforderlichen Ressourcen in Azure-Abonnement ggf. nicht.

```
Publish-WebApplicationVM
–Configuration <configuration>
-SubscriptionName <subscriptionName>
-WebDeployPackage <packageName>
-VMPassword @{Name = "name"; Password = "password")
-DatabaseServerPassword @{Name = "name"; Password = "password"}
-SendHostMessagesToOutput
-Verbose
```

### <a name="configuration"></a>Konfiguration

Der Pfad zur JSON-Konfigurationsdatei, die die Details der Bereitstellung beschrieben.

|Aliase|keine|
|---|---|
|Erforderlich?|true|
|Position|mit dem Namen|
|Standardwert|keine|
|Annehmen Pipeline-Eingabe?|falsch|
|Annehmen Platzhalterzeichen?|falsch|

### <a name="subscriptionname"></a>SubscriptionName

Der Name der Azure-Abonnement auf den virtuellen Computer erstellt werden soll.

|Aliase|keine|
|---|---|
|Erforderlich?|falsch|
|Position|mit dem Namen|
|Standardwert|Das erste Abonnement verwendet in der Abonnementdatei|
|Annehmen Pipeline-Eingabe?|falsch|
|Annehmen Platzhalterzeichen?|falsch|

### <a name="webdeploypackage"></a>WebDeployPackage

Der Pfad zum Webbereitstellungspaket virtuellen Computer veröffentlichen. Sie können dieses Paket mit dem Veröffentlichen-Assistenten in Visual Studio erstellen. Siehe [wie: erstellen ein Webpakets Bereitstellung in Visual Studio](https://msdn.microsoft.com/library/dd465323.aspx).

|Aliase|keine|
|---|---|
|Erforderlich?|falsch|
|Position|mit dem Namen|
|Standardwert|keine|
|Annehmen Pipeline-Eingabe?|falsch|
|Annehmen Platzhalterzeichen?|falsch|

### <a name="allowuntrusted"></a>AllowUntrusted

Bei true können Sie Zertifikate von einer vertrauenswürdigen Stammzertifizierungsstelle signiert sind.

|Aliase|keine|
|---|---|
|Erforderlich?|falsch|
|Position|mit dem Namen|
|Standardwert|falsch|
|Annehmen Pipeline-Eingabe?|falsch|
|Annehmen Platzhalterzeichen?|falsch|

### <a name="vmpassword"></a>VMPassword

Die Anmeldeinformationen für das Konto des virtuellen Computers. Beispiel: VMPassword - @{Name = "Admin"; Kennwort = "Kennwort"}

|Aliase|keine|
|---|---|
|Erforderlich?|falsch|
|Position|mit dem Namen|
|Standardwert|keine|
|Annehmen Pipeline-Eingabe?|falsch|
|Annehmen Platzhalterzeichen?|falsch|

### <a name="databaseserverpassword"></a>DatabaseServerPassword

Die Anmeldeinformationen für die Datenbank SQL Azure. Beispiel: DatabaseServerPassword - @{Name = "Admin"; Kennwort = "Kennwort"}

|Aliase|keine|
|---|---|
|Erforderlich?|falsch|
|Position|mit dem Namen|
|Standardwert|keine|
|Annehmen Pipeline-Eingabe?|falsch|
|Annehmen Platzhalterzeichen?|falsch|

### <a name="sendhostmessagestooutput"></a>SendHostMessagesToOutput

Wenn true, Nachrichten Drucken aus dem Skript in den Ausgabestream.

|Aliase|keine|
|---|---|
|Erforderlich?|falsch|
|Position|mit dem Namen|
|Standardwert|falsch|
|Annehmen Pipeline-Eingabe?|falsch|
|Annehmen Platzhalterzeichen?|falsch|

## <a name="remarks"></a>Beschreibung

Eine vollständige Erläuterung der Verwendung der Skripts zu Entwicklungs- und Umgebung finden Sie unter [Mit Windows PowerShell Skripts zu veröffentlichen, Entwicklung und Test](vs-azure-tools-publishing-using-powershell-scripts.md).

Die JSON-Konfigurationsdatei gibt Details des bereitgestellt werden. Es enthält Informationen, die Sie beim Erstellen des Projekts wie Name, Gruppe, VHD-Abbild und die Größe des virtuellen Computers angegeben. Außerdem enthält die Endpunkte auf dem virtuellen Computer die Datenbanken bereitstellen, ggf. und Bereitstellungsparameter web. Der folgende Code zeigt eine Beispiel JSON-Konfigurationsdatei:

```
{
    "environmentSettings": {
        "cloudService": {
            "name": "myvmname",
            "affinityGroup": "",
            "location": "West US",
            "virtualNetwork": "",
            "subnet": "",
            "availabilitySet": "",
            "virtualMachine": {
                "name": "myvmname",
                "vhdImage": "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-201404.01-en.us-127GB.vhd",
                "size": "Small",
                "user": "vmuser1",
                "password": "",
                "enableWebDeployExtension": true,
                "endpoints": [
                    {
                        "name": "Http",
                        "protocol": "TCP",
                        "publicPort": "80",
                        "privatePort": "80"
                    },
                    {
                        "name": "Https",
                        "protocol": "TCP",
                        "publicPort": "443",
                        "privatePort": "443"
                    },
                    {
                        "name": "WebDeploy",
                        "protocol": "TCP",
                        "publicPort": "8172",
                        "privatePort": "8172"
                    },
                    {
                        "name": "Remote Desktop",
                        "protocol": "TCP",
                        "publicPort": "3389",
                        "privatePort": "3389"
                    },
                    {
                        "name": "Powershell",
                        "protocol": "TCP",
                        "publicPort": "5986",
                        "privatePort": "5986"
                    }
                ]
            }
        },
        "databases": [
            {
                "connectionStringName": "",
                "databaseName": "",
                "serverName": "",
                "user": "",
                "password": ""
            }
        ],
        "webDeployParameters": {
            "iisWebApplicationName": "Default Web Site"
        }
    }
}
```

Sie können JSON-Konfigurationsdatei ändern, was bereitgestellt wird. Ein virtueller Computer und einem Clouddienst sind erforderlich, Abschnitt Datenbank ist jedoch optional.
