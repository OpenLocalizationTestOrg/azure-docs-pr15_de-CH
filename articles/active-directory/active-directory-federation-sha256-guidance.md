<properties
    pageTitle="Änderung Signatur Hashalgorithmus für Office 365 Antworten Partei vertrauen | Microsoft Azure"
    description="Diese Seite enthält Richtlinien für SHA-Algorithmus für die Föderation Vertrauensstellung mit Office 365 ändern"
    keywords="SHA1, SHA256, Office 365, Föderation Aadconnect Adfs, Ad fs, Änderung Sha Föderation vertrauen verlassen Partei vertrauen"
    services="active-directory"
    documentationCenter=""
    authors="anandyadavmsft"
    manager="samueld"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/01/2016"
    ms.author="anandy"/>

# <a name="change-signature-hash-algorithm-for-office-365-replying-party-trust"></a>Ändern Sie Signatur Hashalgorithmus für Office 365 Partei vertrauen Antworten

## <a name="overview"></a>Übersicht

Azure Active Directory-Verbunddienste (AD FS) signiert ihre Tokens für Microsoft Azure Active Directory sicherstellen, dass sie manipuliert werden können. Diese Signatur kann auf SHA1 oder SHA256 basieren. Azure Active Directory unterstützt jetzt Token mit einem SHA256-Algorithmus signiert und wir empfehlen Algorithmus Token signieren, SHA256 für höchste Sicherheit. Dieser Artikel beschreibt die Schritte zum Signieren von Token Algorithmus sicherer SHA256 Ebene festgelegt.

## <a name="change-the-token-signing-algorithm"></a>Ändern des Algorithmus Token signieren

Nach dem Festlegen des Signaturalgorithmus mit einem der folgenden beiden Verfahren signiert AD FS Token für Office 365 Partei vertrauen SHA256 verlassen. Sie müssen zusätzliche Konfiguration ändern und diese Änderung hat keinen Einfluss auf Ihren Zugriff auf Office 365 oder anderen Azure AD-Anwendung.

### <a name="ad-fs-management-console"></a>AD FS-Verwaltungskonsole

1. Öffnen Sie die AD FS-Verwaltungskonsole auf dem Primärserver AD FS.
2. Erweitern Sie den Knoten AD FS und auf **Vertrauen verlassen**.
3. Ihr Office 365-Azure relying Party vertrauen Maustaste, und wählen Sie **Eigenschaften**.
4. Wählen Sie die Registerkarte **Erweitert** und sicheren Hashalgorithmus SHA256.
5. Klicken Sie auf **OK**.

![SHA256 Signaturalgorithmus - MMC](./media/active-directory-aadconnectfed-sha256guidance/mmc.png)

### <a name="ad-fs-powershell-cmdlets"></a>AD FS-PowerShell-cmdlets

1. Öffnen Sie auf jedem Server AD FS PowerShell unter Administratorrechte.
2. Einstellen des sicheren Hashalgorithmus das Cmdlet " **Set-AdfsRelyingPartyTrust** ".

 <code>Set-AdfsRelyingPartyTrust -TargetName 'Microsoft Office 365 Identity Platform' -SignatureAlgorithm 'http://www.w3.org/2001/04/xmldsig-more#rsa-sha256'</code>

## <a name="also-read"></a>Auch

* [Reparieren von Office 365 vertrauen Azure AD verbinden](./active-directory-aadconnect-federation-management.md#repairing-the-trust)
