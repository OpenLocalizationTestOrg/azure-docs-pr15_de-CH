<properties 
    pageTitle="Cloud-Dienst überwachen | Microsoft Azure" 
    description="Informationen Sie zum Cloud-Diensten mit klassischen Azure-Portal überwachen." 
    services="cloud-services" 
    documentationCenter="" 
    authors="rboucher" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/04/2015" 
    ms.author="robb"/>


# <a name="how-to-monitor-cloud-services"></a>Cloud-Dienste überwachen

[AZURE.INCLUDE [disclaimer](../../includes/disclaimer.md)]

Überwachen Sie `key` Performance-Kennzahlen für die Clouddienste im klassischen Azure-Portal. Sie können die Filterebene festlegen auf minimale und ausführlich für jede Rolle Service und überwachen anzeigen können. Ausführliche Daten werden in ein Speicherkonto gespeichert, die Sie außerhalb des Portals zugreifen können. 

Überwachung im klassischen Azure-Portal angezeigt werden konfiguriert. Können die Metriken, die in der Liste Kriterien auf **der Seite** überwachen möchten, und welche Metriken Metriken Diagramme auf der Seite **Überwachen** und das Dashboard darstellen können. 

## <a name="concepts"></a>Konzepte

Standardmäßig wird die minimale Überwachung für einen neuen Clouddienst mit Leistungsindikatoren erfasst vom Host-Betriebssystem für Rolleninstanzen (virtuelle Maschinen) bereitgestellt. Die minimalen Metriken sind auf CPU-Prozentsatz, Daten, Daten, Datenträgerdurchsatz lesen und Schreiben Datenträgerdurchsatz. Ausführliche Überwachung konfigurieren, erhalten Sie zusätzliche Statistiken basierend auf Daten innerhalb virtueller Maschinen (Instanzen). Ausführlichen Kriterien ermöglichen näher Analyse der Probleme bei Anwendung.

Standardmäßig Leistungsindikatordaten aus Rolleninstanzen aufgenommen und übertragen die Rolleninstanz 3 Minuten. Beim Aktivieren der ausführlichen Überwachung die unformatierten Leistungsindikatordaten für jede Rolleninstanz und Instanzen für jede Rolle in Abständen von 5 Minuten, 1 Stunde und 12 Stunden aggregiert. Die aggregierten Daten werden nach 10 Tagen gelöscht.

Nach dem Aktivieren der ausführlichen Überwachung werden die aggregierten Überwachungsdaten in Tabellen in das Speicherkonto gespeichert. Um ausführliche Überwachung für eine Rolle zu aktivieren, müssen Sie einer Verbindungszeichenfolge Diagnose, verknüpft das Speicherkonto konfigurieren. Verschiedene Konten können für verschiedene Rollen.

Beachten Sie, dass eine ausführliche Überwachung ermöglichen die Speicherkosten zur Speicherung von Daten, Datenübertragung und Speichertransaktionen erhöhen. Minimale Überwachung erfordert kein Speicherkonto. Daten für Metriken, die minimale Überwachung Ebene ausgesetzt werden nicht in das Speicherkonto gespeichert, auch wenn die Überwachungsstufe auf ausführlich festgelegt.


## <a name="how-to-configure-monitoring-for-cloud-services"></a>Gewusst wie: Konfigurieren der Überwachung für Cloud-Services

Gehen Sie um ausführlich oder minimale Überwachung von Azure-Verwaltungsportal zu konfigurieren. 

### <a name="before-you-begin"></a>Bevor Sie beginnen

- Erstellen Sie ein Speicherkonto, um die Daten zu speichern. Verschiedene Konten können für verschiedene Rollen. Weitere Informationen finden Sie unter Hilfe für **Speicherkonten**oder finden Sie unter [So erstellen Sie ein Speicherkonto](/manage/services/storage/how-to-create-a-storage-account/).

