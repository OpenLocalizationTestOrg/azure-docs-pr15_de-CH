<properties
    pageTitle="Azure AD Connect: Erste Schritte mit express-Standardeinstellungen | Microsoft Azure"
    description="Informationen Sie zum Downloaden, installieren und führen Sie den Assistenten für Azure AD verbinden."
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/13/2016"
    ms.author="billmath"/>

# <a name="getting-started-with-azure-ad-connect-using-express-settings"></a>Erste Schritte mit Azure AD Verbinden mit express-Standardeinstellungen
Azure AD Connect **Express Einstellungen** wird verwendet, wenn eine Topologie mit einer einzelnen Gesamtstruktur und [Synchronisierung von Kennwörtern](../active-directory-aadconnectsync-implement-password-synchronization.md) für die Authentifizierung. **Express** ist die Standardoption und am häufigsten eingesetzte Szenario verwendet. Sie sind nur einige kurze Klicks in Ihrem lokalen Verzeichnis in die Cloud erweitern.

Vor der Installation von Azure AD Connect sicherstellen [Azure AD Connect](http://go.microsoft.com/fwlink/?LinkId=615771) Herunterladen abgeschlossen Voraussetzung Schritte und [Azure AD Connect: Hardware und Komponenten](../active-directory-aadconnect-prerequisites.md).

Wenn express-Einstellungen entspricht nicht die Topologie, finden Sie [Dokumentationen](#related-documentation) für andere Szenarien.

## <a name="express-installation-of-azure-ad-connect"></a>Schnellinstallation Azure AD Connect
Sie können Schritte in Aktion im Abschnitt [Videos](#videos) anzeigen.

1. Melden Sie sich als lokaler Administrator auf dem Server Azure AD Connect auf installieren möchten. Sie sollten auf dem Server dazu werden Synchronisierungsserver möchten.
2. Navigieren Sie, und doppelklicken Sie auf **AzureADConnect.msi**.
3. Auf der Willkommensseite zu den Lizenzierungsinformationen Kontrollkästchen Sie, und klicken Sie auf **Weiter**.  
4. Klicken Sie auf Express-Einstellungsbildschirm **express eingestellt**.  
![Willkommen bei Azure AD Connect](./media/active-directory-aadconnect-get-started-express/express.png)
5. Geben Sie den Benutzernamen und das Kennwort des globalen Administrators auf mit Azure AD-Bildschirm für Azure AD. Klicken Sie auf **Weiter**.  
![Verbinden mit Azure AD](./media/active-directory-aadconnect-get-started-express/connectaad.png) Wenn Sie eine Fehlermeldung und Verbindungsprobleme, sehen Sie [Problembehandlung bei Verbindungsproblemen](../active-directory-aadconnect-troubleshoot-connectivity.md).
6. Mit AD DS Bildschirm Hingehen Sie Benutzername und Kennwort für ein Administratorkonto Enterprise. Sie können den Domänenteil NetBIOS- oder FQDN FABRIKAM\administrator oder fabrikam.com\administrator Format eingeben. Klicken Sie auf **Weiter**.  
![Verbinden mit AD DS](./media/active-directory-aadconnect-get-started-express/connectad.png)
7. [**-In Azure AD**](../active-directory-aadconnect-user-signin.md#azure-ad-sign-in-configuration) Konfigurationsseite zeigt nur wenn [Komponenten](../active-directory-aadconnect-prerequisites.md) [Überprüfen Ihrer Domänen](../active-directory-add-domain.md) nicht abgeschlossen wurde.
![Nicht überprüfte Domänen](./media/active-directory-aadconnect-get-started-express/unverifieddomain.png)  
Wenn diese Seite angezeigt wird, überprüfen Sie jede Domäne **Nicht hinzugefügt** und **Nicht überprüft**gekennzeichnet. Sicherstellen Sie, dass die verwendeten Domänen in Azure AD überprüft wurden. Klicken Sie auf das Symbol aktualisieren Ihre Domänen überprüft haben.
8. Klicken Sie auf **Installieren**, auf dem Bildschirm konfigurieren.
    - Auf der Seite konfigurieren können Sie optional das Kontrollkästchen **den Synchronisierungsprozess als Abschluss Konfiguration starten** aufheben. Sie sollten dieses Kontrollkästchen deaktivieren, wenn Sie zusätzliche Konfiguration wie [Filtern](../active-directory-aadconnectsync-configure-filtering.md)möchten. Wenn Sie diese Option deaktivieren, wird der Assistent Synchronisierung konfiguriert aber bewirkt, dass der Zeitplan deaktiviert. Es läuft nicht manuell durch [den Installationsassistenten erneut](../active-directory-aadconnectsync-installation-wizard.md)aktivieren.
    - Haben Sie Exchange in Ihre lokale Active Directory müssen Sie auch eine Option [**Exchange Hybrid**](https://technet.microsoft.com/library/jj200581.aspx)aktiviert. Aktivieren Sie diese Option, wenn Sie planen, Exchange-Postfächer in der Cloud und lokal gleichzeitig.
![Azure AD-Verbindung konfigurieren](./media/active-directory-aadconnect-get-started-express/readytoconfigure.png)
9. Wenn die Installation abgeschlossen ist, klicken Sie auf **Beenden**.
10. Nach der Installation zum Melden Sie ab und wieder melden Sie-Manager für Synchronisierungsdienst oder Synchronisierung Regel-Editor verwenden an.

## <a name="videos"></a>Videos

Ein Video zum Verwenden der express-Installations finden Sie unter:

>[AZURE.VIDEO azure-active-directory-connect-express-settings]

## <a name="next-steps"></a>Nächste Schritte
Jetzt haben Sie Azure AD Connect installiert können Sie [prüfen und Lizenzen zuweisen](../active-directory-aadconnect-whats-next.md).

Weitere Informationen zu diesen Features, die bei der Installation konnten: [Automatische Aktualisierung](../active-directory-aadconnect-feature-automatic-upgrade.md), [verhindern, dass versehentlich löscht](../active-directory-aadconnectsync-feature-prevent-accidental-deletes.md)und [Azure AD verbinden](../active-directory-aadconnect-health-sync.md).

Weitere Informationen zu diesen Themen: [Planer und zum Synchronisieren auslösen](../active-directory-aadconnectsync-feature-scheduler.md).

Erfahren Sie mehr über [die lokalen Identitäten Azure Active Directory integrieren](../active-directory-aadconnect.md).

## <a name="related-documentation"></a>Zugehörige Dokumentation

Thema |  
--------- | ---------
Azure AD Connect-Übersicht | [Integration von lokalen Identitäten in Azure Active Directory](../active-directory-aadconnect.md)
Mit benutzerdefinierten Einstellungen installieren | [Benutzerdefinierte Installation von Azure AD verbinden](active-directory-aadconnect-get-started-custom.md)
Aktualisieren von DirSync | [Aktualisieren von Azure AD Synchronisierungsprogramm (DirSync)](active-directory-aadconnect-dirsync-upgrade-get-started.md)
Konten für die installation | [Weitere Informationen zu Azure AD Connect Konten und Berechtigungen](active-directory-aadconnect-accounts-permissions.md)
