<properties
    pageTitle="Häufig gestellte Fragen zur Azure mehrstufige Authentifizierung"
    description="Enthält eine Liste der häufig gestellten Fragen und Antworten zu Azure mehrstufige Authentifizierung. Mehrstufige Authentifizierung ist eine Methode zum Überprüfen der Identität eines Benutzers, der über einen Benutzernamen und ein Kennwort erfordert. Es bietet zusätzlichen Benutzer anmelden und Transaktionen."
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
    ms.topic="article"
    ms.date="10/13/2016"
    ms.author="kgremban"/>

# <a name="azure-multi-factor-authentication-faq"></a>Häufig gestellte Fragen zur Azure mehrstufige Authentifizierung


Diese FAQ beantwortet Fragen zur Azure mehrstufige Authentifizierung und nutzen die mehrstufige Authentifizierung, einschließlich Fragen zur Abrechnung Modell und Verwendbarkeit.

## <a name="general"></a>Allgemein

**F: wie kann Azure mehrstufige Authentifizierungsserver Daten verarbeiten?**

Mehrstufige Authentifizierung Server werden Benutzerdaten auf dem lokalen Server gespeichert. Keine permanenten Benutzerdaten in der Cloud gespeichert. Wenn der Benutzer zwei Überprüfung ausführt, sendet mehrstufige Authentifizierungsserver Daten an Azure mehrstufige Authentifizierung Cloud-Dienst für die Authentifizierung. Kommunikation zwischen mehrstufige Authentifizierungsserver und mehrstufige Authentifizierung Cloud-Dienst verwendet Secure Sockets Layer (SSL) oder Transport Layer Security (TLS) über Port 443 abgehend.

Wenn Authentifizierung werden zum Cloud-Dienst Daten für Authentifizierung und Verwendung gibt. Datenfelder in zwei Überprüfung Protokolle enthalten sind:

- **Eindeutige ID** (entweder Benutzer Namen oder lokalen mehrstufige Authentifizierung Server-ID)
- **Vor-und Nachname** (optional)
- **E-Mail-Adresse** (optional)
- **Telefonnummer** (bei Verwendung eines Telefongesprächs oder SMS-Authentifizierung)
- **Geräte-Token** (bei Verwendung der mobilen Anwendung Authentifizierung)
- **Authentifizierungsmodus**
- **Ergebnis der Authentifizierung**
- **Mehrstufige Authentifizierung Servername**
- **Mehrstufige Authentifizierungsserver IP**
- **Client-IP-** (falls verfügbar)

Optionale Felder können mehrstufige Authentifizierungsserver konfiguriert werden.

Prüfungsergebnis (Erfolg oder DOS) Grund Wenn sie verweigert wurde, mit den Authentifizierungsdaten gespeichert und steht in Authentifizierung und Berichten.


## <a name="billing"></a>Abrechnung

