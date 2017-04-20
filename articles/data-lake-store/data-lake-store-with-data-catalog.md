<properties
   pageTitle="Daten aus See Datenspeicher in Azure Data Catalog registrieren | Microsoft Azure"
   description="Daten vom Datenspeicher in Azure Data Catalog See"
   services="data-lake-store,data-catalog" 
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
   ms.date="10/28/2016"
   ms.author="nitinme"/>

# <a name="register-data-from-data-lake-store-in-azure-data-catalog"></a>Daten vom Datenspeicher in Azure Data Catalog See

In diesem Artikel erfahren Sie, wie Azure See Datenspeicher in Azure Data Catalog Daten innerhalb einer Organisation auffindbar Datenkatalog integrieren integriert. Weitere Informationen zum katalogisieren Daten finden Sie unter [Azure Data Catalog](../data-catalog/data-catalog-what-is-data-catalog.md). Um Szenarien zu verstehen, in denen Datenkatalog verwenden können, finden Sie unter [Azure Data Catalog-Szenarien](../data-catalog/data-catalog-common-scenarios.md).

## <a name="prerequisites"></a>Erforderliche Komponenten

Bevor Sie dieses Lernprogramm beginnen, müssen Sie Folgendes:

- **Ein Azure-Abonnement**. Finden Sie [kostenlose Testversion von Azure zu erhalten](https://azure.microsoft.com/pricing/free-trial/).

- Für Daten See Store Public Preview **Azure Abonnement aktivieren** . [Siehe.](data-lake-store-get-started-portal.md#signup)

- **See Datenspeicher Azure-Konto**. Gehen Sie auf der [Einstieg in Azure See Datenspeicher Azure-Portal](data-lake-store-get-started-portal.md). In diesem Lernprogramm erstellen Sie ein Datenspeicher See-Konto **Datacatalogstore**. 

    Nachdem Sie das Konto erstellt haben, ein Beispieldataset, upload. Laden Sie für dieses Lernprogramm wir alle CSV-Dateien im Ordner **AmbulanceData** in [Azure Data Lake Git Repository](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/). Verschiedene Kunden wie [Azure-Speicher-Explorer](http://storageexplorer.com/)können Sie einen BLOB-Container Datenupload.

- **Azure Data Catalog**. Ihre Organisation muss bereits ein Azure Data Catalog für Ihre Organisation erstellt. Jede Organisation darf jeweils nur ein Katalog.

## <a name="register-data-lake-store-as-a-source-for-data-catalog"></a>Register See Datenspeicher als Quelle für Daten-Katalog

>[AZURE.VIDEO adcwithadl] 

1. Klicken Sie auf `https://azure.microsoft.com/services/data-catalog`, und klicken Sie auf **Erste Schritte**.

2. Azure Data Catalog-Portal melden Sie an und auf **Daten veröffentlichen**.

    ![Registrieren eine Datenquelle] (./media/data-lake-store-with-data-catalog/register-data-source.png "Registrieren eine Datenquelle")

3. Klicken Sie auf der nächsten Seite auf **Anwendung starten**. Dadurch wird die Anwendungsmanifestdatei auf Ihrem Computer downloaden. Doppelklicken Sie auf die manifest-Datei zum Starten der Anwendung.

4. Auf der Seite Willkommen auf **Anmelden**und geben Sie Ihre Anmeldeinformationen.

    ![Willkommenseite] (./media/data-lake-store-with-data-catalog/welcome.screen.png "Willkommenseite")

5. Wählen Sie auf der Seite Datenquelle auswählen **Azure Data Lake aus**und dann auf **Weiter**.

    ![Datenquelle auswählen] (./media/data-lake-store-with-data-catalog/select-source.png "Datenquelle auswählen")

6. Geben Sie auf der nächsten Seite See Datenspeicher Kontonamen, den Datenkatalog registrieren möchten. Lassen Sie die anderen Optionen als Standard, und klicken Sie auf **Verbinden**.

    ![Mit Datenquelle verbinden] (./media/data-lake-store-with-data-catalog/connect-to-source.png "Mit Datenquelle verbinden")

7. Die Seite kann in die folgenden Segmente unterteilt.

    ein. Feld **Hierarchie** stellt die Kontostruktur Ordner See Datenspeicher. **$Root** stellt See Datenspeicher Konto Root und **AmbulanceData** steht für den Ordner im Stamm des Kontos See Datenspeicher erstellt.

    b. Im **verfügbaren Objekte** werden Dateien und Ordner unter dem Ordner **AmbulanceData** .

    c. **Objekte werden registrierte** Listet die Dateien und Ordner, die Sie in Azure Data Catalog registrieren möchten.

    ![Struktur anzeigen] (./media/data-lake-store-with-data-catalog/view-data-structure.png "Struktur anzeigen")

8. Für dieses Lernprogramm sollten Sie alle Dateien im Verzeichnis registrieren. Dafür klicken Sie (![Objekte](./media/data-lake-store-with-data-catalog/move-objects.png "Verschieben von Objekten")) um **Objekte registriert werden** alle Dateien verschieben. 

    Da die Daten in einem organisationsweiten Datenkatalog registriert werden, ist einen Ansatz empfohlen Metadaten hinzufügen, die Sie später verwenden können, um Daten schnell zu finden. Sie können z. B. eine e-Mail-Adresse für Datenbesitzer (z. B. wer die Daten hochladen) hinzufügen oder einen Tag, um Daten hinzuzufügen. Die Bildschirmaufnahme unten zeigt einem Tag, der Daten hinzufügen.

    ![Struktur anzeigen] (./media/data-lake-store-with-data-catalog/view-selected-data-structure.png "Struktur anzeigen")

    Klicken Sie auf **Registrieren**.

8. Die folgenden Bildschirmaufnahme gibt an, dass die Daten im Katalog Daten registriert ist.

    ![Die Registrierung ist abgeschlossen] (./media/data-lake-store-with-data-catalog/registration-complete.png "Struktur anzeigen")

9. Klicken Sie auf **Ansicht Portal** Data Catalog-Portal zurück und überprüfen Sie die Daten jetzt aus dem Portal zugreifen können. Um die Daten zu suchen, können Sie das Tag beim Erfassen der Daten verwendet.

    ![Daten im Katalog] (./media/data-lake-store-with-data-catalog/search-data-in-catalog.png "Daten im Katalog")

10. Sie können nun Vorgänge wie Kommentare und Dokumentation der Daten ausführen. Weitere Informationen finden Sie unter den folgenden Links.
    * [Kommentieren Sie Datenquellen in Data Catalog](../data-catalog/data-catalog-how-to-annotate.md)
    * [Dokument-Datenquellen in Data Catalog](../data-catalog/data-catalog-how-to-documentation.md)

## <a name="see-also"></a>Siehe auch

* [Kommentieren Sie Datenquellen in Data Catalog](../data-catalog/data-catalog-how-to-annotate.md)
* [Dokument-Datenquellen in Data Catalog](../data-catalog/data-catalog-how-to-documentation.md)
* [Andere Azure Services See Datenspeicher integriert](data-lake-store-integrate-with-other-services.md)
