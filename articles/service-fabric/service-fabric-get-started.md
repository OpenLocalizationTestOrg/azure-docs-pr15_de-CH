<properties
   pageTitle="Einrichten der Umgebung | Microsoft Azure"
   description="Installieren Sie der Runtime SDK und Tools und erstellen Sie lokale Entwicklung Cluster. Nach dieser Installation werden Sie Applikationen erstellt."
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/26/2016"
   ms.author="ryanwi"/>

# <a name="prepare-your-development-environment"></a>Vorbereiten der Umgebung

> [AZURE.SELECTOR]
-[Windows](service-fabric-get-started.md)
- [Linux](service-fabric-get-started-linux.md)
- [OSX](service-fabric-get-started-mac.md)

 Erstellen und Ausführen von [Azure Service Fabric-Anwendung] [ 1] Laufzeit, SDK und Tools auf dem Entwicklungscomputer installieren. Sie müssen Windows PowerShell-Skripts im SDK enthalten können.

## <a name="prerequisites"></a>Erforderliche Komponenten
### <a name="supported-operating-system-versions"></a>Unterstützte Betriebssystemversionen
Für die Entwicklung sind die folgenden Betriebssystemversionen unterstützt:

- Windows 7
- Windows 8 und Windows 8.1
- Windows Server 2012 R2
- Windows 10

>[AZURE.NOTE] Windows 7 enthält nur Windows PowerShell 2.0 standardmäßig. Service Fabric-PowerShell-Cmdlets ist PowerShell 3.0 oder höher erforderlich. Sie können [Windows PowerShell 5.0 herunterladen] [ powershell5-download] aus dem Microsoft Download Center.

## <a name="install-the-runtime-sdk-and-tools"></a>Installieren Sie die Laufzeit, SDK und tools

Web Platform Installer bietet zwei Konfigurationen für Service Fabric-Entwicklung:

- [Installieren Sie Service Fabric-Runtime, SDK und Tools für Visual Studio 2015 (Visual Studio 2015 Update 2 oder höher erforderlich)][full-bundle-vs2015]
- [Installieren Sie Service Fabric-Runtime und nur SDK (ohne Visual Studio-Tools)][core-sdk]

## <a name="enable-powershell-script-execution"></a>Ausführung von PowerShell-Skripten aktivieren

Service Fabric verwendet Windows PowerShell-Skripts zum Erstellen eines Clusters Entwicklung und Anwendung von Visual Studio bereitstellen. Windows blockiert standardmäßig diese Skripts ausgeführt. Um sie zu aktivieren, müssen Sie die PowerShell-Ausführungsrichtlinie ändern. Öffnen Sie PowerShell als Administrator, und geben Sie den folgenden Befehl:

```powershell
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Force -Scope CurrentUser
```

## <a name="next-steps"></a>Nächste Schritte
Starten Sie jetzt, dass Sie Umgebung eingerichtet haben, erstellen und Ausführen von apps.

- [Erstellen Sie Ihre erste Service Fabric-Anwendung in Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md)
- [Informationen Sie zum Bereitstellen und Verwalten von Applikationen auf dem lokalen cluster](service-fabric-get-started-with-a-local-cluster.md)
- [Erfahren Sie mehr über den Programmiermodellen: zuverlässige Dienste und zuverlässige Akteure](service-fabric-choose-framework.md)
- [Service Fabric-Codebeispiele für GitHub Auschecken](https://aka.ms/servicefabricsamples)
- [Visualisieren Sie Cluster mit Service Fabric-Explorer](service-fabric-visualizing-your-cluster.md)
- [Service Fabric Learning Path eine umfassende Einführung in die Plattform zu folgen](https://azure.microsoft.com/documentation/learning-paths/service-fabric/)

[1]: http://azure.microsoft.com/en-us/campaigns/service-fabric/ "Service Fabric Kampagnenseite"
[2]: http://go.microsoft.com/fwlink/?LinkId=517106 "VS-RC"
[full-bundle-vs2015]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015 "2015 WebPI VS-link"
[full-bundle-dev15]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-Dev15 "Dev15 WebPI link"
[core-sdk]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-CoreSDK "Core SDK WebPI link"
[powershell5-download]:https://www.microsoft.com/en-us/download/details.aspx?id=50395
