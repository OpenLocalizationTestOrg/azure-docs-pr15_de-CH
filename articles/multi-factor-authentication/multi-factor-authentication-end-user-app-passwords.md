<properties
    pageTitle="Was sind App-Kennwörter in Azure MFA?"
    description="Auf dieser Seite helfen Benutzern kennen Azure MFA was app Kennwörter sind und sie mit Verwendungszweck betrachten."
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



# <a name="what-are-app-passwords-in-azure-multi-factor-authentication"></a>Was sind App-Kennwörter in Azure mehrstufige Authentifizierung?

Mehrstufige Authentifizierung unterstützt bestimmte apps-Browser wie Apple systemeigene e-Mail-Clients, die Exchange ActiveSync verwendet derzeit nicht. Mehrstufige Authentifizierung wird pro Benutzer aktiviert. Dies bedeutet, dass ein Benutzer für die kombinierte Authentifizierung aktiviert wurde und sie apps-Browser verwenden möchten, sie tun können. Ein app-Kennwort kann auftreten.

>[AZURE.NOTE] Moderne Authentifizierung für Office 2013-Clients
>
> Office 2013-Clients (einschließlich Outlook) jetzt neue Authentifizierungsprotokolle unterstützen und ermöglicht mehrstufige Authentifizierung unterstützen.  Dies bedeutet, dass nach der Aktivierung app-Kennwörter nicht für Office 2013-Clients erforderlich sind.  Weitere Informationen finden Sie unter [Office 2013 moderne Authentifizierung public Preview angekündigt](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/).

## <a name="how-to-use-app-passwords"></a>Wie app-Kennwörter

Es folgen einige Dinge wie app Kennwörter verwenden.

- Das Kennwort wird automatisch generiert und nicht vom Benutzer angegeben. Dies ist da automatisch generierte Kennwort ist für Angreifer zu erraten und sicherer.
- Derzeit sind maximal 40 Kennwörter pro Benutzer. Wenn Sie versuchen, erstellen Sie die Grenze erreicht haben, werden Sie aufgefordert, eine vorhandene Anwendung Kennwörter löschen, um eine neue zu erstellen.
- Es wird empfohlen, dass app-Kennwörter pro Gerät und pro Anwendung nicht erstellt werden. Beispielsweise können Sie ein app-Kennwort für Ihr Notebook erstellen und das app-Kennwort für alle Ihre Anwendung diese Laptops verwenden.
- Sie ein app-Kennwort beim ersten erhalten Sie anmelden.  Benötigen Sie weitere, können Sie diese erstellen.

![Setup](./media/multi-factor-authentication-end-user-app-passwords/app.png)

Haben Sie ein app-Kennwort verwenden Sie diese anstelle der ursprünglichen Kennworts mit diesen apps-Browser.  Zum Beispiel, wenn Sie mehrstufige Authentifizierung und Apple systemeigene e-Mail-Client auf Ihrem Telefon verwenden.  Verwenden Sie app-Kennwort, weiterhin und mehrstufige Authentifizierung umgehen können.

## <a name="creating-and-deleting-app-passwords"></a>Erstellen und Löschen von app-Kennwörter
Während der ersten Anmeldung erhalten Sie ein app-Kennwort, das Sie verwenden können.  Darüber hinaus können auch erstellen und später app Kennwörter löschen.  Wie Sie dabei vorgehen, hängt davon ab, wie mehrstufige Authentifizierung verwenden.  Wählen Sie die meisten auf Sie angewendet wird.

