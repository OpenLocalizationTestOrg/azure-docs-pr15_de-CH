<properties
   pageTitle="Service Portal Prinzipal erstellen | Microsoft Azure"
   description="Beschreibt das Erstellen eines neuen Active Directory-Anwendung und Dienstprinzipalnamen mit der rollenbasierte Zugriffskontrolle in Azure-Ressourcen-Manager zum Verwalten des Zugriffs auf Ressourcen verwendet werden kann."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/07/2016"
   ms.author="tomfitz"/>

# <a name="use-portal-to-create-active-directory-application-and-service-principal-that-can-access-resources"></a>Verwenden Sie Portal Active Directory Anwendung und Dienstprinzipalnamen, die auf Ressourcen zugreifen können

> [AZURE.SELECTOR]
- [PowerShell](resource-group-authenticate-service-principal.md)
- [Azure CLI](resource-group-authenticate-service-principal-cli.md)
- [Portal](resource-group-create-service-principal-portal.md)


Wenn Sie eine Anwendung, die Ressourcen zu zugreifen haben, müssen Sie eine Anwendung Active Directory (AD) einrichten und die erforderlichen Berechtigungen zuweisen. In diesem Thema wird veranschaulicht, wie diese Schritte über das Portal. Derzeit verwenden Sie das klassische Portal zum Erstellen einer neuen Active Directory-Anwendung, und wechseln Sie zum Azure-Portal die Anwendung eine Rolle zuweisen. 

> [AZURE.NOTE] Die Schritte in diesem Thema gelten nur, wenn **Verwaltungsportal** zum AD-Anwendung erstellen. **Verwenden Sie Azure-Portal für die Anwendung erstellen, werden diese Schritte nicht erfolgreich.** 
>
> Sie können Ihre Anwendung und Service principal über [PowerShell](resource-group-authenticate-service-principal.md) oder [Azure CLI](resource-group-authenticate-service-principal-cli.md)einrichten einfacher besonders, wenn Sie ein Zertifikat für die Authentifizierung verwenden möchten. Dieses Thema zeigt wie ein Zertifikat verwendet.

Eine Erläuterung der Konzepte von Active Directory finden Sie in der [Anwendung und Service Principal-Objekte](./active-directory/active-directory-application-objects.md). Weitere Informationen zu Active Directory-Authentifizierung finden Sie unter [Authentifizierungsszenarien Azure AD](./active-directory/active-directory-authentication-scenarios.md).

Detaillierte Schritte zur Integration von einer Anwendung in Azure zum Verwalten von Ressourcen finden Sie im [Entwicklerhandbuch Bewilligung mit der Azure-Ressourcen-Manager-API](resource-manager-api-authentication.md).

## <a name="create-an-active-directory-application"></a>Erstellen einer Active Directory-Anwendung

