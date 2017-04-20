<properties
    pageTitle="Arbeiten mit benutzerdefinierten Domänen in Azure AD-Anwendungsproxy | Microsoft Azure"
    description="Deckblätter bearbeiten wie benutzerdefinierte Domänen in Azure AD-Anwendungsproxy"
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

# <a name="working-with-custom-domains-in-azure-ad-application-proxy"></a>Arbeiten mit benutzerdefinierten Domänen in Azure AD-Anwendungsproxy

Mit der Standarddomäne können Sie dieselbe URL als interne und externe URL für den Zugriff auf die Anwendung, so dass die Benutzer nur eine URL an die App, egal wo sie zugreifen festlegen. Hiermit können Sie eine Verknüpfung in die Abdeckung für die Anwendung erstellen. Verwenden von Azure AD-Anwendungsproxy bereitgestellten Standarddomäne ist keine weitere Konfiguration Sie die Domäne aktivieren müssen. Die Verwendung einer benutzerdefinierten Domäne sind einige Dinge zu tun, um sicherzustellen, dass Anwendungsproxy Ihre Domäne erkennt und ihre Zertifikate überprüft.

## <a name="selecting-your-custom-domain"></a>Ihre benutzerdefinierte Domain auswählen

1. Veröffentlichen Sie die Anwendung nach der Anleitung in [Clientanwendungen mit Anwendungsproxy veröffentlichen](active-directory-application-proxy-publish.md).
2. Wenn die Anwendung in der Liste der Programme angezeigt wird, wählen sie und klicken Sie auf **Konfigurieren**.
3. Geben Sie unter **Externe URL**Ihrer benutzerdefinierten Domäne.
4. Ist Ihre externe URL Https, werden Sie aufgefordert, ein Zertifikat hochladen, Azure die URL der Anwendung überprüfen kann. Außerdem können Sie ein Platzhalterzertifikat der externen URL der Anwendung. Diese Domäne muss innerhalb der Liste der Ihre [Azure Domänen überprüft](https://msdn.microsoft.com/library/azure/jj151788.aspx). Azure muss ein Zertifikat für die Domänen-URL oder ein Platzhalterzertifikat der externen URL für die Anwendung.
5. Stellen Sie sicher, der die interne URL an die Anwendung weitergeleitet, die Ihnen die gleiche URL für internen und externen Zugriff und eine einzelne Verknüpfung für die Anwendung in der Liste der Programme ermöglicht DNS-Datensatz hinzufügen.

## <a name="frequently-asked-questions-about-working-with-custom-domains"></a>Häufig gestellte Fragen zum Arbeiten mit benutzerdefinierten Domänen

F: auswählen kann ich ein Zertifikat bereits hochgeladen, ohne erneut hochladen?  
A: zuvor hochgeladene Zertifikate werden automatisch an eine Anwendung gebunden und genau ein Zertifikat mit Hostnamen der Anwendung.  

F: wie kann ich ein Zertifikat hinzufügen und Format des exportierten Zertifikats in übertragen werden sollen?  
A: das Zertifikat sollte auf der Konfigurationsseite Anwendung hochgeladen werden. Das Zertifikat sollte eine PFX-Datei.  

F: werden können ECC-Zertifikate verwendet?  
A: Es gibt keine explizite Einschränkung für Signaturmethoden.  

F: werden können SAN-Zertifikate verwendet?  
A: Ja.  

F: werden können Platzhalter-Zertifikate verwendet?  
A: Ja.  

F: werden kann auf jede Anwendung ein anderes Zertifikat verwendet?  
A: Ja, gegenseitige denselben externen Server teilen.  

F: Wenn ich eine neue Domäne registrieren, kann ich diese Domäne verwenden?  
A: Ja, wird die Liste der Domänen des Mieters verifizierten Domäne aus.  

Was geschieht, wenn ein Zertifikat abläuft?  
A: Sie erhalten eine Warnung im zertifikatsabschnitt auf der Konfigurationsseite Anwendung. Wenn ein Benutzer versucht, die Anwendung zugreifen, wird ein Sicherheitshinweis angezeigt.  

Was ist zu tun, ggf. ein Zertifikat für eine bestimmte Anwendung ersetzen?  
A: Hochladen Sie ein neues Zertifikat auf Konfiguration der Anwendung.  

F: kann ich löschen ein Zertifikat und ersetzen?  
A: Wenn Sie ein neues Zertifikat hochladen und das alte Zertifikat nicht von einer anderen Anwendung verwendet wird, wird sie automatisch gelöscht.  

Was geschieht, wenn ein Zertifikat gesperrt wird?  
A: Sperrung überprüft werden Zertifikate nicht ausgeführt. Wenn ein Benutzer auf die Anwendung der Browser versucht möglicherweise ein Sicherheitshinweis angezeigt.  

F: verwenden kann ein selbstsigniertes Zertifikat?  
A: Ja, dürfen selbstsignierte Zertifikate. Beachten Sie, dass bei Verwendung eine privaten Zertifizierungsstelle, CDP (Verteilungspunkt Certificate Revocation Punkt) für das Zertifikat öffentlich sein soll.  

F: Gibt es Platz alle Zertifikate für Mandanten?  
A: Dies ist in der aktuellen Version nicht unterstützt.  


## <a name="see-also"></a>Siehe auch

- [Veröffentlichen Sie mit Anwendungsproxy](active-directory-application-proxy-publish.md)
- [Einmaliges Anmelden aktivieren](active-directory-application-proxy-sso-using-kcd.md)
- [Bedingter Zugriff](active-directory-application-proxy-conditional-access.md)
- [Azure AD benutzerdefinierten Domänennamen hinzufügen](active-directory-add-domain.md)

Überprüfen Sie die aktuellsten Neuigkeiten und Updates [Anwendungsproxy blog](http://blogs.technet.com/b/applicationproxyblog/)
