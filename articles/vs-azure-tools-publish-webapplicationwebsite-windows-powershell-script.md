<properties
   pageTitle="(Windows PowerShell-Skript) veröffentlichen-WebApplicationWebSite | Microsoft Azure"
   description="Erfahren Sie, wie ein Webprojekt eine Azure-Website veröffentlichen. Dieses Skript erstellt die erforderlichen Ressourcen in Azure-Abonnement ggf. nicht."
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

# <a name="publish-webapplicationwebsite-windows-powershell-script"></a>(Windows PowerShell-Skript) veröffentlichen-WebApplicationWebSite

##<a name="syntax"></a>Syntax

Ein Webprojekt veröffentlicht eine Azure-Website. Das Skript erstellt die erforderlichen Ressourcen in Azure-Abonnement ggf. nicht.

    Publish-WebApplicationWebSite
    –Configuration <configuration>
    -SubscriptionName <subscriptionName>
    -WebDeployPackage <packageName>
    -DatabaseServerPassword @{Name = "name"; Password = "password"}
    -SendHostMessagesToOutput
    -Verbose


## <a name="configuration"></a>Konfiguration

Der Pfad zur JSON-Konfigurationsdatei, die die Details der Bereitstellung beschrieben.

|Parameter|Standardwert|
|---|---|
|Aliase|keine|
|Erforderlich?|true|
|Position|mit dem Namen|
|Standardwert|keine|
|Annehmen Pipeline-Eingabe?|falsch|
|Annehmen Platzhalterzeichen?|falsch|

## <a name="subscriptionname"></a>SubscriptionName

Der Name der Azure-Abonnement, das die Website erstellen möchten.

|Parameter|Standardwert|
|---|---|
|Aliase|keine|
|Erforderlich?|falsch|
|Position|mit dem Namen|
|Standardwert|keine|
|Annehmen Pipeline-Eingabe?|falsch|
|Annehmen Platzhalterzeichen?|falsch|

## <a name="webdeploypackage"></a>WebDeployPackage

Der Pfad zu dem Webpaket Bereitstellung auf der Website veröffentlichen. Sie können dieses Paket mit dem Veröffentlichen-Assistenten in Visual Studio erstellen. Weitere Informationen finden Sie unter [Erste Schritte mit Azure Cloud Services und ASP.NET](http://go.microsoft.com/fwlink/p/?LinkID=623089).

|Parameter|Standardwert|
|---|---|
|Aliase|keine|
|Erforderlich?|falsch|
|Position|mit dem Namen|
|Standardwert|keine|
|Annehmen Pipeline-Eingabe?|falsch|
|Annehmen Platzhalterzeichen?|falsch|

## <a name="databaseserverpassword"></a>DatabaseServerPassword

Benutzername und Kennwort für die Datenbank SQL Azure.

|Parameter|Standardwert|
|---|---|
|Aliase|keine|
|Erforderlich?|falsch|
|Position|mit dem Namen|
|Standardwert|keine|
|Annehmen Pipeline-Eingabe?|falsch|
|Annehmen Platzhalterzeichen?|falsch|

## <a name="sendhostmessagestooutput"></a>SendHostMessagesToOutput

Wenn true, Nachrichten Drucken aus dem Skript in den Ausgabestream.

|Parameter|Standardwert|
|---|---|
|Aliase|keine|
|Erforderlich?|falsch|
|Position|mit dem Namen|
|Standardwert|falsch|
|Annehmen Pipeline-Eingabe?|falsch|
|Annehmen Platzhalterzeichen?|falsch|

## <a name="remarks"></a>Beschreibung

Eine vollständige Erläuterung der Verwendung der Skripts zu Entwicklungs- und Umgebung finden Sie unter [Mit Windows PowerShell Skripts zu veröffentlichen, Entwicklung und Test](vs-azure-tools-publishing-using-powershell-scripts.md).

Die JSON-Konfigurationsdatei gibt Details des bereitgestellt werden. Es enthält Informationen, die Sie beim Erstellen des Projekts wie Name und Benutzername für die Website angegeben. Darüber hinaus die Datenbank bereitgestellt, sofern vorhanden. Der folgende Code zeigt eine Beispiel JSON-Konfigurationsdatei:

    {
        "environmentSettings": {
            "webSite": {
                "name": "WebApplication10554",
                "location": "West US"
            },
            "databases": [
                {
                    "connectionStringName": "DefaultConnection",
                    "databaseName": "WebApplication10554_db",
                    "serverName": "iss00brc88",
                    "user": "sqluser2",
                    "password": "",
                    "edition": "",
                    "size": "",
                    "collation": "",
                    "location": "West US"
                }
            ]
        }
    }

Sie können JSON-Konfigurationsdatei ändern, was bereitgestellt wird. WebSite-Bereich ist erforderlich, Abschnitt Datenbank ist jedoch optional.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter [Veröffentlichen-WebApplicationVM (Windows PowerShell-Skript)](vs-azure-tools-publish-webapplicationvm.md)
