<properties
    pageTitle="Informationen zu Features in BizTalk Services Edition | Microsoft Azure"
    description="Die Funktionen der BizTalk-Dienste Editionen vergleichen: frei, Entwickler, Basic, Standard und Premium. MAK, WABS."
    services="biztalk-services"
    documentationCenter=""
    authors="MandiOhlinger"
    manager="erikre"
    editor=""/>

<tags
    ms.service="biztalk-services"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/15/2016"
    ms.author="mandia"/>


# <a name="biztalk-services-editions-chart"></a>BizTalk-Dienste: Editionen Diagramm

Azure BizTalk-Dienste bietet verschiedene Editionen. Anhand dieses Artikels ermitteln welche Edition Szenarien und sollte.


## <a name="compare-the-editions"></a>Vergleich der Editionen

**Frei (Vorschau)**

Erstellen und Verwalten von Hybridverbindungen. Hybrid-Verbindung ist eine einfache Möglichkeit, eine Azure-Website auf einem lokalen System wie SQL Server herstellen.

**Entwickler**

Hybrid-Verbindungen EAI und EDI-Nachricht einfach zu bedienende trading Partner-Verwaltungsportal und Unterstützung für allgemeine EDI-Schemata und rich EDI-Verarbeitung enthält X12 und AS2. Erstellen EAI-Szenarien mit HTTP-S, REST FTP, WCF und SFTP Protokolle lesen und Schreiben von Services in der Cloud.  Verwenden Sie Verbindung mit Branchensystemen lokalen bereit mit SAP, Oracle eBusiness Oracle DB, Siebel und SQL Server-Adapter. Verwenden Sie eine zentrierte Entwickler-Umgebung mit Visual Studio Tools für einfache Entwicklung und Bereitstellung. Beschränkt auf Entwicklungs- und Testzwecke nur mit keine Service Level Agreement (SLA).

**Grundlegende**

Die meisten Entwicklerfunktionen mit enthält Hybrid-Verbindungen EAI Brücken, EDI-Agreements und BizTalk Adapter Pack Verbindungen. Bietet hohe Verfügbarkeit und ein Service Level Agreement (SLA) skalieren können.

**Standard**

Enthält alle grundlegenden Funktionen mit Hybrid-Verbindungen EAI Brücken, EDI-Agreements und BizTalk Adapter Pack Verbindungen. Bietet hohe Verfügbarkeit und ein Service Level Agreement (SLA) skalieren können.

**Premium**

Enthält die standardmäßigen Funktionen mit Hybrid-Verbindungen EAI Brücken, EDI-Agreements und BizTalk Adapter Pack Verbindungen. Auch archivieren, hohe Verfügbarkeit und ein Service Level Agreement (SLA) skalieren können.


## <a name="editions-chart"></a>Ausgaben-Diagramm
Die folgende Tabelle zeigt die Unterschiede.

<table border="1">
<tr bgcolor="FAF9F9">
        <th></th>
        <th>Frei (Vorschau)</th>
        <th>Entwickler</th>
        <th>Grundlegende</th>
        <th>Standard</th>
        <th>Premium</th>
</tr>

