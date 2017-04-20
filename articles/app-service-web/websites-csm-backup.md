<properties
    pageTitle="Sichern und Wiederherstellen von App Service apps verwenden"
    description="Erfahren Sie, wie Sie RESTful API-Aufrufe zum Sichern und Wiederherstellen einer Anwendung in Azure App Service"
    services="app-service"
    documentationCenter=""
    authors="NKing92"
    manager="wpickett"
    editor="" />

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="nicking"/>
# <a name="use-rest-to-back-up-and-restore-app-service-apps"></a>Sichern und Wiederherstellen von App Service apps verwenden

> [AZURE.SELECTOR]
- [PowerShell](../app-service/app-service-powershell-backup.md)
- [REST-API](websites-csm-backup.md)

[App Service apps](https://azure.microsoft.com/services/app-service/web/) können als Blobs in Azure-Speicher gesichert werden. Die Sicherung kann auch die app-Datenbanken enthalten. Wenn die app einmal versehentlich gelöscht oder die Anwendung auf eine frühere Version wiederhergestellt werden muss, können sie aus einer vorherigen Sicherung wiederhergestellt werden. Backups können jederzeit bei Bedarf erfolgen oder Backups in geeigneten Abständen geplant werden können.

Sichern und Wiederherstellen einer Anwendung mit RESTful API erläutert. Möchten Sie erstellen und Verwalten von app Backups grafisch durch Azure-Portal, finden Sie unter [Sichern einer Web-app in Azure App Service](web-sites-backup.md)

<a name="gettingstarted"></a>
## <a name="getting-started"></a>Erste Schritte
Um weiteren Anfragen zu senden, müssen Sie Ihre app **Name** **Ressourcengruppe**und **Abonnement-Id**kennen. Diese Informationen kann durch Klicken auf die app in der **App** Blatt [Azure-Portal](https://portal.azure.com)gefunden werden. Für die Beispiele in diesem Artikel konfigurieren wir die Website **backuprestoreapiexamples.azurewebsites.net**. Sie werden in der Ressourcengruppe Standard-Web-WestUS und ein Abonnement mit der ID 00001111-2222-3333-4444-555566667777 ausgeführt wird.

![Beispiel-Webseite][SampleWebsiteInformation]

<a name="backup-restore-rest-api"></a>
## <a name="backup-and-restore-rest-api"></a>Sichern und Wiederherstellen von REST-API
Beispiele zur Verwendung von REST-API zum Sichern und Wiederherstellen einer Anwendung wird jetzt behandelt. Jedes Beispiel enthält einen URL und HTTP-Anforderungstext. Beispiel-URL enthält Platzhalter in geschweiften Klammern, z. B. {Abonnement-Id} eingeschlossen. Ersetzen Sie die Platzhalter mit den entsprechenden Informationen für Ihre Anwendung. Zu Referenzzwecken ist hier eine Erklärung jeder Platzhalter in der Beispiel-URLs angezeigt wird.

* Abonnement-Id-ID der Azure-Abonnement mit der Anwendung
* Ressource-Gruppenname – Name der Ressourcengruppe, die die Anwendung
* Name – Name der app
* Backup-Id – ID der app-Sicherung

Eine vollständige Dokumentation der API einschließlich verschiedene optionale Parameter, die die HTTP-Anforderung enthalten [Azure-Ressourcen-Explorer](https://resources.azure.com/)anzeigen

<a name="backup-on-demand"></a>
## <a name="backup-an-app-on-demand"></a>Sichern einer Anwendung bei Bedarf
Um eine Anwendung sofort sichern, senden Sie eine **POST** -Anforderung **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backup/**.

Sieht die URL wie unsere Beispiel-Website. **https://Management.Azure.com/Subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/Providers/Microsoft.Web/Sites/backuprestoreapiexamples/Backup/**

Geben Sie ein JSON-Objekt im Hauptteil der Anforderung an, welches Speicherkonto für die Sicherung verwenden. Das JSON-Objekt muss eine Eigenschaft mit dem Namen **StorageAccountUrl**, das Erteilen von Schreibzugriff auf den Azure-Speicher Container, backup-Blob [SAS-URL](../storage/storage-dotnet-shared-access-signature-part-1.md) enthält. Wenn Sie Ihre Datenbanken sichern möchten, müssen Sie auch eine Liste mit Namen, Typen und Verbindungszeichenfolgen von Datenbanken gesichert werden angeben.

```
{
    "properties":
    {
        "storageAccountUrl": “https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl”,
        “databases”: [
            {
                “databaseType”: “SqlAzure”,
                “name”: “MyDatabase1”,
                "connectionString": "<your database connection string>"
            }
        ]
    }
}
```

Eine Sicherung der Anwendung beginnt unmittelbar beim Empfangen der Anforderung. Der Sicherungsvorgang dauern abgeschlossen lange. Die HTTP-Antwort enthält eine ID, die Sie verwenden können, eine andere Anforderung den Status der Sicherung anzuzeigen. Hier ist ein Beispiel für den Hauptteil der HTTP-Antwort auf unsere backup-Anforderung.

```
{
    "id": "/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples",
    "name": " backuprestoreapiexamples ",
    "type": "Microsoft.Web/sites",
    "location": "WestUS",
    "properties":    {
        "id": 1,
        "storageAccountUrl": “https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl”,
        "blobName": “backup_201509291825.zip”,
        "name": "backup_201509291825",
        "status": 4,
        "sizeInBytes": 0,
        "created": "2015-09-29T18:25:26.3992707Z",
        "log": null,
        "databases": [
            {
                "databaseType": "SqlAzure",
                "name": "MyDatabase1",
                "connectionString": "<your database connection string>"
            }
        ],
        "scheduled": false,
        "correlationId": "ea730f4d-dd06-4f7f-a090-9aa09k662h36",
    }
}
```

>[AZURE.NOTE] Fehlermeldungen finden in der Log-Eigenschaft der HTTP-Antwort.

<a name="schedule-automatic-backups"></a>
## <a name="schedule-automatic-backups"></a>Automatisch Sicherungskopien
Neben einer Anwendung bei Bedarf sichern, können Sie auch eine Sicherung automatisch planen.

### <a name="set-up-a-new-automatic-backup-schedule"></a>Richten Sie einen neuen automatischen Sicherungsplans
Zum Einrichten eines Sicherungsplans Anfrage **PLATZIEREN** , **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/config/backup**.

Hier sieht die URL für die Webseite wird. **https://Management.Azure.com/Subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/Providers/Microsoft.Web/Sites/backuprestoreapiexamples/config/Backup**

Der Anforderungstext muss ein JSON-Objekt, der die Sicherungskonfiguration angibt. Hier ist ein Beispiel mit den erforderlichen Parametern.

```
{
    "location": "WestUS",
    "properties": // Represents an app restore request
    {
        "backupSchedule": { // Required for automatically scheduled backups
            "frequencyInterval": "7",
            "frequencyUnit": "Day",
            "keepAtLeastOneBackup": "True",
            "retentionPeriodInDays": "10",
        },
        "enabled": "True", // Must be set to true to enable automatic backups
        "name": “mysitebackup”,
        "storageAccountUrl": "https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl"
    }
}
```

Das folgende Beispiel konfiguriert die Anwendung alle sieben Tage automatisch gesichert werden. Parameter **FrequencyInterval** und **FrequencyUnit** zusammen bestimmen die Backups wie oft. Gültige Werte für **FrequencyUnit** sind **Stunden** und **Tage**. Stellen Sie FrequencyInterval z. B. zum Sichern einer Anwendung alle 12 Stunden bis 12 und FrequencyUnit Stunde.

Alte Backups werden automatisch aus dem Speicherkonto entfernt. Sie können steuern, wie ALT die Backups werden können, durch Festlegen des **RetentionPeriodInDays** -Parameters. Möchten Sie immer mindestens eine Sicherung gespeichert, unabhängig davon, wie ALT sie **KeepAtLeastOneBackup** auf True festgelegt ist.

### <a name="get-the-automatic-backup-schedule"></a>Abrufen der automatischen backup-Planung
Um eine app backup-Konfiguration zu erhalten, senden Sie eine **POST** -Anforderung an den URL **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/config/backup/list**.

Für unser Beispiel Website-URL ist **https://management.azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples/config/backup/list**.

<a name="get-backup-status"></a>
## <a name="get-the-status-of-a-backup"></a>Den Status der Sicherung
Wie groß die app ist kann eine Sicherung eine Weile dauern. Backups können auch nicht Timeout oder teilweise erfolgreich. Um alle app Backups anzuzeigen, senden Sie eine **GET** -Anforderung an den URL **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups**.

Um den Status einer bestimmten Sicherung finden Sie unter Senden einer GET-Anforderung an URL **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups/{backup-id}**.

Hier sieht die URL für die Webseite wird. **https://Management.Azure.com/Subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/Providers/Microsoft.Web/Sites/backuprestoreapiexamples/Backups/1**

Der Antworttext enthält ein JSON-Objekt entspricht dem folgenden Beispiel.

```
{
    "properties":  {
        "id": 1,
        "storageAccountUrl": "https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl",
        "blobName": "backup_201509291734.zip",
        "name": "backup_201509291734",
        "status": 2,
        "sizeInBytes": 151973,
        "created": "2015-09-29T17:34:57.2091605",
        "scheduled": false,
        "lastRestoreTimeStamp": null,
        "finishedTimeStamp": "2015-09-29T17:35:11.3293602",
        "correlationId": "2379fc13-418a-4b9c-920d-d06e73c5028d",
        "websiteSizeInBytes": 209920
    }
}
```

Der Status der Sicherung ist ein Enumerationstyp. Hier ist jeder möglichen Status.

* 0 – in Bearbeitung: die Sicherung gestartet wurde, jedoch noch nicht abgeschlossen.
* 1-Fehler: Die Sicherung ist fehlgeschlagen.
* 2-erfolgreich: Die Sicherung wurde erfolgreich abgeschlossen.
* 3 – TimedOut: Sicherung wurde nicht rechtzeitig abgeschlossen und wurde abgebrochen.
* 4 – erstellt: Backup Anforderung Warteschlange jedoch nicht gestartet.
* 5 – übersprungen: Die Sicherung aufgrund eines Zeitplans zu viele Backups auslösen nicht fortgesetzt.
* 6 – teilweise erfolgreich: Die Sicherung war erfolgreich, aber einige Dateien wurden nicht gesichert, da sie nicht gelesen werden konnte. Dies tritt normalerweise auf, weil eine exklusive Sperre für die Dateien platziert wurde.
* 7 – DeleteInProgress: Die Sicherung angefordert wurde gelöscht, aber noch nicht gelöscht wurde.
* 8-DeleteFailed: Die Sicherung konnte nicht gelöscht werden. Dies möglicherweise abgelaufen SAS-URL, die zum Erstellen der Sicherung verwendet wurde.
* 9 – gelöscht: Sicherung wurde erfolgreich gelöscht.

<a name="restore-app"></a>
## <a name="restore-an-app-from-a-backup"></a>Eine Anwendung aus einer Sicherung wiederherstellen
Wenn Ihre Anwendung gelöscht wurde oder wenn Sie Ihre Anwendung auf eine frühere Version wiederherstellen möchten, können Sie die Anwendung aus einer Sicherung wiederherstellen. Zum Aufrufen einer Wiederherstellung eine **POST** -Anforderung an den URL senden **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups/{backup-id}/restore**.

Hier sieht die URL für die Webseite wird. **https://Management.Azure.com/Subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/Providers/Microsoft.Web/Sites/backuprestoreapiexamples/Backups/1/Restore**

Senden Sie im Hauptteil Anforderung ein JSON-Objekt mit den Eigenschaften für den Wiederherstellungsvorgang. Hier ist ein Beispiel mit allen erforderlichen Eigenschaften:

```
{
    "location": "WestUS",
    "properties":
    {
        "blobName": "backup_201509280431.zip",
        "databases": [ // Not required unless you’re restoring databases
            {
                “databaseType”: “SqlAzure”,
                “name”: “MyDatabase1”
        }],
        "overwrite": "true",
        "storageAccountUrl": "https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl" // SAS URL to storage container containing your website backup
    }
}
```

### <a name="restore-to-a-new-app"></a>Wiederherstellen einer neuen Applikation
Möglicherweise möchten eine neue Anwendung erstellen, beim Wiederherstellen einer Sicherung statt eine bereits vorhandene Anwendung überschreiben. Hierzu ändern Sie Request-URL auf die neue Anwendung erstellen möchten, und die Eigenschaft **Überschreiben** in JSON auf **false**.

<a name="delete-app-backup"></a>
## <a name="delete-an-app-backup"></a>Löschen einer app-Sicherung
Möchten Sie eine Sicherung löschen, Anfrage **Löschen** zu URL **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups/{backup-id}**.

Hier sieht die URL für die Webseite wird. **https://Management.Azure.com/Subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/Providers/Microsoft.Web/Sites/backuprestoreapiexamples/Backups/1**

<a name="manage-sas-url"></a>
## <a name="manage-a-backups-sas-url"></a>Verwalten einer Sicherung SAS-URL
Azure App Service versucht, löschen die Sicherung von Azure Storage mithilfe SAS-URL, die beim Erstellen der Sicherung bereitgestellt. Wenn SAS-URL nicht mehr gültig ist, kann die Sicherung durch die REST-API gelöscht werden. Sie können jedoch der SAS-URL eine Sicherung durch Senden einer **POST** -Anforderung an den URL **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups/{backup-id}/list**.

Hier sieht die URL für die Webseite wird. **https://Management.Azure.com/Subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/Providers/Microsoft.Web/Sites/backuprestoreapiexamples/Backups/1/List**

Senden Sie im Hauptteil Anforderung ein JSON-Objekt, das die neuen SAS-URL enthält. Hier ist ein Beispiel.

```
{
    "properties":
    {
        "storageAccountUrl": "https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl"
    }
}
```

>[AZURE.NOTE] Aus Sicherheitsgründen wird der SAS-URL eine Sicherung beim Senden einer GET-Anforderung für ein bestimmtes Backup nicht zurückgegeben. Wenn Sie eine Sicherung der SAS-URL anzeigen möchten, Senden einer POST-Anforderung an denselben URL oben. Enthalten Sie ein leeres JSON-Objekt im Hauptteil Anforderung. Die Antwort vom Server enthält alle dieser Sicherung Informationen, einschließlich der SAS-URL.

<!-- IMAGES -->
[SampleWebsiteInformation]: ./media/websites-csm-backup/01siteconfig.png
