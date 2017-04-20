<properties
    pageTitle="Verwenden von Azure Key Vault from a Web Application | Microsoft Azure"
    description="Mithilfe dieses Lernprogramm erfahren Sie, wie eine Anwendung Azure Key Vault verwenden."
    services="key-vault"
    documentationCenter=""
    authors="adhurwit"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/05/2016"
    ms.author="adhurwit"/>

# <a name="use-azure-key-vault-from-a-web-application"></a>Verwenden von Azure Key Vault from a Web Application #

## <a name="introduction"></a>Einführung  
Verwenden Sie dieses Lernprogramm zu wie Azure Key Vault von einer Webanwendung in Azure verwendet. Er führt Sie durch den Prozess auf einen Schlüssel von einem Azure Key Vault, damit sie in der Webanwendung verwendet werden kann.

**Geschätzte Dauer:** 15 Minuten


Übersicht über Azure Key Vault finden Sie unter [Neuigkeiten Azure Key Vault?](key-vault-whatis.md)

## <a name="prerequisites"></a>Erforderliche Komponenten

Um dieses Lernprogramm benötigen Sie Folgendes:

- Ein URI ein Geheimnis einer Azure Key Vault
- Eine Client-ID und einem geheimen für eine Anwendung registriert Azure Active Directory, die Zugriff auf den Schlüsseltresor
- Eine Webanwendung. Wir zeigen die Schritte für eine ASP.NET MVC-Anwendung in Azure Web App bereitgestellt.

> [AZURE.NOTE]  Es ist wichtig, Sie Schritte in [Erste Schritte mit Azure Schlüssel](key-vault-get-started.md) für dieses Lernprogramm, damit den URI einen geheimen Schlüssel und die Client-ID und geheimen für eine Anwendung werden abgeschlossen.

Web-Anwendung, die das Depot Schlüssel zugreifen ist die in Azure Active Directory registriert ist und hat Zugriff auf Ihrem Tresor Schlüssel gewährt wurde. Ist dies nicht der Fall, zurück zum Registrieren einer Anwendung im Lernprogramm beginnen, und wiederholen Sie die Schritte aufgeführt.

Dieses Lernprogramm soll für Webentwickler, die die Grundlagen der Erstellung von ASP.NET-Webanwendungen auf Azure verstehen. Weitere Informationen zu Azure Web Apps Übersicht [Web Apps](../app-service-web/app-service-web-overview.md).



## <a id="packages"></a>Hinzufügen von Nuget-Paketen ##
Es gibt zwei Pakete, die die Webanwendung installiert.

- Active Directory-Authentifizierung Library - enthält Methoden für die Interaktion mit Azure Active Directory und Verwalten von Benutzeridentität
- Azure enthält Key Vault - Methoden für die Interaktion mit Azure Schlüssel


Beide dieser Pakete können mithilfe der Paket-Manager-Konsole mit dem Befehl Install-Package installiert.

    // this is currently the latest stable version of ADAL
    Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.16.204221202

    Install-Package Microsoft.Azure.KeyVault


## <a id="webconfig"></a>Web.Config ändern ##
Es gibt drei Einstellungen, die in der Datei web.config eingefügt werden müssen.

    <!-- ClientId and ClientSecret refer to the web application registration with Azure Active Directory -->
    <add key="ClientId" value="clientid" />
    <add key="ClientSecret" value="clientsecret" />

    <!-- SecretUri is the URI for the secret in Azure Key Vault -->
    <add key="SecretUri" value="secreturi" />


Wenn Sie nicht zum Hosten der Anwendung als Azure Web App, sollten Sie die tatsächlichen ClientId und geheimen Schlüssel-URI-Werte der Web.config-Datei hinzufügen. Andernfalls lassen Sie diese dummy-Werte da wir die tatsächlichen Werten im Azure-Portal für eine zusätzliche Sicherheitsebene hinzugefügt.