1. Melden Sie sich bei der Azure-Konto über das [Verwaltungsportal](https://manage.windowsazure.com/)an. 

2. Stellen Sie sicher wissen von Standard Active Directory für Ihr Abonnement. Sie können nur Programme im selben Verzeichnis wie Ihr Abonnement Zugriff gewähren. **Bearbeiten Sie** und suchen Sie den Verzeichnisnamen mit Ihrem Abonnement verknüpft.  Weitere Informationen finden Sie unter [wie Azure-Abonnements Azure Active Directory zugeordnet sind](./active-directory/active-directory-how-subscriptions-associated-directory.md).
   
     ![Standardverzeichnis suchen](./media/resource-group-create-service-principal-portal/show-default-directory.png)

2. Wählen Sie im linken Bereich **Active Directory** .

     ![Wählen Sie Active Directory](./media/resource-group-create-service-principal-portal/active-directory.png)
     
3. Wählen Sie Active Directory zum Erstellen der Anwendungdes verwenden möchten. Haben Sie mehrere Active Directory erstellen Sie die Anwendung im Standardverzeichnis für Ihr Abonnement.   

     ![Wählen Sie das Verzeichnis](./media/resource-group-create-service-principal-portal/active-directory-details.png)
     
3. Wählen Sie zum Anzeigen der Anwendung im Verzeichnis **Applications**.

     ![Programme anzeigen](./media/resource-group-create-service-principal-portal/view-applications.png)

4. Wenn Sie eine Anwendung in diesem Verzeichnis vor erstellt haben, sollte ungefähr der folgenden Abbildung angezeigt werden. Wählen Sie **eine Anwendung hinzufügen**

     ![Anwendung hinzufügen](./media/resource-group-create-service-principal-portal/create-application.png)

     Oder klicken Sie im unteren Bereich auf **Hinzufügen** .

     ![Hinzufügen](./media/resource-group-create-service-principal-portal/add-icon.png)

5. Wählen Sie die Anwendung erstellen möchten. Wählen Sie für dieses Lernprogramm **von meinem Unternehmen entwickelte Anwendung hinzufügen**. 

     ![neue Anwendung](./media/resource-group-create-service-principal-portal/what-do-you-want-to-do.png)

6. Geben Sie einen Namen für die Anwendung, und wählen Sie die Anwendung erstellen möchten. In diesem Lernprogramm erstellen Sie eine **Web-Anwendung oder WEB-API** und klicken Sie auf die Schaltfläche Weiter. Wenn Sie **SYSTEMEIGENE CLIENTANWENDUNG**auswählen, entspricht die verbleibenden Schritte dieses Artikels nicht Ihre Erfahrung.

     ![Name-Anwendung](./media/resource-group-create-service-principal-portal/tell-us-about-your-application.png)

7. Geben Sie die Eigenschaften für Ihre Anwendung. **Zeichen auf URL**bereitstellen Sie, eine Website, die die Anwendung beschreibt. Das Vorhandensein der Website ist nicht gültig. **APP-ID-URI**bereitstellen Sie, die die Anwendung identifiziert.

     ![Anwendungseigenschaften](./media/resource-group-create-service-principal-portal/app-properties.png)

Sie haben Ihre Anwendung erstellt.

## <a name="get-client-id-and-authentication-key"></a>Client-Id und Authentifizierung erhalten

Programmgesteuert anmelden wird die Id für Ihre Anwendung benötigen. Wenn die Anwendung einen eigenen Anmeldeinformationen ausgeführt wird, benötigen Sie außerdem einen Authentifizierungsschlüssel.

1. Wählen Sie die Registerkarte **Konfigurieren** der Anwendung Kennwort konfigurieren.

     ![Anwendung konfigurieren](./media/resource-group-create-service-principal-portal/application-configure.png)

2. Kopieren Sie die **CLIENT-ID**
  
     ![Client-id](./media/resource-group-create-service-principal-portal/client-id.png)

3. Wenn die Anwendung einen eigenen Anmeldeinformationen ausgeführt wird, **den Abschnitt** Scrollen Sie und wählen Sie aus, wie lange das Kennwort gültig ist.

     ![Schlüssel](./media/resource-group-create-service-principal-portal/create-key.png)

4. Wählen Sie Ihren Schlüssel zu **Speichern** .

     ![Speichern](./media/resource-group-create-service-principal-portal/save-icon.png)

     Gespeicherte Schlüssel angezeigt, und Sie können. Sie können nicht den Schlüssel später abrufen also jetzt kopieren.

     ![gespeicherte Schlüssel](./media/resource-group-create-service-principal-portal/save-key.png)

## <a name="get-tenant-id"></a>Mandanten-Id abrufen

Beim programmgesteuerten anmelden, müssen Sie die Mandanten-Id mit Ihrem Authentifizierungsanfrage übergeben. Web Apps und Web API-Apps können Sie die Mandanten-Id abrufen **Ansicht Endpunkte** am unteren Bildschirmrand auswählen und Abrufen von Id wie im folgenden Bild gezeigt.  

   ![Mandanten-id](./media/resource-group-create-service-principal-portal/save-tenant.png)

Sie können auch die Mandanten-Id über PowerShell abrufen:

    Get-AzureRmSubscription

Oder Azure CLI:

    azure account show --json

## <a name="set-delegated-permissions"></a>Delegierte Berechtigungen

Wenn die Anwendung Ressourcen für einen angemeldeten Benutzer zugreift, müssen Sie Ihre Anwendung Delegierte Berechtigung auf andere Programme gewähren. Sie gewähren diese im Abschnitt **Berechtigungen für weitere** Registerkarte **Konfigurieren** . Standardmäßig ist eine delegierte Berechtigung bereits Azure Active Directory aktiviert. Lassen Sie diese Delegierte Berechtigung unverändert.

1. Wählen Sie die **Anwendung hinzufügen**.

2. Wählen Sie aus der Liste die **Windows Azure Service Management-API**. Wählen Sie das vollständige Symbol an.

      ![Anwendung auswählen](./media/resource-group-create-service-principal-portal/select-app.png)

3. Wählen Sie in der Dropdown-Liste für delegierte Berechtigungen **Zugriff Azure Service-Management in Unternehmen**.

      ![SELECT-Berechtigung](./media/resource-group-create-service-principal-portal/select-permissions.png)

4. Speichern Sie die Änderung.

## <a name="assign-application-to-role"></a>Anwendung Rolle zuweisen

Wenn Ihre Anwendung einen eigenen Anmeldeinformationen ausgeführt wird, muss die Anwendung eine Rolle zuweisen. Entscheiden Sie, welche Rolle die entsprechenden Berechtigungen für die Anwendung darstellt. Informationen zu verfügbaren Rollen finden Sie unter [RBAC: integrierte Rollen](./active-directory/role-based-access-built-in-roles.md). 

Zum Zuweisen einer Rolle zu einer Anwendung müssen Sie die richtigen Berechtigungen. Insbesondere müssen Sie `Microsoft.Authorization/*/Write` Zugriff bis [Besitzerrolle](./active-directory/role-based-access-built-in-roles.md#owner) oder Benutzeradministratorrolle [Zugriff](./active-directory/role-based-access-built-in-roles.md#user-access-administrator) gewährt wird. Die Rolle eines Beitragenden kann nicht richtig zugreifen.

Der Bereich kann auf der Ebene des Abonnements, Ressourcengruppe oder Ressource festgelegt werden. Geringeren Umfang werden Berechtigungen vererbt. Beispielsweise bedeutet Anwendung Rolle für eine Ressourcengruppe Ressourcengruppe und alle darin enthaltenen Ressourcen gelesen werden können.

1. Um die Anwendung eine Rolle zuweisen, wechseln Sie aus dem Verwaltungsportal [Azure-portal](https://portal.azure.com)

1. Überprüfen Sie Ihre Berechtigungen, um sicherzustellen, dass die Dienstprinzipalnamen einer Rolle zuweisen können. Wählen Sie **Meine Berechtigungen** für Ihr Konto.

    ![Wählen Sie meine Berechtigungen](./media/resource-group-create-service-principal-portal/my-permissions.png)

1. Anzeigen der zugewiesenen Berechtigungen für Ihr Konto. Wie bereits erwähnt, muss der Besitzer oder Zugriff der Benutzeradministratorrolle angehören oder eine benutzerdefinierte Rolle, die Schreibzugriff für Microsoft.Authorization gewährt. Die folgende Abbildung zeigt ein Konto Teilnehmerrolle für das Abonnement ist nicht dazu berechtigt zugewiesen, eine Anwendung eine Rolle zuweisen.

    ![Meine Berechtigungen anzeigen](./media/resource-group-create-service-principal-portal/show-permissions.png)

     Wenn Sie nicht die erforderlichen Berechtigungen zum Zugriff auf eine Anwendung gewähren müssen Sie entweder Ihren Abonnementadministrator Zugriff Benutzeradministratorrolle fügt oder anfordern, erhält der Administrator Zugriff auf die Anwendung.

1. Navigieren Sie zu der Ebene des Gültigkeitsbereichs die Anwendung zuweisen möchten. Wählen Sie zum Zuweisen einer Rolle auf Abonnementebene **Abonnements**.

     ![Abonnement auswählen](./media/resource-group-create-service-principal-portal/select-subscription.png)

     Wählen Sie das bestimmte Abonnement die Anwendung zugewiesen.

     ![Abonnement für Zuweisung auswählen](./media/resource-group-create-service-principal-portal/select-one-subscription.png)

     Wählen Sie das **Access** -Symbol in der oberen rechten Ecke.

     ![Wählen Sie Zugriff](./media/resource-group-create-service-principal-portal/select-access.png)
     
     Oder navigieren Sie zum Zuweisen einer Rolle zur Ressourcenbereich Ressourcengruppe. Wählen Sie das Blatt Ressource **Zugriffskontrolle**.

     ![Benutzer auswählen](./media/resource-group-create-service-principal-portal/select-users.png)

     Folgende Schritte sind für jeden Bereich.

2. Wählen Sie **Hinzufügen**.

     ![Wählen Sie hinzufügen](./media/resource-group-create-service-principal-portal/select-add.png)

3. Wählen Sie die Rolle **Leser** (oder welche Rolle die Anwendung zugewiesen werden soll).

     ![Rolle auswählen](./media/resource-group-create-service-principal-portal/select-role.png)

4. Wenn zuerst die Liste der Benutzer, die Sie der Rolle hinzufügen können angezeigt, sehen Sie keine Applications. Sie sehen nur Gruppen und Benutzer.

     ![Benutzer anzeigen](./media/resource-group-create-service-principal-portal/show-users.png)

5. Um die Anwendung zu suchen, müssen Sie es suchen. Geben Sie den Namen der Anwendung und die Liste der verfügbaren Optionen ändern. Wählen Sie Ihre Anwendung, wenn Sie in der Liste angezeigt.

     ![Rolle zuweisen](./media/resource-group-create-service-principal-portal/assign-to-role.png)

6. Wählen Sie **OK** zum Beenden die Rolle zuweisen. Die Anwendung in der Liste der Zuweisung einer Rolle für die Ressourcengruppe verwendet sollte jetzt angezeigt werden.


Weitere Informationen zum Zuweisen von Benutzern und Rollen über das Portal Anwendung finden Sie unter [verwenden Arbeitsaufträge für Benutzerrollen Zugriff auf Ihre Ressourcen Azure-Abonnement verwalten](role-based-access-control-configure.md#manage-access-using-the-azure-management-portal).

## <a name="sample-applications"></a>Anwendungsbeispiele

Die folgende Beispiel-Applikationen anzeigen Anmelden als Service principal

**.NET**

- [Bereitstellen einer SSH aktiviert VM mit einer Vorlage mit .NET](https://azure.microsoft.com/documentation/samples/resource-manager-dotnet-template-deployment/)
- [Verwalten von Azure-Ressourcen und Ressourcengruppen mit .NET](https://azure.microsoft.com/documentation/samples/resource-manager-dotnet-resources-and-groups/)

**Java**

- [Erste Schritte mit Ressourcen - Bereitstellung mit Azure Ressourcenmanager Vorlage - in Java](https://azure.microsoft.com/documentation/samples/resources-java-deploy-using-arm-template/)
- [Erste Schritte mit Ressourcen - Ressourcengruppe verwaltet - in Java](https://azure.microsoft.com/documentation/samples/resources-java-manage-resource-group//)

**Python**

- [Bereitstellen einer SSH aktiviert VM mit einer Vorlage in Python](https://azure.microsoft.com/documentation/samples/resource-manager-python-template-deployment/)
- [Verwalten von Azure Ressourcen und Ressourcengruppen mit Python](https://azure.microsoft.com/documentation/samples/resource-manager-python-resources-and-groups/)

**Node.js**

- [Bereitstellen einer SSH aktiviert VM mit einer Vorlage in Node.js](https://azure.microsoft.com/documentation/samples/resource-manager-node-template-deployment/)
- [Azure-Ressourcen und Ressourcengruppen mit Node.js verwalten](https://azure.microsoft.com/documentation/samples/resource-manager-node-resources-and-groups/)

**Ruby**

- [Bereitstellen einer SSH aktiviert VM mit einer Vorlage in Ruby](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-template-deployment/)
- [Verwalten von Azure Ressourcen und Ressourcengruppen Ruby](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-resources-and-groups/)


## <a name="next-steps"></a>Nächste Schritte

- Zum Festlegen von Sicherheitsrichtlinien finden Sie unter [Azure Role-based Access Control](./active-directory/role-based-access-control-configure.md).  
- Eine Videodemonstration Schritte finden Sie unter [Programmgesteuerte Verwaltung einer Ressource Azure Azure Active Directory aktivieren](https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Enabling-Programmatic-Management-of-an-Azure-Resource-with-Azure-Active-Directory).

