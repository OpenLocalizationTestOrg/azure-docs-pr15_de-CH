<properties
   pageTitle="Grundlegendes zum Anwendungsmanifest Azure Active Directory | Microsoft Azure"
   description="Detaillierte des Anwendungsmanifests Azure Active Directory, die Identität Konfiguration einer Anwendung in Azure AD-Mandanten darstellt und OAuth Autorisierung Zustimmung Erfahrung und mehr erleichtert."
   services="active-directory"
   documentationCenter=""
   authors="bryanla"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/11/2016"
   ms.author="dkershaw;bryanla"/>

# <a name="understanding-the-azure-active-directory-application-manifest"></a>Grundlegendes zu Active Directory Azure Anwendungsmanifest

Applikationen mit Azure Active Directory (AD) integrieren müssen mit einer Azure AD-Mandanten somit eine permanente Identität Konfiguration für die Anwendung registriert. Diese Konfiguration wird zur Laufzeit Szenarien, mit denen eine Anwendung Auslagern und broker-Authentifizierung/Autorisierung über Azure AD gehört. Weitere Informationen zum Azure AD-Anwendung finden Sie unter [Hinzufügen, aktualisieren und Entfernen einer Anwendung] [ ADD-UPD-RMV-APP] Artikel.

## <a name="updating-an-applications-identity-configuration"></a>Aktualisieren einer Anwendung Identität Konfiguration

Es gibt tatsächlich mehrere Optionen zum Aktualisieren der Eigenschaften einer Anwendungskonfiguration Identität in Funktionen und Schwierigkeitsgrade, einschließlich der folgenden variieren:

- Die ** [des Azure Verwaltungsportal] [ AZURE-CLASSIC-PORTAL] Web-Benutzeroberfläche** können Sie die gängigsten Eigenschaften einer Anwendung aktualisieren. Schnellste und mindestens Fehler anfällig wie Aktualisieren der Anwendung Eigenschaften Dies ist Ihnen nicht vollständigen Zugriff auf alle Eigenschaften, wie die beiden Methoden.
- Für komplexere Szenarios sollten Eigenschaften aktualisieren, die im klassischen Azure-Portal nicht verfügbar sind, können Sie das **Anwendungsmanifest**ändern. Dies ist der Schwerpunkt dieses Artikels und wird im nächsten Abschnitt ausführlicher erläutert.
- Es ist auch möglich, **eine Anwendung, die die [Graph-API] verwendet[ GRAPH-API] ** , den Aufwand die Anwendung aktualisieren. Eine attraktive Option möglicherweise, wenn Managementsoftware schreiben oder Anwendungseigenschaften in regelmäßigen Abständen automatisch aktualisieren müssen.

## <a name="using-the-application-manifest-to-update-an-applications-identity-configuration"></a>Das Anwendungsmanifest verwenden Aktualisieren einer Anwendungskonfiguration Identität
[Azure-Verwaltungsportal][AZURE-CLASSIC-PORTAL]verwalten Ihre Anwendungskonfiguration Identität herunterladen und Hochladen einer JSON-Datei Darstellung ein Anwendungsmanifest aufgerufen wird. Keine tatsächlichen Datei wird im Verzeichnis gespeichert. Das Anwendungsmanifest ist nur eine HTTP GET-Operation Entität Azure AD Graph-API-Anwendung und der Upload ist ein HTTP-PATCH Vorgang Anwendung Entität.

Daher müssen Sie Verständnis der Formats und der Eigenschaften des Anwendungsmanifests Graph-API- [Anwendung Entität] verweisen[ APPLICATION-ENTITY] Dokumentation. Beispiele für Updates ausgeführt werden können, obwohl Anwendungsmanifest hochladen:

