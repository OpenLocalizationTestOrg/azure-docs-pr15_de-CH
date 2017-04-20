<properties
    pageTitle="Azure AD Connect und | Microsoft Azure"
    description="Diese Seite ist der zentrale Speicherort für alle Dokumentation zu AD FS-Operationen mit Azure AD verbinden"
    services="active-directory"
    documentationCenter=""
    authors="anandyadavmsft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="anandy"/>


# <a name="azure-ad-connect-and-federation"></a>Azure AD Connect und Föderation

Azure AD Connect können Sie konfigurieren, Föderation mit lokalen AD FS und Azure AD. Mit Föderation können Sie Benutzer Azure Anzeige Services mit ihrer lokalen Kennwörter auf das Unternehmensnetzwerk anmelden ohne ihr Kennwort erneut eingeben. Die Option Föderation mit AD FS können Sie zum Bereitstellen einer neuen oder vorhandenen AD FS in Windows Server 2012 R2 Farm angeben.

Dieses Thema ist die Homepage auf Informationen im Zusammenhang mit Funktionen für Azure AD verbinden und Listen alle anderen Themen zu. Links zu Azure AD Connect finden Sie unter Integration Ihrer lokalen Identitäten in Azure Active Directory.

## <a name="azure-ad-connect---federation-topics"></a>Azure AD Connect - Föderation Themen

| Thema | Was umfasst und beim Lesen |
|:------|:-----------|
| **Azure AD Connect-Anmeldung Benutzeroptionen** ||
| [Grundlegendes zur Benutzer-Optionen](active-directory-aadconnect-user-signin.md) | Beschreibung der verschiedenen Benutzer-Anmeldeoptionen und Einfluss Azure Benutzer |
| **AD FS-Installation mit Azure AD verbinden**||
| [Erforderliche Komponenten](active-directory-aadconnect-get-started-custom.md#ad-fs-configuration-pre-requisites) | Voraussetzung für eine erfolgreiche Installation AD FS über Azure AD Connect|
| [AD FS-Farm konfigurieren](active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs) | Installieren Sie eine neue AD FS-Farm mit Azure AD verbinden |
| **Ändern der AD FS-Konfiguration** | |
| [Repariert die Vertrauensstellung](active-directory-aadconnect-federation-management.md#reparing-the-trust) | Reparieren der aktuellen Vertrauensstellung zwischen lokalen AD FS und Office 365 / Azure |
| [Hinzufügen eines neuen AD FS-Servers](active-directory-aadconnect-federation-management.md#adding-a-new-ad-fs-server) | Erweitern von AD FS-Farm mit zusätzlichen AD FS-Erstinstallation Server buchen |
| [Hinzufügen eines neuen AD FS WAP-Servers](active-directory-aadconnect-federation-management.md#adding-a-new-wap-server) | Erweitern von AD FS-Farm mit zusätzlichen WAP-Erstinstallation Server buchen |
| [Hinzufügen einer neuen föderierten Domäne](active-directory-aadconnect-federation-management.md#add-a-new-federated-domain) | Hinzufügen einer anderen Domäne mit Azure verbunden sein |
|**Aufgaben nach der installation**||
| [Benutzerdefinierte Firmenlogo / Abbildung](active-directory-aadconnect-federation-management.md#add-custom-company-logo-or-illustration)| Ändern der kann durch Angabe von benutzerdefinierten Logos, die auf dem AD FS-Seite angezeigt werden |
| [Anmelden Beschreibung](active-directory-aadconnect-federation-management.md#add-sign-in-description) | Anmelden Beschreibung der AD FS-Seite ändern | 
| [Ändern von AD FS Anspruch Regeln](active-directory-aadconnect-federation-management.md#modifying-ad-fs-claim-rules) | Ändern Sie / fügen Sie Anspruchsregeln in AD FS Azure AD Connect Sync Konfiguration entspricht hinzu |


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Integration von lokalen Identitäten in Azure Active Directory](active-directory-aadconnect.md)
* [AD FS-Bereitstellung in Azure](active-directory-aadconnect-azure-adfs.md)
* [Hohe Verfügbarkeit zwischen geografischen AD FS-Bereitstellung in Azure mit Azure Traffic Manager](active-directory-adfs-in-azure-with-azure-traffic-manager.md)


