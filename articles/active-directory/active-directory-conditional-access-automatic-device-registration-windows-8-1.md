<properties
    pageTitle="Automatische Registrierung für Windows 8.1 Domäne beitreten Geräte konfigurieren | Microsoft Azure"
    description=" Schritte zum Konfigurieren von Gruppenrichtlinien für Windows 8.1 Domäne Geräte automatisch mit Azure registrieren. "
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
    ms.author="Markvi"/>

# <a name="configure-automatic-device-registration-for-windows-81-domain-joined-devices"></a>Konfigurieren Sie automatische Registrierung für Windows 8.1 Domäne beitreten Geräte

Ein Active Directory-Gruppenrichtlinie können Sie Ihre Windows 8.1 Domäne beitreten Geräte sich automatisch mit Azure konfigurieren. Konfigurieren von Gruppenrichtlinien müssen Sie mindestens eine verknüpfte Windows Server 2012 R2 oder Windows 8.1 Domänencomputer mit der Gruppenrichtlinien-Verwaltungskonsole installiert. Sobald diese Gruppenrichtlinie der Domäne aktiviert ist, wird jeder Domänenbenutzer, die auf dem Computer protokolliert automatisch und im Hintergrund mit kein in Azure AD registriert. Kann ein Objekt in Azure AD für jeden registrierten Benutzer des physischen Geräts. Unbedingt lesen, und führen Sie die erforderlichen Komponenten von der automatischen Gerät mit Azure Active Directory for Windows Domain-Joined.

>[AZURE.NOTE]
 Aktuelle Informationen zum automatische Registrierung finden Sie unter [Automatische Registrierung des Windows-Domäne einrichten mit Azure Active Directory verbunden](active-directory-conditional-access-automatic-device-registration-setup.md).



## <a name="configure-the-group-policy-for-your-windows-81-domain-joined-devices"></a>Konfigurieren der Gruppenrichtlinie für Windows 8.1 Domäne beitreten Geräte

1. Öffnen Sie Server-Manager, und navigieren Sie zu **Extras** > **Verwaltung**.
2. Navigieren Sie von der Gruppenrichtlinien-Verwaltungskonsole auf den Knoten, der die der Domäne entspricht in der **Automatischen Arbeitsbereich verknüpfen**aktivieren möchten.
3. Maustaste auf **Gruppenrichtlinienobjekte** , und wählen Sie **neu**. Benennen Sie die Gruppenrichtlinienobjekt, z. B. **Automatische Arbeitsbereich verknüpfen**. Klicken Sie auf **OK**.
4. Mit der rechten Maustaste auf Ihr neues Gruppenrichtlinienobjekt, und wählen Sie dann **Bearbeiten**.
5. Navigieren Sie zu **Computerkonfiguration** > **Richtlinien** > **Administrative Vorlagen** > **Windows-Komponenten** > **Arbeitsbereich verknüpfen**.
6. Maustaste automatisch Arbeitsplatz Join-Clientcomputer, und wählen Sie dann **Bearbeiten**.
7. Wählen Sie das Optionsfeld aktiviert, und klicken Sie auf Übernehmen. Klicken Sie auf **OK**.
8. Sie können jetzt an einem Speicherort Ihrer Wahl das Gruppenrichtlinienobjekt verknüpfen. Zum Aktivieren dieser Richtlinie für die Domäne beitreten Windows 8.1 Geräte in Ihrer Organisation verknüpfen Sie die Gruppenrichtlinie mit der Domäne.

## <a name="unregistering-your-windows-81-domain-joined-devices"></a>Aufheben der Registrierung der Domäne Windows 8.1 Geräte verbunden

Registrierung Ihrer Domäne beitreten Windows 8.1-Geräte wie folgt können: die Arbeitsplatz Join Gruppenrichtlinien ändern im vorherigen Abschnitt erstellt. Legen Sie die automatisch Join Client Computern Arbeitsplatzrichtlinie deaktiviert. Dies verhindert, dass neue Geräte automatisch verknüpfen am Arbeitsplatz.

Registrierung der vorhandenen Domäne hinzugefügt Windows 8.1 Computer mithilfe eines der folgenden zwei Optionen:

* Option 1: **Registrierung Windows 8.1 Domäne Gerät PC-Einstellungen**
  1. Navigieren Sie auf dem Gerät Windows 8.1 **PC**Einstellungen > **Netzwerk** > **am Arbeitsplatz**
  2. Wählen Sie **lassen**.
Dieser Vorgang muss für jedes Domänenbenutzer wiederholt werden, der dem Computer angemeldet ist und automatisch Arbeitsplatz.

* Option 2: Registrierung ein Windows 8.1 Domäne verknüpften Gerät mithilfe eines Skripts
    1. Öffnen Sie ein Eingabeaufforderungsfenster auf dem Computer Windows 8.1 und führen Sie den folgenden Befehl aus:` %SystemRoot%\System32\AutoWorkplace.exe leave`
   
Dieser Befehl muss im Kontext der einzelnen Domänenbenutzer angemeldet hat auf dem Computer ausgeführt werden.

##<a name="event-viewer--errors-for-windows-81-domain-joined-devices"></a>Ereignisanzeige & Fehler für Windows 8.1 Domäne Geräte

Das Windows-Ereignisprotokoll auf einem Computer Windows 8.1 zeigt Nachrichten geräteregistrierung. Sie finden die Nachrichten für erfolgreiche und fehlgeschlagene Ereignisse. 

Im Ereignisprotokoll finden Sie im Viewer unter Programme und Dienste **Protokolle** > **Microsoft** > **Windows > Arbeitsplatz Join**.

##<a name="additional-details"></a>Weitere Informationen

Die Gruppenrichtlinien können geplante Tasks auf dem System, das im Kontext des Benutzers ausgeführt und auf Benutzer anmelden ausgelöst. Die Aufgabe registriert automatisch Benutzer und Gerät mit Azure nachdem die Anmeldung abgeschlossen ist. Der geplante Task auf Geräten Aufgabenplanungsbibliothek unter **Microsoft**Windows 8.1 finden > **Windows** > **Arbeitsbereich verknüpfen**. Die Aufgabe ausführen und alle Active Directory-Benutzer, zu registrieren. 

## <a name="additional-topics"></a>Weitere Themen
- [Azure Active Directory Geräteregistrierung-Übersicht](active-directory-conditional-access-device-registration-overview.md)
- [Automatische Anmeldung mit Azure Active Directory für Windows 10 Domäne](active-directory-conditional-access-automatic-device-registration.md)
- [Konfigurieren Sie automatische Registrierung für Windows 7-Domäne beitreten-Geräte](active-directory-conditional-access-automatic-device-registration-windows7.md)

