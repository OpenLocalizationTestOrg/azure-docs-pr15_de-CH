<properties
    pageTitle="Wählen zwischen Fluss, Logik Apps, Funktionen und Webaufträge | Microsoft Azure"
    description="Vergleichen der Cloud Integration services von Microsoft zu entscheiden, welche Dienste Sie verwenden."
    services="functions,app-service\logic"
    documentationCenter="na"
    authors="cephalin"
    manager="wpickett"
    tags=""
    keywords="Microsoft fortlaufenden Datenfluss Logik apps, Azure Funktionen Funktionen Azure Webaufträge, Webaufträge, Verarbeitung, dynamische Compute serverlose Architektur"/>

<tags
    ms.service="functions"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="09/08/2016"
    ms.author="chrande; glenga"/>

# <a name="choose-between-flow-logic-apps-functions-and-webjobs"></a>Wählen Sie zwischen Fluss, Logik Apps, Funktionen und Webaufträge

Dieser Artikel vergleicht und die folgenden Dienste in der Cloud von Microsoft, die alle Probleme der Integration und Automatisierung von Geschäftsprozessen lösen können gegenübergestellt:

- [Microsoft-Fluss](https://flow.microsoft.com/)
- [Azure Logik Apps](https://azure.microsoft.com/services/logic-apps/)
- [Azure-Funktionen](https://azure.microsoft.com/services/functions/)
- [Azure App Service Webaufträge](../app-service-web/web-sites-create-web-jobs.md)

Diese Dienste eignen sich "zusammen heterogener Systeme Kleben". Sie können alle Eingaben, Aktionen, Zustände und Ausgabe. Sie können jeweils auf einen Zeitplan oder Trigger ausführen. Jedoch jeden Dienst fügt einen eindeutigen Satz von Wert und Vergleich ist nicht die Frage "welcher Dienst das beste?" sondern "der Dienst ist geeignet für diese Situation?" Häufig ist eine Kombination dieser Dienste am besten skalierbare, vollständigen Funktionsumfang integrationslösung schnell erstellen.

<a name="flow"></a>
## <a name="flow-vs-logic-apps"></a>Im Vergleich zu Ablauflogik Apps

Wir können Microsoft Flow und Azure Logik Apps diskutieren sind beide *erste Konfiguration* Integrationsservices Prozesse und Workflows und Integration mit verschiedenen SaaS und Enterprise-Anwendung erleichtert. 

- Flow basiert auf Logik Apps
- Sie haben diesen Workflowdesigner
- [Connectors](../connectors/apis-list.md) , die in einem auch in anderen arbeiten können

Flows können alle Büroangestellter einfache Integration ausführen (z. B. SMS für wichtige e-Mails abrufen) ohne Entwickler oder IT. Auf der anderen Seite können Logik Apps erweiterte oder wichtigen Integrationen (z.B. B2B-Prozesse), wo Methoden DevOps und Sicherheit auf Unternehmensebene erforderlich sind. Es ist ein Geschäftsworkflow Überstunden Komplexität zunehmen. Daher können mit zuerst starten Sie Bedarf Logik App konvertieren.

In der folgenden Tabelle können Sie bestimmen, ob Fluss oder Logik Apps für eine bestimmte Integration.

|               | Fluss                                                                             | Logik-Apps                                                                                          |
|---------------|----------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| Zielgruppe      | Büroangestellte, Unternehmen                                                   | IT-Experten, Entwickler                                                                                 |
| Szenarien     | Self-Service                                                                     | Wichtiger                                                                                    |
| Design-Tool   | In-Browser-Benutzeroberfläche nur                                                              | Im Browser und [Visual Studio](../app-service/logic/app-service-logic-deploy-from-vs.md) [Code anzeigen](../app-service-logic/app-service-logic-author-definitions.md) |
| DevOps        | Ad-hoc-Produktion entwickeln                                                    | Datenquellen-Steuerelement testen, Unterstützung und Automatisierung und Verwaltung in [Azure Ressourcenmanagement](../app-service-logic/app-service-logic-arm-provision.md)|
| Admin-Erfahrung| [https://Flow.Microsoft.com](https://flow.microsoft.com)                       | [https://Portal.Azure.com](https://portal.azure.com)                                                |
| Sicherheit      | Standard-Verfahren: [datensouveränität](https://wikipedia.org/wiki/Technological_Sovereignty), [Verschlüsselung ruhender](https://wikipedia.org/wiki/Data_at_rest#Encryption) Daten usw.. | Sicherheit von Azure: [Azure Security](https://www.microsoft.com/trustcenter/Security/AzureSecurity), [Security Center](https://azure.microsoft.com/services/security-center/) [Überwachungsprotokolle](https://azure.microsoft.com/blog/azure-audit-logs-ux-refresh/)und mehr. |

<a name="function"></a>
## <a name="functions-vs-webjobs"></a>Funktionen im Vergleich zu Webaufträge

Wir können Azure Funktionen und Azure App Service Webaufträge zusammen sind beide *Code First* Integrationsservices und für Entwickler vorgesehen. Sie können ein Skript oder Code als Antwort auf verschiedene Ereignisse [ein WebHook anfordern](functions-bindings-http-webhook.md)oder [Neue Speicher-Blobs](functions-bindings-storage.md) ausgeführt. Hier werden ihre vergleichbar: 

- Beide basieren auf [Azure App Service](../app-service/app-service-value-prop-what-is.md) und Funktionen wie [Versionskontrolle](../app-service-web/app-service-continuous-deployment.md), [Authentifizierung](../app-service/app-service-authentication-overview.md)und [Überwachung](../app-service-web/web-sites-monitor.md).
- Beide sind Entwickler Services.
- Sowohl standard scripting und Programmiersprachen unterstützt.
- Beide verfügen über NuGet und NPM unterstützen.

Funktionen ist die Weiterentwicklung der Webaufträge die besten Dinge über Webaufträge und verbessert werden. Verbesserungen: 

- Optimierte Dev testen und Ausführen des Codes direkt im Browser.
- Die Integration mit mehr Azure Services und 3. Dienste wie [GitHub WebHooks](https://developer.github.com/webhooks/creating/).
- Pay-per-Use, eine [App Service planen](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)bezahlen müssen.
- Automatische, [dynamische Skalierung](functions-scale.md).
- Bestehende Kunden von App Service auf App Service planen können (ausgelastete Ressourcen nutzen zu können).
- Integration mit Logik-Apps.

In der folgenden Tabelle werden die Unterschiede zwischen Funktionen und Webaufträge zusammengefasst:

|                        | Funktionen                                                                                                                                                                | Webaufträge                            |
|------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------|
| Skalierung                | Configurationless skalieren                                                                                                                                                | Skalieren Sie mit App Service-plan        |
| Preisgestaltung                | Pay-per-Use oder Teil der App Service-plan                                                                                                                                  | Teil der App Service-plan           |
| Ausführen-Typ               | ausgelöst, geplant (Timer Trigger)                                                                                                                                  | ausgelöst, geplante kontinuierliches   |
| Trigger-Ereignisse         | [Zeitgeber](functions-bindings-timer.md), [Azure DocumentDB](functions-bindings-documentdb.md) [Azure Ereignis Hubs](functions-bindings-event-hubs) [HTTP/WebHook (GitHub, Pufferzeit)](functions-bindings-http-webhook.md), [Azure App Service Mobile Apps](functions-bindings-mobile-apps.md), [Azure Notification Hubs](functions-bindings-notification-hubs.md), [Azure Service Bus](functions-bindings-service-bus.md), [Azure-Speicher](articles/functions-bindings-storage.md) | [Azure-Speicher](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md), [Azure Servicebus](websites-dotnet-webjobs-sdk-service-bus.md)         |
| Browser-Entwicklung | x                                                                                                                                                                        |                                    |
| Windows scripting       | experimentelle                                                                                                                                                             | x                                  |
| PowerShell             | experimentelle                                                                                                                                                             | x                                  |
| C#                     | x                                                                                                                                                                        | x                                  |
| F#                     | x                                                                                                                                                                        |                                    |
| Bash                   | experimentelle                                                                                                                                                             | x                                  |
| PHP                    | experimentelle                                                                                                                                                             | x                                  |
| Python                 | experimentelle                                                                                                                                                             | x                                  |
| JavaScript             | x                                                                                                                                                                        | x                                  |
| Java                   | experimentelle                                                                                                                                                             | x                                  |

Funktionen oder Webaufträge hängt von letztendlich Sie bereits mit App tun. Wenn eine App Service Apps Codeausschnitte ausgeführt werden soll und Sie sie zusammen in der gleichen DevOps Umgebung verwalten möchten, sollten Sie Webaufträge verwenden. Wenn auszuführenden Codeausschnitte für andere Azure Services oder sogar 3rd Party apps Codeausschnitte Integration unabhängig von Ihrer App Service apps verwalten möchten oder Codeausschnitte Logik App aufgerufen, sollten Sie die verbesserte Funktionen nutzen.  

<a name="together"></a>
## <a name="flow-logic-apps-and-functions-together"></a>Zusammenfließen Sie, Logik-Apps und Funktionen

Wie bereits erwähnt welcher Dienst für Sie am besten geeignet ist der jeweiligen Situation abhängig. 

- Optimierung der einfachen dann verwenden.
- Wenn Ihr Integrationsszenario auch für Flow erweitert, oder DevOps Funktionen und Sicherheit Verstöße, verwenden Sie Logik Apps.
- Wenn ein Schritt in Ihrem Integrationsszenario sehr benutzerdefinierte Transformation oder speziellen Code erfordert, Schreiben Sie eine Funktion app und Auslösen einer Funktion als Aktion in Ihrer Anwendung Logik.

Sie können eine Anwendung Logik in einem aufrufen. Sie können auch eine Funktion in einer Anwendung Logik und eine Logik-app in einer Funktion aufrufen. Integration von Flow, Logik-Apps und Funktionen weiterhin Überstunden verbessern. Sie können etwas Service und in andere Dienste verwenden. Daher lohnt sich Investitionen in diese drei Technologien machen.

## <a name="next-steps"></a>Nächste Schritte

Erste Schritte mit der Dienste erstellen Ihre erste Fluss Logik app, Funktion app oder Webauftrag. Klicken Sie auf die folgenden Links:

- [Erste Schritte mit Microsoft Flow](https://flow.microsoft.com/en-us/documentation/getting-started/)
- [Erstellen einer Logik](../app-service-logic/app-service-logic-create-a-logic-app.md)
- [Erstellen Sie Ihre erste Azure-Funktion](../azure-functions/functions-create-first-azure-function.md)
- [Bereitstellen Sie Webaufträge, die mit Visual Studio](../app-service-web/websites-dotnet-deploy-webjobs.md)

Oder Weitere Informationen zu dieser Integration Services über die folgenden Links:

- [Nutzung der Azure-Funktionen und Azure App Service für Integrationsszenarios Christopher Anderson](http://www.biztalk360.com/integrate-2016-resources/leveraging-azure-functions-azure-app-service-integration-scenarios/)
- [Integration von Charles Lamanna leicht gemacht](http://www.biztalk360.com/integrate-2016-resources/integrations-made-simple/)
- [Logik-Apps Live Webcast](http://aka.ms/logicappslive)
- [Microsoft fließen häufig gestellte Fragen](https://flow.microsoft.com/documentation/frequently-asked-questions/)
- [Azure Webaufträge Dokumentationsressourcen](../app-service-web/websites-webjobs-resources.md)
