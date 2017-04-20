<properties 
   pageTitle="Erste Schritte mit See Datenspeicher REST API | Microsoft Azure" 
   description="Verwenden Sie WebHDFS REST-APIs, um Operationen auf See Datenspeicher" 
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

# <a name="get-started-with-azure-data-lake-store-using-rest-apis"></a>Erste Schritte mit Azure See Datenspeicher REST-APIs

> [AZURE.SELECTOR]
- [Portal](data-lake-store-get-started-portal.md)
- [PowerShell](data-lake-store-get-started-powershell.md)
- [.NET SDK](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [REST-API](data-lake-store-get-started-rest-api.md)
- [Azure CLI](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)

In diesem Artikel erfahren Sie, wie mit WebHDFS REST-APIs und Daten See Speicher REST APIs Account-Management sowie Dateisystemoperationen auf Azure See Datenspeicher. Azure See Datenspeicher macht den eigenen REST APIs für Verwaltungsoperationen Konto. See Datenspeicher ist kompatibel mit bietet und Hadoop Ökosystem unterstützt sie mit WebHDFS REST-APIs für Dateisystemoperationen.

>[AZURE.NOTE] Informationen über die REST-API See Datenspeicher unterstützt, finden Sie unter [Azure Data Lake Speicher REST-API-Referenz](https://msdn.microsoft.com/library/mt693424.aspx).

## <a name="prerequisites"></a>Erforderliche Komponenten

- **Ein Azure-Abonnement**. Finden Sie [kostenlose Testversion von Azure zu erhalten](https://azure.microsoft.com/pricing/free-trial/).

- **Erstellen einer Anwendung Azure Active Directory**. Mithilfe der Azure AD-Anwendung See Datenspeicher Anwendung in Azure AD authentifizieren. Es gibt verschiedene Ansätze bei Azure AD authentifizieren die **Endbenutzer** oder **Dienst - Authentifizierung**. Anleitung und Weitere Informationen zur Authentifizierung finden Sie unter [Authentifizierung mit dem Datenspeicher Azure Active Directory verwenden](data-lake-store-authenticate-using-active-directory.md).

- [Aufrollen](http://curl.haxx.se/). In diesem Artikel verwendet Aufrollen REST API-Aufrufe an ein Konto See Datenspeicher zu veranschaulichen.

## <a name="how-do-i-authenticate-using-azure-active-directory"></a>Wie werden ich mithilfe von Azure Active Directory authentifiziert?

Zwei Ansätze können Sie mithilfe von Azure Active Directory authentifizieren.

### <a name="end-user-authentication-interactive"></a>Endbenutzer-Authentifizierung (interaktiv)

In diesem Szenario die Anwendung fordert den Benutzer zum Anmelden und alle Operationen werden im Kontext des Benutzers ausgeführt. Die folgenden Schritte für interaktive Authentifizierung.

1. Leiten Sie durch die Anwendung den Benutzer zur folgenden URL:

        https://login.microsoftonline.com/<TENANT-ID>/oauth2/authorize?client_id=<CLIENT-ID>&response_type=code&redirect_uri=<REDIRECT-URI>

    >[AZURE.NOTE] \<Umleitung URI > für eine URL codiert werden müssen. Verwenden Sie daher für https://localhost, `https%3A%2F%2Flocalhost`)

    Sie können im Rahmen dieses Lernprogramms ersetzen Platzhalterwerte im URL oben und fügen Sie ihn in die Adressleiste des Webbrowsers. Sie werden zum Authentifizieren der Azure Anmeldung weitergeleitet. Nachdem Sie sich erfolgreich angemeldet haben, wird die Antwort in der Adressleiste des Browsers angezeigt. Die Antworten werden im folgenden Format:
        
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

In diesem Szenario die Anwendung stellt seine eigenen Anmeldeinformationen zur Ausführung der Vorgänge. Hierzu müssen Sie eine POST-Anforderung wie unten ausgeben. 

    curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
      -F grant_type=client_credentials \
      -F resource=https://management.core.windows.net/ \
      -F client_id=<CLIENT-ID> \
      -F client_secret=<AUTH-KEY>

Die Ausgabe dieser Anforderung enthalten Authentifizierungstoken (gekennzeichnet durch `access-token` in der folgenden Ausgabe), die Sie anschließend mit der REST-API-Aufrufe übergeben werden. Speichern Sie diese Authentifizierungstoken in einer Textdatei. Sie benötigen diese später in diesem Artikel.

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1458245447","not_before":"1458241547","resource":"https://management.core.windows.net/","access_token":"<REDACTED>"}

In diesem Artikel verwendet die **nicht interaktive** Ansatz. Weitere Informationen zu nicht interaktiven (Dienst-Aufrufe) finden Sie unter [Service Austauschakkus mit Anmeldeinformationen](https://msdn.microsoft.com/library/azure/dn645543.aspx).

## <a name="create-a-data-lake-store-account"></a>Erstellen Sie ein Konto See Datenspeicher

Dieser Vorgang basiert auf die REST-API definierten Aufruf [hier](https://msdn.microsoft.com/library/mt694078.aspx).

Verwenden Sie den folgenden cURL-Befehl. Ersetzen Sie ** \<Yourstorename >** mit Ihrem Namen See Datenspeicher.

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -H "Content-Type: application/json" https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.DataLakeStore/accounts/<yourstorename>?api-version=2015-10-01-preview -d@"C:\temp\input.json"

Ersetzen Sie im obigen Befehl \< `REDACTED` \> mit der Autorisierung abgerufen früher. Die Anforderungsnutzlast für diesen Befehl sind in der **input.json** , vorgesehen ist, die `-d` oben genannten Parameter. Der Inhalt der Datei input.json folgendermaßen aussehen:

    {
    "location": "eastus2",
    "tags": {
        "department": "finance"
        },
    "properties": {}
    }   

## <a name="create-folders-in-a-data-lake-store-account"></a>Ordner in einem Datenspeicher See-Konto erstellen

Dieser Vorgang basiert auf WebHDFS REST API Aufruf definiert [hier](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Make_a_Directory).

Verwenden Sie den folgenden cURL-Befehl. Ersetzen Sie ** \<Yourstorename >** mit Ihrem Namen See Datenspeicher.

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -d "" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/?op=MKDIRS

Ersetzen Sie im obigen Befehl \< `REDACTED` \> mit der Autorisierung abgerufen früher. Dieser Befehl erstellt ein Verzeichnis namens **Mytempdir** im Stammordner Ihres Kontos See Datenspeicher.

Sehen Sie Antwort wie folgt, wenn der Vorgang erfolgreich abgeschlossen wurde:

    {"boolean":true}

## <a name="list-folders-in-a-data-lake-store-account"></a>Ordner in einem Datenspeicher See Konto auflisten

Dieser Vorgang basiert auf WebHDFS REST API Aufruf definiert [hier](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#List_a_Directory).

Verwenden Sie den folgenden cURL-Befehl. Ersetzen Sie ** \<Yourstorename >** mit Ihrem Namen See Datenspeicher.

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/?op=LISTSTATUS

Ersetzen Sie im obigen Befehl \< `REDACTED` \> mit der Autorisierung abgerufen früher.

Sehen Sie Antwort wie folgt, wenn der Vorgang erfolgreich abgeschlossen wurde:

    {
    "FileStatuses": {
        "FileStatus": [{
            "length": 0,
            "pathSuffix": "mytempdir",
            "type": "DIRECTORY",
            "blockSize": 268435456,
            "accessTime": 1458324719512,
            "modificationTime": 1458324719512,
            "replication": 0,
            "permission": "777",
            "owner": "NotSupportYet",
            "group": "NotSupportYet"
        }]
    }
    }

## <a name="upload-data-into-a-data-lake-store-account"></a>Upload von Daten berücksichtigt See Datenspeicher

Dieser Vorgang basiert auf WebHDFS REST API Aufruf definiert [hier](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Create_and_Write_to_a_File).

Hochladen von Daten mithilfe der WebHDFS-REST-API ist ein zweistufiger Prozess wie unten beschrieben.

1. Fordern Sie HTTP PUT ohne die Daten übertragen werden. Ersetzen Sie den folgenden Befehl ** \<Yourstorename >** mit Ihrem Namen See Datenspeicher.

        curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -d "" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/?op=CREATE

    Die Ausgabe dieses Befehls werden enthält eine temporäre Umleitung URL wie unten.

        HTTP/1.1 100 Continue

        HTTP/1.1 307 Temporary Redirect
        ...
        ...
        Location: https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/somerandomfile.txt?op=CREATE&write=true
        ...
        ...

2. Sie müssen jetzt einen anderen HTTP PUT-Anforderung mit der URL für **die Eigenschaft in der Antwort** aufgeführten senden. Ersetzen Sie ** \<Yourstorename >** mit Ihrem Namen See Datenspeicher.

        curl -i -X PUT -T myinputfile.txt -H "Authorization: Bearer <REDACTED>" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=CREATE&write=true

    Die Ausgabe ist ähnlich der folgenden:

        HTTP/1.1 100 Continue

        HTTP/1.1 201 Created
        ...
        ...

## <a name="read-data-from-a-data-lake-store-account"></a>Lesen von Daten von einem See Datenspeicher

Dieser Vorgang basiert auf WebHDFS REST API Aufruf definiert [hier](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Open_and_Read_a_File).

Lesen von Daten aus einem Datenspeicher See ist Konto ein zweistufiger Prozess.

* Sie senden zunächst eine GET-Anforderung für den Endpunkt `https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN`. Den Speicherort der nächsten GET Antrag gibt zurück.
* Senden Sie die GET-Anforderung für den Endpunkt `https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN&read=true`. Dadurch wird der Inhalt der Datei angezeigt.

Da es keinen Unterschied zwischen dem ersten und dem zweiten Eingabeparameter gibt, Sie können jedoch die `-L` Parameter für die erste Anforderung. `-L`Option wird im Wesentlichen kombiniert zwei Anfragen in einem und cURL Anforderung an den neuen Speicherort wiederherstellen. Die Ausgabe der Anforderung aufrufen wird angezeigt, wie unten gezeigt. Ersetzen Sie ** \<Yourstorename >** mit Ihrem Namen See Datenspeicher.

    curl -i -L GET -H "Authorization: Bearer <REDACTED>" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN

Sie sollte eine Ausgabe ähnlich der folgenden angezeigt:

    HTTP/1.1 307 Temporary Redirect
    ...
    Location: https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/somerandomfile.txt?op=OPEN&read=true
    ...
    
    HTTP/1.1 200 OK
    ...
    
    Hello, Data Lake Store user!

## <a name="rename-a-file-in-a-data-lake-store-account"></a>Umbenennen einer Datei in einem Datenspeicher See Konto

Dieser Vorgang basiert auf WebHDFS REST API Aufruf definiert [hier](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Rename_a_FileDirectory).

Verwenden Sie den folgenden cURL-Befehl zum Umbenennen einer Datei. Ersetzen Sie ** \<Yourstorename >** mit Ihrem Namen See Datenspeicher.

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -d "" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=RENAME&destination=/mytempdir/myinputfile1.txt

Sie sollte eine Ausgabe ähnlich der folgenden angezeigt:

    HTTP/1.1 200 OK
    ...
    
    {"boolean":true}

## <a name="delete-a-file-from-a-data-lake-store-account"></a>Löschen einer Datei von einem Datenspeicher See Konto

Dieser Vorgang basiert auf WebHDFS REST API Aufruf definiert [hier](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Delete_a_FileDirectory).

Verwenden Sie den folgenden cURL-Befehl zum Löschen einer Datei. Ersetzen Sie ** \<Yourstorename >** mit Ihrem Namen See Datenspeicher.

    curl -i -X DELETE -H "Authorization: Bearer <REDACTED>" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile1.txt?op=DELETE

Sie sollte eine Ausgabe ähnlich der folgenden angezeigt:

    HTTP/1.1 200 OK
    ...
    
    {"boolean":true}

## <a name="delete-a-data-lake-store-account"></a>Datenspeicher See-Konto löschen

Dieser Vorgang basiert auf die REST-API definierten Aufruf [hier](https://msdn.microsoft.com/library/mt694075.aspx).

Befehl der folgenden cURL See Datenspeicher Konto löschen. Ersetzen Sie ** \<Yourstorename >** mit Ihrem Namen See Datenspeicher.

    curl -i -X DELETE -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.DataLakeStore/accounts/<yourstorename>?api-version=2015-10-01-preview

Sie sollte eine Ausgabe ähnlich der folgenden angezeigt:

    HTTP/1.1 200 OK
    ...
    ...

## <a name="see-also"></a>Siehe auch

- [Öffnen Sie Big Data Source Applikationen Azure See Datenspeicher mit](data-lake-store-compatible-oss-other-applications.md)
 
