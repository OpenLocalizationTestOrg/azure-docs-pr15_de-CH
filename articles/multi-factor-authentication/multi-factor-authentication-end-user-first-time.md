<properties
    pageTitle="Richten Sie zwei Überprüfung für meine Arbeit oder Schule"
    description="Wenn Ihr Unternehmen Azure mehrstufige Authentifizierung konfiguriert, werden Sie aufgefordert sich zwei Überprüfung. Informationen Sie zum Einrichten von. "
    services="multi-factor-authentication"
    keywords="Verwendung von Azure Directory, active Directory in der Cloud, active Directory-Lernprogramm"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="pblachar"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="kgremban"/>

# <a name="set-up-my-account-for-two-step-verification"></a>Eigenes Konto zur Überprüfung in zwei Schritten

Zweistufige Überprüfung ist eine zusätzliche Sicherheit, mit der Schutz Ihres Kontos durch andere eindringen zu erschweren. Wenn Sie diesen Artikel lesen, haben Sie sich wahrscheinlich eine e-Mail über Ihren Arbeits- oder Schulcomputer Admin über mehrstufige Authentifizierung. Oder vielleicht Sie Anmelden aufgefordert, zusätzliche Überprüfung eingerichtet haben. Ist das der Fall, **Sie sich anmelden können, bis Sie die automatische Registrierung abgeschlossen haben**.

Dieser Artikel hilft Ihnen, Ihre **Arbeit oder Schule Konto**. Wenn Sie zwei Überprüfung für Ihre eigenen persönlichen Microsoft-Konto aktivieren möchten, finden Sie unter [Überprüfung in zwei Schritten](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification).

## <a name="determine-how-you-will-use-multi-factor-authentication"></a>Bestimmt, wie mehrstufige Authentifizierung verwendet

Zweistufige Verifizierung funktioniert nach Aufforderung zwei Teile der Identifikation bei der Anmeldung. Zuerst bitten wir den Benutzernamen und das Kennwort wie gewohnt. Dann bestätigen wir Kontakt eine Telefon, die wir wissen, und Sie gehört der Versuch legitim.  

Versuchen Sie zunächst mit der Einrichtung Ihres Kontos anmelden, wie gewohnt. Wenn Ihr Administrator Ihr Konto zweistufigen Authentifizierung konfiguriert, werden Sie aufgefordert, die automatische Registrierung zu beginnen. Dabei zunächst auf **jetzt einrichten.**

![Setup](./media/multi-factor-authentication-end-user-first-time/first.png)

Die erste Frage im Registrierungsprozess ist wie wir Sie kontaktieren möchten. Betrachten Sie die Optionen in der Tabelle und verwenden Sie die Links die Einrichtungsschritte für jede Methode zu.

