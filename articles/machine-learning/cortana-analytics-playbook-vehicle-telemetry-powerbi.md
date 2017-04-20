<properties 
    pageTitle="Fahrzeug Telemetrie Analytics Lösungsvorlage PowerBI Dashboard einrichten Hinweise | Microsoft Azure" 
    description="Verwenden Sie die Cortana Intelligenz in Echtzeit und prädiktive Einblicke Fahrzeug Gesundheits-und fahren Gewohnheiten." 
    services="machine-learning" 
    documentationCenter="" 
    authors="bradsev" 
    manager="jhubbard" 
    editor="cgronlun" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/12/2016" 
    ms.author="bradsev" />


# <a name="vehicle-telemetry-analytics-solution-template-powerbi-dashboard-setup-instructions"></a>Fahrzeug Telemetrie Analytics Lösung PowerBI Dashboard Setup Hinweise zu Vorlagen

Dieses **Menü** enthält Links zu den Kapiteln dieses Playbook. 

[AZURE.INCLUDE [cap-vehicle-telemetry-playbook-selector](../../includes/cap-vehicle-telemetry-playbook-selector.md)]


Fahrzeug Telemetrie Analytics Lösung zeigt wie Autohändler, Hersteller und Unternehmen nutzen die Funktionen der Cortana Intelligence in Echtzeit und prädiktive Einblicke Fahrzeug Gesundheit und Fahrverhalten Verbesserungen im Bereich des Kunden, Entwicklung und marketing-Kampagnen. Dieses Dokument enthält Schritt für Schritt zu konfigurieren PowerBI Berichte und Dashboards nach dem Bereitstellen der Projektmappe in Ihrem Abonnement. 


