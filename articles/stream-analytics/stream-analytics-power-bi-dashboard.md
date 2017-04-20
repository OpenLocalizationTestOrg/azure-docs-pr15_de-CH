<properties
    pageTitle="Power BI-Dashboard auf Stream Analytics | Microsoft Azure"
    description="Verwenden Sie Business Intelligence und Analysieren von Daten aus einem Stream Analytics-Auftrag umfangreicher Echtzeit streaming Power BI-Dashboard."
    keywords="Analytics Dashboard Echtzeit-dashboard"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>

#  <a name="stream-analytics--power-bi-a-real-time-analytics-dashboard-for-streaming-data"></a>Stream Analytics & Power BI: ein Dashboard Echtzeitanalysen für streaming-Daten

Azure Stream Analytics können Sie eines der führenden Business Intelligence-Tools Microsoft Power BI nutzen. Erfahren Sie mit Azure Stream Analytics umfangreicher streaming und Daten erhalten Einblick in Echtzeit Power BI Analytics-Dashboard analysieren.

Verwenden Sie [Microsoft Power BI](https://powerbi.com/) live Dashboard erstellen. [In diesem Video der Praxis](https://www.youtube.com/watch?v=SGUpT-a99MA).

In diesem Artikel erfahren Sie, wie mit Power BI als Ausgabe für Ihre Azure Stream Analytics-Aufträge Erstellen eigener benutzerdefinierter Business Intelligence-Tools und Echtzeit-Dashboard nutzen.

## <a name="prerequisites"></a>Erforderliche Komponenten

* Microsoft Azure-Konto
* Eingabe für den Auftrag Stream Analytics Streaming-Daten aus. Stream Analytics akzeptiert Eingaben von Azure Ereignis Hubs oder Azure BLOB-Speicher.  
* Arbeits- oder Schulcomputer Konto für Power BI

## <a name="create-azure-stream-analytics-job"></a>Azure Stream Analytics-Auftrag erstellen

[Azure-Verwaltungsportal](https://manage.windowsazure.com)klicken Sie auf **New Data Services Stream Analytics, schnell erstellen**.

Geben Sie einen der folgenden Werte, klicken Sie auf **Auftrag Stream Analytics erstellen**:

* **Auftrag** - Auftrag geben. Beispielsweise **DeviceTemperatures**.
* **Bereich** – wählen Sie den Bereich den Auftrag gespeichert werden soll. Sollten Sie den Auftrag und dem Ereignis Gebiet zu gewährleisten optimale Performance Data Transfer Kosten zwischen Regionen.
* **Konto** - wählen Sie das Speicherkonto Überwachungsdaten für alle Stream Analytics innerhalb dieses Bereichs speichern möchten. Sie können ein vorhandenes Speicherkonto oder einen neuen erstellen.

Klicken Sie im linken Bereich der Stream Analytics-Aufträge auflisten **Stream Analytics** auf.

![graphic1][graphic1]

> [AZURE.TIP] Der neue Auftrag wird mit dem Status **Nicht gestartet**aufgeführt. Beachten Sie, dass am Ende der Seite auf die Schaltfläche **Start** deaktiviert ist. Dies entspricht erwartet konfigurierenden Auftrag Eingabe, Ausgabe und Abfrage usw. vor den Auftrag.

## <a name="specify-job-input"></a>Job-Eingabe angeben

In diesem Lernprogramm wird davon ausgegangen, dass Event Hub als Eingabe mit JSON-Serialisierung UTF-8-Kodierung verwenden.

* Klicken Sie auf den Auftragsnamen.
* **Eingaben** vom oberen Rand der Seite auf, und klicken Sie dann auf **Hinzufügen**. Das Dialogfeld führt Sie durch mehrere Schritte, um Ihre Eingabe einzurichten.
*   Wählen Sie **Datenstrom aus**, und klicken Sie mit der rechte Maustaste.
*   Wählen Sie **Ereignis-Hub**aus, und klicken Sie mit der rechte Maustaste.
*   Eingeben oder auswählen auf der dritten Seite die folgenden Werte:
  * **Input Alias** - Geben Sie einen Anzeigenamen für diesen Auftrag eingeben. Beachten Sie, dass Sie diesen Namen in der Abfrage später verwenden werden.
  * **Event Hub** - Event Hub erstellt wird das Abonnement wählen Stream Analytics-Auftrag Ereignis im ist namespace
*   Ist Haupt-Ereignis in einem anderen Abonnement **Mit Event Hub ein anderes Abonnement** wählen und Informationen für **Service Bus Namespace**, **Hub Ereignisnamen** **Ereignis Hub Richtlinienname**, **Ereignis Hub Richtlinienschlüssel**und **Ereignisanzahl Hub Partition**manuell eingeben.

> [AZURE.NOTE]  Dieses Beispiel verwendet die Standardanzahl von Partitionen ist 16.

* **Ereignisname Hub** - wählen Sie Azure Event Hub.
* **Ereignis Hub Richtlinienname** - Select Event Hub-Richtlinie für Event Hub verwenden werden. Sicherstellen Sie, dass diese Richtlinie hat Berechtigungen verwalten.
*   **Hub Consumer Gruppe** leer lassen oder geben ein Consumer auf Ihrem Haupt-Ereignis. Beachten Sie, dass jede Verbrauchergruppe Event Hubs jeweils nur 5 Leser haben. So entscheiden Sie entsprechend rechts Verbrauchergruppe für Ihren Auftrag. Wenn Sie das Feld leer lassen, wird die Consumer Standardgruppe verwendet.

*   Klicken Sie auf die rechte Maustaste.
*   Geben Sie die folgenden Werte:
  * **Ereignis Serialisierungsprogramm Format** - JSON
  * **Encoding** - UTF8
*   Klicken Sie Kontrollkästchen dieser Quelle hinzufügen und überprüfen, ob der Stream Analytics Ereignis-Hub herstellen können.

## <a name="add-power-bi-output"></a>Power BI-Ausgabe hinzufügen

1.  **Ausgabe** vom oberen Rand der Seite auf und klicken Sie dann auf **Ausgabe hinzufügen**. Sie sehen, dass Power BI optional Ausgabe angezeigt.

    ![graphic2][graphic2]  

2.  Wählen Sie **Power BI** , und klicken Sie mit der rechte Maustaste.
3.  Einen Bildschirm ähnlich dem folgenden wird angezeigt:

    ![graphic3][graphic3]  

4.  Geben Sie in diesem Schritt Arbeits- oder Schulcomputer Konto für die Auftragsausgabe Stream Analytics. Wenn bereits Power BI-Konto verfügen, wählen Sie **Jetzt autorisieren**. Falls nicht, **Registrieren Sie sich jetzt**. [Hier ist ein gutes Blog durch Details Power BI anmelden](http://blogs.technet.com/b/powerbisupport/archive/2015/02/06/power-bi-sign-up-walkthrough.aspx).

    ![graphic11][graphic11]  

5.  Anschließend wird einen Bildschirm ähnlich dem folgenden angezeigt:

    ![graphic4][graphic4]  

Geben Sie die Werte wie folgt:

* **Ausgabealias** – alle Ausgabealias Sie auf Einfügen. Diese Ausgabealias ist besonders hilfreich, möchten Sie mehrere Ausgaben für Ihren Auftrag. In diesem Fall müssen Sie in der Abfrage zu verweisen. Beispielsweise nehmen den Ausgabewert Alias = "OutPbi".
* **Dataset-Name** - einen Datasetnamen der Power BI Ausgabe zu bieten. Beispielsweise verwenden wir "Pbidemo".
*   **Tabellenname** - bieten einen Tabellennamen unter Dataset Power BI-Ausgabe. Angenommen, wir rufen "Pbidemo". Derzeit haben Power BI-Ausgabe von Stream Analytics stellen nur eine Tabelle in einem Dataset.
*   **Arbeitsbereich** – wählen Sie einen Arbeitsbereich in Ihrem Power BI Mandanten unter dem Dataset erstellt wird.

>   [AZURE.NOTE] Sie sollten keine explizit diesem Dataset und Tabelle in Ihrem Power BI-Konto erstellen. Sie werden automatisch erstellt, wenn Sie Ihre Arbeit Stream Analytics und der Einzelvorgang Pumpen Ausgabe in Power BI. Wenn die auftragsabfrage Ergebnisse zurückgibt, werden das Dataset und die Tabelle nicht erstellt werden.

*   Klicken Sie auf **OK**, **Verbindung testen** und die Konfiguration ist abgeschlossen, nachdem Sie ausgeben.

>   [AZURE.WARNING] Beachten Sie auch, wenn Power BI bereits ein Dataset und mit demselben Namen wie in diesem Stream Analytics-Auftrag bereitgestellt, die vorhandenen Daten überschrieben.


## <a name="write-query"></a>Abfrage

Wechseln Sie zur Registerkarte **Abfrage** des Auftrags. Schreiben Sie die Abfrage, die Ausgabe in Power BI. Beispielsweise wäre es etwas wie die folgende SQL-Abfrage:

    SELECT
        MAX(hmdt) AS hmdt,
        MAX(temp) AS temp,
        System.TimeStamp AS time,
        dspl
    INTO
        OutPBI
    FROM
        Input TIMESTAMP BY time
    GROUP BY
        TUMBLINGWINDOW(ss,1),
        dspl



Ihr Auftrag wird gestartet. Haupt-Ereignis empfängt Ereignisse und die Abfrage die erwarteten Ergebnisse zu überprüfen. Wenn Ihre Abfrage 0 Zeilen gibt, werden Power BI-Dataset und Tabellen nicht automatisch erstellt.

## <a name="create-the-dashboard-in-power-bi"></a>Erstellen Sie das Dashboard in Power BI

Gehen Sie zu [Powerbi.com](https://powerbi.com) und melden Sie sich mit der Arbeit oder Schule. Stream Analytics auftragsabfrage Ergebnisse ausgegeben, sehen Sie, dass das Dataset bereits erstellt:

![graphic5][graphic5]

Erstellen des Dashboards Dashboards Option gehen und ein neues Dashboard erstellen.

![graphic6][graphic6]

In diesem Beispiel werden wir Label es "Demo Dashboard".

Klicken Sie jetzt auf das Dataset erstellt der Stream Analytics-Auftrag (im aktuellen Beispiel Pbidemo). Eine Seite wird werden zum Erstellen eines Diagramms auf dieses Dataset. Der folgende Code ist ein Beispiel der Berichte, die Sie erstellen können:

Σ Temp auswählen und Zeitpunkt Felder. Sie gehen automatisch und Achse des Diagramms:

![graphic7][graphic7]

Mit dieser Option erhalten Sie automatisch ein Diagramm wie folgt:

![graphic8][graphic8]

Klicken Sie im Abschnitt auf das Dropdownmenü für Temp **Durchschnitt** für die Temperatur und im Diagramm auswählen klicken Sie auf **Darstellung** und **Liniendiagramm**wählen:

![graphic9][graphic9]

Jetzt erhalten ein Liniendiagramm Durchschnitt über Zeit.  Mit der Pin-Option unten können Sie diese in Ihr Dashboard anheften, die Sie zuvor erstellt haben:

![graphic10][graphic10]

Jetzt sehen Sie beim Anzeigen des Dashboards mit diesem angehefteten Bericht Bericht in Echtzeit aktualisiert hat. Versuchen Sie, die Daten in Ihre Ereignisse – Sammlung Temp oder etwas, und Sie sehen den Echtzeit Effekt im Dashboard dargestellt.

Beachten Sie, dass in diesem Lernprogramm erstellen veranschaulicht jedoch eine Art von Diagramm für ein Dataset. Power BI hilft anderen Kunden Business Intelligence-Tools für Ihr Unternehmen erstellen. Beispielsweise ein Power BI-Dashboard das Video [Erste Schritte mit Power BI](https://youtu.be/L-Z_6P56aas?t=1m58s) .

Weitere Informationen zum Konfigurieren von Power BI-Ausgabe und Power BI-Gruppen finden Sie im [Abschnitt Power BI](stream-analytics-define-outputs.md#power-bi) [Verständnis Stream Analytics gibt](stream-analytics-define-outputs.md "Verständnis Stream Analytics gibt"). Eine weitere nützliche Ressource Weitere Informationen zum Erstellen von Dashboards mit Power BI ist [Power BI Dashboards](https://powerbi.microsoft.com/documentation/powerbi-service-dashboards/).

## <a name="limitations-and-best-practices"></a>Nachteile und best practices

Power BI beschäftigt Parallelität und Durchsatz Constraints wie hier beschrieben: [https://powerbi.microsoft.com/pricing](https://powerbi.microsoft.com/pricing "Power BI Preise")

Aufgrund dieser Länder Power BI selbst natürlich Fälle Azure Stream Analytics eine große Datenmengen laden Verringerung tut.
Wir empfehlen TumblingWindow oder HoppingWindow datenpush höchstens 1 Push/s wäre und Ihre Abfrage innerhalb der Durchsatz kommt, können Sie die folgende Formel den Wert geben Ihr Fenster in Sekunden berechnen:

![equation1](./media/stream-analytics-power-bi-dashboard/equation1.png)  

Beispiel – wenn 1.000 Geräte senden Daten pro Sekunde auf dem Power BI Pro SKU, die 1.000.000 Zeilen pro Stunde unterstützt werden und Sie wollen Durchschnittswerte pro Gerät auf Power BI möglich höchstens einen Push alle 4 Sekunden pro Gerät (siehe unten):  

![preisbildung2](./media/stream-analytics-power-bi-dashboard/equation2.png)  

Das bedeutet, dass wir die ursprüngliche Abfrage ändern möchten:

    SELECT
        MAX(hmdt) AS hmdt,
        MAX(temp) AS temp,
        System.TimeStamp AS time,
        dspl
    INTO
        OutPBI
    FROM
        Input TIMESTAMP BY time
    GROUP BY
        TUMBLINGWINDOW(ss,4),
        dspl

### <a name="powerbi-view-refresh"></a>PowerBI Ansicht aktualisieren

Eine häufige gestellte Frage ist "Warum das Dashboard in PowerBI AutoUpdate nicht?".

Dazu Fragen und PowerBI nutzen eine Frage wie "Maximalwert von Timestamp heute ist Temp" und pin die Kachel zum Dashboard.

### <a name="renew-authorization"></a>Erneuerung der Zulassung

Sie müssen Ihre Power BI-Konto erneut authentifizieren, wenn das Kennwort geändert hat, seit Ihrem Auftrag erstellt oder zuletzt authentifiziert wurde. Wenn mehrstufige Authentifizierung (MFA) auch Power BI Autorisierung alle 2 Wochen erneuern müssen auf Ihrem Mandanten Azure Active Directory (AAD) konfiguriert ist. Ein Symptom dieses Problems ist keine Auftragsausgabe und authentifizieren Benutzerfehler"" in der Protokolle:

![graphic12][graphic12]

Wenn ein Auftrag zu starten versucht, während das Token abgelaufen ist, tritt ein Fehler auf und Start Job fehl. Die Fehlermeldung sieht in etwa wie folgt:

![Validierungsfehler PowerBI](./media/stream-analytics-power-bi-dashboard/stream-analytics-power-bi-dashboard-token-expire.png) 
 

Um dieses Problem zu beheben, beenden Sie ausgeführte Auftrag und zur Power BI-Ausgabe.  Klicken Sie auf "Erneuern Autorisierung" und starten Sie Ihr Projekt von der letzten Ausführung beendet um Datenverluste zu vermeiden.

![PowerBI Überprüfung Erneuerung](./media/stream-analytics-power-bi-dashboard/stream-analytics-power-bi-dashboard-token-renew.png) 

Nach die Autorisierung mit Power BI aktualisiert wird, sehen Sie eine grüne Warnung im Bereich Berechtigung:

![PowerBI Überprüfung Erneuerung](./media/stream-analytics-power-bi-dashboard/stream-analytics-power-bi-dashboard-token-renewed.png) 

## <a name="get-help"></a>Hilfe
Versuchen Sie weitere Unterstützung benötigen, [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Nächste Schritte

- [Einführung in Azure Stream Analytics](stream-analytics-introduction.md)
- [Erste Schritte mit Azure Stream Analytics](stream-analytics-get-started.md)
- [Skalieren von Azure Stream Analytics-Aufträge](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics Management REST-API-Referenz](https://msdn.microsoft.com/library/azure/dn835031.aspx)


[graphic1]: ./media/stream-analytics-power-bi-dashboard/1-stream-analytics-power-bi-dashboard.png
[graphic2]: ./media/stream-analytics-power-bi-dashboard/2-stream-analytics-power-bi-dashboard.png
[graphic3]: ./media/stream-analytics-power-bi-dashboard/3-stream-analytics-power-bi-dashboard.png
[graphic4]: ./media/stream-analytics-power-bi-dashboard/4-stream-analytics-power-bi-dashboard.png
[graphic5]: ./media/stream-analytics-power-bi-dashboard/5-stream-analytics-power-bi-dashboard.png
[graphic6]: ./media/stream-analytics-power-bi-dashboard/6-stream-analytics-power-bi-dashboard.png
[graphic7]: ./media/stream-analytics-power-bi-dashboard/7-stream-analytics-power-bi-dashboard.png
[graphic8]: ./media/stream-analytics-power-bi-dashboard/8-stream-analytics-power-bi-dashboard.png
[graphic9]: ./media/stream-analytics-power-bi-dashboard/9-stream-analytics-power-bi-dashboard.png
[graphic10]: ./media/stream-analytics-power-bi-dashboard/10-stream-analytics-power-bi-dashboard.png
[graphic11]: ./media/stream-analytics-power-bi-dashboard/11-stream-analytics-power-bi-dashboard.png
[graphic12]: ./media/stream-analytics-power-bi-dashboard/12-stream-analytics-power-bi-dashboard.png
[graphic13]: ./media/stream-analytics-power-bi-dashboard/13-stream-analytics-power-bi-dashboard.png
