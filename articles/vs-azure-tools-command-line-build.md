<properties
   pageTitle="Erstellen über die Befehlszeile für Azure | Microsoft Azure"
   description="Erstellen über die Befehlszeile für Azure"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="command-line-build-for-azure"></a>Erstellen über die Befehlszeile für Azure

## <a name="overview"></a>Übersicht

Ein Paket für Azure-Bereitstellung können durch Ausführen von MSBuild in der Befehlszeile. Konfigurieren und Definieren von Builds für Debuggen, Staging und Produktion neben den Buildprozess automatisieren.


## <a name="microsoft-build-engine-msbuild"></a>Microsoft Build Engine (MSBuild)

Mithilfe der Microsoft Build Engine (MSBuild) können Sie Produkte im Build Lab-Umgebung erstellen, in dem Visual Studio installiert ist. MSBuild verwendet XML-Format für Projektdateien, die erweiterbar und von Microsoft vollständig unterstützt. In diesem Dateiformat können Sie beschreiben, welche Elemente sein müssen für Plattformen und Konfigurationen erstellt.

Sie können auch MSBuild über die Befehlszeile ausführen, und dieser Ansatz beschrieben. Durch das Festlegen von Eigenschaften über die Befehlszeile können Sie bestimmte Konfigurationen für ein Projekt erstellen. Ebenso können Sie Ziele definieren, die MSBuild-Befehl erstellen. Weitere Informationen zu Befehlszeilenparametern und MSBuild finden Sie unter [MSBuild Command Line Reference](https://msdn.microsoft.com/library/ms164311.aspx).

## <a name="installation"></a>Installation

Wie im folgenden beschrieben, müssen Sie vorher Software und Tools auf dem Buildserver Azure Paket mit MSBuild erstellt werden können:

1. Installieren Sie das.NET Framework 4 oder höher, einschließlich MSBuild.

1. Installieren Sie die [Azure Authoring Tools](http://go.microsoft.com/fwlink/?LinkId=394615) (Suchen Sie nach MicrosoftAzureAuthoringTools x64.msi oder MicrosoftAzureAuthoringTools x86.msi.

1. Installieren der [Azure-Bibliotheken für .NET](http://go.microsoft.com/fwlink/?LinkId=394616) (Suchen Sie nach MicrosoftAzureLibsForNet x64.msi oder MicrosoftAzureLibs x86.msi.

1. Kopieren Sie die Datei Microsoft.WebApplication.targets aus einer Visual Studio-Installation auf einem anderen Computer.

    Die Datei befindet sich im Verzeichnis C:\Program Files (x86) \MSBuild\Microsoft\Visual Studio\v12.0\WebApplications (11.0 für Visual Studio 2012) und sollte im selben Verzeichnis auf dem Buildserver kopieren.

1. [Azure Tools für Visual Studio](http://go.microsoft.com/fwlink/?LinkId=394616)installieren.

    Suchen Sie nach WindowsAzureTools.vs120.exe, Visual Studio 2013 Projekte zu erstellen.

## <a name="msbuild-parameters"></a>MSBuild-Parameter

Die einfachste Möglichkeit zum Erstellen eines Pakets mit MSBuild ausgeführt werden die `/t:Publish` Option. Dieser Befehl erstellt ein Verzeichnis in Bezug auf den Stammordner des Projekts, wie ProjectDir\bin\Configuration\app.publish\. beim Erstellen von Azure-Projekt generiert zwei Dateien, die Paketdatei selbst und der zugehörigen Konfigurationsdatei:

- Project.cspkg

- ServiceConfiguration.TargetProfile.cscfg

Standardmäßig enthält jede Azure-Projekt ein Service-Konfigurationsdatei für lokale (Debuggen) erstellt und weitere Builds Cloud (Staging oder Produktion), aber Sie können Dateien hinzufügen oder entfernen Konfiguration nach Bedarf. Beim Erstellen eines Pakets in Visual Studio werden die Service-Konfigurationsdatei neben das Paket aufgefordert. Beim Erstellen eines Pakets mit MSBuild ist standardmäßig die lokale Konfiguration Datei enthalten. Um eine andere Konfiguration Datei einzufügen, legen die `TargetProfile` -Eigenschaft des MSBuild-Befehls (`MSBuild /t:Publish /p:TargetProfile=ProfileName`).

Soll einem anderen Verzeichnis gespeicherte Paket und Konfigurationsdateien, legen Sie den Pfad mit der `/p:PublishDir=Directory\` Option, darunter nachgestellten umgekehrten Schrägstrich Trennzeichen.

## <a name="deployment"></a>Bereitstellung

Nachdem das Paket erstellt wurde, können Sie sie in Azure bereitstellen. Ein Lernprogramm, das dieser Prozess veranschaulicht, finden Sie in der Azure-Website. Informationen zum Automatisieren dieses Prozesses finden Sie unter [Kontinuierlichen Bereitstellung Cloud-Dienste in Azure](./cloud-services/cloud-services-dotnet-continuous-delivery.md).