## <a name="prerequisites"></a>Erforderliche Komponenten
1.  Bereitstellen der Projektmappe Fahrzeug Telemetrie Analytics durch Navigieren zur [https://gallery.cortanaanalytics.com/SolutionTemplate/Vehicle-Telemetry-Analytics-3](https://gallery.cortanaanalytics.com/SolutionTemplate/Vehicle-Telemetry-Analytics-3)  
2.  [Installieren Sie Microsoft Power BI Desktop](http://www.microsoft.com/download/details.aspx?id=45331)
3.  Ein [Azure-Abonnement](https://azure.microsoft.com/pricing/free-trial/). Wenn Sie ein Azure-Abonnement haben, beginnen Sie mit Azure kostenloses Abonnement
4.  Microsoft PowerBI-Konto
    

## <a name="cortana-intelligence-suite-components"></a>Cortana Intelligence Suite-Komponenten
Nachrichtendienste von Cortana werden als Teil des Fahrzeugs Telemetrie Analytics Lösungsvorlage Abonnements bereitgestellt.

- **Event Hubs** für Millionen von Fahrzeug Telemetrie-Ereignisse in Azure Einnahme.
- **Stream analytischen**s in Echtzeit Einblick Fahrzeug Gesundheit und speichert diese Daten in Langzeitspeicher für umfangreichere Batch Analysen.
- Erkennen von Netzwerkanomalien in Echtzeit **Maschinelles lernen** und Stapelverarbeitung vorhersehbaren Einblicke.
- **HDInsight** zum Transformieren von Daten genutzt
- **Data Factory** behandelt Orchestrierung, Planung, Ressourcen-Management und Überwachung der Stapelverarbeitung Pipeline.

**Power BI** bietet diese Lösung rich Dashboard für Echtzeitdaten und Vorhersageanalysen Visualisierung. 

Die Lösung verwendet zwei unterschiedliche Datenquellen: **simulierte fahrzeugsignale und Diagnose Dataset** und **Fahrzeug-Katalog**.

Ein Fahrzeug Telematik-Simulator ist Bestandteil dieser Lösung. Diagnoseinformationen ausgegeben und entspricht der Zustand des Fahrzeugs und Muster an einem bestimmten Punkt in Mal signalisiert. 

Fahrzeug-Katalog ist ein Verweis Dataset mit VIN, Zuordnung


## <a name="powerbi-dashboard-preparation"></a>PowerBI Dashboard-Vorbereitung

### <a name="deployment"></a>Bereitstellung

Nach Abschluss die Bereitstellung erhalten Sie im folgende Diagramm mit allen Komponenten in grün markiert. 

- Navigieren Sie auf die entsprechenden Dienste zu überprüfen, ob diese erfolgreich bereitgestellt haben, klicken Sie auf den Pfeil oben rechts den grünen Knoten.
- Klicken Sie das Simulator-Paket den Pfeil oben rechts auf dem **Fahrzeug Telematik Simulator** -Knoten. Speichern Sie, und extrahieren Sie die Dateien lokal auf Ihrem Computer. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/1-deployed-components.png)

Nun können PowerBI Dashboard mit umfangreichen Visualisierung in Echtzeit zu konfigurieren und Gewohnheiten vorhersehbaren Einblicke Fahrzeug Gesundheits-und fahren. Dauert ca. 45 Minuten bis zu einer Stunde alle Berichte erstellen und konfigurieren das Dashboard. 


### <a name="setup-power-bi-real-time-dashboard"></a>Echtzeit-Power BI-Dashboard einrichten

**Simulierte Daten generieren**

1. Wechseln Sie auf dem lokalen Computer zu dem Ordner, in dem das Fahrzeug Telematik Simulator Paket extrahiert![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/2-vehicle-telematics-simulator-folder.png)
2.  Führen Sie die Anwendung ***CarEventGenerator.exe***.
3.  Diagnoseinformationen ausgegeben und entspricht der Zustand des Fahrzeugs und Muster an einem bestimmten Punkt in Mal signalisiert. Dies erscheint mit einer Azure Event Hub-Instanz, die als Teil der Bereitstellung konfiguriert.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/3-vehicle-telematics-diagnostics.png)
     
**Starten Sie die Dashboardanwendung in Echtzeit**

Die Lösung umfasst eine Anwendung, die eine Echtzeit-Dashboard in PowerBI generiert. Diese Anwendung wartet auf eine Event Hub-Instanz aus der Stream Analytics Ereignisse kontinuierlich veröffentlicht. Für jedes Ereignis, das diese Anwendung empfängt, verarbeitet er die Daten mithilfe eines Endpunkts Computer lernen Anforderungsantwort bewertet. Das resultierende Dataset wird auf PowerBI APIs für Visualisierung veröffentlicht. 

Für den download:

1.  Klicken Sie auf den Knoten PowerBI in der Diagrammansicht und auf **Echtzeit-Dashboard-Anwendung herunterladen**"auf den Eigenschaftenbereich.![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard-new1.png)
2.  Extrahieren Sie und speichern Sie die Anwendung lokal![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/4-real-time-dashboard-application.png)

3.  Führen Sie die Anwendung **RealtimeDashboardApp.exe**
4.  Anmeldeinformationen Sie gültige Power BI melden Sie an, und klicken Sie auf **annehmen**
    
    ![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/5-sign-into-powerbi.png)
    
    ![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/6-powerbi-dashboard-permissions.png)


### <a name="configure-powerbi-reports"></a>PowerBI konfigurieren
Berichte in Echtzeit und das Dashboard dauern ca. 30-45 Minuten. Wechseln Sie zu [http://powerbi.com](http://powerbi.com) und melden.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/6-1-powerbi-signin.png)

Ein neues Dataset wird in Power BI generiert. Klicken Sie auf das **ConnectedCarsRealtime** -Dataset.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/7-select-connected-cars-realtime-dataset.png)

Speichern Sie leeren Bericht **STRG**+ s.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/8-save-blank-report.png)

Bieten Sie Berichtsname *Fahrzeug Telemetrie Analytics Echtzeit - Berichte*.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/9-provide-report-name.png)

## <a name="real-time-reports"></a>Berichte in Echtzeit
Diese Lösung umfasst drei Berichte:

1.  Fahrzeuge im Betrieb
2.  Wartenden Fahrzeuge
3.  Fahrzeuge Statistik

Sie können alle drei Echtzeit-Berichte konfigurieren oder nach jeder Stufe beenden und fahren mit dem nächsten Abschnitt der Batch-Berichte konfigurieren. Wir empfehlen die drei Berichte visualisieren die vollständige Einblicke in Echtzeit Pfad der Projektmappe erstellen.  

