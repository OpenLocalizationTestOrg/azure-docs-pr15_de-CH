<properties
   pageTitle="Integration von Applikationen in Azure Active Directory | Microsoft Azure"
   description="Informationen zum Hinzufügen, aktualisieren oder entfernen eine Anwendung in Azure Active Directory (Azure AD)."
   services="active-directory"
   documentationCenter=""
   authors="bryanla"
   manager="mbaldwin"
   editor="mbaldwin" />
<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/29/2016"
   ms.author="mbaldwin;bryanla" />

# <a name="integrating-applications-with-azure-active-directory"></a>Active Directory Azure integrieren applications

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Entwickler und Anbieter von Software als Service (SaaS) entwickeln kommerzielle Cloud-Services oder Anwendungen, die in Azure Active Directory (Azure AD) sicheren Anmeldung und Autorisierung für ihre Dienste integriert werden können. Integration einer Anwendung oder einem Dienst in Azure AD muss Entwickler Details über ihre Anwendung in Azure AD über klassischen Azure-Portal registrieren.

In diesem Artikel wird das Hinzufügen, aktualisieren oder Entfernen einer Anwendung in Azure AD veranschaulicht. Lernen Sie die verschiedenen Anwendungstypen in Azure AD integriert werden können wie der Zugriff auf andere Ressourcen wie Web-APIs konfigurieren.

Erfahren Sie mehr über die zwei Azure AD-Objekte, die eine registrierte Anwendung und Beziehung Siehe [Anwendung und Service Principal-Objekte](active-directory-application-objects.md); Weitere Informationen über die Richtlinien, bei der Entwicklung mit Azure Active Directory verwenden soll, finden Sie unter [Brandingrichtlinien für integrierte Apps](active-directory-branding-guidelines.md).

## <a name="adding-an-application"></a>Hinzufügen einer Anwendung

Jede Anwendung, die Funktionen von Azure AD muss zuerst in Azure AD-Mandanten registriert werden. Diese Registrierung umfasst Azure AD Angaben über die Anwendung wie URL, wo es liegt, URL Antworten senden, nachdem ein Benutzer authentifiziert wurde, der URI, der app usw. bezeichnet.

