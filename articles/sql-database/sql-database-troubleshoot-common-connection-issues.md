<properties
    pageTitle="Allgemeine Verbindungsprobleme zu Azure SQL-Datenbank"
    description="Schritte zum Erkennen und Beheben von allgemeinen Verbindungsfehler für Azure SQL-Datenbank."
    services="sql-database"
    documentationCenter=""
    authors="dalechen"
    manager="felixwu"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/31/2016"
    ms.author="daleche"/>

# <a name="troubleshoot-connection-issues-to-azure-sql-database"></a>Verbindungsprobleme, Azure SQL-Datenbank

Bei Verbindung mit Azure SQL-Datenbank fehlschlägt, werden [Fehlermeldungen](sql-database-develop-error-messages.md)angezeigt. Dieser Artikel ist ein zentrales Thema, das hilft Verbindungsproblemen Azure SQL-Datenbank. Es stellt [die häufigsten Ursachen](#cause) für Probleme, sollten [bei der Problembehandlung](#try-the-troubleshooter-for-azure-sql-database-connectivity-issues) , die Identität des Problems, und beschreibt Schritte [transiente](#troubleshoot-transient-errors) und [persistent oder nicht flüchtigen Fehler](#troubleshoot-the-persistent-errors)lösen. Schließlich werden [alle relevanten Artikel Konnektivitätsprobleme Azure SQL-Datenbank](#all-topics-for-azure-sql-database-connection-problems)aufgelistet.

Falls die Verbindungsprobleme auftreten, versuchen Sie Problembehandlung Schritte, die in diesem Artikel beschrieben werden.
[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="cause"></a>Ursache

Probleme mit der Verbindung können folgende Ursachen haben:

- Fehler während des Entwurfsprozesses Anwendung best Practices und Richtlinien anwenden.  Finden Sie unter [Übersicht über die Entwicklung von SQL-Datenbank](sql-database-develop-overview.md) für den Einstieg.
- Azure SQL-Datenbank-Neukonfiguration
- Firewall Einstellung
- Verbindungstimeout
- Falscher Anmeldeinformationen
- Maximale Anzahl erreicht, einige Ressourcen Azure SQL-Datenbank

Im Allgemeinen können bei Azure SQL-Datenbank wie folgt klassifiziert:

- [Vorübergehender Fehler (kurzlebige oder zeitweise)](#troubleshoot-transient-errors)
- [Persistent oder nicht flüchtigen Fehler (Fehler, die regelmäßig wiederholen)](#troubleshoot-the-persistent-errors)

## <a name="try-the-troubleshooter-for-azure-sql-database-connectivity-issues"></a>Probieren Sie den Ratgeber für Konnektivitätsprobleme Azure SQL-Datenbank

Wenn Fehlermeldung eine Verbindung versuchen Sie [dieses Tool](https://support.microsoft.com/help/10085/troubleshooting-connectivity-issues-with-microsoft-azure-sql-database), das Ihnen hilft schnell Identität und beheben Sie das Problem zu.

## <a name="troubleshoot-transient-errors"></a>Vorübergehender Fehler beheben
Wenn Ihre Anwendung treten vorübergehende Fehler auf, überprüfen Sie Tipps zur Problembehandlung und die Häufigkeit dieser Fehler in den folgenden Themen:

- [Problembehandlung bei Datenbank &lt;x&gt; auf Server &lt;y&gt; ist nicht verfügbar (Fehler: 40613)](sql-database-troubleshoot-connection.md)
- [Problembehandlung, diagnose und verhindern von SQL-Verbindung und vorübergehende Fehler für SQL-Datenbank](sql-database-connectivity-issues.md)

<a id="troubleshoot-the-persistent-errors" name="troubleshoot-the-persistent-errors"></a>

## <a name="troubleshoot-persistent-errors-non-transient-errors"></a>Permanente Fehlerbehebung (nicht flüchtigen Fehler)

Wenn die Anwendung dauerhaft Azure SQL-Datenbank herstellen, wird in der Regel ein Problem mit einem der folgenden:

- Konfiguration der Firewall. Die SQL Azure-Datenbank oder Client-Firewall blockiert Verbindungen zu Azure SQL-Datenbank.
- Netzwerk-Neukonfiguration auf der Clientseite: z. B. eine neue IP-Adresse oder einen Proxy-Server.
- Fehler: z. B. falsch Verbindungsparameter wie den Servernamen in der Verbindungszeichenfolge.

### <a name="steps-to-resolve-persistent-connectivity-issues"></a>Schritte zum permanenten Verbindungsprobleme beheben

1.  [Firewall-Regeln](sql-database-configure-firewall-settings.md) erlauben die Client-IP-Adresse eingerichtet.
2.  Auf allen Firewalls zwischen dem Client und dem Internet muss Port 1433 für ausgehende Verbindungen geöffnet ist. Überprüfen Sie [die Windows-Firewall für den SQL Server-Zugriff konfigurieren](https://msdn.microsoft.com/library/cc646023.aspx) für Weitere Hinweise.
3.  Überprüfen Sie die Verbindungszeichenfolge und andere Einstellungen. Siehe Abschnitt Verbindungszeichenfolge [Konnektivität Probleme Thema](sql-database-connectivity-issues.md#connections-to-azure-sql-database).
4.  Überprüfen Sie Dienst im Dashboard. Wenn Sie, dass ein regionaler Ausfall glauben, finden Sie unter Schritte in einen neuen Bereich wiederherstellen [nach einem Ausfall wiederherstellen](sql-database-disaster-recovery.md) .

## <a name="all-topics-for-azure-sql-database-connection-problems"></a>Alle Themen Verbindungsprobleme Azure SQL-Datenbank

Die folgende Tabelle listet jede Verbindung Problem Thema, die direkt an den Dienst Azure SQL-Datenbank angewendet wird.


| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 1 | [Verbindungsprobleme, Azure SQL-Datenbank](sql-database-troubleshoot-common-connection-issues.md) | Dies ist die Zielseite für die Behandlung von Konnektivitätsproblemen in Azure SQL-Datenbank. Es beschreibt erkennen und Beheben von vorübergehenden und persistent oder nicht flüchtigen Fehler in Azure SQL-Datenbank. |
| 2 | [Problembehandlung, diagnose und verhindern von SQL-Verbindung und vorübergehende Fehler für SQL-Datenbank](sql-database-connectivity-issues.md) | Informationen Sie zum beheben, diagnostizieren und zu verhindern, dass ein Verbindungsfehler SQL oder vorübergehender Fehler in Azure SQL-Datenbank. |
| 3 | [Vorübergehende Fehlerbehandlung Leitfaden](best-practices-retry-general.md) | Enthält allgemeine Hinweise zum vorübergehenden Fehlerbehandlung beim Azure SQL-Datenbank herstellen. |
| 4 | [Beheben von Konnektivitätsproblemen mit Microsoft Azure SQL-Datenbank](https://support.microsoft.com/help/10085/troubleshooting-connectivity-issues-with-microsoft-azure-sql-database) | Mit diesem Tool können die Identität Ihres Problems Verbindungsfehler. |
| 5 | [Problembehandlung bei "Datenbank &lt;x&gt; auf Server &lt;y&gt; ist zurzeit nicht verfügbar. Wiederholen die Verbindung später"Fehler](sql-database-troubleshoot-connection.md) | Beschreibt das Identifizieren und Lösen von 40613 Fehler: "Datenbank &lt;x&gt; auf Server &lt;y&gt; ist zurzeit nicht verfügbar. Versuchen Sie die Verbindung später." |
| 6 | [Fehlercodes für Clientanwendungen SQL Datenbank SQL: Datenbank-Verbindungsfehler und andere Probleme](sql-database-develop-error-messages.md) | Stellt Informationen über SQL-Fehlercodes für Clientanwendungen SQL Datenbank Verbindung Fehlermeldungen kopieren Datenbankproblemen und allgemeine Fehler. |
| 7 | [Azure SQL-Datenbank Leitfaden zur Leistung für einzelne Datenbanken](sql-database-performance-guidance.md) | Erfahren Sie, um zu bestimmen, welche Dienstebene für Ihre Anwendung ist. Auch Leitfaden für Ihre Azure SQL-Datenbank optimal verbessern. |
| 8 | [SQL Datenbank-Anwendungsentwicklung, Überblick](sql-database-develop-overview.md) | Enthält Links zu Codebeispielen für verschiedene Technologien und interagieren mit Azure SQL-Datenbank. |
| 9 | Aktualisieren Sie auf Azure SQL-Datenbank v12 Seite ([Azure-Portal](sql-database-upgrade-server-portal.md), [PowerShell](sql-database-upgrade-server-powershell.md)) | Beschreibt, wie Sie für das Aktualisieren von vorhandenen Azure SQL-Datenbank V11 Server und Datenbanken auf Azure SQL-Datenbank V12 Azure-Portal oder PowerShell. |


## <a name="next-steps"></a>Nächste Schritte

- [Problembehandlung bei Leistungsproblemen Azure SQL-Datenbank](sql-database-troubleshoot-performance.md)
- [Problembehandlung bei Berechtigungsproblemen Azure SQL-Datenbank](sql-database-troubleshoot-permissions.md)
- [Finden Sie alle Themen der Azure SQL-Datenbank-Dienst](sql-database-index-all-articles.md)
- [Suchen der Dokumentation zu Microsoft Azure](http://azure.microsoft.com/search/documentation/)
- [Die neuesten Updates für Azure SQL-Datenbankdienst anzeigen](http://azure.microsoft.com/updates/?service=sql-database)


## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [SQL Datenbank-Anwendungsentwicklung, Überblick](sql-database-develop-overview.md)
- [Vorübergehende Fehlerbehandlung Leitfaden](../best-practices-retry-general.md)
- [Bibliotheken für SQL-Datenbank und SQL Server-Verbindung](sql-database-libraries.md)
- [Der Learning Path für die Verwendung von Azure SQL-Datenbank](https://azure.microsoft.com/documentation/learning-paths/sql-database-training-learn-sql-database)
- [Der Learning Path für elastische Datenbankfunktionen und tools](https://azure.microsoft.com/documentation/learning-paths/sql-database-elastic-scale) 