### <a name="1-vehicles-in-operation"></a>1. Fahrzeuge in Betrieb
  
Doppelklicken Sie auf **Seite 1** und auf"Arbeitsgang" Umbenennen  
    ![Verbundene Fahrzeuge - Fahrzeuge in Betrieb](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4a.png)  

Wählen Sie Feld **vin** aus **Feldern** und visualisierungstyp **"Karte"**.  

Karte Visualisierung wird wie in Abbildung erstellt.  
    ![Verbundene Fahrzeuge - Select vin](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4b.png)

Klicken Sie auf den leeren Bereich, um neue Visualisierung hinzufügen.  

Wählen Sie die **Stadt** und **FIN** aus. Visualisierung, **"Karte"**ändern. Ziehen Sie im Wertebereich **vin** . Ziehen Sie die **Stadt** aus auf **Legende** .   
    ![Auto - Karte Visualisierung verbunden](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4c.png)
  
**Visualisierung**Abschnitt **Format** auswählen und auf **Titel** den **Text** **"**Fahrzeuge in Betrieb Stadt" ändern.  
    ![Auto - Fahrzeuge in Betrieb Ort verbunden](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4d.png)   

Endgültige Visualisierung sieht aus wie in Abbildung dargestellt.    
    ![Verbundene Fahrzeuge - letzte Visualisierung](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4e.png)

Klicken Sie auf den leeren Bereich, um neue Visualisierung hinzufügen.  

Wählen Sie **Ort** und **FIN**, ändern Sie Visualisierung **gruppierten**Säulendiagramm. Sicherzustellen Sie **Ort **Achse** und **vin** im **Bereich** **  

Sortieren Sie Diagramm **"Anzahl der FIN"**  
    ![Verbundene Fahrzeuge - Anzahl vin](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4f.png)  

Ändern Sie **Diagrammtitel** **"** Fahrzeuge in Betrieb Stadt"  

Klicken Sie auf Abschnitt **Format** **Daten Farben**klicken Sie auf **"On"** auf **Alle anzeigen**  
    ![Verbundene Fahrzeuge - anzeigen alle Daten Farben](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4g.png)  

Ändern Sie die Farbe der einzelnen Stadt auf Symbol.  
    ![Verbundene Fahrzeuge - Farben ändern](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4h.png)  

Klicken Sie auf den leeren Bereich, um neue Visualisierung hinzufügen.  

Visualisierung **Gruppierten Säulendiagramm** Visualisierung auswählen, **Ort** **Achse** Bereich **Modell** **Legende** und **FIN** im **Bereich** ziehen.  
    ![Verbundene Fahrzeuge - Diagramms mit gruppierten Säulen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4i.png)  
    ![Verbundene Fahrzeuge - Rendering](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4j.png)
  
Ordnen Sie alle Visualisierung auf dieser Seite, wie in Abbildung.  
    ![Verbundene Fahrzeuge - Visualisierung](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4k.png)

"Fahrzeuge in Betrieb" Echtzeit Bericht wurde erfolgreich konfiguriert. Sie können fortfahren, zu den nächsten Echtzeit Bericht erstellen oder hier Dashboard konfigurieren. 

### <a name="2-vehicles-requiring-maintenance"></a>2. Fahrzeuge Wartung
  
Klicken Sie auf ![hinzufügen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4add.png) zum Hinzufügen eines neuen Berichts umbenennen **"Fahrzeuge erfordert Wartung"**

![Verbundene Fahrzeuge - Wartung Fahrzeuge](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4l.png)  

Wählen Sie Feld **vin** und ändern Sie Visualisierung **Karte**.  
    ![Verbundene Fahrzeuge - Vin Karte Visualisierung](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4m.png)  

Wir haben ein Feld namens "MaintenanceLabel" im Dataset. Dieses Feld kann den Wert "0" oder "1" enthalten." Azure Machine Learning-Modell im Rahmen der Lösung bereitgestellt und integriert Echtzeit Pfad liegt. Der Wert "1" zeigt an, dass ein Fahrzeug gewartet werden müssen. 

