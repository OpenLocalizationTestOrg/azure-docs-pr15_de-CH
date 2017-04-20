<properties 
   pageTitle="Erste Schritte mit Azure See Datenspeicher mit Azure SDK für Node.js | Microsoft Azure"
   description="Informationen Sie zum Node.js mit See Datenspeicher Konten und das Dateisystem verwenden." 
   services="data-lake-store" 
   documentationCenter="" 
   authors="nitinme" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="09/27/2016"
   ms.author="nitinme"/>

# <a name="get-started-with-azure-data-lake-store-using-azure-sdk-for-nodejs"></a>Erste Schritte mit Azure See Datenspeicher Azure SDK für Node.js

> [AZURE.SELECTOR]
- [Portal](data-lake-store-get-started-portal.md)
- [PowerShell](data-lake-store-get-started-powershell.md)
- [.NET SDK](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [REST-API](data-lake-store-get-started-rest-api.md)
- [Azure CLI](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)


Informationen Sie zum Azure SDK Node.js Azure See Datenspeicher registrieren und grundlegende Vorgänge wie Ordner erstellen hoch- und Herunterladen von Dateien, löschen Sie Ihr Konto usw. Weitere Informationen über See Datenspeicher finden Sie unter [Übersicht über der See Datenspeicher](data-lake-store-overview.md). Derzeit unterstützt das SDK

  *  **Node.js-Version: 0.10.0 oder höher**
  *  **REST API-Version für Konto: 2015-10-01-Vorschau**
  *  **REST API-Version für Dateisystem: 2015-10-01-Vorschau**

## <a name="prerequisites"></a>Erforderliche Komponenten

Vor diesem Artikel benötigen Sie Folgendes:

- **Ein Azure-Abonnement**. Finden Sie [kostenlose Testversion von Azure zu erhalten](https://azure.microsoft.com/pricing/free-trial/).

- **Erstellen einer Anwendung Azure Active Directory**. Mithilfe der Azure AD-Anwendung See Datenspeicher Anwendung in Azure AD authentifizieren. Es gibt verschiedene Ansätze bei Azure AD authentifizieren die **Endbenutzer** oder **Dienst - Authentifizierung**. Anleitung und Weitere Informationen zur Authentifizierung finden Sie unter [Authentifizierung mit dem Datenspeicher Azure Active Directory verwenden](data-lake-store-authenticate-using-active-directory.md).

## <a name="how-to-install"></a>Installieren von

```bash
npm install azure-arm-datalake-store
```

## <a name="authenticate-using-azure-active-directory"></a>Authentifizierung mit Active Directory Azure

Ausschnitte unten zeigen zwei Wege See Datenspeicher authentifizieren mit Azure AD. Ausführliche Informationen über verschiedene Methoden zur Authentifizierung mit See Datenspeicher Siehe [Authentifizierung mit dem Datenspeicher Azure Active Directory verwenden](data-lake-store-authenticate-using-active-directory.md).

Der folgende Ausschnitt erfordert auch Eingaben wie Azure AD Domänennamen, Client-ID für einen Azure AD app usw.. Diese Details können aus einer Azure AD-Anwendung, die Sie erstellt, abgerufen werden, die Details auch in oben enthalten sind.

 ```javascript
 var msrestAzure = require('ms-rest-azure');
 //user authentication
 var credentials = new msRestAzure.UserTokenCredentials('your-client-id', 'your-domain', 'your-username', 'your-password', 'your-redirect-uri');
 //service principal authentication
 var credentials = new msRestAzure.ApplicationTokenCredentials('your-client-id', 'your-domain', 'your-secret');
 ```

## <a name="create-the-data-lake-store-clients"></a>Datenspeicher See Clients erstellen

```javascript
var adlsManagement = require("azure-arm-datalake-store");
var acccountClient = new adlsManagement.DataLakeStoreAccountClient(credentials, "your-subscription-id");
var filesystemClient = new adlsManagement.DataLakeStoreFileSystemClient(credentials);
```

## <a name="create-a-data-lake-store-account"></a>Erstellen Sie ein Konto See Datenspeicher

```javascript
var util = require('util');
var resourceGroupName = 'testrg';
var accountName = 'testadlsacct';
var location = 'eastus2';

// account object to create
var accountToCreate = {
  tags: {
    testtag1: 'testvalue1',
    testtag2: 'testvalue2'
  },
  name: accountName,
  location: location
};

client.account.create(resourceGroupName, accountName, accountToCreate, function (err, result, request, response) {
  if (err) {
    console.log(err);
    /*err has reference to the actual request and response, so you can see what was sent and received on the wire.
      The structure of err looks like this:
      err: {
        code: 'Error Code',
        message: 'Error Message',
        body: 'The response body if any',
        request: reference to a stripped version of http request
        response: reference to a stripped version of the response
      }
    */
  } else {
    console.log('result is: ' + util.inspect(result, {depth: null}));
  }
});
```

## <a name="create-a-file-with-content"></a>Erstellen Sie eine Datei mit Inhalt
```javascript
var util = require('util');
var accountName = 'testadlsacct';
var fileToCreate = '/myfolder/myfile.txt';
var options = {
  streamContents: new Buffer('some string content')
}

filesystemClient.fileSystem.listFileStatus(accountName, fileToCreate, options, function (err, result, request, response) {
  if (err) {
    console.log(err);
  } else {
    // no result is returned, only a 201 response for success.
    console.log('response is: ' + util.inspect(response, {depth: null}));
  }
});
```

## <a name="get-a-list-of-files-and-folders"></a>Eine Liste von Dateien und Ordnern

```javascript
var util = require('util');
var accountName = 'testadlsacct';
var pathToEnumerate = '/myfolder';
filesystemClient.fileSystem.listFileStatus(accountName, pathToEnumerate, function (err, result, request, response) {
  if (err) {
    console.log(err);
  } else {
    console.log('result is: ' + util.inspect(result, {depth: null}));
  }
});
```

## <a name="see-also"></a>Siehe auch

- [Microsoft Azure SDK Node.js](https://github.com/azure/azure-sdk-for-node)
- [Microsoft Azure SDK Node.js - See Analytics Datenmanagement](https://www.npmjs.com/package/azure-arm-datalake-analytics)
