<properties
    pageTitle="App-V-apps mit Azure RemoteApp | Microsoft Azure"
    description="Erfahren Sie mehr über das App-V-apps in Azure RemoteApp verwenden."
    services="remoteapp"
    documentationCenter=""
    authors="ericorman"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016" 
    ms.author="elizapo" />



# <a name="using-app-v-apps-in-azure-remoteapp"></a>Verwendung von App-V-apps in Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp wird eingestellt. Lesen Sie die [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) für Details.

App-V-Anwendung können in eine Azure RemoteApp Hybrid-Sammlung Domänenbeitritt erfordern.

Bevor Sie beginnen, müssen Sie 5.1 App-V-Client die neuesten Updates zu installieren. Sie müssen ein [benutzerdefiniertes Bild](remoteapp-create-custom-image.md) erstellen, den App-V-Client enthält.  

Es ist einfach die App-V-Infrastruktur Azure RemoteApp verwenden. Hybrid-Sammlung in ein Azure-VNET, die auf dem Domänencontroller bereitgestellt und VMs-Domäne beitreten, können Sie Ihre vorhandenen App-V-Infrastruktur und Bereitstellung Methoden zwar Host App-V-Anwendung in Azure RemoteApp nutzen. Hier sind einige Aspekte, die Sie beachten sollten anhand der App-V-Bereitstellung derzeit haben:

| Konfigurationsoptionen |                       | Positive                                                               | Negative                                                                                              |
|-----------------------|-----------------------|------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------|
| Methode       | Streaming (auf Anforderung) | Anwendung ist immer die neueste und frische                                     | Erste Wartezeit                                                                                    |
|                       | Bereitgestellt               | Schnellste; Anwendung ist bereits auf dem virtuellen Computer                              | Aufblasen - nimmt Platz für Bilder (max. 127 GB)                                                           |
| App Speicherort speichern  | Freigegebener Inhalt        | App im Speicher Azure RemoteApp-Instanz ausgeführt wird                         | Frisst, befindet sich die Anwendung Speicher und gute Verbindung zum streaming (Dateiserver)                      |
|                       | Datenträger (zwischengespeichert)         | Schnelle Ausführung. Unabhängig von der Verfügbarkeit der Inhaltsquelle App | Aufblasen - nimmt Platz für Bilder (max. 127 GB)                                                           |
| Zielgruppenadressierung             | Benutzer                  | Erfordert volle eigenständige App-V-Infrastruktur                          |                                                                                                       |
|                       | Global (Computer)      |  Bereits veröffentlicht oder Ziel Veröffentlichung Server                         |  Müssen Sie Ihre Azure aktualisieren (große) Anwendung aktualisiert werden soll. Auf Bild Speicherplatz belegt. |

 Nach dem Erstellen Ihres benutzerdefinierten Abbilds Hybrid-Sammlung veröffentlichen Sie die Anwendung Benutzer zuweisen und Ihre vorhandene App-V-Anwendung in Azure RemoteApp übermittelt jedem Gerät überall genießen.
