<properties
    pageTitle="Schlüsseltresor mit Schlüsselrotation Ende Überwachung Einrichtung | Microsoft Azure"
    description="Mit dieser Vorgehensweise können Sie Setup mit Schlüsselrotation Key Vault Überwachungsprotokolle erhalten"
    services="key-vault"
    documentationCenter=""
    authors="swgriffith"
    manager="mbaldwin"
    tags=""/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/05/2016"
    ms.author="jodehavi;stgriffi"/>
#<a name="how-to-setup-key-vault-with-end-to-end-key-rotation-and-auditing"></a>Schlüsseltresor mit Schlüsselrotation Ende Überwachung einrichten

##<a name="introduction"></a>Einführung

Nach dem Erstellen der Azure Key Vault können Site dieses Depot zum Speichern Ihrer Schlüssel und geheime Schlüssel Sie. Ihre Anwendung mehr müssen weiterhin Ihre Schlüssel oder Kennwörter, sondern sie Key Vault anfordern wird Bedarf. Dadurch können Sie Schlüssel und geheime Daten beeinträchtigt das Verhalten Ihrer Anwendung eine Breite Palette von rund um Ihre Schlüssel öffnet und geheime Verhalten aktualisieren.

Dieser Artikel führt durch ein Beispiel für die Nutzung von Azure Key Vault um einen geheimen Schlüssel in diesem Fall ein Speicherkonto Azure Schlüssel speichern, die von einer Anwendung zugegriffen wird. Implementierung der geplanten Drehung, Speicherschlüssel Konto wird auch demonstrieren. Schließlich führt sie durch eine Demonstration der wichtigsten Tresor Überwachungsprotokolle und Warnungen bei unerwarteten angefordert werden.

> \[AZURE. Hinweis\] dieses Lernprogramm soll nicht ausführlich den Anfangssatz von Azure Key Vault erklären. Diese Informationen finden Sie unter [Erste Schritte mit Azure Schlüssel](key-vault-get-started.md). Oder Hinweise plattformübergreifende Befehlszeilenschnittstelle [Lernprogramms entspricht](key-vault-manage-with-cli.md).

##<a name="setting-up-keyvault"></a>Schlüsseltresor einrichten

Damit eine Anwendung ein Geheimnis von Azure Key Vault abrufen kann, müssen zunächst den geheimen Schlüssel zu erstellen und auf Ihrem Depot hochladen. Dies kann problemlos über PowerShell ausgeführt werden, wie unten dargestellt.

Starten Sie eine Azure PowerShell-Sitzung und melden Sie an, um Ihre Azure-Konto mit dem folgenden Befehl:

```powershell
Login-AzureRmAccount
```

Geben Sie im Popupmenü Browserfenster Ihre Azure Benutzernamen und Kennwort. Azure PowerShell erhalten alle Abonnements, die mit diesem Konto und standardmäßig verwendet die erste.

Haben Sie mehrere Abonnements möglicherweise eine bestimmte angeben, die mit Ihrem Tresor Azure Schlüssel erstellt wurde. Geben Sie Folgendes ein, um die Abonnements für Ihr Konto finden Sie unter:

```powershell
Get-AzureRmSubscription
```

Um das Abonnement angeben, das mit Ihrem Schlüssel zugeordnet sind, die Sie anmelden, geben Sie:

```powershell
Set-AzureRmContext -SubscriptionId <subscriptionID> 
```

Wie in diesem Artikel wird veranschaulicht, wie ein Speicher Konto Schlüssel geheim, müssen Sie dieses Konto Speicherschlüssel erhalten.

```powershell
Get-AzureRmStorageAccountKey -ResourceGroupName <resourceGroupName> -Name <storageAccountName>
```

Nach dem Abrufen der geheimen Schlüssel, in diesem Fall der Speicherschlüssel Konto müssen Sie eine sichere Zeichenfolge zu konvertieren, und erstellen Sie einen geheimen Schlüssel mit den Wert im Schlüssel Tresor.

