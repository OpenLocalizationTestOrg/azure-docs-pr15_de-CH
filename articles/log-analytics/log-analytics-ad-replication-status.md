<properties
    pageTitle="Active Directory-Replikationsstatus Lösung Protokollanalyse | Microsoft Azure"
    description="Active Directory-Replikationsstatus Lösungspaket regelmäßig überwacht Active Directory-Umgebung für alle Replikationsfehler und meldet die Ergebnisse auf dem OMS-Dashboard."
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

# <a name="active-directory-replication-status-solution-in-log-analytics"></a>Active Directory-Replikationsstatus Lösung Protokollanalyse

Active Directory ist eine IT-Umgebung eines Unternehmens. Hohe Verfügbarkeit und hohe Leistung hat jeder Domänencontroller seine eigene Kopie der Active Directory-Datenbank. Domänencontroller replizieren untereinander um unternehmensweit weiterleiten. Fehler in dieser Replikationsvorgang kann eine Vielzahl von Problemen im Unternehmen.

AD-Replikationsstatus Lösungspaket regelmäßig überwacht Active Directory-Umgebung für alle Replikationsfehler und meldet die Ergebnisse auf dem OMS-Dashboard.

## <a name="installing-and-configuring-the-solution"></a>Installation und Konfiguration der Lösung
Anhand der folgenden Informationen zum Installieren und Konfigurieren der Lösung.

- Agents installiert auf Domänencontrollern, die Mitglieder der Domäne ausgewertet werden oder auf AD-Replikationsdaten OMS senden konfiguriert. OMS Windows Computer Verbindung finden Sie unter [Verbinden von Windows-Computern Protokollanalyse](log-analytics-windows-agents.md). Wenn Ihre Domänencontroller bereits vorhandenen System Center Operations Manager-Umgebung, die OMS herstellen möchten gehört, finden Sie unter [Protokollanalyse Operations Manager herstellen](log-analytics-om-agents.md).
- Hinzufügen der Active Directory-Replikationsstatus Lösung OMS Arbeitsbereich mithilfe der Schritte im [Lösungskatalog Lösungen Protokollanalyse hinzufügen](log-analytics-add-solutions.md).  Es ist keine weitere Konfiguration erforderlich.


## <a name="ad-replication-status-data-collection-details"></a>Anzeige des Replikationsstatus Einzelheiten zur Datensammlung

Tabelle von Datenerfassungsmethoden und andere Details wie der Daten für AD-Replikationsstatus.

| Plattform | Direkte Agent | SCOM-Agenten | Azure-Speicher | SCOM erforderlich? | SCOM-Agentdaten per Verwaltungsgruppe | Häufigkeit der Collection |
|---|---|---|---|---|---|---|
|Windows|![Ja](./media/log-analytics-ad-replication-status/oms-bullet-green.png)|![Ja](./media/log-analytics-ad-replication-status/oms-bullet-green.png)|![Nein](./media/log-analytics-ad-replication-status/oms-bullet-red.png)|![Nein](./media/log-analytics-ad-replication-status/oms-bullet-red.png)|![Ja](./media/log-analytics-ad-replication-status/oms-bullet-green.png)| alle 5 Tage|


## <a name="optionally-enable-a-non-domain-controller-to-send-ad-data-to-oms"></a>Optional können Sie Nichtdomänencontroller OMS AD-Daten an
Möchten Sie alle Domänencontroller OMS direkt herstellen, können Sie andere OMS verbundene Computer in Ihrer Domäne Daten Lösungspaket AD Replication Status und die Daten zu senden.

### <a name="to-enable-a-non-domain-controller-to-send-ad-data-to-oms"></a>So aktivieren Sie einen Domänencontroller OMS AD-Daten an
1.  Stellen Sie sicher, dass der Computer Mitglied der Domäne ist, die mit AD-Replikationsstatus Lösung überwachen möchten.
2.  [Verbinden der Computer Windows OMS](log-analytics-windows-agents.md) oder [Ihrer vorhandenen Umgebung Operations Manager OMS verbinden](log-analytics-om-agents.md), wenn sie nicht bereits verbunden ist.
3.  Legen Sie den folgenden Registrierungsschlüssel auf dem Computer:
    - Schlüssel: **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\HealthService\Parameters\Management Gruppen\<"Verwaltungsgruppenname" > \Solutions\ADReplication**
    - Wert: **IsTarge**
    - Wertdaten: **true**

    >[AZURE.NOTE]Diese Änderung werden nicht bis zum Neustart Microsoft Monitoring Agent Service (HealthService.exe) wirksam.

## <a name="understanding-replication-errors"></a>Grundlegendes zu Replikationsfehler
Haben AD Replication Statusdaten an OMS sehen Sie eine Kachel ähnlich der folgenden auf, die angibt, wie viele Replikationsfehler derzeit OMS-Dashboard.  
![AD-Replikationsstatus nebeneinander](./media/log-analytics-ad-replication-status/oms-ad-replication-tile.png)

