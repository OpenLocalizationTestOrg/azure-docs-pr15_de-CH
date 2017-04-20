<properties
    pageTitle="Erforderliche Komponenten Azure AD reporting-API zugreifen. | Microsoft Azure"
    description="Erfahren Sie mehr über die erforderlichen Komponenten auf Azure AD reporting API"
    services="active-directory"
    documentationCenter=""
    authors="dhanyahk"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="09/25/2016"
    ms.author="dhanyahk;markvi"/>

# <a name="prerequisites-to-access-the-azure-ad-reporting-api"></a>Erforderliche Azure AD reporting-API-Zugriff 

[Azure AD reporting APIs](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-reports-and-events-preview) ermöglichen den programmgesteuerten Zugriff auf die Daten durch eine Reihe von REST-basierten APIs. Sie können aus einer Vielzahl von Programmiersprachen und Tools diese APIs aufrufen.

Die reporting-API verwendet [OAuth](https://msdn.microsoft.com/library/azure/dn645545.aspx) zum Autorisieren des Zugriffs auf das Web APIs. 

Um den Zugriff auf die reporting-API vorzubereiten, müssen Sie:

1. Erstellen einer Anwendung in Azure AD-Mandanten 

2. Berechtigungen der Anwendung Zugriff auf die Azure AD-Daten

3. Sammeln Sie Konfiguration aus dem Verzeichnis

Fragen, Fragen oder Feedback wenden Sie [AAD Reporting helfen](mailto:aadreportinghelp@microsoft.com).


## <a name="create-an-azure-ad-application"></a>Erstellen einer Azure AD-Anwendung

Konfigurieren Sie Ihr Verzeichnis Azure AD reporting API zugreifen müssen Sie sich zum klassischen Azure-Portal mit einem Administratorkonto Azure-Abonnement anmelden, die auch Mitglied der globalen Administratorrolle des Verzeichnisses in Azure AD-Mandanten ist.

> [AZURE.IMPORTANT] Applikationen mit Anmeldeinformationen "Administratorrechte" So kann sehr leistungsstark, sodass Sie die Anwendung ID-Schlüssel Anmeldeinformationen schützen.


1. Klicken Sie im [klassischen Azure-Portal](https://manage.windowsazure.com)im linken Navigationsbereich auf **Active Directory**.

    ![Registrieren der Anwendung](./media/active-directory-reporting-api-prerequisites/01.png) 

2. Wählen Sie das Verzeichnis aus **active Directory** .

3. Klicken Sie im Menü oben auf **Applications**.

    ![Registrieren der Anwendung](./media/active-directory-reporting-api-prerequisites/02.png) 

4. Klicken Sie in der Leiste unten auf **Hinzufügen**.

    ![Registrieren der Anwendung](./media/active-directory-reporting-api-prerequisites/03.png) 

5. Auf der **Was möchten Sie tun?** Dialogfeld, klicken Sie auf **Mein Unternehmen entwickelten Anwendung hinzufügen**. 

    ![Registrieren der Anwendung](./media/active-directory-reporting-api-prerequisites/04.png) 


6. Im Dialogfeld **Angaben über die Anwendung** der folgenden Schritte: 

    ![Registrieren der Anwendung](./media/active-directory-reporting-api-prerequisites/05.png) 

    ein. Geben Sie in das Textfeld **Name** einen Namen (z.B.: Reporting-API-Anwendung).

    b. Wählen Sie **Web-Anwendung oder Web-API**.

    c. Klicken Sie auf **Weiter**.


7. Gehen Sie im Dialogfeld **Anwendung** für: 

    ![Registrieren der Anwendung](./media/active-directory-reporting-api-prerequisites/06.png) 

    ein. Geben Sie im Textfeld **Anmeldung URL** `https://localhost`.

    b. Geben Sie im Textfeld **App ID URI** ```https://localhost/ReportingApiApp```.

    c. Klicken Sie auf **abgeschlossen**.



## <a name="grant-your-application-permission-to-use-the-api"></a>Ihre Anwendung die Berechtigung der API

1. Klicken Sie im [klassischen Azure-Portal](https://manage.windowsazure.com/)im linken Navigationsbereich auf **Active Directory**.

    ![Registrieren der Anwendung](./media/active-directory-reporting-api-prerequisites/01.png) 

2. Wählen Sie das Verzeichnis aus **active Directory** .

3. Klicken Sie im Menü oben auf **Applications**.

    ![Registrieren der Anwendung](./media/active-directory-reporting-api-prerequisites/02.png)


3. Wählen Sie in der Anwendungsliste Ihrer neu erstellte Anwendung.

    ![Registrieren der Anwendung](./media/active-directory-reporting-api-prerequisites/07.png)

4. Klicken Sie im Menü oben auf **Konfigurieren**.

    ![Registrieren der Anwendung](./media/active-directory-reporting-api-prerequisites/08.png)


5. Klicken Sie im Abschnitt **Weitere Berechtigungen** für die Ressource **Azure Active Directory** auf die Dropdownliste **Berechtigungen** und wählen Sie **Verzeichnisdaten lesen**.

    ![Registrieren der Anwendung](./media/active-directory-reporting-api-prerequisites/09.png)


5. Klicken Sie in der Leiste unten auf **Speichern**.

    ![Registrieren der Anwendung](./media/active-directory-reporting-api-prerequisites/10.png)


## <a name="gather-configuration-settings-from-your-directory"></a>Sammeln Sie Konfiguration aus dem Verzeichnis

Dieser Abschnitt erläutert zu Folgendes Verzeichnis:

- Domänenname
- Client-ID
- Clientschlüssel

Sie benötigen diese Werte beim Konfigurieren der reporting-API aufgerufen. 


### <a name="get-your-domain-name"></a>Der Domain-Namen

1. Klicken Sie im [klassischen Azure-Portal](https://manage.windowsazure.com)im linken Navigationsbereich auf **Active Directory**.

    ![Registrieren der Anwendung](./media/active-directory-reporting-api-prerequisites/01.png) 

2. Wählen Sie das Verzeichnis aus **active Directory** .

3. Klicken Sie im Menü oben auf **Domänen**.

    ![Registrieren der Anwendung](./media/active-directory-reporting-api-prerequisites/11.png) 

4. Kopieren Sie in der Spalte **Domänenname** Ihren Domänennamen.

    ![Registrieren der Anwendung](./media/active-directory-reporting-api-prerequisites/12.png) 


### <a name="get-the-applications-client-id"></a>Abrufen der Anwendung Client-ID

1. Klicken Sie im [klassischen Azure-Portal](https://manage.windowsazure.com)im linken Navigationsbereich auf **Active Directory**.

    ![Registrieren der Anwendung](./media/active-directory-reporting-api-prerequisites/01.png) 

2. Wählen Sie das Verzeichnis aus **active Directory** .

3. Klicken Sie im Menü oben auf **Applications**.

    ![Registrieren der Anwendung](./media/active-directory-reporting-api-prerequisites/02.png) 

4. Wählen Sie in der Anwendungsliste Ihrer neu erstellte Anwendung.

    ![Registrieren der Anwendung](./media/active-directory-reporting-api-prerequisites/07.png)

4. Klicken Sie im Menü oben auf **Konfigurieren**.

    ![Registrieren der Anwendung](./media/active-directory-reporting-api-prerequisites/08.png)

4. Kopieren Sie die **Client-ID**

    ![Registrieren der Anwendung](./media/active-directory-reporting-api-prerequisites/13.png)


### <a name="get-the-applications-client-secret"></a>Die Anwendung geheimen abrufen

Zu geheimen der Anwendung müssen Sie einen neuen Schlüssel zu erstellen und die Werte auf den neuen Schlüssel speichern, weil es nicht möglich, diesen Wert später nicht mehr abrufen.

1. Klicken Sie im [klassischen Azure-Portal](https://manage.windowsazure.com)im linken Navigationsbereich auf **Active Directory**.

    ![Registrieren der Anwendung](./media/active-directory-reporting-api-prerequisites/01.png) 

2. Wählen Sie das Verzeichnis aus **active Directory** .

3. Klicken Sie im Menü oben auf **Applications**.

    ![Registrieren der Anwendung](./media/active-directory-reporting-api-prerequisites/02.png) 

4. Wählen Sie in der Anwendungsliste Ihrer neu erstellte Anwendung.

    ![Registrieren der Anwendung](./media/active-directory-reporting-api-prerequisites/07.png)

4. Klicken Sie im Menü oben auf **Konfigurieren**.

    ![Registrieren der Anwendung](./media/active-directory-reporting-api-prerequisites/08.png)

5. Im Abschnitt **Schlüssel** die folgenden Schritte: 

    ![Registrieren der Anwendung](./media/active-directory-reporting-api-prerequisites/14.png)

    ein. Wählen Sie aus der Liste Dauer

    b. Klicken Sie in der Leiste unten auf **Speichern**.

    ![Registrieren der Anwendung](./media/active-directory-reporting-api-prerequisites/10.png)

    c. Kopieren Sie den Schlüsselwert.

## <a name="next-steps"></a>Nächste Schritte

- Möchten Sie den Datenzugriff von Azure AD programmgesteuerte Berichterstattung API? [Erste Schritte mit der Azure Active Directory Reporting-API](active-directory-reporting-api-getting-started.md)Auschecken.

- Wenn Sie Active Directory Azure reporting erfahren möchten, finden Sie unter [Azure Active Directory-Berichte-Handbuch](active-directory-reporting-guide.md).  
