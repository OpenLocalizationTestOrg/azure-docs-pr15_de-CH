<properties
   pageTitle="Mit See Datenspeicher mithilfe von Active Directory authentifizieren | Microsoft Azure"
   description="Informationen Sie zum See Datenspeicher Active Directory authentifizieren"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/17/2016"
   ms.author="nitinme"/>

# <a name="end-user-authentication-with-data-lake-store-using-azure-active-directory"></a>Endbenutzer-Authentifizierung mit See Datenspeicher Azure Active Directory

> [AZURE.SELECTOR]
- [Dienst-Authentifizierung](data-lake-store-authenticate-using-active-directory.md)
- [Endbenutzer-Authentifizierung](data-lake-store-end-user-authenticate-using-active-directory.md)


Azure See Datenspeicher verwendet Azure Active Directory für die Authentifizierung. Vor der Erstellung einer Anwendung mit Azure See Datenspeicher oder Azure Data Lake Analytics müssen Sie zuerst entscheiden, wie Sie Ihre Anwendung in Azure Active Directory (Azure AD) zu authentifizieren. Die zwei wichtigsten Optionen sind verfügbar:

* Anwender-Authentifizierung und 
* Dienst-Authentifizierung. 

Beide Optionen führen die Anwendung mit einer OAuth 2.0-Token an jede Anforderung in Azure See Datenspeicher oder Azure Data Lake Analytics angefügt wird.

Dieser Artikel beschreibt, wie Erstellen einer Azure AD für Endbenutzer Authentifizierung. Hinweise zur Konfiguration Azure AD-Dienst Authentifizierung Siehe [Dienst - Authentifizierung mit dem Datenspeicher Azure Active Directory verwenden](data-lake-store-authenticate-using-active-directory.md).

## <a name="prerequisites"></a>Erforderliche Komponenten