Wenn Sie eine Anwendung, die nur für Benutzer in Azure AD erstellen-in unterstützt, können Sie einfach die folgenden Anweisungen. Wenn Ihre Anwendung Anmeldeinformationen oder Berechtigungen für den Zugriff auf ein Web API, oder Benutzern aus anderen Mandanten Azure AD [Aktualisieren einer Anwendung](#updating-an-application) Siehe zum Konfigurieren der Anwendung Zugriff auf.

### <a name="to-register-a-new-application-in-the-azure-classic-portal"></a>Eine neue Anwendung im klassischen Azure-Portal registrieren

1. Melden Sie sich bei [Azure-Verwaltungsportal](https://manage.windowsazure.com)an.

1. Klicken Sie im linken Menü auf Active Directory, und klicken Sie dann auf das gewünschte Verzeichnis.

1. Klicken Sie im oberen Menü auf **Programme**. Wenn keine apps zu Ihrem Verzeichnis hinzugefügt haben, zeigt diese Seite nur hinzufügen App Link. Klicken Sie auf den Link oder Sie können auf der Befehlsleiste auf **Hinzufügen** klicken.

1. Möchten Sie auf der Seite, klicken auf den Link **Mein Unternehmen entwickelten Anwendung hinzufügen**.

1. Auf die von Ihrer Anwendungsseite Geben Sie einen Namen für die Anwendung sowie Anwendung anzugeben, die Sie mit Azure registrieren.  Sie können entweder einen [Webanwendungsclient](active-directory-dev-glossary.md#client-application) / [Web Resource/API](active-directory-dev-glossary.md#resource-server) -Anwendung (auch funktionieren als) oder eine [systemeigene](active-directory-dev-glossary.md#native-client) Anwendung. Nachdem, klicken Sie auf den Pfeil in der unteren rechten Ecke der Seite.

1. Bereitzustellen Sie auf der Eigenschaftenseite Anwendung anmelden URL und App-ID URI, wenn eine Anwendung oder nur die Umleitung URI für eine systemeigene Anwendung registriert sind, dann aktivieren Sie das Kontrollkästchen in der unteren rechten Ecke der Seite.

1. Die Anwendung hinzugefügt wurde, und Sie gelangen zur Seite Schnellstart für Ihre Anwendung. Je nachdem, ob die Anwendung eine Web- oder eine systemeigene Anwendung sehen Sie verschiedene Optionen zum Hinzufügen weiterer Funktionen der Anwendung. Nach die Anwendung hinzugefügt wurde, können Sie beginnen, aktualisiert die Anwendung die Benutzer anmelden, Zugriff auf Web-APIs in einer anderen Anwendung oder Multi-Tenant-Anwendung (wodurch andere Unternehmen auf die Anwendung zugreifen kann) konfigurieren.

>[AZURE.NOTE] Standardmäßig ist die Registrierung neu erstellte Anwendung konfiguriert Benutzer aus dem Verzeichnis der Anwendung anmelden.

## <a name="updating-an-application"></a>Aktualisieren einer Anwendung

Nach der Registrierung Ihrer Anwendung in Azure AD müssen sie aktualisiert werden, Zugriff auf Web-APIs und anderen Organisationen verfügbar gemacht werden. Dieser Abschnitt beschreibt verschiedene Arten Sie möglicherweise Ihre Anwendung konfigurieren müssen. Zunächst starten wir eine Übersicht der Zustimmung ist wichtig zu verstehen, wenn Sie Ressourcen-API-Anwendung erstellen, die von Clientanwendungen, die Entwickler in Ihrem Unternehmen oder anderen Organisationen verwendet werden.

Weitere Informationen über die Authentifizierung in Azure AD, finden Sie unter [Authentifizierungsszenarien Azure AD](active-directory-authentication-scenarios.md).

### <a name="overview-of-the-consent-framework"></a>Übersicht über die Zustimmung

Azure AD Zustimmung Framework erleichtert die Entwicklung Multi-Tenant-Web und systemeigenen Clientanwendungen, die auf Web-APIs abgesichert durch einen anderen die Clientanwendung Sitz Azure AD-Mandanten. Diese Web-APIs enthalten Graph-API, Office 365 und anderen Microsoft-Diensten neben Ihren eigenen Web-APIs. Das Framework basiert auf einem Benutzer oder Administrator Einwilligung für eine Anwendung mit der Frage in ihrem Verzeichnis registriert werden Zugriff auf Verzeichnisdaten beteiligt ist.

Beispielsweise sollte eine Office 365-Web API-Ressource Anwendung Kalender Informationen über den Benutzer aufrufen-WebClient Benutzer müssen an die Clientanwendung zuzustimmen. Nach Zustimmung gegeben, werden die Client-Anwendung die Office 365-Web-API im Namen des Benutzers und Kalender Informationen nach Bedarf.

Zustimmung Framework basiert auf OAuth 2.0 und die verschiedenen Ströme wie Authorization Code gewähren und Client-Anmeldeinformationen erteilen öffentlichen oder vertrauliche Clients verwenden. Mithilfe von OAuth 2.0 ermöglicht Azure AD viele verschiedene Clientanwendungen wie ein Telefon, Tablet, Server oder eine Web-Anwendung und auf die erforderlichen Ressourcen zugreifen.

Ausführlichere Informationen zu Zustimmung Framework finden Sie unter [OAuth 2.0 in Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx) [Authentifizierungsszenarien Azure AD](active-directory-authentication-scenarios.md)und Office 365 Thema [Grundlegendes zur Authentifizierung mit Office 365-APIs](https://msdn.microsoft.com/office/office365/howto/common-app-authentication-tasks).

#### <a name="example-of-the-consent-experience"></a>Beispiel für die Zustimmung Erfahrung

Die folgenden Schritte zeigen, wie die Zustimmung für die Entwickler und Benutzer auftreten.

1. Auf der festgelegt Konfigurationsseite in der Azure-Verwaltungsportal Clientanwendung mithilfe von Dropdown-Menüs in anderen Applikationen Steuerelement Berechtigungen sind Berechtigungen.

    ![Berechtigungen für andere Programme](./media/active-directory-integrating-applications/permissions.png)

1. Beachten Sie, dass die Berechtigungen der Anwendung wurden aktualisiert die Anwendung ausgeführt wird und ein Benutzer wird zum ersten Mal verwendet. Wenn die Anwendung eine Zugriffs- oder Aktualisierung nicht bereits erworben hat token, die Anwendung muss Azure AD Autorisierung Endpunkt einen Autorisierungscode abrufen, die mit einem neuen Zugang und aktualisieren token.

1. Wenn der Benutzer nicht authentifiziert ist, werden sie aufgefordert Azure AD anmelden.

    ![Benutzer oder Administrator anmelden Azure AD](./media/active-directory-integrating-applications/useradminsignin.png)

1. Nachdem der Benutzer angemeldet hat, bestimmt Azure AD, ob der Benutzer eine Seite Zustimmung angezeigt werden muss. Diese Entscheidung basiert auf, ob der Benutzer (oder Administrator ihrer Organisation) bereits die Anwendung Zustimmung erteilt hat. Wenn Zustimmung nicht bereits gewährt wurde, Azure AD fordert den Benutzer zur Zustimmung und zeigt die erforderlichen Berechtigungen zu funktionieren. Der Satz von Berechtigungen im Zustimmungsdialogfeld angezeigt sind identisch mit der Auswahl in der Berechtigungen an andere Applikationen Steuerelement im klassischen Azure-Portal.

    ![Zustimmung Benutzerfunktionalität](./media/active-directory-integrating-applications/userconsent.png)

1. Nachdem der Benutzer Zustimmung erteilt, wird eine Autorisierung der Anwendung ausgegeben, Zugriffstoken und aktualisieren Token eingelöst werden können. Weitere Informationen über diese finden Sie in des [Web Application web API-Abschnitt](active-directory-authentication-scenarios.md#web-application-to-web-api) Abschnitts [Authentifizierungsszenarien Azure AD](active-directory-authentication-scenarios.md).

### <a name="configuring-a-client-application-to-access-web-apis"></a>Konfigurieren eine Clientanwendung auf Web-APIs

Damit ein Web/vertrauliche Client-Anwendung können eine Zulassung erteilen Flow, die Authentifizierung erfordert (und erhalten ein Zugriffstoken) müssen sie sichere Anmeldeinformationen herstellen. Die Standardauthentifizierungsmethode von Azure-Portal unterstützt ist die Client-ID + symmetrischen Schlüssel. Dieser Abschnitt behandelt die Konfigurationsschritte, dem geheimen Schlüssel des Clients Anmeldeinformationen.

Darüber hinaus vor kann ein Client Access Web-API verfügbar gemacht durch eine Anwendung Resource (ie: Azure AD Graph-API), Zustimmung Framework gewährleistet der Client erhält die Berechtigungen erforderlich, basierend auf den Berechtigungen angefordert. Standardmäßig können alle Berechtigungen Azure Active Directory (Graph API) und Azure Service Management API Azure AD "Anmeldung aktivieren und Lesen Benutzerprofil" Berechtigung bereits standardmäßig auswählen. Wenn die Clientanwendung in ein Office 365 Azure AD-Mandanten registriert wird, werden Web-APIs und Berechtigungen für SharePoint und Exchange Online auch ausgewählt. Sie können [zwei Arten von Berechtigungen](active-directory-dev-glossary.md#permissions) in den Dropdown-Menüs neben der gewünschten Web API auswählen:

- Berechtigungen: Die Clientanwendung muss Zugriff auf das Web API direkt als selbst (kein Benutzerkontext). Diese Art der Berechtigung Administrator Zustimmung und ist auch nicht für systemeigene Clientanwendungen verfügbar.

- Stellvertreter: Die Clientanwendung muss Zugriff auf das Web API angemeldeten Benutzers, sondern beschränkt die ausgewählte Berechtigung Zugriff. Diese Art der Berechtigung kann von einem Benutzer erteilt werden, wenn die Berechtigung als Administrator Zustimmung konfiguriert ist.

#### <a name="to-add-credentials-or-permissions-to-access-web-apis"></a>Anmeldeinformationen oder Zugriffsberechtigungen für Web-APIs

1. Melden Sie sich bei der [Azure-Verwaltungsportal.](https://manage.windowsazure.com)

1. Klicken Sie auf Active Directory im linken Menü auf und dann auf das gewünschte Verzeichnis.

1. Klicken Sie im Hauptmenü auf **Programme**und klicken Sie auf die Anwendung, die Sie konfigurieren möchten. Der Schnellstart wird einmaliges Anmelden und andere Konfigurationsinformationen angezeigt. Klicken Sie am oberen Rand der Seite zur Konfigurationsseite der Anwendung stattfinden **Konfigurieren** .

1. Scrollen Sie im Abschnitt "Schlüssel" um einen geheimen Schlüssel für die Webanwendung Anmeldeinformationen hinzufügen.  

    - Ersten "Select Dauer" Dropdown-Liste, und wählen Sie 1 oder 2 Jahre Dauer. 
    - Dann wird eine neue Zeile mit "Gültig ab" und "Läuft am" Datum ausgefüllt angezeigt.
    - Spalte ganz rechts enthält den Schlüsselwert, nachdem Sie die Konfiguration (unten) speichern. Achten Sie auf diesen Abschnitt und kopieren es speichern, drücken sie für die Verwendung in der Clientanwendung während der Authentifizierung zur Laufzeit müssen Sie.

2. Scrollen Sie zum Hinzufügen von Berechtigungen auf Ressourcen-APIs vom Client im Abschnitt "Berechtigungen an andere Programme". 
    - Klicken Sie zunächst auf die Schaltfläche "Anwendung hinzufügen".
    - Verwenden Sie die Dropdownliste "Anzeigen", um Ressourcentyp gewünschte aus.
    - Die erste Spalte können Sie Applikationen verfügbaren Ressourcen im Verzeichnis auswählen, das eine Web-API verfügbar gemacht. Die Ressource, der Sie interessieren, klicken Sie auf das Häkchen in der rechten unteren Ecke.
    - Nach der Auswahl wird die Ressource der Liste "Berechtigungen für andere Programme" angezeigt.
    - Wählen Sie die gewünschten Berechtigungen für die Clientanwendung mit "Anwendung" und "Delegiert Berechtigungen" Dropdown-Listen.

1. Klicken Sie abschließend auf die Schaltfläche **Speichern** auf der Befehlsleiste. Wenn ein Schlüssel für die Anwendung angegeben wird, wird es nach dem Klicken auf Speichern auch generiert.

>[AZURE.NOTE] Schaltfläche Speichern automatisch wird die Berechtigungen für die Anwendung im Verzeichnis basierend auf den Berechtigungen auf andere Programme, die Sie konfiguriert.  Sie können diese Berechtigungen anzeigen auf der Registerkarte Eigenschaften Anwendung.

### <a name="configuring-a-resource-application-to-expose-web-apis"></a>Konfigurieren einer ressourcenanwendung Web-APIs verfügbar machen

Erstellen eine Web-API und mithilfe Zugriff auf [Bereiche](active-directory-dev-glossary.md#scopes) und [Rollen](active-directory-dev-glossary.md#roles)Clientanwendungen zur Verfügung stellen. Eine korrekt konfigurierten Web-API wird wie andere Microsoft Web APIs, einschließlich Office 365 APIs und Graph-API verfügbar gemacht. Zugriff auf Bereiche und Rollen sind über das [Anwendungsmanifest](active-directory-dev-glossary.md#application-manifest)verfügbar ist eine JSON-Datei, die die Konfiguration der Anwendung Identität darstellt.  

Abschnitt zeigen Ihnen, wie Access Bereiche durch Ändern der Ressource Anwendungsmanifest bereitgestellt.

#### <a name="adding-access-scopes-to-your-resource-application"></a>Anwendungsspezifische Ressourcen Zugriff Bereiche hinzufügen

1. Melden Sie sich bei [Azure-Verwaltungsportal](https://manage.windowsazure.com)an.

1. Klicken Sie auf Active Directory im linken Menü auf und dann auf das gewünschte Verzeichnis.

1. Im oberen Menü klicken Sie auf **Programme**und klicken Sie dann auf ressourcenanwendung zu konfigurieren. Der Schnellstart wird einmaliges Anmelden und andere Konfigurationsinformationen angezeigt.

1. Klicken Sie auf **Manage manifest** in der Befehlszeile, und wählen Sie **Download Manifest**.

1. JSON-Anwendungsmanifestdatei öffnen und folgenden JSON-Ausschnitt "oauth2Permissions" Knoten ersetzen. Dieser Ausschnitt ist ein Beispiel ein Bereichs als "Benutzeridentitätswechsel", wodurch ein Ressourcenbesitzer zu einer Clientanwendung ein delegierter Zugriff auf eine Ressource verfügbar machen. Stellen Sie sicher, dass Sie den Text und die Werte für Ihre eigene Anwendung ändern:

        "oauth2Permissions": [
        {
            "adminConsentDescription": "Allow the application full access to the Todo List service on behalf of the signed-in   user",
            "adminConsentDisplayName": "Have full access to the Todo List service",
            "id": "b69ee3c9-c40d-4f2a-ac80-961cd1534e40",
            "isEnabled": true,
            "type": "User",
            "userConsentDescription": "Allow the application full access to the todo service on your behalf",
            "userConsentDisplayName": "Have full access to the todo service",
            "value": "user_impersonation"
            }
        ],

    Der ID-Wert muss eine neue generierte GUID, die ein [GUID-Generierungstool](https://msdn.microsoft.com/library/ms241442%28v=vs.80%29.aspx) erstellt werden oder programmgesteuert. Es stellt einen eindeutigen Bezeichner für die Berechtigung, die vom Web-API verfügbar gemacht wird. Sobald der Client Zugriff auf Ihre Web-API und die Web-API anfordern entsprechend konfiguriert ist, präsentieren sie OAuth 2.0 JWT-Token, der hat der Bereich (scp) den Wert oben, in diesem Fall User_impersonation.

    >[AZURE.NOTE] Sie können zusätzliche Bereiche später bei Bedarf verfügbar machen. Beachten Sie, dass Ihr Web API mehrere Bereiche, die eine Vielzahl von unterschiedlichen Funktionen zugeordneten verfügbar machen kann. Nun steuern Sie Zugriff auf die API mit der Forderung Bereich (scp) empfangenen OAuth 2.0 JWT-Token.

1. Speichern Sie die aktualisierte Datei JSON und Hochladen Sie Schaltfläche **Verwalten manifest** in der Befehlsleiste **Manifest hochladen**auf die aktualisierte Manifestdatei auswählen und dann auswählen. Nach dem Hochladen ist Web API jetzt konfiguriert durch andere Programme im Verzeichnis verwendet werden.

#### <a name="to-verify-the-web-api-is-exposed-to-other-applications-in-your-directory"></a>Überprüfen das Web API andere Programme im Verzeichnis ausgesetzt

1. Im oberen Menü klicken Sie auf **Programme**, wählen Sie die gewünschten Client-Anwendung im Web API konfiguriert werden soll und klicken Sie auf **Konfigurieren**.

1. Die Berechtigungen in anderen Applikationen Abschnitt scrollen. Klicken Sie im Dropdown-Menü Select Anwendung, und Sie werden im Web API auswählen, die nur eine Berechtigung für verfügbar. Wählen Sie im Dropdownmenü Berechtigungen delegiert die neue Berechtigung.

![Aufgabenliste Berechtigungen werden angezeigt.](./media/active-directory-integrating-applications/listpermissions.png)

#### <a name="more-on-the-application-manifest"></a>Weitere Informationen zum Anwendungsmanifest
Tatsächlich dient das Anwendungsmanifest aktualisiert die Anwendung Entität definiert alle Attribute einer Azure AD-Anwendung Identität Konfiguration einschließlich API-Zugriff Bereiche besprochenen. Weitere Informationen zur Entität, Anwendung finden Sie in [Graph API Anwendungsdokumentation Entität](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity). Darin finden Sie umfassende Referenzinformationen zu den Berechtigungen für Ihre API zur Anwendung Entitätsmember:  

- AppRoles Member ist eine Sammlung von [AppRole](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#approle-type) Entitäten, die zum Definieren der **Berechtigungen** für eine Web-API verwendet werden kann  
- oauth2Permissions Member ist eine Auflistung von [OAuth2Permission](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permission-type) -Entitäten, die mit **Delegierten Berechtigungen** für Web-API definieren

Weitere Informationen zum Anwendung finden manifest Konzepte im allgemeinen [Anwendungsmanifest Azure Active Directory verstehen](active-directory-application-manifest.md).

### <a name="accessing-the-azure-ad-graph-and-office-365-apis"></a>Zugriff auf Azure AD Graph und Office 365-APIs

Wie bereits erwähnt neben Verfügbarmachen/Zugriff auf APIs auf eigene Ressourcen können Sie Ihre Client-Anwendung von Microsoft-Ressourcen verfügbar gemachten APIs Zugriff auf aktualisieren.  -API Azure AD Graph "Azure Active Directory" in der Liste der Berechtigungen auf andere Programme aufgerufen wird, wird standardmäßig für alle Programme, die in Azure AD registriert sind. Wenn Ihre Client-Anwendung in Azure AD-Mandanten von Office 365 bereitgestellt Registrierung können Sie auch alle Berechtigungen von APIs, die verschiedenen Office 365-Ressourcen verfügbar gemacht zugreifen.

Eine ausführliche Übersicht über Access-Bereiche von verfügbar gemacht:  

- Azure AD Graph-API finden Sie unter [Berechtigung Bereiche | Graph-API-Konzepte](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes) Artikel.
- Office 365-APIs finden Sie unter [Authentifizierung und Autorisierung mit gemeinsamen Zustimmung](https://msdn.microsoft.com/office/office365/howto/application-manifest) Artikel. Größere Diskussion über eine Clientanwendung erstellen, die mit Office 365-APIs integriert finden Sie unter [Einrichten von Office 365 Umgebung](https://msdn.microsoft.com/office/office365/HowTo/setup-development-environment) .

>[AZURE.NOTE] Durch eine aktuelle Beschränkung können systemeigene Clientanwendungen nur in Azure AD Graph-API aufrufen, verwenden sie die Berechtigung "Zugriff auf das Verzeichnis der Organisation".  Diese Einschränkung gilt nicht für Webwaren.

### <a name="configuring-multi-tenant-applications"></a>Konfigurieren von Multi-Tenant-Clientanwendungen

Beim Hinzufügen einer Anwendung in Azure AD sollten Sie die Anwendung nur von Benutzern in Ihrer Organisation zugegriffen werden. Alternativ sollten Sie Ihre Anwendung von Benutzern in externen Organisationen zugegriffen werden. Diese beiden Typen werden einzelne und Multi-Tenant-Applikationen bezeichnet. Sie können die Konfiguration für eine einzelne Anwendung zu einer Multi-Tenant-Anwendung, die unten in diesem Abschnitt erläutert.

Es ist wichtig, beachten Sie die Unterschiede zwischen einzelne und Multi-Tenant-Anwendung:  

- Eine einzelne Anwendung dient zur Verwendung in einer Organisation. Sie sind in der Regel eine Line-of-Business (LoB) Anwendung ein Enterprise Developer geschrieben. Eine einzelne Anwendung nur von Benutzern in einem Verzeichnis zugegriffen werden muss und daher nur muss in einem Verzeichnis bereitgestellt werden.
- Eine Multi-Tenant-Anwendung für die Verwendung in vielen Organisationen. Eine Software als Service (SaaS) Web-Anwendung in der Regel von einem unabhängigen Softwareanbieter (ISV) geschrieben sind. Multi-Tenant-Applikationen müssen in jedem Verzeichnis, in dem sie verwendet werden, Benutzer oder Administrator Zustimmung, Azure AD Zustimmung Rahmen unterstützt registrieren erfordert bereitgestellt. Beachten Sie, dass alle systemeigenen Clientanwendungen mehrinstanzenfähigen standardmäßig diese auf der Ressourcenbesitzer Gerät installiert sind. Übersicht Consent Framework Abschnitt Weitere Details auf Zustimmung Framework.

#### <a name="enabling-external-users-to-grant-your-application-access-to-their-resources"></a>Externe Benutzer die Anwendungszugriff auf ihre Ressourcen gewähren

Wenn Sie eine Anwendung, die Sie Ihren Kunden oder Partnern außerhalb Ihrer Organisation zur Verfügung stellen möchten schreiben, müssen Sie die Anwendungsdefinition im klassischen Azure-Portal aktualisieren.

>[AZURE.NOTE] Beim Aktivieren von Multi-Tenant müssen Sie sicherstellen, dass der Anwendung App ID URI überprüft Domäne gehört. Darüber hinaus muss der Rückgabe-URL mit https:// beginnen. Weitere Informationen finden Sie in der [Anwendung und Service Principal-Objekte](active-directory-application-objects.md).

Externen Benutzern Zugriff auf Ihre Anwendung zu aktivieren: 

1. Melden Sie sich bei der [Azure-Verwaltungsportal.](https://manage.windowsazure.com)

1. Klicken Sie auf Active Directory im linken Menü auf und dann auf das gewünschte Verzeichnis.

1. Klicken Sie im Hauptmenü auf **Programme**und klicken Sie auf die Anwendung, die Sie konfigurieren möchten. Der Schnellstart wird mit Konfigurationsoptionen angezeigt.

1. Erweitern Sie im Abschnitt **Konfigurieren mehrinstanzenfähigen Anwendung** Quick Start und klicken Sie **jetzt konfigurieren** im Abschnitt Zugriff aktivieren. Die Anwendung Eigenschaften wird angezeigt.

1. Klicken Sie auf die Schaltfläche **Ja** neben Anwendung ist Multi-Tenant klicken Sie auf der Befehlsleiste auf die Schaltfläche **Speichern** .

Sobald die vorstehend beschriebene Änderung vorgenommen haben, werden können die Anwendungszugriff auf Verzeichnis und andere Daten Benutzern und Administratoren in anderen Organisationen.

#### <a name="triggering-the-azure-ad-consent-framework-at-runtime"></a>Azure AD Zustimmung Framework zur Laufzeit auslösen

Um die Zustimmung verwenden, müssen Multi-Tenant-Clientanwendungen OAuth 2.0 mit Genehmigung anfordern. [Codebeispiele](https://azure.microsoft.com/documentation/samples/?service=active-directory&term=multi-tenant) stehen wie ein Webanwendungsprojekt systemeigene Anwendung oder Server-Daemon Anwendung Autorisierungscodes Zugriffstoken Web-APIs aufrufen anfordert und anzeigen.

Webtests kann Anmeldeprozess für Benutzer bieten. Wenn Sie ein Anmeldeprozess anbieten, erwartet der Benutzer auf eine Schaltfläche klicken, Umleitung der Browser Azure AD OAuth2.0 Endpunkt oder Endpunkt Userinfo OpenID verbinden autorisieren. Diese Endpunkte ermöglichen die Anwendung Informationen zu den neuen Benutzer anhand der ID-Token. Nach der Anmeldung Phase wird der Benutzer in der Übersicht der Zustimmung Framework Abschnitt oben ähnlich Zustimmung aufgefordert angezeigt.

Sie können Webtests Erlebnis bieten, die Administratoren sich "Meine Firma". Dies würde auch Umleiten des Benutzers auf Azure AD OAuth 2.0 autorisieren Endpunkt. In diesem Fall jedoch übergeben eine Aufforderung Admin_consent-Parameter auf Autorisieren Endpunkt Erfahrung Zustimmung Administrator erzwingen, der Administrator Zustimmung im Namen ihrer Organisation gewährt. Nur ein Benutzer, der über ein Konto authentifiziert, der der globalen Administratorrolle angehört bieten Zustimmung; andere Benutzer erhalten eine Fehlermeldung. Die Antwort enthält erfolgreiche Zustimmung Admin_consent = True. Beim Zugriffstoken einlösen, erhalten Sie ein ID-Token, die Informationen über die Organisation und der Administrator, der für die Anwendung.

### <a name="enabling-oauth-20-implicit-grant-for-single-page-applications"></a>Aktivieren OAuth 2.0 implizite für einzelne Seite gewähren

Einzelne Seite Anwendung (SPA) sind in der Regel mit einem JavaScript-schwer aufgebaut, das Ende, um die Geschäftslogik ausführen im Browser, der wodurch die Anwendung Web-API aufgerufen wird. SPA in Azure AD gehostet wird verwenden Sie implizite Gewährung von OAuth 2.0 Azure AD Benutzer authentifizieren und ein Token, das Sie Anrufe von der Anwendung JavaScript-Client auf die Back-End-Web-API verwenden können. Nachdem der Benutzer Zustimmung erteilt hat, kann diese dieselbe Authentifizierungsprotokoll herangezogen Token sichern Aufrufe zwischen dem Client und anderen Web-API-Ressourcen für die Anwendung konfiguriert. Erfahren Sie mehr über implizite Autorisierung finden Sie unter gewähren und Sie entscheiden, ob es für Ihr Szenario geeignete Hilfe [OAuth2 implizite Gewährung Fluss in Azure Active Directory verstehen ](active-directory-dev-understanding-oauth2-implicit-grant.md).

Implizite Gewährung OAuth 2.0 ist für die Anwendung deaktiviert. Sie können OAuth 2.0 implizite Gewährung für die Anwendung Festlegen der `oauth2AllowImplicitFlow`"" Wert in seiner [Anwendungsmanifest](active-directory-application-manifest.md), die JSON-Datei der Anwendung Identität Konfiguration darstellt.

#### <a name="to-enable-oauth-20-implicit-grant"></a>Implizite Gewährung OAuth 2.0 aktivieren 

1. Melden Sie sich bei der [Azure-Verwaltungsportal.](https://manage.windowsazure.com)
1. Klicken Sie auf **Active Directory** im linken Menü auf und dann auf das gewünschte Verzeichnis.
1. Klicken Sie im Hauptmenü auf **Programme**und klicken Sie auf die Anwendung, die Sie konfigurieren möchten. Der Schnellstart wird einmaliges Anmelden und andere Konfigurationsinformationen angezeigt.
1. Klicken Sie auf **Manage manifest** in der Befehlszeile, und wählen Sie **Download Manifest**.
Öffnen Sie die Anwendungsmanifestdatei JSON und legen Sie den Wert "oauth2AllowImplicitFlow" auf "True". Standardmäßig wird "false".

    `"oauth2AllowImplicitFlow": true,`

1. Speichern Sie die aktualisierte Datei JSON und Hochladen Sie Schaltfläche **Verwalten manifest** in der Befehlsleiste **Manifest hochladen**auf die aktualisierte Manifestdatei auswählen und dann auswählen. Nach dem Hochladen ist das Web-API jetzt konfiguriert OAuth 2.0 implizite Gewährung zum Authentifizieren von Benutzern.


### <a name="legacy-experiences-for-granting-access"></a>Ältere Funktionen für den Zugriff

Dieser Abschnitt beschreibt die legacy Zustimmung Erfahrung 12 März 2014. Dies wird weiterhin unterstützt und beschrieben wird. Vor der neuen Funktionalität kann nur die folgenden Berechtigungen gewähren:

- Benutzer ihrer Organisation anmelden

- Benutzer anmelden und Lesen der Verzeichnisdaten ihrer Organisation (wie die Anwendung)

- Benutzer melden Sie an und Daten lesen Sie und Schreiben Sie ihrer Organisation Verzeichnis (wie die Anwendung)

Sie können die Schritte ausführen, in [Entwicklung Multi-Tenant ASP.NET-Webanwendungen mit Azure](https://msdn.microsoft.com/library/azure/dn151789.aspx) Zugriff für die neue Anwendung in Azure AD registriert. Es ist wichtig zu beachten, dass neue Zustimmung Framework viel leistungsfähigere Applikationen kann und auch Benutzer diesen nur Administratoren zustimmen.

#### <a name="building-the-link-that-grants-access-for-external-users-legacy"></a>Erstellen der Verknüpfung, die Zugriff für externe Benutzer (Legacy)

Damit externe Benutzer für ihre Organisation Konten mithilfe müssen Sie Aktualisieren Ihrer Anwendung um einer Schaltfläche mit einem Link zu der Seite auf Azure AD anzuzeigen, die sie gewähren können. Anleitung für dieses Zeichen Taste Branding im Thema [Brandingrichtlinien für integrierte](active-directory-branding-guidelines.md) erläutert. Nachdem der Benutzer gewährt oder verweigert, leiten Azure AD Grant Access Seite Browser für Ihre Anwendung eine Antwort. Weitere Informationen zu Anwendungseigenschaften finden Sie unter [Anwendungsobjekte und Service-Prinzipien](active-directory-application-objects.md).

Datenzugriffsseite Erteilen von Azure AD erstellt und finden einen Link auf Ihre app-Konfigurationsseite im klassischen Azure-Portal. Um zur Seite Konfiguration zu erhalten, klicken auf **Applikationen** im Hauptmenü Azure AD-Mandanten auf app zu konfigurieren, klicken Sie auf **Konfigurieren** Menü oben auf der Seite Quick Start.

Die Verknüpfung für die Anwendung wird so aussehen: `http://account.activedirectory.windowsazure.com/Consent.aspx?ClientID=058eb9b2-4f49-4850-9b78-469e3176e247&RequestedPermissions=DirectoryReaders&ConsentReturnURL=https%3A%2F%2Fadatum.com%2FExpenseReport.aspx%3FContextId%3D123456`. Die folgende Tabelle beschreibt die Teile der Verknüpfung:

|Parameter|Beschreibung|
|---|---|
|ClientId|Erforderlich. Die Client-ID abgerufen als Teil Ihrer Anwendung hinzufügen.|
|RequestedPermissions|Optional. Zugriffsebene, die Ihre Anwendung anfordert, die dem Benutzer die Anwendung Zugriff angezeigt werden. Wenn nicht angegeben, wird die angeforderte Zugriffsebene einmaliges nur standardmäßig. Die Optionen sind DirectoryReaders und DirectoryWriters. Weitere Informationen über diese Zugriffsebenen finden Sie unter Zugriffsebenen Anwendung.|
|ConsentReturnUrl|Optional. Der URL auf Antwort erteilen Zugriff soll. Dieser Wert muss URL-codiert und muss unter der gleichen Domäne wie die Antwort-URL definieren Sie in der Anwendung konfiguriert werden. Wenn nicht angegeben, wird Antwort Zugriff erteilen die konfigurierte Antwort-URL umgeleitet.|

Ein ConsentReturnUrl getrennt von den Antwort-URL angeben können Ihre app separate Logik zu implementieren, der die Antwort auf eine andere URL aus dem Antwort-URL verarbeiten kann (die normalerweise SAML-Tokens Schild auf verarbeitet). Sie können auch angeben, dass zusätzliche Parameter in die ConsentReturnURL URL codiert. für Ihre Anwendung bei der Umleitung werden diese als Abfragezeichenfolgen-Parameter übergeben.  Dieser Mechanismus kann verwendet werden, zusätzliche Informationen oder app Anforderung Zugriff gewähren auf die Antwort von Azure AD binden.

#### <a name="grant-access-user-experience-and-response-legacy"></a>GRANT Access-Benutzeroberfläche und Antwort (Legacy)

Wenn eine Anwendung auf Grant Access Link leitet zeigen die folgenden Bilder erleben der Benutzer.

Wenn der Benutzer nicht angemeldet ist, werden sie dazu aufgefordert werden:

![AAD anmelden](./media/active-directory-integrating-applications/signintoaad.png)

Nachdem der Benutzer authentifiziert ist, leitet Azure AD Benutzer Seite Zugriff gewähren:

![Zugriff gewähren](./media/active-directory-integrating-applications/grantaccess.png)

>[AZURE.NOTE] Nur der Unternehmensadministrator der externen Organisation kann Zugriff auf Ihre Anwendung gewähren. Ist der Benutzer kein Unternehmensadministrator, sie können e-Mail an ihren Unternehmensadministrator anweisen, diese app Zugriff erhalten.

Nachdem der Kunden für Ihre Anwendung Zugriff wurde durch Klicken auf Zugriff gewähren oder Zugriff verweigert haben, klicken Sie auf Abbrechen, sendet Azure AD eine Antwort, die ConsentReturnUrl oder die konfigurierte Antwort-URL. Diese Antwort enthält die folgenden Parameter:

|Parameter|Beschreibung|
|---|---|
|TenantId|Die eindeutige ID des Unternehmens in Azure Anzeige, die für Ihre Anwendung Zugriff hat. Dieser Parameter ist nur, wenn der Kunde Zugriff gewährt.|
|Zustimmung|Der Wert wird gewährt wurde die Anwendung Zugriff gewährt oder verweigert die Anforderung abgelehnt wurde festgesetzt.|

Zusätzliche Parameter werden an die Anwendung zurückgegeben, wenn sie als Teil der ConsentReturnUrl URL codiert angegeben wurden. Im folgenden werden ein Beispielantwort auf Anforderung Zugriff gewähren, die die Anwendung autorisiert angibt und enthält ein KontextID angegeben wurde Anforderung Zugriff gewähren: `https://adatum.com/ExpenseReport.aspx?ContextID=123456&Consent=Granted&TenantId=f03dcba3-d693-47ad-9983-011650e64134`.

>[AZURE.NOTE] Zugriff erteilen Antwort enthält ein Sicherheitstoken für den Benutzer nicht. die Anwendung muss den Benutzer separat anmelden.

Im folgenden finden ein Beispiel auf eine Anfrage erteilen, die verweigert wurde:`https://adatum.com/ExpenseReport.aspx?ContextID=123456&Consent=Denied`

#### <a name="rolling-app-keys-for-uninterrupted-access-to-the-graph-api-legacy"></a>Paralleles app-Schlüssel für den ununterbrochenen Zugriff der Graph-API (Legacy)

Während der Lebensdauer der Anwendung müssen Sie die Schlüssel ändern, wenn Sie in Azure AD zu Zugriffstoken aufrufen Graph-API aufrufen.  Im Allgemeinen ändern fallen in zwei Kategorien: Notfall Rollover, wo der Schlüssel gefährdet ist oder Rollover der aktuelle Schlüssel wird abläuft. Folgende Schritte müssen ausgeführt werden, um Ihre app ununterbrochenen Zugriff während der Schlüssel (hauptsächlich für den zweiten Fall) aktualisieren.

1. In der Azure-Verwaltungsportal auf Ihr Verzeichnis Mandanten, klicken Sie im oberen Menü auf **Programme** , klicken die app zu konfigurieren. Der Schnellstart wird einmaliges Anmelden und andere Konfigurationsinformationen angezeigt.

1. Klicken Sie auf **Konfigurieren** im oberen Menü auf Ihre Anwendung Eigenschaften aufgelistet und sehen Sie eine Liste der Schlüssel.

1. Unter Schlüssel klicken Sie auf die Dropdownliste, wählen Sie **Dauer** wählen Sie 1 oder 2 Jahre, und klicken Sie auf **Speichern** in der Befehlsleiste. Dies generiert einen neuen Kennwortschlüssel für Ihre Anwendung. Kopieren Sie dieses neue Kennwortschlüssel. An diesem Punkt kann sowohl vorhandene und neue Schlüssel von der Anwendung verwendet werden Azure AD Zugriffstoken erhältlich.

1. Zurück zu Ihrer app und die Konfiguration starten mit dem neuen Kennwortschlüssel aktualisieren. Finden Sie ein Beispiel, in dem diese Aktualisierung stattfinden soll [mithilfe der Graph-API Azure AD abgefragt](https://msdn.microsoft.com/library/azure/dn151791.aspx) .

1. Sie sollten jetzt diese Änderung in Ihrem Produktionsumfeld – Überprüfung zuerst auf einen Dienstknoten vor dem Rollout der Rest ausrollen.

1. Nach die Aktualisierung in der produktionsbereitstellung abgeschlossen ist, können Sie wieder zum klassischen Azure-Portal, und entfernen Sie den alten Schlüssel.

#### <a name="changing-app-properties-after-enabling-access-legacy"></a>Ändern der app nach Zugang (Legacy)

Wenn Sie Ihre Anwendung für externe Benutzer aktivieren, können Sie weiterhin Ihre app Eigenschaften im klassischen Azure-Portal ändern. Kunden bereits Zugriff auf Ihre Anwendung gewährt vor app Änderungen sehen jedoch nicht die Änderungen beim Anzeigen von Details über Ihre Anwendung in Azure-Verwaltungsportal. Nachdem die Anwendung zur Verfügung gestellt wird, müssen Sie sehr vorsichtig sein, wenn bestimmte Änderungen. Beispielsweise werden bei der Aktualisierung der App-ID URI Ihre bestehenden Kunden, die vor dieser Änderung Zugriff nicht mithilfe ihrer Firma oder Schule Konten anmelden werden.

Wenn Sie ändern eine höhere Zugriffsebene anfordern Kunden app Funktionalität verwenden, die jetzt nutzt die neue API-Aufrufe mit höheren RequestedPermissions erhalten Zugriff auf Zugriff verweigert Antwort von Graph-API.  Ihre app sollte diese Fälle und Fragen Sie den Kunden auf Ihre Anwendung mit der Anforderung für eine höhere Zugriffsebene zu gewähren.

## <a name="removing-an-application"></a>Entfernen einer Anwendung

Dieser Abschnitt beschreibt, wie eine Anwendung in Azure AD-Mandanten zu entfernen.

### <a name="removing-an-application-authored-by-your-organization"></a>Entfernen einer Anwendung von Ihrer Organisation erstellt
Dies sind Programme, die für Ihren Mandanten Azure AD Filter "Applikationen besitzt Mein Unternehmen" auf "Programme" angezeigt. Technisch gesehen sind diese über klassischen Azure-Portal oder programmgesteuert über PowerShell oder Graph-API registriert. Genauer gesagt, werden sie durch eine Anwendung und Service Principal-Objekt in Ihrem Mandanten dargestellt. Weitere Informationen finden Sie in der [Anwendung und Service Principal-Objekte](active-directory-application-objects.md) .

#### <a name="to-remove-a-single-tenant-application-from-your-directory"></a>Eine einzelne Anwendung aus dem Verzeichnis entfernen

1. Melden Sie sich bei der [Azure-Verwaltungsportal.](https://manage.windowsazure.com)

1. Klicken Sie im linken Menü auf Active Directory, und klicken Sie dann auf das gewünschte Verzeichnis.

1. Klicken Sie im Hauptmenü auf **Programme**und klicken Sie auf die Anwendung, die Sie konfigurieren möchten. Der Schnellstart wird einmaliges Anmelden und andere Konfigurationsinformationen angezeigt.

1. Klicken Sie auf die Schaltfläche **Löschen** in der Befehlszeile.

1. Klicken Sie in der Bestätigungsnachricht auf **Ja** .

#### <a name="to-remove-a-multi-tenant-application-from-your-directory"></a>Multi-Tenant-Anwendung aus dem Verzeichnis entfernen

1. Melden Sie sich bei der [Azure-Verwaltungsportal.](https://manage.windowsazure.com)

1. Klicken Sie im linken Menü auf Active Directory, und klicken Sie dann auf das gewünschte Verzeichnis.

1. Klicken Sie im Hauptmenü auf **Programme**und klicken Sie auf die Anwendung, die Sie konfigurieren möchten. Der Schnellstart wird einmaliges Anmelden und andere Konfigurationsinformationen angezeigt.

1. Die Anwendung wird Multi-Tenant-Abschnitt klicken Sie auf **Nein**. Die Anwendung einzelne konvertiert, aber die Anwendung bleibt in einer Organisation, die bereits zugestimmt hat.

1. Klicken Sie auf die Schaltfläche **Löschen** in der Befehlszeile.

1. Klicken Sie in der Bestätigungsnachricht auf **Ja** .

### <a name="removing-a-multi-tenant-application-authorized-by-another-organization"></a>Entfernen einer Multi-Tenant-Anwendung von einer anderen Organisation autorisiert
Diese sind eine Teilmenge der Programme, die mit dem Filter "Applikationen verwendet Mein Unternehmen" main "Applications" auf Ihre Azure AD-Mandanten, insbesondere diejenigen, die nicht in der Liste anzeigen "Programme besitzt Mein Unternehmen". Technisch gesehen sind diese Multi-Tenant bei Zustimmung registriert. Genauer gesagt, sind sie nur ein Service Principal-Objekt in Ihrem Mandanten dargestellt. Weitere Informationen finden Sie in der [Anwendung und Service Principal-Objekte](active-directory-application-objects.md) .

Um eine Multi-Tenant-Anwendung Zugriff auf das Verzeichnis entfernen (nach müssen Zustimmung erteilt), muss der Unternehmensadministrator Azure-Abonnement Zugriff über Azure-Verwaltungsportal zu entfernen. Einfach zur Konfigurationsseite der Anwendung navigieren und klicken Sie unten auf "Zugriff verwalten". Auch können der Unternehmensadministrator [Azure AD PowerShell-Cmdlets](http://go.microsoft.com/fwlink/?LinkId=294151) Zugriff entfernen.

## <a name="next-steps"></a>Nächste Schritte

- [Brandingrichtlinien für integrierte Apps](active-directory-branding-guidelines.md) für visuelle Hinweise für Ihre anzeigen

- Weitere Informationen zur Beziehung zwischen Anwendung und Service Principal-Objekte einer Anwendung finden Sie unter [Anwendung und Service Principal-Objekte](active-directory-application-objects.md).

- Erfahren Sie mehr über die Rolle Wiedergabe der app-manifest, finden Sie unter [Understanding Anwendungsmanifest Azure Active Directory](active-directory-application-manifest.md)

- [Azure AD Entwickler Glossar](active-directory-dev-glossary.md) Definitionen einiger Azure Active Directory (AD) Developer Kernkonzepte anzeigen

- Die [Active Directory-Entwicklerhandbuch](active-directory-developers-guide.md) Überblick aller Entwickler Inhalte.
