<properties
    pageTitle="Microsoft Authenticator app für Mobiltelefone | Microsoft Azure"
    description="Informationen Sie zum Aktualisieren auf die neueste Version von Azure Authenticator."
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
    ms.date="08/22/2016"
    ms.author="kgremban"/>

# <a name="microsoft-authenticator"></a>Microsoft Authentifikator

Microsoft Authenticator app bietet eine zusätzliche Sicherheitsstufe in Azure-Konto (z. B. bsimon@contoso.onmicrosoft.com), Ihrem lokalen Konto arbeiten (z. B. bsimon@contoso.com), oder Ihr Microsoft-Konto (z. B. bsimon@outlook.com).

Die Anwendung funktioniert auf zwei Arten:

- **Benachrichtigung**. Die Anwendung hilft verhindert unbefugten Zugriff auf Konten und betrügerische Transaktionen per Benachrichtigung an Ihr Smartphone oder Tablet beenden. Einfach zeigen Sie die Benachrichtigung an und wenn es legitim ist, wählen **Sie prüfen**. Andernfalls können Sie **Verweigern**auswählen. Informationen zu verweigern Benachrichtigung finden Sie unter verweigern und Bericht Betrug Feature für die kombinierte Authentifizierung verwenden.

- **Verifizierungscode ein Kennwort**. Die app kann als Softwaretoken generiert einen Verifizierungscode OAuth verwendet werden. Sie geben den Code Wenn die App in den Bildschirm Anmeldung mit Benutzername und Kennwort aufgefordert. Der Bestätigungscode stellt eine zweite Form der Authentifizierung.

Mit der Veröffentlichung von Microsoft Authenticator app wird die alte Azure Authenticator app ersetzt.  Azure Authenticator app weiterhin funktionieren, aber wenn neue Microsoft Authenticator App verschieben möchten, in diesem Artikel helfen.  

## <a name="install-the-app"></a>Installieren der Anwendung