**Seite** Filter für Fahrzeuge Daten, die Wartung erfordern hinzu 

1. Ziehen Sie das Feld **"MaintenanceLabel"** in **Ebene Seitenfilter**.  
![Verbundene Fahrzeuge - Seite Filter](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n1.png)  

2. Klicken Sie auf Menü **Standardfilter** unten MaintenanceLabel Seitenfilter Ebene vorhanden.  
![Verbundene Fahrzeuge - Standardfilter](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n2.png)  

3.  Der Filterwert auf **"1"** gesetzt    
![Verbundene Fahrzeuge - Filterwert](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n3.png)  


Klicken Sie auf den leeren Bereich, um neue Visualisierung hinzufügen.  

Visualisierung **Gruppierten Säulendiagramm** auswählen  
![Verbundene Fahrzeuge - Vind Karte Visualisierung](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4o.png)  
![Verbundene Fahrzeuge - Diagramms mit gruppierten Säulen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4p.png)

Ziehen Sie Feld **Modell** **Achse** Bereich **Vin** zum **Bereich** . Sortieren Sie Visualisierung von **Anzahl vin**.  Ändern Sie **Diagrammtitel** **"** Fahrzeuge wartenden Modell"  

Ziehen Sie **vin** Felder in **Farbsättigungsgrad** an **Felder** ![Felder](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4field.png) Abschnitt der Registerkarte **Visualisierung**  
![Verbundene Fahrzeuge - Farbe-Sättigung](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4q.png)  

Ändern Sie **Daten** im **Format** Abschnitt Visualisierung  
Minimale Farbe ändern: **F2C812**  
Maximale Farbe ändern: **FF6300**  
![Auto - Farbe ändert verbunden](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4r.png)  
![Verbundene Fahrzeuge - Visualisierung Farben](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4s.png)  

Klicken Sie auf den leeren Bereich, um neue Visualisierung hinzufügen.  

Visualisierung **Clustered Säulendiagramm** auswählen, ziehen Sie Feld **vin** in **Bereich** **Ort** **Achse** Bereich ziehen. Diagramm **"Anzahl der FIN"**sortieren. Ändern Sie **Diagrammtitel** **"** Fahrzeuge wartenden Stadt"   
![Verbundene Fahrzeuge - Fahrzeuge wartenden Stadt](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4t.png)  

Klicken Sie auf den leeren Bereich, um neue Visualisierung hinzufügen.  

Visualisierung wählen Sie **Mehrerer Zeilen Karte** Visualisierung aus, ziehen Sie **Modell** und **FIN** im Bereich **Felder** .  
![Verbundene Fahrzeuge - mehrzeilige Karte](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4u.png)    

Neu anordnen aller Visualisierung, folgt endgültige Bericht wie:  
![Verbundene Fahrzeuge - mehrzeilige Karte](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4v.png)  

Echtzeit-Bericht "Fahrzeuge erfordert Wartung" wurde erfolgreich konfiguriert. Sie können fortfahren, zu den nächsten Echtzeit Bericht erstellen oder hier Dashboard konfigurieren. 

### <a name="3-vehicles-health-statistics"></a>3. Fahrzeuge Statistik
  
Klicken Sie auf ![hinzufügen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4add.png) zum Hinzufügen neuen Berichts umbenennen **"Fahrzeuge Health Statistics"**  

Visualisierung wählen Sie **der Monitor** Visualisierung aus und ziehen Sie das Feld **Geschwindigkeit** in **Wert Minimalwert, Maximalwert** Bereiche.  
![Verbundene Fahrzeuge - mehrzeilige Karte](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4w.png)  

Ändern Sie die Standardaggregation **Geschwindigkeit** im **Bereich** zum **durchschnittlichen** 

Ändern Sie die Standardaggregation **Geschwindigkeit** **mindestens** Bereich **minimalen**

Ändern Sie die Standardaggregation **Geschwindigkeit** im **Bereich maximale** **Maximum**

![Verbundene Fahrzeuge - mehrzeilige Karte](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4x.png)  

Benennen Sie den **Monitor Titel** in **"Average Speed"** 
 
