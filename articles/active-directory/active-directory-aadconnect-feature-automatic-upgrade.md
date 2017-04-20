<properties
   pageTitle="Azure AD Connect: Automatische Aktualisierung | Microsoft Azure"
   description="Dieses Thema beschreibt die integrierte automatische Aktualisierungsfunktion Azure AD Connect synchronisiert."
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/24/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-automatic-upgrade"></a>Azure AD verbinden: Automatische Aktualisierung
Dieses Feature wurde mit 1.1.105.0 (Februar 2016 veröffentlicht).

## <a name="overview"></a>Übersicht
Sicherstellen, dass Ihre Installation Azure AD Connect immer aktuell war nie mit der Funktion für die **Automatische Aktualisierung** . Dieses Feature ist standardmäßig für express-Installationen und upgrades DirSync. Wenn eine neue Version veröffentlicht wird, wird die Installation automatisch aktualisiert.

Automatische Aktualisierung ist standardmäßig für Folgendes:

- Express-Einstellungen Installation und Dirsync-Upgrades.
- SQL Express LocalDB, ist dem wie Express-Einstellungen immer verwenden. DirSync mit SQL Express LocalDB auch.
- Das Konto ist MSOL_ Standardkonto DirSync mit Express-Einstellungen erstellt.
- Müssen Sie weniger als 100.000 Objekte im Metaverse.

Das PowerShell-Cmdlet der aktuelle Status der automatischen Aktualisierung eingesehen werden `Get-ADSyncAutoUpgrade`. Es hat die folgenden Zustände:

Zustand | Kommentar
---- | ----
Aktiviert | Automatische Aktualisierung aktiviert ist.
Angehalten | Durch das System festgelegt. Das System ist nicht mehr Anspruch auf automatische Aktualisierung.
Deaktiviert | Automatische Aktualisierung deaktiviert.

Sie können zwischen **aktiviert** und **deaktiviert** `Set-ADSyncAutoUpgrade`. Das System sollte der Status **angehalten**festgelegt.

Automatische Aktualisierung verwendet Azure AD Connect Health Upgrade-Infrastruktur. Automatische Aktualisierung funktioniert sicherstellen Sie, dass die URLs in Ihren Proxy-Server für **Azure AD Connect Health** öffnen wie in [Office 365 URLs und IP-Adressbereiche](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2).

Wenn **Synchronisierung Service Manager** Benutzeroberfläche auf dem Server ausgeführt wird, wird die Aktualisierung unterbrochen, bis die Benutzeroberfläche geschlossen wird.

## <a name="troubleshooting"></a>Problembehandlung
Die Connect-Installation nicht selbst aktualisiert erwartungsgemäß, dann gehen Sie folgendermaßen vor um herauszufinden, was falsch sein könnte.

Zunächst erwarten nicht, automatische Upgrade auf den ersten Tag versucht, den eine neue Version freigegeben wird. Es ist beabsichtigt Zufälligkeit vor der Aktualisierung ist also keine Panik, wenn Ihre Installation wird nicht sofort aktualisiert.

Wenn man etwas stimmt nicht, führen Sie dann zuerst `Get-ADSyncAutoUpgrade` sicherstellen, automatische Aktualisierung aktiviert ist.

