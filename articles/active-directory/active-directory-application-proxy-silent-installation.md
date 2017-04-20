<properties
    pageTitle="Azure AD Application Proxy Connector automatisch installieren | Microsoft Azure"
    description="Beschreibt das Ausführen einer unbeaufsichtigten Installations von Azure AD Application Proxy Connector sicheren Remotezugriff auf Ihre lokalen apps."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/22/2016"
    ms.author="kgremban"/>

# <a name="how-to-silently-install-the-azure-ad-application-proxy-connector"></a>Azure AD Application Proxy Connector automatisch installieren

Sie möchten können ein Installationsskript für mehrere Windows-Server oder Windows-Servern, die Benutzeroberfläche aktiviert senden. Dieses Thema erläutert, wie ein Windows PowerShell-Skript zu erstellen, können unbeaufsichtigte Installation installieren und registrieren Ihre Azure AD Application Proxy Connector.

## <a name="enabling-access"></a>Zugang
Anwendungsproxy kann einen schlanken Windows Server Dienst namens Connector in Ihrem Netzwerk installieren. Application Proxy Connector funktioniert hat mit Ihrem Azure AD-Verzeichnis mit den globalen Administrator registriert werden. Dies wird normalerweise während der Installation des Connectors in einem Popup-Dialogfeld eingegeben. Alternativ können Sie Windows PowerShell Anmeldeinformationsobjekt Geben Sie Ihre Registrierungsinformationen erstellen oder Erstellen eigener Token und verwenden Ihre Anmeldedaten eingeben.

## <a name="step-1--install-the-connector-without-registration"></a>Schritt 1: Installieren des Connectors ohne Registrierung


Connector-MSIs ohne den Connector wie folgt installieren:


1. Öffnen Sie ein Eingabeaufforderungsfenster.
2. Führen Sie folgenden Befehl/q Unbeaufsichtigte Installation - bedeutet wird die Installation nicht an den Endbenutzer-Lizenzvertrag aufgefordert.

        AADApplicationProxyConnectorInstaller.exe REGISTERCONNECTOR="false" /q

## <a name="step-2-register-the-connector-with-azure-active-directory"></a>Schritt 2: Registrieren des Connectors in Azure Active Directory
Dies kann erfolgen über eine der folgenden Methoden:


- Registrieren Sie mithilfe einer Windows PowerShell Anmeldeinformationsobjekt
- Mit einem Token offline erstellte Connector registrieren

### <a name="register-the-connector-using-a-windows-powershell-credential-object"></a>Registrieren Sie mithilfe einer Windows PowerShell Anmeldeinformationsobjekt


1. Windows PowerShell Anmeldeinformationsobjekt durch Ausführen der Folgendes erstellen, "<username>"und"<password>" sollte mit den Benutzernamen und das Kennwort für das Verzeichnis ersetzt werden:

        $User = "<username>"
        $PlainPassword = '<password>'
        $SecurePassword = $PlainPassword | ConvertTo-SecureString -AsPlainText -Force
        $cred = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $User, $SecurePassword

2. Wechseln Sie zu **C:\Program Files\Microsoft AAD App Proxy Connector** und Skript erstellten Objekt mit PowerShell Anmeldeinformationen verwenden, ist der Name des PowerShell durch $cred erstellte Objekt Anmeldeinformationen:

        RegisterConnector.ps1 -modulePath "C:\Program Files\Microsoft AAD App Proxy Connector\Modules\" -moduleName "AppProxyPSModule" -Authenticationmode Credentials -Usercredentials $cred


### <a name="register-the-connector-using-a-token-created-offline"></a>Mit einem Token offline erstellte Connector registrieren

1. Erstellen Sie einen offline-Token mithilfe der AuthenticationContext-Klasse unter Verwendung der Werte im Codeausschnitt:


        using System;
        using System.Diagnostics;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;

        class Program
        {
        #region constants
        /// <summary>
        /// The AAD authentication endpoint uri
        /// </summary>
        static readonly Uri AadAuthenticationEndpoint = new Uri("https://login.windows.net/common/oauth2/token?api-version=1.0");

        /// <summary>
        /// The application ID of the connector in AAD
        /// </summary>
        static readonly string ConnectorAppId = "55747057-9b5d-4bd4-b387-abf52a8bd489";

        /// <summary>
        /// The reply address of the connector application in AAD
        /// </summary>
        static readonly Uri ConnectorRedirectAddress = new Uri("urn:ietf:wg:oauth:2.0:oob");

        /// <summary>
        /// The AppIdUri of the registration service in AAD
        /// </summary>
        static readonly Uri RegistrationServiceAppIdUri = new Uri("https://proxy.cloudwebappproxy.net/registerapp");

        #endregion

        #region private members
        private string token;
        private string tenantID;
        #endregion

        public void GetAuthenticationToken()
        {
            AuthenticationContext authContext = new AuthenticationContext(AadAuthenticationEndpoint.AbsoluteUri);

            AuthenticationResult authResult = authContext.AcquireToken(RegistrationServiceAppIdUri.AbsoluteUri,
                ConnectorAppId,
                ConnectorRedirectAddress,
                PromptBehavior.Always);

            if (authResult == null || string.IsNullOrEmpty(authResult.AccessToken) || string.IsNullOrEmpty(authResult.TenantId))
            {
                Trace.TraceError("Authentication result, token or tenant id returned are null");
                throw new InvalidOperationException("Authentication result, token or tenant id returned are null");
            }

            token = authResult.AccessToken;
            tenantID = authResult.TenantId;
        }





2. Haben Sie das Token mit dem Token SecureString erstellen: <br>
`$SecureToken = $Token | ConvertTo-SecureString -AsPlainText -Force`
3. Führen Sie den folgenden Windows PowerShell-Befehl, SecureToken den Namen des zuvor erstellten Tokens und TenantID des Mieters GUID: <br>
`RegisterConnector.ps1 -modulePath "C:\Program Files\Microsoft AAD App Proxy Connector\Modules\" -moduleName "AppProxyPSModule" -Authenticationmode Token -Token $SecureToken -TenantId <tenant GUID>`



## <a name="see-also"></a>Siehe auch

- [Anwendungsproxy für Azure Active Directory aktivieren](active-directory-application-proxy-enable.md)
- [Veröffentlichen Sie mit Ihren eigenen Domänennamen Applikationen](active-directory-application-proxy-custom-domains.md)
- [Einmalige Anmeldung aktivieren](active-directory-application-proxy-sso-using-kcd.md)
- [Problembehandlung, mit Anwendungsproxy](active-directory-application-proxy-troubleshoot.md)

Überprüfen Sie die aktuellsten Neuigkeiten und Updates [Anwendungsproxy blog](http://blogs.technet.com/b/applicationproxyblog/)
