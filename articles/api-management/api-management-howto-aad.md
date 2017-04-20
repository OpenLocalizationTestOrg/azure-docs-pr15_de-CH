<properties 
    pageTitle="Entwicklerkonten mithilfe von Azure Active Directory in Azure API Management Autorisierung vor" 
    description="Erfahren Sie, wie Benutzer mit Active Directory Azure API Management autorisiert." 
    services="api-management" 
    documentationCenter="API Management" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="how-to-authorize-developer-accounts-using-azure-active-directory-in-azure-api-management"></a>Entwicklerkonten mithilfe von Azure Active Directory in Azure API Management Autorisierung vor


## <a name="overview"></a>Übersicht
Dieses Handbuch veranschaulicht die Zugang zum Entwicklerportal für alle Benutzer in einer oder mehreren Azure Active Directory. Dieses Handbuch zeigt auch Verwalten von Azure Active Directory Gruppen durch Hinzufügen von externen Gruppen, die Benutzer von Azure Active Directory enthalten.

>Die Schritte in diesem Handbuch müssen Sie zuerst Azure Active Directory, eine Anwendung zu erstellen.

## <a name="how-to-authorize-developer-accounts-using-azure-active-directory"></a>Wie entwicklerkonten Azure Active Directory autorisieren

Klicken Sie zunächst auf **Verwalten** in Azure-Verwaltungsportal für Ihre API-Verwaltungsdienst. Dadurch gelangen Sie zu Publisher API Verwaltungsportal.

![Publisher-portal][api-management-management-console]

>Wenn Sie eine Dienstinstanz API Management noch nicht erstellt haben, finden Sie im Lernprogramm [Erste Schritte mit Azure API Management][] [Erstellen API Management Service][] .

**Sicherheit** der **API-** Menü links klicken Sie und **Externe Identitäten**.

![Externe Identitäten][api-management-security-external-identities]

Klicken Sie auf **Azure Active Directory**. Notieren der **URL umleiten** und Switch über zu Azure Active Directory in der Azure-Verwaltungsportal.

![Externe Identitäten][api-management-security-aad-new]

Klicken Sie auf **Hinzufügen** , um eine neue Active Directory Azure-Anwendung erstellen und auswählen **von meinem Unternehmen entwickelte Anwendung hinzufügen**.

![Fügen Sie neue Active Directory Azure-Anwendung][api-management-new-aad-application-menu]

Geben Sie einen Namen für die Anwendung und klicken Sie auf die Schaltfläche Wählen Sie **Web-Anwendung oder Web-API**.

![Neue Active Directory Azure-Anwendung][api-management-new-aad-application-1]

**Anmelden URL**Geben Sie URL auf Ihre Entwicklerportal. In diesem Beispiel die **Anmelden URL** ist `https://aad03.portal.current.int-azure-api.net/signin`. 

**App ID URL**Geben Sie die Standarddomäne oder eine benutzerdefinierte Domäne für Azure Active Directory und fügen Sie eine eindeutige Zeichenfolge hinzu. In diesem Beispiel wird die Standarddomäne für **https://contoso5api.onmicrosoft.com** mit dem Suffix **/api/API** angegebene verwendet.

![Neue Azure Active Directory Anwendungseigenschaften][api-management-new-aad-application-2]

Klicken Sie auf prüfen, um neue Anwendung erstellen, speichern und wechseln Sie zur Registerkarte **Konfigurieren** die neue Anwendung konfigurieren.

![Neue Active Directory Azure-Anwendung erstellt][api-management-new-aad-app-created]

Wenn mehrere Azure Active Directory eingesetzt werden sollen, klicken Sie auf **Ja** für **Multi-Tenant ist**. Die Standardeinstellung ist **Nein**.

![Multi-Tenant ist][api-management-aad-app-multi-tenant]

