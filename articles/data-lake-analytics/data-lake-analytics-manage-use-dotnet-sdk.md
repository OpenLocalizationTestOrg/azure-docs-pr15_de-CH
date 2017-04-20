<properties 
   pageTitle="Verwalten von Azure Data Lake Analytics mit Azure .NET SDK | Azure" 
   description="Informationen Sie zum See Datenanalyse Aufträge, Datenquellen, Benutzer verwalten. " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="mumian" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="09/23/2016"
   ms.author="jgao"/>

# <a name="manage-azure-data-lake-analytics-using-azure-net-sdk"></a>Verwalten von Azure Data Lake Analytics mit Azure .NET SDK

[AZURE.INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

Informationen Sie zum Verwalten von Azure Data Lake Analytics Konten, Datenquellen, Benutzer und Aufträge mithilfe von Azure .NET SDK. Klicken Sie Themen mit anderen Tools die Registerkarte wählen oben.

**Erforderliche Komponenten**

Bevor Sie dieses Lernprogramm beginnen, müssen Sie Folgendes:

- **Ein Azure-Abonnement**. Finden Sie [kostenlose Testversion von Azure zu erhalten](https://azure.microsoft.com/pricing/free-trial/).


<!-- ################################ -->
<!-- ################################ -->


## <a name="connect-to-azure-data-lake-analytics"></a>Verbinden Sie mit Azure Data Lake Analytics

Sie benötigen die folgenden Nuget-Pakete:

    Install-Package Microsoft.Rest.ClientRuntime.Azure.Authentication -Pre
    Install-Package Microsoft.Azure.Common 
    Install-Package Microsoft.Azure.Management.ResourceManager -Pre
    Install-Package Microsoft.Azure.Management.DataLake.Analytics -Pre


Im folgenden Codebeispiel wird veranschaulicht, wie Azure und vorhandenen See Datenanalyse Konten unter Azure-Abonnement aufgeführt.

    using System;
    using System.Collections.Generic;
    using System.Threading;

    using Microsoft.Rest;
    using Microsoft.Rest.Azure.Authentication;

    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.Azure.Management.DataLake.Store;
    using Microsoft.Azure.Management.DataLake.Analytics;
    using Microsoft.Azure.Management.DataLake.Analytics.Models;

    namespace ConsoleAcplication1
    {
        class Program
        {

            private const string SUBSCRIPTIONID = "<Enter Your Azure Subscription ID>";
            private const string CLIENTID = "1950a258-227b-4e31-a9cf-717495945fc2";
            private const string DOMAINNAME = "common"; // Replace this string with the user's Azure Active Directory tenant ID or domain name, if needed.

            private static DataLakeAnalyticsAccountManagementClient _adlaClient;

            private static void Main(string[] args)
            {

                var creds = AuthenticateAzure(DOMAINNAME, CLIENTID);

                _adlaClient = new DataLakeAnalyticsAccountManagementClient(creds);
                _adlaClient.SubscriptionId = SUBSCRIPTIONID;

                var adlaAccounts = ListADLAAccounts();

                Console.WriteLine("You have %i Data Lake Analytics account(s).", adlaAccounts.Count);
                for (int i = 0; i < adlaAccounts.Count; i ++)
                {
                    Console.WriteLine(adlaAccounts[i].Name);
                }

                System.Console.WriteLine("Press ENTER to continue");
                System.Console.ReadLine();
            }

            public static ServiceClientCredentials AuthenticateAzure(
            string domainName,
            string nativeClientAppCLIENTID)
            {
                // User login via interactive popup
                SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());
                // Use the client ID of an existing AAD "Native Client" application.
                var activeDirectoryClientSettings = ActiveDirectoryClientSettings.UsePromptOnly(nativeClientAppCLIENTID, new Uri("urn:ietf:wg:oauth:2.0:oob"));
                return UserTokenProvider.LoginWithPromptAsync(domainName, activeDirectoryClientSettings).Result;
            }

            public static List<DataLakeAnalyticsAccount> ListADLAAccounts()
            {
                var response = _adlaClient.Account.List();
                var accounts = new List<DataLakeAnalyticsAccount>(response);

                while (response.NextPageLink != null)
                {
                    response = _adlaClient.Account.ListNext(response.NextPageLink);
                    accounts.AddRange(response);
                }

                return accounts;
            }
        }
    }


## <a name="manage-accounts"></a>Konten verwalten

Vor dem Ausführen von Datenanalysen See Aufträge, benötigen Sie ein Konto See Datenanalyse. Im Gegensatz zu Azure HDInsight bezahlen nicht Sie Analytics-Konto, wenn keinen Auftrag ausgeführt wird.  Sie Zahlen nur für die Zeit, wenn sie einen Auftrag ausgeführt wird.  Weitere Informationen finden Sie unter [Azure Data Lake Analytics Overview](data-lake-analytics-overview.md).  

###<a name="create-accounts"></a>Erstellen von Konten

Benötigen Sie ein Azure Ressource Verwaltungsgruppe und Datenspeicher See Konto vor dem Ausführen des folgenden Beispiels.

Der folgende Code zeigt, wie eine Ressourcengruppe erstellen:

    public static async Task<ResourceGroup> CreateResourceGroupAsync(
        ServiceClientCredentials credential,
        string groupName,
        string subscriptionId,
        string location)
    {

        Console.WriteLine("Creating the resource group...");
        var resourceManagementClient = new ResourceManagementClient(credential)
        { SubscriptionId = subscriptionId };
        var resourceGroup = new ResourceGroup { Location = location };
        return await resourceManagementClient.ResourceGroups.CreateOrUpdateAsync(groupName, resourceGroup);
    }

Im folgenden Codebeispiel wird ein Datenspeicher See Konto erstellen:

    var adlsParameters = new DataLakeStoreAccount(location: _location);
    _adlsClient.Account.Create(_resourceGroupName, _adlsAccountName, adlsParameters);

Der folgende Code zeigt, wie Data Lake Analytics-Konto erstellen:

    var defaultAdlsAccount = new List<DataLakeStoreAccountInfo> { new DataLakeStoreAccountInfo(adlsAccountName, new DataLakeStoreAccountInfoProperties()) };
    var adlaProperties = new DataLakeAnalyticsAccountProperties(defaultDataLakeStoreAccount: adlsAccountName, dataLakeStoreAccounts: defaultAdlsAccount);
    var adlaParameters = new DataLakeAnalyticsAccount(properties: adlaProperties, location: location);
    var adlaAccount = _adlaClient.Account.Create(resourceGroupName, adlaAccountName, adlaParameters);

###<a name="list-accounts"></a>Konten auflisten

[Verbinden mit dem Azure Datenanalyse](#connect_to_azure_data_lake_analytics)anzeigen

###<a name="find-an-account"></a>Suchen nach einer Firma

Man ein Objekt einer Datenanalyse See Kontenliste können folgenden Sie einem Konto suchen:

    Predicate<DataLakeAnalyticsAccount> accountFinder = (DataLakeAnalyticsAccount a) => { return a.Name == adlaAccountName; };
    var myAdlaAccount = adlaAccounts.Find(accountFinder);

###<a name="delete-data-lake-analytics-accounts"></a>Datenanalyse See löschen

Der folgende Codeausschnitt Löscht ein Konto See Datenanalyse:

    _adlaClient.Account.Delete(resourceGroupName, adlaAccountName);

<!-- ################################ -->
<!-- ################################ -->
## <a name="manage-account-data-sources"></a>Konto-Datenquellen verwalten

Datenanalyse See unterstützt derzeit die folgenden Datenquellen:

- [Azure See Datenspeicher](../data-lake-store/data-lake-store-overview.md)
- [Azure-Speicher](../storage/storage-introduction.md)

Beim Erstellen eines Analytics-Kontos müssen Sie ein Azure See Datenspeicher Konto das Standardkonto Speicher festlegen. Das Standardkonto See Datenspeicher zum Auftrag Metadaten und Auftrag Überwachungsprotokolle speichern. Nachdem Sie ein Analytics-Konto erstellt haben, können Sie zusätzliche Daten See Speicherkonten und Azure Storage-Konto hinzufügen. 

### <a name="find-the-default-data-lake-store-account"></a>Suchen Sie das Standardkonto See Datenspeicher

Finden Sie ein Konto in diesem Artikel die Datenanalyse See Konten. Verwenden Sie dann Folgendes:

    string adlaDefaultDataLakeStoreAccountName = myAccount.Properties.DefaultDataLakeStoreAccount;


## <a name="use-azure-resource-manager-groups"></a>Azure Ressourcenmanager Gruppen

Anwendung normalerweise viele Komponenten, z. B. eine Webanwendung, Datenbank Datenbank, Speicher und 3. Dienste bestehen. Azure Ressourcen-Manager können Sie mit den Ressourcen in der Anwendung als Gruppe als eine Azure-Ressourcengruppe arbeiten. Sie bereitstellen, aktualisieren, überwachen oder alle Ressourcen für die Anwendung in einem einzigen koordinierten Vorgang löschen. Verwenden Sie eine Vorlage für die Bereitstellung und die Vorlage kann für verschiedene Unternehmen wie Tests, Staging und Produktion. Abrechnung für Ihr Unternehmen zu klären die mehrstufigen Kosten für die gesamte Gruppe anzeigen. Weitere Informationen finden Sie unter [Azure-Ressourcen-Manager (Übersicht)](../azure-resource-manager/resource-group-overview.md). 

Datenanalyse See Service kann folgenden Komponenten umfassen:

- Azure Data Lake Analytics-Konto
- Erforderliche Azure See Datenspeicher Konto
- Zusätzliche Azure Data Lake Speicherkonten
- Zusätzliche Azure-Speicherkonten

Alle diese Komponenten im Rahmen einer Ressource Verwaltungsgruppe leichter verwalten können.

![Azure Data Lake Analytics Konto und Speicher](./media/data-lake-analytics-manage-use-portal/data-lake-analytics-arm-structure.png)

Ein Konto See Datenanalyse und abhängige Speicherkonten müssen im gleichen Azure-Rechenzentrum befinden.
Ressourcenmanagement-Gruppe kann jedoch in einem anderen Rechenzentrum befinden.  

##<a name="see-also"></a>Siehe auch 

- [Übersicht über Microsoft Azure Data Lake Analytics](data-lake-analytics-overview.md)
- [Erste Schritte mit See Datenanalyse mit Azure-portal](data-lake-analytics-get-started-portal.md)
- [Verwalten Sie Azure Data Lake Analytics mit Azure-portal](data-lake-analytics-manage-use-portal.md)
- [Überwachung und Problembehandlung von Azure Data Lake Analytics Aufträge mithilfe von Azure-portal](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)

