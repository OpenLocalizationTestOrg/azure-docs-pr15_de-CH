<properties
   pageTitle="Daten See Speicher .NET SDK zur Anwendungsentwicklung verwenden | Microsoft Azure"
   description="Verwenden Sie Azure Data Lake Speicher .NET SDK zur Anwendungsentwicklung"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/27/2016"
   ms.author="nitinme"/>

# <a name="get-started-with-azure-data-lake-store-using-net-sdk"></a>Erste Schritte mit Azure See Datenspeicher mit .NET SDK

> [AZURE.SELECTOR]
- [Portal](data-lake-store-get-started-portal.md)
- [PowerShell](data-lake-store-get-started-powershell.md)
- [.NET SDK](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [REST-API](data-lake-store-get-started-rest-api.md)
- [Azure CLI](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)

Informationen Sie zum [Azure Data Lake Speicher .NET SDK](https://msdn.microsoft.com/library/mt581387.aspx) verwenden grundlegende Operationen ausführen, die z. b. Ordner erstellen hoch- und Herunterladen von Dateien usw. Weitere Informationen zu Data Lake finden Sie unter [Azure See Datenspeicher](data-lake-store-overview.md).

## <a name="prerequisites"></a>Erforderliche Komponenten

* **Visual Studio 2013 oder 2015**. Hinweise verwenden Visual Studio 2015.

* **Ein Azure-Abonnement**. Finden Sie [kostenlose Testversion von Azure zu erhalten](https://azure.microsoft.com/pricing/free-trial/).

* **See Datenspeicher Azure-Konto**. Informationen zum Erstellen eines Kontos finden Sie unter [Erste Schritte mit Azure See Datenspeicher](data-lake-store-get-started-portal.md)

* **Erstellen einer Anwendung Azure Active Directory**. Mithilfe der Azure AD-Anwendung See Datenspeicher Anwendung in Azure AD authentifizieren. Es gibt verschiedene Ansätze bei Azure AD authentifizieren die **Endbenutzer** oder **Dienst - Authentifizierung**. Anleitung und Weitere Informationen zur Authentifizierung finden Sie unter [Authentifizierung mit dem Datenspeicher Azure Active Directory verwenden](data-lake-store-authenticate-using-active-directory.md).

## <a name="create-a-net-application"></a>Erstellen Sie eine

1. Öffnen Sie Visual Studio, und erstellen Sie eine.

2. Klicken Sie im Menü **Datei** auf **neu**und klicken Sie auf **Projekt**.

3. **Neues Projekt**Geben Sie ein oder wählen Sie die folgenden Werte aus:

  	| Eigenschaft | Wert                       |
  	|----------|-----------------------------|
  	| Kategorie | Vorlagen/Visual C# / Windows |
  	| Vorlage | Konsolenanwendungsprojekt         |
  	| Name     | CreateADLApplication        |

4. Klicken Sie auf **OK** , um das Projekt zu erstellen.

5. Nuget-Pakete zum Projekt hinzufügen

    1. Mit der rechten Maustaste im Projektmappen-Explorer des Projektnamen, und klicken Sie auf **NuGet-Pakete verwalten**.
    2. Auf der Registerkarte **Nuget Paket-Manager** unbedingt **Paketquelle** auf **nuget.org** und diese **Vorabversion einschließen** aktiviert ist.
    3. Suchen Sie und installieren Sie die folgenden NuGet-Pakete:

        * `Microsoft.Azure.Management.DataLake.Store`-Dieses Lernprogramms v0.12.5 Vorschau.
        * `Microsoft.Azure.Management.DataLake.StoreUploader`-Dieses Lernprogramms v0.10.6 Vorschau.
        * `Microsoft.Rest.ClientRuntime.Azure.Authentication`-Dieses Lernprogramms v2.2.8 Vorschau.

        ![Hinzufügen einer Nuget-Quelle] (./media/data-lake-store-get-started-net-sdk/ADL.Install.Nuget.Package.png "Erstellen eines neuen Kontos Azure Data Lake")

    4. Schließen Sie die **Nuget-Paket-Manager**.

6. Öffnen Sie **Program.cs**, löschen Sie den vorhandenen Code und enthalten Sie die folgenden Aussagen Verweise auf Namespaces hinzuzufügen.

        using System;
        using System.Threading;
        
        using Microsoft.Rest.Azure.Authentication;
        using Microsoft.Azure.Management.DataLake.Store;
        using Microsoft.Azure.Management.DataLake.StoreUploader;

7. Deklarieren Sie die Variablen wie folgt, und geben Sie die Werte für Datenspeicher See und Ressource-Gruppennamen, die bereits vorhanden. Stellen Sie außerdem sicher, dass der lokale Pfad und der Dateiname, die Sie hier auf dem Computer vorhanden sein muss. Fügen Sie den folgenden Codeausschnitt nach Namespacedeklarationen.

        namespace SdkSample
        {
            class Program
            {
                private static DataLakeStoreAccountManagementClient _adlsClient;
                private static DataLakeStoreFileSystemManagementClient _adlsFileSystemClient;
                
                private static string _adlsAccountName;
                private static string _resourceGroupName;
                private static string _location;
                private static string _subId;

                
                private static void Main(string[] args)
                {
                    _adlsAccountName = "<DATA-LAKE-STORE-NAME>"; // TODO: Replace this value with the name of your existing Data Lake Store account.
                    _resourceGroupName = "<RESOURCE-GROUP-NAME>"; // TODO: Replace this value with the name of the resource group containing your Data Lake Store account.
                    _location = "East US 2";
                    _subId = "<SUBSCRIPTION-ID>";
                    
                    string localFolderPath = @"C:\local_path\"; // TODO: Make sure this exists and can be overwritten.
                    string localFilePath = localFolderPath + "file.txt"; // TODO: Make sure this exists and can be overwritten.
                    string remoteFolderPath = "/data_lake_path/";
                    string remoteFilePath = remoteFolderPath + "file.txt";
                }
            }
        }

In den restlichen Abschnitten dieses Artikels sehen Sie, wie Sie die verfügbaren Methoden auf .NET Operationen wie Authentifizierung, Datei-Upload.

## <a name="authentication"></a>Authentifizierung

### <a name="if-you-are-using-end-user-authentication-recommended-for-this-tutorial"></a>Bei Verwendung von Anwender-Authentifizierung (empfohlen für dieses Lernprogramm)

Verwenden Sie diese mit einer vorhandenen Azure AD "Native Client" Anwendung; eine wird unten bereitgestellt. Um dieses Lernprogramm schneller zu erleichtern, empfehlen wir diesen Ansatz verwenden.

    // User login via interactive popup
    // Use the client ID of an existing AAD "Native Client" application.
    SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());
    var domain = "common"; // Replace this string with the user's Azure Active Directory tenant ID or domain name, if needed.
    var nativeClientApp_clientId = "1950a258-227b-4e31-a9cf-717495945fc2";
    var activeDirectoryClientSettings = ActiveDirectoryClientSettings.UsePromptOnly(nativeClientApp_clientId, new Uri("urn:ietf:wg:oauth:2.0:oob"));
    var creds = UserTokenProvider.LoginWithPromptAsync(domain, activeDirectoryClientSettings).Result;

Einige Aspekte dieser Codeausschnitt oben kennen.

* Um das Lernprogramm schneller abzuschließen, dieser Ausschnitt verwendet ein Azure AD Domain und Client-ID für alle Azure-Abonnements zur Verfügung. So können **mithilfe dieser Ausschnitt als-Ihre Anwendung**.
* Allerdings möchten Sie eigene Azure AD-Domänen und Client-ID verwenden, müssen Sie Erstellen einer systemeigene Anwendung Azure AD Azure AD-Domäne Client-ID verwenden und URI für die erstellte Anwendung umleiten. Weitere Informationen finden Sie in der [Active Directory-Anwendung erstellen](../resource-group-create-service-principal-portal.md#create-an-active-directory-application) .

>[AZURE.NOTE] Die Anleitung oben links sind für eine Web-Anwendung Azure AD. Allerdings sind die Schritte genau dasselbe auch wenn Sie eine systemeigene Anwendung erstellen. 

### <a name="if-you-are-using-service-to-service-authentication-with-client-secret"></a>Wenn Sie mit geheimen Dienst-Authentifizierung verwenden 

Im folgenden Codeausschnitt wird die Anwendung nicht interaktiv, Authentifizieren über den Client geheimen / Schlüssel für eine Anwendung / Service Principal. Verwenden Sie diese mit einer vorhandenen [Azure AD "Web"-Anwendung](../resource-group-create-service-principal-portal.md).

    // Service principal / appplication authentication with client secret / key
    // Use the client ID and certificate of an existing AAD "Web App" application.
    SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());
    var domain = "<AAD-directory-domain>";
    var webApp_clientId = "<AAD-application-clientid>";
    var clientSecret = "<AAD-application-client-secret>";
    var clientCredential = new ClientCredential(webApp_clientId, clientSecret);
    var creds = ApplicationTokenProvider.LoginSilentAsync(domain, clientCredential).Result;

### <a name="if-you-are-using-service-to-service-authentication-with-certificate"></a>Wenn Sie mit Dienst-Authentifizierung verwenden

Als dritte option im folgenden Codeausschnitt lässt sich nicht interaktiv, authentifiziert die Anwendung mithilfe des Zertifikats für eine Anwendung / Service Principal. Verwenden Sie diese mit einer vorhandenen [Azure AD "Web"-Anwendung](../resource-group-create-service-principal-portal.md).

    // Service principal / application authentication with certificate
    // Use the client ID and certificate of an existing AAD "Web App" application.
    SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());
    var domain = "<AAD-directory-domain>";
    var webApp_clientId = "<AAD-application-clientid>";
    System.Security.Cryptography.X509Certificates.X509Certificate2 clientCert = <AAD-application-client-certificate>
    var clientAssertionCertificate = new ClientAssertionCertificate(webApp_clientId, clientCert);
    var creds = ApplicationTokenProvider.LoginSilentWithCertificateAsync(domain, clientAssertionCertificate).Result;

## <a name="create-client-objects"></a>Client-Objekte erstellen

Der folgende Codeausschnitt erstellt die See Datenspeicher Konto und Dateisystem Objekte verwendet werden, um Anfragen an den Dienst ausgeben.

    // Create client objects and set the subscription ID
    _adlsClient = new DataLakeStoreAccountManagementClient(creds);
    _adlsFileSystemClient = new DataLakeStoreFileSystemManagementClient(creds);

    _adlsClient.SubscriptionId = _subId;

## <a name="list-all-data-lake-store-accounts-within-a-subscription"></a>Listen Sie alle Datenspeicher See Konten innerhalb eines Abonnements

Im folgenden Codeausschnitt Listet alle Datenspeicher See Konten innerhalb eines bestimmten Azure-Abonnements.

    // List all ADLS accounts within the subscription
    public static List<DataLakeStoreAccount> ListAdlStoreAccounts()
    {
        var response = _adlsClient.Account.List();
        var accounts = new List<DataLakeStoreAccount>(response);
        
        while (response.NextPageLink != null)
        {
            response = _adlsClient.Account.ListNext(response.NextPageLink);
            accounts.AddRange(response);
        }
        
        return accounts;
    }

## <a name="create-a-directory"></a>Erstellen Sie ein Verzeichnis

Der folgende Ausschnitt zeigt einen `CreateDirectory` Methode zum Erstellen eines Verzeichnisses in einem Datenspeicher See Konto.

    // Create a directory
    public static void CreateDirectory(string path)
    {
        _adlsFileSystemClient.FileSystem.Mkdirs(_adlsAccountName, path);
    }

## <a name="upload-a-file"></a>Hochladen einer Datei

Der folgende Ausschnitt zeigt ein `UploadFile` Methode zum Hochladen von Dateien auf See Datenspeicher-Konto.

    // Upload a file
    public static void UploadFile(string srcFilePath, string destFilePath, bool force = true)
    {
        var parameters = new UploadParameters(srcFilePath, destFilePath, _adlsAccountName, isOverwrite: force);
        var frontend = new DataLakeStoreFrontEndAdapter(_adlsAccountName, _adlsFileSystemClient);
        var uploader = new DataLakeStoreUploader(parameters, frontend);
        uploader.Execute();
    }

`DataLakeStoreUploader`Rekursive Upload- und zwischen lokalen Dateipfad und einen Dateipfad See Datenspeicher unterstützt.    

## <a name="get-file-or-directory-info"></a>Datei oder Verzeichnis Informationen

Der folgende Ausschnitt zeigt einen `GetItemInfo` Methode zum Abrufen von Informationen über eine Datei oder ein Verzeichnis im Datenspeicher See. 

    // Get file or directory info
    public static FileStatusProperties GetItemInfo(string path)
    {
        return _adlsFileSystemClient.FileSystem.GetFileStatus(_adlsAccountName, path).FileStatus;
    }

## <a name="list-file-or-directories"></a>Datei oder Verzeichnisse

Der folgende Ausschnitt zeigt einen `ListItem` Methode zum Auflisten von Dateien und Verzeichnissen in einem Datenspeicher See Konto verwenden kann.

    // List files and directories
    public static List<FileStatusProperties> ListItems(string directoryPath)
    {
        return _adlsFileSystemClient.FileSystem.ListFileStatus(_adlsAccountName, directoryPath).FileStatuses.FileStatus.ToList();
    }

## <a name="concatenate-files"></a>Verketten von Dateien

Der folgende Ausschnitt zeigt einen `ConcatenateFiles` Methode, um Dateien zu verketten. 

    // Concatenate files
    public static void ConcatenateFiles(string[] srcFilePaths, string destFilePath)
    {
        _adlsFileSystemClient.FileSystem.Concat(_adlsAccountName, destFilePath, srcFilePaths);
    }

## <a name="append-to-a-file"></a>An eine Datei anhängen

Der folgende Ausschnitt zeigt einen `AppendToFile` Methode Daten in eine Datei bereits in einem Datenspeicher See Konto hinzufügen.

    // Append to file
    public static void AppendToFile(string path, string content)
    {
        var stream = new MemoryStream(Encoding.UTF8.GetBytes(content));
        
        _adlsFileSystemClient.FileSystem.Append(_adlsAccountName, path, stream);
    }

## <a name="download-a-file"></a>Datei herunterladen

Der folgende Ausschnitt zeigt einen `DownloadFile` Methode, eine Datei von einem Datenspeicher See Konto.

    // Download file
    public static void DownloadFile(string srcPath, string destPath)
    {
        var stream = _adlsFileSystemClient.FileSystem.Open(_adlsAccountName, srcPath);
        var fileStream = new FileStream(destPath, FileMode.Create);
        
        stream.CopyTo(fileStream);
        fileStream.Close();
        stream.Close();
    }

## <a name="next-steps"></a>Nächste Schritte

- [Sichere Daten im Datenspeicher See](data-lake-store-secure-data.md)
- [Verwenden Sie Azure Data Lake Analytics See Datenspeicher](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Verwenden von Azure HDInsight Lake Datenspeicher](data-lake-store-hdinsight-hadoop-use-portal.md)
- [Datenspeicher See .NET SDK-Referenz](https://msdn.microsoft.com/library/mt581387.aspx)
- [Datenspeicher See REST Verweis](https://msdn.microsoft.com/library/mt693424.aspx)