| Kontaktmethode | Beschreibung |
| --- | --- |
[Mobile-app](#use-a-mobile-app-as-the-contact-method) | - **Benachrichtigung zur Bestätigung.** Diese Option legt eine Benachrichtigung auf Ihrem Smartphone oder Tablet Authenticator App. Benachrichtigung anzeigen und wenn es legitim ist, wählen Sie **Authentifizierung** in der Anwendung. Ihre Arbeit oder Schule erfordern eine PIN eingeben, bevor Sie authentifizieren.<br>- **Verwenden Sie Verifizierungscode.** In diesem Modus erzeugt Authenticator app Verifizierungscode alle 30 Sekunden aktualisiert. Geben Sie die aktuelle Überprüfung in der Schnittstelle.<br>Microsoft Authenticator app ist für [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072)und [IOS](http://go.microsoft.com/fwlink/?Linkid=825073). |
[Mobiltelefon oder text](#use-your-mobile-phone-as-the-contact-method) | - **Anruf** wird einen automatisierten Anruf die Telefonnummer, die Sie bereitstellen. Anruf, und drücken Sie # in der Telefontastatur zu authentifizieren.<br>- **Textnachricht** endet eine SMS mit einem Bestätigungscode. Beantworten Sie nach der Aufforderung im Text Text oder geben Sie den Bestätigungscode in der Schnittstelle angegeben. |  
[Office-Anruf](#use-your-office-phone-as-the-contact-method) | Fügt einen automatisierten Anruf die Telefonnummer, die Sie bereitstellen. Anruf und # in der Telefontastatur zu authentifizieren. |

## <a name="use-a-mobile-app-as-the-contact-method"></a>Verwenden Sie eine app als die Kontaktmethode

Mit dieser Methode erfordert die Installation einer Anwendung Authentifikator auf dem Telefon oder Tablet. Die Schritte in diesem Artikel basieren auf Microsoft Authenticator app für [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072)und [IOS](http://go.microsoft.com/fwlink/?Linkid=825073)verfügbar ist.

1. **Mobile-app** aus der Dropdown-Liste auswählen
2. Wählen Sie **Benachrichtigungen zur Überprüfung** oder **Verifizierungscode verwenden**, dann **eingerichtet**.

    ![Zusätzliche Überprüfung Bildschirm](./media/multi-factor-authentication-end-user-first-time-mobile-app/mobileapp.png)

3. Auf dem Telefon oder Tablet app öffnen, und wählen Sie **+** ein Konto hinzuzufügen. (Wählen Sie die drei Punkte dann **Konto hinzufügen**auf Android-Geräte.)
4. Geben Sie Arbeits- oder Schulcomputer Konto hinzufügen möchten. QR Code-Scanner auf Ihrem Telefon wird geöffnet. Wenn Ihre Kamera nicht funktioniert, können Sie auswählen, Ihre Firmendaten manuell eingeben. Weitere Informationen finden Sie unter [Konto manuell hinzufügen](#add-an-account-manually).  
5. Gescannte QR Code, der mit dem Bildschirm für die Konfiguration der app.  Wählen Sie QR Code-Bildschirm zu **geschehen** .  

    ![QR Code-Bildschirm](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan2.png)

6. Abschluss der Aktivierung über das Telefon, **Kontakt**wählen.  Dadurch sendet eine Benachrichtigung oder einen Bestätigungscode an Ihr Mobiltelefon. Wählen Sie **Überprüfen**.  
7. Wenn Ihr Unternehmen eine PIN zur Genehmigung anmelden Überprüfung erfordert, geben sie.

    ![Feld für die Eingabe einer PIN](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan3.png)

8. Wählen Sie nach Abschluss der PIN-Eingabe **Schließen**. An diesem Punkt sollte Ihre Überprüfung erfolgreich.
9. Wir empfehlen, Ihre Mobiltelefonnummer eingeben, falls Sie der mobilen Anwendung zugreifen. Geben Sie Ihr Land aus der Dropdown-Liste, und geben Sie Ihre Mobiltelefonnummer in das Feld neben den Namen des Landes. Wählen Sie **Weiter**.
10. Zu diesem Zeitpunkt werden Sie aufgefordert, app-Kennwörter nicht Browser Apps wie Outlook 2010 oder älter oder systemeigene e-Mail-app auf Apple-Geräten eingerichtet. Deshalb, weil einige apps zweistufigen Authentifizierung unterstützen. Wenn Sie diese Anwendung nicht verwenden, klicken Sie auf **Fertig** , und überspringen Sie die restlichen Schritte.
11. Bei Verwendung dieser apps kopieren app Kennwort bereitgestellt und Ihre Anwendung anstelle der regulären Kennwort einfügen. Das gleiche app-Kennwort können mehrere Apps. Weitere Informationen [app Kennwörter Hilfe].
12. Klicken Sie auf **Fertig**.


### <a name="add-an-account-manually"></a>Konto manuell hinzufügen
Möchten Sie die app manuell ein Konto hinzufügen, statt mit dem QR-Reader folgendermaßen Sie vor.

1. Wählen Sie **Konto manuell eingeben** .  
2. Geben Sie den Code und die URL, die auf derselben Seite bereitgestellt werden, die den Barcode zeigt. Diese Informationen weiter in den Feldern **Code** und **URL** der app.

    ![Setup](./media/multi-factor-authentication-end-user-first-time-mobile-app/barcode2.png)

3. Wenn die Aktivierung abgeschlossen ist, wählen Sie **Kontakt**. Dadurch sendet eine Benachrichtigung oder einen Bestätigungscode an Ihr Mobiltelefon. Wählen Sie **Überprüfen**.

## <a name="use-your-mobile-phone-as-the-contact-method"></a>Verwenden Sie Ihr Mobiltelefon als die Kontaktmethode

1. Wählen Sie aus der Dropdown-Liste **Authentifizierung Telefon** .  

    ![Setup](./media/multi-factor-authentication-end-user-first-time-mobile-phone/phone.png)  

2. Wählen Sie Ihr Land aus der Dropdown-Liste, und geben Sie Ihre Mobiltelefonnummer.
3. Wählen Sie die gewünschte Mobiltelefon - Text oder Aufruf verwenden.
4. Wählen Sie **Kontakt** Ihre Telefonnummer verifizieren. Abhängig vom ausgewählten Modus werden Sie SMS oder rufen Sie. Folgen Sie der Anleitung auf dem Bildschirm, und wählen **Sie prüfen**.
5. Zu diesem Zeitpunkt werden Sie aufgefordert, app-Kennwörter nicht Browser Apps wie Outlook 2010 oder älter oder systemeigene e-Mail-app auf Apple-Geräten eingerichtet. Deshalb, weil einige apps zweistufigen Authentifizierung unterstützen. Wenn Sie diese Anwendung nicht verwenden, klicken Sie auf **Fertig** , und überspringen Sie die restlichen Schritte.
6. Bei Verwendung dieser apps kopieren app Kennwort bereitgestellt und Ihre Anwendung anstelle der regulären Kennwort einfügen. Das gleiche app-Kennwort können mehrere Apps. Weitere Informationen [app Kennwörter Hilfe].
7. Klicken Sie auf **Fertig**.

## <a name="use-your-office-phone-as-the-contact-method"></a>Verwenden Sie Ihr Telefon als die Kontaktmethode

1. **Telefon** aus der Dropdown-Liste auswählen  

    ![Setup](./media/multi-factor-authentication-end-user-first-time-office-phone/office.png)  

2. Rufnummer ist Ihre Unternehmenskontaktinformationen automatisch eingetragen. Wenn die Anzahl falsch oder nicht vorhanden ist, muss der Administrator ändern.
4. Wählen Sie **Kontakt** Ihre Telefonnummer verifizieren und wir rufen Sie an. Folgen Sie der Anleitung auf dem Bildschirm, und wählen **Sie prüfen**.
5. Zu diesem Zeitpunkt werden Sie aufgefordert, app-Kennwörter nicht Browser Apps wie Outlook 2010 oder älter oder systemeigene e-Mail-app auf Apple-Geräten eingerichtet. Deshalb, weil einige apps zweistufigen Authentifizierung unterstützen. Wenn Sie diese Anwendung nicht verwenden, klicken Sie auf **Fertig** , und überspringen Sie die restlichen Schritte.
6. Bei Verwendung dieser apps kopieren app Kennwort bereitgestellt und Ihre Anwendung anstelle der regulären Kennwort einfügen. Das gleiche app-Kennwort können mehrere Apps. Weitere Informationen finden Sie unter [Was sind App-Kennwörter](multi-factor-authentication-end-user-app-passwords.md).
7. Klicken Sie auf **Fertig**.

## <a name="next-steps"></a>Nächste Schritte

- Ändern Sie Ihre Optionen und [Einstellungen für die zweistufige Überprüfung verwalten](multi-factor-authentication-end-user-manage-settings.md)
- Einrichten von [app-Kennwörter](multi-factor-authentication-end-user-app-passwords.md) für systemeigene Geräte-apps, die Überprüfung in zwei Schritten unterstützen.
- Checken Sie [Microsoft Authenticator app](multi-factor-authentication-microsoft-authenticator.md) für schnelle, sichere Authentifizierung auch bei nicht bestehender zellenservice.
