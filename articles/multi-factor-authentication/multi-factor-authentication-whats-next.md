<properties
    pageTitle="Azure mehrstufige Authentifizierung - was"
    description="Dies ist der Azure mehrstufige Authentifizierungsseite, die was jetzt Mastertitel beschreibt.  Dazu gehören Berichte, Betrug, einmalige umgehen, benutzerdefinierte Sprachnachrichten vertrauenswürdige IP-Adressen und app Kennwörter zwischenspeichern."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/23/2016"
    ms.author="kgremban"/>

# <a name="configuring-azure-multi-factor-authentication"></a>Konfiguration von Azure mehrstufige Authentifizierung

Dieser Artikel hilft Ihnen Azure mehrstufige Authentifizierung verwalten, damit Sie ausgeführt werden.  Es umfasst eine Vielzahl von Themen, in denen Azure mehrstufige Authentifizierung optimal.  Nicht alle diese Funktionen sind in jeder Version von Azure mehrstufige Authentifizierung.

Die Konfiguration für einige der folgenden Features befindet sich im Verwaltungsportal von Azure Mehrfaktor-Authentifizierung. Es gibt zwei unterschiedliche Arten, auf die Sie MFA-Verwaltungsportal beide Azure Portal ausgeführt werden. Die erste ist eine mehrstufige Authentifizierung Anbieter verwalten, wenn MFA Verbrauch basierend. Das zweite ist über die MFA Einstellungen. Die zweite Option ist eine mehrstufige Authentifizierungsanbieter oder eine Azure MFA, Azure AD Premium oder Enterprise Mobility Suite-Lizenz erforderlich.

MFA-Verwaltungsportal über einen Azure mehrstufige Authentifizierung Anbieter Zugriff auf Azure-Portal als Administrator anmelden und die Option Active Directory. Klicken Sie auf der Registerkarte **Mehrstufige Authentifizierung Anbieter** wählen Sie das Verzeichnis, und klicken Sie im unteren Bereich **Verwalten** .

MFA-Verwaltungsportal über die Einstellungsseite MFA-Dienst Zugriff auf Azure-Portal als Administrator anmelden und die Option Active Directory. Klicken Sie auf das Verzeichnis, und klicken Sie dann auf die Registerkarte **Konfigurieren** . Wählen Sie im Abschnitt mehrstufige Authentifizierung **Verwalten Service**. Am unteren Seitenrand MFA-Einstellungen klicken Sie **im Portal** .


