<properties
    pageTitle="Optimieren Sie Ihre Umgebung mit der Lösung SQL in Protokollanalyse | Microsoft Azure"
    description="Die Lösung SQL können Sie die Risiko- und Server Umgebungen in regelmäßigen Abständen bewerten."
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
    ms.date="08/11/2016"
    ms.author="banders"/>

# <a name="optimize-your-environment-with-the-sql-assessment-solution-in-log-analytics"></a>Optimieren Sie Ihre Umgebung mit der Lösung SQL in Protokollanalyse


Die Lösung SQL können Sie die Risiko- und Server Umgebungen in regelmäßigen Abständen bewerten. Dieser Artikel hilft Ihnen die Projektmappe installieren, damit Sie Gegenmaßnahmen für mögliche Probleme nutzen können.

Diese Lösung bietet eine priorisierte Liste der Vorschläge für die bereitgestellten Server-Infrastruktur. Die Empfehlung über sechs kategorisiert die schnell dennoch und Gegenmaßnahmen zu ergreifen.

Die Empfehlungen basieren auf das Wissen und die Erfahrung von Microsoft-Experten aus Tausenden von Kundenbesuchen. Jede Empfehlung enthält Hinweise zu warum ein Problem für Sie sind möglicherweise zu der vorgeschlagenen Änderungen.

Sie können Schwerpunkte, die für Ihre Organisation wichtigsten und verfolgen Ihre Fortschritte mit einem Risiko frei und gesunde Umwelt.

Nachdem die Projektmappe hinzugefügt haben und eine Bewertung abgeschlossen, Zusammenfassung ist zeigt Informationen für Schwerpunktbereiche Armaturenbrett **SQL-Bewertung** für die Infrastruktur in Ihrer Umgebung. In den folgenden Abschnitten wird beschrieben, wie die Informationen im Schaltpult **SQL Bewertung** verwenden, wo Sie anzeigen und dann Aktionen empfohlen für die SQL Server-Infrastruktur.

![Bild nebeneinander SQL-Bewertung](./media/log-analytics-sql-assessment/sql-assess-tile.png)

![Bild des SQL-Bewertung Dashboards](./media/log-analytics-sql-assessment/sql-assess-dash.png)

## <a name="installing-and-configuring-the-solution"></a>Installation und Konfiguration der Lösung
SQL-Bewertung funktioniert mit allen derzeit unterstützten Versionen von SQL Server für Standard, Developer und Enterprise Edition.

Anhand der folgenden Informationen zum Installieren und Konfigurieren der Lösung.

