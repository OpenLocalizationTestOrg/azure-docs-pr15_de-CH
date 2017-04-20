<properties
    pageTitle="Zugangskontrolle für Applikationen mit Azure AD-Anwendungsproxy veröffentlicht"
    description="Beschreibt die Zugangskontrolle für Applikationen einrichten um greifen entfernt mithilfe von Azure AD-Anwendungsproxy veröffentlichen."
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
    ms.date="06/22/2016"
    ms.author="kgremban"/>

# <a name="working-with-conditional-access"></a>Arbeiten mit Zugangskontrolle

Sie können Zugriffsregeln, um Clientanwendungen veröffentlicht Anwendungsproxy bedingten Zugriff konfigurieren. Dadurch können Sie:

- Mehrstufige Authentifizierung pro Anwendung
- Nur wenn der Benutzer nicht im Büro sind mehrstufige Authentifizierung
- Verhindern Sie, dass Benutzer auf die Anwendung zugreifen, wenn sie nicht am Arbeitsplatz

Diese Regeln können für alle Benutzer und Gruppen oder nur für bestimmte Benutzer und Gruppen angewendet werden. Standardmäßig gilt die Regel für alle Benutzer, die auf die Anwendung zugreifen. Jedoch kann die Regel auch auf Benutzer beschränkt werden, die Mitglieder der angegebenen Sicherheitsgruppen sind.  

Regeln werden ausgewertet, wenn ein Benutzer eine verbundene Anwendung zugreift, die OAuth 2.0, OpenID Connect, SAML oder WS-Verbund verwendet. Darüber hinaus werden Zugriffsregeln mit OAuth 2.0 OpenID verbinden ausgewertet, wenn ein Aktualisierungstoken verwendet wird, ein Zugriffstoken zu.

## <a name="conditional-access-prerequisites"></a>Zugangskontrolle-Komponenten

- Abonnement von Azure Active Directory Premium
- Föderations- oder verwaltete Azure Active Directory Mieter
- Föderierte Mieter erfordern die mehrstufige Authentifizierung (MFA) aktiviert werden  
    ![Konfigurieren von Zugriffsregeln - mehrstufige Authentifizierung](./media/active-directory-application-proxy-conditional-access/application-proxy-conditional-access.png)

## <a name="configure-per-application-multi-factor-authentication"></a>Pro Anwendung mehrstufige Authentifizierung konfigurieren
1. Melden Sie sich als Administrator im klassischen Azure-Portal.
2. Wechseln Sie zu Active Directory und wählen Sie das Verzeichnis in dem Anwendungsproxy erstellt werden soll.
3. Klicken Sie auf **Applikationen** und scrollen Abschnitt **Zugriffsregeln** . Zugriff im Abschnitt wird nur angezeigt, wenn die Anwendung veröffentlicht Anwendungsproxy, die Verbundauthentifizierung verwenden.
4. Aktivieren der Regel **Aktivieren Zugriffsregeln** **auf**auswählen.
5. Geben Sie die Benutzer und Gruppen, die die Regeln gelten. Verwenden Sie die Schaltfläche **Gruppe hinzufügen** eine oder mehrere Gruppen auswählen, für die die Regel gelten soll. Dieses Dialogfeld kann auch verwendet werden, ausgewählte Gruppen entfernen.  Die Regeln auf Gruppen anwenden ausgewählt, werden die Zugriffsregeln nur für Benutzer erzwungen, die einer der angegebenen Sicherheitsgruppen angehören.  

  - Um die Regel explizit Sicherheitsgruppen ausschließen, **außer** überprüfen und eine oder mehrere Gruppen angeben. Mitglieder einer Gruppe in der Liste mit Ausnahme nicht müssen Mehrfaktor-Authentifizierung durchzuführen.  

  - Wenn ein Benutzer mithilfe des Features pro Benutzer mehrstufige Authentifizierung konfiguriert wurde, wird diese Einstellung die kombinierte Authentifizierungsregeln Vorrang. Dies bedeutet, dass ein Benutzer, der für die kombinierte Authentifizierung pro Benutzer konfiguriert für mehrstufige Authentifizierung erforderlich, auch wenn Anwendungsregeln mehrstufige Authentifizierung ausgenommen wurden. Erfahren Sie mehr über [mehrstufige Authentifizierung und benutzerspezifischen Einstellungen](../multi-factor-authentication/multi-factor-authentication.md).

6. Wählen Sie die Regel, die Sie festlegen möchten:
    - **Verlangen der kombinierten Authentifizierung**: Benutzer für die Zugriffsregeln gelten werden vollständige mehrstufige Authentifizierung vor dem Zugriff auf die Anwendung, die die Regel gilt.
    - **Erfordern mehrstufige Authentifizierung nicht am Arbeitsplatz**: Benutzer Zugriff auf die Anwendung von einer vertrauenswürdigen IP-Adresse nicht, mehrstufige Authentifizierung erforderlich. Vertrauenswürdige IP-Adressbereiche können auf die kombinierte Authentifizierung konfiguriert werden.
    - **Zugriff nicht am Arbeitsplatz**: Benutzer Zugriff auf die Anwendung von außerhalb des Unternehmensnetzwerks dürfen auf die Anwendung zugreifen.


## <a name="configuring-mfa-for-federation-services"></a>Konfigurieren von MFA für Verbunddienste
Föderierte Mieter kann mehrstufige Authentifizierung (MFA) Azure Active Directory oder die lokal durchgeführt werden AD FS-Server. Standardmäßig treten MFA auf jeder Seite von Azure Active Directory gehostet. Konfiguration lokal MFA Windows PowerShell führen Sie aus und die SupportsMFA-Eigenschaft Azure AD-Modul festgelegt.

Das folgende Beispiel zeigt lokale MFA mithilfe des [Set-MsolDomainFederationSettings-Cmdlet](https://msdn.microsoft.com/library/azure/dn194088.aspx) contoso.com Mieter aktivieren:`Set-MsolDomainFederationSettings -DomainName contoso.com -SupportsMFA $true `

Zusätzlich zum Festlegen dieses Flags muss verbundenen Mieter AD FS-Instanz für die mehrstufige Authentifizierung konfiguriert werden. Anleitung für die [Bereitstellung von Microsoft Azure mehrstufige Authentifizierung vor Ort](../multi-factor-authentication/multi-factor-authentication-get-started-server.md).


## <a name="see-also"></a>Siehe auch

- [Arbeiten mit Ansprüchen Beachten](active-directory-application-proxy-claims-aware-apps.md)
- [Veröffentlichen Sie mit Anwendungsproxy](active-directory-application-proxy-publish.md)
- [Einmalige Anmeldung aktivieren](active-directory-application-proxy-sso-using-kcd.md)
- [Veröffentlichen Sie mit Ihren eigenen Domänennamen Applikationen](active-directory-application-proxy-custom-domains.md)

Überprüfen Sie die aktuellsten Neuigkeiten und Updates [Anwendungsproxy blog](http://blogs.technet.com/b/applicationproxyblog/)