```powershell
$secretvalue = ConvertTo-SecureString <storageAccountKey> -AsPlainText -Force

Set-AzureKeyVaultSecret -VaultName <vaultName> -Name <secretName> -SecretValue $secretvalue
```
Anschließend sollten Sie den URI für den Schlüssel erhalten erstellten. In einem späteren Schritt wird hiermit Anrufe Schlüssel Depot geheime abrufen. Führen Sie folgenden PowerShell-Befehl und notieren Sie die geheimen URI 'Id'-Wert.

```powershell
Get-AzureKeyVaultSecret –VaultName <vaultName>
```

##<a name="setting-up-application"></a>Anwendung einrichten

Jetzt haben Sie einen geheimen Schlüssel gespeichert möchten Sie das Geheimnis abrufen und Verwenden von Code. Es gibt einige Schritte zu erreichen, das erste und wichtigste die Anwendung in Azure Active Directory registrieren und dann die Anwendungsinformationen, Azure Key Vault, damit Anfragen von Ihrer Anwendung können.

> \[AZURE. Hinweis\] Ihrer Anwendung muss auf demselben Azure Active Directory Mandanten als Vault Schlüssel erstellt werden. 

Zuerst öffnen Sie die Registerkarte Programme von Azure Active Directory

![Offene Anträge in Azure AD](./media/keyvault-keyrotation/AzureAD_Header.png)

Wählen Sie "Hinzufügen" eine neue Anwendung in Azure AD

![Wählen Sie Anwendung hinzufügen](./media/keyvault-keyrotation/Azure_AD_AddApp.png)

Lassen Sie den Anwendungstyp "Web Application oder WEB API" und nennen Sie der Anwendung.

![Name der Anwendung](./media/keyvault-keyrotation/AzureAD_NewApp1.png)

Geben Sie der Anwendung ein "URL anmelden" und "App-ID URI". Diese können beliebig für diese Demo und können später geändert werden, falls erforderlich.

![Bereitstellen der erforderlichen URIs](./media/keyvault-keyrotation/AzureAD_NewApp2.png)

Sobald die Anwendung in Azure AD hinzugefügt wurde, werden Sie in der Anwendungsseite gebracht. Klicken Sie auf 'Konfigurieren' ab diesem Zeitpunkt suchen Sie und kopieren Sie den Client-ID-Wert. Notieren Sie sich die Client-ID für spätere Schritte.

Als Nächstes müssen Sie einen Schlüssel für die Anwendung mit der Azure AD generiert. Sie können dies im Abschnitt "Schlüssel" auf der Registerkarte "Konfiguration" erstellen. Notieren Sie den neu generierten Schlüssel von Azure AD-Anwendung für die Verwendung in einem späteren Schritt.

![Azure AD App-Schlüssel](./media/keyvault-keyrotation/Azure_AD_AppKeys.png)

Bevor Sie alle Aufrufe der Anwendung Tresor Schlüssel müssen Schlüssel Depot über Ihre Anwendung informieren und ' Berechtigungen. Der folgende Befehl akzeptiert den Namen Depot und die Client-ID von Azure AD app und gewährt Zugriff auf den Schlüssel Tresor 'Get' für die Anwendung.

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName <vaultName> -ServicePrincipalName <clientIDfromAzureAD> -PermissionsToSecrets Get
```

An diesem Punkt können Sie bauen Ihre Anwendung aufrufen. In der Anwendung müssen Sie zunächst mit Azure Key Vault und Azure Active Directory erforderlichen NuGet-Pakete installieren. Geben Sie die folgenden Befehle in der Konsole Visual Studio Paket-Manager. Beachten Sie, dass bei der Erstellung dieses Artikels die aktuelle Version des Active Directory 3.10.305231913, sollten Sie die neueste Version bestätigen und entsprechend zu aktualisieren.

```powershell
Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 3.10.305231913

