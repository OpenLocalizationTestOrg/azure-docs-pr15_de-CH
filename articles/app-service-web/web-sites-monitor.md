<properties
    pageTitle="Monitor-Apps in Azure App Service"
    description="Erfahren Sie, wie Apps in Azure App Service mit Azure-Portal überwachen."
    services="app-service"
    documentationCenter=""
    authors="btardif"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/07/2016"
    ms.author="byvinyal"/>

# <a name="how-to-monitor-apps-in-azure-app-service"></a>Gewusst wie: Überwachen von Apps in Azure App Service

[App Service](http://go.microsoft.com/fwlink/?LinkId=529714) bietet integrierte Überwachungsfunktionalität in [Azure-Portal](https://portal.azure.com).
Dies schließt die Möglichkeit **Kontingente** und **Metriken** für eine Anwendung sowie die App Service, **Alarme** und sogar **Skalierung** basierend auf diesen Kriterien automatisch einrichten.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="understanding-quotas-and-metrics"></a>Grundlegendes zu Kontingenten und Metriken

### <a name="quotas"></a>Kontingente

In App-Dienst gehostet sind innerhalb bestimmter *Grenzen* der Ressourcen, die sie verwenden können. Die Grenzwerte werden mit der Anwendung verknüpfte **App Service-Plan** definiert.

Wenn die Anwendung in einem Plan **frei** oder **Shared** gehostet wird, werden die Grenzwerte für die Ressourcen, die der Anwendung können, durch **Kontingente**definiert.

Wenn die Anwendung in **Basic**, **Standard** oder **Premium** -Plan und die Grenzwerte gehostet wird Ressourcen können **Größe** (klein, Mittel, groß) und **Anzahl der Instanzen** (1, 2, 3,...) des **App-Serviceplan**setzen.

**Kontingente** für **frei** oder **Shared** apps sind:

* **CPU(short)**
   * Betrag der CPU für diese Anwendung in 3 Minuten zulässig. Dieses Kontingent wird alle 3 Minuten.
* **CPU(Day)**
   * Gesamtbetrag der CPU für diese Anwendung an einem Tag zulässig. Dieses Kontingent wird täglich um Mitternacht UTC.
* **Speicher**
   * Gesamtumfang des Arbeitsspeichers für diese Anwendung zulässig.
* **Bandbreite**
   * Gesamtbetrag der ausgehende Bandbreite für diese Anwendung an einem Tag.
   Dieses Kontingent wird täglich um Mitternacht UTC.
* **Dateisystem**
   * Speicher zulässig.

Nur Kontingent für apps auf **einfache**, **Standard-** und **Premium** -Pläne gehostet ist **Dateisystem**.

Weitere Informationen zu bestimmten Quoten, Grenzen und Funktionen der verschiedenen App Service-SKUs finden Sie hier: [Azure Abonnement Service Grenzen](../azure-subscription-service-limits.md#app-service-limits)

#### <a name="quota-enforcement"></a>Kontingent erzwingen

Wenn eine Anwendung die Verwendung der **CPU (short)**überschreitet, werden **CPU (Tag)**oder **Bandbreite** Kontingent dann Anwendung beendet bis das Kontingent wird. Während dieser Zeit führt alle eingehende Anfragen einen **HTTP 403**.
![][http403]

Wenn die Anwendung **Speicher** überschritten wird, wird die Anwendung neu gestartet werden.

Wenn das **Dateisystem** -Kontingent überschritten wird, schreiben eine Operation fehl, einschließlich Protokolle schreiben.

Kontingente können erhöht oder Ihre app durch Erweiterung Ihres App entfernt werden.

### <a name="metrics"></a>Metriken

**Metriken** Informationen der app oder App Service-Plan Problem.

Für eine **Anwendung**werden die verfügbaren Metriken:

* **Durchschnittliche Antwortzeit**
   * Die durchschnittliche Verarbeitungszeit für die app zu Anfragen in Millisekunden.
* **Durchschnittliche Arbeitsspeicher Arbeitssatz**
   * Die durchschnittliche Speichergröße MiBs von der Anwendung verwendet.
* **CPU-Zeit**
   * Der Betrag der CPU in Sekunden von der Anwendung verwendet. Weitere Informationen über diese Metrik finden Sie unter: [CPU-Zeit Vs CPU-Prozentsatz](#cpu-time-vs-cpu-percentage)
* **Daten In**
   * Der Betrag der eingehende Bandbreite von der app in MiBs.
* **Daten**
   * Der Betrag der ausgehende Bandbreite von der app in MiBs.
* **HTTP-2xx**
   * Anzahl der Anfragen, was einen HTTP-Statuscode > = 200 aber < 300.
* **HTTP-3xx**
   * Anzahl der Anfragen, was einen HTTP-Statuscode > = 300 aber < 400.
* **HTTP-Fehler 401**
   * Anzahl der Anfragen, was HTTP 401-Statuscode.
* **HTTP 403**
   * Anzahl der Anfragen, was HTTP 403-Statuscode.
* **HTTP 404**
   * Anzahl der Anfragen, was HTTP 404-Statuscode.
* **HTTP 406**
   * Anzahl der Anfragen, was Statuscode HTTP 406.
* **HTTP-4xx**
   * Anzahl der Anfragen, was einen HTTP-Statuscode > = 400 jedoch < 500.
* **HTTP-Server-Fehler**
   * Anzahl der Anfragen, was einen HTTP-Statuscode > = 500 aber < 600.
* **Arbeit**
   * Aktuelle Speichergröße von app im MiBs verwendet.
* **Anfragen**
   * Die Gesamtzahl der Anfragen unabhängig von deren resultierende HTTP-Statuscode.

Eine **App Service-Plan**sind die verfügbaren Metriken:

>[AZURE.NOTE] App Dienstmetrik Plan stehen nur für Pläne in **Basic**, **Standard** und **Premium** SKU.

* **CPU-Prozentsatz**
   * Die durchschnittliche CPU-Verwendung für alle Instanzen des Plans.
* **Speicher in Prozent**
   * Der durchschnittliche Speicher für alle Instanzen des Plans verwendet.
* **Daten In**
   * Die durchschnittliche eingehende Bandbreite für alle Instanzen des Plans.
* **Daten**
   * Die durchschnittliche ausgehende Bandbreite für alle Instanzen des Plans.
* **Warteschlangenlänge des Datenträgers**
   * Die durchschnittliche Anzahl der sowohl Lese- und Schreibanfragen, die in die Warteschlange gestellt wurden im Speicher. Eine hohe Warteschlangenlänge ist ein Anzeichen einer Anwendung durch übermäßige Festplatte verlangsamt werden kann.
* **HTTP-Länge**
   * Die durchschnittliche Anzahl von HTTP-Anfragen, die sich in der Warteschlange vor erfüllt. Eine hohe oder zunehmende HTTP-Warteschlangenlänge ist ein Symptom eines Plans stark ausgelastet.

### <a name="cpu-time-vs-cpu-percentage"></a>CPU-Zeit Vs CPU-Prozentsatz
<!-- To do: Fix Anchor (#CPU-time-vs.-CPU-percentage) -->

Es gibt 2 Metriken, die CPU-Auslastung widerspiegeln. **CPU-Zeit** und **CPU-Prozentsatz**

**CPU-Zeit** ist nützlich für apps gehostet **frei** oder **gemeinsame** Pläne seit ihrer Quoten von der app verwendeten CPU Minuten definiert.

**CPU-Prozentsatz** ist andererseits für apps können Sie skaliert werden und diese Metrik ist ein guter Indikator für die allgemeine Verwendung für alle Instanzen in **basic**, **standard** und **Premium** gehostet.

##<a name="metrics-granularity-and-retention-policy"></a>Metriken Granularität und Aufbewahrungsrichtlinien

Metriken für eine Anwendung und app-Serviceplan protokolliert und vom Dienst mit folgenden Granularität der Aufbewahrungsrichtlinien zusammengefasst:

 * **Minute** Granularität Metriken sind **48** Stunden beibehalten.
 * **Stunde** Granularität Metriken sind **30** Tage beibehalten.
 * **Tag** Granularität Metriken sind **90** Tage lang aufbewahrt

## <a name="monitoring-quotas-and-metrics-in-the-azure-portal"></a>Kontingente und Metriken in Azure-Portal.

Überprüfen Sie den Status der verschiedenen **Kontingente** und **Metriken** einer Anwendung in [Azure-Portal](https://portal.azure.com).

![][quotas]
**Kontingente** finden Sie unter Settings >**Kontingente**. Bedienen können Sie: (1) die Kontingente Name (2) die Resetintervall, (3) die aktuelle Begrenzung und (4) aktuelle Wert.

![][metrics]
**Metriken** können direkt aus dem Blade Ressource werden. Sie können auch das Diagramm: (1) **Klicken Sie auf** und wählen (2) **Diagramm bearbeiten**.
Hier können Sie den **Zeitraum**(3), (4) **Diagrammtyp**und **Metriken** anzeigen (5) ändern.  

Sie erfahren hier Metriken: [Monitor Dienstmetrik](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md).

## <a name="alerts-and-autoscale"></a>Alarme und skalieren
Metriken für ein App oder App Service-Plan bis angeschlossen werden hinweist, Weitere Informationen, siehe [Benachrichtigungen erhalten](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)

Basic, standard oder Premium App Service-Pläne gehosteten App Service apps unterstützen **Skalieren**. Dadurch können Sie Regeln konfigurieren, die App Service-Plan Metriken überwachen und erhöhen oder verringern die Instanzenzahl bietet zusätzliche Ressourcen je nach Bedarf oder Geld sparen bei die Anwendung über ist. Sie erfahren hier automatisch skalieren: [wie Skalierung](../monitoring-and-diagnostics/insights-how-to-scale.md) und hier [Best Practices für die automatische Skalierung Azure-Monitor](../monitoring-and-diagnostics/insights-autoscale-best-practices.md)

>[AZURE.NOTE] Wenn Sie mit Azure App Service beginnen, bevor Sie sich für ein Azure-Konto, gehen Sie [Versuchen App Service](http://go.microsoft.com/fwlink/?LinkId=523751)sofort eine kurzlebige Starter Web app in App Service können Sie erstellen. Keine Kreditkarten erforderlich; keine Zusagen.

## <a name="whats-changed"></a>Was hat sich geändert
* Eine Anleitung zur Änderung von Websites zu App Service finden Sie unter: [Azure App Service und seine Auswirkung auf vorhandene Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)

[fzilla]:http://go.microsoft.com/fwlink/?LinkId=247914
[vmsizes]:http://go.microsoft.com/fwlink/?LinkID=309169



<!-- Images. -->
[http403]: ./media/web-sites-monitor/http403.png
[quotas]: ./media/web-sites-monitor/quotas.png
[metrics]: ./media/web-sites-monitor/metrics.png
