<properties 
   pageTitle="U-SQL benutzerdefinierte Operatoren für Azure Data Lake Analytics Projekte entwickeln | Azure" 
   description="Informationen Sie zu benutzerdefinierten Operatoren verwendet und Wiederverwendung in Lake Datenanalyse Aufträge. " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="edmacauley" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="05/16/2016"
   ms.author="edmaca"/>


# <a name="develop-u-sql-user-defined-operators-for-azure-data-lake-analytics-jobs"></a>Entwickeln Sie U-SQL benutzerdefinierte Operatoren für Azure Data Lake Analytics Projekte

Informationen Sie zu benutzerdefinierten Operatoren verwendet und Wiederverwendung in Lake Datenanalyse Aufträge. Sie entwickeln einen benutzerdefinierten Operator Ländernamen konvertieren.

##<a name="prerequisites"></a>Erforderliche Komponenten

- Visual Studio 2015, Visual Studio 2013 update 4 oder Visual Studio 2012 mit Visual C++ installiert 
- Microsoft Azure SDK für .NET Version 2.5 oder höher.  Installieren Sie mit dem Webplattform-Installer.
- Ein Konto See Datenanalyse.  [Erste Schritte mit Azure See Datenanalyse mithilfe von Azure-Portal](data-lake-analytics-get-started-portal.md)anzeigen
- Das Lernprogramm [Erste Schritte mit Azure Data Lake Analytics U SQL Studio](data-lake-analytics-u-sql-get-started.md) durchlaufen.
- Azure herstellen Sie, finden Sie unter [Erste Schritte mit Azure Data Lake Analytics U SQL Studio](data-lake-analytics-u-sql-get-started.md#connect-to-azure). 
- Hochladen Sie die Daten, finden Sie unter [Erste Schritte mit Azure Data Lake Analytics U SQL Studio](data-lake-analytics-u-sql-get-started.md#upload-source-data-files). 

## <a name="define-and-use-user-defined-operator-in-u-sql"></a>Definieren und Verwenden von benutzerdefinierten Operators in U-SQL

**Erstellen und Senden eines U-SQL-Auftrags** 

1. Klicken Sie im Menü **Datei** auf **neu**und klicken Sie auf **Projekt**.
2. **U-SQL** Projekttyp ausgewählt.

    ![Neues U SQL Visual Studio-Projekt](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-new-project.png)

3. Klicken Sie auf **OK**. Visual Studio erstellt eine Lösung mit einer Script.usql-Datei.
4. **Projektmappen-Explorer**erweitern Sie Script.usql, und doppelklicken Sie auf **Script.usql.cs**.
5. Fügen Sie den folgenden Code in die Datei:

        using Microsoft.Analytics.Interfaces;
        using System.Collections.Generic;
        
        namespace USQL_UDO
        {
            public class CountryName : IProcessor
            {
                private static IDictionary<string, string> CountryTranslation = new Dictionary<string, string>
                {
                    {
                        "Deutschland", "Germany"
                    },
                    {
                        "Schwiiz", "Switzerland"
                    },
                    {
                        "UK", "United Kingdom"
                    },
                    {
                        "USA", "United States of America"
                    },
                    {
                        "中国", "PR China"
                    }
                };
        
                public override IRow Process(IRow input, IUpdatableRow output)
                {
        
                    string UserID = input.Get<string>("UserID");
                    string Name = input.Get<string>("Name");
                    string Address = input.Get<string>("Address");
                    string City = input.Get<string>("City");
                    string State = input.Get<string>("State");
                    string PostalCode = input.Get<string>("PostalCode");
                    string Country = input.Get<string>("Country");
                    string Phone = input.Get<string>("Phone");
        
                    if (CountryTranslation.Keys.Contains(Country))
                    {
                        Country = CountryTranslation[Country];
                    }
                    output.Set<string>(0, UserID);
                    output.Set<string>(1, Name);
                    output.Set<string>(2, Address);
                    output.Set<string>(3, City);
                    output.Set<string>(4, State);
                    output.Set<string>(5, PostalCode);
                    output.Set<string>(6, Country);
                    output.Set<string>(7, Phone);
        
                    return output.AsReadOnly();
                }
            }
        }

5. Öffnen Sie Script.usql, und fügen Sie folgende U-SQL-Skript:

        @drivers =
            EXTRACT UserID      string,
                    Name        string,
                    Address     string,
                    City        string,
                    State       string,
                    PostalCode  string,
                    Country     string,
                    Phone       string
            FROM "/Samples/Data/AmbulanceData/Drivers.txt"
            USING Extractors.Tsv(Encoding.Unicode);
        
        @drivers_CountryName =
            PROCESS @drivers
            PRODUCE UserID string,
                    Name string,
                    Address string,
                    City string,
                    State string,
                    PostalCode string,
                    Country string,
                    Phone string
            USING new USQL_UDO.CountryName();    
        
        OUTPUT @drivers_CountryName
            TO "/Samples/Outputs/Drivers.csv"
            USING Outputters.Csv(Encoding.Unicode);

6. **Projektmappen-Explorer**klicken Sie mit der rechten Maustaste auf **Script.usql**und klicken Sie dann auf **Skript erstellen**.
6. **Projektmappen-Explorer**klicken Sie mit der rechten Maustaste auf **Script.usql**und klicken Sie dann auf **Skript senden**.
7. Wenn Sie noch nicht auf Ihre Azure-Abonnement, werden Sie schnell Ihre Azure-Konto-Anmeldeinformationen eingeben.
7. Klicken Sie auf **Senden**. Übermittlung Ergebnisse und Auftrag Link stehen im Fenster Ergebnisse bei die Übermittlung abgeschlossen ist.
8. Sie müssen die Schaltfläche Aktualisieren, um die aktuellen Auftragsstatus und Aktualisieren des Bildschirms klicken.

**Die Auftragsausgabe an**

1. Im **Server-Explorer**erweitern **Azure** **See Datenanalyse**, erweitern Sie Ihr Konto See Datenanalyse, **Speicherkonten**erweitern, Maustaste standardmäßig Speicher und anschließend klicken Sie auf **Explorer**. 
2. Erweitern Sie Beispiele Ausgaben, und doppelklicken Sie dann auf **Treiber.csv**.


##<a name="see-also"></a>Siehe auch

- [Erste Schritte mit See Datenanalyse mithilfe von PowerShell](data-lake-analytics-get-started-powershell.md)
- [Erste Schritte mit See Datenanalyse mithilfe des Azure-Portals](data-lake-analytics-get-started-portal.md)
- [Verwenden Sie Data See Tools für Visual Studio für U-SQL Anwendungsentwicklung](data-lake-analytics-data-lake-tools-get-started.md)