## <a id="gettoken"></a>Add-Methode, um ein Token zuzugreifen ##
Der Schlüssel Vault API benötigen Sie ein Zugriffstoken. Key Vault Client übernimmt die Schlüssel Vault-API aufgerufen, aber Sie müssen mit einer Funktion angeben, die das Zugriffstoken wird.  

Folgendes ist der Code ein token von Azure Active Directory zugreifen. Dieser Code kann überall in der Anwendung. Hinzufügen einer Klasse Utils oder EncryptionHelper möchte.  

    //add these using statements
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using System.Threading.Tasks;
    using System.Web.Configuration;

    //this is an optional property to hold the secret after it is retrieved
    public static string EncryptSecret { get; set; }

    //the method that will be provided to the KeyVaultClient
    public static async Task<string> GetToken(string authority, string resource, string scope)
    {
        var authContext = new AuthenticationContext(authority);
        ClientCredential clientCred = new ClientCredential(WebConfigurationManager.AppSettings["ClientId"],
                    WebConfigurationManager.AppSettings["ClientSecret"]);
        AuthenticationResult result = await authContext.AcquireTokenAsync(resource, clientCred);

        if (result == null)
            throw new InvalidOperationException("Failed to obtain the JWT token");

        return result.AccessToken;
    }

> [AZURE.NOTE] 
> Mit Client Clientschlüssel ist die einfachste Möglichkeit zur Authentifizierung einer Azure AD-Anwendung. Und Ihre Web-Anwendung ermöglicht die Trennung von Aufgaben und mehr Kontrolle über Ihre Schlüsselmanagement. Doch es beruht auf den Client-Schlüssel in der Konfiguration die für einige so gefährlich wie das Geheimnis, das Sie in Ihrer Konfiguration schützen möchten. Nachfolgend finden Sie eine Diskussion über eine Client-ID und Zertifikat Client-ID und geheimen anstelle die Azure AD-Anwendung zu authentifizieren.



## <a id="appstart"></a>Rufen Sie das Geheimnis auf Starten ##
Nun brauchen wir Code Key Vault-API und rufen Sie das Geheimnis. Im folgende Code an einer beliebigen Stelle setzen wie heißt, bevor Sie sie verwenden müssen. Dieser Code habe im Ereignis starten Global.asax ich einmal beim Start ausgeführt wird und den Schlüssel für die Anwendung verfügbar macht.

    //add these using statements
    using Microsoft.Azure.KeyVault;
    using System.Web.Configuration;

    // I put my GetToken method in a Utils class. Change for wherever you placed your method.
    var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetToken));

    var sec = kv.GetSecretAsync(WebConfigurationManager.AppSettings["SecretUri"]).Result.Value;

    //I put a variable in a Utils class to hold the secret for general  application use.
    Utils.EncryptSecret = sec;



## <a id="portalsettings"></a>Hinzufügen von App-Einstellungen im Azure-Portal (optional) ##
Azure Web Apps können Sie die tatsächlichen Werte jetzt für AppSettings in Azure-Portal hinzufügen. Damit die tatsächlichen Werte werden nicht in der Datei web.config jedoch Portal Sie Steuerungsfunktionen in separaten Zugriff müssen geschützt. Diese Werte werden für die Werte ersetzt, die in web.config eingegeben. Stellen Sie sicher, dass die Namen übereinstimmen.

![ApplicationSettings in Azure-Portal angezeigt][1]


## <a name="authenticate-with-a-certificate-instead-of-a-client-secret"></a>Authentifizierung mit einem Zertifikat anstelle eines geheimen
Eine andere Möglichkeit zur Authentifizierung einer Anwendung Azure AD wird anstelle einer Client-ID und ein Zertifikat eine Client-ID und geheimen. Sind die Schritte ein Zertifikat in ein Azure Web App verwenden:

