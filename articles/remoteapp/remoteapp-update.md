<properties
   pageTitle="Aktualisieren Sie Ihre Azure RemoteApp-Auflistung | Microsoft Azure"
   description="Informationen Sie zum Aktualisieren Ihrer Azure RemoteApp-Sammlung"
   services="remoteapp"
   documentationCenter=""
   authors="lizap"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="remoteapp"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="compute"
   ms.date="08/15/2016"
   ms.author="elizapo"/>

# <a name="update-a-collection-in-azure-remoteapp"></a>Aktualisieren einer Auflistung in Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp wird eingestellt. Lesen Sie die [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) für Details.

Es kommt der Zeitpunkt, natürlich Wenn Sie apps oder Bild in Ihrer Sammlung Azure RemoteApp aktualisieren müssen. Verwenden Sie eines der Bilder mit Ihrem Abonnement Azure RemoteApp Cloud oder Hybrid-Auflistung werden alle Updates so einfach können Sie von Azure RemoteApp selbst behandelt.

Verwenden Sie ein benutzerdefiniertes Bild (neu erstellt oder erstellt eine der Bilder), sind Sie jedoch für Bild und apps erhalten bleiben. Möchten Sie das Bild oder die Apps darin aktualisieren, müssen Sie eine neue, aktualisierte Version des Bildes erstellen und Ersetzen Sie das vorhandene Bild in Ihrer Sammlung mit diesem neuen aktualisierten.

Also, wie können wir Ihre Sammlung aktualisieren? Es ist ziemlich einfach:

1. Aktualisieren Sie das Bild, das in der Auflistung verwendet. Anwenden von Patches oder Updates benötigt, und unter einem neuen Namen speichern.
2. [Hochladen](remoteapp-uploadimage.md) oder [Importieren](remoteapp-image-on-azurevm.md) , die RemoteApp Bild.
3. Klicken Sie nun auf der Seite auf **Aktualisieren**.
4. Wählen Sie das neue Bild aus der **Vorlage** .
4. Hier ist das schwierige Teil - müssen Sie entscheiden, wie sich mit anderen Benutzern, die zurzeit eine Anwendung in der Auflistung. Sie haben folgenden Optionen:
    - **Benutzern 60 Minuten nach der Aktualisierung**. Sobald die Aktualisierung abgeschlossen ist, wird Azure RemoteApp eine Meldung zu jedem aktiven Benutzer sagen ihre Arbeit speichern und Abmelden und wieder anmelden. Nach 60 Minuten werden alle aktiven Benutzer nicht abgemeldet automatisch abgemeldet. Benutzer können sofort melden.
    - **Benutzer sofort anmelden**. Sobald die Aktualisierung abgeschlossen ist, melden Sie alle Benutzer automatisch ohne Warnung. Wenn Sie diese Option auswählen, können Benutzer Daten verloren gehen. Allerdings können sie die App sofort wieder herstellen.

1. Klicken Sie auf das Häkchen, um die Aktualisierung zu starten.
