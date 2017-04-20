
<properties
    pageTitle="Uploaden Sie ein benutzerdefiniertes Bild für Azure RemoteApp | Microsoft Azure"
    description="Erfahren Sie, wie ein benutzerdefiniertes Bild für Azure RemoteApp"
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
    ms.author="ericor" />



# <a name="upload-a-custom-image-for-azure-remoteapp"></a>Ein benutzerdefiniertes Bild für Azure RemoteApp hochladen

> [AZURE.IMPORTANT]
> Azure RemoteApp wird eingestellt. Lesen Sie die [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) für Details.

Jetzt Ihre benutzerdefinierte Vorlagenbild erstellt haben oder aktualisiert es mit müssen Sie Ihre Azure RemoteApp Bildbibliothek, Bild. Gehen Sie folgendermaßen vor.


## <a name="before-you-start"></a>Bevor Sie beginnen

1.      Überprüfen Sie, ob das benutzerdefinierte Abbild [Bild Vorschriften](remoteapp-imagereqs.md) und [Anforderungen](remoteapp-appreqs.md)erfüllt.
2.      [Azure PowerShell-Modul](../powershell-install-configure.md)zu installieren.

## <a name="step-by-step-on-how-to-upload-custom-image"></a>Schritt für Schritt zum benutzerdefinierten Bild

1.      Öffnen Sie Azure-Verwaltungsportal, und navigieren Sie zur Seite RemoteApp.
2.      Klicken Sie auf der Registerkarte **Vorlage Bilder** am unteren Rand der Seite **Hochladen** .
4.      Geben Sie einen Anzeigenamen für das Bild und den Speicherort für das Konto. Sicherstellen Sie, dass die Position am gleichen Speicherort wie die RemoteApp-Sammlung oder einen Speicherort erstellen.
5.      Bei Aufforderung herunterladen Sie das Skript auf Ihren lokalen PC.
6.      Kopieren Sie der Befehlsparameter im Textfeld in die Zwischenablage.
7.      Eine höhere Windows PowerShell-Fenster zu öffnen.
8.      Erweiterte Windows PowerShell-Fenster Navigieren Sie im selben Verzeichnis, in dem das Skript heruntergeladen.
9.      Fügen Sie den kopierten Befehl und drücken Sie die **EINGABETASTE**.

    Der Ladevorgang beginnt und Dauer, viele Faktoren, die Geschwindigkeit und die Bildgröße hängt

11.    Wenn der Upload netzwerkunterbrechung oder Dinge wie fehlschlägt, können Sie immer während des Uploads fortsetzen Sie begann. Um einen Upload fortzufahren, führen Sie das Skript erneut mit derselben Befehlszeile.

> [AZURE.WARNING] Ändern Sie niemals das Upload-Skript. Um sicherzustellen, dass das Bild Bild Vorschriften und Anforderungen erfüllt wurden bestimmte Kontrollen umgesetzt.

## <a name="common-problems"></a>Allgemeine Probleme

- Stellen Sie sicher, dass Windows PowerShell nicht Azure PowerShell verwenden. Sie müssen Azure PowerShell-Modul installiert werden, da bestimmte Module während des Uploads benötigt werden.
- Das Skript verändern, Validierung erleichtern.
- Wenn beim Hochladen die Vhd-Datei gesperrt wird, kopieren Sie die Datei, oder verschieben Sie sie an einen neuen Speicherort und versuchen Upload erneut. Möglicherweise werden einige Windows-Prozess, der Upload verhindert wird.  
