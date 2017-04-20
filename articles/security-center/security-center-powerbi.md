<properties
   pageTitle="Einblicke aus Azure Security Center mit Power BI | Microsoft Azure"
   description="Azure Security Center Power BI Content Pack erleichtert Sicherheitshinweise Recommendations finden Ressourcen angegriffen und trends, basierend auf einem Dataset für die Berichte erstellt."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/22/2016"
   ms.author="yurid"/>

# <a name="get-insights-from-azure-security-center-data-with-power-bi"></a>Einblicke aus Azure Security Center mit Power BI
[Power BI-Dashboard](http://aka.ms/azure-security-center-power-bi) für Azure Security Center können Sie visualisieren, analysieren und Vorschläge und Sicherheitshinweise aus Ihrem mobile Gerät einschließlich filtern. Verwenden des Power BI-Dashboards Trends und Muster - Ansicht Sicherheitshinweise Ressource oder IP-Quelladresse und unadressierte Sicherheitsrisiken Ressource oder ALTER anzugreifen. 

Sie können auch Sicherheitscenter Recommendations und Sicherheitshinweise mit anderen Daten auf interessante Weise stampfen beispielsweise anhand von [Azure Prüfprotokolle](https://powerbi.microsoft.com/blog/monitor-azure-audit-logs-with-power-bi/) und [Überwachung zu Azure SQL-Datenbank](https://powerbi.microsoft.com/blog/monitor-your-azure-sql-database-auditing-activity-with-power-bi/). Beide bieten Power BI-Dashboards und können Sie auch diese Daten nach Excel exportieren, um einfache Berichte über den Sicherheitsstatus Ihrer Cloud-Ressourcen.

##<a name="using-azure-security-center-dashboard-to-access-power-bi"></a>Mithilfe von Azure Security Center Dashboard auf Power BI
Azure Security Center-Dashboard können Sie Power BI-Berichte zugreifen. Gehen Sie zum Ausführen dieser Aufgabe: 

1. Klicken Sie im Dashboard **Azure Security Center** **Durchsuchen in Power BI** .

    ![Verbinden Sie mit Azure Security Center mit Power BI](./media/security-center-powerbi/security-center-powerbi-fig1-new10.png) 

2. **Explorer in Power BI** -Blade öffnet auf der rechten Seite, wie im folgenden Bild gezeigt:

    ![Verbinden Sie mit Azure Security Center mit Power BI](./media/security-center-powerbi/security-center-powerbi-fig1-new2.png)

3. Wenn Sie zum ersten Mal Power BI-Dashboard erstellen, können Sie eine der folgenden Optionen im **Explorer in Power BI** -Blade: 

    - **Security Insights Dashboard**: Wählen Sie diese Option Wenn Sie ein Dashboard erstellen Sicherheitsstatus, Threads und Erkennung. Diese Option ist ein häufiger DevOps Rolle, die ihren Schutzstatus analysieren und Alarme über Abonnements gefunden.
    - **Policy Management Dashboard**: Wählen Sie diese Option Wenn Sie zu Verwaltung und Umsetzung von Richtlinien.  Diese Option wird ein häufiger für zentrale IT, mehr Governance ausgerichtet sind. Diesem Dashboard können sie um Sichtbarkeit und Erkenntnisse zur Einhaltung der Richtlinien Sicherheit in ihrer Organisation zu erhalten.
    - Haben Sie bereits ein Power BI-Dashboard, klicken Sie auf **das aktuelle Power BI-Dashboard**.

4. Klicken Sie beispielsweise auf **Security Insights Dashboard** . Ist dies das erste Mal, erstellen Sie ein Power BI-Dashboard Sicherheitscenter, die Sie aufgefordert werden, das Content Pack installieren. Klicken Sie auf **Get** im Fenster **Pakete für Power BI** wie im folgenden Bild gezeigt:

    ![Azure Security Center Security Insights-dashboard](./media/security-center-powerbi/security-center-powerbi-fig1-new3.png)

5. **Mit Azure Security Center Security Insights** -Fenster angezeigt. Sicherstellen Sie **die Authentifizierungsmethode** **oAuth2** wie folgt und **Anmelden** klicken.
    
    ![Authentifizierung](./media/security-center-powerbi/security-center-powerbi-fig1-new4.png)

6. Möglicherweise müssen Sie erneut mit der Azure-Anmeldeinformationen authentifizieren. Nach der Authentifizierung des Dashboards erstellt werden. Erstelltes Dashboard sehen Sie einen Bericht mit ähnlicher Struktur, wie im folgenden Bild gezeigt:

    ![Power BI-Dashboard](./media/security-center-powerbi/security-center-powerbi-fig1-new5.png)


> [AZURE.NOTE] Aktualisieren des Berichts soll täglich statt. Bei ein Fehlers auf diese Aktualisierung auftreten lesen Sie weitere Informationen zur Problembehandlung bei [Aktualisieren Probleme mit Azure Security Center Power BI](https://blogs.msdn.microsoft.com/azuresecurity/2016/04/07/azure-security-center-power-bi-refresh-fails/).

Hier sehen Sie die Anzahl der Sicherheitshinweise und Vorschläge sowie die Anzahl der VMs, Azure SQL-Datenbanken und Netzwerkressourcen von Azure Security Center.

Eine Verknüpfung zu Azure Security Center leitet Sie Azure-Portal. Die Diagramme erleichtern die Sicherheitsaspekte und Warnungen, einschließlich Informationen zu visualisieren:

- Ressourcenstatus-Sicherheit
- Ausstehende Vorschläge
- VM-Empfehlung
- Warnung mit der Zeit
- Angegriffenen Ressourcen
- Angegriffenen IPs

Hinter jedem Diagramm sind zusätzliche Informationen. Wählen Sie eine Kachel um weitere Informationen anzuzeigen. Beispielsweise zeigt die **Ressource Sicherheitsstatus** Kachel Sie zusätzliche Details über ausstehende Vorschläge von Ressourcen wie im folgenden Bild gezeigt:

![Empfehlung](./media/security-center-powerbi/security-center-powerbi-fig1-new6.png)

Wenn Sie auf eine Zeile in diesem Diagramm klicken, werden andere grau, und Sie nur auf die gewählte. Dem Dashboard klicken Sie **Azure Security Center** unter der Option **Dashboards** auf der linken Seite.

> [AZURE.NOTE] Wenn Sie Berichte anpassen, indem Sie zusätzliche Felder hinzufügen oder vorhandene Grafik ändern möchten, bearbeiten Sie den Bericht. Weitere Informationen finden Sie in der [Interaktion mit einem Bericht in Power BI-Ansicht bearbeiten](https://powerbi.microsoft.com/documentation/powerbi-service-interact-with-a-report-in-editing-view/) .

**Alerts über Zeit und Ressourcen angegriffen** und **Angreifer IPs** Kacheln haben ähnliche Ausgabe jedes einzelnen anklicken. Dies geschieht, da der Bericht über die drei Variablen Informationen und **Ressourcen angegriffen** wie im folgenden Bild gezeigt wird:

![Ressourcen angegriffen](./media/security-center-powerbi/security-center-powerbi-fig1-new7.png)

An dieser Stelle können Sie diesen Bericht speichern, Drucken auch mithilfe der Optionen im Menü **Datei** auf Web veröffentlichen.

![Dateimenü](./media/security-center-powerbi/security-center-powerbi-fig8.png)

## <a name="exploring-your-azure-security-center-data-with-power-bi-services"></a>Erkunden Ihre Azure Security Center Daten mit Power BI

Der [Power BI Content Pack Services](https://msit.powerbi.com/groups/me/getdata/services) in Power BI, und führen Sie folgende Schritte aus:

1. **Inhaltspaket für Power BI** -Fenster sehen Sie zwei Optionen, wie unten dargestellt.

    ![Inhaltspaket für Power BI](./media/security-center-powerbi/security-center-powerbi-fig1-new.png)

    >[AZURE.NOTE] Wenn den ersten Teil dieses Artikels bereits ausgeführt, sehen Sie Azure Security Center Policy Management ist nur eine Option.

2. **Klicken Sie für dieses Beispiel auf der Kachel **Azure Security Center Policy Management** .**

3. Im Fenster **Verbinden Azure Security Center Policy Management** unbedingt **oAuth2** **Authentifizierungsmethode** Dropdown-siehe unten klicken und dann auf **Anmelden** .

    ![Policy-Management-Fenster](./media/security-center-powerbi/security-center-powerbi-fig1-new8.png)

4. Sie werden auf einer Authentifizierungsseite umgeleitet sollten Sie Anmeldeinformationen eingeben, die Sie mit Azure Security Center herstellen. Nach Abschluss des Authentifizierungsvorgangs startet Power BI Datenimport Berichte erstellen. Während dieser Zeit sehen Sie die Meldung in der rechten Ecke des Browsers:

    ![Verbinden Sie mit Azure Security Center mit Power BI](./media/security-center-powerbi/security-center-powerbi-fig4.png)

    >[AZURE.NOTE] Wenn das Dashboard zum ersten Mal erstellt wird dauert länger, vor allem für Szenarien es mehrere Abonnements kommen. 

5. Nach Abschluss des Vorgangs wird das Azure Security Center Power BI-Dashboard **Richtlinienmanagement** ähnlich der unten im Bericht geladen:

    ![Policy Management-dashboard](./media/security-center-powerbi/security-center-powerbi-fig1-new9.png)

## <a name="see-also"></a>Siehe auch
In diesem Dokument haben Sie gelernt, wie Sie Power BI in Azure Security Center. Erfahren Sie mehr über Azure Security Center finden Sie hier:

- [Planen von Azure Security Center und Operations Guide](security-center-planning-and-operations-guide.md) – lernen Azure Security Center Annahme planen.
- [Festlegen von Sicherheitsrichtlinien in Azure Security Center](security-center-policies.md) – Informationen zum Konfigurieren von Sicherheit in Azure Security Center
- [Verwalten von und reagieren auf Sicherheitshinweise in Azure Security Center](security-center-managing-and-responding-alerts.md) – Informationen zum Verwalten und Sicherheitshinweisen auf
- [Häufig gestellte Fragen zur Azure Security Center](security-center-faq.md) – häufig gestellte Fragen zur Verwendung des Dienstes suchen
- [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) – Suchen über Azure Sicherheits- und Compliance-Blogbeiträge