Install-Package Microsoft.Azure.KeyVault
```

Erstellen Sie eine Klasse die Methode für die Active Directory-Authentifizierung in Ihrem Anwendungscode. In diesem Beispiel heißt Klasse "Utils". Sie müssen die folgenden hinzufügen.

```csharp
using Microsoft.IdentityModel.Clients.ActiveDirectory;
```

Fügen Sie die folgende Methode von Azure AD JWT-Token abzurufen. Für eine einfachere Verwaltung der Festplatte verschieben möchten codierten Werte Ihrer Web- oder Anwendungsserver-Konfiguration.

```csharp
public async static Task<string> GetToken(string authority, string resource, string scope)
{
    var authContext = new AuthenticationContext(authority);

    ClientCredential clientCred = new ClientCredential("<AzureADApplicationClientID>","<AzureADApplicationClientKey>");

    AuthenticationResult result = await authContext.AcquireTokenAsync(resource, clientCred);

    if (result == null)

    throw new InvalidOperationException("Failed to obtain the JWT token");

    return result.AccessToken;
}
```

Schließlich können den erforderlichen Code Key Vault und Ihre geheimen Wert abzurufen. Sie müssen zuerst die folgenden hinzufügen mit Anweisung.

```csharp
using Microsoft.Azure.KeyVault;
```

Anschließend fügen Sie Methodenaufrufe Depot Schlüssel und geheime abzurufen. In dieser Methode bieten den Schlüssel-URI, die Sie zuvor gespeichert. Beachten Sie die Verwendung der GetToken-Methode von oben erstellten Utils-Klasse.
    
```csharp
var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetToken));

var sec = kv.GetSecretAsync(<SecretID>).Result.Value;
```

Beim Ausführen der Anwendung sollten Sie jetzt können Azure Active Directory authentifizieren und Ihre geheimen Wert von Azure Key Vault abgerufen.

##<a name="key-rotation-using-azure-automation"></a>Schlüsselrotation Azure Automatisierung

Es gibt verschiedene Optionen zur Umsetzung einer Drehung Werte Azure Key Vault Geheimnisse speichern. Geheimnisse als Teil manuell gedreht werden können oder über ein Automatisierungsskript gedreht werden sie möglicherweise gedreht werden, programmgesteuert mithilfe der API-Aufrufe. Für die Zwecke dieses Artikels ist sein nutzen wir Azure PowerShell zusammen mit Azure Automation Zugriffstaste Azure Storage-Konto ändern, und wir werden wichtige Vault-Schlüssel mit dem neuen Schlüssel aktualisieren. 

Um Azure Automation geheimen Werte im Schlüssel Tresor zulassen müssen Sie erhalten die Client-ID für die Verbindung mit dem Namen 'AzureRunAsConnection' der Azure Automatisierungsinstanz hergestellt erstellt wurde. Diese ID erhalten durch Ihre Azure Automatisierungsinstanz 'Anlagen' auswählen. Dort wählen Sie "Verbindungen" und wählen Sie dann 'AzureRunAsConnection'-Dienstprinzipalname. Sie möchten die '-ID' beachten. 

![Azure Automation Client-ID](./media/keyvault-keyrotation/Azure_Automation_ClientID.png)

Im Fenster Anlagen sind auch soll 'Module' auswählen. Module "Katalog" wählen und suchen und 'Import' Versionen die folgenden Module aktualisiert.

    Azure
    Azure.Storage   
    AzureRM.Profile
    AzureRM.KeyVault
    AzureRM.Automation
    AzureRM.Storage
    
> \[AZURE. Hinweis\] Zeitpunkt der Erstellung dieses Artikels erwähnt nur die oben aufgeführten Module müssen für das Skript unter gemeinsame aktualisiert werden. Wenn Sie feststellen, dass die Automatisierung Auftrag fehlschlägt, sollten Sie sicherstellen, haben alle erforderlichen Module und deren abhängigen Dateien importiert.

Nachdem Sie die ID für die Verbindung Azure Automation abgerufen haben, müssen Sie Ihre Azure Key Vault sagen, dass diese Anwendung Zugriff auf vertrauliche Informationen in Ihrem Tresor aktualisieren. Dies kann mit dem folgenden PowerShell-Befehl erfolgen.

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName <vaultName> -ServicePrincipalName <applicationIDfromAzureAutomation> -PermissionsToSecrets Set
```

