<properties
    pageTitle="Verbinden mit SQL-Datenbank mithilfe von Python | Microsoft Azure"
    description="Stellt ein Codebeispiel Python, mit Azure SQL-Datenbank herstellen."
    services="sql-database"
    documentationCenter=""
    authors="meet-bhagdev"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="drivers"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="meetb"/>


# <a name="connect-to-sql-database-by-using-python"></a>Verbinden Sie mit SQL-Datenbank mithilfe von Python


[AZURE.INCLUDE [sql-database-develop-includes-selector-language-platform-depth](../../includes/sql-database-develop-includes-selector-language-platform-depth.md)] 


In diesem Thema veranschaulicht, wie zu einer Azure SQL-Datenbank mit Python Abfragen. In diesem Beispiel führen Sie Windows, Ubuntu Linux und Mac-Plattformen.


## <a name="step-1-create-a-sql-database"></a>Schritt 1: Erstellen einer SQL-Datenbank

Finden Sie die [Seite Erste Schritte](sql-database-get-started.md) auf eine Beispieldatenbank erstellen.  Es ist wichtig, dass Sie die Anleitung zum Erstellen einer **Vorlage der AdventureWorks-Datenbank**. Die nachfolgenden Beispiele funktionieren nur mit **AdventureWorks-Schema**. Nachdem Sie Ihre Datenbank sicher erstellen aktivieren Sie den Zugriff auf Ihre IP-Adresse von Firewall-Regeln aktivieren, siehe [Seite Erste Schritte](sql-database-get-started.md)

## <a name="step-2-configure-development-environment"></a>Schritt 2: Konfigurieren von Development Environment

### <a name="mac-os"></a>**Mac OS**   
### <a name="install-the-required-modules"></a>Installieren Sie die erforderlichen Module
Öffnen Sie das Terminal und installieren

    ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
    brew install FreeTDS
    sudo -H pip install pymssql=2.1.1

### <a name="linux-ubuntu"></a>**Linux (Ubuntu)**

Öffnen Sie das Terminal und navigieren Sie zu einem Verzeichnis auf Ihrem Python-Skript erstellt werden soll. Geben Sie die folgenden Befehle **FreeTDS** und **Pymssql**installieren. Pymssql verwendet FreeTDS Verbindung zum SQL-Datenbanken.

    sudo apt-get --assume-yes update
    sudo apt-get --assume-yes install freetds-dev freetds-bin
    sudo apt-get --assume-yes install python-dev python-pip
    sudo pip install pymssql=2.1.1
    
### <a name="windows"></a>**Windows**

