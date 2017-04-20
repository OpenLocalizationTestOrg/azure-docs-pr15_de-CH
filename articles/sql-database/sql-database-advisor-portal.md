<properties 
   pageTitle="Azure SQL-Datenbank Berater mit Azure-Portal | Microsoft Azure" 
   description="Können Sie Azure SQL-Datenbank Advisor in Azure-Portal überprüfen und Vorschläge für die SQL-Datenbanken, die aktuelle abfrageleistung verbessern können." 
   services="sql-database" 
   documentationCenter="" 
   authors="stevestein" 
   manager="jhubbard" 
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management" 
   ms.date="09/30/2016"
   ms.author="sstein"/>

# <a name="sql-database-advisor-using-the-azure-portal"></a>SQL Datenbank-Berater mit der Azure-portal

> [AZURE.SELECTOR]
- [SQL Datenbank Advisor (Übersicht)](sql-database-advisor.md)
- [Portal](sql-database-advisor-portal.md)

Können Sie Azure SQL-Datenbank Advisor in Azure-Portal überprüfen und Vorschläge für die SQL-Datenbanken, die aktuelle abfrageleistung verbessern können.

## <a name="viewing-recommendations"></a>Vorschläge anzeigen

Empfohlene Seite werden Empfehlungen basierend auf der potenziellen Auswirkung Leistung angezeigt. Sie können auch den Status von Verlaufsdaten Operationen anzeigen. Wählen Sie eine Empfehlung oder Status, um weitere Details anzuzeigen.

Anzeigen und Vorschläge gelten, benötigen Sie die Berechtigungen korrekt [Rollenbasierte Zugriffskontrolle](../active-directory/role-based-access-control-configure.md) in Azure. **Reader** **SQL DB** Teilnehmerberechtigungen müssen Ansicht Recommendations und **Besitzer** **SQL DB** Teilnehmerberechtigungen auszuführende Aktionen erforderlich; Erstellen oder Löschen von Indizes und indexerstellung Abbrechen.

