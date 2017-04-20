<properties
    pageTitle="Problembehandlung bei Anwendungsproxy | Microsoft Azure"
    description="Erläutert die Problembehandlung in Azure AD-Anwendungsproxy."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/19/2016"
    ms.author="kgremban"/>



# <a name="troubleshoot-application-proxy"></a>Problembehandlung bei Anwendungsproxy

Treten Fehler beim Zugriff auf eine veröffentlichte Anwendung oder veröffentlichen, überprüfen Sie die folgenden Optionen auf Microsoft Azure AD-Anwendungsproxy funktionsfähig ist:

- Öffnen Sie Windows-Dienste und überprüfen, ob **Microsoft AAD Application Proxy Connector** -Dienst aktiviert ist und ausgeführt. Auch empfiehlt sich der Anwendungsproxy Eigenschaftenseite wie in der folgenden Abbildung dargestellt:  
  ![Microsoft AAD Proxy Connector Anwendungseigenschaften Fenster screenshot](./media/active-directory-application-proxy-troubleshoot/connectorproperties.png)

- Ereignisanzeige öffnen und nach Anwendungsproxy Connector Ereignisse und **Dienstprotokolle** > **Microsoft** > **AadApplicationProxy** > **Connector** > **Admin**.
- Bei Bedarf werden detailliertere Protokolle Analytics Debuggen Protokolle und Anwendungsproxy Connector Sitzungsprotokoll einschalten einschalten.

## <a name="the-page-is-not-rendered-correctly"></a>Die Seite wird nicht korrekt gerendert.

Wenn Sie keine Fehlermeldung erhalten, müssen Sie weiterhin Probleme mit Ihrer Anwendung rendern oder funktioniert nicht ordnungsgemäß. Dies kann auftreten, wenn Sie Artikel Pfad veröffentlicht, aber die Anwendung erfordert Inhalte außerhalb dieser Pfad.

Beispielsweise wird nicht wenn Pfad https://yourapp/app veröffentlichen die Anwendung ruft Bilder in https://yourapp/media, sie gerendert werden. Stellen Sie sicher, dass die Anwendung mit der höchsten Ebene Pfad, den Sie alle relevanten Inhalte müssen veröffentlichen. In diesem Beispiel wäre es http://yourapp/.