Installieren Sie Pymssql [**hier**](http://www.lfd.uci.edu/~gohlke/pythonlibs/#pymssql). 

Sicherstellen Sie, dass die richtigen Whl-Datei auszuwählen. Beispiel: Wenn Sie Python 2.7 auf einem 64-Bit-Computer verwenden: Pymssql‑2.1.1‑cp27‑none‑win_amd64.whl. Downloaden Sie die Datei speichern platzieren Sie es in den Ordner C:/Python27.

Mit Pip über Befehlszeile Pymssql Treiber jetzt installieren. CD in c-Python27 und führen folgenden
    
    pip install pymssql‑2.1.1‑cp27‑none‑win_amd64.whl

Auf Pip verwenden aktivieren finden Sie [hier](http://stackoverflow.com/questions/4750806/how-to-install-pip-on-windows)

## <a name="step-3-run-sample-code"></a>Schritt 3: Ausführen von Beispielcode

Eine Datei namens **sql_sample.py** , und fügen sie den folgenden Code. Sie können dies von der Befehlszeile ausführen:
    
    python sql_sample.py

### <a name="connect-to-your-sql-database"></a>Verbinden Sie mit der SQL-Datenbank

Die Funktion [pymssql.connect](http://pymssql.org/en/latest/ref/pymssql.html) zum SQL-Datenbank herstellen.

    import pymssql
    conn = pymssql.connect(server='yourserver.database.windows.net', user='yourusername@yourserver', password='yourpassword', database='AdventureWorks')


### <a name="execute-an-sql-select-statement"></a>Führen Sie eine SQL SELECT-Anweisung

Die [cursor.execute](http://pymssql.org/en/latest/ref/pymssql.html#pymssql.Cursor.execute) -Funktion kann verwendet werden, eine Ergebnismenge aus einer Abfrage in SQL-Datenbank abrufen. Diese Funktion akzeptiert im Wesentlichen Fragen und gibt, die eine Ergebnismenge, mit [cursor.fetchone()](http://pymssql.org/en/latest/ref/pymssql.html#pymssql.Cursor.fetchone)durchlaufen werden können.


    import pymssql
    conn = pymssql.connect(server='yourserver.database.windows.net', user='yourusername@yourserver', password='yourpassword', database='AdventureWorks')
    cursor = conn.cursor()
    cursor.execute('SELECT c.CustomerID, c.CompanyName,COUNT(soh.SalesOrderID) AS OrderCount FROM SalesLT.Customer AS c LEFT OUTER JOIN SalesLT.SalesOrderHeader AS soh ON c.CustomerID = soh.CustomerID GROUP BY c.CustomerID, c.CompanyName ORDER BY OrderCount DESC;')
    row = cursor.fetchone()
    while row:
        print str(row[0]) + " " + str(row[1]) + " " + str(row[2])   
        row = cursor.fetchone()


### <a name="insert-a-row-pass-parameters-and-retrieve-the-generated-primary-key"></a>Einfügen einer Zeile Parameter übergeben und die generierte primäre Schlüssel

SQL-Datenbank die [IDENTITY](https://msdn.microsoft.com/library/ms186775.aspx) -Eigenschaft und [das Sequenzobjekt](https://msdn.microsoft.com/library/ff878058.aspx) dienen [Primärschlüsselwerte](https://msdn.microsoft.com/library/ms179610.aspx) automatisch generiert. 


    import pymssql
    conn = pymssql.connect(server='yourserver.database.windows.net', user='yourusername@yourserver', password='yourpassword', database='AdventureWorks')
    cursor = conn.cursor()
    cursor.execute("INSERT SalesLT.Product (Name, ProductNumber, StandardCost, ListPrice, SellStartDate) OUTPUT INSERTED.ProductID VALUES ('SQL Server Express', 'SQLEXPRESS', 0, 0, CURRENT_TIMESTAMP)")
    row = cursor.fetchone()
    while row:
        print "Inserted Product ID : " +str(row[0])
        row = cursor.fetchone()


### <a name="transactions"></a>Transaktionen


Dieses Codebeispiel veranschaulicht die Verwendung von Transaktionen in der Sie:

* Eine Transaktion beginnen
* Fügen Sie eine Zeile von Daten
* Rollback der Transaktion einfügen rückgängig machen 

Fügen Sie folgenden Code in sql_sample.py.
    
    import pymssql
    conn = pymssql.connect(server='yourserver.database.windows.net', user='yourusername@yourserver', password='yourpassword', database='AdventureWorks')
    cursor = conn.cursor()
    cursor.execute("BEGIN TRANSACTION")
    cursor.execute("INSERT SalesLT.Product (Name, ProductNumber, StandardCost, ListPrice, SellStartDate) OUTPUT INSERTED.ProductID VALUES ('SQL Server Express New', 'SQLEXPRESS New', 0, 0, CURRENT_TIMESTAMP)")
    cnxn.rollback()

## <a name="next-steps"></a>Nächste Schritte

* Lesen Sie die [SQL-Datenbank-Anwendungsentwicklung, Überblick](sql-database-develop-overview.md)
* Weitere Informationen über [Microsoft Python-Treiber für SQL Server](https://msdn.microsoft.com/library/mt652092.aspx)
* Besuchen Sie das [Developer Center Python](/develop/python/).

## <a name="additional-resources"></a>Zusätzliche Ressourcen 

* [Entwurfsmuster für Multi-Tenant SaaS-Applikationen mit SQL Azure-Datenbank](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* Durchsuchen Sie alle [Funktionen der SQL-Datenbank](https://azure.microsoft.com/services/sql-database/)
