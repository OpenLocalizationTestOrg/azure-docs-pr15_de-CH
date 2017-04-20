<properties
    pageTitle="Rufen Sie eine benutzerdefinierte API in Logik-apps"
    description="Mithilfe der benutzerdefinierten Logik Apps App-Dienst gehosteten API"
    authors="stepsic-microsoft-com"
    manager="dwrede"
    editor=""
    services="logic-apps"
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/31/2016"
    ms.author="stepsic"/>

# <a name="using-your-custom-api-hosted-on-app-service-with-logic-apps"></a>Mithilfe der benutzerdefinierten Logik Apps App-Dienst gehosteten API

Logik-apps hat einen umfangreichen Satz von 40 Connectors für eine Vielzahl von Diensten, sollten Sie Ihre eigene benutzerdefinierte API aufrufen, die Ihren eigenen Code ausführen können. Die einfachste und skalierbarste Arten hosten Sie Ihre eigenen benutzerdefinierte Web API ist App nutzen. Dieser Artikel beschreibt die Web app App Service API, WebApp oder mobile Anwendung gehostet API aufrufen.

Informationen zum Erstellen von APIs als Trigger oder Aktion Logik Apps finden Sie [in diesem Artikel](app-service-logic-create-api-app.md).

## <a name="deploy-your-web-app"></a>Bereitstellen Sie Ihrer Anwendung

