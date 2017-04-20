<properties
   pageTitle="Analysieren von Daten im Datenspeicher See mit Power BI | Microsoft Azure"
   description="Verwenden von Power BI in Azure See Datenspeicher gespeicherte Daten analysieren"
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
   ms.date="10/05/2016"
   ms.author="nitinme"/>

# <a name="analyze-data-in-data-lake-store-by-using-power-bi"></a>Analysieren von Daten im Datenspeicher See mit Power BI

In diesem Artikel erfahren Sie, wie Sie Power BI Desktop analysieren und Visualisieren von Daten in Azure See Datenspeicher.

## <a name="prerequisites"></a>Erforderliche Komponenten

Bevor Sie dieses Lernprogramm beginnen, müssen Sie Folgendes:

- **Ein Azure-Abonnement**. Finden Sie [kostenlose Testversion von Azure zu erhalten](https://azure.microsoft.com/pricing/free-trial/).

- **See Datenspeicher Azure-Konto**. Gehen Sie auf der [Einstieg in Azure See Datenspeicher Azure-Portal](data-lake-store-get-started-portal.md). Es wird vorausgesetzt, dass bereits ein Datenspeicher See Konto namens **Mybidatalakestore**erstellt und eine Beispieldatendatei (**Drivers.txt**) hochgeladen. Diese Beispieldatei wird von [Azure Data Lake Git Repository](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/Drivers.txt)heruntergeladen.

- **Power BI Desktop**. Sie können das [Microsoft Download](https://www.microsoft.com/en-us/download/details.aspx?id=45331)Center herunterladen. 


## <a name="create-a-report-in-power-bi-desktop"></a>Erstellen eines Berichts in Power BI Desktop

1. Starten Sie Power BI Desktop auf dem Computer.

2. **Klicken Sie auf der Multifunktionsleiste **Home** **und klicken Sie auf Weitere. Klicken Sie im Dialogfeld **Daten importieren** auf **Azure**klicken Sie **Azure See Datenspeicher**, und klicken Sie auf **Verbinden**.

    ![Verbindung mit dem Datenspeicher] (./media/data-lake-store-power-bi/get-data-lake-store-account.png "Verbindung mit dem Datenspeicher")

3. Erscheint ein Dialogfeld über den Connector in eine Entwicklungsphase wählen Sie um fortzufahren.

4. Klicken Sie im Dialogfeld **Microsoft Azure See Datenspeicher** die URL zu Ihrem Konto See Datenspeicher und klicken Sie auf **OK**.

    ![URL für Datenspeicher See] (./media/data-lake-store-power-bi/get-data-lake-store-account-url.png "URL für Datenspeicher See")

5. Klicken Sie im nächsten Dialogfeld auf See Datenspeicher Konto anmelden **Anmelden** . Sie werden zur Anmeldeseite Ihrer Organisation weitergeleitet. Folgen Sie dem Konto anmelden.

    ![Melden Sie sich bei dem Datenspeicher] (./media/data-lake-store-power-bi/get-data-lake-store-account-signin.png "Melden Sie sich bei dem Datenspeicher")

6. Nachdem Sie sich erfolgreich angemeldet haben, klicken Sie auf **Verbinden**.

    ![Verbindung mit dem Datenspeicher] (./media/data-lake-store-power-bi/get-data-lake-store-account-connect.png "Verbindung mit dem Datenspeicher")

7. Im nächste Dialogfeld zeigt die Datei, die Sie Ihrem Konto See Datenspeicher hochgeladen. Überprüfen Sie die Informationen und klicken Sie auf **Laden**.

    ![Daten aus dem Datenspeicher laden] (./media/data-lake-store-power-bi/get-data-lake-store-account-load.png "Daten aus dem Datenspeicher laden")

8. Nachdem die Daten in Power BI erfolgreich geladen wurde, sehen Sie die folgenden Felder in die Registerkarte **Felder** .

    ![Importierte Felder] (./media/data-lake-store-power-bi/imported-fields.png "Importierte Felder")

    Dagegen visualisieren und analysieren wir die Daten der folgenden Felder

    ![Gewünschte Felder] (./media/data-lake-store-power-bi/desired-fields.png "Gewünschte Felder")

    In den nächsten Schritten aktualisieren wir die Abfrage, um die importierten Daten im gewünschten Format zu konvertieren.

9. Klicken Sie auf der Multifunktionsleiste **Home** **Abfragen bearbeiten**.

    ![Bearbeiten von Abfragen] (./media/data-lake-store-power-bi/edit-queries.png "Bearbeiten von Abfragen")

10. Im Abfrage-Editor **Inhalt** Spalte klicken Sie auf **Binär**.

    ![Bearbeiten von Abfragen] (./media/data-lake-store-power-bi/convert-query1.png "Bearbeiten von Abfragen")

11. Ein Dateisymbol wird angezeigt, die die **Drivers.txt** -Datei darstellt, die Sie hochgeladen. Maustaste auf die Datei und auf **CSV**.  

    ![Bearbeiten von Abfragen] (./media/data-lake-store-power-bi/convert-query2.png "Bearbeiten von Abfragen")

12. Ausgabe sollte angezeigt werden, wie unten dargestellt. Ihre Daten sind jetzt verfügbar in ein Format Visualisierung erstellen.

    ![Bearbeiten von Abfragen] (./media/data-lake-store-power-bi/convert-query3.png "Bearbeiten von Abfragen")

13. Die Multifunktionsleiste **Home** auf **Schließen und übernehmen**und klicken Sie auf **Schließen und zu übernehmen**.

    ![Bearbeiten von Abfragen] (./media/data-lake-store-power-bi/load-edited-query.png "Bearbeiten von Abfragen")

14. Nach der Aktualisierung der Abfrage zeigt die Registerkarte **Felder** neue Felder zur Visualisierung.

    ![Aktualisiert Felder] (./media/data-lake-store-power-bi/updated-query-fields.png "Aktualisiert Felder")

15. Wir erstellen Sie ein Kreisdiagramm Treiber in jeder Stadt eines bestimmten Landes darstellt. Zu diesem Zweck treffen.

    1. Klicken Sie auf der Registerkarte Visualisierung auf das Symbol für ein Kreisdiagramm.

        ![Diagramm erstellen] (./media/data-lake-store-power-bi/create-pie-chart.png "Diagramm erstellen")

    2. Die Spalten, die wir verwenden werden **Spalte 4** (Name der Stadt) und **Spalte 7** (Name des Landes). Ziehen Sie diese Spalten aus Registerkarte **Felder** zur **Visualisierung** Registerkarte wie unten dargestellt.

        ![Visualisierung erstellen] (./media/data-lake-store-power-bi/create-visualizations.png "Visualisierung erstellen")

    3. Kreisdiagramm sollte jetzt wie unten gezeigt aussehen.

        ![Kreisdiagramm] (./media/data-lake-store-power-bi/pie-chart.png "Visualisierung erstellen")

16. Die Seitenfilter auf ein bestimmtes Land auswählen, sehen Sie jetzt die Anzahl der in jeder Stadt des gewählten Landes. Unter der Registerkarte **Visualisierung** unter **Seite filtert**, Beispiel **Brasilien**.

    ![Wählen Sie ein Land] (./media/data-lake-store-power-bi/select-country.png "Wählen Sie ein Land")

17. Im Diagramm wird automatisch aktualisiert, um Treiber in den Städten Brasiliens anzuzeigen.

    ![Treiber in einem Land] (./media/data-lake-store-power-bi/driver-per-country.png "Treiber pro Land")

18. Klicken Sie im Menü **Datei** auf **Speichern** , um die Visualisierung als Power BI Desktop-Datei speichern.

## <a name="publish-report-to-power-bi-service"></a>Bericht auf Power BI-Dienst veröffentlichen

Nach dem Erstellen der Visualisierung in Power BI Desktop können Sie es Power BI-Dienst veröffentlichen für andere freigeben. Informationen dazu finden Sie unter [Veröffentlichen von Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-upload-desktop-files/).

## <a name="see-also"></a>Siehe auch

* [Analysieren von Daten in Lake Datenspeicher See Datenanalyse](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
