<properties
   pageTitle="Problembehandlung mit Event Tracing | Microsoft Azure"
   description="Die häufigsten Probleme beim Bereitstellen von Diensten auf Microsoft Azure Service."
   services="service-fabric"
   documentationCenter=".net"
   authors="mattrowmsft"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="03/31/2016"
   ms.author="mattrow"/>


# <a name="troubleshoot-common-issues-when-you-deploy-services-on-azure-service-fabric"></a>Behandeln Sie häufige Probleme beim Bereitstellen von Diensten auf Azure Service Fabric

Sie Dienste auf Ihrem Entwicklercomputer arbeiten, wird [Visual Studio debugging Tools](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)einfach zu verwenden. Für remote-Cluster sind [Berichte](service-fabric-view-entities-aggregated-health.md) immer ein guter Anfang. Am einfachsten Zugriff auf diese Berichte sind über PowerShell oder [SFX](service-fabric-visualizing-your-cluster.md). Es wird vorausgesetzt, dass einem Remotecluster Debuggen und grundlegende Kenntnisse der Verwendung dieser Tools.

##<a name="application-crash"></a>Absturz
Die "Partition ist unter Ziel Replikat oder Instanz Count" Bericht ist ein guter Indikator, der der Dienst abstürzt. Um herauszufinden, dauert wo der Dienst abstürzt eine weitere Untersuchung. Wenn Ihr Ebene Dienst wird Dein beste Freund ein gut durchdachtes Spuren festgelegt.  Wir empfehlen für diese Spuren sammeln und eine Lösung wie [Elastische](service-fabric-diagnostic-how-to-use-elasticsearch.md) für anzeigen und Durchsuchen der Spuren [Azure-Diagnose](service-fabric-diagnostics-how-to-setup-wad.md) versuchen.

![SFX Partition Gesundheit](./media/service-fabric-diagnostics-troubleshoot-common-scenarios/crashNewApp.png)

###<a name="during-service-or-actor-initialization"></a>Dienst oder Akteur Initialisierung
Ausnahmen vor der Initialisierung des Diensttyps werden den Prozess zum Absturz verursachen. Für diese Art von Abstürzen zeigen im Anwendungsereignisprotokoll den Fehler von Ihrem Dienst.
Dies sind die häufigsten Ausnahmen sehen, bevor der Dienst initialisiert wird.

***System.IO.FileNotFoundException***

Dieser Fehler wird häufig durch fehlende Assemblyabhängigkeiten. Überprüfen Sie die CopyLocal-Eigenschaft in Visual Studio oder im globalen Assemblycache des Knotens.

***System.Runtime.InteropServices.COMException***
 *in System.Fabric.Interop.NativeRuntime+IFabricRuntime.RegisterStatefulServiceFactory (IntPtr, IFabricStatefulServiceFactory)*
 
 Dies gibt an, dass der registrierte Typ Dienstname nicht das Manifest.

[Azure-Diagnose](service-fabric-diagnostics-how-to-setup-wad.md) kann konfiguriert werden, um das Anwendungsereignisprotokoll für alle Knoten automatisch hochzuladen.

###<a name="runasync-or-onactivateasync"></a>RunAsync() oder OnActivateAsync()
Absturz während der Initialisierung oder Ausführung der registrierten Diensttyp oder Akteur Fall wird die Ausnahme von Azure Service Fabric. Sie können von der Ereignisquelle Anbieter im Abschnitt "Nächste Schritte" anzeigen.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu vorhandenen Diagnose durch Service:

* [Zuverlässige Akteure Diagnose](service-fabric-reliable-actors-diagnostics.md)
* [Zuverlässige Dienste Diagnose](service-fabric-reliable-services-diagnostics.md)
