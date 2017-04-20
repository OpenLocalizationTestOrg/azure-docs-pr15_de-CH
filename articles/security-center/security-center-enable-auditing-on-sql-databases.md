<properties
   pageTitle="Aktivieren der Überwachung auf SQL-Datenbanken in Azure Security Center | Microsoft Azure"
   description="Dieses Dokument veranschaulicht die **Überwachung Datenbanken SQL**Azure Security Center-Empfehlung implementiert."
   services="security-center"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/29/2016"
   ms.author="terrylan"/>

# <a name="enable-auditing-on-sql-databases-in-azure-security-center"></a>Aktivieren der Überwachung auf SQL-Datenbanken in Azure Security Center

Azure Security Center wird empfohlen, die Funktion für die Überwachung für alle SQL-Datenbanken, wenn die Überwachung nicht bereits aktiviert ist. Überwachung können Sie die Einhaltung behördlicher Auflagen und Datenbankaktivität verstehen Einblick in Diskrepanzen und Anomalien, die Unternehmen hinweisen oder Verstößen Sicherheit.

Nachdem die Überwachung aktiviert haben können Sie Erkennung Einstellungen und e-Mails Sicherheitshinweise erhalten konfigurieren. Erkennung erkennt außergewöhnlicher Datenbankaktivitäten potenzielle Sicherheitsrisiken für die Datenbank angibt. Dadurch erkennen und reagieren auf mögliche Gefahren an.

Diese Empfehlung gilt für den Azure SQL-Dienst. SQL auf die virtuellen Computer nicht eingeschlossen werden.

> [AZURE.NOTE] Dieses Dokument stellt den Dienst mithilfe einer beispielbereitstellung.  Dies ist nicht Schritt.

## <a name="implement-the-recommendation"></a>Implementieren der Empfehlung

1. Wählen Sie Blade **Recommendations** **Auf SQL-Datenbanken Überwachung aktivieren**.  **Auf SQL-Datenbanken Überwachung aktivieren** Blade wird geöffnet.
![Aktivieren der Überwachung auf SQL-Datenbanken][1]

2. Wählen Sie eine SQL-Datenbank überwachen. **Überwachung & Bedrohung Erkennung** Blade wird geöffnet.
![Überwachung und Bedrohung][2]
3. **Überwachung & Bedrohung Erkennung** Blade wählen Sie **auf** unter **Überwachung**.
![Überwachung und Bedrohung aktivieren][3]


5. Gehen Sie in [Erste Schritte mit SQL Datenbank Erkennung](../sql-database/sql-database-threat-detection-get-started.md) aktivieren und Konfigurieren der Erkennung und Konfigurieren der e-Mails, die Sicherheitshinweise bei außergewöhnlichen Aktivitäten erhalten.

## <a name="see-also"></a>Siehe auch

Dieser Artikel wurde gezeigt, wie das Sicherheitscenter Empfehlung implementieren "Aktivieren Sie die Überwachung auf SQL-Datenbanken." Um weitere Informationen zum Sichern Ihrer SQL-Datenbank finden Sie hier:

- [Die SQL-Datenbank sichern](../sql-database/sql-database-security.md)

Erfahren Sie mehr über das Sicherheitscenter finden Sie hier:

- [Festlegen von Sicherheitsrichtlinien in Azure Security Center](security-center-policies.md) – Informationen zum Konfigurieren von Sicherheitsrichtlinien für Ihren Azure-Abonnements und Ressourcengruppen.
- [Verwalten von Sicherheitsaspekte in Azure Security Center](security-center-recommendations.md) – erfahren Sie, wie Recommendations Azure Ressourcen schützen.
- [Security Health monitoring in Azure Security Center](security-center-monitoring.md) – erfahren Sie, wie Ihre Azure Ressourcen überwachen.
- [Verwalten von und reagieren auf Sicherheitshinweise in Azure Security Center](security-center-managing-and-responding-alerts.md) - Informationen verwalten und Sicherheitshinweise auf.
- [Überwachung Partner Solutions mit Azure Security Center](security-center-partner-solutions.md) erfahren Sie den Status Ihrer Lösungen Partner überwachen.
- [Azure Security Center FAQ](security-center-faq.md) - finden Sie häufig gestellte Fragen zur Verwendung des Dienstes.
- [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) - Sicherheit von Azure-Neuigkeiten und Informationen.

<!--Image references-->
[1]: ./media/security-center-enable-auditing-on-sql-databases/enable-auditing-on-sql-databases.png
[2]:./media/security-center-enable-auditing-on-sql-databases/auditing-threat-detection.png
[3]: ./media/security-center-enable-auditing-on-sql-databases/auditing-threat-detection-blade.png