* Ein Azure-Abonnement. Finden Sie [kostenlose Testversion von Azure zu erhalten](https://azure.microsoft.com/pricing/free-trial/).
* Ihre Abonnement-ID Sie können das Azure-Portal abrufen. Es ist z. B. Blade Konto See Datenspeicher ab.

    ![Abonnement-ID abrufen](./media/data-lake-store-end-user-authenticate-using-active-directory/get-subscription-id.png)

* Azure AD Domänennamens. Sie können diese Maus rechts oben das Azure-Portal abrufen. Im folgenden Screenshot der Domänenname ist **contoso.microsoft.com**und die GUID in Klammern ist die Mandanten-ID 

    ![AAD Domäne abrufen](./media/data-lake-store-end-user-authenticate-using-active-directory/get-aad-domain.png)

## <a name="end-user-authentication"></a>Endbenutzer-Authentifizierung

Dies wird empfohlen, wenn ein Endbenutzer die Anwendung über Azure AD anmelden möchten. Die Anwendung werden die gleiche Zugriffsebene als Endbenutzer Azure Ressourcen zugreifen, die angemeldet. Der Endbenutzer müssen ihre Anmeldeinformationen regelmäßig in Ihrer Anwendung zugreifen.

Das Ergebnis des Endbenutzers Anmelden ist, dass die Anwendung ein Zugriffstoken und ein Aktualisierungstoken erhält. Das Zugriffstoken wird jede Anforderung an See Datenspeicher oder See Datenanalyse an und gilt standardmäßig eine Stunde. Aktualisierungstoken wird ein Zugriffstoken erhalten und ist gültig für bis zu zwei Wochen standardmäßig regelmäßig verwendet. Sie können zwei verschiedene Ansätze für Endbenutzer anmelden.

### <a name="using-the-oauth-20-pop-up"></a>Verwenden das OAuth 2.0-Popup

Die Anwendung kann ein Popup OAuth 2.0 Autorisierung Auslösen in dem Endbenutzer ihre Anmeldeinformationen eingeben kann. Dieses Popupfenster funktioniert auch mit Azure AD zwei-Faktor-Authentifizierung (2FA), wenn erforderlich. 

>[AZURE.NOTE] Diese Methode ist noch nicht in Azure AD Authentifizierung Library (ADAL) für Python oder Java unterstützt.

### <a name="directly-passing-in-user-credentials"></a>Benutzeranmeldeinformationen übergeben direkt

Die Anwendung bietet direkt Benutzeranmeldeinformationen Azure AD. Diese Methode funktioniert nur mit Organisationseinheiten ID Benutzerkonten; Es ist nicht kompatibel mit / "live ID" Benutzerkonten einschließlich der Endung @outlook.com oder @live.com. Darüber hinaus ist diese Methode nicht mit Benutzerkonten, die Azure AD zweistufige Authentifizierung (2FA).

### <a name="what-do-i-need-to-use-this-approach"></a>Was muss ich diesen Ansatz verwenden?

* Azure Active Directory-Domänennamen. Dies ist bereits in der Voraussetzung dieses Artikels aufgeführt.

* Azure AD- **Anwendung**

* Client-ID für Azure AD der Anwendung

* Antwort URI Azure AD der Anwendung

* Delegierte Berechtigungen

Anleitung zum Erstellen einer Azure AD und für die oben aufgeführten Vorschriften konfigurieren finden Sie im Abschnitt unten [eine Active Directory-Anwendung erstellen](#create-an-active-directory-application) . 

## <a name="create-an-active-directory-application"></a>Erstellen einer Active Directory-Anwendung

In diesem Abschnitt erfahren wir erstellen und Konfigurieren einer Azure AD Web-Anwendung für Endbenutzer Authentifizierung mit Azure See Datenspeicher mit Azure Active Directory.


### <a name="step-1-create-an-azure-active-directory-application"></a>Schritt 1: Erstellen einer Active Directory Azure-Anwendung

>[AZURE.NOTE] Die folgenden Schritte verwenden Azure-Portal. Sie können auch eine Azure AD Anwendung [Azure PowerShell](../resource-group-authenticate-service-principal.md) oder [Azure CLI](../resource-group-authenticate-service-principal-cli.md)erstellen.

1. Melden Sie sich bei der Azure-Konto über das [Verwaltungsportal](https://manage.windowsazure.com/)an.

2. Wählen Sie im linken Bereich **Active Directory** .

     ![Wählen Sie Active Directory](./media/data-lake-store-end-user-authenticate-using-active-directory/active-directory.png)
     
3. Wählen Sie Active Directory zum Erstellen der neuen Anwendung verwenden möchten. Haben mehrere Active Directory soll in der Regel erstellen Sie die Anwendung im Verzeichnis Ihres Abonnements Speicherort. Sie können Ihr Abonnement für Applikationen in demselben Verzeichnis wie Ihr Abonnement nur Zugriff auf Ressourcen gewähren.  

     ![Wählen Sie das Verzeichnis](./media/data-lake-store-end-user-authenticate-using-active-directory/active-directory-details.png)
    
    
3. Klicken Sie die Programme im Verzeichnis auf **Anwendung**.

     ![Programme anzeigen](./media/data-lake-store-end-user-authenticate-using-active-directory/view-applications.png)

4. Erstellen eine Anwendung in diesem Verzeichnis angezeigt sollte ähnlich der folgenden Abbildung. Klicken Sie auf **eine Anwendung hinzufügen**

     ![Anwendung hinzufügen](./media/data-lake-store-end-user-authenticate-using-active-directory/create-application.png)

     Oder klicken Sie im unteren Bereich auf **Hinzufügen** .

     ![Hinzufügen](./media/data-lake-store-end-user-authenticate-using-active-directory/add-icon.png)

6. Geben Sie einen Namen für die Anwendung, und wählen Sie die Anwendung erstellen möchten. In diesem Lernprogramm erstellen Sie eine **Web-Anwendung oder WEB-API** und klicken Sie auf die Schaltfläche Weiter.

     ![Name-Anwendung](./media/data-lake-store-end-user-authenticate-using-active-directory/tell-us-about-your-application.png)

7. Geben Sie die Eigenschaften für Ihre Anwendung. **Zeichen auf URL**bereitstellen Sie, eine Website, die die Anwendung beschreibt. Das Vorhandensein der Website ist nicht gültig. **APP-ID-URI**bereitstellen Sie, die die Anwendung identifiziert.

     ![Anwendungseigenschaften](./media/data-lake-store-end-user-authenticate-using-active-directory/app-properties.png)

    Klicken Sie auf das Häkchen, um den Assistenten beenden und die Anwendung erstellen.

### <a name="step-2-get-client-id-reply-uri-and-set-delegated-permissions"></a>Schritt 2: Erhalten Sie Client-Id zu, Antworten Sie URI und festlegen delegierte Berechtigungen

1. Klicken Sie auf die Registerkarte **Konfigurieren** der Anwendung Kennwort konfigurieren.

     ![Anwendung konfigurieren](./media/data-lake-store-end-user-authenticate-using-active-directory/application-configure.png)

2. Kopieren Sie die **CLIENT-ID**
  
     ![Client-id](./media/data-lake-store-end-user-authenticate-using-active-directory/client-id.png)

3. Abschnitt **auf** **Antwort URI**kopieren

    ![Client-id](./media/data-lake-store-end-user-authenticate-using-active-directory/aad-end-user-auth-get-reply-uri.png)

4. Klicken Sie unter **Berechtigungen für andere Programme**auf **Anwendung hinzufügen**

    ![Client-id](./media/data-lake-store-end-user-authenticate-using-active-directory/aad-end-user-auth-set-permission-1.png)

5. Wählen Sie im Assistenten **Berechtigungen für andere Programme** **Daten See Azure** und **Windows Azure** **Service Management API**, und klicken Sie auf das Häkchen.

6. Standardmäßig ist die **Delegierte Berechtigungen** für die neu hinzugefügten Dienste auf NULL gesetzt. Auf **Berechtigungen delegiert** Dropdown für Azure Data Lake und Windows Azure Management Service und Kontrollkästchen Sie die verfügbaren Werte auf 1 festgelegt. Das Ergebnis sollte wie folgt aussehen.

     ![Client-id](./media/data-lake-store-end-user-authenticate-using-active-directory/aad-end-user-auth-set-permission-2.png)

7. Klicken Sie auf **Speichern**.


## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel eine Azure AD Web-Anwendung erstellt und die Informationen, die in Clientanwendungen, .NET SDK, Java SDK usw. zu erstellen. Sie können in den folgenden Artikeln nun, die zur Verwendung von Azure AD der Anwendung zuerst See Datenspeicher authentifizieren und führen Sie andere Vorgänge im Speicher sprechen.

- [Erste Schritte mit Azure See Datenspeicher mit .NET SDK](data-lake-store-get-started-net-sdk.md)
- [Erste Schritte mit Azure See Datenspeicher mit Java SDK](data-lake-store-get-started-java-sdk.md)
