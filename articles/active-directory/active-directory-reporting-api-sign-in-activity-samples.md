<properties
    pageTitle="Azure Active Directory Anmeldeaktivität API Beispielberichte | Microsoft Azure"
    description="Erste Schritte mit der Azure Active Directory Reporting-API"
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

# <a name="azure-active-directory-sign-in-activity-report-api-samples"></a>Azure Active Directory Anmeldeaktivität API Beispielberichte

Dieses Thema ist Teil einer Sammlung von Themen zu Azure Active Directory reporting API.  
Azure AD reporting bietet eine API, die Zeichen in Daten mit Code oder verwandte Tools zugreifen kann.  
Dieses Thema soll mit Beispielcode **Anmeldeaktivität API**bereitstellen.

Sieh:

- [Überwachungsprotokolle](active-directory-reporting-azure-portal.md#audit-logs) konzeptionelle Informationen
- [Erste Schritte mit der Azure Active Directory Reporting-API](active-directory-reporting-api-getting-started.md) Weitere Informationen die reporting-API.

Fragen, Fragen oder Feedback wenden Sie [AAD Reporting helfen](mailto:aadreportinghelp@microsoft.com).


## <a name="prerequisites"></a>Erforderliche Komponenten
Bevor Sie die Beispiele in diesem Thema verwenden können, müssen Sie [auf Azure AD reporting-API-Komponenten](active-directory-reporting-api-prerequisites.md)abzuschließen.  


##<a name="powershell-script"></a>PowerShell-Skript

    # This script will require the Web Application and permissions setup in Azure Active Directory
    $ClientID       = "<clientId>"             # Should be a ~35 character string insert your info here
    $ClientSecret   = "<clientSecret>"         # Should be a ~44 character string insert your info here
    $loginURL       = "https://login.windows.net/"
    $tenantdomain   = "<tenantDomain>"
    $ daterange            # For example, contoso.onmicrosoft.com

    $7daysago = "{0:s}" -f (get-date).AddDays(-7) + "Z"
    # or, AddMinutes(-5)

    Write-Output $7daysago

    # Get an Oauth 2 access token based on client id, secret and tenant domain
    $body       = @{grant_type="client_credentials";resource=$resource;client_id=$ClientID;client_secret=$ClientSecret}

    $oauth      = Invoke-RestMethod -Method Post -Uri $loginURL/$tenantdomain/oauth2/token?api-version=1.0 -Body $body

    if ($oauth.access_token -ne $null) {
    $headerParams = @{'Authorization'="$($oauth.token_type) $($oauth.access_token)"}

    $url = "https://graph.windows.net/$tenantdomain/activities/signinEvents?api-version=beta&`$filter=signinDateTime ge $7daysago"
    
    $i=0
    
    Do{
        Write-Output "Fetching data using Uri: $url"
        $myReport = (Invoke-WebRequest -UseBasicParsing -Headers $headerParams -Uri $url)
        Write-Output "Save the output to a file SigninActivities$i.json"
        Write-Output "---------------------------------------------"
        $myReport.Content | Out-File -FilePath SigninActivities$i.json -Force
        $url = ($myReport.Content | ConvertFrom-Json).'@odata.nextLink'
        $i = $i+1
    } while($url -ne $null)

    } else {
    
        Write-Host "ERROR: No Access Token"
    }




## <a name="executing-the-script"></a>Ausführen des Skripts
Nach Bearbeitung des Skripts, ausführen und überprüfen, ob die erwarteten Daten aus dem Bericht protokolliert werden zurückgegeben.

Das Skript gibt die Ausgabe aus dem Bericht Anmelden im JSON-Format zurück. Erstellt ein `SigninActivities.json` mit der gleichen Datei. Sie können experimentieren, ändern Sie das Skript, um Berichte und kommentieren Sie die Ausgabeformate müssen keine Daten zurückgeben.



## <a name="next-steps"></a>Nächste Schritte

- Möchten Sie die Beispiele in diesem Thema anpassen? Checken Sie die [Azure Active Directory Anmeldeaktivität-API-Referenz](active-directory-reporting-api-sign-in-activity-reference.md). 

- Finden eine vollständige Übersicht auf reporting API Azure Active Directory finden Sie in der [API Einstieg in Azure Active Directory reporting](active-directory-reporting-api-getting-started.md).

- Wenn Sie Active Directory Azure reporting erfahren möchten, finden Sie unter [Azure Active Directory-Berichte-Handbuch](active-directory-reporting-guide.md).  