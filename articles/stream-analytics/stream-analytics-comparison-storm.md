<properties
    pageTitle="Analytics Plattformen: Apache Storm Vergleich Stream Analytics | Microsoft Azure"
    description="Auswählen einer Cloud Analytics Plattform mit einem Apache Storm gegenüber Stream Analytics erfahren. Verstehen Sie Funktionen und Unterschiede."
    keywords="Analytics Plattform Analytics Plattformen Cloudplattform Analytics, Sturm-Vergleich"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>

# <a name="help-choosing-a-streaming-analytics-platform-apache-storm-comparison-to-azure-stream-analytics"></a>Hilfe bei der Auswahl einer streaming Analytics Plattform: Apache Storm Vergleich zu Azure Stream Analytics

Wählen eine Cloud Analytics mithilfe dieser Apache Storm Vergleich zu Azure Stream Analytics erfahren. Verstehen Sie nutzen der Stream Analytics und Apache Storm als verwalteter Dienst auf Azure HDInsight, damit Sie die richtige Lösung für Ihr Unternehmen können Anwendungsfälle.

Sowohl Analytics Vorteile einer PaaS-Lösung bieten, gibt es einige charakteristische Hauptfunktionen, die sie unterscheiden. Funktionen land sowie die Grenzen dieser Dienste dazu unten aufgeführten auf die Lösung zum Erreichen Ihrer Ziele.

## <a name="storm-comparison-to-stream-analytics-general-features"></a>Storm-Vergleich Stream Analytics: Allgemeines ##
<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong> </strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
                    <strong>Azure Stream Analytics</strong>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
                    <strong>Apache Storm auf HDInsight</strong>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Open Source</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Azure Stream Analytics ist eine proprietäre Microsoft bietet.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Apache Storm ist Ja, eine Apache lizenzierte Technologie.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Microsoft unterstützt</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Ja </p>
            </td>
            <td width="246" valign="top">
                <p>
Ja </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Die Hardware</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Es gibt keine hardwareanforderungen. Azure Stream Analytics ist ein Azure.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Es gibt keine hardwareanforderungen. Apache Storm ist ein Azure.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Oberste Ebene Einheit</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Azure Stream Analytics Kunden bereitstellen und streaming Aufträge zu überwachen.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Mit Apache HDInsight Kunden bereitstellen Sie und überwachen Sie der gesamte Cluster mehrere Sturm Aufträge sowie andere Arbeitslasten (inkl. Batch) hosten kann.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Preis</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Stream Analytics ist vom Umfang der Daten und die Anzahl der Streaming-Einheiten (pro Stunde, die der Einzelvorgang ausgeführt wird).
                </p>
                <p>
                    <a href="http://azure.microsoft.com/en-us/pricing/details/stream-analytics/">Weitere Informationen zu Preisen finden Sie hier.</a>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Apache Sturm auf HDInsight Einheit Einkauf basiert auf Cluster und zahlt abhängig, Cluster, unabhängig von Aufträgen bereitgestellt ausgeführt wird.
                </p>
                <p>
                    <a href="http://azure.microsoft.com/en-us/pricing/details/hdinsight/">Weitere Informationen zu Preisen finden Sie hier.</a>
                </p>
            </td>
        </tr>
    </tbody>
</table>
##Analytics Plattform erstellen##
<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong> </strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
                    <strong>Azure Stream Analytics</strong>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
                    <strong>Apache Storm auf HDInsight</strong>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Funktionen: SQL DSL</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Ja, ist eine benutzerfreundliche SQL Language Support verfügbar.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Benutzer müssen nicht, Schreiben Sie Code in Java C# oder Trident-APIs verwenden.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Funktionen: Zeitliche Operatoren</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Im Fenstermodus Aggregate und zeitliche Joins werden standardmäßig unterstützt.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Zeitliche Operatoren müssen vom Benutzer implementiert werden.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Erfahrung in der Entwicklung</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Interaktiv erstellen und Debuggen Azure-Portal auf Beispieldaten.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Ist Debugging- und Überwachungstool Erfahrung durch Visual Studio für .NET Benutzer für Java und andere Sprachen Entwickler IDE ihrer Wahl verwenden kann.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Debugging-Unterstützung</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Stream Analytics bietet grundlegende Auftragsstatus und Log So debuggen, aber derzeit bietet Flexibilität bei was und wie viel in den Protokollen ie enthalten ist: ausführlichen Modus.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Detaillierte Protokolle stehen für debugging-Zwecke. Zweierlei in Surface Protokolle Benutzer, visual Studio oder Benutzer RDP Cluster Protokolle zugreifen können.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Unterstützung für UDF (benutzerdefinierte Funktionen)</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Derzeit ist keine Unterstützung für UDFs.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
UDFs können in C#, Java oder der gewünschten Sprache geschrieben werden.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Extensible - benutzerdefiniertem code</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Es gibt keine Unterstützung für erweiterbare Code im Stream Analytics.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Ja, ist die Verfügbarkeit auf benutzerdefinierten Code in C#, Java oder anderen Sprachen geschrieben.
                </p>
            </td>
        </tr>
    </tbody>