<tr>
<td><strong>Startpreis</strong></td>
<td colspan="5"><a HREF="http://go.microsoft.com/fwlink/p/?LinkID=304011"> Preise Azure BizTalk-Dienste</a> <br/><br/> <a HREF="http://azure.microsoft.com/pricing/calculator/?scenario=full">Azure-Preisrechner</a></td>
</tr>
<tr>
<td><strong>Minimale Standardkonfiguration</strong></td>
<td>1 freie Einheit</td>
<td>1 Entwicklereinheit</td>
<td>1 Einheit</td>
<td>1 standard-Einheit</td>
<td>1 Einheit</td>
</tr>
<tr>
<td><strong>Skalierung</strong></td>
<td>Keine Skalierung</td>
<td>Keine Skalierung</td>
<td>Ja, in Schritten von 1 Einheit</td>
<td>Ja, in Schritten von 1 Standard Einheit</td>
<td>Ja, in Schritten von 1 Einheit</td>
</tr>
<tr>
<td><strong>Maximale Skalierung</strong></td>
<td>Keine Skalierung</td>
<td>Keine Skalierung</td>
<td>Bis zu 8 Stück</td>
<td>Bis zu 8 Stück</td>
<td>Bis zu 8 Stück</td>
</tr>
<tr>
<td><strong>EAI-Brücken pro Einheit</strong></td>
<td>Nicht enthalten</td>
<td>25</td>
<td>25</td>
<td>125</td>
<td>500</td>
</tr>
<tr>
<td><strong>EDI AS2</strong>
<br/><br/>
TPM-Abkommen enthält</td>
<td>Nicht enthalten</td>
<td>Enthalten. 10 Vereinbarungen pro Einheit.</td>
<td>Enthalten. 50 Vereinbarungen pro Einheit.</td>
<td>Enthalten. 250 Vereinbarungen pro Einheit.</td>
<td>Enthalten. 1000 Vereinbarungen pro Einheit.</td>
</tr>
<tr>
<td><strong>Hybridverbindungen pro Einheit</strong></td>
<td>5</td>
<td>5</td>
<td>10</td>
<td>50</td>
<td>100</td>
</tr>
<tr>
<td><strong>Hybride Verbindung Datenübertragung (GB) pro Einheit</strong></td>
<td>5</td>
<td>5</td>
<td>50</td>
<td>250</td>
<td>500</td>
</tr>
<tr>
<td><strong>BizTalk Adapter Verbindungen zu lokalen LOB-Systemen</strong></td>
<td>Nicht enthalten</td>
<td>1 Verbindung</td>
<td>2 Anschlüsse</td>
<td>5 Anschlüsse</td>
<td>25-Verbindungen</td>
</tr>
<tr>
<td align="left"><strong>Unterstützte Protokolle/Systeme:</strong>
<ul>
<li>HTTP</li>
<li>HTTPS</li>
<li>FTP</li>
<li>SFTP</li>
<li>WCF</li>
<li>Servicebus (SB)</li>
<li>Azure Blob</li>
<li>REST-APIs</li>
</ul>
</td>
<td>Nicht enthalten</td>
<td>Enthalten</td>
<td>Enthalten</td>
<td>Enthalten</td>
<td>Enthalten</td>
</tr>
<tr>
<td><strong>Hohe Verfügbarkeit</strong>
<br/><br/>
Service Level Agreement (SLA) finden Sie in <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=304011">Azure BizTalk Services Preise</a>.
</td>
<td>Nicht enthalten</td>
<td>Nicht enthalten</td>
<td>Enthalten</td>
<td>Enthalten</td>
<td>Enthalten</td>
</tr>
<tr>
<td><strong>Sicherung und Wiederherstellung</strong></td>
<td>Nicht enthalten</td>
<td>Enthalten</td>
<td>Enthalten</td>
<td>Enthalten</td>
<td>Enthalten</td>
</tr>
<tr>
<td><strong>Überwachung</strong></td>
<td>Nicht enthalten</td>
<td>Enthalten</td>
<td>Enthalten</td>
<td>Enthalten</td>
<td>Enthalten</td>
</tr>
<tr>
<td><strong>Archivierung</strong><br/><br/>
Enthält Nachweisbarkeit der Empfangsbestätigung (NRR) und nachverfolgte Nachrichten herunterladen</td>
<td>Nicht enthalten</td>
<td>Enthalten</td>
<td>Nicht enthalten</td>
<td>Nicht enthalten</td>
<td>Enthalten</td>
</tr>
<tr>
<td><strong>Verwenden von benutzerdefiniertem code</strong></td>
<td>Nicht enthalten</td>
<td>Enthalten</td>
<td>Enthalten</td>
<td>Enthalten</td>
<td>Enthalten</td>
</tr>
<tr>
<td><strong>Verwenden von Transformationen, einschließlich benutzerdefinierter XSLT</strong></td>
<td>Nicht enthalten</td>
<td>Enthalten</td>
<td>Enthalten</td>
<td>Enthalten</td>
<td>Enthalten</td>
</tr>
</table>

