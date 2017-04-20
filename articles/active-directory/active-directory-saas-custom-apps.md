<properties 
    pageTitle="Einmaliges Anmelden für Clientanwendungen, die nicht in der Galerie Azure Active Directory konfigurieren | Microsoft Azure" 
    description="Erfahren Sie, wie Self-Service herstellen apps in Azure Active Directory SAML mit SSO-Passwort" 
    services="active-directory" 
    authors="asmalser-msft"  
    documentationCenter="na" manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="02/09/2016" 
    ms.author="asmalser" />

#<a name="configuring-single-sign-on-to-applications-that-are-not-in-the-azure-active-directory-application-gallery"></a>Einmaliges Anmelden für Clientanwendungen konfigurieren, die nicht in der Galerie Azure Active Directory

Dieser Artikel beschreibt das Feature Administratoren für einmaliges Anmelden für Clientanwendungen nicht vorhanden in der Azure Active Directory app Gallery *ohne Schreiben von Code*konfigurieren. Dieses Feature wurde aus technischen Vorschau am 18. November 2015 veröffentlicht und in [Azure Active Directory](active-directory-editions.md)enthalten. Wenn Sie stattdessen für Entwickler Anleitung Azure AD Code benutzerdefinierte apps Integration suchen, finden Sie unter [Authentifizierungsszenarien Azure AD](active-directory-authentication-scenarios.md).

Die Galerie Azure Active Directory enthält eine Liste der Programme, die eine Form des einmaligen Anmeldens Azure Active Directory unterstützen, wie in [diesem Artikel](active-directory-appssoaccess-whatis.md)beschrieben. Wenn Sie (wie ein IT-Spezialist oder Systemintegrator in Ihrer Organisation) die Anwendung gefunden haben, eine Verbindung herstellen möchten, können Sie loslegen von Anweisungen schrittweise im Azure-Verwaltungsportal angezeigt-auf aktivieren.

Kunden mit Lizenzen [Azure Active Directory Premium](active-directory-editions.md) erhalten außerdem diese zusätzlichen Funktionen:

