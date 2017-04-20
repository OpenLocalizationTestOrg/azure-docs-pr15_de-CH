<properties
    pageTitle="Best Practices für Azure Monitor automatische Skalierung. | Microsoft Azure"
    description="Lernen Sie Grundsätze effektive Skalierung in Azure Monitor verwenden."
    authors="kamathashwin"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="ashwink"/>

# <a name="best-practices-for-azure-monitor-autoscaling"></a>Best Practices für die automatische Skalierung Azure-Monitor

In den folgenden Abschnitten in diesem Dokument können Sie die best Practices für automatische Skalierung in Azure verstehen. Nach dem Überprüfen dieser Informationen werden Sie besser skalieren Ihrer Azure-Infrastruktur effektiv nutzen.

## <a name="autoscale-concepts"></a>Skalieren (Konzepte)

- Eine Ressource können nur *eine* Einstellung skalieren
- Eine Einstellung skalieren kann ein oder mehrere Profile und jedes Profil eine oder mehrere Regeln skalieren können.
- Eine Einstellung skalieren skaliert also *,* durch Erhöhung *der Instanzen und* durch Verringern der Anzahl der Instanzen Instanzen horizontal.
 Eine Einstellung skalieren hat ein Maximum, Minimum und Standardwert Instanzen.
- Ein Auftrag skalieren liest immer zugeordnete Metrik, Skalieren wird geprüft, ob es den eingestellten Schwellenwert für dezentrales Skalieren oder Skalierung in überschritten hat. Anzeigen eine Liste von Metriken, skalieren kann Skalieren bei [Azure Monitor Skalierung allgemeinen Bewertungsgrundlagen](insights-autoscale-common-metrics.md).
- Alle Schwellenwerte werden in einer Instanz berechnet. Beispielsweise "Skalierung von 1 Instanz als durchschnittliche CPU-> 80 % bei Instanzenzahl 2", bedeutet Dezentrales Skalieren wird für alle Instanzen die durchschnittliche CPU größer als 80 %.
- Sie erhalten immer Fehler Benachrichtigung per e-Mail. Insbesondere Besitzer, Mitwirkende und Leser der Zielressource-e-Mail. Sie e-Mail immer eine *Wiederherstellung* , wenn skalieren nach einem Ausfall wiederhergestellt und funktionsfähig.
- Sie können anmelden, um erfolgreiche Skalierung Aktion per e-Mail und Webhooks benachrichtigt.

## <a name="autoscale-best-practices"></a>Bewährte skalieren

Verwenden Sie die folgenden best Practices wie skalieren.

### <a name="ensure-the-maximum-and-minimum-values-are-different-and-have-an-adequate-margin-between-them"></a>Sicherstellen der maximalen und minimalen Werte unterscheiden und müssen einen ausreichenden Spielraum zwischen
Wenn Sie eine Einstellung mit mindestens 2 = maximale = 2 und der aktuellen Instanz beträgt 2 keine Aktion möglich. Halten Sie einen ausreichenden Spielraum zwischen maximalen und minimalen Instanz Anzahl inklusive sind. Automatische Skalierung skaliert immer zwischen diesen.

### <a name="manual-scaling-is-reset-by-autoscale-min-and-max"></a>Manuelle Skalierung durch Skalieren min und Max zurücksetzen
Manuellen Aktualisieren die Instanzenzahl auf einen Wert über oder unter die maximale skaliert Autoscale Engine Minimum (sofern unten) oder das Maximum (Wenn oben) automatisch. Beispielsweise legen Sie zwischen 3 und 6. Haben Sie eine ausgeführte Instanz skaliert Autoscale Modul 3 Instanzen bei seiner nächsten Ausführung. Ebenso würde Scale-in 8 Instanzen zurück 6 bei seiner nächsten Ausführung.  Manuelle Skalierung sei sehr Autoscale Regeln zurücksetzen.