Anschließend wählen Sie Ressource "Runbooks" unter Ihrem Azure Automatisierungsinstanz und ein Runbook wählen Sie 'hinzufügen'. Wählen Sie "Schnell erstellen". Ihr Runbook und wählen Sie "PowerShell" Dateityp Runbook. Fügen Sie optional eine Beschreibung hinzu. Klicken Sie abschließend auf "Erstellen".

![Runbook erstellen](./media/keyvault-keyrotation/Create_Runbook.png)

Fügen Sie im Editorbereich für neue Runbook PowerShell-Skript.

```powershell
$connectionName = "AzureRunAsConnection"
try
{
    # Get the connection "AzureRunAsConnection "
    $servicePrincipalConnection=Get-AutomationConnection -Name $connectionName         

    "Logging in to Azure..."
    Add-AzureRmAccount `
        -ServicePrincipal `
        -TenantId $servicePrincipalConnection.TenantId `
        -ApplicationId $servicePrincipalConnection.ApplicationId `
        -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint 
    "Login complete."
}
catch {
    if (!$servicePrincipalConnection)
    {
        $ErrorMessage = "Connection $connectionName not found."
        throw $ErrorMessage
    } else{
        Write-Error -Message $_.Exception
        throw $_.Exception
    }
}

#Optionally you may set the following as parameters
$StorageAccountName = <storageAccountName>
$RGName = <storageAccountResourceGroupName>
$VaultName = <keyVaultName>
$SecretName = <keyVaultSecretName>

#Key name. For example key1 or key2 for the storage account
New-AzureRmStorageAccountKey -ResourceGroupName $RGName -StorageAccountName $StorageAccountName -KeyName "key2" -Verbose
$SAKeys = Get-AzureRmStorageAccountKey -ResourceGroupName $RGName -Name $StorageAccountName

$secretvalue = ConvertTo-SecureString $SAKeys[1].Value -AsPlainText -Force

$secret = Set-AzureKeyVaultSecret -VaultName $VaultName -Name $SecretName -SecretValue $secretvalue
```

Im Editorbereich können Sie 'Testbereich um das Skript zu testen. Nach der Ausführung des Skripts ohne Fehler können die Option "Veröffentlichen" und dann einen Zeitplan für das Runbook in den Konfigurationsbereich Runbook angewendet werden können.

##<a name="key-vault-auditing-pipeline"></a>Pipeline Schlüssel Depot Überwachung

Beim Einrichten einer Azure Key Vault können Sie Protokolle Zugriff Anträge Schlüssel Depot sammeln auditing aktivieren. Diese Protokolle werden in festgelegten Azure Storage-Konto und können dann herausgezogen werden, überwacht und analysiert. Unter schrittweise ein Szenario, das Azure-Funktionen nutzt Überwachungsprotokolle Azure Logik Apps und Schlüssel Depot erstellen eine Pipeline e-Mail Geheimnisse aus dem Tresor einer App abgerufen werden, die die app-Id der Web-app übereinstimmt.

Zunächst müssen Sie die Protokollierung auf Ihre Schlüssel. Dies kann über die folgenden PowerShell-Befehle (Einzelheiten sehen [hier](key-vault-logging.md)):

```powershell
$sa = New-AzureRmStorageAccount -ResourceGroupName <resourceGroupName> -Name <storageAccountName> -Type Standard\_LRS -Location 'East US'
$kv = Get-AzureRmKeyVault -VaultName '<vaultName>' 
Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent
```

Wenn diese Option aktiviert ist, werden dann Überwachungsprotokolle sammeln designierten Speicher berücksichtigt. Diese Protokolle enthalten Ereignisse und beim Zugriff auf Ihre Schlüssel Depots und von wem. 

> \[AZURE. Hinweis\] der Protokollinformationen können höchstens zugreifen, 10 Minuten nach dem Schlüssel Depot Vorgang. In den meisten Fällen werden dadurch schneller.

Im nächste Schritt wird [eine Azure Service Bus-Warteschlange erstellen](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md). Wo Überwachungsprotokolle Schlüssel Depot abgelegt werden wird. In der Warteschlange App Logik einmal Abholen und handeln. Erstellen einer Service Bus ist relativ Geradlinig und unten sind die Schritte:

