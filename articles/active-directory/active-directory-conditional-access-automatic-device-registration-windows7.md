<properties
    pageTitle="# Automatische Registrierung für Windows 7-Domäne beitreten-Geräte konfigurieren | Microsoft Azure"
    description="Schritte zum Konfigurieren der Windows 7-Domäne angeschlossen Geräte automatisch mit Azure registrieren. und Schritte das Softwarepaket Gerät Registrierung Ihrer Domäne Windows 7 Bereitstellen mit Softwareverteilungssystem wie System Center Configuration Manager."
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/21/2016"
    ms.author="MarkVi"/>

# <a name="configure-automatic-device-registration-for-windows-7-domain-joined-devices"></a>Konfigurieren Sie automatische Registrierung für Windows 7-Domäne beitreten-Geräte

Als IT-Administrator können Sie Ihre Windows 7-Domäne beitreten Geräte sich automatisch mit Azure konfigurieren. Hierzu das Softwarepaket Gerät Registrierung Ihrer Domäne Windows 7 bereitstellen müssen verknüpft mit Softwareverteilungssystem wie System Center Configuration Manager. Unbedingt lesen und die erforderlichen Komponenten vollständig automatische Registrierung des Geräts mit Azure Active Directory für Windows-Domäne aufgeführt.

>[AZURE.NOTE]
 Aktuelle Informationen zum automatische Registrierung finden Sie unter [Automatische Registrierung des Windows-Domäne einrichten mit Azure Active Directory verbunden](active-directory-conditional-access-automatic-device-registration-setup.md).

##<a name="installing-the-device-registration-software-package-on-windows-7-domain-joined-devices"></a>Das Gerät Registrierung Softwarepaket auf Windows 7 installieren beitreten Geräte Domäne

Gerät erfolgt für Windows 7 als [herunterladbares MSI-Paket](https://connect.microsoft.com/site1164)verfügbar. Das Paket muss auf Windows 7-Computern installiert, die Active Directory-Domäne angehören. Sie sollten das Paket Softwareverteilungssystem wie System Center Configuration Manager bereitstellen. Das MSI-Paket unterstützt die standardmäßigen Hintergrundinstallation Optionen/quiet Parameter.
Das Softwarepaket steht zum Download auf der [Microsoft Connect-Website](https://connect.microsoft.com/site1164). Hier können Sie auswählen und dann Arbeitsplatz Join für Windows 7 herunterladen.

![](./media/active-directory-conditional-access/device-registration-process-windows7.gif)

## <a name="workplace-join-with-azure-active-directory"></a>Arbeitsplatz Join mit Azure Active Directory
Geräteregistrierung für Windows 7-Domäne beitreten-Geräte erfordern weder Benutzeroberfläche enthalten. Nach der Installation auf dem Computer wird jeder Domänenbenutzer, die auf dem Computer protokolliert automatisch und im Hintergrund mit einem Geräteobjekt in Azure AD registriert. Kann ein Objekt in Azure AD für jeden registrierten Benutzer des physischen Geräts.

Das Installationsprogramm erstellt einen geplanten Task auf dem System, das im Kontext des Benutzers ausgeführt und auf Benutzer anmelden ausgelöst. Die Aufgabe den Benutzer automatisch registriert und Azure AD nach Benutzer Zeichen-Gerät ist abgeschlossen.
Der geplante Task finden Sie in der Aufgabenplanungsbibliothek unter **Microsoft** > **Arbeitsbereich verknüpfen**.
Die Aufgabe ausführen und registrieren alle Active Directory-Benutzer, die dem Computer anmelden.
Die folgende Abbildung enthält schrittweise für die automatische Registrierung.

![](./media/active-directory-conditional-access/automatic-device-registration-windows7.png)

1. Ein Benutzer (Mitarbeiter) anmeldet, ein Windows 7-Clientcomputer mit Anmeldeinformationen für Active Directory-Domäne.
1. Der Task Arbeitsplatz Join wird ausgeführt.
1. Der Benutzer wird mit AD FS mithilfe der integrierten Windows-Authentifizierung automatisch authentifiziert.
1. Windows 7-PC ist für den Benutzer in Azure AD registriert.
1. Ein Objekt und das Zertifikat wird in Azure AD erstellt. Das Objekt stellt die user@device.
1. Das Arbeitsbereich beitreten Zertifikat wird auf dem Computer gespeichert.

## <a name="unregistering-your-windows-7-domain-joined-devices"></a>Aufheben der Registrierung der Windows 7-Domäne angeschlossen Geräte

Registrierung der Domäne beitreten Windows 7-Geräte wie folgt können: Deinstallieren der Arbeitsplatz Join Softwarepaket aus Ihrer Domäne mit Windows 7 angeschlossen Geräte Softwareverteilungssystem wie System Center Configuration Manager.

Dann öffnen Sie ein Eingabeaufforderungsfenster auf dem Windows 7-Computer und führen Sie folgenden Befehl des Geräts aufgehoben werden:

    %ProgramFiles%\Microsoft Workplace Join\AutoWorkplace.exe /leave

>[AZURE.NOTE]
>Dieser Befehl muss im Kontext der einzelnen Domänenbenutzer angemeldet hat auf dem Computer ausgeführt werden.
Ereignisanzeige und Windows 7 Fehler Domäne Geräte.

Das Windows-Ereignisprotokoll auf dem Computer Windows 7 zeigt Nachrichten Arbeitsplatz an. Erfolgreiche und fehlgeschlagene Ereignisse auf Arbeitsplatz Join können Sie Nachrichten suchen. Im Ereignisprotokoll finden Sie im Viewer Applikationen und Dienstprotokolle > Microsoft Arbeitsbereich verknüpfen.

## <a name="additional-topics"></a>Weitere Themen

- [Azure Active Directory Geräteregistrierung-Übersicht](active-directory-conditional-access-device-registration-overview.md)
- [Automatische Anmeldung mit Azure Active Directory for Windows Domain-Joined](active-directory-conditional-access-automatic-device-registration.md)
- [Konfigurieren Sie automatische Registrierung für Windows 8.1 Domäne beitreten Geräte](active-directory-conditional-access-automatic-device-registration-windows-8-1.md)
- [Automatische Anmeldung mit Azure Active Directory für Windows 10 Domäne](active-directory-azureadjoin-devices-group-policy.md)
