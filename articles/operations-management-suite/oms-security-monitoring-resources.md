<properties
   pageTitle="Überwachen von Ressourcen in Operations Management Suite Sicherheit und Audits Lösung | Microsoft Azure"
   description="Dieses Dokument hilft Ihnen OMS Sicherheit und Audit-Funktionen Ihrer Ressourcen überwachen und Erkennen von Sicherheitsproblemen."
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

# <a name="monitoring-resources-in-operations-management-suite-security-and-audit-solution"></a>Überwachen von Ressourcen in Operations Management Suite Sicherheit und Audit-Lösung

Dieses Dokument hilft Ihnen Ressourcen überwachen und Erkennen von Sicherheitsproblemen OMS Sicherheits- und Audit-Funktionen verwenden.

## <a name="what-is-oms"></a>Was ist OMS?

Microsoft Operations Management Suite (OMS) ist die Microsoft-Cloud IT Management-Lösung, die Sie verwalten und schützen Ihre lokalen und cloud-Infrastruktur hilft. Weitere Informationen über OMS Artikel [Operations Management Suite](https://technet.microsoft.com/library/mt484091.aspx).

## <a name="monitoring-resources"></a>Überwachen von Ressourcen

Wenn möchten verhindern, dass sicherheitsrelevante Vorfälle überhaupt möglich ist. Allerdings ist es unmöglich, alle sicherheitsrelevanten Ereignisse zu verhindern. Wenn ein sicherheitsrelevantes Ereignis geschieht, müssen Sie seine Auswirkung minimieren.  Es gibt drei wichtige Empfehlung, die Anzahl und die Auswirkung von Sicherheitsvorfällen verwendet werden können:

- Bewerten Sie regelmäßig Sicherheitsrisiken in Ihrer Umgebung.
- Überprüfen Sie routinemäßig alle Computersysteme und Netzwerkgeräte, um sicherzustellen, dass alle aktuellen Patches installiert haben.
- Regelmäßiges Überprüfen Sie aller Protokolle und Protokollierungsmechanismen, einschließlich Betriebssystem-Ereignisprotokolle, anwendungsspezifische Protokolle sowie die Systemprotokolle von Angriffserkennungssystemen.

OMS Sicherheits- und Audit-Lösung ermöglicht der IT überwachen alle Ressourcen erleichtert minimieren die Wirkung von Sicherheitsvorfällen. OMS Sicherheits- und hat Sicherheitsdomänen zum Überwachen von Ressourcen verwendet werden können. Bereich Sicherheit bietet schnellen Zugriff auf Optionen, zur Überwachung der Sicherheit, die folgende Bereiche ausführlicher behandelt werden:

- Malware-Bewertung
- Bewertung aktualisieren
- Identitäts- und

> [AZURE.NOTE] eine Übersicht über diese Optionen finden Sie unter [Erste Schritte mit Operations Management Suite Sicherheits- und Audit-Lösung](oms-security-getting-started.md).

### <a name="monitoring-system-protection"></a>Überwachen den Computerschutz

In einer mehrstufigen Verteidigungsstrategie ist jede Schutzschicht wichtig für den gesamten Sicherheitsstatus Ihrer Ressource. Computer mit erkannten Gefahren und Computer mit nicht genügend dargestellt Malware Bewertung Kachel unter Security Domains. Mithilfe der Informationen in der Malware erkennen Sie einen Plan zum Schutz auf die Server anwenden, die sie benötigen. Zugriff auf diese Option wie folgt:

1. Klicken Sie im Hauptfenster **Microsoft Operations Management Suite** Dashboard auf **Sicherheits- und** nebeneinander.

    ![Sicherheits- und Prüfprotokolle](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig1.png)

2. Klicken Sie im Schaltpult **Sicherheits- und** **Antimalware Bewertung** unter **Security Domains**. **Antimalware-Bewertung** Dashboard wird wie folgt dargestellt:

![Malware-Bewertung](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig2-ga.png)

**Malware Bewertung** Dashboard können Sie um die folgenden Sicherheitsaspekte zu identifizieren:

- **Aktive Risiken**: Computer, gefährdet wurden und aktive Gefahren im System.
- **Remediated Gefahren**: Computer gefährdet wurden jedoch die Risiken wurden behoben.
- **Signatur veraltet**: Computer mit Malware-Schutz aktiviert, aber die Signatur ist veraltet.
- **Kein Schutz in Echtzeit**: Computer, auf denen Malware installiert haben.

### <a name="monitoring-updates"></a>Überwachen von updates 

Aus Sicherheitsgründen ist die neuesten Sicherheitsupdates und sollten in Ihrem Updateverwaltungsstrategie einfließen. Microsoft Monitoring Agent Service (HealthService.exe) liest Informationen aus überwachten Computern und sendet diese Informationen an den OMS-Dienst in der Cloud zur Verarbeitung. Microsoft Monitoring Agent Service als automatischen Dienst konfiguriert und sollte immer in der Zielcomputer ausgeführt.

![Überwachen von updates](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig3.png)

Logik, die Aktualisierungsdaten angewendet und Cloud-Dienst die Daten. Fehlende Updates gefunden werden, werden sie auf dem Schaltpult **Updates** angezeigt. Das Dashboard **Updates** können fehlende Updates und entwickeln Sie einen Plan für die Server anzuwenden, die sie benötigen. Gehen Sie das **Updates** Dashboard zugreifen:

1. Klicken Sie im Hauptfenster **Microsoft Operations Management Suite** Dashboard auf **Sicherheits- und** nebeneinander.
2. Klicken Sie im Schaltpult **Sicherheits- und** **Update Bewertung** unter **Security Domains**. Das Update-Schaltpult wird angezeigt, wie unten dargestellt:

![Bewertung aktualisieren](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig4.png)

In diesem Dashboard können Sie eine Bewertung Update, um zu verstehen, den aktuellen Zustand des Computers und die wichtigsten Risiken durchführen. Mithilfe der Kachel **kritische und Sicherheitsupdates** werden IT-Administratoren auf detaillierte Informationen zu den Updates, die wie folgt:

![Suchergebnis](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig5.png)

Dieser Bericht enthalten wichtige Informationen zu der Art der Bedrohung ist das System gefährdet, einschließlich Sicherheitsupdates und MS Bulletin, die weitere Details zu den zugeordneten Microsoft KB-Artikel.

### <a name="monitoring-identity-and-access"></a>Identitäts- und Überwachung

Für Benutzer von überall arbeiten mit verschiedenen Geräten und Zugreifen auf einen riesigen Cloud und lokalen apps unbedingt ihre Anmeldeinformationen geschützt sind. Anmeldeinformationen Diebstahl Angriffe sind in der Angreifer zunächst einen regulären Benutzer Anmeldeinformationen Zugriff auf ein System im Netzwerk zugreift. Oft ersten Angriff ist nur eine Möglichkeit, Zugriff auf das Netzwerk, das Ziel Zugriffsrechte zu ermitteln. 

Angreifer bleibt im Netzwerk mit frei um Anmeldeinformationen aus den anderen Konten angemeldet zu extrahieren. Abhängig von der Systemkonfiguration können diese Anmeldeinformationen in Form eines Hashes, Tickets oder sogar Klartextkennwörter extrahiert werden.  

> [AZURE.NOTE] Computer direkt mit dem Internet verbunden sind, sehen viele Versuche, die Anmeldung mit allen bekannten Benutzernamen (z. B. Administrator). In den meisten Fällen darf die bekannten Benutzernamen nicht verwendet werden und wenn das Kennwort streng genug ist.

Es ist möglich, diese Eindringlinge zu erkennen, bevor sie ein Konto mit Berechtigungen gefährden. Nutzen Sie **OMS Sicherheit und Audits Lösung** Identitäts- und überwachen. Gehen Sie das **Identitäts- und** Dashboard zugreifen:

1. Klicken Sie im Hauptfenster **Microsoft Operations Management Suite** Dashboard Sicherheits- und anordnen.
2. Klicken Sie im Schaltpult **Sicherheits- und** **Identitäts- und** unter **Security Domains**. **Identitäts- und** Dashboard wird wie folgt dargestellt:

![Identitäts- und](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig6-ga.png)

Als Teil der regulären Überwachungsstrategie müssen Identität Überwachung einbezogen werden. IT-Administrator sollte prüfen, ob bestimmter gültiger Benutzername, die viele Versuche. Dies kann anzugeben oder Angreifer, die den echten Benutzernamen erworben und brute-Force- oder ein automatisches Tool, verwendet hartcodiertes Kennwort abgelaufen.

Dieses Dashboard kann die IT Bedrohung zur Identität und Zugriff auf Ressourcen des Unternehmens schnell identifizieren. Es ist insbesondere wichtig auch potenzielle Trends erkennen, z. B. der Kachel Anmeldung im Zeitverlauf Sie sehen Zeitraum wie oft ein fehlgeschlagener Anmeldeversuch erfolgte. In diesem Fall erhalten der Computer **Dateiserver** 35 Anmeldeversuche. Weitere Informationen über diesen Computer erkunden durch Anklicken. 

![Weitere Informationen](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig7-new.png)

Für diesen Computer generierte Bericht zeigt wichtige Details zu diesem Muster. Festgestellt, dass die Spalte **Konto** das Benutzerkonto, das verwendet wurde erhält, um Zugriff auf das System die **TIMEGENERATED** -Spalte gibt das Zeitintervall, in dem der Versuch durchgeführt wurde, und die **LOGONTYPENAME** -Spalte gibt den Speicherort, in dem dieser Versuch durchgeführt wurde. Wenn diese Versuche lokal von einem Programm im System ausgeführt wurden, wird die Spalte **Prozess** den Prozessnamen angezeigt. In Szenarien, wo der Anmeldeversuch eines Programms stammt, bereits der Name des Prozesses und jetzt möglich, weitere Untersuchung im Zielsystem.

## <a name="see-also"></a>Siehe auch

In diesem Dokument haben Sie gelernt, OMS Sicherheit und Lösung Ihrer Ressourcen zu überwachen. Weitere OMS-Sicherheit finden Sie in den folgenden Artikeln:

- [Operations Management Suite (OMS) (Übersicht)](operations-management-suite-overview.md)
- [Erste Schritte mit Operations Management Suite Sicherheits- und Audit-Lösung](oms-security-getting-started.md)
- [Überwachen und reagieren auf Sicherheitshinweise Operations Management Suite Sicherheit und Audit-Lösung](oms-security-responding-alerts.md)