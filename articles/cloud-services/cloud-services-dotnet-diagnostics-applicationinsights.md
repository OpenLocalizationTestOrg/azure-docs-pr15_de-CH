<properties
   pageTitle="Problembehandlung bei Cloud-Diensten mit Application Insights | Microsoft Azure"
   description="Informationen Sie zum Cloud-Dienst Problembehandlung mithilfe von Application Insights Daten aus Azure-Diagnose."
   services="cloud-services"
   documentationCenter=".net"
   authors="sbtron"
   manager="timlt"
   editor="tysonn" />
<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="12/15/2015"
   ms.author="saurabh" />


# <a name="troubleshoot-cloud-services-using-application-insights"></a>Problembehandlung bei Cloud-Diensten mit Anwendung Einblicke

[Azure SDK 2.8](https://azure.microsoft.com/downloads/) und Azure Diagnostics-Erweiterung können Sie 1.5 jetzt Ihre Azure-Diagnosedaten für den Cloud-Dienst an Application Insights senden. Verschiedene Protokolle von Azure Diagnostics Anwendungsprotokolle, Windows-Ereignisprotokolle einschließlich gesammelten, ETW protokolliert und Leistungsindikatoren können jetzt an Application Insights und Application Insights-Portal Benutzeroberfläche dargestellt. Bei Verwendung mit Application Insights SDK erhalten Sie jetzt Einblicke Metriken und Protokolle die Anwendung sowie die System- und Infrastruktur Daten von Azure-Diagnose aus.

## <a name="configure-azure-diagnostics-to-send-data-to-application-insights"></a>Konfigurieren von Azure Diagnostics Daten an Application Insights senden

Gehen Sie setup-Cloud-Dienstprojekt Azure-Diagnosedaten an Application Insights senden.

1) Im Visual Studio-Projektmappen-Explorer mit der rechten Maustaste auf eine Rolle, und wählen Sie **Eigenschaften** den Rolle Designer öffnen

![Projektmappen-Explorer Rolleneigenschaften][1]

2) Rolle Designer Abschnitt Diagnose Kontrollkästchen Sie das **Diagnosedaten an Application Insights** senden

![Rolle Designer Diagnosedaten an Application Insights senden][2]

3) Wählen Sie im Dialogfeld, das erscheint, die Anwendungsressource Einblicke, die Azure Diagnostics Daten senden möchten. Im Dialogfeld können Sie eine vorhandene Anwendung Einblicke Ressource aus Ihrem Abonnement oder Key Instrumentation eine Application Insights-Ressource manuell angeben. Haben Sie eine vorhandene Anwendung Einblicke Ressource können Sie auf Verknüpfung **Erstellen einer neuen Ressource** ein Browserfenster klassischen Azure-Portal öffnet können Sie Einblicke Anwendungsressource erstellen auf erstellen. Weitere Informationen zum Erstellen einer Anwendung Einblicke Ressource finden Sie unter [Erstellen einer neuen Application Insights-Ressource](../application-insights/app-insights-create-new-resource.md)

![Wählen Sie Einblicke Anwendungsressource][3]

4) Application Insights-Ressource hinzugefügt haben, wird der instrumentationsschlüssel für die Ressource als Einstellung ein Dienst mit dem Namen **APPINSIGHTS_INSTRUMENTATIONKEY**gespeichert. Ändern dieser Einstellung für jede Konfiguration oder Umgebung Service Configuration Drop eine andere Konfiguration auswählen nach unten und einen neue instrumentationsschlüssel für diese Konfiguration angeben.

![Wählen Sie Konfiguration][4]

Einstellung der **APPINSIGHTS_INSTRUMENTATIONKEY** wird von Visual Studio zum diagnoseerweiterung mit der entsprechenden Anwendung Einblicke Ressourceninformationen während der Veröffentlichung konfigurieren. Die Einstellung ist eine bequeme definieren unterschiedliche Instrumentation Schlüssel für verschiedene Dienstkonfigurationen. Visual Studio übersetzt diese Einstellung und beim Veröffentlichen in öffentlichen konfiguriert die Diagnose einfügen. Vereinfacht die diagnoseerweiterung mit PowerShell Konfigurieren der Ausgabe von Visual Studio auch umfasst öffentlichen Konfigurations-XML mit der entsprechenden Anwendung Einblicke Ausstattung enthalten. Die Öffentliche Dateien werden im Ordner "Extensions" erstellt und folgen dem Muster PaaSDiagnostics. <RoleName>. PubConfig.xml. PowerShell basiert Einsatz können dieses Muster jede Konfiguration einer Rolle zuordnen.