### <a name="always-use-a-scale-out-and-scale-in-rule-combination-that-performs-an-increase-and-decrease"></a>Immer kombinieren Sie Skalierung und Skalierung in Regel, die eine zu- und Abnahme ausführt
Verwenden Sie nur einen Teil der Kombination ist Autoscale Scale-in, das Maximum oder Minimum, oder in ein erreicht.

### <a name="do-not-switch-between-the-azure-portal-and-the-azure-classic-portal-when-managing-autoscale"></a>Wechseln Sie beim Skalieren verwalten nicht zwischen Azure-Portal und klassischen Azure-portal
Verwenden Sie für Cloud-Dienste und Anwendungsdienste (Web Apps) Azure-Portal (portal.azure.com) erstellen und verwalten skalieren. Verwenden Sie für Virtual Machine skalieren PoSH, CLI oder REST-API erstellen und verwalten Skalieren festlegen. Nicht wechseln Sie zwischen der klassischen Azure-Portal (manage.windowsazure.com) und Azure-Portal (portal.azure.com) beim Skalieren Konfigurationen verwalten. Azure-Verwaltungsportal und seiner zugrunde liegenden Backend ist eingeschränkt. Verschieben der Azure-Portal Skalieren mit einer Benutzeroberfläche verwalten. Die Optionen sind Autoscale PowerShell, CLI oder REST-API (über Azure Resource Explorer) verwenden.

### <a name="choose-the-appropriate-statistic-for-your-diagnostics-metric"></a>Wählen Sie die entsprechende Statistik für die Diagnose Metrik
Diagnose-Metriken können zwischen * *Mittelwert*, *Minimum*, *Maximum* und* als Metrik Sie durch skalieren. Die häufigste Statistik ist *Durchschnitt*.

### <a name="choose-the-thresholds-carefully-for-all-metric-types"></a>Wählen Sie die Schwellenwerte für alle metrischen sorgfältig
Wir empfehlen verschiedene Schwellenwerte für die Skalierung und Scale-Praxis anhand sorgfältig auszuwählen.

Wir *empfehlen nicht* skalieren Einstellungen wie die Beispiele unten mit den gleichen oder ähnliche Schwellenwerte für, und:

- Instanzen um 1 erhöhen, wenn zählen Thread Count < = 600
- Instanzen von 1 verringern zählen Wenn Thread Count > = 600

Betrachten wir ein Beispiel, was ein Verhalten führen kann, die verwirrend. Cosider die folgende Sequenz.

1. Angenommen Sie, gibt 2 Instanzen zu beginnen und dann die durchschnittliche Anzahl von Threads pro Instanz wird auf 625.
2. Automatische Skalierung skaliert 3. Instanz hinzufügen.
3. Nehmen Sie die durchschnittliche Threadanzahl für Instanzen 575 fällt.
4. Vor der Skalierung nach unten werden skalieren versucht den Endzustand schätzen in skaliert. Beispiel: 575 x 3 (aktuelle Instanzenzahl) = 1725 / 2 (letzte Anzahl Instanzen Wenn verkleinert) = 862.5 Threads. Dies bedeutet, dass skalieren muss sofort wieder Skalierung auch nach, skaliert, wenn die durchschnittliche Threadanzahl bleibt oder auch nur eine kleine Menge. Allerdings wird wieder der gesamte Prozess skaliert wiederhole zu einer Endlosschleife.
5. Vermeidung dieser ("Flügelschlagen" bezeichnet) wird skalieren nicht zu verkleinern. Stattdessen springt und stellt den Zustand wieder, die Serviceaufgabe ausgeführt wird. Dies könnte viele Benutzer verwirren, da skalieren wirken würde nicht die durchschnittliche Threadanzahl kam 575.

Vorkalkulation bei einer Skalierung soll "flappy" vermeiden. Bewahren Sie dieses Verhalten bei der Auswahl die gleichen Schwellenwerte für dezentrales Skalieren und in.

Wir empfehlen einen ausreichenden Spielraum Dezentrales Skalieren und Schwellenwerte auszuwählen. Beispielsweise sollten Sie die folgenden bessere Regelkombination.