1. Mit der [Azure-Portal](https://portal.azure.com/)anmelden.
2. Klicken Sie auf **Weitere Dienste** > **SQL-Datenbanken**, und wählen Sie Ihre Datenbank.
5. Klicken Sie auf **Leistung Empfehlung** um verfügbaren Vorschläge für die ausgewählte Datenbank anzuzeigen.

> [AZURE.NOTE] Um Vorschläge eine Datenbank muss über einen Tag der Verwendung und Aktivitäten werden müssen. Außerdem müssen einige konsistent sein. SQL Datenbank Advisor kann leichter für zufällige Tupfen Aktivitätsschübe können konsistente Abfragemuster optimieren. Wenn Empfehlung nicht verfügbar sind, sollten die Seite **Leistung Empfehlung** eine Nachricht Warum bereitstellen.

![Empfehlung](./media/sql-database-advisor-portal/recommendations.png)

Hier ist ein Beispiel der Empfehlung im Azure-Portal "Schema Problem beheben".

![Schema-Problem beheben](./media/sql-database-advisor-portal/sql-database-advisor-schema-issue.png)

Empfohlene sortiert ihre potenzielle Auswirkung auf die folgenden vier Kategorien:

| Auswirkung | Beschreibung |
| :--- | :--- |
| Hoch | Hohe Empfehlung sollte die wichtigsten Leistungseinbußen. |
| Mittel | Mittlere Auswirkung Empfehlung sollte die Leistung verbessert, aber nicht wesentlich. |
| Niedrig | Geringe Auswirkung Empfehlung sollte eine bessere Leistung als ohne jedoch verbessert möglicherweise nicht erheblich. 


### <a name="removing-recommendations-from-the-list"></a>Vorschläge entfernen aus der Liste

Wenn die Liste der empfohlenen Elemente, die Sie aus der Liste entfernen möchten enthält, können Sie die Empfehlung verwerfen:

1. Wählen Sie in der Liste der **empfohlenen**Empfehlung.
2. Klicken Sie auf die **Details** **zu verwerfen** .


Bei Bedarf können Sie gelöschte Elemente an der **empfohlenen** Liste hinzufügen:

1. Klicken Sie auf die **Vorschläge** auf **Ansicht gelöscht**.
1. Wählen Sie einen verworfenen um seine Details anzuzeigen.
1. Klicken Sie optional auf **Rückgängig: Löschen** , um den Index zu der Hauptliste **empfohlenen**hinzuzufügen.



## <a name="applying-recommendations"></a>Empfohlene anwenden

SQL Datenbank Advisor bietet vollständige Kontrolle über wie empfohlen werden mithilfe der folgenden drei Optionen aktiviert: 

- Eine einzelne Empfehlung gleichzeitig zuweisen.
- Aktivieren die Berater automatisch Vorschläge gelten (derzeit auf Index-Empfehlung gilt).
- Um eine Empfehlung manuell zu implementieren, führen Sie das empfohlene T-SQL-Skript für Ihre Datenbank.

Wählen Sie eine Empfehlung zu der Detailansicht **Skript anzeigen** , um Details zu genau wie die Empfehlung erstellt wird.

Die Datenbank bleibt online während der Advisor - Empfehlung gilt mit SQL Datenbank Advisor nie eine Datenbank offline geschaltet.

### <a name="apply-an-individual-recommendation"></a>Eine einzelne Empfehlung anwenden

Sie können überprüfen und akzeptieren eine Empfehlung zu einem Zeitpunkt.

1. Klicken Sie auf das Blade **Recommendations** auf Empfehlung.
2. Klicken Sie auf die **Informationen** **anwenden**.

    ![Empfehlung anwenden](./media/sql-database-advisor-portal/apply.png)

### <a name="enable-automatic-index-management"></a>Aktivieren Sie automatische Indexverwaltungsservern

SQL Datenbank Berater Recommendations automatisch implementieren lassen. Empfehlung verfügbar sind, werden sie automatisch angewendet. Wie werden mit allen Index ist die Leistungseinbußen negativ vom Dienst verwaltet die Empfehlung zurückgesetzt.

1. Klicken Sie auf das Blade **Recommendations** **automatisieren**:

    ![Berater-Einstellung](./media/sql-database-advisor-portal/settings.png)

2. Legen Sie den Ratgeber automatisch die Indizes **Erstellen** oder **Löschen** :

    ![Indizes empfohlen](./media/sql-database-advisor-portal/automation.png)


### <a name="manually-run-the-recommended-t-sql-script"></a>Empfohlene T-SQL-Skript ausführen manuell

Wählen Sie Empfehlung, und klicken Sie dann auf **Skript anzeigen**. Skript für die Datenbank die Empfehlung manuell anwenden.

*Indizes, die manuell ausgeführt werden, nicht überwacht und überprüft für Leistungseinbußen durch den Dienst* vorgeschlagen wird, nach der Erstellung überprüfen sie bieten Leistung anpassen oder löschen diese Indizes überwachen. Einzelheiten zum Erstellen von Indizes finden Sie unter [CREATE INDEX (Transact-SQL)](https://msdn.microsoft.com/library/ms188783.aspx).


### <a name="canceling-recommendations"></a>Empfohlene Abbrechen

Vorschläge, die im Status **Pending** **Überprüfen**oder **Erfolg** können abgebrochen werden. Vorschläge mit dem Status der **Ausführung** können nicht abgebrochen werden.

1. Wählen Sie eine Empfehlung **Historie optimieren** Blade **Recommendations Details** zu öffnen.
2. Klicken Sie auf **Abbrechen** , um der Anwendung der Empfehlung abzubrechen.



## <a name="monitoring-operations"></a>Überwachung

Anwenden einer Empfehlung möglicherweise nicht sofort ausgeführt werden. Das Portal enthält Details zum Status der Empfehlung Operationen. Es folgen mögliche Zustände, die ein Index in:

| Status | Beschreibung |
| :--- | :--- |
| Ausstehend | Wenden Sie Empfehlung wurde und für die Ausführung geplant an. |
| Ausführen | Die Empfehlung gilt. |
| Erfolg | Empfehlung wurde erfolgreich angewendet. |
| Fehler | Fehler bei der Anwendung der Empfehlung. Dies ist ein vorübergehendes Problem oder ein Schema der Tabelle ändern und das Skript ist nicht mehr gültig. |
| Zurücksetzen | Die Empfehlung wurde jedoch wurde als nicht-leistungsfähigen und automatisch zurückgesetzt. |
| Zurückgesetzt | Die Empfehlung wurde zurückgesetzt. |

Klicken Sie auf Empfehlung im Prozess in der Liste um weitere Details anzuzeigen:

![Indizes empfohlen](./media/sql-database-advisor-portal/operations.png)


### <a name="reverting-a-recommendation"></a>Wiederherstellen einer Empfehlung

Wenn die Berater verwendet, um die Empfehlung kehrt (d. h. das T-SQL-Skript nicht manuell ausgeführt wurde) es automatisch es findet die Leistungseinbußen negativ sein. Wenn aus irgendeinem Grund Sie lediglich eine Empfehlung wiederherstellen möchten können Sie Folgendes:


1. Wählen Sie erfolgreich angewendete Empfehlung **Tuning Geschichte** .
2. Klicken Sie auf die **Empfehlung Details** auf **Wiederherstellen** .

![Indizes empfohlen](./media/sql-database-advisor-portal/details.png)


## <a name="monitoring-performance-impact-of-index-recommendations"></a>Überwachen der Leistung durch Index recommendations

Nach erfolgreich Recommendations (derzeit index Operationen und Parametrisieren von Abfragen-Empfehlung) Empfehlung Details Blade in [Query Performance Einblicke](sql-database-query-performance.md) öffnen und Anzeigen der Leistung durch Ihre oberen Abfragen **Abfrage Einblicke** anklicken.

![Monitor-Leistungseinbußen](./media/sql-database-advisor-portal/query-insights.png)



## <a name="summary"></a>Zusammenfassung

SQL Datenbank Advisor Leitfaden zur Verbesserung der Leistung der SQL-Datenbank. Mit T-SQL-Skripts unterstützt einzelne sowie vollautomatische (derzeit nur Index), Berater hilfreich in Ihrer Datenbank optimieren und letztendlich die Leistung verbessert.



## <a name="next-steps"></a>Nächste Schritte

Überwachen der empfohlenen und Weiter, um die Leistung zu optimieren. Datenbanken sind dynamisch und ständig ändern. SQL Datenbank Advisor weiterhin überwachen und Vorschläge, die möglicherweise die Datenbank-Performance verbessern können. 

 - Überblick über SQL Datenbank Advisor finden Sie unter [SQL Datenbank Advisor](sql-database-advisor.md) .
 - [Abfrage Leistung Einblicke](sql-database-query-performance.md) Informationen zum Anzeigen der Leistung durch Ihre oberen Abfragen anzeigen

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Abfrage laden](https://msdn.microsoft.com/library/dn817826.aspx)
- [INDEX ERSTELLEN](https://msdn.microsoft.com/library/ms188783.aspx)
- [Rollenbasierte Zugriffskontrolle](../active-directory/role-based-access-control-configure.md)