Kopieren Sie den **URL umleiten** von **Azure Active Directory** Teil der Registerkarte **Externe Identitäten** im Publisher-Portal und **Antwort-URL** -Feld einfügen. 

![Antwort-URL][api-management-aad-reply-url]

Führen Sie einen Bildlauf zum unteren Rand der Registerkarte konfigurieren, Dropdownliste **Berechtigungen** wählen, und aktivieren Sie **Verzeichnisdaten lesen**.

![Berechtigungen][api-management-aad-app-permissions]

Wählen Sie die **Berechtigungen der Stellvertretung** Dropdownliste und überprüfen Sie **Anmelden aktivieren und Benutzerprofile lesen**.

![Delegieren von Berechtigungen][api-management-aad-delegated-permissions]

>Weitere Informationen zur Anwendung und Delegieren von Berechtigungen finden Sie in der [Graph-API zugreifen][].

Kopieren Sie die **Client-Id** in die Zwischenablage.

![Client-Id][api-management-aad-app-client-id]

Wechseln Sie zu Publisher-Portal, und fügen Sie die **Client-Id** aus der Anwendungskonfiguration Azure Active Directory kopiert.

![Client-Id][api-management-client-id]

Wechseln Sie zur Azure Active Directory-Konfiguration auf **Dauer auswählen** Dropdown-Liste im Abschnitt **Schlüssel** und geben Sie ein Intervall. In diesem Beispiel wird **1 Jahr** verwendet.

![Schlüssel][api-management-aad-key-before-save]

Klicken Sie auf **Speichern** , um die Konfiguration zu speichern und Anzeigen des Schlüssels. Kopieren Sie den Schlüssel in die Zwischenablage.

>Notieren Sie sich diesen Schlüssel. Sobald Sie Azure Active Directory Konfigurationsfenster schließen, kann der Schlüssel wieder angezeigt werden.

![Schlüssel][api-management-aad-key-after-save]

Wechseln Sie zu Publisher-Portal und fügen Sie den Schlüssel in das Textfeld **Geheimen** .

![Clientschlüssel][api-management-client-secret]

**Zulässige Mieter** gibt an, welche Verzeichnisse die Dienstinstanz API Management-APIs zugreifen. Geben Sie die Domänen Instanzen Azure Active Directory Sie Zugriff gewähren möchten. Sie können mehrere Domänen durch Zeilenumbrüche, Leerzeichen oder Kommas trennen.

![Zulässige Mieter][api-management-client-allowed-tenants]

Mehrere Domänen können in Abschnitt **Mieter zulässig** angegeben. Vor jeder Benutzer aus einer anderen Domäne als die ursprüngliche Domäne anmelden kann, wo die Anwendung registriert wurde, muss ein globaler Administrator der anderen Domäne der Anwendung Zugriff auf Verzeichnisdaten Berechtigung. Berechtigung erteilen muss ein globaler Administrator bei der Anwendung anmelden und klicken Sie auf **annehmen**. Im folgenden Beispiel `miaoaad.onmicrosoft.com` hat **Mieter zulässig** und ein globaler Administrator dieser Domäne zum ersten Mal anmelden.

![Berechtigungen][api-management-permissions-form]

>Wenn nicht globalen Administrator versucht, sich anzumelden, bevor von einem globalen Administrator Berechtigungen, der Anmeldeversuch fehlschlägt und eine Fehlermeldung angezeigt.

Wenn die gewünschte Konfiguration angegeben ist, klicken Sie auf **Speichern**.

![Speichern][api-management-client-allowed-tenants-save]

Sobald die Änderungen gespeichert sind, können Benutzer in der angegebenen Azure Active Directory Entwicklerportal anmelden mithilfe der Schritte im [Entwicklerportal mit Azure Active Directory-Konto anmelden][].

## <a name="how-to-add-an-external-azure-active-directory-group"></a>Eine externe Azure Active Directory-Gruppe hinzufügen