- **Declare berechtigungsbereiche (oauth2Permissions)** von Ihrem Web-API verfügbar gemacht. Finden Sie im Thema "Offenlegen von APIs auf andere ASP.NET-Webanwendungen" [Applikationen Azure Active Directory Integration] [ INTEGRATING-APPLICATIONS-AAD] Informationen zum Implementieren von Benutzeridentitätswechsel mithilfe der oauth2Permissions Berechtigungsbereich delegiert. Wie bereits erwähnt, sind Anwendung Entitätseigenschaften Graph API [Entität und komplexen Typ] dokumentiert[ APPLICATION-ENTITY] Referenzartikel, einschließlich der oauth2Permissions-Eigenschaft ist eine Auflistung vom Typ [OAuth2Permission][APPLICATION-ENTITY-OAUTH2-PERMISSION].
- **Declare Anwendungsrollen (AppRoles) von der Anwendung verfügbar gemacht**. Die Anwendung Entität AppRoles-Eigenschaft ist eine Auflistung des Typs [AppRole][APPLICATION-ENTITY-APP-ROLE]. [Rollenbasierte Zugriffskontrolle in Azure AD mit Cloudanwendungen] finden Sie[ RBAC-CLOUD-APPS-AZUREAD] Artikel beispielsweise Implementierung.
- **Declare bezeichnet Client Applications (KnownClientApplications)**, die Zustimmung der angegebenen Client Anwendung(en) Ressource/Web API logisch binden können.
- **Azure AD Gruppe erteilen Mitgliedschaften Anspruch anfordern** für den angemeldeten Benutzer (GroupMembershipClaims).  Dies kann auch konfiguriert werden die Mitgliedschaften des Benutzers Verzeichnis Rollen Ansprüche ausstellen. [Autorisierung in Cloudanwendungen mithilfe von AD-Gruppen] finden Sie unter[ AAD-GROUPS-FOR-AUTHORIZATION] Artikel beispielsweise Implementierung.
- **Zulassen der Anwendung OAuth 2.0 implizite Unterstützung gewähren** fließen (oauth2AllowImplicitFlow). Dieser Zuschuss Fluss ist mit eingebetteten JavaScript-Webseiten oder einzelne Seite Applications (SPA) verwendet. Weitere Informationen auf das implizite Autorisierung finden Sie unter [Grundlegendes zur impliziten OAuth2 Fluss in Azure Active Directory erteilen][IMPLICIT-GRANT].
- **Zertifikate ermöglichen Verwendung von X509 als geheimer Schlüssel** (KeyCredentials). Finden Sie unter [Service und Daemon apps in Office 365 erstellen] [ O365-SERVICE-DAEMON-APPS] und [Entwicklerhandbuch AUTH Azure Ressourcenmanager API] [ DEV-GUIDE-TO-AUTH-WITH-ARM] Artikel Beispiele für die Implementierung.
- **Hinzufügen einer neuen Anwendung ID URI** für Ihre Anwendung (identifierURIs[]). App-ID URIs dienen zur eindeutigen Identifizierung eine Anwendung in Azure AD-Mandanten (oder mehrere Azure AD-Mandanten für Multi-Tenant Szenarien über verifizierte benutzerdefinierte Domäne qualifiziert). Sie wird beim Anfordern von Berechtigungen für eine ressourcenanwendung oder ein Zugriffstoken für eine ressourcenanwendung erwerben. Beim Aktualisieren dieses erfolgt das Update entsprechende Dienstprinzipalnamen ServicePrincipalNames [] Auflistung lebt in home Mandanten der Anwendung.

Das Anwendungsmanifest beinhaltet auch eine gute Möglichkeit, den Zustand Ihrer Anwendung Registrierung nachzuverfolgen. Steht im JSON-Format kann die Datei Darstellung in der Datenquellen-Steuerelement mit Quellcode der Anwendung überprüft werden.

## <a name="step-by-step-example"></a>Schritt für Schritt-Beispiel
Jetzt können die Schritte erforderlich, um die Konfiguration der Anwendung Identität durch das Anwendungsmanifest aktualisieren. Eines der vorhergehenden Beispiele wird hervorzuheben veranschaulicht einen neuen Berechtigungsbereich auf eine ressourcenanwendung:

1. Navigieren Sie zu der [Azure-Verwaltungsportal] [ AZURE-CLASSIC-PORTAL] und sich mit einem Konto mit Dienstberechtigungen oder Co-Administrator.

2. Nachdem authentifiziert haben, blättern Sie nach unten und wählen Sie die Erweiterung Azure "Active Directory" im linken Navigationsfenster (1) dann auf registriert die Azure AD-Mandanten die Anwendung ist (2).

    ![Azure AD-Mandanten auswählen][SELECT-AZURE-AD-TENANT]

3. Bei die Directory-Seite klicken Sie auf "Applications" (1) oben auf der Seite Anwendung registriert die Mandanten aufgelistet. Finden Sie die Anwendung in der Liste aktualisieren, und klicken auf (2).

    ![Azure AD-Mandanten auswählen][SELECT-AZURE-AD-APP]

