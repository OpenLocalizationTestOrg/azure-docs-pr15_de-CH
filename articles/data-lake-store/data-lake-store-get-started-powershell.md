<properties
   pageTitle="Erste Schritte mit See Datenspeicher | Azure"
   description="Mithilfe von Azure PowerShell See Datenspeicher registrieren und grundlegende Operationen"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/04/2016"
   ms.author="nitinme"/>

# <a name="get-started-with-azure-data-lake-store-using-azure-powershell"></a>Erste Schritte mit Azure See Datenspeicher Azure PowerShell

> [AZURE.SELECTOR]
- [Portal](data-lake-store-get-started-portal.md)
- [PowerShell](data-lake-store-get-started-powershell.md)
- [.NET SDK](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [REST-API](data-lake-store-get-started-rest-api.md)
- [Azure CLI](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)

Erfahren Sie, wie mithilfe von Azure PowerShell Azure See Datenspeicher registrieren und grundlegende Vorgänge wie Ordner erstellen hoch- und Herunterladen von Dateien, löschen Sie Ihr Konto usw.. Weitere Informationen über See Datenspeicher finden Sie unter [Übersicht über der See Datenspeicher](data-lake-store-overview.md).

## <a name="prerequisites"></a>Erforderliche Komponenten

Bevor Sie dieses Lernprogramm beginnen, müssen Sie Folgendes:

* **Ein Azure-Abonnement**. Finden Sie [kostenlose Testversion von Azure zu erhalten](https://azure.microsoft.com/pricing/free-trial/).

* **Azure PowerShell 1.0 oder höher**. Informationen Sie [zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md).

## <a name="authentication"></a>Authentifizierung

In diesem Artikel verwendet einfacher Authentifizierung mit See Datenspeicher, in dem Sie aufgefordert werden, Ihre Azure-Konto-Anmeldeinformationen eingeben. Zugriff auf See Datenspeicher Konto und Dateisystem die Zugriffsebene der Benutzer dann geregelt ist. Es gibt jedoch andere Ansätze zur Authentifizierung mit See Datenspeicher sowie die **Endbenutzer** oder **Dienst - Authentifizierung**. Anleitung und Weitere Informationen zur Authentifizierung finden Sie unter [Authentifizierung mit dem Datenspeicher Azure Active Directory verwenden](data-lake-store-authenticate-using-active-directory.md).

## <a name="create-an-azure-data-lake-store-account"></a>Erstellen Sie ein Konto Azure See Datenspeicher

1. Öffnen Sie auf dem Desktop ein neues Windows PowerShell und geben Sie den folgenden Codeausschnitt Azure-Konto anmelden, das Abonnement festgelegt und Datenspeicher See-Anbieter registrieren. Aufforderung Anmelden sicherstellen Sie, dass Sie als eines der Abonnementbesitzer/Administratoren anmelden:

        # Log in to your Azure account
        Login-AzureRmAccount

        # List all the subscriptions associated to your account
        Get-AzureRmSubscription

        # Select a subscription
        Set-AzureRmContext -SubscriptionId <subscription ID>

        # Register for Azure Data Lake Store
        Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"


2. Ein Azure-See Datenspeicher Konto ist Azure-Ressourcengruppe zugeordnet. Zunächst erstellen eine Ressourcengruppe Azure.

        $resourceGroupName = "<your new resource group name>"
        New-AzureRmResourceGroup -Name $resourceGroupName -Location "East US 2"

    ![Ein Azure-Ressourcengruppe erstellen] (./media/data-lake-store-get-started-powershell/ADL.PS.CreateResourceGroup.png "Ein Azure-Ressourcengruppe erstellen")

2. Erstellen eines Kontos Azure See Datenspeicher. Der angegebene Name darf nur Kleinbuchstaben und Zahlen enthalten.

        $dataLakeStoreName = "<your new Data Lake Store name>"
        New-AzureRmDataLakeStoreAccount -ResourceGroupName $resourceGroupName -Name $dataLakeStoreName -Location "East US 2"

    ![Azure See Datenspeicher Konto erstellen] (./media/data-lake-store-get-started-powershell/ADL.PS.CreateADLAcc.png "Azure See Datenspeicher Konto erstellen")

3. Stellen Sie sicher, dass das Konto erfolgreich erstellt wurde.

        Test-AzureRmDataLakeStoreAccount -Name $dataLakeStoreName

    Die Ausgabe für diesen sollte **True**sein.

## <a name="create-directory-structures-in-your-azure-data-lake-store"></a>Erstellen Sie Verzeichnisstrukturen im Datenspeicher See Azure

Erstellen Sie Verzeichnisse unter Ihrem Konto Azure See Datenspeicher verwalten und speichern.

1. Geben Sie ein Stammverzeichnis.

        $myrootdir = "/"

2. Erstellen Sie ein neues Verzeichnis namens **Mynewdirectory** unter dem angegebenen Stamm.

        New-AzureRmDataLakeStoreItem -Folder -AccountName $dataLakeStoreName -Path $myrootdir/mynewdirectory

3. Stellen Sie sicher, dass das neue Verzeichnis erstellt wird.

        Get-AzureRmDataLakeStoreChildItem -AccountName $dataLakeStoreName -Path $myrootdir

    Es sollte eine Ausgabe ähnlich der folgenden angezeigt:

    ![Verzeichnis überprüfen] (./media/data-lake-store-get-started-powershell/ADL.PS.Verify.Dir.Creation.png "Verzeichnis überprüfen")


## <a name="upload-data-to-your-azure-data-lake-store"></a>Daten an den Datenspeicher See Azure hochladen

Sie können Ihre Daten hochladen, See Datenspeicher direkt auf der Stammebene oder ein Verzeichnis innerhalb. Die folgenden Ausschnitte führen Beispieldaten im vorherigen Abschnitt erstellte Verzeichnis (**Mynewdirectory**) hinzufügen.

Wenn Sie Beispieldaten hochladen suchen, erhalten Sie von [Azure Data Lake Git Repository](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData) **Krankenwagen** Datenordner. Downloaden Sie die Datei und speichern Sie es in ein lokales Verzeichnis auf dem Computer wie C:\sampledata\.

    Import-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path "C:\sampledata\vehicle1_09142014.csv" -Destination $myrootdir\mynewdirectory\vehicle1_09142014.csv


## <a name="rename-download-and-delete-data-from-your-data-lake-store"></a>Umbenennen, herunterladen und Löschen von Daten aus dem Datenspeicher See

Verwenden Sie den folgenden Befehl, um eine Datei umzubenennen:

    Move-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path $myrootdir\mynewdirectory\vehicle1_09142014.csv -Destination $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv

Um eine Datei herunterzuladen, verwenden Sie den folgenden Befehl ein:

    Export-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv -Destination "C:\sampledata\vehicle1_09142014_Copy.csv"

Verwenden Sie zum Löschen einer Datei den folgenden Befehl ein:

    Remove-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Paths $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv

Geben Sie bei Aufforderung **Y** zu löschen. Wenn Sie mehr als eine Datei gelöscht haben, können Sie alle Pfade, die durch Komma getrennt bereitstellen.

    Remove-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Paths $myrootdir\mynewdirectory\vehicle1_09142014.csv, $myrootdir\mynewdirectoryvehicle1_09142014_Copy.csv

## <a name="delete-your-azure-data-lake-store-account"></a>Löschen Sie Ihr Konto Azure See Datenspeicher

Verwenden Sie den folgenden Befehl Löschen Ihres Kontos See Datenspeicher.

    Remove-AzureRmDataLakeStoreAccount -Name $dataLakeStoreName

Geben Sie bei Aufforderung **Y** ein, um das Konto zu löschen.


## <a name="next-steps"></a>Nächste Schritte

- [Sichere Daten im Datenspeicher See](data-lake-store-secure-data.md)
- [Verwenden Sie Azure Data Lake Analytics See Datenspeicher](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Verwenden von Azure HDInsight Lake Datenspeicher](data-lake-store-hdinsight-hadoop-use-portal.md)
