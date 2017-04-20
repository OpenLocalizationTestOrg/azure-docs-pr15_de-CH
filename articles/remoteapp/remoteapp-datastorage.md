
<properties
    pageTitle="Vertrauliche Daten niemals auf benutzerdefinierte Bilder für Azure RemoteApp speichern | Microsoft Azure"
    description="Erfahren Sie mehr über die Richtlinien für das Speichern von Daten in benutzerdefinierte Bilder in Azure RemoteApp"
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


# <a name="never-store-sensitive-data-on-custom-images"></a>Vertrauliche Daten niemals auf benutzerdefinierte Bilder speichern

> [AZURE.IMPORTANT]
> Azure RemoteApp wird eingestellt. Lesen Sie die [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) für Details.

Beim Hosten Ihrer Anwendung in Azure RemoteApp ist der erste Schritt zum Erstellen eines benutzerdefinierten Abbilds. Wir verwenden das benutzerdefinierte Abbild auf VM-Instanzen erstellen, die Ihre apps für Benutzer bereitstellen. Benutzerdefinierte Bild sollte nur Anträge und nie vertrauliche Daten wie SQL-Datenbanken, Personal oder spezielle Dateien wie QuickBooks Firmendateien verloren gehen können. Alle vertrauliche Daten sollten externe Azure RemoteApp auf einem Dateiserver, einem anderen Azure VM oder SQL Azure befinden. Das Bild sollte nur die Bereitstellung der Anwendung, die eine Verbindung mit der Datenquelle und die Daten. Überprüfen Sie [für Azure RemoteApp Bilder](remoteapp-imagereqs.md) für Weitere Informationen. 

Um zu verstehen, warum Daten nicht speichern soll, müssen Sie Azure RemoteApp verstehen. Wenn eine Auflistung wird erstellt oder aktualisiert, im Hintergrund mehrere Clones oder Kopien des Bildes erstellt. Diese VM-Instanzen sind genaue Replikate des benutzerdefinierten Bildes; Wenn Benutzer Programme starten werden sie zu dieser VM Instanzen verbunden. Aber dieselbe Instanz garantiert nicht und sollte keine Rolle spielen, sind nicht persistent. Hosten der Anwendung VM-Instanzen sind nicht persistent und zerstört oder gelöscht werden können z. B. während Auflistung Update basiert. 

Die Auflistung bereitgestellt und Benutzer starten VMs mit Benutzerdaten ist beständig und geschützt, da einem separaten Speicher innerhalb einer virtuellen Festplatte gespeichert wird, nennen wir einen [Benutzer Profil Datenträger (UPD)](remoteapp-upd.md)das Benutzerprofil c:\users\<Userprofile >. Beim Start einer Anwendung das UPD bereitgestellt und wie ein lokales Benutzerprofil vom Betriebssystem behandelt. Weitere Informationen über das [Azure RemoteApp Einstellungen und Benutzerdaten gespeichert](remoteapp-upd.md).

Beispieldaten, die nicht im Bild befinden soll:

- Freigegebene Daten Benutzer zugreifen
- SQL-DB oder QuickBooks-DB
- Alle Daten im D:\

Beispieldaten im Profil in jeder Benutzer UPD kopiert werden können:

- Dateien pro Benutzer
- Skripts, die Benutzer müssen ihre UPD beibehalten

Kernpunkte:

- Nie Speicher vertrauliche Daten, die beim Erstellen eines benutzerdefinierten Bildes im Bild verloren gehen.
- Vertrauliche Daten sollten immer auf einem separaten Dateiserver separate Azure VM in der Cloud und immer externe Hosting-Anwendung in Azure RemoteApp VM-Instanzen befinden. 
- Benutzerdaten werden und weiterhin Benutzer Profil Datenträger (UPD)


