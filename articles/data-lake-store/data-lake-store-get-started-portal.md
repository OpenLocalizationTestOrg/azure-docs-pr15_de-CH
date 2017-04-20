<properties 
   pageTitle="Erste Schritte mit See Datenspeicher | Azure" 
   description="Das Portal mit der See Datenspeicher registrieren und grundlegende Operationen im Datenspeicher See" 
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
   ms.date="09/13/2016"
   ms.author="nitinme"/>

# <a name="get-started-with-azure-data-lake-store-using-the-azure-portal"></a>Erste Schritte mit Azure See Datenspeicher Azure-Portal

> [AZURE.SELECTOR]
- [Portal](data-lake-store-get-started-portal.md)
- [PowerShell](data-lake-store-get-started-powershell.md)
- [.NET SDK](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [REST-API](data-lake-store-get-started-rest-api.md)
- [Azure CLI](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)

Informationen Sie zum Azure-Portal Azure See Datenspeicher registrieren und grundlegende Vorgänge wie Ordner erstellen hoch- und Herunterladen von Dateien, löschen Sie Ihr Konto usw. Weitere Informationen über See Datenspeicher finden Sie unter [Übersicht von Azure See Datenspeicher](data-lake-store-overview.md).

## <a name="prerequisites"></a>Erforderliche Komponenten

Bevor Sie dieses Lernprogramm beginnen, müssen Sie Folgendes:

- **Ein Azure-Abonnement**. Finden Sie [kostenlose Testversion von Azure zu erhalten](https://azure.microsoft.com/pricing/free-trial/).

## <a name="do-you-learn-fast-with-videos"></a>Lernen Sie mit Videos?

Die folgenden Videos Einstieg in Lake Datenspeicher.

* [Erstellen Sie ein Konto See Datenspeicher](https://mix.office.com/watch/1k1cycy4l4gen)
* [Verwalten Sie Daten im Datenspeicher See im Daten-Explorer](https://mix.office.com/watch/icletrxrh6pc)

## <a name="create-an-azure-data-lake-store-account"></a>Erstellen Sie ein Konto Azure See Datenspeicher

1. Melden Sie sich auf das neue [Azure-Portal](https://portal.azure.com).

2. Klicken Sie auf **neu**und klicken Sie dann auf **Azure See Datenspeicher**auf **Daten + Speicher**. Lesen Sie die Informationen in **Azure See Datenspeicher** Blade und dann auf **Erstellen** links unten das Blade.

3. Blatt **Neu See Datenspeicher** enthalten Sie Werte wie im folgenden Screenshot gezeigt:

    ![Neues See Datenspeicher Azure-Konto erstellen] (./media/data-lake-store-get-started-portal/ADL.Create.New.Account.png "Erstellen eines neuen Kontos Azure Data Lake")

    - **Abonnement**. Wählen Sie das Abonnement, unter dem Sie ein neues Konto See Datenspeicher erstellen möchten.
    - **Ressourcengruppe**. Wählen Sie eine vorhandene Ressourcengruppe oder klicken Sie auf **Erstellen einer Ressourcengruppe** zu erstellen. Eine Ressourcengruppe ist ein Container, der zugehörige Ressourcen für eine Anwendung enthält. Weitere Informationen finden Sie unter [Ressourcengruppen in Azure](azure-resource-manager/resource-group-overview.md#resource-groups).
    - **Ort**: Wählen Sie einen Speicherort See Datenspeicher-Konto erstellt werden soll.

4. Wählen Sie **an Startmenü anheften** , wenn das Datenspeicher See Konto über das Startmenü.

5. Klicken Sie auf **Erstellen**. Wenn Sie das Konto an das Startmenü anheften möchten, Sie gelangen wieder in das Startmenü, und sehen Sie den Fortschritt der Bereitstellung der See Datenspeicher. Nach Konto See Datenspeicher bereitgestellt wird, wird das Konto Blade.

6. Die **Essentials** Dropdownliste, ob Informationen zu Ihrem Konto See Datenspeicher wie die Ressourcengruppe ist Teil der Position usw. zu erweitern. Klicken Sie auf **Schnellstart** -Symbol, um Links zu anderen Ressourcen See Datenspeicher.

    ![Der Azure-See Datenspeicher Konto] (./media/data-lake-store-get-started-portal/ADL.Account.QuickStart.png "Ihre Azure Data Lake Konto")

## <a name="createfolder"></a>Ordner in Azure See Datenspeicher-Konto erstellen

Sie können Ordner erstellen, unter Ihrem Konto See Datenspeicher verwalten und speichern.

1. Öffnen Sie die See Datenspeicher Firma, die Sie gerade erstellt haben. Im linken Bereich klicken Sie auf **Durchsuchen**, klicken Sie auf **See Datenspeicher**und klicken Sie den Kontonamen, unter dem Ordner soll, Blatt See Datenspeicher. Konto an das Startmenü angeheftet, klicken Sie auf die Kachel Konto.

2. Klicken Sie in Ihrem Konto Blade See Datenspeicher auf **Daten-Explorer**.

    ![Erstellen von Ordnern in dem Datenspeicher Konto] (./media/data-lake-store-get-started-portal/ADL.Create.Folder.png "Erstellen von Ordnern in dem Datenspeicher Konto")

3. Klicken Sie in Ihrem Konto Blade See Datenspeicher auf **Neuer Ordner**, geben Sie einen Namen für den neuen Ordner und klicken Sie dann auf **OK**.
    
    ![Erstellen von Ordnern in dem Datenspeicher Konto] (./media/data-lake-store-get-started-portal/ADL.Folder.Name.png "Erstellen von Ordnern in dem Datenspeicher Konto")
    
    Der neu erstellte Ordner in das Blade **Daten-Explorer** aufgeführt. Sie können verschachtelte Ordner bis zu jeder Ebene erstellen.

    ![Erstellen von Ordnern in Daten dem Konto] (./media/data-lake-store-get-started-portal/ADL.New.Directory.png "Erstellen von Ordnern in Daten dem Konto")


## <a name="uploaddata"></a>Upload von Daten auf See Datenspeicher Azure-Konto

Ein Konto Azure See Datenspeicher direkt auf der Stammebene oder einen Ordner, den das Konto erstellt, können Sie die Daten hochladen. Im Screenshot unten die Schritte zum Hochladen einer Datei in einem Unterordner aus dem **Daten-Explorer** -Blade. In diesem Screenshot wird die Datei in einen Unterordner im Breadcrumbs (rot markiert) angezeigt hochgeladen.

Wenn Sie Beispieldaten hochladen suchen, erhalten Sie von [Azure Data Lake Git Repository](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData) **Krankenwagen** Datenordner.

![Upload von Daten] (./media/data-lake-store-get-started-portal/ADL.New.Upload.File.png "Upload von Daten")


## <a name="properties"></a>Eigenschaften und Aktionen auf die gespeicherten Daten

Klicken Sie auf die neu hinzugefügte Datei Blade **Eigenschaften** öffnen. Auf diesem Blatt stehen die Eigenschaften und Aktionen auf der Datei ausführen. Sie können auch den vollständigen Pfad zur Datei in Ihrem Konto Azure See Datenspeicher im roten Feld im Screenshot unten hervorgehoben kopieren.

![Eigenschaften der Daten] (./media/data-lake-store-get-started-portal/ADL.File.Properties.png "Eigenschaften der Daten")

* Klicken Sie auf **Vorschau** , um eine Vorschau der Datei direkt in den Browser. Sie können das Format der Vorschau sowie angeben. Klicken Sie auf **Vorschau**, klicken Sie in der **Vorschau der** Blade- **Format** , und geben Sie in der **Vorschau-Dateiformat** Blade-Optionen wie Anzahl Zeilen anzuzeigen, Codierung mit Trennzeichen usw. verwenden.

  ![Vorschau-Dateiformat] (./media/data-lake-store-get-started-portal/ADL.File.Preview.png "Vorschau-Dateiformat")

* Klicken Sie zum Herunterladen der Datei auf Ihren Computer **herunterladen** .

* Klicken Sie auf **Datei** , um die Datei umzubenennen.

* Klicken Sie auf **Löschen** , um die Datei zu löschen.


## <a name="secure-your-data"></a>Sichern Sie Ihre Daten

Sie können die Daten in Ihrem Azure See Datenspeicher-Konto mithilfe von Azure Active Directory und Zugriffskontrolle (ACLs) sichern. Informationen dazu finden Sie unter [Sichern von Daten in Azure See Datenspeicher](data-lake-store-secure-data.md).


## <a name="delete-azure-data-lake-store-account"></a>Konto löschen Azure See Datenspeicher

Klicken Sie ein Konto Azure See Datenspeicher aus Ihrem Datenspeicher See Blade **Löschen**. Um die Aktion zu bestätigen, werden Sie aufgefordert, das Konto eingeben, die Sie löschen möchten. Geben Sie den Namen des Kontos und dann auf **Löschen**.

![Konto löschen Daten See] (./media/data-lake-store-get-started-portal/ADL.Delete.Account.png "Konto löschen Daten See")


## <a name="next-steps"></a>Nächste Schritte

- [Sichere Daten im Datenspeicher See](data-lake-store-secure-data.md)
- [Verwenden Sie Azure Data Lake Analytics See Datenspeicher](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Verwenden von Azure HDInsight Lake Datenspeicher](data-lake-store-hdinsight-hadoop-use-portal.md)
- [Access Diagnoseprotokolle für Datenspeicher See](data-lake-store-diagnostic-logs.md)
