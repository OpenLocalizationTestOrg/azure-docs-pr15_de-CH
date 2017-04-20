<properties
    pageTitle="Azure AD verbinden: Häufig gestellte Fragen | Microsoft Azure"
    description="Diese Seite enthält häufig gestellte Fragen zu Azure AD verbinden."
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/08/2016"
    ms.author="billmath"/>

# <a name="azure-ad-connect-faq"></a>Azure AD Connect FAQ

## <a name="general-installation"></a>Allgemeine installation
**F: funktioniert Installation hat Azure AD globaler Administrator 2FA aktiviert?**  
Builds ab Februar 2016 wird dies unterstützt.

**F: Gibt es eine Möglichkeit, Azure AD Verbindung unbeaufsichtigte Installation?**  
Es werden nur Azure AD Connect mithilfe des Assistenten installieren. Eine unbeaufsichtigte und automatische Installation wird nicht unterstützt.

**F: Ich habe eine Gesamtstruktur, in einer Domäne hergestellt werden. Installieren Azure AD verbinden**  
Builds ab Februar 2016 wird dies unterstützt.

**F: funktioniert der AD DS-Agent auf Server Core?**  
Ja. Nach der Installation des Agents können Sie mit dem folgenden PowerShell-Cmdlet registrieren: 

`Register-AzureADConnectHealthADDSAgent -Credentials $cred`

## <a name="network"></a>Netzwerk
**F: Ich habe eine Firewall, die Netzwerkgeräte oder etwas, das beschränkt die maximale Zeit Verbindungen im Netzwerk geöffnet bleiben kann. Wie lange sollte mein Client Side Timeoutschwellenwert bei Azure AD verwenden?**  
Alle Netzwerksoftware, Geräten oder alles, was die Verbindung geöffnet bleiben können die maximale Zeit begrenzt verwenden einen Schwellenwert von mindestens 5 Minuten (300 Sekunden) für die Verbindung zwischen Server, Azure AD Connect-Client installiert ist, und Azure Active Directory. Dies gilt auch für alle zuvor veröffentlichten Microsoft Identity Synchronisierungsprogramme.

**Gibt SLDs (einzelne Bezeichnung Domänen) unterstützt?**  
Nein, Azure AD Connect unterstützt lokale Gesamtstrukturen/Domänen SLDs verwenden.

**F: sind NetBios namens "dotted" unterstützt?**  
Nein, Azure AD Connect unterstützt lokale Gesamtstrukturen/Domänen, in denen der NetBIOS-Name einen Punkt enthält "." im Namen.

## <a name="federation"></a>Föderation
**Was tun, wenn ich eine e-Mail erhalten, dass ich meine Office 365 Zertifikat erneuern**  
Verwenden Sie die Anleitung im Thema zum Erneuern der Zertifikate [Erneuern von Zertifikaten](active-directory-aadconnect-o365-certs.md) erläutert.

**F: Ich habe "Automatisch aktualisieren relying Party" für Office 365 relying Party festgelegt. Müssen Maßnahmen ergreifen, wenn mein Tokensignaturzertifikat automatisch übernommen werden?**  
Verwenden Sie die Anleitung im Artikel [Erneuern von Zertifikaten](active-directory-aadconnect-o365-certs.md)beschrieben.

## <a name="environment"></a>Umgebung
**F: unterstützt ist es den Server nach der Installation von Azure AD Connect umbenennen?**  
Nein. Ändern des Servernamens bewirkt, dass das Synchronisierungsmodul nicht mit der SQL-Datenbank herstellen können, und der Dienst nicht gestartet werden.

## <a name="identity-data"></a>Identitätsdaten
**F: UPN (UserPrincipalName) Attributs in Azure AD entspricht auf Prem UPN - nicht warum?**  
Finden Sie in folgenden Artikeln:

- [Benutzernamen in Office 365, Azure oder Intune übereinstimmen nicht lokalen UPN oder alternativen Benutzernamen.](https://support.microsoft.com/en-us/kb/2523192)
- [Änderungen werden nicht von Azure Active Directory Sync-Tool synchronisiert, nach dem Ändern des UPN des Benutzers Verwendung eine andere föderierten Domäne](https://support.microsoft.com/en-us/kb/2669550)

Sie können auch Azure AD damit das Synchronisierungsmodul UserPrincipalName in [Azure AD Connect Service Synchronisierungsfunktionen](active-directory-aadconnectsyncservice-features.md)beschrieben aktualisieren können.

## <a name="custom-configuration"></a>Konfiguration
**F: Wo sind die PowerShell-Cmdlets für Azure AD Connect dokumentiert?**  
Mit Ausnahme der auf dieser Website dokumentiert Cmdlets sind andere PowerShell-Cmdlets in Azure AD Connect gefunden für Kunden nicht unterstützt.

* *F: kann ich verwenden "Export Servern importieren" gefunden *Synchronisierung Service Manager* Konfiguration wechseln Servers? **  
Nein. Diese Option alle Einstellungen abgerufen und sollte nicht verwendet werden. Verwenden Sie stattdessen den Assistenten zu erstellen die Basiskonfiguration auf dem zweiten Server synchronisieren Regeleditor zu PowerShell-Skripts, um benutzerdefinierte Regel zwischen Servern zu verschieben. Finden Sie unter [Konfiguration von aktiv in staging-Server verschieben](active-directory-aadconnect-upgrade-previous-version.md#move-custom-configuration-from-active-to-staging-server).

## <a name="troubleshooting"></a>Problembehandlung
**F: Wie erhalte ich Hilfe bei Azure AD verbinden?**

[Die Microsoft Knowledge Base (KB)](https://www.microsoft.com/en-us/Search/result.aspx?q=azure%20active%20directory%20connect&form=mssupport)

- Durchsuchen der Microsoft Knowledge Base (KB) technischen Lösungen für allgemeine Probleme zur Unterstützung von Azure AD verbinden.

[Microsoft Azure Active Directory Foren](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WindowsAzureAD)

- Suchen und Durchsuchen von technischen Fragen und Antworten aus der Community oder eigene Frage durch Klicken auf [hier](https://social.msdn.microsoft.com/Forums/azure/en-US/newthread?category=windowsazureplatform&forum=WindowsAzureAD&prof=required).

[Azure AD Connect-Kundensupport](https://manage.windowsazure.com/?getsupport=true)

- Verwenden Sie diesen Link Azure-Portal zu unterstützen.
