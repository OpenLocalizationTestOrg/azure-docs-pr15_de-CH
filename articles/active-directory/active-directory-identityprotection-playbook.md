<properties
    pageTitle="Azure Active Directory Schutz Playbook | Microsoft Azure"
    description="Erfahren Sie, wie Azure AD-Schutz ermöglicht die Fähigkeit der Angreifer eine gefährdete Identität oder Gerät und sichere Identität oder ein Gerät, das zuvor vermutet oder gefährdet bezeichnet."
    services="active-directory"
    keywords="Azure active Directory Schutz, Cloud app Discovery, Verwalten von Applikationen, Sicherheit, Risiken, Risiko, Schwachstelle, Sicherheitsrichtlinien"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="markvi"/>

#<a name="azure-active-directory-identity-protection-playbook"></a>Azure Active Directory Schutz playbook 

Diese Playbook hilft Ihnen:

- Daten in der Umgebung Schutz durch Simulation Risikoereignisse und Schwachstellen Auffüllen
- Risikobasierte bedingte Richtlinien einrichten und Testen der Auswirkung dieser Richtlinien


## <a name="simulating-risk-events"></a>Simulieren von Risiken

Dieser Abschnitt enthält Schritte zum Simulieren von Ereignistypen von Risiken:

- Anmelden von anonymen Adressen (einfach)
- Anmelden von unbekannten Standorten (Mittel)
- Willkommen untypische Speicherorte (schwer)

Andere Risikoereignisse können nicht auf sichere Weise simuliert werden.


### <a name="sign-ins-from-anonymous-ip-addresses"></a>Anmelden von anonymen Adressen

Dieses Ereignistyps Risiko identifiziert Benutzer, die eine IP-Adresse angemeldet haben, die anonyme Proxy IP-Adresse identifiziert. Diese Proxys werden von Personen verwendet, die IP-Adresse des Geräts ausblenden und böswillige Absicht verwendet werden.

**Simulieren einer Anmeldung eine anonyme IP folgendermaßen**:

