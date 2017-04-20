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

# <a name="service-to-serivce-authentication-with-data-lake-store-using-azure-active-directory"></a>Authentifizierung mit See Datenspeicher Azure Active Directory Service Service

> [AZURE.SELECTOR]
- [Dienst-Authentifizierung](data-lake-store-authenticate-using-active-directory.md)
- [Endbenutzer-Authentifizierung](data-lake-store-end-user-authenticate-using-active-directory.md)

Azure See Datenspeicher verwendet Azure Active Directory für die Authentifizierung. Vor der Erstellung einer Anwendung mit Azure See Datenspeicher oder Azure Data Lake Analytics müssen Sie zuerst entscheiden, wie Sie Ihre Anwendung in Azure Active Directory (Azure AD) zu authentifizieren. Die zwei wichtigsten Optionen sind verfügbar:

* Anwender-Authentifizierung und 
* Dienst-Authentifizierung. 

Beide Optionen führen die Anwendung mit einer OAuth 2.0-Token an jede Anforderung in Azure See Datenspeicher oder Azure Data Lake Analytics angefügt wird.

Dieser Artikel beschreibt, wie Erstellen einer Azure AD-Dienst Authentifizierung. Azure AD Konfiguration für Endbenutzer Authentifizierung Informationen finden Sie unter [Authentifizierung mit dem Datenspeicher Endbenutzer Azure Active Directory verwenden](data-lake-store-end-user-authenticate-using-active-directory.md).


## <a name="prerequisites"></a>Erforderliche Komponenten

