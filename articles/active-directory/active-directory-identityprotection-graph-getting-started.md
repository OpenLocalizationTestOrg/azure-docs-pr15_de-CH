<properties
    pageTitle="Erste Schritte mit Azure Active Directory Schutz und Microsoft Graph | Microsoft Azure"
    description="Enthält eine Einführung in Microsoft Graph Abfrage eine Liste der Risiken und Informationen von Azure Active Directory."
    services="active-directory"
    keywords="Azure active Directory Schutz Risikoereignis Schwachstelle, Sicherheitsrichtlinien, Microsoft Graph"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="markvi"/>

# <a name="get-started-with-azure-active-directory-identity-protection-and-microsoft-graph"></a>Erste Schritte mit Azure Active Directory Schutz und Microsoft Graph

Microsoft Graph ist Microsofts einheitliche API Endpunkt [Azure Active Directory Identitätsschutz](active-directory-identityprotection.md) APIs. Unsere erste API **IdentityRiskEvents**können Sie eine Abfrage Microsoft Graph für eine Liste von [Risiken](active-directory-identityprotection-risk-events-types.md) und Informationen. Dieser Artikel wird Ihnen diese API Abfragen. Detaillierte Einführung, vollständige Dokumentation und Zugriff auf das Graph-Explorer finden Sie in der [Microsoft Graph-Website](https://graph.microsoft.io/).

Es gibt drei Schritte zum Schutz Datenzugriffe in Microsoft Graph:

1. Eine Anwendung mit einem geheimen hinzufügen. 

2. Verwenden Sie diesen Schlüssel und einige andere Informationen zu Microsoft Graph, authentifiziert, ein Authentifizierungstoken wird. 

3. Verwenden Sie dieses Token Anfragen an den API-Endpunkt und Schutz von Daten zurück.

Bevor Sie beginnen, benötigen Sie:

- Administratorrechte zum Erstellen der Anwendung in Azure AD
- Der Name des Mieters Domäne (z. B. contoso.onmicrosoft.com)



## <a name="add-an-application-with-a-client-secret"></a>Hinzufügen einer Anwendung mit einem geheimen


1. Um Ihre Azure-Verwaltungsportal als Administrator [Anmelden](https://manage.windowsazure.com) . 

1. Klicken Sie im linken Navigationsbereich klicken Sie auf **Active Directory**. 

    ![Erstellen einer Anwendung](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_01.png)

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Klicken Sie im Menü oben auf **Applications**.

    ![Erstellen einer Anwendung](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_02.png)

4. Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Erstellen einer Anwendung](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_03.png)

5. Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **Mein Unternehmen entwickelten Anwendung hinzufügen**.

    ![Erstellen einer Anwendung](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_04.png)


5. Im Dialogfeld **Angaben über die Anwendung** der folgenden Schritte:

    ![Erstellen einer Anwendung](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_05.png)

    ein. Geben Sie in das Textfeld **Name** einen Namen für Ihre Anwendung (z. B.: AADIP Risiko Ereignis API-Anwendung).

    b. Wählen Sie als **Typ** **Web Anwendung oder Web-API**.

    c. Klicken Sie auf **Weiter**.


5. Gehen Sie im Dialogfeld **Anwendung** für:

    ![Erstellen einer Anwendung](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_06.png)

    ein. Geben Sie im Textfeld **Anmeldung URL** `http://localhost`.

    b. Geben Sie im Textfeld **App ID URI** `http://localhost`.

    c. Klicken Sie auf **abgeschlossen**.


Sie können jetzt Ihre Anwendung konfigurieren.

![Erstellen einer Anwendung](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_07.png)



## <a name="grant-your-application-permission-to-use-the-api"></a>Ihre Anwendung die Berechtigung der API


1. Die Anwendung im Menü auf der oberen klicken Sie auf **Konfigurieren**. 

    ![Erstellen einer Anwendung](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_08.png)

2. Klicken Sie im Abschnitt **Berechtigungen auf andere Programme** auf **Anwendung hinzufügen**.

    ![Erstellen einer Anwendung](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_09.png)


2. Klicken Sie im Dialogfeld **Berechtigungen für weitere** Schritte:

    ![Erstellen einer Anwendung](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_10.png)

    ein. Wählen Sie **Microsoft Graph**.

    b. Klicken Sie auf **abgeschlossen**.


1. Klicken Sie auf **Berechtigungen: 0**, und wählen Sie dann **Alle Identität Risiko Ereignisinformationen zu lesen**.

    ![Erstellen einer Anwendung](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_11.png)

1. Klicken Sie am unteren Rand der Seite **Speichern** .

    ![Erstellen einer Anwendung](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_12.png)


## <a name="get-an-access-key"></a>Abrufen einer Zugriffstaste

1. Wählen Sie auf der Anwendungsseite im Abschnitt **Schlüssel** 1 Jahr als Dauer.

    ![Erstellen einer Anwendung](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_13.png)

1. Klicken Sie am unteren Rand der Seite **Speichern** .

    ![Erstellen einer Anwendung](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_12.png)

1. Kopieren Sie im Abschnitt Schlüssel den Wert der neu erstellten Schlüssel, und fügen Sie ihn in einem sicheren Speicherort.

    ![Erstellen einer Anwendung](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_14.png)

    > [AZURE.NOTE] Wenn Sie diesen Schlüssel verlieren, müssen Sie in diesem Abschnitt und einen neuen Schlüssel zu erstellen. Dieser Schlüssel geheim halten: Wer es auf Ihre Daten zugreifen kann.


1. Kopieren Sie im Abschnitt **Eigenschaften** die **Client-ID**und fügen Sie ihn in einem sicheren Speicherort. 


## <a name="authenticate-to-microsoft-graph-and-query-the-identity-risk-events-api"></a>Microsoft Graph authentifizieren und Identität Risiko Ereignisse API Abfragen

An diesem Punkt sollten Sie haben:

- Die Client-ID oben kopiert

- Die Taste oben kopiert

- Der Name des Mieters Domäne


Senden Sie zur Authentifizierung eine Post-Anforderung `https://login.microsoft.com` mit den folgenden Parametern im Text:

- Grant_type: "**Client_credentials**"

- Ressource: "**https://graph.microsoft.com**"

- Client_id:<your client ID>

- Client_secret:<your key>


> [AZURE.NOTE] Sie müssen Werte für **Client_id** und **Client_secret** Parameter bereitzustellen.

Wenn erfolgreich, zurückgegeben ein Authentifizierungstoken.  
Um die API aufzurufen, erstellen Sie einen Header mit dem folgenden Parameter:

    `Authorization`=”<token_type> <access_token>"


Bei Tokentyp und Zugriffstoken finden Sie in der zurückgegebenen Token.

Senden Sie dieser Header als Anforderung an den folgenden API-URL:`https://graph.microsoft.com/beta/identityRiskEvents`

Die Antwort besteht bei Erfolg der Identität Risikoereignisse und zugeordneten Daten analysiert und behandelt wie OData JSON-Format.

Hier ist Beispielcode für die Authentifizierung und die Verwendung von Powershell-API aufrufen.  
Schlüssel und Mieter Domäne nur die Client-ID hinzufügen.

    $ClientID       = "<your client ID here>"        # Should be a ~36 hex character string; insert your info here
    $ClientSecret   = "<your client secret here>"    # Should be a ~44 character string; insert your info here
    $tenantdomain   = "<your tenant domain here>"    # For example, contoso.onmicrosoft.com

    $loginURL       = "https://login.microsoft.com"
    $resource       = "https://graph.microsoft.com"

    $body       = @{grant_type="client_credentials";resource=$resource;client_id=$ClientID;client_secret=$ClientSecret}
    $oauth      = Invoke-RestMethod -Method Post -Uri $loginURL/$tenantdomain/oauth2/token?api-version=1.0 -Body $body

    Write-Output $oauth

    if ($oauth.access_token -ne $null) {
        $headerParams = @{'Authorization'="$($oauth.token_type) $($oauth.access_token)"}

        $url = "https://graph.microsoft.com/beta/identityRiskEvents"
        Write-Output $url

        $myReport = (Invoke-WebRequest -UseBasicParsing -Headers $headerParams -Uri $url)

        foreach ($event in ($myReport.Content | ConvertFrom-Json).value) {
            Write-Output $event
        }

    } else {
        Write-Host "ERROR: No Access Token"
    } 


## <a name="next-steps"></a>Nächste Schritte

Herzlichen Glückwunsch, Sie nur der erste Aufruf von Microsoft Graph.  
Sie können jetzt Abfragen Risikoereignisse Identität und gewünscht Daten verwendet.

Erfahren Sie mehr über Microsoft Graph und Anwendung der Graph-API verwenden, überprüfen Sie [Dokumentation](https://graph.microsoft.io/docs) und vieles mehr auf der [Website Microsoft Graph](https://graph.microsoft.io/). Außerdem unbedingt die [Azure AD Schutz API](https://graph.microsoft.io/docs/api-reference/beta/resources/identityprotection_root) Lesezeichen, die Liste der Identität-Schutz-APIs im Diagramm. Wir neue Methoden zum Arbeiten mit über API-Schutz hinzufügen, wird auf der Seite angezeigt werden.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Azure Active Directory-Schutz](active-directory-identityprotection.md)

- [Ereignistypen Risiko von Azure Active Directory Schutz erkannt](active-directory-identityprotection-risk-events-types.md)

- [Microsoft Graph](https://graph.microsoft.io/)

- [Übersicht über Microsoft Graph](https://graph.microsoft.io/docs)

- [Azure AD-Identität Protection Service Stamm](https://graph.microsoft.io/docs/api-reference/beta/resources/identityprotection_root)
