<properties 
   pageTitle="Beginnen mit der Datenanalyse See mit REST-API | Microsoft Azure" 
   description="Verwenden Sie WebHDFS REST-APIs, um Operationen auf See Datenanalyse" 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="mumian" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="10/19/2016"
   ms.author="jgao"/>

# <a name="get-started-with-azure-data-lake-analytics-using-rest-apis"></a>Erste Schritte mit Azure Data Lake Analytics mit REST-APIs

[AZURE.INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Erfahren Sie, wie mit WebHDFS REST-APIs und Daten See Analytics REST APIs See Datenanalyse Firmen, Aufträge und Katalog verwalten. 

## <a name="prerequisites"></a>Erforderliche Komponenten

- **Ein Azure-Abonnement**. Finden Sie [kostenlose Testversion von Azure zu erhalten](https://azure.microsoft.com/pricing/free-trial/).

- **Erstellen einer Anwendung Azure Active Directory**. Mithilfe der Azure AD-Anwendung die Datenanalyse See Anwendung in Azure AD authentifizieren. Es gibt verschiedene Ansätze bei Azure AD authentifizieren die **Endbenutzer** oder **Dienst - Authentifizierung**. Anleitung und Weitere Informationen zur Authentifizierung finden Sie unter [See Datenanalyse mithilfe von Azure Active Directory zu authentifizieren](../data-lake-store/data-lake-store-authenticate-using-active-directory.md).

- [Aufrollen](http://curl.haxx.se/). In diesem Artikel verwendet Aufrollen REST API-Aufrufe an ein Konto See Datenanalyse zu veranschaulichen.

## <a name="authenticate-with-azure-active-directory"></a>Azure Active Directory authentifizieren

Es gibt zwei Methoden für die Authentifizierung bei Active Directory Azure.

### <a name="end-user-authentication-interactive"></a>Endbenutzer-Authentifizierung (interaktiv)

Mit dieser Methode Anwendung fordert den Benutzer zum Anmelden und alle Operationen werden im Kontext des Benutzers ausgeführt. 

Folgendermaßen Sie für interaktive Authentifizierung

1. Leiten Sie durch die Anwendung den Benutzer zur folgenden URL:

        https://login.microsoftonline.com/<TENANT-ID>/oauth2/authorize?client_id=<CLIENT-ID>&response_type=code&redirect_uri=<REDIRECT-URI>

    >[AZURE.NOTE] \<Umleitung URI > für eine URL codiert werden müssen. Verwenden Sie daher für https://localhost, `https%3A%2F%2Flocalhost`)

    Sie können im Rahmen dieses Lernprogramms ersetzen Platzhalterwerte im URL oben und fügen Sie ihn in die Adressleiste des Webbrowsers. Sie werden zum Authentifizieren der Azure Anmeldung weitergeleitet. Sobald Sie erfolgreich anmelden, wird die Antwort in der Adressleiste des Browsers angezeigt. Die Antworten werden im folgenden Format:
        
        http://localhost/?code=<AUTHORIZATION-CODE>&session_state=<GUID>

2. Erfassen Sie den Autorisierungscode aus der Antwort. Für dieses Lernprogramm können Sie den Autorisierungscode aus der Adressleiste des Webbrowsers kopieren und geben sie in die POST-Anforderung an den Endpunkt token wie folgt:

        curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token \
        -F redirect_uri=<REDIRECT-URI> \
        -F grant_type=authorization_code \
        -F resource=https://management.core.windows.net/ \
        -F client_id=<CLIENT-ID> \
        -F code=<AUTHORIZATION-CODE>

    >[AZURE.NOTE] In diesem Fall die \<Umleitung URI > müssen nicht codiert.

3. Die Antwort ist ein JSON-Objekt, ein Zugriffstoken enthält (z.B. `"access_token": "<ACCESS_TOKEN>"`) und aktualisieren (z.B. `"refresh_token": "<REFRESH_TOKEN>"`). Die Anwendung verwendet das Zugriffstoken beim Zugriff auf Azure See Datenspeicher und Aktualisierungstoken zu anderen Zugriffstoken Ablauf ein Zugriffstoken.

        {"token_type":"Bearer","scope":"user_impersonation","expires_in":"3599","expires_on":"1461865782","not_before": "1461861882","resource":"https://management.core.windows.net/","access_token":"<REDACTED>","refresh_token":"<REDACTED>","id_token":"<REDACTED>"}

4.  Ablauf des Zugriffstokens können Sie ein Zugriffstoken mit Aktualisierungstoken anfordern, wie unten dargestellt:

         curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
            -F grant_type=refresh_token \
            -F resource=https://management.core.windows.net/ \
            -F client_id=<CLIENT-ID> \
            -F refresh_token=<REFRESH-TOKEN>
 
Weitere Informationen zu interaktiven Benutzer finden Sie unter [Autorisierungscode Fluss gewähren](https://msdn.microsoft.com/library/azure/dn645542.aspx).

### <a name="service-to-service-authentication-non-interactive"></a>Dienst-Authentifizierung (interaktiv)

Diese Methode stellt die Anwendung eigene Anmeldeinformationen zur Ausführung der Vorgänge. Hierzu müssen Sie eine POST-Anforderung wie unten ausgeben: 

    curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
      -F grant_type=client_credentials \
      -F resource=https://management.core.windows.net/ \
      -F client_id=<CLIENT-ID> \
      -F client_secret=<AUTH-KEY>

Die Ausgabe dieser Anforderung enthalten Authentifizierungstoken (gekennzeichnet durch `access-token` in der folgenden Ausgabe), die Sie anschließend mit der REST-API-Aufrufe übergeben werden. Speichern Sie diese Authentifizierungstoken in einer Textdatei. Sie benötigen diese später in diesem Artikel.

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1458245447","not_before":"1458241547","resource":"https://management.core.windows.net/","access_token":"<REDACTED>"}

In diesem Artikel verwendet die **nicht interaktive** Ansatz. Weitere Informationen zu nicht interaktiven (Dienst-Aufrufe) finden Sie unter [Service Austauschakkus mit Anmeldeinformationen](https://msdn.microsoft.com/library/azure/dn645543.aspx).
## <a name="create-a-data-lake-analytics-account"></a>Erstellen Sie ein Konto See Datenanalyse

Bevor See Datenanalyse Konto erstellen können, müssen Sie eine Azure-Ressourcengruppe und Datenspeicher See-Konto erstellen.  Finden Sie unter [See Datenspeicher Konto erstellen](../data-lake-store/data-lake-store-get-started-rest-api.md#create-a-data-lake-store-account).

Folgende Curl-Befehl ist ein Konto erstellen:

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -H "Content-Type: application/json" https://management.azure.com/subscriptions/<AzureSubscriptionID>/resourceGroups/<AzureResourceGroupName>/providers/Microsoft.DataLakeAnalytics/accounts/<NewAzureDataLakeAnalyticsAccountName>?api-version=2016-11-01 -d@"C:\tutorials\adla\CreateDataLakeAnalyticsAccountRequest.json"

Ersetzen \< `REDACTED` \> mit Autorisierungstoken, \< `AzureSubscriptionID` \> mit Ihrem Abonnement-ID, \< `AzureResourceGroupName` \> mit einem vorhandenen Namen Azure-Ressourcengruppe und \< `NewAzureDataLakeAnalyticsAccountName` \> mit einem neuen Daten See Analytics-Kontonamen. Die Anforderungsnutzlast für diesen Befehl sind in der **CreateDatalakeAnalyticsAccountRequest.json** , vorgesehen ist, die `-d` oben genannten Parameter. Der Inhalt der Datei input.json folgendermaßen aussehen:

    {  
        "location": "East US 2",  
        "name": "myadla1004",  
        "tags": {},  
        "properties": {  
            "defaultDataLakeStoreAccount": "my1004store",  
            "dataLakeStoreAccounts": [  
                {  
                    "name": "my1004store"  
                }     
            ]
        }  
    }  


## <a name="list-data-lake-analytics-accounts-in-a-subscription"></a>Liste Data Lake Analytics Konten in einem Abonnement

Folgende Curl-Befehl ist wie Konten in einem Abonnement:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/<AzureSubscriptionID>/providers/Microsoft.DataLakeAnalytics/Accounts?api-version=2016-11-01

Ersetzen Sie \< `REDACTED` \> mit Autorisierungstoken, \< `AzureSubscriptionID` \> die Abonnement-ID Die Ausgabe ähnelt:

    {
        "value": [
            {
            "properties": {
                "provisioningState": "Succeeded",
                "state": "Active",
                "endpoint": "myadla0831.azuredatalakeanalytics.net",
                "accountId": "21e74660-0941-4880-ae72-b143c2615ea9",
                "creationTime": "2016-09-01T12:49:12.7451428Z",
                "lastModifiedTime": "2016-09-01T12:49:12.7451428Z"
            },
            "location": "East US 2",
            "tags": {},
            "id": "/subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/myadla0831rg/providers/Microsoft.DataLakeAnalytics/accounts/myadla0831",
            "name": "myadla0831",
            "type": "Microsoft.DataLakeAnalytics/accounts"
            },
            {
            "properties": {
                "provisioningState": "Succeeded",
                "state": "Active",
                "endpoint": "myadla1004.azuredatalakeanalytics.net",
                "accountId": "3ff9b93b-11c4-43c6-83cc-276292eeb350",
                "creationTime": "2016-10-04T20:46:42.287147Z",
                "lastModifiedTime": "2016-10-04T20:46:42.287147Z"
            },
            "location": "East US 2",
            "tags": {},
            "id": "/subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/myadla1004rg/providers/Microsoft.DataLakeAnalytics/accounts/myadla1004",
            "name": "myadla1004",
            "type": "Microsoft.DataLakeAnalytics/accounts"
            }
        ]
    }

## <a name="get-information-about-a-data-lake-analytics-account"></a>Informationen Sie zu einer Firma See Datenanalyse

Der folgende Curl-Befehl veranschaulicht ein Kontoinformationen:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/<AzureSubscriptionID>/resourceGroups/<AzureResourceGroupName>/providers/Microsoft.DataLakeAnalytics/accounts/<DataLakeAnalyticsAccountName>?api-version=2015-11-01

Ersetzen \< `REDACTED` \> mit Autorisierungstoken, \< `AzureSubscriptionID` \> mit Ihrem Abonnement-ID, \< `AzureResourceGroupName` \> mit einem vorhandenen Namen Azure-Ressourcengruppe und \< `DataLakeAnalyticsAccountName` \> mit dem Namen eines vorhandenen Daten See Analytics Kontos. Die Ausgabe ähnelt:

    {
        "properties": {
            "defaultDataLakeStoreAccount": "my1004store",
            "dataLakeStoreAccounts": [
            {
                "properties": {
                "suffix": "azuredatalakestore.net"
                },
                "name": "my1004store"
            }
            ],
            "provisioningState": "Creating",
            "state": null,
            "endpoint": null,
            "accountId": "3ff9b93b-11c4-43c6-83cc-276292eeb350",
            "creationTime": null,
            "lastModifiedTime": null
        },
        "location": "East US 2",
        "tags": {},
        "id": "/subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/myadla1004rg/providers/Microsoft.DataLakeAnalytics/accounts/myadla1004",
        "name": "myadla1004",
        "type": "Microsoft.DataLakeAnalytics/accounts"
    }

## <a name="list-data-lake-stores-of-a-data-lake-analytics-account"></a>Liste See Datenspeicher eines Kontos See Datenanalyse

Aufrollen Folgendes veranschaulicht Datenspeicher eines Kontos See Liste:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/<AzureSubscriptionID>/resourceGroups/<AzureResourceGroupName>/providers/Microsoft.DataLakeAnalytics/accounts/<DataLakeAnalyticsAccountName>/DataLakeStoreAccounts/?api-version=2016-11-01

Ersetzen \< `REDACTED` \> mit Autorisierungstoken, \< `AzureSubscriptionID` \> mit Ihrem Abonnement-ID, \< `AzureResourceGroupName` \> mit einem vorhandenen Namen Azure-Ressourcengruppe und \< `DataLakeAnalyticsAccountName` \> mit dem Namen eines vorhandenen Daten See Analytics Kontos. Die Ausgabe ähnelt:

    {
        "value": [
            {
            "properties": {
                "suffix": "azuredatalakestore.net"
            },
            "id": "/subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/myadla1004rg/providers/Microsoft.DataLakeAnalytics/accounts/myadla1004/dataLakeStoreAccounts/my1004store",
            "name": "my1004store",
            "type": "Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts"
            }
        ]
    }

## <a name="submit-u-sql-jobs"></a>U-SQL-Aufträge

Der folgende Curl-Befehl zeigt U-SQL-Auftrag senden:

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" https://<DataLakeAnalyticsAccountName>.azuredatalakeanalytics.net/Jobs/<NewGUID>?api-version=2016-03-20-preview -d@"C:\tutorials\adla\SubmitADLAJob.json"

Ersetzen Sie \< `REDACTED` \> mit Autorisierungstoken, \< `DataLakeAnalyticsAccountName` \> mit dem Namen eines vorhandenen Daten See Analytics Kontos. Die Anforderungsnutzlast für diesen Befehl sind in der **SubmitADLAJob.json** , vorgesehen ist, die `-d` oben genannten Parameter. Der Inhalt der Datei input.json folgendermaßen aussehen:

    {
        "jobId": "8f8ebf8c-4b63-428a-ab46-a03d2cc5b65a",
        "name": "convertTSVtoCSV",
        "type": "USql",
        "degreeOfParallelism": 1,
        "priority": 1000,
        "properties": {
            "type": "USql",
            "script": "@searchlog =\n    EXTRACT UserId          int,\n            Start           DateTime,\n            Region          string,\n            Query          
        string,\n            Duration        int?,\n            Urls            string,\n            ClickedUrls     string\n    FROM \"/Samples/Data/SearchLog.tsv\"\n    US
        ING Extractors.Tsv();\n\nOUTPUT @searchlog   \n    TO \"/Output/SearchLog-from-Data-Lake.csv\"\nUSING Outputters.Csv();"
        }
    }

Die Ausgabe ähnelt:

    {
        "jobId": "8f8ebf8c-4b63-428a-ab46-a03d2cc5b65a",
        "name": "convertTSVtoCSV",
        "type": "USql",
        "submitter": "myadl@SPI",
        "degreeOfParallelism": 1,
        "priority": 1000,
        "submitTime": "2016-10-05T13:54:59.9871859+00:00",
        "state": "Compiling",
        "result": "Succeeded",
        "stateAuditRecords": [
            {
            "newState": "New",
            "timeStamp": "2016-10-05T13:54:59.9871859+00:00",
            "details": "userName:myadl@SPI;submitMachine:N/A"
            }
        ],
        "properties": {
            "owner": "myadl@SPI",
            "resources": [],
            "runtimeVersion": "default",
            "rootProcessNodeId": "00000000-0000-0000-0000-000000000000",
            "algebraFilePath": "adl://myadls0831.azuredatalakestore.net/system/jobservice/jobs/Usql/2016/10/05/13/54/8f8ebf8c-4b63-428a-ab46-a03d2cc5b65a/algebra.xml",
            "compileMode": "Semantic",
            "errorSource": "Unknown",
            "totalCompilationTime": "PT0S",
            "totalPausedTime": "PT0S",
            "totalQueuedTime": "PT0S",
            "totalRunningTime": "PT0S",
            "type": "USql"
        }
    }


## <a name="list-u-sql-jobs"></a>U-SQL-Aufträge aufgelistet

Folgende Curl-Befehl zeigt U-SQL-Aufträge aufzulisten:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://<DataLakeAnalyticsAccountName>.azuredatalakeanalytics.net/Jobs?api-version=2016-11-01 

Ersetzen Sie \< `REDACTED` \> mit dem Autorisierungstoken und \< `DataLakeAnalyticsAccountName` \> mit dem Namen eines vorhandenen Daten See Analytics Kontos. 


Die Ausgabe ähnelt:

    {
    "value": [
        {
        "jobId": "65cf1691-9dbe-43cd-90ed-1cafbfb406fb",
        "name": "convertTSVtoCSV",
        "type": "USql",
        "submitter": "someone@microsoft.com",
        "account": null,
        "degreeOfParallelism": 1,
        "priority": 1000,
        "submitTime": "Wed, 05 Oct 2016 13:46:53 GMT",
        "startTime": "Wed, 05 Oct 2016 13:47:33 GMT",
        "endTime": "Wed, 05 Oct 2016 13:48:07 GMT",
        "state": "Ended",
        "result": "Succeeded",
        "errorMessage": null,
        "storageAccounts": null,
        "stateAuditRecords": null,
        "logFilePatterns": null,
        "properties": null
        },
        {
        "jobId": "8f8ebf8c-4b63-428a-ab46-a03d2cc5b65a",
        "name": "convertTSVtoCSV",
        "type": "USql",
        "submitter": "someoneadl@SPI",
        "account": null,
        "degreeOfParallelism": 1,
        "priority": 1000,
        "submitTime": "Wed, 05 Oct 2016 13:54:59 GMT",
        "startTime": "Wed, 05 Oct 2016 13:55:43 GMT",
        "endTime": "Wed, 05 Oct 2016 13:56:11 GMT",
        "state": "Ended",
        "result": "Succeeded",
        "errorMessage": null,
        "storageAccounts": null,
        "stateAuditRecords": null,
        "logFilePatterns": null,
        "properties": null
        }
    ],
    "nextLink": null,
    "count": null
    }


## <a name="get-catalog-items"></a>Katalog Elemente

Der folgende Curl-Befehl zeigt, wie Datenbanken aus dem Katalog zu:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://<DataLakeAnalyticsAccountName>.azuredatalakeanalytics.net/catalog/usql/databases?api-version=2016-11-01

Die Ausgabe ähnelt:

    {
    "@odata.context":"https://myadla0831.azuredatalakeanalytics.net/sqlip/$metadata#databases","value":[
        {
        "computeAccountName":"myadla0831","databaseName":"mytest","version":"f6956327-90b8-4648-ad8b-de3ff09274ea"
        },{
        "computeAccountName":"myadla0831","databaseName":"master","version":"e8bca908-cc73-41a3-9564-e9bcfaa21f4e"
        }
    ]
    }

## <a name="see-also"></a>Siehe auch

- Eine komplexe Abfrage finden Sie unter [Analysieren Website Protokolle mit Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).
- Finden Sie zunächst Anwendungsentwicklung U-SQL [entwickeln U-SQL-Skripts mit Data Lake-Tools für Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).
- U-SQL finden Sie unter [Einstieg in Azure Data Lake Analytics U-SQL-Sprache](data-lake-analytics-u-sql-get-started.md).
- Management-Aufgaben finden Sie unter [Verwalten von Azure Data Lake Analytics verwenden Azure-Portal](data-lake-analytics-manage-use-portal.md).
- Um einen Überblick der Datenanalyse See Übersicht [Azure Data Lake Analytics](data-lake-analytics-overview.md).
- Klicken Sie das gleiche Lernprogramm mit anderen Tools Registerkarte Selektoren oben auf der Seite.
- Diagnose-Informationen protokollieren finden Sie unter [Accessing Diagnoseprotokolle für Azure Data Lake Analytics](data-lake-analytics-diagnostic-logs.md)