Auf der [Seite mehrstufige Authentifizierung Preise](https://azure.microsoft.com/pricing/details/multi-factor-authentication/)können die meisten Fragen beantwortet werden.

**F: Dient Mein Unternehmen berechnet Telefonanrufe oder SMS zum Benutzer authentifizieren?**

Unternehmen Zahlen für einzelne Anrufe platziert oder Text Nachrichten an Benutzer über Azure mehrstufige Authentifizierung. -Besitzer können Anrufe oder SMS erhalten sie, die entsprechend ihren persönlichen Telefondienst berechnet.

**F: kann pro Benutzer zu-/ Rechnungsadresse Modell basierend auf der Anzahl der Benutzer, die mehrstufige Authentifizierung konfiguriert sind oder die Anzahl der Benutzer, die Durchführung?**

Abrechnung basiert auf der Anzahl der Benutzer mehrstufige Authentifizierung konfiguriert.

**F: wie funktioniert mehrstufige Authentifizierung Abrechnung?**

Bei Verwendung der "pro Benutzer" oder "Authentifizierung" Modell, Azure MFA ist ein Verbrauch basiert. Die Organisation Azure-Abonnement wie virtuelle Computer, Websites usw. werden Gebühren berechnet.

Bei Verwendung des Lizenzierungsmodells Azure mehrstufige Authentifizierung Lizenzen gekauft und dann Benutzern zugewiesen, wie für Office 365 und andere Abonnement-Produkte.

**F: Gibt es eine kostenlose Version von Azure mehrstufige Authentifizierung für Administratoren?**

In einigen Fällen Ja. Mehrstufige Authentifizierung für Administratoren Azure stellt eine Teilmenge der Azure MFA-Funktionen ohne Mehrkosten. Dieses Angebot gilt für Azure globale Administratorgruppe in Azure Active Directory-Instanzen, die mit einem Verbrauch Azure mehrstufige Authentifizierung Anbieter verknüpft sind. Mit einem mehrstufigen Authentifizierung Anbieter aktualisiert alle Administratoren und Benutzer im Verzeichnis, mehrstufige Authentifizierung mit der Vollversion von Azure mehrstufige Authentifizierung konfiguriert sind.

**F: Gibt es eine kostenlose Version von Azure mehrstufige Authentifizierung für Office 365-Benutzer?**

In einigen Fällen Ja. Mehrstufige Authentifizierung für Office 365 bietet eine Teilmenge der Azure MFA-Funktionen ohne Mehrkosten. Dieses Angebot gilt für Benutzer eine Office 365-Lizenz zugewiesen, wenn ein Verbrauch Azure mehrstufige Authentifizierung Anbieter nicht die entsprechende Instanz von Azure Active Directory verbunden ist. Mit der kombinierten Authentifizierung Anbieter aktualisiert alle Administratoren und Benutzer im Verzeichnis, mehrstufige Authentifizierung mit der Vollversion von Azure mehrstufige Authentifizierung konfiguriert sind.

**F: können Mein Unternehmen zwischen pro Benutzer und pro Authentifizierung Verbrauch Abrechnung jederzeit?**

Ihre Organisation Abrechnung Modell beim Erstellen einer Ressource. Nach Ressource bereitgestellt wird, können Sie eine Abrechnung Modell ändern. Sie können jedoch eine andere mehrstufige Authentifizierung Ressource um das Original zu ersetzen erstellen. Einstellungen und Konfigurationsoptionen können die neue Ressource übertragen werden.

**Kann meine Organisation verbrauchsabrechnung und Lizenzmodell jederzeit wechseln?**

Wenn Lizenzen zu einem Verzeichnis hinzugefügt werden, die bereits einen benutzerspezifischen Azure mehrstufige Authentifizierung Anbieter wird verbrauchsabhängigen durch die Anzahl der Lizenzen im Besitz verringert. Wenn mehrstufige Authentifizierung konfiguriert alle Lizenzen zugewiesen haben, kann der Administrator Azure mehrstufige Authentifizierung Anbieter löschen.

Pro Authentifizierung verbrauchsabrechnung mit einer Lizenz können nicht vermischt werden. Wenn ein pro-mehrstufige Authentifizierung Authentifizierungsanbieter ein Verzeichnis verknüpft ist, wird für alle kombinierten Authentifizierung Überprüfung Anfragen unabhängig von Lizenzen im Besitz die Organisation abgerechnet.

**F: verfügt Mein Unternehmen und Identitäten Azure mehrstufige Authentifizierung synchronisieren?**

Wenn eine Organisation eine Abrechnung Verbrauch basierendes Modell verwendet, ist Azure Active Directory nicht erforderlich. Mehrstufige Authentifizierungsanbieter ein Verzeichnis verknüpfen ist optional. Wenn Ihre Organisation nicht in einem Verzeichnis verknüpft ist, können sie Azure mehrstufige Authentifizierungsserver oder Azure mehrstufige Authentifizierung SDK lokalen bereitstellen.

Azure Active Directory ist erforderlich für das Lizenzmodell, da Lizenzen erwerben und Benutzern im Verzeichnis zuweisen zum Verzeichnis hinzugefügt werden.


## <a name="usability"></a>Handhabung

**Was tut ein Benutzer auf dem Telefon erhalten oder das Telefon nicht für den Benutzer verfügbar ist?**

Wenn der Benutzer eine Ersatzrufnummer konfiguriert, sie erneut und wählen Sie das Telefon Aufforderung auf der Anmeldeseite. Wenn der Benutzer eine andere Methode konfiguriert hat, kann die Organisations-Administrator die Nummer Telefon des Benutzers aktualisieren.


**Was tut Administrator, falls Benutzer über ein Konto Administrator Kontakt, die der Benutzer nicht zugreifen können?**

Der Administrator kann Benutzerkonto zurücksetzen, Fragen sie die Registrierung wieder. Weitere Informationen über das [Verwalten von Benutzer- und Einstellungen Azure mehrstufige Authentifizierung in der Cloud](multi-factor-authentication-manage-users-and-devices.md).

**Was tut Administrator bei Verlust oder Diebstahl Telefon des Benutzers, die app Kennwörter verwendet?**

Der Administrator kann alle Benutzer app Kennwörter vor nicht autorisierten Zugriffen löschen. Nachdem der Benutzer ein Ersatzgerät verfügt, kann der Benutzer Kennwörter wiederherstellen. Weitere Informationen über das [Verwalten von Benutzer- und Einstellungen Azure mehrstufige Authentifizierung in der Cloud](multi-factor-authentication-manage-users-and-devices.md).

**F: Was geschieht, wenn der Benutzer auf nicht-Browser-apps nicht anmelden?**

Ein Benutzer, mehrstufige Authentifizierung konfiguriert, benötigt ein Kennwort app einige apps-Browser anmelden. Anwender löschen (Delete)-Informationen und starten Sie die Anwendung über ihre Namen und app Benutzerkennwort anzumelden.

Erfahren Sie mehr zum Erstellen von app-Kennwörter und andere [app Kennwörter unterstützen](multi-factor-authentication-end-user-app-passwords.md).


>[AZURE.NOTE] Moderne Authentifizierung für Office 2013-clients
>
> Office 2013-Clients (einschließlich Outlook) unterstützen die neuen Authentifizierungsprotokolle. Sie können Office 2013 mehrstufige Authentifizierung unterstützen. Nach dem Konfigurieren von Office 2013 sind app-Kennwörter nicht für Office 2013-Clients erforderlich. Weitere Informationen finden Sie unter [Office 2013 moderne Authentifizierung public Preview-Ankündigung](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/).

**Was tut Benutzer, wenn der Benutzer eine Textnachricht nicht oder der Benutzer auf eine SMS-Nachricht antwortet jedoch die Überprüfung ein Timeout?**

Übermitteln von Nachrichten und Empfang der Antworten in bidirektionalen SMS ist nicht gewährleistet, da unkontrollierbare Faktoren, die die Zuverlässigkeit des Dienstes beeinträchtigen können. Dazu gehören das Zielland der Mobilfunkanbieter und Signalstärke.

Benutzer zuverlässig empfangen von Textnachrichten Problemen sollten die mobile app oder Telefonat stattdessen auswählen. Die app kann über Mobilfunk und WLAN Benachrichtigungen. Darüber hinaus erzeugen die app Bestätigungscodes, auch wenn das Gerät kein Signal hat. Microsoft Authenticator app ist für [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072)und [IOS](http://go.microsoft.com/fwlink/?Linkid=825073).

Wenn Sie SMS verwenden müssen, empfehlen wir unidirektionale SMS als bidirektionale SMS möglich. Unidirektionale SMS zuverlässiger und verhindert, dass Benutzer globale SMS Gebühren von Antworten auf eine Textnachricht aus einem anderen Land.


**F: verwenden kann ich Hardwaretoken Azure mehrstufige Authentifizierung Server?**

Bei Verwendung von Azure mehrstufige Authentifizierungsserver können Drittanbieter-offene Authentifizierung (EID) zeitbasierte, einmalige (TOTP) Kennworttokens importieren und dann zwei Überprüfung verwenden.

ActiveIdentity Token können die EID TOTP Token abgelegt geheimen Schlüssel in eine CSV-Datei und Azure mehrstufige Authentifizierung Server importieren. Sie können EID Token mit Active Directory Federation Services (ADFS), Remote Authentication Dial-in Benutzer Service (RADIUS) Wenn das Clientsystem Access-Challenge-Antworten verarbeiten kann und Internet Information Server (IIS) formularbasierte Authentifizierung verwenden.

Sie können Dritter EID TOTP Token in den folgenden Formaten:  
- Tragbare symmetrischen Schlüsselcontainer (PSKC)  
- CSV-Datei enthält eine Seriennummer, einen geheimen Schlüssel im Format 32 Base und ein Zeitintervall  

**F: verwenden kann ich Azure mehrstufige Authentifizierungsserver zum Sichern der Terminaldienste?**

Ja, aber wenn Sie Windows Server 2012 R2 oder höher von Remotedesktopgateway (RD Gateway).

Änderungen der Sicherheit in Windows Server 2012 R2 wurden verändert, die das Sicherheitspaket lokale Sicherheitsautorität (LSA) in Windows Server 2012 und früheren Versionen Azure mehrstufige Authentifizierungsserver Verbindung. Versionen der Terminaldienste in Windows Server 2012 oder früher können Sie [eine Anwendung mit Windows-Authentifizierung zu sichern](multi-factor-authentication-get-started-server-windows.md#to-secure-an-application-with-windows-authentication-use-the-following-procedure). Wenn Sie Windows Server 2012 R2 verwenden, benötigen Sie Remotedesktopgateway.

**Warum würde ein Benutzer eine mehrstufige Authentifizierung Anonymer Aufrufer Anruf nach dem Anrufer-ID einrichten?**

Mehrstufige Authentifizierung Anrufe über das öffentliche Telefonnetz platziert werden, werden auch sie durch einen Netzbetreiber geleitet, die Anrufer-ID nicht unterstützt Anrufer-ID ist daher nicht gewährleistet, obwohl mehrstufige Authentifizierung das System immer es sendet.


## <a name="errors"></a>Fehler

**Was sollten Benutzer tun, wenn sie "Authentifizierungsanfrage ist ein aktiviertes Konto" Fehlermeldung bei Verwendung der mobilen Anwendung Benachrichtigungen?**

Teilen sie diesem Verfahren entfernen Sie ihr Konto von der app anschließend wieder hinzufügen:

1. [Ihre Azure Portal](https://account.activedirectory.windowsazure.com/profile/) Profil und Konto Ihrer Organisation anmelden.
2. Wählen Sie **zusätzliche Überprüfung**.
4. Entfernen Sie das vorhandene Konto aus der app.
5. Klicken Sie auf **Konfigurieren**und gehen Sie die app konfigurieren.


**Was sollten Benutzer tun, wenn sie eine 0x800434D4L beim Anmelden einer Browseranwendung Fehlermeldung?**

Derzeit können Benutzer zusätzliche Überprüfung nur für Programme und Dienste, die der Benutzer über einen Browser zugreifen kann. Nicht-Browseranwendungen (auch bezeichnet als *rich Client-Anwendung*), die auf einem lokalen Computer wie Windows PowerShell installiert funktioniert nicht mit Konten, die zusätzliche Überprüfung erforderlich. In diesem Fall sieht der Benutzer die Anwendung ein Fehler 0x800434D4L.

Eine Abhilfe für diese separaten Benutzer Konten für Admin-bezogenen und ohne Administratorrechte. Später können Sie Postfächer zwischen Ihrem Administratorkonto und nicht-Administratorkonto verknüpfen, damit Sie Outlook mit Ihrem Administratorkonto anmelden können. Für weitere Details erfahren Sie, wie [einem Administrator die Möglichkeit zu öffnen und den Inhalt des Postfachs eines Benutzers](http://help.outlook.com/141/gg709759.aspx?sl=1).

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihre Frage hier nicht beantwortet wird, lassen Sie es in die Kommentare am Ende der Seite. Und hier sind einige weiteren Optionen zum Anzeigen der Hilfe:


**F: Wie erhalte ich Hilfe zu Azure mehrstufige Authentifizierung?**

- Durchsuchen Sie die [Microsoft Support Knowledge Base](https://www.microsoft.com/en-us/Search/result.aspx?form=mssupport&q=phonefactor&form=mssupport) Lösungen für technische Probleme.

- Suchen und Durchsuchen von technischen Fragen und Antworten aus der Community oder eigene Frage in den [Foren Azure Active Directory](https://social.msdn.microsoft.com/Forums/azure/newthread?category=windowsazureplatform&forum=WindowsAzureAD&prof=required).

- Wenn Sie einen älteren PhoneFactor Kunden und Fragen haben oder Hilfe beim Zurücksetzen eines Kennworts benötigen, verwenden Sie Link [Kennwort zurücksetzen](mailto:phonefactorsupport@microsoft.com) eine Support-Anfrage öffnen.

- Wenden Sie sich an einen Supportmitarbeiter [Azure mehrstufige Authentifizierungsserver (PhoneFactor)](https://support.microsoft.com/oas/default.aspx?prid=14947)unterstützt. Wenn Sie uns kontaktieren, ist es hilfreich, wenn Ihr Problem möglichst viele Informationen aufnehmen kann. Informationen, die Sie angeben können, enthält die Seite Sie gesehen, wo haben der Fehler, Fehlercode bestimmte Sitzung-ID und die ID des Benutzers, der den Fehler gesehen haben.
