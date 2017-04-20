<properties
    pageTitle="Verbinden mit SQL-Datenbank mit Java JDBC unter Windows | Microsoft Azure"
    description="Zeigt ein Java-Codebeispiel, mit Azure SQL-Datenbank herstellen. Das Beispiel verwendet JDBC und läuft auf einem Windows-Client-Computer."
    services="sql-database"
    documentationCenter=""
    authors="LuisBosquez"
    manager="jhubbard"
    editor="genemi"/>


<tags
    ms.service="sql-database"
    ms.workload="drivers"
    ms.tgt_pltfrm="na"
    ms.devlang="java"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="lbosq"/>


# <a name="connect-to-sql-database-by-using-java-with-jdbc-on-windows"></a>Verbinden Sie mit SQL-Datenbank mit Java JDBC unter Windows


[AZURE.INCLUDE [sql-database-develop-includes-selector-language-platform-depth](../../includes/sql-database-develop-includes-selector-language-platform-depth.md)] 


Dieses Thema enthält eine Java-Beispielcode, mit dem Sie Azure SQL-Datenbank herstellen. Java-Beispiel beruht auf dem Java Development Kit (JDK) Version 1.8. Im Beispiel verbindet mit einer Azure SQL-Datenbank mit der JDBC-Treiber.

## <a name="step-1--configure-development-environment"></a>Schritt 1: Konfigurieren von Development Environment

[Konfigurieren der Umgebung für die Entwicklung von Java](https://msdn.microsoft.com/library/mt720658.aspx)

## <a name="step-2-create-a-sql-database"></a>Schritt 2: Erstellen einer SQL-Datenbank

Finden Sie die [Seite Erste Schritte](sql-database-get-started.md) auf eine Datenbank erstellen.  

## <a name="step-3-get-connection-string"></a>Schritt 3: Abrufen Sie Verbindungszeichenfolge

[AZURE.INCLUDE [sql-database-include-connection-string-jdbc-20-portalshots](../../includes/sql-database-include-connection-string-jdbc-20-portalshots.md)]

> [AZURE.NOTE] Wenn Sie JTDS JDBC-Treiber verwenden, müssen Sie hinzufügen "Ssl = erforderlich" an die URL der Verbindung Zeichenfolge und müssen die folgende Option für die JVM "-Djsse.enableCBCProtection=false". Diese JVM-Option deaktiviert eine Korrektur für eine Sicherheitslücke, also sicher, dass Sie wissen, welches Risiko beteiligt ist, bevor Sie diese Einstellung.

## <a name="step-4-run-sample-code"></a>Schritt 4: Ausführen von Beispielcode

* [Verbinden mit SQL mit Java Machbarkeitsstudie](https://msdn.microsoft.com/library/mt720656.aspx)

## <a name="next-steps"></a>Nächste Schritte

* Besuchen Sie die [Java Developer Center](/develop/java/).
* Lesen Sie die [SQL-Datenbank-Anwendungsentwicklung, Überblick](sql-database-develop-overview.md)
* Weitere Informationen über [Microsoft JDBC-Treiber für SQL Server](https://msdn.microsoft.com/library/mt484311.aspx)

## <a name="additional-resources"></a>Zusätzliche Ressourcen 

* [Entwurfsmuster für Multi-Tenant SaaS-Applikationen mit SQL Azure-Datenbank](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* Durchsuchen Sie alle [Funktionen der SQL-Datenbank](https://azure.microsoft.com/services/sql-database/)