1. Erstellen Sie einen Service Bus-Namespace (haben Sie bereits eine soll dafür verwenden und fahren Sie mit Schritt 2).
2. Den Service Bus im Portal suchen Sie, und wählen Sie den Namespace die Warteschlange erstellen möchten.
3. Wählen Sie neu und Service Bus-Warteschlange >, und geben Sie die erforderlichen Details.
4. Packen der Verbindungsinformationen Service Bus Namespace auswählen und auf _Informationen_. Sie benötigen diese Informationen für den nächsten Teil.

Anschließend müssen Sie [erstellen eine Azure-Funktion](../azure-functions/functions-create-first-azure-function.md) , um Schlüssel Depot Protokolle in das Speicherkonto abrufen und neue Ereignisse. Eine Funktion werden, die nach einem Zeitplan ausgelöst wird.

Erstellen Sie eine Azure-Funktion (Neu -> Funktion App im Portal). Während der Erstellung können vorhandenen Hosting-Plan verwenden oder eine neue erstellen. Sie können auch zum Hosten von dynamischen entscheiden. Weitere Informationen zur Funktion Hostingoptionen finden Sie [hier](../azure-functions/functions-scale.md).

Beim Erstellen der Azure-Funktion navigieren und wählen Sie eine Timer-Funktion und C\# klicken Sie auf dem Startbildschirm **Erstellen** .

![Azure Funktionen Blade starten](./media/keyvault-keyrotation/Azure_Functions_Start.png)

Registerkarte _entwickeln_ ersetzen Sie run.csx Code durch Folgendes:

```csharp
#r "Newtonsoft.Json"

using System;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Auth;
using Microsoft.WindowsAzure.Storage.Blob;
using Microsoft.ServiceBus.Messaging; 
using System.Text;

public static void Run(TimerInfo myTimer, TextReader inputBlob, TextWriter outputBlob, TraceWriter log) 
{ 
    log.Info("Starting");

    CloudStorageAccount sourceStorageAccount = new CloudStorageAccount(new StorageCredentials("<STORAGE_ACCOUNT_NAME>", "<STORAGE_ACCOUNT_KEY>"), true);

    CloudBlobClient sourceCloudBlobClient = sourceStorageAccount.CreateCloudBlobClient();

    var connectionString = "<SERVICE_BUS_CONNECTION_STRING>";
    var queueName = "<SERVICE_BUS_QUEUE_NAME>";

    var sbClient = QueueClient.CreateFromConnectionString(connectionString, queueName);

    DateTime dtPrev = DateTime.UtcNow;
    if(inputBlob != null)
    {
        var txt = inputBlob.ReadToEnd();

        if(!string.IsNullOrEmpty(txt))
        {
            dtPrev = DateTime.Parse(txt);
            log.Verbose($"SyncPoint: {dtPrev.ToString("O")}");
        }
        else
        {
            dtPrev = DateTime.UtcNow;
            log.Verbose($"Sync point file didnt have a date. Setting to now.");
        }
    }

    var now = DateTime.UtcNow;

    string blobPrefix = "insights-logs-auditevent/resourceId=/SUBSCRIPTIONS/<SUBSCRIPTION_ID>/RESOURCEGROUPS/<RESOURCE_GROUP_NAME>/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/<KEY_VAULT_NAME>/y=" + now.Year +"/m="+now.Month.ToString("D2")+"/d="+ (now.Day).ToString("D2")+"/h="+(now.Hour).ToString("D2")+"/m=00/";

    log.Info($"Scanning:  {blobPrefix}");

    IEnumerable<IListBlobItem> blobs = sourceCloudBlobClient.ListBlobs(blobPrefix, true);

    log.Info($"found {blobs.Count()} blobs");

    foreach(var item in blobs)
    {
        if (item is CloudBlockBlob)
        {
            CloudBlockBlob blockBlob = (CloudBlockBlob)item;

            log.Info($"Syncing: {item.Uri}");

            string sharedAccessUri = GetContainerSasUri(blockBlob);

            CloudBlockBlob sourceBlob = new CloudBlockBlob(new Uri(sharedAccessUri));

            string text;
            using (var memoryStream = new MemoryStream())
            {
                sourceBlob.DownloadToStream(memoryStream);
                text = System.Text.Encoding.UTF8.GetString(memoryStream.ToArray());
            }

            dynamic dynJson = JsonConvert.DeserializeObject(text);

            //required to order by time as they may not be in the file
            var results = ((IEnumerable<dynamic>) dynJson.records).OrderBy(p => p.time);

            foreach (var jsonItem in results)
            {
                DateTime dt = Convert.ToDateTime(jsonItem.time);

                if(dt>dtPrev){
                    log.Info($"{jsonItem.ToString()}");

                    var payloadStream = new MemoryStream(Encoding.UTF8.GetBytes(jsonItem.ToString()));
                    //When sending to ServiceBus, use the payloadStream and set keeporiginal to true
                    var message = new BrokeredMessage(payloadStream, true);
                    sbClient.Send(message);
                    dtPrev = dt;
                }
            }
        }
    }
    outputBlob.Write(dtPrev.ToString("o"));
}

static string GetContainerSasUri(CloudBlockBlob blob) 
{
    SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy();

    sasConstraints.SharedAccessStartTime = DateTime.UtcNow.AddMinutes(-5);
    sasConstraints.SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24);
    sasConstraints.Permissions = SharedAccessBlobPermissions.Read;

    //Generate the shared access signature on the container, setting the constraints directly on the signature.
    string sasBlobToken = blob.GetSharedAccessSignature(sasConstraints);

    //Return the URI string for the container, including the SAS token.
    return blob.Uri + sasBlobToken;
}
```
> \[AZURE. Hinweis\] ersetzen Sie die Variablen im obigen Code darauf das Speicherkonto, Key Vault-Protokolle geschrieben werden, die zuvor erstellten Service Bus und bestimmten Pfad zum Schlüssel Depot Storage-Protokolle.

