<properties
    pageTitle="Konto Ressourcenmanagement mit Batch Management | Microsoft Azure"
    description="Erstellen, löschen und Azure Batch kontoressourcen mit Batch Management .NET Bibliothek ändern."
    services="batch"
    documentationCenter=".net"
    authors="mmacy"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="batch"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="big-compute"
    ms.date="10/19/2016"
    ms.author="marsma"/>

# <a name="manage-azure-batch-accounts-and-quotas-with-batch-management-net"></a>Verwalten von Azure Batch Konten und Kontingente mit Batch Management

> [AZURE.SELECTOR]
- [Azure-portal](batch-account-create-portal.md)
- [Batchverwaltung .NET](batch-management-dotnet.md)

Sie können niedrigere Wartung Aufwand in Azure Batch Clientanwendungen mit [Batch Management.NET] [ api_mgmt_net] Bibliothek Batch-Konto erstellen, löschen, Schlüsselmanagement und Kontingent Discovery automatisieren.

- **Erstellen und Löschen von Konten Batch** in jeder Region. Wenn Sie als unabhängiger Softwareanbieter (ISV) beispielsweise einen Dienst für Ihre Kunden bieten Ihnen in denen jeweils ein separates Konto Batch für Zahlungszwecke zugewiesen wird, können Sie Ihre Kundenportal Konto erstellen und Löschen von Funktionen hinzufügen.
- **Abrufen und Regenerieren kontoschlüssel** programmgesteuert Batch Konten. Dadurch können Sie die Einhaltung von Sicherheitsrichtlinien, die periodische Rollover oder Ablauf kontoschlüssel erzwungen. Wenn Sie mehrere Stapel Konten in Azure Regionen haben, Stärkere Automatisierung dabei Rollover Effizienz Ihrer Lösung.
- **Konto Quoten prüfen** und nehmen Versuch und Irrtum Rätselraten bestimmen die Batch-Konten welcher Grenzen. Überprüfen Sie die Konto-Kontingente vor Aufträge, Pools erstellen und Hinzufügen von Compute-Knoten können proaktiv Where oder anpassen beim Berechnen dieser Ressourcen erstellt. Sie können bestimmen, welche Konten müssen Quote erhöht, bevor zusätzliche Ressourcen auf diesen Konten.
- **Kombinieren von anderen Azure Services** für eine umfassende Erfahrung – mit Batch Management .NET [Azure Active Directory][aad_about], und der [Azure-Ressourcen-Manager] [ resman_overview] zusammen in derselben Anwendung. Diese Features und ihre APIs verwenden, können Sie eine reibungslose Authentifizierungsvorgang die Möglichkeit zum Erstellen und Löschen von Ressourcengruppen und die Funktionen, die oben für eine End-to-End-Lösung bereitstellen.

> [AZURE.NOTE] Konzentriert sich dieser Artikel auf die programmgesteuerte Verwaltung Batch-Konten, Schlüssel und Kontingente können diesen Aktivitäten mithilfe der [Azure-Portal]ausführen[azure_portal]. Weitere Informationen finden Sie unter [erstellen mnmthCreatinganAccount Azure Batch Azure-Portal](batch-account-create-portal.md) und [Kontingente und Grenzwerte für Azure Batch-Dienst](batch-quota-limit.md).

## <a name="create-and-delete-batch-accounts"></a>Erstellen und Löschen von Batch-Konten

Wie bereits erwähnt, ist eines der Hauptfeatures der Batch-API erstellen und Löschen von Konten in Azure-Region Batch. Dazu verwenden Sie [BatchManagementClient.Account.CreateAsync] [ net_create] und [DeleteAsync][net_delete], oder ihren synchronen Gegenstücken.

Der folgende Codeausschnitt erstellt ein Benutzerkonto, erhält das neu erstellte Konto von Batch-Dienst und anschließend gelöscht. In diesem Ausschnitt und anderen in diesem Artikel `batchManagementClient` ist eine vollständig initialisierte Instanz [BatchManagementClient][net_mgmt_client].

