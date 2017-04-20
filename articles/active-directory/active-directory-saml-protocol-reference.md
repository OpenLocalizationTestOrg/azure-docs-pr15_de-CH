<properties
    pageTitle="Azure AD SAML Protokoll Referenz | Microsoft Azure"
    description="Dieser Artikel bietet eine Übersicht über Single Sign-On und einzelne Abmelde SAML Profile in Azure Active Directory."
    services="active-directory"
    documentationCenter=".net"
    authors="priyamohanram"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/23/2016"
    ms.author="priyamo"/>


# <a name="how-azure-active-directory-uses-the-saml-protocol"></a>Wie Active Directory Azure SAML-Protokoll verwendet

Azure Active Directory (Azure AD) verwendet das SAML 2.0 Protokoll Applikationen zu einer einzigen Anmeldung Benutzern aktivieren. [Einmaliges Anmelden](active-directory-single-sign-on-protocol-reference.md) und [Abmelden einzelne](active-directory-single-sign-out-protocol-reference.md) SAML Profile von Azure AD erklären wie SAML-Assertionen, Protokolle und Bindungen Service Provider Identität verwendet werden.

SAML-Protokoll erfordert Identitätsprovider (Azure AD) und dem Dienstanbieter (Anwendung), Informationen auszutauschen.

Beim Registrieren einer Anwendung in Azure AD registriert die Anwendungsentwickler Föderation Informationen in Azure AD. Dazu gehören **URI umleiten** und **Metadaten-URI** der Anwendung.

Azure AD verwendet **Metadaten-URI** der Cloud-Dienst den Signaturschlüssel und Logout URI der Cloud-Dienst abgerufen. Wenn die Anwendung einen Metadaten-URI nicht unterstützt, der Entwickler muss Support von Microsoft bieten die Abmeldung URI und Signaturschlüssel.

Azure Active Directory macht mandantenspezifische und allgemeine (Tenant-unabhängig) einzelne anmelden und einzelne Abmeldung Endpunkte. Diese URLs darstellen adressierbaren Speicherorte – sind nicht nur ein Bezeichner - Endpunkt zum Lesen der Metadaten gehen können.

 - Mandantenspezifische Endpunkt befindet sich unter `https://login.microsoftonline.com/<TenantDomainName>/FederationMetadata/2007-06/FederationMetadata.xml`.  Die <TenantDomainName> steht ein registrierter Domänenname oder TenantID GUID von Azure AD-Mandanten. Beispielsweise ist die Verbundmetadaten contoso.com Mieter: https://login.microsoftonline.com/contoso.com/FederationMetadata/2007-06/FederationMetadata.xml

- Tenant-unabhängige Endpunkt befindet sich unter `https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml`. In dieser Endpunktadresse erscheint **häufig** , statt eines Mandanten Domänen- oder ID

Finden Sie Informationen zu den Verbundmetadaten Azure AD veröffentlicht [Verbundmetadaten](active-directory-federation-metadata.md).
