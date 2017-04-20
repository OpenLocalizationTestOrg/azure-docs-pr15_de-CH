<properties
    pageTitle="Analytics FAQ anmelden | Microsoft Azure"
    description="Antworten auf häufig gestellte Fragen zu den Protokollanalyse-Dienst."
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
    ms.date="10/10/2016"
    ms.author="banders"/>

# <a name="log-analytics-faq"></a>Protokollanalyse FAQ

Diese FAQ Microsoft ist eine Liste der häufig gestellten Fragen Protokollanalyse in Microsoft Operations Management Suite (OMS). Haben Sie weiteren Fragen zur Protokollanalyse zum [Diskussionsforum](https://social.msdn.microsoft.com/Forums/azure/home?forum=opinsights) und Fragen. Ein Mitglied der Community hilft Ihnen Ihre Antworten. Wenn eine häufig Frage werden wir es zu diesem Artikel hinzufügen, damit es schnell und einfach finden.

## <a name="general"></a>Allgemein

**Welche Kontrollen durch die Anzeige erfolgen und SQL-Bewertung?**

A. Die folgende Abfrage zeigt eine Beschreibung aller Schecks, die derzeit durchgeführt:

```
(Type=SQLAssessmentRecommendation OR Type=ADAssessmentRecommendation) | dedup RecommendationId | select FocusArea, ActionArea, Recommendation, Description | sort Type, FocusArea,ActionArea, Recommendation
```

Die Ergebnisse können dann zur weiteren Überprüfung nach Excel exportiert werden.

* *F: Warum sehe ich etwas anderes als *OMS* in SCOM Administration? **

A: je nach was Update Rollup von SCOM in sind, sehen Sie einen Knoten für *System Center Advisor*, *Betriebliche Informationen*oder *Protokollanalyse*.

Die Zeichenfolge Aktualisierung *OMS* Text ist in einem Management Pack enthalten die manuell importiert werden. Folgen Sie den neuesten SCOM Update Rollup KB-Artikel und aktualisieren Sie OMS-Konsole, um die neuesten Updates in *OMS* -Knoten finden Sie unter.

* *F: Gibt es eine *für lokale* Version OMS? **

A: Nein. Protokollanalyse verarbeitet und sehr große Datenmengen gespeichert werden. Als Cloud-Dienst kann Protokollanalyse zu skalieren ggf. Leistungseinbußen für Ihre Umgebung.

Auch als Cloud-Dienst bedeutet zu Protokollanalyse-Infrastruktur benötigen ausgeführt und erhalten häufig Updates und Fixes.

## <a name="configuration"></a>Konfiguration
**F. ändern kann der Name des Containers Tabelle-Blob zum Lesen von Azure Diagnostics (Bündel)?**  

A.  Nein, dies ist derzeit nicht möglich, aber für eine zukünftige Version geplant.

**F: welche IP-Adressen verwenden die OMS-Dienste? Wie sicherstellen kann ich, dass Firewall nur Datenverkehr zu Diensten OMS lässt?**  

A. Der Protokollanalyse-Dienst basiert auf Azure und Endpunkte empfangen IPs in [Microsoft Azure Datacenter IP-Bereiche](http://www.microsoft.com/download/details.aspx?id=41653).

Bereitstellung von Diensten, wie ändern die tatsächlichen IP-Adressen der OMS-Dienste. Die DNS-Namen über die Firewall zulassen sind [Konfigurieren Proxy- und Firewall](log-analytics-proxy-firewall.md)-Einstellungen in Protokollanalyse dokumentiert.

**F: Ich verwende ExpressRoute für Azure mit. Wird Datenverkehr Protokollanalyse ExpressRoute Verbindung verwenden?**  

A. Die verschiedenen ExpressRoute Datenverkehr werden in der [ExpressRoute-Dokumentation](./expressroute/expressroute-faqs.md#supported-services)beschrieben.

Datenverkehr Protokollanalyse verwendet öffentliche peering ExpressRoute-Verbindung.

**Gibt es eine einfache Möglichkeit zu einem vorhandenen Arbeitsbereich Protokollanalyse anderen Protokollanalyse Workspace-Azure-Abonnement?**  Haben mehrere Kunden OMS Arbeitsbereiche, die wir testen und Erprobung unserer Azure Abonnement und Bewegung zur Produktion, wir sie in Azure-OMS Abonnement zu verschieben möchten.  

A. Die `Move-AzureRmResource` Cmdlet können Sie einen Arbeitsbereich Protokollanalyse und Automation-Konto aus einem Azure-Abonnement auf einen anderen verschieben. Weitere Informationen finden Sie unter [AzureRmResource verschieben](http://msdn.microsoft.com/library/mt652516.aspx).

Diese Änderung kann auch im Azure-Portal erfolgen.

Sie können nicht Daten aus einem Protokollanalyse Arbeitsbereich in einen anderen verschieben oder ändern Bereich Protokollanalyse Daten gespeichert werden.

**F: wie wird SCOM hinzufügen OMS?**

A: Aktualisierung auf das neueste Updaterollup und Importieren von managementpacks können Sie Protokollanalyse SCOM herstellen.

Beachten Sie, dass die SCOM Verbindung mit Protokollanalyse nur für SCOM 2012 SP1 und höher.

**F: wie kann werden sichergestellt, dass ein Agent mit Protokollanalyse kommunizieren?**

A: um sicherzustellen, dass der Agent mit OMS kommunizieren kann, gehen Sie zu: Bedienfeld und Security Settings **Microsoft Monitoring Agent**.

Suchen Sie auf der Registerkarte **Azure Protokoll Analytics (OMS)** ein grünes Häkchen. Ein grünes Häkchen wird bestätigt, dass der Agent mit dem OMS-Dienst kommunizieren kann.

Ein gelbes Warnsymbol bedeutet, dass der Agent Kommunikation mit OMS Probleme auftreten. Ein häufiger Grund ist Microsoft Monitoring Agent-Dienst wurde beendet und neu gestartet werden muss.

**F: wie wird beenden einen Agent Protokollanalyse Kommunikation?**

A: In SCOM, entfernen Sie den Computer aus der Liste der verwalteten OMS. Kommunikation über SCOM für diesen Agent wird beendet. Für Agents OMS direkt verbunden, Sie können aufhalten OMS durch Kommunikation: Bedienfeld und Security Settings **Microsoft Monitoring Agent**.
Entfernen Sie unter **Azure Protokoll Analytics (OMS)**alle Arbeitsbereiche angezeigt.

## <a name="agent-data"></a>Agentdaten

**Wie viele Daten kann ich durch den Agent Protokollanalyse senden? Gibt es eine maximale Datenmenge pro Kunde?**  
A. Kostenlose Plan wird täglich maximal 500 MB pro Arbeitsbereich. Standard- und Premium-Pläne haben keine Begrenzung auf die Datenmenge, die geuploadet wird. Cloud-Dienst Protokollanalyse OMS soll automatisch skalieren Handle das Volume von einem Kunden – selbst wenn Terabyte pro Tag wird kommen.

Der Protokollanalyse-Agent wurde hat eine kleine Stellfläche und enthält einige grundlegende Datenkompression. Tatsächlich schrieb eines Kunden einen Blog auf unsere Agenten und wie beeindruckt sie durchgeführten Tests. Das Datenvolumen hängt Ihre Kunden Solutions. Sie können detaillierte Informationen auf dem Datenträger und Trennung von Lösung unter **Verwendung** der OMS-Übersichtsseite nebeneinander.

Weitere Informationen können Sie einen [Debitor Blog](http://thoughtsonopsmgr.blogspot.com/2015/09/one-small-footprint-for-server-one.html) zu niedrigen Speicherbedarf des Agents OMS lesen.

**Wie viel Netzwerkbandbreite durch Microsoft Management Agent (MMA) verwendet, wenn Daten Protokollanalyse senden?**

A. Bandbreite ist auf die Menge der gesendeten Daten. Daten werden komprimiert, über das Netzwerk gesendet wird.

**Wie viele Daten pro Agent gesendet werden?**

A. Dies hängt:

- Lösung, die Sie aktiviert haben
- die Anzahl der Protokolle und Leistungsindikatoren erfasst wird
- die Datenmenge in den Protokollen

Kostenlose Tarif ist eine gute Möglichkeit, integrierte mehrere Server und Monitor normalerweise Datenvolumen. Insgesamt wird auf **der Verwendungsseite** Verwendung angezeigt.
Für Computer mit den WireData-Agent ausführen, sehen Sie wie viele Daten mithilfe der folgenden Abfrage gesendet wird:

```
Type=WireData (ProcessName="C:\\Program Files\\Microsoft Monitoring Agent\\Agent\\MonitoringHost.exe") (Direction=Outbound) | measure Sum(TotalBytes) by Computer
```



## <a name="next-steps"></a>Nächste Schritte

- Erfahren mehr über Protokollanalyse und sich innerhalb von Minuten [beginnen mit der Protokollanalyse](log-analytics-get-started.md) .
