<properties
   pageTitle="Erste Schritte mit Operations Management Suite Sicherheit und Audits Lösung | Microsoft Azure"
   description="Dieses Dokument unterstützt Sie beim Einstieg in Operations Management Suite Sicherheits- und Lösung Überwachungsfunktionen der hybridcloud überwachen."
   services="operations-management-suite"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="operations-management-suite"
   ms.topic="get-started-article" 
   ms.devlang="na"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/20/2016"
   ms.author="yurid"/>
 
# <a name="getting-started-with-operations-management-suite-security-and-audit-solution"></a>Erste Schritte mit Operations Management Suite Sicherheits- und Audit-Lösung
Dieses Dokument hilft Ihnen schnell mit Operations Management Suite (OMS) Sicherheits- und Lösung zunächst führen Sie durch jede Option.

## <a name="what-is-oms"></a>Was ist OMS?
Microsoft Operations Management Suite (OMS) ist Microsoft Cloud-basierten IT Management Lösung, verwalten und schützen Ihre lokalen und cloud-Infrastruktur. Weitere Informationen über OMS Artikel [Operations Management Suite](https://technet.microsoft.com/library/mt484091.aspx).

## <a name="oms-security-and-audit-dashboard"></a>OMS Sicherheits- und Dashboards

Die OMS Sicherheits- und bietet einer umfassenden Ansicht der Organisation IT-Sicherheitslage mit integrierter Suchanfragen für wichtige Probleme, die Ihre Aufmerksamkeit erfordern. **Sicherheits- und Prüfprotokolle** Dashboard ist die Startseite rund um Sicherheit in OMS. Es bietet allgemeine Einblicke in den Sicherheitsstatus des Computers. Darüber hinaus die Möglichkeit, alle Ereignisse der letzten 24 Stunden, 7 Tage oder alle benutzerdefinierten Zeitrahmen anzuzeigen. Gehen Sie folgendermaßen vor, um **Sicherheits- und** Dashboards zuzugreifen:

1. Klicken Sie im Hauptfenster **Microsoft Operations Management Suite** Dashboard **Einstellungen** Kachel links.
2. Blatt **Einstellungen** klicken Sie **Solutions** **Sicherheits- und** Option.
3. **Sicherheits- und Prüfprotokolle** Dashboard wird angezeigt:

    ![OMS Sicherheits- und Dashboards](./media/oms-security-getting-started/oms-getting-started-fig1-ga.png)

Zugreifen auf diesem Dashboard zum ersten Mal müssen Geräte von OMS überwacht, werden Kacheln mit Daten vom Agent nicht aufgefüllt. Nach der Installation des Agents dauert einige Zeit füllen, daher zunächst sehen Sie fehlen einige Daten wie sie noch in die Cloud hochladen.  In diesem Fall ist es normal einige Kacheln ohne konkrete Informationen finden Sie unter. Lesen Sie [Verbinden Windows-Computern direkt OMS](https://technet.microsoft.com/library/mt484108.aspx) Weitere OMS-Agent ein Windows-System und [Linux verbinden Computer OMS](https://technet.microsoft.com/library/mt622052.aspx) für Weitere Informationen zu dieser Aufgabe in einem Linux-System installieren.

> [AZURE.NOTE] Der Agent sammelt die Informationen über die aktuellen Ereignisse, die aktiviert sind, beispielsweise Computernamen, IP-Adresse und Benutzer. Jedoch kein Dokument-Dateien, Datenbankname oder private Daten gesammelt.   

Solutions sind eine Sammlung von Geschäftslogik, Visualisierung und Übernahme Regeln, die Probleme der Kunden. Sicherheits- und eine Lösung, andere können einzeln hinzugefügt werden. Lesen Sie den Artikel [Hinzufügen Lösungen](https://technet.microsoft.com/library/mt674635.aspx) für Weitere Informationen zum Hinzufügen einer neuen Lösung.

OMS Sicherheits- und Dashboard ist in vier Hauptkategorien unterteilt:

- **Security Domains**: in diesem Bereich werden möglicherweise weitere erkunden Sicherheitseinträge Zeitverlauf, Malware Tests aktualisieren Bewertung Sicherheit Identitäts- und Informationen, Computer mit Sicherheitsereignisse und schnellen Zugang zu Azure Security Center Dashboard.
- **Wichtige Punkte**: Diese Option können Sie die Anzahl der aktiven Probleme und der Schweregrad dieser Probleme schnell zu identifizieren.
- **Erkennung (Vorschau)**: ermöglicht das Angriffsmuster Sicherheitshinweise visualisieren, wie sie für Ihre Ressourcen zu identifizieren.
- **Bedrohungsanalyse**: erkennen Sie Angriffsmuster visualisieren Gesamtanzahl Server ausgehenden IP-Verkehr bösartige Bedrohungstyp und eine Karte, wo diese IP-Adressen kommen. 
- **Allgemeine Sicherheitsabfragen**: Diese Option bietet eine Liste der häufigsten Sicherheitsabfragen, mit denen Sie Ihre Umgebung überwachen können. Klicken Sie auf eines dieser Abfragen geöffnet Blade **Suche** die Ergebnisse für die Abfrage.

> [AZURE.NOTE] Weitere Hinweise wie OMS Daten sicher hält lesen Sie wie OMS Ihre Daten schützt.

## <a name="security-domains"></a>Sicherheitsdomänen

Bei Ressourcen unbedingt den aktuellen Zustand der Umgebung zugreifen können. Jedoch ist es auch wichtig Back Ereignisse verfolgen, die in der Vergangenheit aufgetreten sind, die zu einem besseren Verständnis der in Ihrer Umgebung zu bestimmten Zeitpunkt geschieht führen können. 

> [AZURE.NOTE] Data Retention entspricht Plan Preise der OMS. Weitere Informationen finden Sie in der [Microsoft Operations Management Suite](https://www.microsoft.com/server-cloud/operations-management-suite/pricing.aspx) Preisseite.

Incident Response und forensische Untersuchung Szenarien profitieren von den Ergebnissen der **Sicherheitseinträge Zeitverlauf** Kachel verfügbar.

![Sicherheitseinträge langfristig](./media/oms-security-getting-started/oms-getting-started-fig2.JPG)

Wenn Sie auf diese Kachel Blade **Suchen** wird geöffnet, mit einem Abfrageergebnis **Sicherheitsereignisse** (Typ = SecurityEvents) mit den Daten der letzten sieben Tage wie folgt:

![Sicherheitseinträge langfristig](./media/oms-security-getting-started/oms-getting-started-fig3.JPG)

Das Suchergebnis ist in zwei Bereiche unterteilt: der linken gibt einen Überblick über die Anzahl der gefundenen Computer, in der diese Ereignisse gefunden, die Anzahl der Konten, die auf den betreffenden Computern entdeckt wurden und welche Aktivitäten, Sicherheitsereignisse. Rechten bietet die Endergebnisse und eine chronologische Ansicht von Sicherheitsereignissen mit Namen und dem Computer. Sie können auch **Weitere zeigen** mehr Einzelheiten darüber, wie Daten, die Ereignis-ID und die Ereignisquelle klicken.

> [AZURE.NOTE] Weitere Informationen zu OMS-Suchabfrage finden Sie [OMS Verweis suchen](https://technet.microsoft.com/library/mt450427.aspx).

### <a name="antimalware-assessment"></a>Antimalware-Bewertung

Diese Option können Sie Computer von Malware beeinträchtigt werden Computer mit nicht genügend Schutz schnell identifizieren. Malware Bewertung Status und erkannten Gefahren auf den überwachten Servern lesen und die Daten an den OMS-Dienst in der Cloud zur Verarbeitung gesendet. Server mit erkannten Gefahren und Servern mit unzureichender Schutz Malware Bewertung Dashboard angezeigt werden, die nach der Kachel **Antimalware Bewertung anklicken** zugänglich ist. 

![Malware-Bewertung](./media/oms-security-getting-started/oms-getting-started-fig4-ga.png)

Wie alle anderen live Kachel für OMS Dashboard klickt, öffnet **Suche** Blade mit dem Abfrageergebnis. Für diese Option klickt Option **Meldung** unter **Schutz**haben das Abfrageergebnis Sie, das diesen einen Eintrag, der den Computernamen und sein Rang enthält zeigt, wie unten dargestellt:

![Suchergebnis](./media/oms-security-getting-started/oms-getting-started-fig5.png)

> [AZURE.NOTE] *Rang* ist auf den Stand des Schutzes (, aktualisiert usw.) und Gefahren, die gefunden werden. Die als ein hilft Aggregationen zu.

Wenn Sie den Computernamen klicken, haben Sie chronologische Ansicht der Sicherheitsstatus dieses Computers. Dies ist besonders für Szenarien, in denen müssen verstehen, wenn das Modul einmal installiert wurde und wurde entfernt.   

### <a name="update-assessment"></a>Bewertung aktualisieren 

Diese Option können Sie das Gesamtrisiko, potenzielle Sicherheitsprobleme schnell feststellen und ob oder wie wichtig diese Updates für Ihre Umgebung. OMS-Sicherheit und Audits Lösung nur die Visualisierung der Updates, die tatsächlichen Daten stammen von [Updates Lösungen](https://technet.microsoft.com/library/mt484096.aspx), die ein anderes Modul in OMS. Hier ein Beispiel für die Updates:

![System-updates](./media/oms-security-getting-started/oms-getting-started-fig6.png)

> [AZURE.NOTE] Weitere Informationen zu Updates Lösung finden Sie [Server mit dem System-Updates aktualisiert werden](https://technet.microsoft.com/library/mt484096.aspx).

### <a name="identity-and-access"></a>Identitäts- und

Identität sollte die Steuerungsebene für Ihr Unternehmen Schutz Ihrer Identität sollte Ihre oberste Priorität. In der Vergangenheit gab Perimeter um Organisationen und diese Perimeter eines primären defensiven Grenzen, wird heute mehr Daten und mehr apps in der Cloud Die Identität neuer Randbereich. 

> [AZURE.NOTE] derzeit Daten basiert auf Sicherheitsereignisse Zugangsdaten (Ereignis-ID 4624) in die zukünftigen Office365 Benutzernamen und Azure AD-Daten werden ebenfalls berücksichtigt.

Durch Überwachung der Identität werden Sie proaktive Maßnahmen vor Sicherheitsvorfällen Ort oder reaktive Maßnahmen einen Angriffsversuch zu nutzen. **Identitäts- und** Dashboard bietet Ihnen einen Überblick Ihrer Identität, einschließlich der Anzahl der fehlgeschlagenen Anmeldeversuche, diese Versuche, Konten gesperrt wurden, Konten mit verwendeten, Benutzerkonto geändert oder zurückgesetzt, Kennwort und Anzahl Konten angemeldet sind. 

Beim Anklicken der Kachel **Identitäts- und** sehen Sie das folgende Dashboard:

![Identitäts- und](./media/oms-security-getting-started/oms-getting-started-fig7-ga.png)

Die Informationen in diesem Dashboard unterstützen sofort eine mögliche verdächtige Aktivitäten zu erkennen. Beispielsweise sind 338 Anmeldeversuche als **Administrator** und 100 % Versuch ist fehlgeschlagen. Dies kann durch einen brute-Force-Angriff auf dieses Konto verursacht. Wenn Sie auf dieses Konto erhalten Sie weitere Informationen, mit denen Sie die Ressource für dieses potenziellen Angriffs ermitteln können:

![Suchergebnisse](./media/oms-security-getting-started/oms-getting-started-fig8.JPG)

Der ausführliche Bericht enthält wichtige Informationen zu diesem Ereignis einschließlich: der Zielcomputer, die Art der Anmeldung (in diesem Fall Network Logon) und der Aktivität (in diesem Fall Ereignis 4625) eine umfassende Zeitachse jedem Versuch. 

### <a name="computers"></a>Computer

Diese Kachel kann auf allen Computern, die aktiv von Sicherheitsereignissen verwendet werden. Wenn Sie auf diese Kachel klicken sehen Sie die Liste der Computer mit Sicherheitsereignisse die Ereignisse auf jedem Computer:

![Computer](./media/oms-security-getting-started/oms-getting-started-fig9.JPG)

Sie können weiterhin Ihre Untersuchung auf jedem Computer und überprüfen die Sicherheitsereignisse gekennzeichnet wurden.

### <a name="azure-security-center"></a>Azure Security Center

Diese Kachel ist im Grunde eine Verknüpfung zur Azure Security Center Dashboard zugreifen. Weitere Informationen zu dieser Lösung finden Sie bei [Erste Schritte mit Azure Security Center](../security-center/security-center-get-started.md) .

## <a name="notable-issues"></a>Wichtige Punkte

Das Hauptziel dieser Gruppe von Optionen ist ermöglichen einen schnellen Überblick über die Probleme, die Sie in Ihrer Umgebung durch Kategorisierung kritisch, Warnung und informativ. Aktives Problem Typ Kachel eine Visualisierung dieser Probleme, aber es ist kann nicht für weitere Details zu, müssen Sie den unteren Teil der Kacheln verwenden, die den Namen des Problems (NAME), wie viele Objekte (Anzahl) dies und wie wichtig es (Schweregrad).

![Wichtige Punkte](./media/oms-security-getting-started/oms-getting-started-fig10.JPG)

Sie sehen, dass diese Probleme in verschiedenen Bereichen der Gruppe **Sicherheitsdomänen** bereits wurden die Absicht dieser Ansicht verstärkt: die wichtigsten Probleme in der Umgebung von einem zentralen Punkt darstellen.

## <a name="detections-preview"></a>Erkennung (Vorschau)

Das Hauptziel dieser Option soll IT-identifizieren Sie potenzielle Gefahren für ihre Umgebung und die Schwere dieser Bedrohung.

![Bedrohung Intel](./media/oms-security-getting-started/oms-getting-started-fig12.png)

Diese Option kann auch verwendet werden während der Untersuchung Vorfällen Bewertung und Weitere Informationen über den Angriff.

> [AZURE.NOTE] Weitere Informationen zur Verwendung von OMS für Vorfällen [wie Azure Security Center und Microsoft Operations Management Suite Incident Response](https://channel9.msdn.com/Blogs/Taste-of-Premier/ToP1703)ansehen

## <a name="threat-intelligence"></a>Bedrohungsanalyse

Neue Threat Intelligence Abschnitt Lösung Sicherheits- und Visualisierung Angriffsmustern auf verschiedene Arten: Gesamtanzahl Server ausgehenden IP-Verkehr bösartige Bedrohungstyp und eine Karte, wo diese IP-Adressen kommen. Interagieren mit der Karte, und klicken Sie auf die IP-Adressen für Weitere Informationen.

Gelbe Pins auf der Karte angeben aus böswilliger Datenverkehr Ist nicht ungewöhnlich für Server, die Ihre bösartigen Datenverkehr ausgesetzt sind, aber wir empfehlen diesen versucht sicherzustellen, dass keine erfolgreiche Überprüfung. Diese Indikatoren basieren auf IIS-Protokolle, WireData und Windows-Firewall protokolliert.  

![Bedrohung Intel](./media/oms-security-getting-started/oms-getting-started-fig11-ga.png)

## <a name="common-security-queries"></a>Allgemeine Sicherheitsabfragen

Die Liste der allgemeinen Sicherheitsabfragen werden für Sie zu schnell die Informationen basierend auf den Bedürfnissen Ihrer Umgebung anpassen. Diese allgemeinen Abfragen sind:

- Alle Sicherheitsmaßnahmen
- Sicherheitsmaßnahmen auf dem Computer "computer01.contoso.com" (ersetzen durch Ihre eigenen Computernamen)
- Sicherheitsmaßnahmen auf dem Computer "computer01.contoso.com" Konto "Administrator" (ersetzen durch eigene Namen für Computer und Konto)
- Aktivität nach Computer anmelden
- Microsoft Antimalware auf jedem Computer beendet Konten
- Computer, der Microsoft Antimalware-Prozess beendet wurde
- Computer wurde "hash.exe" ausgeführt (ersetzen durch andere Prozessname)
- Alle ausgeführten Prozessnamen
- Aktivität nach Firma anmelden
- Konten auf dem Computer "computer01.contoso.com" (ersetzen durch Ihre eigenen Computernamen) angemeldet

## <a name="see-also"></a>Siehe auch

In diesem Dokument wurden OMS Sicherheits- und Lösung vorgestellt. Weitere OMS-Sicherheit finden Sie in den folgenden Artikeln:

- [Operations Management Suite (OMS) (Übersicht)](operations-management-suite-overview.md)
- [Überwachen und reagieren auf Sicherheitshinweise Operations Management Suite Sicherheit und Audit-Lösung](oms-security-responding-alerts.md)
- [Überwachen von Ressourcen in Operations Management Suite Sicherheit und Audit-Lösung](oms-security-monitoring-resources.md)