1. Abrufen oder Erstellen eines Zertifikats
2. Ordnen Sie das Zertifikat mit einer Azure AD-Anwendung
3. Fügen Sie Code zu Ihrer Anwendung das Zertifikat
4. Ihrer Anwendung ein Zertifikat hinzufügen


**Abrufen oder Erstellen eines Zertifikats** Für unsere Zwecke stellen wir ein Testzertifikat. Hier sind einige Befehle, mit denen Sie eine Befehlszeile Entwickler ein Zertifikat erstellen. Wechseln Sie zu dem Zertifikat Dateien erstellt werden sollen.

    makecert -sv mykey.pvk -n "cn=KVWebApp" KVWebApp.cer -b 07/31/2015 -e 07/31/2016 -r
    pvk2pfx -pvk mykey.pvk -spc KVWebApp.cer -pfx KVWebApp.pfx -po test123

Notieren Sie das Enddatum und das Kennwort für die PFX-Datei (in diesem Beispiel: 31/07/2016 und test123). Diese werden unten benötigt.

Weitere Informationen zum Erstellen eines Testzertifikats finden Sie [wie: Ihr eigenes Testzertifikat erstellen](https://msdn.microsoft.com/library/ff699202.aspx)


**Ordnen Sie das Zertifikat mit einer Azure AD-Anwendung** Ein Zertifikat verfügen, müssen Sie eine Azure AD-Anwendung zuordnen. Azure-Verwaltungsportal unterstützt jedoch nicht das jetzt. Stattdessen müssen Sie Powershell verwenden. Es folgen die Befehle, die ausgeführt werden müssen:

    $x509 = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2

    PS C:\> $x509.Import("C:\data\KVWebApp.cer")

    PS C:\> $credValue = [System.Convert]::ToBase64String($x509.GetRawCertData())

    PS C:\> $now = [System.DateTime]::Now

    # this is where the end date from the cert above is used
    PS C:\> $yearfromnow = [System.DateTime]::Parse("2016-07-31")

    PS C:\> $adapp = New-AzureRmADApplication -DisplayName "KVWebApp" -HomePage "http://kvwebapp" -IdentifierUris "http://kvwebapp" -KeyValue $credValue -KeyType "AsymmetricX509Cert" -KeyUsage "Verify" -StartDate $now -EndDate $yearfromnow

    PS C:\> $sp = New-AzureRmADServicePrincipal -ApplicationId $adapp.ApplicationId

    PS C:\> Set-AzureRmKeyVaultAccessPolicy -VaultName 'contosokv' -ServicePrincipalName $sp.ServicePrincipalName -PermissionsToSecrets all -ResourceGroupName 'contosorg'

    # get the thumbprint to use in your app settings
    PS C:\>$x509.Thumbprint

Nach dem Ausführen dieser Befehle können Sie die Anwendung in Azure AD sehen. Wenn Sie nicht die Anwendung zuerst suchen "Applications Mein Unternehmen" statt "verwendet Applikationen meine Firma".

Mehr über Azure AD-Anwendung und ServicePrincipal Objekte finden Sie unter [Anwendung und Service Principal-Objekte](../active-directory/active-directory-application-objects.md)



**Code Ihrer Anwendung das Zertifikat hinzufügen** Jetzt fügen wir Code zu Ihrer Anwendung zu das Zertifikat zur Authentifizierung verwenden.

Zunächst wird Code auf das Zertifikat zugreifen.

    public static class CertificateHelper
    {
        public static X509Certificate2 FindCertificateByThumbprint(string findValue)
        {
            X509Store store = new X509Store(StoreName.My, StoreLocation.CurrentUser);
            try
            {
                store.Open(OpenFlags.ReadOnly);
                X509Certificate2Collection col = store.Certificates.Find(X509FindType.FindByThumbprint,
                    findValue, false); // Don't validate certs, since the test root isn't installed.
                if (col == null || col.Count == 0)
                    return null;
                return col[0];
            }
            finally
            {
                store.Close();
            }
        }
    }


Beachten Sie, dass StoreLocation CurrentUser statt LocalMachine. Und false der Find-Methode liefern wir da wir ein Test-Zertifikat verwenden.


Weiter ist Code, der die CertificateHelper verwendet und erstellt eine ClientAssertionCertificate für die Authentifizierung benötigt wird.

    public static ClientAssertionCertificate AssertionCert { get; set; }

    public static void GetCert()
    {
        var clientAssertionCertPfx = CertificateHelper.FindCertificateByThumbprint(WebConfigurationManager.AppSettings["thumbprint"]);
        AssertionCert = new ClientAssertionCertificate(WebConfigurationManager.AppSettings["clientid"], clientAssertionCertPfx);
    }


Hier ist der neue Code Zugriffstoken abrufen. Dies ersetzt die obige GetToken-Methode. Sie haben einen anderen Namen für Komfort mir.

    public static async Task<string> GetAccessToken(string authority, string resource, string scope)
    {
        var context = new AuthenticationContext(authority, TokenCache.DefaultShared);
        var result = await context.AcquireTokenAsync(resource, AssertionCert);
        return result.AccessToken;
    }

Ich habe diesen Code in meinen Project Web App-Utils-Klasse für einfache Bedienung.

Die letzte Änderung des Codes ist in der Application_Start-Methode. Zuerst muss der GetCert()-Methode der ClientAssertionCertificate geladen werden. Und wir ändern die Rückrufmethode, die wir beim Erstellen einer neuen KeyVaultClient bereitstellen. Beachten Sie, dass dies der Code, die wir ersetzt über.

    Utils.GetCert();
    var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetAccessToken));