Vergewissern Sie sich, dass die erforderlichen URLs im Proxy oder Firewall geöffnet haben. Siehe [Übersicht über](#overview)verwendet automatische Updates Azure AD Connect Health. Wenn Sie einen Proxy verwenden, sicherzustellen Sie, dass Gesundheit Verwendung eines [Proxyservers](active-directory-aadconnect-health-agent-install.md#configure-azure-ad-connect-health-agents-to-use-http-proxy)konfiguriert wurde. Testen Sie auch [Health Konnektivität](active-directory-aadconnect-health-agent-install.md#test-connectivity-to-azure-ad-connect-health-service) Azure AD.

Mit der Azure AD überprüft ist es Zeit, sich in die Ereignisprotokolle. Starten der Ereignisanzeige und suchen Sie im Ereignisprotokoll **Anwendung** . Einen Eventlog **Azure AD aktualisieren verbinden** Quelle und Ereignis-Id Bereich **300-399**hinzufügen.  
![EventLog-Filter für die automatische Aktualisierung](./media/active-directory-aadconnect-feature-automatic-upgrade/eventlogfilter.png)  

Sie sehen jetzt die Ereignisprotokolle des Status für die automatische Aktualisierung zugeordnet.  
![EventLog-Filter für die automatische Aktualisierung](./media/active-directory-aadconnect-feature-automatic-upgrade/eventlogresult.png)  

Der Ergebniscode hat ein Präfix mit einem Überblick.

Ergebnis Codepräfix | Beschreibung
--- | ---
Erfolg | Die Installation wurde erfolgreich aktualisiert.
UpgradeAborted | Eine temporäre Bedingung beendet die Aktualisierung. Wird erneut wiederholt werden und es wird erwartet, dass später erfolgreich.
UpgradeNotSupported | Das System hat eine Konfiguration, die blockiert das System automatisch aktualisiert. Es wird wiederholt, um festzustellen, ob der Zustand ändern, aber es wird erwartet, dass das System manuell aktualisiert werden muss.

Hier ist eine Liste der am häufigsten verwendeten Nachrichten finden Sie. Es werden nicht alle aufgelistet, aber die Meldung muss mit dem Problem.

Meldung | Beschreibung
--- | ---
**UpgradeAborted** |
UpgradeAbortedCouldNotSetUpgradeMarker | Die Registrierung konnte nicht geschrieben werden.
UpgradeAbortedInsufficientDatabasePermissions | Der integrierten Administratorgruppe hat keine Berechtigungen für die Datenbank. Manuell aktualisieren Sie auf die neueste Version von Azure AD mit diesem Problem.
UpgradeAbortedInsufficientDiskSpace | Es ist nicht genügend Speicherplatz zum Upgrade unterstützt.
UpgradeAbortedSecurityGroupsNotPresent | Nicht finden und beheben alle Sicherheitsgruppen durch das Synchronisierungsmodul verwendet.
UpgradeAbortedServiceCanNotBeStarted | Der NT-Dienst **Microsoft Azure AD-Synchronisierung** konnte nicht gestartet werden.
UpgradeAbortedServiceCanNotBeStopped | Der NT-Dienst **Microsoft Azure AD-Synchronisierung** konnte nicht beendet werden.
UpgradeAbortedServiceIsNotRunning | Der NT-Dienst **Microsoft Azure AD-Synchronisierung** wird nicht ausgeführt.
UpgradeAbortedSyncCycleDisabled | Die SyncCycle-Option im [Planer](active-directory-aadconnectsync-feature-scheduler.md) wurde deaktiviert.
UpgradeAbortedSyncExeInUse | [-Manager für Synchronisierungsdienst Benutzeroberfläche](active-directory-aadconnectsync-service-manager-ui.md) ist auf dem Server geöffnet.
UpgradeAbortedSyncOrConfigurationInProgress | Der Installations-Assistent ausgeführt wird, oder eine Synchronisierung außerhalb des Planers geplant wurde.
**UpgradeNotSupported** |
UpgradeNotSupportedCustomizedSyncRules | Sie haben benutzerdefinierten Regeln zur Konfiguration hinzugefügt.
UpgradeNotSupportedDeviceWritebackEnabled | Sie haben das [Gerät Rückschreiben](active-directory-aadconnect-feature-device-writeback.md) Feature aktiviert.
UpgradeNotSupportedGroupWritebackEnabled | Sie haben die [gruppenrückschreiben](active-directory-aadconnect-feature-preview.md#group-writeback) -Funktion aktiviert.
UpgradeNotSupportedInvalidPersistedState | Die Installation ist ein Express-Einstellungen oder Dirsync-Aktualisierung.
UpgradeNotSupportedMetaverseSizeExceeeded | Sie haben mehr als 100.000 Objekte im Metaverse.
UpgradeNotSupportedMultiForestSetup | Sie Verbinden mit mehreren Gesamtstrukturen. Express-Setup nur verbunden mit einer Gesamtstruktur.
UpgradeNotSupportedNonLocalDbInstall | Verwenden Sie nicht einer SQL Server Express LocalDB.
UpgradeNotSupportedNonMsolAccount | [Active Directory Connector-Konto](active-directory-aadconnect-accounts-permissions.md#active-directory-account) ist das Standardkonto für MSOL_ nicht mehr.
UpgradeNotSupportedStagingModeEnabled | Der Server ist, der [Modus](active-directory-aadconnectsync-operations.md#staging-mode)festgelegt.
UpgradeNotSupportedUserWritebackEnabled | Sie haben [Benutzer Rückschreiben](active-directory-aadconnect-feature-preview.md#user-writeback) -Funktion aktiviert.

## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie mehr über [die lokalen Identitäten Azure Active Directory integrieren](active-directory-aadconnect.md).