![Verbundene Fahrzeuge - Monitor](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4y.png)  

Klicken Sie auf den leeren Bereich, um neue Visualisierung hinzufügen.  

Einen **Monitor** für **durchschnittliche Öl**, **durchschnittliche**und **durchschnittliche Modul gemäßigten**ebenso hinzufügen.  

Die Standardaggregation Felder in jeden Monitor nach oben **"Average Speed"** Schritte ändern beurteilen.

![Verbundene Fahrzeuge - Monitore](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4z.png)

Klicken Sie auf den leeren Bereich, um neue Visualisierung hinzufügen.

Wählen **und gruppierten Säulendiagramm** Visualisierung und **Gemeinsame Achse**ziehen **Geschwindigkeit** **Felder Reifendruck und Engineoil** in **Spaltenwerte** Bereich ziehen **Ort** ändern ihre Aggregation in **durchschnittliche**. 

Ziehen Sie das Feld **EngineTemperature** in **Zeile** Wertebereich, ändern Sie Aggregation, **durchschnittliche**. 

![Verbundene Fahrzeuge - Visualisierung Felder](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4aa.png)

Ändern Sie **den Diagrammtitel** **"Average Speed Reifendruck, Motoröl**und Motor".  

![Verbundene Fahrzeuge - Visualisierung Felder](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4bb.png)

Klicken Sie auf den leeren Bereich, um neue Visualisierung hinzufügen.

Visualisierung wählen Sie **Treemap** Visualisierung aus, ziehen Sie **das Feld** in **Bereich** und ziehen Sie das Feld **MaintenanceProbability** in **den Wertebereich** .

Ändern Sie **den Diagrammtitel** in **"Fahrzeugtypen wartenden"**.

![Verbundene Fahrzeuge - Titel ändern](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4cc.png)

Klicken Sie auf den leeren Bereich, um neue Visualisierung hinzufügen.

Visualisierung auswählen **100 % gestapeltes Balkendiagramm** , ziehen Sie das Feld **Ort** im Bereich **Achse** und **MaintenanceProbability** **RecallProbability** Felder in **den Wertebereich** .

![Verbundene Fahrzeuge - Hinzufügen neuer Visualisierung](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4dd.png)

Klicken Sie auf **Format**, **Daten**Farben und **MaintenanceProbability** Farbe auf den Wert **"F2C80F"**festgelegt.

Ändern Sie den **Titel** des Diagramms in **"Wahrscheinlichkeit des Fahrzeugs Wartung & zurückrufen nach Ort"**.

![Verbundene Fahrzeuge - Hinzufügen neuer Visualisierung](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ee.png)

Klicken Sie auf den leeren Bereich, um neue Visualisierung hinzufügen.

Visualisierung von Visualisierung wählen Sie **Flächendiagramm aus** , ziehen Sie **das Feld** in den Bereich **Achse** und ziehen Sie die Felder **EngineOil, Reifendruck, Geschwindigkeit und MaintenanceProbability** in **den Wertebereich** . Ändern Sie ihre Aggregationstyp in **"Durchschnitt"**. 

![Verbundene Fahrzeuge - Änderungstyp](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ff.png)

Ändern Sie den Titel des Diagramms **"durchschnittliche Öl**, Druck, Geschwindigkeit und Wartung Wahrscheinlichkeit Modell Reifen".

![Verbundene Fahrzeuge - Titel ändern](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4gg.png)

Klicken Sie auf den leeren Bereich, um neue Visualisierung hinzufügen:

1. Visualisierung **Punktdiagramm** Visualisierung auswählen.
2. Ziehen Sie **das Feld** in den **Details** und **Legende** .
3. Ziehen Sie das Feld **Kraftstoff** in den Bereich der **X-Achse** , ändern Sie Aggregation, **durchschnittliche**.
4. Ziehen Sie **EngineTemparature** in **Y-Achse Bereich**, ändern Sie die Aggregation in **durchschnittliche**
5. Ziehen Sie das Feld **vin** in **den Bereich** .


![Verbundene Fahrzeuge - Hinzufügen neuer Visualisierung](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4hh.png)

Ändern der **Titel** **"Durchschnittswerte Kraftstoff**, Engine Temperatur Modell".

