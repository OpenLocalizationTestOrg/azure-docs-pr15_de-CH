<properties 
    pageTitle="Erste Schritte mit Anwendung Einblicke mit Java in Eclipse" 
    description="Verwenden Sie die Eclipse plug-in Performance und Nutzung Ihrer Website Java-Anwendung zum Überwachen" 
    services="application-insights" 
    documentationCenter="java"
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="03/02/2016" 
    ms.author="awills"/>
 
# <a name="get-started-with-application-insights-with-java-in-eclipse"></a>Erste Schritte mit Anwendung Einblicke mit Java in Eclipse

Application Insights SDK sendet Telemetrie aus Ihrer Anwendung Java Web, Auslastung und Performance analysieren. Eclipse plug-in für Application Insights installiert das SDK im Projekt, so dass Sie aus dem Feld Telemetrie plus eine API, mit denen Sie benutzerdefinierte Telemetrie schreiben.   


## <a name="prerequisites"></a>Erforderliche Komponenten

Derzeit das plug-in arbeitet für Maven und dynamische Web-Projekte in Eclipse. ([Application Insights für andere Java-Projekt hinzufügen][java].)

Sie benötigen Folgendes:

* Oracle JRE 1.6 oder höher
* Ein [Microsoft Azure-](https://azure.microsoft.com/)Abonnement. (Sie können mit der [Testversion](https://azure.microsoft.com/pricing/free-trial/)beginnen.)
* [Eclipse-IDE für Java EE Entwickler](http://www.eclipse.org/downloads/)Indigo oder höher.
* Windows 7 oder höher oder WindowsServer 2008 oder höher

## <a name="install-the-sdk-on-eclipse-one-time"></a>Installieren Sie das SDK auf Eclipse (einmal)

Sie müssen nur einmal pro Computer durchführen. Durch diesen Schritt wird eine Toolkit, die dann das SDK Dynamic Web hinzufügen kann.

1. Klicken Sie in Eclipse auf Hilfe, neue Software installieren.

    ![Installieren neuen Software](./media/app-insights-java-eclipse/0-plugin.png)

2. Das SDK ist in http://dl.windowsazure.com/eclipse unter Azure Toolkit. 
3. Deaktivieren Sie **Alle Update-Sites zu kontaktieren...**

    ![Application Insights SDK erhalten Sie löschen alle Update-sites](./media/app-insights-java-eclipse/1-plugin.png)

Die verbleibenden Schritte für jede Java-Projekt.

## <a name="create-an-application-insights-resource-in-azure"></a>Erstellen Sie eine Ressource Anwendung Einblicke in Azure

1. Mit der [Azure-Portal](https://portal.azure.com)anmelden.
2. Erstellen Sie eine neue Application Insights-Ressource.  

    ![Klicken Sie auf + und Anwendung Einblicke](./media/app-insights-java-eclipse/01-create.png)  
3. Legen Sie den Anwendungstyp Java Web Application.  

    ![Geben Sie einen Namen, wählen Sie Java WebApp und klicken Sie auf Erstellen](./media/app-insights-java-eclipse/02-create.png)  
4. Suchen Sie den instrumentationsschlüssel der neuen Ressource. Sie müssen dies in Kürze in das Codeprojekt einfügen.  

    ![Übersicht über neue Ressourcen auf Eigenschaften, und kopieren Sie den Schlüssel Instrumentation](./media/app-insights-java-eclipse/03-key.png)  


## <a name="add-application-insights-to-your-project"></a>Anwendung Erkenntnisse zu Ihrem Projekt hinzufügen

1. Anwendung Erkenntnisse aus dem Kontextmenü Java Webprojekt hinzufügen

    ![Übersicht über neue Ressourcen auf Eigenschaften, und kopieren Sie den Schlüssel Instrumentation](./media/app-insights-java-eclipse/02-context-menu.png)

2. Fügen Sie des Instrumentation-Schlüssels, den von Azure-Portal haben.

    ![Übersicht über neue Ressourcen auf Eigenschaften, und kopieren Sie den Schlüssel Instrumentation](./media/app-insights-java-eclipse/03-ikey.png)


Der Schlüssel wird zusammen mit jedem Element der Telemetrie gesendet und Anwendung Einblicke in die Ressource angezeigt wird.

## <a name="run-the-application-and-see-metrics"></a>Führen Sie die Anwendung und Metriken finden Sie unter

Die Anwendung auszuführen.

Zurück zu der Ressource Anwendung Einblicke in Microsoft Azure.

HTTP-Anfragen Daten werden auf die Übersicht angezeigt. (Wenn keine, warten Sie einige Sekunden und klicken Sie auf aktualisieren.)

![Serverantwort Anfrage zählt und Fehler ](./media/app-insights-java-eclipse/5-results.png)
 

Klicken Sie auf Diagramme detailliertere Kriterien anzeigen. 

![Anforderung zählt nach Namen](./media/app-insights-java-eclipse/6-barchart.png)


[Erfahren Sie mehr über Metriken.][metrics]

 

Und beim Anzeigen der Eigenschaften einer Anforderung sehen Sie die Telemetrie-Ereignisse Ausnahmen wie Anfragen zugeordnet.
 
![Alle Spuren für diese Anforderung](./media/app-insights-java-eclipse/7-instance.png)


## <a name="client-side-telemetry"></a>Clientseitige Telemetrie

Das Blade Schnellstart klicken Sie auf Code abrufen, um Webseiten zu überwachen: 

![Ihre app Übersicht Blade wählen Sie Quick Start, erhalten Sie meine Webseiten überwachen. Kopieren Sie das Skript.](./media/app-insights-java-eclipse/02-monitor-web-page.png)

Den Codeausschnitt im head-Bereich der HTML-Dateien einfügen.

#### <a name="view-client-side-data"></a>Client-seitige Daten

Die aktualisierten Webseiten öffnen und verwenden. Warten ein oder zwei Minuten, dann wieder Anwendung Einblicke und Blade Verwendung öffnen. (Blatt Übersicht unten Sie und klicken Sie auf Verwendung)

Seite anzeigen, Benutzer und Sitzung-Metriken werden auf die Verwendung angezeigt:

![Sitzung, Benutzer und Seitenansichten](./media/app-insights-java-eclipse/appinsights-47usage-2.png)

[Weitere Informationen zum Einrichten von clientseitigen Telemetrie.][usage]

## <a name="publish-your-application"></a>Veröffentlichen Sie die Anwendung

Jetzt veröffentlichen Sie Ihre Anwendung auf dem Server können mit, und Überwachung der Telemetrie im Portal angezeigt.

* Stellen Sie sicher, dass die Firewall diese Ports Telemetriedaten senden kann:

 * DC.Services.VisualStudio.com:443
 * DC.Services.VisualStudio.com:80
 * F5.Services.VisualStudio.com:443
 * F5.Services.VisualStudio.com:80


* Auf Windows-Servern zu installieren:

 * [Microsoft Visual C++ Redistributable](http://www.microsoft.com/download/details.aspx?id=40784)

    (Diese kann Leistungsindikatoren.)

## <a name="exceptions-and-request-failures"></a>Ausnahmen und fehlgeschlagene

Nicht behandelte Ausnahmen werden automatisch erfasst:

![](./media/app-insights-java-eclipse/21-exceptions.png)

Datensammlung andere Ausnahmen haben Sie zwei Optionen:

* [Legen Sie TrackException im Code aufrufen](app-insights-api-custom-events-metrics.md#track-exception). 
* [Java-Agent auf dem Server installieren](app-insights-java-agent.md). Sie geben die Methoden, die Sie überwachen möchten.


## <a name="monitor-method-calls-and-external-dependencies"></a>Überwachen von Methodenaufrufen und externe Faktoren

[Java-Agent installieren](app-insights-java-agent.md) sich interne Methoden und Anrufe über JDBC mit Daten angegeben.


## <a name="performance-counters"></a>Leistungsindikatoren

Die Blade Übersicht unten und **Server** klicken. Sie sehen einen Bereich von Leistungsindikatoren.


![Klicken Sie auf die Servern nebeneinander Scrollen](./media/app-insights-java-eclipse/11-perf-counters.png)

### <a name="customize-performance-counter-collection"></a>Sammlung von Leistungsindikatoren anpassen

Fügen Sie den folgenden Code unter dem Stammknoten der Datei ApplicationInsights.xml Auflistung der Standardsatz von Leistungsindikatoren um zu deaktivieren:

    <PerformanceCounters>
       <UseBuiltIn>False</UseBuiltIn>
    </PerformanceCounters>

### <a name="collect-additional-performance-counters"></a>Zusätzliche Leistungsindikatoren sammeln

Sie können weitere Leistungsindikatoren gesammelt werden.

#### <a name="jmx-counters-exposed-by-the-java-virtual-machine"></a>JMX-Indikatoren (verfügbar durch die Java Virtual Machine)

    <PerformanceCounters>
      <Jmx>
        <Add objectName="java.lang:type=ClassLoading" attribute="TotalLoadedClassCount" displayName="Loaded Class Count"/>
        <Add objectName="java.lang:type=Memory" attribute="HeapMemoryUsage.used" displayName="Heap Memory Usage-used" type="composite"/>
      </Jmx>
    </PerformanceCounters>

*   `displayName`– Der Name im Application Insights-Portal angezeigt.
*   `objectName`– Der JMX-Objektname.
*   `attribute`– Das Attribut des Objektnamens JMX abrufen
*   `type`(optional) – der Typ des Attributs JMX Objekts:
 *  Standard: einen einfachen Typ wie Int oder Long.
 *  `composite`: die Leistungsindikatorendaten werden im Format 'Attribut.Daten'
 *  `tabular`: die Leistungsindikatorendaten hat das Format einer Tabellenzeile



#### <a name="windows-performance-counters"></a>Windows-Leistungsindikatoren

Jeder [Windows-Leistungsindikator](https://msdn.microsoft.com/library/windows/desktop/aa373083.aspx) ist Mitglied einer Kategorie (auf die gleiche Weise ein Feld ist ein Member einer Klasse). Kategorien kann global oder nummeriert oder benannte Instanzen.

    <PerformanceCounters>
      <Windows>
        <Add displayName="Process User Time" categoryName="Process" counterName="%User Time" instanceName="__SELF__" />
        <Add displayName="Bytes Printed per Second" categoryName="Print Queue" counterName="Bytes Printed/sec" instanceName="Fax" />
      </Windows>
    </PerformanceCounters>

*   DisplayName-Application Insights-Portal angezeigte Name.
*   Kategoriename – die Leistungsindikatorkategorie (Leistungsobjekt) dieser Leistungsindikator zugeordnet ist.
*   CounterName – der Name des Leistungsindikators.
*   Instanzname – der Name der Kategorie Leistungsindikatorinstanz oder eine leere Zeichenfolge (""), wenn die Kategorie eine einzelne Instanz enthält. Bei CategoryName Prozess und sammeln möchten Leistungsindikator aus dem aktuellen JVM-Prozess auf dem Ihre Anwendung ausgeführt, `"__SELF__"`.

Die Leistungsindikatoren werden als benutzerdefinierte Messgrößen im [Metrik-Explorer][metrics].

![](./media/app-insights-java-eclipse/12-custom-perfs.png)


### <a name="unix-performance-counters"></a>UNIX-Leistungsindikatoren

* Eine Vielzahl von Daten und [reststoffe mit Application Insights-Plugin installieren](app-insights-java-collectd.md) .

## <a name="availability-web-tests"></a>Verfügbarkeit von Webtests

Anwendung Einblicke Testen Ihre Website regelmäßig zu überprüfen, die es gut reagiert. [Zum Einrichten von][availability], Bildlauf auf Verfügbarkeit.

![Scrollen Sie nach unten, klicken Sie auf Verfügbarkeit, dann fügen Sie Webtests](./media/app-insights-java-eclipse/31-config-web-test.png)

Erhalten Sie Diagramme, die Reaktionszeit sowie e-Mail-Benachrichtigungen fällt der Site.

![Beispiel für Web test](./media/app-insights-java-eclipse/appinsights-10webtestresult.png)

[Informationen Sie über Verfügbarkeit von Webtests.][availability] 

## <a name="diagnostic-logs"></a>Diagnoseprotokolle

Verwenden Sie Logback oder Log4J (1.2 oder v2. 0) für die Protokollierung, können Sie die Ablaufverfolgungsprotokolle Anwendung Erkenntnisse können Sie durchsuchen und suchen Sie automatisch gesendet.

[Erfahren Sie mehr über Diagnoseprotokolle][javalogs]

## <a name="custom-telemetry"></a>Benutzerdefinierte Telemetrie 

Legen Sie ein paar Codezeilen in Java Webtests, was Benutzer mit oder Probleme diagnostizieren. 

Sie können sowohl in Webseite JavaScript serverseitige Java Code einfügen.

[Erfahren Sie mehr über benutzerdefinierte Telemetrie][track]



## <a name="next-steps"></a>Nächste Schritte

#### <a name="detect-and-diagnose-issues"></a>Erkennung und diagnose von Problemen

* [Hinzufügen von Web Client Telemetrie] [ usage] zu Leistung Telemetrie über den WebClient.
* [Einrichten von Webtests] [ availability] um sicherzustellen, dass Ihre Anwendung bleibt live und Reaktionsfähigkeit.
* [Suchen von Ereignissen und Protokollen] [ diagnostic] Probleme diagnostizieren.
* [Log4J oder Logback-Traces erfassen][javalogs]

#### <a name="track-usage"></a>Nutzung nachverfolgen

* [Hinzufügen von Web Client Telemetrie] [ usage] Monitor Seitenansichten und grundlegenden Metriken.
* [Nachverfolgen von benutzerdefinierten Ereignissen und Metriken] [ track] zu lernen, wie die Anwendung sowohl auf dem Client und dem Server verwendet wird.


<!--Link references-->

[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[java]: app-insights-java-get-started.md
[javalogs]: app-insights-java-trace-logs.md
[metrics]: app-insights-metrics-explorer.md
[track]: app-insights-api-custom-events-metrics.md
[usage]: app-insights-web-track-usage.md

 