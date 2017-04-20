<properties
    pageTitle="Protokollanalyse Solutions aus Lösungskatalog hinzufügen | Microsoft Azure"
    description="Log analytiklösungen sind eine Sammlung von Logik, Visualisierung und Datenerfassung Regeln bieten Metriken Umgebung Problems gedreht."
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

# <a name="add-log-analytics-solutions-from-the-solutions-gallery"></a>Protokollanalyse Solutions aus Lösungskatalog hinzufügen

Protokoll-Analyse-Lösungen sind eine Sammlung von **, **Visualisierung** und **Daten Übernahme Geschäftsregeln** , die Metriken pivotiert ein bestimmtes Problem Umgebung bereitstellen**. Dieser Artikel Listen Solutions unterstützt Protokollanalyse und enthält Informationen zum Hinzufügen und Entfernen mit Lösungskatalog.

Projektmappen können tiefere Einblicke:

- Untersuchen und beheben Probleme schneller
- Sammeln und Korrelieren von Daten verschiedener
- unterstützen Sie proaktiv mit Planung Patch Statusberichte und Sicherheitskontrolle.


>[AZURE.NOTE] Sie benötigen eine Lösung aktivieren enthält Protokollanalyse Protokoll Suchfunktion. Allerdings erhalten Sie mit Lösungen in den Lösungskatalog Daten Visualisierung, vorgeschlagene sucht und Einblicke.

Nach dem Hinzufügen einer Lösung Daten von den Servern in Ihrer Infrastruktur gesammelt und an den OMS-Dienst gesendet. Verarbeitung durch die OMS Minuten Dienst normalerweise wenige bis zu einer Stunde. Nachdem der Dienst Daten verarbeitet, können Sie es in OMS anzeigen.

Sie können einfach nicht mehr benötigt eine Lösung entfernen. Wenn Sie eine Lösung entfernen, Daten nicht an OMS gesendet wird die Daten Ihrer täglichen Quote reduzieren können ggf..


## <a name="solutions-supported-by-the-microsoft-monitoring-agent"></a>Microsoft Monitoring Agent unterstützt Solutions

OMS mit Microsoft Monitoring Agent an Server können die meisten Lösungen, einschließlich zu diesem Zeitpunkt

- Active Directory-Bewertung
- Alert-Management (ohne SCOM Alerts)
- Antimalware
- Versionsvergleich
- Sicherheit
- SQL-Bewertung
- System-Updates

Folgendes sind jedoch *nicht* mit Microsoft Monitoring Agent und System Center Operations Manager (SCOM) Agent erforderlich.

- Alert-Management (einschließlich SCOM Alerts)
- Das Capacity Management
- Konfiguration-Bewertung

Weitere [Verbindung Operations Manager Protokollanalyse](log-analytics-om-agents.md) Informationen Protokollanalyse SCOM-Agenten herstellen.

### <a name="to-add-a-solution-using-the-solutions-gallery"></a>Eine Lösung im Lösungskatalog hinzufügen

1. Klicken Sie auf der Seite Übersicht in OMS **Lösungskatalog** .    
    ![Lösungskatalog](./media/log-analytics-add-solutions/sol-gallery.png)
2. Auf der Seite OMS Lösungskatalog enthält Informationen Sie zu jeder Lösung. Klicken Sie auf den Namen der Projektmappe, die Sie in OMS hinzufügen möchten.
3. Detaillierte Informationen zu der Lösung wird auf der Seite für die gewählte Lösung angezeigt. Klicken Sie auf **Hinzufügen**.
4. Eine neue Kachel für die Lösung angezeigt auf der Seite OMS und Sie wieder verwenden kann hinzugefügt, nachdem der OMS-Dienst Daten verarbeitet.

## <a name="to-configure-solutions"></a>Konfigurieren von Projektmappen
1. Sie müssen einige Lösungen konfigurieren. Beispielsweise müssen Sie Automatisierung, Azure Site Recovery und Backup konfigurieren, bevor Sie sie verwenden können.
2. Klicken Sie auf die Kachel auf der Übersichtsseite für alle Lösungen.  
    ![Konfigurieren der Lösung](./media/log-analytics-add-solutions/configure-additional.png)
3. Dann konfigurieren Sie die Projektmappe mit den erforderlichen Informationen und klicken Sie dann auf **Speichern**.  
    ![Konfigurieren der Lösung](./media/log-analytics-add-solutions/configure.png)

### <a name="to-remove-a-solution-using-the-solutions-gallery"></a>Eine Lösung im Lösungskatalog entfernen

1. Klicken Sie auf der Seite Übersicht in OMS **Einstellungen** .
2. Auf der Seite Einstellungen unter der Registerkarte Solutions klicken Sie auf **Entfernen** für die Lösung, die Sie entfernen möchten.
3. Klicken Sie im Bestätigungsdialogfeld auf **Ja,** um die Projektmappe zu entfernen.

