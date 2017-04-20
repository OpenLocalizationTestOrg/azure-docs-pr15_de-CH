<properties
   pageTitle="Reaktion auf einen Vorfall Azure Security Center über | Microsoft Azure"
   description="Dieses Dokument erläutert, wie Azure Security Center ein Incident-Response-Szenario."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.topic="hero-article"
   ms.devlang="na"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/20/2016"
   ms.author="yurid"/>

# <a name="using-azure-security-center-for-an-incident-response"></a>Mithilfe von Azure Security Center für eine Reaktion auf Sicherheitsvorfälle
Viele Organisationen lernen erst einen Angriff auf Sicherheitsvorfälle reagieren. Zur Reduzierung von Kosten und Schäden unbedingt zu einem Sicherheitsvorfall an planen, bevor ein Angriff stattfindet. Azure Security Center können Sie in verschiedenen Stufen der Reaktion auf einen Vorfall.

## <a name="incident-response-planning"></a>Planen von Vorfällen

Ein effektives Plans hängt von drei Hauptfunktionen: schützen, erkennen und reagieren auf Gefahren. Schutz ist zur Vermeidung von Vorfällen Erkennung wird zur Identifizierung von potenziellen Gefahren frühzeitig und reagiert den Angreifer entfernen und Wiederherstellen von Systemen, um die Folgen einer Verletzung zu verringern.

