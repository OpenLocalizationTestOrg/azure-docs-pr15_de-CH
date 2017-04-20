<properties
    pageTitle="Azure MFA und AD FS | Microsoft Azure"
    description="Dies ist der Azure-mehrstufige Authentifizierungsseite, die Azure MFA mit AD FS Schritte beschreibt."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="yossib"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na" ms.topic="get-started-article"
    ms.date="10/17/2016"
    ms.author="kgremban"/>

# <a name="getting-started-with-azure-multi-factor-authentication-and-active-directory-federation-services"></a>Erste Schritte mit Azure mehrstufige Authentifizierung und Active Directory Federation Services



<center>![Cloud](./media/multi-factor-authentication-get-started-adfs/adfs.png)</center>

Wenn Ihrer Organisation lokale Active Directory Azure AD FS mit Active Directory verbundene hat, stehen zwei Optionen für die Verwendung von Azure mehrstufige Authentifizierung.

- Sichere Cloudressourcen Azure Mehrfaktor-Authentifizierung oder Active Directory Federation Services
- Sichere Cloud und lokale Ressourcen mithilfe von Azure mehrstufige Authentifizierungsserver

Die folgende Tabelle zeigt die Überprüfung Erfahrung Mittel Azure mehrstufige Authentifizierung mit AD FS

|Überprüfung Erfahrung - Apps durchsuchen basierend|Überprüfung Erfahrung - Browser basierende Apps
:------------- | :------------- | :------------- |
Azure AD Mittel Azure mehrstufige Authentifizierung |<li>Den ersten Schritt erfolgt lokal mit AD FS.</li> <li>Der zweite Schritt ist eine telefonische Methode Cloud Authentifizierung durchgeführt.</li>|Endbenutzer können app Kennwörter anmelden.
Sichern von Azure AD-Ressourcen mit Active Directory Federation Services |<li>Den ersten Schritt erfolgt lokal mit AD FS.</li><li>Der zweite Schritt ist lokal ausgeführte durch die Forderung berücksichtigt.</li>|Endbenutzer können app Kennwörter anmelden.

Mit app-Kennwörtern für Föderationsbenutzer Vorsichtsmaßnahmen:

- App-Kennwörter werden überprüft Authentifizierung Cloud, so dass sie Föderation umgehen. Föderation wird nur verwendet, wenn ein app-Kennwort einrichten.
- Lokalen Client Access Control Settings sind app Kennwörter nicht berücksichtigt.
- Verlieren Sie lokale Authentifizierung Protokollfunktion für app-Kennwörter.
- Konto deaktivieren und Löschen von dauert bis zu drei Stunden Verzeichnis synchronisieren deaktivieren/Löschen von app Kennwörter in die cloudidentität verzögern.

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Einrichten von Azure mehrstufige Authentifizierung oder Azure mehrstufige Authentifizierungsserver mit AD FS finden Sie in folgenden Artikeln:

- [Sichere Cloudressourcen Azure mehrstufige Authentifizierung mit AD FS](multi-factor-authentication-get-started-adfs-cloud.md)
- [Sichere Cloud und lokalen Ressourcen Azure mehrstufige Authentifizierungsserver mit Windows Server 2012 R2 AD FS](multi-factor-authentication-get-started-adfs-w2k12.md)
- [Sichere Cloud und lokalen Ressourcen Azure mehrstufige Authentifizierungsserver mit AD FS 2.0](multi-factor-authentication-get-started-adfs-adfs2.md)
