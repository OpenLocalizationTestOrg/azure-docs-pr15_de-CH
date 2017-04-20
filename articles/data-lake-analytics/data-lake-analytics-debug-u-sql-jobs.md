<properties 
   pageTitle="U-SQL-Aufträge Debuggen | Microsoft Azure" 
   description="Informationen Sie zum fehlgeschlagenen Scheitelpunkt U-SQL mit Visual Studio debuggen. " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="mumian" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="09/02/2016"
   ms.author="jgao"/>



#<a name="debug-c-code-in-u-sql-for-data-lake-analytics-jobs"></a>Debuggen Sie C#-Code in U-SQL Data Lake Analytics-Aufträge 

Erfahren Sie, wie Azure Data Lake Visual Studio-Tools zum Debuggen fehlgeschlagene SQL U Aufträge aufgrund von Fehlern im Benutzercode verwendet. 

Visual Studio-Tools können Sie Cluster und Debuggen fehlgeschlagene Aufträgen kompilierten Code und erforderlichen Vertexdaten herunterladen.

Große Systeme stellen normalerweise Erweiterungsmodell in Sprachen wie Java, C#, Python, etc.. Viele Systeme bieten begrenzte Laufzeit Debuginformationen, die Laufzeitfehler in benutzerdefiniertem Code Debuggen erschwert. Die neuesten Visual Studio-Tools enthält ein Feature namens "Fehler Scheitelpunkt Debug". Mit dieser Funktion können Sie die Laufzeitdaten Azure lokalen Arbeitsstation herunterladen, damit Sie fehlerhaften benutzerdefinierten C#-Code mit der gleichen Laufzeit genau Eingabedaten aus der Cloud Debuggen können.  Nachdem die Probleme behoben wurden, können Sie den überarbeiteten Code in Azure die Tools erneut ausführen.

