<properties
   pageTitle="Erste Schritte mit See Datenspeicher plattformübergreifende Befehlszeilenschnittstelle | Microsoft Azure"
   description="Verwenden Sie Azure plattformübergreifende Befehlszeile See Datenspeicher registrieren und grundlegende Operationen"
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

# <a name="get-started-with-azure-data-lake-store-using-azure-command-line"></a>Erste Schritte mit Azure See Datenspeicher Azure-Befehlszeile

> [AZURE.SELECTOR]
- [Portal](data-lake-store-get-started-portal.md)
- [PowerShell](data-lake-store-get-started-powershell.md)
- [.NET SDK](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [REST-API](data-lake-store-get-started-rest-api.md)
- [Azure CLI](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)

Erfahren Sie, wie Azure Befehlszeilenschnittstelle Azure See Datenspeicher registrieren und grundlegende Vorgänge wie Ordner erstellen uploaden und Downloaden von Dateien verwenden, löschen Sie Ihr Konto usw.. Weitere Informationen über See Datenspeicher finden Sie unter [Übersicht über der See Datenspeicher](data-lake-store-overview.md).

Node.js ist der Azure-CLI implementiert. Sie können auf jeder Plattform verwendet werden, Node.js, einschließlich Windows, Mac und Linux unterstützt. Azure-CLI ist open Source. Der Quellcode wird in GitHub am <a href= "https://github.com/azure/azure-xplat-cli">https://github.com/azure/azure-xplat-cli</a>. Dieser Artikel behandelt nur See Datenspeicher mit der Azure-CLI. Eine allgemeine Anleitung zur Verwendung von Azure CLI finden Sie unter [Azure-CLI verwenden] [azure-command-line-tools].


##<a name="prerequisites"></a>Erforderliche Komponenten

Vor diesem Artikel benötigen Sie Folgendes:

- **Ein Azure-Abonnement**. Finden Sie [kostenlose Testversion von Azure zu erhalten](https://azure.microsoft.com/pricing/free-trial/).

- **Azure-CLI** - Siehe [Installieren und Konfigurieren der Azure-CLI](../xplat-cli-install.md) für Installations-und Konfigurationsinformationen. Stellen Sie sicher, dass Sie Ihren Computer neu, nachdem die CLI installiert.

## <a name="authentication"></a>Authentifizierung

In diesem Artikel verwendet Authentifizierung einfacher Datenspeicher See, wo Sie als Endbenutzer Benutzer anmelden. Zugriff auf See Datenspeicher Konto und Dateisystem die Zugriffsebene der Benutzer dann geregelt ist. Es gibt jedoch andere Ansätze zur Authentifizierung mit See Datenspeicher sowie die **Endbenutzer** oder **Dienst - Authentifizierung**. Anleitung und Weitere Informationen zur Authentifizierung finden Sie unter [Authentifizierung mit dem Datenspeicher Azure Active Directory verwenden](data-lake-store-authenticate-using-active-directory.md).

##<a name="login-to-your-azure-subscription"></a>Melden Sie Ihre Azure-Abonnement

1. Folgen Sie den Schritten in [Verbindung mit einer Azure-Abonnement aus der Azure-Befehlszeilenschnittstelle (CLI Azure)](../xplat-cli-connect.md) dokumentiert und verbinden Ihr Abonnement mit dem `azure login` Methode.

2. Liste der Abonnements, die mit Ihrem Konto verbunden sind die `azure account list` Befehl.

        info:    Executing command account list
        data:    Name              Id                                    Current
        data:    ----------------  ------------------------------------  -------
        data:    Azure-sub-1       ####################################  true
        data:    Azure-sub-2       ####################################  false

    Aus der obigen Ausgabe **Azure-Sub-1** aktiviert ist, und das Abonnement wird **Azure unter 2**. 

3. Wählen Sie das Abonnement unter arbeiten möchten. Abonnements Azure unter 2 arbeiten, verwenden Sie die `azure account set` Befehl.

        azure account set Azure-sub-2


## <a name="create-an-azure-data-lake-store-account"></a>Erstellen Sie ein Konto Azure See Datenspeicher

Öffnen Sie ein Eingabeaufforderungsfenster, Shell oder Terminaldienste und führen Sie die folgenden Befehle.

2. Wechseln Sie zur Azure-Ressourcen-Manager-Modus mit dem folgenden Befehl:

        azure config mode arm


5. Erstellen Sie eine neue Ressourcengruppe. Geben Sie folgenden Befehl Parameterwerte, die Sie verwenden möchten.

        azure group create <resourceGroup> <location>

    Wenn der Standortnamen Leerzeichen enthält, setzen Sie es in Anführungszeichen ein. Beispielsweise "USA Osten 2".

5. Registrieren Sie auf See Datenspeicher.

        azure datalake store account create <dataLakeStoreAccountName> <location> <resourceGroup>

## <a name="create-folders-in-your-data-lake-store"></a>Erstellen Sie Ordner im Datenspeicher See

Sie können Ordner erstellen, unter Ihrem Konto Azure See Datenspeicher verwalten und speichern. Verwenden Sie den folgenden Befehl zum Ordner "Mynewfolder" im Stammverzeichnis des Datenspeichers See erstellen.

    azure datalake store filesystem create <dataLakeStoreAccountName> <path> --folder

Zum Beispiel:

    azure datalake store filesystem create mynewdatalakestore /mynewfolder --folder

## <a name="upload-data-to-your-data-lake-store"></a>Upload von Daten auf See Datenspeicher

Sie können Ihre Daten hochladen, See Datenspeicher direkt auf der Stammebene oder einen Ordner, den das Konto erstellt. Die folgenden Ausschnitte veranschaulichen uploaden Beispieldaten auf den Ordner (**Mynewfolder**), die Sie im vorherigen Abschnitt erstellt.

Wenn Sie Beispieldaten hochladen suchen, erhalten Sie von [Azure Data Lake Git Repository](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData) **Krankenwagen** Datenordner. Downloaden Sie die Datei und speichern Sie es in ein lokales Verzeichnis auf dem Computer wie C:\sampledata\.

    azure datalake store filesystem import <dataLakeStoreAccountName> "<source path>" "<destination path>"

Zum Beispiel:

    azure datalake store filesystem import mynewdatalakestore "C:\SampleData\AmbulanceData\vehicle1_09142014.csv" "/mynewfolder/vehicle1_09142014.csv"


## <a name="list-files-in-data-lake-store"></a>Dateien im Datenspeicher See

Verwenden Sie den folgenden Befehl, die Dateien in einem Datenspeicher See Konto aufgelistet.

    azure datalake store filesystem list <dataLakeStoreAccountName> <path>

Zum Beispiel:

    azure datalake store filesystem list mynewdatalakestore /mynewfolder

Die Ausgabe sollte ähnlich der folgenden:

    info:    Executing command datalake store filesystem list
    data:    accessTime: 1446245025257
    data:    blockSize: 268435456
    data:    group: NotSupportYet
    data:    length: 1589881
    data:    modificationTime: 1446245105763
    data:    owner: NotSupportYet
    data:    pathSuffix: vehicle1_09142014.csv
    data:    permission: 777
    data:    replication: 0
    data:    type: FILE
    data:    ------------------------------------------------------------------------------------
    info:    datalake store filesystem list command OK

## <a name="rename-download-and-delete-data-from-your-data-lake-store"></a>Umbenennen, herunterladen und Löschen von Daten aus dem Datenspeicher See

* **Um eine Datei umzubenennen**, verwenden Sie folgenden Befehl:

        azure datalake store filesystem move <dataLakeStoreAccountName> <path/old_file_name> <path/new_file_name>

    Zum Beispiel:

        azure datalake store filesystem move mynewdatalakestore /mynewfolder/vehicle1_09142014.csv /mynewfolder/vehicle1_09142014_copy.csv

* **Herunterladen eine Datei**mithilfe des folgenden Befehls. Stellen Sie sicher, dass der Zielpfad bereits vorhanden ist.

        azure datalake store filesystem export <dataLakeStoreAccountName> <source_path> <destination_path>

    Zum Beispiel:

        azure datalake store filesystem export mynewdatalakestore /mynewfolder/vehicle1_09142014_copy.csv "C:\mysampledata\vehicle1_09142014_copy.csv"

* **Um eine Datei zu löschen**, verwenden Sie folgenden Befehl:

        azure datalake store filesystem delete <dataLakeStoreAccountName> <path>

    Zum Beispiel:

        azure datalake store filesystem delete mynewdatalakestore /mynewfolder/vehicle1_09142014_copy.csv

    Geben Sie bei Aufforderung **Y** zu löschen.

## <a name="view-the-access-control-list-for-a-folder-in-data-lake-store"></a>Anzeigen der Zugriffssteuerungsliste für einen Ordner im Datenspeicher See

Verwenden Sie folgenden Befehl die ACLs auf See Datenspeicher Ordner anzeigen. In der aktuellen Version können nur auf den Stamm des Datenspeichers See ACLs festgelegt werden. Parameters Pfad werden also immer Root (/).

    azure datalake store permissions show <dataLakeStoreName> <path>

Zum Beispiel:

    azure datalake store permissions show mynewdatalakestore /


## <a name="delete-your-data-lake-store-account"></a>Löschen Sie Ihr Konto See Datenspeicher

Verwenden Sie folgenden Befehl ein Datenspeicher See Konto löschen.

    azure datalake store account delete <dataLakeStoreAccountName>

Zum Beispiel:

    azure datalake store account delete mynewdatalakestore

Geben Sie bei Aufforderung **Y** ein, um das Konto zu löschen.


## <a name="next-steps"></a>Nächste Schritte

- [Sichere Daten im Datenspeicher See](data-lake-store-secure-data.md)
- [Verwenden Sie Azure Data Lake Analytics See Datenspeicher](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Verwenden von Azure HDInsight Lake Datenspeicher](data-lake-store-hdinsight-hadoop-use-portal.md)


[azure-command-line-tools]: ../xplat-cli-install.md