**Hinzufügen eines Zertifikats zu Ihrer Anwendung über Azure-Portal** Hinzufügen eines Zertifikats zu Ihrer Anwendung ist eine einfachen Schritten. Zunächst zum Azure-Portal, und navigieren Sie zu Ihrer Anwendung. Blatt Einstellungen für Ihre Web-App klicken Sie auf den Eintrag für "Custom Domains und SSL". Auf das Blatt, das geöffnet wird werden von oben KVWebApp.pfx erstellten Zertifikat laden, stellen Sie sicher, dass das Kennwort für die Pfx.

![Hinzufügen eines Zertifikats zum Web App im Azure-Portal][2]


Zuletzt Sie müssen Ihrer Anwendung eine Anwendungseinstellung hinzu, die den WEBSITE ist\_laden\_Zertifikate und der Wert *. Dadurch wird sichergestellt, dass alle Zertifikate geladen werden. Wenn Sie die Zertifikate zu laden, die Sie hochgeladen haben, können Sie eine durch Kommas getrennte Liste der ihre Fingerabdrücke eingeben.

Informationen zum Hinzufügen eines Zertifikats zum Web App finden Sie unter [Zertifikate in Azure Websites Applikationen verwenden](https://azure.microsoft.com/blog/2014/10/27/using-certificates-in-azure-websites-applications/)


**Hinzufügen eines Zertifikats, das Depot Schlüssel geheim** Anstatt das Zertifikat direkt an den Dienst Web App hochladen können im Tresor Schlüssel geheim gespeichert und dort bereitstellen. Dies ist im folgenden Blogbeitrag [Azure Web App Zertifikat bereitstellen, durch Schlüssel Depot](https://blogs.msdn.microsoft.com/appserviceteam/2016/05/24/deploying-azure-web-app-certificate-through-key-vault/) beschriebenen Schritten



## <a id="next"></a>Nächste Schritte ##


Programmierung Verweise finden Sie unter [Azure Key Vault C# Client-API-Referenz](https://msdn.microsoft.com/library/azure/dn903628.aspx).


<!--Image references-->
[1]: ./media/key-vault-use-from-web-application/PortalAppSettings.png
[2]: ./media/key-vault-use-from-web-application/PortalAddCertificate.png