Zuerst müssen Sie Ihre API als Web-App in App Service bereitstellen. Die Anleitung behandelt grundlegende Bereitstellung: [Erstellen einer ASP.NET Web-Anwendung](../app-service-web/web-sites-dotnet-get-started.md).  Während Sie aus einer Anwendung Logik in jeder API aufrufen, für optimale Ergebnisse empfehlen wir stolz Metadaten fügen Sie mit Logik apps integrieren hinzu.  Zum [Hinzufügen von stolz](../app-service-api/app-service-api-dotnet-get-started.md#use-swagger-api-metadata-and-ui)finden Sie.

### <a name="api-settings"></a>API-Einstellung

Reihenfolge für Logik apps Designer analysiert Ihre stolz unbedingt CORS aktivieren und Festlegen der Eigenschaften APIDefinition Ihrer Anwendung.  Dies ist sehr einfach im Azure-Portal.  Einfach öffnen Blatt Einstellungen der Web-App im Abschnitt API Festlegen der "API-Definition" auf die URL der Datei swagger.json (Dies ist in der Regel https://{name}.azurewebsites.net/swagger/docs/v1) und Hinzufügen einer Richtlinie CORS für ' *' Anfragen von Logik apps Designer ermöglicht.

## <a name="calling-into-the-api"></a>Aufrufen der API

Fügen Sie innerhalb der Logik apps Portal, wenn CORS gesetzt haben und die API-Eigenschaften einfach man sollte benutzerdefinierte API Aktionen innerhalb der hinzu.  Im Designer können Sie Ihr Abonnement zum Auflisten von Websites stolz URL definiert Webbrowser auswählen.  Sie können auch die HTTP + stolz Aktion auf ein stolz und Listen der verfügbaren Aktionen und Eingaben.  Schließlich können Sie jederzeit erstellen eine Anforderung mit HTTP-Aktion jeder API auch aufrufen, die nicht oder stolz Doc verfügbar machen.

Wenn Sie Ihre API sichern möchten, sind einige andere Wege:

1. Keine Änderung des Codes erforderlich – Azure Active Directory kann zu Ihrer API ohne Code Änderung oder erneute Bereitstellung verwendet werden.
1. Erzwingen Sie Standardauthentifizierung AAD-Authentifizierung oder Zertifikat Auth im Code der API.

## <a name="securing-calls-to-your-api-without-a-code-change"></a>Sichere Aufrufe Ihrer API ohne code

In diesem Abschnitt erstellen Sie zwei Azure Active Directory Applikationen – einen für Ihre Logik und einen für Ihre Web-Anwendung.  Sie müssen Aufrufe Ihrer Anwendung mit AAD Anwendung für Ihre Logik Dienstprinzipalnamen (Client-Id und Schlüssel) authentifizieren. Außerdem enthalten Sie die Anwendung in die Logik app Definition IDs.

### <a name="part-1-setting-up-an-application-identity-for-your-logic-app"></a>Teil 1: Einrichten einer Anwendungsidentität für Ihre Logik

Dadurch wird die Logik-app verwendet wird, active Directory authentifizieren. Nur Sie *müssen* dazu einmal für das Verzeichnis. Beispielsweise können Sie mit derselben Identität aller Logik-apps, obwohl Sie außerdem erstellen möglicherweise eindeutige Identitäten Pro Logic app. Sie tun dies in der Benutzeroberfläche oder mithilfe von PowerShell.

#### <a name="create-the-application-identity-using-the-azure-classic-portal"></a>Erstellen Sie Anwendung mit klassischen Azure-portal

1. Navigieren Sie zu [Active Directory im klassischen Azure-Portal](https://manage.windowsazure.com/#Workspaces/ActiveDirectoryExtension/directory) und wählen Sie das Verzeichnis für Ihr Web App verwenden
2. Klicken Sie auf die Registerkarte **Applications**
3. Klicken Sie in der Befehlsleiste am unteren Rand der Seite auf **Hinzufügen**
4. Benennen Sie Ihre Identität verwenden, klicken Sie auf den Pfeil
5. In eine eindeutige Zeichenfolge formatiert als Domäne in die beiden Textfelder, und klicken Sie auf das Häkchen
6. Klicken Sie auf die Registerkarte **Konfigurieren** für diese Anwendung
7. Kopieren Sie die **Client-ID**, diese wird in Ihrer Anwendung Logik
8. **Der Abschnitt** auf **Dauer auswählen** , und wählen Sie entweder 1 oder 2 Jahre
9. Klicken Sie auf **Speichern** am unteren Bildschirmrand (möglicherweise einige Sekunden)
10. Jetzt müssen Sie den Schlüssel in das Feld zu kopieren. Dies wird auch in Ihrer Anwendung Logik verwendet

#### <a name="create-the-application-identity-using-powershell"></a>Erstellen Sie Anwendung mit PowerShell
1. `Switch-AzureMode AzureResourceManager`
2. `Add-AzureAccount`
3. `New-AzureADApplication -DisplayName "MyLogicAppID" -HomePage "http://someranddomain.tld" -IdentifierUris "http://someranddomain.tld" -Password "Pass@word1!"`
4. Kopieren Sie die **Mandanten-ID**und die **ID der Anwendung** verwendete Kennwort

### <a name="part-2-protect-your-web-app-with-an-aad-app-identity"></a>Teil 2: Ihrer Anwendung AAD app Identität schützen

Wenn bereits Ihrer Anwendung können Sie diese nur im Portal aktivieren. Andernfalls können Sie Aktivieren der Autorisierung in der Bereitstellung von Azure Resource Manager.

#### <a name="enable-authorization-in-the-azure-portal"></a>Autorisierung in Azure-Portal aktivieren

1. Web app und Sie auf **Einstellungen** in der Befehlszeile.
2. Klicken Sie auf **Autorisierung-Authentifizierung**.
3. **Aktivieren Sie es.**

Zu diesem Zeitpunkt wird eine Anwendung automatisch für Sie erstellt. Sie benötigen diese Anwendung Client-ID für Teil 3, Sie müssen:

1. Wechseln Sie zu [Active Directory im klassischen Azure-Portal](https://manage.windowsazure.com/#Workspaces/ActiveDirectoryExtension/directory) , und wählen Sie das Verzeichnis.
2. Suchen Sie die app in das Suchfeld
3. Klicken Sie in der Liste auf
4. Klicken Sie auf die Registerkarte **Konfigurieren**
5. Die **Client-ID** sollte angezeigt werden.

#### <a name="deploying-your-web-app-using-an-arm-template"></a>Bereitstellen Ihrer Anwendung mithilfe einer Vorlage ARM

Zunächst müssen Sie eine Anwendung für Ihr Web app erstellen. Dies sollte von der Anwendung sein, die für Ihre Logik verwendet wird. Zunächst die Schritte oben in Teil 1, sondern jetzt die **HomePage** und **IdentifierUris** die tatsächlichen https://**URL** Ihrer Anwendung.

>[AZURE.NOTE]Beim Erstellen der Anwendungdes für Ihre Web-app verwenden Sie [Azure klassische Portal Ansatz](https://manage.windowsazure.com/#Workspaces/ActiveDirectoryExtension/directory)wie PowerShell-Cmdlet nicht die erforderlichen Berechtigungen für eine Website Benutzer anmelden wird.

Wenn Ihre Kunden und Mandanten-ID, folgende Sub Ressource Web App in Ihrer Bereitstellungsvorlage:

```
"resources" : [
    {
        "apiVersion" : "2015-08-01",
        "name" : "web",
        "type" : "config",
        "dependsOn" : [
            "[concat('Microsoft.Web/sites/','parameters('webAppName'))]"
        ],
        "properties" : {
            "siteAuthEnabled": true,
            "siteAuthSettings": {
              "clientId": "<<clientID>>",
              "issuer": "https://sts.windows.net/<<tenantID>>/",
            }
        }
    }
]
```

Führen Sie eine Bereitstellung automatisch, die bringt eine leere Web app und Logik app zusammen, mit denen AAD klicken:

[![In Azure bereitstellen](./media/app-service-logic-custom-hosted-api/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-logic-app-custom-api%2Fazuredeploy.json)

Vollständige Vorlage finden Sie unter [app Ruft eine benutzerdefinierte API App-Dienst gehostet und AAD geschützt](https://github.com/Azure/azure-quickstart-templates/blob/master/201-logic-app-custom-api/azuredeploy.json).


### <a name="part-3-populate-the-authorization-section-in-the-logic-app"></a>Teil 3: Abschnitt Autorisierung der Anwendung Logik füllen

Im Abschnitt **Authorization** **HTTP** -Aktion:`{"tenant":"<<tenantId>>", "audience":"<<clientID from Part 2>>", "clientId":"<<clientID from Part 1>>","secret": "<<Password or Key from Part 1>>","type":"ActiveDirectoryOAuth" }`

| Element | Beschreibung |
|---------|-------------|
| Typ | Art der Authentifizierung. ActiveDirectoryOAuth-Authentifizierung ist der Wert ActiveDirectoryOAuth. |
| Mieter | Der Mieter Bezeichner zur Identifikation der AD-Mandanten. |
| Zielgruppe | Erforderlich. Die Ressource, der Sie eine Verbindung herstellen. |
| clientID | Die Client-ID für die Azure AD-Anwendung. |
| Geheimnis | Erforderlich. Kennwort des Clients, die das Token angefordert werden. |

Diese Vorlage hat dies bereits, aber wenn Sie die Anwendung Logik direkt erstellen, müssen Sie die volle Berechtigung Abschnitt.

## <a name="securing-your-api-in-code"></a>Sichern Ihre API im code

### <a name="certificate-auth"></a>Authentifizierung Zertifikat

Clientzertifikate können um eingehenden Anfragen zu Ihrer Anwendung zu überprüfen. Siehe [Wie auf TLS gegenseitige Authentifizierung konfigurieren Web App](../app-service-web/app-service-web-configure-tls-mutual-auth.md) für Code einrichten.

Im Abschnitt *Authorization* soll: `{"type": "clientcertificate","password": "test","pfx": "long-pfx-key"}`.

| Element | Beschreibung |
|---------|-------------|
| Typ | Erforderlich. Art der Authentifizierung. Der Wert muss für SSL Clientzertifikate ClientCertificate sein. |
| PFX | Erforderlich. Base64-codierten Inhalt der PFX-Datei. |
| Kennwort | Erforderlich. Kennwort für die PFX-Datei zugreifen. |

### <a name="basic-auth"></a>Standardauthentifizierung

Standardauthentifizierung (Benutzername und Kennwort) können Sie eingehenden Anfragen überprüfen. Standardauthentifizierung ist ein allgemeines Muster und Sie kann es in jeder Sprache in Ihrer Anwendung zu erstellen.

Im Abschnitt *Authorization* soll: `{"type": "basic","username": "test","password": "test"}`.

| Element | Beschreibung |
|---------|-------------|
| Typ | Erforderlich. Art der Authentifizierung. Bei der Standardauthentifizierung muss der Wert Basic. |
| Benutzername | Erforderlich. Der Benutzername authentifiziert. |
| Kennwort | Erforderlich. Kennwort authentifizieren. |

### <a name="handle-aad-auth-in-code"></a>AAD-Authentifizierung im Code behandeln

Standardmäßig Azure Active Directory-Authentifizierung, die im Portal ermöglichen eine fein abgestufte Autorisierung nicht. Beispielsweise wird nicht Ihre API einen bestimmten Benutzer oder einer Anwendung, sondern nur auf einen bestimmten Mandanten gesperrt werden.

Wenn Sie die API nur die Logik App, z. B. im Code einschränken können Sie Header extrahieren enthält JWT und prüfen, wer der Aufrufer ist, Anfragen ablehnen, die nicht übereinstimmen.

Weiter, wenn Sie in Ihrem eigenen Code implementieren und Portal-Funktion nicht nutzen möchten, können Sie diesen Artikel lesen: [Verwenden Sie Active Directory für die Authentifizierung in Azure App Service](../app-service-web/web-sites-authentication-authorization.md).

Weiterhin müssen die obigen Schritte zum eine Anwendungsidentität für Ihre Logik erstellen und verwenden, um die API aufzurufen.