Dieser Artikel wird Sicherheitsvorfällen Phasen aus dem Artikel [Microsoft Azure Security Response in der Cloud](https://gallery.technet.microsoft.com/Azure-Security-Response-in-dd18c678) verwenden, wie im folgenden Diagramm dargestellt:

![Lebenszyklus von Vorfällen](./media/security-center-incident-response/security-center-incident-response-fig1.png)

Sicherheitscenter können bei erkennen, bewerten und diagnostizieren. Es folgen Beispiele für wie Security Center bei der drei ersten Vorfällen kann nützlich sein:

- **Erkennen**: erste Anzeichen einer Untersuchung Ereignis überprüfen.
    - Beispiel: Überprüfung der anfänglichen Überprüfung, dass im Sicherheitscenter Dashboard eine wichtige Warnung ausgelöst wurde.
- **Bewertung**: durchführen die erste Beurteilung Weitere Informationen über verdächtige Aktivitäten.
    - Beispiel: erhalten Sie weitere Informationen über die Warnung.
- **Diagnose**: eine technische Untersuchung und Kapselung, Ausgleich und Abhilfe Strategien zu identifizieren.
    - Beispiel: folgen Sie Sicherheitscenter in dieser bestimmten Sicherheitshinweis beschriebenen Maßnahmen

Das folgende Szenario veranschaulicht das Sicherheitscenter bei erkennen, bewerten und Diagnose-Antworten eines Sicherheitsvorfalls nutzen. Im Sicherheitscenter ist ein [Sicherheitsvorfall](security-center-incident.md) eine Aggregation alle Alerts für eine Ressource mit [kill Kette](https://blogs.technet.microsoft.com/office365security/addressing-your-cxos-top-five-cloud-security-concerns/) ausrichten. Ereignisse werden in den [Sicherheitshinweise](security-center-managing-and-responding-alerts.md) Kachel und Blade. Ein Vorfall zeigt die Liste der verwandten Warnungen, die Sie mehr Informationen über jedes Vorkommen kann. Security Center stellt auch eigenständige Sicherheitshinweise auch Aufspüren verdächtiger Aktivitäten verwendet werden.

## <a name="scenario"></a>Szenario

Contoso migriert kürzlich ihre lokalen Ressourcen in Azure, einschließlich virtueller Maschinen LOB Arbeitslasten und SQL-Datenbanken. Derzeit hat Contoso Core Computer Security Incident Response Team (CSIRT) ein Problem untersuchen Sicherheitsprobleme durch Sicherheitsinformationen nicht mit ihrer aktuellen Incident-Response-Tools integriert. Diese mangelnde Integration führt zu einem Problem Phase erkennen (zu viele falsche Positivdiagnosen) sowie während der Bewertungsphase und Diagnose. Als Teil der Migration sich entschlossen sie im Sicherheitscenter zu helfen, dieses Problem zu beheben.

Die erste Phase der Migration abgeschlossen, nachdem sie diesem alle Ressourcen und alle Sicherheitsaspekte Security Center. Contoso CSIRT ist zentraler Punkt für den Umgang mit Sicherheitsvorfällen Computer. Das Team besteht aus einer Gruppe von Personen, für die Behandlung aller Sicherheitsvorfälle. Die Teammitglieder Aufgaben sicherzustellen, dass kein Bereich der Antwort ist klar definierte entdeckt.

Im Rahmen dieses Szenarios wird sich auf Rollen die folgenden Rollen, die Contoso CSIRT gehören:

![Lebenszyklus von Vorfällen](./media/security-center-incident-response/security-center-incident-response-fig2.png)

Judy ist bei. Zu ihren Aufgaben zählen:

- Überwachen und reagieren auf Sicherheitsrisiken rund um die Uhr.
- Cloud Arbeitslast Besitzer oder Security Analyst ggf. eskalieren.

SAM ist eine Sicherheit und seine Aufgaben umfassen:

- Untersuchen von Angriffen.
- Beseitigen Alerts.
- Arbeiten mit Aufgaben bestimmen und Gegenmaßnahmen.

Wie Sie sehen können, Judy und Sam haben unterschiedliche Aufgaben und muss arbeiten zusammen, um das Sicherheitscenter Informationen.

## <a name="recommended-solution"></a>Empfohlene Lösung

Da Judy und Sam Rollen, verwenden verschiedene Bereiche des Sicherheitscenters sie Informationen für ihre täglichen Aktivitäten zu. Judy verwendet **Sicherheitshinweise** als Teil ihrer täglichen Überwachung.

![Sicherheitshinweise](./media/security-center-incident-response/security-center-incident-response-fig3.png)

Judy verwendet Sicherheitshinweise während der erkennen und bewerten. Nach Abschluss Judy anfängliche Bewertung können sie an Sam zu eskalieren, wenn weitere Untersuchung erforderlich ist. Zu diesem Zeitpunkt verwendet Sam Informationen, die vom Sicherheitscenter manchmal in Verbindung mit anderen Datenquellen in der Diagnose Stufe bereitgestellt wurde.


## <a name="how-to-implement-this-solution"></a>Zum Implementieren dieser Lösung

Wie Sie Azure Security Center in einem Szenario mit Sicherheitsvorfällen verwenden finden wir Judys Schritte in Phasen erkennen und bewerten und sehen was Sam, um das Problem zu diagnostizieren.

### <a name="detect-and-assess-incident-response-stages"></a>Entdeckung und Bewertung von Vorfällen Phasen

Judy Azure-Portal angemeldet und in der Konsole Security Center arbeiten. Als Teil ihrer täglichen Überwachung starten sie wichtige Sicherheitshinweise anhand der folgenden Schritte überprüfen:

1. Klicken Sie auf die **Sicherheitshinweise** und zugreifen Sie **Sicherheitshinweise** Blade.
    ![Security alert blade](./media/security-center-incident-response/security-center-incident-response-fig4.png)

    > [AZURE.NOTE] Im Rahmen dieses Szenarios wird Judy eine Bewertung auf Warnung Aktivität bösartiger SQL ausführen wie in der obigen Abbildung dargestellt.
2. Klicken Sie auf die Warnung **böswillige SQL-Aktivität** und angegriffenen Ressourcen **böswillige SQL Aktivität** Blatt:  ![Ereignis Details](./media/security-center-incident-response/security-center-incident-response-fig5.png)

    Auf diesem Blatt Judy kann Notizen über die Ressourcen angegriffen, wie oft dieser Angriff passiert und wenn es erkannt wurde.
3. Klicken Sie auf die **Ressourcen angegriffen** Weitere Informationen über diesen Angriff.

Nach dem Lesen der Beschreibung ist Judy überzeugt sich nicht um ein falsches Positivum und sollten sie diese Systemaustauschs SAM.

### <a name="diagnose-incident-response-stage"></a>Diagnostizieren von Vorfällen Phase

SAM Judy Fall empfängt und startet die Sicherheitscenter vorgeschlagenen Maßnahmen überprüfen.

![Lebenszyklus von Vorfällen](./media/security-center-incident-response/security-center-incident-response-fig6.png)

### <a name="additional-resources"></a>Zusätzliche Ressourcen

Das Sicherheitsteam kann auch die [Security Center Power BI](security-center-powerbi.md) Möglichkeit, verschiedene Berichte nutzen. Diese Berichte können helfen ihnen bei der Untersuchung zu visualisieren, analysieren und Vorschläge sowie Sicherheitshinweise zu filtern. Für Unternehmen, die deren Sicherheitsinformations- und Event Management (SIEM) Lösung während der Untersuchung verwenden, können sie auch [Security Center mit ihrer Lösung integrieren](security-center-integrating-alerts-with-log-integration.md). Sie können auch Azure Prüfprotokolle und Sicherheitsereignisse Virtual Machine (VM) mit [Azure Protokoll Integrationstool](https://blogs.msdn.microsoft.com/azuresecurity/2016/07/21/microsoft-azure-log-integration-preview/)integrieren. Um einen Angriff zu untersuchen, können Sie diese Informationen zusammen mit den Informationen, die Security Center bietet.


## <a name="conclusion"></a>Abschluss

Zusammenstellen eines Teams vor eines Vorfalls auftreten ist sehr wichtig für Ihr Unternehmen und Behandlung von Vorfällen positiv beeinflusst. Die richtigen Tools zum Überwachen Ressourcen helfen dieses Team zur Behebung von Sicherheitsvorfällen genaue Schritte. Security Center- [Funktionen](security-center-detection-capabilities.md) unterstützen IT schnell reagieren auf Sicherheitsvorfälle und Sicherheitsprobleme beheben.