5) Ermöglicht das **Senden von Diagnosedaten an Application Insights** konfiguriert automatisch Azure Diagnostics aus, um alle Leistungsindikatoren und Ebene Fehlerprotokolle, die vom Agent Azure Diagnostics Anwendung Erkenntnisse gesammelt werden. Möchten Sie weiter zu konfigurieren, welche Daten an Application Insights gesendet werden, müssen Sie die Datei *diagnostics.wadcfgx* für jede Rolle manuell bearbeiten. Finden Sie unter [Konfigurieren der Azure-Diagnose zum Senden von Daten an Application Insights](../azure-diagnostics-configure-applicationinsights.md) Weitere Informationen über die Konfiguration manuell aktualisieren.

Nach Cloud-Dienst senden Azure Diagnosedaten an Anwendung wissen Sie es in Azure bereitstellen können, wie Sie normalerweise konfiguriert Azure Diagnostics Erweiterung sichergestellt ist. Finden Sie [einen Cloud-Dienst mit Visual Studio veröffentlicht](../vs-azure-tools-publishing-a-cloud-service.md).  

## <a name="viewing-azure-diagnostics-data-in-application-insights"></a>Anzeigen von Azure Diagnostics in Anwendung Einblicke
Azure Diagnose Telemetrie erscheint in der Anwendung Einblicke für den Clouddienst konfiguriert.

Ist wie die verschiedenen Azure Diagnostics Anwendung Einblicke Konzepte Typen zuordnen anmelden:  

-  Leistungsindikatoren werden als benutzerdefinierte Messgrößen in Application Insights angezeigt.
-  Windows-Ereignisprotokolle werden als Spuren und benutzerdefinierte Ereignisse in Application Insights angezeigt.
-  Anwendung anmeldet, ETW-Protokolle und Protokolle Diagnoseinfrastruktur werden als Spuren in Application Insights angezeigt.

Azure-Diagnosedaten in Application Insights anzeigen:

- Verwenden Sie [Metriken Explorer](../application-insights/app-insights-metrics-explorer.md) Leistungsindikatoren oder Anzahl verschiedener Windows-Ereignisprotokollereignisse visualisieren.

![Benutzerdefinierte Messgrößen im Metrik-Explorer][5]

- [Suchen](../application-insights/app-insights-diagnostic-search.md) mithilfe von durchsuchen die verschiedenen Ablaufverfolgungsprotokolle von Azure Diagnostics gesendet. Beispielsweise hat eine nicht behandelte Ausnahme in einer Rolle verursacht die Rolle zum Absturz gebracht und Wiederverwenden von Informationen in den *Windows-Ereignisprotokoll* *Anwendung* zeigen würden. Die Suchfunktion können Sie das Windows-Ereignisprotokoll auf Fehler und vollständige Stapelrahmen für die Ausnahme aktivieren Sie die Ursache des Problems zu finden.

![Suche Spuren][6]

## <a name="next-steps"></a>Nächste Schritte

- [Add Application Insights SDK zu Ihrem Clouddienst](../application-insights/app-insights-cloudservices.md) zum Senden von Daten zu Anfragen, Ausnahmen Abhängigkeiten und benutzerdefinierte Telemetrie aus der Anwendung. Kombiniert mit der Azure-Diagnose Daten eine vollständige Ansicht der Anwendung und System in Application Insight Ressource zu erhalten.  


<!--Image references-->
[1]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/solution-explorer-properties.png
[2]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/role-designer-sendtoappinsights.png
[3]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/select-appinsights-resource.png
[4]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/role-designer-appinsights-serviceconfig.png
[5]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/metrics-explorer-custom-metrics.png
[6]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/search-windowseventlog-error.png