Eine Videopräsentation dieser Funktion finden Sie unter [Debuggen des benutzerdefinierten Codes in Azure Data Lake Analytics](https://mix.office.com/watch/1bt17ibztohcb).

>[AZURE.NOTE] Visual Studio kann hängen oder abstürzen, haben Sie die folgenden zwei Windows-Upgrades: [Microsoft Visual C++ 2015 Redistributable Update 2](https://www.microsoft.com/download/details.aspx?id=51682), [Universal C Runtime für Windows](https://www.microsoft.com/download/details.aspx?id=50410&wa=wsignin1.0).


##<a name="prerequisites"></a>Erforderliche Komponenten
-   Im [ersten](data-lake-analytics-data-lake-tools-get-started.md) Artikel haben durchlaufen werden.

## <a name="create-and-configure-debug-projects"></a>Erstellen und Konfigurieren von Projekten für debug

Beim Öffnen eines Auftragsfehlers in Daten dem Visual Studio-Tools erhalten Sie eine Warnung. Detaillierte Fehlerinformationen werden in der Registerkarte Fehler und die Warnung gelbe Leiste oben im Fenster angezeigt. 

![Azure Data Lake Analytics U-SQL Debuggen visual Studio Download Scheitelpunkt](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-download-vertex.png)

**Zu Eckpunkt Erstellen einer Debug-Lösung**

1.  Öffnen eines Auftragsfehlers U-SQL in Visual Studio.
2.  Klicken Sie auf **herunterladen** , um die erforderlichen Ressourcen und Eingabestreams herunterladen. Klicken Sie auf **Wiederholen** , wenn der Download ist fehlgeschlagen.
3.  Klicken Sie auf **Öffnen** , nachdem der Download abgeschlossen ist, um ein lokales Debuggen Projekt erstellen. Eine neue Visual Studio-Projektmappe namens **VertexDebug** mit einem leeren Projekt namens **LocalVertexHost** erstellt.

Benutzerdefinierte Operatoren in U-SQL-Code (Script.usql.cs) verwendet, muss Class Library C#-Projekt mit dem benutzerdefinierten Operatoren Code erstellen und das Projekt in der Projektmappe VertexDebug enthalten.

Wenn Sie DLL-Assemblys zur Datenanalyse See Datenbank registriert haben, müssen Sie den Quellcode der Assemblys der Projektmappe VertexDebug hinzufügen.
 
Erstellen eine separate C#-Klassenbibliothek für U-SQL-Code und registrierte DLL-Assemblys zur Datenanalyse See Datenbank müssen Sie das Quellprojekt C# Assemblys der Projektmappe VertexDebug hinzufügen.

In seltenen Fällen verwenden Sie benutzerdefinierte Operatoren in U-SQL CodeBehind-Datei (Script.usql.cs) der ursprünglichen Lösung. Wunsch zu arbeiten, müssen Sie eine C#-Bibliothek mit dem Quellcode erstellen und Ändern der Assemblyname im Cluster registriert. Sie erhalten den Assemblynamen im Cluster registriert ist, überprüft das Skript im Cluster ausgeführt haben. Dazu öffnen den U-SQL-Auftrag, und klicken Sie auf "Skript" im Bedienfeld "Projekt". 

**Die Lösung konfigurieren**

1.  Im Projektmappen-Explorer mit der rechten Maustaste des C#-Projekts erstellten und dann auf **Eigenschaften**.
2.  Legen Sie den Ausgabepfad als LocalVertexHost Projekt Verzeichnispfad. Sie erhalten LocalVertexHost Arbeitsverzeichnis Projektpfad durch LocalVertexHost Eigenschaften.
3.  Das C#-Projekt erstellen, um die PDB-Datei zu LocalVertexHost Projekt Arbeitsverzeichnis, oder Sie können die PDB-Datei manuell in diesen Ordner kopieren.
4.  **Ausnahmeeinstellungen**checken Sie Common Language Runtime-Ausnahmen ein:

![Azure Data Lake Analytics U-SQL Debugeinstellung visual studio](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-clr-exception-setting.png)
 
##<a name="debug-the-job"></a>Debuggen des Auftrags

Eine Debug-Lösung durch Herunterladen des Scheitelpunkts erstellt haben und konfiguriert die Umgebung können Sie U-SQL-Code Debuggen starten.

1.  Im Projektmappen-Explorer mit der rechten Maustaste des **LocalVertexHost** Projekts erstellten **Debuggen**zeigen und klicken Sie auf **neue Instanz starten**. Die LocalVertexHost muss als Startprojekt festgelegt werden. Sie sehen die Meldung zum ersten Mal ignorieren können. Es dauert bis zu einer Minute Debug-Bildschirm zu.
 
    ![Azure Data Lake Analytics U-SQL Debugwarnung visual studio](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-visual-studio-u-sql-debug-warning.png)

4.  Mit Visual Studio basieren Debuggen (Uhr, Variablen usw.), das Problem zu beheben. 
5.  Nachdem Sie ein Problem identifiziert haben, korrigieren Sie den Code und neu erstellen Sie des C#-Projekts bevor es erneut testen, bis die Probleme gelöst sind. Nach das Debuggen wurde, zeigt die folgende Meldung im Ausgabefenster abgeschlossen 

        The Program ‘LocalVertexHost.exe’ has exited with code 0 (0x0).
 
##<a name="resubmit-the-job"></a>Ausgeben

Nach dem Debuggen des Codes U-SQL können Sie den fehlerhaften Auftrag erneut.

1. Registrieren Sie neue DLL-Assemblys zur ADLA Datenbank.

    1.  Erweitern Sie im Server-Explorer-Cloud Explorer in Daten See Visual Studio Tools **Datenbanken** 
    2.  Assemblys mit der rechten Maustaste auf Register Assemblys. 
    3.  Registrieren Sie DLL-Assemblys neue Datenbank ADLA.
 
2.  Oder den C#-Code in script.usql.cs--C# CodeBehind-Datei kopieren.
3.  Ihre Aufgabe erneut.

##<a name="next-steps"></a>Nächste Schritte

- [Lernprogramm: Erste Schritte mit Azure Data Lake Analytics U-SQL-Sprache](data-lake-analytics-u-sql-get-started.md)
- [Lernprogramm: Entwickeln Sie U-SQL-Skripts mit Daten See Tools für Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
- [Entwickeln Sie U-SQL benutzerdefinierte Operatoren für Azure Data Lake Analytics Projekte](data-lake-analytics-u-sql-develop-user-defined-operators.md)