> [AZURE.NOTE] Robustheit vor Hardwarefehlern bedeutet hohe Verfügbarkeit mit mehreren VMs innerhalb einer BizTalk-Einheit.


## <a name="faqs"></a>Häufig gestellte Fragen

#### <a name="what-is-a-biztalk-unit"></a>Was ist eine BizTalk-Einheit?
"Einheit" ist der atomare Azure BizTalk Services-Bereitstellung. Jede Edition verfügt über eine Einheit, die verschiedene Rechenkapazität und Speicher. Beispielsweise eine weitere Compute als Entwickler hat Standard hat mehr berechnen als grundlegende und So weiter. Beim Skalieren von BizTalk Service skalieren Sie Einheiten.

#### <a name="what-is-the-difference-between-biztalk-services-and-azure-biztalk-vm"></a>Was ist der Unterschied zwischen BizTalk Services und Azure BizTalk VM?
BizTalk-Dienste bietet eine true Platform-as-a-Service (PaaS) Architektur für Integration Lösungen in der Cloud. PaaS-Modell Sie vollständig auf die Anwendungslogik konzentrieren und Infrastruktur-Management an Microsoft, einschließlich:

- Ohne verwalten oder patch für virtuelle Computer.
- Microsoft gewährleistet die Verfügbarkeit.
- Sie steuern die Skalierung bei Bedarf einfach anfordern mehr oder weniger Kapazität durch Azure-Portal.

BizTalk Server auf Azure Virtual Machines bietet eine Infrastruktur-as-a-Service (IaaS)-Architektur. Virtuelle Computer erstellen und konfigurieren sie genau wie Ihre lokale Umgebung erleichtert das vorhandene Programme in der Cloud keine Änderungen ausführen. Mit IaaS sind Sie für die virtuellen Computer konfigurieren, Verwaltung von virtuellen Maschinen (z. B. Installation von Software und Betriebssystem-Patches) und Architektur der Anwendung für hohe Verfügbarkeit verantwortlich.

Wenn Sie betrachten, neue Integration Lösungen, die Ihre Infrastruktur Verwaltungsaufwand zu minimieren, verwenden Sie BizTalk-Dienste. Wenn Sie schnell suchen vorhandenen BizTalk Projektmappen migrieren oder einer bedarfsgesteuerten Umgebung entwickeln und Testen von BizTalk Server-Anwendung, verwendet BizTalk Server eine Azure Virtual Machine.

#### <a name="what-is-the-difference-between-biztalk-adapter-service-and-hybrid-connections"></a>Was ist der Unterschied zwischen BizTalk Adapter Service und Hybrid?
Der BizTalk-Adapter-Dienst wird von einer Azure BizTalk-Dienst verwendet. Der BizTalk-Adapter-Dienst verwendet BizTalk Adapter Pack auf einen lokalen Zeile Geschäftsbereiche (LOBS). Hybrid-Verbindung stellt eine einfache und bequeme Möglichkeit, Azure Applications in Azure App Service und Azure Mobile Dienste wie die Web Apps auf eine lokale Ressource verbinden.

#### <a name="what-does-hybrid-connection-data-transfer-gb-per-unit-mean-is-this-per-minutehourdayweekmonth-what-happens-when-the-limit-is-reached"></a>Was bedeutet "Hybrid Verbindung Datenübertragung (GB) pro Einheit"? Ist dies pro Minute/Stunde/Tag/Woche/Monat Was geschieht, wenn der Grenzwert erreicht ist?

