<properties
    pageTitle="Instanzenzahl manuell oder automatisch skalieren | Microsoft Azure"
    description="Erfahren Sie, wie Ihre Azure-Dienste."
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

# <a name="scale-instance-count-manually-or-automatically"></a>Instanzenzahl manuell oder automatisch skalieren

In [Azure-Portal](https://portal.azure.com/)können Sie die Anzahl der Instanzen des Dienstes manuell einstellen oder Parameter können festgelegt, damit diese automatisch je nach Bedarf skalieren. Dies wird normalerweise als *Dezentrales Skalieren* oder *bei*bezeichnet.

Vor Skalierung Instanzenzahl, sollten Sie skalieren **Preisstufe** neben Instanzenzahl betroffen ist. Andere Tarifen können unterschiedliche Adern und Speicher, und so haben sie eine bessere Leistung für die gleiche Anzahl von Instanzen (das *Aufwärtsskalieren* oder *Skalierung nach unten*). Dieser Artikel behandelt insbesondere *Skalierung in* und *out*.

Im Portal skaliert werden können und Sie können auch die [REST-API](https://msdn.microsoft.com/library/azure/dn931953.aspx) oder [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights/) manuell oder automatisch skalieren.

> [AZURE.NOTE] Dieser Artikel beschreibt das Erstellen einer Einstellung Skalieren im Portal unter [http://portal.azure.com](http://portal.azure.com). Skalierungsgröße Einstellungen in diesem Portal nicht bearbeitet Verwaltungsportal ([http://manage.windowsazure.com](http://manage.windowsazure.com)).

## <a name="scaling-manually"></a>Manuelles skalieren

1. [Azure-Portal](https://portal.azure.com/)klicken Sie auf **Durchsuchen**und navigieren Sie zu der Ressource wie eine **App Service-Plan**skalieren möchten.

2. **Skalieren** Kachel **Operationen** wird zum Angeben der Skalierung: **aus** Wenn Sie manuell skalieren **auf** Wenn Sie durch eine oder mehrere Leistungsmetriken skalieren.
    ![Skalieren Kachel](./media/insights-how-to-scale/Insights_UsageLens.png)

3. Klicken auf die Kachel gelangen Sie zur **Skalierung** Blade. Oben auf der Skala Blade sehen Geschichte Autoscale Aktionen den Dienst Sie.  
    ![Skalierung blade](./media/insights-how-to-scale/Insights_ScaleBladeDayZero.png)

>[AZURE.NOTE] Nur Aktionen, die durch Skalieren durchgeführt werden in diesem Diagramm angezeigt. Wenn Sie die Instanzenzahl manuell anpassen, wird die Änderung in diesem Diagramm nicht berücksichtigt.

4. Sie können die Anzahl **Instanzen** mit Schieberegler manuell anpassen.
5. Klicken Sie auf den Befehl **Speichern** und Sie werden werden um die Anzahl der Instanzen gleich.

## <a name="scaling-based-on-a-pre-set-metric"></a>Skalierung basierend auf vordefinierten Metrik

Sie ggf. die Anzahl der Instanzen automatisch anhand einer Metrik wählen Sie gewünschte Metrik in der Dropdownliste **nach skalieren aus** Für eine **App Service-Plan** können Sie z. B. **CPU-**Prozentsatz skalieren.

1. Wenn eine Metrik auswählen, erhalten Sie eine Schiebereglers oder Textfelder ein, und geben Sie die Anzahl der Instanzen zwischen skalieren möchten:

    ![Blade mit CPU-Skalierung](./media/insights-how-to-scale/Insights_ScaleBladeCPU.png)

    Skalierungsgröße dauert nie Ihren Dienst unter oder über die Grenzen, die unabhängig von der Last festgelegt.

2. Anschließend wählen Sie den Zielbereich für die Metrik. Beispielsweise wählt **CPU-Prozentsatz**kann ein Ziel für die durchschnittliche CPU aller Instanzen in Ihrem Dienst festlegen. Dezentrales Skalieren geschieht, wenn die durchschnittliche CPU übersteigt die maximale definieren, ebenso eine Skala in erfolgt immer mindestens den durchschnittlichen CPU unterschreitet.

3. Klicken Sie auf den Befehl **Speichern** . Skalierungsgröße überprüfen alle paar Minuten dafür sind in der Instanz und die Metrik. Wenn Ihr Dienst zusätzlichen Datenverkehr empfängt, erhalten Sie weitere Instanzen, ohne etwas.

## <a name="scale-based-on-other-metrics"></a>Basierend auf andere skalieren

Sie können basierend auf Kennzahlen als Vorgaben skalieren, die in der Dropdownliste **Skalieren** angezeigt und können auch haben komplexe skalieren und in Regeln.

### <a name="adding-or-changing-a-rule"></a>Hinzufügen oder Ändern einer Regel

1. Wählen Sie **Zeitplan und Leistungsregeln** in der Dropdownliste **nach skalieren** : ![von Leistungsregeln](./media/insights-how-to-scale/Insights_PerformanceRules.png)

2. Wenn zuvor skalieren sehen auf eine Ansicht der genauen Regeln Sie die.

3. Skalieren basierend auf anderen metrischen auf Zeile **Regel hinzufügen** . Sie können auch klicken Sie auf den vorhandenen Zeilen aus der Metrik Ändern der Metrik zuvor durch skalieren möchten.
![Regel hinzufügen](./media/insights-how-to-scale/Insights_AddRule.png)

4. Jetzt Sie die Metrik auswählen müssen, um skaliert werden soll. Bei einer Metrik gibt es einige Punkte zu beachten:
    * Die *Ressource* , die Metrik stammt. Normalerweise wird dadurch die Ressource identisch sein, die Sie skalieren. Um die Tiefe einer Warteschlange Speicher vergrößert werden soll, ist die Ressource jedoch Warteschlange durch skalieren möchten.
    * Der *metrische Namen* .
    * Der *Zeitaggregation* Metrik. Dies ist die Daten kombinieren die *Dauer*.

5. Nach Auswahl der Metrik wählen Sie den Schwellenwert für die Metrik und den Operator. Beispielsweise könnte **größer als** **80 %**sagen.

6. Wählen Sie die Aktion, die Sie übernehmen möchten. Es gibt einige Arten von Aktionen:
    * Erhöhen oder verringern, indem Sie-dies wird hinzufügen oder Entfernen von **Wert von Instanzen, die Sie definieren**
    * Erhöhen oder verringern Sie den Prozent - Dies ändert die Instanzenzahl Prozent. Beispielsweise könnte man 25 im Feld **Wert** und hätten Sie derzeit 8 Instanzen, 2 hinzugefügt.
    * Erhöhen oder verringern - dadurch wird die Anzahl der Instanzen festgelegt, auf den **Wert** festlegen.

7. Schließlich können Sie abkühlen - wie lange diese Regel nach der vorherigen Aktion erneut skalieren warten soll.

8. Nach dem Konfigurieren der Regel drücken Sie **OK**.

9. Wenn Sie alle Regeln konfiguriert haben, werden soll, müssen Sie das **Speichern** des Befehls.

### <a name="scaling-with-multiple-steps"></a>Skalieren mit mehreren Schritten

Beispiele sind ziemlich einfach. Ggf. aggressiver Skalierung nach oben (oder unten) können Sie mehrere Skalierung Regeln für dieselbe Metrik selbst hinzufügen. Beispielsweise können Sie zwei Skalierung auf CPU-Prozentsatz Regeln:

1. Skalieren von 1 Instanz ist CPU-Prozentsatz 60 %
2. Skalieren von 3 Instanzen ist CPU-Prozentsatz über 85 %

![Mehrere Regeln zur Skalierung](./media/insights-how-to-scale/Insights_MultipleScaleRules.png)

Diese zusätzliche Regel überschreitet Ihre 85 % eine Aktion, erhalten zwei zusätzliche Instanzen nicht Sie.

## <a name="scale-based-on-a-schedule"></a>Skalierung basierend auf einem Zeitplan


Standardmäßig beim Erstellen einer Regel skalieren gilt sie immer. Sie sehen, wenn Sie auf das Profil klicken:

![Profil](./media/insights-how-to-scale/Insights_Profile.png)

Sie möchten jedoch haben aggressivere Skalierung während des Tages oder der Woche als am Wochenende. Ihr Dienst vollständig aus Arbeitszeit kann auch Herunterfahren.

1. Dazu auf das Profil, wählen Sie **Wiederholung** statt **,** und wählen Sie die Häufigkeit, mit der das Profil gelten soll.

2. Beispielsweise ein Profil haben, die die Woche gilt, in der Dropdownliste **Tage** deaktivieren Sie **Samstag** und **Sonntag**.

3. Um ein Profil, die tagsüber gilt, legen Sie **Startzeit** die Tageszeit zu beginnen.
    ![Standardeinstellung](./media/insights-how-to-scale/Insights_ProfileRecurrence.png)

4. Klicken Sie auf **OK**.

5. Anschließend müssen Sie das Profil hinzufügen zu anderen Zeiten anwenden. Klicken Sie auf die Zeile **Profil hinzufügen** .
    ![Arbeit](./media/insights-how-to-scale/Insights_ProfileOffWork.png)

6. Namen der neuen, zweiten Profil, Beispiel konnte rufen Sie es **frei**.

7. Wählen Sie **Wiederholung** erneut, und wählen Sie den Instanz Count Bereich während dieser Zeit.

8. Mit dem Standardprofil **Tage** auswählen soll dieses Profil zuweisen und die **Startzeit** während des Tages.

>[AZURE.NOTE] Skalieren verwendet Sommerzeit Spareinlagen Regeln für welche **Zeitzone** , die Sie auswählen. Bei der UTC-Offset die Basis Zeitzonenoffset nicht den Spareinlagen UTC Offset zeigt Sommerzeit.

9. Klicken Sie auf **OK**.

10. Nun müssen Sie alle Regeln hinzufügen der zweiten Profil anwenden möchten. Klicken Sie auf **Regel hinzufügen**und dann während des Standardprofils haben dieselbe Regel erstellt werden konnte.
    ![Fügen Sie Regel Aufgaben aus hinzu](./media/insights-how-to-scale/Insights_RuleOffWork.png)

11. Achten Sie darauf, dass zu erstellen einer Regel für dezentrales Skalieren in oder Profil die Anzahl der Instanzen nur wachsen (oder verringern).

12. Klicken Sie abschließend auf **Speichern**.

## <a name="next-steps"></a>Nächste Schritte

* [Dienstmetrik überwachen](insights-how-to-customize-monitoring.md) um sicherzustellen, dass der Dienst verfügbar ist und reagiert wird.
* Sammeln Sie detaillierte Hochfrequenz-Metriken für den Dienst [Überwachung und Diagnose](insights-how-to-use-diagnostics.md) .
* [Benachrichtigung erhalten](insights-receive-alert-notifications.md) , wenn operationelle Ereignisse geschehen oder Metriken cross-Schwellenwert.
* [Leistung der Anwendung überwachen](../application-insights/app-insights-azure-web-apps.md) möchten Sie wissen genau, wie Code in der Cloud ausgeführt wird.
* [Anzeigen von Ereignissen und Überwachungsprotokolle](insights-debugging-with-events.md) zu, die in Ihrem Dienst geschehen.
* [Monitor-Verfügbarkeit und Reaktionsfähigkeit der Webseite](../application-insights/app-insights-monitor-web-app-availability.md) Anwendung zum heraus, ob Ihre Seite finden ist.
