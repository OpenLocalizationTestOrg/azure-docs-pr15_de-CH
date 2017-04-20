<properties
    pageTitle="Überwachung aktivieren und Diagnose in Microsoft Azure | Microsoft Azure "
    description="Informationen Sie für Ressourcen in Azure Diagnostics einrichten."
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

# <a name="enable-monitoring-and-diagnostics"></a>Überwachung und Diagnose

In [Azure-Portal](https://portal.azure.com)können Sie umfangreiche, häufig, Überwachung und Diagnose zu Ressourcen konfigurieren. Die [REST-API](https://msdn.microsoft.com/library/azure/dn931932.aspx) oder [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights/) können auch programmgesteuert Diagnose konfigurieren.

Diagnose und Überwachung metrische Daten in Azure werden ein Speicherkonto Ihrer Wahl gespeichert. Dadurch werden alle Werkzeuge sollen die Daten aus dem Speicher-Explorer auf Power BI-Tools von Drittanbietern lesen.

## <a name="when-you-create-a-resource"></a>Wenn Sie eine Ressource erstellen

Die meisten Dienste können Diagnose aktivieren, wenn sie in [Azure-Portal](https://portal.azure.com)erstellen.

1. Rufen Sie **neu** , und wählen Sie die Ressource, der Sie interessieren.

2. **Optionale Konfiguration**auswählen
    ![Diagnose-blade](./media/insights-how-to-use-diagnostics/Insights_CreateTime.png)

3. Wählen Sie **Diagnostics**, und klicken Sie **auf**. Sie müssen das Speicherkonto auswählen, Diagnose gespeichert werden soll. Sie Zahlen normalen Datenraten für Speicher und Transaktionen Diagnose Storage-Konto senden.

4. Klicken Sie auf **OK** , und erstellen Sie die Ressource zu.

## <a name="change-settings-for-an-existing-resource"></a>Ändern Sie die Einstellung für eine vorhandene Ressource

Wenn eine Ressource bereits erstellt haben, und die diagnoseeinstellungen (die Sammlung von Daten, z. B. ändern) möchten, können Sie dieses Recht in der Azure-Portal tun.

1. Die Ressource und klicken Sie auf den Befehl **Eigenschaften** .

2. Wählen Sie **Diagnostics**aus.

3. **Diagnose** -Blade verfügt über alle Diagnostik und Überwachungsdaten Sammlung für diese Ressource. Einige Ressourcen können Sie **eine Aufbewahrungsrichtlinie für die Daten aus Ihrem Speicherkonto bereinigen** .
    ![Speicher-Diagnose](./media/insights-how-to-use-diagnostics/Insights_StorageDiagnostics.png)

4. Nachdem Sie Einstellungen ausgewählt haben, klicken Sie auf den Befehl **Speichern** . Es dauert ein wenig bei Überwachungsdaten angezeigt, wenn Sie zum ersten Mal aktivieren.

### <a name="categories-of-data-collection-for-virtual-machines"></a>Kategorien der Datensammlung für virtuelle Maschinen
Für virtuelle Computer werden alle Metriken und Protokolle in Intervallen von einer Minute aufgezeichnet werden, damit Sie immer die aktuellste Informationen über den Computer.

- **Grundlegende Kriterien** : Zustandsmetriken über Ihren PC wie Prozessor und Speicher
- **Netzwerk- und Metriken** : Metriken zu Netzwerkanschlüssen und Webdienste
- **.NET Metriken** : Metriken zu .NET und ASP.NET Applikationen auf dem virtuellen Computer
- **SQL-Metriken** : läuft Microsoft SQL Service die Leistungsmetriken
- **Windows-Ereignisprotokolle Anwendung** : Windows-Ereignissen an den Kanal Anwendung gesendet werden
- **Windows-Ereignisprotokolle System** : Windows-Ereignissen an den Kanal gesendet werden. Dazu gehören auch alle Ereignisse von [Microsoft Antimalware](http://go.microsoft.com/fwlink/?LinkID=404171&clcid=0x409).
- **Windows-Ereignisprotokolle Security** : an den Sicherheitskanal Windows-Ereignisse
- **Diagnoseprotokolle Infrastruktur** : über die Infrastruktur für Diagnose Protokollierung
- **IIS-Protokolle** : Protokolle über den IIS-Server

Beachten Sie, dass bestimmte Linux-Distributionen nicht unterstützt werden und Gast-Agent auf dem virtuellen Computer installiert sein muss.

## <a name="next-steps"></a>Nächste Schritte

* [Benachrichtigung erhalten](insights-receive-alert-notifications.md) , wenn operationelle Ereignisse geschehen oder Metriken cross-Schwellenwert.
* [Dienstmetrik überwachen](insights-how-to-customize-monitoring.md) um sicherzustellen, dass der Dienst verfügbar ist und reagiert wird.
* Um sicherzustellen, dass Ihr Dienst Maßstab bei Bedarf [Instanzenzahl automatisch skaliert](insights-how-to-scale.md) .
* [Leistung der Anwendung überwachen](../application-insights/app-insights-azure-web-apps.md) möchten Sie wissen genau, wie Code in der Cloud ausgeführt wird.
* [Anzeigen von Ereignissen und Überwachungsprotokolle](insights-debugging-with-events.md) zu, die in Ihrem Dienst geschehen.
* [Track-Dienststatus](insights-service-health.md) wann Azure Leistung Abbau- oder Service-Unterbrechung aufgetreten ist.
