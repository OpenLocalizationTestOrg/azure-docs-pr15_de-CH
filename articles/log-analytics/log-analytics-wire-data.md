<properties
    pageTitle="Lösung in Protokollanalyse Draht | Microsoft Azure"
    description="Wire werden konsolidierte Netzwerk- und Daten von Computern mit OMS Operations Manager sowie Agents Windows verbunden. Daten werden mit Ihren Daten zu korrelieren von Daten kombiniert."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="banders"/>

# <a name="wire-data-solution-in-log-analytics"></a>Lösung in Protokollanalyse verbinden

Wire werden konsolidierte Netzwerk- und Daten von Computern mit OMS Operations Manager sowie Agents Windows verbunden. Daten werden mit Ihren Daten zu korrelieren von Daten kombiniert. OMS-Agents auf Computern in der IT-Infrastruktur überwachen Netzwerk und gesendeten Daten auf diesen Computern für installiert network 2-3 Ebenen im [OSI-Modell](https://en.wikipedia.org/wiki/OSI_model) , einschließlich der verschiedenen Protokolle und Ports verwendet.

>[AZURE.NOTE] Die Lösung Draht ist derzeit nicht verfügbar Arbeitsbereiche hinzugefügt werden. Kunden, die bereits die Draht Lösung aktiviert können weiterhin Daten für Wire-Lösung verwenden.

Standardmäßig sammelt OMS Protokolldaten für CPU, Arbeitsspeicher, Datenträger und Netzwerk-Performance-Daten von Leistungsindikatoren in Windows integriert. Netzwerk und anderen Daten erfolgt in Echtzeit für jeden Agent Subnetze sowie auf Anwendungsebene verwendeten Protokolle, vom Computer. Sie können weitere Leistungsindikatoren auf der Registerkarte Protokolle hinzufügen.

Wenn Sie [sFlow](http://www.sflow.org/) oder andere Software mit [Cisco NetFlow](http://www.cisco.com/c/en/us/products/collateral/ios-nx-os-software/ios-netflow/prod_white_paper0900aecd80406232.html)verwendet haben, werden Statistiken und Daten aus Draht finden vertraut sein.

Welche integrierte Protokoll Suchabfragen gehören:

- Agenten, die Übertragung von Daten
- IP-Adresse des Agenten Draht Daten
- Ausgehende Kommunikation von IP-Adressen
- Anzahl von Anwendungsprotokollen gesendeten bytes
- Anzahl der Anwendungsdienst gesendeten bytes
- Andere Protokolle empfangenen Bytes
- Gesamtanzahl der Bytes gesendet und empfangen
- IP-Adressen, die mit Subnetz 10.0.0.0/8 kommuniziert haben
- Durchschnittliche Wartezeit für Verbindung verlässlich bewertet
- Computerprozesse, die initiiert Netzwerkverkehr
- Umfang des Netzwerkverkehrs für einen Prozess

Beim Suchen der Draht Daten filtern und Gruppieren von Daten zu den Top-Agenten Top-Protokolle anzeigen. Oder Sie können bei bestimmten Computern (IP-Adressen-MAC-Adressen) mitgeteilt, wie lange und wie viele Daten gesendet wurde hier Metadaten Netzwerkverkehr Suche basiert – grundsätzlich.

Da Sie Metadaten anzeigen, ist es jedoch nicht unbedingt für eine ausführliche Problembehandlung hilfreich. Drahtdaten in OMS ist keine vollständige Aufzeichnung von Netzwerkdaten. So ist es nicht tief Paketebene Problembehandlung vorgesehen.
Der Vorteil des Agents im Vergleich zu anderen Auflistung, also müssen Sie Geräte installieren, konfigurieren die Netzwerk-Switches oder komplizierte Konfigurationen durchführen. Wire-Daten ist einfach Agent – den Agent auf einem Computer installieren und es eigene Netzwerkverkehr überwacht. Ein weiterer Vorteil ist, wenn Cloudanbieter Hostinganbieter oder Microsoft Azure, wo Benutzer die Fabric-Ebene besitzt ausgeführten Arbeitslasten überwachen.

Im Gegensatz dazu müssen Sie nicht vollständige Transparenz der Vorgänge im Netzwerk, wenn Sie Agenten auf allen Computern in der Netzwerkinfrastruktur nicht installieren.

## <a name="installing-and-configuring-the-solution"></a>Installation und Konfiguration der Lösung
Anhand der folgenden Informationen zum Installieren und Konfigurieren der Lösung.

- Die Draht Lösung erhält Daten von Computern mit Windows Server 2012 R2, Windows 8.1 und höher.
- Microsoft.NET Framework 4.0 oder höher muss auf Computern drahtdaten erhalten soll.
- Hinzufügen der Lösung Draht zu OMS Arbeitsbereich mithilfe der Schritte im [Lösungskatalog Lösungen Protokollanalyse hinzufügen](log-analytics-add-solutions.md).  Es ist keine weitere Konfiguration erforderlich.
- Ggf. Draht Daten für eine bestimmte Lösung müssen Sie die Lösung bereits OMS Arbeitsbereich hinzugefügt.

## <a name="wire-data-data-collection-details"></a>Einzelheiten zur Datensammlung Daten verbinden

Wire-Daten sammelt Metadaten Netzwerkverkehr mit Agenten, die Sie aktiviert haben.

Tabelle von Datenerfassungsmethoden und andere Details wie der Daten für Wire-Daten.


| Plattform | Direkte Agent | SCOM-Agenten | Azure-Speicher | SCOM erforderlich? | SCOM-Agentdaten per Verwaltungsgruppe | Häufigkeit der Collection |
|---|---|---|---|---|---|---|
|Windows (2012 R2 / 8.1 oder höher)|![Ja](./media/log-analytics-wire-data/oms-bullet-green.png)|![Ja](./media/log-analytics-wire-data/oms-bullet-green.png)|![Nein](./media/log-analytics-wire-data/oms-bullet-red.png)|            ![Nein](./media/log-analytics-wire-data/oms-bullet-red.png)|![Nein](./media/log-analytics-wire-data/oms-bullet-red.png)| Jede minute|


## <a name="combining-wire-data-with-other-solution-data"></a>Kombinieren von Netzwerk Daten mit anderen Projektmappen

Von oben integrierten Abfragen zurückgegebene Daten möglicherweise selbst interessant. Jedoch die Nützlichkeit der drahtdaten tritt auf, wenn mit Informationen aus anderen OMS Solutions kombiniert. Sie können beispielsweise verwenden Sicherheitsereignisdaten Sicherheits- und Lösung gesammelt und Draht zu ungewöhnlichen Anmeldeversuche für benannte Prozesse suchen Daten kombinieren.  In diesem Beispiel verwenden Sie die Operatoren IN und DISTINCT an Daten in Ihrer Suchabfrage.

Anforderungen: Im folgende Beispiel verwenden möchten, müssen Sie die Sicherheits- und Lösung installiert. Allerdings können Sie Daten von anderen Projektmappen Verbinden mit Daten für Wire werden ähnliche Ergebnisse erzielt.

### <a name="to-combine-wire-data-with-security-events"></a>Wire-Daten mit Sicherheitsereignisse kombinieren

1. Klicken Sie auf der Seite Übersicht **WireData** .
2. Klicken Sie in der Liste **Allgemeine WireData Abfragen**auf **Betrag der Netzwerkverkehr (in Bytes) vom Prozess** finden in der Liste der zurückgegebenen Prozesse.
    ![Wiredata Abfragen](./media/log-analytics-wire-data/oms-wiredata-01.png)
3. Ist die Liste der Prozesse lang anzuzeigen, können Sie die Suchabfrage entsprechen:

    ```
    Type WireData | measure count() by ProcessName | where AggregatedValue <40
    ```
    Im folgenden Beispiel gezeigt ist ein Prozess DancingPigs.exe, benannt verdächtig erscheinen.
    ![Suchergebnisse wiredata](./media/log-analytics-wire-data/oms-wiredata-02.png)

4. Klicken Sie in der Liste zurückgegebenen Daten mit einen benannten Prozess. In diesem Beispiel wurde DancingPigs.exe geklickt. Die nachfolgenden Ergebnisse Beschreiben des Netzwerkverkehrs wie ausgehende Kommunikation über verschiedene Protokolle.
    ![Wiredata Ergebnisse einen benannten Prozess](./media/log-analytics-wire-data/oms-wiredata-03.png)

5. Da Sicherheits- und Lösung installiert ist, können Sie in die Sicherheitsereignisse Prüfpunkt, die gleichen Prozessname Feldwert ändern die Suchabfrage mit Abfrageoperatoren IN und DISTINCT suchen. Sie dazu Klicken bei drahtdaten und andere Protokolle Lösung Werte im gleichen Format. Ändern Sie die Suchabfrage entsprechen:

    ```
    Type=SecurityEvent ProcessName IN {Type:WireData "DancingPigs.exe" | distinct ProcessName}
    ```    

    ![Wiredata ergibt zusammen mit Daten](./media/log-analytics-wire-data/oms-wiredata-04.png)
6. In den Ergebnissen sehen Kontoinformationen angezeigt wird. Jetzt können Sie Ihre Suchabfrage herausfinden, wie das Konto mit Sicherheits- und Daten mit einer Abfrage wie häufig wurde verfeinern:        

    ```
    Type=SecurityEvent ProcessName IN {Type:WireData "DancingPigs.exe" | distinct ProcessName} | measure count() by Account
    ```

    ![Wiredata Ergebnisse Daten](./media/log-analytics-wire-data/oms-wiredata-05.png)



## <a name="next-steps"></a>Nächste Schritte

- [Suche Protokolle](log-analytics-log-searches.md) Draht detaillierte Suche Datensätze anzeigen.
- Sehen Sie Dan [verwenden Draht Daten im Blog Operations Management Suite Protokollsuche](http://blogs.msdn.com/b/dmuscett/archive/2015/09/09/using-wire-data-in-operations-management-suite.aspx) zusätzliche Informationen wie oft Daten gesammelt werden und wie Sie Auflistungseigenschaften für Operations Manager-Agents ändern.