- Instanzen um 1 erhöhen, wenn zählen CPU% > = 80
- Instanzen von 1 verringern beim zählen CPU% < = 60

In diesem Fall  

1. Annehmen Sie, dass es 2 Instanzen zu.
2. Geht die durchschnittliche CPU Instanzen 80 skaliert skalieren eine dritte Instanz hinzufügen.
3. Nehmen wir nun an, dass sich langfristig die CPU auf 60.
4. Skalieren der Skala in Regel schätzt den Endzustand würde Skalierung in. Beispiel: 60 x 3 (aktuelle Instanzenzahl) = 180 / 2 (letzte Anzahl Instanzen Wenn verkleinert) = 90. So skalieren nicht skalieren-in da müsste erneut skalieren. Stattdessen springt es verkleinert.
5. Das nächste Mal skalieren überprüft die CPU weiterhin auf 50. Schätzt-Instanz 50 x 3 = 150 / 2 Instanzen = 75 in erfolgreich 2 Instanzen skaliert Skalierung Schwelle von 80 ist.

### <a name="considerations-for-scaling-threshold-values-for-special-metrics"></a>Hinweise zur Skalierung Schwellenwerte für besondere Kriterien
 Der Schwellenwert ist für spezielle Metriken wie Speicher oder Service Bus Queue Length Metrik die durchschnittliche Anzahl von Nachrichten pro aktuelle Anzahl von Instanzen. Sorgfältig auswählen wählen Sie den Schwellenwert für die Metrik.

Wir illustrieren Beispiel um sicherzustellen, dass das Verhalten besser verstehen.

- Instanzen von 1 Anzahl erhöhen, wenn Speicherwarteschlange message Count > = 50
- Instanzen von 1 Anzahl verringern, wenn Speicherwarteschlange Nachricht Anzahl < = 10

Beachten Sie Folgendes:

1. 2 Speicher Warteschlange Instanzen sind vorhanden.
2. Nachrichten kommen und bei der Überprüfung der Speicherwarteschlange liest die Gesamtzahl 50. Sie könnten annehmen, skalieren Scale-Out-Aktion beginnen soll. Beachten Sie jedoch, dass es noch 50/2 = 25 Nachrichten pro Instanz. So erfolgt Skalierung. Für die erste Skalierung erfolgen sollte die gesamte Nachrichtenanzahl in der Speicherwarteschlange 100 sein.
3. Nehmen Sie die gesamte Anzahl 100 erreicht.
4. Ein 3. Speicherinstanz Warteschlange wird aufgrund einer Skalierung hinzugefügt.  Nächste Scale-Out-Aktion nicht statt, bis die gesamte Nachrichtenanzahl in der Warteschlange 150 erreicht, da 150/3 = 50.
5. Jetzt wird die Anzahl der Nachrichten in der Warteschlange verkleinert. 3 Instanzen die erste Skalierung in Aktion geschieht, wenn die Gesamtzahl der Nachrichten in allen Warteschlangen bis zu 30 da hinzufügen 30/3 = 10 Nachrichten pro Instanz der Skala in Schwellenwert ist.

### <a name="considerations-for-scaling-when-multiple-profiles-are-configured-in-an-autoscale-setting"></a>Faktoren für die Skalierung mehrerer Profile in einer Autoscale konfiguriert sind

In einer skalieren, können Sie ein Standardprofil, das immer ohne jede Abhängigkeit zu Zeitplan angewendet wird, oder Sie können wiederkehrende Profil oder ein Profil für einen bestimmten Zeitraum mit Datum und Uhrzeit.

Beim Skalieren Dienst diese Prozesse überprüft immer in der folgenden Reihenfolge:

1. Feste Datum Profil
2. Wiederkehrende Profil
3. Standardprofil ("immer")

Wenn ein Profil Bedingung überprüft skalieren nicht die nächste Profil Bedingung darunter. Skalierungsgröße verarbeitet jeweils nur ein Profil. Daher ggf. auch Verarbeitung Bedingung Profil niedrigeren Tier Vorschriften sowie im aktuellen Profil aufnehmen müssen.

