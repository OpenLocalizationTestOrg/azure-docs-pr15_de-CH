<properties 
    pageTitle="Das SAML-basierte einmaliges Anwendung in Azure Active Directory Debuggen | Microsoft Azure" 
    description="Informationen Sie zum Debuggen SAML-basierte einmaliges Anwendung in Azure Active Directory " 
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

#<a name="how-to-debug-saml-based-single-sign-on-to-applications-in-azure-active-directory"></a>Debuggen SAML-basierten SSO-Anwendung in Azure Active Directory

Debuggen SAML-basierte Anwendungsintegration ist es oft hilfreich, verwenden Sie ein Tool wie [Fiddler](http://www.telerik.com/fiddler) SAML-Anforderung, Antwort SAML und tatsächlichen SAML-Token, das der Anwendung ausgegeben wird. Anhand des SAML-Tokens können Sie sicherstellen, dass alle erforderlichen Attribute, der Benutzername in der SAML-Antragsteller und Aussteller URI kommen über wie erwartet.

![][1]

Antwort von Azure Anzeige SAML-Token ist in der Regel die tritt nach HTTP 302 Umleiten von https://login.windows.net und konfigurierte **Antwort-URL** der Anwendung gesendet. 
 
Anzeigen des SAML-Tokens durch Auswählen dieser Zeile auswählen und dann die **Inspektoren > WebForms** auf der rechten Seite. Rechten Sie von dort **SAMLResponse** und wählen Sie **TextWizard senden**. Dann Menü **Von Base64** **Transformieren** zu decodieren das Token seinen Inhalt.
 
**Hinweis**: um diese HTTP-Anforderung zu sehen, Fiddler fordert Sie möglicherweise tun müssen Entschlüsselung von HTTPS-Datenverkehr konfigurieren.

## <a name="related-articles"></a>Verwandte Artikel

- [Artikel-Index für das Anwendungsmanagement in Azure Active Directory](active-directory-apps-index.md)
- [Einmaliges Anmelden für Clientanwendungen konfigurieren, die nicht in der Galerie Azure Active Directory](active-directory-saas-custom-apps.md)
- [Im SAML-Token für integrierte Apps ausgestellte Ansprüche anpassen](active-directory-saml-claims-customization.md)

<!--Image references-->
[1]: ./media/active-directory-saml-debugging/fiddler.png