1.  [Tor Browser](https://www.torproject.org/projects/torbrowser.html.en)herunterladen
2.  Navigieren Sie Tor Browser zu [https://myapps.microsoft.com](https://myapps.microsoft.com).   
3.  Geben Sie die Anmeldeinformationen des Kontos, das im Bericht **Anmeldungen von anonymen Adressen** angezeigt werden soll.

Die Anmeldung wird im Schaltpult Schutz innerhalb von 5 Minuten angezeigt. 


###<a name="sign-ins-from-unfamiliar-locations"></a>Anmelden von unbekannten Standorten

Unbekannten Standorten besteht ein Echtzeit-evaluierungsmechanismus, der nach Anmeldung Standorte berücksichtigt (IP, Breite / Länge und ASN) neue und unbekannte Standorte bestimmen. Das System speichert die vorherigen IPs Latitude / Längen- und ASNs eines Benutzers und den bekannten Standorten. Ein Anmeldestandort vertraut gilt nicht vorhandene bekannte Speicherorte anmelden Position übereinstimmt.

Azure Active Directory-Schutz:  

 - hat eine anfängliche lernen von 14 Tagen, während, die keine neue Speicherorte als unbekannte Speicherorte kennzeichnen.
 - ignoriert Anmeldungen vertraut und geografisch Nähe einer vorhandenen vertraut sind.

Simulieren von unbekannten Standorten müssen Sie anmelden, Speicherort und Gerät das Konto nicht vor angemeldet hat. 


**Gehen Sie folgendermaßen vor, um eine Anmeldung von einem unbekannten Speicherort zu simulieren**:

1.  Wählen Sie ein Konto mit mindestens 14-Tage-Verlauf. 

2.  Gehen Sie vor:
    
    ein. Verwenden eines VPN, navigieren Sie zu [https://myapps.microsoft.com](https://myapps.microsoft.com) und geben Sie die Anmeldeinformationen des Kontos Risikoereignis für simulieren möchten.

    b. Bitten Sie Mitarbeiter an einem anderen Speicherort der (nicht empfohlen) Anmeldeinformationen anmelden.

Die Anmeldung wird im Schaltpult Schutz innerhalb von 5 Minuten angezeigt.
 
### <a name="impossible-travel-to-atypical-location"></a>Reisen untypische an
Simuliert die Bedingung Reisen ist schwierig, da der Algorithmus Computer zur auszusondern Fehlalarme herkömmlicher Geräte Reisen oder Anmeldungen von VPNs, die von anderen Benutzern im Verzeichnis verwendet werden. Erforderlich Algorithmus eine-in der Geschichte von 3 bis 14 Tagen für den Benutzer vor dem Beginn Risikoereignisse generieren.

**Simulieren unmöglich Reisen untypische an folgendermaßen**:

1.  Navigieren Sie in Ihrem Standardbrowser zu [https://myapps.microsoft.com](https://myapps.microsoft.com).  

2.  Geben Sie die Anmeldeinformationen des Kontos, das Sie für ein Ereignis Reisen Risiko erstellen möchten.

3.  Ändern des Benutzer-Agents. Sie können Benutzer-Agent in Internet Explorer von Entwicklertools ändern oder ändern den Benutzer-Agent in Firefox oder Chrome mit einer Benutzer-Agent Switcher Add-on.

4.  Ändern Sie die IP-Adresse. Sie können Ihre IP-Adresse ändern, VPN ein Add-on Tor oder einen neuen Computer in Azure in einem anderen Rechenzentrum dreht.

5.  Anmelden Sie dieselben Anmeldeinformationen wie zuvor mit [https://myapps.microsoft.com](https://myapps.microsoft.com) und innerhalb weniger Minuten nach der vorherigen Zeichen.

Die Anmeldung wird im Dashboard Schutz innerhalb von 2 bis 4 Stunden angezeigt.<br>
Durch learning Modelle komplexer Computer besteht die Möglichkeit, die er nicht abgeholt wird.<br> Sie können diese Schritte für mehrere Azure AD-Konten replizieren möchten.


## <a name="simulating-vulnerabilities"></a>Schwachstellen simulieren 
Schwachstellen werden Sicherheitslücken in Azure AD-Umgebung, die von einem schlecht ausgenutzt werden können. Derzeit sind 3 Arten von Sicherheitslücken in Azure AD-Schutz, die andere Funktionen von Azure AD nutzen aufgetaucht. Diese Sicherheitslücken Identitätsschutz Schaltpult erscheint automatisch sobald diese Funktionen eingerichtet sind.

-   Azure AD [mehrstufige Authentifizierung?](../multi-factor-authentication/multi-factor-authentication.md)
-   Azure AD [Cloud App Discovery](active-directory-cloudappdiscovery-whatis.md).
-   Azure AD [privilegierten Identitätsmanagement](active-directory-privileged-identity-management-configure.md). 



##<a name="user-compromise-risk"></a>Risiko der Kompromittierung eines Benutzers

**Testen Kompromiss Gefahr, gehen Sie folgendermaßen vor**:

1.  Anmelden bei [https://portal.azure.com](https://portal.azure.com) mit globalen Administratorrechten für Ihren Mandanten.

2.  Navigieren Sie zum **Schutz der Identität**. 

3.  ** **Azure AD Identitätsschutz** Haupt-Blade klicken.** 

4.  Blatt **Portal Einstellungen** **Sicherheitsregeln**, klicken Sie auf **Benutzer beeinträchtigen Risiko**. 

5.  Blade **Risiko anmelden** deaktivieren Sie **Regel aktivieren** , und klicken Sie dann auf **Speichern** .

6.  Für ein bestimmtes Benutzerkonto simulieren einen unbekannten Standorten oder anonyme IP Risikoereignis. Dies wird die Risikostufe Benutzer für diesen Benutzer **Mittel**erhöhen.

7.  Warten Sie einige Minuten, und stellen Sie sicher, dass Benutzer Ihre Benutzer **Mittel**ist.

8.  Gehe zu Blatt **Portal Einstellungen** .

9.  Das **Risiko der Gefährdung** Blade unter **Regel aktivieren**wählen Sie **auf** . 

10. Wählen Sie eine der folgenden Optionen:

    ein. Wählen Sie blockieren, **Mittel** **Block anmelden**.

    b. Wählen Sie enforce Passwort ändern **Mittel** **mehrstufige Authentifizierung**.

13. Klicken Sie auf **Speichern**.

14. Anmelden mit einem Benutzer mit einem erhöhten Risiko können Sie jetzt risikobasierte bedingten Zugriff testen. Wenn Benutzer Mittel besteht, je nach Konfiguration Ihres Anmeldename entweder blockiert oder Sie Ihr Kennwort ändern müssen. 
<br><br>
![Playbook](./media/active-directory-identityprotection-playbook/201.png "Playbook")
<br>

 
##<a name="sign-in-risk"></a>Anmelden Risiko

 
**Um ein Risiko zu testen, führen Sie die folgenden Schritte:**

1.  Anmelden bei [https://portal.azure.com](https://portal.azure.com) mit globalen Administratorrechten für Ihren Mandanten.

2.  Navigieren Sie zum **Schutz der Identität**.

3.  ** **Azure AD Identitätsschutz** Haupt-Blade klicken.** 

4.  Klicken Sie auf Blatt **Portal Einstellungen** **Sicherheitsregeln**auf **Risiko anmelden**.

5.  Wählen Sie auf Blade **Risiko anmelden ** **auf** unter **Regel aktivieren**. 

7.  Wählen Sie eine der folgenden Optionen:

    ein. Wählen Sie blockieren, **Mittel** **Block anmelden**

    b. Wählen Sie enforce Passwort ändern **Mittel** **mehrstufige Authentifizierung**.

8.  Wählen Sie blockieren, Mittel Block anmelden.

9.  Wählen Sie um mehrstufige Authentifizierung erzwingen, **Mittel** **mehrstufige Authentifizierung**.

10. Klicken Sie auf **Speichern**.

11. Sie können jetzt Zugangskontrolle risikobasierte testen, von unbekannten Standorten oder anonyme IP Risikoereignisse sind beide **Mittel** Risikoereignisse simulieren.

<br>
![Playbook](./media/active-directory-identityprotection-playbook/200.png "Playbook")
<br>


## <a name="see-also"></a>Siehe auch

 - [Azure Active Directory-Schutz](active-directory-identityprotection.md)
