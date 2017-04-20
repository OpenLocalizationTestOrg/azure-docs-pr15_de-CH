<properties
    pageTitle="Azure AD Connect Cloud von Microsoft Deutschland"
    description="Azure AD-Verbindung werden die lokalen Verzeichnisse Azure Active Directory integriert. Dadurch zu einer gemeinsamen Identität für Office 365, Azure und SaaS in Azure AD integriert."
    keywords="Einführung in Azure AD Connect Azure AD Connect Übersicht was Azure AD Connect ist active Directory installieren Deutschland schwarzen Wald"
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/08/2016"
    ms.author="billmath"/>

#<a name="azure-ad-connect-in-microsoft-cloud-germany---public-preview"></a>Azure AD Connect Deutschlands Microsoft Cloud - Public Preview

## <a name="introduction"></a>Einführung
Azure AD Connect bietet Synchronisierung zwischen der lokalen Active Directory und Active Directory Azure.
Derzeit viele der Szenarien in [Microsoft Cloud Deutschland](https://www.microsoft.com/de-de/cloud/deutschland/default.aspx) vom Betreiber erfolgt. Bei Microsoft Cloud Deutschland muss Folgendes sein:


- Die folgenden URLs müssen einen Proxyserver für die Synchronisierung erfolgreich geöffnet werden:
    - *. microsoftonline.de
    - *. Windows
    - + Zertifikatssperrlisten

- Wenn Sie Ihre Azure AD-Verzeichnis anmelden, verwenden Sie ein Konto im Bereich onmicrosoft.de.
- Die folgenden Features sind nicht verfügbar:
    - Azure AD Connect Health
    - Automatische updates
    - Kennwort Rückschreiben

## <a name="download"></a>Herunterladen
Sie können Azure AD Connect aus dem Azure AD Connect Blade im Portal.  Suchen Sie Azure AD Connect-Blade mit beschrieben.

### <a name="the-azure-ad-connect-blade"></a>Azure AD Connect Blade

Nachdem Sie den Azure-Portal angemeldet haben, führen Sie folgende Schritte aus:

1. Gehe zu durchsuchen
2.  Wählen Sie Azure Active Directory
3.  Wählen Sie Azure AD verbinden

Sie sollten Folgendes angezeigt:

![Azure AD Connect Blade](media\active-directory-aadconnect-germany\germany1.png)

 
Die folgende Tabelle beschreibt die Funktionen auf dem Blatt angezeigt.


Titel|Beschreibung|
----- | ----- |
SYNCHRONISIERUNGSSTATUS|Wir wissen Sie, ob die Synchronisierung aktiviert oder deaktiviert ist.|
LETZTE SYNCHRONISIERUNG|Der letzten Synchronisierung erfolgreichen abgeschlossen.|
FÖDERATIONEN|Zeigt die Anzahl der Föderationen zurzeit konfiguriert.|


## <a name="installation"></a>Installation
Zum Installieren von Azure AD Connect können Sie die Dokumentation [hier](active-directory-aadconnect.md#install-azure-ad-connect).

## <a name="advanced-features-and-additional-information"></a>Erweiterte Funktionen und Weitere Informationen
Weitere Informationen und Hinweise auf benutzerdefinierte Einstellungen oder Konfigurationen beginnen Sie mit [Ihrer lokalen Identitäten Azure Active Directory integrieren](active-directory-aadconnect.md).  Diese Seite enthält Informationen und Links zu zusätzliche Anleitung.
