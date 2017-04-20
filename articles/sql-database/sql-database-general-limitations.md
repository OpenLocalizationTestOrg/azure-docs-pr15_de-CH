<properties
   pageTitle="SQL Azure Database General Grenzen und Richtlinien"
   description="Diese Seite beschreibt einige allgemeinen Einschränkungen für Azure SQL-Datenbank sowie Interoperabilität und Support."
   services="sql-database"
   documentationCenter="na"
   authors="CarlRabeler"
   manager="jhubbard"
   editor="monicar" />
<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="09/06/2016"
   ms.author="carlrab" />

# <a name="azure-sql-database-general-limitations-and-guidelines"></a>SQL Azure Database General Grenzen und Richtlinien

Dieses Thema enthält allgemeine Grenzen und Richtlinien für Azure SQL-Datenbank. Ein umfassender Überblick Quoten, Ressourcenmanagement und Unterstützung finden Sie [zusätzliche Ressourcen](#additional-guidelines) am Ende dieses Themas.

## <a name="connectivity-and-authentication"></a>Konnektivität und Authentifizierung

  - Windows-Authentifizierung wird nicht unterstützt. Finden Sie unter [Verwalten von Datenbanken und Anmeldedaten in Azure SQL-Datenbank](sql-database-manage-logins.md). Azure Active Directory-Authentifizierung ist jedoch mit Einschränkungen unterstützt. Finden Sie unter [Verbinden mit Datenbank SQL Azure Active Directory-Authentifizierung](sql-database-aad-authentication.md).

  - Microsoft Azure SQL-Datenbank unterstützt tabular Data Stream (TDS)-Protokoll-Clientversion 7.3 oder höher.

  - Nur TCP-Verbindungen sind zulässig.

  - SQL Server 2008 SQL Server-Browser wird nicht unterstützt, da Microsoft Azure SQL-Datenbank nicht dynamische Ports nur Port 1433.

## <a name="sql-server-agentjobs"></a>SQL Server-Agent/Aufträge

Microsoft Azure SQL-Datenbank unterstützt keine SQL Server-Agent, aber Sie elastische Aufträge können Aufträge über zu viele Datenbanken ausgeführt. Weitere Informationen über elastische Aufträge anzeigen Sie [elastische Aufträge](sql-database-elastic-jobs-overview.md)

## <a name="sql-server-collation-support"></a>Unterstützung für SQL Server-Sortierung

Datenbankkollatierung Microsoft Azure SQL-Datenbank ist **SQL_LATIN1_GENERAL_CP1_CI_AS**, wo **LATIN1_GENERAL** ist Englisch (USA), **CP1** Codepage 1252 ist **CI** wird Groß-und Kleinschreibung und **Akzenten liegt** . Es ist nicht möglich die Sortierung für V12 Datenbanken ändern. Weitere Informationen zum Festlegen die Kollatierung finden Sie unter [COLLATE (Transact-SQL)](https://msdn.microsoft.com/library/ms184391.aspx).

## <a name="naming-requirements"></a>Namenskonventionen

Bestimmter Benutzernamen sind aus Sicherheitsgründen nicht zulässig. Die folgenden Namen können nicht verwendet werden:

 - **Admin**
 - **Administrator**
 - **Gast**
 - **Stamm**
 - **SA**

Namen für alle neuen Objekte müssen die SQL Server-Regeln für Bezeichner entsprechen. Weitere Informationen finden Sie unter [Identifiers](https://msdn.microsoft.com/library/ms175874.aspx).

Anmeldung und dürfen nicht darüber hinaus enthalten die \ Zeichen (Windows-Authentifizierung wird nicht unterstützt).

## <a name="additional-guidelines"></a>Weitere Richtlinien

- Neben den in diesem Artikel beschriebenen allgemeinen eingeschränkt hat SQL-Datenbank bestimmte ressourcenkontingente und Einschränkungen basierend auf der **Dienstebene**. Überblick Dienstebenen finden Sie unter [Dienstebenen SQL-Datenbank](sql-database-service-tiers.md).

- Andere Grenzwerte SQL-Datenbank finden Sie unter [Azure SQL Datenbank Ressourcengrenzen](sql-database-resource-limits.md).

- Sicherheit verwandte Richtlinien finden Sie unter [Azure SQL Datenbank Richtlinien und Einschränkungen](sql-database-security-guidelines.md).

- Eine andere umgibt Verwandte die Kompatibilität auf lokale Versionen von SQL Server, SQL Server 2014 wie SQL Server 2016 Azure SQL-Datenbank verfügt. Die neueste Version V12 Azure SQL-Datenbank hat viele in diesem Bereich verbessert. Weitere Informationen finden Sie unter [Neuigkeiten bei SQL Datenbank V12](sql-database-v12-whats-new.md).

- Informationen zu Verfügbarkeit und Support für SQL-Datenbank finden Sie unter [Bibliotheken für SQL-Datenbank und SQL Server-Verbindung](sql-database-libraries.md).
