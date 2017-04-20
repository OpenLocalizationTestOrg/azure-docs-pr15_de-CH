<properties 
    pageTitle="Azure mehrstufige Authentifizierung – Funktionsweise"
    description="Azure mehrstufige Authentifizierung hilft Schutz Zugriff auf Daten und Applikationen Benutzer Nachfrage nach einem einfachen Vorgang. Es bietet zusätzliche Sicherheit durch eine zweite Form der Authentifizierung anfordern und bietet eine starke Authentifizierung über eine Reihe von einfachen Überprüfungsoptionen."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtland"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

#<a name="how-azure-multi-factor-authentication-works"></a>Funktionsweise von Azure mehrstufige Authentifizierung

Die Sicherheit der kombinierten Authentifizierung liegt in der Ebenen. Dabei mehrere Authentifizierungsstufen stellt eine erhebliche Herausforderung für Angreifer. Selbst wenn ein das Kennwort des Benutzers erfahren Angreifer, ist es ohne Besitz vertrauenswürdiges Gerät nutzlos. Der Benutzer das Gerät verlieren, werden gefundenen Person sie verwenden, wenn sie auch das Kennwort des Benutzers kennt.

![Proofup](./media/multi-factor-authentication-how-it-works/howitworks.png)



Azure mehrstufige Authentifizierung hilft Schutz Zugriff auf Daten und Applikationen Benutzer Nachfrage nach einem einfachen Vorgang.  Es bietet zusätzliche Sicherheit durch eine zweite Form der Authentifizierung anfordern und strenge Authentifizierung über eine Reihe von einfachen Überprüfungsoptionen bietet:

- Telefonanruf
- Textnachricht
- Mobile-app-Benachrichtigung-Benutzer die Methode sie bevorzugen
- Verifizierungscode Mobile-app
- 3. Partei EID Token

Weitere oh Funktionsweise finden Sie im folgende Video.

>[AZURE.VIDEO multi-factor-authentication-deep-dive-securing-access-on-premises]

##<a name="methods-available-for-multi-factor-authentication"></a>Methoden für mehrstufige Authentifizierung
Wenn sich ein Benutzer anmeldet, wird eine zusätzliche Überprüfung an den Benutzer gesendet.  Nachfolgend eine Liste der Methoden, die für diese zweite Überprüfung verwendet werden kann.

Verifizierungsmethode  | Beschreibung
------------- | ------------- |
Telefonanruf | Ein Anruf an einen Benutzer Smartphone bitten um sicherzustellen, dass sie Anmelden durch das Zeichen # drücken.  Dadurch wird die Überprüfung abgeschlossen.  Diese Option ist konfigurierbar und kann geändert werden, um einen Code, den Sie angeben.
Textnachricht | Smartphone mit 6 Ziffern des Benutzers wird eine Textnachricht erhalten.  Geben Sie diesen Code in die Überprüfung abgeschlossen.
Mobile-App-Benachrichtigung | Smartphone bitten vollständige Überprüfung der app überprüfen auswählen eines Benutzers eine Anforderung Überprüfung erhalten. Dies geschieht, wenn app-Benachrichtigung als primäre Überprüfungsmethode ausgewählt.  Wenn sie diese erhalten, wenn sie nicht anmelden, können sie es als Betrug melden.
Verifizierungscode Mobile-App | Verifizierungscode an die app erhalten, die auf Smartphone des Benutzers ausgeführt wird.  Dies geschieht, wenn primäre Bestätigungsmethode Verifizierungscode gewählt.


##<a name="available-versions-of-azure-multi-factor-authentication"></a>Verfügbaren Versionen von Azure mehrstufige Authentifizierung
Azure mehrstufige Authentifizierung ist in drei verschiedenen Versionen erhältlich.  In der folgenden Tabelle werden die einzelnen näher beschrieben.

