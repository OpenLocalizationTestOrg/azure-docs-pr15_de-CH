<properties
   pageTitle="Die Anwendung in der Galerie Azure Active Directory auflisten"
   description="Wie eine Anwendung, die einmaliges Anmelden in der Galerie Azure Active Directory unterstützt | Microsoft Azure"
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="bryanla"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/16/2016"
   ms.author="mbaldwin"/>


# <a name="listing-your-application-in-the-azure-active-directory-application-gallery"></a>Die Anwendung in der Galerie Azure Active Directory auflisten

Um eine Anwendung aufzulisten, die in [Azure AD Gallery](https://azure.microsoft.com/marketplace/active-directory/all/)einmaliges Azure Active Directory unterstützt, muss die Anwendung eine der folgenden Integration implementieren:

* **OpenID verbinden** - direkte Integration in Azure AD für Authentifizierung und Azure AD Zustimmung API für Konfiguration mit OpenID verbinden. Integration nur starten und Ihre Anwendung SAML nicht unterstützt, ist dieser Modus wird empfohlen.

* **SAML** -Ihre Anwendung bereits kann dritten Identitätsprovider mit SAML-Protokoll konfigurieren.

Angebot an jedem Modus sind unten aufgeführt.

##<a name="openid-connect-integration"></a>OpenID verbinden Integration

Ihre Anwendung in Azure AD die [Entwickler Informationen](active-directory-authentication-scenarios.md)integrieren. Führen Sie die folgenden Fragen und Senden an waadpartners@microsoft.com.

* Anmeldeinformationen Sie für ein Konto oder Test Mieter mit der Anwendung, von Azure AD-Team die Integrationstests.  

* Bereitgestellt wie Azure AD anmelden und kann eine Instanz von Azure AD der Anwendung [Azure AD Zustimmung Rahmen](active-directory-integrating-applications.md#overview-of-the-consent-framework)verbinden. 

* Bieten Sie weitere Anweisungen Azure AD Team einmaliges Anmelden mit Ihrer Anwendung zu testen. 

* Geben Sie die folgenden Informationen:

> Name des Unternehmens:
> 
> Firmenwebseite:
> 
> Anwendungsname:
> 
> Beschreibung der Anwendung (256 Zeichen):
> 
> Anwendung (Information) Website:
> 
> Anwendung Technical Support-Website oder Kontaktinformationen:
> 
> Client-ID der Anwendung, der in der Anwendung Daten auf https://manage.windowsazure.com gezeigt:
> 
> Kunden wo anmelden Anmeldung URL Anwendung und/oder Erwerb die Anwendung:
> 
> Kategorien Sie bis zu drei für Ihre Anwendung (für Kategorien Siehe Azure Active Directory Marketplace) unter aufgeführt:
> 
> Fügen Sie kleine Anwendungssymbol (PNG-Datei, 45px von 45px, einfarbigen Hintergrund):
> 
> Große Anwendungssymbol (PNG-Datei, 215px von 215px, einfarbigen Hintergrund) anfügen
> 
> Anwendung-Logo (PNG-Datei, 150px von 122px, transparente Hintergrundfarbe) anfügen

##<a name="saml-integration"></a>SAML-Integration

Jede Anwendung, die SAML 2.0 unterstützt kann direkt mit einer Azure AD-Mandanten mit [folgenden Schritten fügen Sie eine benutzerdefinierte Anwendung](active-directory-saas-custom-apps.md)integriert werden. Nachdem Sie getestet haben, dass die Anwendungsintegration mit Azure AD, senden die folgende Informationen zu <waadpartners@microsoft.com>.

* Anmeldeinformationen Sie für ein Konto oder Test Mieter mit der Anwendung, von Azure AD-Team die Integrationstests.  

* Geben Sie Werte (Assertion Kundendienst) SAML-auf URL, Aussteller URL (Entity ID) und Antwort-URL für Ihre Anwendung als Beschreibung [hier](active-directory-saas-custom-apps.md). Wenn in der Regel diese Werte als Teil einer Metadatendatei SAML bieten schicken Sie, die ebenfalls.

* Bieten Sie eine kurze Beschreibung der Azure AD als Identitätsanbieter in der Anwendung mit SAML 2.0 konfigurieren. Wenn Ihre Anwendung Konfiguration von Azure AD als Identitätsanbieter eine administrative Self-service-Portal unterstützt, stellen Sie sicher, dass Anmeldeinformationen über die Möglichkeit zur Einrichtung dieser enthalten.

* Geben Sie die folgenden Informationen:

> Name des Unternehmens:
> 
> Firmenwebseite:
> 
> Anwendungsname:
> 
> Beschreibung der Anwendung (256 Zeichen):
> 
> Anwendung (Information) Website:
> 
> Anwendung Technical Support-Website oder Kontaktinformationen:
> 
> Kunden wo anmelden Anmeldung URL Anwendung und/oder Erwerb die Anwendung:
> 
> Wählen Sie bis zu drei Kategorien für Ihre Anwendung (für Kategorien Siehe [Azure Active Directory Marketplace](https://azure.microsoft.com/marketplace/active-directory/)) unter aufgeführt):
> 
> Fügen Sie kleine Anwendungssymbol (PNG-Datei, 45px von 45px, einfarbigen Hintergrund):
> 
> Große Anwendungssymbol (PNG-Datei, 215px von 215px, einfarbigen Hintergrund) anfügen
> 
> Anwendung-Logo (PNG-Datei, 150px von 122px, transparente Hintergrundfarbe) anfügen