![Verbundene Fahrzeuge - Titel ändern](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ii.png)

Der Abschlussbericht aussehen wie unten dargestellt.

![Verbundene Autos Abschlussbericht](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4jj.png)

### <a name="pin-visualizations-from-the-reports-to-the-real-time-dashboard"></a>PIN-Visualisierung aus den Berichten in Echtzeit Dashboard
  
Erstellt ein leeres Dashboard durch Klicken auf das Pluszeichen neben Dashboards. Sie können "Telemetrie Analytics Armaturenbrett" Namen

![Verbundene Fahrzeuge-Dashboard](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.5.png)

Fixieren Sie die Visualisierung der oben genannten Berichte zum Dashboard. 
 
![Verbundene Fahrzeuge-Dashboard](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.6.png)

Das Dashboard sollte wie folgt aussehen, wenn alle drei Berichte erstellt werden und entsprechende Visualisierung zum Dashboard fixiert. Wenn Sie nicht alle Berichte erstellt haben, kann Ihr Dashboard unterscheiden. 

![Verbundene Fahrzeuge-Dashboard](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-4.0.png)

Herzlichen Glückwunsch! Erstellt das Dashboard in Echtzeit Wie Sie CarEventGenerator.exe und RealtimeDashboardApp.exe, sollte live-Updates auf dem Dashboard angezeigt werden. Ca. 10 bis 15 Minuten dauert es die folgenden Schritte ausführen.

 
##  <a name="setup-power-bi-batch-processing-dashboard"></a>Power BI Batch-Verarbeitung Dashboard einrichten

>[AZURE.NOTE] Es dauert etwa zwei Stunden (nach erfolgreichem Abschluss der Bereitstellung) für die Pipeline Ende Stapelverarbeitung ausführen und ein Jahr Wert generierten Daten verarbeiten. So warten Sie vor dem Fortfahren mit den nächsten Schritten abgeschlossen. 

**Die PowerBI-Designer herunterladen**

-   Eine vorkonfigurierte PowerBI Designerdatei ist Bestandteil der Bereitstellung
-   Klicken Sie auf den Knoten PowerBI in der Diagrammansicht und klicken Sie auf **Download die Designerdatei PowerBI** im Eigenschaftenbereich![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/9.5-download-powerbi-designer.png)

-   Lokal speichern

**PowerBI konfigurieren**

-   Designer öffnen 'VehicleTelemetryAnalytics - Desktop Report.pbix PowerBI Desktop verwenden. Wenn Sie nicht bereits verfügen, installieren Sie PowerBI Desktop von [PowerBI Desktop installieren](http://www.microsoft.com/download/details.aspx?id=45331). 

-   Klicken Sie auf **Abfragen bearbeiten**.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/10-edit-powerbi-query.png)

- Doppelklicken Sie auf die **Quelle**.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/11-set-powerbi-source.png)

- Aktualisiert die Verbindungszeichenfolge mit SQL Azure-Server, der als Teil der Bereitstellung bereitgestellt haben. Klicken Sie auf SQL Azure-Knoten im Diagramm und zeigen Sie den Servernamen im Bereich Eigenschaften an.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/11.5-view-server-name.png)

- Behalten Sie *Connectedcar* **Datenbank** .

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/12-set-powerbi-database.png)

- Klicken Sie auf **OK**.
- **Windows-Anmeldeinformationen** Registerkarten durch standardmäßig auf der Registerkarte **Datenbank** rechts Datenbank **Anmeldeinformationen** ändern wird angezeigt.
- Geben Sie den **Benutzernamen** und das **Kennwort** der Datenbank SQL Azure, die während der Bereitstellung Setup angegeben wurde.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/13-provide-database-credentials.png)

- Klicken Sie auf **Verbinden**
- Wiederholen Sie die Schritte für jeden der drei übrigen Abfragen an rechten, und aktualisieren Sie die Details der Datenquelle Verbindung.
- Klicken Sie auf **Schließen und Laden**. Power BI Desktop Datei Datasets sind mit SQL Azure Tabellen verbunden.
- **Schließen** Power BI Desktop-Datei.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/14-close-powerbi-desktop.png)

