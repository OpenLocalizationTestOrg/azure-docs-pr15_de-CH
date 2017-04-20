<properties
    pageTitle="Lernprogramm: Verschlüsseln und Entschlüsseln von Blobs in Azure Key Vault mit Microsoft Azure Storage | Microsoft Azure"
    description="Dieses Lernprogramm führt Sie durch das Verschlüsseln und Entschlüsseln clientseitige Verschlüsselung für Microsoft Azure Storage mit Schlüssel Azure Blob."
    services="storage"
    documentationCenter=""
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="required"
    ms.date="10/18/2016"
    ms.author="lakasa;robinsh"/>

# <a name="tutorial-encrypt-and-decrypt-blobs-in-microsoft-azure-storage-using-azure-key-vault"></a>Lernprogramm: Verschlüsseln und Entschlüsseln von Blobs in Azure Key Vault mit Microsoft Azure-Speicher

## <a name="introduction"></a>Einführung

In diesem Lernprogramm behandelt wie clientseitige Speicher Verschlüsselung mit Azure Schlüssel verwenden. Er führt Sie durch das Verschlüsseln und Entschlüsseln eines BLOBs in einem Konsolenanwendungsprojekt Technologien.

**Geschätzte Zeit:** 20 Minuten

Übersicht über Azure Key Vault finden Sie unter [Neuigkeiten Azure Key Vault?](../key-vault/key-vault-whatis.md).

Übersicht über clientseitigen Verschlüsselung für Azure Storage finden Sie [clientseitige Verschlüsselung und Azure Schlüssel Depot für Microsoft Azure-Speicher](storage-client-side-encryption.md).


## <a name="prerequisites"></a>Erforderliche Komponenten

Um dieses Lernprogramm benötigen Sie Folgendes:

- Ein Azure Storage-Konto
- Visual Studio 2013 oder höher
- Azure PowerShell


## <a name="overview-of-client-side-encryption"></a>Übersicht über die clientseitige Verschlüsselung

Einen Überblick über die clientseitige Verschlüsselung für Azure Storage finden Sie unter [clientseitige Verschlüsselung und Azure Schlüssel Depot für Microsoft Azure Storage](storage-client-side-encryption.md)

Hier wird eine kurze Beschreibung der Funktionsweise der clientseitigen Verschlüsselung:

1. Azure Storage Client SDK generiert eine Inhaltsverschlüsselungsschlüssel (CEK) ist ein one-time-Use symmetrischen Schlüssel.
2. Kundendaten werden mit diesem CEK verschlüsselt.
3. Die CEK wird dann eingeschlossen (verschlüsselt) Verschlüsselung Taste (KEK). Die KEK kann wird durch eine Schlüssel-ID identifiziert und ein asymmetrisches Schlüsselpaar oder eines symmetrischen Schlüssels und können werden lokal verwaltete oder in Azure Schlüssel Depot gespeichert. Speicher-Client hat nie Zugriff auf die KEK. Ruft nur wichtige Umbruch-Algorithmus, der von Schlüssel bereitgestellt. Kunden können benutzerdefinierte Anbieter für Verpackung/Entpacken sie ggf. Schlüssel verwenden.
4. Die verschlüsselten Daten werden dann in Azure Storage Service hochgeladen.


## <a name="set-up-your-azure-key-vault"></a>Richten Sie Ihre Azure Key Vault
Um dieses Lernprogramm fortsetzen, müssen Sie in [Erste Schritte mit Azure Schlüssel](../key-vault/key-vault-get-started.md)Lernprogramm beschriebenen folgende Schritte durchzuführen:

- Erstellen Sie ein Schlüssel Depot.
- Hinzufügen eines Schlüssels oder geheimen Schlüssel Depot.
- Registrieren einer Anwendung in Azure Active Directory.
- Autorisieren Sie die Anwendung des Schlüssels oder geheimen.

Notieren Sie die ClientID und ClientSecret, die beim Registrieren einer Anwendung in Azure Active Directory generiert wurden.

Erstellen Sie beide Schlüssel im Schlüssel Depot. Vorausgesetzt für den Rest des Lernprogramms die folgenden Namen verwenden: ContosoKeyVault und TestRSAKey1.


## <a name="create-a-console-application-with-packages-and-appsettings"></a>Erstellen Sie eine mit AppSettings

Erstellen Sie in Visual Studio eine neue.

Erforderlichen Nuget-Pakete in der Paket-Manager-Konsole hinzufügen.

    Install-Package WindowsAzure.Storage

    // This is the latest stable release for ADAL.
    Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.16.204221202

    Install-Package Microsoft.Azure.KeyVault
    Install-Package Microsoft.Azure.KeyVault.Extensions


App.Config AppSettings hinzufügen.

    <appSettings>
        <add key="accountName" value="myaccount"/>
        <add key="accountKey" value="theaccountkey"/>
        <add key="clientId" value="theclientid"/>
        <add key="clientSecret" value="theclientsecret"/>
        <add key="container" value="stuff"/>
    </appSettings>

