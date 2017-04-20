<properties
   pageTitle="Aktivieren der Überwachung für SQL Server in Azure Security Center | Microsoft Azure"
   description="Dieses Dokument veranschaulicht der Azure Security Center Empfehlung **SQL Server überwachen**."
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

# <a name="enable-auditing-on-sql-servers-in-azure-security-center"></a>Aktivieren der Überwachung für SQL Server in Azure Security Center

Azure Security Center wird empfehlen, dass Sie Überwachung für alle Datenbanken auf Ihrem SQL Azure-Server, wenn die Überwachung nicht bereits aktiviert ist. Überwachung können Sie die Einhaltung behördlicher Auflagen und Datenbankaktivität verstehen Einblick in Diskrepanzen und Anomalien, die Unternehmen hinweisen oder Verstößen Sicherheit.

Nachdem die Überwachung aktiviert haben können Sie Erkennung Einstellungen und e-Mails Sicherheitshinweise erhalten konfigurieren. Erkennung erkennt außergewöhnlicher Datenbankaktivitäten potenzielle Sicherheitsrisiken für die Datenbank angibt. Dadurch erkennen und reagieren auf mögliche Gefahren an.

Diese Empfehlung gilt für den Azure SQL-Dienst. er enthalten nicht auf die virtuellen Computer in Azure Infrastructure Services (Azure IaaS) SQL Server.

> [AZURE.NOTE] Dieses Dokument stellt den Dienst mithilfe einer beispielbereitstellung.  Dies ist nicht Schritt.

## <a name="implement-the-recommendation"></a>Implementieren der Empfehlung

1. Blade **Recommendations** wählen Sie **SQL Server überwachen können**.  **Überwachung aktivieren für SQL Server** -Blade wird geöffnet.
![Aktivieren der Überwachung für SQL Server][1]

2. Wählen Sie einen SQL Server überwachen. Blade **Überwachung Settings** wird geöffnet.
![Überwachungsrichtlinien][2]
3. Wählen Sie Blatt **Überwachung Einstellungen** **auf** unter **Überwachung**.
![Überwachung aktivieren][3]

4. Führen Sie die Schritte [beginnen mit der SQL Datenbank überwachen](../sql-database/sql-database-auditing-get-started.md) , konfigurieren, in dem die Überwachungsprotokolle gespeichert werden. Das Abonnement Speicherkonto für die Datensammlung ist das Standardkonto Speicher.

5. Gehen Sie in [Erste Schritte mit SQL Datenbank Erkennung](../sql-database/sql-database-threat-detection-get-started.md) aktivieren und Konfigurieren der Erkennung und Konfigurieren der e-Mails, die Sicherheitshinweise bei außergewöhnlichen Aktivitäten erhalten.

## <a name="see-also"></a>Siehe auch

In diesem Artikel wird gezeigt, wie der Security Center-Empfehlung implementiert "Aktivieren der Überwachung für SQL Server". Um weitere Informationen zum Sichern Ihrer SQL-Datenbank finden Sie hier:

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
[1]: ./media/security-center-enable-auditing-on-sql-server/enable-auditing-on-sql-servers.png
[2]:./media/security-center-enable-auditing-on-sql-server/enable-auditing.png
[3]: ./media/security-center-enable-auditing-on-sql-server/auditing-settings-blade.png
