<properties
    pageTitle="Datenbank auf dem Server ist nicht verfügbar, Verbinden mit SQL-Datenbank | Microsoft Azure"
    description="Problembehandlung bei der Datenbank auf Server ist nicht verfügbar Fehler, wenn eine Anwendung mit SQL-Datenbank verbindet."
    services="sql-database"
    documentationCenter=""
    authors="dalechen"
    manager="felixwu"
    editor=""
    keywords="Datenbank auf dem Server ist nicht verfügbar, Verbinden mit Sql-Datenbank"/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/21/2016"
    ms.author="daleche"/>

# <a name="error-database-on-server-is-not-currently-available-when-connecting-to-sql-database"></a>Fehler "Datenbank auf dem Server ist derzeit nicht verfügbar" beim Verbinden mit Sql-Datenbank
[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

Wenn eine Anwendung mit einer Azure SQL-Datenbank herstellt, wird die folgende Fehlermeldung angezeigt:

```
Error code 40613: "Database <x> on server <y> is not currently available. Please retry the connection later. If the problem persists, contact customer support, and provide them the session tracing ID of <z>"
```

> [AZURE.NOTE] Diese Fehlermeldung wird normalerweise vorübergehend (kurzlebige).

Dieser Fehler tritt auf, wenn die Azure-Datenbank wird verschoben (oder neu) und die Anwendung die Verbindung mit der SQL-Datenbank. SQL Datenbank Neukonfiguration Ereignisse tritt aufgrund einer geplanten (z. B. eines Software-Upgrades) oder ein ungeplantes Ereignis (z. B. einen Prozessabsturz oder Netzwerklastenausgleich). Die meisten Neukonfiguration Ereignisse sind im Allgemeinen kurzlebig und höchstens in weniger als 60 Sekunden abgeschlossen sein sollte. Jedoch können diese Ereignisse gelegentlich länger benötigt wie wenn eine große Transaktion langer Recovery verursacht.

## <a name="steps-to-resolve-transient-connectivity-issues"></a>Auf vorübergehenden Verbindungsproblemen
1.  Überprüfen der [Microsoft Azure Service Dashboard](https://azure.microsoft.com/status) für bekannte Ausfälle, die während der Zeit trat der Fehler der Anwendung gemeldet wurden.
2. Cloud-Dienst herstellen, wie Azure SQL-Datenbank sollte erwarten periodische Neukonfiguration Ereignisse implementieren Applikationen Wiederholungslogik behandeln Sie diese Fehler statt als Anwendungsfehler Benutzer angezeigt. Lesen Sie Abschnitt [vorübergehender Fehler](sql-database-connectivity-issues.md) und die best Practices und Design Richtlinien [SQL Datenbank Development Overview](sql-database-develop-overview.md) Informationen und allgemeine Strategien wiederholen. Lesen Sie Einzelheiten Codebeispiele in [Bibliotheken für SQL-Datenbank und SQL Server-Verbindung](sql-database-libraries.md) .
3.  Eine Datenbank die Ressourcengrenzen nähert, kann es scheinen vorübergehende Verbindung. Finden Sie unter [Problembehandlung bei Leistungsproblemen](sql-database-troubleshoot-performance.md).
4.  Wenn Verbindungsprobleme weiterhin für die Bewerbung fehlerbehaftete 60 Sekunden überschreitet oder mehrere Vorkommen des Fehlers in einem bestimmten Tag angezeigt, Datei Azure Supportanfrage **Erhalten Unterstützung** auf der [Azure-Support](https://azure.microsoft.com/support/options) -Website auswählen.

## <a name="next-steps"></a>Nächste Schritte
- Wenn eine andere Fehlermeldung Werten Sie für Hinweise auf die Ursache der [Fehlermeldung](sql-database-develop-error-messages.md) .
- Wenn persistent ist, besuchen Sie diese Anleitung [Problembehandlung häufig Probleme in Azure SQL-Datenbank](sql-database-troubleshoot-common-connection-issues.md).
