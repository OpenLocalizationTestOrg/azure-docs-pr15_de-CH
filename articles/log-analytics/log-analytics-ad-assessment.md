<properties
    pageTitle="Optimieren Sie Ihre Umgebung mit der Active Directory-Bewertung Lösung Protokollanalyse | Microsoft Azure"
    description="Die Lösung Active Directory können Sie das Risiko- und Server Umgebungen in regelmäßigen Abständen bewerten."
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

# <a name="optimize-your-environment-with-the-active-directory-assessment-solution-in-log-analytics"></a>Optimieren Sie Ihre Umgebung mit der Active Directory-Bewertung Lösung Protokollanalyse

Die Lösung Active Directory können Sie das Risiko- und Server Umgebungen in regelmäßigen Abständen bewerten. Dieser Artikel hilft Ihnen beim Installieren und Verwenden der Lösung, damit Maßnahmen für Probleme nutzen können.

Diese Lösung bietet eine priorisierte Liste der Vorschläge für die bereitgestellten Server-Infrastruktur. Die Empfehlung über vier kategorisiert die schnell dennoch und Aktion.

Die Informationen basieren auf das Wissen und die Erfahrung von Microsoft-Experten aus Tausenden von Kundenbesuchen. Jede Empfehlung enthält Hinweise zu warum ein Problem für Sie sind möglicherweise zu der vorgeschlagenen Änderungen.

Sie können Schwerpunkte, die für Ihre Organisation wichtigsten und verfolgen Ihre Fortschritte mit einem Risiko frei und gesunde Umwelt.

Nachdem die Projektmappe hinzugefügt haben und eine Bewertung abgeschlossen, Zusammenfassung ist zeigt Informationen für Schwerpunktbereiche Armaturenbrett **AD-Bewertung** für die Infrastruktur in Ihrer Umgebung. In den folgenden Abschnitten wird beschrieben, wie die Informationen im Schaltpult **AD Bewertung** verwenden, wo Sie anzeigen und dann Aktionen empfohlen für Ihre Active Directory-Serverinfrastruktur.

![Bild nebeneinander SQL-Bewertung](./media/log-analytics-ad-assessment/ad-tile.png)

![Bild des SQL-Bewertung Dashboards](./media/log-analytics-ad-assessment/ad-assessment.png)


## <a name="installing-and-configuring-the-solution"></a>Installation und Konfiguration der Lösung
Anhand der folgenden Informationen zur Installation und Konfiguration der Lösung.

- Agents werden auf Domänencontrollern installiert, die Mitglieder der Domäne ausgewertet werden.
- Die Lösung Active Directory erfordert.NET Framework 4 auf jedem Computer, der ein OMS Agent installiert.
- OMS Arbeitsbereich mithilfe der Schritte im [Lösungskatalog Lösungen Protokollanalyse hinzufügen](log-analytics-add-solutions.md)fügen Sie die Lösung Active Directory hinzu.  Es ist keine weitere Konfiguration erforderlich.

    >[AZURE.NOTE] Nach dem Hinzufügen der Lösung Server mit die Datei AdvisorAssessment.exe hinzugefügt. Konfigurationsdaten lesen und an den OMS-Dienst in der Cloud zur Verarbeitung gesendet wird. Logik wird angewendet, um die empfangenen Daten und Cloud-Dienst die Daten.

## <a name="active-directory-assessment-data-collection-details"></a>Active Directory-Bewertung Einzelheiten zur Datensammlung

Active Directory-Bewertung sammelt WMI-Daten, Registrierung und Leistungsdaten mithilfe von Agents, die Sie aktiviert haben.

Die folgende Tabelle zeigt die Datenerfassungsmethoden Agents, ob Operations Manager (SCOM) erforderlich ist, und wie oft Daten von einem Agenten erfasst.

| Plattform | Direkte Agent | SCOM-Agenten | Azure-Speicher | SCOM erforderlich? | SCOM-Agentdaten per Verwaltungsgruppe | Häufigkeit der Collection |
|---|---|---|---|---|---|---|
|Windows|![Ja](./media/log-analytics-ad-assessment/oms-bullet-green.png)|![Ja](./media/log-analytics-ad-assessment/oms-bullet-green.png)|![Nein](./media/log-analytics-ad-assessment/oms-bullet-red.png)|   ![Nein](./media/log-analytics-ad-assessment/oms-bullet-red.png)|![Ja](./media/log-analytics-ad-assessment/oms-bullet-green.png)| 7 Tage|


## <a name="understanding-how-recommendations-are-prioritized"></a>Verstehen, wie Recommendations priorisiert werden

Jede Empfehlung ist einen Gewichtungswert zugewiesen, der die relative Wichtigkeit der Empfehlung gibt. Die zehn wichtigsten Vorschläge werden angezeigt.

### <a name="how-weights-are-calculated"></a>Berechnung von Gewicht

Gewichte werden Aggregatwerte basierend auf drei Faktoren:

- Die *Wahrscheinlichkeit* , dass ein Problem Probleme verursachen. Wahrscheinlichkeit entspricht einer größeren Gesamtfaktor für die Empfehlung.

- Die *Auswirkung* auf Ihre Organisation, wenn es ein Problem verursacht. Eine höhere Wirkung entspricht einer größeren Gesamtfaktor für die Empfehlung.

- Der *Aufwand* für die Empfehlung. Ein höherer Aufwand entspricht einer kleineren Gesamtfaktor für die Empfehlung.

Die Gewichtung für jede Empfehlung wird das Gesamtergebnis für jeden Schwerpunkt prozentual ausgedrückt. Beispielsweise wenn eine Empfehlung Schwerpunkt Sicherheit und Compliance einen Wert von 5 hat % erhöht Umsetzung dieser Empfehlung Ihre allgemeine Sicherheit und Compliance Bewertung von 5 %.

### <a name="focus-areas"></a>Schwerpunkte

**Sicherheits- und Compliance** - dieser Schwerpunkt Zeigt Vorschläge für Sicherheitsrisiken, Verstöße, Unternehmensrichtlinien und technischen, rechtlichen und behördlichen Auflagen.

**Verfügbarkeit und Business Continuity** - dieser Schwerpunkt Zeigt Vorschläge für Verfügbarkeit und Stabilität Ihrer Infrastruktur und Business Protection.

**Performance- und Skalierbarkeitslösungen** - dieser Schwerpunkt zeigt auf Ihrer Organisation zur IT-Infrastruktur wachsen, dass Ihre IT-Umgebung aktuelle Leistung erfüllt und reagieren zu Infrastruktur.

**Aktualisierung, Migration und Bereitstellung** – diese Schwerpunkt Zeigt Vorschläge können Sie die Aktualisierung, Migration und Bereitstellung von Active Directory in eine vorhandene Infrastruktur.

### <a name="should-you-aim-to-score-100-in-every-focus-area"></a>Sie sollten in jedem Schwerpunkt 100 % Faktor?

Nicht unbedingt. Die Informationen basieren auf das Wissen und die Erfahrung von Microsoft-Ingenieuren in Tausenden von Kundenbesuchen. Jedoch keine zwei Serverinfrastrukturen entsprechen und spezifische Vorschläge möglicherweise mehr oder weniger für Sie. Einige Sicherheitsaspekte möglicherweise z. B. weniger relevant, wenn die virtuellen Computer nicht mit dem Internet verbunden sind. Verfügbarkeit empfehlen weniger relevante Dienste möglicherweise, die ad-hoc-Datensammlung niedriger Priorität und reporting. Probleme für ältere Unternehmen möglicherweise wichtiger neu. Möglicherweise möchten identifizieren, welche Schwerpunkte der Prioritäten und sehen Sie sich Ihre Ergebnisse mit der Zeit ändern.

Jede Empfehlung enthält Anleitung, warum es wichtig ist. Diese Anleitung verwenden, Eignung implementieren die Empfehlung, angesichts der IT-Services und dem Bedarf Ihrer Organisation.

## <a name="use-assessment-focus-area-recommendations"></a>Bewertung Fokus Bereich Recommendations verwenden

Bevor Sie eine Lösung in OMS verwenden können, müssen Sie die Lösung installiert. Weitere Informationen über Installation, siehe [Lösungen Lösungskatalog Protokollanalyse hinzufügen](log-analytics-add-solutions.md). Nach der Installation, können Sie die Zusammenfassung der empfohlenen mit Bewertung nebeneinander auf der Seite Übersicht OMS anzeigen.

Zusammengefasste Compliance Assessments für Ihre Infrastruktur und Drilldown in Vorschläge anzeigen


### <a name="to-view-recommendations-for-a-focus-area-and-take-corrective-action"></a>Empfehlung für ein Schwerpunkt anzeigen und Maßnahmen

1. Klicken Sie auf der Seite **Übersicht** **Bewertung** für Ihre Serverinfrastruktur.
2. Auf der Seite **Bewertung** zusammenfassende Informationen in einem Blade Fokus Bereich, und klicken Sie auf eine Anzeige Vorschläge für diesen Fokusbereich.
3. Auf den Bereichsseiten Fokus priorisierten Empfehlungen für Ihre Umgebung angezeigt. Klicken Sie unter **Betroffene Objekte** zeigen Sie Details zu die Empfehlung Warum erfolgt auf Empfehlung.  
    ![Bild der Bewertung empfohlenen](./media/log-analytics-ad-assessment/ad-focus.png)
4. Sie nehmen Maßnahmen im **Empfohlenen Maßnahmen**vorgeschlagen. Wenn das Element behandelt, zeichnet höher Assessments, die ergriffen wurden, und nimmt Ihre Compliance-Faktor empfohlen. Korrigierte Elemente erscheinen als **Objekte übergeben**.