Der Einzelpreis Hybrid-Verbindung hängt BizTalk Services Edition. Kosten sind einfach, wie viele Daten übertragen. Z. B. kostet das Übertragen von 10 GB Daten täglich weniger als 100 GB täglich übertragen. Mit der [Rechner Preise](https://azure.microsoft.com/pricing/calculator/?scenario=full) für BizTalk-Dienste können bestimmte Kosten bestimmen. Die Grenzwerte sind in der Regel täglich erzwungen. Die überschritten wird jeder Überschuss in Höhe von $1 pro GB berechnet.

#### <a name="when-i-create-an-agreement-in-biztalk-services-why-does-the-number-of-bridges-go-up-by-two-instead-of-just-one"></a>Beim Erstellen einer Vereinbarung BizTalk-Dienste Warum geht Anzahl Brücken durch zwei statt einem einrichten?

Jede Vereinbarung umfasst zwei verschiedene Brücken: Brücke Kommunikation Seite senden und empfangen Seite Kommunikationsbrücke.

####  <a name="what-happens-when-i-hit-the-quota-limit-on-the-number-of-bridges-or-agreements"></a>Was geschieht, wenn ich die Kontingentgrenze für die Anzahl der Brücken oder getroffen?

Sie können nicht neuen Brücken bereitstellen oder Erstellen neuer Verträge. Um mehr bereitzustellen, müssen Sie mehr Einheiten der BizTalk-Dienst skaliert oder auf eine höhere Edition aktualisieren.

#### <a name="how-do-i-migrate-from-one-tier-of-biztalk-services-to-another"></a>Wie migriere ich von einer Ebene der BizTalk-Dienste auf einem anderen?

Die kostenlose Version nicht migriert oder "Skalieren" auf einer anderen Ebene können nicht gesichert und auf einer anderen Ebene wiederhergestellt. Benötigen Sie eine andere Ebene, Erstellen einer neuen BizTalk Service mit der neuen Ebene. Artefakte mit der kostenlosen Edition einschließlich Hybrid, erstellt im neuen BizTalk-Service neu erstellt werden müssen. 

Für die verbleibenden Editionen sollten die Sicherung und Wiederherstellung für Artefakte von einer Ebene zu einer anderen migrieren. Artefakte in der Standard z. B. Sichern und anschließend auf die Premium. [Der BizTalk-Dienste: Sicherung und Wiederherstellung](biztalk-backup-restore.md) beschreibt die unterstützten Migrationspfaden und führt die Artefakte gesichert werden. Beachten Sie, dass Hybrid-Verbindungen nicht gesichert werden. Sichern und Wiederherstellen auf eine neue Ebene erstellen Sie dann hybridverbindungen.  


#### <a name="is-the-biztalk-adapter-service-included-in-the-service-how-do-i-receive-the-software"></a>Ist der BizTalk Adapter-Dienst im Service enthalten? Wie erhalte ich die Software?

Ja, der BizTalk Adapter-Dienst BizTalk Adapter Pack enthaltenen Azure BizTalk-Dienste SDK [herunterladen](http://www.microsoft.com/download/details.aspx?id=39087).

## <a name="next-steps"></a>Nächste Schritte

Zum Erstellen von Azure BizTalk-Dienste in Azure-Portal gehen Sie zu [BizTalk-Dienste: Bereitstellung mit Azure-Portal](biztalk-provision-services.md). Gehen Sie starten erstellen ASP.NET-Webanwendungen [Azure BizTalk-Dienste](http://go.microsoft.com/fwlink/p/?LinkID=235197).

## <a name="additional-resources"></a>Zusätzliche Ressourcen
- [BizTalk-Dienste: Bereitstellung mit Azure-portal](biztalk-provision-services.md)<br/>
- [BizTalk-Dienste: Bereitstellung Statusdiagramm](biztalk-service-state-chart.md)<br/>
- [Der BizTalk-Dienste: Registerkarten Dashboard, Monitor und skalieren](biztalk-dashboard-monitor-scale-tabs.md)<br/>
- [Der BizTalk-Dienste: Sicherung und Wiederherstellung](biztalk-backup-restore.md)<br/>
- [BizTalk-Dienste: Drosselung](biztalk-throttling-thresholds.md)<br/>
- [BizTalk-Dienste: Ausstellername und Aussteller Schlüssel](biztalk-issuer-name-issuer-key.md)<br/>
- [Wie kann ich starten mithilfe von Azure BizTalk-Dienste SDK](http://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