Die Funktion nimmt die neueste Protokolldatei das Speicherkonto, Key Vault-Protokolle geschrieben werden, greift die neuesten Ereignisse aus dieser Datei und Service Bus-Warteschlange legt. Da eine einzelne Datei mehrere Ereignisse haben konnte, erstellen z.B. über eine Stunde, dann wir eine _sync.txt_ -Datei, der die Funktion auch schaut den Zeitstempel des letzten Ereignisses Ermitteln der abgeholt wurde. Dadurch wird sichergestellt, dass wir das gleiche Ereignis mehrmals drücken nicht. Diese _sync.txt_ -Datei enthält einen Zeitstempel des letzten Ereignisses aufgetreten. Protokolle beim Laden, müssen sortiert werden, basierend auf den Zeitstempel, um sicherzustellen, dass diese richtig angeordnet werden.

Für diese Funktion verweisen wir ein paar zusätzliche Bibliotheken nicht bei Azure Funktionen verfügbar sind. Um hierzu benötigen wir Azure Funktionen zum Ziehen mit Nuget. Wählen Sie die Option _Dateien anzeigen_ 

![Option Dateien anzeigen](./media/keyvault-keyrotation/Azure_Functions_ViewFiles.png)

und fügen Sie eine neue Datei namens _project.json_ mit folgendem Inhalt:

```json
    {
      "frameworks": {
        "net46":{
          "dependencies": {
                "WindowsAzure.Storage": "7.0.0",
                "WindowsAzure.ServiceBus":"3.2.2"
          }
        }
       }
    }
```
Beim _Speichern_ wird diese Azure Funktionen zum Herunterladen der erforderlichen Binärdateien ausgelöst. 