**Kritische** treten Replikation bei oder über 75 % der [Tombstone-Lebensdauer](https://technet.microsoft.com/library/cc784932%28v=ws.10%29.aspx) für Active Directory-Gesamtstruktur.

Wenn Sie die Kachel klicken, sehen Sie weitere Informationen.
![Anzeige des Replikationsstatus dashboard](./media/log-analytics-ad-replication-status/oms-ad-replication-dash.png)


### <a name="destination-server-status-and-source-server-status"></a>Serverstatus Ziel und Quelle Serverstatus
Diese Karten zeigen den Status der Zielserver und Quellserver, die Replikationsfehler auftreten. Die Zahl nach jeder Name des Domänencontrollers gibt die Anzahl der Replikationsfehler auf diesem Domänencontroller.

Fehler für Zielserver und Quellserver werden angezeigt, da einige Probleme einfacher beheben Source Server und andere aus Serversicht Ziel sind.

In diesem Beispiel sehen Sie, dass viele Zielserver ungefähr dieselbe Anzahl von Fehlern, aber ein Source Server (ADDC35), die viele Fehler als alle anderen. Es ist wahrscheinlich, dass ein Problem auf ADDC35, die zum Senden von Daten an seine Replikationspartner Fehler verursacht. Problembehebung für ADDC35 wahrscheinlich lösen Fehler, die im Ziel Serverblade angezeigt.

### <a name="replication-error-types"></a>Fehler Replikationstypen
Dieses Blatt gibt Informationen über den Fehler im gesamten Unternehmen. Jeder Fehler hat einen eindeutigen numerischen Code und eine Meldung, die die Ursache des Fehlers helfen kann.

Ring oben können Sie nachvollziehen, welche Fehler mehr angezeigt und in Ihrer Umgebung.

Dies können Sie anzeigen, wenn mehrere Domänencontroller den gleichen Replikationsfehler auftreten. In diesem Fall möglicherweise zu einer Lösung auf einem Domänencontroller und auf andere Domänencontroller der gleichen Fehler wiederholt.

### <a name="tombstone-lifetime"></a>Tombstone-Lebensdauer
Die Tombstone-Lebensdauer bestimmt, wie lange ein gelöschtes Objekt als Tombstone bezeichnet, in der Active Directory-Datenbank gespeichert. Wenn ein gelöschtes Objekt die Tombstone-Lebensdauer erreicht, entfernt eine Garbage Collection-Prozesses automatisch aus Active Directory-Datenbank.

Die Tombstonelebensdauer Standard ist 180 Tage für die neuesten Versionen von Windows, aber 60 Tage auf älteren Versionen und explizit von einem Active Directory-Administrator geändert werden.

Es ist wichtig zu wissen, ob Replikationsfehler aufgetreten ist, die nähern oder über die Tombstonelebensdauer. Zwei Domänencontroller einen Replikationsfehler auftritt, der nach der Tombstone beibehalten, wird Replikation zwischen diesen beiden Domänencontrollern selbst wenn zugrunde liegende Replikationsfehler behoben ist deaktiviert.

Die Tombstone-Lebensdauer Blade hilft Ihnen stellen diese ist in Gefahr. Jeder Fehler in der **über 100 % TSL** Kategorie stellt eine Partition, die nicht zwischen der Quelle und dem Ziel für mindestens die Tombstone-Lebensdauer für die Gesamtstruktur repliziert hat.

In diesem Fall wird lediglich den Replikationsfehler beheben nicht ausreichen. Zumindest müssen Sie manuell identifizieren untersuchen und veraltete Objekte bereinigen, bevor Sie Replikation starten können. Sie müssen auch einen Domänencontroller außer Betrieb setzen.

Neben Replikationsfehler hinter der Tombstone beibehalten haben, sollten Sie auch die **TSL 50-75** oder **75 bis 100 % TSL** Kategorien Fehler achten.

Dies sind Fehler, die deutlich veraltete, nicht vorübergehend müssen sie wahrscheinlich die Intervention zu. Die gute Nachricht ist, dass sie nicht die Tombstone-Lebensdauer erreicht haben. Wenn diese Probleme schnell beheben und *vor* erreichen sie die Tombstone-Lebensdauer, Replikation mit minimalem Benutzereingriff starten kann.

Wie bereits erwähnt, werden nebeneinander Dashboard für die AD-Replikationsstatus Lösung die Anzahl *der* Replikationsfehler in Ihrer Umgebung als Fehler, die über 75 % der Tombstone-Lebensdauer definiert ist (einschließlich Fehler über 100 % TSL) Diese Nummer 0 beibehalten wollen.

>[AZURE.NOTE] Alle veralteten Lebensdauer Prozentsatz Berechnungen basieren auf tatsächlichen Tombstone-Lebensdauer für die Active Directory-Gesamtstruktur vertrauen können, dass diese Prozentsätze korrekt sind, auch wenn Sie eine benutzerdefinierte Tombstone-Gültigkeitsdauer festgelegt.

### <a name="ad-replication-status-details"></a>Details zum Active Directory-Replikation
Wenn Sie ein Element in einer Liste klicken, sehen Sie zusätzliche Informationen protokollieren Suche. Die Ergebnisse werden gefiltert, um nur die Fehler zu diesem Element anzuzeigen. Beispielsweise, wenn auf dem ersten Domänencontroller unter **Ziel Serverstatus (ADDC02)**aufgeführt dort gefilterte Suchergebnisse anzeigen Fehler mit diesem Domänencontroller als Zielserver aufgeführt:

![AD Replication Status Fehler in Suchergebnissen](./media/log-analytics-ad-replication-status/oms-ad-replication-search-details.png)

Hier können Sie weiter filtern, ändern die Abfrage und usw.. Weitere Informationen über die Protokoll-Suche finden Sie unter [Protokoll suchen](log-analytics-log-searches.md).

**HelpLink** -Feld zeigt die URL einer Seite TechNet Weitere Details zu diesem Fehler. Sie können kopieren und Informationen zur Problembehandlung und zum Beheben des Fehlers Browserfenster hier einfügen.

Sie können auch **Exportieren** , um die Ergebnisse nach Excel exportieren klicken. Dadurch werden Fehler Replikationsdaten in keiner Weise darstellen möchten.

![exportierte AD Replication Status Fehler in Excel](./media/log-analytics-ad-replication-status/oms-ad-replication-export.png)

## <a name="ad-replication-status-faq"></a>AD-Replikationsstatus FAQ
**F: wie oft werden AD Replication Status aktualisiert?**
A: die Aktualisierung wird alle 5 Tage.

**F: Gibt es eine Möglichkeit zu konfigurieren, wie oft die Daten aktualisiert werden?**
A: derzeit nicht.

**F: muss ich alle meine Domänencontroller OMS Arbeitsbereich hinzufügen, um Replikationsstatus sehen?**
Nein, muss nur ein einzelner Domänencontroller hinzugefügt werden. Haben Sie mehrere Domänencontroller in Ihrem Arbeitsbereich OMS werden Daten von allen OMS gesendet.

**F: Ich möchte Domänencontroller Mein Arbeitsbereich OMS hinzufügen. Kann ich trotzdem Replikationsstatus AD-Lösung verwenden?**
A: Ja. Den Wert des Registrierungsschlüssels zu lassen. Finden Sie [einen Domänencontroller zu OMS AD-Daten senden aktivieren](#to-enable-a-non-domain-controller-to-send-ad-data-to-oms).

**Was ist der Name des Prozesses, der die Auflistung enthält?**
A: AdvisorAssessment.exe

**F: dauert wie lange Daten gesammelt werden?**
A: Daten Auflistung hängt von der Größe der Active Directory-Umgebung jedoch in der Regel weniger als 15 Minuten.

**F: welche Art von Daten werden gesammelt?**
A: Replikationsinformationen werden über LDAP.

**F: Gibt es eine Möglichkeit, Daten konfigurieren?**
A: derzeit nicht.

**Welche Berechtigungen müssen Daten gesammelt werden?**
A: normalen Benutzerberechtigungen in Active Directory sind normalerweise ausreichend.

## <a name="troubleshoot-data-collection-problems"></a>Problembehandlung bei der Daten-Auflistung
Um Daten zu sammeln, erfordert AD Replikationsstatus Lösungspaket mindestens ein Domänencontroller OMS Arbeitsbereich verbunden werden. Bis Sie dies tun, sehen Sie eine Meldung, **weiterhin Daten gesammelt werden**.

Benötigen Sie Hilfe beim Anschließen eines Domänencontroller Dokumentation am [Computer verbinden Windows Protokollanalyse](log-analytics-windows-agents.md)angezeigt. Wenn der Domänencontroller in einer vorhandenen System Center Operations Manager-Umgebung bereits verbunden ist, können Sie alternativ Dokumentation in [System Center Operations Manager, Protokollanalyse Verbindung](log-analytics-om-agents.md)anzeigen.

Wenn Sie einen Domänencontroller direkt mit OMS oder SCOM verbinden möchten, finden Sie unter [Nichtdomänencontroller AD Daten OMS senden aktivieren](#to-enable-a-non-domain-controller-to-send-ad-data-to-oms).


## <a name="next-steps"></a>Nächste Schritte

- Mit der Active Directory-Replikation Status Detaildaten anzeigen [Log durchsucht Protokollanalyse](log-analytics-log-searches.md) .
