<properties 
    pageTitle="Authentifizieren Sie mit mobilen REST-APIs"
    description="Beschreibt die Authentifizierung mit Azure Mobile Engagement REST-APIs" 
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo"
    manager="erikre"
    editor=""/>

<tags
    ms.service="mobile-engagement"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="mobile-multiple"
    ms.workload="mobile" 
    ms.date="10/05/2016"
    ms.author="wesmc;ricksal"/>

# <a name="authenticate-with-mobile-engagement-rest-apis"></a>Authentifizieren Sie mit mobilen REST-APIs

## <a name="overview"></a>Übersicht

Dieses Dokument beschreibt die gültigen AAD Oauth token zur Authentifizierung mit Mobile Engagement REST APIs abrufen. 

Es wird angenommen, Sie ein gültiges Azure-Abonnement haben und eine Mobile Engagement app eine [Developer Tutorials](mobile-engagement-windows-store-dotnet-get-started.md)erstellt haben.

## <a name="authentication"></a>Authentifizierung

Microsoft Azure Active Directory basierte OAuth-Token zur Authentifizierung verwendet wird. 

Um zu Authentifizierungsanfrage API muss einem Autorisierungsheader jeder Anforderung hinzugefügt werden die folgende Form:

    Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGmJlNmV2ZWJPamg2TTNXR1E...

>[AZURE.NOTE] Azure Active Directory Token läuft in 1 Stunde.

Es gibt mehrere Wege, ein Token abzurufen. Da die APIs im Allgemeinen von einem Clouddienst aufgerufen werden, einen API-Schlüssel verwendet werden soll. Ein API-Schlüssel in der Azure-Terminologie ist ein principal Kennwort aufgerufen. Das folgende Verfahren beschreibt eine Möglichkeit manuell einrichten.

### <a name="one-time-setup-using-script"></a>Einmalige Installation (mit Skripts)

Befolgen Sie die Gruppe von Anweisungen mithilfe eines PowerShell-Skripts Mindestzeit für Setup sondern verwendet die zulässige Standardwerte Setup ausführen. Optional können auch [manuelle Installation](mobile-engagement-api-authentication-manual.md) dazu von Azure-Portal direkt auszuführen und eine genauere Konfiguration. 

