
<properties
    pageTitle="Migrieren von Benutzerdaten von Azure RemoteApp | Microsoft Azure"
    description="Informationen Sie zum Migrieren der Benutzerdaten in Azure RemoteApp."
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />



# <a name="how-to-migrate-data-into-and-out-of-azure-remoteapp"></a>Migrieren von Daten in und aus Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp wird eingestellt. Lesen Sie die [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) für Details.

Viele verschiedene Tools und Methoden können Sie [Daten](remoteapp-upd.md) in und aus Azure RemoteApp übertragen. Hier sind einige Methoden:

- Kopieren und Einfügen verwenden gemeinsame Nutzung der Zwischenablage
- Kopieren von Dateien und Daten auf einem Dateiserver
- Kopieren von Dateien auf OneDrive für Unternehmen über einen browser
- Umleitung Dateien

>[AZURE.NOTE] 
> OneDrive kann nicht für Unternehmen oder Verbraucher Sync-Agents – aktivieren sie Azure RemoteApp [nicht unterstützt](remoteapp-onedrive.md) .

## <a name="use-copy-and-paste-in-file-explorer"></a>Kopieren und Einfügen im Datei-Explorer

Kopieren und Einfügen über die Zwischenablage ist in RemoteApp Bereitstellung [standardmäßig](remoteapp-redirection.md)aktiviert. Dadurch können Benutzer Dateien zwischen ihrer lokalen PC und RemoteApp-apps zu kopieren. Häufig verfügen über RemoteApp apps bei normalen, Benutzer gespeichert Dateien auf ihr Benutzerprofil-Datenträger -, dass Daten aus RemoteApp einfach ist:

1. [Datei-Explorer als app veröffentlichen](remoteapp-publish.md) RemoteApp-Auflistung. (Beachten Sie, dass dies eine Verwaltungsaufgabe ist.)
2. Fordern Sie die Benutzer, Datei-Explorer starten Sie veröffentlicht und mit kopieren und Einfügen von Dateien in ihre UPD und aus.

## <a name="upload-files-and-data-to-a-file-server-by-using-standard-network-file-copy"></a>Hochladen von Dateien und Daten auf einem Dateiserver durch standard-Netzwerk kopieren

Häufig Organisationen verwenden Dateiserver allgemeine Daten speichern. Wenn Sie den Servernamen oder Speicherort kennen, können Benutzer für den Server im lokale Netzwerk durchsuchen und kopieren Sie ihre Dateien, ähnlich wie oben. Erneut sollten Sie Datei-Explorer auf RemoteApp veröffentlichen und dann für die Benutzer freigeben.

>[AZURE.NOTE] 
> Der Dateiserver muss routingfähig Netzwerk, die RemoteApp in bereitgestellt wurde.

## <a name="copy-files-to-onedrive-for-business"></a>Kopieren Sie Dateien auf OneDrive für Unternehmen
Obwohl OneDrive für Business-Sync-Agent im RemoteApp nicht aktivieren, können Sie weiterhin Dateien aus Ihrem UPD auf OneDrive für Unternehmen über einen Browser kopieren. 

1. Veröffentlichen Sie Datei-Explorer auf RemoteApp und teilen Sie Zugriff auf die Dateien über die app. 
2. Am einfachsten Übertragung komprimiert, damit Benutzer eine ZIP-Datei, die alle Dateien erstellen sollten sich auf OneDrive für Unternehmen enthält.
3. Fordern Sie Benutzer zum Office 365-Portal gehen zu OneDrive und Laden Sie die ZIP-Datei.

## <a name="copy-files-by-using-drive-redirection"></a>Dateien Sie vom Laufwerk Umleitung

Wenn [laufwerkumleitung](remoteapp-redirection.md)aktiviert haben, haben bereits ein zugeordnetes Laufwerk für Benutzer erstellt werden. In diesem Fall können sie zip-Dateien auf das umgeleitete Laufwerk und speichern Sie sie auf ihrem lokalen PC.