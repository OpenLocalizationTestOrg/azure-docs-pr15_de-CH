<properties
    pageTitle="SQL-Datenbank für kompatible Clients unterstützen und IP-Endpunkt Überwachung ändert | Microsoft Azure"
    description="Informationen Sie zu SQL-Datenbank zur Unterstützung für kompatible Clients und IP-Endpunkt ändert Überwachung."
    services="sql-database"
    documentationCenter=""
    authors="ronitr"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/10/2016"
    ms.author="ronitr"/>

# <a name="sql-database----downlevel-clients-support-and-ip-endpoint-changes-for-auditing"></a>SQL-Datenbank - kompatible Clients unterstützen und IP-Endpunkt Überwachung ändert


[Überwachung](sql-database-auditing-get-started.md) funktioniert automatisch mit SQL-Clients, die TDS-Umleitung unterstützen.


##<a id="subheading-1"></a>Unterstützung für kompatible clients

Jeder Client TDS 7.4 implementiert sollten auch Umleitung unterstützen. Ausnahmen dieser JDBC 4.0 in der Umleitung-Funktion wird nicht unterstützt und langweilig für die Umleitung Node.JS wurde nicht implementiert.

Für "Kompatible Clients" sollte also die Unterstützung TDS Version 7.3 und -den FQDN des Servers in der Verbindung Zeichenfolge geändert werden:

FQDN des ursprünglichen Servers in der Verbindungszeichenfolge: <*Servername*>. database.windows.net

Geänderte Server FQDN in der Verbindungszeichenfolge: <*Servername*> .database. **sichere**. Windows

"Ältere Clients" zählen:

- .NET 4.0 und früher,
- ODBC-10.0 und darunter.
- JDBC (JDBC TDS 7.4 unterstützt, TDS Umleitung-Funktion wird nicht unterstützt)
- (Für Node.JS)

**Hinweis:** Über Server FDQN Änderung möglicherweise auch für Richtlinien eine SQL Server-Überwachung auf ohne einen Konfigurationsschritt in jeder Datenbank (temporäre Mitigation).

##<a id="subheading-2"></a>IP-Endpunkt wird beim Aktivieren der Überwachung

Beachten Sie, dass beim Aktivieren der Überwachung IP-Endpunkt der Datenbank ändern. Haben Sie strenge Firewall Settings, aktualisieren Sie die Firewall Settings entsprechend.

Neue Datenbank-IP-Endpunkt wird die Datenbankregion abhängig:

| Datenbankregion | Mögliche IP-Endpunkte |
|----------|---------------|
| China North  | 139.217.29.176, 139.217.28.254 |
| China-OST  | 42.159.245.65, 42.159.246.245 |
| Australien OST  | 104.210.91.32, 40.126.244.159, 191.239.64.60, 40.126.255.94 |
| Australien Südost | 191.239.184.223, 40.127.85.81, 191.239.161.83, 40.127.81.130 |
| Brasilien Süd  | 104.41.44.161, 104.41.62.230, 23.97.99.54, 104.41.59.191 |
| USA  | 104.43.255.70, 40.83.14.7, 23.99.128.244, 40.83.15.176 |
| Ostasien   | 23.99.125.133, 13.75.40.42, 23.97.71.138, 13.94.43.245 |
| USA 2 OST | 104.209.141.31, 104.208.238.177, 191.237.131.51, 104.208.235.50 |
| Osten der USA   | 23.96.107.223, 104.41.150.122, 23.96.38.170, 104.41.146.44 |
| Zentrale Indien  | 104.211.98.219, 104.211.103.71 |
| Süd Indien   | 104.211.227.102, 104.211.225.157 |
| West Indien  | 104.211.161.152, 104.211.162.21 |
| Japan OST   | 104.41.179.1, 40.115.253.81, 23.102.64.207, 40.115.250.196 |
| Japan West    | 104.214.140.140, 104.214.146.31, 191.233.32.34, 104.214.146.198 |
| Norden der USA – zentral  | 191.236.155.178, 23.96.192.130, 23.96.177.169, 23.96.193.231 |
| Nordeuropa  | 104.41.209.221, 40.85.139.245, 137.116.251.66, 40.85.142.176 |
| Südlichen zentralen USA  | 191.238.184.128, 40.84.190.84, 23.102.160.153, 40.84.186.66 |
| Südostasien  | 104.215.198.156, 13.76.252.200, 23.97.51.109, 13.76.252.113 |
| Westeuropa  | 104.40.230.120, 13.80.23.64, 137.117.171.161, 13.80.8.37, 104.47.167.215, 40.118.56.193, 104.40.176.73, 40.118.56.20 |
| Westen der USA  | 191.236.123.146, 138.91.163.240, 168.62.194.148, 23.99.6.91 |
| Kanada zentral  | 13.88.248.106 |
| Kanada Osten  |  40.86.227.82 |
