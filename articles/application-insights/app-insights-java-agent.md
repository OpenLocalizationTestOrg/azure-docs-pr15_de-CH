<properties 
    pageTitle="Überwachen von Abhängigkeiten, Ausnahmen und Ausführungszeiten in Java webapps" 
    description="Erweiterte Überwachung Ihrer Website Java Anwendung Einblicke" 
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
    ms.date="08/24/2016" 
    ms.author="awills"/>
 
# <a name="monitor-dependencies-exceptions-and-execution-times-in-java-web-apps"></a>Überwachen von Abhängigkeiten, Ausnahmen und Ausführungszeiten in Java webapps

*Anwendung Informationen ist in der Vorschau.*

Wenn Sie [Ihrer Anwendung Java Anwendung zum instrumentiert]haben[java], können Sie Java-Agent tiefere Einblicke, ohne Code:


* **Abhängigkeiten:** Daten über Aufrufe von die Anwendung auf andere Komponenten, einschließlich:
 * **REST-Aufrufe** über HttpClient, OkHttp und RestTemplate (Spring).
 * **Redis** Aufrufe über den Client Jedis. Nimmt der Anruf länger als 10 s, ruft der Agent auch die Aufrufargumente.
 * **[JDBC-Aufrufe](http://docs.oracle.com/javase/7/docs/technotes/guides/jdbc/)** - MySQL, SQL Server, PostgreSQL, SQLite, Oracle DB oder Apache Derby DB. Aufrufe von "ExecuteBatch" werden unterstützt. MySQL und PostgreSQL Aufruf dauert länger als 10, meldet der Agent den Abfrageplan. 
* **Ausnahmen abgefangen:** Daten zu Ausnahmen, die durch den Code behandelt werden.
* **-Methode Ausführungszeit:** Daten über die Zeit bestimmte Methoden ausführen.

Um Java-Agent verwenden, installieren Sie auf dem Server. Die Web-apps müssen [Anwendung Einblicke Java SDK]instrumentiert[java].

## <a name="install-the-application-insights-agent-for-java"></a>Installieren Sie den Agent Anwendung Einblicke für Java

1. Auf dem Computer läuft Ihr Java, [den Agenten herunterladen](https://aka.ms/aijavasdk).
2. Bearbeiten Sie Application Server-Startskript, und fügen Sie die folgende JVM:

    `javaagent:`*vollständigen Pfad Agent JAR-Datei*

    In Tomcat auf einem Linux-Computer:

    `export JAVA_OPTS="$JAVA_OPTS -javaagent:<full path to agent JAR file>"`


3. Starten Sie den Anwendungsserver neu.

## <a name="configure-the-agent"></a>Konfigurieren Sie den agent

Erstellen Sie eine Datei mit dem Namen `AI-Agent.xml` und in demselben Ordner wie die Agenten JAR-Datei.

Legen Sie den Inhalt der XML-Datei. Bearbeiten Sie die Indexnummer oder Features lassen möchten. 

```XML

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationInsightsAgent>
      <Instrumentation>
        
        <!-- Collect remote dependency data -->
        <BuiltIn enabled="true">
           <!-- Disable Redis or alter threshold call duration above which arguments are sent.
               Defaults: enabled, 10000 ms -->
           <Jedis enabled="true" thresholdInMS="1000"/>
           
           <!-- Set SQL query duration above which query plan is reported (MySQL, PostgreSQL). Default is 10000 ms. -->
           <MaxStatementQueryLimitInMS>1000</MaxStatementQueryLimitInMS>
        </BuiltIn>

        <!-- Collect data about caught exceptions 
             and method execution times -->

        <Class name="com.myCompany.MyClass">
           <Method name="methodOne" 
               reportCaughtExceptions="true"
               reportExecutionTime="true"
               />

           <!-- Report on the particular signature
                void methodTwo(String, int) -->
           <Method name="methodTwo"
              reportExecutionTime="true"
              signature="(Ljava/lang/String;I)V" />
        </Class>
        
      </Instrumentation>
    </ApplicationInsightsAgent>

```

Sie müssen Berichte Ausnahme und Methode Anzeigedauer für die einzelnen Methoden aktivieren.

Standardmäßig `reportExecutionTime` gilt und `reportCaughtExceptions` ist falsch.

## <a name="view-the-data"></a>Daten anzeigen

In Application Insights-Ressource aggregierten Abhängigkeit und Methode Ausführungszeiten erscheint [unter Leistung Kachel][metrics]. 

Um einzelne Instanzen Abhängigkeit, Ausnahme und Methode suchen, [Suche]öffnen[diagnostic]. 

[Abhängigkeitsprobleme Diagnose - erfahren Sie mehr](app-insights-dependencies.md#diagnosis).



## <a name="questions-problems"></a>Haben Sie Fragen? Probleme?

* Keine Daten? [Firewallausnahmen festlegen](app-insights-ip-addresses.md)
* [Problembehandlung bei Java](app-insights-java-troubleshoot.md)



<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[apiexceptions]: app-insights-api-custom-events-metrics.md#track-exception
[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[eclipse]: app-insights-java-eclipse.md
[java]: app-insights-java-get-started.md
[javalogs]: app-insights-java-trace-logs.md
[metrics]: app-insights-metrics-explorer.md
[usage]: app-insights-web-track-usage.md

 
