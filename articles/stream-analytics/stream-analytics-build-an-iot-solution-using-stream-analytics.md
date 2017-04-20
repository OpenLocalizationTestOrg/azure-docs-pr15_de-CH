<properties
    pageTitle="IoT-Lösung mithilfe von Stream Analytics erstellen | Microsoft Azure"
    description="Erste Schritte-Lernprogramm Stream Analytics IoT Lösung Mautstation Szenario"
    keywords="Fensterfunktionen IOT-Lösung"
    documentationCenter=""
    services="stream-analytics"
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"
/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"
/>

# <a name="build-an-iot-solution-by-using-stream-analytics"></a>Erstellen Sie IoT-Lösung mithilfe von Stream Analytics

## <a name="introduction"></a>Einführung

In diesem Lernprogramm erfahren Sie, wie Sie Azure Stream Analytics Echtzeit Einblicke aus den Daten abgerufen. Entwickler können problemlos Datenströme, auf Streams, Protokolle und Gerät generierte Ereignisse mit Datensätze oder Daten auf geschäftliche Einblicke kombinieren. Als Dienst, der in Microsoft Azure gehostet Berechnung vollständig verwaltete Echtzeit Stream bietet Azure Stream Analytics integrierte Flexibilität, niedrige Latenz und skalierbar und in Minuten zu.

Nach Abschluss dieses Tutorials können, Sie:

-   Machen Sie sich mit dem Azure Stream Analytics-Portal.
-   Konfigurieren und Bereitstellen eines Streaming-Auftrags.
-   Erläutern Sie der Probleme und lösen sie mithilfe der Abfragesprache Stream Analytics.
-   Entwickeln Sie Streaming-Lösungen für Ihre Kunden vertrauen Stream Analytics mit.
-   Verwenden Sie die Überwachung und Protokollierung Erfahrung zur Fehlerbehebung.

## <a name="prerequisites"></a>Erforderliche Komponenten

Sie benötigen die folgenden Komponenten zum Bearbeiten dieses Lernprogramms:

