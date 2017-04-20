<properties
    pageTitle="Einfache Anmeldung Management für Enterprise-apps in Azure Active Directory Vorschau | Microsoft Azure"
    description="Verwalten Sie einmaliges Anmelden für Unternehmen Apps mithilfe von Azure Active Directory"
    services="active-directory"
    documentationCenter=""
    authors="asmalser"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="09/30/2016"
    ms.author="asmalser"/>

# <a name="preview-managing-single-sign-on-for-enterprise-apps-in-the-new-azure-portal"></a>Vorschau: Einmaliges Anmelden für Unternehmen apps im neuen Azure-Portal verwalten

> [AZURE.SELECTOR]
- [Azure-portal](active-directory-enterprise-apps-manage-sso.md)
- [Azure-Verwaltungsportal](active-directory-sso-integrate-saas-apps.md)

Dieser Artikel beschreibt das [Azure-Portal](https://portal.azure.com) verwenden, um einzelne Anmeldung handelt, insbesondere die Standardeinstellungen, die von [Azure Active Directory (Azure AD) Anwendungskatalog](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery)hinzugefügt wurden. Dieser Artikel beschreibt die neuen Funktionen sowie einige temporäre Einschränkungen nur während der Vorschau werden die Azure AD-Verwaltungsfunktionen für einmaliges Anmelden ist derzeit public Preview-Version [Neuigkeiten in der Vorschau](active-directory-preview-explainer.md)

##<a name="finding-your-apps-in-the-new-portal"></a>Suchen Ihre apps in das neue portal

Ab September 2016 können Anträge für einzelne konfiguriert wurden in einem Verzeichnis ein Verzeichnis Administrator [Azure Active Directory Anwendungskatalog](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery) im [klassischen Azure-Portal](https://manage.windowsazure.com)anmelden, jetzt angezeigt und verwaltet werden im Azure-Portal.

Diesen Abschnitt **Anwendung** Azure-Portal finden Sie ein die Link, der in der Liste **Weitere Dienste** im [Portal](https://portal.azure.com)finden. Enterprise-apps sind apps, die bereitgestellt wurden und von Benutzern in Ihrer Organisation verwendet werden.

![NTD-blade][1]

Wählen Sie **Alle Programme** an, die konfiguriert wurden einschließlich apps, die aus der Galerie hinzugefügt wurde, eine Liste aller. Auswählen einer Anwendung lädt Ressourcen Blade App, wo Berichte können für die Anwendung und Einstellungen verwaltet werden kann.

Um Einstellungen für einzelne Zeichen zu verwalten, wählen Sie **einmaliges Anmelden**.

![Anwendung Ressource blade][2]


##<a name="single-sign-on-modes"></a>Modi für einzelne Zeichen

Blade **auf** beginnt mit einem **Modus** Menü single Sign-On Modus konfiguriert werden. Die Optionen sind verfügbar:

* **SAML-basierte Anmeldung** - diese Option ist verfügbar, wenn die Anwendung vollständigen verbundenen einmaliges Azure Active Directory mit dem SAML 2.0-Protokoll unterstützt.

* **Passwort anmelden** – diese Option ist verfügbar, wenn Azure AD Kennwort für diese Anwendung unterstützt.

* **Verknüpfte anmelden** - ehemals Administratoren "Vorhandene einmaliges Anmelden", diese Option eine Verknüpfung zu dieser Anwendung des Benutzers Azure AD Access oder Office 365 Anwendungsstartprogramm platzieren.

Weitere Informationen über diese Modi finden Sie unter [wie einmaliges Anmelden mit Azure Active Directory](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work).


##<a name="saml-based-sign-on"></a>SAML-basierte anmelden

Die Option **SAML-basierte Anmeldung** zeigt eine-Blade in vier Abschnitte unterteilt:

###<a name="domains-and-urls"></a>Domänen und URLs
Dies ist, wo alle Details der Anwendung Domäne und URLs Azure AD-Verzeichnis hinzugefügt werden. Alle Eingaben erforderlich, damit app-auf Arbeit werden direkt auf dem Bildschirm angezeigt, während alle optionale Eingaben durch Aktivieren des Kontrollkästchens **Erweiterte URL-Einstellungen anzeigen** angezeigt werden können. Die vollständige Liste der unterstützten Eingaben enthält:

* **Anmelde-URL** – wo der Benutzer dieser Anwendung anmelden geht. Wenn die Anwendung führen einmaliges für Service Provider initiiert konfiguriert, dann bei einer dieser URL navigiert hat Dienstanbieter erforderliche Umleitung Azure AD authentifizieren und Benutzer anmelden. Wenn dieses Feld ausgefüllt ist, dann verwendet Azure AD URL zum Starten der Anwendung von Office 365 und Azure AD-Abdeckung. Wenn dieses Feld ausgelassen, Azure AD führt stattdessen Identitätsanbieter-initiierte anmelden, wenn die app von Office 365, die Abdeckung von Azure AD oder Azure AD gestartet wird single Sign-On URL.

* **ID** - dieser URI sollte eindeutig die Anwendung für die einzelnen anmelden konfiguriert ist. Dies ist der Wert, den zurück an die Anwendung als Zielgruppe Parameter des SAML-Tokens Azure AD sendet und die Bewerbung überprüfen. Dieser Wert wird auch als Element-ID im SAML Metadaten der Anwendung.

* **Antwort-URL** - Antwort-URL ist, an dem die Anwendung das SAML-Token zu erhalten. Dies wird auch als URL Assertion Consumer Service (ACS) bezeichnet. Nachdem diese eingegeben haben, klicken Sie auf Weiter, um zum nächsten Bildschirm. Dieser Bildschirm enthält Informationen darüber, was auf die Anwendungsseite zu akzeptieren ein SAML-Token von Azure AD konfiguriert werden.

* **Relay-Status** - Zustand des Relais ist ein optionaler Parameter, mit denen der Anwendung mitteilen, wo den Benutzer nach Abschluss der Authentifizierung umgeleitet. In der Regel der Wert eine gültige URL der Anwendung jedoch einige Programme dieses Feld anders (siehe die app einmaliges Dokumentation). Den Relay-Zustand festlegen ist ein neues Feature neues Azure-Portal ist.

###<a name="user-attributes"></a>Benutzerattribute
Hier wird Administratoren können anzeigen und Bearbeiten der Attribute, die im SAML-Token, die Azure AD an die Anwendung gesendet werden, wenn Benutzer anmelden.

Für das erste Release Preview ist nur bearbeitbare Attribut unterstützt die **Benutzer-ID** -Attribut. Der Wert dieses Attributs ist das Feld in Azure AD, die jeder Benutzer innerhalb der Anwendung eindeutig. Z. B. wenn die Anwendung mit "e-Mail-Adresse" als Benutzernamen und eindeutiger Bezeichner bereitgestellt wurde, würde dann der Wert in das Feld "user.mail" in Azure AD festgelegt werden.

Zusätzliche Attribute bearbeiten werden in einer nachfolgenden Vorschau unterstützt.

###<a name="saml-signing-certificate"></a>SAML-Signaturzertifikat
Dieser Abschnitt zeigt die Details des Zertifikats Azure AD verwendet, sich an die Anwendung gesendet werden jedem Benutzer authentifiziert SAML-Tokens. Sie können die Eigenschaften des aktuellen Zertifikats zu einschließlich des Ablaufdatums.

Die Möglichkeit, das Zertifikat Rollover und Verwalten zusätzlicher Optionen in nachfolgenden Preview-Version unterstützt. Beachten Sie, dass vollständige Verwaltung von Zertifikaten im [klassischen Azure-Portal](active-directory-sso-certs.md)weiterhin ausgeführt werden kann.

###<a name="application-configuration"></a>Konfiguration
Der letzte Abschnitt enthält Dokumentation oder Steuerelemente, die Anwendung von Azure Active Directory als Identitätsanbieter konfigurieren.

**Anwendung konfigurieren** Flyout-Menü bietet neue präzise, eingebettete Anweisungen für die Anwendung konfigurieren. Dies ist ein weiteres neues Feature für neue Azure-Portal.

> [AZURE.NOTE] Ein vollständiges Beispiel für embedded-Dokumentation finden Sie unter Salesforce.com-Anwendung. Dokumentation für zusätzliche apps wird kontinuierlich während der Vorschau hinzugefügt.

![Eingebettete Dokumente][3]

##<a name="password-based-sign-on"></a>Passwort anmelden
Unterstützt für die Anwendung konfiguriert den kennwortbasierte SSO-Modus auswählen und **auswählen sofort** zu kennwortbasierten SSO. Weitere Informationen zum Bereitstellen von SSO kennwortbasierte finden Sie [wie einmaliges Anmelden mit Azure Active Directory](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work).

![Passwort anmelden][4]


##<a name="linked-sign-on"></a>Verknüpfte anmelden
Wenn für die Anwendung unterstützt, kann verknüpften SSO-Modus auswählen URL eingeben im Azure AD Access oder Office 365 an umgeleitet wird, wenn Benutzer auf diese Anwendung. Weitere Informationen zu verknüpften SSO (früher als "vorhandene SSO" bezeichnet) finden Sie unter [wie einmaliges Anmelden mit Azure Active Directory](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work).

![Verknüpfte anmelden][5]

[1]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-blade.PNG
[2]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-sso-blade.PNG
[3]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-blade-embedded-docs.PNG
[4]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-blade-password-sso.PNG
[5]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-blade-linked-sso.PNG
