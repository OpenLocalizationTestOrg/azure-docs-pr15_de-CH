<properties
    pageTitle="Einmaliges Anmelden mit Anwendungsproxy | Microsoft Azure"
    description="Erläutert, wie einmaliges Anmelden mit Azure AD-Anwendungsproxy bereitstellen."
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
    ms.date="08/10/2016"
    ms.author="kgremban"/>


# <a name="single-sign-on-with-application-proxy"></a>Einmaliges Anmelden mit Anwendungsproxy

Einmaliges Anmelden ist ein Schlüsselelement von Azure AD-Anwendungsproxy. Es bietet die optimale Darstellung mit den folgenden Schritten:

1. Ein Benutzer meldet sich bei der cloud  
2. Alle Security Validierungen geschehen in der Cloud (Vorauthentifizierung)  
3. Wenn die Anforderung für die Anwendung auf Prem imitiert Application Proxy Connector den Benutzer. Die Back-End-Anwendung denkt ist ein normaler Benutzer ein Domäne aus.

![Access Diagramm vom Endbenutzer über Application Proxy auf das Unternehmensnetzwerk](./media/active-directory-application-proxy-sso-using-kcd/app_proxy_sso_diff_id_diagram.png)

Azure AD-Anwendungsproxy können Sie eine einmaliges Anmelden (SSO) für Benutzer bereitzustellen. Gehen Sie folgendermaßen vor, Ihre apps mit SSO veröffentlichen:


## <a name="sso-for-on-prem-iwa-apps-using-kcd-with-application-proxy"></a>SSO auf Prem IWA Apps Anwendungsproxy KCD mit
Sie können auf den Clientanwendungen mit integrierten Windows-Authentifizierung (IWA) mit Application Proxy Connectors Berechtigungen in Active Directory Benutzer imitieren und senden und empfangen auf ihren Namen.


### <a name="network-diagram"></a>Netzwerkdiagramm

Dieses Diagramm erklärt den Fluss, wenn ein Benutzer versucht, eine Anwendung auf Prem zugreifen, die IWA verwendet.

![Microsoft AAD Authentifizierung Datenflussdiagramm](./media/active-directory-application-proxy-sso-using-kcd/AuthDiagram.png)

1. Der Benutzer gibt den URL Anwendungsproxy auf Prem-Anwendung zugreifen.
2. Anwendungsproxy leitet die Anforderung an Azure AD Authentifizierungsdienste vorauthentifiziert werden soll. Zu diesem Zeitpunkt wendet Azure AD für Authentifizierung und Autorisierungsrichtlinien wie die kombinierte Authentifizierung. Wenn der Benutzer überprüft, Azure AD erstellt ein Token und an den Benutzer gesendet.
3. Der Benutzer übergibt das Token an Anwendungsproxy.
4. Anwendungsproxy überprüft das Token und User Principal Name (UPN) entnimmt und sendet dann die Anforderung, UPN und Service Principal Name (SPN) an den Anschluss durch einen Dual authentifizierten sicheren Kanal.
5. Der Connector führt Kerberos Constrained Delegation (KCD) Verhandlungen mit der Prem AD Benutzeridentität zu der Anwendung ein Kerberos-Token.
6. Active Directory sendet das Kerberos-Token für die Anwendung mit dem Anschluss.
7. Der Connector sendet die ursprüngliche Anforderung an den Anwendungsserver mithilfe des Kerberos-Tokens erhielt von AD.
8. Die Anwendung sendet die Antwort an den Stecker dann Anwendungsproxys Service zurückgegeben wird und für den Benutzer.

### <a name="prerequisites"></a>Erforderliche Komponenten

Zunächst mit SSO für Anwendungsproxy, stellen Sie sicher, dass Ihre Umgebung mit den folgenden Konfigurationen bereit ist:

- Ihre apps wie SharePoint Web apps sind integrierte Windows-Authentifizierung festgelegt. Weitere Informationen finden Sie unter [Unterstützung für Kerberos-Authentifizierung aktivieren](https://technet.microsoft.com/library/dd759186.aspx)und SharePoint finden Sie unter [Planen für Kerberos-Authentifizierung in SharePoint 2013](https://technet.microsoft.com/library/ee806870.aspx).

- Alle apps haben Dienstprinzipalnamen.

- Der Server mit den Connector und der Server die Anwendung sind Domäne beitreten und Teil der gleichen Domäne oder vertrauenden Domänen. Weitere Informationen zum Beitreten zu einer Domäne finden Sie unter [Fügt einen Computer einer Domäne](https://technet.microsoft.com/library/dd807102.aspx).

- Der Server mit dem Connector hat Lesezugriff auf die TokenGroupsGlobalAndUniversal für Benutzer. Dies ist eine Standardeinstellung, die Sicherheit die Umgebung Absichern beeinträchtigt werden könnte. Zusätzliche Hilfe mit diesem in [KB2009157](https://support.microsoft.com/en-us/kb/2009157).

### <a name="active-directory-configuration"></a>Active Directory-Konfiguration

Active Directory-Konfiguration hängt davon ab, ob Ihre Anwendung Proxy Connector und dem veröffentlichten Server in derselben Domäne oder nicht sind.

#### <a name="connector-and-published-server-in-the-same-domain"></a>Connector und veröffentlichten Server in derselben Domäne

1. Wechseln Sie in Active Directory zu **Tools** > **Benutzer und Computer**.
2. Wählen Sie den Server mit den Connector.
3. Mit der rechten Maustaste und wählen Sie **Eigenschaften** > **Delegieren**.
4. Wählen Sie **Computer für Delegierungszwecke angegebener Dienste vertrauen** und unter **dieses Konto delegierte Anmeldeinformationen präsentieren kann Dienste**fügen Sie den Wert für die Identität des Dienstprinzipalnamens des Anwendungsservers hinzu.
5. Dies ermöglicht Application Proxy Connector Identität von Benutzern AD für die Anwendung in der Liste definiert.

![Connector-SVR Eigenschaften Fenster screenshot](./media/active-directory-application-proxy-sso-using-kcd/Properties.jpg)

#### <a name="connector-and-published-server-in-different-domains"></a>Connector und veröffentlichten Server in unterschiedlichen Domänen

1. Finden Sie eine Liste der erforderlichen Komponenten für die Arbeit mit KCD domänenübergreifend [Eingeschränkte Kerberos-Delegierung zwischen Domänen](https://technet.microsoft.com/library/hh831477.aspx).
2. In Windows 2012 R2 verwenden die `principalsallowedtodelegateto` -Eigenschaft auf dem Connectorserver Aktivieren der Anwendungsproxy für den Server Connector Delegieren der veröffentlichte Server ist `sharepointserviceaccount` und delegierende Server `connectormachineaccount`.

        $connector= Get-ADComputer -Identity connectormachineaccount -server dc.connectordomain.com

        Set-ADComputer -Identity sharepointserviceaccount -PrincipalsAllowedToDelegateToAccount $connector

        Get-ADComputer sharepointserviceaccount -Properties PrincipalsAllowedToDelegateToAccount


>[AZURE.NOTE] `sharepointserviceaccount`ist der SPS-Computerkonto oder ein Dienstkonto, unter dem der SPS-Anwendungspool ausgeführt wird.


### <a name="azure-classic-portal-configuration"></a>Azure klassische Portalkonfiguration

1. Veröffentlichen Sie die Anwendung nach der Anleitung in [Clientanwendungen mit Anwendungsproxy veröffentlichen](active-directory-application-proxy-publish.md). Achten Sie darauf, dass **Azure Active Directory** als die **Vorauthentifizierung Methode**auswählen.
2. Wenn Ihre Anwendung in der Liste der Programme angezeigt wird, wählen sie und klicken Sie auf **Konfigurieren**.
3. Legen Sie unter **Eigenschaften** **Interne Authentifizierungsmethode** auf **Integrierte Windows-Authentifizierung**.  
  ![Erweiterte Konfiguration](./media/active-directory-application-proxy-sso-using-kcd/cwap_auth2.png)  
4. Geben Sie die **Interne Anwendung SPN** des Anwendungsservers. In diesem Beispiel wird der SPN für veröffentlichte Anwendung http/lob.contoso.com.  

>[AZURE.IMPORTANT] Wenn Ihre lokalen Benutzerprinzipalnamen und UPN in Azure Active Directory nicht identisch sind, müssen Sie [den Anmeldenamen des Teilnehmers übertragen](#delegated-login-identity) Vorauthentifizierung zu konfigurieren.

|  |  |
| --- | --- |
| Interne Authentifizierungsmethode | Bei Verwendung von Azure AD für die Vorauthentifizierung können Sie interne Authentifizierungsmethode, die Benutzer profitieren von einmaliges Anmelden (SSO) für diese Anwendung festlegen. <br><br> Wählen Sie **Integrierte Windows-Authentifizierung** (IWA), wenn Ihre IWA Anwendung und Kerberos Constrained Delegation (KCD) zum Aktivieren von SSO für diese Anwendung konfigurieren. IWA verwenden, mit KCD konfiguriert werden müssen, andernfalls Anwendungsproxy dürfen diesen veröffentlichen. <br><br> Wählen Sie **keine** aus, wenn die Anwendung nicht IWA verwenden. |
| Interne Anwendung SPN | Der Service Principal Name (SPN) des internen Anwendung ist in die auf-Prem Azure AD konfiguriert Der SPN Application Proxy Connector dient, Kerberos-Token für die Anwendung KCD abzurufen. |


## <a name="sso-for-non-windows-apps"></a>SSO für Windows-apps
Kerberos-Delegierung Fluss in Azure AD-Anwendungsproxy beginnt Azure AD-Authentifizierung des Benutzers in der Cloud. Die Anforderung wird lokal gibt Azure AD Application Proxy Connector ein Kerberos-Ticket für den Benutzer durch die Interaktion mit der lokalen Active Directory. Dieser Prozess wird als Kerberos Constrained Delegation (KCD) bezeichnet. In der nächsten Phase wird eine Anforderung an die Back-End-Anwendung mit Kerberos-Ticket gesendet. Es gibt eine Reihe von Protokollen, die definieren, wie solche Anfragen gesendet. Die meisten Windows-Server erwarten Negotiate/SPNego, die jetzt in Azure AD-Anwendungsproxy unterstützt wird.

![Nicht-Windows SSO-Diagramm](./media/active-directory-application-proxy-sso-using-kcd/app_proxy_sso_nonwindows_diagram.png)

### <a name="delegated-login-identity"></a>Delegierte Login Identität
Delegierte Login Identität können Sie zwei verschiedene Login-Szenarios:

- Nicht-Windows-Anwendung, die in der Regel Benutzeridentität einen Benutzernamen oder den SAM-Kontonamen, keine e-Mail-Adresse erhalten (username@domain).
- Alternative Login-Konfigurationen, UPN in Azure AD und der UPN im lokalen Active Directory.

Wählen Sie mit Anwendungsproxy welche Identität verwenden, um das Kerberos-Ticket erhalten. Diese Einstellung wird pro Anwendung. Einige dieser Optionen sind für Systeme, die keine e-Mail-Adressformat akzeptieren, andere sind für alternative Login.

![Delegierte Login Identität Parameter screenshot](./media/active-directory-application-proxy-sso-using-kcd/app_proxy_sso_diff_id_upn.png)

Delegierte Login Identität verwendet, kann der Wert nicht für alle Domänen oder Gesamtstrukturen in der Organisation eindeutig sein. Sie können dieses Problem vermeiden, diesen zweimal mit zwei unterschiedlichen connectorgruppen veröffentlichen. Da jede Anwendung eine andere Benutzergruppe als Zielgruppe verfügt, können Sie die Verbinder in eine andere Domäne verknüpfen.


## <a name="working-with-sso-when-on-premises-and-cloud-identities-are-not-identical"></a>Arbeiten mit SSO bei lokalen und Cloud Identitäten nicht identisch sind.
Sofern nicht anderweitig konfiguriert, vorausgesetzt Anwendungsproxy Benutzer genau die gleiche Identität in der Cloud und lokal. Sie können für jede Anwendung konfigurieren, welche Identität verwendet werden soll, wenn einmaliges ausführen.  

Viele Organisationen auf Prem und Cloud Identitäten müssen SSO aus der Cloud auf Prem apps ohne die Benutzer verschiedene Benutzernamen und Kennwörter eingeben können. Dies umfasst Unternehmen, die:

- Haben mehrere Domänen intern (joe@us.contoso.com, joe@eu.contoso.com) und eine einzelne Domäne in der Cloud (joe@contoso.com).

- Intern nicht routbare Domänennamen haben (joe@contoso.usa) und rechtliche in der Cloud.

- Verwenden Sie keine Domänennamen intern (Joe)

- Verwenden Sie unterschiedliche Aliase auf Prem und in der Cloud. Z.b. joe-johns@contoso.comim Vergleich zujoej@contoso.com  

Es hilft auch bei Programmen, die keine Adressen aus e-Mail akzeptieren ist ein sehr häufiges Szenario für Windows-Back-End-Server.


### <a name="setting-sso-for-different-cloud-and-on-prem-identities"></a>Festlegen von SSO für Cloud und auf Prem Identitäten

1. Konfigurieren Sie Azure AD Connect Identität werden die e-Mail-Adresse (Mail). Als Teil des Prozesses anpassen wird dazu Feld **Benutzerprinzipalnamen** Sync-Einstellung ändern. Beachten Sie, dass diese Einstellung auch festlegen, wie Benutzer Office365 Windows10 Geräten anmelden und andere Programme, die als ihre Identitätsspeicher Azure AD verwenden.  
  ![Identifizieren von Benutzern Screenshot - Benutzerprinzipalnamen-dropdown](./media/active-directory-application-proxy-sso-using-kcd/app_proxy_sso_diff_id_connect_settings.png)  
2. Wählen Sie in der Konfiguration der Anwendung für die Anwendung, die Sie ändern möchten die **Login-Identität übertragen** verwendet werden:
  - Benutzerprinzipalnamen:joe@contoso.com  
  - Alternative Benutzerprinzipalnamen:joed@contoso.local  
  - Benutzername Teil Benutzerprinzipalnamen: Joe  
  - Teil des alternativen Benutzerprinzipalnamen Benutzername: Joed  
  - Lokale SAM-Kontoname: je auf Prem Konfiguration

  ![Delegierte Login Identität im Dropdownmenü screenshot](./media/active-directory-application-proxy-sso-using-kcd/app_proxy_sso_diff_id_upn.png)  

### <a name="troubleshooting-sso-for-different-identities"></a>Problembehandlung bei SSO für Identitäten
Liegt ein Fehler in der SSO-Prozess wird im Ereignisprotokoll Connector Computer angezeigt wie [Problembehandlung](active-directory-application-proxy-troubleshoot.md)erläutert.
Aber in einigen Fällen die Anforderung erfolgreich erhalten die Back-End-Anwendung während dieser Anwendung in verschiedenen anderen HTTP-Antworten beantworten. Problembehandlung bei diesen sollten anhand der Ereignisnummer 24029 auf dem Computer Connector im Ereignisprotokoll Anwendungsproxy Sitzung beginnen. Die Benutzeridentität, die für die Delegierung verwendet wird im Feld "Benutzer" in den Ereignisdetails angezeigt. Zum Aktivieren der Sitzungsprotokoll wählen Sie **Anzeigen von analytischen und Debugprotokollen** bei Anzeige im Menü anzeigen aus


## <a name="see-also"></a>Siehe auch

- [Veröffentlichen Sie mit Anwendungsproxy](active-directory-application-proxy-publish.md)
- [Problembehandlung, mit Anwendungsproxy](active-directory-application-proxy-troubleshoot.md)
- [Arbeiten mit Ansprüchen Beachten](active-directory-application-proxy-claims-aware-apps.md)
- [Bedingter Zugriff](active-directory-application-proxy-conditional-access.md)

Überprüfen Sie die aktuellsten Neuigkeiten und Updates [Anwendungsproxy blog](http://blogs.technet.com/b/applicationproxyblog/)


<!--Image references-->
[1]: ./media/active-directory-application-proxy-sso-using-kcd/AuthDiagram.png
[2]: ./media/active-directory-application-proxy-sso-using-kcd/Properties.jpg
