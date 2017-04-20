<properties
    pageTitle="Azure MFA Vs Cloudserver | Microsoft Azure"
    description="Wählen Sie die kombinierte Authentifizierung Sicherheitslage Lösung richtig gefragt, welche am i sichern und wo Benutzer befindet.  Wählen Sie Cloud MFA-Server oder AD FS."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="yossib"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/14/2016"
    ms.author="kgremban"/>

#<a name="choose-the-azure-multi-factor-authentication-solution-for-you"></a>Wählen Sie die Azure Multi-Factor Authentication-Lösung

Da gibt es verschiedene Arten von Azure mehrstufige Authentifizierung (MFA), müssen wir beantworten Fragen zu ermitteln, welche Version die richtige ist.  Diese Fragen sind:

-   [Was habe ich sichern](#what-am-i-trying-to-secure)
-   [Wo befinden sich die Benutzer](#where-are-the-users-located)
- [Benötige wozu ich?](#what-featured-do-i-need)

Die folgenden Abschnitte enthalten Hinweise auf diese Antworten festlegen.

## <a name="what-am-i-trying-to-secure"></a>Was habe ich sichern?

Ermitteln der richtigen Überprüfung benötigen, müssen zuerst wir die Frage, was Sie mit einer zweiten Authentifizierung sichern beantworten.  Ist eine Anwendung in Azure?  Oder remote-Zugriff?  Ermittelt, was wir sichern möchten, beantworten wir die Frage, mehrstufige Authentifizierung muss aktiviert sein.  


Was möchten Sie schützen| Mehrstufige Authentifizierung in der cloud|Mehrstufige Authentifizierungsserver
------------- | :-------------: | :-------------: |
Erstanbieter-Microsoft apps|● |● |
SaaS-apps in der Galerie|● |● |
IIS-Anwendung über Azure AD App Proxy veröffentlicht|● |● |
IIS-Anwendung nicht über Azure AD App Proxy veröffentlicht | |● |
RAS wie VPN RDG| |● |



## <a name="where-are-the-users-located"></a>Wo befinden sich die Benutzer

Anschließend kann befinden sich unsere Benutzer betrachten die richtige Lösung in der Cloud oder lokal mit MFA-Server verwenden.



Speicherort| Mehrstufige Authentifizierung in der cloud|Mehrstufige Authentifizierungsserver
------------- | :-------------: | :-------------: |
Azure Active Directory|● | |
Azure AD und lokalen Föderation mit AD FS mit AD|● |● |
Azure AD und lokale AD mit DirSync Azure AD Sync Azure AD Connect - ohne Kennwort synchronisieren|● |● |
Azure AD und lokale Kennwort synchronisieren mit DirSync Azure AD Sync Azure AD Connect - Anzeige|● | |
Lokale Active Directory| |● |

## <a name="what-features-do-i-need"></a>Benötige wozu ich?

Die folgende Tabelle enthält einen Vergleich der Funktionen, die kombinierte Authentifizierung in der Cloud und mehrstufige Authentifizierungsserver verfügbar sind.

 | Mehrstufige Authentifizierung in der cloud | Mehrstufige Authentifizierungsserver
------------- | :-------------: | :-------------: |
Mobile-app-Benachrichtigung als zweiter Faktor | ● | ● |
Mobile-app Verifizierungscode als zweiter Faktor | ● | ●
Anruf als zweite Faktor | ● | ●
Unidirektionale SMS als zweite Faktor | ● | ●
Bidirektionale SMS als zweite Faktor |  | ●
Hardware-Tokens als zweite Faktor |  | ●
App-Kennwörter für Clients, die keine MFA unterstützen | ● |  
Admin Kontrolle über Authentifizierungsmethoden | ● | ●
PIN-Modus |  | ●
Betrug | ● | ●
MFA-Berichte | ● | ●
Einmalige umgehen |  | ●
Benutzerdefinierte Ansage für Anrufe | ● | ●
Anpassbare Anrufer-ID für Anrufe | ● | ●
Vertrauenswürdige IP-Adressen | ● | ●
MFA für vertrauenswürdige Geräte speichern  | ● |  
Zugangskontrolle | ● | ●
Cache |  | ●

Da ermittelt Cloud mehrstufige Authentifizierung oder die lokal MFA-Server kann es losgehen einrichten und Azure mehrstufige Authentifizierung. **Wählen Sie das Symbol für das Szenario.**

<center>




[![Cloud](./media/multi-factor-authentication-get-started/cloud2.png)](multi-factor-authentication-get-started-cloud.md)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[![Proofup](./media/multi-factor-authentication-get-started/server2.png)](multi-factor-authentication-get-started-server.md)  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
</center>