Wechseln Sie zur Registerkarte **integrieren** und geben Sie dem Timer-Parameter einen aussagekräftigen Namen innerhalb der Funktion verwenden. Im obigen Code erwartet den Zeitgeber _MyTimer_aufgerufen werden. Geben Sie einen [CRON-Ausdruck](../app-service-web/web-sites-create-web-jobs.md#CreateScheduledCRON) wie folgt: 0 \* \* \* \* \* für den Zeitgeber für die Funktion einmal pro Minute ausgeführt wird. 

Fügen Sie derselben Registerkarte **Integration** Eingabe vom Typ _Azure BLOB-Speicher_werden. Dies wird in der Datei _sync.txt_ verweisen, den Zeitstempel des letzten Ereignisses untersucht die Funktion enthält. Dies in die Funktion vom Namen Parameters erfolgt. Im obigen Code erwartet die Eingabe Azure BLOB-Speicher Parameternamen zu _InputBlob_. Wählen Sie das Speicherkonto in die Datei _sync.txt_ gespeichert wird (möglicherweise dieselbe oder eine andere Speicherkonto) und geben Sie im pfadfeld den Pfad, in dem sich die Datei, im Format {container-name}/path/to/sync.txt befindet.

Fügen Sie eine Ausgabe hinzu, Typ _Azure BLOB-Speicher_ Ausgabe. Dies wird in der Datei _sync.txt_ darauf soeben definierten in der Eingabe. Dies wird durch die Funktion den Zeitstempel des letzten Ereignisses sah schreiben verwendet. Der obige Code erwartet, dass dieser Parameter _OutputBlob_aufgerufen werden.

Die Funktion ist nun bereit. Stellen Sie sicher, wechseln Sie zurück zur Registerkarte **Erstellen** und _Speichern Sie_ den Code. Überprüfen Sie im Ausgabefenster Kompilierungsfehler und korrigieren Sie diese entsprechend. Wenn sie kompiliert wird, der Code läuft jetzt und jede Minute den Schlüssel Depot Protokollen und alle neuen Ereignisse auf die definierten Service Bus-Warteschlange drücken. Finden Sie unter Informationen in jedem Fenster Log schreiben die Funktion ausgelöst wird.

###<a name="azure-logic-app"></a>Azure Logik-App

Als Nächstes müssen wir eine Azure Logik App erstellen, wählen Sie die Ereignisse, die die Funktion der Service Bus-Warteschlange beschäftigt ist, analysiert den Inhalt und e-Mail basierend auf einer Bedingung, die verglichen.

[Erstellen einer Anwendung Logik](../app-service-logic/app-service-logic-create-a-logic-app.md) auf Neu -> Logik App. 

Erstellte Anwendung Logik navigieren Sie und _Bearbeiten_. Innerhalb der Logik App-Editor wählen Sie _Service Bus-Warteschlange_ api verwaltet und Anmeldeinformationen Sie eine Verbindung zu der Warteschlange Service Bus.

![Azure Logik App Service Bus](./media/keyvault-keyrotation/Azure_LogicApp_ServiceBus.png)

Als nächstes wählen Sie _eine Bedingung_hinzu. Wechseln Sie in der Bedingung zum _erweiterten Editor_ und geben Sie Folgendes ein, ersetzen die APP_ID mit den tatsächlichen APP_ID der Web-app:

```
@equals('<APP_ID>', json(decodeBase64(triggerBody()['ContentData']))['identity']['claim']['appid'])
```

Dieser Ausdruck wird im Wesentlichen **false** zurückgeben, wenn die **Appid** -Eigenschaft vom eingehenden Ereignis (der Hauptteil der Nachricht Service Bus) nicht **Appid** der Anwendung ist. 

Erstellen Sie jetzt eine Aktion unter der Option _andernfalls nicht..._ .

![Azure Logik App wählen Sie Aktion](./media/keyvault-keyrotation/Azure_LogicApp_Condition.png)

Die Aktion wählen Sie _Office 365 - e-Mail_. Füllen Sie die Felder, erstellen Sie eine e-Mail senden, wenn die definierte Bedingung false zurückgibt. Haben Sie keine Office 365 könnte Sie alternativen zu gleich aussehen.

Zu diesem Zeitpunkt haben Sie eine Ende-Pipeline, die einmal pro Minute für neue Key Vault-Überwachungsprotokolle aussieht. Alle neuen Protokolle gefundenen, wird es sie Service Bus-Warteschlange senden an. Die Logik-App wird als eine neue Nachricht in der Warteschlange landet und nicht Appid innerhalb der app-Id der aufrufenden Anwendung entsprechen dann e-Mail ausgelöst. 