Version  | Beschreibung
------------- | ------------- |
Mehrstufige Authentifizierung für Office 365 | Diese Version funktioniert ausschließlich mit Office 365 und wird von Office 365-Portal. So können Administratoren nun schützen ihre Office 365-Ressourcen mithilfe der kombinierten Authentifizierung.  Diese Version enthält ein Office 365-Abonnement.
Mehrstufige Authentifizierung Azure-Administratoren | Dieselbe Teilmenge der kombinierten Authentifizierung Funktionen für Office 365 kostenlos für alle Azure-Administratoren zur Verfügung. Jedes Administratorkonto ein Azure-Abonnement kann zusätzlichen Schutz jetzt diese Kernfunktionen mehrstufige Authentifizierung aktivieren. Damit ein Administrator, der Azure-Portal erstellen Sie einen virtuellen Computer eine Website zugreifen will Speichermanagement, kann mobile Dienste oder andere Azure Service mehrstufige Authentifizierung seinem Administratorkonto hinzufügen.
Azure mehrstufige Authentifizierung | Azure Mehrfaktor-Authentifizierung bietet die umfassendsten Funktionen. <br><br>Bietet zusätzliche Konfigurationsoptionen der Azure-Verwaltungsportal, erweiterte reporting und Unterstützung für eine Vielzahl von lokalen und Applikationen. Azure mehrstufige Authentifizierung kann als eigenständige Lizenz erworben und Azure Active Directory Premium und Enterprise Mobility Suite gebündelt. <br><br>Auch kann auf Basis Verbrauch erworben werden, Azure mehrstufige Authentifizierungsanbieter in Azure-Abonnement erstellen.
##<a name="feature-comparison-of-versions"></a>Funktionsvergleich von Versionen
Die folgende Tabelle enthält eine Liste der Features, die in den verschiedenen Versionen von Azure mehrstufige Authentifizierung verfügbar sind.


Funktion  | Mehrstufige Authentifizierung für Office 365 (enthalten in Office 365 SKUs)|Mehrstufige Authentifizierung Azure-Administratoren (in Azure-Abonnement enthalten) | Azure mehrstufige Authentifizierung (in Azure AD Premium und Enterprise Mobility Suite enthalten)
------------- | :-------------: |:-------------: |:-------------: |
Administratoren können Konten mit MFA schützen.| * | * (Verfügbar nur für Azure Administratorkonten)|*
Mobile Anwendung als zweiter Faktor|* | * | *
Anruf als zweiter Faktor|* | * | *
SMS als zweiter Faktor|* | * | *
App-Kennwörter für Clients, die keine MFA unterstützen|* | * | *
Admin Kontrolle über Authentifizierungsmethoden| *| *| *
PIN-Modus| | | *
Betrug| | | *
MFA-Berichte| | | *
Einmalige umgehen| | | *
Benutzerdefinierte Ansage für Anrufe| | | *
Anpassung der Anrufer-ID für Anrufe| | | *
Ereignis-Bestätigung| | | *
Vertrauenswürdige IP-Adressen| | | *
Unterbrechen Sie MFA für gespeicherte Geräte (Public Preview)| | | *
MFA-SDK| | | *
MFA für lokale Applikationen mit MFA-server| | | *


##<a name="how-to-get-azure-multi-factor-authentication"></a>Wie man Azure mehrstufige Authentifizierung

Die vollständige Funktionalität von Azure mehrstufige Authentifizierung statt nur die Benutzer Office 365 und Azure Administratoren möchten, sind mehrere Optionen zu:

1.  Azure mehrstufige Authentifizierung Lizenzen und Benutzer zuweisen.
2.  Lizenzen Sie, die Azure mehrstufige Authentifizierung innerhalb Azure Active Directory Premium oder Enterprise Mobility Suite gebündelt und Benutzer zuweisen.
3.  Erstellen Sie mehrstufige Authentifizierungsanbieter Azure in Azure-Abonnement. Wenn Sie bereits ein Azure-Abonnement haben, können Sie ein Azure Testabonnement anmelden. Testabonnements müssen vor Ablauf der normalen Abonnements konvertiert werden.

Verwendung von Azure mehrstufige Authentifizierungsanbieter gibt es zwei Verwendung Modelle verfügbar, die von der Azure-Abonnement abgerechnet werden:


- **Pro Benutzer**. Im Allgemeinen soll für Unternehmen, die kombinierte Authentifizierung für eine feste Anzahl von Mitarbeitern, die regelmäßig Authentifizierung.
- **Pro Authentifizierung**. Im Allgemeinen soll für Unternehmen, die kombinierte Authentifizierung für eine große Gruppe von externen Benutzern, die selten Authentifizierung.

Preisinformationen finden Sie unter [Azure MFA Preisgestaltung.](https://azure.microsoft.com/pricing/details/multi-factor-authentication/)

Wählen Sie pro Arbeitsplatz oder Verbrauch-Modell, das am besten für Ihre Organisation.   Dann Schritte sehen [Einstieg](multi-factor-authentication-get-started.md)