- Agenten müssen auf Servern installiert, die SQL Server installiert haben.
- SQL-Bewertung erfordert.NET Framework 4 auf jedem Computer, der ein OMS-Agent ist installiert.
- SQL-Bewertung der Operations Manager-Agent mit müssen Sie ein Konto Operations Manager Stammebene verwenden. Weitere Informationen finden Sie unter [Operations Manager ausführen als Konto für OMS](#operations-manager-run-as-accounts-for-oms) .

    >[AZURE.NOTE] MMA-Agent unterstützt keine Operations Manager Stammebene Konten.

- Hinzufügen der Lösung SQL in den OMS-Arbeitsbereich mithilfe der Schritte im [Lösungskatalog Lösungen Protokollanalyse hinzufügen](log-analytics-add-solutions.md). Es ist keine weitere Konfiguration erforderlich.

>[AZURE.NOTE] Nach dem Hinzufügen der Lösung Server mit die Datei AdvisorAssessment.exe hinzugefügt. Konfigurationsdaten lesen und an den OMS-Dienst in der Cloud zur Verarbeitung gesendet wird. Logik wird angewendet, um die empfangenen Daten und Cloud-Dienst die Daten.

## <a name="sql-assessment-data-collection-details"></a>Einzelheiten zur Datensammlung SQL-Bewertung

SQL-Bewertung sammelt WMI-Daten, Registrierung, Leistungsdaten und dynamische SQL Server Ergebnisse anzeigen mit Agenten, die Sie aktiviert haben.

Die folgende Tabelle zeigt die Datenerfassungsmethoden Agents, ob Operations Manager (SCOM) erforderlich ist, und wie oft Daten von einem Agenten erfasst.

| Plattform | Direkte Agent | SCOM-Agenten | Azure-Speicher | SCOM erforderlich? | SCOM-Agentdaten per Verwaltungsgruppe | Häufigkeit der Collection |
|---|---|---|---|---|---|---|
|Windows|![Ja](./media/log-analytics-sql-assessment/oms-bullet-green.png)|![Ja](./media/log-analytics-sql-assessment/oms-bullet-green.png)|![Nein](./media/log-analytics-sql-assessment/oms-bullet-red.png)|    ![Nein](./media/log-analytics-sql-assessment/oms-bullet-red.png)|![Ja](./media/log-analytics-sql-assessment/oms-bullet-green.png)|   7 Tage|

## <a name="operations-manager-run-as-accounts-for-oms"></a>Operations Manager ausführen als Konten für OMS

Protokollanalyse OMS verwendet die Operations Manager-Agent und Management-Gruppe zu sammeln und Senden von Daten an den OMS-Dienst. OMS baut auf managementpacks für Arbeitslasten zu Mehrwert-Services. Jede Arbeitslast erfordert Workload-spezifische Berechtigungen für managementpacks in einem anderen Sicherheitskontext wie ein Domänenkonto ausgeführt. Sie müssen Anmeldeinformationen bereitstellen, indem ein Operations Manager ausführen als Konto konfigurieren.

Mit den folgenden Informationen fest Operations Manager ausführen als Konto für SQL.

### <a name="set-the-run-as-account-for-sql-assessment"></a>Legen Sie das Konto ausführen als SQL-Bewertung

 Wenn Sie SQL Server Management Pack bereits verwenden, sollten Sie das Konto ausführen als verwenden.

#### <a name="to-configure-the-sql-run-as-account-in-the-operations-console"></a>So konfigurieren Sie SQL ausführen als Konto in der Betriebskonsole

>[AZURE.NOTE] Wenn Sie direkte OMS-Agent als SCOM-Agenten verwenden, wird das Management Pack immer im Kontext des lokalen Systemkontos ausgeführt. Überspringen Sie Schritte 1 bis 5 unten und führen Sie den T-SQL oder Powershell Beispiel Systemkonto als Benutzernamen angeben.

1. Öffnen Sie in Operations Manager die Betriebskonsole, und klicken Sie dann auf **Verwaltung**.

2. **Als Konfiguration ausführen**klicken Sie auf **Profile**und **OMS SQL Bewertung Ausführen als**Profil.

3. Klicken Sie auf der Seite **Run As Accounts** auf **Hinzufügen**.

4. Wählen Sie ein Windows ausführen als die Anmeldeinformationen für SQL Server enthält und klicken Sie auf **neu** erstellen.
    >[AZURE.NOTE] Ausführen als Kontenart muss Windows. Ausführen als Konto muss auch Teil der Gruppe der lokalen Administratoren auf allen Windows-Servern Hosten von SQL Server-Instanzen sein.

5. Klicken Sie auf **Speichern**.

6. Ändern Sie, und führen Sie das folgende T-SQL-Beispiel für jede SQL Server-Instanz erforderliche Mindestberechtigungen zum Ausführen als Konto auszuführenden SQL-Bewertung gewähren. Allerdings müssen Sie vorgehen, wenn eine Ausführung als Konto bereits Teil der Serverrolle Sysadmin in SQL Server-Instanzen ist.

```
---
    -- Replace <UserName> with the actual user name being used as Run As Account.
    USE master

    -- Create login for the user, comment this line if login is already created.
    CREATE LOGIN [<UserName>] FROM WINDOWS

    -- Grant permissions to user.
    GRANT VIEW SERVER STATE TO [<UserName>]
    GRANT VIEW ANY DEFINITION TO [<UserName>]
    GRANT VIEW ANY DATABASE TO [<UserName>]

    -- Add database user for all the databases on SQL Server Instance, this is required for connecting to individual databases.
    -- NOTE: This command must be run anytime new databases are added to SQL Server instances.
    EXEC sp_msforeachdb N'USE [?]; CREATE USER [<UserName>] FOR LOGIN [<UserName>];'

```
#### <a name="to-configure-the-sql-run-as-account-using-windows-powershell"></a>Mit Windows PowerShell SQL ausführen als Konto konfigurieren

Öffnen Sie ein PowerShell-Fenster und führen Sie das folgende Skript aus, nachdem Sie Ihre Daten aktualisiert haben:

```

    import-module OperationsManager
    New-SCOMManagementGroupConnection "<your management group name>"
     
    $profile = Get-SCOMRunAsProfile -DisplayName "OMS SQL Assessment Run As Profile"
    $account = Get-SCOMrunAsAccount | Where-Object {$_.Name -eq "<your run as account name>"}
    Set-SCOMRunAsProfile -Action "Add" -Profile $Profile -Account $Account
```

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

**Aktualisierung, Migration und Bereitstellung** – diese Schwerpunkt Zeigt Vorschläge zu aktualisieren, migrieren und SQL Server in eine vorhandene Infrastruktur bereitstellen.

**Operations and Monitoring** - dieser Schwerpunkt Zeigt Vorschläge zur Optimierung Ihres IT-Betriebs, Wartung implementieren und Leistung optimieren.

**Change and Configuration Management** - dieser Schwerpunkt Zeigt Vorschläge zu schützen Alltagsbetrieb, sicherzustellen, dass ändert nicht negativ beeinflussen Ihrer Infrastruktur Änderungskontrollverfahren, einrichten und zu Systemkonfigurationen überwachen.

### <a name="should-you-aim-to-score-100-in-every-focus-area"></a>Sie sollten in jedem Schwerpunkt 100 % Faktor?

Nicht unbedingt. Die Informationen basieren auf das Wissen und die Erfahrung von Microsoft-Ingenieuren in Tausenden von Kundenbesuchen. Jedoch keine zwei Serverinfrastrukturen entsprechen und spezifische Vorschläge möglicherweise mehr oder weniger für Sie. Einige Sicherheitsaspekte möglicherweise z. B. weniger relevant, wenn die virtuellen Computer nicht mit dem Internet verbunden sind. Verfügbarkeit empfehlen weniger relevante Dienste möglicherweise, die ad-hoc-Datensammlung niedriger Priorität und reporting. Probleme für ältere Unternehmen möglicherweise wichtiger neu. Möglicherweise möchten identifizieren, welche Schwerpunkte der Prioritäten und sehen Sie sich Ihre Ergebnisse mit der Zeit ändern.

Jede Empfehlung enthält Anleitung, warum es wichtig ist. Diese Anleitung verwenden, Eignung implementieren die Empfehlung, angesichts der IT-Services und dem Bedarf Ihrer Organisation.

## <a name="use-assessment-focus-area-recommendations"></a>Bewertung Fokus Bereich Recommendations verwenden

Bevor Sie eine Lösung in OMS verwenden können, müssen Sie die Lösung installiert. Weitere Informationen über Installation, siehe [Lösungen Lösungskatalog Protokollanalyse hinzufügen](log-analytics-add-solutions.md). Nach der Installation, können Sie die Zusammenfassung der empfohlenen mit SQL Bewertung nebeneinander auf der Seite Übersicht OMS anzeigen.

Zusammengefasste Compliance Assessments für Ihre Infrastruktur und Drilldown in Vorschläge anzeigen

### <a name="to-view-recommendations-for-a-focus-area-and-take-corrective-action"></a>Empfehlung für ein Schwerpunkt anzeigen und Maßnahmen

1. Klicken Sie auf der Seite **Übersicht** **SQL Bewertung** .
2. Auf der Seite **SQL Bewertung** zusammenfassende Informationen in einem Blade Bereich Fokus und klicken Sie auf eine Anzeige Vorschläge für diesen Bereich konzentrieren.
3. Auf den Bereichsseiten Fokus priorisierten Empfehlungen für Ihre Umgebung angezeigt. Klicken Sie unter **Betroffene Objekte** zeigen Sie Details zu die Empfehlung Warum erfolgt auf Empfehlung.  
    ![Bild SQL Bewertung empfohlenen](./media/log-analytics-sql-assessment/sql-assess-focus.png)
4. Sie nehmen Maßnahmen im **Empfohlenen Maßnahmen**vorgeschlagen. Wenn das Element behandelt, zeichnet höher Assessments, die ergriffen wurden, und nimmt Ihre Compliance-Faktor empfohlen. Korrigierte Elemente erscheinen als **Objekte übergeben**.

## <a name="ignore-recommendations"></a>Empfehlung ignorieren

Haben Sie Vorschläge, die Sie ignorieren möchten, können Sie eine Textdatei erstellen, mit dem OMS zu Recommendations in Ihre Ergebnisse angezeigt werden.

### <a name="to-identify-recommendations-that-you-will-ignore"></a>Vorschläge zu identifizieren, die Sie ignorieren

1.  Melden Sie sich bei Ihrem Arbeitsbereich und Protokollsuche öffnen. Verwenden Sie die folgende Abfrage Liste Vorschläge, die nicht für Computer in Ihrer Umgebung.

    ```
    Type=SQLAssessmentRecommendation RecommendationResult=Failed | select  Computer, RecommendationId, Recommendation | sort  Computer
    ```

    Hier ist ein Screenshot zeigt Log Suchabfrage: ![Vorschläge fehlgeschlagen](./media/log-analytics-sql-assessment/sql-assess-failed-recommendations.png)

2.  Wählen Sie die Empfehlung ignorieren möchten. Sie verwenden die Werte für RecommendationId in der nächsten Prozedur.


### <a name="to-create-and-use-an-ignorerecommendationstxt-text-file"></a>Erstellen und Verwenden einer Textdatei IgnoreRecommendations.txt

1.  Erstellen Sie eine Datei namens IgnoreRecommendations.txt.
2.  Fügen Sie oder geben Sie jede RecommendationId für jede Empfehlung möchten OMS in einer separaten Zeile ignorieren und speichern und schließen Sie die Datei.
3.  Speichern Sie die Datei im folgenden Ordner auf jedem Computer OMS Empfehlung ignorieren soll.
    - Auf Computern mit Microsoft Monitoring Agent (direkt oder über Operations Manager verbunden) - *Systemlaufwerk*: \Programme\Microsoft Überwachung Agent\Agent
    - Auf dem Operations Manager-Verwaltungsserver - *Systemlaufwerk*: \Programme\Microsoft System Center 2012 R2\Operations Manager\Server

### <a name="to-verify-that-recommendations-are-ignored"></a>Überprüfen, ob Vorschläge ignoriert werden

1.  Nach der nächsten Bewertung führt standardmäßig alle 7 Tage eingeplant, die angegebenen Informationen werden ignoriert gekennzeichnet und erscheint nicht auf dem Dashboard Bewertung.
2.  Suchabfragen von Protokoll können Sie alle ignorierten empfohlenen Liste.

    ```
    Type=SQLAssessmentRecommendation RecommendationResult=Ignored | select  Computer, RecommendationId, Recommendation | sort  Computer
    ```
3.  Wenn Sie später entscheiden, finden Sie unter Empfohlene ignoriert, IgnoreRecommendations.txt Dateien entfernen möchten, entfernen RecommendationIDs von ihnen.

## <a name="sql-assessment-solution-faq"></a>SQL Lösung häufig gestellte Fragen

*Wie oft führt eine Bewertung?*
- Die Bewertung wird alle 7 Tage ausgeführt.

*Gibt es eine Möglichkeit zu konfigurieren, wie oft die Bewertung ausgeführt wird?*
- Derzeit nicht.

*Ein weiterer Server für entdeckt, nachdem ich die Lösung SQL hinzugefügt, wird bewertet?*
- Ja, sobald er erkannt wird, dann alle 7 Tage festgestellt.

*Wenn ein Server außer Betrieb genommen wird, wenn werden es aus Bewertung entfernt?*
- Wenn ein Server nicht Daten 3 Wochen, entfernt.

*Was ist der Name des Prozesses, der die Auflistung enthält?*
- AdvisorAssessment.exe

*Wie lange Daten gesammelt werden dauert?*
- Die aktuelle Auflistung auf dem Server dauert ca. 1 Stunde. Es kann auf Servern dauern, die eine große von SQL-Instanzen oder Datenbanken Anzahl.

*Welche Daten werden gesammelt?*
- Welche Daten werden gesammelt:
    - WMI
    - Registrierung
    - Leistungsindikatoren
    - SQL dynamische Verwaltungsansichten (DMV).

*Gibt es eine Möglichkeit, Daten konfigurieren?*
- Derzeit nicht.

*Warum muss ich eine Ausführung als Konto konfigurieren?*
- Für SQL Server sind eine kleine Anzahl von SQL-Abfragen ausführen. Damit diese Ausführung muss eine Ausführung als Konto mit Berechtigungen für SQL SERVER ANSICHTSZUSTAND verwendet.  Darüber hinaus sind zur Abfrage von WMI lokale Administratorrechte erforderlich.

*Warum werden angezeigt nur die 10 wichtigsten Vorschläge?*
- Anstatt Sie überwältigend abschließend Aufgaben, sollten auf priorisierte Recommendations zuerst konzentrieren. Nachdem Sie sich damit befassen, werden zusätzliche Informationen verfügbar. Wunsch eine detaillierte Liste finden alle empfohlenen OMS Protokoll suchen angezeigt.

*Gibt es eine Möglichkeit, eine Empfehlung ignorieren?*
- Ja, siehe [ignorieren Recommendations](#ignore-recommendations) oben.



## <a name="next-steps"></a>Nächste Schritte

- [Suche Protokolle](log-analytics-log-searches.md) detaillierte SQL Daten und Vorschläge anzeigen.