* Ein Azure-Abonnement. Finden Sie [kostenlose Testversion von Azure zu erhalten](https://azure.microsoft.com/pricing/free-trial/).
* Ihre Abonnement-ID Sie können das Azure-Portal abrufen. Es ist z. B. Blade Konto See Datenspeicher ab.

    ![Abonnement-ID abrufen](./media/data-lake-store-authenticate-using-active-directory/get-subscription-id.png)

* Azure AD Domänennamens. Sie können diese Maus rechts oben das Azure-Portal abrufen. Im folgenden Screenshot der Domänenname ist **contoso.microsoft.com**und die GUID in Klammern ist die Mandanten-ID 

    ![AAD Domäne abrufen](./media/data-lake-store-authenticate-using-active-directory/get-aad-domain.png)

## <a name="service-to-service-authentication"></a>Dienst-Authentifizierung

Dies wird empfohlen, soll die Anwendung automatisch bei Azure AD ohne Endbenutzer ihre Anmeldeinformationen authentifizieren. Die Anwendung werden selbst authentifizieren für seine Anmeldeinformationen gültig sind, werden in Jahren angepasst werden können.

### <a name="what-do-i-need-to-use-this-approach"></a>Was muss ich diesen Ansatz verwenden?

* Azure Active Directory-Domänennamen. Dies ist bereits in der Voraussetzung dieses Artikels aufgeführt.

* Azure AD- **Anwendung**.

* Client-ID für Azure AD der Anwendung.

* Geheimen für Azure AD der Anwendung.

* Token Endpunkt für Azure AD der Anwendung.

* Für Azure AD der Anwendung Zugriff auf den Datenspeicher See Datei oder der Ordner oder die Datenanalyse See Sie Konto arbeiten.

Anleitung zum Erstellen einer Azure AD und für die oben aufgeführten Vorschriften konfigurieren finden Sie im Abschnitt unten [eine Active Directory-Anwendung erstellen](#create-an-active-directory-application) .

>[AZURE.NOTE] Standardmäßig ist Azure AD-Anwendung konfiguriert die geheimen, die von Azure AD-Anwendung abrufen kann. Jedoch wenn Sie Azure AD-Anwendung ein Zertifikat verwenden möchten, legen mithilfe von Azure PowerShell Azure AD-Web-Anwendung Sie Siehe Create [Dienstprinzipal mit](../resource-group-authenticate-service-principal.md#create-service-principal-with-certificate).

## <a name="create-an-active-directory-application"></a>Erstellen einer Active Directory-Anwendung

In diesem Abschnitt erfahren wir erstellen und Konfigurieren einer Azure AD Web-Anwendung für Dienst-Authentifizierung mit Azure See Datenspeicher mit Azure Active Directory. 


### <a name="step-1-create-an-azure-active-directory-application"></a>Schritt 1: Erstellen einer Active Directory Azure-Anwendung

>[AZURE.NOTE] Die folgenden Schritte verwenden Azure-Portal. Sie können auch eine Azure AD Anwendung [Azure PowerShell](../resource-group-authenticate-service-principal.md) oder [Azure CLI](../resource-group-authenticate-service-principal-cli.md)erstellen.

1. Melden Sie sich bei der Azure-Konto über das [Verwaltungsportal](https://manage.windowsazure.com/)an.

2. Wählen Sie im linken Bereich **Active Directory** .

     ![Wählen Sie Active Directory](./media/data-lake-store-authenticate-using-active-directory/active-directory.png)
     
3. Wählen Sie Active Directory zum Erstellen der neuen Anwendung verwenden möchten. Haben mehrere Active Directory soll in der Regel erstellen Sie die Anwendung im Verzeichnis Ihres Abonnements Speicherort. Sie können Ihr Abonnement für Applikationen in demselben Verzeichnis wie Ihr Abonnement nur Zugriff auf Ressourcen gewähren.  

     ![Wählen Sie das Verzeichnis](./media/data-lake-store-authenticate-using-active-directory/active-directory-details.png)
    
    
3. Klicken Sie die Programme im Verzeichnis auf **Anwendung**.

     ![Programme anzeigen](./media/data-lake-store-authenticate-using-active-directory/view-applications.png)

4. Erstellen eine Anwendung in diesem Verzeichnis angezeigt sollte ähnlich der folgenden Abbildung. Klicken Sie auf **eine Anwendung hinzufügen**

     ![Anwendung hinzufügen](./media/data-lake-store-authenticate-using-active-directory/create-application.png)

     Oder klicken Sie im unteren Bereich auf **Hinzufügen** .

     ![Hinzufügen](./media/data-lake-store-authenticate-using-active-directory/add-icon.png)

6. Geben Sie einen Namen für die Anwendung, und wählen Sie die Anwendung erstellen möchten. In diesem Lernprogramm erstellen Sie eine **Web-Anwendung oder WEB-API** und klicken Sie auf die Schaltfläche Weiter.

     ![Name-Anwendung](./media/data-lake-store-authenticate-using-active-directory/tell-us-about-your-application.png)

7. Geben Sie die Eigenschaften für Ihre Anwendung. **Zeichen auf URL**bereitstellen Sie, eine Website, die die Anwendung beschreibt. Das Vorhandensein der Website ist nicht gültig. **APP-ID-URI**bereitstellen Sie, die die Anwendung identifiziert.

     ![Anwendungseigenschaften](./media/data-lake-store-authenticate-using-active-directory/app-properties.png)

    Klicken Sie auf das Häkchen, um den Assistenten beenden und die Anwendung erstellen.

### <a name="step-2-get-client-id-client-secret-and-token-endpoint"></a>Schritt 2: Client-Id, geheimen und token Endpunkt abrufen

Programmgesteuert anmelden wird die Id für Ihre Anwendung benötigen. Wenn die Anwendung einen eigenen Anmeldeinformationen ausgeführt wird, benötigen Sie auch einen Authentifizierungsschlüssel.

1. Klicken Sie auf die Registerkarte **Konfigurieren** der Anwendung Kennwort konfigurieren.

     ![Anwendung konfigurieren](./media/data-lake-store-authenticate-using-active-directory/application-configure.png)

2. Kopieren Sie die **CLIENT-ID**
  
     ![Client-id](./media/data-lake-store-authenticate-using-active-directory/client-id.png)

3. Wenn die Anwendung einen eigenen Anmeldeinformationen ausgeführt wird, **die Abschnitt** Scrollen Sie und wählen Sie aus, wie lange das Kennwort gültig ist.

     ![Schlüssel](./media/data-lake-store-authenticate-using-active-directory/create-key.png)

4. Wählen Sie Ihren Schlüssel zu **Speichern** .

    ![Speichern](./media/data-lake-store-authenticate-using-active-directory/save-icon.png)

    Gespeicherte Schlüssel angezeigt, und Sie können. Sie können nicht den Schlüssel jetzt kopieren müssen später abrufen.

    ![gespeicherte Schlüssel](./media/data-lake-store-authenticate-using-active-directory/save-key.png)

5. Abrufen des token Endpunkts **Ansicht Endpunkte** am unteren Bildschirmrand auswählen und den Wert für Feld **Token OAuth 2.0-Endpunkt** wie unten dargestellt.  

    ![Mandanten-id](./media/data-lake-store-authenticate-using-active-directory/save-tenant.png)

### <a name="step-3-assign-the-azure-ad-application-to-the-azure-data-lake-store-account-file-or-folder-only-for-service-to-service-authentication"></a>Schritt 3: Zuweisen der Azure AD-Anwendung zu Azure See Datenspeicher Datei oder Ordner (nur für Dienst-Authentifizierung)

1. Melden Sie das neue [Azure-Portal an](https://portal.azure.com) und öffnen Sie Azure See Datenspeicher-Konto, das Sie zuvor erstellten Anwendung Azure Active Directory zuordnen möchten.

1. Klicken Sie in Ihrem Konto Blade See Datenspeicher auf **Daten-Explorer**.

    ![Verzeichnisse in dem Datenspeicher-Konto erstellen] (./media/data-lake-store-authenticate-using-active-directory/adl.start.data.explorer.png "Verzeichnisse Daten See-Konto erstellen")

2. Blatt **Daten-Explorer** auf die Datei oder den Ordner Zugriff auf Azure AD-Anwendung werden soll und **Klicken Sie auf**. Konfigurieren Sie Zugriff auf eine Datei klicken Sie aus der **Vorschau der** Blade **zugreifen** .

    ![Festlegen von ACLs auf See Daten] (./media/data-lake-store-authenticate-using-active-directory/adl.acl.1.png "Festlegen von ACLs auf See Daten")

3. **Access** -Blade listet standard Zugriff und benutzerdefinierten Zugriff Stamm bereits zugewiesen. Klicken Sie auf **Hinzufügen** , um Stufe ACLs hinzugefügt.

    ![Liste Standard- und benutzerdefinierten Zugriff] (./media/data-lake-store-authenticate-using-active-directory/adl.acl.2.png "Liste Standard- und benutzerdefinierten Zugriff")

4. Klicken Sie auf **Hinzufügen** , **Fügen Sie benutzerdefinierte Zugriff** Blade öffnen. Dieses Blatt klicken Sie auf **Benutzer oder Gruppe auswählen**und dann Blatt **Benutzer oder Gruppe** Suchen in Azure Active Directory erstellten Sicherheitsgruppe. Haben viele Suche verwenden Sie das Textfeld oben auf den Gruppennamen zu filtern. Klicken Sie auf die Gruppe, die Sie hinzufügen möchten und klicken Sie auf **auswählen**.

    ![Gruppe hinzufügen] (./media/data-lake-store-authenticate-using-active-directory/adl.acl.3.png "Gruppe hinzufügen")

5. Klicken Sie auf **Select-Berechtigungen**und die Berechtigungen möchten Berechtigungen standardmäßig ACL, ACL oder beides zugreifen. Klicken Sie auf **OK**.

    ![Zuweisen von Berechtigungen zu einer Gruppe] (./media/data-lake-store-authenticate-using-active-directory/adl.acl.4.png "Zuweisen von Berechtigungen zu einer Gruppe")

    Weitere Informationen zu Berechtigungen im Datenspeicher See und Standard-Access ACLs finden Sie in der [Zugriffskontrolle im Datenspeicher See](data-lake-store-access-control.md).


6. Blatt **Hinzufügen benutzerdefinierten Zugriff** klicken Sie auf **OK**. Die neu hinzugefügte Gruppe zugeordneten Berechtigungen wird jetzt im Blatt **Zugriff** aufgeführt.

    ![Zuweisen von Berechtigungen zu einer Gruppe] (./media/data-lake-store-authenticate-using-active-directory/adl.acl.5.png "Zuweisen von Berechtigungen zu einer Gruppe") 


## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel eine Azure AD Web-Anwendung erstellt und die Informationen, die in Clientanwendungen, .NET SDK, Java SDK usw. zu erstellen. Sie können in den folgenden Artikeln nun, die zur Verwendung von Azure AD der Anwendung zuerst See Datenspeicher authentifizieren und führen Sie andere Vorgänge im Speicher sprechen.

- [Erste Schritte mit Azure See Datenspeicher mit .NET SDK](data-lake-store-get-started-net-sdk.md)
- [Erste Schritte mit Azure See Datenspeicher mit Java SDK](data-lake-store-get-started-java-sdk.md)
