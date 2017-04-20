<properties
    pageTitle="Verschlüsselt: Schützen Sie vertrauliche Daten in Azure SQL-Datenbank mit Verschlüsselung | Microsoft Azure"
    description="Schützen Sie vertrauliche Daten in der SQL-Datenbank in Minuten."
    keywords="Verschlüsseln von Daten, Sql-Verschlüsselung, Verschlüsselung, vertrauliche Daten verschlüsselt"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor="cgronlun"/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/18/2016"
    ms.author="sstein"/>

# <a name="always-encrypted-protect-sensitive-data-in-sql-database-and-store-your-encryption-keys-in-the-windows-certificate-store"></a>Verschlüsselt: Schützen Sie vertrauliche Daten in SQL-Datenbank und Speichern von Verschlüsselungsschlüsseln im Windows-Zertifikatspeicher

> [AZURE.SELECTOR]
- [Azure-Tresor](sql-database-always-encrypted-azure-key-vault.md)
- [Windows-Zertifikatspeicher](sql-database-always-encrypted.md)


Dieser Artikel beschreibt, wie vertrauliche Daten in einer SQL-Datenbank mit Verschlüsselung mithilfe des [Assistenten immer verschlüsselt](https://msdn.microsoft.com/library/mt459280.aspx) in [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/hh213248.aspx)gesichert. Es zeigt auch, wie die Verschlüsselungsschlüssel im Windows-Zertifikatspeicher gespeichert.

Immer verschlüsselt ist eine neue Technologie der Daten-Verschlüsselung in Azure SQL-Datenbank hilft SQL Server-schützen Sie vertrauliche Daten auf dem Server während der Bewegung zwischen Client und Server und während die Daten verwendet werden, sicherstellen, dass vertraulichen Daten nie als Klartext in der Datenbanksystem angezeigt. Nach dem verschlüsseln Daten können nur Clientanwendungen oder Anwendungsserver, die Zugriff auf die Schlüssel unverschlüsselte Daten zugreifen. Ausführliche Informationen finden Sie unter [(Datenbankmodul) verschlüsselt](https://msdn.microsoft.com/library/mt163865.aspx).

Nach der Konfiguration der Datenbank verschlüsselt, erstellen Sie eine Clientanwendung in C# mit Visual Studio arbeiten mit verschlüsselten Daten.

Führen Sie die Schritte in diesem Artikel erfahren, wie immer verschlüsselt für eine SQL Azure-Datenbank eingerichtet. In diesem Artikel erfahren Sie, wie Sie folgende Aufgaben ausführen:

- Mithilfe des Assistenten verschlüsselt in SSMS [immer verschlüsselt](https://msdn.microsoft.com/library/mt163865.aspx#Anchor_3)erstellen.
    - Erstellen einer [Spalte Hauptschlüssel (CMK)](https://msdn.microsoft.com/library/mt146393.aspx).
    - Erstellen einer [Spalte Schlüssel (CEK)](https://msdn.microsoft.com/library/mt146372.aspx).
- Erstellen einer Datenbanktabelle und Spalten zu verschlüsseln.
- Erstellen Sie eine Anwendung, die eingefügt, ausgewählt und Daten aus den verschlüsselten Spalten.

## <a name="prerequisites"></a>Erforderliche Komponenten

In diesem Lernprogramm benötigen Sie:

- Ein Azure-Konto und ein Abonnement. Haben Sie eine [kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/)registrieren.
- [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) Version 13.0.700.242 oder höher.
- [.NET Framework 4.6](https://msdn.microsoft.com/library/w0x726c2.aspx) oder höher (auf dem Clientcomputer).
- [Visual Studio](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx).



## <a name="create-a-blank-sql-database"></a>Erstellen Sie eine leere SQL-Datenbank
1. Mit der [Azure-Portal](https://portal.azure.com/)anmelden.
2. **Klicken Sie auf** > **Daten + Speicher** > **SQL-Datenbank**.
3. Erstellen Sie eine **leere** Datenbank **Clinic** auf einer neuen oder vorhandenen Namen. Weitere Informationen zum Erstellen einer Datenbank in Azure-Portal finden Sie unter [Erstellen einer SQL-Datenbank in Minuten](sql-database-get-started.md).

    ![Erstellen einer leeren Datenbank](./media/sql-database-always-encrypted/create-database.png)

Sie benötigen die Verbindungszeichenfolge später im Lernprogramm. Nach der Erstellung der Datenbank zur neuen Clinic-Datenbank und die Verbindungszeichenfolge kopieren. Abrufen der Verbindungszeichenfolge jederzeit, aber es ist einfach kopieren bei Azure-Portal.

1. Klicken Sie auf **SQL-Datenbanken** > **Klinik** > **Datenbank-Verbindungszeichenfolgen anzeigen**.
2. Kopieren Sie die Verbindungszeichenfolge für **ADO.NET**.

    ![Die Verbindungszeichenfolge kopieren](./media/sql-database-always-encrypted/connection-strings.png)


## <a name="connect-to-the-database-with-ssms"></a>Verbinden Sie mit der Datenbank mit SSMS

SSMS öffnen und an den Server mit der Klinik Datenbank verbinden.


1. SSMS zu öffnen. (Klicken Sie auf **Verbinden** > -**Datenbank-Engine** **mit Server** -Fenster öffnen, wenn er nicht geöffnet ist).
2. Geben Sie die Server- und Anmeldeinformationen. Der Servername finden Sie auf der SQL-Datenbank und in der Verbindungszeichenfolge kopiert zuvor. Geben Sie den vollständigen Servernamen ein, einschließlich *database.windows.net*.

    ![Die Verbindungszeichenfolge kopieren](./media/sql-database-always-encrypted/ssms-connect.png)

Öffnet das Fenster **Neue Firewall-Regel** in Azure anmelden und SSMS für Sie eine neue Firewallregel erstellen lassen.


## <a name="create-a-table"></a>Erstellen einer Tabelle

In diesem Abschnitt erstellen Sie eine Tabelle für Patientendaten. Wird eine normale Tabelle zunächst - Verschlüsselung konfigurieren Sie im nächsten Abschnitt.

1. Erweitern Sie **Datenbanken**.
1. **Klinik** -Datenbank und **Neue Abfrage**.
2. Neues Abfragefenster und **Ausführen** der folgenden Transact-SQL (T-SQL) fügen sie.


        CREATE TABLE [dbo].[Patients](
         [PatientId] [int] IDENTITY(1,1),
         [SSN] [char](11) NOT NULL,
         [FirstName] [nvarchar](50) NULL,
         [LastName] [nvarchar](50) NULL,
         [MiddleName] [nvarchar](50) NULL,
         [StreetAddress] [nvarchar](50) NULL,
         [City] [nvarchar](50) NULL,
         [ZipCode] [char](5) NULL,
         [State] [char](2) NULL,
         [BirthDate] [date] NOT NULL
         PRIMARY KEY CLUSTERED ([PatientId] ASC) ON [PRIMARY] );
         GO


## <a name="encrypt-columns-configure-always-encrypted"></a>Verschlüsseln von Spalten (Konfigurieren verschlüsselt)

SSMS bietet einen Assistenten einfach verschlüsselt Einstellung CMK, CEK und verschlüsselten Spalten konfigurieren.

1. Erweitern Sie **Datenbanken** > **Klinik** > **Tabellen**.
2. Tabelle **Patienten** Maustaste, und wählen Sie **Spalten verschlüsseln** verschlüsselt-Assistenten zu öffnen:

    ![Verschlüsseln von Spalten](./media/sql-database-always-encrypted/encrypt-columns.png)

Verschlüsselt-Assistent umfasst die folgenden Abschnitte: **Spaltenauswahl**, **Hauptschlüssel Konfiguration** (CMK), **Validierung**und **Zusammenfassung**.

### <a name="column-selection"></a>Spaltenauswahl ###

Klicken Sie auf **der Einführungsseite auf der Seite **Spalte** ** **Weiter** . Auf dieser Seite Wählen Sie die Spalten zu verschlüsseln, [den Typ der Verschlüsselung, und welche Spalte (CEK)](https://msdn.microsoft.com/library/mt459280.aspx#Anchor_2) verwenden.

Verschlüsseln Sie Informationen für jeden Patienten **SSN** und **Geburtsdatum** . Deterministische Verschlüsselung verwendet unterstützt Gleichheit Lookups Joins und Gruppieren nach Spalte **SSN** . Die **BirthDate** -Spalte verwendet zufällige Verschlüsselung die Vorgänge nicht unterstützt.

**Verschlüsselungstyp** für die Spalte **SSN** auf **deterministische** und die **BirthDate** -Spalte auf **Randomized**festgelegt. Klicken Sie auf **Weiter**.

![Verschlüsseln von Spalten](./media/sql-database-always-encrypted/column-selection.png)

### <a name="master-key-configuration"></a>Hauptschlüssel-Konfiguration###

**Hauptschlüssel** -Konfigurationsseite wird Ihre CMK einrichten und Schlüsselspeicher wählen, wo die CMK gespeichert werden. Derzeit können Sie eine CMK im Windows-Zertifikatspeicher, Azure Schlüssel Depot oder einem Hardwaresicherheitsmodul (HSM) speichern. Dieses Lernprogramm zeigt, wie Schlüssel im Windows-Zertifikatspeicher gespeichert.

**Windows-Zertifikatspeicher** aktiviert sein, und klicken Sie auf **Weiter**.

![Hauptschlüssel-Konfiguration](./media/sql-database-always-encrypted/master-key-configuration.png)


### <a name="validation"></a>Validierung###

Sie können jetzt Spalten verschlüsseln oder ein PowerShell-Skripts zur späteren Ausführung zu speichern. In diesem Lernprogramm wählen Sie **gehen jetzt** und auf **Weiter**.

### <a name="summary"></a>Zusammenfassung###

Überprüfen Sie die Einstellung alle korrigieren, und klicken Sie auf **Fertig stellen** , um das Setup für verschlüsselt abzuschließen.

![Zusammenfassung](./media/sql-database-always-encrypted/summary.png)


### <a name="verify-the-wizards-actions"></a>Der Assistent Aktionen überprüfen

Nachdem der Assistent abgeschlossen ist, wird die Datenbank für verschlüsselt eingerichtet. Assistenten folgenden Aktionen ausgeführt:

- Erstellt ein CMK.
- Erstellt eine CEK.
- Die ausgewählten Spalten für Verschlüsselung konfiguriert. Die Tabelle **Patienten** hat zurzeit keine Daten, aber alle vorhandenen Daten in den ausgewählten Spalten nun verschlüsselt.

Überprüfen die Erstellung der Schlüssel in SSMS auf **Technologieüberblick** > **Security** > **Schlüssel immer verschlüsselt**. Sie sehen jetzt neue Schlüssel, die der Assistent für Sie generiert.


## <a name="create-a-client-application-that-works-with-the-encrypted-data"></a>Erstellen einer Clientanwendung, die mit der verschlüsselten Daten

Verschlüsselt eingerichtet ist, können Sie eine Anwendung erstellen, die den verschlüsselten Spalten *eingefügt* und *wählt* ausführt. Zum beispielanwendung ausführen führen Sie es auf dem gleichen Computer, in dem Sie den Assistenten verschlüsselt ausgeführt. Um die Anwendung auf einem anderen Computer auszuführen, müssen Sie Ihre Zertifikate verschlüsselt an den Computer mit der Clientanwendung bereitstellen.  

> [AZURE.IMPORTANT] Wenn unverschlüsselte Daten an den Server verschlüsselt Spalten muss die Anwendung [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx) -Objekte verwenden. Literale Werte ohne SqlParameter-Objekten übergeben führt zu einer Ausnahme.


1. Öffnen Sie Visual Studio, und erstellen Sie eine neue C#. Stellen Sie sicher, dass das Projekt auf **.NET Framework 4.6** oder höher festgelegt ist.
2. Nennen Sie das Projekt **AlwaysEncryptedConsoleApp** , und klicken Sie auf **OK**.

![Neue](./media/sql-database-always-encrypted/console-app.png)



## <a name="modify-your-connection-string-to-enable-always-encrypted"></a>Ändern Sie die Verbindungszeichenfolge verschlüsselt aktivieren

Dieser Abschnitt erläutert die verschlüsselt in der Datenbank-Verbindungszeichenfolge aktivieren. Die Konsole app ändern Sie erstellten im nächsten Abschnitt "Immer verschlüsselt Beispiel Konsolenanwendungsprojekt."


Aktivieren verschlüsselt, müssen Sie die Verbindungszeichenfolge **Spalte Verschlüsselungseinstellung** Schlüsselwort hinzu und **aktiviert**festgelegt.

Sie können dies direkt in der Verbindungszeichenfolge festlegen oder mithilfe einer [SqlConnectionStringBuilder](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.aspx)festlegen. Die beispielanwendung im nächsten Abschnitt veranschaulicht, wie **SqlConnectionStringBuilder**.

> [AZURE.NOTE] Dies ist die einzige Änderung in einer Clientanwendung bestimmte verschlüsselt erforderlich. Wenn Sie eine vorhandene Anwendung haben, die Verbindungszeichenfolge extern gespeichert (d. h. in einer Konfigurationsdatei), möglicherweise verschlüsselt ohne Code aktivieren.


### <a name="enable-always-encrypted-in-the-connection-string"></a>Die Verbindungszeichenfolge verschlüsselt aktivieren

Fügen Sie das folgende Schlüsselwort zur Verbindungszeichenfolge:

    Column Encryption Setting=Enabled


### <a name="enable-always-encrypted-with-a-sqlconnectionstringbuilder"></a>Verschlüsselt mit SqlConnectionStringBuilder aktivieren

Der folgende Code veranschaulicht die verschlüsselt aktivieren, indem Sie die [SqlConnectionStringBuilder.ColumnEncryptionSetting](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.columnencryptionsetting.aspx) [aktiviert](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectioncolumnencryptionsetting.aspx).

    // Instantiate a SqlConnectionStringBuilder.
    SqlConnectionStringBuilder connStringBuilder =
       new SqlConnectionStringBuilder("replace with your connection string");

    // Enable Always Encrypted.
    connStringBuilder.ColumnEncryptionSetting =
       SqlConnectionColumnEncryptionSetting.Enabled;



## <a name="always-encrypted-sample-console-application"></a>Immer verschlüsselt Beispiel Konsolenanwendungsprojekt

In diesem Beispiel wird veranschaulicht, wie Sie:

- Ändern Sie die Verbindungszeichenfolge verschlüsselt aktivieren.
- Fügen Sie den verschlüsselten Spalten mit Daten.
- Wählen Sie einen Datensatz durch einen bestimmten Wert in eine verschlüsselte Spalte filtern.

Ersetzen Sie den Inhalt von **"Program.cs"** durch den folgenden Code. Ersetzen Sie die Verbindungszeichenfolge für die globale ConnectionString-Variable in der Zeile direkt oberhalb der Main-Methode mit der gültigen Verbindungszeichenfolge Azure-Portal. Dies ist die einzige Änderung dieser Code zu müssen.

Führen Sie die Anwendung verschlüsselt in Aktion zu sehen.

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;
    using System.Data;
    using System.Data.SqlClient;

    namespace AlwaysEncryptedConsoleApp
    {
    class Program
    {
        // Update this line with your Clinic database connection string from the Azure portal.
        static string connectionString = @"Replace with your connection string";

        static void Main(string[] args)
        {
            Console.WriteLine("Original connection string copied from the Azure portal:");
            Console.WriteLine(connectionString);

            // Create a SqlConnectionStringBuilder.
            SqlConnectionStringBuilder connStringBuilder =
                new SqlConnectionStringBuilder(connectionString);

            // Enable Always Encrypted for the connection.
            // This is the only change specific to Always Encrypted
            connStringBuilder.ColumnEncryptionSetting =
                SqlConnectionColumnEncryptionSetting.Enabled;

            Console.WriteLine(Environment.NewLine + "Updated connection string with Always Encrypted enabled:");
            Console.WriteLine(connStringBuilder.ConnectionString);

            // Update the connection string with a password supplied at runtime.
            Console.WriteLine(Environment.NewLine + "Enter server password:");
            connStringBuilder.Password = Console.ReadLine();


            // Assign the updated connection string to our global variable.
            connectionString = connStringBuilder.ConnectionString;


            // Delete all records to restart this demo app.
            ResetPatientsTable();

            // Add sample data to the Patients table.
            Console.Write(Environment.NewLine + "Adding sample patient data to the database...");

            InsertPatient(new Patient() {
                SSN = "999-99-0001", FirstName = "Orlando", LastName = "Gee", BirthDate = DateTime.Parse("01/04/1964") });
            InsertPatient(new Patient() {
                SSN = "999-99-0002", FirstName = "Keith", LastName = "Harris", BirthDate = DateTime.Parse("06/20/1977") });
            InsertPatient(new Patient() {
                SSN = "999-99-0003", FirstName = "Donna", LastName = "Carreras", BirthDate = DateTime.Parse("02/09/1973") });
            InsertPatient(new Patient() {
                SSN = "999-99-0004", FirstName = "Janet", LastName = "Gates", BirthDate = DateTime.Parse("08/31/1985") });
            InsertPatient(new Patient() {
                SSN = "999-99-0005", FirstName = "Lucy", LastName = "Harrington", BirthDate = DateTime.Parse("05/06/1993") });


            // Fetch and display all patients.
            Console.WriteLine(Environment.NewLine + "All the records currently in the Patients table:");

            foreach (Patient patient in SelectAllPatients())
            {
                Console.WriteLine(patient.FirstName + " " + patient.LastName + "\tSSN: " + patient.SSN + "\tBirthdate: " + patient.BirthDate);
            }

            // Get patients by SSN.
            Console.WriteLine(Environment.NewLine + "Now let's locate records by searching the encrypted SSN column.");

            string ssn;

            // This very simple validation only checks that the user entered 11 characters.
            // In production be sure to check all user input and use the best validation for your specific application.
            do
            {
                Console.WriteLine("Please enter a valid SSN (ex. 123-45-6789):");
                ssn = Console.ReadLine();
            } while (ssn.Length != 11);

            // The example allows duplicate SSN entries so we will return all records
            // that match the provided value and store the results in selectedPatients.
            Patient selectedPatient = SelectPatientBySSN(ssn);

            // Check if any records were returned and display our query results.
            if (selectedPatient != null)
            {
                Console.WriteLine("Patient found with SSN = " + ssn);
                Console.WriteLine(selectedPatient.FirstName + " " + selectedPatient.LastName + "\tSSN: "
                    + selectedPatient.SSN + "\tBirthdate: " + selectedPatient.BirthDate);
            }
            else
            {
                Console.WriteLine("No patients found with SSN = " + ssn);
            }

            Console.WriteLine("Press Enter to exit...");
            Console.ReadLine();
        }


        static int InsertPatient(Patient newPatient)
        {
            int returnValue = 0;

            string sqlCmdText = @"INSERT INTO [dbo].[Patients] ([SSN], [FirstName], [LastName], [BirthDate])
         VALUES (@SSN, @FirstName, @LastName, @BirthDate);";

            SqlCommand sqlCmd = new SqlCommand(sqlCmdText);


            SqlParameter paramSSN = new SqlParameter(@"@SSN", newPatient.SSN);
            paramSSN.DbType = DbType.AnsiStringFixedLength;
            paramSSN.Direction = ParameterDirection.Input;
            paramSSN.Size = 11;

            SqlParameter paramFirstName = new SqlParameter(@"@FirstName", newPatient.FirstName);
            paramFirstName.DbType = DbType.String;
            paramFirstName.Direction = ParameterDirection.Input;

            SqlParameter paramLastName = new SqlParameter(@"@LastName", newPatient.LastName);
            paramLastName.DbType = DbType.String;
            paramLastName.Direction = ParameterDirection.Input;

            SqlParameter paramBirthDate = new SqlParameter(@"@BirthDate", newPatient.BirthDate);
            paramBirthDate.SqlDbType = SqlDbType.Date;
            paramBirthDate.Direction = ParameterDirection.Input;

            sqlCmd.Parameters.Add(paramSSN);
            sqlCmd.Parameters.Add(paramFirstName);
            sqlCmd.Parameters.Add(paramLastName);
            sqlCmd.Parameters.Add(paramBirthDate);

            using (sqlCmd.Connection = new SqlConnection(connectionString))
            {
                try
                {
                    sqlCmd.Connection.Open();
                    sqlCmd.ExecuteNonQuery();
                }
                catch (Exception ex)
                {
                    returnValue = 1;
                    Console.WriteLine("The following error was encountered: ");
                    Console.WriteLine(ex.Message);
                    Console.WriteLine(Environment.NewLine + "Press Enter key to exit");
                    Console.ReadLine();
                    Environment.Exit(0);
                }
            }
            return returnValue;
        }


        static List<Patient> SelectAllPatients()
        {
            List<Patient> patients = new List<Patient>();


            SqlCommand sqlCmd = new SqlCommand(
              "SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients]",
                new SqlConnection(connectionString));


            using (sqlCmd.Connection = new SqlConnection(connectionString))

            using (sqlCmd.Connection = new SqlConnection(connectionString))
            {
                try
                {
                    sqlCmd.Connection.Open();
                    SqlDataReader reader = sqlCmd.ExecuteReader();

                    if (reader.HasRows)
                    {
                        while (reader.Read())
                        {
                            patients.Add(new Patient()
                            {
                                SSN = reader[0].ToString(),
                                FirstName = reader[1].ToString(),
                                LastName = reader["LastName"].ToString(),
                                BirthDate = (DateTime)reader["BirthDate"]
                            });
                        }
                    }
                }
                catch (Exception ex)
                {
                    throw;
                }
            }

            return patients;
        }


        static Patient SelectPatientBySSN(string ssn)
        {
            Patient patient = new Patient();

            SqlCommand sqlCmd = new SqlCommand(
                "SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE [SSN]=@SSN",
                new SqlConnection(connectionString));

            SqlParameter paramSSN = new SqlParameter(@"@SSN", ssn);
            paramSSN.DbType = DbType.AnsiStringFixedLength;
            paramSSN.Direction = ParameterDirection.Input;
            paramSSN.Size = 11;

            sqlCmd.Parameters.Add(paramSSN);


            using (sqlCmd.Connection = new SqlConnection(connectionString))
            {
                try
                {
                    sqlCmd.Connection.Open();
                    SqlDataReader reader = sqlCmd.ExecuteReader();

                    if (reader.HasRows)
                    {
                        while (reader.Read())
                        {
                            patient = new Patient()
                            {
                                SSN = reader[0].ToString(),
                                FirstName = reader[1].ToString(),
                                LastName = reader["LastName"].ToString(),
                                BirthDate = (DateTime)reader["BirthDate"]
                            };
                        }
                    }
                    else
                    {
                        patient = null;
                    }
                }
                catch (Exception ex)
                {
                    throw;
                }
            }
            return patient;
        }


        // This method simply deletes all records in the Patients table to reset our demo.
        static int ResetPatientsTable()
        {
            int returnValue = 0;

            SqlCommand sqlCmd = new SqlCommand("DELETE FROM Patients");
            using (sqlCmd.Connection = new SqlConnection(connectionString))
            {
                try
                {
                    sqlCmd.Connection.Open();
                    sqlCmd.ExecuteNonQuery();

                }
                catch (Exception ex)
                {
                    returnValue = 1;
                }
            }
            return returnValue;
        }
    }

    class Patient
    {
        public string SSN { get; set; }
        public string FirstName { get; set; }
        public string LastName { get; set; }
        public DateTime BirthDate { get; set; }
    }
    }


## <a name="verify-that-the-data-is-encrypted"></a>Stellen Sie sicher, dass die Daten

Sie können schnell überprüfen, dass die eigentlichen Daten auf dem Server Abfragen **Patientendaten mit SSMS** verschlüsselt ist. (Verwenden Sie die aktuelle Verbindung, wird die Einstellung der Spalte Verschlüsselung noch nicht aktiviert.)

Führen Sie die folgende Abfrage in der Datenbank des Technologieüberblicks.

    SELECT FirstName, LastName, SSN, BirthDate FROM Patients;

Sie können sehen, dass die verschlüsselten Spalten keine nur-Text-Daten enthält.

   ![Neue](./media/sql-database-always-encrypted/ssms-encrypted.png)


Um SSMS unverschlüsselte Daten zugreifen, können Sie die **Spalte Verschlüsselungseinstellung = aktiviert** Parameter für die Verbindung.

1. In SSMS mit der rechten Maustaste des Servers im **Objekt-Explorer**und klicken Sie dann auf **Trennen**.
2. Klicken Sie auf **Verbinden** > **-Datenbank-Engine** , öffnen Sie das Fenster **Verbindung mit Server herstellen** und dann auf **Optionen**.
3. Klicken Sie auf **Zusätzliche Verbindungsparameter** und Typ **Spalte Verschlüsselungseinstellung = aktiviert**.

    ![Neue](./media/sql-database-always-encrypted/ssms-connection-parameter.png)

4. Führen Sie die folgende Abfrage in der Datenbank **des Technologieüberblicks** .

        SELECT FirstName, LastName, SSN, BirthDate FROM Patients;

     Unverschlüsselte Daten in verschlüsselten Spalten sehen.


    ![Neue](./media/sql-database-always-encrypted/ssms-plaintext.png)



> [AZURE.NOTE] Wenn Sie mit SSMS (oder jeder Client) auf einem anderen Computer verbinden, keinen Zugriff auf die Verschlüsselungsschlüssel und werden nicht die Daten zu entschlüsseln.



## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie eine Datenbank erstellen, die verschlüsselt verwendet, sollten Sie Folgendes:

- Dieses Beispiel wird von einem anderen Computer ausführen. Sie haben keinen Zugriff auf die Verschlüsselungsschlüssel nicht erfolgreich ausgeführt und haben keinen Zugriff auf die Daten im Klartext.
- [Drehen und Ihre Schlüssel](https://msdn.microsoft.com/library/mt607048.aspx).
- [Migrieren von Daten, die bereits mit verschlüsselt verschlüsselt ist](https://msdn.microsoft.com/library/mt621539.aspx).
- [Zertifikate für andere Clientcomputer bereitstellen verschlüsselt](https://msdn.microsoft.com/library/mt723359.aspx#Anchor_1) (siehe Abschnitt "Machen Zertifikate verfügbar, Applikationen und Benutzer").

## <a name="related-information"></a>Informationen

- [Verschlüsselt (Entwicklung)](https://msdn.microsoft.com/library/mt147923.aspx)
- [Transparente Verschlüsselung](https://msdn.microsoft.com/library/bb934049.aspx)
- [SQL Server-Verschlüsselung](https://msdn.microsoft.com/library/bb510663.aspx)
- [Immer verschlüsselt Assistenten](https://msdn.microsoft.com/library/mt459280.aspx)
- [Immer verschlüsselt Blog](http://blogs.msdn.com/b/sqlsecurity/archive/tags/always-encrypted/)