4. Nun, dass Sie die Hauptseite der Anwendung ausgewählt haben, beachten Sie die Funktion "Manifest verwalten" am unteren Rand der Seite (1). Wenn Sie auf diesen Link klicken, werden Sie aufgefordert, herunterladen oder Hochladen der JSON-Manifestdatei. Klicken Sie auf "Download Manifest" (2). Unmittelbar danach wird werden mit Bestätigungsdialogfeld Download aufgefordert zu bestätigen, indem Sie auf "Download Manifest" (3), und öffnen oder speichern Sie die Datei lokal (4).

    ![Verwalten des Manifests, download-option][MANAGE-MANIFEST-DOWNLOAD]

    ![Downloaden Sie das manifest][DOWNLOAD-MANIFEST]

5. In diesem Beispiel speichern wir die Datei lokal, ermöglicht in einem Editor öffnen, ändern Sie die JSON und erneut speichern. Hier sieht die JSON-Struktur im Visual Studio-JSON-Editor. Beachten Sie, dass das Schema für die [Anwendung Entität] folgt[ APPLICATION-ENTITY] wie bereits erwähnt:

    ![Aktualisieren des Bereitstellungsmanifests JSON][UPDATE-MANIFEST]

    Beispielsweise würde angenommen, wir implementieren/eine neue Berechtigung "Employees.Read.All" in unserer Anwendung Resource (API) machen möchten, einfach ein neues pro Sekunde Element der Auflistung oauth2Permissions ie hinzufügen:

        "oauth2Permissions": [
        {
        "adminConsentDescription": "Allow the application to access MyWebApplication on behalf of the signed-in user.",
        "adminConsentDisplayName": "Access MyWebApplication",
        "id": "aade5b35-ea3e-481c-b38d-cba4c78682a0",
        "isEnabled": true,
        "type": "User",
        "userConsentDescription": "Allow the application to access MyWebApplication on your behalf.",
        "userConsentDisplayName": "Access MyWebApplication",
        "value": "user_impersonation"
        },
        {
        "adminConsentDescription": "Allow the application to have read-only access to all Employee data.",
        "adminConsentDisplayName": "Read-only access to Employee records",
        "id": "2b351394-d7a7-4a84-841e-08a6a17e4cb8",
        "isEnabled": true,
        "type": "User",
        "userConsentDescription": "Allow the application to have read-only access to your Employee data.",
        "userConsentDisplayName": "Read-only access to your Employee records",
        "value": "Employees.Read.All"
        }
        ],

    Der Eintrag muss eindeutig sein und müssen daher eine neue global eindeutigen ID (GUID) für generieren die `"id"` Eigenschaft. In diesem Fall da angegeben `"type": "User"`, mit dieser Berechtigung kann von jedem Konto authentifiziert von Azure AD Mieter in der Ressourcen-API-Anwendung registriert wird zugestimmt. Dadurch erhält den Client auf auf das Konto Namen zugreifen. Namenszeichenfolgen Beschreibung und Darstellung werden bei Zustimmung und für die Anzeige im klassischen Azure-Portal verwendet.

6. Sie aktualisieren das Manifest abschließend Azure AD Anwendungsseite im klassischen Azure-Portal zurück, und auf die "Manifest verwalten" Funktion erneut (1). Aber dieses Mal die Option "Hochladen" Manifest "(2). Ähnlich den Download erneut in einem zweiten Dialogfeld begrüßen Sie für den Speicherort der JSON-Datei. Klicken Sie auf "Datei suchen" (3), dann im Dialogfeld "Wählen Sie Datei hinzufügen" können Sie die JSON (4), und drücken Sie "Öffnen". Sobald das Dialogfeld verschwindet, aktivieren Sie das Kontrollkästchen "OK" (5) und Manifest hochgeladen.  

    ![Verwalten des Manifests, Option hochladen][MANAGE-MANIFEST-UPLOAD]

    ![Manifest JSON hochladen][UPLOAD-MANIFEST]

    ![Hochladen des Manifesten JSON - Bestätigung][UPLOAD-MANIFEST-CONFIRM]

Da das Manifest gespeichert wird, können eine registrierte Anwendung Clientzugriff auf die neue Berechtigung über hinzugefügt. Dieses Mal die Azure Verwaltungsportal Webbenutzeroberfläche können Sie statt der Clientanwendung Manifest:  