## <a name="data-collection-details-for-oms-features-and-solutions"></a>Einzelheiten zur Datensammlung für OMS-Features und solutions

Tabelle von Datenerfassungsmethoden und andere Details wie der Daten für OMS-Features und Solutions. Direkte Agenten und SCOM-Agenten sind im Wesentlichen identisch, jedoch direkte Agent zusätzliche Funktionen enthält so OMS-Arbeitsbereich herstellen und über einen Proxy weiterzuleiten. SCOM-Agenten verwenden, muss es als Vermittler OMS OMS zu richten. SCOM-Agenten in dieser Tabelle sind OMS-Agents SCOM verbunden sind. Informationen zum Verbinden Ihrer vorhandenen Umgebung SCOM mit OMS finden Sie unter [Protokollanalyse Operations Manager herstellen](log-analytics-om-agents.md) .

>[AZURE.NOTE] Der Agent die Verwendung bestimmt zu OMS mit Folgendes zum Senden der Daten:

- Sie verwenden entweder direkte Agent oder Agents OMS SCOM zugeordnet.
- Wenn SCOM erforderlich ist, werden Daten von SCOM-Agenten für die Lösung immer OMS gesendet mit der Verwaltungsgruppe SCOM. Wenn SCOM erforderlich ist, wird außerdem SCOM-Agenten von der Lösung verwendet.
- SCOM muss nicht der Tabelle SCOM Agentdaten mithilfe der Verwaltungsgruppe OMS gesendet werden, werden immer SCOM-Agentdaten an OMS gesendet Verwaltungsgruppen verwenden. Umgehen die Verwaltungsgruppe und senden ihre Daten direkt mit OMS Direct.
- Wenn Daten von SCOM-Agenten nicht mithilfe einer Verwaltungsgruppe gesendet werden, dann die Daten direkt an OMS gesendet – die Verwaltungsgruppe zu umgehen.


|Datentyp| Plattform | Direkte Agent | SCOM-Agenten | Azure-Speicher | SCOM erforderlich? | SCOM-Agentdaten per Verwaltungsgruppe | Häufigkeit der Collection |
|---|---|---|---|---|---|---|---|
|AD-Bewertung|Windows|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|  7 Tage|
|Status der Active Directory-Replikation|Windows|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|5 Tage|
|Alarme (Nagios)|Linux|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|Bei der Ankunft|
|Alarme (Zabbix)|Linux|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|1 minute|
|Alarme (Operations Manager)|Windows|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|3 Minuten|
|Antimalware|Windows|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)| stündlich|
|Das Capacity Management|Windows|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)| stündlich|
|Versionsvergleich|Windows|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)| stündlich|
|Versionsvergleich|Linux|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|stündlich|
|Konfiguration Bewertung (legacy Advisor)|Windows|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)| zweimal täglich|
|ETW|Windows|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|5 Minuten|
|IIS-Protokolle|Windows|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|5 Minuten|
|Wichtige Depots|Windows|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|10 Minuten|
|Anwendung Netzwerkgateways|Windows|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|10 Minuten|
|Netzwerk-Sicherheitsgruppen|Windows|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|10 Minuten|
|Office 365|Windows|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|Benachrichtigung|
|Leistungsindikatoren|Windows|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|als geplant, mindestens 10 Sekunden|
|Leistungsindikatoren|Linux|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|als geplant, mindestens 10 Sekunden|
|Service Fabric|Windows|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|5 Minuten|
|SQL-Bewertung|Windows|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)| 7 Tage|
|SurfaceHub|Windows|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|Bei der Ankunft|
|Syslog|Linux|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|von Azure-Speicher: 10 Minuten; vom Agent: Ankunft|
|System-Updates|Windows|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)| 2 Mal pro Tag und 15 Minuten nach der Installation eines Updates|
|Windows-Sicherheitsereignisprotokolle|Windows|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)| für Azure-Speicher: 10 Minuten; für den Agent: Ankunft|
|Windows-Firewall-Protokolle|Windows|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)| Bei der Ankunft|
|Windows-Ereignisprotokolle|Windows|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)| für Azure-Speicher: 1 min; für den Agent: Ankunft|
|Wire-Daten|Windows (2012 R2 / 8.1 oder höher)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)| Jede minute|

## <a name="log-analytics-preview-solutions-and-features"></a>Log Analytics Vorschau Lösungen und Features

Einen Dienst und Devops Vorgehensweisen können wir Kunden Features und Solutions partner.

Bei privaten Vorschau geben wir eine kleine Gruppe von Kunden eine frühe Implementierung des KE oder Feedback und Verbesserung der Lösung. Diese frühe Implementierung bietet minimale Funktionen betriebsbereit.

Unser Ziel ist schnell auszuprobieren was funktioniert, und was nicht finden. Durchlaufen wir diesen Prozess bis die privaten Vorschau Kundenfeedback uns informiert, dass wir eine öffentliche Vorschau bereit.