Verwendung von kombinierten authentiation|Beschreibung
:------------- | :------------- |
[Verwenden sie mit Office 365](#creating-and-deleting-app-passwords-with-office-365)|  Dies bedeutet, dass app-Kennwörter über Office 365-Portal erstellen möchten.
[Ich weiß es nicht](#creating-and-deleting-app-passwords-with-myapps-portal)|Dies bedeutet, dass möchten app Kennwörter durch [https://myapps.microsoft.com](https://myapps.microsoft.com)
[Verwenden sie mit Microsoft Azure](#create-app-passwords-in-the-azure-portal)| Dies bedeutet, dass wird app Kennwörter über Azure-Portal erstellen.

## <a name="creating-and-deleting-app-passwords-with-office-365"></a>Erstellen und Löschen von app-Kennwörter mit Office 365

Wenn Sie mehrstufige Authentifizierung mit Office 365, die Sie erstellen und app-Kennwörter über Office 365-Portal löschen möchten verwenden.

### <a name="to-create-app-passwords-in-the-office-365-portal"></a>App-Kennwörter in Office 365-Portal erstellen
--------------------------------------------------------------------------------

1. Melden Sie sich bei [Office 365-Portal](https://login.microsoftonline.com/).
2. In der rechten oberen Ecke wählen Sie das Widget und Office 365 Settings.
3. Klicken Sie auf zusätzliche Überprüfung.
4. Klicken Sie auf der rechten Seite auf den Link **Meine Rufnummern Konto Sicherheit aktualisieren.** 
 ![Setup](./media/multi-factor-authentication-end-user-manage/o365a.png)
5. Dadurch gelangen Sie zur Seite, die Einstellungen ändern dürfen.
![Cloud](./media/multi-factor-authentication-end-user-manage/o365b.png)
6. Klicken Sie auf oben neben zusätzliche Überprüfung **app Kennwörter.**
7. Klicken Sie auf **Erstellen**.
![Cloud](./media/multi-factor-authentication-end-user-app-passwords-create-o365/apppass.png)
8. Geben Sie einen Namen für das app-Kennwort, und klicken Sie auf **Weiter**.
![Erstellen Sie ein app-Kennwort](./media/multi-factor-authentication-end-user-app-passwords/create1.png)
9. App-Kennwort in die Zwischenablage kopieren und in Ihre Anwendung einfügen.
![Erstellen Sie ein app-Kennwort](./media/multi-factor-authentication-end-user-app-passwords/create2.png)


### <a name="to-delete-app-passwords-using-the-office-365-portal"></a>App-Kennwörter mit Office 365-Portal löschen
--------------------------------------------------------------------------------


1. Melden Sie sich bei [Office 365-Portal](https://login.microsoftonline.com/).
2. In der rechten oberen Ecke wählen Sie das Widget und Office 365 Settings.
3. Klicken Sie auf zusätzliche Überprüfung.
4. Klicken Sie auf der rechten Seite auf den Link **Meine Rufnummern Konto Sicherheit aktualisieren.** 
 ![Setup](./media/multi-factor-authentication-end-user-manage/o365a.png)
5. Dadurch gelangen Sie zur Seite, die Einstellungen ändern dürfen.
![Cloud](./media/multi-factor-authentication-end-user-manage/o365b.png)
6. Klicken Sie auf oben neben zusätzliche Überprüfung **app Kennwörter.**
7. Neben der app-Kennwort zu löschen, klicken Sie auf **Löschen**.
![Löschen Sie ein app-Kennwort](./media/multi-factor-authentication-end-user-app-passwords/delete1.png)
8. Bestätigen Sie den Vorgang, indem Sie auf **Ja**.
![Löschen bestätigen](./media/multi-factor-authentication-end-user-app-passwords/delete2.png)
9. Gelöschte app-Kennwort können Sie **Schließen**klicken.
![Schließen](./media/multi-factor-authentication-end-user-app-passwords/delete3.png)


## <a name="creating-and-deleting-app-passwords-with-myapps-portal"></a>Erstellen und Löschen von Kennwörtern mit Myapps app.
Wenn Sie nicht sicher sind, wie mehrstufige Authentifizierung verwenden, können Sie immer erstellen und app-Kennwörter über das Portal Myapps löschen.

### <a name="to-create-an-app-password-using-the-myapps-portal"></a>Über das Portal Myapps app-Passwort erstellen

1. Melden Sie sich bei [https://myapps.microsoft.com](https://myapps.microsoft.com)
2. Wählen Sie oben Profil.
3. Wählen Sie zusätzliche Überprüfung.
![Cloud](./media/multi-factor-authentication-end-user-manage/myapps1.png)
4. Dadurch gelangen Sie zur Seite, die Einstellungen ändern dürfen.
![Setup](./media/multi-factor-authentication-end-user-manage-myapps/proofup.png)
5. Klicken Sie auf oben neben zusätzliche Überprüfung **app Kennwörter.**
6. Klicken Sie auf **Erstellen**.
![Erstellen Sie ein app-Kennwort](./media/multi-factor-authentication-end-user-app-passwords/create3.png)
7. Geben Sie einen Namen für das app-Kennwort, und klicken Sie auf **Weiter**.
![Erstellen Sie ein app-Kennwort](./media/multi-factor-authentication-end-user-app-passwords/create1.png)
8. App-Kennwort in die Zwischenablage kopieren und in Ihre Anwendung einfügen.
![Erstellen Sie ein app-Kennwort](./media/multi-factor-authentication-end-user-app-passwords/create2.png)

### <a name="to-delete-an-app-password-using-the-myapps-portal"></a>So löschen Sie eine app Kennwort Myapps portal

1. Melden Sie sich bei [https://myapps.microsoft.com](https://myapps.microsoft.com)
2. Wählen Sie oben Profil.
3. Wählen Sie zusätzliche Überprüfung.
![Cloud](./media/multi-factor-authentication-end-user-manage/myapps1.png)
4. Dadurch gelangen Sie zur Seite, die Einstellungen ändern dürfen.
![Setup](./media/multi-factor-authentication-end-user-manage-myapps/proofup.png)
5. Klicken Sie auf oben neben zusätzliche Überprüfung **app Kennwörter.**
6. Neben der app-Kennwort zu löschen, klicken Sie auf **Löschen**.
![Löschen Sie ein app-Kennwort](./media/multi-factor-authentication-end-user-app-passwords/delete1.png)
7. Bestätigen Sie den Vorgang, indem Sie auf **Ja**.
![Löschen bestätigen](./media/multi-factor-authentication-end-user-app-passwords/delete2.png)
8. Gelöschte app-Kennwort können Sie **Schließen**klicken.
![Schließen](./media/multi-factor-authentication-end-user-app-passwords/delete3.png)


## <a name="create-app-passwords-in-the-azure-portal"></a>App-Kennwörter im Azure-Portal erstellen

Verwenden Sie mehrstufige Authentifizierung mit Azure app Kennwörter über Azure-Portal erstellen möchten.

### <a name="to-create-app-passwords-in-the-azure-portal"></a>App-Kennwörter im Azure-Portal erstellen

1. Anmelden bei Azure-Verwaltungsportal.
2. Oben mit der rechten Maustaste auf Ihren Benutzernamen, und wählen Sie zusätzliche Prüfung.
3. Wählen Sie auf der Seite Proofup oben app-Kennwörter
4. Klicken Sie auf **Erstellen**.
5. Geben Sie einen Namen für das app-Kennwort, und klicken Sie auf **Weiter**
6. App-Kennwort in die Zwischenablage kopieren und in Ihre Anwendung einfügen.
![Cloud](./media/multi-factor-authentication-end-user-app-passwords-create-azure/app2.png)

### <a name="to-delete-app-passwords-in-the-azure-portal"></a>App-Kennwörter im Azure-Portal löschen

1. Anmelden bei Azure-Verwaltungsportal.
2. Oben mit der rechten Maustaste auf Ihren Benutzernamen, und wählen Sie zusätzliche Prüfung.
3. Klicken Sie auf oben neben zusätzliche Überprüfung **app Kennwörter.**
4. Neben der app-Kennwort zu löschen, klicken Sie auf **Löschen**.
5. Bestätigen Sie den Vorgang, indem Sie auf **Ja**.
6. Gelöschte app-Kennwort können Sie **Schließen**klicken.
![Schließen](./media/multi-factor-authentication-end-user-app-passwords/delete3.png)
