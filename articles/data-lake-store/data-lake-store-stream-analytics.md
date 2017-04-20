<properties
   pageTitle="Eingangsdaten aus Stream Analytics in Lake Datenspeicher | Azure"
   description="Verwenden von Azure Stream Analytics Daten in Azure See Datenspeicher"
   services="data-lake-store,stream-analytics" 
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
   ms.date="07/07/2016"
   ms.author="nitinme"/>

# <a name="stream-data-from-azure-storage-blob-into-data-lake-store-using-azure-stream-analytics"></a>Streaming von Daten von Azure Storage Blob in Lake Datenspeicher mithilfe von Azure Stream Analytics

In diesem Artikel erfahren Sie, wie Sie Azure See Datenspeicher als Ausgabe für eine Azure Stream Analytics-Auftrag. Dieser Artikel beschreibt ein einfaches Szenario, das Daten von einem Blob Azure Storage (Eingabe) liest und schreibt die Daten See Datenspeicher (Ausgabe).

>[AZURE.NOTE] Erstellung und Konfiguration der See Datenspeicher Ausgaben für Stream Analytics ist derzeit nur in [Azure-Verwaltungsportal](https://manage.windowsazure.com)unterstützt. Daher werden Teile des Lernprogramms Azure-Verwaltungsportal verwenden.

## <a name="prerequisites"></a>Erforderliche Komponenten

Bevor Sie dieses Lernprogramm beginnen, müssen Sie Folgendes:

- **Ein Azure-Abonnement**. Finden Sie [kostenlose Testversion von Azure zu erhalten](https://azure.microsoft.com/pricing/free-trial/).

- Für Daten See Store Public Preview **Azure Abonnement aktivieren** . [Siehe.](data-lake-store-get-started-portal.md#signup)

- **Azure Storage-Konto**. Einen BLOB-Container von diesem Konto verwendet Daten für einen Stream Analytics-Auftrag. Übernehmen Sie für dieses Lernprogramm erstellen ein Speicherkonto namens **Datalakestoreasa** und ein Container innerhalb **Datalakestoreasacontainer**aufgerufen. Nach dem Erstellen des Containers, Hochladen Sie eine Beispieldatendatei. Sie können eine Beispieldatendatei aus dem [Azure Data Lake Git Repository](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/Drivers.txt)abrufen. Verschiedene Kunden wie [Azure-Speicher-Explorer](http://storageexplorer.com/)können Sie einen BLOB-Container Datenupload.

    >[AZURE.NOTE] Wenn Sie das Konto aus dem Azure-Portal erstellen, stellen Sie sicher, erstellen mit dem **klassischen** Bereitstellungsmodell. Dadurch das Speicherkonto von Azure-Verwaltungsportal zugegriffen werden kann, da das, was wir einen Stream Analytics-Auftrag erstellen. Informationen zum Erstellen ein Speicherkonto der klassischen Bereitstellung Azure-Portal finden Sie unter [Azure Storage-Konto erstellen](../storage/storage-create-storage-account/#create-a-storage-account).
    >
    > Oder Sie erstellen ein Speicherkonto von Azure-Verwaltungsportal.

- **See Datenspeicher Azure-Konto**. Gehen Sie auf der [Einstieg in Azure See Datenspeicher Azure-Portal](data-lake-store-get-started-portal.md).  


## <a name="create-a-stream-analytics-job"></a>Erstellen Sie einen Stream Analytics-Auftrag

Erstellen zunächst Sie einen Stream Analytics-Auftrag, der eine Eingabequelle und Ausgabeziel enthält. Für dieses Lernprogramm die Quelle einer Azure BLOB-Container und Ziel See Datenspeicher.

1. Das [klassische Azure-Portal](https://manage.windowsazure.com)anmelden.

2. Klicken Sie unten links im Bildschirm **New** **Data Services** **Stream Analytics**, **Schnell erstellen**. Geben Sie die Werte wie folgt ein, und klicken Sie auf **Stream Analytics-Auftrag erstellen**.

    ![Erstellen Sie einen Stream Analytics-Auftrag] (./media/data-lake-store-stream-analytics/create.job.png "Erstellen eines Auftrags Stream Analytics")

## <a name="create-a-blob-input-for-the-job"></a>Ein Blob für den Auftrag erstellen

1. Öffnen Sie die Seite für den Stream Analytics-Auftrag, klicken Sie auf die Registerkarte **Eingaben** und klicken Sie auf **Add Eingabe** um einen Assistenten zu starten.

2. Wählen Sie auf der Seite **Hinzufügen Eingabe Ihrer Arbeit** **Datenstrom aus**und klicken Sie auf den Vorwärtspfeil.

    ![Eingabe in Ihrem Auftrag hinzufügen] (./media/data-lake-store-stream-analytics/create.input.1.png "Eingabe in Ihrem Auftrag hinzufügen")

3. Wählen Sie auf der Seite **hinzufügen einen Datenstrom für ein Projekt** **BLOB-Speicher**und klicken Sie auf den Vorwärtspfeil.

    ![Einen Datenstrom zum Auftrag hinzufügen] (./media/data-lake-store-stream-analytics/create.input.2.png "Einen Datenstrom zum Auftrag hinzufügen")

4. Angaben Sie auf der Seite **Einstellungen Blob** für den BLOB-Speicher, den Sie als Quelle der Daten verwenden.

    ![Das Blob Einstellungen bereitstellen] (./media/data-lake-store-stream-analytics/create.input.3.png "Das Blob Einstellungen bereitstellen")

    * **Geben Sie Alias input**. Dies ist ein eindeutiger Name für das Projekt eingeben bieten.
    * **Wählen Sie ein Speicherkonto**. Stellen Sie sicher das Speicherkonto ist im Bereich der Stream Analytics-Auftrag entstehen zusätzliche Kosten von Daten zwischen Regionen.
    * **Bereitstellen eines**. Sie können einen neuen Container erstellen, oder wählen einen vorhandenen Container.

    Klicken Sie auf den Vorwärtspfeil.

5. Festlegen Sie auf der Seite **Serialisierungseinstellungen** Serialisierungsformat als **CSV**Trennzeichen **Registerkarte**Codierung **UTF8**, und klicken Sie auf das Häkchen.

    ![Die Serialisierung Einstellungen] (./media/data-lake-store-stream-analytics/create.input.4.png "Die Serialisierung Einstellungen")

6. Nachdem Sie den Assistenten abgeschlossen haben, die BLOB-Eingabe Registerkarte **Eingaben** hinzugefügt und **Diagnose** Spalte **OK**anzeigen. Verbindung mit der Eingabe können Sie auch mithilfe der Schaltfläche **Verbindung testen** am unteren testen.

## <a name="create-a-data-lake-store-output-for-the-job"></a>Erstellen Sie eine Datenspeicher See Ausgabe für das Projekt

1. Öffnen Sie die Seite für den Stream Analytics-Auftrag, klicken Sie auf **Ausgaben** und dann auf **Hinzufügen eine Ausgabe** um einen Assistenten zu starten.

2. Wählen Sie auf der Seite **Hinzufügen eine Ausgabe Ihrer Arbeit** **See Datenspeicher aus**und klicken Sie auf den Vorwärtspfeil.

    ![Hinzufügen einer Ausgabe für Ihren Auftrag] (./media/data-lake-store-stream-analytics/create.output.1.png "Hinzufügen einer Ausgabe für Ihren Auftrag")

3. Auf der Seite **Verbindung autorisieren** See Datenspeicher Konto bereits erstellt haben, klicken Sie **Jetzt autorisieren**. Andernfalls klicken Sie auf **Anmelden** , um ein neues Konto erstellen. Angenommen Sie für dieses Lernprogramm, dass Sie bereits ein Konto See Datenspeicher (genannt Voraussetzung). Sie werden automatisch mit dem klassischen Azure-Portal angemeldet Anmeldeinformationen autorisiert werden.

    ![Autorisieren See Datenspeicher] (./media/data-lake-store-stream-analytics/create.output.2.png "Autorisieren See Datenspeicher")

4. Geben Sie auf der Seite **Daten See Speicher Settings** Informationen wie im folgenden Screenshot gezeigt.

    ![Geben Sie dem Datenspeicher settings] (./media/data-lake-store-stream-analytics/create.output.3.png "Geben Sie dem Datenspeicher settings")

    * **Geben Sie einen Ausgabealias**. Dies ist ein eindeutiger Name für die Auftragsausgabe bieten.
    * **Geben Sie ein Konto See Datenspeicher**. Sie sollten bereits, erstellt haben wie die Voraussetzung.
    * **Geben Sie einen Pfad Präfixmuster**. Dies ist erforderlich, die Ausgabedateien identifizieren, die vom Stream Analytics-Auftrags See Datenspeicher geschrieben werden. Da Ausgaben geschrieben durch den Auftrag im GUID-Format sind, einschließlich Präfix können die geschriebene Ausgabe identifiziert werden. Datum und Uhrzeit als Teil des Präfixes werden sollen sollten Sie sicher, dass Sie `{date}/{time}` im Präfixmuster. Wenn Sie dazu die Felder **Datum **und **Zeitformat** aktiviert sind, und wählen Sie das Format der Wahl.

    Klicken Sie auf den Vorwärtspfeil.

5. Festlegen Sie auf der Seite **Serialisierungseinstellungen** Serialisierungsformat als **CSV**Trennzeichen **Registerkarte**Codierung **UTF8**, und klicken Sie auf das Häkchen.

    ![Geben Sie das Ausgabeformat] (./media/data-lake-store-stream-analytics/create.output.4.png "Geben Sie das Ausgabeformat")

6. Nachdem Sie den Assistenten abgeschlossen haben, See Datenspeicher Ausgabe Registerkarte **Ausgaben** hinzugefügt und **Diagnose** Spalte anzeigen **OK**. Sie können die Verbindung mit der Ausgabe auch mithilfe der Schaltfläche **Verbindung testen** am unteren testen.

## <a name="run-the-stream-analytics-job"></a>Die Stapelverarbeitung Stream Analytics

Zum Ausführen eines Auftrags Stream Analytics führen Sie eine Abfrage in der Registerkarte Abfrage. Für dieses Lernprogramm können Sie die Abfrage ausführen, indem mit den Auftrag eingeben und Aliase, wie im folgenden Screenshot gezeigt.

![Abfrage ausführen] (./media/data-lake-store-stream-analytics/run.query.png "Abfrage ausführen")

Klicken Sie auf den unteren Bildschirmrand **Speichern** und dann auf **Starten**. Wählen Sie im Dialogfeld **Benutzerdefiniert**, und wählen Sie ein Datum wie **1/1/2016**. Klicken Sie auf das Häkchen, um den Auftrag zu starten. Es kann ein paar Minuten dauern, den Auftrag zu starten.

![Projekt einstellen] (./media/data-lake-store-stream-analytics/run.query.2.png "Projekt einstellen")

Nachdem der Auftrag gestartet wurde, klicken Sie auf die Registerkarte **Monitor** , um zu sehen, wie die Daten.

![Monitor-Auftrag] (./media/data-lake-store-stream-analytics/run.query.3.png "Monitor-Auftrag")

Schließlich können Sie [Azure-Portal](https://portal.azure.com) Kontos See Datenspeicher öffnen und überprüfen, ob die Daten erfolgreich auf das geschrieben wurde.

![Ausgabe überprüfen] (./media/data-lake-store-stream-analytics/run.query.4.png "Ausgabe überprüfen")

Im Daten-Explorer beachten die Ausgabe in einen Ordner wie angegeben im See Datenspeicher geschrieben output Settings (`streamanalytics/job/output/{date}/{time}`).  

## <a name="see-also"></a>Siehe auch

* [Erstellen eines HDInsight Clusters See Datenspeicher](data-lake-store-hdinsight-hadoop-use-portal.md)