Funktion| Beschreibung| Inhalt
:------------- | :------------- | :------------- |
[Betrug](#fraud-alert)|Betrug kann konfiguriert und eingerichtet, sodass Benutzer betrügerische Zugriffsversuche auf Ressourcen melden können.|Zum Einrichten, konfigurieren und Betrug melden
[Einmalige umgehen](#one-time-bypass) |Eine einmalige umgehen kann Benutzer einmal "umgehen" Multifaktor-Authentifizierung authentifizieren.|Einrichten und Konfigurieren einer einmaligen umgehen
[Benutzerdefinierte Sprachnachrichten](#custom-voice-messages) |Benutzerdefinierte Sprachnachrichten können Sie eigene Aufnahmen oder Greetings Mehrfaktor-Authentifizierung verwenden. |Einrichten und Konfigurieren von benutzerdefinierten Grüße und Nachrichten
[Zwischenspeichern](#caching-in-azure-multi-factor-authentication)|Zwischenspeichern können Sie eine bestimmte Zeit, damit nachfolgende Authentifizierungsversuche automatisch erfolgreich festgelegt. |Zum Einrichten und Konfigurieren der Authentifizierung Zwischenspeichern.
[Vertrauenswürdige IP-Adressen](#trusted-ips)|Vertrauenswürdige IPs ist eine mehrstufige Authentifizierung, die Administratoren verwaltet oder verbundenen Mandanten können mehrstufige Authentifizierung für Benutzer umgehen, die von lokalen Firmenintranet anmelden.|Konfigurieren Sie und IP-Adressen für mehrstufige Authentifizierung ausgenommen
[App-Kennwörter](#app-passwords)|Ein app-Kennwort ermöglicht es einer Anwendung, die nicht MFA-aware mehrstufige Authentifizierung umgehen und weiterarbeiten.|Informationen zu app-Kennwörter.
[Mehrstufige Authentifizierung für gespeicherte Geräte und Browser speichern](#remember-multi-factor-authentication-for-devices-users-trust)|Geräte für eine festgelegte Anzahl von Tagen, nachdem ein Benutzer angemeldet hat mit MFA speichern können.|Informationen zum Aktivieren dieser Funktion und die Anzahl der Tage einrichten.
[Auswählbare Prüfmittel](#selectable-verification-methods)|Können Sie Authentifizierungsmethoden auswählen, die für Benutzer verfügbar sind.|Informationen zum Aktivieren oder deaktivieren bestimmte Authentifizierungsmethoden wie Call oder SMS.



## <a name="fraud-alert"></a>Betrug
Betrug kann konfiguriert und eingerichtet, sodass Benutzer betrügerische Zugriffsversuche auf Ressourcen melden können.  Benutzer können mit der app oder über ihr Telefon Betrug melden.

### <a name="to-set-up-and-configure-fraud-alert"></a>Einrichten und Konfigurieren von Betrug

1.  Melden Sie sich bei http://azure.microsoft.com
2.  Navigieren Sie zu der MFA-Verwaltungsportal Anweisungen am oberen Rand dieser Seite.
3.  Klicken Sie in Azure mehrstufige Authentifizierung-Verwaltungsportal im Abschnitt konfigurieren.
4.  Überprüfen Sie im Abschnitt Betrug Warnung Einstellungen Benutzer um Betrug Alerts das Kontrollkästchen senden.
5.  Wenn Benutzer berichtet Betrug blockiert werden sollen, aktivieren Sie Benutzer blockieren Wenn Betrug gemeldet wird.
6.  Geben Sie im Textfeld **Code zum Bericht Betrug während der anfänglichen Begrüßung** Code einen, der bei Aufruf Überprüfung verwendet werden kann. Falls dieser Code sowie # statt nur das #-Zeichen eingegeben, wird eine betrugswarnung gemeldet.
7.  Klicken Sie unten auf Speichern.

>[AZURE.NOTE]
>Microsoft Standard-Voicemail-Grußformeln weisen 0 # betrugswarnung senden drücken. Wenn Sie einen Code als 0 verwenden möchten, sollten Sie aufzeichnen und Hochladen eigener benutzerdefinierter Voicemail-Grußformeln mit entsprechenden.


![Cloud](./media/multi-factor-authentication-whats-next/fraud.png)

### <a name="to-report-fraud-alert"></a>Bericht Betrug Warnung
E-Mail kann auf zwei Arten gemeldet werden.  Oder über die app über das Telefon.  

### <a name="to-report-fraud-alert-with-the-mobile-app"></a>Bericht Betrug Warnung mit der app



1. Wenn eine Überprüfung an Ihr Telefon gesendet wird, wählen Sie Microsoft Authenticator app gestartet.
2. Betrug zu melden, klicken Sie auf Abbrechen und Betrug melden. Dadurch wird ein Feld Ihrer Organisation IT-Supportmitarbeiter benachrichtigt werden.
3. Klicken Sie auf Bericht Betrug.
4. Die Anwendung klicken Sie auf Schließen.

![Cloud](./media/multi-factor-authentication-whats-next/report1.png)


![Cloud](./media/multi-factor-authentication-whats-next/fraud2.png)

### <a name="to-report-fraud-alert-with-the-phone"></a>Bericht Betrug Warnung mit Telefon

1. Wenn eine Überprüfung auf Ihr Telefon Anruf, beantworten.  
2. Betrug melden, geben den Code Meldung über das Telefon und das #-Zeichen entsprechen konfiguriert wurde. Sie werden benachrichtigt, dass betrugswarnung gesendet wurde.
3. Beenden Sie den Anruf.

### <a name="to-view-the-fraud-report"></a>Betrugsbericht anzeigen

1. [Http://azure.microsoft.com](https://azure.microsoft.com/) melden
2. Wählen Sie auf der linken Seite Active Directory.
3. Wählen Sie oben mehrstufige Authentifizierung Anbieter. Dadurch wird eine Liste der die kombinierte Authentifizierung.
4. Haben Sie mehr als eine mehrstufige Authentifizierungsanbieter, wählen Sie die zu Betrug Warnung Bericht anzeigen und verwalten am unteren Rand der Seite auf. Haben Sie nur ein, klicken Sie auf verwalten. Verwaltungsportal von Azure mehrstufige Authentifizierung wird geöffnet.
5. Azure mehrstufige Authentifizierung Verwaltungsportal auf der linken Seite unter Ansicht ein Klicken Sie auf Warnung Betrug.
6. Geben Sie den Datumsbereich, den Sie im Bericht anzeigen möchten. Sie können auch bestimmten Benutzernamen, Telefonnummern und den Status des Benutzers angeben.
7. Klicken Sie auf ausführen. Dadurch wird ein Bericht ähnlich der unten. Möchten Sie den Bericht zu exportieren, können Sie auch Exportieren nach CSV klicken.

## <a name="one-time-bypass"></a>Einmalige umgehen

Eine einmalige umgehen kann Benutzer einmal "umgehen" Multifaktor-Authentifizierung authentifizieren. Das ist temporär und läuft nach der angegebenen Anzahl von Sekunden ab.  Damit in Situationen, in denen die mobile Anwendung oder das Telefon keine Benachrichtigung oder einen Anruf erhält, Sie eine einmalige umgehen können, damit der Benutzer die gewünschte Ressource zugreifen kann.

### <a name="to-create-a-one-time-bypass"></a>Erstellen einer einmaligen umgehen

1.  Melden Sie sich bei http://azure.microsoft.com
2.  Navigieren Sie zu der MFA-Verwaltungsportal Anweisungen am oberen Rand dieser Seite.
3.  In Azure mehrstufige Authentifizierung Verwaltungsportal siehst du den Namen des Mandanten oder Azure MFA-Anbieters mit einer +, klicken Sie auf das + finden Sie verschiedene MFA Replikation Servergruppen und Azure-Standardgruppe. Klicken Sie auf die entsprechende Gruppe.
4.  Klicken Sie unter Benutzer auf **Einmalige umgehen**.
![Cloud](./media/multi-factor-authentication-whats-next/create1.png)
5.  Klicken Sie auf der Seite Einmaliges umgehen auf **Neue einmalige umgehen**.
6.  Geben Sie den Benutzernamen des Benutzers, die Anzahl der Sekunden, die Papierzufuhr vorhanden, den Grund für das umgehen und auf **umgehen**.
![Cloud](./media/multi-factor-authentication-whats-next/create2.png)
7.  Zu diesem Zeitpunkt muss der Benutzer anmelden, vor Ablauf der einmaligen umgangen.



### <a name="to-view-the-one-time-bypass-report"></a>Einmalige Bypass-Bericht anzeigen

1. [Http://azure.microsoft.com](https://azure.microsoft.com/) melden
2. Wählen Sie auf der linken Seite Active Directory.
3. Wählen Sie oben mehrstufige Authentifizierung Anbieter. Dadurch wird eine Liste der die kombinierte Authentifizierung.
4. Haben Sie mehr als eine mehrstufige Authentifizierungsanbieter, wählen Sie die zu Betrug Warnung Bericht anzeigen und verwalten am unteren Rand der Seite auf. Haben Sie nur ein, klicken Sie auf verwalten. Verwaltungsportal von Azure mehrstufige Authentifizierung wird geöffnet.
5. Azure mehrstufige Authentifizierung Verwaltungsportal auf der linken Seite unter Ansicht ein Klicken Sie auf Einmalumgehung.
6. Geben Sie den Datumsbereich, den Sie im Bericht anzeigen möchten. Sie können auch bestimmten Benutzernamen, Telefonnummern und den Status des Benutzers angeben.
7. Klicken Sie auf ausführen. Dadurch wird ein Bericht ähnlich der unten. Möchten Sie den Bericht zu exportieren, können Sie auch Exportieren nach CSV klicken.

<center>![Cloud](./media/multi-factor-authentication-whats-next/report.png)</center>

## <a name="custom-voice-messages"></a>Benutzerdefinierte Sprachnachrichten

Benutzerdefinierte Sprachnachrichten können Sie eigene Aufnahmen oder Greetings Mehrfaktor-Authentifizierung verwenden.  Diese können dazu verwendet werden, oder Datensätze Microsoft ersetzen.

Bevor Sie beginnen werden Sie sollten Sie Folgendes beachten:

- Die aktuelle Dateiformate werden unterstützt, MP3 und WAV.
- Die maximale Dateigröße beträgt 5 MB.
- Es wird empfohlen, die Authentifizierung Nachrichten nicht länger als 20 Sekunden sein. Etwas größer kann die Überprüfung fehlschlagen, da der Benutzer nicht reagieren können, bevor die Nachricht abgeschlossen und Überprüfung Timeout.



### <a name="to-set-up-custom-voice-messages-in-azure-multi-factor-authentication"></a>Benutzerdefinierte Sprachnachrichten in Azure Mehrfaktor-Authentifizierung einrichten
1.  Erstellen Sie eine benutzerdefinierte Nachricht mit einem der unterstützten Dateiformate.
2.  Melden Sie sich bei http://azure.microsoft.com
3.  Navigieren Sie zu der MFA-Verwaltungsportal Anweisungen am oberen Rand dieser Seite.
4.  Klicken Sie in Azure mehrstufige Authentifizierung-Verwaltungsportal Sprachnachrichten Abschnitt konfigurieren.
5.  Klicken Sie im Abschnitt Sprachnachrichten auf **Neue Nachricht**.
![Cloud](./media/multi-factor-authentication-whats-next/custom1.png)
6.  Auf der konfigurieren: Neue Sprachnachrichten auf **Audiodateien verwalten**.
![Cloud](./media/multi-factor-authentication-whats-next/custom2.png)
7.  Auf der konfigurieren: Sound hinzufügen, klicken Sie auf **Sound-Datei hochladen**.
![Cloud](./media/multi-factor-authentication-whats-next/custom3.png)
8.  Auf der konfigurieren: Audiodatei hochladen, klicken Sie auf **Durchsuchen** und navigieren Sie zu der Nachricht, klicken Sie auf **Öffnen**.
![Cloud](./media/multi-factor-authentication-whats-next/custom4.png)
9.  Beschreibung, und klicken Sie auf hochladen.
10. Sobald dies abgeschlossen ist, durch eine Meldung bestätigt, dass die Datei erfolgreich hochgeladen haben.
11. Klicken Sie auf der linken Seite auf Sprachnachrichten.
12. Klicken Sie im Abschnitt Sprachnachrichten auf neue Nachricht.
13. Wählen Sie aus der Dropdownliste Sprache eine Sprache aus.
14. Ist diese Meldung für eine bestimmte Anwendung, geben sie im Feld.
15. Wählen Sie den Nachrichtentyp Nachrichtentyp mit unserer neuen Meldung überschrieben werden.
16. Wählen Sie aus der Dropdownliste Audiodatei Sounddatei.
17. Klicken Sie auf **Erstellen**. Eine Meldung bestätigt, dass eine Nachricht erfolgreich erstellt wurde.
![Cloud](./media/multi-factor-authentication-whats-next/custom5.png)</center>



## <a name="caching-in-azure-multi-factor-authentication"></a>Zwischenspeichern in Azure mehrstufige Authentifizierung

Zwischenspeichern können Sie eine bestimmte Zeit, damit nachfolgende Authentifizierungsversuche automatisch erfolgreich festgelegt. Dies wird hauptsächlich beim lokalen Systemen wie VPN mehrere Überprüfung Anfragen während die erste Anforderung noch nicht abgeschlossen ist. Dadurch wird die nachfolgenden Anfragen automatisch erfolgreich ausgeführt, nachdem der Benutzer die Überprüfung gerade erfolgreich. Beachten Sie, dass das Zwischenspeichern nicht soll anmelden Azure AD verwendet werden.


### <a name="to-set-up-caching-in-azure-multi-factor-authentication"></a>Zwischenspeichern in Azure Mehrfaktor-Authentifizierung einrichten

1.  Melden Sie sich bei http://azure.microsoft.com
2.  Navigieren Sie zu der MFA-Verwaltungsportal Anweisungen am oberen Rand dieser Seite.
3.  Klicken Sie in Azure mehrstufige Authentifizierung-Verwaltungsportal Zwischenspeichern unter Abschnitt konfigurieren.
4.  Klicken Sie auf der Seite zwischenspeichern Konfigurieren neuer Cache
5.  Wählen Sie den Cachetyp und Cache Sekunden. Klicken Sie auf erstellen.

<center>![Cloud](./media/multi-factor-authentication-whats-next/cache.png)</center>

## <a name="trusted-ips"></a>Vertrauenswürdige IP-Adressen

Vertrauenswürdige IPs ist eine mehrstufige Authentifizierung, die Administratoren verwaltet oder verbundenen Mandanten können mehrstufige Authentifizierung für Benutzer umgehen, die von lokalen Firmenintranet anmelden. Die Features sind für Azure AD-Mandanten, die Azure AD Premium, Enterprise Mobility Suite oder Azure mehrstufige Authentifizierung Lizenzen verfügbar.


Typ der Azure AD-Mandanten| Vertrauenswürdige IP-Optionen
:------------- | :------------- |
Verwaltet|Bestimmte IP-Adressbereiche – Administratoren können einen Bereich von IP-Adressen angeben, die kombinierte Authentifizierung für Benutzer umgehen können, die vom Intranet der Firma anmelden.
Verbund|<li>Alle verbundene Benutzer: alle verbundenen Benutzer anmelden von innerhalb der Organisation umgehen mehrstufige Authentifizierung mithilfe einer Forderung von AD FS ausgestellt.</li><li>Bestimmte IP-Adressbereiche – Administratoren können einen Bereich von IP-Adressen angeben, die kombinierte Authentifizierung für Benutzer umgehen können, die vom Intranet der Firma anmelden.

Dies funktioniert nur von innerhalb des Intranets eines Unternehmens zu umgehen. So nur alle verbundenen Benutzer ausgewählt und ein Benutzer anmeldet muss von außerhalb des Unternehmens-Intranet Benutzer authentifizieren, selbst wenn der Benutzer einen AD FS-Anspruch bietet mehrstufige Authentifizierung. Die folgende Tabelle beschreibt, wenn mehrstufige Authentifizierung und app Kennwörter müssen innerhalb Corpnet und außerhalb Corpnet vertrauenswürdige IP-Adressen aktiviert ist.


|Vertrauenswürdige IP-Adressen aktiviert| Vertrauenswürdige IP-Adressen deaktiviert
:------------- | :------------- | :------------- |
Innen corpnet|Browser Datenflüsse nicht mehrstufige Authentifizierung erforderlich.|Browser Datenflüsse mehrstufige Authentifizierung erforderlich
|Für umfangreiche Clientanwendungen arbeiten reguläre Kennwörtern, wenn app Kennwörter der Benutzer nicht erstellt wurde. Erstellte app-Passwort sind app-Kennwörter erforderlich.|Für umfangreiche Clientanwendungen app Kennwörter
Externe corpnet|Browser-Flows mehrstufige Authentifizierung erforderlich.|Browser-Flows mehrstufige Authentifizierung erforderlich.
|Für umfangreiche Clientanwendungen app Kennwörter erforderlich.|Für umfangreiche Clientanwendungen app Kennwörter erforderlich.

### <a name="to-enable-trusted-ips"></a>Vertrauenswürdige IP-Adressen aktivieren

1. Anmelden bei klassischen Azure-Portal.
2. Klicken Sie auf der linken Seite auf Active Directory.
3. Klicken Sie unter auf Verzeichnis auf das Verzeichnis auf vertrauenswürdige IPsing einrichten möchten.
4. Für das Verzeichnis ausgewählt haben, klicken Sie auf konfigurieren.
5. Klicken Sie im Abschnitt mehrstufige Authentifizierung auf Einstellungen verwalten.
6. Wählen Sie auf der Seite Einstellungen unter Vertrauenswürdige IP-Adressen entweder:

    - Anfragen von verbundene Benutzer aus meiner Intranet – alle federated werden Benutzern im Unternehmensnetzwerk anmelden mehrstufige Authentifizierung mit AD FS ausgestellten Anspruch umgehen.
    - Anfragen von einem bestimmten Bereich von öffentlichen IPs – Geben Sie die IP-Adressen in die Textfelder CIDR-Notation. Beispiel: xxx.xxx.xxx.0/24 für IP-Adressen im Bereich xxx.xxx.xxx.1-xxx.xxx.xxx.254 oder xxx.xxx.xxx.xxx/32 für eine einzelne IP-Adresse. Sie können bis zu 50 IP-Adressbereiche eingeben.

7. Klicken Sie auf Speichern.
8. Nachdem die Updates angewendet wurden, klicken Sie auf Schließen.



![Vertrauenswürdige IP-Adressen](./media/multi-factor-authentication-whats-next/trustedips3.png)




## <a name="app-passwords"></a>App-Kennwörter

Apps, wie Office 2010 oder älter und Apple Mail kann nicht mehrstufige Authentifizierung verwenden.  Um diese Anwendung verwenden zu können, müssen Sie "app Kennwörter" anstelle der herkömmlichen Kennworts.  App-Kennwort erlaubt es die mehrstufige Authentifizierung umgehen und weiterarbeiten.

>[AZURE.NOTE] Moderne Authentifizierung für Office 2013-Clients
>
> Office 2013-Clients (einschließlich Outlook) jetzt neue Authentifizierungsprotokolle unterstützen und ermöglicht mehrstufige Authentifizierung unterstützen.  Dies bedeutet, dass nach der Aktivierung app-Kennwörter nicht für Office 2013-Clients erforderlich sind.  Weitere Informationen finden Sie unter [Office 2013 moderne Authentifizierung public Preview angekündigt](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/).



### <a name="important-things-to-know-about-app-passwords"></a>Wichtige Informationen zu app-Kennwörter

Folgendes ist eine wichtige Liste der Dinge, die app Kennwörter kennen sollten.

- Benutzer können mehrere app-Kennwörter, die Fläche für Diebstahl. App-Kennwörter schwer zu merken sind, können sie diese aufschreiben ermutigen. Dies wird nicht empfohlen und sollte abgeraten, da nur einen Faktor sich mit app-Kennwort erforderlich ist.
- Apps, die Kennwörter zwischenspeichern und in lokalen Szenarien beginnen fehl, da das Kennwort app außerhalb der Organisations-Id bekannt ist. Ein Beispiel ist Exchange e-Mails, die lokal archivierte e-Mail in der Cloud ist. Dasselbe Kennwort funktioniert nicht.
- Das Kennwort wird automatisch generiert und nicht vom Benutzer angegeben. Dies ist da automatisch generierte Kennwort ist für Angreifer zu erraten und sicherer.
- Derzeit sind maximal 40 Kennwörter pro Benutzer. Sie werden aufgefordert, eine vorhandene Anwendung Kennwörter löschen, um eine neue zu erstellen.
- Wenn mehrstufige Authentifizierung für ein Benutzerkonto aktiviert ist, app-Kennwörter können mit den meisten nicht-Browser-Clients wie Outlook und Lync verwendet aber Verwaltungsvorgänge können nicht app Kennwörter über nicht-Browseranwendungen wie Windows PowerShell verwenden, auch wenn der Benutzer ein administratives Konto durchgeführt werden.  Stellen Sie sicher, ein Dienstkonto mit einem sicheren Kennwort PowerShell-Skripts erstellen und dieses Konto für mehrstufige Authentifizierung nicht aktiviert.

>[AZURE.WARNING]  App-Kennwörter arbeiten nicht in einer hybriden Umgebung, wo Clients kommunizieren sowohl lokal und cloud AutoErmittlung Endpunkte. Ist Domänenkennwörter müssen lokale Authentifizierung und app-Kennwörter müssen mit der Cloud zu authentifizieren.


### <a name="naming-guidance-for-app-passwords"></a>Anleitung für App-Kennwörter benennen
Es wird empfohlen, dass app Kennwort Namen des Geräts widerspiegelt, auf dem sie verwendet werden. Beispielsweise haben Sie einen Laptop, der nicht-Browser-Programme wie Outlook, Word und Excel hat, müssen Sie nur ein app-Kennwort mit dem Namen Laptop erstellen und dieses Kennwort verwenden, app in allen diesen. Obwohl separate Kennwörter für alle diese Programme erstellen wird nicht empfohlen. Empfehlen ist wie ein app-Kennwort pro Gerät.


<center>![Cloud](./media/multi-factor-authentication-whats-next/naming.png)</center>


### <a name="federated-sso-app-passwords"></a>App-Kennwörter verbundenen (SSO)
Azure AD unterstützt mit lokalen Windows Server Active Directory-Domänendienste (AD DS). Wenn Ihre Organisation federated(SSO) mit Azure und werden zu Azure mehrstufige Authentifizierung dann folgenden wichtig ist, dass Sie beim app Kennwörter verwendet werden. Dies gilt nur für federated(SSO) Kunden.

- App-Kennwort von Azure AD überprüft und damit umgeht Föderation. Föderation wird nur verwendet, wenn App-Kennwort einrichten.
- Federated(SSO) Benutzer gehen wir nie Identitätsprovider (IdP) im Gegensatz zu passiver Fluss. Die Kennwörter werden in der Organisations-Id gespeichert. Wenn der Benutzer das Unternehmen verlässt, besitzt dieser Organisations-Id mit DirSync in Echtzeit übertragen. Konto deaktivieren und Löschen von dauert bis zu drei Stunden zu synchronisieren, deaktivieren/Löschen von App-Kennwort in Azure AD verzögern.
- Lokalen Client Access Control Settings werden vom App-Kennwort nicht berücksichtigt.
- Keine lokale Authentifizierung Protokollierung / Überwachung Funktion steht für App-Kennwort
- Mehr Endbenutzer Bildung ist für Microsoft Lync 2013-Client erforderlich. Die erforderlichen Schritte finden Sie unter app-Kennwort das Kennwort per e-Mail ändern.
- Bestimmte erweiterte Architektur erfordern mit einer Kombination aus organisatorischen Benutzernamen und Kennwörter und app-Kennwörter beim Kunden, wo sie authentifizieren mit mehrstufigen Authentifizierung. Für Clients, die eine lokale Infrastruktur authentifizieren, verwenden Sie eine Organisationseinheit Benutzername und Kennwort. Für Clients, die Azure AD authentifizieren, verwenden Sie das app-Kennwort.

Beispielsweise haben Sie eine Architektur, die Folgendes aus:

- Sie werden die lokale Instanz von Active Directory mit Azure Föderation.
- Sie verwenden Exchange online
- Sie verwenden Lync, die speziell auf lokale
- Azure mehrstufige Authentifizierung verwenden


![Proofup](./media/multi-factor-authentication-whats-next/federated.png)

 In diesen Fällen müssen Sie Folgendes ein:

- Beim Signieren-Lync in verwenden Sie Ihre Organisationen Benutzernamen und Kennwort.
- Beim Zugriff auf das Adressbuch über Outlook-Clients, die mit Exchange online verbindet verwenden Sie ein app-Kennwort.

### <a name="allowing-app-password-creation"></a>App Erstellung zulassen
Standardmäßig können keine Benutzer app Kennwörter erstellen.  Dieses Feature muss aktiviert sein.  Gehen Sie folgendermaßen vor, um Benutzern app Kennwörter erstellen.

#### <a name="to-enable-users-to-create-app-passwords"></a>Damit Benutzer app Kennwörter erstellen



1. Mit klassischen Azure-Portal anmelden.
2. Klicken Sie auf der linken Seite auf Active Directory.
3. Klicken Sie unter auf Verzeichnis auf das Verzeichnis für den Benutzer aktivieren möchten.
4. Klicken Sie oben auf Benutzer.
5. Klicken Sie am unteren Rand der Seite auf mehrstufige Authentifizierung verwalten  
6. Klicken Sie am oberen Seitenrand mehrstufige Authentifizierung auf Service.
7. Sicherstellen Sie, dass das Optionsfeld neben Benutzern app Kennwörter nicht Browseranwendungen Anmelden aktiviert ist.


![App-Kennwörter erstellen](./media/multi-factor-authentication-whats-next/trustedips3.png)

### <a name="creating-app-passwords"></a>App-Kennwörter erstellen
Benutzer können während ihrer ersten Registrierung app Kennwörter erstellen.  Sie erhalten eine Option am Ende der Registrierung, die sie erstellen können.

Darüber hinaus können Benutzer auch app Kennwörter später ihre Einstellungen in Azure-Portal Office 365-Portal erstellen

### <a name="to-create-app-passwords-in-the-office-365-portal"></a>App-Kennwörter in Office 365-Portal erstellen
--------------------------------------------------------------------------------


1. Office 365-Portal anmelden
2. Wählen Sie in der oberen rechten Ecke des Einstellungen Widgets
3. Wählen Sie auf der linken Seite zusätzliche Prüfung
4. Wählen Sie auf der rechten Seite **Meine Rufnummern Konto Sicherheit aktualisieren**
5. Wählen Sie auf der Seite Proofup oben app-Kennwörter
6. Klicken Sie auf **Erstellen**
7. Geben Sie einen Namen für das app-Kennwort, und klicken Sie auf **Weiter**
8. App-Kennwort in die Zwischenablage kopieren und in Ihre Anwendung einfügen.

<center>![Cloud](./media/multi-factor-authentication-whats-next/security.png)</center>


### <a name="to-create-app-passwords-in-the-azure-portal"></a>App-Kennwörter im Azure-Portal erstellen
--------------------------------------------------------------------------------
1. Mit klassischen Azure-Portal anmelden.
3. Oben mit der rechten Maustaste auf Ihren Benutzernamen, und wählen Sie zusätzliche Prüfung.
5. Wählen Sie auf der Seite Proofup oben app-Kennwörter
6. Klicken Sie auf **Erstellen**
7. Geben Sie einen Namen für das app-Kennwort, und klicken Sie auf **Weiter**
8. App-Kennwort in die Zwischenablage kopieren und in Ihre Anwendung einfügen.


![App-Kennwörter](./media/multi-factor-authentication-whats-next/app2.png)

### <a name="to-create-app-passwords-if-you-do-not-have-an-office-365-or-azure-subscription"></a>App-Kennwörter erstellen, wenn Sie nicht über ein Office 365 oder Azure-Abonnement verfügen
--------------------------------------------------------------------------------
1. Melden Sie sich bei [https://myapps.microsoft.com](https://myapps.microsoft.com)
2. Wählen Sie oben Profil.
3. Klicken Sie auf Ihren Benutzernamen, und wählen Sie zusätzliche Prüfung.
5. Wählen Sie auf der Seite Proofup oben app-Kennwörter
6. Klicken Sie auf **Erstellen**
7. Geben Sie einen Namen für das app-Kennwort, und klicken Sie auf **Weiter**
8. App-Kennwort in die Zwischenablage kopieren und in Ihre Anwendung einfügen.

![App-Kennwörter](./media/multi-factor-authentication-whats-next/myapp.png)

## <a name="remember-multi-factor-authentication-for-devices-users-trust"></a>Mehrstufige Authentifizierung für Benutzer vertrauen Geräte speichern

Mehrstufige Authentifizierung erinnern für Geräte und Browser, dass Benutzer vertrauen kostenloses Feature für alle MFA Benutzer.  Ermöglicht Benutzern die Möglichkeit, Bypass MFA für eine festgelegte Anzahl von Tagen nach Durchführung einer erfolgreichen mit MFA. Dadurch kann die Verwendbarkeit für Benutzer verbessern.

Da Benutzer vertrauenswürdige Geräte MFA erinnern, kann dieses Feature Kontensicherheit reduzieren. Konto aus Sicherheitsgründen sollten Sie mehrstufige Authentifizierung für Geräte für die folgenden Szenarien wiederherstellen:

- Wenn ihre corporate-Konto beschädigt werden
- Wenn gespeicherte Gerät verloren geht oder gestohlen wird

> [AZURE.NOTE] Dieses Feature wird als Browser Cookiecache implementiert. Es funktioniert nicht, wenn der Browsercookies nicht aktiviert sind.

### <a name="how-to-enabledisable-remember-multi-factor-authentication"></a>Wie speichern mehrstufige Authentifizierung aktivieren

1. Mit klassischen Azure-Portal anmelden.
2. Klicken Sie auf der linken Seite auf Active Directory.
3. Klicken Sie unter Active Directory Directory merken mehrstufige Authentifizierung für Geräte einrichten möchten.
4. Für das Verzeichnis ausgewählt haben, klicken Sie auf konfigurieren.
5. Klicken Sie im Abschnitt mehrstufige Authentifizierung auf Einstellungen verwalten.
6. Auf der Seite Einstellungen unter benutzereinstellungen verwalten, die **kombinierte Authentifizierung auf Geräten an vertrauenswürdige Benutzer**aktivieren/deaktivieren.
![Beachten Sie Geräte](./media/multi-factor-authentication-whats-next/remember.png)
8. Legen Sie die Anzahl von Tagen die Aussetzung soll. Der Standardwert ist 14 Tage.
9. Klicken Sie auf Speichern.
10. Klicken Sie auf Schließen.


## <a name="selectable-verification-methods"></a>Auswählbare Prüfmittel
Die Cloud und die lokalen Versionen können Sie die Verfahren für die Benutzer verfügbar sind. Nachfolgend Übersicht eine kurze der einzelnen Methoden.

Wenn die Benutzer ihre Konten MFA registrieren, wählen sie ihre bevorzugte Verifizierungsmethode aus Optionen aktiviert. Die Anleitung für ihre Registrierung fällt in [meinem Konto zweistufigen Authentifizierung einrichten](multi-factor-authentication-end-user-first-time.md)

-Methode|Beschreibung
:------------- | :------------- |
Call-to-phone |  Fügt einen automatisierten Anruf Telefon Authentifizierung. Der Benutzer beantwortet den Anruf und # in der Telefontastatur authentifizieren klickt. Diese Telefonnummer ist nicht für lokale Active Directory synchronisiert.
An Telefon | Sendet eine Textnachricht enthält einen Bestätigungscode an die Benutzer. Antworten SMS mit dem Bestätigungscode oder den Verifizierungscode der Schnittstelle wird der Benutzer aufgefordert.
Benachrichtigung über mobile-app | In diesem Microsoft Authenticator app verhindert den unberechtigten Zugriff auf Konten und betrügerische Transaktionen beendet. Dies erfolgt über eine Push-Benachrichtigung auf Ihrem Mobiltelefon oder registrierte Gerät. Benachrichtigung anzeigen, und wenn es legitim auf überprüfen. Andernfalls wählen Sie verweigern oder verweigern und melden die betrügerische Benachrichtigung. Informationen zum Anzeigen von betrügerischer Benachrichtigungen finden Sie unter verweigern und Bericht Betrug Feature für die kombinierte Authentifizierung verwenden.</br></br>Microsoft Authenticator app ist für [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072)und [IOS](http://go.microsoft.com/fwlink/?Linkid=825073).|
Verifizierungscode von mobile-app | In diesem Modus kann Microsoft Authenticator app als Softwaretoken generiert einen Verifizierungscode EID verwendet werden. Dieser Bestätigungscode kann dann zusammen mit dem Benutzernamen und Kennwort auf die zweite Form der Authentifizierung eingegeben werden.</li><br><p> Microsoft Authenticator app ist für [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072)und [IOS](http://go.microsoft.com/fwlink/?Linkid=825073).

### <a name="how-to-enabledisable-authentication-methods"></a>Wie Authentifizierungsmethoden aktivieren

1. Mit klassischen Azure-Portal anmelden.
2. Klicken Sie auf der linken Seite auf Active Directory.
3. Klicken Sie unter Active Directory Directory zu aktivieren oder Deaktivieren von Authentifizierungsmethoden.
4. Für das Verzeichnis ausgewählt haben, klicken Sie auf konfigurieren.
5. Klicken Sie im Abschnitt mehrstufige Authentifizierung auf Einstellungen verwalten.
6. Deaktivieren Sie auf der Seite Einstellungen unter Überprüfungsoptionen aktivieren/Optionen verwenden möchten.</br></br>
![Überprüfungsoptionen](./media/multi-factor-authentication-whats-next/authmethods.png)
9. Klicken Sie auf Speichern.
10. Klicken Sie auf Schließen.
