<properties
   pageTitle="Überwachen und reagieren auf Sicherheitshinweise in Operations Management Suite Sicherheit und Audits Lösung | Microsoft Azure"
   description="Dieses Dokument hilft Ihnen, die Option Threat Intelligence für OMS Sicherheits- und überwachen und reagieren auf Sicherheitshinweise."
   services="operations-management-suite"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="operations-management-suite"
   ms.topic="article" 
   ms.devlang="na"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/18/2016"
   ms.author="yurid"/>

# <a name="monitoring-and-responding-to-security-alerts-in-operations-management-suite-security-and-audit-solution"></a>Überwachen und reagieren auf Sicherheitshinweise Operations Management Suite Sicherheit und Audit-Lösung

Dieses Dokument soll Ihnen die Option Threat Intelligence für OMS Sicherheits- und überwachen und reagieren auf Sicherheitshinweise.

## <a name="what-is-oms"></a>Was ist OMS?

Microsoft Operations Management Suite (OMS) ist die Microsoft-Cloud IT Management-Lösung, die Sie verwalten und schützen Ihre lokalen und cloud-Infrastruktur hilft. Weitere Informationen über OMS Artikel [Operations Management Suite](https://technet.microsoft.com/library/mt484091.aspx).

## <a name="threat-intelligence"></a>Bedrohungsanalyse

Im Unternehmensumfeld, in dem Benutzer einen umfassenden Zugriff auf das Netzwerk und Verbindung mit Daten eine Vielzahl von Geräten verwenden, ist es unerlässlich, dass Ihre Ressourcen überwachen und auf Sicherheitsvorfälle reagieren. Dies ist besonders wichtig aus der Sicherheitsperspektive Lebenszyklus, da einige Cyber, Gefahren auslösen können, warnt oder verdächtigen Aktivitäten, die von herkömmlichen technischen Sicherheitskontrollen identifiziert werden können. 

Mit der **Bedrohungsanalyse** Optionen OMS Sicherheits- und IT-Administratoren Sicherheitsrisiken für die Umgebung beispielsweise identifizieren, erkennen, ob ein bestimmter Computer Teil ein [Botnet](https://www.microsoft.com/security/sir/story/default.aspx#!botnetsection). Computer werden Knoten in einem Botnet Angreifer unerlaubt Malware installieren, das heimlich dieser Computer Befehl und verbindet. Sie können auch Gefahren aus unterirdischen Kommunikationskanäle wie [Darknet](https://www.microsoft.com/security/sir/story/default.aspx#!botnetsection_honeypots_darkents)identifizieren. 

Für die Erstellung dieser Bedrohungsanalyse verwenden OMS Sicherheits- und Daten aus mehreren Quellen in Microsoft. OMS Sicherheits- und wird diese Daten zur Identifizierung von potenzieller Gefahren für Ihre Umgebung nutzen.

Der Bedrohungsanalyse Bereich besteht aus drei Hauptoptionen:
- Server mit ausgehenden böswilliger Datenverkehr
- Erkannte Sicherheitsrisiken Typen
- Threat Intelligence Karte

> [AZURE.NOTE] eine Übersicht über diese Optionen finden Sie unter [Erste Schritte mit Operations Management Suite Sicherheits- und Audit-Lösung](oms-security-getting-started.md).

### <a name="responding-to-security-alerts"></a>Reagieren auf Sicherheitshinweise

Die Schritte des Prozesses [bei Sicherheitsvorfällen](https://technet.microsoft.com/library/cc512623.aspx) gehört zu den Schweregrad der Gefährdung Systeme. In dieser Phase sollten Sie die folgenden Aufgaben ausführen:

- Bestimmen Sie die Art des Angriffs
- Bestimmen der Angriff begonnen
- Bestimmen der Absicht des Angriffs. Wurde der Angriff speziell an Ihre Organisation Informationen zu, oder war er zufällig?
- Die betroffenen Systeme zu identifizieren
- Identifizieren von Dateien, auf die zugegriffen wurde und Bestimmen der Vertraulichkeit dieser Dateien

Sie können **Bedrohungsanalyse** Informationen OMS Sicherheit und Audit-Lösung dieser Aufgaben zu nutzen. Gehen Sie diese **Bedrohungsanalyse** Optionen zuzugreifen:

1. Klicken Sie im Hauptfenster **Microsoft Operations Management Suite** Dashboard auf **Sicherheits- und** nebeneinander.

    ![Sicherheits- und Prüfprotokolle](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig1.png)

2. Im Dashboard **Sicherheits- und** sehen Sie die **Bedrohungsanalyse** Optionen rechts wie folgt:

    ![Bedrohung intel](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig2-ga.png)

Diese drei Kacheln geben Ihnen einen Überblick über die aktuellen Gefahren. **Server mit ausgehenden böswilliger Datenverkehr** können Sie identifizieren, ist jeder Computer überwachen (innerhalb oder außerhalb des Netzwerks) sendet, die bösartigen Datenverkehr im Internet. 

**Entdeckt Bedrohungstypen** Kachel zeigt eine Zusammenfassung der Gefahren, die aktuellen "in der Wildnis", wenn Sie auf diese Kachel wird Weitere Informationen über diese Risiken wie unten:

![Erkannten Bedrohungstypen](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig3.png)

Anklicken, um weitere Informationen zu jeder Gefahr zu extrahieren. Das folgende Beispiel zeigt weitere Details Botnet:

![Weitere Informationen zu einer Bedrohung](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig4.png)

Wie am Anfang dieses Abschnitts beschrieben, kann diese Informationen im Einzelfall Sicherheitsvorfällen sehr nützlich sein. Es kann wichtig sein forensische Untersuchung, müssen Sie die Quelle des Angriffs, welches System kompromittiert wurde und die Zeitleiste. In diesem Bericht Sie erkennen einige wichtige Details über den Angriff wie: die Quelle, die lokale IP gefährdet und aktuellen Sitzungszustand der Verbindung. 

**Threat Intelligence Karte** können Sie die Standorte auf der ganzen Welt identifizieren bösartigen Datenverkehr. Gibt Orange (eingehend) und Rot (ausgehend) Pfeile in dieser Zuordnung, die die Richtung des Datenverkehrs zu identifizieren, wenn Sie auf die Pfeile klicken, es zeigt den Typ der Bedrohung und die Richtung des Datenverkehrs wie folgt:

![Bedrohung Intel Karte](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig5.png)

> [AZURE.NOTE] Sie sehen eine Demo zur Verwendung dieser Funktion während eines Vorfalls Reaktionsprozess durch die Präsentation [mindern Datacenter Sicherheitsrisiken geführte Untersuchung mit Operations Management Suite](https://myignite.microsoft.com/videos/5000) bei Microsoft Ignite geliefert.

## <a name="see-also"></a>Siehe auch

In diesem Dokument haben Sie gelernt, wie die **Bedrohungsanalyse** OMS Sicherheit und Audits Lösung Option Sicherheitshinweise reagieren. Weitere OMS-Sicherheit finden Sie in den folgenden Artikeln:

- [Operations Management Suite (OMS) (Übersicht)](operations-management-suite-overview.md)
- [Erste Schritte mit Operations Management Suite Sicherheits- und Audit-Lösung](oms-security-getting-started.md)
- [Überwachen von Ressourcen in Operations Management Suite Sicherheit und Audit-Lösung](oms-security-monitoring-resources.md)