1. Erhalten Sie die neueste Version von Azure PowerShell [hier](http://aka.ms/webpi-azps). Für Weitere Informationen auf das Download sehen Sie diesen [Link](../powershell-install-configure.md).  

2. Nach Azure PowerShell installiert ist, verwenden Sie die folgenden Befehle, um sicherzustellen, dass der **Azure-Modul** installiert haben:

    ein. Stellen Sie sicher, dass Azure PowerShell-Modul in der Liste der verfügbaren Module verfügbar ist. 
    
        Get-Module –ListAvailable 

    ![Azure Module][1]
        
    b. Wenn Sie Azure PowerShell-Modul nicht in der obigen Liste finden müssen Sie Folgendes ausführen:
        
        Import-Module Azure 
        
3. Anmeldung an Azure-Ressourcen-Manager von PowerShell den folgenden Befehl ausführen und Ihren Benutzernamen und Ihr Kennwort für Ihre Azure-Konto: 
        
        Login-AzureRmAccount

4. Haben Sie mehrere Abonnements sollten Sie Folgendes ausführen:

    ein. Eine Liste aller Abonnements und kopieren SubscriptionId das Abonnement, das Sie verwenden möchten. Stellen Sie sicher, dass dieses Abonnement ist die Mobile Engagement App mit APIs interagieren soll. 

        Get-AzureRmSubscription

    b. Führen Sie den folgenden Befehl mit der SubscriptionId Abonnement verwendet werden sollen.

        Select-AzureRmSubscription –SubscriptionId <subscriptionId>

5. Kopieren Sie den Text für das [Neue AzureRmServicePrincipalOwner.ps1](https://raw.githubusercontent.com/matt-gibbs/azbits/master/src/New-AzureRmServicePrincipalOwner.ps1) -Skript auf dem lokalen Computer und speichern Sie diese als PowerShell-Cmdlet (z.B. `APIAuth.ps1`) und führen Sie sie `.\APIAuth.ps1`. 
    
6. Das Skript wird **PrincipalName**zu einer Eingabe aufgefordert. Geben Sie einem geeigneten Namen zum Erstellen einer Active Directory-Anwendung (z. B. APIAuth) verwenden möchten. 

7. Nach Abschluss des Skripts zeigt es die folgenden vier Werte, die Sie programmgesteuert mit authentifizieren müssen daher unbedingt kopieren. 
        
    **TenantId**, **SubscriptionId** **ApplicationId**und **Schlüssel**.

    Verwenden Sie TenantId als `{TENANT_ID}`, ApplicationId als `{CLIENT_ID}` und als `{CLIENT_SECRET}`.

    > [AZURE.NOTE] Die Standardsicherheitsrichtlinie blockiert Sie PowerShell-Skripts ausführen. In diesem Fall konfigurieren Sie vorübergehend Ihre Ausführungsrichtlinie um Skript Ausführung des folgenden Befehls:

        > Set-ExecutionPolicy RemoteSigned

8. Hier wird die Gruppe der PS-Cmdlets wie würde. 

    ![][3]

9. Überprüfen Sie im Azure-Verwaltungsportal, eine neue AD-Anwendung mit dem Namen erstellt wurde, zu dem Skript namens **PrincipalName** unter **Anzeigen Applikationen meine Firma besitzt**.

    ![][4]

#### <a name="steps-to-get-a-valid-token"></a>Schritte, um ein gültiges token

1. Rufen Sie die API mit den folgenden Parametern und PÄCHTER ersetzen\_-ID, CLIENT\_ID und CLIENT\_Schlüssel:

    - **Request-URL** als *https://login.microsoftonline.com/ {MIETER\_ID} / oauth2 Token*
    - **Http-Inhaltstyp - Header** *Application/X-www-form-urlencoded*
    - **HTTP-Anforderungstext** *erteilen\_Type = Client\_Anmeldedaten & Client_id = {CLIENT\_ID} & Client_secret = {CLIENT\_Schlüssel} & resource=https%3A%2F%2Fmanagement.core.windows.net%2F*

    Der folgende Code ist ein Beispiel für eine Anforderung:

        POST /{TENANT_ID}/oauth2/token HTTP/1.1
        Host: login.microsoftonline.com
        Content-Type: application/x-www-form-urlencoded
        grant_type=client_credentials&client_id={CLIENT_ID}&client_secret={CLIENT_SECRET}&reso
        urce=https%3A%2F%2Fmanagement.core.windows.net%2F

    Hier ist eine Beispielantwort:

        HTTP/1.1 200 OK
        Content-Type: application/json; charset=utf-8
        Content-Length: 1234
    
        {"token_type":"Bearer","expires_in":"3599","expires_on":"1445395811","not_before":"144
        5391911","resource":"https://management.core.windows.net/","access_token":{ACCESS_TOKEN}}

    In diesem Beispiel enthaltenen URL-Codierung von der POST-Parameter `resource` Wert ist `https://management.core.windows.net/`. Achten Sie auch URL codieren `{CLIENT_SECRET}` Sonderzeichen enthalten kann.

    > [AZURE.NOTE] Zu Testzwecken können Sie ein HTTP-Clienttool wie [Fiddler](http://www.telerik.com/fiddler) oder [Chrom Postbote Erweiterung](https://chrome.google.com/webstore/detail/postman/fhbjgbiflinjbdggehcddcbncdddomop) 

2. Jetzt in jedem API-Aufruf gehören Sie Anforderung Autorisierungsheader:

        Authorization: Bearer {ACCESS_TOKEN}

    Wenn Statuscode 401 zurückgegeben, prüfen Sie den Antworttext es kann Ihnen sagen, das Token abgelaufen ist. In diesem Fall erhalten Sie ein neues Token.

##<a name="using-the-apis"></a>Mithilfe der APIs

Jetzt haben Sie ein gültiges Token können Sie die API-Aufrufe.

1. In jeder Anforderung API müssen Sie eine gültige, nicht abgelaufene Token Sie erhalten im vorherigen Abschnitt übergeben.

2. Sie müssen einige Parameter in der Anfrage-URI anschließen die Anwendung identifiziert. Die Anforderung sieht der URI wie folgt

        https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/
        providers/Microsoft.MobileEngagement/appcollections/{app-collection}/apps/{app-resource-name}/

    Um den Parameter und dann auf auf dem Anwendungsnamen Dashboard und sehen eine Seite wie folgt mit 3 Parameter.

    - **1**`{subscription-id}`
    - **2**`{app-collection}`
    - **3**`{app-resource-name}`
    - **4** der Ressourcengruppe Name wird **MobileEngagement** , wenn Sie eine neue Datei erstellt. 

    ![Mobile Engagement API-URI-Parameter][2]

>[AZURE.NOTE] <br/>
>1. Ignorieren Sie API-Stamm Adresse, wie dies bei früheren APIs.<br/>
>2. Wenn Sie die app Azure-Verwaltungsportal erstellt müssen Sie Anwendung Ressourcennamen verwenden, der den Namen der Anwendung unterscheidet. Wenn Sie die Anwendung in Azure-Portal erstellt sollten Sie Anwendungsname selbst verwenden (es gibt keine Unterscheidung zwischen Anwendung Ressourcennamen und Anwendungsname für apps in das neue Portal erstellt).  

<!-- Images -->
[1]: ./media/mobile-engagement-api-authentication/azure-module.png
[2]: ./media/mobile-engagement-api-authentication/mobile-engagement-api-uri-params.png
[3]: ./media/mobile-engagement-api-authentication/ps-cmdlets.png
[4]: ./media/mobile-engagement-api-authentication/ad-app-creation.png



