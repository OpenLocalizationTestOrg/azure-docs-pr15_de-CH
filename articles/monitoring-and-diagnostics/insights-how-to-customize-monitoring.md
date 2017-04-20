<properties
    pageTitle="Übersicht der Metrik in Microsoft Azure | Microsoft Azure"
    description="Informationen Sie zum monitoring Diagramme in Azure anpassen."
    authors="rboucher"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2015"
    ms.author="robb"/>

# <a name="overview-of-metrics-in-microsoft-azure"></a>Übersicht über Microsoft Azure Kennzahlen

Alle Azure Services verfolgen Sie wichtige Messwerte, mit denen Sie Status, Leistung, Verfügbarkeit und Nutzung der Dienste überwachen. Können diese Metriken in Azure-Portal und [REST-API](https://msdn.microsoft.com/library/azure/dn931930.aspx) oder [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights/) kann auch programmgesteuert auf den vollständigen Satz von Metriken verwenden.

Bei einigen Diensten müssen Sie Diagnose aktivieren, um alle Metriken anzuzeigen. Für andere virtuelle Computer erhalten Sie einen grundlegenden Satz von Metriken, jedoch erforderlich ermöglichen die vollständige Hochfrequenz-Metriken. [Überwachung und Diagnose](insights-how-to-use-diagnostics.md) Weitere anzeigen

## <a name="using-monitoring-charts"></a>Überwachung Diagramme verwenden

Stellen Sie die Metriken Sie Auswahl eines Zeitraums.

1. Klicken Sie in [Azure-Portal](https://portal.azure.com/) **Durchsuchen**und eine Ressource, die Sie überwachen möchten.

2. Der Abschnitt **Überwachung** enthält die wichtigsten Metriken für jede Azure Ressource. Web app hat beispielsweise **Anfragen und Fehler**, wobei als virtueller Computer **CPU-Prozentsatz** und **Datenträger lesen und Schreiben**müssten:  ![Überwachung objektiv](./media/insights-how-to-customize-monitoring/Insights_MonitoringChart.png)

3. Auf Diagramme zeigen Ihnen die **Metrik** Blade. Das Blade neben dem Diagramm ist eine Tabelle mit Sie Aggregationen Metriken (z. B. Durchschnitt, Mindest- und Höchstwerte für den Zeitraum fest, den Sie ausgewählt haben). Nachfolgend sind die Warnungsregeln für die Ressource.
    ![Blatt Metrik](./media/insights-how-to-customize-monitoring/Insights_MetricBlade.png)

4. Anpassen die Zeilen, die angezeigt werden, klicken Sie auf die Schaltfläche **Bearbeiten** auf das Diagramm oder den Befehl **Diagramm bearbeiten** auf Blatt Metrik.

5. Auf die Abfrage bearbeiten können Sie drei Dinge tun:
    - Ändern des Zeitraums
    - Wechseln die Darstellung zwischen Leiste und Linie
    - Wählen Sie verschiedene tikartikeln ![Abfrage bearbeiten](./media/insights-how-to-customize-monitoring/Insights_EditQuery.png)

6. Ändern des Zeitraums ist so einfach wie einen anderen Bereich (z. B. **Stunde**) auswählen und auf **Speichern** unten das Blade. Sie können auch **benutzerdefinierte**, die längere Zeit in den letzten 2 Wochen auswählen können. Beispielsweise sehen Sie die zwei Wochen und 1 Stunde von gestern. Geben Sie im Feld Geben Sie eine andere Stunde.
    ![Benutzerdefinierter Zeitraum](./media/insights-how-to-customize-monitoring/Insights_CustomTime.png)

7. Unter den Zeitraum Sie Kanal wählen Sie eine beliebige Anzahl von Kennzahlen für das Diagramm angezeigt.

8. Beim Klicken auf Speichern werden die Änderung für die jeweilige Ressource gespeichert. Beispielsweise wenn Sie zwei virtuelle Computer, und ein Diagramm auf einem ändern, wirkt sie nicht die andere.

## <a name="creating-side-by-side-charts"></a>Side-by-Side-Diagramme

Mit der leistungsstarken Anpassung im Portal können Sie so viele Diagramme wie gewünscht hinzufügen.

1. Klicken Sie im Menü **...** am Anfang des Blades **Kacheln hinzufügen**:  
    ![Menü hinzufügen](./media/insights-how-to-customize-monitoring/Insights_AddMenu.png)
2. Dann Sie können wählen ein Diagramm auf der rechten Seite des Bildschirms **Katalog** :  ![Galerie](./media/insights-how-to-customize-monitoring/Insights_Gallery.png)
3. Wenn gewünschte Metrik nicht angezeigt wird, können Sie immer eines der vordefinierten Messgrößen und **Bearbeiten** hinzufügen Diagramm die Metrik, die Sie benötigen.

## <a name="monitoring-usage-quotas"></a>Überwachen der verwendungskontingente

Die meisten Metriken zeigen Sie Trends mit der Zeit aber bestimmte Daten wie verwendungskontingente sind Point-in-Time-Informationen mit einem Schwellenwert.

Verwendungskontingente können auch auf die Ressourcen angezeigt werden, die Kontingente wurden:

![Verwendung](./media/insights-how-to-customize-monitoring/Insights_UsageChart.png)

Wie mit Metriken können [REST-API](https://msdn.microsoft.com/library/azure/dn931963.aspx) oder [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights/) Sie die vollständige verwendungskontingente programmgesteuert zugreifen.

## <a name="next-steps"></a>Nächste Schritte

* [Benachrichtigung erhalten](insights-receive-alert-notifications.md) , wenn eine Metrik einen Schwellenwert überschreitet.
* Sammeln Sie detaillierte Hochfrequenz-Metriken für den Dienst [Überwachung und Diagnose](insights-how-to-use-diagnostics.md) .
* Um sicherzustellen, dass Ihr Dienst verfügbar und reaktionsfähig ist [Instanzenzahl automatisch skaliert](insights-how-to-scale.md) .
* [Leistung der Anwendung überwachen](../application-insights/app-insights-azure-web-apps.md) möchten Sie wissen genau, wie Code in der Cloud ausgeführt wird.
* Verwenden Sie [Anwendung Einblicke für JavaScript-apps und Webseiten](../application-insights/app-insights-web-track-usage.md) zu Client-Analysen über die Browser, die Webseite.
* [Monitor-Verfügbarkeit und Reaktionsfähigkeit der Webseite](../application-insights/app-insights-monitor-web-app-availability.md) Anwendung zum heraus, ob Ihre Seite finden ist.