```csharp
// Create a new Batch account
await batchManagementClient.Account.CreateAsync("MyResourceGroup",
    "mynewaccount",
    new BatchAccountCreateParameters() { Location = "West US" });

// Get the new account from the Batch service
AccountResource account = await batchManagementClient.Account.GetAsync(
    "MyResourceGroup",
    "mynewaccount");

// Delete the account
await batchManagementClient.Account.DeleteAsync("MyResourceGroup", account.Name);
```

> [AZURE.NOTE] Batch Management .NET Library und die BatchManagementClient-Klasse verwenden, müssen **Dienstadministrator** oder **Coadministrator** Zugriff auf das Abonnement, das besitzt das Batch-Konto verwaltet werden. Weitere Informationen finden Sie im Abschnitt [Azure Active Directory](#azure-active-directory) und [AccountManagement] [ acct_mgmt_sample] Beispiel.

## <a name="retrieve-and-regenerate-account-keys"></a>Laden Sie und zu regenerieren Sie kontoschlüssel

Primäre und sekundäre kontoschlüssel von Batch-Konto innerhalb Ihres Abonnements erhalten, indem Sie [ListKeysAsync][net_list_keys]. Regeneriert die Schlüssel mithilfe von [RegenerateKeyAsync][net_regenerate_keys].

```csharp
// Get and print the primary and secondary keys
BatchAccountListKeyResult accountKeys =
    await batchManagementClient.Account.ListKeysAsync(
        "MyResourceGroup",
        "mybatchaccount");
Console.WriteLine("Primary key:   {0}", accountKeys.Primary);
Console.WriteLine("Secondary key: {0}", accountKeys.Secondary);

// Regenerate the primary key
BatchAccountRegenerateKeyResponse newKeys =
    await batchManagementClient.Account.RegenerateKeyAsync(
        "MyResourceGroup",
        "mybatchaccount",
        new BatchAccountRegenerateKeyParameters() {
            KeyName = AccountKeyType.Primary
            });
```

> [AZURE.TIP] Sie können einen optimierte Verbindung Workflow für Ihre Verwaltung erstellen. Erstens erhalten einen kontoschlüssel für die Batch-Konto [ListKeysAsync]verwalten möchten[net_list_keys]. Verwenden Sie diesen Schlüssel dann bei der Stapelverarbeitung .NET Library [BatchSharedKeyCredentials] [ net_sharedkeycred] Klasse, die verwendet wird, bei [BatchClient][net_batch_client].

## <a name="check-azure-subscription-and-batch-account-quotas"></a>Azure-Abonnements und Batch-Konto Kontingente überprüfen

Azure-Abonnements und einzelne Azure Services wie alle Batch? Standardkontingente, die bestimmte Elemente innerhalb dieser Anzahl Die Standardkontingente für Azure-Abonnements finden Sie unter [Azure-Abonnement und Service Grenzen, Kontingente und Einschränkungen](./../azure-subscription-service-limits.md). Die Standardkontingente des Batch-Diensts finden Sie unter [Kontingente und Grenzwerte für Azure Batch-Dienst](batch-quota-limit.md). Überprüfen Sie mithilfe der Bibliothek Batch Management .NET diese Kontingente in Ihrer Anwendung. Dadurch werden Entscheidungen vor dem Hinzufügen von Konten oder compute-Ressourcen wie Pools und compute-Knoten.

### <a name="check-an-azure-subscription-for-batch-account-quotas"></a>Azure-Abonnement für Batch-Konto Kontingente überprüfen

Vor der Erstellung eines Batch in einer Region, können Sie Ihre Azure-Abonnement um festzustellen, ob Sie ein Konto in der betreffenden Region hinzufügen überprüfen.

Im folgenden Codeausschnitt verwenden wir zuerst [BatchManagementClient.Account.ListAsync] [ net_mgmt_listaccounts] eine Auflistung aller Stapel Konten zu, die innerhalb eines Abonnements. Nachdem wir diese Auflistung erhalten haben, bestimmen wir wie viele Konten in den Zielbereich. Dann wir [BatchManagementClient.Subscriptions verwenden] [ net_mgmt_subscriptions] Kontingent Konto Batch und bestimmen, wie viele Konten (falls vorhanden) in dieser Region erstellt werden können.

```csharp
// Get a collection of all Batch accounts within the subscription
BatchAccountListResponse listResponse =
        await batchManagementClient.Account.ListAsync(new AccountListParameters());
IList<AccountResource> accounts = listResponse.Accounts;
Console.WriteLine("Total number of Batch accounts under subscription id {0}:  {1}",
    creds.SubscriptionId,
    accounts.Count);

// Get a count of all accounts within the target region
string region = "westus";
int accountsInRegion = accounts.Count(o => o.Location == region);

// Get the account quota for the specified region
SubscriptionQuotasGetResponse quotaResponse = await batchManagementClient.Subscriptions.GetSubscriptionQuotasAsync(region);
Console.WriteLine("Account quota for {0} region: {1}", region, quotaResponse.AccountQuota);

// Determine how many accounts can be created in the target region
Console.WriteLine("Accounts in {0}: {1}", region, accountsInRegion);
Console.WriteLine("You can create {0} accounts in the {1} region.", quotaResponse.AccountQuota - accountsInRegion, region);
```

Im obigen Codeausschnitt `creds` eine Instanz von [TokenCloudCredentials][azure_tokencreds]. Ein Beispiel zum Erstellen dieses Objekts finden Sie unter ["AccountManagement"] [ acct_mgmt_sample] Codebeispiel auf GitHub.

### <a name="check-a-batch-account-for-compute-resource-quotas"></a>Ein Batch-Konto für Compute ressourcenkontingente überprüfen

Bevor computeressourcen in der Stapelverarbeitung Lösung erhöhen, können Sie dafür Ressourcen reservieren möchten das Konto Kontingente überschreiten wird nicht überprüfen. Im folgenden Codeausschnitt Drucken wir die Kontingentinformationen für das Batch-Konto mit dem Namen `mybatchaccount`. Solche Informationen können Sie in Ihrer Anwendung um zu bestimmen, ob das Konto zusätzlichen Ressourcen zu erstellenden verarbeiten kann.

```csharp
// First obtain the Batch account
BatchAccountGetResponse getResponse =
    await batchManagementClient.Account.GetAsync("MyResourceGroup", "mybatchaccount");
AccountResource account = getResponse.Resource;

// Now print the compute resource quotas for the account
Console.WriteLine("Core quota: {0}", account.Properties.CoreQuota);
Console.WriteLine("Pool quota: {0}", account.Properties.PoolQuota);
Console.WriteLine("Active job and job schedule quota: {0}", account.Properties.ActiveJobAndJobScheduleQuota);
```

> [AZURE.IMPORTANT] Standardkontingente für Azure-Abonnements und Dienstleistungen sind, viele dieser Grenzen ausgelöst werden können eine Anforderung in der [Azure-Portal][azure_portal]. Beispielsweise finden Sie unter [Kontingente und Grenzwerte für Azure Batch-Dienst](batch-quota-limit.md) Hinweise auf die Stapelverarbeitung Konto Kontingente erhöhen.

## <a name="batch-management-net-azure-ad-and-resource-manager"></a>Batch-Management .NET Azure AD und Ressourcenmanager

[Beim Arbeiten mit der Bibliothek Batch Management .NET normalerweise auch Azure Active Directory] verwenden[ aad_about] (Azure AD) und [Azure Resource Manager][resman_overview]. Erläutert das Beispielprojekt verwendet Azure Active Directory und der Ressourcen-Manager während der Batch-.NET API veranschaulicht.

### <a name="azure-active-directory"></a>Azure Active Directory

Azure Azure AD für die Authentifizierung von Kunden, Administratoren und Organisationseinheit Benutzer verwendet. Im Rahmen der Stapelverarbeitung Management .NET verwenden Sie Azure AD ein Abonnement oder Co-Administrator zu authentifizieren. Dadurch Management Library Abfragen des Batch-Dienstes und führen Sie die in diesem Artikel beschriebenen Vorgänge.

Im Beispielprojekt erläutert, Azure [Active Directory Authentifizierungsbibliothek] [ aad_adal] (ADAL) wird verwendet, um ihre Microsoft-Anmeldeinformationen abgefragt. Beim Administrator oder Coadministrator Anmeldeinformationen angegeben werden, kann die Anwendung Azure eine Liste der Abonnements - Abfragen können erstellen und Ressourcengruppen und Batch-Konten löschen.

### <a name="resource-manager"></a>Ressourcen-Manager

Beim Erstellen von Batch-Konten mit Batch Management .NET Library werden in der Regel erstellen Sie sie innerhalb einer [Ressourcengruppe][resman_overview]. Können die Ressourcengruppe programmgesteuert mithilfe von [ResourceManagementClient] [ resman_client] Klasse im [Ressourcenmanager .NET] [ resman_api] Bibliothek. Oder eine vorhandene Ressourcengruppe, die zuvor mithilfe der [Azure-Portal]erstellt ein Konto hinzufügen[azure_portal].

## <a name="sample-project-on-github"></a>Beispielprojekt auf GitHub

Batch Management .NET in Aktion überprüfen, [AccountManagment] [ acct_mgmt_sample] -Beispielprojekt auf GitHub. Diese Konsolenanwendung veranschaulicht die Erstellung und Verwendung von [BatchManagementClient] [ net_mgmt_client] und [ResourceManagementClient][resman_client]. Veranschaulicht auch die Verwendung von Azure [Active Directory Authentifizierungsbibliothek] [ aad_adal] (ADAL), die beide Clients erforderlich ist.

Zum Ausführen von Beispiel-Anwendung müssen Sie zuerst mit Azure registrieren, mithilfe des Azure-Portals. Folgen Sie den Schritten im Abschnitt [Anwendung](../active-directory/active-directory-integrating-applications.md#adding-an-application) in [Azure Active Directory Integration Anwendung] [ aad_integrate] Beispiel in Ihr eigenes Konto registrieren des Standard-Verzeichnis. **Systemeigene Clientanwendung** für die Anwendung wählen und ein beliebiger gültigen URI angeben (z. B. `http://myaccountmanagementsample`) für den **URI umleiten**– es muss kein echte Endpunkt.

Nach dem Hinzufügen der Anwendung Delegieren der Berechtigung **Zugriff Azure Service-Management in Unternehmen** *Windows Azure Service Management API* -Anwendung in den Einstellungen im Portal:

![Berechtigungen in Azure-portal][2]

> [AZURE.TIP] Wenn **Windows Azure Service Management-API** unter *Berechtigungen für andere Programme*nicht angezeigt wird, klicken Sie auf **Anwendung hinzufügen**, **Windows Azure Service Management-API**, klicken Sie auf die Schaltfläche aktivieren. Delegieren Sie Berechtigungen wie oben angegeben.

Sobald Sie eine Anwendung wie oben beschrieben hinzugefügt haben, `Program.cs` in [AccountManagment] [ acct_mgmt_sample] Beispielprojekt mit Ihrer Anwendung URI umleiten und Client-ID. Diese Werte finden Sie in der Registerkarte **Konfigurieren** der Anwendung:

![Anwendungskonfiguration in Azure-portal][3]

[AccountManagment] [ acct_mgmt_sample] Beispiel-Anwendung veranschaulicht die folgenden Vorgänge:

1. Erwerben Sie ein Sicherheitstoken von Azure AD mit [ADAL][aad_adal]. Wenn der Benutzer nicht angemeldet ist, werden sie ihre Azure-Anmeldeinformationen aufgefordert.
2. Erstellen Sie mithilfe von Azure AD abgerufen Sicherheitstoken [SubscriptionClient] [ resman_subclient] Abfrage Azure eine Liste der dem Konto zugeordneten Abonnements. Dadurch wird an ein Abonnement, wenn mehrere gefunden.
3. Erstellen Sie das ausgewählte Abonnement zugeordneten Anmeldeinformationsobjekt.
4. Erstellen von [ResourceManagementClient] [ resman_client] mithilfe der Anmeldeinformationen.
5. Verwenden Sie [ResourceManagementClient] [ resman_client] eine Ressourcengruppe erstellen.
6. Verwenden Sie [BatchManagementClient] [ net_mgmt_client] mehrere Stapel Konto Operationen durchführen:
  - Erstellen einer Batch-Konto in die neue Ressourcengruppe.
  - Das neu erstellte Konto aus der Batch-Dienst abrufen.
  - Drucken Sie die kontoschlüssel für das neue Konto.
  - Generieren Sie einen neuen Primärschlüssel für das Konto erneut.
  - Drucken Sie die Kontingentinformationen für das Konto.
  - Drucken Sie die Kontingentinformationen für das Abonnement.
  - Drucken Sie alle Konten innerhalb des Abonnements.
  - Löschen Sie neu erstelltes Konto
7. Löschen Sie die Ressourcengruppe aus.

Bevor Sie die neu erstellte Konto und Ressource Gruppe löschen, überprüfen Sie in [Azure-Portal][azure_portal]:

![Azure-Portal zur Anzeige der Ressourcengruppe und Batch-Konto][1]
<br />
*Azure-Portal anzeigen, neue Ressourcengruppe und Batch-Konto*

[aad_about]: ../active-directory/active-directory-whatis.md "Was ist Azure Active Directory?"
[aad_adal]: ../active-directory/active-directory-authentication-libraries.md
[aad_auth_scenarios]: ../active-directory/active-directory-authentication-scenarios.md "Authentifizierungsszenarien Azure AD"
[aad_integrate]: ../active-directory/active-directory-integrating-applications.md "Integration von Applikationen in Azure Active Directory"
[acct_mgmt_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/AccountManagement
[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_mgmt_net]: https://msdn.microsoft.com/library/azure/mt463120.aspx
[azure_portal]: http://portal.azure.com
[azure_storage]: https://azure.microsoft.com/services/storage/
[azure_tokencreds]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.tokencloudcredentials.aspx
[batch_explorer_project]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[net_batch_client]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_list_keys]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.accountoperationsextensions.listkeysasync.aspx
[net_create]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.accountoperationsextensions.createasync.aspx
[net_delete]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.accountoperationsextensions.deleteasync.aspx
[net_regenerate_keys]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.accountoperationsextensions.regeneratekeyasync.aspx
[net_sharedkeycred]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.auth.batchsharedkeycredentials.aspx
[net_mgmt_client]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.batchmanagementclient.aspx
[net_mgmt_subscriptions]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.batchmanagementclient.subscriptions.aspx
[net_mgmt_listaccounts]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.iaccountoperations.listasync.aspx
[resman_api]: https://msdn.microsoft.com/library/azure/mt418626.aspx
[resman_client]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.resources.resourcemanagementclient.aspxs
[resman_subclient]: https://msdn.microsoft.com/library/azure/microsoft.azure.subscriptions.subscriptionclient.aspx
[resman_overview]: ../azure-resource-manager/resource-group-overview.md

[1]: ./media/batch-management-dotnet/portal-01.png
[2]: ./media/batch-management-dotnet/portal-02.png
[3]: ./media/batch-management-dotnet/portal-03.png
