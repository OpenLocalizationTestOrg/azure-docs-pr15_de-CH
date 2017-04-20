<properties
   pageTitle="Installieren Sie Visual Studio und SSDT für SQL Datawarehouse | Microsoft Azure"
   description="Installieren Sie Visual Studio und SQL Server-Entwicklungstools (SSDT) für SQL Azure Datawarehouse"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/16/2016"
   ms.author="sonyama;barbkess"/>

# <a name="install-visual-studio-2015-and-ssdt-for-sql-data-warehouse"></a>Installieren Sie Visual Studio 2015 und SSDT für SQL Datawarehouse

Um die Anwendungsentwicklung für SQL Data Warehouse empfehlen wir Visual Studio 2015 mit der neuesten Version von SQL Server Data Tools (SSDT).  Visual Studio 2013 Update 5 SSDT ist auch für Abwärtskompatibilität unterstützt.  

SSDT mit Visual Studio können Sie mit SQL Server-Objekt-Explorer visuell Durchsuchen von Tabellen, Ansichten, gespeicherte Prozeduren und viele weitere Objekte im SQL Data Warehouse sowie Abfragen auszuführen.

> [AZURE.NOTE] SQL Data Warehouse unterstützt noch keine Visual Studio Database Projects.  Dieses Feature wird in einer zukünftigen Version hinzugefügt.

## <a name="step-1-install-visual-studio-2015"></a>Schritt 1: Installieren von Visual Studio 2015

Folgen Sie diesen Links zum Herunterladen und Installieren von Visual Studio 2015. Wenn bereits Visual Studio 2013 oder 2015 installiert haben, fahren Sie mit Schritt2 fort, Sie SSDT installieren.

1. [Visual Studio 2015 herunterladen][].
2. [Installieren von Visual Studio][] führen auf MSDN, und wählen Sie die Standardkonfigurationen.

## <a name="step-2-install-ssdt"></a>Schritt 2: Installieren von SSDT

Installieren SSDT für Visual Studio einfach überprüfen, ob ein SSDT Update in Visual Studio folgende Schritte.

1. Klicken Sie in Visual Studio auf **Extras** / **Extensions und Updates**  /  **Updates**
2. Wählen Sie **Produkt-Updates** und **Tools von Microsoft SQL Server-Update für die Datenbank** suchen

Wenn ein Update nicht gefunden wird, sollten Sie die neueste Version installiert haben.  Bestätigen SSDT installiert ist, klicken Sie auf **Hilfe** / **Zu Microsoft Visual Studio** und SQL Server Data Tools in der Liste suchen.  Die neueste Version von SSDT ist 14.0.60525.0.  Wenn nicht die Option zum Installieren von Visual Studio verfügbar ist, oder besuchen Sie [SSDT][] -Downloadseite herunter und installieren SSDT manuell.

## <a name="next-steps"></a>Nächste Schritte

Jetzt haben Sie die neueste Version von SSDT können Sie [die Verbindung][] zu SQL Data Warehouse.

<!--Anchors-->

<!--Image references-->

<!--Articles-->
[Verbinden]: ./sql-data-warehouse-query-visual-studio.md

<!--Other-->
[Herunterladen von Visual Studio 2015]: https://www.visualstudio.com/downloads/
[Installieren von Visual Studio]: https://msdn.microsoft.com/library/e2h7fzkw.aspx
[SSDT herunterladen]: https://msdn.microsoft.com/library/mt204009.aspx