Fügen Sie nach dem Aktivieren des Zugriffs für Benutzer in Active Directory Azure, Azure Active Directory-Gruppen in API-Zuordnung der Entwickler in der Gruppe mit dem gewünschten Produkte leichter verwalten.

> Um externe Azure Active Directory-Gruppen konfigurieren, muss Azure Active Directory zunächst auf der Registerkarte Identitäten konfiguriert werden durch die Schritte im vorherigen Abschnitt. 

Externe Azure Active Directory-Gruppen werden aus der Registerkarte **Sichtbarkeit** des Produkts hinzugefügt der Gruppe Zugriff gewährt werden soll. Klicken Sie auf **Produkte**und klicken Sie dann auf den Namen des gewünschten Produkts.

![Produkt konfigurieren][api-management-configure-product]

Wechseln Sie zur Registerkarte **Sichtbarkeit** , und klicken Sie auf **Gruppen von Azure Active Directory hinzufügen**.

![Gruppen hinzufügen][api-management-add-groups]

**Azure Active Directory Mieter** aus der Dropdown-Liste Wählen Sie aus, und geben Sie den Namen der gewünschten Gruppe **Gruppen** Textfeld hinzugefügt werden.

![Gruppe auswählen][api-management-select-group]

Dieser Gruppenname werden in **der Gruppenliste** für Ihre Azure Active Directory finden wie im folgenden Beispiel gezeigt.

![Liste der Azure Active Directory-Gruppen][api-management-aad-groups-list]

Klicken Sie auf **Hinzufügen** , um den Gruppennamen überprüfen und fügen Sie die Gruppe. In diesem Beispiel **Contoso 5 Entwickler** wird externen Gruppe hinzugefügt. 

![Hinzugefügte Gruppe][api-management-aad-group-added]

Klicken Sie auf **Speichern** , um die neue Auswahl speichern.

Wenn Azure Azure Active Directory-Gruppen von einem Produkt konfiguriert wurde, wird auf der Registerkarte **Sichtbarkeit** für die Produkte in die Dienstinstanz API Management aktiviert werden.

Um zu überprüfen, und konfigurieren Sie die Eigenschaften für externe Gruppen, nachdem sie hinzugefügt wurden, klicken Sie auf den Namen der Gruppe auf der Registerkarte **Gruppen** .

![Verwalten von Gruppen][api-management-groups]

Hier können Sie den **Namen** und die **Beschreibung** der Gruppe bearbeiten.

![Gruppe bearbeiten][api-management-edit-group]

Benutzer konfigurierten Azure Active Directory können melden Entwicklerportal und anzeigen und abonnieren Sie Gruppen für die Sichtbarkeit haben wie im folgenden Abschnitt.

## <a name="how-to-log-in-to-the-developer-portal-using-an-azure-active-directory-account"></a>Entwicklerportal mit Azure Active Directory-Konto anmelden

Anmeldung mit einem Azure Active Directory-Konto in den vorherigen Abschnitten konfiguriert Entwicklerportal öffnen Sie ein neues Browserfenster mit der **Anmeldung URL** aus der Active Directory-Anwendungskonfiguration und **Azure Active Directory**auf.

![Entwicklerportal][api-management-dev-portal-signin]

Geben Sie die Anmeldeinformationen eines Benutzer in Azure Active Directory, und klicken Sie auf **Anmelden**.

![Anmelden][api-management-aad-signin]

Sie können ein Registrierungsformular aufgefordert, wenn weitere Informationen erforderlich ist. Füllen Sie das Registrierungsformular aus, und klicken Sie auf **Anmelden**.

![Registrierung][api-management-complete-registration]

Der Benutzer ist jetzt für die Dienstinstanz API Management-Entwicklerportal angemeldet.

![Die Registrierung ist abgeschlossen][api-management-registration-complete]