* Self-service Integration jeder Anwendung, die SAML 2.0 Identitätsprovider (SP initiiert oder IdP initiiert) unterstützt
* Integration von jede Web-Anwendung, die eine HTML-basierte Anmeldeseite über [kennwortbasierte SSO](active-directory-appssoaccess-whatis.md#password-based-single-sign-on) Self-service
* Self-service-Verbindung, das SCIM-Protokoll für Benutzer bereitstellen ([beschriebene](active-directory-scim-provisioning.md)) verwenden
* Möglichkeit einer Anwendung der [Office 365-Startprogramm](https://blogs.office.com/2014/10/16/organize-office-365-new-app-launcher-2/) oder [Azure AD-Abdeckung](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users) Links hinzufügen

Zählen nicht SaaS-Anwendung, die Sie jedoch haben nicht noch wurde auf-an Bord, der Galerie Azure AD nur Ihrer Organisation auf Servern bereitgestellt hat, die Sie entweder in der Cloud oder lokale Kontrolle, Drittanbieter-Webapplikationen.

Diese Funktionen auch *app Integration Vorlagen*bieten standardbasierte Verbindung für apps, die SAML, SCIM oder formularbasierte Authentifizierung unterstützt und sind flexibel und Einstellungen für die Kompatibilität mit einer Vielzahl von Applikationen. 

##<a name="adding-an-unlisted-application"></a>Nicht aufgeführte Anwendung

Zum Verbinden einer Anwendung mit einer app Integration Vorlage Azure-Verwaltungsportal über Ihre Azure Active Directory-Administratorkonto anmelden und navigieren Sie zu der **Active Directory > [Verzeichnis] > Applications** Abschnitt, wählen Sie **Hinzufügen**und dann **eine Anwendung aus dem Katalog hinzufügen**. 

![][1]

In der Galerie app fügen Sie eine nicht aufgeführte Anwendung über die **benutzerdefinierte** Kategorie links oder durch **Hinzufügen einer nicht aufgelisteten Anwendung** Verknüpfung, die in den Suchergebnissen angezeigt wird, wenn die gewünschte Anwendung nicht gefunden wurde. Geben Sie einen Namen für Ihre Anwendung, können Sie die single Sign-On-Optionen und Verhalten konfigurieren. 

**Tipp**: Es wird empfohlen, mithilfe der Suchfunktion überprüfen existiert bereits in der Galerie. Wenn die app und seine Beschreibung "einmaliges Anmelden", und die Anwendung erwähnt wird bereits für föderierte einmaliges unterstützt. 

![][2]

Hinzufügen einer Anwendung auf diese Weise bietet ähnliche für integrierte Anwendung zu. Wählen Sie dazu **Konfigurieren Sie einmaliges Anmelden**. Der nächste Bildschirm zeigt die folgenden drei Optionen zum Konfigurieren von einzelnen Zeichen, die in den folgenden Abschnitten beschrieben werden.

![][3]


##<a name="azure-ad-single-sign-on"></a>Azure AD einmaliges Anmelden

Wählen Sie diese Option SAML-Authentifizierung für die Anwendung konfigurieren. Hierfür Anwendungssupport SAML 2.0 und Informationen zur Verwendung von SAML-Funktionen der Anwendung, bevor Sie fortfahren gesammelt werden sollen. Nachdem Sie auf **Weiter**, werden Sie aufgefordert, drei verschiedene URLs für SAML-Endpunkte für die Anwendung eingeben. 

![][4]
 
Diese sind:

* **Anmelde-URL (SP initiiert nur)** – wo der Benutzer dieser Anwendung anmelden geht. Wenn die Anwendung führen einmaliges für Service Provider initiiert konfiguriert, dann bei einer dieser URL navigiert tun Dienstanbieter erforderliche Umleitung Azure AD authentifizieren und den Benutzer anmelden. Wenn dieses Feld ausgefüllt ist, dann verwendet Azure AD URL zum Starten der Anwendung von Office 365 und Azure AD-Abdeckung. Wenn dieses Feld Flags, Azure AD führt stattdessen Identitätsanbieter-initiierte anmelden, wenn die app von Office 365, die Abdeckung von Azure AD oder Azure AD gestartet wird single Sign-On URL (kopierbar von Dashboard-Registerkarte).

* **Aussteller URL** - Aussteller zu URL eindeutig die Anwendung für die einmalige Anmeldung auf sollten wird konfiguriert. Dies ist der Wert, den zurück an die Anwendung als **Zielgruppe** Parameter des SAML-Tokens Azure AD sendet und die Bewerbung überprüfen. Dieser Wert wird auch als **Entitäts-ID** in der Anwendung SAML Metadaten. Dokumentation der Anwendung SAML Details auf Entitäts-ID oder Zielgruppe Wert. Unten ist ein Beispiel für die Zielgruppe URL wie im SAML-Token an die Anwendung zurückgegeben wird:

```
    <Subject>
    <NameID Format="urn:oasis:names:tc:SAML:2.0:nameid-format:unspecificed">chad.smith@example.com</NameID>
        <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer" />
      </Subject>
      <Conditions NotBefore="2014-12-19T01:03:14.278Z" NotOnOrAfter="2014-12-19T02:03:14.278Z">
        <AudienceRestriction>
          <Audience>https://tenant.example.com</Audience>
        </AudienceRestriction>
      </Conditions>
```

* **Antwort-URL** - Antwort-URL ist, an dem die Anwendung das SAML-Token zu erhalten. Dies wird auch als **URL Assertion Consumer Service (ACS)**bezeichnet. Dokumentation der Anwendung SAML Details auf SAML token antwortet oder ACS URL.
 Nachdem diese eingegeben haben, klicken Sie auf **Weiter** um zum nächsten Bildschirm zu gelangen. Dieser Bildschirm enthält Informationen darüber, was auf die Anwendungsseite zu akzeptieren ein SAML-Token von Azure AD konfiguriert werden. 

![][5]

Erforderliche Werte wird je nach Anwendung variieren, so Dokumentation die Anwendung SAML Details. **Anmelden** und **Abmelden** Dienst-URL, die beide in dieselbe IP-Adresse, die SAML Anforderungsbehandlung Endpunkt für die Instanz von Azure AD aufgelöst. Aussteller-URL ist der Wert, der als "Aussteller" innerhalb des SAML-Tokens ausgestellt für die Anwendung angezeigt wird. 

Nachdem die Anwendung konfiguriert wurde, klicken Sie auf **Weiter** und **abgeschlossen** , um das Dialogfeld zu schließen. 

##<a name="assigning-users-and-groups-to-your-saml-application"></a>Zuweisen von Benutzern und Gruppen der SAML-Anwendung 

Sobald die Anwendung konfiguriert wurde, um Azure AD als SAML-basierte Identitätsanbieter verwenden ist fast fertig zum Testen. Als eine Sicherheitskontrolle stellt Azure AD keinen Token zu der Anwendung anmelden, wenn sie mit Azure AD Zugriff gewährt wurde. Benutzer werden Zugriff gewährt direkt oder durch eine Gruppe, der deren Mitglied Sie sind. 

Um einen Benutzer oder eine Gruppe die Anwendung zuzuweisen, klicken Sie auf **Benutzer zuweisen** . Wählen Sie den Benutzer oder die Gruppe zuweisen möchten, und wählen Sie dann die Schaltfläche **zuweisen** . 

![][6]

Zuweisen eines Benutzers können Azure AD einen Token für den Benutzer, sowie eine Kachel für diese Anwendung im Zugriff des Benutzers auf ausstellen. Eine Anwendung Kachel wird auch in Office 365 Anwendungsstartprogramm angezeigt, wenn der Benutzer Office 365. 

Sie können ein Logo Kachel für die Anwendung die Schaltfläche **Logo hochladen** auf die Registerkarte **Konfigurieren** der Anwendung hochladen. 

###<a name="customizing-the-claims-issued-in-the-saml-token"></a>Im SAML-Token ausgestellte Ansprüche anpassen 

Wenn ein Benutzer der Anwendung authentifiziert wird Azure AD ausstellen ein SAML-Token App, die Informationen (oder Ansprüche) enthält über den Benutzer, die eindeutig identifiziert. Standardmäßig sind dies des Benutzers Benutzername, e-Mail-Adresse, Vorname und Nachname. 

Sie können anzeigen oder Bearbeiten der Ansprüche im SAML-Token für die Anwendung unter der Registerkarte **Attribute** gesendet. 

![][7]

Es gibt zwei Gründe, warum möglicherweise im SAML-Token ausgestellte Ansprüche bearbeiten: •The Anwendung wurde geschrieben, um verschiedene Anspruch URIs erforderlich oder Forderung Werte Dein Anwendung bereitgestellt wurde, auf eine Weise, die die NameIdentifier erfordert ausgeben als in Azure Active Directory gespeicherten Benutzernamen (AKA User principal Name). 

Informationen zum Hinzufügen und Bearbeiten von Ansprüchen für diese Szenarien finden Sie in diesem [Artikel Ansprüchen anpassen](active-directory-saml-claims-customization.md). 

###<a name="testing-the-saml-application"></a>Testen der SAML-Anwendung 

Sobald der SAML-URLs und das Zertifikat in Azure AD und der Anwendung konfiguriert wurden, Benutzern oder Gruppen zugewiesen wurden, die Anwendung in Azure Ansprüche überprüft und ggf. bearbeitet, und der Benutzer kann die Anwendung anmelden. 

Test einfach anmelden Azure AD-Abdeckung am https://myapps.microsoft.com mit einem Benutzerkonto, das der Anwendung zugeordnet, und klicken Sie auf die Kachel für die Anwendung der einzelnen SSO-Prozess starten. Alternativ können Sie direkt auf die URL Anmeldung für die Anwendung und melden Sie sich dort durchsuchen. 

Tipps zum Debuggen finden Sie im [Artikel zum SAML-basierte einmaliges Anwendung debuggen](active-directory-saml-debugging.md) 

##<a name="password-single-sign-on"></a>Einmaliges Anmelden für Kennwort

Wählen Sie diese Option, [kennwortbasierte einmaliges Anmelden](active-directory-appssoaccess-whatis.md) für eine Anwendung konfigurieren eine HTML-Seite. Kennwortbasierte SSO auch bezeichnet als Kennwort vaulting, ermöglicht Ihnen, Benutzerzugriff und Kennwörter Web Applications, die Identitätsverbund nicht unterstützen. Es eignet sich auch für Szenarios, in denen mehrere Benutzer ein einzelnes Konto wie auf Ihrem Unternehmen soziale Medien app freigeben müssen. 

Nachdem Sie auf **Weiter**, werden Sie aufgefordert, die URL der Anwendung webbasierte Anmeldeseite eingeben. Beachten Sie, dass dies die Seite, die die Eingabefelder Benutzername und Kennwort enthält. Sobald eingegeben, startet Azure AD einen Prozess zum Analysieren der Anmeldeseite für eine Eingabe Benutzernamen und ein Kennwort eingeben. Wenn der Vorgang nicht erfolgreich ist, führt Sie dann es durch eine andere Installation eine Browsererweiterung (erfordert Internet Explorer, Chrome oder Firefox), mit die Sie die Felder manuell erfassen kann.

Nach der Erfassung der Anmeldeseite Benutzern und Gruppen zugewiesen werden können und wie reguläre [Kennwort SSO-apps](active-directory-appssoaccess-whatis.md)Anmeldeinformationen Richtlinien eingestellt werden.

Hinweis: Sie können ein Logo Kachel für die Anwendung die Schaltfläche **Logo hochladen** auf die Registerkarte **Konfigurieren** der Anwendung hochladen. 

##<a name="existing-single-sign-on"></a>Vorhandene einmaliges Anmelden

Mit dieser Option fügen Sie einen Link zu einer Anwendung zu Azure AD Access oder Office 365-Portal Ihres Unternehmens. Dadurch können Sie benutzerdefinierte webapps Links hinzu, die derzeit Azure Active Directory Federation Services (oder andere Verbunddienst) statt Azure AD für Authentifizierung. Oder fügen deep Links zu bestimmten SharePoint-Seiten oder anderen Webseiten nur Zugriff auf des Benutzers angezeigt werden soll. 

Nachdem Sie auf **Weiter**, werden Sie aufgefordert, geben Sie die URL der Anwendung zu verknüpfen. Abschluss können Benutzer und Gruppen der Anwendung zugewiesen werden, die wodurch die Anwendung angezeigt in [Office 365-Startprogramm](https://blogs.office.com/2014/10/16/organize-office-365-new-app-launcher-2/) oder [Azure AD-Abdeckung](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users) für diese Benutzer.

Hinweis: Sie können ein Logo Kachel für die Anwendung die Schaltfläche **Logo hochladen** auf die Registerkarte **Konfigurieren** der Anwendung hochladen.

## <a name="related-articles"></a>Verwandte Artikel

- [Artikel-Index für das Anwendungsmanagement in Azure Active Directory](active-directory-apps-index.md)
- [Im SAML-Token für integrierte Apps ausgestellte Ansprüche anpassen](active-directory-saml-claims-customization.md)
- [Problembehandlung bei SAML-basierte Single Sign-On](active-directory-saml-debugging.md)

<!--Image references-->
[1]: ./media/active-directory-saas-custom-apps/customapp1.png
[2]: ./media/active-directory-saas-custom-apps/customapp2.png
[3]: ./media/active-directory-saas-custom-apps/customapp3.png
[4]: ./media/active-directory-saas-custom-apps/customapp4.png
[5]: ./media/active-directory-saas-custom-apps/customapp5.png
[6]: ./media/active-directory-saas-custom-apps/customapp6.png
[7]: ./media/active-directory-saas-custom-apps/customapp7.png