- Aktivieren Sie Azure-Diagnose für die Cloud-Dienstverwaltungsrollen. Siehe [Diagnose für Cloud-Dienste konfigurieren](https://msdn.microsoft.com/library/azure/dn186185.aspx#BK_EnableBefore).

Sicherstellen Sie, dass die Diagnose in der Konfiguration vorhanden ist. Sie können ausführliche Überwachung bis Sie Azure-Diagnose aktivieren und eine Diagnose Verbindungszeichenfolge in die Konfiguration aktivieren.   

> [AZURE.NOTE] Projekte Azure SDK 2.5 nicht automatisch Diagnose-Verbindungszeichenfolge in der Projektvorlage enthalten. Projekte müssen Sie die Konfiguration der Verbindungszeichenfolge Diagnose manuell hinzufügen.

**Konfiguration der Diagnose Verbindungszeichenfolge manuell hinzufügen**

1. Cloud-Dienst-Projekt in Visual Studio öffnen
2. Klicken Sie doppelt auf die **Rolle** den Rolle Designer öffnen und wählen Sie **die Registerkarte**
3. Suchen Sie eine Einstellung mit dem Namen **Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString**. 
4. Wenn diese Einstellung nicht vorhanden ist Klicken Sie auf **Einstellung hinzufügen** , um die Konfiguration hinzu, und ändern Sie den Typ für die neue Einstellung in **ConnectionString**
5. Legen Sie den Wert für die Verbindungszeichenfolge der durch Klicken auf die Schaltfläche **...** . Es öffnet sich ein Dialogfeld, wodurch Sie ein Speicherkonto.

    ![Visual Studio Settings](./media/cloud-services-how-to-monitor/CloudServices_Monitor_VisualStudioDiagnosticsConnectionString.png)

### <a name="to-change-the-monitoring-level-to-verbose-or-minimal"></a>Um die Überwachung zu ausführlichen minimal ändern

1. Im [klassischen Azure-Portal](https://manage.windowsazure.com/)Öffnen der Seite für die Cloudbereitstellung-Dienst **Konfigurieren** .

2. Klicken Sie **auf** **ausführlich** oder **Minimal**. 

3. Klicken Sie auf **Speichern**.

Nachdem Sie ausführliche Überwachung aktivieren, sollten Sie beginnen, sehen die Überwachungsdaten in Azure-Verwaltungsportal innerhalb einer Stunde.

Die unformatierten Leistungsindikatordaten und aggregierte Überwachungsdaten werden Tabellen gekennzeichnet durch die Bereitstellung ID für die Rollen in der Speicher gespeichert. 

## <a name="how-to-receive-alerts-for-cloud-service-metrics"></a>Gewusst wie: Warnungen für Cloud Service Metriken

Basierend auf dem Cloud-Dienst Überwachung Metriken Alerts erhalten. Auf der Seite **Management Services** des klassischen Azure-Portal erstellen Sie eine Regel eine Warnung auslöst, wenn die Metrik gewählten Wert erreicht, den Sie angeben. Sie können auch e-Mail gesendet, wenn der Alarm ausgelöst wird. Weitere Informationen finden Sie unter [wie: Benachrichtigungen erhalten und Verwalten von Warnregeln in Azure](http://go.microsoft.com/fwlink/?LinkId=309356).

## <a name="how-to-add-metrics-to-the-metrics-table"></a>Gewusst wie: metriktabelle Kennzahlen hinzufügen

1. Öffnen Sie in [Azure-Verwaltungsportal](http://manage.windowsazure.com/)Seite **Monitor** für den Clouddienst.

    Standardmäßig zeigt die Tabelle Kennzahlen eine Teilmenge der verfügbaren Metriken. Die Abbildung zeigt standardmäßig ausführliche Metriken für einen Clouddienst auf Rollenebene zusammengesetzt Leistungsindikator Speicher\Verfügbare MB beschränkt ist. Verwenden Sie **Metriken hinzufügen** um zusätzliche aggregate und Rollenebene Statistiken im klassischen Azure-Portal überwachen auszuwählen.

    ![Ausführliche Anzeige](./media/cloud-services-how-to-monitor/CloudServices_DefaultVerboseDisplay.png)
 
2. Metriken metriktabelle hinzu

    1. Klicken Sie auf **Kriterien hinzufügen** **Wählen Sie Metriken**unten öffnen.

        Die erste verfügbare Metrik wird erweitert, um die verfügbaren Optionen anzuzeigen. Für jede Metrik zeigt die erste Option aggregierte Daten für alle Rollen. Darüber hinaus können Sie Rollen für die Datenanzeige.

        ![Kriterien hinzufügen](./media/cloud-services-how-to-monitor/CloudServices_AddMetrics.png)

    2. Messgrößen für die Anzeige auswählen

        - Die Metrik die Überwachungsoptionen erweitern klicken Sie auf den Pfeil nach unten.
        - Aktivieren Sie das Kontrollkästchen für jede Überwachung Option, die Sie anzeigen möchten.

        Sie können bis zu 50 Metriken Metriken Tabelle anzeigen.

        > [AZURE.TIP] Ausführliche Überwachung kann die Metriken Liste Dutzende von Metriken enthalten. Eine Bildlaufleiste, Mauszeiger über der rechten Seite des Dialogfelds angezeigt. Zum Filtern der Liste klicken Sie auf das Symbol und geben Sie Text in das Suchfeld ein, wie unten dargestellt.
    
        ![Metriken Suche hinzufügen](./media/cloud-services-how-to-monitor/CloudServices_AddMetrics_Search.png)


3. Kriterien ausgewählt haben, klicken Sie auf OK (Häkchen).

    Ausgewählten Metriken werden der metriktabelle wie folgt hinzugefügt.

    ![Monitor-Metriken](./media/cloud-services-how-to-monitor/CloudServices_Monitor_UpdatedMetrics.png)

 
4. Löschen eine Metrik aus metriktabelle auf Metrik, um es auszuwählen und dann auf **Löschen Metrik**. (Sie sehen nur **Metrik löschen** Wenn Sie eine Metrik ausgewählt haben.)

### <a name="to-add-custom-metrics-to-the-metrics-table"></a>Benutzerdefinierte Messgrößen Metriken Tabelle hinzufügen

**Verbose** Überwachungsstufe enthält eine Liste von standardmetriken, die Sie überwachen können im Portal. Darüber hinaus können Sie benutzerdefinierte Messgrößen oder von der Anwendung über das Portal definierten Leistungsindikatoren überwachen.

Die folgenden Schritte gehen, **ausführlich** Überwachungsstufe aktiviert haben und Sie Konfigurieren der Anwendung sammeln und benutzerdefinierte Leistungsindikatoren. 

Benutzerdefinierte Leistungsindikatoren im Portal anzeigen müssen Sie die Konfiguration im Bündel Steuerelementcontainer aktualisieren:
 
1. Öffnen Sie Bündel Steuerelementcontainer Blob in das Speicherkonto für die Diagnose. Visual Studio oder andere Speicher-Explorer können Sie dies tun.

    ![Server-Explorer von Visual Studio](./media/cloud-services-how-to-monitor/CloudServices_Monitor_VisualStudioBlobExplorer.png)

2. Navigieren Sie mit dem Muster **RoleName/DeploymentId/RoleInstance** die Konfiguration für die Rolleninstanz Blob-Pfad. 

    ![Speicher-Explorer von Visual Studio](./media/cloud-services-how-to-monitor/CloudServices_Monitor_VisualStudioStorage.png)
3. Herunterladen Sie die Konfigurationsdatei für die Rolleninstanz und aktualisieren Sie, um benutzerdefinierte Leistungsindikatoren enthalten. Fügen Sie z. B. *Bytes/s Schreiben des Datenträgers* für das *Laufwerk C* überwachen Folgendes unter **PerformanceCounters\Subscriptions** Knoten

    ```xml
    <PerformanceCounterConfiguration>
    <CounterSpecifier>\LogicalDisk(C:)\Disk Write Bytes/sec</CounterSpecifier>
    <SampleRateInSeconds>180</SampleRateInSeconds>
    </PerformanceCounterConfiguration>
    ```
4. Das Speichern und Laden der Konfigurationsdatei an demselben Speicherort wird die Datei im Blob.
5. Wechseln Sie zur ausführlichen Modus in Azure klassische Portalkonfiguration. Wenn Sie im ausführlichen Modus bereits müssen Sie minimale und zu ausführlichen umschalten.
6. Klicken Sie im Dialogfeld **Kriterien hinzufügen** wird jetzt der Leistungsindikator verfügbar sein. 

## <a name="how-to-customize-the-metrics-chart"></a>Gewusst wie: Anpassen des Diagramms Metriken

1. Wählen Sie in der Tabelle Kennzahlen bis zu 6 Metriken Metriken Diagramm geplottet aus. Um eine Metrik auszuwählen, klicken Sie auf das Kontrollkästchen auf der linken Seite. Um eine Metrik Metriken Diagramm zu entfernen, deaktivieren Sie das Kontrollkästchen in der metriktabelle.

    Metriken Metriken Tabelle auswählen, werden die Metriken Metriken Diagramm hinzugefügt. Auf eine schmale Anzeige enthält eine Dropdownliste **n weitere** metrische Köpfe, die die Anzeige passt.

 
2. Wählen Sie zum Wechseln zwischen der Anzeige relative Werte (Endwert für jede Metrik) und absolut (y-Achse angezeigt), relativen oder absoluten am oberen Rand des Diagramms.

    ![Relative oder Absolute](./media/cloud-services-how-to-monitor/CloudServices_Monitor_RelativeAbsolute.png)

3. Ändern des Zeitraums auswählen Metriken Diagramm zeigt 1 Stunde, 24 Stunden und 7 Tage am oberen Rand des Diagramms

    ![Monitor anzeigen Periode](./media/cloud-services-how-to-monitor/CloudServices_Monitor_DisplayPeriod.png)

    Diagramm Metriken Dashboard unterscheidet die Methode zum Zeichnen von Metriken. Steht eine Reihe von Metriken und Metriken hinzugefügt oder entfernt metrische wählen.

### <a name="to-customize-the-metrics-chart-on-the-dashboard"></a>Metriken Diagramms in das Dashboard anpassen

1. Öffnen Sie das Dashboard für den Clouddienst.

2. Hinzufügen oder Entfernen von Metriken aus dem Diagramm:

    - Um eine neue Metrik darzustellen, aktivieren Sie das Kontrollkästchen für die Metrik in den Headern Diagramm. Klicken Sie auf eine schmale Anzeige durch ** *n*??metrics** eine Metrik geplottet, die Diagrammfläche Header nicht unterstützt.

    - Deaktivieren Sie das Kontrollkästchen durch Header, um eine Metrik zu löschen, die im Diagramm dargestellt.

3. Wechseln Sie zwischen **relativen** und **absoluten** angezeigt.

4. Wählen Sie 1 Stunde, 24 Stunden und 7 Tage Daten angezeigt.

## <a name="how-to-access-verbose-monitoring-data-outside-the-azure-classic-portal"></a>Gewusst wie: Zugriff auf ausführliche Überwachungsdaten außerhalb der Azure-Verwaltungsportal

Ausführliche Daten werden in Tabellen in der Speicherkonten gespeichert, die Sie für jede Rolle angeben. Jede Bereitstellung Cloud-Dienst werden sechs Tabellen für die Rolle erstellt. Für jeden (5 Minuten, 1 Stunde und 12 Stunden), werden zwei Tabellen erstellt. Einer dieser Tabellen speichert Rollenebene Aggregationen. die Tabelle speichert Aggregationen für Instanzen. 

Die Tabellennamen haben Folgendes Format:

```
WAD*deploymentID*PT*aggregation_interval*[R|RI]Table
```

Wo:

- *DeploymentID* ist die Cloudbereitstellung-Dienst zugewiesene GUID

- *Aggregation_interval* = 5 M, 1 H oder 12 H

- Rolle auf Aggregationen = R

- Aggregationen für Rolleninstanzen = RI

Die folgenden Tabellen würde z. B. ausführliche Daten aggregiert alle 1 Stunden speichern:

```
WAD8b7c4233802442b494d0cc9eb9d8dd9fPT1HRTable (hourly aggregations for the role)

WAD8b7c4233802442b494d0cc9eb9d8dd9fPT1HRITable (hourly aggregations for role instances)
```