Microsoft Authenticator app ist für [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072)und [IOS](http://go.microsoft.com/fwlink/?Linkid=825073).

## <a name="add-accounts-to-the-app"></a>Der app Konten hinzufügen

Verwenden Sie für jedes Konto, das die Anwendung Microsoft Authenticator hinzufügen möchten eines der folgenden Verfahren.

### <a name="add-an-account-to-the-app-by-using-the-qr-code-scanner"></a>Die Anwendung mit dem QR Code Scanner fügen Sie ein Konto hinzu

1. Gehen Sie zum Bildschirm Sicherheit Überprüfung Settings.  Informationen zu diesem finden Sie unter [Ändern der Benutzerberechtigungen](multi-factor-authentication-end-user-manage-settings.md).

2. Wählen Sie **Konfigurieren**.

    ![Die Schaltfläche "konfigurieren" auf die Sicherheit überprüfen](./media/multi-factor-authentication-azure-authenticator/azureauthe.png)

    Dadurch wird ein Bildschirm mit einem QR-Code auf.

    ![Bildschirm, der den QR-Code enthält](./media/multi-factor-authentication-azure-authenticator/barcode2.png)

3. Öffnen Sie Microsoft Authenticator app. Wählen Sie auf dem Bildschirm **Konten** **+**, und geben Sie dann ein Arbeits- oder Schulcomputer Konto hinzufügen möchten.

    ![Der Bildschirm Konten mit Pluszeichen](./media/multi-factor-authentication-azure-authenticator/addaccount3.png)

    ![Bildschirm für die Arbeit oder Schule Konto angeben](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan.png)

4. Verwenden der Kamera QR-Code Scannen und wählen QR Code-Bildschirm zu **geschehen** .

    ![Bildschirm einen QR-Code Scannen](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan2.png)

    Wenn Ihre Kamera nicht funktioniert, können Sie den QR-Code und die URL manuell eingeben. Weitere Informationen finden Sie in [der Anwendung ein Konto manuell hinzufügen](#add-an-account-to-the-app-manually).

5. Warten Sie, während das Konto aktiviert ist. Wenn die Aktivierung abgeschlossen ist, wählen Sie **Kontakt**.  Eine Benachrichtigung oder einen Bestätigungscode an Ihr Telefon gesendet.  Wählen Sie **Überprüfen**.

    ![Auswählen, wo Sie überprüfen anmelden](./media/multi-factor-authentication-end-user-first-time-mobile-app/verify.png)

6. Wenn Ihr Unternehmen eine PIN zur Genehmigung anmelden Überprüfung erfordert, geben sie.

    ![Feld für die Eingabe einer PIN](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan3.png)

7. Wählen Sie nach Abschluss der PIN-Eingabe **Schließen**. An diesem Punkt sollte Ihre Überprüfung erfolgreich.
8. Wir empfehlen, Ihre Mobiltelefonnummer eingeben, wenn Sie Ihre Anwendung zugreifen. Geben Sie Ihr Land aus der Dropdown-Liste, und geben Sie Ihre Mobiltelefonnummer in das Feld neben den Namen des Landes. Wählen Sie **Weiter**.
9. Zu diesem Zeitpunkt haben Sie die Kontaktmethode eingerichtet. Jetzt ist es Zeit app Kennwörter nicht Browser apps, wie Outlook 2010 oder älter. Wählen Sie diese apps nicht verwenden, **Fertig**. Andernfalls mit dem nächsten Schritt fortfahren.

    ![Bildschirm ein app-Kennwort erstellen](./media/multi-factor-authentication-end-user-first-time-mobile-app/step4.png)

10. Wenn apps-Browser verwenden, kopieren Sie bereitgestellte Anwendung Kennwort und fügen Sie das Kennwort in Ihre apps. Schritte auf einzelne apps wie Outlook und Lync finden Sie ändern das Kennwort per e-Mail zu app-Kennwort und das Kennwort in der Anwendung das app-Kennwort ändern.
11. Wählen Sie **Fertig**.

Das neue Konto sollte jetzt auf dem Bildschirm **Konten** angezeigt werden.

![Bildschirm Konten](./media/multi-factor-authentication-azure-authenticator/accounts.png)

### <a name="add-an-account-to-the-app-manually"></a>Der app manuell ein Konto hinzufügen

1. Gehen Sie zum Bildschirm Sicherheit Überprüfung Settings.  Informationen zu diesem finden Sie unter [Ändern der Benutzerberechtigungen](multi-factor-authentication-end-user-manage-settings.md).

2. Wählen Sie **Konfigurieren**.

    ![Die Schaltfläche "konfigurieren" auf die Sicherheit überprüfen](./media/multi-factor-authentication-azure-authenticator/azureauthe.png)

    Dadurch wird ein Bildschirm mit einem QR-Code auf.  Beachten Sie den Code und die URL.

    ![Bildschirm, die QR Code und URL](./media/multi-factor-authentication-azure-authenticator/barcode2.png)

3. Öffnen Sie Microsoft Authenticator app. Wählen Sie auf dem Bildschirm **Konten** **+**, und geben Sie dann ein Arbeits- oder Schulcomputer Konto hinzufügen möchten.

    ![Der Bildschirm Konten mit Pluszeichen](./media/multi-factor-authentication-azure-authenticator/addaccount3.png)

    ![Bildschirm für die Arbeit oder Schule Konto angeben](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan.png)

4. Wählen Sie den Scanner **Code manuell eingeben**.

    ![Bildschirm einen QR-Code Scannen](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan2.png)

5. Geben Sie den Code und die URL in die entsprechenden Felder in der Anwendung.

    ![Bildschirm für die Eingabe von Code und URL](./media/multi-factor-authentication-azure-authenticator/manual.png)

    ![Bildschirm für die Eingabe von Code und URL](./media/multi-factor-authentication-end-user-first-time-mobile-app/addaccount2.png)

6. Warten Sie, während das Konto aktiviert ist. Wenn die Aktivierung abgeschlossen ist, wählen Sie **Kontakt**. Eine Benachrichtigung oder einen Bestätigungscode an Ihr Telefon gesendet. Wählen Sie **Überprüfen**.

Das neue Konto sollte jetzt auf dem Bildschirm **Konten** angezeigt werden.

![Bildschirm Konten](./media/multi-factor-authentication-azure-authenticator/accounts.png)

### <a name="add-an-account-to-the-app-by-using-touch-id"></a>Hinzufügen eines Kontos zur app mit Touch ID

Microsoft Authenticator app für iOS unterstützt Touch-ID.  Azure Multifaktor-Authentifizierung ermöglicht PIN für Geräte erforderlich. Mit Touch ID müssen nicht iOS Benutzer eine PIN eingeben. Sie können stattdessen Fingerabdruck überprüfen und **Genehmigen**auswählen.

Touch-ID mit Microsoft Authenticator ist einfach. Sie führen eine normale Überprüfung Herausforderung mit einer PIN. Wenn Ihr Gerät Touch ID unterstützt, wird Microsoft Authenticator es automatisch für dieses Konto eingerichtet.

![Überprüfung der Touch ID setup](./media/multi-factor-authentication-azure-authenticator/touchid1.png)

Von dem Punkt, wenn Sie Ihre Anmeldung überprüfen müssen, Sie empfangene Push-Benachrichtigung und Fingerabdruck anstatt PIN scannen.

![Push-Benachrichtigung](./media/multi-factor-authentication-azure-authenticator/touchid2.png)

## <a name="uninstall-the-old-azure-authentication-app"></a>Deinstallieren Sie die alte Azure Authentifizierung app

Nach dem Hinzufügen der Konten der neuen App können Sie von Ihrem Telefon alte Anwendung deinstallieren.

## <a name="delete-an-account"></a>Konto löschen

Zum Entfernen eines Kontos aus Microsoft Authenticator app, wählen Sie das Konto und wählen Sie dann **Löschen**.

![Schaltfläche Löschen](./media/multi-factor-authentication-azure-authenticator/remove.png)