Ändern Sie den Pfad, um verwiesen wird, sondern es benötigen Benutzer auf eine tiefere Verbindung im Pfad, finden Sie im Blogbeitrag [den richtigen Link für Anwendungsproxy Azure AD-Abdeckung und Office 365-Startprogramm festlegen](https://blogs.technet.microsoft.com/applicationproxyblog/2016/04/06/setting-the-right-link-for-application-proxy-applications-in-the-azure-ad-access-panel-and-office-365-app-launcher/).

## <a name="general-errors"></a>Allgemeine Fehler

Fehler | Beschreibung | Auflösung
--- | --- | ---
Diese Unternehmen app kann nicht zugegriffen werden. Sie sind nicht berechtigt diese Anwendung zugreifen. Autorisierung fehlgeschlagen. Achten Sie darauf, dass Benutzer mit Zugriff auf diese Anwendung zuweisen. | Sie können nicht den Benutzer für diese Anwendung zugewiesen. | Finden Sie auf der Registerkarte **Anwendung** , **Benutzer und Gruppen**, weisen Sie Benutzer oder Benutzergruppen dieser Anwendung zu.
Diese Unternehmen app kann nicht zugegriffen werden. Sie sind nicht berechtigt diese Anwendung zugreifen. Autorisierung fehlgeschlagen. Stellen Sie sicher, dass der Benutzer eine Lizenz für Azure Active Directory Premium oder Basic. | Benutzer können diese Fehlermeldung beim Versuch, die Anwendung veröffentlicht, wenn sie explizit eine Premium-Basic Lizenz vom Administrator des Abonnenten zugeordnet waren nicht zugreifen. | Wechseln Sie auf dem Abonnenten Active Directory **Lizenzen** Registerkarte und stellen Sie sicher, dass diese Benutzer oder eine Benutzergruppe ein Premium oder Basic Lizenz zugeordnet ist.


## <a name="connector-troubleshooting"></a>Verbindung zu Problembehandlung
Wenn Registrierung während der Installation der Connector-Assistenten fehlschlägt, zweierlei um den Grund für den Fehler. Suchen Sie im Ereignisprotokoll unter **Programme und Dienste Logs\Microsoft\AadApplicationProxy\Connector\Admin**oder den folgenden Windows PowerShell-Befehl.

    Get-EventLog application –source “Microsoft AAD Application Proxy Connector” –EntryType “Error” –Newest 1

| Fehler | Beschreibung | Auflösung |
| --- | --- | --- |
| Fehler beim Registrieren der Connector: sicher aktiviert Anwendungsproxy in Azure-Verwaltungsportal und Ihre Active Directory-Benutzernamen und das Kennwort richtig eingegeben haben. Fehler: "ein oder mehrere Fehler aufgetreten." | Das Anmeldefenster wird ohne Anmeldung bei Azure AD geschlossen. | Führen Sie Connector-Assistenten erneut aus, und registrieren Sie den Connector. |
| Fehler beim Registrieren der Connector: sicher aktiviert Anwendungsproxy in Azure-Verwaltungsportal und Ihre Active Directory-Benutzernamen und das Kennwort richtig eingegeben haben. Fehler: "AADSTS50001: Resource `https://proxy.cloudwebappproxy.net /registerapp` ist deaktiviert." | Anwendungsproxy ist deaktiviert.  | Stellen Sie sicher, dass Anwendungsproxy im klassischen Azure-Portal aktivieren, bevor Sie den Connector registrieren möchten. Weitere Informationen zum Aktivieren der Anwendungsproxy anzeigen Sie [Anwendungsproxy aktivieren services](active-directory-application-proxy-enable.md) |
| Fehler beim Registrieren der Connector: sicher aktiviert Anwendungsproxy in Azure-Verwaltungsportal und Ihre Active Directory-Benutzernamen und das Kennwort richtig eingegeben haben. Fehler: "ein oder mehrere Fehler aufgetreten." | Wenn Registrierungsfenster öffnet und dann sofort schließt ohne Sie anmelden, erhalten Sie möglicherweise dieser Fehler. Dieser Fehler tritt auf, wenn auf Ihrem System ein Netzwerkfehler vorliegt. | Sicher ist es möglich, über einen Browser auf einer öffentlichen Website und die Anschlüsse gemäß [Anwendungsproxy Komponenten](active-directory-application-proxy-enable.md)geöffnet sind. |
| Fehler beim Registrieren der Connector: Stellen Sie sicher, dass Ihr Computer mit dem Internet verbunden. Fehler: "Es wurde kein Endpunkt abgehört `https://connector.msappproxy.net :9090/register/RegisterConnector` konnte die Nachricht akzeptieren. Dies wird häufig durch eine fehlerhafte Adresse oder SOAP-Aktion verursacht. InnerException, falls vorhanden, Weitere Einzelheiten siehe. " | Wenn Sie diese Fehlermeldung mit Azure AD Benutzernamen und Kennwort anmelden, kann es sein, dass alle Ports über 8081 blockiert werden. | Stellen Sie sicher, dass die erforderlichen Ports geöffnet sind. Weitere Informationen finden Sie unter [Anwendungsproxy erforderliche Komponenten](active-directory-application-proxy-enable.md). |
| Fehlerbeseitigung wird im Registrierungsfenster angezeigt. Kann nicht fortgesetzt werden nur zum Schließen des Fensters –. | Das eingegebene falscher Benutzername oder Kennwort. | Wiederholen. |
| Fehler beim Registrieren der Connector: sicher aktiviert Anwendungsproxy in Azure-Verwaltungsportal und Ihre Active Directory-Benutzernamen und das Kennwort richtig eingegeben haben. Fehler: "AADSTS50059: keine Mieter Informationen entweder in der Anforderung gefunden oder impliziert bereitgestellten Anmeldeinformationen und Suche nach Service-Prinzip URI ist fehlgeschlagen. | Sie versuchen, melden Sie sich mit Microsoft Account und nicht zu einer Domäne, die die Organisations-ID des Verzeichnisses gehört Sie zugreifen wollen. | Sicherstellen, dass der Administrator Teil Domänennamen der Tenant-Domäne, z. B. ist die Azure AD-Domäne contoso.com, der Administrator sollte admin@contoso.com. |
| Fehler beim Abrufen der aktuelle Ausführungsrichtlinie für PowerShell-Skripts ausgeführt. | Schlägt die Installation des Connectors überprüfen Sie um sicherzustellen, dass PowerShell-Ausführungsrichtlinie nicht deaktiviert ist. | Öffnen Sie den Gruppenrichtlinien-Editor. **Computerkonfiguration**zur > **Administrative Vorlagen** > **Windows-Komponenten** > **Windows PowerShell** und doppelklicken Sie auf **Skript ausführen aktivieren**. Dies kann entweder **Nicht konfiguriert** oder **aktiviert**festgelegt werden. Wenn **aktiviert**, stellen Sie sicher, dass unter Optionen die **lokale Skripts zulassen und remote signierte Skripts** oder **Alle Skripts**festgelegt ist. |
| Connector konnte die Konfiguration laden. | Der Connector-Clientzertifikat, das für die Authentifizierung verwendet wird, ist abgelaufen. Dies kann auftreten, wenn den Connector hinter einem Proxy installiert haben. In diesem Fall wird der Connector keinen Zugang zum Internet und werden keine Anträge für Remotebenutzer bereitstellen. | Erneuern Vertrauensstellung manuell mithilfe der `Register-AppProxyConnector` in Windows PowerShell-Cmdlet. Wenn der Connector hinter einem Proxyserver befindet, ist es erforderlich, Internetzugriff auf Connector "Netzwerkdienste" und "Lokales System" Dies können Sie durch Gewähren von Zugriff auf den Proxy oder indem sie den Proxyserver umgehen. |
| Fehler beim Registrieren der Connector: sicherstellen, dass Sie ein globaler Administrator von Active Directory den Connector registriert sind. Fehler: "die Anfrage wurde verweigert." | Für die Anmeldung gewünschte Alias nicht Administrator für diese Domäne. Der Connector wird immer für das Verzeichnis installiert, das die Domäne besitzt. | Stellen Sie sicher, dass der Administrator anmelden als globale Berechtigungen für Azure AD-Mandanten.|


## <a name="kerberos-errors"></a>Kerberos-Fehler

| Fehler | Beschreibung | Auflösung |
| --- | --- | --- |
| Fehler beim Abrufen der aktuelle Ausführungsrichtlinie für PowerShell-Skripts ausgeführt. | Schlägt die Installation des Connectors überprüfen Sie um sicherzustellen, dass PowerShell-Ausführungsrichtlinie nicht deaktiviert ist. | Öffnen Sie den Gruppenrichtlinien-Editor. **Computerkonfiguration**zur > **Administrative Vorlagen** > **Windows-Komponenten** > **Windows PowerShell** und doppelklicken Sie auf **Skript ausführen aktivieren**. Dies kann entweder **Nicht konfiguriert** oder **aktiviert**festgelegt werden. Wenn **aktiviert**, stellen Sie sicher, dass unter Optionen die **lokale Skripts zulassen und remote signierte Skripts** oder **Alle Skripts**festgelegt ist. |
| 12008 - Azure AD überschritten maximal zulässige Kerberos Authentifizierungsversuchen Back-End-Server. | Dieses Ereignis möglicherweise falsche Konfiguration zwischen Azure und Back-End-Anwendungsserver oder kein Datum und Uhrzeit Konfiguration auf beiden Computern. | Der Backend-Server abgelehnt das Kerberos-Ticket von Azure AD erstellt. Stellen Sie sicher, dass Azure AD und Back-End-Anwendungsserver ordnungsgemäß konfiguriert sind. Stellen Sie sicher, dass Datum und Uhrzeit Konfiguration auf Azure AD und der Back-End-Anwendungsserver synchronisiert. |
| 13016 - Azure AD kann kein Kerberos-Ticket für den Benutzer abgerufen werden, da kein UPN im Rand Token Cookie oder in Access ist. | Es ist ein Problem mit der STS-Konfiguration. | Beheben Sie UPN-Anspruch-Konfiguration in der STS. |
| 13019 - die folgenden allgemeinen API-Fehler beim Abrufen eines Kerberos-Tickets für den Benutzer von Azure AD. | Dieses Ereignis möglicherweise falsche Konfiguration zwischen Azure und Domain Controller-Servers oder kein Datum und Uhrzeit Konfiguration auf beiden Computern. | Der Domänencontroller hat das Kerberos-Ticket erstellt von Azure AD abgelehnt. Vergewissern Sie sich, Azure AD und der Back-End-Anwendungsserver konfiguriert, insbesondere die SPN-Konfiguration. Sicherstellen Sie das Azure AD Domäne Mitglied derselben Domäne wie Domänencontroller, um sicherzustellen, dass der Domänencontroller mit Azure AD richtet. Stellen Sie sicher, dass Datum und Uhrzeit Konfiguration auf Azure AD und der Domänencontroller synchronisiert. |
| 13020 - Azure AD kann kein Kerberos-Ticket für den Benutzer abgerufen werden, da der Datenbankserver SPN nicht definiert ist. | Dieses Ereignis möglicherweise falsche Konfiguration zwischen Azure und Domain Controller-Servers oder kein Datum und Uhrzeit Konfiguration auf beiden Computern. | Der Domänencontroller hat das Kerberos-Ticket erstellt von Azure AD abgelehnt. Vergewissern Sie sich, Azure AD und der Back-End-Anwendungsserver konfiguriert, insbesondere die SPN-Konfiguration. Sicherstellen Sie das Azure AD Domäne Mitglied derselben Domäne wie Domänencontroller, um sicherzustellen, dass der Domänencontroller mit Azure AD richtet. Stellen Sie sicher, dass Datum und Uhrzeit Konfiguration auf Azure AD und der Domänencontroller synchronisiert. |
| 13022 - Azure AD kann nicht den Benutzer authentifizieren, weil der Back-End-Server Kerberos-Authentifizierung versucht mit einem HTTP-Fehler 401 beantwortet. | Dieses Ereignis möglicherweise falsche Konfiguration zwischen Azure und Back-End-Anwendungsserver oder kein Datum und Uhrzeit Konfiguration auf beiden Computern. | Der Backend-Server abgelehnt das Kerberos-Ticket von Azure AD erstellt. Stellen Sie sicher, dass Azure AD und Back-End-Anwendungsserver ordnungsgemäß konfiguriert sind. Stellen Sie sicher, dass Datum und Uhrzeit Konfiguration auf Azure AD und der Back-End-Anwendungsserver synchronisiert. |
| Die Website kann die Seite nicht anzeigen. | Benutzer kann diese Fehlermeldung beim Versuch, die Anwendung Zugriff auf veröffentlichte, wenn die Anwendung eine Anwendung IWA. Definierte SPN für diese Anwendung könnte fehlerhaft sein. | IWA Apps: sicherstellen, dass für diese Anwendung konfigurierten SPN korrekt ist. |
| Die Website kann die Seite nicht anzeigen. | Benutzer kann diese Fehlermeldung beim Versuch, die Anwendung Zugriff auf veröffentlichte, wenn die Anwendung eine OWA-Anwendung. Dies kann folgende Ursachen haben: <br> -Definierte SPN für diese Anwendung ist falsch. <br> -Die Benutzer, die Zugriff auf die Anwendung Microsoft-Konto anstatt der richtigen corporate-Konto anmelden, oder der Benutzer ist Gast. <br> -Die Benutzer, die Zugriff auf die Anwendung ist für diese Anwendung auf auf Prem nicht ordnungsgemäß definiert. | Die Schritte zum entsprechend zu verringern: <br> -Stellen Sie sicher, dass für diese Anwendung konfigurierten SPN korrekt ist. <br> -Stellen Sie sicher, dass meldet den Benutzer mit ihrer corporate-Konto, die Domäne der veröffentlichten Anwendung entspricht. Microsoft Account Benutzer und Gast können nicht IWA Applikationen zugreifen. <br> -Stellen Sie sicher, dass dieser Benutzer die entsprechenden Berechtigungen verfügt, Back-End-Anwendung auf dem Computer auf Prem definiert. |
| Diese Unternehmen app kann nicht zugegriffen werden. Sie sind nicht berechtigt diese Anwendung zugreifen. Autorisierung fehlgeschlagen. Achten Sie darauf, dass Benutzer mit Zugriff auf diese Anwendung zuweisen. | Benutzer können diese Fehlermeldung beim Versuch, die Anwendung Zugriff auf veröffentlichte, verwenden sie Microsoft-Konten anstelle ihrer corporate-Konto anmelden. Gastbenutzer können dieser Fehler auch angezeigt. | Microsoft Account Benutzer und Gäste können nicht IWA Applikationen zugreifen. Vergewissern Sie sich mithilfe ihrer corporate-Konto, die Domäne der veröffentlichten Anwendung entspricht der Benutzer anmeldet. |
| Diese Unternehmen app kann jetzt zugegriffen werden. Versuchen Sie es später erneut... Die Verbindung hat das Zeitlimit überschritten. | Die Benutzer können diese Fehlermeldung beim Zugriff der Anwendung veröffentlicht, wenn sie nicht ordnungsgemäß für diese Anwendung auf auf Prem definiert sind. | Stellen Sie sicher, dass die Benutzer die entsprechenden Berechtigungen verfügen, Back-End-Anwendung auf dem Computer auf Prem definiert. |


## <a name="see-also"></a>Siehe auch

- [Anwendungsproxy für Azure Active Directory aktivieren](active-directory-application-proxy-enable.md)
- [Veröffentlichen Sie mit Anwendungsproxy](active-directory-application-proxy-publish.md)
- [Einmaliges Anmelden aktivieren](active-directory-application-proxy-sso-using-kcd.md)
- [Bedingter Zugriff](active-directory-application-proxy-conditional-access.md)

Überprüfen Sie die aktuellsten Neuigkeiten und Updates [Anwendungsproxy blog](http://blogs.technet.com/b/applicationproxyblog/)


<!--Image references-->
[1]: ./media/active-directory-application-proxy-troubleshoot/connectorproperties.png
[2]: ./media/active-directory-application-proxy-troubleshoot/sessionlog.png