Betrachten wir das Beispiel:

Die Abbildung unten zeigt eine Einstellung Skalieren mit dem Standardprofil minimale Instanzen = 2 und maximale Instanzen = 10. In diesem Beispiel Regeln Dezentrales Skalieren wird die Nachrichtenanzahl in der Warteschlange größer als 10 konfiguriert und Scale-in die Nachrichtenanzahl in der Warteschlange kleiner als 3 ist. Nun kann die Ressource zwischen 2 und 10 Instanzen skalieren.

Darüber hinaus ist eine wiederkehrende Profil für Montag. Wert für minimum Instanzen = 2 und maximale Instanzen = 12. Daher am Montag, das erste Mal Autoscale überprüft diese Bedingung ist die Anzahl der Instanzen 2 zum neuen mindestens 3 skaliert. Als skalieren weiterhin diese Bedingung Profil übereinstimmen (Montag), nur die CPU-Scale-Out in Regeln für dieses Profil konfiguriert verarbeitet. Zu diesem Zeitpunkt überprüft nicht für die Länge der Warteschlange. Jedoch wenn auch Queue Length Zustand überprüft werden soll, sollte Sie Vorschriften über das Standardprofil sowie in Ihrem Profil Montag enthalten.

Ebenso schaltet automatisch skalieren, Profil, wird zunächst überprüft die minimalen und maximalen Vorschriften erfüllt. Ist die Anzahl der Instanzen gleichzeitig 12 skaliert es 10 für das Standardprofil zulässig.

![Skalierungsgröße settings](./media/insights-autoscale-best-practices/insights-autoscale-best-practices.png)

### <a name="considerations-for-scaling-when-multiple-rules-are-configured-in-a-profile"></a>Faktoren für die Skalierung mehrerer Regeln in einem Profil konfiguriert sind
Es gibt Fälle, in denen Sie möglicherweise mehrere Regeln in einem Profil. Die folgende Gruppe skalieren Regeln werden beim mehrere Regeln mithilfe der Dienste verwendet.

Auf *Skalierung*führt skalieren erfüllt jede Regel.
Unter *Skalierung in*skalieren benötigen alle Regeln eingehalten werden.

Um zu veranschaulichen, annehmen Sie, dass Sie 4 Autoscale Folgendes:

- Wenn CPU < 30 % Skalieren im 1
- Wenn Arbeitsspeicher < 50 % Skalieren im 1
- Wenn CPU-> 75 % skalieren 1
- Wenn Speicher > 75 % skalieren 1

Dann wird die folgen:

- Wenn CPU 76 % und Speicher 50 %, Skalierung, wir.
- Wenn 50 % CPU ist und Arbeitsspeicher 76 % wir Skalierung.

Andererseits, wenn CPU 25 % und Speicher 51 % Skalieren wird **nicht** Skalieren bei. Um Dezimalstellen im CPU muß 29 % und Speicher 49 %.

### <a name="always-select-a-safe-default-instance-count"></a>Wählen Sie immer eine sichere Standardeinstellung Instanzenzahl
Die Anzahl der Instanzen unbedingt skalieren Dienst zählen skaliert, wenn Metriken nicht verfügbar sind. Daher wählen Sie eine standardmäßige Instanz, die für Ihre Arbeitslasten.

### <a name="configure-autoscale-notifications"></a>Skalieren-Benachrichtigung konfigurieren
Skalierungsgröße benachrichtigt den Administratoren und der Ressource per e-Mail, wenn Folgendes auftritt:

- Skalierungsgröße Dienst nicht handeln.
- Metriken sind nicht verfügbar für Skalieren Dienst Skala entscheiden.
- Metriken sind verfügbar (Wiederherstellung) wieder zu einer Skala.
Konfigurieren Sie zusätzlich zu den oben genannten e-Mail oder Webhook Benachrichtigungen für erfolgreiche Aktionen benachrichtigt.