Fügen Sie den folgenden `using` Aussagen und vergewissern Sie sich, das Projekt einen Verweis auf den System.Configuration hinzu.

    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using System.Configuration;
    using Microsoft.WindowsAzure.Storage.Auth;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    using Microsoft.Azure.KeyVault;
    using System.Threading;     
    using System.IO;


## <a name="add-a-method-to-get-a-token-to-your-console-application"></a>Hinzufügen einer Methode zum Abrufen eines Tokens für die Konsolenanwendung

Die folgende Methode dient Key Vault-Klassen, die für den Zugriff auf den Schlüssel Tresor authentifizieren müssen.

    private async static Task<string> GetToken(string authority, string resource, string scope)
    {
        var authContext = new AuthenticationContext(authority);
        ClientCredential clientCred = new ClientCredential(
            ConfigurationManager.AppSettings["clientId"],
            ConfigurationManager.AppSettings["clientSecret"]);
        AuthenticationResult result = await authContext.AcquireTokenAsync(resource, clientCred);

        if (result == null)
            throw new InvalidOperationException("Failed to obtain the JWT token");

        return result.AccessToken;
    }

## <a name="access-storage-and-key-vault-in-your-program"></a>Zugriff und Schlüssel Depot im Programm

Fügen Sie der Main-Funktion folgenden Code hinzu.

    // This is standard code to interact with Blob storage.
    StorageCredentials creds = new StorageCredentials(
        ConfigurationManager.AppSettings["accountName"],
        ConfigurationManager.AppSettings["accountKey"]);
    CloudStorageAccount account = new CloudStorageAccount(creds, useHttps: true);
    CloudBlobClient client = account.CreateCloudBlobClient();
    CloudBlobContainer contain = client.GetContainerReference(ConfigurationManager.AppSettings["container"]);
    contain.CreateIfNotExists();

    // The Resolver object is used to interact with Key Vault for Azure Storage.
    // This is where the GetToken method from above is used.
    KeyVaultKeyResolver cloudResolver = new KeyVaultKeyResolver(GetToken);


> [AZURE.NOTE] Key Vault-Objektmodelle
>
>Unbedingt verstehen, dass es eigentlich zwei Schlüssel Depot Objektmodelle berücksichtigen: eine basiert auf die REST-API (Schlüsseltresor-Namespace) und die andere ist eine Erweiterung für die clientseitige Verschlüsselung.

> Key Vault Client interagiert mit dem REST-API und versteht Internetschlüssel JSON und Geheimnisse für zwei Arten von Was Key Vault enthalten sind.

> Key Vault Extensions sind Klassen, die scheinen speziell für die clientseitige Verschlüsselung in Azure-Speicher. Sie enthalten eine Schnittstelle (IKey) und Klassen basierend auf das Konzept der Resolver Schlüssel. Es gibt zwei Implementierungen IKey Sie wissen müssen: RSAKey und SymmetricKey. Jetzt sie Dinge entsprechen, die in einem Schlüssel enthaltenen passieren, aber sie sind unabhängige Klassen (also den Schlüssel und geheime Schlüssel Vault Client abgerufen IKey nicht implementiert).


## <a name="encrypt-blob-and-upload"></a>Blob verschlüsseln und Hochladen
Fügen Sie folgenden Code zum Verschlüsseln eines BLOBs und Ihrem Konto Azure-Speicher hochgeladen. Die Methode **ResolveKeyAsync** gibt einen IKey.


    // Retrieve the key that you created previously.
    // The IKey that is returned here is an RsaKey.
    // Remember that we used the names contosokeyvault and testrsakey1.
    var rsa = cloudResolver.ResolveKeyAsync("https://contosokeyvault.vault.azure.net/keys/TestRSAKey1", CancellationToken.None).GetAwaiter().GetResult();


    // Now you simply use the RSA key to encrypt by setting it in the BlobEncryptionPolicy.
    BlobEncryptionPolicy policy = new BlobEncryptionPolicy(rsa, null);
    BlobRequestOptions options = new BlobRequestOptions() { EncryptionPolicy = policy };

    // Reference a block blob.
    CloudBlockBlob blob = contain.GetBlockBlobReference("MyFile.txt");

    // Upload using the UploadFromStream method.
    using (var stream = System.IO.File.OpenRead(@"C:\data\MyFile.txt"))
        blob.UploadFromStream(stream, stream.Length, null, options, null);


