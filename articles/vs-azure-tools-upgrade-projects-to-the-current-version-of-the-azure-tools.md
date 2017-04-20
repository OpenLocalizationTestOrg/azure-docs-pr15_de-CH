<properties
   pageTitle="Projekte in die aktuelle Version von Azure Tools aktualisieren | Microsoft Azure"
   description="Informationen Sie zum Aktualisieren von Azure-Projekt in Visual Studio auf die aktuelle Version der Azure-tools"
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

# <a name="how-to-upgrade-projects-to-the-current-version-of-the-azure-tools-for-visual-studio"></a>Projekte, die aktuelle Version von Azure Tools für Visual Studio aktualisieren

## <a name="overview"></a>Übersicht

Nach der Installation der aktuellen Version von Azure Tools (oder einer früheren Version neuer ist als 1.6) lassen sich mithilfe von Azure Tools erstellte Projekte vor 1.6 (November 2011) wird automatisch aktualisiert, sobald Sie sie öffnen. Wenn Sie Projekte mit 1,6 (November 2011) Version des Tools erstellt und Sie diese Version installiert noch, können die Projekte in der älteren Version öffnen und später entscheiden, ob sie aktualisieren.

## <a name="how-your-project-changes-when-you-upgrade-it"></a>Wie Projekt ändert, wenn Sie aktualisieren

Wenn ein Projekt aktualisiert wird oder sie aktualisieren möchten, das Projekt mit aktuellen Versionen von bestimmte Assemblys geändert und einige Eigenschaften werden ebenfalls geändert, wie in diesem Abschnitt beschrieben. Wenn das Projekt anderen Änderungen mit der neueren Version der Tools erfordert, müssen Sie diese manuell ändern.

- Die Datei web.config für Webrollen und der App.config.Datei für Workerthreads werden auf die neuere Version von Microsoft.WindowsAzure.Diagnostics.DiagnosticMonitoirTraceListener.dll.

- Assemblys Microsoft.WindowsAzure.StorageClient.dll, Microsoft.WindowsAzure.Diagnostics.dll und Microsoft.WindowsAzure.ServiceRuntime.dll werden auf die neuen Versionen aktualisiert.

- Veröffentlichungsprofile in Azure Projektdatei (.ccproj) gespeichert wurden, werden in eine separate Datei mit der Erweiterung .azurePubXml im Unterverzeichnis **Veröffentlichen** verschoben.

- Einige Eigenschaften im Veröffentlichungsprofil werden aktualisiert, um neue und geänderte Funktionen unterstützen. **AllowUpgrade** wird durch **DeploymentReplacementMethod** ersetzt, da bereitgestellten Clouddienst gleichzeitig oder inkrementell aktualisiert werden kann.

- Die **UseIISExpressByDefault** -Eigenschaft hinzugefügt und auf False festgelegt, sodass der Webserver zum debugging verwendet wird automatisch von Internet Information Services (IIS), IIS Express ändert. IIS Express ist der Standardwebserver für Projekte, die mit den neueren Versionen der Tools erstellt werden.

- Wenn Azure Caching in einer oder mehreren Rollen des Projekts befindet, sind einige Eigenschaften in der Dienstkonfiguration (.cscfg Datei) und die Dienstdefinition (.csdef Datei) beim Aktualisieren eines Projekts geändert. Das Projekt Azure Caching NuGet-Paket verwendet, wird das Projekt auf die neueste Version des Pakets aktualisiert. Öffnen Sie die Datei web.config, und stellen Sie sicher, dass die Client-Konfiguration während des Upgrades korrekt geführt wurde. Wenn Sie Verweise auf Azure Caching Clientassemblys ohne NuGet-Paket hinzugefügt, wird nicht die Assemblys aktualisiert; Sie müssen diese Verweise auf die neuen Versionen manuell aktualisieren.

>[AZURE.IMPORTANT] F#-Projekten müssen Sie Verweise auf Azure Assemblys manuell aktualisieren, damit auf neueren Versionen von Assemblys verweisen.

### <a name="how-to-upgrade-an-azure-project-to-the-current-release"></a>Ein Azure-Projekt auf die aktuelle Version aktualisieren

1. Installieren der aktuellen Version von Azure-Tools in der Installation von Visual Studio für das aktualisierte Projekt verwenden möchten, und öffnen Sie das Projekt, das Sie aktualisieren möchten. Wenn das Projekt mit einer Azure-Tools erstellt wurde freigegeben, bevor 1.6 (November 2011) das Projekt automatisch auf die aktuelle Version aktualisiert. Wenn das Projekt erstellt wurde November 2011 Freigeben dieser Version noch installiert und das Projekt wird in dieser Version geöffnet.

1. Öffnen Sie im Projektmappen-Explorer das Kontextmenü für den Knoten **Eigenschaften Sie**, und wählen Sie die Registerkarte **Anwendung** im Dialogfeld.

    Die Registerkarte **Anwendung** zeigt die Toolversion, die dem Projekt zugeordnet ist. Erscheint die aktuelle Version von Azure Tools wurde das Projekt bereits aktualisiert. Erscheint Installation eine neuere Version der Tools als was die Registerkarte zeigt eine Schaltfläche **Aktualisieren** .

1. Wählen Sie **Aktualisieren** ein Projekts auf die aktuelle Version der Tools zu aktualisieren.

1. Erstellen Sie das Projekt, und beheben Sie alle Fehler, die aus der API Changes. Informationen zum Code für die neue Version ändern finden Sie in der Dokumentation bestimmte API.