## <a name="ignore-recommendations"></a>Empfehlung ignorieren

Haben Sie Vorschläge, die Sie ignorieren möchten, können Sie eine Textdatei erstellen, mit dem OMS zu Recommendations in Ihre Ergebnisse angezeigt werden.

### <a name="to-identify-recommendations-that-you-will-ignore"></a>Vorschläge zu identifizieren, die Sie ignorieren

1.  Melden Sie sich bei Ihrem Arbeitsbereich und Protokollsuche öffnen. Verwenden Sie die folgende Abfrage Liste Vorschläge, die nicht für Computer in Ihrer Umgebung.

    ```
    Type=ADAssessmentRecommendation RecommendationResult=Failed | select  Computer, RecommendationId, Recommendation | sort  Computer
    ```

    Hier ist ein Screenshot zeigt Log Suchabfrage: ![Vorschläge fehlgeschlagen](./media/log-analytics-ad-assessment/ad-failed-recommendations.png)

2.  Wählen Sie die Empfehlung ignorieren möchten. Sie verwenden die Werte für RecommendationId in der nächsten Prozedur.


### <a name="to-create-and-use-an-ignorerecommendationstxt-text-file"></a>Erstellen und Verwenden einer Textdatei IgnoreRecommendations.txt

1.  Erstellen Sie eine Datei namens IgnoreRecommendations.txt.
2.  Fügen Sie oder geben Sie jede RecommendationId für jede Empfehlung möchten Protokollanalyse in einer separaten Zeile ignorieren und speichern und schließen Sie die Datei.
3.  Speichern Sie die Datei im folgenden Ordner auf jedem Computer OMS Empfehlung ignorieren soll.
    - Auf Computern mit Microsoft Monitoring Agent (direkt oder über Operations Manager verbunden) - *Systemlaufwerk*: \Programme\Microsoft Überwachung Agent\Agent
    - Auf dem Operations Manager-Verwaltungsserver - *Systemlaufwerk*: \Programme\Microsoft System Center 2012 R2\Operations Manager\Server

### <a name="to-verify-that-recommendations-are-ignored"></a>Überprüfen, ob Vorschläge ignoriert werden

Nach der nächsten Bewertung führt standardmäßig alle 7 Tage eingeplant, die angegebenen Informationen werden *ignoriert* gekennzeichnet und erscheint nicht auf dem Dashboard Bewertung.

1. Suchabfragen von Protokoll können Sie alle ignorierten empfohlenen Liste.

    ```
    Type=ADAssessmentRecommendation RecommendationResult=Ignored | select  Computer, RecommendationId, Recommendation | sort  Computer
    ```

2.  Wenn Sie später entscheiden, finden Sie unter Empfohlene ignoriert, IgnoreRecommendations.txt Dateien entfernen möchten, entfernen RecommendationIDs von ihnen.

## <a name="ad-assessment-solutions-faq"></a>AD Bewertung Solutions – häufig gestellte Fragen

*Wie oft führt eine Bewertung?*
- Die Bewertung wird alle 7 Tage ausgeführt.

*Gibt es eine Möglichkeit zu konfigurieren, wie oft die Bewertung ausgeführt wird?*
- Derzeit nicht.

*Ein weiterer Server für entdeckt, nachdem ich eine Lösung hinzugefügt haben, wird es werden überprüft?*
- Ja, sobald er erkannt wird, dann alle 7 Tage festgestellt.

*Wenn ein Server außer Betrieb genommen wird, wenn werden es aus Bewertung entfernt?*
- Wenn ein Server nicht Daten 3 Wochen, entfernt.

*Was ist der Name des Prozesses, der die Auflistung enthält?*
- AdvisorAssessment.exe

*Wie lange Daten gesammelt werden dauert?*
- Die aktuelle Auflistung auf dem Server dauert ca. 1 Stunde. Es kann auf Servern dauern, die eine große Anzahl von Active Directory-Server.

*Welche Daten werden gesammelt?*
- Welche Daten werden gesammelt:
    - WMI
    - Registrierung
    - Leistungsindikatoren

*Gibt es eine Möglichkeit, Daten konfigurieren?*
- Derzeit nicht.

*Warum werden angezeigt nur die 10 wichtigsten Vorschläge?*
- Anstatt Sie überwältigend abschließend Aufgaben, sollten auf priorisierte Recommendations zuerst konzentrieren. Nachdem Sie sich damit befassen, werden zusätzliche Informationen verfügbar. Wunsch eine detaillierte Liste finden alle empfohlenen Protokoll Suche angezeigt.

*Gibt es eine Möglichkeit, eine Empfehlung ignorieren?*
- Ja, siehe [ignorieren Recommendations](#ignore-recommendations) oben.


## <a name="next-steps"></a>Nächste Schritte

- Verwenden Sie [Log durchsucht Protokollanalyse](log-analytics-log-searches.md) detaillierte Anzeige Daten und Vorschläge.