- Klicken Sie auf **Speichern** , um die Änderung zu speichern. 
 
Sie haben jetzt die Batch-Verarbeitung Pfad in der Projektmappe entspricht Berichte konfiguriert. 


## <a name="upload-to-powerbicom"></a>*Powerbi.com* hochladen
 
1.  Navigieren Sie zu PowerBI Webportal Login zu http://powerbi.com.
2.  Klicken Sie auf **Daten**  
3.  Hochladen Sie Power BI Desktop-Datei.  
4.  Klicken Sie auf Hochladen, **Daten -> Dateien lokalen Datei ->**  
5.  Navigieren Sie zu **"VehicleTelemetryAnalytics – Desktop Report.pbix"**  
6.  Nachdem die Datei hochgeladen wurde, werden Sie an Ihrem Arbeitsplatz Power BI navigiert.  

Ein Dataset, Bericht und ein leeres Dashboard werden für Sie erstellt.  
 

PIN Diagramme vorhandenen Dashboards **Telemetrie Analytics Armaturenbrett** in **Power BI**. Klicken Sie auf die oben erstellte leere Dashboard und navigieren Sie dann zum klicken **Berichte** neu hochgeladenen Bericht.  

![Fahrzeug Telemetrie PowerBI.com](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard1.png) 


**Hinweis: der Bericht hat sechs Seiten:**  
Seite 1: Fahrzeugdichte  
Seite 2: Echtzeit-Fahrzeug Gesundheit  
Seite 3: Gesteuerte aggressiv Fahrzeuge   
Seite 4: Zurückgerufen Fahrzeuge  
Seite 5: Kraftstoff effizient gesteuerte Fahrzeuge  
Seite 6: Contoso-Logo  

![Verbundene Autos PowerBI.com](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard2.png)
 

**Seite 3**Pin Folgendes:  

1.  Anzahl der Fahrzeug-Identifizierungsnummer  
    ![Verbundene Autos PowerBI.com](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard3.png) 

2.  Aggressiv Modell wasserfalldiagramm angetrieben Fahrzeuge  
    ![Fahrzeug Telemetrie - Pin Diagramme 4](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard4.png)

**Von Seite 5**, Pin Folgendes: 
 
1.  Anzahl der Fahrzeug-Identifizierungsnummer    
    ![Fahrzeug Telemetrie - Pin Diagramme 5](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard5.png)  
2.  Kraftstoff Fahrzeuge Modell: gruppierte 3D-Säulen  
    ![Fahrzeug Telemetrie - Pin Diagramme 6](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard6.png)

**Von Seite 4**Pin Folgendes:  

1.  Anzahl der Fahrzeug-Identifizierungsnummer  
    ![Fahrzeug Telemetrie - Pin Diagramme 7](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard7.png) 

2.  Modell zurückgerufenen Fahrzeuge Stadt: Treemap  
    ![Fahrzeug Telemetrie - Pin Diagramme 8](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard8.png)  

**Von Seite 6**Pin Folgendes:  

1.  Contoso Motors logo  
    ![Fahrzeug Telemetrie - Pin Diagramme 9](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard9.png)

**Dashboard verwalten**  

1.  Navigieren Sie zum dashboard
2.  Jedes Diagramm und umbenennen, basierend auf der Benennung gemäß Abbildung umfassende, Mauszeiger. Auch verschieben Sie die Diagrammen unten Dashboard aussehen.  
    ![Fahrzeug Telemetrie - organisiert Schaltpult 2](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-organize-dashboard2.png)  
    ![Fahrzeug Telemetrie PowerBI.com](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard.png)
3.  Wenn Sie alle Berichte erstellt haben, wie in diesem Dokument erwähnt, sieht das letzte abgeschlossene Dashboard folgendermaßen. 

![Fahrzeug Telemetrie - organisiert Schaltpult 2](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-organize-dashboard3.png)

Herzlichen Glückwunsch! Sie haben erfolgreich die Berichte und Dashboards in Echtzeit, vorhersehbaren und batch Einblicke Fahrzeug Gesundheits-und fahren Gewohnheiten.  