Folgendes ist ein Screenshot aus dem [Azure-Verwaltungsportal](https://manage.windowsazure.com) für ein Blob mit clientseitigen Encryption Key Vault einen Schlüssel verschlüsselt wurde. Die **Schlüssel-ID** -Eigenschaft ist der URI für den Schlüssel im Schlüssel-Depot als der KEK fungiert. Die **EncryptedKey** -Eigenschaft enthält die verschlüsselte Version der CEK.

![Screenshot zeigt Blob-Metadaten, die Verschlüsselung Metadaten][1]

> [AZURE.NOTE] Betrachten Sie den Konstruktor BlobEncryptionPolicy, sehen Sie einen Schlüssel oder einen Konfliktlöser annehmen kann. Beachten Sie, jetzt Sie können einen Konfliktlöser für die Verschlüsselung verwenden, da es derzeit keinen Standardschlüssel unterstützt.



## <a name="decrypt-blob-and-download"></a>Blob zu entschlüsseln und herunterladen
Die Entschlüsselung ist wirklich beim mithilfe der Resolver Klassen sinnvoll. Die ID der Schlüssel für die Verschlüsselung ist es also keinen Grund, den Schlüssel abzurufen und die Zuordnung zwischen Schlüssel und Blob Blob in den Metadaten zugeordnet. Sie müssen nur sicherstellen, dass der Schlüssel im Schlüssel Depot bleibt.   

Des privaten Schlüssels des RSA-Schlüssel verbleibt im Schlüsseltresor so zur Entschlüsselung auf den Schlüssel verschlüsselten Blob-Metadaten, die die CEK enthält für die Entschlüsselung Schlüssel Tresor gesendet wird.

Fügen Sie folgenden Blob zu entschlüsseln, das Sie gerade hochgeladen.

    // In this case, we will not pass a key and only pass the resolver because
    // this policy will only be used for downloading / decrypting.
    BlobEncryptionPolicy policy = new BlobEncryptionPolicy(null, cloudResolver);
    BlobRequestOptions options = new BlobRequestOptions() { EncryptionPolicy = policy };

    using (var np = File.Open(@"C:\data\MyFileDecrypted.txt", FileMode.Create))
        blob.DownloadToStream(np, null, options, null);


> [AZURE.NOTE] Es gibt ein paar andere Konfliktlöser Schlüsselmanagement einschließlich erleichtern: AggregateKeyResolver und CachingKeyResolver.


## <a name="use-key-vault-secrets"></a>Verwenden Sie Schlüssel Depot geheime Schlüssel
Verwendung eines geheimen Schlüssels mit clientseitigen Verschlüsselung geht über die SymmetricKey-Klasse ist ein geheimer Schlüssel im Wesentlichen einen symmetrischen Schlüssel. Aber wie bereits erwähnt, eine geheime Schlüssel Depot zugeordnet ist genau ein SymmetricKey. Es gibt einige Dinge zu verstehen:


- Der Schlüssel in einer SymmetricKey muss eine feste Länge: 128, 192, 256, 384 und 512 Bits.
- Schlüssel in einer SymmetricKey muss Base64-codiert sein.
- Key Vault Geheimnis, das als ein SymmetricKey verwendet werden muss Inhaltstyp "Application/Octet-Stream" im Schlüsseltresor.

Hier ist ein Beispiel in PowerShell erstellen einen geheimen Schlüssel im Schlüssel-Depot, das als ein SymmetricKey verwendet.
Hinweis: Der hartcodierte Wert $key, ist für Demo-Zwecke. In Ihrem eigenen Code sollten Sie diesen Schlüssel erstellen.

    // Here we are making a 128-bit key so we have 16 characters.
    //  The characters are in the ASCII range of UTF8 so they are
    //  each 1 byte. 16 x 8 = 128.
    $key = "qwertyuiopasdfgh"
    $b = [System.Text.Encoding]::UTF8.GetBytes($key)
    $enc = [System.Convert]::ToBase64String($b)
    $secretvalue = ConvertTo-SecureString $enc -AsPlainText -Force

    // Substitute the VaultName and Name in this command.
    $secret = Set-AzureKeyVaultSecret -VaultName 'ContoseKeyVault' -Name 'TestSecret2' -SecretValue $secretvalue -ContentType "application/octet-stream"

Gleichen Anruf können Sie in der Konsolenanwendung diesen Schlüssel als eine SymmetricKey abrufen.

    SymmetricKey sec = (SymmetricKey) cloudResolver.ResolveKeyAsync(
        "https://contosokeyvault.vault.azure.net/secrets/TestSecret2/",
        CancellationToken.None).GetAwaiter().GetResult();

Das wars. Viel Spaß!

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Microsoft Azure Storage mit C# finden Sie unter [Microsoft Azure Storage-Clientbibliothek für .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).

Weitere Informationen über die BLOB-REST-API finden Sie unter [Blob Service REST-API](https://msdn.microsoft.com/library/azure/dd135733.aspx).

Aktuelle Informationen zu Microsoft Azure-Speicher finden Sie im [Microsoft Azure Storage-Teamblog](http://blogs.msdn.com/b/windowsazurestorage/).


<!--Image references-->
[1]: ./media/storage-encrypt-decrypt-blobs-key-vault/blobmetadata.png