-   Die neueste Version von [Azure PowerShell](../powershell-install-configure.md)
-   Visual Studio 2015 oder der [Visual Studio-Community](https://www.visualstudio.com/products/visual-studio-community-vs.aspx)
-   Ein [Azure-Abonnement](https://azure.microsoft.com/pricing/free-trial/)
-   Administratorrechte auf dem computer
-   Herunterladen der [TollApp.zip](http://download.microsoft.com/download/D/4/A/D4A3C379-65E8-494F-A8C5-79303FD43B0A/TollApp.zip) aus dem Microsoft Download Center
-   Optional: Quellcode Ereignisgenerator TollApp in [GitHub](https://aka.ms/azure-stream-analytics-toll-source)

## <a name="scenario-introduction-hello-toll"></a>Szenario-Einführung: "Hallo, Toll!"


Eine Station gebührenpflichtig ist häufig. Treten sie auf viele Autobahnen, Brücken und Tunnel weltweit. Jeder Mautstation hat mehrere mautstellen. Manuelle stände beenden Sie zu der Toll einer Telefonzentrale. Automatisierte stände scannt ein Sensor auf jedem Stand eine RFID-Karte an der Windschutzscheibe des Fahrzeugs angebracht ist, wie Sie die Mautstation. Es ist einfach den Durchgang von Fahrzeugen durch diese Mautstation als Ereignisstream visualisieren die interessante Operationen ausgeführt werden können.

![Bild des Autos an gebührenpflichtig](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image1.jpg)

## <a name="incoming-data"></a>Eingehende Daten

In diesem Lernprogramm kann mit zwei Datenströme. Sensoren in der Eingangs- und der mautstellen erzeugt den ersten Datenstrom. Der zweite Stream ist eines statischen Datasets Fahrzeug Registrierungsdaten.

### <a name="entry-data-stream"></a>Eingabe-Datenstrom

Eingabe-Datenstrom enthält Informationen über Autos Mautstation betreten.


| TollID | EntryTime               | Nummernschild | Zustand | Stellen   | Modell   | VehicleType | VehicleWeight | Gebührenpflichtig | Tag       |
|--------|-------------------------|--------------|-------|--------|---------|-------------|---------------|------|-----------|
| 1      | 2014-09-10 12:01:00.000 | JNB 7001     | NY    | Honda  | KRV     | 1           | 0             | 7    |           |
| 1      | 2014-09-10 12:02:00.000 | YXZ 1001     | NY    | Toyota | Camry   | 1           | 0             | 4    | 123456789 |
| 3      | 2014-09-10 12:02:00.000 | ABC 1004     | CT    | Ford   | Stier  | 1           | 0             | 5    | 456789123 |
| 2      | 2014-09-10 12:03:00.000 | XYZ 1003     | CT    | Toyota | Krone | 1           | 0             | 4    |           |
| 1      | 2014-09-10 12:03:00.000 | 1007 BNJ     | NY    | Honda  | KRV     | 1           | 0             | 5    | 789123456 |
| 2      | 2014-09-10 12:05:00.000 | CDE 1007     | NJ    | Toyota | 4 x 4     | 1           | 0             | 6    | 321987654 |


Hier wird eine kurze Beschreibung der Spalten:

| Spalte | Beschreibung |
|--------------|--------------------------------------------------------------------|
| TollID       | Gebührenpflichtig Stand Kennung, die eindeutig eine Mautstation            |
| EntryTime    | Datum und Uhrzeit der Eingabe des Fahrzeugs Mautstation in UTC |
| Nummernschild | Das Kennzeichen des Fahrzeugs                            |
| Zustand        | Status in USA                                           |
| Stellen         | Der Hersteller des Autos                                 |
| Modell        | Die Modellnummer des Autos                                 |
| VehicleType  | 1 für Pkws oder 2 für Nutzfahrzeuge       |
| WeightType   | Gewicht in Tonnen; 0 für PKW                   |
| Gebührenpflichtig         | Gebührenpflichtige Wert in CHF                                              |
| Tag          | Das e-Tag auf das Auto, das Zahlung automatisiert; leer, in dem die Zahlung manuell durchgeführt wurde |


### <a name="exit-data-stream"></a>Exit-Datenstrom

Exit-Datenstrom enthält Informationen über Autos die Mautstation verlassen.


| **TollId** | **ExitTime**                 | **Nummernschild** |
|------------|------------------------------|------------------|
| 1          | 2014-09-10T12:03:00.0000000Z | JNB 7001         |
| 1          | 2014-09-10T12:03:00.0000000Z | YXZ 1001         |
| 3          | 2014-09-10T12:04:00.0000000Z | ABC 1004         |
| 2          | 2014-09-10T12:07:00.0000000Z | XYZ 1003         |
| 1          | 2014-09-10T12:08:00.0000000Z | 1007 BNJ         |
| 2          | 2014-09-10T12:07:00.0000000Z | CDE 1007         |

Hier wird eine kurze Beschreibung der Spalten:


| Spalte | Beschreibung |
|--------------|-----------------------------------------------------------------|
| TollID       | Gebührenpflichtig Stand Kennung, die eindeutig eine Mautstation         |
| ExitTime     | Datum und Uhrzeit des Verlassens des Fahrzeugs Mautstation in UTC |
| Nummernschild | Das Kennzeichen des Fahrzeugs                         |

### <a name="commercial-vehicle-registration-data"></a>Registrierungsdaten Nutzfahrzeug

Das Lernprogramm verwendet einen statischen Snapshot der Registrierungsdatenbank Nutzfahrzeug.


| Nummernschild | Attribute | Abgelaufen |
|--------------|----------------|---------|
| SVT 6023     | 285429838      | 1       |
| XLZ 3463     | 362715656      | 0       |
| PK 1005     | 876133137      | 1       |
| RIV 8632     | 992711956      | 0       |
| SNY 7188     | 592133890      | 0       |
| ELH 9896     | 678427724      | 1       |

Hier wird eine kurze Beschreibung der Spalten:


| Spalte | Beschreibung |
|--------------|-----------------------------------------------------------------|
| Nummernschild | Das Kennzeichen des Fahrzeugs                         |
| Attribute     | Das Fahrzeug-Identifikationsnummer |
| Abgelaufen | Registrierungsstatus des Fahrzeugs: 0 Kfz-Steuer ist 1, wenn die Registrierung abgelaufen ist                 |


## <a name="set-up-the-environment-for-azure-stream-analytics"></a>Einrichten der Umgebung für Azure Stream Analytics

Um dieses Lernprogramm benötigen Sie ein Microsoft Azure-Abonnement. Microsoft bietet kostenlose Testversion für Microsoft Azure Services.

Wenn Sie nicht über ein Azure-Konto verfügen, können Sie [eine kostenlose Testversion](http://azure.microsoft.com/pricing/free-trial/).

> [AZURE.NOTE] Um sich für eine kostenlose Testversion, benötigen Sie ein mobiles Gerät, das Textnachrichten und eine gültige Kreditkarte empfangen kann.

Müssen Sie die Schritte im Abschnitt "Bereinigen Sie Ihre Azure-Konto" am Ende dieses Artikels, damit Sie die $200 Azure Gutschrift frei nutzen können.

## <a name="provision-azure-resources-required-for-the-tutorial"></a>Bereitstellung von Azure Ressourcen für das Lernprogramm

Dieses Lernprogramm erfordert zwei Ereignis-Hubs *Eintrag* und *Beenden* Datenströme empfangen. Azure SQL-Datenbank gibt die Ergebnisse der Stream Analytics-Aufträge. Azure-Speicher speichert Referenzdaten über Neuzulassungen.

Das Skript Setup.ps1 können im Ordner TollApp auf GitHub Sie um alle erforderliche Ressourcen zu erstellen. Aus Zeitgründen werden wir empfehlen ausführen. Wenn Sie weitere Informationen zu diesen Ressourcen in Azure-Portal konfigurieren möchten, finden Sie im Anhang "Tutorial Ressourcen in Azure-Portal konfigurieren".

Laden Sie und speichern Sie die unterstützende [TollApp](http://download.microsoft.com/download/D/4/A/D4A3C379-65E8-494F-A8C5-79303FD43B0A/TollApp.zip) Ordner und Dateien.

Öffnen Sie ein **Microsoft Azure PowerShell** -Fenster _als Administrator_. Wenn Sie noch nicht über Azure PowerShell verfügen, gehen Sie in [Installation und Konfiguration von Azure PowerShell](../powershell-install-configure.md) installieren.

Da Windows automatisch ps1, DLL- und .exe-Dateien blockiert, müssen Sie die Ausführungsrichtlinie festlegen, bevor Sie das Skript ausführen. Stellen Sie sicher, dass im Azure PowerShell _als Administrator_ausgeführt wird. **Set-ExecutionPolicy uneingeschränkt**ausgeführt. Wenn Sie aufgefordert werden, geben Sie **J**ein.

![Screenshot von "Set-ExecutionPolicy uneingeschränkten" Azure PowerShell-Fenster Ausführen](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image2.png)

Führen Sie **Get-ExecutionPolicy** sicherstellen, dass der Befehl funktioniert.

![Screenshot von "Get-ExecutionPolicy" in Azure PowerShell-Fenster Ausführen](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image3.png)

Wechseln Sie zum Verzeichnis mit den Skripts und Generator Anwendung.

![Screenshot von "cd .\TollApp\TollApp" Azure PowerShell-Fenster Ausführen](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image4.png)

Typ **.\\ Setup.ps1** zum Einrichten der Azure-Konto erstellen und konfigurieren und alle erforderliche Ressourcen zum Generieren von Ereignissen starten. Das Skript wählt nach dem Zufallsprinzip eine Region, Ihre Ressourcen zu erstellen. Um explizit einen Bereich angeben, kann die **-Speicherort** Parameter wie im folgenden Beispiel:

**. \\Setup.ps1-Speicherort "USA"**

![Screenshot der Azure-Anmeldeseite](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image5.png)

Das Skript wird **die Anmeldeseite** für Microsoft Azure. Geben Sie Ihre Anmeldeinformationen ein.

> [AZURE.NOTE] Wenn Ihr Konto mehrere Abonnements zugreifen, müssen Sie den Namen eingeben, den für das Lernprogramm verwenden möchten.

Das Skript dauert mehrere Minuten. Nach Beendigung sollte die Ausgabe die folgenden Screenshot aussehen.

![Screenshot der Ausgabe des Skripts in Azure PowerShell-Fenster](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image6.PNG)

Sie sehen auch ein anderes Fenster, die ungefähr dem folgenden Screenshot. Diese Anwendung sendet Ereignisse an Azure Ereignis Hubs benötigt, um das Lernprogramm auszuführen. So beenden Sie die Anwendung oder nicht schließen Sie dieses Fenster, bis Sie das Lernprogramm abgeschlossen haben.

![Screenshot von "Senden Ereignisdaten Hub"](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image7.png)

Sie sollten jetzt alle erstellte Ressourcen in Azure-Portal angezeigt werden. Rufen Sie <https://manage.windowsazure.com>und Ihre Anmeldeinformationen anmelden.

### <a name="azure-event-hubs"></a>Azure Event Hubs

Klicken Sie auf **SERVICE BUS** auf der linken Seite der Azure-Portal um das Skript im vorherigen Abschnitt erstellte Ereignis Hubs anzuzeigen.

![Servicebus](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image8.png)

Sie sehen alle verfügbaren Namespaces in Ihrem Abonnement. Klicken Sie auf die mit *Tolldata* (in unserem Beispiel tolldata4637388511). Klicken Sie auf die Registerkarte **Ereignis HUBS** .

![Ereignis-Hubs auf Azure-portal](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image9.png)

Sie sehen zwei benannte *Eintrag* Ereignis Hubs und *Beenden* in diesem Namespace erstellt.

![Screenshot von "Entry" und "Beenden" Ereignis-Hubs in Azure-portal](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image10.png)

### <a name="azure-storage-container"></a>Azure Behälter

1. Klicken Sie auf **Speicher** auf der linken Seite der Azure-Portal Azure Storage Container angezeigt, der im Lernprogramm verwendet wird.

    ![Menüelement für Speicher](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image11.png)

2. Klicken Sie auf die mit *Tolldata* (in unserem Beispiel tolldata4637388511). Klicken Sie auf die Registerkarte **Container** um den erstellten Container anzuzeigen.

    ![Container auf Azure-portal](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image12.png)

3. Klicken Sie auf **Tolldata** Container JSON Uploaddatei sehen, das fahrzeugregisterdaten.

    ![Screenshot der Datei registration.json im container](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image13.png)

### <a name="azure-sql-database"></a>SQL Azure-Datenbank

1. Klicken Sie auf **SQL-Datenbanken** auf der linken Seite des Portals Azure SQL-Datenbank angezeigt, die im Lernprogramm verwendet wird.

    ![SQL-Datenbanken](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image14.png)

2. Klicken Sie auf **Tolldatadb**.

    ![Screenshot der erstellten SQL-Datenbank](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image15.png)

3. Kopieren Sie den Servernamen ohne Anschlussnummer (*Servername*. database.windows.net Beispiel).

## <a name="connect-to-the-database-from-visual-studio"></a>Verbinden Sie mit der Datenbank von Visual Studio

Verwenden Sie Visual Studio, um Abfrageergebnisse in der Ausgabedatenbank zuzugreifen.

Verbinden Sie mit der SQL-Datenbank (Ziel) von Visual Studio:

1. Öffnen Sie Visual Studio, und klicken Sie auf **Extras** > **mit Datenbank verbinden**.

2. Wenn Sie aufgefordert werden, klicken Sie auf **Microsoft SQL Server** als Datenquelle.

    ![Das Dialogfeld Datenquelle ändern](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image16.png)

3. Fügen Sie im Feld **Servername** den Namen, die Sie im vorherigen Abschnitt von Azure-Portal kopiert (d. h. *Servername*. database.windows.net).

4. Klicken Sie auf **SQL Server-Authentifizierung verwenden**.

5. Geben Sie im Feld **Benutzername** **Tolladmin** und **123toll!** in das Feld **Kennwort** .

6. Klicken Sie auf **Wählen oder geben Sie einen Datenbanknamen**, und wählen Sie **TollDataDB** als Datenbank.

    ![Verbindung hinzufügen](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image17.jpg)

7. Klicken Sie auf **OK**.

8. Öffnen Sie Server-Explorer.

    ![Server-Explorer](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image18.png)

9. Finden Sie vier Tabellen in der Datenbank TollDataDB.

    ![Tabellen in der Datenbank TollDataDB](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image19.jpg)

## <a name="event-generator-tollapp-sample-project"></a>Ereignisgenerator: TollApp-Beispielprojekt

PowerShell-Skript automatisch Ereignisse mithilfe der TollApp Beispiel-Anwendung senden. Sie müssen keine zusätzlichen Schritte auszuführen.

Aber wenn Sie Implementierungsdetails interessiert sind, finden den Quellcode der Anwendung TollApp Sie in GitHub [Proben-TollApp](https://aka.ms/azure-stream-analytics-toll-source).

![Screenshot des Beispielcodes in Visual Studio angezeigt](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image20.png)

## <a name="create-a-stream-analytics-job"></a>Erstellen Sie einen Stream Analytics-Auftrag

1. Azure-Portal öffnen Sie Stream Analytics und auf **neu** in der unteren linken Ecke der Seite zum Erstellen eines neuen Auftrags Analytics.

    ![Neue Schaltfläche](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image21.png)

2. Klicken Sie auf **QUICK erstellen**. Auswählen der Region, wo die Ressourcen vom Skript erstellt werden.

3. Wählen Sie für die Einstellung **Regionalen Überwachung SPEICHERKONTO** **Neue SPEICHERKONTO** und geben Sie einen eindeutigen Namen. Azure Stream Analytics verwendet dieses Konto, um Überwachungsinformationen für alle zukünftigen Projekte speichern.

4. Klicken Sie am unteren Rand der Seite **STREAM ANALYTICS-Auftrag erstellen** .

    ![Stream Analytics Job Option erstellen](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image22.png)

## <a name="define-input-sources"></a>Eingabequellen definieren

1. Klicken Sie auf Auftrag erstellten Analytics im Portal.

    ![Screenshot des Auftrags Analytics im portal](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image23.jpg)

2. Klicken Sie auf **Eingänge** , um die Daten zu definieren.

    ![Registerkarte Eingaben](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image24.jpg)

3. Klicken Sie auf **Eingabe**.

    ![Ein Eingabe-Option hinzufügen](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image25.png)

4. Klicken Sie auf der ersten Seite **DATENSTREAM** .

    ![Die Option-Datenstrom](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image26.png)

5. Klicken Sie auf der zweiten Seite des Assistenten auf **EVENT HUB** .

    ![Die Option Event Hub](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image27.png)

6. Geben Sie **EntryStream** als **Eingabe ALIAS**.

7. Klicken Sie auf den Dropdownpfeil **Event Hub** und wählen Sie aus, die mit "TollData" (beispielsweise TollData9518658221).

8. **Eintrag als Hubnamen und **Alle** als Ereignis Hub Richtlinienname.**

    Wie sieht Ihre:

    ![Ereignis-Hub settings](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image28.png)

9. Wählen Sie auf der nächsten Seite **JSON** **SERIALISIERUNGSFORMAT Ereignis** und **UTF8** **Kodierung**.

    ![Serialisierungseinstellungen](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image29.png)

10. Klicken Sie auf **OK** am unteren Rand der Seite, um den Assistenten zu beenden.

    ![Screenshot des EntryStream Eingabe in Azure-portal](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image30.jpg)

    Erstellung den Eintrag Stream folgen Sie dieselben Schritte zum Erstellen des Streams beenden. Müssen Sie auf der dritten Seite des Assistenten wie in der folgenden Screenshot eingeben.

    ![Einstellung für den Stream beenden](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image31.png)

    Sie haben zwei Eingabestreams definiert:

    ![Azure-Portal gemäß Eingabestreams](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image32.jpg)

    Als Nächstes fügen Sie Verweis Dateneingabe für BLOB-Datei, die Auto-Registrierungsdaten enthält.

11. Klicken Sie auf **Hinzufügen**und dann auf **Daten**.

    ![Optionen mit Referenzdaten ausgewählt "Eingabe hinzufügen"](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image33.png)

12. Wählen Sie auf der nächsten Seite das Speicherkonto mit **Tolldata**beginnt. Der Containername sollte **Tolldata**und der BLOB-Name unter **Pfad Muster** sollte **registration.json**sein. Dieser Dateiname Groß-/Kleinschreibung und Kleinbuchstaben werden sollte.

    ![Blogeinstellungen Speicher](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image34.png)

13. Wählen Sie die Werte wie in der folgenden Abbildung auf der nächsten Seite, und klicken Sie auf **OK** , um den Assistenten abzuschließen.

    ![Auswahl von JSON für "auch Serialisierungsformat" und UTF8 "Kodierung"](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image35.png)

Jetzt werden alle Eingaben definiert.

![Screenshot der drei definierten Eingaben](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image36.jpg)

## <a name="define-output"></a>Definieren

1. Klicken Sie auf der Registerkarte **Ausgabe** und klicken Sie dann auf **Eine Ausgabe hinzufügen**.

    ![Registerkarte "Ausgabe" und "Ausgabe hinzufügen"](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image37.jpg)

2. Klicken Sie auf **SQL-Datenbank**.

3. Wählen Sie den Servernamen, der im Abschnitt "Verbindung zu Datenbank von Visual Studio" des Artikels verwendet wurde. Der Datenbankname ist **TollDataDB**.

4. Geben Sie im Feld **Benutzername** **Tolladmin** **123toll!** im Feld **Kennwort** und **TollDataRefJoin** im Feld **Tabelle** .

    ![SQL Database settings](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image38.jpg)

## <a name="azure-stream-analytics-query"></a>Azure Stream Analytics Abfrage

Die Registerkarte **Abfrage** enthält eine SQL-Abfrage, die die eingehenden Daten transformiert.

![Eine Abfrage auf der Registerkarte Abfrage hinzugefügt](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image39.jpg)

In diesem Lernprogramm versucht, verschiedene Geschäftsfragen beantworten werden, Stream Analyseabfragen Daten und erstellt gebührenpflichtige, die entsprechenden Antworten in Azure Stream Analytics verwendet werden können.

Vor der ersten Stream Analytics-Auftrag betrachten wir einige Szenarien und die Abfragesyntax.

## <a name="introduction-to-azure-stream-analytics-query-language"></a>Einführung in Azure Stream Analytics Abfragesprache
-----------------------------------------------------

Angenommen, die Anzahl der Fahrzeuge, die eine Mautstation eingeben müssen. Dies ist ein fortlaufendes Ereignisse müssen Sie eine "Zeitraum." Wir ändern die Frage "wie viele Fahrzeuge eine Mautstation alle drei Minuten eingeben?". Dies wird häufig als Tumbling Count bezeichnet.

Betrachten wir Azure Stream Analytics-Abfrage, die diese Frage beantwortet:

    SELECT TollId, System.Timestamp AS WindowEnd, COUNT(*) AS Count
    FROM EntryStream TIMESTAMP BY EntryTime
    GROUP BY TUMBLINGWINDOW(minute, 3), TollId

Azure Stream Analytics verwendet eine Abfragesprache, die wie SQL und fügt einige Extensions zeitliche Aspekte der Abfrage angeben.

Ausführliche Informationen Sie zu [Zeit](https://msdn.microsoft.com/library/azure/mt582045.aspx) und [Fensterfunktionen](https://msdn.microsoft.com/library/azure/dn835019.aspx) Konstrukte der Abfrage von MSDN.

## <a name="testing-azure-stream-analytics-queries"></a>Testen von Azure Stream Analytics Abfragen

Nachdem Sie Ihre erste Azure Stream Analytics Abfrage geschrieben haben, ist Testen mit Beispieldateien im Ordner TollApp unter dem folgenden Pfad gespeichert werden:

**.. \\TollApp\\TollApp\\Daten**

Dieser Ordner enthält die folgenden Dateien:

- Entry.JSON
- Exit.JSON
- Registration.JSON

## <a name="question-1-number-of-vehicles-entering-a-toll-booth"></a>Frage 1: Anzahl der Fahrzeuge einer Zahlstelle

1. Öffnen des Azure-Portals und Beruf Azure Stream Analytics erstellt. Klicken Sie auf die Registerkarte **Abfrage** , und fügen Sie Abfrage aus dem vorherigen Abschnitt.

    ![Abfrage in der Registerkarte Abfrage eingefügt](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image40.png)

2. Um diese Abfrage Daten zu überprüfen, klicken Sie auf **Test** . Gehen Sie in dem daraufhin angezeigten Dialogfeld Entry.json eine Datei mit Daten aus dem **EntryTime** .

    ![Screenshot der Entry.json-Datei](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image41.png)

3. Ist die Ausgabe der Abfrage zu überprüfen:

    ![Die Testergebnisse](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image42.jpg)

## <a name="question-2-report-total-time-for-each-car-to-pass-through-the-toll-booth"></a>Frage 2: Bericht Gesamtzeit für jedes Auto passieren der Zahlstelle

Die durchschnittliche Zeit, die für ein Auto Verkehrsnetze passieren muss hilft um die Effizienz und die Kundenzufriedenheit zu bewerten.

Um die Gesamtzeit zu suchen, müssen Sie EntryTime-Stream mit der ExitTime verknüpfen. Datenströme Spalten TollId und LicencePlate treten. Der **JOIN** -Operator erfordert zeitliche Spielraum geben, der die zulässige Zeit zwischen verknüpften Ereignisse beschreibt. **DATEDIFF** -Funktion verwendet um anzugeben, dass Ereignisse nicht mehr als 15 Minuten voneinander sein soll. Sie gelten auch die **DATEDIFF** -Funktion beenden und Eintrag Zeiten berechnet die tatsächliche Zeit, die ein Auto in der Mautstation verbringt. Beachten Sie den Unterschied zur Verwendung von **DATEDIFF** , wenn in einer **SELECT** -Anweisung als eine **JOIN** -Bedingung verwendet wird.

    SELECT EntryStream.TollId, EntryStream.EntryTime, ExitStream.ExitTime, EntryStream.LicensePlate, DATEDIFF (minute , EntryStream.EntryTime, ExitStream.ExitTime) AS DurationInMinutes
    FROM EntryStream TIMESTAMP BY EntryTime
    JOIN ExitStream TIMESTAMP BY ExitTime
    ON (EntryStream.TollId= ExitStream.TollId AND EntryStream.LicensePlate = ExitStream.LicensePlate)
    AND DATEDIFF (minute, EntryStream, ExitStream ) BETWEEN 0 AND 15

1. Aktualisieren Sie zum Testen dieser Abfrage die Abfrage auf der Registerkarte **Abfrage** des Auftrags:

    ![Abfrage in der Registerkarte "Abfrage"](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image43.jpg)

2. Klicken Sie auf **Test** und geben Sie input Beispieldateien für EntryTime und ExitTime an.

    ![Screenshot der ausgewählten Dateien](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image44.png)

3. Aktivieren Sie das Kontrollkästchen zum Testen der Abfrage und zeigen die Ausgabe:

    ![Ausgabe](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image45.png)

## <a name="question-3-report-all-commercial-vehicles-with-expired-registration"></a>Frage 3: Bericht alle Nutzfahrzeuge mit abgelaufenen Registrierung

Azure Stream Analytics können statische Snapshots der Daten mit temporäre Datenströme. Um dies zu demonstrieren, verwenden Sie die folgenden beispielfrage.

Wenn eines gebührenpflichtig Unternehmen registriert ist, können sie ohne Kontrolle durch die Mautstation übergeben. Nachschlagetabelle Nutzfahrzeug Registrierung verwendet alle Nutzfahrzeuge identifizieren, der Registrierung abgelaufen sind.

    SELECT EntryStream.EntryTime, EntryStream.LicensePlate, EntryStream.TollId, Registration.RegistrationId
    FROM EntryStream TIMESTAMP BY EntryTime
    JOIN Registration
    ON EntryStream.LicensePlate = Registration.LicensePlate
    WHERE Registration.Expired = '1'

Zum Testen einer Abfrage mithilfe von Daten müssen Sie eine Eingabequelle für Referenzdaten, definieren Sie getan haben.

1. Um diese Abfrage zu testen, fügen Sie die Abfrage in der Registerkarte **Abfrage** klicken Sie auf **Test**und angegeben Sie zwei Eingabequellen:

    ![Screenshot der ausgewählten Dateien](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image46.png)

2. Die Ausgabe der Abfrage anzeigen:

    ![Screenshot der Abfrageausgabe](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image47.png)

## <a name="start-the-stream-analytics-job"></a>Stream Analytics-Auftrag starten


Jetzt ist es Zeit, die Konfiguration und starten Sie den Auftrag. Speichern Sie die Abfrage von Frage 3 wird die Ausgabe, die das Schema der **TollDataRefJoin** Ausgabetabelle entspricht.

Gehe zu Einzelvorgang **DASHBOARD**, und klicken Sie auf **START**.

![Screenshot der Schaltfläche Start Job-dashboard](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image48.jpg)

Klicken Sie im Dialogfeld, das geöffnet wird, ändern Sie die **Ausgabe beginnen** **benutzerdefinierte**Zeit. Ändern Sie die Stunde auf eine Stunde vor der aktuellen Zeit. Diese Änderung wird sichergestellt, dass alle Ereignisse vom Event Hub seit dem Generieren der Ereignisse am Anfang des Lernprogramms verarbeitet werden. Klicken Sie nun auf das Kontrollkästchen, um den Auftrag zu starten.

![Benutzerdefinierte Auswahl](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image49.png)

Beginnen kann einige Minuten dauern. Sie können den Status auf der obersten Ebene Stream Analytics finden Sie unter.

![Bildschirmabbild des Status des Einzelvorgangs](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image50.jpg)

## <a name="check-results-in-visual-studio"></a>Ergebnisse in Visual Studio

1. Visual Studio Server-Explorer öffnen und mit der rechten Maustaste in der **TollDataRefJoin** -Tabelle.
2. Klicken Sie auf die **Tabellendaten anzeigen** , um die Ausgabe des Projekts angezeigt.

    ![Auswahl von "Tabellendaten anzeigen" im Server-Explorer](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image51.jpg)

## <a name="scale-out-azure-stream-analytics-jobs"></a>Skalierung bei Azure Stream Analytics Aufträge

Azure Stream Analytics elastisch skalieren soll, dass große Datenmengen verarbeiten kann. Eine **PARTITION BY** -Klausel können Abfrage Azure Stream Analytics System erkennen, dem diesen Schritt skalieren. **IDs** ist eine spezielle Spalte entsprechend die Partitions-ID der Eingabe (Ereignis Hub) hinzugefügt.

    SELECT TollId, System.Timestamp AS WindowEnd, COUNT(*)AS Count
    FROM EntryStream TIMESTAMP BY EntryTime PARTITION BY PartitionId
    GROUP BY TUMBLINGWINDOW(minute,3), TollId, PartitionId

1. Beenden Sie aktuellen Auftrag, aktualisieren Sie die Abfrage in der Registerkarte **Abfrage** und öffnen Sie die Registerkarte **Skalierung**

    **Streaming-Einheiten** definieren Compute-Stromversorgung, die der Auftrag empfangen können.

2. Verschieben Sie den Regler auf 6.

    ![Abbildung 6 streaming Einheiten auswählen](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image52.jpg)

3. Wechseln Sie zur Registerkarte **Ausgaben** , und ändern Sie den Namen der SQL-Tabelle in **TollDataTumblingCountPartitioned**.

Wenn Sie ihn starten, können Azure Stream Analytics Arbeit auf mehr Datenverarbeitungsressourcen verteilen und besseren Durchsatz erreichen. Beachten Sie, dass TollApp Anwendung auch Partitionen nach TollId Ereignisse senden.

## <a name="monitor"></a>Monitor

Die Registerkarte **MONITOR** enthält Statistiken über die laufende Arbeit.

![Screenshot von Statistiken zum Ausführen von Aufträgen](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image53.png)

Sie können die Registerkarte **DASHBOARD** **Protokolle** zugreifen.

![Die Option "Protokolle"](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image54.jpg)

![Bildschirmabbild des Status von Aufträgen Sie finden Protokolle](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image55.png)

Um weitere Informationen zu einem bestimmten Ereignis anzuzeigen, klicken Sie auf das Ereignis und klicken Sie dann auf die Schaltfläche **DETAILS** .

![Screenshot von Details zu einem ausgewählten Ereignis](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image56.png)

## <a name="conclusion"></a>Abschluss

Diese praktische Einführung sollte Ihnen Azure Stream Analytics-Dienst. Konfigurieren von Eingaben und Ausgaben für das Projekt Stream Analytics gezeigt. Mit Szenario gebührenpflichtig, erläutert das Lernprogramm auftretenden Probleme innerhalb von Daten mit SQL-ähnliche Abfragen in Azure Stream Analytics und wie sie gelöst werden können. Das Lernprogramm beschriebenen SQL Erweiterung Konstrukte für temporäre Daten. Es zeigte den Datenstrom mit statischen Daten bereichern Datenströme beitreten und zum Skalieren einer Abfrage zu höheren Durchsatz.

Obwohl dieses Lernprogramm Einführung bietet, ist es nicht von Mitteln. Finden Sie weitere [Beispiele für allgemeine Stream Analytics Verwendungsmuster für](stream-analytics-stream-analytics-query-patterns.md)SAQL Sprache mit Abfragemuster.
Lesen Sie die [online-Dokumentation](https://azure.microsoft.com/documentation/services/stream-analytics/) Azure Stream Analytics erfahren.

## <a name="clean-up-your-azure-account"></a>Bereinigen Sie Ihre Azure-Konto

1. Beenden Sie Stream Analytics in Azure-Portal.

    Das Setup.ps1-Skript erstellt zwei Ereignis-Hubs und eine SQL-Datenbank. Folgendermaßen können Sie Bereinigen von Ressourcen am Ende des Lernprogramms.

2. Geben Sie im PowerShell-Fenster **.\\ Cleanup.ps1** , das Skript zu starten, die im Lernprogramm verwendete Ressourcen löscht.

    > [AZURE.NOTE] Ressourcen werden durch den Namen identifiziert. Stellen Sie sicher, dass Sie jedes Element überprüfen vor entfernen.

    ![Screenshot der Cleanup-Prozess](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image57.png)