</table>
##Datenquellen und Ausgaben##
<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong> </strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
                    <strong>Azure Stream Analytics</strong>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
                    <strong>Apache Storm auf HDInsight</strong>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                 <strong>Input-Datenquellen</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>Die unterstützten Eingabequellen sind Azure Ereignis Hubs und Azure-Blobs.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Gibt Connectors Ereignis Hubs, Service Bus, Kafka. Nicht unterstützte Connectors können über benutzerdefinierten Code implementiert werden.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Eingabe von Datenformaten</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Unterstützte Eingabeformate sind Avro, JSON und CSV.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Jedes Format kann über benutzerdefinierten Code implementiert werden.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Ausgaben</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Streaming kann mehrere Ausgaben arbeiten. Unterstützte Ausgaben: Azure Ereignis Hubs, Azure BLOB-Speicher Azure-Tabellen, SQL Azure DB und PowerBI.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Unterstützung für viele Ausgaben in einer Topologie möglicherweise jede Ausgabe benutzerdefinierten Logik für die Weiterverarbeitung. Standardmäßig enthält Sturm Connectors für PowerBI, Azure Ereignis Hubs Azure BLOB-Speicher, Azure DocumentDB, SQL und HBase. Nicht unterstützte Connectors können über benutzerdefinierten Code implementiert werden.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Codierung Datenformate</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Stream Analytics erfordert UTF-8-Datenformat verwendet werden.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Jedem Codierungsformat Daten möglicherweise über benutzerdefinierten Code implementiert werden.
                </p>
            </td>
        </tr>
    </tbody>
</table>
##Verwaltung und Betrieb##
<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong> </strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
                    <strong>Azure Stream Analytics</strong>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
                    <strong>Apache Storm auf HDInsight</strong>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Job-Bereitstellungsmodell</strong>
                </p>
                <p>
                    - <strong>Azure-Portal</strong>
                </p>
                <p>
                    - <strong>Visual Studio</strong>
                </p>
                <p>
                    - <strong>PowerShell</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Bereitstellung wird über Azure-Portal, PowerShell und REST-APIs implementiert.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Depolyment wird über Azure-Portal, PowerShell, Visual Studio und REST-APIs implementiert.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Überwachung (Stats Leistungsindikatoren usw.)</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Überwachung über Azure-Portal und REST-APIs implementiert.
                </p>
                <p>
Der Benutzer kann auch Azure-Warnungen konfigurieren.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Überwachung über Storm-Benutzeroberfläche und REST-APIs implementiert.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Skalierung</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Anzahl der Streaming-Einheiten für jedes Projekt. Jede Einheit Streaming bis zu 1 MB/s verarbeitet. Maximal 50 Einheiten standardmäßig. Aufruf zu erhöhen.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Anzahl der Knoten im Cluster HDI Sturm. Keine Beschränkung für die Anzahl der Knoten (obere Grenze von Azure Kontingent definiert). Aufruf zu erhöhen.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Datenverarbeitung beschränkt</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Benutzer können nach oben oder unten Stückzahl Streaming Datenverarbeitung vergrößern oder optimieren skalieren.
                </p>
                <p>
Bis zu 1 GB/s skalieren </p>
            </td>
            <td width="246" valign="top">
                <p>
Benutzer kann nach oben oder unten Clustergröße Bedürfnisse skalieren.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Beenden, fortsetzen</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Anhalten Sie und fortsetzen Sie der letzten Stelle beendet.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Anhalten und Fortsetzen der letzten platzieren beendet basierend auf das Wasserzeichen.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Update Service und Rahmen</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Automatische Patches ohne Ausfallzeiten.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Automatische Patches ohne Ausfallzeiten.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Business Continuity durch ein äußerst Dienst mit garantierte SLAS</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
SLA 99,9 % Betriebszeit </p>
                <p>
Automatische Wiederherstellung nach Fehlern </p>
                <p>
Wiederherstellung der statusbehafteten zeitliche Operatoren ist integriert.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
SLA 99,9 % Betriebszeit des Clusters Sturm. Apache Storm ist eine Fault tolerant streaming-Plattform, aber es obliegt dem Kunden sicherzustellen, dass ihre streaming Arbeit ohne Unterbrechung ausgeführt.
                </p>
            </td>
        </tr>
    </tbody>
</table>
##Erweiterte Funktionen##
<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong> </strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
                    <strong>Azure Stream Analytics</strong>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
                    <strong>Apache Storm auf HDInsight</strong>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Verspätung und Ereignisbehandlung angeordnet</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Integrierte konfigurierbare neu anordnen, drop-Ereignisse oder Ereignis einstellen.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Benutzer müssen Logik, um dieses Szenario zu behandeln.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Referenzdaten</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Referenzdaten von Azure-Blobs mit max. 100 MB Cache im Arbeitsspeicher Suche verfügbar. Aktualisieren von Daten wird vom Dienst verwaltet.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Unbegrenzt Datengröße. Anschlüsse für HBase, DocumentDB und Azure SQL Server verfügbar. Nicht unterstützte Connectors können über benutzerdefinierten Code implementiert werden.
                </p>
                <p>
Aktualisieren von Daten muss von benutzerdefiniertem Code behandelt werden.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Integration mit Computer</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Durch Konfigurieren von veröffentlicht Azure Machine Learning Modelle Funktionen während ASA Auftrag erstellen <a href="http://blogs.msdn.com/b/streamanalytics/archive/2015/05/24/real-time-scoring-of-streaming-data-using-machine-learning-models.aspx">(private Vorschau)</a>.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Storm Schrauben verfügbar.
                </p>
            </td>
        </tr>
    </tbody>
</table>