[api-management-management-console]: ./media/api-management-howto-aad/api-management-management-console.png
[api-management-security-external-identities]: ./media/api-management-howto-aad/api-management-security-external-identities.png
[api-management-security-aad-new]: ./media/api-management-howto-aad/api-management-security-aad-new.png
[api-management-new-aad-application-menu]: ./media/api-management-howto-aad/api-management-new-aad-application-menu.png
[api-management-new-aad-application-1]: ./media/api-management-howto-aad/api-management-new-aad-application-1.png
[api-management-new-aad-application-2]: ./media/api-management-howto-aad/api-management-new-aad-application-2.png
[api-management-new-aad-app-created]: ./media/api-management-howto-aad/api-management-new-aad-app-created.png
[api-management-aad-app-permissions]: ./media/api-management-howto-aad/api-management-aad-app-permissions.png
[api-management-aad-app-client-id]: ./media/api-management-howto-aad/api-management-aad-app-client-id.png
[api-management-client-id]: ./media/api-management-howto-aad/api-management-client-id.png
[api-management-aad-key-before-save]: ./media/api-management-howto-aad/api-management-aad-key-before-save.png
[api-management-aad-key-after-save]: ./media/api-management-howto-aad/api-management-aad-key-after-save.png
[api-management-client-secret]: ./media/api-management-howto-aad/api-management-client-secret.png
[api-management-client-allowed-tenants]: ./media/api-management-howto-aad/api-management-client-allowed-tenants.png
[api-management-client-allowed-tenants-save]: ./media/api-management-howto-aad/api-management-client-allowed-tenants-save.png
[api-management-aad-delegated-permissions]: ./media/api-management-howto-aad/api-management-aad-delegated-permissions.png
[api-management-dev-portal-signin]: ./media/api-management-howto-aad/api-management-dev-portal-signin.png
[api-management-aad-signin]: ./media/api-management-howto-aad/api-management-aad-signin.png
[api-management-complete-registration]: ./media/api-management-howto-aad/api-management-complete-registration.png
[api-management-registration-complete]: ./media/api-management-howto-aad/api-management-registration-complete.png
[api-management-aad-app-multi-tenant]: ./media/api-management-howto-aad/api-management-aad-app-multi-tenant.png
[api-management-aad-reply-url]: ./media/api-management-howto-aad/api-management-aad-reply-url.png
[api-management-permissions-form]: ./media/api-management-howto-aad/api-management-permissions-form.png
[api-management-configure-product]: ./media/api-management-howto-aad/api-management-configure-product.png
[api-management-add-groups]: ./media/api-management-howto-aad/api-management-add-groups.png
[api-management-select-group]: ./media/api-management-howto-aad/api-management-select-group.png
[api-management-aad-groups-list]: ./media/api-management-howto-aad/api-management-aad-groups-list.png
[api-management-aad-group-added]: ./media/api-management-howto-aad/api-management-aad-group-added.png
[api-management-groups]: ./media/api-management-howto-aad/api-management-groups.png
[api-management-edit-group]: ./media/api-management-howto-aad/api-management-edit-group.png

[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Erste Schritte mit Azure API Management]: api-management-get-started.md
[API Management policy reference]: api-management-policy-reference.md
[Caching policies]: api-management-policy-reference.md#caching-policies
[Erstellen Sie eine Instanz der API Management service]: api-management-get-started.md#create-service-instance

[http://oauth.net/2/]: http://oauth.net/2/
[WebApp-GraphAPI-DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet
[Graph-API zugreifen]: http://msdn.microsoft.com/library/azure/dn132599.aspx#BKMK_Graph

[Prerequisites]: #prerequisites
[Configure an OAuth 2.0 authorization server in API Management]: #step1
[Configure an API to use OAuth 2.0 user authorization]: #step2
[Test the OAuth 2.0 user authorization in the Developer Portal]: #step3
[Next steps]: #next-steps

[Entwicklerportal mit Azure Active Directory-Konto anmelden]: #Log-in-to-the-Developer-portal-using-an-Azure-Active-Directory-account