1. Zuerst zur Seite "Konfigurieren" der Clientanwendung, die neue API Zugriff hinzufügen möchten, und klicken Sie auf die Schaltfläche "Anwendung hinzufügen".
2. Dann wird die Liste der registrierten Ressource Applications (APIs) der Mandanten angezeigt werden. Klicken Sie auf das Plus / + Symbol neben der ressourcenanwendung um es auszuwählen.  
3. Klicken Sie auf das Häkchen unten rechts.
4. Wenn Sie zum Abschnitt "Anwendung hinzufügen" Seite Konfiguration des Clients zurückkehren, sehen Sie neue ressourcenanwendung in der Liste. Wenn Sie über den Abschnitt "Berechtigungen delegiert" rechts neben der Zeile bewegen, sehen Sie eine Dropdown-Liste angezeigt. Klicken Sie auf die Liste und wählen Sie neue Kunden angeforderten Liste der Berechtigungen hinzufügen. Hinweis: Diese neue Berechtigung wird in der Clientanwendung Identität Konfiguration in die Collection-Eigenschaft "RequiredResourceAccess" gespeichert.

![Berechtigungen für andere Programme][PERMS-TO-OTHER-APPS]

![Berechtigungen für andere Programme][PERMS-SELECT-APP]

![Berechtigungen für andere Programme][PERMS-SELECT-PERMS]

Das wars. Jetzt werden die Anwendung mit ihrer neuen Identität Konfiguration ausgeführt.

## <a name="next-steps"></a>Nächste Schritte
- Weitere Informationen zur Beziehung zwischen Anwendung und Service Principal-Objekte einer Anwendung finden Sie unter [Anwendung und principal-Objekte in Azure AD][AAD-APP-OBJECTS].
- Siehe [Azure AD Entwickler Glossar] [ AAD-DEVELOPER-GLOSSARY] Definitionen einiger Azure Active Directory (AD) Developer Kernkonzepte.

Verwenden Sie DISQUS Kommentare Abschnitt zu Feedback zu optimieren und unseren Inhalt.

<!--Image references-->
[DOWNLOAD-MANIFEST]: ./media/active-directory-application-manifest/download-manifest.png
[MANAGE-MANIFEST-DOWNLOAD]: ./media/active-directory-application-manifest/manage-manifest-download.png
[MANAGE-MANIFEST-UPLOAD]: ./media/active-directory-application-manifest/manage-manifest-upload.png
[PERMS-SELECT-APP]: ./media/active-directory-application-manifest/portal-perms-select-app.png
[PERMS-SELECT-PERMS]: ./media/active-directory-application-manifest/portal-perms-select-perms.png
[PERMS-TO-OTHER-APPS]: ./media/active-directory-application-manifest/portal-perms-to-other-apps.png
[SELECT-AZURE-AD-APP]: ./media/active-directory-application-manifest/select-azure-ad-application.png
[SELECT-AZURE-AD-TENANT]: ./media/active-directory-application-manifest/select-azure-ad-tenant.png
[UPDATE-MANIFEST]: ./media/active-directory-application-manifest/update-manifest.png
[UPLOAD-MANIFEST]: ./media/active-directory-application-manifest/upload-manifest.png
[UPLOAD-MANIFEST-CONFIRM]: ./media/active-directory-application-manifest/upload-manifest-confirm.png

<!--article references -->
[AAD-APP-OBJECTS]: active-directory-application-objects.md
[AAD-DEVELOPER-GLOSSARY]: active-directory-dev-glossary.md
[AAD-GROUPS-FOR-AUTHORIZATION]: http://www.dushyantgill.com/blog/2014/12/10/authorization-cloud-applications-using-ad-groups/
[ADD-UPD-RMV-APP]: active-directory-integrating-applications.md
[APPLICATION-ENTITY]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[APPLICATION-ENTITY-APP-ROLE]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#approle-type
[APPLICATION-ENTITY-OAUTH2-PERMISSION]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permission-type
[AZURE-CLASSIC-PORTAL]: https://manage.windowsazure.com
[DEV-GUIDE-TO-AUTH-WITH-ARM]: http://www.dushyantgill.com/blog/2015/05/23/developers-guide-to-auth-with-azure-resource-manager-api/
[GRAPH-API]: active-directory-graph-api.md
[IMPLICIT-GRANT]: active-directory-dev-understanding-oauth2-implicit-grant.md
[INTEGRATING-APPLICATIONS-AAD]: https://azure.microsoft.com/documentation/articles/active-directory-integrating-applications/
[O365-PERM-DETAILS]: https://msdn.microsoft.com/office/office365/HowTo/application-manifest
[O365-SERVICE-DAEMON-APPS]: https://msdn.microsoft.com/office/office365/howto/building-service-apps-in-office-365
[RBAC-CLOUD-APPS-AZUREAD]: http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/