Während die public Preview Verfügbarmachen wir das Feature oder die Lösung für alle Benutzer zu mehr Feedback überprüfen unsere Skalierung und Effizienz. In dieser Phase:

- Vorschaufunktionen werden in der Registerkarte angezeigt und können von jedem Benutzer aktiviert
- Vorschau Solutions in der Sammlung hinzugefügt werden oder eines veröffentlichten Skripts

### <a name="what-should-i-know-about-preview-features-and-solutions"></a>Was sollte ich über Vorschaufunktionen und Lösungen wissen?

Wir freuen uns über neue Funktionen und Lösungen Liebe Zusammenarbeit zu entwickeln.

Vorschaufunktionen und Lösungen nicht für alle, vor sich eine private Vorschau oder eine öffentliche Vorschau aktivieren sicherstellen, dass Sie OK mit arbeiten, die in der Entwicklung.

Wenn eine Vorschau über das Portal aktivieren anzeigen Sie eine Warnung, die Sie daran erinnert, dass die Funktion in der Vorschau

#### <a name="for-both-private-and-public-preview"></a>*Private* und *Öffentliche* Vorschau

Öffentliche und private Vorschau gilt Folgendes:

- Was funktioniert immer nicht richtig.
  - Probleme reichen von kleinen Ärgernis über etwas nicht funktioniert
- Gibt die Vorschau beeinträchtigen Ihre Systeme / Umgebung
  - Wir versuchen zu negativen Dinge, die Systeme verwenden mit OMS jedoch manchmal unerwarteten Dinge auftreten
- Datenverlust / Beschädigung kann auftreten.
- Wir bitten Sie Diagnoseprotokolle oder andere Daten zu Problemen sammeln
- Das Feature oder die Lösung möglicherweise (vorübergehend oder dauerhaft) entfernt werden
  - Basierend auf unsere Erkenntnisse während der Vorschau, die wir beschließen, das Feature oder die Projektmappe nicht freigeben
- Vorschau nicht oder können nicht getestet wurden mit allen Konfigurationen und wir begrenzen:
  - Die Betriebssysteme verwendet werden kann (z. B. eine Funktion nur gelten, Linux in Vorschau)
  - Der Typ des Agents (MMA, SCOM), die verwendet werden kann (z. B. ein Feature funktioniert nicht mit SCOM in Vorschau)  
- Vorschau Solutions und Funktionen fallen nicht unter die Service Level Agreements
- Nutzung von Vorschaufunktionen entstehen Nutzungsgebühren
- Merkmale oder Funktionen müssen Sie für das Feature / Lösung hilfreich möglicherweise fehlende oder unvollständige
- Funktionen / Lösungen möglicherweise nicht verfügbar in allen Regionen
- Funktionen / Solutions sind nicht lokalisiert
- Funktionen / Lösungen möglicherweise einen Grenzwert für die Anzahl der Kunden oder Geräte verwenden können
- Sie müssen Skripts konfigurieren und die Lösung-Funktion verwenden
- Die Benutzeroberfläche (UI) unvollständig und kann von Tag zu Tag ändern
- Öffentliche Vorschau möglicherweise nicht für die Produktion / kritische Systeme

#### <a name="for-private-preview"></a>*Private* Vorschau

Neben den obigen ist für private Vorschau:

- Wir erwarten Dich zu uns auf Ihr Feedback, damit wir die Farm verbessern können
- Wir können Sie Feedback Umfragen, Telefonanrufe oder E-mail kontaktieren.
- Immer wird nicht korrekt funktioniert
- Wir benötigen eine Non-Disclosure-Vereinbarung (NDA) für die Teilnahme oder enthalten möglicherweise vertrauliche Inhalte
  - Blogs, TWEETS oder andernfalls Kommunikation mit dritten überprüfen Sie mit dem Programm-Manager verantwortlich für die Vorschau Beschränkungen Offenlegung verstehen
- Führen nicht Produktion / kritische Systeme


### <a name="how-do-i-get-access-to-private-preview-features-and-solutions"></a>Wie erhalte ich Zugriff auf private Vorschaufunktionen und Solutions?

Wir laden zum privaten Vorschau über verschiedene je nach der Vorschau.

- Monatliche Kundenumfrage beantworten und uns Berechtigung mit Sie verbessert Ihre Chancen auf einen privaten Vorschau.
- Microsoft-Kundenbetreuer können Sie benennen.
- Basierend auf Twitter [Msopsmgmt](https://twitter.com/msopsmgmt) am anmelden können
- Basierend auf Details gemeinsame Ereignisse – anmelden können treffen wir betrachten ups, Konferenzen und online-Communities.


## <a name="next-steps"></a>Nächste Schritte

- [Suche Protokolle](log-analytics-log-searches.md) Solutions ausführlichen Informationen anzeigen.
